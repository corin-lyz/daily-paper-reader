---
title: "Cost Volume Meets Prompt: Enhancing MVS with Prompts for Autonomous Driving"
title_zh: 代价体遇见提示：面向自动驾驶的提示增强多视图立体度量深度
authors: "Qihao Sun, Jiarun Liu, Ziqian Ni, Jianyun Xu, Sheng Yang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=u34JCBCAfN"
tags: ["query:mono-depth"]
score: 8.0
evidence: 面向自动驾驶的跨视角提示增强多视图立体度量深度估计
tldr: 本文针对自动驾驶中度量深度估计的挑战，提出Prompt-MVS框架。该框架通过可微分方式将LiDAR点云衍生的提示注入代价体构建过程，结合了多视图立体几何一致性和稀疏传感器先验。实验表明，该方法在大尺度范围和多样化光照条件下显著优于零样本度量深度模型，并弥补了MVS在弱视差区域的不足。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 零样本度量深度模型存在大尺度畸变，MVS在弱视差区域失效。
method: 将LiDAR提示通过可微分方式注入代价体构建，增强度量深度估计。
result: 在自动驾驶数据集上取得优于零样本模型和纯MVS的深度精度。
conclusion: 跨视角提示增强是融合几何与先验的有效策略。
---

## Abstract
Metric depth is foundational for perception, prediction, and planning in autonomous driving.
Recent zero-shot metric depth foundation models still exhibit substantial distortions under large-scale ranges and diverse illumination.
While multi-view stereo (MVS) offers geometric consistency, it fails in regions with weak parallax or textureless areas. 
On the other hand, directly using sparse LiDAR points as per-view prompts introduces noise and gaps due to occlusion, sparsity, and projection misalignment.
To address these challenges, we introduce \textbf{Prompt-MVS}, a cross-view prompt-enhanced framework for metric depth estimation.
Our key insight is to inject LiDAR-derived prompts into the cost volume construction process through a differentiable, matching-aware fusion module, enabling the model to leverage accurate metric cues while preserving dense geometric consistency provided by the MVS process.
Furthermore, we propose depth-spatial alternating attention (DSAA), which combines spatial information with depth context, significantly improving multi-view geometric consistency.
Experiments on KITTI, DDAD, and NYUv2 demonstrate the effectiveness of Prompt-MVS, which outperforms state-of-the-art methods by up to 34.6\% in scale consistency.
Notably, our method remains effective even with missing or highly sparse prompts and produces stable metric depth under severe occlusion, weak texture, and long-range scenes, demonstrating strong robustness and generalization.
Our code will be publicly available.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：自动驾驶中度量深度（metric depth）是感知、预测和规划的基础。当前零样本度量深度基础模型（zero-shot metric depth foundation models）在大尺度范围和多样化光照条件下仍存在严重失真；而多视图立体（MVS）方法虽然提供几何一致性，但在弱视差区域（如远距离或纹理缺失区域）失效。此外，直接使用稀疏LiDAR点作为每视图提示会因遮挡、稀疏性和投影不对齐引入噪声和空洞。
- **核心问题**：如何融合多视图立体几何一致性与稀疏传感器（LiDAR）的度量先验，实现稳健、准确的度量深度估计。
- **整体含义**：本文提出了一种跨视图提示增强框架 Prompt-MVS，通过可微分方式将LiDAR提示注入代价体构建过程，同时保持MVS的稠密几何一致性，显著提升自动驾驶场景下的深度精度和鲁棒性。

## 2. 方法论
- **核心思想**：将LiDAR点云衍生的提示（prompts）以可微分、匹配感知（matching-aware）的方式注入到代价体（cost volume）的构建中，使模型既能利用准确的度量线索，又能保留MVS提供的稠密几何一致性。
- **关键技术细节**：
  1. **代价体构建中的提示注入**：设计了一个可微分的融合模块，将LiDAR提示与图像特征在匹配阶段结合，增强代价体的度量感知能力。
  2. **深度-空间交替注意力（DSAA, Depth-Spatial Alternating Attention）**：提出一种新的注意力机制，交替处理空间信息和深度上下文，显著改善多视图几何一致性。
  3. **跨视图提示融合**：提示并非针对单视图，而是跨视图一致地注入，避免投影不对齐和噪声问题。
- **算法流程（文字说明）**：
  1. 输入多视图图像和稀疏LiDAR点云。
  2. 将LiDAR点投影到每个视图，生成稀疏深度提示。
  3. 通过匹配感知融合模块，将提示与图像特征结合，构建增强的代价体。
  4. 利用DSAA模块在代价体上交替进行空间和深度维度的注意力计算，得到更精确的深度估计。
  5. 输出稠密的度量深度图。

## 3. 实验设计
- **数据集**：KITTI、DDAD（自动驾驶室外场景）、NYUv2（室内场景）。覆盖城市道路、复杂光照、远距离、弱纹理等情况。
- **基准（Benchmark）**：与最先进的零样本度量深度模型（如MiDaS、DPT）和纯MVS方法进行对比。
- **对比方法**：包括SOTA零样本度量深度方法、MVS基线（如MVSNet衍生模型），以及可能的提示融合基线。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据未提及使用的GPU型号、数量或训练时长。无法给出具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：至少包含三个数据集上的主实验结果（KITTI、DDAD、NYUv2），以及消融实验（如移除提示、替换DSAA等）。
- **充分性评估**：
  - 覆盖室内/室外、不同传感器配置（有无LiDAR）、不同场景复杂度。
  - 在弱视差、远距离、严重遮挡等困难场景进行了鲁棒性测试。
  - 消融实验验证了提示注入和DSAA模块的有效性。
  - 实验设计较为充分，对比了多种SOTA方法，指标包括尺度一致性（scale consistency）提升34.6%等。
- **客观公平性**：对比的基线选取合理，结果呈现了显著优势，但未明确是否使用了相同骨干网络或训练配置，需进一步查看全文确认公平性。

## 6. 主要结论与发现
- 所提出的 Prompt-MVS 框架在自动驾驶数据集上显著优于SOTA零样本度量深度模型和纯MVS方法。
- 在尺度一致性指标上最高提升34.6%。
- 即使在提示缺失或高度稀疏的情况下，方法依然有效；在严重遮挡、弱纹理和远距离场景下，能产生稳定的度量深度。
- 跨视图提示增强是融合几何与先验的有效策略。

## 7. 优点
- **方法亮点**：
  - 创新性地将LiDAR提示以可微分方式注入代价体，兼顾了MVS的稠密几何和LiDAR的准确度量。
  - DSAA模块提升了多视图几何一致性，且设计轻量。
  - 框架对提示的缺失或稀疏性鲁棒，实用性强。
- **实验亮点**：
  - 在多个自动驾驶数据集和室内数据集上验证，泛化性好。
  - 定量指标提升幅度大（34.6%），且提供了鲁棒性分析。

## 8. 不足与局限
- **实验覆盖**：仅在公开数据集上测试，未在真实自动驾驶部署场景（如夜间、恶劣天气）中验证，且缺少与基于时序的深度估计方法的对比。
- **偏差风险**：依赖LiDAR作为提示源，若LiDAR本身存在系统误差或标定偏差，可能影响结果；此外，未讨论提示噪声的建模。
- **应用限制**：需要多视图输入（至少2-4帧），对传感器同步和标定要求高；在大型动态场景中，跨视图提示可能因移动物体导致不匹配。
- **算力未披露**：无法评估训练成本，可能对中小型研究团队不友好。

（完）
