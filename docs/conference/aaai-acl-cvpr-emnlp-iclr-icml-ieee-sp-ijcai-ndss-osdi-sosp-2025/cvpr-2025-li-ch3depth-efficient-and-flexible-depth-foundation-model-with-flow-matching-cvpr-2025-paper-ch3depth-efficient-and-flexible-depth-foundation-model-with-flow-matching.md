---
title: "CH3Depth: Efficient and Flexible Depth Foundation Model with Flow Matching"
title_zh: CH3Depth：基于流匹配的高效灵活深度基础模型
authors: "Li, Jiaqi, Wang, Yiran, Zheng, Jinghong, Zhang, Junrui, Shen, Liao, Liu, Tianqi, Cao, Zhiguo"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Li_CH3Depth_Efficient_and_Flexible_Depth_Foundation_Model_with_Flow_Matching_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 使用流匹配的高效灵活深度基础模型
tldr: 深度估计基础模型需要在细节、时序一致性和效率上同时表现出色，而现有模型往往难以兼顾。本文提出CH3Depth，通过将流匹配目标重构为InDI提升精度，设计非均匀采样减少采样步数，并提出潜在时序稳定模块增强时序一致性。实验表明，该模型在多个深度基准上以更高效率取得细致且稳定的深度预测。其统一优化思路为高效深度基础模型提供了新方向。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1804, \"height\": 969, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1804, \"height\": 808, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1797, \"height\": 736, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1801, \"height\": 468, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1711, \"height\": 831, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 840, \"height\": 488, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 851, \"height\": 318, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 840, \"height\": 268, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 785, \"height\": 427, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-ch3depth-efficient-and-flexible-depth-foundation-model-with-flow-matching-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 811, \"height\": 357, \"label\": \"Table\"}]"
motivation: 现有深度基础模型难以同时兼顾细节精度、时序一致性和运行效率，亟需统一的优化框架。
method: 将流匹配目标重构为InDI以提升精度，引入非均匀采样降低采样步数，并设计潜在时序稳定模块增强时序一致性。
result: 在多个深度基准上以较少采样步数达到细致、稳定的深度预测，效率与精度均优于现有基础模型。
conclusion: 流匹配结合InDI重定义与高效采样策略，能够构建兼具细节和效率的深度基础模型。
---

## Abstract
Depth estimation is a fundamental task in 3D vision. An ideal depth estimation model is expected to embrace meticulous detail, temporal consistency, and high efficiency. Although existing foundation models can perform well in certain specific aspects, most of them fall short of fulfilling all the above requirements simultaneously. In this paper, we present CH_3Depth, an efficient and flexible model for depth estimation with flow matching to address this challenge. Specifically, 1) we reframe the optimization objective of flow matching as the Inversion by Direct Iteration (InDI) to improve accuracy. 2) To enhance efficiency, we propose non-uniform sampling to achieve better prediction with fewer sampling steps. 3) We design the Latent Temporal Stabilizer (LTS) to enhance temporal consistency by aggregating latent codes of adjacent frames, enabling our method to be lightweight and compatible for video depth estimation. CH_3Depth achieves state-of-the-art performance in zero-shot evaluations across multiple image and video datasets, excelling in prediction accuracy, efficiency, and temporal consistency, highlighting its potential as the next foundation model for depth estimation.

---

## 论文详细总结（自动生成）

# CH3Depth：基于流匹配的高效灵活深度基础模型（CVPR 2025）

## 1. 论文的核心问题与整体含义

- **研究背景**：深度估计是3D视觉中的基础任务，理想模型应同时满足三个关键需求：
  - **细节精度**：预测结果需保留高频细节，支撑内容生成等应用；
  - **时序一致性**：在视频序列中保持深度预测的稳定性，避免闪烁；
  - **高效性**：降低推理开销，使模型能部署于机器人、自动驾驶等资源受限平台。
- **现有方法的不足**：
  - Lotus、Depth Anything 等可预测精细细节，但在视频上会产生闪烁；
  - Marigold 等基于扩散模型，采样步骤多、推理耗时严重；
  - NVDS 等视频一致性方法以牺牲空间细节为代价；
  - 已有方法难以在细节、一致性和效率三个方面同时取得良好表现。
- **核心问题**：如何通过一个统一框架同时实现高效、细致且时序一致的深度估计。
- **整体含义**：论文提出 CH3Depth——基于流匹配（Flow Matching）的深度估计基础模型，通过统一优化目标、采样策略与潜在空间时序稳定模块，在单一框架中同时支持图像和视频深度估计，并宣称在精度、效率和一致性上均达到当前最优水平。

## 2. 论文提出的方法论

### 2.1 核心思想
CH3Depth 将深度估计建模为流匹配生成任务：以 RGB 图像的潜在编码为条件，从噪声分布逐步还原深度图的潜在编码。在此基础上引入三项关键技术——**InDI 流匹配**、**非均匀采样**和**潜在时序稳定器（LTS）**。

### 2.2 InDI 流匹配（Inversion by Direct Iteration）

- **传统流匹配（FM-OT）**：固定预测从初始高斯噪声到目标分布的全局速度场，公式为：
  - 加噪路径：ϕ_t(z_d) = t·z_d + (1−t)·ε；
  - 优化目标：L_FM = E_t‖N_fm(ϕ_t(z_d), z_c, t) − (z_d − ε)‖²。
  - 问题：无论当前去噪进度如何，网络都需要预测全局路径，浪费网络容量。
- **InDI 流匹配**：将优化目标改写为预测从当前分布到目标分布的局部最优传输方向：
  - L_InDI = E_t‖N_fm(ϕ_t(z_d), z_c, t) − (ϕ_1(z_d) − ϕ_t(z_d))‖²。
  - 本质上等价于给不同噪声水平的训练样本分配不同的 ELBO 系数（乘以 1−t），让网络更关注困难初始去噪阶段，从而提升精度与细节。

### 2.3 非均匀采样（Non-uniform Sampling）

- 传统方法采用均匀时间步进行数值积分（如等间隔 ODE 求解）。
- CH3Depth 采用**凹函数映射** f(·) 调整各采样步的步长系数：
  - 初始阶段用大步长实现快速收敛；
  - 后期阶段用小步长进行细节微调；
- 公式为：ϕ̂_1 = ϕ_0 + Σ_{s=0}^{S−1} [f(t + 1/S) − f(t)] / (1−t) · N_fm(ϕ̂_t, z_c, t)。
- 实验表明，凹函数 f(x) = x^(1/2) 在两步采样下的精度/效率平衡最优。

### 2.4 潜在时序稳定器（Latent Temporal Stabilizer, LTS）

- **结构**：一个轻量级无条件的 UNet，输入为滑窗内 w 个相邻帧的初始深度潜在码（各加噪后）与当前帧图像潜在码的拼接，输出为时序一致的深度潜在码。
- **特点**：在潜在空间直接操作，不需要解码成深度图再计算一致性损失，显著节省内存。
- **训练方式**：两阶段训练——
  - 先在合成视频数据集上使用标准流匹配损失；
  - 再加入自然场景视频数据（WSVD、VDW），为应对自然场景标注不精确的问题，提出**时序一致偏差损失（Temporal Consistent Deviation Loss）**：只要求 LTS 预测与真值保持一致的偏差（即相邻帧预测误差相近），避免直接学习有噪声的标签。

### 2.5 总体流程

- 输入 RGB → VAE 编码器压缩为潜在码 z_c；
- 将 z_c 与噪声深度潜在码拼接输入流匹配网络 N_fm，通过非均匀采样（S 步）迭代去噪，得到初始深度预测 z_init^d；
- 视频场景下，LTS 以滑窗方式聚合相邻帧的初始深度潜在码，输出时序稳定的最终深度潜在码 ẑ_d；
- 经 VAE 解码器输出深度图。

## 3. 实验设计

### 3.1 训练数据集
- **图像深度估计（N_fm）**：
  - 基础版本（74K 样本）：Hypersim（54K）+ Virtual Kitti 2（20K），与 Marigold 一致；
  - 扩展版本（96K 样本）：额外增加从 TartanAir、SHIFT、MVS-Synth 中随机采样的 22K 户外样本。
- **视频时序稳定器（LTS）**：
  - 合成数据：DynamicStereo、SPRING、TartanAir、IRS 等；
  - 自然场景数据：VDW、WSVD（配合时序一致偏差损失）。

### 3.2 评估数据集与 Benchmark

| 任务 | 数据集 | 指标 |
|---|---|---|
| 图像深度估计（零样本） | NYUv2、KITTI、ETH3D、ScanNet | AbsRel、δ1 |
| 视频深度估计 | Sintel、NYUv2 | AbsRel、δ1、OPW（时序一致性） |

### 3.3 对比方法
- **图像深度估计**：判别式模型（DiverseDepth、MiDaS、LeReS、Omnidata、HDN、DPT、Metric3D、Depth Anything）与生成式模型（Marigold、DepthFM、GeoWizard、Lotus）——对比条件与 Marigold 相同的零样本评估协议。
- **视频深度估计**：DPT、Marigold、DepthFM、Robust-CVD、ST-CLSTM、NVDS、ChronoDepth、DepthCrafter 等。
- **效率对比**：Marigold、DepthFM、ChronoDepth、DepthCrafter，在单张 NVIDIA A6000 GPU 上评测。

## 4. 资源与算力

- **论文未明确说明训练所使用的 GPU 型号、数量及总训练时长**，仅在实现细节中给出：
  - 有效迭代次数：4K，批量大小：128；
  - Adam 优化器，初始学习率 3×10⁻⁵，3K 迭代后降至 3×10⁻⁷。
- 推理效率评测在单张 NVIDIA A6000 GPU 上进行（如表 3 所示）：CH3Depth 单步推理约 0.25s（1 step）、0.36s（2 steps）、0.43s（3 steps）——但该时间未包括 VAE 编解码等完整流程。

## 5. 实验数量与充分性

### 实验数量概览
论文共包含**6 组以上的定量实验**和**多组定性可视化**：

| 实验类型 | 具体内容 |
|---|---|
| 图像深度评估 | 4 个数据集上的零样本评测，2 种训练数据版本（74K/96K） |
| 视频深度评估 | 2 个数据集上的精度与一致性评测 |
| 效率对比 | 5 种生成式模型的参数量与推理时间对比 |
| 组件消融 | InDI、非均匀采样、LTS、时序一致偏差损失的逐项消融（表 4） |
| 采样策略消融 | 均匀/凸函数/凹函数在不同步数下表现（表 5） |
| LTS 消融 | 窗口大小（w=3/4/5）对比 + LTS 迁移到 Marigold/DepthFM 的效果（表 6） |
| 定性对比 | 图像、视频、时序切片等可视化 |

### 充分性与公平性评价
- **优点**：
  - 消融实验完整覆盖了每一项贡献，且逐项给出量化增益；
  - 与 Marigold、DepthFM 等采用相同训练数据和网络架构进行对比，能较公平地验证 InDI 流匹配带来的增益；
  - 不同训练数据规模（74K/96K）分别汇报，区分了数据量和方法贡献的影响。
- **可改进之处**：
  - 与判别式模型（如 Depth Anything V2、Metric3D）的对比缺乏同训练数据规模的公平条件（判别模型训练数据量比 CH3Depth 高 1～2 个数量级）——论文对此有所承认；
  - 未与更多近期的生成式深度模型（如 Depth Anything V2 的生成版本、多视角方法等）对比，覆盖范围有限；
  - 视频一致性实验仅用 OPW 单一指标，且 Sintel/NYUv2 的"视频"属性有限（多为短片段），对长视频一致性的验证不够充分；
  - 无专门的鲁棒性实验（如低光照、运动模糊、遮挡场景）来检验极限表现。

## 6. 论文的主要结论与发现

1. **InDI 流匹配显著优于 FM-OT**：在相同数据和架构下，CH3Depth 在 NYUv2 上 AbsRel 较 DepthFM 降低约 20%（6.5→5.2），在 KITTI 上 δ1 较 Marigold 提升 2.5 个百分点。
2. **非均匀采样可同时提升效率与精度**：效果优于均匀采样，启发式凹函数(如 f(x)=x^(1/2))在减少采样步数时不损精度、甚至提升精度——2 步采样即超越 DepthFM 的 3 步和 Marigold 的 50 步。
3. **潜在空间时序稳定器轻量有效**：LTS 将视频深度估计的 OPW 误差从 30.7 降至 9.5，获得与 SOTA 视频深度方法（DepthCrafter、ChronoDepth）相当的一致性，但参数量更少、模块更加简洁（CH3Depth 的 2.7× 提速 vs DepthFM）。
4. **LTS 具有良好迁移性**：可直接作为后处理模块增强其他生成式深度模型（Marigold、DepthFM）的时序一致性，OPW 显著下降。
5. **统一框架可行**：CH3Depth 同时兼容图像与视频深度估计，并在两类任务上都达到生成式方法中 SOTA，证明"精度—效率—一致性"三者可在一个框架内取得更好平衡。

## 7. 优点

- **方法创新性**：将 InDI 与流匹配结合用于深度估计，在理论上给出与 FM-OT 的等价变换关系（乘以 1−t），分析扎实；非均匀采样思路简单有效，易于推广到其他生成式感知任务。
- **统一架构高灵活性**：一个网络架构同时支持图像和视频深度估计，且 LTS 是即插即用的"社区贡献"模块，可迁移至其他模型。
- **训练流程精简高效**：从图像模型收敛后训练 LTS 的两阶段流程简洁清晰，不需引入额外的大型视频扩散模型，极大降低了视频深度估计的复杂度。
- **效率与精度兼顾**：量化对比表明其在同等效果下推理速度远快于 Marigold、DepthFM 等主要基线，实用性更强。
- **消融实验设计合理**：逐项验证各组件贡献，并报告不同数据规模下的性能差异，结果透明度高。

## 8. 不足与局限

- **自述局限**：论文承认在**镜面反射、极端光照**等具有挑战性的场景中，CH3Depth 可能产生失败预测——这是所有生成式深度模型面临的共性问题，但论文未提出针对性的鲁棒性改进或分析。
- **实验覆盖不足**：
  - 缺少对真实世界复杂场景（如反射、透明物体、动态物体）的针对性评测；
  - 高分辨率输入的深度细节表现没有专项实验验证；
  - 视频评测数据集（Sintel、NYUv2）的时序长度和场景多样性有限，长视频的稳定性证据不够充分——虽然这受到现有公共视频深度数据集的限制。
- **训练数据规模有限**：相比 Depth Anything（63.5M）和 Metric3D（8M 以上）等判别式基础模型，CH3Depth 的训练数据约 96K（结合 LTS 后总样本量相对增加），在零样本泛化能力上仍有差距，与判别式模型的对比在数据规模上不对等。
- **训练流程可能导致偏差**：两阶段流水线中，LTS 在初版 N_fm 输出上训练；若未来 N_fm 更新，LTS 需一并重新训练——级联误差风险没有被充分分析评估。
- **计算资源不透明**：论文未报告训练所需的 GPU 配置和时间，使他人难以评估复现成本和实际资源需求。
- **时间测量不完整**：表 3 的推理时间若干环节划分方式不够明确，若包含 VAE 编解码的完整流程，真实耗时会更长，跨方法比较的公平性有一定风险。
- **应用限制**：KITTI 等自动驾驶相关数据集的评估是**相对/尺度对齐**协议，不能直接体现绝对尺度精度；对于后续在真实机器人等场景中的部署，尺度性能仍然是一个关键风险。

（完）
