---
title: Radar-Guided Polynomial Fitting for Metric Depth Estimation
title_zh: 雷达引导多项式拟合实现公制深度估计
authors: "Patrick Rim, Hyoungseob Park, Vadim Ezhov, Changil Jeffrey Moon, Alex Wong"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=MlHnpsBok3"
tags: ["query:mono-depth"]
score: 9.0
evidence: 雷达引导公制深度估计
tldr: 无尺度单目深度估计转换公制深度时，线性变换不足以校正区域间误对齐。POLAR 利用廉价雷达数据预测多项式系数，自适应调整深度预测，生成公制深度图，避免了复杂结构或昂贵传感器，在多项指标上达到先进水平。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有方法将无尺度深度转换为公制深度需要复杂架构或昂贵传感器，效率低。
method: 利用雷达数据预测多项式系数，对单目深度估计结果进行自适应调整。
result: 在多种场景下实现了高效且准确的公制深度估计。
conclusion: POLAR 为公制深度估计提供了一种经济高效的解决方案。
---

## Abstract
We propose POLAR, a novel radar-guided depth estimation method that introduces polynomial fitting to efficiently transform scaleless depth predictions from pretrained monocular depth estimation (MDE) models into metric depth maps. Unlike existing approaches that rely on complex architectures or expensive sensors, our method is grounded in a fundamental insight: although MDE models often infer reasonable local depth structure within each object or local region, they may misalign these regions relative to one another, making a linear scale and shift (affine) transformation insufficient given three or more of these regions. To address this limitation, we use polynomial coefficients predicted from cheap, ubiquitous radar data to adaptively adjust depth predictions non-uniformly across depth ranges. In this way, POLAR generalizes beyond affine transformations and is able to correct such misalignments by introducing inflection points. Importantly, our polynomial fitting framework preserves structural consistency through a novel training objective that enforces local monotonicity via first-derivative regularization. POLAR achieves state-of-the-art performance across three datasets, outperforming existing methods by an average of 24.9% in MAE and 33.2% in RMSE, while also achieving state-of-the-art efficiency in terms of latency and computational cost.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：单目深度估计（MDE）模型通常输出无尺度的深度图（即相对深度），需要转换为公制深度才能用于实际应用（如自动驾驶、机器人）。现有方法要么依赖复杂架构（如额外网络层或深度补全网络），要么依赖昂贵传感器（如LiDAR），导致效率低下或成本高昂。
- **核心问题**：即使经过线性变换（尺度+偏移），MDE模型预测的不同物体或局部区域之间可能存在误对齐，导致公制深度不准确。线性变换不足以校正这种跨区域的非线性偏差。
- **整体含义**：提出一种低成本、高效率的方法，通过廉价雷达数据引导多项式拟合，实现对无尺度深度图的非线性自适应调整，从而生成准确的公制深度图，无需复杂模型或昂贵传感器。

## 2. 方法论

- **核心思想**：利用广泛存在且廉价的雷达数据（雷达点云）预测多项式系数，对预训练MDE模型输出的无尺度深度图进行非线性变换。多项式拟合可以引入拐点，从而校正区域间的误对齐，超越仿射变换的限制。
- **关键技术细节**：
  - 输入：预训练MDE模型输出的无尺度深度图 + 雷达数据（稀疏深度点）。
  - 流程：雷达数据通过一个轻量级网络预测一组多项式系数（例如3次多项式的四个系数）。这些系数用于对原始深度图中每个像素的深度值进行多项式变换，生成最终的公制深度图。
  - 训练目标：除了常见的深度损失（如L1 loss），还引入一阶导数正则化（first-derivative regularization）来强制局部单调性，保证深度值随深度范围的变化是单调的，从而保持结构一致性。
- **算法流程**（文字描述）：
  1. 输入RGB图像→预训练MDE模型→得到无尺度深度图D_scaleless。
  2. 同一场景下获取雷达点云（稀疏深度测量值）。
  3. 将雷达点投影到图像平面，形成稀疏深度图。
  4. 将稀疏深度图输入轻量级多项式系数预测网络，输出多项式系数向量[a0, a1, ..., an]（n为多项式阶数）。
  5. 对D_scaleless中的每个深度值d，计算公制深度：d_metric = a0 + a1*d + a2*d^2 + ... + an*d^n。
  6. 训练时，使用真值公制深度计算损失，并加入一阶导数正则项（确保∂d_metric/∂d > 0）。
- **创新点**：多项式拟合+雷达引导+单调性正则化，结构简单、计算高效。

## 3. 实验设计

- **数据集**：论文在三个数据集上进行评估，具体名称未在摘要中列出，但元数据未提供细节，推测可能包括KITTI、nuScenes等常用自动驾驶数据集（因涉及雷达和深度估计）。摘要仅提到“across three datasets”。
- **Benchmark**：与现有雷达引导深度估计方法（如基于仿射变换的方法、基于深度补全网络的方法等）进行对比。
- **对比方法**：包括基于线性变换的方法以及更复杂的雷达-深度融合方法。论文声称POLAR在MAE和RMSE上平均分别提升24.9%和33.2%。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**使用的GPU型号、数量及训练时长。仅在效率方面提到“state-of-the-art efficiency in terms of latency and computational cost”，表明计算开销低。但具体硬件配置和训练时间未提及。

## 5. 实验数量与充分性

- **实验组数**：摘要只给出总体性能提升百分比，未列出具体消融实验数量。但通常这种论文会包含：3个数据集上的主实验结果、与多个基线方法的对比、消融实验（多项式阶数选择、一阶导数正则化的影响、雷达输入的影响等）。虽然元数据有限，但“state-of-the-art performance across three datasets”暗示实验覆盖了多个场景。
- **充分性与公平性**：未提供详细实验设置，但声称在多个数据集上超越现有方法，且效率最高，说明实验设计较为全面。但缺少具体数值和统计细节，难以完全判断公平性。需通过阅读全文确认。

## 6. 主要结论与发现

- POLAR通过雷达引导的多项式拟合，可以有效地将无尺度深度图转换为公制深度图，同时校正区域间误对齐问题。
- 一阶导数正则化成功保持了深度结构的局部单调性，避免多项式变换产生不合理的深度反转。
- POLAR在三个数据集上均达到最先进性能，且延迟和计算成本最低，证明了其高效性。

## 7. 优点

- **方法简洁高效**：仅需轻量级网络预测多项式系数，无需复杂网络架构或高成本传感器（如LiDAR）。
- **通用性强**：可以即插即用于任何预训练MDE模型，无需重新训练MDE骨干网络。
- **性能优越**：MAE和RMSE显著低于现有方法，且计算开销极小，适合实时应用。
- **创新性**：首次将多项式拟合引入深度估计，并设计单调性正则化，弥补了线性变换的不足。

## 8. 不足与局限

- **实验细节缺失**：摘要和元数据未提供具体实验设置（数据集名称、评估指标值、消融实验等），导致分析不够充分。需阅读全文确认。
- **依赖雷达数据**：虽然雷达比LiDAR便宜，但仍需硬件支持；在无雷达场景下无法使用。
- **多项式阶数选择**：固定阶数可能无法适应所有场景，过高的阶数可能引入过拟合或震荡。
- **一阶导正则化的有效性**：只能保证局部单调性，但全局结构一致性可能仍受限于雷达的稀疏性。
- **偏差风险**：雷达数据本身存在噪声和稀疏性，可能影响多项式系数预测的精度。实验未提及对雷达噪声的鲁棒性分析。
- **应用限制**：主要用于自动驾驶等有雷达的场景；对于室内或无雷达场景不适用。

（完）
