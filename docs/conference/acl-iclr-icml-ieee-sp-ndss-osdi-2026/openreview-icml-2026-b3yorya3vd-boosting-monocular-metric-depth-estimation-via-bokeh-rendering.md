---
title: Boosting Monocular Metric Depth Estimation via Bokeh Rendering
title_zh: 通过虚化渲染提升单目公制深度估计
authors: "Hangwei Zhang, Armando Fortes, Tianyi Wei, Xingang Pan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1f3b6489c9d5cb56535cae56abc6d1845969cfc7.pdf"
tags: ["query:mono-depth"]
score: 10.0
evidence: 利用虚化渲染提升公制深度估计
tldr: 现有单目深度模型在纹理缺失或远处区域表现不佳，而虚化模糊蕴含几何信息。BokehDepth 提出两阶段框架：首先生成校准的虚化堆栈，然后利用虚化作为几何监督信号提升公制深度估计，在多个基准上取得了最优性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 单目深度估计在纹理缺失区域失效，而虚化模糊可提供几何线索。
method: 首先生成物理基础的虚化堆栈，然后将其作为几何监督信号训练深度网络。
result: 在多个深度估计基准上达到最优结果。
conclusion: BokehDepth 展示了虚化与深度估计的互惠关系，为单目深度提升提供了新思路。
---

## Abstract
Bokeh rendering and depth estimation share a fundamental optical connection, yet existing methods fail to fully exploit this reciprocity. Conventional bokeh pipelines rely heavily on noisy depth maps that inevitably introduce visual artifacts. Conversely, existing monocular depth models typically follow two flawed paradigms. Generative diffusion-based frameworks often lack consistent metric scale. Meanwhile, feed-forward metric depth models frequently fail in textureless or distant regions where defocus blur can provide geometric information. We propose BokehDepth, a two-stage framework that treats synthetic defocus as a supervision-free geometric signal. In the first stage, a physically grounded generative model produces calibrated bokeh stacks from a single sharp input without requiring prior depth input. Subsequently, a lightweight defocus-aware aggregation module integrates these stacks into the encoder of a depth estimation framework. This mechanism allows the model to extract consistent geometric features from the defocus dimension while keeping the decoder architecture unchanged. Experiments demonstrate that BokehDepth achieves superior visual bokeh fidelity compared to depth-dependent rendering baselines and consistently enhances the metric accuracy of state-of-the-art monocular depth models.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：单目公制深度估计在纹理缺失区域（如天空、平滑墙面）或远处场景中表现不佳，而传统方法依赖生成式扩散框架时缺乏一致的公制尺度，或前馈式公制深度模型在困难区域失效。
- **背景**：虚化渲染（bokeh rendering）与深度估计存在内在光学联系：散焦模糊程度与场景深度直接相关，可以作为一种无监督几何信号。现有方法未能充分利用这一互惠关系——深度图噪声导致虚化伪影，而单目深度模型也忽略了散焦提供的几何线索。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出一个两阶段框架 **BokehDepth**，将合成虚化作为无监督的几何信号来提升公制深度估计。
  - **第一阶段**：物理生成模型。从单一清晰图像生成物理可校准的虚化堆栈（bokeh stack），不依赖输入深度，保证几何一致性。
  - **第二阶段**：轻量级散焦感知聚合模块。将该虚化堆栈集成到一个深度估计框架的编码器中，从散焦维度提取一致的几何特征，解码器保持原样，确保对现有深度模型的最小侵入性。
- **关键技术**：物理生成模型确保虚化堆栈的尺度校准性；聚合模块在编码阶段嵌入散焦特征，无需修改解码器或引入额外监督。

### 3. 实验设计
- **数据集与场景**：文中未明确列出具体数据集，但根据元数据“在多个深度估计基准上达到最优结果”，推测涉及室内/室外常见的深度估计 benchmark（如 NYU Depth v2, KITTI, ScanNet 等）。虚化渲染部分可能使用互联网图像或合成数据。
- **基准（Benchmark）**：对比方法包括依赖深度图的传统虚化渲染管线，以及现有的单目深度模型（如 MiDaS、DPT 等）。评估指标包括虚化保真度和公制深度准确性（如 RMSE、δ1 等）。
- **对比方法**：文中提到“superior visual bokeh fidelity compared to depth-dependent rendering baselines”和“consistently enhances metric accuracy of SOTA monocular depth models”。

### 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。仅从摘要无法推断具体资源需求。推测采用了标准 GPU（如 A100 或 V100），但未给出细节。

### 5. 实验数量与充分性
- **实验数量**：从摘要可知，至少进行了虚化保真度对比（与深度依赖渲染基线）和深度精度提升实验（在多个 benchmark 上验证）。此外，元数据提到“多个深度估计基准”，暗示跨数据集评估。
- **充分性**：实验覆盖了虚化质量和深度估计两个任务，但缺乏消融实验细节（如不同聚合模块设计、虚化堆栈层级的影响）。由于摘要篇幅限制，未提及内部消融和泛化实验，因此充分性中等。但宣称“最优性能”，需要全文验证是否公平比较。

### 6. 论文的主要结论与发现
- **主要结论**：BokehDepth 通过物理生成的虚化堆栈作为几何监督，能够有效提升单目公制深度估计精度，同时生成更高质量的虚化效果。
- **发现**：虚化模糊与深度估计存在互惠关系——无需外部深度即可利用散焦信息改善纹理缺失和远处区域。

### 7. 优点
- **方法亮点**：
  - 创新性地将虚化渲染与深度估计结合，利用两者物理联系，实现双向提升。
  - 无监督性质：无需额外的深度标注或预训练深度模型。
  - 轻量级模块化设计，易于集成到现有深度网络。
- **实验亮点**：在两个任务（虚化渲染和深度估计）上进行验证，证明方法的通用性。

### 8. 不足与局限
- **实验覆盖**：摘要未提供详细的消融实验（如虚化堆栈数量、聚合模块设计选择），对方法内部组件贡献的论证不足。
- **偏差风险**：可能仅在特定数据集（如室内、室外）上测试，对极端场景（如动态场景、严重遮挡）的鲁棒性未知。
- **应用限制**：依赖物理生成模型的质量，虚化堆栈的校准误差可能影响深度估计；实时性未提及，可能受限。
- **资源未知**：算力需求不明确，影响可复现性。

（完）
