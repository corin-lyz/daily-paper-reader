---
title: "UniCT Depth: Event-Image Fusion Based Monocular Depth Estimation with Convolution-Compensated ViT Dual SA Block"
title_zh: UniCT Depth：基于事件-图像融合与卷积补偿ViT双自注意力块的单目深度估计
authors: "(PDF |   Details)"
date: 2025-08-01
pdf: "https://www.ijcai.org/proceedings/2025/0144.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 基于事件-图像融合与卷积补偿ViT的单目深度估计
tldr: 针对单目深度估计在动态场景和光照变化下精度不足的问题，提出UniCT Depth方法，融合事件相机数据与常规图像。该方法采用卷积补偿的视觉Transformer双自注意力块，有效结合事件相机的高时间分辨率信息与图像的丰富纹理信息。实验验证该融合机制可显著提升深度估计精度，尤其在动态场景中表现突出。该工作为事件-图像融合的单目深度估计提供了一种高效网络结构。
source: IJCAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-144/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 887, \"height\": 427, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-144/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1819, \"height\": 948, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-144/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1838, \"height\": 868, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-144/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1839, \"height\": 673, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-144/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1801, \"height\": 729, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-144/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1834, \"height\": 409, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-144/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 888, \"height\": 421, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-144/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 891, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-144/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1786, \"height\": 409, \"label\": \"Table\"}]"
motivation: 单目深度估计在动态场景与光照剧烈变化下，单一RGB图像信息不足，需要引入事件相机的高时间分辨率线索。
method: 提出卷积补偿的ViT双自注意力块，对事件流与RGB图像进行跨模态融合，以增强深度估计的时空特征。
result: 在事件-图像深度数据集上取得更优的精度，动态场景下优势明显。
conclusion: 事件-图像融合结合卷积补偿Transformer，可有效提升单目深度估计在动态环境中的鲁棒性。
---

## Abstract
No abstract is available.

---

## 论文详细总结（自动生成）

# UniCT Depth: 基于事件-图像融合与卷积补偿ViT双自注意力块的单目深度估计

## 1. 论文的核心问题与整体含义

- **研究动机**：单目深度估计在三维场景理解中至关重要，广泛应用于自动驾驶、医学影像等领域。传统基于普通图像的深度估计方法在极端光照、快速运动、遮挡等复杂场景下性能大幅下降，容易丢失关键细节。
- **事件相机的优势与瓶颈**：事件相机具有高动态范围和高时间分辨率，能够捕捉像素级亮度变化，适合挑战性场景；但事件数据本身是异步、稀疏的，难以直接生成稠密深度图。
- **融合的必要性**：将事件数据与普通图像融合，可以互补二者优势——事件提供动态与光照鲁棒性，图像提供丰富纹理与场景细节。然而，现有融合方法存在明显不足：
  - CNN-based 方法感受野有限，难以处理遮挡和深度差异大的区域；
  - Transformer-based 方法往往独立处理各模态，缺乏深层次模态交互；
  - 部分方法使用标准自注意力在拼接后的token上计算，计算代价高且融合粗糙，易受长程噪声干扰。
- **本文目标**：提出 **UniCT Depth**，一种统一 CNN-Transformer 架构的事件-图像融合单目深度估计方法，通过局部与全局特征协同建模，提升复杂场景下的深度估计精度，尤其在遮挡和单模态可见目标区域表现更好。

## 2. 论文提出的方法论

- **总体架构**：采用 U-Net 风格的编码器-解码器结构，包含预处理器（Preprocessor）、编码器（Encoder）、解码器（Decoder）三部分。
  - **预处理器**：对事件帧和强度图像分别进行卷积特征提取，生成全分辨率特征图，再拼接并卷积融合。
  - **编码器**：由两个残差块和四个 CcViT-DA 模块构成，逐级下采样（分辨率依次为 1/2, 1/4, 1/8, 1/16, 1/32）并提取高层语义。
  - **解码器**：由五个解码块组成，通过反卷积逐步恢复分辨率（1/16 → 1/8 → 1/4 → 1/2 → 1），使用跳跃连接进行跨层特征融合。
- **事件表示**：将事件流编码为时空体素网格（spatio-temporal voxel grid），通过时间域离散化和双线性插值累积事件，保留时间信息并减少运动模糊。体素网格尺寸为 H×W×B，B 为时间箱数量（实验中设为 5）。
  - 关键公式：\( \mathbf{V}_k(x,y,t) = \sum_i p_i \delta(x-x_i, y-y_i) \max\{0, 1 - |t - t_i^*|\} \)（其中 \( t_i^* = \frac{B-1}{\Delta T}(t_i - t_0) \)）
- **核心模块：Convolution-compensated ViT Dual SA (CcViT-DA) Block**
  - 包含三部分：Patch Embedding 层、ViT 双自注意力（ViT Dual SA）块、细节补偿卷积（DCC）块。
  - **CMSA (Context Modeling Self-Attention)**：在空间维度上按非重叠窗口计算多头自注意力，捕获局部上下文依赖，适应遮挡与深度突变。复杂度从 \(O(P^2 d)\) 降至 \(O(P P_w d)\)。
  - **MFSA (Modal Fusion Self-Attention)**：在通道维度上对转置后的 token 分组做自注意力（头数为 1），建立跨模态全局依赖，自适应增强可靠模态、抑制噪声。复杂度为 \(O(P C C_g)\)。
  - **DCC (Detail Compensation Convolution) Block**：利用全局最大池化和全局平均池化聚合通道信息，拼接后经卷积和 Sigmoid 生成空间注意力图，再对输入特征进行加权；最后通过两个卷积层和激活函数输出。用于增强局部纹理细节和边缘特征。
  - **融合方式**：CMSA 与 MFSA 两个分支的输出分别与 DCC 输出做点乘，然后拼接，经过合并层（Merge Layer）得到最终特征。
- **损失函数**：同时使用 L1 和 L2 损失的加权和。对每个预测深度图计算与真值的差值 \(R_k = D_k^* - D_k\)，总损失为 \(L = \frac{1}{n} \sum_u (R_k(x,y) + R_k(x,y)^2)\)。

## 3. 实验设计

- **数据集**：
  - **MVSEC**：真实世界事件相机数据集，包含户外白天和夜间多个序列（Outdoor day1、Outdoor night1、night2、night3），是事件相机深度估计的常用 benchmark。
  - **DENSE**：模拟数据集，用于验证方法在新场景上的泛化能力。
- **评估协议**：主要使用平均绝对深度误差（Avg. Error），在 10m、20m、30m 截断距离下评估；同时使用绝对相对误差（Abs. Rel.）、对数均方误差（RMSE log）和阈值精度（δ < 1.25, 1.25², 1.25³）等标准指标。
- **对比方法**：
  - **基于图像的方法**：MonoDepth、MegaDepth、MonoViT、DPT、IEBins 等。
  - **基于事件的方法**：Zhu et al.、DTL-、E2Depth、Mixed-EF2DNet 等。
  - **融合方法**：RAMNet、EMoDepth、EVT+、HMNet、Transformer-based (2024)、SRFNet 等。
- **结果摘要**：
  - 在 MVSEC 上，UniCT Depth 在所有序列的平均 Avg. Error（10/20/30m）上均取得最优结果；在 12 项详细指标中取得了 10 项最优。Abs. Rel. 相比最优基线 SRFNet 在白天和夜间分别提升 5.56% 和 7.16%。
  - 在 DENSE 上，所有截断距离的 Avg. Error 均最优，在 10m、20m、30m 相对第二好结果分别提升 13.3%、30%、10.9%。

## 4. 资源与算力

- 论文明确提到：模型使用 PyTorch 实现，在 **两块 NVIDIA GeForce RTX 3090 GPU** 上训练。
- 训练设置：MVSEC 学习率 0.0002，DENSE 学习率 0.002；使用 ADAMW 优化器和 MultiStepLR 调度器；batch size 为 16；训练 50 个 epoch；在第 10、20、30 个 epoch 将学习率乘以 0.5。
- 输入图像尺寸为 224×224，体素网格时间箱数为 5。
- **未明确说明**：总训练时长、单卡 vs 双卡并行策略、显存占用等细节。

## 5. 实验数量与充分性

- **实验组数**：
  1. 在 MVSEC 数据集上与大量 SOTA 方法对比（表 1、表 2）。
  2. 在 DENSE 数据集上进行泛化性验证（表 3）。
  3. 对 CcViT-DA Block 的消融实验（表 4），考查 CMSA、MFSA、DCC 的贡献。
  4. 对输入模态的消融实验（表 5），比较仅事件、仅图像、融合输入。
  5. 提供了定性对比图（图 3、图 4）和 FPS 测量。
- **充分性评估**：
  - 优点：覆盖了真实和模拟数据集、多种光照条件、多个主流基线、模块级和模态级消融，实验比较全面。
  - 可改进之处：未在更多真实场景（如室内、极端天气）上验证；缺少与更多最近方法的对比；消融实验仅报告了 MVSEC 子集，未详细展示 DENSE 上的消融；没有统计显著性检验或多次运行方差报告。

## 6. 论文的主要结论与发现

- UniCT Depth 通过统一的 CNN-Transformer 架构，有效融合事件和图像模态，在 MVSEC 和 DENSE 上均达到领先性能，证明了其鲁棒性和准确性。
- 空间维度的 CMSA 与通道维度的 MFSA 联合使用，能够同时建模局部上下文和全局跨模态依赖，显著优于使用单一自注意力或标准自注意力。
- DCC 块对局部细节和边缘表示的增强进一步提升了深度估计质量。
- 消融实验确认：事件和图像模态互补，单一模态在白天或夜间各有优势，融合后在两类场景都最优；CMSA+MFSA+DCC 的组合是最优配置，且速度（25 FPS）仍可接受（在 RTX 3090 上）。
- 方法在只有单一模态可见的区域（如夜间车辆、树木）以及遮挡场景中，能够比基线更准确地估计深度。

## 7. 优点

- **架构设计的创新性**：将 CNN 的局部建模能力与 Transformer 的全局依赖建模能力统一在同一编码器中，避免了传统分离式设计带来的冗余计算和浅层交互。
- **高效的注意力机制**：CMSA 和 MFSA 分别沿空间和通道维度降低复杂度，在保持性能的同时显著提升计算效率（相比标准自注意力，FPS 从 13 提升到 25）。
- **模态融合的针对性**：MFSA 专门设计用于通道维度的跨模态关系建模，优于简单的 token 拼接和标准注意力，能够自适应强化可靠模态、抑制噪声。
- **结构细节增强**：DCC 块为融合特征提供局部纹理补偿，改善边缘和高纹理物体的深度预测。
- **实验验证充分**：在真实数据（MVSEC）和模拟数据（DENSE）上都验证了有效性；对比基线覆盖图像、事件、融合三大类方法，且同时报告了定量指标和定性可视化。
- **实用性**：在保持较高帧率（25 FPS）的前提下实现了最佳精度，具有实时应用的潜力。

## 8. 不足与局限

- **实验场景覆盖有限**：主要使用室外自动驾驶场景（MVSEC）和模拟数据集（DENSE），未涉及室内环境、恶劣天气（雨雾）、极端高速运动等场景，泛化性证据不足。
- **对比公平性存疑**：不同方法的训练设置、输入表示可能不完全一致（例如某些基线使用不同事件表示或不同分辨率），论文未详细说明是否采用统一复现设置，可能存在隐式偏向。
- **消融实验的细节不足**：消融研究仅报告了 Avg.Error 和 FPS，未报告 Abs. Rel.、δ 等其它指标；DENSE 上未做消融；未分析 DCC 中不同池化组合（如仅平均池化）的影响。
- **统计鲁棒性缺失**：未报告多次运行的标准差或统计显著性检验，无法判断指标差异的稳定性。
- **资源与时间成本未详细披露**：虽然列出了 GPU 型号和 epoch 数，但未说明每个 epoch 耗时、总训练时间和显存占用，可复现性信息不够完整。
- **事件稀疏性处理依赖体素网格**：该方法将事件流转换为固定分辨率体素网格，可能损失异步事件的高时间分辨率信息，在极高频率动态下或低事件率场景中的表现未验证。
- **深度范围局限**：实验仅评估了 10m/20m/30m 截断距离，未对更远距离（如 50m+）的深度估计性能进行讨论，而自动驾驶中远距离深度同样关键。

（完）
