---
title: Self-supervised Monocular Depth Estimation Robust to Reflective Surface Leveraged by Triplet Mining
title_zh: 利用三元组挖掘对反射表面鲁棒的自监督单目深度估计
authors: "Wonhyeok Choi, Kyumin Hwang, Wei Peng, Minwoo Choi, Sunghoon Im"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=XdRIno98gG"
tags: ["query:mono-depth"]
score: 9.0
evidence: 自监督单目深度估计，提出反射感知三元组挖掘策略
tldr: 自监督单目深度估计在反射表面常因违反朗伯假设而失效。本文提出基于相机几何的逐像素反射区域三元组挖掘策略，并设计反射感知三元组损失，有针对性地惩罚反射区域的错误深度预测。实验表明该方法在反射表面深度精度上大幅提升，且不影响整体性能，推动了自监督深度估计在真实场景中的鲁棒应用。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 反射表面违反朗伯假设，导致自监督单目深度估计在该类区域误差大。
method: 利用相机几何定位反射区域，设计反射感知三元组挖掘损失进行训练。
result: 在反射表面深度精度上显著提升，整体性能保持领先。
conclusion: 为自监督单目深度估计处理高光反射场景提供了有效解决方案。
---

## Abstract
Self-supervised monocular depth estimation (SSMDE) aims to predict the dense depth map of a monocular image, by learning depth from RGB image sequences, eliminating the need for ground-truth depth labels.
Although this approach simplifies data acquisition compared to supervised methods, it struggles with reflective surfaces, as they violate the assumptions of Lambertian reflectance, leading to inaccurate training on such surfaces.
To tackle this problem, we propose a novel training strategy for an SSMDE by leveraging triplet mining to pinpoint reflective regions at the pixel level, guided by the camera geometry between different viewpoints.
The proposed reflection-aware triplet mining loss specifically penalizes the inappropriate photometric error minimization on the localized reflective regions while preserving depth accuracy on non-reflective areas.
We also incorporate a reflection-aware knowledge distillation method that enables a student model to selectively learn the pixel-level knowledge from reflective and non-reflective regions. This results in robust depth estimation across areas.
Evaluation results on multiple datasets demonstrate that our method effectively enhances depth quality on reflective surfaces and outperforms state-of-the-art SSMDE baselines.

---

## 论文详细总结（自动生成）

# 论文总结：利用三元组挖掘对反射表面鲁棒的自监督单目深度估计

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：自监督单目深度估计（SSMDE）通过从 RGB 图像序列中学习深度，无需真实深度标注，降低了数据获取成本。
- **核心问题**：反射表面（如玻璃、镜面、光滑地面等）违反朗伯反射假设，导致自监督训练中的光度一致性约束在这些区域失效，深度估计误差显著增大。
- **整体含义**：本文旨在解决 SSMDE 在反射表面上的鲁棒性问题，使模型在真实场景（含大量高光反射区域）中也能准确估计深度，推动自监督深度估计的实用化。

## 2. 方法论（核心思想、关键技术细节）

- **核心思想**：利用不同视角之间的相机几何关系，逐像素定位反射区域，并设计专门的训练策略，阻止模型在反射区域上“错误地最小化光度误差”，同时保持非反射区域的深度精度。
- **关键技术细节**：
  - **反射感知三元组挖掘（Reflection-aware Triplet Mining）**：通过相机几何从多个视图中挖掘反射区域对应的像素级三元组关系，从而精确定位反射表面。
  - **反射感知三元组损失（Reflection-aware Triplet Mining Loss）**：有针对性地惩罚反射区域中不恰当的光度误差最小化，抑制模型被反射纹理误导，同时不影响非反射区域的正常学习。
  - **反射感知知识蒸馏（Reflection-aware Knowledge Distillation）**：让学生模型分别从反射区域和非反射区域选择性学习像素级知识，从而在不同类型的区域上都能获得稳健的深度估计。
- **说明**：摘要中未给出具体公式和损失函数细节，但整体流程可概括为“反射区域定位 → 反射加权损失 → 知识蒸馏”三阶段。

## 3. 实验设计

- **数据集**：摘要仅表明在“多个数据集（multiple datasets）”上进行了评估，但未列出具体数据集名称（如 KITTI、Cityscapes 等常见基准也未提及）。
- **Benchmark**：与当前最先进的自监督单目深度估计基线方法（state-of-the-art SSMDE baselines）进行了对比。
- **评估指标**：未明确说明具体指标，但关注点在“反射表面的深度质量”以及“整体深度精度”。

## 4. 资源与算力

- 论文摘要及提供的元数据中**未提及**任何算力信息，如 GPU 型号、数量、训练时长、参数量等。
- 因此，无法从现有内容中总结训练成本或资源需求。

## 5. 实验数量与充分性

- 摘要仅概括性地提到“在多个数据集上验证”，未给出具体的实验组数、消融实验设置、可视化结果或数值表格。
- 因此，从现有信息来看：
  - **实验数量**：不明确，无法判断是否包含充足的消融实验（如反射区域定位有效性、损失项贡献、蒸馏效果等）。
  - **充分性**：不足以进行客观评估——没有具体数值、统计显著性检验、与基线的详细对比结果，也无法判断实验条件是否公平（如是否采用相同骨干网络、输入分辨率等）。

## 6. 主要结论与发现

- 所提出的反射感知三元组挖掘策略能够显著提升反射表面的深度估计质量。
- 在整体深度精度上，该方法优于现有的自监督单目深度估计基线。
- 验证了“利用相机几何定位反射区域 + 反射感知损失/蒸馏”这一思路的有效性，为自监督深度估计处理反射场景提供了可行方案。

## 7. 优点

- **问题针对性强**：直接面向反射表面这一实际场景中的难点，弥补了自监督方法在非朗伯表面上的短板。
- **无需额外标注**：仍保持自监督特性，仅依靠相机几何和图像序列即可实现，扩展性好。
- **精细化处理**：逐像素定位反射区域，能够区分反射与非反射区域，避免一刀切影响正常区域。
- **结合蒸馏框架**：通过知识蒸馏选择性地传递区域知识，有利于模型在复杂场景中平衡不同区域的学习。
- **有效性得到跨数据集验证**：至少在多个数据集上声称优于 SOTA，具备一定泛化潜力。

## 8. 不足与局限

- **信息缺失**：提供的文本仅为摘要，缺少数据集、实验结果、消融、参数设置等关键细节，无法全面评估方法的可靠性与可复现性。
- **潜在依赖**：方法依赖不同视角之间的相机几何关系来定位反射区域，若相机位姿估计不准或多视角影像质量不佳，反射区域定位可能不准确。
- **反射类型覆盖有限**：摘要未明确说明方法对哪些类型的反射（如平面镜、曲面反射、强高光）有效，可能对复杂反射场景（如曲面反射、多重重影）仍有局限。
- **性能上限未知**：仅提到“优于 SOTA”，未给出相对提升幅度，无法判断实际增益是否显著。
- **应用限制**：若场景中反射区域极少或相机运动受限，该策略的收益可能不明显；同时，任何依赖光度一致性的自监督方法在动态物体、遮挡等场景中仍面临共性挑战。

（完）
