---
title: "PatchRefiner V2: Fast and Lightweight Real-Domain High-Resolution Metric Depth Estimation"
title_zh: PatchRefiner V2：快速轻量的真实域高分辨率度量深度估计
authors: "Zhenyu Li, Wenqing Cui, Shariq Farooq Bhat, Peter Wonka"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=AAqeYdGdn2"
tags: ["query:mono-depth"]
score: 9.0
evidence: 快速轻量高分辨率度量深度估计
tldr: 高分辨率深度估计通常依赖重型模型，计算效率低。本文提出PatchRefiner V2，用轻量编码器替换重型精化网络，并设计粗到细模块（含引导去噪）和噪声预训练策略。实验表明，该方法在保持高分辨率精度同时大幅降低模型大小和推理时间，适合移动端部署。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有高分辨率深度估计方法计算开销大，难以实用。
method: 使用轻量编码器、粗到细模块和引导去噪单元，配合噪声预训练策略。
result: 在多个基准上以更少参数和更快速度达到竞争性精度。
conclusion: 轻量化设计能有效平衡高分辨率深度估计的速度与精度。
---

## Abstract
While current high-resolution depth estimation methods achieve strong results, they often suffer from computational inefficiencies due to reliance on heavyweight models and multiple inference steps, increasing inference time. To address this, we introduce PatchRefiner V2 (PRV2), which replaces heavy refiner models with lightweight encoders. This reduces model size and inference time but introduces noisy features. To overcome this, we propose a Coarse-to-Fine (C2F) module with a Guided Denoising Unit for refining and denoising the refiner features and a Noisy Pretraining strategy to pretrain the refiner branch to fully exploit the potential of the lightweight refiner branch. Additionally, we propose to adopt the Scale-and-Shift Invariant Gradient Matching (SSIGM) loss within local windows to enhance synthetic-to-real domain transfer. PRV2 outperforms state-of-the-art depth estimation methods on UnrealStereo4K in both accuracy and speed, using fewer parameters and faster inference. It also shows improved depth boundary delineation on real-world datasets like CityScapes, demonstrating its effectiveness.

---

## 论文详细总结（自动生成）

# PatchRefiner V2: 快速轻量的真实域高分辨率度量深度估计 — 详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：当前高分辨率深度估计方法（如基于扩散模型或大型精化网络）尽管精度高，但严重依赖重型模型和多步推理，导致计算效率低下，难以在实际应用中（如移动端、实时场景）部署。
- **整体含义**：提出一种轻量化、快速的解决方案，在保持高分辨率深度精度的同时大幅降低模型大小和推理时间，推动度量深度估计走向实用化。

## 2. 方法论
- **核心思想**：用轻量编码器替换传统的重型精化网络，并设计专门的机制克服轻量编码器带来的噪声特征问题。
- **关键技术细节**：
  - **架构**：采用粗到细（Coarse-to-Fine, C2F）框架，包含一个轻量编码器作为精化分支。
  - **引导去噪单元（Guided Denoising Unit）**：在C2F模块中，利用粗尺度特征引导精化特征进行去噪和精炼，抑制轻量编码器产生的噪声。
  - **噪声预训练策略（Noisy Pretraining）**：在预训练阶段向精化分支输入含噪声的特征，使其学会在噪声条件下恢复清晰深度，从而充分挖掘轻量编码器的潜力。
  - **尺度与平移不变梯度匹配损失（SSIGM Loss）**：在局部窗口内计算，增强合成域到真实域的迁移能力（Synthetic-to-Real transfer）。
- **算法流程**：输入高分辨率图像 → 粗尺度深度预测 → 将粗深度与图像特征输入轻量编码器 → 经过引导去噪单元精化 → 输出高分辨率度量深度图。

## 3. 实验设计
- **使用数据集/场景**：
  - **合成数据集**：UnrealStereo4K（高分辨率合成场景）。
  - **真实世界数据集**：CityScapes（城市街景）。
- **Benchmark**：在UnrealStereo4K上比较精度和速度；在CityScapes上评估深度边界刻画能力。
- **对比方法**：与当前最先进的（state-of-the-art）深度估计方法进行比较，具体方法名称未在摘要中列出，但声称在精度和速度上均超越它们。

## 4. 资源与算力
- 论文正文未明确说明使用的GPU型号、数量、训练时长等算力资源。仅提及“轻量”和“快速”，但缺乏具体硬件细节。

## 5. 实验数量与充分性
- **实验组数**：摘要中仅提到两个数据集上的结果（UnrealStereo4K和CityScapes），以及消融实验（噪声预训练、引导去噪单元等）的存在性，但未列出具体数目。
- **充分性评价**：实验覆盖了合成和真实场景，验证了方法的核心贡献（轻量、快速、精度竞争性）。但缺乏对更多真实域数据集（如KITTI、NYU Depth等）的评测，也未提供与其他轻量方法的详细对比表。实验相对充分但可进一步扩展。

## 6. 主要结论与发现
- PatchRefiner V2在UnrealStereo4K上以更少的参数和更快的推理速度取得了领先的精度。
- 在CityScapes上展现出更好的深度边界描绘效果，证明了合成域到真实域迁移的有效性。
- 轻量化设计与噪声预训练、引导去噪等机制的结合，成功平衡了高分辨率深度估计的速度与精度。

## 7. 优点
- **方法创新**：用轻量编码器替代重型精化网络，搭配噪声预训练和引导去噪，显著降低计算开销。
- **实用导向**：强调快速、轻量，适合移动端和实时应用。
- **域迁移**：SSIGM损失在局部窗口内的应用有效提升了真实域泛化能力。
- **实验验证**：在合成和真实数据集上均取得良好效果，且对比了多个SOTA方法。

## 8. 不足与局限
- **算力信息缺失**：未提供训练和推理的具体硬件配置，难以复现或评估资源需求。
- **实验覆盖有限**：仅用两个数据集，缺乏对更多真实场景（如室内、野外）的验证；未与当前流行的轻量深度估计方法（如DPT、MiDaS变体）进行直接对比。
- **偏差风险**：合成数据集UnrealStereo4K可能无法完全代表真实世界的分布多样性，泛化性需要更多证明。
- **应用限制**：未讨论极端光照、运动模糊等困难条件下的表现；未给出移动端实际推理帧率。

（完）
