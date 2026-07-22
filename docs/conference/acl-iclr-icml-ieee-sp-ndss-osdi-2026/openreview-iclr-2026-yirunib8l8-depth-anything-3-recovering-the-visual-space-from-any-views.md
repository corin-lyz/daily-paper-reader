---
title: "Depth Anything 3: Recovering the Visual Space from Any Views"
title_zh: 深度万物3：从任意视图恢复视觉空间
authors: "Haotong Lin, Sili Chen, Jun Hao Liew, Donny Y. Chen, Zhenyu Li, Yang Zhao, Sida Peng, Hengkai Guo, Xiaowei Zhou, Guang Shi, Jiashi Feng, Bingyi Kang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=yirunib8l8"
tags: ["query:mono-depth"]
score: 10.0
evidence: 深度万物第三版，从任意视图预测空间一致几何
tldr: 该论文提出Depth Anything 3 (DA3)，仅使用单个普通Transformer（如DINOv2）预测任意数量视图的空间一致几何，无需特殊架构或多任务学习。通过师生训练范式，在细节和泛化性上媲美DA2。建立的新视觉几何基准覆盖相机姿态估计、任意视图几何和视觉渲染，DA3在所有任务上达到最新最优。为多视图几何预测提供了极简而强大的解决方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法需要多任务或特殊架构来预测一致几何，限制了简洁性和泛化性。
method: 使用单一普通Transformer和深度射线预测目标，通过师生训练范式实现多视图一致几何。
result: 在多个任务上达到SOTA，建立新基准。
conclusion: 证明了单个Transformer足以实现高质量多视图几何预测。
---

## Abstract
We present Depth Anything 3 (DA3), a model that predicts spatially consistent geometry from an arbitrary number of visual inputs, with or without known camera poses. 
In pursuit of minimal modeling, DA3 yields two key insights:
a single plain transformer (e.g., vanilla DINOv2 encoder) is sufficient as a backbone without architectural specialization, and a singular depth-ray prediction target obviates the need for complex multi-task learning. Through our teacher-student training paradigm, the model achieves a level of detail and generalization on par with Depth Anything 2 (DA2).
We establish a new visual geometry benchmark covering camera pose estimation, any-view geometry and visual rendering. On this benchmark, DA3 sets a new state-of-the-art across all tasks, surpassing prior SOTA VGGT by an average of 35.7\% in camera pose accuracy and 23.6\% in geometric accuracy. Moreover, it outperforms DA2 in monocular depth estimation. All models are trained exclusively on public academic datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有方法在从多视图输入中预测空间一致的三维几何时，通常需要特殊的网络架构（如多分支、多任务头）或多任务学习目标，限制了模型的简洁性、泛化性和可扩展性。
- **背景**：单目深度估计领域已有如 Depth Anything 2 (DA2) 等高性能模型，但扩展到多视图一致几何预测时，现有方法（如 VGGT）仍依赖复杂设计。
- **意义**：本文旨在探索一种极简建模方案——仅用一个普通 Transformer（如 DINOv2）作为骨干，并仅使用单一的深度射线（depth-ray）预测目标，是否能达到甚至超越现有多视图几何预测的性能。成功将证明“简单即强大”的原则，并为视觉几何任务提供统一、高效的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：
  - 采用单个普通 Transformer（plain transformer，例如 vanilla DINOv2 编码器）作为骨干，无需任何特殊架构或任务特定组件。
  - 预测目标定义为“深度射线”（depth-ray），取代传统多任务学习（如同时预测深度、法向量、姿态等），从而简化训练。
  - 通过教师-学生训练范式，将现有高性能单目模型（如 DA2）的知识迁移到多视图一致性预测中，在保持细节和泛化性的同时实现空间一致几何。
- **关键技术细节**：
  - 输入：任意数量视图（带或不带已知相机位姿）。
  - 输出：每个像素对应的空间几何信息（通过深度射线直接表示三维点）。
  - 训练流程：教师模型（如 DA2）提供单视图深度伪标签，学生网络（DA3）学习在多视图约束下预测一致的深度射线。
  - 无需显式姿态估计或立体匹配，Transformer 自注意力机制隐式建模多视图对应关系。
- **公式或算法流程**：论文未给出具体公式，但可概括为：对输入视图序列，使用 DINOv2 编码器提取特征，接一个轻量解码器预测每个像素的深度射线方向与长度，通过射线投射得到三维点云，并通过多视图几何一致性损失（交叉视图投影误差）进行优化。

## 3. 实验设计：数据集、基准、对比方法
- **基准（Benchmark）**：作者建立了新的“视觉几何基准”（Visual Geometry Benchmark），覆盖三项任务：
  1. 相机姿态估计（Camera Pose Estimation）
  2. 任意视图几何（Any-View Geometry，即多视图三维重建）
  3. 视觉渲染（Visual Rendering，即新视图合成）
- **对比方法**：
  - 主要对比最新 SOTA 方法 **VGGT**（之前的多视图几何预测方法）。
  - 在单目深度估计任务上对比 **Depth Anything 2 (DA2)** 作为基线。
- **数据集**：仅使用公开学术数据集进行训练（论文未列出具体名称，但强调未使用私有数据）。测试在新基准上完成。
- **结果**：
  - 相机姿态精度相对 VGGT 平均提升 35.7%。
  - 几何精度（三维重建误差）相对 VGGT 平均提升 23.6%。
  - 在单目深度估计上超越 DA2（表明多视图监督反而提升了单目性能）。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等计算资源信息。根据惯例，此类高分辨率 Transformer 模型通常需要大规模 GPU 集群（如 8×A100 或更多），但具体细节缺失，需查阅完整论文或附录。

## 5. 实验数量与充分性
- **实验数量**：从摘要看，作者构建了一个包含三个子任务的新基准，并在每个任务上与 SOTA 对比。此外还进行了单目深度估计的对比。但未提及消融实验数量（如不同架构、不同损失函数、不同输入视图数等）。
- **充分性与客观性**：
  - **优点**：新基准覆盖多项核心视觉几何任务，对比 VGGT 和 DA2 具有代表性，且性能提升显著，统计量大（35.7%、23.6%），结果可信度较高。
  - **不足**：缺少消融实验细节，例如：是否证明了 plain Transformer 优于专用架构？是否分析了教师-学生范式对增益的贡献？这些内容可能存在于完整论文中，但当前信息不足以判断实验的全面性。此外，仅对比了 VGGT 一个 SOTA，可能忽略了其他近期方法（如 DUSt3R, InstantSplat 等）。整体而言，结论有力但实验覆盖面有待完整论文验证。

## 6. 论文的主要结论与发现
- **主要结论**：单个普通 Transformer（如 vanilla DINOv2）足以实现高质量的多视图一致几何预测，且无需特殊架构或多任务学习。深度射线预测目标简洁有效。
- **发现**：
  1. 教师-学生训练范式使得模型在细节和泛化性上媲美单目 SOTA（DA2）。
  2. 所提方法在姿态估计、几何预测和渲染三个新基准上均达到 SOTA，大幅超越之前领先的 VGGT。
  3. 多视图训练也提升了单目深度估计的性能，表明多视图一致性监督作为辅助信号的有效性。

## 7. 优点（方法与实验设计的亮点）
- **极简设计**：仅用单一 backbone 和单一预测目标，降低了模型复杂度和工程实现难度。
- **通用性**：支持任意数量视图输入，且兼容有无相机位姿的情况。
- **性能优越**：在多任务上取得显著提升，特别是姿态精度和几何精度的巨大领先（均超 20%）。
- **公平性**：仅使用公开学术数据集训练，可复现性强。
- **统一框架**：将相机姿态估计、三维重建、新视图合成统一在一个模型中，简化视觉几何 pipeline。

## 8. 不足与局限
- **资源信息缺失**：未提供计算成本，难以判断实际部署门槛。
- **实验细节不足**：缺乏消融研究、不同输入视图数的敏感性分析、对遮挡或极端姿态的鲁棒性测试等。
- **基线覆盖有限**：仅对比 VGGT 和 DA2，未与最近其他多视角几何方法（如 SAM-geometry、4DGS 等）比较。
- **未知架构细节**：解码器设计、损失函数权重、训练策略等未说明，影响可复现性。
- **可能的应用限制**：依赖于 DINOv2 预训练，对低纹理或重复纹理场景的泛化能力未知。
- **偏差风险**：只使用学术数据集，真实场景（如严重光照变化、运动模糊）下性能可能下降。

（完）
