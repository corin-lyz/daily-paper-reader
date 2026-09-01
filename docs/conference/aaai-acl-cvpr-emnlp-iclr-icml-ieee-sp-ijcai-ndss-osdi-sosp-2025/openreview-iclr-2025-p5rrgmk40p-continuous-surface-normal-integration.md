---
title: Continuous Surface Normal Integration
title_zh: 连续表面法线积分
authors: "Lixiong Chen, Bohan Yu, Victor Adrian Prisacariu, Imari Sato"
date: 2024-09-22
pdf: "https://openreview.net/pdf?id=P5rRGMk40p"
tags: ["query:mono-depth"]
score: 7.0
evidence: 从单目法线场进行连续表面深度估计
tldr: 针对传统表面法线积分局限于规则网格的问题，提出连续表面法线积分方法，将表面视为底层连续梯度场的采样，用多项式场建模拓扑并训练网络预测任意查询点的法线与深度。该方法直接将单目法线测量转化为连续表面深度估计，扩展了单目显式表面重建的能力，实验表明其在连续深度预测任务上具有良好精度与连续性，为几何重建提供了新工具。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 传统法线积分限于规则网格，无法直接估计连续表面深度。
method: 用多项式场参数化梯度场，网络预测任意点的法线和深度。
result: 实现了单目连续表面的深度和法线联合估计。
conclusion: 为单目显式表面重建提供了连续深度估计的新方法。
---

## Abstract
We address a novel task for monocular explicit surface reconstruction that extends traditional surface normal integration over measurements on a regular grid to direct continuous surface depth estimation. Our solution accepts coordinates as queries and predicts both the normal and depth of an arbitrary query point by its relative locations and orientations to the points distributed in its vicinity. In general, all points are regarded by our model as random samples drawn from an underlying continuous gradient field of a surface which we parameterize using a field of polynomials to establish its topology. We establish a mapping from coordinates to a sequence of learnable polynomial coefficients to model a continuous surface and train a neural network to approximate it. We  decompose a continuous surface representation into two components: (1) a set of grid points of unknown orientations whose locations are picked by a quadtree and (2) a set of sample points whose orientations are directly observable. Our training workflow estimates the normal of grid points and the locations of depth discontinuities iteratively. During each iteration, we generate a normal map of grid points for it to be processed by a standard bilateral normal integrator to identify the locations of depth discontinuities, which we use to refine the estimation for grid-based normal map in the subsequent iteration. As a result, the learned model generates both normal and depth for arbitrary coordinates accurately in a continuous field. We provide both theoretical formulation for our design and extensive empirical evidence to demonstrate that our proposed method not only delivers a performance as effective as its grid-based counterpart approaches but also flexibly and accurately addresses the continuous cases that existing methods are unable to handle.

---

## 论文详细总结（自动生成）

# 论文总结：Continuous Surface Normal Integration（连续表面法线积分）

## 1. 核心问题与整体含义
- **研究背景**：传统的表面法线积分方法通常只能在规则网格上处理法线测量值，并据此重建离散的深度图。这类方法受限于网格结构，难以直接对连续表面进行深度估计。
- **核心问题**：本文提出一项新任务——**单目显式表面重建中的连续表面深度估计**，即从单目法线场直接恢复连续表面上任意查询点的深度，而不局限于规则网格。
- **整体含义**：将表面法线积分从“网格上有序测量”推广到“任意连续坐标上的深度/法线查询”，为单目三维重建提供更灵活、更连续的显式表面表示。

## 2. 方法论
- **核心思想**：
  - 将表面上的所有点视为从一个底层连续梯度场中随机采样的样本。
  - 使用**多项式场**参数化该连续梯度场，从而建立表面拓扑。
  - 建立从坐标到可学习多项式系数序列的映射，并训练神经网络来近似该连续表面。
- **表示分解**：
  1. **网格点**：位置由四叉树（quadtree）选取，方向未知；
  2. **样本点**：方向（法线）可直接观测。
- **训练流程**：
  - 迭代估计网格点的法线和深度不连续位置。
  - 每次迭代生成网格法线图，用**标准双边法线积分器**处理，识别深度不连续位置。
  - 利用识别结果在下一轮迭代中细化网格法线图的估计。
- **输出**：训练后的模型可对任意坐标查询，同时输出该点的法线和深度，实现连续场的联合估计。
- **理论贡献**：提供了连续表面法线积分的理论公式化描述。

## 3. 实验设计
- 根据摘要，论文声称进行了“广泛的实证验证”（extensive empirical evidence）。
- 对比对象：与基于网格的传统方法（grid-based counterparts）进行比较。
- 特殊场景：重点验证了现有方法无法处理的连续情况。
- **注意**：在提供的摘要和元数据中，**未明确列出**所使用的具体数据集、评测基准（benchmark）以及完整的基线方法列表。因此无法从现有信息中确认实验场景的具体构成。

## 4. 资源与算力
- 摘要和元数据中**未提及**任何关于 GPU 型号、数量、训练时长、显存占用或计算成本的信息。
- 因此无法评估该方法的资源需求和训练开销。

## 5. 实验数量与充分性
- 从摘要看，实验覆盖了：
  - 与网格基线方法的性能对比；
  - 连续场景下的定性/定量验证；
  - 可能包含迭代细化过程的消融或分析（但摘要未明确说明）。
- **充分性判断**：由于本文仅提供摘要级别的信息，实验数量、数据集多样性、消融覆盖程度均无法具体核实。摘要声称“广泛”，但客观证据不足，需阅读全文才能判断其公平性与完备性。

## 6. 主要结论与发现
- 提出的连续法线积分方法在网格化对比任务中能达到与基于网格的方法相当的效果。
- 在连续表面上能灵活、准确地预测任意坐标的深度和法线，而传统方法无法处理此类连续情况。
- 该方法将单目法线测量直接转化为连续表面深度估计，扩展了单目显式重建的能力边界。

## 7. 优点
- **任务新颖**：首次将法线积分推广到任意查询点的连续深度估计，突破规则网格限制。
- **表示设计巧妙**：用多项式场参数化梯度场，结合四叉树选取未知方向的网格点，既保留结构又支持连续采样。
- **迭代优化机制**：通过“法线图估计→双边法线积分→深度不连续识别→细化法线估计”的闭环迭代，提升不连续区域的精度。
- **输入输出灵活**：以坐标作为查询，输出法线和深度，适合任意分辨率或局部区域查询。
- **理论与实践结合**：既给出理论公式化，又有实证支持。

## 8. 不足与局限
- **实验透明度不足**：从摘要和元数据中看不到具体数据集、评测指标、基线明细，无法独立判断方法的泛化能力和相对优势。
- **资源开销未知**：未说明训练/推理的计算成本，难以评估实际部署可行性。
- **依赖初始网格结构**：虽然目标是连续估计，但仍依赖四叉树选定的网格点，网格生成策略可能影响结果。
- **应用限制**：论文聚焦单目法线场输入，实际单目法线估计本身的误差可能影响后续重建质量；文中未讨论噪声鲁棒性。
- **缺乏与其他连续表示方法（如隐式神经场、神经辐射场相关方法）的对比**：摘要中仅提及与网格基方法对比，未说明与现有连续深度回归或隐式重建方法的比较。

（完）
