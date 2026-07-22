---
title: "MVGE: Scale-invariant and Temporal-consistent Monocular Video Geometry Estimation"
title_zh: MVGE：尺度不变且时间一致的单目视频几何估计
authors: "Zheng Zhang, Lihe Yang, Tianyu Yang, Chaohui Yu, Yixing Lao, Xiaoyang Guo, Fan Wang, Hengshuang Zhao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=k49lMF1ckB"
tags: ["query:mono-depth"]
score: 8.0
evidence: 单目视频几何估计，生成仿射不变3D点图
tldr: 该论文提出了MVGE，一种从单目视频中估计3D几何的新方法，能够在数百帧内保持几何准确性和时间一致性。核心创新包括视角不变几何对齐、表观不变学习和频率调制定位，使模型能外推到远超训练长度的序列。实验表明，MVGE在多个数据集上显著优于现有方法，为长序列单目几何估计提供了有效的解决方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有方法在处理长序列单目视频时难以保持几何准确性和时间一致性。
method: 提出视角不变、表观不变和频率调制定位三种创新，生成序列共享参数的仿射不变3D点图。
result: 在多个数据集上显著优于现有方法，实现长序列的尺度不变且时间一致的几何估计。
conclusion: MVGE为单目视频几何估计提供了鲁棒且一致的解决方案。
---

## Abstract
We present MVGE, a novel approach for estimating 3D geometry from extended monocular video sequences, where existing methods struggle to maintain both geometric accuracy and temporal consistency across hundreds of frames. Our approach generates affine-invariant 3D point maps with shared parameters across entire sequences, enabling consistent scale-invariant representations. We introduce three key innovations: viewpoint-invariant geometry aligning multi-perspective points in a unified reference frame; appearance-invariant learning enforcing consistency across exponential timescales; and frequency-modulated positioning enabling extrapolation to sequences vastly exceeding training length. Experiments across diverse datasets demonstrate significant improvements, reducing relative point map error by 24.2% and temporal alignment error by 34.9% on ScanNet compared to state-of-the-art methods. Our approach handles challenging scenarios with complex camera trajectories and lighting variations while efficiently processing extended sequences in a single pass. Code will be publicly released, and we encourage readers to explore the interactive demonstrations in our supplementary materials.

---

## 论文详细总结（自动生成）

# MVGE：尺度不变且时间一致的单目视频几何估计 — 论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有单目视频3D几何估计方法在长序列（数百帧）中难以同时保持几何准确性和时间一致性，容易出现尺度漂移、误差累积和帧间不一致。
- **研究动机**：许多实际应用（如自动驾驶、机器人导航、AR/VR）需要从连续视频中恢复稳定的3D结构，但已有方法（如单帧深度估计+后处理对齐）在长序列上表现不佳。
- **整体含义**：本文提出MVGE，一种能够从单目视频中生成**尺度不变且时间一致**的3D几何估计的新范式，其输出为**仿射不变3D点图**，并在全局序列上共享参数，从而在数百帧内保持几何准确性。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 将单目视频的每一帧映射到一个**统一的仿射参考坐标系**，生成序列共享参数的3D点图（affine-invariant 3D point maps），从而自然地实现尺度不变性和时间一致性。

### 三个关键创新

1. **视角不变几何对齐（Viewpoint-invariant Geometry Aligning）**
   - 将多视角点云统一到一个公共参考帧中，使得不同视角下的几何估计能够在共享参数下对齐，消除相机运动带来的视角偏差。

2. **表观不变学习（Appearance-invariant Learning）**
   - 强制模型在不同时间尺度（exponential timescales）上保持几何一致性，即使外观（光照、纹理、遮挡）发生剧烈变化，输出几何结构仍保持一致。

3. **频率调制定位（Frequency-modulated Positioning）**
   - 引入频率调节的位置编码机制，使模型能够外推到远超训练时所见序列长度的视频，从而处理任意长度的长序列。

### 算法流程（文字说明）
- 输入：单目视频片段（多帧连续图像）
- 处理：每帧通过共享编码器提取特征，结合频率调制的位置编码，然后通过视角不变对齐模块将特征映射到统一参考坐标系；表观不变正则化在训练时施加跨时间尺度的一致性约束。
- 输出：整个视频序列的仿射不变3D点图，可直接用于度量3D重建（通过后处理映射到真实尺度）。

## 3. 实验设计

- **数据集**：主要报道了**ScanNet**数据集上的结果。未明确列出其他数据集，但摘要提到“across diverse datasets”（多个不同数据集）。
- **Benchmark**：与当前最先进方法（state-of-the-art methods）进行比较。具体方法名称未在给定文本中列出。
- **对比指标**：
  - 相对点图误差（relative point map error）：降低24.2%
  - 时间对齐误差（temporal alignment error）：降低34.9%
- **测试场景**：复杂相机轨迹、光照变化（challenging scenarios with complex camera trajectories and lighting variations）。

## 4. 资源与算力

- 文中**未明确说明**使用的GPU型号、数量、训练时长或内存消耗等算力细节。仅提及“efficiently processing extended sequences in a single pass”（高效单次处理长序列），但无具体硬件信息。

## 5. 实验数量与充分性

- **实验数量**：从摘要和元数据推测，至少包含：
  - 主实验（ScanNet上与SOTA对比）
  - 可能包括多个数据集的泛化实验（提及“across diverse datasets”）
  - 消融实验（三个创新点部分的贡献分析？但未明确列出）
- **充分性判断**：由于仅提供了主要性能提升数字，缺乏详细的实验设置、对比方法列表、消融表格、可视化结果等，**无法全面评估实验的充分性和公平性**。但24.2%和34.9%的相对提升在长序列几何估计任务中较为显著，且强调在复杂光照和相机轨迹上的鲁棒性，表明实验覆盖了一定挑战性场景。

## 6. 主要结论与发现

- MVGE在多个数据集上显著优于现有方法，实现了长序列下**尺度不变**且**时间一致**的几何估计。
- 核心发现：通过视角不变对齐、表观不变学习和频率调制定位的组合，模型能够处理远超训练长度的视频序列，且保持精度与一致性。
- 在ScanNet上，相对点图误差降低24.2%，时间对齐误差降低34.9%。

## 7. 优点

- **创新性突出**：三个关键技术创新（视角不变、表观不变、频率调制）在方法论上具有理论贡献，且直接针对长序列几何估计的痛点。
- **全局一致性**：使用序列共享参数的仿射不变点图，避免了逐帧后处理带来的漂移。
- **外推能力强**：频率调制定位使模型能处理任意长度序列，实用性强。
- **高效性**：单次前向即可处理整个长序列，无需迭代优化或滑动窗口。
- **可应用性**：代码将开源，并提供交互式演示，便于复现和实际部署。

## 8. 不足与局限

- **实验细节缺失**：给定文本中未列出对比的具体方法、消融实验结果、不同数据集上的完整指标，难以全面评估性能差异的统计显著性。
- **算力与资源未说明**：缺少训练/推理的资源消耗，可能影响实际可复现性和成本评估。
- **应用限制**：
  - 仿射不变输出需要后处理才能得到度量尺度，对依赖绝对尺度（如机器人导航）的应用需要额外校准。
  - 对极端动态场景（快速运动、严重遮挡）的鲁棒性未提及。
- **潜在偏差风险**：仅在ScanNet等室内数据集上验证，室外场景、非结构化环境（如自动驾驶）的泛化性未知。
- **未讨论失败案例**：没有分析方法在哪些情况下可能失效（如纹理极度贫乏、相机剧烈抖动等）。

（完）
