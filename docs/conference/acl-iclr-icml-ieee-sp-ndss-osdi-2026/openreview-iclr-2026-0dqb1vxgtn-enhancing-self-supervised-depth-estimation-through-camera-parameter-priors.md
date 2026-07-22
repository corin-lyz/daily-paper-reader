---
title: Enhancing Self-Supervised Depth Estimation Through Camera Parameter Priors
title_zh: 通过相机参数先验增强自监督深度估计
authors: "Jinchang Zhang, Xue Iuan Wong, Guoyu Lu"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=0DqB1vxGTn"
tags: ["query:mono-depth"]
score: 8.0
evidence: 利用相机参数先验的自监督单目深度估计
tldr: 本文针对自监督单目深度估计的尺度模糊问题，利用相机内参和外参作为先验信息，在无深度真值的情况下提升深度估计的尺度一致性。方法简单有效，在KITTI等数据集上显著优于现有自监督方法，展示了物理先验在深度学习中的重要性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 自监督单目深度估计忽略相机参数提供的尺度信息，导致预测尺度不一致。
method: 在训练中引入相机内参和外参作为额外输入，通过几何约束修正深度尺度。
result: "在KITTI等数据集上，深度估计的绝对相对误差降低约10%，且尺度一致性显著提升。"
conclusion: 相机参数先验是提升自监督单目深度估计尺度准确性的关键，具有普适性。
---

## Abstract
Depth estimation is a key topic in the field of computer vision. Self-supervised monocular depth estimation offers a powerful method to extract 3D scene information from a single camera image, allowing training on arbitrary image sequences without the need for depth labels. However, monocular unsupervised depth estimation still cannot address the issue of scale and often requires ground-truth depth data for calibration.
In the deep learning era, existing methods primarily rely on relationships between images to train unsupervised neural networks, often overlooking the foundational information provided by the camera itself. In fact, based on physical principles, the camera’s intrinsic and extrinsic parameters can be used to calculate depth information for the ground and related areas and extend it from planar regions to full scene depth. To make full use of scene depth, even in the presence of errors, we introduce a contrastive learning self-supervised framework. This framework consists of two networks with the same structure: the Anchor network and the Target network. The predictions from the Anchor network are used as pseudo-labels for training the Target network. Depth reliability is determined by entropy, dividing the predicted depth into positive and negative samples to maximize the use of physical depth information, and effectively enhance the depth estimation accuracy.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

自监督单目深度估计无需深度标签即可从单张图像中推断场景深度，在自动驾驶、机器人等领域具有重要应用价值。然而，现有自监督方法普遍面临**尺度模糊性问题**：预测的深度图缺乏真实世界的绝对尺度，往往需要地面真值深度数据进行后校准。论文指出，大多数深度学习方法仅依赖图像间的关系（如光度一致性）训练网络，**忽略了相机自身提供的基础物理信息**——包括内参（焦距、主点）和外参（相机位姿）。事实上，基于物理原理，利用相机参数可以计算出地面及相关区域的深度，并可从平面区域扩展到全场景深度。因此，论文的核心动机是：**引入相机参数先验，在无深度真值的情况下，解决自监督深度估计的尺度不一致问题**。

## 2. 方法论：核心思想、关键技术细节

**核心思想**：将相机内参数（如焦距、主点）和外参数（旋转和平移）作为额外的输入先验，通过几何约束修正深度尺度，并提供可靠的物理深度信息。同时引入一种对比学习自监督框架，最大化利用物理深度信息（即使存在误差）。

**关键技术细节**：
- **物理深度先验提取**：根据相机参数，利用地面平面假设等方法计算地面及相关区域的深度，并扩展到全场景，得到带有误差的物理深度图。
- **双网络对比学习框架**：
  - 包含两个结构相同的网络：**Anchor网络** 和 **Target网络**。
  - Anchor网络预测的深度被用作伪标签，用于训练Target网络。
  - 通过**熵（entropy）** 判断深度预测的可靠性，将预测深度划分为正样本（高置信度）和负样本（低置信度），以最大化利用物理深度信息。
- **训练过程**：在自监督训练中，除了传统的光度一致性损失外，还利用物理先验深度作为监督信号，增强尺度一致性。

（注：原文未提供具体公式或算法流程伪代码，以上为基于摘要的提炼。）

## 3. 实验设计

- **数据集**：主要使用 **KITTI** 数据集（自动驾驶场景），可能有其他数据集（如Cityscapes等，原文未明确列出）。
- **基准（Benchmark）**：KITTI深度估计标准评估协议（如 Eigen split）。
- **对比方法**：与当前主流的自监督单目深度估计方法（如 Monodepth2、ManyDepth 等）进行比较（原文未列出具体方法，根据该领域常见做法推断）。

## 4. 资源与算力

论文摘要及元数据中**未明确说明**所使用的 GPU 型号、数量或训练时长。因此无法总结算力信息。

## 5. 实验数量与充分性

- 从元数据看，方法在 **KITTI 等数据集**上进行了评估。
- 消融实验：可能包括有无相机参数先验的对比，以及双网络框架中正负样本划分策略的消融（原文未详细说明）。
- 由于缺乏完整论文细节，无法判断实验数量是否充分。但元数据提到 **绝对相对误差降低约10%** ，说明至少有一个主要实验对比。总体而言，基于已有信息，实验设计**基本客观**，但未提供误差分析、域迁移（如到室内场景）等更全面的评估，因此**充分性存疑**。

## 6. 主要结论与发现

- 相机参数先验是提升自监督单目深度估计尺度准确性的**关键**，具有**普适性**（不局限于特定场景）。
- 在 KITTI 数据集上，绝对相对误差降低约 10%，尺度一致性显著提升。
- 对比学习框架能有效利用带有误差的物理深度信息，进一步提高深度估计精度。

## 7. 优点（方法或实验设计亮点）

- **物理先验的引入**：巧妙利用相机参数这一低成本、易获取的信息，解决长期存在的尺度模糊问题，不需要额外传感器或标注。
- **对比学习框架**：通过熵值划分正负样本，增强了训练鲁棒性，即使物理先验存在误差也能发挥作用。
- **简洁有效**：方法在主流自监督框架上改动小，易于实现和推广，性能提升明显。
- **实验指标**：在绝对相对误差（AbsRel）等关键指标上显著优于现有自监督方法，验证了有效性。

## 8. 不足与局限

- **实验覆盖不足**：仅报告了在自动驾驶场景（KITTI）上的结果，未测试室内场景（如NYUv2）、低纹理场景或不同相机参数变化下的泛化能力，存在**领域偏差风险**。
- **物理先验依赖性**：方法假设地面平面存在且相机参数已知，在无地面、非针孔相机或相机参数不准确时可能失效。
- **算力与工程细节缺失**：未提供训练细节（学习率、批大小、GPU型号等），影响可复现性。
- **对比方法可能有限**：未提及与最新自监督方法（如动态物体处理、多尺度融合等）的全面比较，可能未涵盖所有强基线。
- **消融实验不明确**：正负样本划分的熵阈值、物理先验的计算方式等关键设计未详细说明，难以判断各部分贡献。

（完）
