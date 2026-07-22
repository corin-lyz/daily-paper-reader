---
title: Unleashing Semantic and Geometric Priors for 3D Scene Completion
title_zh: 释放语义与几何先验用于3D场景完成
authors: "Shiyuan Chen, Wei Sui, Bohao Zhang, Zeyd Boukhers, John See, Cong Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37294/41256"
tags: ["query:stereo-depth"]
score: 5.0
evidence: 场景完成中的立体代价体用于几何分支
tldr: 现有3D场景完成方法中语义与几何先验耦合，导致性能取舍。本文提出FoundationSSC，在源端和路径端双重解耦：基础编码器为语义分支提供丰富语义，为几何分支提供高保真立体代价体。通过专用解耦路径精化先验，在自动驾驶基准上取得最佳几何与语义精度，为双目深度信息在场景理解中的应用提供了有效范例。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37294/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 868, \"height\": 552, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37294/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1843, \"height\": 549, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37294/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 867, \"height\": 468, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37294/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 861, \"height\": 384, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37294/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1845, \"height\": 599, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1816, \"height\": 662, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 882, \"height\": 314, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1835, \"height\": 586, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 879, \"height\": 277, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 873, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37294/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 205, \"label\": \"Table\"}]"
motivation: 传统耦合编码器使语义与几何先验冲突，限制整体性能。
method: 提出双解耦框架，基础编码器分别提供语义特征和立体代价体，经专用路径精化。
result: 在SemanticKITTI等数据集上实现领先的几何与语义场景完成精度。
conclusion: 语义与几何先验的解耦能有效提升3D场景完成性能。
---

## Abstract
Camera-based 3D semantic scene completion (SSC) provides dense geometric and semantic perception for autonomous driving and robotic navigation. However, existing methods rely on a coupled encoder to deliver both semantic and geometric priors, which forces the model to make a trade-off between conflicting demands and limits its overall performance. To tackle these challenges, we propose FoundationSSC, a novel framework that performs dual decoupling at both the source and pathway levels. At the source level, we introduce a foundation encoder that provides rich semantic feature priors for the semantic branch and high-fidelity stereo cost volumes for the geometric branch. At the pathway level, these priors are refined through specialised, decoupled pathways, yielding superior semantic context and depth distributions. Our dual-decoupling design produces disentangled and refined inputs, which are then utilised by a hybrid view transformation to generate complementary 3D features. Additionally, we introduce a novel Axis-Aware Fusion (AAF) module that addresses the often-overlooked challenge of fusing these features by anisotropically merging them into a unified representation. Extensive experiments demonstrate the advantages of FoundationSSC, achieving simultaneous improvements in both semantic and geometric metrics, surpassing prior bests by +0.23 mIoU and +2.03 IoU on SemanticKITTI. Additionally, we achieve state-of-the-art performance on SSCBench-KITTI-360, with 21.78 mIoU and 48.61 IoU.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：相机式3D语义场景补全（SSC）需要同时输出稠密的几何结构（占用网格）和语义标签，但现有方法普遍使用**耦合编码器**，即同一个图像编码器同时提取语义和几何特征。这导致模型必须在两者之间做出权衡，例如过度强调几何精度会损害语义细粒度，反之亦然。这种内在冲突限制了整体的感知性能。
- **整体含义**：本文提出**FoundationSSC**，通过**双解耦**方式从源头和路径上分离语义与几何处理，从而突破耦合限制，同时提升几何和语义指标。这一思想为SSC乃至更广泛的3D感知任务提供了新范式。

## 2. 方法论

- **核心思想**：在**源端**使用一个集成的基础编码器（FoundationStereo）同时提供解耦的语义先验（来自DINOv2/DepthAnythingV2的2D特征）和几何先验（立体匹配的代价体体积）；在**路径端**通过两个专用的、解耦的子网络分别精化这些先验，最后通过混合视图变换和轴感知融合模块（AAF）生成统一的3D表示。
- **关键技术细节**：
  - **Foundation Encoder**：冻结的FoundationStereo模型，输出：
    - 单目图像特征 \(F_{2D}\)（多尺度融合后）。
    - 视差代价体特征 \(V_{disp}\)（保留概率分布信息）。
    - 稠密深度图 \(Z\)。
  - **Geometry-Aware Context Adapter (GCA)**：增强语义特征的3D几何一致性。构建几何先验矩阵 \(M_g = \alpha M_d + (1-\alpha)M_s\)，其中 \(M_d\) 为像素间深度距离，\(M_s\) 为曼哈顿距离。将此矩阵作为自注意力调制偏置：  
    \[
    \text{GeoAttn}(Q,K,V,M_g) = (\text{Softmax}(QK^T) \odot \beta M_g)V
    \]
    实现几何引导的注意力。
  - **Disparity-to-Depth Volume Mapping (DDVM)**：将视差代价体 \(V_{disp}\) 通过可学习的非线性映射转为深度概率分布 \(D \in \mathbb{R}^{D_{\text{depth}}\times H\times W}\)，避免信息丢失。包含通道映射块和3D CNN精化 + softmax。
  - **Hybrid View Transformation**：结合LSS（Lift-Splat-Shoot）和体素Transformer。LSS利用上下文特征 \(C\) 和深度分布 \(D\) 外积生成视锥体特征，经体素池化得到 \(F_{lss}\)；体素Transformer利用 \(F_{lss}\) 和深度图生成查询进行可变形交叉注意力与自注意力，得到 \(F_{vt}\)。
  - **Axis-Aware Fusion (AAF)**：针对 \(F_{lss}\)（近场几何细节好）和 \(F_{vt}\)（远场/遮挡上下文好），沿着三个正交轴（X,Y,Z）分别设计融合单元。每个单元在对应平面上结合局部和全局信息生成动态注意力 \(\sigma_d\)，最终：  
    \[
    F_{\text{fused}} = \sum_{d \in \{XY, XZ, YZ\}} (\sigma_d F_{lss} + (1-\sigma_d)F_{vt})
    \]
  - **损失函数**：总损失 = \(\lambda_d L_d + \lambda_s L_s + L_{ce} + L_{\text{geo}}^{\text{scal}} + L_{\text{sem}}^{\text{scal}}\)，其中 \(L_d\) 深度损失，\(L_s\) 2D语义分割辅助损失，\(L_{ce}\) 加权交叉熵，后两项为场景级和类别级仿射损失。

## 3. 实验设计

- **数据集与基准**：
  - **SemanticKITTI**（KITTI Odometry的语义标注），包含100+序列，用于驾驶场景。
  - **SSCBench-KITTI-360**（基于KITTI-360的SSC基准），覆盖更大范围。
  - 评估指标：**IoU**（几何占用准确率）和**mIoU**（语义分割平均交并比）。
- **对比方法**：包含 MonoScene、TPVFormer、VoxFormer、OccFormer、Symphonies、BRGScene、HTCL（带时序）、CGFormer、VLScene（带语言模型）、ScanSSC、SOAP（时序）等11种当前主流方法。
- **实验设置**：在SemanticKITTI隐藏测试集和SSCBench-KITTI-360测试集上报告结果；同时在SemanticKITTI验证集上进行消融实验。

## 4. 资源与算力

- **文中未明确说明使用的GPU型号、数量或训练时长**。仅提到在“Implementation Details”部分（论文未在正文中详细给出，可能在补充材料中），但根据元数据及一般惯例，该类SSC方法通常使用4-8块V100或A100 GPU训练数天。本文未提供具体数字，无法准确汇报。

## 5. 实验数量与充分性

- **实验数量**：较多，覆盖：
  - 主表测试：两个数据集上的定量结果（表1、表2）。
  - 消融实验：共5组（表3-表6），分别验证：
    - 架构组件消融（FE、GCA、DDVM、AAF）。
    - 不同基础编码器（EfficientNet、DINOv2、DepthAnything等）比较。
    - 深度生成策略消融（Depth Refinement、Cost Volume+AR、DDVM）。
    - 3D特征融合策略消融（无融合、3D通道注意力、AAF）。
  - 定性可视化（图5）。
- **充分性与公平性**：实验设计较为充分，消融逐步验证了每个模块的有效性；对比方法均为近两年SOTA且结果来自原始论文或公开报告，公平性较好。但缺少对计算开销的对比（参数、FLOPs、推理速度），以及缺少在室内数据集（如NYU）上的泛化验证，略有局限。

## 6. 主要结论与发现

- **结论**：双解耦设计能有效解决语义与几何的固有冲突，显著提升SSC的联合性能。
- **关键发现**：
  - 源端使用基础编码器（FoundationStereo）提供高质量解耦先验，比传统耦合编码器高出+2.06 mIoU、+1.33 IoU。
  - GCA模块增强了语义特征的3D几何一致性，DDVM有效保留代价体概率信息，两者协同获得额外+0.97 mIoU、+1.23 IoU。
  - AAF各向异性融合优于各向同性的3D通道注意力（+0.28 mIoU）。
  - 整体在SemanticKITTI上达到19.32 mIoU、48.12 IoU，在SSCBench-KITTI-360上达到21.78 mIoU、48.61 IoU，均达到当时SOTA。

## 7. 优点

- **理念创新**：明确识别并解决了SSC中长期存在的语义-几何耦合问题，提出双解耦框架。
- **技术融合**：巧妙利用VFMs（FoundationStereo）同时提供语义和几何先验，无需额外微调。
- **模块设计精细**：GCA利用深度引导注意力、DDVM通过可学习映射保留概率信息、AAF各向异性融合，每个模块均有理论动机且实验验证有效。
- **性能提升显著**：在几何和语义指标上同步大幅提升，突破以往此消彼长的局限。
- **泛化性好**：在两个不同驾驶数据集上均取得SOTA结果。

## 8. 不足与局限

- **依赖立体输入**：方法需要左右视图（立体相机），不适用于单目场景，限制了应用范围（如仅配备单摄像头的系统）。
- **缺乏计算量报告**：未提供模型参数量、FLOPs或推理时间，难以评估实际部署成本。
- **未在室内场景验证**：仅在驾驶场景（KITTI系列）上测试，在NYUv2等室内SSC数据集上的表现未知。
- **消融实验未覆盖所有组合**：例如，未分析在不同数据集上各模块的贡献度是否一致；未探讨AAF单元数量的影响。
- **基础编码器冻结导致特征可能不是最优**：虽然避免了微调开销，但冻结的VFMs在特定场景（如恶劣天气）可能存在分布偏移。
- **数据集偏差**：SemanticKITTI的类别分布不均衡（如“行人”样本远少于“道路”），模型在长尾类上仍有提升空间（表格显示部分类别非最优）。

（完）
