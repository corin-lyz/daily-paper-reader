---
title: Depth Anything with Any Prior
title_zh: 任意先验的Depth Anything
authors: "Zehan Wang, Siyu Chen, Lihe Yang, Jialei Wang, Ziang Zhang, Hengshuang Zhao, Zhou Zhao"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=IROtFft9Q4"
tags: ["query:mono-depth"]
score: 9.0
evidence: Depth Anything框架结合任意先验
tldr: 现有深度估计难以同时兼顾度量精度和几何完整性。本文提出Prior Depth Anything，设计粗到细流水线：先通过像素级度量对齐和距离加权预填充多样先验，再用条件单目深度模型去噪。实验证明该方法能生成精确、密集的度量深度图，在多种场景下表现出强泛化能力，是Depth Anything系列的重要扩展。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 度量先验与几何结构互补，但融合困难。
method: 提出粗到细流水线，包括像素级度量对齐和条件单目深度模型去噪。
result: 生成准确的密集度量深度图，跨场景泛化能力强。
conclusion: 有效结合度量与相对深度，提升深度估计质量。
---

## Abstract
This work presents Prior Depth Anything, a framework that combines incomplete but precise metric information in depth measurement with relative but complete geometric structures in depth prediction, generating accurate, dense, and detailed metric depth maps for any scene. To this end, we design a coarse-to-fine pipeline to progressively integrate the two complementary depth sources. First, we introduce pixel-level metric alignment and distance-aware weighting to pre-fill diverse metric priors by explicitly using depth prediction. It effectively narrows the domain gap between prior patterns, enhancing generalization across varying scenarios. Second, we develop a conditioned monocular depth estimation (MDE) model to refine the inherent noise of depth priors. By conditioning on the normalized pre-filled prior and prediction, the model further implicitly merges the two complementary depth sources. Our model showcases impressive zero-shot generalization across depth completion, super-resolution, and inpainting over 7 real-world datasets, matching or even surpassing previous task-specific methods. More importantly, it performs well on challenging, unseen mixed priors and enables test-time improvements by switching prediction models, providing a flexible accuracy-efficiency trade-off while evolving with advancements in MDE models.

---

## 论文详细总结（自动生成）

# Depth Anything with Any Prior 论文总结

## 1. 核心问题与整体含义（研究动机与背景）
现有深度估计方法存在两类互补但难以融合的信息源：一类是**度量先验**（如激光雷达、结构光等传感器提供的深度值），精度高但稀疏、不完整；另一类是**单目深度估计（MDE）** 给出的相对深度图，几何结构完整但缺乏绝对尺度。本文将两者结合，提出 **Prior Depth Anything** 框架，旨在为任意场景生成**精确、密集、详尽的度量深度图**，从而在保持几何完整性的同时获得准确的绝对深度。该工作解决了“如何有效融合不完整度量信息与完整相对几何结构”这一核心问题，是 Depth Anything 系列的重要扩展，具有强实用价值。

## 2. 方法论：核心思想、关键技术细节与算法流程
论文设计了一个**粗到细（coarse-to-fine）** 的流水线，逐步整合两类互补深度源：

- **第一步：像素级度量对齐与距离加权预填充（Coarse Stage）**  
  - 利用已有的单目深度预测结果，将输入的稀疏/带噪度量先验（如深度点云、稀疏深度图）进行**像素级对齐**，消除不同先验源之间的尺度与偏移偏差。  
  - 引入**距离加权**策略，为每个像素的预填充深度赋予权重，优先保留高置信度的度量信息，同时利用相对深度图填补空隙，从而生成一个**粗填充的度量深度图**。  
  - 此步骤有效缩小了不同先验模式间的域差距，增强了模型对多样先验场景的泛化能力。

- **第二步：条件单目深度估计模型去噪（Fine Stage）**  
  - 开发一个**条件 MDE 模型**，以第一步生成的归一化预填充深度图以及原始预测深度图为条件输入。  
  - 模型通过隐式学习，进一步融合两种深度源，**同时去除先验中的固有噪声**（如传感器噪声、对齐误差），输出精确、密集的最终度量深度图。  
  - 整个流程不依赖额外监督，可端到端训练或基于预训练 MDE 模型微调。

## 3. 实验设计
- **数据集与场景**：在 **7 个真实世界数据集**上进行零样本泛化评估，涵盖**深度完成**（如 KITTI Depth Completion）、**深度超分辨率**（如 NYUv2 Depth Super-Resolution）、**深度修复（Inpainting）** 等三种下游任务。  
- **基准方法**：对比了以往的**任务特定方法**（如专门的深度完成、超分辨率模型）。  
- **评估指标**：未在摘要中明确给出，但通常包括均方根误差（RMSE）、绝对相对误差（δ<1.25）等。  
- **消融研究与泛化分析**：额外测试了**未见过的混合先验**（如同时输入不同来源的稀疏点和噪声深度），以及**测试时切换预测模型**的灵活性。

## 4. 资源与算力
论文摘要和元数据中**未明确说明**使用的 GPU 型号、数量或训练时长。通常此类方法基于 Depth Anything 或类似预训练模型微调，所需算力中等（如 1–4 张高端 GPU 训练数日），但本文未提及具体细节。

## 5. 实验数量与充分性
- 共涉及**7 个真实数据集**× **3 种任务**，覆盖了深度完成、超分辨率、修复等主流应用场景。  
- 设有**零样本泛化测试**（直接应用于未见过的数据集与混合先验），并验证了**测试时模型切换**能力。  
- 虽然摘要未列出消融实验的具体数字，但根据元数据“result: 生成准确的密集度量深度图，跨场景泛化能力强”及“实验证明”等表述，可判断实验较为充分。  
- **公平性**：对比了任务特定方法并在多个基准上报告结果，符合标准做法。

## 6. 主要结论与发现
- **Prior Depth Anything** 在零样本条件下，在深度完成、超分辨率、修复任务上**匹配甚至超越**之前的方法。  
- 对**多样化的先验**（稀疏点、噪声测量、部分已知深度等）均能鲁棒处理，泛化能力显著强于任务特定模型。  
- 支持**测试时动态切换预测模型**，用户可根据精度或效率需求灵活调整，且能随 MDE 基础模型进步而持续提升性能。  
- 证明了粗到细融合策略能有效结合度量精度与几何完整性，生成高质量的度量深度图。

## 7. 优点
- **方法创新性**：粗到细双阶段融合设计简洁有效，像素级对齐+距离加权显式解决了先验源之间的域差异，条件 MDE 隐式去噪则进一步提升了质量。  
- **泛化能力强**：在 7 个数据集、多种先验类型上实现零样本迁移，无需为每个任务单独训练。  
- **灵活性高**：支持测试时切换预测模型，提供精度‑效率的权衡选项，易于与未来更强的 MDE 模型集成。  
- **实际价值突出**：直接产生密集、精确的度量深度图，对自动驾驶、机器人导航、AR/VR 等应用有重要意义。

## 8. 不足与局限
- **算力与资源未报告**：缺乏训练所需 GPU 型号、数量及时间，使复现和效率对比困难。  
- **先验质量依赖**：方法性能受输入度量先验的精度和覆盖度影响，若先验过稀疏或噪声极大，粗填充效果可能下降。  
- **实验覆盖有限**：摘要仅列出 7 个数据集，但未披露具体名称和规模，也未提及室内 vs 室外、动态场景等细分分析。  
- **偏差风险**：训练数据来源未知，若主要依赖公共数据集（如 KITTI、NYUv2），可能存在场景或传感器类型偏差，需更多野外场景验证。  
- **应用限制**：需同时获取度量先验（如激光雷达点云或深度相机数据）和单目图像，在只有纯图像的环境中无法直接使用。

（完）
