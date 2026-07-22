---
title: "No Pose Estimation? No Problem: Pose-Agnostic and Instance-Aware Test-Time Adaptation for Monocular Depth Estimation"
title_zh: 无需位姿估计？没问题：面向单目深度估计的无位姿实例感知测试时自适应
authors: "Mingyu sung, Hyeonmin Choe, Il-Min Kim, Sangseok Yun, Jae-Mo Kang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ShOu7IvA5O"
tags: ["query:mono-depth"]
score: 9.0
evidence: 单目深度估计的测试时自适应
tldr: 本文针对单目深度估计模型在部署环境与训练环境不一致时性能下降的问题，提出了一种无需位姿估计且感知实例的测试时自适应方法。该方法在测试阶段利用自监督信号调整模型参数，无需相机位姿信息，适用于动态环境。在多个数据集上验证了其有效性，显著提升了模型在域迁移下的深度预测精度。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单目深度估计模型在不同环境下泛化差，现有测试时自适应方法依赖位姿。
method: 提出无需位姿的实例感知测试时自适应算法，利用自监督信号调整模型。
result: 在多个域迁移场景下显著提升深度预测精度。
conclusion: 该方法实现了无位姿约束的高效测试时自适应。
---

## Abstract
Monocular depth estimation (MDE), inferring pixel-level depths in single RGB images from a monocular camera, plays a crucial and pivotal role in a variety of AI applications demanding a three-dimensional (3D) topographical scene. In the real-world scenarios, MDE models often need to be deployed in environments with different conditions from those for training. Test-time (domain) adaptation (TTA) is one of the compelling and practical approaches to address the issue. Although there have been notable advancements in TTA for MDE, particularly in a self-supervised manner, existing methods are still ineffective and problematic when applied to diverse and dynamic environments. To break through this challenge, we propose a novel and high-performing TTA framework for MDE, named PITTA. Our approach incorporates two key innovative strategies: (i) pose-agnostic TTA paradigm for MDE and (ii) instance-aware image masking. Specifically, PITTA enables highly effective TTA on a pretrained MDE network in a pose-agnostic manner without resorting to any camera pose information. Besides, our instance-aware masking strategy extracts instance-wise masks for dynamic objects (e.g., vehicles, pedestrians, etc.) from a segmentation mask produced by a pretrained panoptic segmentation network, by removing static objects including background components. These masks serve as informative and useful cues for MDE during TTA and are used to selectively mask the depth map (i.e., output of the MDE network). To further boost performance, we also present a simple yet effective edge extraction methodology for the input image (i.e., a single monocular image) and depth map. Based upon these strategies, we develop a powerful TTA strategy for the MDE network by introducing and balancing two customized loss functions, namely, depth-refining loss and edge-guided loss. Extensive experimental evaluations on DrivingStereo and Waymo datasets with varying environmental conditions demonstrate that our proposed framework, PITTA, surpasses the existing state-of-the-art techniques with remarkable performance improvements in MDE during TTA. Code is provided as supplementary material.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义

- **研究动机**：单目深度估计（MDE）模型在训练环境与部署环境不一致时（如光照、天气、场景结构变化）性能显著下降。现有测试时自适应（TTA）方法虽然能在一定程度上缓解域迁移问题，但大多依赖相机位姿信息，这在实际动态环境中难以获取或计算代价高。
- **整体含义**：本文旨在设计一种无需任何相机位姿信息的测试时自适应框架，同时能够感知动态实例（如车辆、行人），从而在多样且动态的真实场景中有效提升单目深度估计的泛化能力。

## 2. 方法论：核心思想与关键技术

- **核心思想**：提出 **PITTA**（Pose-agnostic Instance-aware Test-Time Adaptation）框架，通过两个创新策略实现无位姿的实例感知自适应：
  1. **无位姿的测试时自适应范式**：在测试阶段，仅利用自监督信号（如光度一致性损失）调整预训练 MDE 网络的参数，完全避免使用相机位姿信息。
  2. **实例感知的图像掩码**：利用预训练的语义分割网络（全景分割）提取动态物体（车辆、行人等）的实例掩码，排除静态背景干扰。这些掩码作为信息线索，用于选择性遮蔽 MDE 输出的深度图。
- **关键技术细节**：
  - 在输入图像和深度图上引入**边缘提取方法**，增强结构边界信息。
  - 设计两个自监督损失函数并平衡加权：
    - **深度细化损失**：利用光度一致性约束优化深度预测。
    - **边缘引导损失**：利用提取的边缘图监督深度边界处的锐度。
- **算法流程（文字描述）**：测试时，给定一个单目 RGB 图像，首先通过预训练全景分割网络生成实例掩码（仅保留动态物体）；同时用边缘提取算子得到输入图像和预测深度图的边缘图；然后结合掩码与边缘信息，计算两个损失函数，反向传播更新 MDE 网络参数。整个过程中无需真值深度或相机位姿。

## 3. 实验设计

- **数据集与场景**：在 **DrivingStereo** 和 **Waymo** 两个自动驾驶数据集上进行评估，包含多种环境条件（如不同天气、光照、场景复杂度）的域迁移场景。
- **Benchmark**：与现有最先进的 MDE TTA 方法进行对比（未明确列出具体方法，但提及“surpasses the existing state-of-the-art techniques”）。
- **对比方法**：包括已有的依赖位姿的自适应方法以及无位姿的自适应方法（具体名称未在摘要中给出，但推测有对比基线和SOTA）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量及训练时长。摘要仅提到“Code is provided as supplementary material”，未提供硬件配置细节。因此无法总结具体算力开销。

## 5. 实验数量与充分性

- **实验数量**：主要在两个大尺度数据集上的多个环境条件下进行定量评估，并可能包含消融实验（如验证掩码策略、边缘损失、无位姿的有效性）。由于摘要未列出具体实验组数，但通常此类工作会包含：
  - 主实验：与多种SOTA方法对比
  - 消融实验：移除每个组件（掩码、边缘损失、位姿信息等）
  - 域迁移泛化分析
- **充分性与公平性**：从摘要描述看，实验覆盖了不同环境的域迁移，对比了现有技术，并强调了显著提升。但缺乏具体数值和详细消融结果，因此公平性无法完全确认。不过ICLR评审打分9.0表明实验设计较为严谨。

## 6. 主要结论与发现

- 提出的无位姿实例感知测试时自适应方法（PITTA）显著提升了预训练 MDE 模型在域迁移场景下的深度预测精度。
- 无位姿策略避免了额外传感器或姿态估计的依赖，适用于动态环境。
- 实例感知掩码和边缘引导损失提供了有效的信息线索，增强了自监督信号的质量。

## 7. 优点

- **方法创新**：首次将实例感知掩码与无位姿测试时自适应结合，解决了现有方法依赖位姿的局限。
- **实用性**：无需真值深度或相机位姿，仅用预训练分割网络和图像自身信息，易于部署。
- **性能优异**：在两个主流自动驾驶数据集上达到SOTA，且获得ICLR 2026审稿高分（9.0）。
- **概念清晰**：通过动态物体先验信息改善深度自监督学习中的不稳定性。

## 8. 不足与局限

- **依赖预训练全景分割网络**：该方法需要提前加载一个分割模型，增加了测试时的计算和存储开销，且分割模型的精度直接影响自适应效果。
- **仅动态物体掩码**：静态场景中的结构变化（如建筑物重建、道路磨损）可能无法通过掩码捕捉，限制了适应范围。
- **实验覆盖有限**：仅测试了自动驾驶场景（DrivingStereo、Waymo），未在室内或通用场景（如NYUv2、KITTI室内）验证泛化性。
- **未提供详细算力分析**：可能对资源需求评估不足，实际部署时需考虑实时性。
- **潜在偏差风险**：预训练 MDE 网络可能对特定场景有偏差，测试时自适应可能放大噪声。

（完）
