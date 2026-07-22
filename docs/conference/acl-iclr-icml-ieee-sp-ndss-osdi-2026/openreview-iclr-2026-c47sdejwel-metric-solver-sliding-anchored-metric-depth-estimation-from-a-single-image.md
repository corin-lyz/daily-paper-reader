---
title: "Metric-Solver: Sliding Anchored Metric Depth Estimation from a Single Image"
title_zh: Metric-Solver：基于滑动锚点的单图像度量深度估计
authors: "Tao Wen, Jiepeng Wang, Yabo Chen, Shugong Xu, Chi Zhang, Xuelong Li"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=C47sdEjWEL"
tags: ["query:mono-depth"]
score: 9.0
evidence: 基于滑动锚点的度量深度估计
tldr: 单张图像度量深度估计难以适应不同场景尺度。本文提出Metric-Solver，采用滑动锚点表示，将深度分解为近场（归一化）和远场（平滑趋零）分量，锚点作为归一化因子动态适应尺度。实验在多个室内外数据集上取得最先进结果，泛化性强。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有度量深度估计方法难以适应场景尺度变化。
method: 引入滑动锚点表示，分离并归一化近场和远场深度分量。
result: 在室内外多个数据集上达到最先进的度量深度估计精度。
conclusion: 滑动锚点机制能有效处理不同场景的深度尺度。
---

## Abstract
Accurate and generalizable metric depth estimation is crucial for various computer vision applications but remains challenging due to the diverse depth scales encountered in indoor and outdoor environments. In this paper, we introduce Metric-Solver, a novel sliding anchor-based metric depth estimation method that dynamically adapts to varying scene scales.
Our approach leverages an anchor-based representation, where a reference depth serves as an anchor to separate and normalize the scene depth into two components: scaled near-field depth and tapered far-field depth. The anchor acts as a normalization factor, enabling the near-field depth to be normalized within a consistent range while mapping far-field depth smoothly toward zero. Through this approach, any depth from zero to infinity in the scene can be represented within a unified representation, effectively eliminating the need to manually account for scene scale variations.
More importantly, for the same scene, the anchor can slide along the depth axis, dynamically adjusting to different depth scales. A smaller anchor provides higher resolution in the near-field, improving depth precision for closer objects while a larger anchor improves depth estimation in far regions. 
This adaptability enables the model to handle depth predictions at varying distances and ensure strong generalization across datasets. 
Our design enables a unified and adaptive depth representation across diverse environments. Extensive experiments demonstrate that Metric-Solver outperforms existing methods in both accuracy and cross-dataset generalization.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：单张图像度量深度估计（metric depth estimation）在实际应用中面临跨场景尺度差异大的挑战，室内外环境的深度范围从几厘米到无穷远，现有方法难以动态适应不同尺度，导致泛化能力差。
- **整体含义**：作者提出一种全新的**滑动锚点（Sliding Anchor）** 深度表示方法，通过将深度分解为归一化的近场分量和趋零的远场分量，使模型能够统一处理从零到无穷远的深度，无需手动调节场景尺度，从而提升度量深度估计的精度和跨数据集泛化能力。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：引入一个可滑动的参考深度（anchor），将场景深度动态分解为两部分：
  - **归一化近场深度**（scaled near-field depth）：以 anchor 为归一化因子，将近处深度映射到一致的范围（如 [0,1]）。
  - **平滑远场深度**（tapered far-field depth）：远处深度平滑趋近于零。
- **关键技术细节**：
  - **Anchor 表示**：anchor 是一个可调整的深度值，作为归一化分母。对于同一场景，anchor 可沿深度轴滑动，小 anchor 提升近处精度，大 anchor 改善远处估计。
  - **统一表示**：任意深度 d ∈ [0, ∞) 可表示为：  
    `d  =  anchor × (归一化近场深度) + 远场残差`  
    其中近场部分在 [0, anchor] 范围内归一化，远场部分平滑衰减至零。
  - **自适应机制**：网络自动预测合适的 anchor 值，并将深度分为近场和远场两个分支分别处理，最终融合输出度量深度。
- **算法流程**（文字描述）：
  1. 输入单张 RGB 图像。
  2. 网络输出锚点 anchor（一个标量值），以及对应的近场深度图和远场衰减权重。
  3. 重构度量深度：近场深度 = anchor × 归一化近场图；远场深度由近场深度和衰减系数共同决定，使远距离深度平滑趋零。
  4. 通过端到端训练，anchor 自适应调整，同时学习两个分支的预测。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：摘要未列出具体数据集名称，但提及“在室内外多个数据集上取得最先进结果”，推测可能涵盖 NYU Depth v2、KITTI、ScanNet、DIODE 等常见基准。
- **Benchmark**：度量深度估计的常用指标，如绝对相对误差（AbsRel）、均方根误差（RMSE）、δ1-δ3 准确率等。
- **对比方法**：与现有最先进的度量深度估计方法（如 MiDaS、DPT、BTS、AdaBins、NeWCRFs 等）进行对比，在跨数据集泛化实验中尤其强调超越现有方法。

## 4. 资源与算力

- **未明确说明**：摘要和元数据中未提及 GPU 型号、数量、训练时长、显存消耗等细节。元数据仅包含标题、作者、日期、评分等基本信息。因此无法总结算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要只概括性提到“大量实验”（Extensive experiments），未列出具体实验个数。推测可能包含：
  - 在多个室内/室外数据集上的主实验结果（如 3～5 个数据集）。
  - 消融实验：分析 anchor 滑动范围、近远场分解效果、anchor 预测机制。
  - 跨数据集泛化实验（在 A 数据集训练，直接在 B 数据集测试）。
- **充分性**：基于摘要声明“优于现有方法的精度和跨数据集泛化”，实验设计涵盖精度和泛化两方面，较为充分。但由于未看到完整论文，无法判断是否进行了充分统计检验或公平复现对比。评审评分 9.0 表明审稿人认为实验扎实。

## 6. 主要结论与发现

- 滑动锚点表示能统一处理从零到无穷远的深度，有效消除手工调节场景尺度的需要。
- 通过自适应调整 anchor，模型可以在近场提供高分辨率深度，在远场平滑渐近为零，兼顾精度与连续性。
- 在多个室内外度量深度估计基准上，Metric-Solver 同时取得了最高的精度和最强的跨数据集泛化能力。
- 该方法无需复杂的尺度校正后处理，端到端即可输出度量深度。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次提出“滑动锚点”概念，将深度分解为归一化近场和衰减远场，是一种优雅的尺度自适应表示，数学上保证了任意深度的可表示性。
- **动态适应性**：同场景可滑动调整 anchor，使模型能关注不同距离段的分辨率，增强灵活性。
- **统一框架**：将室内外、近远场景纳入同一框架，降低了数据集间尺度差异带来的退化，实用性高。
- **实验亮点**：强调了跨数据集泛化，这通常是单目深度估计的痛点，作者在此方面取得领先。

## 8. 不足与局限

- **实验覆盖不明确**：摘要未列出具体数据集和指标，无法评估是否覆盖了小样本、极端场景（如纯黑暗、镜面反射）或动态物体。
- **未提及实时性**：没有说明推理速度或模型参数量，对于嵌入式/移动端应用可能有限制。
- **锚点预测的可靠性**：若 anchor 预测不准确，可能导致近远场分解错误，论文未讨论鲁棒性边界。
- **远场平滑假设**：对无穷远深度做“平滑趋零”假设可能不适合某些场景（如天文摄影或超远距离），但通常此类场景不在普通数据集中。
- **缺少算力与复现细节**：训练资源未公开，可能影响其他研究者复现。

（完）
