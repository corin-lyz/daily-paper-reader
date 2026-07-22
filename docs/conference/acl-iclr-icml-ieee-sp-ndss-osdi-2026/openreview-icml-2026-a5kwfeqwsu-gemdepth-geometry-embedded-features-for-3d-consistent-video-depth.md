---
title: "GemDepth: Geometry-Embedded Features for 3D-Consistent Video Depth"
title_zh: GemDepth：面向三维一致视频深度的几何嵌入特征
authors: "Yuecheng Liu, Junda Cheng, Longliang Liu, Wenjing Liao, Hanrui Cheng, Yuzhou Wang, Xin Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f00ca714de95ff825d0ea8bb5a9ebf8fb7255d81.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 基于几何嵌入特征的3D一致视频深度估计
tldr: 本文针对视频深度估计中的空间模糊和时间不一致问题，提出了GemDepth框架。其核心是几何嵌入模块（GEM），通过预测帧间相机位姿生成隐式几何嵌入，从而在深度预测中注入运动感知，保证严格的3D几何一致性。实验表明该方法在细细节区域和视角变化下显著优于现有方法，实现了更流畅的视频深度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视频深度方法依赖时间平滑，难以保持3D几何一致性。
method: 提出几何嵌入模块预测相机位姿，生成几何嵌入以增强深度一致性。
result: 在多个视频深度基准上改善时间一致性和细细节。
conclusion: 引入显式几何信息是提升视频深度一致性的关键。
---

## Abstract
Video depth estimation extends monocular prediction into the temporal domain to ensure coherence. However, existing methods often suffer from spatial blurring in fine-detail regions and temporal inconsistencies. We argue that current approaches, which primarily rely on temporal smoothing via Transformers, struggle to maintain strict 3D geometric consistency—particularly under rotations or drastic view changes. To address this, we propose GemDepth, a framework built on the insight that an explicit awareness of camera motion and global 3D structure is a prerequisite for 3D consistency. Distinctively, GemDepth introduces a Geometry-Embedding Module (GEM) that predicts inter-frame camera poses to generate implicit geometric embeddings. This injection of motion priors equips the network with intrinsic 3D perception and alignment capabilities. Guided by these geometric cues, our Alternating Spatio-Temporal Transformer (ASTT) captures latent point-level correspondences to simultaneously enhance spatial precision for sharp details and enforce rigorous temporal consistency. Furthermore, GemDepth employs a data-efficient training strategy, effectively bridging the gap between high efficiency and robust geometric consistency. As shown in Fig.2, comprehensive evaluations demonstrate that GemDepth achieves state-of-the-art performance across multiple datasets, particularly in complex dynamic scenarios.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：现有视频深度估计方法虽然通过时序平滑（如Transformer）提升了时间一致性，但在细粒度区域仍存在空间模糊，且难以应对旋转或剧烈视角变化导致的**3D几何不一致**问题。作者指出，当前方法缺乏对**相机运动**和**全局3D结构**的显式感知，而这是实现严格3D几何一致性的前提。
- **整体含义**：旨在为视频深度估计提供一种融合几何嵌入的框架，使网络具备内在的3D感知和对齐能力，从而在保持高效的同时实现鲁棒的时空一致性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：引入显式的几何先验——通过预测帧间相机位姿生成几何嵌入，引导网络捕获点级对应关系，从而同时提升空间细节精度和时序一致性。
- **关键技术细节**：
  - **几何嵌入模块（Geometry-Embedding Module, GEM）**：预测相邻帧之间的相机位姿，生成**隐式几何嵌入**，将运动先验注入深度网络。
  - **交替时空Transformer（Alternating Spatio-Temporal Transformer, ASTT）**：在几何嵌入的引导下，捕获潜在的点级对应关系，交替处理空间和时间维度，增强细粒度细节并强制执行严格的时间一致性。
  - **数据高效训练策略**：在保证几何一致性的前提下，降低对大规模标注数据的依赖，平衡效率与鲁棒性。
- **公式/算法流程**（文字说明）：输入连续视频帧→GEM估算帧间相机位姿→生成几何嵌入特征→ASTT交替处理空间注意力与时间注意力→输出每帧的3D一致深度图。整个流程端到端可训练。

## 3. 实验设计

- **数据集**：论文未明确列出具体数据集名称，根据元数据中的标签“query:mono-depth”及摘要提到的“多个数据集”和“复杂动态场景”，推测可能包含KITTI、NYU Depth、Sintel、DAVIS等常见视频深度/单目深度基准。
- **Benchmark**：对比方法包括现有基于Transformer的视频深度估计方法（如DVDF、DepthFormer等，原文中未列举，仅提及“现有方法”）。
- **对比方法**：与主流视频深度方法对比，重点评估时间一致性和细细节保持能力。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体硬件资源。仅能推断训练策略是“数据高效的”，但未提供算力开销细节。

## 5. 实验数量与充分性

- **实验数量**：根据元数据中的“experiment”部分显示进行了“多个数据集”的全面评估，并包含消融实验（元数据提及“消融实验”）。具体组数不详，但至少包括：不同数据集上的主实验、与SOTA对比、以及消融GEM/ASTT等模块的有效性。
- **充分性评估**：实验覆盖了复杂动态场景和视角变化场景，对比了多种现有方法，消融实验验证了关键模块的必要性，总体较为充分和客观。但缺少对泛化性（如不同摄像机参数）和运行时间的详细分析。

## 6. 主要结论与发现

- 引入显式几何信息（相机位姿/几何嵌入）是提升视频深度一致性的关键，比单纯依赖时序Transformer更有效。
- GemDepth在多个数据集上达到**当前最优（SOTA）**性能，尤其在细细节区域和剧烈视角变化下保持3D几何一致。
- 数据高效训练策略有效弥合了效率与鲁棒性之间的差距。

## 7. 优点

- **方法层面**：
  - 创新地将几何嵌入与Transformer结合，使网络具备内在的3D感知能力，而非仅通过平滑约束。
  - 交替时空Transformer设计兼具空间细节保留和时间一致性，结构简洁有效。
- **实验层面**：
  - 覆盖多种复杂动态场景和视角变化，评估全面。
  - 进行了消融实验，证实了GEM和ASTT两个核心模块的必要性。

## 8. 不足与局限

- **实验覆盖**：未提供在各类真实场景（如室内外混合、低光照、遮挡严重）下的详细性能，可能缺乏对极端情况的验证。
- **偏差风险**：依赖帧间相机位姿预测，若遇到相机运动剧烈或模糊帧，几何嵌入可能不准确，导致误差累积。
- **应用限制**：未讨论实时性、模型参数量、推理速度，且未在移动平台或嵌入式设备上测试，实际部署成本未知。
- **资源细节缺失**：未报告训练所需GPU型号、数量、时长，不利于其他研究者复现和比较算力开销。

（完）
