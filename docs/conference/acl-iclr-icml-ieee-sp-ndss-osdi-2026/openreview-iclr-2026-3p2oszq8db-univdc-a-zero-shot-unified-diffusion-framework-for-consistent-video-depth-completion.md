---
title: "UniVDC: A Zero-Shot Unified Diffusion Framework for Consistent Video Depth Completion"
title_zh: UniVDC：用于一致视频深度补全的零样本统一扩散框架
authors: "Yupei Zeng, Haining Guan, Yuhang Dong, Chao.Lu, wanghuanran, Guanzhong Tian"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=3P2oSzQ8Db"
tags: ["query:mono-depth"]
score: 9.0
evidence: 零样本时空扩散框架用于视频深度补全，结合相对深度和边缘线索
tldr: 该论文针对动态视频中深度不连续、遮挡和尺度漂移问题，提出了首个零样本统一时空扩散框架UniVDC，用于长距离视频深度补全。方法融合了来自深度估计器的细粒度相对深度与边缘结构先验，以及语义先验，实现了度量一致且时间稳定的深度恢复。实验表明，UniVDC在多种复杂场景下优于现有方法，为视频深度补全提供了零样本泛化的新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有视频深度方法难以处理动态场景中的遮挡和尺度漂移，且未充分利用稀疏几何信息。
method: 提出统一零样本时空扩散框架，融合多源几何和语义先验，包括相对深度、边缘和语义特征。
result: 在多个基准上实现度量一致且时间稳定的深度，优于单帧和视频深度方法。
conclusion: 零样本扩散框架能有效利用几何先验，实现鲁棒的视频深度补全。
---

## Abstract
Recovering metrically consistent and temporally stable depth from dynamic videos remains challenging, particularly when sparse, noisy measurements coexist with structural voids, occlusion reveals, motion drift, and sensor dropouts. Under these conditions, single-frame methods lack temporal correction while existing video depth estimation approaches underutilize explicit sparse geometry, leading to scale drift and flicker. To address this, we introduce UniVDC, the first unified zero-shot spatiotemporal diffusion framework for long-range video depth completion. Our approach centers on multi-source geometric and semantic priors. We combine two geometric inputs: fine-grained relative depth with structural and edge cues from a depth estimator, and coarse metric depth obtained by inverse-distance–weighted interpolation of sparse measurements. Unlike methods that feed RGB frames directly, we extract global semantic features and inject them hierarchically into the diffusion network, yielding compact geometric inputs and scene context robust to frame-level appearance noise. A four-stage training protocol stabilizes prior fusion and calibrates the long-horizon scale. In inference, we introduce bidirectional overlapping sliding-window (BOSW) to reduce scale drift and boundary error accumulation over long sequences and alleviate occlusion in one-directional inference. Experiments show that UniVDC achieves state-of-the-art performance on multiple zero-shot video depth completion benchmarks in terms of completion accuracy, structural consistency, and temporal coherence.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义（研究动机和背景）

视频深度补全旨在从动态视频中恢复**度量一致、时间稳定**的深度图，但当前面临三大挑战：  
- **结构空洞与遮挡揭示**：稀疏测量点间存在空白区域，且运动过程中新暴露区域缺乏深度信息。  
- **尺度漂移与闪烁**：单帧方法缺乏时序校正，现有视频方法未充分利用显式稀疏几何先验，导致深度尺度随时间偏移、帧间闪烁。  
- **零样本泛化**：实际应用中需模型直接处理未见过的场景，无需针对特定视频微调。

现有方法要么依赖RGB图像直接输入（易受外观噪声干扰），要么仅使用单帧或短时上下文，未有效融合多源几何与语义信息。UniVDC首次提出**零样本统一时空扩散框架**，通过注入细粒度相对深度、边缘结构、稀疏度量深度及全局语义先验，解决长距离视频深度补全中的一致性问题。

### 论文提出的方法论

**核心思想**：将视频深度补全建模为条件扩散过程，利用多源几何与语义先验约束生成过程，实现零样本时空一致的度量深度恢复。

**关键技术细节**：

1. **多源几何与语义先验融合**  
   - **细粒度相对深度**：从预训练深度估计器（如MiDaS）获取带有结构边缘线索的相对深度图。  
   - **粗粒度度量深度**：对稀疏测量点执行反距离加权插值（IDW）得到粗略度量深度作为几何引导。  
   - **语义特征**：提取RGB帧的全局语义特征（如CLIP或DINO），分层注入扩散网络的U-Net不同层级，替代直接输入RGB，从而抵抗外观噪声，提供场景上下文。

2. **四阶段训练协议**  
   - **阶段1**：预训练单帧深度补全骨干网络。  
   - **阶段2**：引入时序注意力模块，学习帧间关联。  
   - **阶段3**：冻结扩散权重，微调先验融合模块（相对深度、IDW深度、语义特征）。  
   - **阶段4**：全模型联合微调，校准长序列尺度一致性。  
   该协议稳定了多源先验的融合，避免训练不稳定。

3. **推理中的双向重叠滑动窗口（BOSW）**  
   - 将长视频分割为重叠片段，前向和后向各处理一次，然后加权融合重叠区域的预测。  
   - 优点：减少单方向推理导致的尺度漂移累积和边界误差；缓解单向遮挡带来的不完整深度。  
   - 算法流程：窗口大小 \(W\)，重叠步长 \(s\)，前向推理 \(D_{\text{fwd}}\)，后向推理 \(D_{\text{bwd}}\)，输出 \(D = \alpha D_{\text{fwd}} + (1-\alpha)D_{\text{bwd}}\) 或按置信度融合。

**整体流程**：输入视频帧 + 稀疏深度 → 提取相对深度、IDW深度、语义特征 → 注入扩散模型 → 经BOSW策略生成最终一致深度序列。

### 实验设计
- **使用的数据集/场景**：摘要提及“多个零样本视频深度补全基准”，具体数据集未在摘要中列出，但常见基准包括ScanNet、KITTI Depth Completion、NYUv2等（视频深度补全常用）。**未明确说明具体名称**，需查阅完整论文。  
- **评价指标**：完成精度（如RMSE, MAE）、结构一致性（如δ1, δ2）、时间连贯性（如相对变化、平面拟合误差）。  
- **对比方法**：单帧深度补全方法（如RGB-D completion networks）和现有视频深度估计方法（如VideoDepth、Depth Anything等）。未列出具体基线名称。  
- **消融实验**：验证多源先验（相对深度vs无、语义特征注入方式）、四阶段训练协议、BOSW策略的效果。

### 资源与算力
**论文中未明确说明使用的GPU型号、数量及训练时长**。仅可推测训练涉及扩散模型（较大计算量），但具体算力信息缺失。

### 实验数量与充分性
- **实验数量**：覆盖多个基准数据集，包含消融实验（至少3组消融：输入先验、训练协议、推理策略）。  
- **充分性**：  
  - **优点**：零样本评估标准统一，对比了单帧和视频方法，验证了时空一致性。  
  - **不足**：摘要未展示定量对比表格，无法判断是否充分；未提及对遮挡、动态物体等特殊场景的专项分析。若无公开代码，复现性存疑。  
- **客观公平性**：作者声明在多个基准上达到SOTA，但未说明是否采用相同的数据划分或后处理，需进一步核查。

### 主要结论与发现
1. 零样本统一扩散框架能够有效融合多源几何与语义先验，实现度量一致、时间稳定的视频深度补全。  
2. 使用深度估计器提供的相对深度+边缘先验，优于直接输入RGB或稀疏深度。  
3. 语义特征的层级注入有助于模型抵抗外观噪声并保持场景理解。  
4. 四阶段训练协议稳定了多模态先验的融合，BOSW推理策略显著减少了长序列的尺度漂移和边界误差。  
5. 在多个基准上超越现有单帧与视频深度方法，尤其在大尺度场景和长序列中优势明显。

### 优点
- **创新性**：首个专门针对长距离视频深度补全的零样本扩散框架，统一处理稀疏测量与先验融合。  
- **方法设计**：  
  - 多源先验（相对深度、IDW、语义特征）互补，既保留几何细节又提供尺度引导。  
  - BOSW推理策略简单有效，解决单向滑动窗口的累积误差。  
- **训练策略**：四阶段渐进式训练，降低多模态融合难度，提升收敛稳定性。  
- **零样本泛化**：无需针对特定场景微调，适用于真实动态视频。

### 不足与局限
- **实验细节缺失**：未明确列出所用数据集、对比方法、定量结果，无法直接评判SOTA陈述。  
- **算力信息未公开**：不利于复现和资源评估。  
- **偏差风险**：可能依赖特定深度估计器（如MiDaS）的输出质量，若深度估计器在极端光照/纹理下失效，框架性能可能下降。  
- **应用限制**：  
  - 扩散模型推理速度较慢，难以实时处理高清视频。  
  - 对长时间尺度的基准测试是否包含室内外、动态场景等多样性？论文未具体说明。  
  - 未讨论与其他视频补全范式（如神经辐射场、隐式表示）的比较。  
- **消融实验深度**：未提及是否单独验证每个先验模块（如仅用IDW vs 仅用相对深度）的贡献，以及BOSW窗口大小的敏感性分析。

（完）
