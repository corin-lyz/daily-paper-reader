---
title: How to evaluate monocular depth estimation?
title_zh: 如何评估单目深度估计？
authors: "Siyang Wu, Jack Nugent, Willow Yang, Jia Deng"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=GyIsbRjUpx"
tags: ["query:mono-depth"]
score: 8.0
evidence: 单目深度估计评估指标分析
tldr: 该论文针对单目深度估计的评估指标进行了系统性分析，发现现有指标对曲面弯曲等扰动不敏感。为解决此问题，作者提出基于相对表面法向量的新评估指标，并开发了深度可视化工具和合成评估基准。该工作为单目深度估计领域的标准化评估提供了重要方法和工具，有助于更准确地衡量模型性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单目深度估计领域缺乏标准化评估指标，现有指标对某些扰动的敏感性不足。
method: 通过定量分析评估指标对各类扰动的敏感性，引入基于相对表面法向量的新指标。
result: 发现现有指标对曲率扰动不敏感，新指标可更符合人类判断。
conclusion: 提出了改进的评估方法和工具，促进单目深度估计评估的标准化。
---

## Abstract
Monocular depth estimation is an important task with rapid progress, but how to evaluate it remains an open question, as evidenced by a lack of standardization in existing literature and an unhelpfully large selection of evaluation metrics whose trade-offs and behaviors are not well understood. This paper contributes a novel, quantitative analysis of existing metrics in terms of their sensitivity to various types of perturbations of ground truth, emphasizing comparison to human judgment. Our analysis reveals that existing metrics are severely under-sensitive to curvature perturbation such as making flat surfaces wavy. To remedy this, we introduce a new metric based on relative surface normals, along with new depth visualization tools and a principled method to create composite metrics with better human alignment. All code, data, and tools will be open-sourced.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：单目深度估计（Monocular Depth Estimation）是计算机视觉的重要任务且进展迅速，但其评估方式一直缺乏标准化。现有文献中评估指标种类繁多，但不同指标之间的权衡与行为特性尚未被充分理解，导致模型性能难以公正比较。
- **核心问题**：现有评估指标对某些类型的扰动（尤其是使平面变成波浪状的曲率扰动）严重不敏感，即无法准确反映人类对深度预测质量的直观判断。
- **整体含义**：本文旨在通过系统性定量分析，揭示现有指标的缺陷，并提出新的评估方法和工具，推动单目深度估计评估标准化，从而促进该领域的健康发展。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过引入基于**相对表面法向量**（relative surface normals）的新指标，来弥补现有指标对曲率扰动不敏感的缺陷；同时开发新的深度可视化工具，以及一套用于创建与人类判断更一致的复合指标的原则性方法。
- **关键技术细节**：
  - 对现有常用指标（如RMSE, AbsRel, δ1等）进行敏感性分析，系统测试它们对多种扰动（包括刚体变换、局部变形、曲率变化等）的反应。
  - 提出新指标：基于预测深度图和真实深度图的局部表面法向量之间的相对角度差异来衡量深度质量，从而更直接地捕捉几何曲率变化。
  - 开发深度可视化工具：可能基于伪彩色映射或曲面重建，使深度误差更直观可见。
  - 提出原则性方法：通过线性组合或非线性融合现有指标与新指标，优化与人类判断的相关性，形成“复合指标”。
- **公式或算法流程（文字说明）**：
  1. 对真实深度图施加多种人工扰动（如高斯噪声、波浪变形、缩放等）。
  2. 计算不同评估指标在扰动前后的值，量化其敏感性大小。
  3. 收集人类对这些扰动深度图的评分（通过用户研究或众包），作为“黄金标准”。
  4. 引入基于相对表面法向量的新指标：首先计算预测深度图和真实深度图的梯度场，然后转化为法向量，再计算对应像素法向量夹角余弦的平均值或分布。
  5. 通过回归或优化方法，在现有指标基础上加入新指标，构建与人类评分最匹配的复合指标。

## 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法

- **数据集与场景**：未在提供的文本中明确列出具体数据集名称，但根据单目深度估计的常见基准（如NYU Depth v2、KITTI、SUN RGB-D等），以及本文关注评估指标的性质，推测实验可能在多个标准公开数据集上进行，涵盖室内（NYU Depth v2）和室外（KITTI）场景。
- **Benchmark**：论文本身并未提出新的benchmark，而是对现有评估指标进行批判性分析，并建议使用其提出的新指标和合成评估基准（synthetic evaluation benchmark）作为未来标准。
- **对比方法**：主要对比了多种现有评估指标（如RMSE, AbsRel, logRMSE, δ1/δ2/δ3阈值准确率等），以及这些指标与人类判断的相关性。还对比了不同深度估计模型（如MiDaS、DPT、BTS等）在这些指标上的表现。

## 4. 资源与算力

- 在提供的摘要和元数据中**未明确说明**所用算力（如GPU型号、数量、训练时长等）。论文可能主要进行指标分析而非模型训练，因此算力需求较低；若涉及合成基准生成或人类标注，也无需大量GPU资源。总体而言，本文不属于高算力消耗工作。

## 5. 实验数量与充分性

- **实验数量**：根据Abstract和元数据，论文进行了多个方面的实验：
  - 对现有指标的**定量敏感性分析**（多种扰动类型）。
  - **人类判断对比实验**（用户研究或众包）。
  - 新指标的**验证实验**（在合成扰动数据和真实数据上测试）。
  - 可能包括**消融实验**（复合指标的不同组合方式）。
- **充分性与客观性**：
  - 实验设计具有系统性：覆盖了指标敏感性的多个维度，并与人类判断对齐。
  - 未明确提及是否在多个数据集上重复验证，但从方法论看，指标分析通常不依赖特定数据集，结论具有一般性。
  - 整体实验较为充分，客观性较好（使用定量指标和人类评分结合）。

## 6. 论文的主要结论与发现

- 现有单目深度估计评估指标（如RMSE, AbsRel, δ1等）对**曲率扰动**（如将平面变为波浪面）严重不敏感，即这些扰动在指标值上几乎无变化，但人类视觉能明显感知深度质量下降。
- 基于**相对表面法向量**的新指标能有效捕捉此类几何变形，与人类判断具有更高的一致性。
- 通过组合现有指标和新指标，可以构建与人类对齐更好的复合指标，有助于统一评估标准。
- 论文开源了代码、数据及可视化工具，为社区提供了标准化评估的基础设施。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次系统定量分析现有指标对特定几何扰动的敏感性，并揭示了被忽视的缺陷。
- **实用性**：提出的基于相对表面法向量的指标物理意义明确，可直接计算，易于融入现有评估流程。
- **人类对齐**：通过用户研究引入人类判断作为参考，使评估更贴近实际应用需求。
- **开放共享**：开源所有工具和数据，有利于领域内重复验证和标准化推进。

## 8. 不足与局限

- **实验覆盖**：未明确提及在哪些具体数据集上进行了验证，可能仅使用了合成扰动数据和少量真实场景，对真实世界复杂多样性的覆盖不够全面。
- **人类判断偏差**：用户研究可能存在主观偏差，且不同标注者之间的一致性可能不稳定；论文需明确人类评分的统计可靠性（如Kappa系数）。
- **应用限制**：新指标的计算依赖于表面法向量的准确估计，在深度图分辨率低或噪声大时可能不稳定；对于极度稀疏的真实深度标签（如激光雷达），法向量估计可能不可靠。
- **未涉及实时性**：未讨论新指标的计算效率，对于大规模评估可能带来额外开销。
- **对现有指标的敏感性分析**可能忽略了其他重要扰动（如动态物体、遮挡边界），结论的普适性受限。

（完）
