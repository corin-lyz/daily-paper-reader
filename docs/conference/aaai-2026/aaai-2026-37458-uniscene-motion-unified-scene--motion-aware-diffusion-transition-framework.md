---
title: "UniScene-MoTion: Unified Scene & Motion-aware Diffusion Transition Framework"
title_zh: "UniScene-MoTion: 统一的场景与运动感知扩散过渡框架"
authors: "Rui Jiang, Chongmian Wang, Xinghe Fu, Yehao Lu, Teng Li, Xi Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37458/41420"
tags: ["query:mono-depth"]
score: 6.0
evidence: 利用单张图像深度预测生成视频过渡
tldr: 本文提出UniScene-MoTion框架，利用单图像深度预测将相机运动对齐到公制尺度几何，实现物理一致的视频过渡。通过双向条件控制模块和渐进训练策略，该方法无需精确相机输入即可生成连贯的过渡效果。在视频过渡生成任务上，该框架在时序一致性和真实感方面显著优于现有方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37458/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1651, \"height\": 952, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37458/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1662, \"height\": 824, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37458/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1663, \"height\": 655, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37458/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1800, \"height\": 662, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37458/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 863, \"height\": 379, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37458/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1830, \"height\": 575, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37458/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1846, \"height\": 535, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37458/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 854, \"height\": 225, \"label\": \"Table\"}]"
motivation: 现有视频过渡方法缺乏物理场景结构，难以保证时序连贯性。
method: 利用单图像深度预测和扩散模型，结合双向条件控制和渐进训练，实现尺度感知的视频过渡生成。
result: 实验表明生成的视频过渡在时序一致性和物理合理性上优于传统方法。
conclusion: 深度感知的3D推理是提升视频过渡质量的有效途径。
---

## Abstract
Video transitions are critical for ensuring temporal coherence in edited media, yet existing methods often rely on handcrafted effects or relative-scale trajectories that fail to capture the physical structure of real-world scenes. In this work, we introduce a scale-aware video transition framework that explicitly incorporates depth-aware 3D reasoning into a diffusion-based generation pipeline. Built upon a powerful I2V foundation, our method leverages single-image depth prediction to align camera motion with metric-scale geometry, enabling physically consistent transitions. To reduce reliance on precise camera inputs, we propose a bidirectional conditional control module and a progressive training strategy with conditional dropout, enhancing generalization to loosely specified or missing camera trajectories. Extensive experiments demonstrate that our approach achieves state-of-the-art performance, delivering realistic, geometrically coherent transitions across diverse scenes and applications with minimal input guidance.

---

## 论文详细总结（自动生成）

# UniScene-MoTion: 统一的场景与运动感知扩散过渡框架 —— 中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：视频过渡是视频编辑中保证时序连贯性和叙事流畅性的关键。传统方法（如硬切、溶解、划变）以及早期基于深度学习的插值方法，往往无法捕捉真实场景的物理几何结构和运动动力学，导致生成的过渡存在视觉不连续、空间错位和感知突兀。特别是在涉及相机运动时，缺乏对场景深度、物体3D位置和动态运动的准确理解，难以生成物理真实的过渡效果。
- **背景**：现有方法主要分为两类：文本驱动的视频生成（控制不够精细，难以表达精确的相机参数）和相机轨迹驱动的方法（依赖相对尺度轨迹，缺乏公制尺度感知，且精确轨迹难以获取）。主流模型（如I2VGen、EasyAnimate、AnimateAnything等）在尺度感知上存在缺陷，导致物体大小变化异常、前后景运动混淆。
- **整体含义**：作者提出一种尺度感知的视频过渡框架，通过引入深度信息提供公制尺度先验，使相机运动与真实场景几何对齐，生成物理一致的过渡。同时，设计双向条件控制模块和渐进训练策略，降低对精确相机输入的需求，提升实用性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 将公制尺度的深度信息（经尺度对齐）作为显式先验注入扩散生成管道，使模型能理解真实世界几何，生成与物理条件一致的相机运动和场景变换。

### 关键技术细节

#### (a) 公制尺度数据对齐
- 输入视频序列，对每帧使用单目深度估计器（如Metric3D）预测公制尺度深度图，并转换为视差图（depth disparity）。
- 利用SfM（如COLMAP）获得相对尺度的相机轨迹和视差。
- 通过全局优化求解尺度因子γ，最小化公制视差与相对视差之间的差异（排除极端值，保留高置信度像素）。公式：
  \[
  \gamma^* = \frac{\sum_i D_i^{abs} \cdot D_i^{rel}}{\sum_i (D_i^{rel})^2}
  \]
- 将γ乘以相对外参中的平移向量t，得到校准后的公制相机位姿矩阵 \(E = [R, \gamma^* \cdot t]\)。

#### (b) 先验引导的双向控制器网络
- 基于ControlNetXS和DiT架构，设计轻量级控制器网络。
- 关键设计：在控制路径中解耦文本特征与控制信号，控制层不直接输入文本嵌入；同时构建双向信息流：控制层输入特征与DiT解码路径的隐藏状态相互融合，通过Zero Up/Down Proj模块实现零初始化映射。
- 方程：\(F'_{control} = G(F_{control}, H, T_{timestep})\)，其中G为控制层。

#### (c) 渐进训练与动态丢弃（Progressive Training with Dynamic Dropout）
- 三阶段渐进式训练：
  - **阶段一**：低分辨率（81×224×448），仅使用起始帧、相机轨迹和文本提示，并额外引入视差视频作为辅助监督（Ldisp损失），强化3D运动推理。
  - **阶段二**：加入结束帧，提供更强的时间约束，学习内容保持的插值。
  - **阶段三**：高分辨率（81×768×1360），对相机轨迹进行随机条件丢弃（概率p_i），提高对缺失控制的鲁棒性。
- 总训练损失为流匹配损失（Flow Matching Loss）加上视差监督损失。

## 3. 实验设计

### 数据集
- **RealEstate10K**：500个视频片段，涵盖室内外场景、多种相机运动。
- **Pexels**：50个高质量视频片段，涵盖自然风景、室内外、肖像、烹饪、艺术风格等，包含多种运动模式。

### Benchmark与评估指标
- **指标**：LPIPS（帧级感知相似度）、FID（帧级图像质量）、FVD（视频整体质量）、VBench（多维度评估）、FVMD（强调运动一致性的FVD变体）。
- **对比方法**：SEINE、DynamiCrafter（两种分辨率）、TRF、GI、FCVG、VideoX-Fun（1.3B和14B版本）、FLF2V等。

### 对比设置
- 分别在帧间隔为23和79两种条件下进行定量比较（短间隔和长间隔）。
- 定量结果见Table 1，定性比较见图4。

## 4. 资源与算力

- 文中明确说明：基于CogVideoX1.5-5B I2V作为基础模型，整个框架在8张NVIDIA A100（80GB）GPU上训练70k次迭代。
- 优化器：AdamW，学习率1e-4，β1=0.9，β2=0.999。
- 每个训练分辨率阶段使用最大可能的batch size。

## 5. 实验数量与充分性

- **主要定量实验**：在RealEstate10K和Pexels两个数据集上，分别针对两种帧间隔（14和79）进行了指标对比，涵盖所有主要baseline，共报告了多个指标（LPIPS, FID, FVMD, FVD）。
- **定性对比**：在多个挑战场景下展示中间帧结果（图4），并补充附录中的完整视频帧。
- **消融实验**：两个关键组件——双向交互注入（Bidirection）和渐进训练（包括视差监督）——在VBench指标上的对比（Table 3）。
- **应用展示**：展示了相机轨迹引导过渡、不同视频序列间的无缝过渡等应用示例（图5）。
- **充分性评估**：实验覆盖了常见baseline，且包含短间隔和长间隔挑战；消融实验验证了核心设计。但缺少对每个超参数（如丢弃概率、不同深度估计器）的详细消融，且仅在两个数据集上评估，多样性可能有限。总体而言，实验设计较为完整，结果客观。

## 6. 主要结论与发现

- 本文提出的UniScene-MoTion在多数据和多指标上达到最先进性能，尤其在长间隔过渡任务上表现突出。
- 引入公制尺度深度先验显著提升了视频过渡的几何一致性和物理合理性。
- 双向交互控制模块和渐进训练策略有效降低了对精确相机输入的依赖，提高了模型的鲁棒性和泛化能力。
- 消融实验表明，移去双向交互或渐进训练均会导致性能下降，验证了各组件的重要性。

## 7. 优点

- **创新性**：首次将公制尺度深度显式引入视频过渡扩散框架，利用单张图像深度预测实现物理一致的相机运动，思路新颖。
- **方法论设计合理**：尺度对齐方法简洁有效，双向控制器网络实现轻量级控制，渐进训练策略兼顾分辨率提升与条件不完整鲁棒性。
- **工程实用性强**：通过条件丢弃机制，在仅有粗糙甚至无轨迹输入时仍能生成合理过渡，符合实际应用需求。
- **实验充分**：在两大类数据集、多种baseline、不同帧间隔下验证，并提供了消融分析，说服力强。

## 8. 不足与局限

- **实验覆盖**：仅在RealEstate10K（室内外场景）和Pexels（通用视频）上测试，缺乏对其他复杂场景（如剧烈运动、低纹理区域、动态光照）的评估。数据集规模中等（500+50），泛化性证据有限。
- **基准对比**：部分baseline（如FLF2V）仅在定性部分展示，未出现在定量表格中；且未与最新的端到端视频生成模型（如Sora、HunyuanVideo）直接对比（可能因模型不可控或无法用于过渡任务）。
- **指标局限性**：作者在文中指出，所有自动指标（LPIPS、FID、FVD等）“not capable of precisely evaluating temporal stability”，建议观看视频结果。这意味着实验结论可能不完全依赖于数值指标，定性结果具有主观性。
- **消融实验强度**：只做了两组消融，未对深度对齐方式、丢弃概率、辅助视差损失权重等超参数进行系统性分析。
- **计算成本**：使用8×A100-80GB训练70k迭代，虽未给出具体时间，但算力要求较高，可能限制小型团队复现。
- **依赖深度估计质量**：方法性能受限于单目深度估计器（如Metric3D）的准确度。在深度估计失效的场景（如透明物体、反射面、极端视角）可能产生误差传播。

（完）
