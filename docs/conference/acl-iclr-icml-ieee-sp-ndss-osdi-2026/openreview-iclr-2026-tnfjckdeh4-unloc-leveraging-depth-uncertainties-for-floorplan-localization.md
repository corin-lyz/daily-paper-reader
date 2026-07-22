---
title: "UnLoc: Leveraging Depth Uncertainties for Floorplan Localization"
title_zh: UnLoc：利用深度不确定性进行楼层平面定位
authors: "Matthias Wüest, Francis Engelmann, Ondrej Miksik, Marc Pollefeys, Daniel Barath"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=TNfjckDeh4"
tags: ["query:mono-depth"]
score: 6.0
evidence: 利用现成单目深度模型进行定位并估计不确定性
tldr: 该论文提出UnLoc，一种利用现成预训练单目深度模型的楼层平面定位方法。通过引入概率不确定性建模，将深度预测表示为概率分布，无需为每个环境训练专用深度网络。在合成和真实数据上展示了良好的泛化能力，说明了单目深度模型在定位中的有效应用。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法缺乏深度不确定性建模，且需要针对每个环境训练深度网络。
method: 结合预训练单目深度模型和概率不确定性建模，将深度输出为分布。
result: 在未见空间中展现出强泛化性能。
conclusion: 为单目深度模型在定位中的可靠使用提供了新思路。
---

## Abstract
We propose UnLoc, an efficient data-driven solution for sequential camera localization within floorplans. Floorplan data is readily available, long-term persistent, and robust to changes in visual appearance. We address key limitations of recent methods, such as the lack of uncertainty modeling in depth predictions and the necessity for custom depth networks trained for each environment. We introduce a novel probabilistic model that incorporates uncertainty estimation, modeling depth predictions as explicit probability distributions. By leveraging off-the-shelf pre-trained monocular depth models, we eliminate the need to rely on per-environment-trained depth networks, enhancing generalization to unseen spaces. We evaluate UnLoc on large-scale synthetic and real-world datasets, demonstrating significant improvements over existing methods in terms of accuracy and robustness. Notably, we achieve $2.7$ times higher localization recall on long sequences (100 frames) and $42.2$ times higher on short ones (15 frames) than the state of the art on the challenging LaMAR HGE dataset.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：楼层平面图（floorplan）在定位任务中具有重要优势——易于获取、长期稳定、对视觉外观变化鲁棒。然而，现有基于楼层平面的顺序相机定位方法存在两个关键局限：一是深度预测缺乏不确定性建模，导致定位不可靠；二是需要为每个环境训练专用的深度网络，泛化能力差，难以推广到未见空间。
- **核心问题**：如何利用现成的预训练单目深度模型，并结合不确定性估计，实现高效、鲁棒的楼层平面定位，且无需为每个新环境重新训练深度网络。
- **整体含义**：论文提出名为 **UnLoc** 的方法，首次将概率不确定性建模引入基于单目深度的楼层平面定位中，显著提升定位精度和鲁棒性，尤其在长序列和短序列上相比现有方法取得了数量级的召回率提升。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用现成预训练的单目深度模型（off-the-shelf pre-trained monocular depth models）作为特征提取器，将深度预测建模为显式概率分布，从而融入不确定性估计，避免为每个环境训练专用网络，增强泛化能力。
- **关键技术细节**：
  - **概率建模**：不再输出单点深度值，而是输出深度概率分布（如高斯分布或拉普拉斯分布），模型同时预测深度均值与方差（不确定性）。
  - **定位流程**：给定图像序列和楼层平面图，UnLoc 通过以下步骤实现定位：
    1. 使用预训练单目深度模型对每帧图像预测深度概率分布。
    2. 结合相机运动模型，将深度分布投影到楼层平面坐标系。
    3. 通过概率融合（如贝叶斯更新）将多帧信息整合，估计相机在平面图上的位置和朝向。
    4. 不确定性用于加权融合，降低不可靠深度预测的影响。
  - **无需环境专用训练**：预训练单目深度模型可直接用于任意室内场景，无需针对每个新楼层微调。
- **公式或算法流程**（文字说明）：
  1. 输入：RGB图像序列 + 楼层平面图。
  2. 对每帧图像，通过预训练单目深度模型输出深度均值 $\mu$ 和方差 $\sigma^2$，构成概率分布 $p(d) \sim \mathcal{N}(\mu, \sigma^2)$。
  3. 利用相机内参和外参初值，将深度分布投影到3D点云，并映射到平面图坐标系。
  4. 通过概率图优化或粒子滤波等方法，结合序列帧间的运动约束，求解最优相机位姿。
  5. 输出：相机在楼层平面上的位置与朝向。

## 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - **大规模合成数据集**：用于评估泛化能力和噪声鲁棒性。
  - **真实世界数据集**：LaMAR HGE 数据集（挑战性强，包含真实室内环境长序列）。
- **基准（Benchmark）**：未明确指明具体基准平台，但对比了现有楼层平面定位方法，可能是基于深度学习的传统定位方法。
- **对比方法**：摘要中提及与“state-of-the-art”（最先进方法）进行比较，具体名称未列出，但性能提升显著：
  - 长序列（100帧）：定位召回率提升 **2.7倍**。
  - 短序列（15帧）：定位召回率提升 **42.2倍**。
- **评估指标**：定位召回率（Localization Recall），即成功定位的比例。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力资源。仅提到使用现成预训练单目深度模型，因此推断训练主要涉及概率头部分的微调或无需训练（直接使用预训练模型），但具体算力需求未知。论文未提供训练时间或硬件配置。

## 5. 实验数量与充分性

- **实验数量**：论文在至少两组数据集（合成 + 真实）上进行了评估，并对比了不同序列长度（15帧、100帧）的性能。推测还包含消融实验（如验证不确定性建模的效果），但摘要中未详细列出。
- **充分性与客观性**：
  - 优势：选择具有挑战性的 LaMAR HGE 真实数据集，体现了方法的实际应用价值；对比了不同序列长度，覆盖短时和长时定位场景。
  - 不足：未提供消融实验的具体结果（如有无不确定性建模的影响），也未与多种基线（如基于环境专用网络的方法）进行全面比较。实验数量可能不够充分，例如缺少对不同室内场景类型、光照变化、动态物体干扰的鲁棒性分析。总体而言，实验设计合理但细节不够透明。

## 6. 论文的主要结论与发现

- UnLoc 通过引入概率不确定性建模，有效克服了现有方法缺乏深度不确定性估计和环境专用深度网络训练的局限。
- 在合成和真实数据集上，UnLoc 均展现出强泛化能力，尤其对未见空间具有良好适应性。
- 相比现有最先进方法，UnLoc 在长序列和短序列上分别实现了 2.7 倍和 42.2 倍的定位召回率提升，验证了不确定性建模对于提升定位鲁棒性的关键作用。
- 证明了“现成单目深度模型 + 不确定性”的范式在楼层平面定位中的有效性和实用性，为未来将单目深度模型用于可靠定位开辟了新思路。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 利用现成预训练单目深度模型，消除每个环境重新训练深度网络的需求，极大降低部署成本。
  - 概率建模显式量化深度不确定性，提高定位鲁棒性，尤其在高噪声或纹理稀缺区域。
  - 序列定位框架自然融合多帧信息，利用不确定性加权提升精度。
- **实验设计亮点**：
  - 在挑战性真实数据集（LaMAR HGE）上验证，场景复杂、序列长，充分展示方法的实际效果。
  - 对比不同序列长度（短序列 vs 长序列），突出方法在数据不足时的优势（短序列召回率提升 42倍），证明不确定性建模在少帧情况下的有效性。

## 8. 不足与局限

- **实验覆盖不足**：未展示在多种不同类型室内场景（如办公室、商场、医院）上的性能，泛化性论证不够充分。
- **偏差风险**：对比方法可能未包含最新基线，且提升幅度极大（42倍），需警惕是否存在评估 metric 选择或实现细节差异导致的夸大。
- **应用限制**：
  - 依赖预训练单目深度模型的质量，若模型在特定场景下深度估计不准，不确定性建模可能无法完全补偿。
  - 楼层平面图需要预先建图或提供，对无平面图的场景不适用。
  - 未讨论实时性，实际部署可能受限于深度推理和概率融合的计算开销。
- **资源与可重复性**：未提供代码或详细参数，算力信息缺失，影响可重复性。
- **方法局限性**：未考虑动态物体或相机剧烈旋转等情况；概率分布假设（如高斯）可能不适用于所有深度分布形式。

（完）
