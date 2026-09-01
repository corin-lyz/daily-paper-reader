---
title: "FoundationStereo: Zero-Shot Stereo Matching"
title_zh: FoundationStereo：零样本立体匹配
authors: "Wen, Bowen, Trepte, Matthew, Aribido, Joseph, Kautz, Jan, Gallo, Orazio, Birchfield, Stan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wen_FoundationStereo_Zero-Shot_Stereo_Matching_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 面向零样本立体深度估计的基础模型
tldr: 针对立体匹配零样本泛化能力弱的问题，提出FoundationStereo基础模型，构建100万对高真实感合成训练数据并采用自动自清洗流程去除歧义样本，同时设计侧调优特征骨干等组件以增强可扩展性。实验表明其在零样本设置下显著优于现有立体匹配方法，该模型在未见数据上也能保持稳定高质量匹配，向真正的基础模型迈出关键一步。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1786, \"height\": 661, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1713, \"height\": 630, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1802, \"height\": 317, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1761, \"height\": 361, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1781, \"height\": 778, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1720, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 893, \"height\": 547, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1742, \"height\": 113, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 748, \"height\": 461, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 561, \"height\": 485, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 835, \"height\": 428, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 279, \"height\": 167, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wen-foundationstereo-zero-shot-stereo-matching-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 441, \"height\": 314, \"label\": \"Table\"}]"
motivation: 深度立体匹配跨域泛化能力不足，缺少基础模型。
method: 构建大规模合成数据、自动清洗流程，并设计侧调优骨干等架构组件。
result: 零样本立体匹配性能显著优于已有方法。
conclusion: 为立体匹配提供了具备强零样本泛化能力的基础模型。
---

## Abstract
Tremendous progress has been made in deep stereo matching to excel on benchmark datasets through per-domain fine-tuning. However, achieving strong zero-shot generalization - a hallmark of foundation models in other computer vision tasks - remains challenging for stereo matching. We introduce FoundationStereo, a foundation model for stereo depth estimation designed to achieve strong zero shot generalization. To this end, we first construct a large scale (1M stereo pairs) synthetic training dataset featuring large diversity and high photorealism, followed by an automatic self-curation pipeline to remove ambiguous samples. We then design a number of network architecture components to enhance scalability, including a side-tuning feature backbone that adapts rich monocular priors from vision foundation models to mitigate the sim-to-real gap, and long-range context reasoning for effective cost volume filtering. Together, these components lead to strong robustness and accuracy across domains, establishing a new standard in zero-shot stereo depth estimation. Project page: https://nvlabs.github.io/FoundationStereo

---

## 论文详细总结（自动生成）

根据您提供的论文内容，我生成了以下结构化中文总结：

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：深度立体匹配方法在基准数据集上通过按域微调（per-domain fine-tuning）已取得卓越成绩，但在**零样本通用化（zero-shot generalization）**方面仍面临巨大挑战——即在不进行目标域微调的情况下，对未见过的新场景进行准确、鲁棒的视差估计。
- **研究背景**：视觉基础模型（Vision Foundation Models）在其他任务（如分割、单目深度估计）中凭借“规模定律”展现出强大泛化能力，而立体匹配领域主要方法仍依赖目标域微调，缺乏能作为“开箱即用”解决方案的基础模型。现有跨域泛化方法（如DSMNet、Mask-CFNet等）受限于：
  - 训练数据规模小（多在仅有4万对的Scene Flow上训练）；
  - 网络结构缺乏全局上下文推理能力，难以有效扩展。
- **目标**：构建一个**立体匹配基础模型**，实现强零样本泛化能力，无需按域微调即可在多种真实场景下达到甚至超越现有微调方法的性能。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 2.1 整体架构（前端特征提取 → 代价体构建与过滤 → 迭代优化）
- 采用 **STA（Side-Tuning Adapter）** 进行单目基础模型特征适配 → 构建 **混合代价体（Hybrid Cost Volume）** → **AHCF（Attentive Hybrid Cost Filtering）** 过滤代价体 → 初始视差预测 → **多级 GRU 迭代细化**。

### 2.2 Side-Tuning Adapter（STA）——单目先验适配模块
- **核心思想**：利用在大规模真实单目图像上预训练的基础模型（DepthAnythingV2 [78]）提取的丰富几何/语义先验，弥补合成训练数据与真实域之间的 sim-to-real gap。
- **关键设计**：
  - 采用 **EdgeNeXt-S** 作为CNN骨架提取多级金字塔特征（1/4、1/8、1/16、1/32尺度）；
  - 将输入图像调整至可被14整除后送入冻结的 DepthAnythingV2，并取最终输出头之前的特征（1/4尺度），与CNN同尺度特征拼接，得到“混合特征”；
  - 左右图像共享 STA 权重；上下文特征同样经由 STA 提取，用于初始化并引导 GRU 迭代。
- **设计选择的显著性**：论文比较了三种变体（a）直接用 DPT 头特征、（b）类似 ViT-Adapter 双向交换特征、（c）将 ViT 最终头前特征与 CNN 特征拼接。实验表明简单设计（c）在立体匹配任务上显著优于其他选择。

### 2.3 Attentive Hybrid Cost Filtering（AHCF）——注意力式混合代价体过滤
- **混合代价体构建**（公式1）：结合**分组相关（Group-wise Correlation）** 与**特征拼接（Concatenation）**两种方式：
  - 组间相关：将特征按通道分为G=8组，进行L2归一化后计算逐组点积；
  - 特征拼接：保留单目先验信息，将左右特征沿通道拼接。
- **轴向-平面卷积（Axial-Planar Convolution, APC）**：
  - 将标准 3×3×3 三维卷积**解耦为空间维卷积（Ks×Ks×1）和视差维卷积（1×1×Kd）** 两个步骤，各自跟随 BatchNorm 和 ReLU；
  - 优势：在保持强大表达力的同时显著降低显存消耗，支持更大的感受野（尤其对视差维），克服大视差高分辨率下显存不足的瓶颈。
- **视差 Transformer（Disparity Transformer, DT）**：
  - 先用 4×4×4 stride=4 的三维卷积下采样代价体，再沿**视差维度**进行 token 序列化；
  - 采用 FlashAttention 进行多头自注意力（4个头），并加入位置编码；输出经三线性插值恢复到原尺寸后与沙漏网络输出求和；
  - **关键发现**：仅在视差维度做注意力优于对完整4D代价体做全局注意力——因为4D代价体空间极大，难以训练，而视差维注意力已足够提供全局上下文。
- **初始视差预测**：对过滤后的代价体沿视差维做 Softmax 后再加权求和（soft-argmin），得到1/4分辨率的初始视差。

### 2.4 迭代细化
- 基于 RAFT 风格的多级 GRU 更新架构，每级使用不同分辨率（1/4、1/8、1/16）的隐藏状态；
- 每次迭代结合从过滤后代价体和相关代价体中查表（lookup）获取的代价特征、当前视差、STA 提取的上下文特征作为 GRU 输入；
- 使用**三重 GRU 层级**进行从粗到细的状态更新，并通过基于 attention 的频率选择机制融合不同频率信息；
- 最终通过凸采样（convex sampling）将视差上采样至全分辨率。

### 2.5 损失函数
- 结合 Smooth L1 损失监督初始视差 + 按指数权重加权的 L1 损失监督 K 次迭代输出（权重衰减因子 γ=0.9）。

### 2.6 大规模合成训练数据集（FoundationStereo Dataset, FSD）与自清洗流程
- **FSD 特点**：基于 NVIDIA Omniverse 构建的 **100万对立体图像**，远超现有合成数据集（Scene Flow 4万对，CREStereo 20万对等）；
- **域随机化（Domain Randomization）**：随机立体基线、焦距、相机视角、光照、物体配置；
- **高真实感**：采用高质量3D资产、路径追踪渲染，覆盖室内/室外/驾驶等多种场景，并刻意设计反射、低纹理、遮挡等立体匹配难点；
- **自动迭代自清洗（Iterative Self-Curation）**：
  - 先用 FSD 训练初始模型，在 FSD 上评估，将 BP-2 误差大于 60% 的样本视为“歧义样本”并替换为新生成样本；
  - 训练与清洗交替进行（论文中迭代两次），同时更新数据集和模型权重。

## 3. 实验设计：数据集、基准与对比方法

- **零样本泛化评估基准（4个公共真实数据集）**：
  - **Middlebury**（室内，高精度真值，半分辨率上评估）；
  - **ETH3D**（含室内外灰度图像）；
  - **KITTI 2012 / KITTI 2015**（真实驾驶场景，LiDAR 稀疏真值）；
  - 指标：BP-2、BP-1、D1、EPE。
- **对比方法（零样本场景）**：CREStereo++、DSMNet、Mask-CFNet、HVT-RAFT、RAFT-Stereo、Selective-IGEV、IGEV、Former-RAFT-DAM、IGEV++、NMRF 等。共分两组：仅用 Scene Flow 训练的方法（第一组）；允许使用除目标域以外的任意数据集训练的方法（第二组）。
- **In-the-Wild 定性对比**：与 CroCo v2、CREStereo、IGEV、Selective-IGEV 的已发布最佳 checkpoint 比较，涵盖 DROID 机器人场景、室内外自定义拍摄等，包括反射、半透明、重复纹理、强光照、薄结构等挑战。
- **域内（In-Domain）对比**：
  - **Scene Flow**：按官方划分训练/测试，对比 LEAStereo、GANet、ACVNet、IGEV-Stereo、NMRF、MoCha-Stereo、Selective-IGEV；
  - **ETH3D leaderboard**：对比 GMStereo、HITNet、EAI-Stereo、RAFT-Stereo、CREStereo、IGEV-Stereo、CroCo-Stereo、MoCha-Stereo、Selective-IGEV（其中多数已使用 ETH3D 训练集微调）。
- **消融实验**：
  - **STA 模块消融（8组）**：不同基础模型（DINOv2-L vs DepthAnythingV2 不同规格）、三种STA设计变体、是否冻结 ViT；
  - **AHCF 模块消融（15组）**：不同位置编码（RoPE vs Cosine）、不同特征尺度、全代价体注意力 vs 视差维注意力、不同的 DT 放置位置（pre/post/parallel）、不同 APC 卷积核尺寸（视差维从5到21）；
  - **整体模块消融（5组）**：逐个加入 STA、APC、DT 的累积效果；
  - **数据集消融（2组）**：是否加入 FSD 数据集对最终模型的影响。

## 4. 资源与算力

- **训练配置**：详见论文第6节实现细节——使用 **32 张 NVIDIA A100 GPU** 进行分布式训练；
- **训练规模**：200K 训练步，batch size = 128（平均每GPU 4张）；优化器为 AdamW，初始学习率 1e-4，在80%训练处衰减为原来的 0.1；
- **训练数据**：混合 FSD (100万对) + Scene Flow + Sintel + CREStereo + FallingThings + InStereo2K + Virtual KITTI 2；
- **图像尺寸**：随机裁剪至 320×736，GRU 迭代 22 次；
- **推理时间**：论文在局限部分说明，在 A100 上 375×1242 分辨率耗时约 0.7 秒，尚未针对效率优化。
- 注：论文未明确披露总训练时长（墙钟时间）。

## 5. 实验数量与充分性

- **实验数量较大且覆盖面广**：
  - 4个公共数据集的零样本量化对比（Tab. 2）；
  - 多场景 in-the-wild 定性对比（Fig. 5）；
  - Scene Flow 域内对比（Tab. 3）；
  - ETH3D leaderboard 对比（含微调与零样本两种评估模式）（Tab. 4）；
  - 共 **约30组消融实验**（Tab. 5-7），覆盖了 STA 设计、AHCF 内部各组件、以及 FSD 数据贡献。
- **充分性优势**：
  - 消融实验系统地隔离了各模块的贡献，设计选择都有定量依据，严谨且信息丰富；
  - 零样本评估的数据集多样性良好（室内/室外/驾驶/灰度/彩色）；
  - 与 SOTA 方法的对比包含了仅用 Scene Flow 训练与用多数据集训练两种协议，兼顾公平性与实用性。
- **潜在不足**：
  - 消融实验主要在缩小的100K FSD子集上训练，与最终模型在200K步完整数据上训练的效果之间存在一定偏差风险；
  - 部分对比方法（如第一组仅用 Scene Flow 训练的方法）在训练数据规模上与本文不一致，公平性对比主要是针对训练协议的差异；
  - In-the-wild 评测缺少定量指标，只有定性可视化。

## 6. 论文的主要结论与发现

- **零样本性能显著超越现有方法**：仅用 Scene Flow 训练，FoundationStereo 在 Middlebury、ETH3D、KITTI-12、KITTI-15 上全面优于对比方法（如 Middlebury BP-2: 5.5 vs 最佳对比 7.5）；使用全部可用数据训练后，零样本结果更是大幅领先（如 Middlebury BP-2: **1.1**，ETH3D BP-1: **0.5**），接近甚至超过部分方法的微调性能。
- **域内性能同样优异**：Scene Flow 上 EPE 从 0.41 降至 **0.34**；ETH3D 上微调后排名 leaderboard 第1（BP-0.5: 1.26、BP-1: 0.26），且零样本推理（BP-0.5: 2.31）已优于多数经过微调的对比方法。
- **STA 的有效性**：利用冻结的 DepthAnythingV2 单目先验特征显著提升零样本泛化，尤其在光照异常、无纹理等歧义区域；而解冻 ViT 会破坏预训练先验，导致性能下降。
- **AHCF 的有效性**：APC 在分离空间/视差卷积后显著扩大感受野并节约显存；DT 在视差维度做全自注意力足以提供长距离上下文，优于对完整4D代价体的注意力。
- **数据规模的价值**：加入 100 万对的 FSD 数据集对最终模型性能有明显提升，验证了数据规模对基础模型训练的重要性。
- **自清洗流程有效**：自动剔除歧义样本（严重重复纹理、反射、纯色等）可提高数据集质量与模型鲁棒性。

## 7. 优点

- **系统性地解决零样本泛化问题**：数据（大规模+自清洗）+ 架构（单目先验适配+长距离代价体推理）双管齐下，方法完整且有说服力。
- **创新性的架构设计**：
  - APC 对3D卷积的轴向-平面解耦是一个实用且内存友好的创新，显存瓶颈下仍能扩大感受野；
  - DT 仅在视差维做注意力，以相对低的计算成本获得长距离上下文，消融验证了该设计选择的有效性；
  - STA 简单而有效，将单目视觉基础模型的先验迁移到立体任务，且使用冻结模型保证了先验不被破坏。
- **高质量的大规模数据集**：FSD 是迄今最大的合成立体训练集，覆盖场景广、真实感强、相机参数随机化多样；迭代自清洗的思想具有通用借鉴价值。
- **实验充分严谨**：消融实验覆盖所有核心设计决策，且有定量支撑；零样本评测在多个标准基准上进行，结果可信度高。

## 8. 不足与局限

- **效率未优化**：模型推理耗时约0.7秒（A100，375×1242），未达到实时性要求，论文也承认未针对效率优化，未来需结合知识蒸馏和剪枝；
- **透明物体覆盖有限**：数据集中透明物体数量有限，影响对透明/半透明场景的鲁棒性，文中将这一限制视为数据多样性不足的问题；
- **资源门槛高**：需要32张A100 GPU进行200K步训练，对一般研究团队复现构成较高资源门槛；
- **消融训练配置与最终模型不完全一致**：消融用100K子集，与完整100万对训练之间的规模差异可能使结论存在一定程度的外推风险；
- **In-the-wild 仅定性展示**：缺少在真实开放场景上的定量评估指标，说服力略有折扣；
- **零样本评估仍有灰度域偏差**：ETH3D 为灰度图像，与彩色训练图像之间的域差可能影响在该基准上的表现解读。

（完）
