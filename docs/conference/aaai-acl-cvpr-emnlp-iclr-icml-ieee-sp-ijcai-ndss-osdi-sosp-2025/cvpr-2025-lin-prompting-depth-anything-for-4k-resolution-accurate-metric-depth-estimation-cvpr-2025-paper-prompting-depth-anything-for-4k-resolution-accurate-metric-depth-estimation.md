---
title: Prompting Depth Anything for 4K Resolution Accurate Metric Depth Estimation
title_zh: 为4K高分辨率精确度量深度估计提示Depth Anything
authors: "Lin, Haotong, Peng, Sida, Chen, Jingxiao, Peng, Songyou, Sun, Jiaming, Liu, Minghuan, Bao, Hujun, Feng, Jiashi, Zhou, Xiaowei, Kang, Bingyi"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Lin_Prompting_Depth_Anything_for_4K_Resolution_Accurate_Metric_Depth_Estimation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 提示Depth Anything实现4K度量深度估计
tldr: 度量深度估计通常需要大量精确标注，而LiDAR等传感器可提供提示信息。本文首次将提示机制引入深度基础模型，用低成本LiDAR作为提示，在多尺度深度解码器中融合。配合合成数据LiDAR模拟与真实数据伪真值管线，实现4K分辨率的精确度量深度输出，开创了新范式。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1795, \"height\": 620, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1777, \"height\": 547, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 869, \"height\": 515, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1659, \"height\": 1355, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1666, \"height\": 718, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 288, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1793, \"height\": 759, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 870, \"height\": 485, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 860, \"height\": 723, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1526, \"height\": 651, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 427, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lin-prompting-depth-anything-for-4k-resolution-accurate-metric-depth-estimation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 865, \"height\": 217, \"label\": \"Table\"}]"
motivation: Depth Anything模型能给出有力的深度先验，但缺少准确尺度信息，难以直接输出度量深度。
method: 将低成本LiDAR作为提示条件注入Depth Anything的多尺度深度解码器，并设计合成数据LiDAR模拟与真实数据伪真值管线。
result: 实现了高达4K分辨率的精确度量深度估计，缓解了标注数据稀缺带来的训练难题。
conclusion: 首次将提示机制引入深度基础模型，为度量深度估计建立了新范式。
---

## Abstract
Prompts play a critical role in unleashing the power of language and vision foundation models for specific tasks. For the first time, we introduce prompting into depth foundation models, creating a new paradigm for metric depth estimation termed Prompt Depth Anything. Specifically, we use a low-cost LiDAR as the prompt to guide the Depth Anything model for accurate metric depth output, achieving up to 4K resolution. Our approach centers on a concise prompt fusion design that integrates the LiDAR at multiple scales within the depth decoder. To address training challenges posed by limited datasets containing both LiDAR depth and precise GT depth, we propose a scalable data pipeline that includes synthetic data LiDAR simulation and real data pseudo GT depth generation. Our approach sets new state-of-the-arts on the ARKitScenes and ScanNet++ datasets. Furthermore, we demonstrate that it benefits several downstream applications, including 3D reconstruction and generalized robotic grasping. Code will be released.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：单目深度估计模型（如 Depth Anything）虽然具备强大的相对深度预测能力和泛化性，但由于单目尺度模糊（scale ambiguity）问题，难以直接输出高精度的度量深度（metric depth），如图 1(b) 所示，现有方法（如 Metric3D v2）在尺度精度和一致性方面存在明显不足。
- **背景驱动**：
  - 语言与视觉基础模型通过“提示（prompting）”机制能够在特定任务上释放巨大潜力；受此启发，作者首次将提示机制引入深度基础模型。
  - 低成本 LiDAR（如 iPhone 上的 ARKit LiDAR）具备精确的度量尺度信息，且在现代移动设备中普遍存在，使其成为理想且实用的“度量提示”来源。
- **整体含义**：论文提出了 **Prompt Depth Anything** 这一新范式，将度量深度估计视为深度基础模型的下游任务，以低成本 LiDAR 作为提示条件，实现了高达 4K 分辨率的精确度量深度输出，为度量深度估计开辟了新方向。

## 2. 论文提出的方法论

### 核心思想
- 将深度基础模型（Depth Anything v2，ViT encoder + DPT decoder）视为强大的“局部形状学习器”，通过注入低分辨率、带噪声的 LiDAR 深度作为提示信息，为其提供精确的空间尺度信息，从而实现高分辨率度量深度估计。

### 关键技术细节

**（1）Prompt Fusion 架构（多尺度提示融合）**
- 在 DPT 解码器的每个尺度阶段（scale stage）注入 LiDAR 信息。
- 具体流程：
  - 将低分辨率 LiDAR 深度图通过双线性插值调整到当前尺度的空间分辨率。
  - 使用一个浅层卷积网络（两层卷积）提取深度特征。
  - 通过零初始化（zero-initialized）卷积层投影至与图像特征相同的维度。
  - 将融合后的深度特征与 DPT 中间特征逐元素相加，参与后续深度解码。
- 优势：
  - 仅增加约 **5.7%** 的计算开销（1.789 TFLOPs vs 1.691 TFLOPs，输入为 756×1008 图像）。
  - 零初始化保证了训练初期输出与原始基础模型完全一致，完全继承预训练基础模型的能力。

**（2）可扩展数据管线（Scalable Data Pipeline）**
- **合成数据 LiDAR 模拟**：针对 HyperSim 合成数据没有 LiDAR 深度的问题，提出“稀疏锚点插值（sparse anchor interpolation）”方法：
  - 先将 GT 深度缩放到与 iPhone ARKit Depth 相同的低分辨率（192×256）。
  - 在低分辨率图上以步长（实践中为 7）的扭曲网格进行稀疏采样作为锚点。
  - 利用 RGB 相似度 + KNN 插值恢复其余深度值，从而有效模拟 LiDAR 的低分辨率与噪声特性（避免模型退化为单纯的深度超分辨率学习）。
- **真实数据伪 GT 深度生成**：针对 ScanNet++ 的 FARO LiDAR 标注深度存在大量空洞和边缘质量差的问题：
  - 使用 Zip-NeRF 对每个场景进行重建，并利用无模糊帧的 iPhone 视频和 DSLR 高分辨率视频作为输入，渲染出高质量伪 GT 深度。
- **边缘感知深度损失（Edge-aware Depth Loss）**：
  - 综合 FARO 标注 GT（在无纹理平面区域精度高）和 Zip-NeRF 伪 GT（边缘质量高）两者优势。
  - 公式（1）：$L_{edge} = L_1(D_{gt}, \hat{D}) + \lambda \cdot L_{grad}(D_{pseudo}, \hat{D})$，实验中 λ=0.5。
  - 公式（2）：$L_{grad}$ 为深度梯度域的 L1 损失，主要作用于深度边缘区域，引导模型学习精确的薄结构边缘；$L_1$ 分支则保证整体深度准确性。

**（3）深度归一化**
- 将 LiDAR 深度数据按最小/最大值线性缩放到 [0, 1] 区间，网络输出也使用相同缩放参数进行归一化，提高训练收敛稳定性。

**（4）训练策略**
- 从 Depth Anything v2 发布的度量模型初始化，先进行 10K 步 warm-up 微调，使其输出归一化深度；随后训练 200K 步。
- 优化器：AdamW；ViT backbone 学习率 5e-6，其他参数学习率 5e-5；batch size 为 2，使用 8 块 GPU。

## 3. 实验设计

### 数据集与 Benchmark
- **合成数据**：HyperSim（虚拟室内场景，含精确 GT 深度）。
- **真实数据**：ScanNet++（iPhone RGB-LiDAR 数据 + FARO 高精度标注）和 ARKitScenes（iPhone RGB-LiDAR 数据 + 高功率 LiDAR 标注）。
  - ARKitScenes：遵循官方协议，40K 训练图像、5K 评估图像。
  - ScanNet++：从 230 个训练场景约 60K 图像训练，从 50 个验证场景中随机选取 20 个（约 5K 图像）进行验证。
- **评估分辨率**：384×512、768×1024、1440×1920（4K 级别）。
- 评估指标包括：L1、RMSE、AbsRel、δ0.5、TSDF 重建指标（Accuracy、Completeness、Precision、Recall、F-score）等。

### 对比方法
- **单目深度估计（MDE）**：Metric3D v2、ZoeDepth、DepthPro、Depth Anything v1/v2、Marigold、Lotus。
- **深度补全/上采样方法**：BPNet、Depth Prompting（D.P.）、MSPF。
- 对比策略上做了严谨的公平性处理：对 MDE 方法使用 RANSAC 将预测与 ARKit LiDAR 对齐；区分了零样本（zero-shot）与非零样本（non zero-shot）两类设置。

### 下游任务评估
- **3D 重建**：ScanNet++ 上的 TSDF 重建质量评估；同时扩展到室外场景，将提示替换为车辆 LiDAR（Waymo 数据）进行大场景重建。
- **机器人抓取**：使用 ACT 策略，针对漫反射、透明、镜面等多种材质物体测试不同输入信号（Ours 深度 vs LiDAR vs RGB）下的抓取成功率。

## 4. 资源与算力

- 论文中明确给出的训练配置：**8 块 GPU，batch size 为 2，训练 200K 步，加上 10K 步 warm-up**。
- **未明确说明的部分**：
  - 未指定 GPU 型号（如 A100/V100 等）。
  - 未给出总训练时长（天/小时）。
  - 未提供具体参数量估算。

## 5. 实验数量与充分性

### 主要实验组
- **主实验（定量对比）**：ARKitScenes（表 1）和 ScanNet++（表 2）上的多分辨率深度精度对比，以及 ScanNet++ 上的 TSDF 重建对比。
- **消融实验（表 3）**：共 10 组，覆盖以下维度：
  - （a）基线（仅合成数据训练）。
  - （b）移除提示融合 vs（a）。
  - （c）移除基础模型初始化。
  - （d-f）对比不同提示注入架构：AdaLN、Cross-attention、ControlNet。
  - （g）加入 ARKitScenes 数据。
  - （h）加入 ScanNet++ FARO 标注 GT 数据。
  - （i）加入 ScanNet++ 伪 GT 数据（直接监督）。
  - （j）完整方案（伪 GT + 边缘感知损失）。
- **零样本泛化测试**：室内新场景、健身房薄结构、昏暗博物馆、人体和室外环境等多样化场景。
- **传感器可替换性实验**：将 LiDAR 提示替换为车辆 LiDAR 进行户外重建（定性）。
- **应用实验**：机器人抓取成功率（四种物体类型 × 三个位置）。

### 充分性与公平性评估
- **充分**：消融实验覆盖了方法设计的关键模块（提示机制、基础模型、架构设计、数据来源、损失函数），共 10 组，数量充足、逻辑严密。
- **客观公平**：
  - 对 MDE 方法使用 RANSAC 对齐处理，避免尺度偏差带来的不公比较。
  - 区分零样本与非零样本设置，并承认对部分对比方法进行了微调（* 标注）。
  - 消融实验（j）验证了设计选择的必要性。
- **相对不足**：室外车辆 LiDAR 的结果仅提供定性展示，缺乏定量评估。

## 6. 论文的主要结论与发现

- 首次验证了“提示深度基础模型”这一范式在度量深度估计中的可行性，用低成本 LiDAR 作为度量提示，可使深度基础模型输出高精度、高分辨率（最高 4K）的度量深度。
- 提出的 Prompt Fusion 架构简洁高效，仅增加约 5.7% 计算开销，即可有效解决深度基础模型的尺度模糊问题，并完全继承预训练模型的泛化能力。
- 可扩展数据管线（稀疏锚点插值 + Zip-NeRF 伪 GT + 边缘感知损失）有效解决了“同时具备 LiDAR 和精确 GT 的数据集稀缺”这一关键训练瓶颈。
- 在 ARKitScenes 和 ScanNet++ 上达到 SOTA 性能；零样本模型（Ours syn）优于部分非零样本对比方法，体现强泛化性与提示策略的有效性。
- 方法具备良好的可扩展性：基础模型可替换为 DepthPro，LiDAR 提示可替换为车辆 LiDAR。
- 证明精确度量深度可显著提升下游任务性能，包括高质量 3D 重建和泛化机器人抓取（特别是透明/镜面物体场景）。

## 7. 优点

- **范式创新**：首次将“提示”概念引入深度基础模型，为度量深度估计提供了全新的解决思路。
- **实用性强**：基于 iPhone LiDAR 这一普及硬件，方法易于部署到实际设备和机器人系统。
- **设计简洁高效**：prompt fusion 结构轻量（仅 5.7% 额外计算），零初始化策略保证继承基础模型全部先验能力，训练稳定。
- **数据工程贡献突出**：针对不同数据源（合成无 LiDAR、真实有 LiDAR 但 GT 质量差）分别设计了仿真和伪 GT 生成策略，并配套边缘感知损失，解决了实际数据稀缺的核心瓶颈。
- **实验全面且严谨**：覆盖了精度评估、重建质量评估、零样本泛化、传感器可替换性、机器人应用等多个层面，并进行了多组系统消融，公平性处理到位。
- **应用价值明确**：在 3D 重建和机器人抓取两个实际场景中验证了方法的实际收益，尤其对透明/镜面物体的抓取表现突出。

## 8. 不足与局限

- **传感器固有局限**：iPhone LiDAR 在远距离场景下噪声显著，限制了方法在远距离深度估计上的表现。
- **时序稳定性问题**：LiDAR 深度存在时间维度的闪烁，导致预测深度也会出现闪烁现象，论文未给出有效的时序平滑方案。
- **伪 GT 的依赖**：
  - 真实数据伪 GT 依赖 Zip-NeRF 重建质量；在高度无纹理或反射区域，重建伪 GT 本身可能引入误差，虽然边缘感知损失有所缓解，但未完全消除。
  - 合成数据 LiDAR 仿真虽然更接近真实，但与实际传感器噪声特性仍存在 domain gap。
- **实验覆盖不完整**：
  - 室外车辆 LiDAR 实验（图 6）仅展示定性结果，缺乏定量指标。
  - 算力硬件信息（GPU 型号、训练总时长）未明确说明，可复现性信息略有欠缺。
- **方法局限性**：论文仅验证了 DPT 架构的深度基础模型，未覆盖基于扩散模型的深度基础模型（Marigold 等），适用范围存在一定边界。
- **应用局限性**：机器人抓取实验在特定实验室环境下进行，物体类别和场景多样性有限（4 类物体 × 3 个位置），真实世界的复杂多变性尚待验证。

（完）
