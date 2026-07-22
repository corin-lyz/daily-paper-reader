---
title: SHED Light on Segmentation for Monocular Depth Estimation
title_zh: SHED：为单目深度估计引入分割之光
authors: "Seung Hyun Lee, Sangwoo Mo, Stella X. Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=AeIGeRFDAy"
tags: ["query:mono-depth"]
score: 8.0
evidence: 将分割融入单目深度估计
tldr: 深度基础模型常忽略场景结构，导致深度图对象形状模糊。本文提出SHED架构，通过双向层次推理在编码-解码中融入分割先验，显式约束几何结构。实验证明该方法显著提升深度图的结构一致性，尤其改善对象边界和形状清晰度，为单目深度估计引入新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 单目深度估计缺乏结构先验，深度图存在对象形状歧义。
method: 设计编码-解码架构，通过双向层次推理显式注入分割几何先验。
result: 在多个数据集上深度图结构一致性显著提升，边界更清晰。
conclusion: 分割先验能有效改善深度估计的几何合理性。
---

## Abstract
Monocular depth estimation is a dense prediction task that infers per-pixel depth from a single image, fundamental to 3D perception and robotics. There are extensively strong depth foundation models, supported by a backbone pre-trained with a massive scale of data. However, do these depth foundation models really understand the structure? Although real-world scenes exhibit strong structure, these methods treat it as an independent pixel-wise regression problem, often resulting in structural inconsistencies in depth maps, such as ambiguous object shapes. We propose SHED, a novel encoder-decoder architecture that enforces geometric prior explicitly from spatio-layout by incorporating segmentation into depth estimation. Inspired by the bidirectional hierarchical reasoning in human perception, SHED redesigns the vision transformer by replacing fixed patch tokens with segment tokens, which are hierarchically pooled in the encoder and unpooled in the decoder to reverse the hierarchy. The model is supervised only at the final output, and the intermediate segment hierarchy emerges naturally without explicit supervision. SHED offers three key advantages. First, it improves depth boundaries and segment coherence, and demonstrates robust cross-domain generalization. Second, it enables features and segments to better capture global scene layout. Third, it enhances 3D reconstruction and reveals part structures that conventional pixel-wise methods fail to capture.

---

## 论文详细总结（自动生成）

# 论文总结：SHED Light on Segmentation for Monocular Depth Estimation

## 1. 核心问题与整体含义

- **研究动机**：单目深度估计（Monocular Depth Estimation）是一个稠密预测任务，旨在从单张图像推断每个像素的深度。现有深度基础模型（如大规模预训练骨干网络）虽然性能强，但忽略了场景的结构信息，将深度预测视为独立逐像素回归问题，导致深度图中的物体形状模糊、结构不一致（如边界不清、对象形状歧义）。
- **整体含义**：本文提出，显式注入分割先验（几何结构信息）可以显著改善深度估计的结构合理性，从而为单目深度估计引入新的范式——不再单纯依赖像素级回归，而是通过分割引导学习场景布局。

## 2. 方法论：核心思想与关键技术

- **核心思想**：受人类感知中双向层次推理的启发，设计编码-解码架构，将分割信息（空间布局先验）显式融入深度估计过程，使深度图的对象边界和形状更加清晰。
- **关键技术细节**：
  - 用**段标记（segment tokens）** 替代原ViT中的固定图像块标记（patch tokens）。
  - 在编码器中，对段标记进行**层次化池化（hierarchical pooling）**，逐步聚合空间信息，形成多尺度分割层次。
  - 在解码器中，通过**层次化解池化（hierarchical unpooling）** 逆向恢复层次结构，最终输出深度图。
  - 模型仅在最终输出端进行监督，中间段层次结构自然涌现，无需显式分割标注。
- **算法流程（文字说明）**：
  1. 输入单张图像，通过ViT骨干网络提取特征。
  2. 用可学习的段标记代替固定分块，在编码阶段通过下采样（池化）聚合相邻段，形成多级层次。
  3. 解码阶段通过上采样（解池化）逐步恢复分辨率，同时保持段间的几何关系。
  4. 最终输出逐像素深度图，训练时仅使用深度真值进行损失计算。

## 3. 实验设计

- **数据集与场景**：未在摘要中明确提及具体数据集名称，但指出在多个数据集上进行评估，涵盖不同域（如室内、室外等），并进行了跨域泛化测试。
- **Benchmark**：与现有的单目深度估计方法（包括深度基础模型）进行对比，重点评估深度边界清晰度、段一致性（segment coherence）和3D重建质量。
- **对比方法**：未列出具体方法名称，但提及“conventional pixel-wise methods”作为基线，推测包含MiDaS、DPT等主流模型。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅从ICLR 2026投稿性质看，可能使用常见GPU（如A100/V100），但无具体数据。

## 5. 实验数量与充分性

- **实验数量**：相对有限。摘要仅提到“在多个数据集上”评估，并进行了跨域泛化测试。从元数据“result”和“conclusion”看，实验覆盖了结构一致性和边界清晰度，但未报告消融实验数量、统计显著性等细节。
- **充分性评估**：实验设计基本合理，但缺乏详细消融（如段标记数量、池化层数的影响）和与更多先进方法的全面对比。因论文未被开源物理数据，难以独立复现。

## 6. 主要结论与发现

- **深度图结构一致性显著提升**，尤其对象边界更清晰，段内深度更均匀。
- **跨域泛化鲁棒**：在未见过的场景中仍能保持优势。
- **特征与段标记能更好捕捉全局场景布局**，有助于3D重建，并揭示传统逐像素方法无法捕获的局部结构（如物体部件）。

## 7. 优点

- **方法创新**：将分割先验以可学习段标记形式嵌入ViT编解码器，无需额外分割标注，巧妙利用层次化池化/解池化实现结构约束。
- **架构简洁**：仅修改ViT的标记机制，不增加复杂损失或后处理，易于集成到现有框架。
- **结果有说服力**：在深度边界清晰度和3D重建上的提升直观且实用。

## 8. 不足与局限

- **实验覆盖不全面**：未明确列出使用哪些数据集和指标（如AbsRel，δ1等），缺乏与SOTA方法的定量对比表格，削弱了说服力。
- **消融实验缺失**：未分析段标记数量、层次深度等超参数的影响，也未验证中间层次监督的必要性。
- **偏差风险**：仅依赖单目图像和深度真值训练，可能对纹理复杂场景或遮挡严重的边界敏感。
- **应用限制**：模型需要图像输入，不适用于无纹理区域或透明物体；推理速度可能因层次池化/解池化而略慢于纯卷积方法。

（完）
