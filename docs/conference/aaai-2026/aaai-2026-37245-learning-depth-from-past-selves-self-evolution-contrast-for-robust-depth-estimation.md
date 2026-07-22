---
title: "Learning Depth from Past Selves: Self-Evolution Contrast for Robust Depth Estimation"
title_zh: "从过去的自己学习深度: 自进化对比用于鲁棒深度估计"
authors: "Jing Cao, Kui Jiang, Shenyi Li, Xiaocheng Feng, Yong Huang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37245/41207"
tags: ["query:mono-depth"]
score: 9.0
evidence: 自监督单目深度估计，对恶劣天气鲁棒
tldr: 针对自监督深度估计在雨雾等恶劣天气下性能严重下降的问题，本文提出自进化对比学习框架SEC-Depth。该方法利用训练过程中的中间参数构建时序演化模型，并通过自进化对比方案缓解退化。在KITTI等数据集上，SEC-Depth在雨雾场景下显著优于现有方法，且在正常天气下保持竞争力，证明了自进化对比策略的有效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37245/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 343, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37245/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1606, \"height\": 907, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37245/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 628, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37245/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1818, \"height\": 946, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 864, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 865, \"height\": 153, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 873, \"height\": 1382, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 871, \"height\": 837, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1807, \"height\": 376, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37245/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 864, \"height\": 370, \"label\": \"Table\"}]"
motivation: 现有自监督深度估计方法在恶劣天气下精度大幅降低，缺乏鲁棒性。
method: 提出自进化对比学习框架，利用训练中历史参数构建时变模型，通过对比学习增强鲁棒性。
result: 在雨雾环境中深度估计误差显著降低，同时正常天气性能保持稳定。
conclusion: 自进化对比学习有效提升了深度估计在复杂天气下的鲁棒性。
---

## Abstract
Self-supervised depth estimation has gained significant attention in autonomous driving and robotics. However, existing methods exhibit substantial performance degradation under adverse weather conditions such as rain and fog, where reduced visibility critically impairs depth prediction. To address this issue, we propose a novel self-evolution contrastive learning framework called SEC-Depth for self-supervised robust depth estimation tasks. Our approach leverages intermediate parameters generated during training to construct temporally evolving latency models. Using these, we design a self-evolution contrastive scheme to mitigate performance loss under challenging conditions. Concretely, we first design a dynamic update strategy of latency models for the depth estimation task to capture optimization states across training stages. To effectively leverage latency models, we introduce a self-evolution contrastive Loss (SECL) that treats outputs from historical latency models as negative samples. This mechanism adaptively adjusts learning objectives while implicitly sensing weather degradation severity, reducing the needs for manual intervention. Experiments show that our method integrates seamlessly into diverse baseline models and significantly enhances robustness in zero-shot evaluations.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：自监督深度估计在自动驾驶和机器人领域至关重要，但现有方法在雨、雾、雪等恶劣天气下性能严重退化。原因在于天气粒子破坏了光度一致性假设（自监督学习的核心约束），导致深度预测不可靠。
- **背景**：现有鲁棒性提升方法包括域适应、知识蒸馏、对比学习等，但存在以下局限：
  - 域适应缺乏泛化性；
  - 知识蒸馏受限于教师模型性能；
  - 对比学习常依赖额外网络、预设数据集或复杂的课程学习调度，且直接最小化深度差异可能导致解坍塌。
- **本文目标**：提出一种轻量化、即插即用的自进化对比学习框架（SEC-Depth），无需修改基线模型架构或引入外部先验，即可显著提升恶劣天气下的深度估计鲁棒性。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 利用训练过程中产生的历史模型参数（称为“延时模型”，latency models）构建对比学习中的负样本。随着训练进行，这些延时模型逐步收敛到次优解，其输出与当前模型在恶劣天气下的输出形成有意义的对比，从而引导模型学习鲁棒表示。
- 设计基于区间的深度分布约束，将连续深度值离散化为概率分布，更好地捕捉全局结构关系，克服像素级损失在局部退化下的局限。

### 关键技术细节
1. **延时模型队列的动态更新**
   - 维护一个包含 \( j \) 个历史模型的队列，每个模型通过指数移动平均（EMA）从当前主模型参数更新：
     \[
     \theta_k^* = \omega \theta_k^* + (1-\omega) \theta
     \]
     其中 \(\omega = 0.01\)。
   - 更新策略：每 \( T \) 步正常更新；当当前负样本多样性不足（深度差异方差小于锚点-正样本差异方差）时主动触发更新（算法1）。

2. **三元组的生成**
   - **锚点（Anchor）**：当前模型 \( F_t \) 对增强图像（恶劣天气）的预测 \( D_A = F_t(I_{aug}) \)。
   - **正样本（Positive）**：当前模型对对应干净图像的预测 \( D_P = F_t(I) \)。
   - **负样本（Negative）**：从延时模型队列中随机选择一个模型 \( F_{N_k} \)，对同一增强图像的预测 \( D_N = F_{N_k}(I_{aug}) \)。

3. **区间深度分布约束**
   - 将视差范围 \([0,1]\) 划分为 \( N \) 个等宽区间，用高斯核将每个像素的视差值分配为区间权重，得到归一化的概率分布 \( P_X \)（\( X \) 表示锚点、正样本或负样本）。
   - 使用 Jensen-Shannon（JS）散度衡量分布相似性。

4. **自进化对比损失（SECL）**
   \[
   L_c = \text{JS}(P_A\|P_P) + \frac{1}{M} \sum_k \left[ \delta \Delta_k^1 + \text{JS}(P_A\|P_{N_k}) \Delta_k^2 \right]
   \]
   其中：
   - \(\Delta_k^i = \max(\alpha_i - \text{JS}(P_A\|P_{N_k}), 0)\)，i=1,2。
   - \(\alpha_1\) 采用非线性指数衰减：\(\alpha_1 = a e^{-15 t/T} + c\)，动态调节锚点与负样本间距；\(\alpha_2\) 固定为0.005。
   - \(\delta\) 为权重系数。

5. **整体训练损失**
   \[
   L = L_{ph} + w L_c
   \]
   其中 \( L_{ph} \) 为光度重建损失（对干净和增强图像均计算），\( w \) 从0.01开始按epoch逐步增长至稳定值（公式12）。

### 算法流程概述
- 每间隔 \( S \) 步，将增强图像 \( I_{aug} \) 送入训练，计算对比损失 \( L_c \)。
- 主模型在干净图像和增强图像上交替优化，延时模型队列按算法1更新。

## 3. 实验设计：数据集、基准与对比方法

### 数据集与场景
| 数据集 | 类型 | 条件 | 用途 |
|--------|------|------|------|
| **WeatherKITTI** | 合成 | 雨、雾、雪各2种 | 训练（3种条件）与测试（Eigen split 697张） |
| **DrivingStereo** | 真实 | 雨、雾各500张 | 零样本测试 |
| **Foggy Cityscapes** | 合成 | 雾（1525张） | 零样本测试 |
| **Rain Cityscapes** | 合成 | 3种强度雨（1188张） | 零样本测试 |
| **Snow Cityscapes** | 合成 | 3种强度雪（1510张） | 零样本测试 |
| **Dense** | 真实 | 雪（500张） | 零样本测试 |

### 对比方法
- **基于 MonoViT 基线**：MonoViT（基线）、Robust-Depth、EC-Depth、EC-Depth*、WeatherDepth*。
- **基于 PlaneDepth 基线**：PlaneDepth（基线）、WeatherDepth†。
- 注：SEC-Depth 分别集成到两个基线中，命名为 SEC-Depth 和 SEC-Depth*。

### 评估指标
- 常用的深度估计指标：AbsRel、SqRel、RMSE、\(\delta_1\)（准确率 a1）、\(\delta_2\)（a2）、\(\delta_3\)（a3）。

## 4. 资源与算力

- 论文未明确说明使用的 GPU 型号和数量。
- 在消融实验中（表6）给出了训练时长，例如 `S=5` 时 MonoViT 基线训练时长约 21.5 小时，但未说明对应硬件配置。
- 因此，算力信息不透明，无法直接复现硬件需求。

## 5. 实验数量与充分性

### 实验数量
- **主实验**：在 WeatherKITTI 上进行定量和定性比较（表1、表2）。
- **零样本实验**：6个独立数据集（DrivingStereo 雨/雾、Foggy/Rain/Snow Cityscapes、Dense），每个数据集列在表3（MonoViT基线）和表5（PlaneDepth基线）中。
- **消融实验**：
  - 组件消融（表4）：逐步添加对比学习、区间分布约束、自进化对比损失，共5组配置。
  - 负样本步长 \( S \) 消融（表6）：测试 \( S \in \{1,5,10,20\} \)，包括 WeatherKITTI 和零样本平均结果。
- **定性结果**：图4展示了多个场景的深度图对比。

### 充分性评价
- **充分**：涵盖了合成和真实数据，多种恶劣天气，两个主流基线，对比了当前最先进的鲁棒方法。
- **客观公平**：统一基线（MonoViT 和 PlaneDepth），使用官方发布的预训练参数，遵循原协议的训练/测试划分。
- **消融全面**：验证了每个组件的贡献，并分析了关键超参数 \( S \) 的影响。
- 不足：缺少在更多真实恶劣天气（如夜间、暴雨）上的验证；未与其他非对比学习方法（如扩散模型蒸馏）进行详尽对比。

## 6. 论文的主要结论与发现

- **SEC-Depth 显著提升了自监督深度估计在恶劣天气下的鲁棒性**：
  - 在 WeatherKITTI 上，基于 MonoViT 的 AbsRel 降低 13.33%（0.120 → 0.104），基于 PlaneDepth 降低 37.97%（0.158 → 0.098）。
  - 在零样本测试的 6 个数据集中，SEC-Depth 在大多数指标上优于现有对比方法（如 WeatherDepth*、Robust-Depth）。
- **即插即用性**：无需修改基线架构，可集成到 MonoViT 和 PlaneDepth 等多种模型中。
- **自进化对比机制的有效性**：利用历史模型参数作为负样本，动态调整学习目标，能够在无手动干预下感知天气退化严重程度。
- **区间分布约束优于像素级对齐**：消融实验表明，将深度分布离散化后，RMSE 和 \(\delta_1\) 均有改善。

## 7. 优点

- **创新性**：首次将训练过程中的“历史自我”状态用于对比学习，无需外部教师或额外数据集。
- **轻量化与通用性**：仅需维护一个模型参数队列，不改变推理结构，易于嵌入任意自监督深度估计框架。
- **动态适应**：负样本更新策略和自适应对比损失（\(\alpha_1\) 指数衰减）使训练过程更稳定、收敛更好。
- **分布约束设计**：用 JS 散度衡量深度概率分布，比像素级损失能更好区分局部结构差异。
- **实验充分**：大量零样本跨数据测试，验证了泛化能力。

## 8. 不足与局限

- **实验覆盖有限**：
  - 训练数据仅使用 KITTI 合成天气集（WeatherKITTI），未在真实恶劣数据集（如夜间、暴雨）上训练。
  - 零样本测试虽覆盖部分真实场景（DrivingStereo、Dense），但真实天气多样化不足（如缺少重度雾、冰雹等）。
- **偏差风险**：合成天气模拟可能无法完全代表真实分布，实际部署时可能存在域偏移。
- **算力需求未明确**：缺少 GPU 型号和数量说明，复现时需自行评估。
- **超参数敏感性**：\( S \)、队列大小 \( j \)、衰减参数 \( a, c \) 等需要手动调节，论文仅对 \( S \) 进行了消融，其他参数固定，未做充分探索。
- **应用限制**：仅针对自监督单目深度估计，未验证是否适用于立体匹配或多传感器融合场景。

（完）
