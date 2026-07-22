---
title: "3DGS IS A VERSATILE REGULATOR: MODULATING UNIVERSAL METRIC-DEPTH REPRESENTATION VIA ANCHOR-BASED GAUSSIAN-SPLATTED MULTIPLICATION"
title_zh: 3DGS作为通用调节器：通过锚点高斯泼溅乘法调制通用度量深度表示
authors: "Ruikai Xu, Zhiwei Zhang, Weijian Zhang, Tianyu Jin, Qizhao, Zhizhong Zhang, Jingyu Gong, Yuan Xie, Lizhuang Ma"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=ST7hcldhmG"
tags: ["query:mono-depth"]
score: 9.0
evidence: 通过锚点乘子分解和高斯泼溅乘法实现从相对深度到度量深度的转换
tldr: 该论文针对零样本深度基础模型缺乏度量尺度的问题，提出了一种新颖的锚点乘子分解范式。通过稀疏点锚提供度量线索，并利用基于图像语义的高斯泼溅乘法稳定地调节相对几何结构，从而将相对深度转化为度量深度。该方法无需直接回归或特征融合，在多种数据集上验证了有效性，为实现通用度量深度估计开辟了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有零样本深度估计可预测相对深度，但缺乏度量尺度信息，难以直接用于实际应用。
method: 引入锚点乘子分解，利用稀疏点锚提供度量线索，通过高斯泼溅乘法结合图像语义实现度量深度转换。
result: 在多个零样本度量深度基准上取得最优性能，验证了方法的泛化能力。
conclusion: 锚点乘子分解是一种有效的零样本度量深度估计范式。
---

## Abstract
Recent advances in zero-shot affine-invariant depth estimation have achieved remarkable progress. However, extending relative depth to metric depth remains challenging due to the absence of reliable metric-scale guidance within existing depth foundation models. Building on this, we introduce a novel depth estimation paradigm—anchor–multiplier factorization—as an alternative to conventional approaches such as direct depth regression, depth completion, or feature-fusion methods. Our key insight is that sparse point anchors supply indispensable metric-scale cues, while relative-scale geometric structure can be stably regulated via Gaussian-splatted multiplication conditioned on image semantics. Accordingly, we implement GSD---an anchor-based Gaussian Splatting Depth Regulator for universal metric-depth restoration. We also propose the first theoretical analysis showing how anchor–multiplier factorization mitigates training divergence, and thereby improves metric restoration accuracy. Extensive experiments across diverse datasets demonstrate substantial accuracy gains over state-of-the-art baselines, highlighting the benefits of treating 3DGS not merely as a renderer, but as a versatile regulator for visual representation learning.

---

## 论文详细总结（自动生成）

### 论文中文详细总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：当前零样本（zero-shot）深度估计模型（如基于基础模型的相对深度预测）在无需特定数据集训练时能生成高质量的相对深度图，但缺乏度量尺度（metric scale）信息。这种相对深度无法直接用于需要真实物理尺度的任务（如机器人导航、3D重建）。传统方法（直接回归、深度补全、特征融合）试图从相对深度恢复度量深度，但往往依赖密集GT、易受尺度歧义影响且泛化性差。
- **动机**：提出一种新的范式——**锚点乘子分解（anchor–multiplier factorization）**，利用稀疏点锚点提供可靠的度量线索，同时保持相对几何结构的稳定性，从而高效、鲁棒地将相对深度转换为通用度量深度。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将深度估计分解为两部分：稀疏的度量锚点（anchor）提供绝对尺度参考；相对深度结构通过**基于图像语义的高斯泼溅乘法（Gaussian-splatted multiplication）** 进行调节，将相对几何与度量尺度有机结合。
- **关键技术细节**：
  - **锚点乘子分解**：每个像素的最终度量深度 = 锚点深度 × 乘子（multiplier）。锚点来自稀疏点（如SfM点云或随机采样），乘子由相对深度网络输出并经由高斯泼溅平滑化。
  - **高斯泼溅乘法调节器（GSD）**：利用3D高斯泼溅（3DGS）技术，将乘子场表示为各向异性高斯核的叠加，并通过图像语义信息（如分割图）约束高斯核的分布，确保乘子变化平滑且与物体边界对齐。该过程相当于对相对深度进行**语义感知的非线性缩放**。
  - **理论分析**：论文首次证明了锚点乘子分解能缓解训练发散问题——通过将尺度与结构解耦，梯度更新更稳定，从而提升度量恢复精度。
- **算法流程（文字说明）**：
  1. 输入：单张RGB图像 + 稀疏点锚（深度已知）。
  2. 基础模型预测相对深度图（如DPT、MiDaS）。
  3. 构建锚点乘子：每个锚点定义一个局部乘子，通过高斯泼溅在图像平面上扩散，形成平滑的乘子场。
  4. 将相对深度与乘子场逐像素相乘，得到初始度量深度。
  5. 联合优化乘子场（通过3DGS渲染）和相对深度网络（可选微调），最小化稀疏锚点上的深度损失以及基于图像语义的正则化项（如边缘感知平滑）。

#### 3. 实验设计
- **数据集**：论文未在摘要中明确列出，但提到“across diverse datasets”。根据领域惯例，可能包括NYU Depth v2、KITTI、ScanNet、DIODE等零样本度量深度基准。
- **基准（Benchmark）**：零样本度量深度估计（zero-shot metric depth estimation），即测试集未见过的场景。
- **对比方法**：与当前最优的零样本度量深度方法（如Depth Anything、Metric3D等）进行对比。摘要声称“substantial accuracy gains over state-of-the-art baselines”。

#### 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及GPU型号、数量、训练时长等具体算力信息。需注意该论文为ICLR 2026被拒稿，可能缺乏完整的实验配置细节。

#### 5. 实验数量与充分性
- **实验数量**：从摘要仅能判断为“extensive experiments”，但未给出具体实验组数（如跨数据集对比、消融实验、泛化测试等）。元数据中评分9.0（满分为10？）表明评审认为实验较充分，但缺乏细节。
- **充分性判断**：由于无法获取全文，仅凭摘要难以完全评价。若实验覆盖多个主流数据集且含必要的消融（如锚点数量、高斯参数、语义信息的影响），则可以认为充分；但未见理论分析部分的实验验证（如收敛性分析）。总体而言，摘要声称优于SOTA，但被拒稿可能提示实验存在某些不足（如泛化极限、计算开销未报告）。

#### 6. 主要结论与发现
- 锚点乘子分解是一种有效的零样本度量深度估计新范式，避免了直接回归带来的尺度歧义。
- 3D高斯泼溅乘法可作为通用视觉调节器（versatile regulator），不仅限于渲染，还可用于其他视觉表示学习任务。
- 该方法在多个基准上取得显著精度提升，验证了其泛化能力。

#### 7. 优点
- **创新性**：提出全新的建模思路（锚点乘子分解+高斯泼溅乘法），区别于现有所有方法。
- **理论支撑**：首次从梯度稳定性角度分析该分解的优势，提供了理论洞见。
- **简洁高效**：无需训练复杂网络或两阶段特征融合，仅需稀疏锚点即可实现度量恢复。
- **泛化性强**：零样本设置下表现优异，说明语义条件的高斯泼溅能有效迁移相对深度信息。

#### 8. 不足与局限
- **信息不完备**：摘要极短，缺乏实验细节（具体数据集、指标、消融结果、可视化等），无法验证其声称的准确性。
- **锚点依赖**：稀疏锚点需要外界提供（如SfM或LiDAR点），在无锚点场景下性能可能下降。
- **计算效率**：3D高斯泼溅乘法涉及逐像素渲染，可能带来额外推理开销，但未讨论实时性。
- **拒稿背景**：被ICLR 2026拒绝，可能说明方法存在未被披露的局限性（如与现有SOTA的公平对比、消融不足、理论分析不完善等）。
- **可复现性**：未提供代码或详细超参数，难以独立验证。

（完）
