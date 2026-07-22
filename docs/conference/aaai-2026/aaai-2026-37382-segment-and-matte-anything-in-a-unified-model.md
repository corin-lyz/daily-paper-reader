---
title: Segment and Matte Anything in a Unified Model
title_zh: 统一模型中的任意分割与抠图
authors: "Zezhong Fan, Xiaohan Li, Topojoy Biswas, Kaushiki Nag, Kannan Achan"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37382/41344"
tags: ["query:matting"]
score: 9.0
evidence: 统一分割与交互抠图模型
tldr: 该论文提出一个统一的分割与交互抠图模型，将SAM的零样本分割能力与精细alpha抠图结合，解决了真实应用中抠图精度不足的问题。通过单一框架实现分割和抠图，支持用户交互提示，在多种抠图基准上达到最优性能，为无需trimap的高质量人像抠图提供了新范式。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37382/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1819, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37382/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 657, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37382/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 872, \"height\": 823, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37382/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 826, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37382/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1274, \"height\": 829, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37382/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1482, \"height\": 272, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37382/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 755, \"height\": 269, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37382/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 698, \"height\": 274, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37382/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 245, \"label\": \"Table\"}]"
motivation: 现有SAM分割精度不足以满足实际应用，且交互式抠图尚未集成到SAM框架中。
method: 设计统一模型，在SAM基础上引入抠图头，实现分割与抠图联合训练与推理。
result: 在多个抠图数据集上超越现有方法，并能零样本推广到新场景。
conclusion: 统一分割与抠图是可行的，且能显著提升边界质量。
---

## Abstract
Segment Anything (SAM) has recently pushed the boundaries of segmentation by demonstrating remarkable zero-shot generalization and flexible prompting after training on over one billion masks. Despite this, its mask prediction accuracy often falls short of the precision required in real-world applications. While several refinement modules have been proposed to boost SAM’s segmentation quality, achieving highly accurate object delineation within a single, unified framework remains an open challenge. Furthermore, interactive image matting—which aims to generate fine-grained alpha mattes guided by diverse user hints—has not yet been explored in the context of SAM. Insights from recent studies highlight strong correlations between segmentation and matting, suggesting the feasibility of a unified model capable of both tasks.

In this paper, we introduce Segment And Matte Anything (SAMA), a lightweight extension of SAM that delivers high-quality interactive image segmentation and matting with minimal extra parameters or computational cost. Our Multi-View Localization Encoder (MVLE) captures detailed features from local views, while the Localization Adapter (Local-Adapter) refines mask outputs by recovering subtle boundary details. We also incorporate two prediction heads for each task into the architecture to generate segmentation and matting tasks, simultaneously. Trained on a diverse dataset aggregated from publicly available sources, SAMA achieves state-of-the-art performance across multiple segmentation and matting benchmarks, showcasing its adaptability and effectiveness in a wide range of downstream tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **研究动机**：Segment Anything Model（SAM）在零样本分割上取得了突破，但其掩膜预测精度在实际应用中往往不够精确。现有的细化模块（如HQ-SAM、DIS-SAM等）虽能提升质量，但通常需要额外的后处理模型或更多用户交互，增加了复杂度。同时，交互式图像抠图（interactive matting）——即根据用户提示生成精细的alpha遮罩——尚未在SAM框架中得到探索。
- **整体含义**：图像分割与抠图具有强互补性：分割提供全局物体线索，抠图提供局部边界精度。将二者统一到一个轻量级、高效的模型中，既能提升分割边界质量，又能实现无需trimap的交互式抠图，具有重要的实际应用价值（如照片编辑、增强现实等）。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：在冻结SAM所有参数的基础上，增加三个轻量级模块（总参数量仅增加1.8%），实现高质量分割与抠图的统一。
- **关键技术细节**：
  - **Multi-View Localization Encoder (MVLE)**：将输入图像均匀裁剪为4个不重叠的局部块，放大至原始分辨率后通过SAM图像编码器提取局部高分辨率特征，并通过跨注意力机制与全局特征对齐，捕捉精细结构。
  - **Localization Adapter (Local-Adapter)**：在每个解码器层之后插入，包含两个跨注意力层和置信度映射。第一层以解码器输出为查询、MVLE输出与早期层特征的融合为键/值；第二层交换查询与键/值实现双向全局-局部交互。最后通过置信度图自适应融合局部细节，防止过拟合。
  - **预测头**：引入两个可学习的SAMA token（分别用于分割和抠图），并设计了两个轻量级上采样模块（含插值、卷积、归一化、GELU激活），分别输出二值掩膜和alpha遮罩。
  - **训练策略**：冻结SAM全部参数，仅训练上述模块。分割和抠图任务分别训练（对应任务的头被激活，另一头冻结）。损失函数：分割采用BCE+IoU+SSIM；抠图采用ℓ1+SSIM+梯度损失+拉普拉斯损失。

## 3. 实验设计：数据集、基准、对比方法
- **分割任务**：
  - 训练集：DIS-5K（高精度分割）、ThinObject-5K（细薄物体）
  - 测试集：DIS-VD（470张）、DIS-TE1~TE4（各500张，复杂度递增）、DIS-TE(All 2000张)
  - 对比方法：SAM、HQ-SAM、Pi-SAM、DIS-SAM、IS-Net、UDUN、BiRefNet
  - 评估指标：最大F-measure (Fβmax)、加权F-measure (Fwβ)、平均绝对误差(MAE)、S-measure、增强对齐度量(Eφ)
  - 零样本交互分割：COIFT数据集，不同点提示数（1,3,5,10点），评估mIoU
- **抠图任务**：
  - 训练集：Adobe Image Matting (AIM)、AIM-500
  - 测试集：Composition-1K、Distinction-646
  - 对比方法：
    - trimap-based: I-F, DIM, DCNN, MGMatting, VITMatte
    - trimap-free: LFM, MODNet, MFC-Net
  - 评估指标：SAD（绝对差之和）、MSE（均方误差）
- **消融实验**：在DIS-VD和COIFT（分割）、AM2K和P3M-500（抠图）上验证MVLE和Local-Adapter的有效性；多任务学习实验在DIS-VD（分割）和RefMatte-RW100（抠图）上比较单独训练与联合训练。

## 4. 资源与算力
- **文中未明确说明**：论文未提及GPU型号、数量、训练时长等具体算力信息。仅说明所有SAM参数冻结，新增模块参数仅占1.8%，训练数据来自公开数据集。

## 5. 实验数量与充分性
- **实验数量**：约6大类实验：
  1. 分割基准对比（5个测试子集，6个对比方法，5个指标）
  2. 抠图基准对比（2个数据集，8个对比方法，2个指标）
  3. 零样本交互分割（不同点提示数）
  4. 消融实验（MVLE和LA在分割与抠图上的贡献）
  5. 多任务学习对比
  6. 可视化对比展示
- **充分性**：实验覆盖了主流分割和抠图基准，对比方法涵盖经典SAM变体及专用模型，消融和多任务实验设计合理，展示了模块独立贡献以及联合训练的优势。但未在更多样化场景（如视频）上测试，也未进行模型复杂度与推理速度的详细量化对比。

## 6. 主要结论与发现
- SAMA在分割任务上显著优于SAM、HQ-SAM、Pi-SAM等全部SAM变体，在DIS-VD等子集上接近甚至超过专用模型（如BiRefNet），尤其在边界细节上提升明显。
- 在抠图任务上，无需trimap即达到与领先的trimap-based方法（VITMatte）相当的性能，远超其他trimap-free方法。
- 零样本交互分割（COIFT）中，SAMA在不同点提示数下均优于HQ-SAM和SAM，尤其在少量点提示下提升显著。
- 消融实验证明MVLE和Local-Adapter均对性能有显著贡献；多任务联合训练相比单独训练在两项任务上均有提升，表明分割与抠图可互相促进。

## 7. 优点
- **统一框架首创性**：首次将SAM扩展为同时支持高质量交互分割和抠图的统一模型，无需额外的trimap或后处理模型。
- **轻量高效**：新增仅1.8%参数，冻结SAM主干，训练高效，且保持SAM的提示灵活性（点、框等）。
- **模块设计新颖**：MVLE利用多局部视图增强精细感知；Local-Adapter通过双向跨注意力与置信度融合，在不牺牲零样本能力的前提下细化边界。
- **实验充分、对比公平**：在多数据集上对分割和抠图均进行了系统对比，消融实验清晰验证了各组件贡献，多任务学习实验展示了互惠效应。

## 8. 不足与局限
- **算力信息缺失**：未报告训练所需GPU型号、数量、时长，难以评估可复现性及资源门槛。
- **实验覆盖有限**：仅在高精度分割（DIS）和抠图（Composition-1K等）数据集上评估，未在其他常见分割基准（如COCO、Cityscapes）或视频、医学图像等场景上测试。
- **可能有数据集偏差风险**：训练数据仅来自DIS-5K、ThinObject-5K和AIM等公开数据集，这些数据包含较多细薄/透明物体，可能偏向特定场景，在通用场景下的泛化性未充分验证。
- **推理速度/实时性未评估**：虽然参数增量小，但多局部视图处理和额外注意力可能引入延迟，文中未与基线模型比较推理时间。
- **交互方式限制**：论文仅展示了框提示和点提示的交互分割，未探索文本提示或自由形状提示等SAM支持的其他模式。
- **联合训练细节**：分割和抠图分别训练而非完全联合优化，真正的多任务端到端同步训练是否更优尚未验证。

（完）
