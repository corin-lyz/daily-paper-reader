---
title: "DepthCrafter: Generating Consistent Long Depth Sequences for Open-world Videos"
title_zh: DepthCrafter：为开放世界视频生成一致的长时间深度序列
authors: "Hu, Wenbo, Gao, Xiangjun, Li, Xiaoyu, Zhao, Sijie, Cun, Xiaodong, Zhang, Yong, Quan, Long, Shan, Ying"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Hu_DepthCrafter_Generating_Consistent_Long_Depth_Sequences_for_Open-world_Videos_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 开放世界视频中的零样本深度估计
tldr: 开放世界视频深度估计常受视频外观、运动、相机移动和长度多样性的困扰。本文提出DepthCrafter，利用预训练图像到视频扩散模型，通过精心设计的三阶段训练，在不依赖相机位姿或光流的情况下，一次性生成长度可变、最多110帧且细节丰富的时间一致深度序列。该方法在多样视频上展现出优异的零样本泛化能力，为视频深度估计树立了新标杆。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1747, \"height\": 777, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1734, \"height\": 592, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 867, \"height\": 610, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1793, \"height\": 1363, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 863, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 865, \"height\": 297, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 866, \"height\": 375, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1800, \"height\": 391, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 862, \"height\": 190, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hu-depthcrafter-generating-consistent-long-depth-sequences-for-open-world-videos-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 865, \"height\": 204, \"label\": \"Table\"}]"
motivation: 视频深度估计在开放世界中面临外观多样、运动复杂、长度不定等挑战，现有方法难以生成长而一致的深度序列。
method: 从预训练图像到视频扩散模型出发，设计三阶段训练策略，使视频到深度模型可生成变长且一致的深度序列。
result: 一次生成最多110帧的精细深度序列，在开放世界多类型视频上取得领先的泛化性能。
conclusion: 证实了基于扩散模型的视频深度生成路线可兼顾长序列的一致性与细节精度。
---

## Abstract
Estimating video depth in open-world scenarios is challenging due to the diversity of videos in appearance, content motion, camera movement, and length. We present DepthCrafter, an innovative method for generating temporally consistent long depth sequences with intricate details for open-world videos, without requiring any supplementary information such as camera poses or optical flow. The generalization ability to open-world videos is achieved by training the video-to-depth model from a pre-trained image-to-video diffusion model, through our meticulously designed three-stage training strategy. Our training approach enables the model to generate depth sequences with variable lengths at one time, up to 110 frames, and harvest both precise depth details and rich content diversity from realistic and synthetic datasets. We also propose an inference strategy that can process extremely long videos through segment-wise estimation and seamless stitching. Comprehensive evaluations on multiple datasets reveal that DepthCrafter achieves state-of-the-art performance in open-world video depth estimation under zero-shot settings. Furthermore, DepthCrafter facilitates various downstream applications, including depth-based visual effects and conditional video generation.

---

## 论文详细总结（自动生成）

# DepthCrafter：为开放世界视频生成一致的长时间深度序列——论文详细总结

## 1. 核心问题与研究动机

- **背景问题**：单目深度估计是连接2D观测与3D世界的桥梁，在混合现实、AIGC、自动驾驶、机器人等领域有广泛的应用价值。然而，单张图像观测的信息不足以唯一确定场景深度，存在固有的病态性（ill-posed）问题。
- **现有方法的不足**：
  - **单图像深度估计方法**（如Depth-Anything系列、Marigold等）未考虑时间维度，直接应用于视频会产生时间不一致的闪烁伪影（flickering）。
  - **现有视频深度估计方法**（如NVDS、DeepV2D等）通常依赖相机位姿或光流等额外信息，在开放世界视频中这些信息难以获取，且其性能对动态内容比例和相机位姿质量高度敏感。
- **核心挑战**：开放世界视频的多样性（外观、内容运动、相机运动、长度各不相同）使得生成时间一致、细节丰富的长深度序列非常困难。
- **本文方案**：提出 **DepthCrafter**，利用预训练的图像到视频扩散模型（Stable Video Diffusion, SVD），通过精心设计的三阶段训练策略，在不依赖任何附加信息（相机位姿、光流）的情况下，一次性生成最长110帧、时间一致且细节精细的深度序列，并支持任意长度视频的推断。

## 2. 方法论

### 2.1 核心思想

- 将视频深度估计建模为**条件扩散生成问题**，学习条件分布 $p(d | v)$，即以视频帧序列 $v$ 为条件，生成对应的深度序列 $d$。
- 采用**潜在扩散模型（LDM）框架**，在低维潜空间中操作以降低计算开销。
- 利用SVD预训练模型的强泛化能力，通过微调使其适配视频到深度的生成任务。

### 2.2 关键技术细节

**（1）潜空间变换**
- 使用SVD自带的VAE（变分自编码器）编码/解码视频帧，并发现其可**直接用于深度序列**，重建误差可忽略。
- 深度序列复制三通道以适配VAE的3通道输入格式，解码时对三通道输出取平均。

**（2）条件注入机制**
- 将输入视频的潜表示 $z(v)$ 与带噪深度潜表示 $z(d)$ 在**逐帧级别**拼接，而非仅首帧。
- 使用CLIP编码器提取视频帧的高层语义嵌入，通过**逐帧交叉注意力**注入到去噪网络中，确保生成深度与视频内容对齐。

**（3）三阶段训练策略**（核心创新）

| 阶段 | 训练数据 | 序列长度 | 训练范围 | 目的 |
|------|----------|----------|----------|------|
| 阶段1 | 大规模真实数据集（~200K对） | 随机采样 [1, 25] 帧 | 全模型 | 适应视频到深度生成任务，学习变长生成 |
| 阶段2 | 大规模真实数据集 | 随机采样 [1, 110] 帧 | 仅时间层 | 学习长时间上下文，精确布置整体深度分布 |
| 阶段3 | 小型合成数据集（~3K对） | 固定45帧 | 仅空间层 | 学习精细准确的深度细节 |

- **数据集构建**：
  - 真实数据集：从大量双目视频中，利用SOTA视频立体匹配方法BiDAStereo生成时间一致的深度序列，获得约200K对、长度50-200帧的数据。
  - 合成数据集：结合DynamicReplica和MatrixCity，约3K条、150帧、细粒度深度标注。

**（4）超长视频推理策略**
- 将视频划分为**重叠段**（每段≤110帧）。
- 对于重叠帧，使用**前一阶段去噪后的潜表示加噪声**进行初始化，以锚定深度分布的尺度与平移。
- 采用**榫卯式（mortise-and-tenon）潜空间插值策略**拼接相邻段，权重线性递减，保证跨段时间平滑。

## 3. 实验设计

### 3.1 评估数据集（零样本设置）

| 数据集 | 场景类型 | 序列长度 | 深度标注 |
|--------|----------|----------|----------|
| Sintel | 合成、动态场景、多样相机运动 | ~50帧 | 精确密集深度 |
| ScanNet v2 | 室内静态场景 | 90帧 | Kinect传感器深度 |
| KITTI | 室外驾驶场景 | 110帧 | LiDAR稀疏度量深度 |
| Bonn | 室内动态场景 | 110帧 | RGB-D |
| NYU-v2 | 室内单图像 | 1帧 | 稀疏噪声深度 |

### 3.2 评估指标

- **AbsRel**（绝对相对误差）↓：$\frac{1}{N}\sum |\hat{d} - d| / d$
- **δ1**（阈值精度）↑：$max(\hat{d}/d, d/\hat{d}) < 1.25$ 的像素百分比
- 关键区别：采用**整段视频共享的尺度和平移对齐**，而非逐帧对齐，对时间一致性提出更高要求。

### 3.3 对比方法

- 单图像深度估计：Marigold、Depth-Anything、Depth-Anything-V2（使用大模型变体）
- 视频深度估计：NVDS、ChronoDepth

## 4. 资源与算力

- **训练配置**：8块GPU，总训练时间约**5天**。
- **训练分辨率**：320×640（推断时可扩展至576×1024等任意分辨率）。
- **迭代次数**：三个阶段分别为80K、40K、10K次。
- **优化器**：Adam，学习率1×10⁻⁵，batch size为8。
- **推断**：默认5步去噪；在单张NVIDIA A100上，1024×576分辨率下约**466 ms/帧**；110帧段长需约24GB显存，40帧段长可降至12GB。
- **说明**：文中未明确说明GPU型号，仅在后文提及A100用于推断速度测试。

## 5. 实验数量与充分性

### 5.1 实验数量汇总

- **主实验**：5个数据集（Sintel、ScanNet、KITTI、Bonn、NYU-v2）的零样本定量评估，对比5种方法。
- **消融实验**：
  - 三阶段训练有效性（评估各阶段结束时的性能）
  - 推理策略组件有效性（baseline / +初始化 / +初始化&拼接）
- **定性实验**：DAVIS数据集、Sora生成视频、开放世界视频（人类动作、动物、建筑、卡通、游戏），长度90-195帧。
- **下游应用**：雾效模拟、深度条件视频生成（采用Control-A-Video）。
- **补充材料**：更多数据集上的消融和更多视觉效果。

### 5.2 充分性与公平性评价

- **充分性较高**：覆盖合成/真实、室内/室外、静态/动态多种场景，序列长度从50到110帧不等，零样本设置公允。
- **公平性考量**：所有方法使用官方代码，Depth-Anything采用large模型变体以达到最佳性能；Marigold采用LCM版本+5次集成。整段视频共享尺度平移对齐对视频方法更严格，对单图方法不利但也更符合实际需求。
- **局限说明**：NYU-v2单图评估中，DepthCrafter的AbsRel（0.072）逊于Depth-Anything系列（~0.042-0.043），说明其单图精度并非最优，但δ1差距极小且定性结果更精细。

## 6. 主要结论与发现

1. **SOTA性能**：DepthCrafter在4个视频数据集上均取得最优的零样本性能，在KITTI上相比Depth-Anything-V2的AbsRel提升25.7%，在Sintel上提升25%以上。
2. **时间一致性**：从时间剖面（temporal profile）可清晰看到，对比方法（NVDS、Depth-Anything-V2）存在锯齿状伪影（闪烁），而DepthCrafter生成的深度序列平滑一致。
3. **变长支持**：一次生成支持1-110帧可变长度，通过推理策略可扩展到任意长度视频。
4. **双重优势融合**：三阶段训练策略成功融合了真实数据集的丰富内容多样性和合成数据集的精细深度细节。
5. **单图泛化能力**：虽为视频深度估计设计，但在单图像深度估计上也具有竞争力，且定性结果比Depth-Anything-V2更细腻。
6. **下游应用价值**：为雾效模拟、深度条件视频生成等任务提供了高质量、时间一致的深度输入。

## 7. 方法优点

- **无需额外信息**：不依赖相机位姿或光流，在开放世界视频中普适性更强。
- **三阶段训练策略设计巧妙**：
  - 渐进式训练降低了长序列训练的内存消耗（阶段2仅训练时间层）；
  - 分别利用真实和合成数据集的互补优势；
  - 支持可变长度训练，灵活适应不同长度输入。
- **推理策略鲁棒**：噪声初始化锚定深度分布+榫卯式潜空间插值拼接，有效消除了段间闪烁。
- **共享尺度平移对齐**：保证整段视频深度的一致性，而不是逐帧独立归一化。
- **实现扎实**：全面提供项目网站、开源代码，定量定性结果详实。

## 8. 不足与局限

- **计算开销较高**：
  - 扩散模型的迭代去噪过程导致推理速度慢于Depth-Anything-V2（466 vs 180 ms/帧）；
  - 110帧长段需约24GB显存，对硬件要求较高（40帧段可降至12GB）。
- **单图精度非最优**：在NYU-v2单图基准上AbsRel不如Depth-Anything系列，说明专门为单图优化的方法仍有优势。
- **训练数据构建依赖立体匹配**：大规模真实数据集的深度标签由BiDAStereo生成，可能引入该方法的系统性误差，在动态边缘、弱纹理区域可能不够精确。
- **应用限制**：未探讨在自动驾驶等对绝对尺度有要求的场景中的表现（输出为仿射不变深度），需要额外手段恢复度量深度。
- **潜在偏差风险**：训练数据中合成数据的多样性有限（DynamicReplica、MatrixCity），可能对特定类型场景的泛化产生影响。

---

（完）
