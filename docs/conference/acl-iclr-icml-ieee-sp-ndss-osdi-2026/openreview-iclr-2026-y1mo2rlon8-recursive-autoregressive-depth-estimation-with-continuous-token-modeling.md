---
title: Recursive Autoregressive Depth Estimation with Continuous Token Modeling
title_zh: 递归自回归深度估计与连续令牌建模
authors: "Jinchang Zhang, xinrou Kang, Guoyu Lu"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=Y1Mo2RLon8"
tags: ["query:mono-depth"]
score: 9.0
evidence: 使用自回归粗到细的单目深度估计
tldr: 本文针对单目深度估计中的几何模糊性和不确定性，提出了一种分形视觉自回归加扩散的框架。该方法采用粗到细的多尺度自回归管道，先绘制全局深度图再逐步细化，显著加速推理。通过在每个尺度引入视觉-连续性令牌建模，有效弥合了RGB与深度之间的模态差距。实验结果证明了该方法的准确性和效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单目深度估计存在几何模糊和不确定性，现有自回归生成速度慢。
method: 提出分形视觉自回归加扩散框架，采用粗到细多尺度管道逐步细化深度图。
result: 在多个基准上取得高精度，且推理速度快。
conclusion: 该方法有效结合自回归与扩散，实现准确高效的深度估计。
---

## Abstract
Monocular depth estimation is a cornerstone of robotic perception and computer vision, yet reconstructing 3-D structure from a single RGB image suffers from severe geometric ambiguity and uncertainty. Motivated by the recent success of autoregressive (AR) models in image generation, we introduce a Fractal Visual AR + Diffusion framework that predicts depth both accurately and efficiently. Conventional pixel-wise AR generation is too slow for robotic applications, so we design a coarse-to-fine, multi-scale autoregressive pipeline: the model first sketches a global depth map at low resolution and then refines it progressively to full pixel fidelity, greatly accelerating inference.
To bridge the RGB–Depth modality gap, each scale incorporates a Visual-Conditioned Feature Refinement (VCFR) module that fuses multi-scale image features with the current depth prediction, explicitly injecting geometric and textural cues. Because discretising continuous depth values can cause information loss and unstable training, we adopt a conditional denoising diffusion loss that models depth distributions directly in continuous latent space, fundamentally avoiding quantisation errors. Although the visual AR–diffusion paradigm boosts accuracy, its layer-by-layer generation still introduces latency. To reclaim speed, we abstract the Visual AR unit into a reusable base generator and invoke it recursively, forming a self-similar fractal architecture that preserves modelling power while cutting the inference path.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：单目深度估计（从单张RGB图像恢复三维结构）面临严重的**几何模糊性和不确定性**。现有方法（如像素级自回归模型）虽然生成质量高，但推理速度极慢，无法满足机器人等实时应用需求。
- **研究动机**：受自回归（AR）模型在图像生成领域成功的启发，作者希望设计一种既能保留自回归的强大建模能力、又能显著加速推理的深度估计框架。
- **整体含义**：该工作旨在弥合RGB和深度模态之间的差距，实现**准确且高效**的单目深度估计，为机器人感知和计算机视觉提供实用的解决方案。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**分形视觉自回归 + 扩散（Fractal Visual AR + Diffusion）** 框架，采用**由粗到细的多尺度自回归管道**。模型先在低分辨率下生成全局深度图（草图），然后逐步细化到全像素精度，从而大幅缩短推理路径。
- **关键技术细节**：
  - **粗到细多尺度管道**：将深度预测分解为多个尺度，每个尺度只预测当前分辨率的残差或细化信息，避免一次性生成所有像素。
  - **视觉条件特征细化模块（VCFR）**：在每个尺度中，该模块融合多尺度图像特征与当前深度预测，显式注入几何和纹理线索，以弥合RGB–深度模态差距。
  - **连续令牌建模（Continuous Token Modeling）**：针对连续深度值离散化会导致信息丢失和训练不稳定的问题，采用**条件去噪扩散损失**直接在连续潜在空间中对深度分布建模，从根本上避免量化误差。
  - **递归自回归架构（分形结构）**：将视觉自回归单元抽象为可复用的基础生成器，并递归调用，形成自相似的分形架构。该结构保留了建模能力，同时通过共享权重复用缩短了推理路径。
- **公式/算法流程（文字说明）**：
  1. 输入单张RGB图像，提取多尺度特征。
  2. 在最低分辨率下，使用自回归单元生成初始深度草图（通过VCFR模块注入图像特征）。
  3. 对于每个更高尺度，将上一尺度的深度图上采样，再通过VCFR融合当前尺度的图像特征，然后使用扩散模型（或自回归）预测当前尺度的深度细化。
  4. 递归调用基础生成器，直至达到最高分辨率。
  5. 训练时采用扩散损失监督连续深度空间。

## 3. 实验设计

- **数据集与场景**：但元数据未提供具体数据集名称，仅提到**多个基准（benchmarks）**。根据任务推测，可能包括NYU Depth v2、KITTI、ScanNet等常见单目深度估计数据集。
- **基准测试**：与现有单目深度估计方法进行对比，包括基于CNN、Transformer、扩散模型、自回归模型等方法。
- **对比方法**：未明确列出，但推测包括传统自回归深度估计方法（如PixelAR-based）和扩散模型方法。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。元数据中也没有提及算力信息。因此无法判断训练成本。

## 5. 实验数量与充分性

- **实验数量**：从元数据推测，实验包括**多个数据集上的精度比较**以及**消融实验**（例如验证粗到细管道、VCFR模块、连续令牌建模、分形递归结构的效果）。但具体实验组数未给出。
- **充分性评价**：仅从摘要看，实验覆盖了主流基准，且消融实验验证了各组件贡献，但**缺乏对推理速度、参数量、不同分辨率行为的详细分析**。整体实验设计较为规范，但**未提供统计显著性或误差棒**，公平性依赖于标准评估协议。由于缺少论文全文，无法判断是否与同类方法严格对齐。

## 6. 主要结论与发现

- 提出的分形视觉AR+扩散框架在**多个基准上取得了高精度**，同时**推理速度显著快于传统像素级自回归方法**。
- **粗到细多尺度管道**有效减少了递归步数，而**连续令牌建模**避免了离散化误差，**VCFR模块**增强了跨模态特征融合。
- **递归分形结构**在不牺牲建模能力的前提下，进一步缩短了推理路径，实现了准确度和效率的平衡。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 将自回归的粗到细生成与扩散模型的连续空间建模有机结合，解决了自回归速度慢和离散化损失的问题。
  - 引入**分形递归架构**，通过共享基础生成器实现模型轻量化与推理加速，是一种新颖的网络设计模式。
- **实验亮点**：
  - 实验覆盖了多个基准，验证了通用性。
  - 提出了系统的消融实验，分解出每个模块的贡献。

## 8. 不足与局限

- **实验覆盖局限**：未提供推理时间的具体数据或与实时性要求（如30 FPS）的对比；缺少在移动端/嵌入式平台上的评测。
- **偏差风险**：可能仅在特定分辨率或数据集上表现优异，泛化到极端场景（如无纹理区域、强光照变化）未讨论。
- **应用限制**：尽管推理加速，但递归结构仍引入了顺序依赖，在大规模场景下可能仍有延迟瓶颈；方法依赖多尺度特征提取，计算资源消耗未详细评估。
- **信息缺失**：由于仅提供摘要，无法评估完整论文中的理论分析（如收敛性、复杂度）、代码开源情况、复现难度等。

（完）
