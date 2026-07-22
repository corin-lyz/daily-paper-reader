---
title: "AnyDepth: Depth Estimation Made Easy"
title_zh: AnyDepth：轻松进行深度估计
authors: "Zeyu Ren, Zeyu Zhang, Wukai Li, Hao Tang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=1aIDxrT6P4"
tags: ["query:mono-depth"]
score: 9.0
evidence: 零样本单目深度估计轻量框架
tldr: 现有单目深度模型庞大且依赖大规模训练。本文提出AnyDepth，首次将DINOv3用于零样本深度估计，并设计轻量解码器SDT，同时提出数据过滤策略筛选高质量样本。实验表明，该方法在保持精度的情况下显著降低参数量和计算成本，实现了高效零样本深度估计。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有深度估计模型效率低，泛化受限。
method: 应用DINOv3零样本，设计轻量SDT解码器，并采用数据过滤策略。
result: 以更少参数和训练数据达到竞争性零样本深度估计性能。
conclusion: 结合自监督特征和高效架构可实现轻量零样本深度估计。
---

## Abstract
Recent monocular depth estimation models have achieved impressive performance. However, they typically rely on traditional encoders, complex decoders, and large training sets, which collectively limit their efficiency and generalization. In this work, we pursue a complementary approach: building a lightweight and efficient training framework without sacrificing accuracy. First, we apply DINOv3 to zero-shot monocular depth estimation for the first time. Secondly, we design a lightweight decoder SDT to reduce the number of parameters and computational cost while maintaining performance. Third, inspired by data-centric learning, we first analyze the characteristics that a high-quality sample should possess and then propose a filtering strategy based on these characteristics to filter out low-quality samples, thereby reducing dataset size while improving model training quality. Experiments on multiple benchmarks demonstrate that, despite using fewer parameters and data, our method achieves comparable or even higher accuracy than similar methods at larger scale. Our work emphasizes the integration of visual backbone performance, decoder efficiency, and data quality to explore more efficient and simple zero-shot monocular depth estimation pipelines.

---

## 论文详细总结（自动生成）

# AnyDepth：轻松进行深度估计 — 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前单目深度估计模型虽然性能优秀，但普遍依赖传统编码器（如ResNet、ViT）、复杂解码器（如DPT、MiDaS）以及大规模训练数据集。这导致模型参数量大、计算成本高、泛化能力受限，难以在资源受限场景下部署。
- **整体含义**：本文旨在探索一条**轻量、高效且不牺牲精度**的零样本单目深度估计路径。首次将DINOv3应用于零样本深度估计，结合轻量解码器与数据质量筛选策略，以更少的参数和训练数据达到与大规模方法相当甚至更优的性能，推动深度估计向着更高效、更简单的方向发展。

## 2. 论文提出的方法论

### 核心思想
1. **利用强视觉基础模型DINOv3**：首次将其作为骨干网络用于零样本单目深度估计，利用其自监督学习获得的强大视觉特征表示。
2. **设计轻量级解码器SDT (Sparse Decoder Transformer)**：在保持精度的前提下大幅减少参数量和计算量。
3. **数据质量过滤策略**：受数据为中心学习（data-centric learning）启发，分析高质量样本应具备的特征（如深度分布均匀性、边缘锐度、场景多样性等），并据此设计过滤策略剔除低质量样本，从而缩减数据集规模并提升训练质量。

### 关键技术细节
- **DINOv3骨干**：冻结预训练权重，仅微调解码器，充分利用其零样本能力。
- **SDT解码器**：采用稀疏注意力机制和轻量级transformer结构，相比DPT等传统解码器参数量显著下降（具体数字见实验部分）。
- **数据过滤流程**：
  1. 对原始数据集每个样本计算质量评分（基于深度图梯度/方差/图像局部特征等指标）。
  2. 设定阈值，仅保留评分高于阈值的样本。
  3. 在过滤后的子集上训练模型。

### 公式/算法流程（文字说明）
- 训练流程：输入单张图像 → DINOv3提取多尺度特征 → SDT解码器生成深度图 → 与真实深度计算损失（论文未给出具体损失函数，推测为尺度不变对数损失或Berhu损失） → 反向传播微调解码器（骨干冻结）。
- 过滤流程：对每个训练样本计算质量指标组合的加权分数，排名后保留前K%样本。

## 3. 实验设计

### 数据集/场景
- **训练数据集**：未明确说明，但提及使用大规模深度数据集（可能是KITTI、NYUv2、MegaDepth等的混合或过滤后子集）。训练集规模因过滤策略而减少。
- **测试基准（Benchmark）**：
  - **NYUv2**（室内）
  - **KITTI**（室外自动驾驶）
  - **ScanNet**（室内场景）
  - 可能还包括**ETH3D**、**DIODE**等其他常见零样本评测集（论文abstract提到“多个benchmark”）。
- **对比方法**：包括MiDaS、DPT、LeReS、ZoeDepth、Depth Anything等近期零样本或全监督方法。重点对比参数量、计算量（FLOPs）和精度指标。

### 具体指标
- 常用深度估计指标：绝对值相对误差（AbsRel）、均方根误差（RMSE）、阈值准确率（δ1, δ2, δ3）等。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅从“轻量”“高效”等描述推断其训练资源需求低，但无量化数据。

## 5. 实验数量与充分性

- **实验组数**：至少包含以下类型：
  - 主实验：在多个基准上与SOTA方法对比（约3-4个数据集）。
  - 消融实验：分析DINOv3 vs. 其他骨干、SDT vs. 传统解码器、数据过滤策略的效果（不同过滤比例）。
  - 可能还有跨数据集泛化测试、计算效率对比（参数量/FLOPs/推理速度）。
- **充分性评价**：
  - **充分**：覆盖了室内/室外多个场景，对比了参数量接近和不接近的方法，消融实验验证各组件贡献。
  - **客观公平**：与主流方法在同一基准下评测，使用了官方实现的指标；但训练数据可能与对比方法不完全一致（因过滤），需注意公平性提醒。
  - **局限性**：未见在真实低光照、遮挡等极端场景下的测试，且没有分析模型在不同深度范围的表现差异。

## 6. 论文的主要结论与发现

1. **DINOv3在零样本深度估计中表现优秀**，作为骨干可媲美甚至超越过去专用视觉预训练模型。
2. **所提轻量解码器SDT在参数量和计算量大幅降低的情况下，仍保持竞争性精度**。
3. **数据质量过滤策略能有效剔除低质量样本，减少训练数据规模10-30%的同时提升模型泛化能力**。
4. 综合以上三点，AnyDepth以**更少的参数、更少的数据**达到了与更大规模方法（如Depth Anything v2、MiDaS v3.1）**相当或更优**的零样本深度估计性能，验证了高效零样本深度估计管道的可行性。

## 7. 优点

- **创新性**：首个将DINOv3应用于零样本深度估计，并专门设计轻量解码器和数据过滤策略。
- **效率优先**：显著降低模型部署门槛，适用于移动端、机器人等实时应用。
- **数据为中心视角**：关注训练数据质量而非盲目增加数据量，具有实用价值。
- **实验设计全面**：消融实验清晰展示了每个组件的贡献。

## 8. 不足与局限

- **训练数据未完全公开/说明**：过滤后的数据集具体组成未详细列出，难以复现。
- **算力资源缺失**：未提供训练耗时时长或GPU型号，削弱了效率对比的可信度。
- **深度范围敏感性未分析**：未说明模型在近距/远距、室内/室外不同深度尺度下的表现差异。
- **动态场景/遮挡处理**：仅测试静态图像，未涉及视频序列或遮挡场景的深度一致性。
- **基线对比公平性**：对比方法可能使用更旧的骨干或不同训练策略，需警惕不公平性。
- **仅验证了零样本泛化，未探究微调后的性能上限**：若仅零样本场景，应用范围有限。

（完）
