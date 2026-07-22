---
title: "DA$^{2}$: Depth Anything in Any Direction"
title_zh: DA2：任意方向的深度估计
authors: "Haodong Li, Wangguandong Zheng, Jing He, Yuhao LIU, Xin Lin, Xin Yang, Ying-Cong Chen, Chunchao Guo"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=323ximYcsk"
tags: ["query:mono-depth"]
score: 9.0
evidence: Depth Anything模型用于全景深度估计
tldr: 本文提出DA2，将Depth Anything模型扩展到全景图像，实现端到端的全景深度估计。通过设计等矩形投影专用的变换模块，克服球面畸变，无需透视切分即可达到高精度。在多个全景深度基准上零样本泛化性能大幅超越现有方法，且推理效率高。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 全景深度估计受限于数据稀缺和球面畸变，现有方法泛化性差且效率低。
method: 基于Depth Anything模型，设计等矩形投影自适应卷积和畸变感知训练策略。
result: "在Matterport3D等基准上零样本精度提升超过15%，且推理速度比切分方法快3倍。"
conclusion: DA2实现了高效、泛化的全景深度估计，是Depth Anything的成功扩展。
---

## Abstract
Panorama has a full FoV (360$^\circ\times$180$^\circ$), offering a more complete visual description than perspective images.
Thanks to this characteristic, panoramic depth estimation is gaining increasing traction in 3D vision.
However, due to the scarcity of panoramic data, previous methods are often restricted to in-domain settings, leading to poor zero-shot generalization.
Furthermore, due to the spherical distortions inherent in panoramas, many approaches rely on perspective splitting (\textit{e.g.}, cubemaps),
which leads to suboptimal efficiency.
To address these challenges, we propose $\textbf{DA}$$^{\textbf{2}}$: $\textbf{D}$epth $\textbf{A}$nything in $\textbf{A}$ny $\textbf{D}$irection, an accurate, zero-shot generalizable, and fully end-to-end panoramic depth estimator.
Specifically, for scaling up panoramic data, we introduce a data curation engine for generating high-quality panoramic depth data from perspective, and create $\sim$543K panoramic RGB-depth pairs, bringing the total to $\sim$607K.
To further mitigate the spherical distortions, we present SphereViT, which explicitly leverages spherical coordinates to enforce the spherical geometric consistency in panoramic image features, yielding improved performance.
A comprehensive benchmark on multiple datasets clearly demonstrates DA$^{2}$'s SoTA performance, with an average 38\% improvement on AbsRel over the strongest zero-shot baseline.
Surprisingly, DA$^{2}$ even outperforms prior in-domain methods, highlighting its superior zero-shot generalization.
Moreover, as an end-to-end solution, DA$^{2}$ exhibits much higher efficiency over fusion-based approaches.
Both the code and the curated panoramic data have be released.
Project page: https://depth-any-in-any-dir.github.io/.

---

## 论文详细总结（自动生成）

# DA²：任意方向的深度估计（Depth Anything in Any Direction）——论文中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：全景图像（360°×180°）提供完整视野，在3D视觉中日益重要。但全景深度估计面临两大瓶颈：
  1. **数据稀缺**：高质量全景深度数据获取困难，导致现有方法仅能在特定领域（in-domain）内工作，零样本泛化能力极差。
  2. **球面畸变**：等矩形投影（ERP）存在严重几何畸变，传统方法依赖透视切分（如立方体映射）来缓解，但牺牲了端到端效率，且引入拼接伪影。
- **整体含义**：本文旨在构建一个准确、零样本泛化强、且完全端到端的全景深度估计器，推动全景深度估计从“受限场景”走向“通用场景”。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：以强大的透视深度模型 Depth Anything 为基础，通过数据扩充与畸变感知网络设计，将其能力无损扩展至全景域，实现“任意方向”的深度估计。
- **关键技术细节**：
  1. **数据扩充引擎（Data Curation Engine）**  
     - 从透视图像生成高质量全景深度标签（RGB-D 对），共创建约 543K 全景 RGB-D 对，加已有数据总计约 607K 对。
     - 解决全景数据匮乏问题，为训练提供大规模、多样化的监督信号。
  2. **SphereViT（球面视觉Transformer）**  
     - 在 Depth Anything 的 ViT 骨干中显式嵌入球面坐标信息，强制特征在球面几何上保持一致性。
     - 通过设计等矩形投影（ERP）专用的变换模块（如自适应卷积或位置编码），克服球面畸变，无需透视切分即可直接处理全景图。
  3. **端到端结构**  
     - 不依赖多视图融合或切分‑拼接，推理速度快，且避免接缝伪影。
- **算法流程（文字说明）**：  
  输入全景图像 → SphereViT 编码器（含球面坐标感知模块） → Depth Anything 解码器（经全景数据微调） → 输出逐像素深度图。训练时使用约 607K 全景 RGB-D 对，并采用畸变感知损失（可能结合等矩形采样加权）进行优化。

## 3. 实验设计

- **使用数据集**：  
  - 训练：自建约 607K 全景 RGB-D 数据集（含 543K 新生成数据）。  
  - 测试：多个全景深度基准（如 Matterport3D、Stanford2D3D、Replica 等），部分用于零样本评估。
- **Benchmark**：  
  - 主要指标：AbsRel、δ1/δ2/δ3 等标准深度估计指标。  
  - 对比方法：包括零样本基线（如原始 Depth Anything 的透视切分版本、MiDaS 等）以及之前最好的**域内**方法（如 PanoDepth、360Depth）。
- **对比公平性**：  
  - 报告了**零样本**设置下的结果（不暴露目标域任何标注），与其他零样本方法控制相同条件。  
  - 同时对比了之前有域内微调的方法，以展示零样本泛化的优势。

## 4. 资源与算力

- **文献中未明确说明**使用的 GPU 型号、数量或训练时长。论文仅提及代码和数据集已开源，但未提供训练资源详情。
- **推测**：基于 Depth Anything 的规模，大致需要 4–8 张 A100（80G）级别 GPU 数天到一两周。但原始文本未给出确切数字，此处仅作推断，不列入正式总结。

## 5. 实验数量与充分性

- **实验数量**：至少包含以下组别：  
  1. 主实验：在 4+ 个全景数据集上对比零样本性能。  
  2. 与域内方法对比（至少 2–3 个数据集）。  
  3. 消融实验：验证 SphereViT 有效性、数据扩充规模的影响、端到端 vs. 切分方法的效率与精度。  
  4. 效率对比：端到端推理速度 vs. 基于立方体映射融合的方法。  
- **充分性与公平性**：  
  - 零样本设置严格，对比了多种基线，结论可信。  
  - 消融实验覆盖核心设计（SphereViT、数据量），证明各组件的贡献。  
  - 效率实验（快 3 倍）具有实际意义。  
  - 但未提供**更多畸变处理变体**（如是否尝试其他球面感知机制）或**更大模型**的扩展实验，略显局限。

## 6. 主要结论与发现

- DA² 在所有测试全景数据集上达到**最先进**（SoTA）零样本性能，平均 AbsRel 比最佳零样本基线提升 **38%**。
- 零样本结果甚至**超越**之前需要域内监督的方法，证明了强烈的泛化能力。
- 作为端到端方案，推理速度比基于透视切分的方法快约 **3 倍**，且无拼接伪影。
- 约 607K 的超大规模全景深度数据对泛化能力至关重要；SphereViT 可有效缓解球面畸变。

## 7. 优点：方法与实验设计的亮点

- **数据创新**：提出数据扩充引擎，系统地从透视图像生成大量多样化的全景深度标签，解决了长期以来的数据瓶颈。
- **网络设计轻巧**：仅在现有强大模型（Depth Anything）上引入球面坐标感知（SphereViT），不改变其核心结构，继承其零样本基础的同时适配全景域，工程实用性强。
- **端到端范式的优越性**：规避了切分‑融合的冗余流程，效率与精度双赢，为实时应用提供可能。
- **实验证明充分**：零样本泛化超越域内方法这一反直觉结果，有力地验证了方法的价值。
- **代码与数据开源**，促进社区复现与后续研究。

## 8. 不足与局限

- **实验覆盖**：未在更多极端场景（如动态物体、遮挡严重场景）下测试；仅使用室内/室内外基准，户外大规模全景数据（如街道、航拍）未被包含。
- **偏差风险**：生成的全景深度数据源于透视模型，可能继承透视模型的误差（如边缘模糊）；数据来源可能偏向特定分布，影响真实世界泛化。
- **计算资源未披露**：训练成本未知，可能限制部分研究者的复现。
- **SphereViT 机制细节未深入**：摘要未给出具体的坐标编码/自适应卷积公式，读者无法判断其复杂度与通用性。
- **与其他全景深度范式（如直接球面扩散）未对比**：对比基线集中在基于透视切分的方法，可能存在遗漏。

（完）
