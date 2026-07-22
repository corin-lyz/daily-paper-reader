---
title: Large Depth Completion Model from Sparse Observations
title_zh: 大规模稀疏观测深度补全模型
authors: "Zhu Yu, zhengyi zhao, Runmin Zhang, Lingteng Qiu, Kejie Qiu, Yisheng He, Siyu Zhu, Zilong Dong, Si-Yuan Cao, Hui-liang Shen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=I9o2OkPwCX"
tags: ["query:depth-refine"]
score: 9.0
evidence: 大规模深度补全模型，利用单目基础模型提升稀疏深度质量
tldr: 该论文提出LDCM，利用现有单目基础模型提㐀稀疏深度输入质量，并结合基于泊松的深度初始化策略和重新设计的训练目标，从稀疏观测生成度量精确的密集深度图。该方法简单而强大，在多个数据集上超越了现有深度补全方法。它直接服务于深度图精修和超分需求，尤其适合稀疏输入场景。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 单视图度量深度补全中，稀疏观测质量差且几何结构难以保持。
method: 利用单目基础模型改进稀疏深度，采用泊松初始化生成粗密深度图，再优化训练目标。
result: 在多种稀疏观测下取得最优性能。
conclusion: 为大规模深度补全提供了有效且简单的基线。
---

## Abstract
This work presents the Large Depth Completion Model (LDCM), a simple, effective, and robust framework for single-view metric depth estimation with sparse observations. Without relying on complex architectural designs, LDCM generates metric-accurate dense depth maps use a transformer. It outperforms existing approaches across diverse datasets and sparse observations. We achieve this from two key perspectives: (1) leveraging existing monocular foundation models to improve the quality of sparse depth inputs, and (2) reformulating training objectives to better capture geometric structure and metric consistency. Specifically, a Poisson-based depth initialization strategy is firstly introduced to generate a uniform coarse dense depth map from diverse sparse observations, providing a strong structural prior for the network. Regarding the training objective, we replace the conventional depth head with a point map head that regresses per-pixel 3D coordinates in camera space, enabling the model to directly learn the underlying 3D scene structure instead of performing pixel-wise depth map restoration. Moreover, this design eliminates the need for camera intrinsic parameters, allowing LDCM to naturally produce metric-scaled 3D point maps. Extensive experiments demonstrate that LDCM consistently outperforms state-of-the-art methods across multiple benchmarks and varying sparsity levels in both depth completion and point map estimation, showcasing its effectiveness and strong generalization to unseen data distributions. Code and models are publicly available at \href{https://pkqbajng.github.io/ldcm/}{pkqbajng.github.io/ldcm/}.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：单视图度量深度补全任务中，输入为稀疏深度观测（如激光雷达点或SfM点），这些观测往往质量不佳（噪声、不均匀分布），且难以保持场景的几何结构。
- **整体含义**：本文提出大规模深度补全模型（LDCM），旨在从稀疏、不均匀的深度观测中生成度量精确的密集深度图，同时不依赖复杂网络架构，为下游任务（如三维重建、机器人感知）提供简单有效的基线。

## 2. 论文提出的方法论

### 核心思想
- 联合利用（1）现有单目深度基础模型提升稀疏观测质量；（2）重新设计训练目标以捕获几何结构和度量一致性。
- 避免传统逐像素深度回归，转而直接学习相机空间下的三维场景结构。

### 关键技术细节
1. **稀疏深度改进**：利用预训练的单目深度基础模型对稀疏输入进行补全与去噪，提升输入观测的质量与稠密度。
2. **泊松深度初始化**：引入基于泊松方程的深度初始化策略，从不同稀疏度的观测中生成均匀的粗尺度密集深度图，为网络提供强的结构先验。
3. **点图头（Point Map Head）**：将传统深度回归头替换为点图头，输出每个像素在相机空间下的三维坐标（X, Y, Z），使模型直接学习底层3D场景结构，而非仅恢复深度值。
4. **消除相机内参依赖**：点图头设计使得模型无需相机内参（如焦距、主点）即可自然生成具有度量尺度的三维点图。

### 算法流程说明
1. 输入稀疏深度观测 → 经单目基础模型处理 → 得到更稠密、质量更高的稀疏深度。
2. 应用泊松初始化生成粗尺度密集深度图。
3. 粗深度图与图像特征一起输入Transformer网络。
4. 网络通过点图头回归每个像素的相机空间三维坐标。
5. 通过度量一致的损失函数（如3D点位置误差）进行端到端训练。

## 3. 实验设计

- **数据集/场景**：论文未在摘要中列出具体数据集名称，但提及在多个公开基准（multiple benchmarks）上进行实验，覆盖不同数据集和不同稀疏度水平。
- **基准（Benchmark）**：深度补全任务和点图估计任务（point map estimation）。
- **对比方法**：与现有最先进方法（state-of-the-art）进行对比，包括深度补全和点图估计两类方法。
- **稀疏度变化**：实验覆盖了不同稀疏度（如均匀采样、随机采样、SfM稀疏点等）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。但从“大规模”模型名称推断可能使用了多GPU训练，但无准确数据。

## 5. 实验数量与充分性

- **实验数量**：摘要指出进行了“大量实验”（extensive experiments），但在提供的文本中未列出具体消融数量。从描述看，至少包括：多个数据集上的主实验结果、不同稀疏度对比、深度补全与点图估计两个任务、与SOTA对比等。
- **充分性与公平性**：论文声称“一致优于现有方法”，且突出了泛化到未见数据分布的能力。实验设计覆盖多种稀疏观测，具有一定客观性。但缺乏具体数据量（如消融组数、数据集规模），无法完全判断实验完备度。

## 6. 论文的主要结论与发现

1. LDCM在深度补全和点图估计两个任务上，在多个基准和多种稀疏度下均一致优于现有最先进方法。
2. 泊松深度初始化能提供有效的结构先验，消除稀疏观测分布不均匀的影响。
3. 点图头设计比传统深度头更利于学习3D几何结构，且无需相机内参，提升了模型的泛化性。
4. 方法简单有效，不依赖复杂架构，可作为大规模深度补全的强基线。

## 7. 优点

- **简洁有效**：利用现有单目基础模型和泊松初始化，避免复杂设计。
- **消除相机内参依赖**：点图头使得模型可直接输出度量尺度的3D点图，适用于内参未知的场景。
- **强泛化能力**：在未见数据分布上表现优异。
- **代码开源**：提供公开可访问的代码和模型，有利于复现和应用。

## 8. 不足与局限

- **算力与效率未讨论**：未报告训练时间、模型参数量、推理速度等效率指标，难以判断资源开销。
- **实验细节缺失**：由于摘要内容有限，未列出具体数据集名称、消融实验组数、误差指标（如RMSE, δ1等），无法从文本独立评估实验公平性。
- **局限性未分析**：论文未讨论在极端稀疏、无纹理区域、动态场景等困难条件下的表现，也未对比与纯学习方法的差异。
- **依赖基础模型质量**：性能可能受限于用到的单目基础模型（如泛化性、推理速度），未讨论该依赖带来的风险。

（完）
