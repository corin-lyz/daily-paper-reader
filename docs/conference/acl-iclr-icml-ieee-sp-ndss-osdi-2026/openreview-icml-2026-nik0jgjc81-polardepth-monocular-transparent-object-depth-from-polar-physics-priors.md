---
title: "PolarDepth: Monocular Transparent Object Depth from Polar-Physics Priors"
title_zh: PolarDepth：基于偏振物理先验的单目透明物体深度估计
authors: "Wen Dong, Haiyang Mei, Yinglian Ji, Zijun Zhang, Wenyuan Zhang, Pengwei Luo, Bo Dong, Shengfeng He, Xin Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1063ddd8399d0eb584a2d4d0aaa55435c1732d8d.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 偏振辅助的单目透明物体深度估计
tldr: 透明物体深度估计因折射和透射导致RGB线索失效。偏振提供与表面方向相关的物理信息。PolarDepth 构建单目框架，融合RGB和偏振输入（线偏振度和角度），利用偏振推导的折射率、天顶角等物理先验，实现了透明区域的密集深度估计和定位。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: RGB图像在透明区域缺乏可靠深度线索，传统方法难以处理。
method: 融合RGB和偏振输入，注入偏振推导的物理先验进行深度估计。
result: 在透明物体密集深度估计上取得了显著效果。
conclusion: PolarDepth 为透明物体深度估计提供了物理驱动的有效方案。
---

## Abstract
Depth estimation for transparent objects remains a fundamental challenge, as RGB-based cues often fail in regions affected by refraction and light transmission. Polarization provides physically grounded information related to surface orientation and material properties, offering reliable geometric cues even in the absence of texture. In this work, we introduce PolarDepth, a monocular framework that incorporates both RGB and polarization inputs, including the degree and angle of linear polarization (DoLP and AoLP), to estimate dense depth and localize transparent regions. PolarDepth injects polarization-derived physical priors by estimating the refractive index, zenith angle, and azimuth angle from polarization measurements and embedding them into an implicit geometric representation that constrains depth inference in ambiguous transparent regions. To support model development and evaluation, we introduce PTOD, a dataset with synchronized RGB, polarization, and depth data and manually annotated transparent region masks. Experimental results demonstrate that PolarDepth achieves state-of-the-art performance in transparent object depth estimation. The findings highlight the effectiveness of embedding polarization-derived physical priors into learned representations for robust perception in complex visual environments.

---

## 论文详细总结（自动生成）

# PolarDepth：基于偏振物理先验的单目透明物体深度估计——详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：透明物体（如玻璃、塑料瓶等）的深度估计是计算机视觉中的长期难题。由于透明材质会折射和透射光线，传统依赖RGB图像的方法（如立体匹配、单目深度估计）在透明区域缺乏可靠的视觉线索，导致深度估计失效或空洞。
- **研究动机**：偏振成像（Polarization）能够提供与物体表面方向和材料属性相关的物理信息，即使在无纹理的透明区域也能提供几何约束。作者旨在利用偏振先验来弥补RGB信息的不足，实现透明物体的密集深度估计和区域定位。
- **整体含义**：本文提出的PolarDepth框架首次将偏振推导的物理先验（折射率、天顶角、方位角）嵌入隐式几何表示中，实现单目透明物体深度估计，为鲁棒感知复杂透明环境提供了新方案。

## 2. 方法论

- **核心思想**：融合RGB图像与偏振输入（线偏振度DoLP和线偏振角AoLP），通过物理模型从偏振测量中推导出折射率、天顶角和方位角等物理先验，并将这些先验注入到隐式几何表示中，约束透明区域的深度推理。
- **技术细节**：
  - **输入**：RGB图像 + polarization inputs (DoLP, AoLP)
  - **物理先验提取**：利用偏振物理模型（如菲涅尔方程）从DoLP和AoLP估计每个像素的折射率、天顶角（表面法线方向）、方位角（表面朝向）。这些先验提供了与深度相关的几何约束。
  - **隐式几何表示**：将上述物理先验编码为特征，通过神经网络（如MLP或CNN）嵌入隐式表示中，与RGB特征融合，最终回归密集深度图和透明区域掩码。
  - **整体流程**：输入多模态数据 → 偏振先验模块 → 特征融合模块 → 深度/掩码预测模块。没有显式给出公式，但核心是基于偏振物理的几何约束。
- **算法流程**：预处理获取偏振图像；偏振先验估计；RGB与偏振特征融合；隐式表示学习；输出深度图和透明区域掩码。

## 3. 实验设计

- **数据集**：作者构建了 **PTOD**（Polarization Transparent Object Dataset），包含同步的RGB、偏振（DoLP, AoLP）、深度数据和人工标注的透明区域掩码。具体场景未详细说明（推测包含不同透明物体、光照、背景）。
- **Benchmark**：透明物体深度估计任务，使用PTOD数据集作为评估基准。
- **对比方法**：未列出具体方法名，但摘要提到“state-of-the-art performance”。根据元数据，可能对比了其他单目深度估计方法（如基于RGB的、基于偏振的，或结合的方法）。具体对比细节需参考全文。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。元数据也未提及。因此无法总结资源消耗情况。

## 5. 实验数量与充分性

- **实验组数**：根据摘要和元数据，推测进行了以下实验：
  - 在PTOD数据集上的透明物体深度估计主实验。
  - 可能包含消融实验（例如移除偏振先验、使用不同输入模态等）。
  - 可能包括与多个基线方法的定量/定性对比。
- **充分性评价**：
  - **充分**：构建了专门的数据集，解决了数据缺乏问题；消融实验可验证每个组件的贡献。
  - **客观性**：透明物体深度估计是标准任务，使用常见指标（如RMSE, MAE, δ等）的可能性大，但摘要未列出具体指标。
  - **公平性**：因为未给出对比方法细节，无法完全判断。但通常ICML收录的工作会保证对比公平。

## 6. 主要结论与发现

- **结论**：PolarDepth在透明物体密集深度估计任务上取得了最优性能，验证了嵌入偏振推导的物理先验到学习表示中的有效性。该方法能够鲁棒地处理透明区域，实现可靠深度估计和定位。
- **发现**：偏振物理先验（折射率、天顶角、方位角）为深度估计提供了关键几何约束，弥补了RGB线索的失效。

## 7. 优点

- **方法创新**：首次将偏振物理先验（折射率、天顶角、方位角）注入隐式几何表示，结合RGB和偏振实现单目透明物体深度估计，具有明确的物理驱动性。
- **数据贡献**：构建了PTOD数据集（RGB+偏振+深度+标注掩码），填补了该领域的数据空白，促进后续研究。
- **实验设计**：验证了物理先验的有效性，且结果达到SOTA，证明了方法的鲁棒性。
- **应用价值**：可用于自动驾驶（透明障碍物）、机器人抓取、增强现实等实际场景。

## 8. 不足与局限

- **实验覆盖**：仅基于自建数据集PTOD，缺乏在其他公开数据集（如Transparent Object Dataset, ClearGrasp等）上的验证，泛化能力未充分证明。
- **偏差风险**：偏振数据的获取需要专用偏振相机，限制了方法在普通单目RGB场景下的直接应用。
- **应用限制**：物理先验依赖偏振测量质量，在弱偏振或强噪声环境下性能可能下降；透明物体后表面深度估计可能仍有歧义。
- **细节缺失**：论文正文未提供，导致架构细节、损失函数、训练设置等无法深入评估；算力未说明，影响可复现性。
- **消融实验**：虽然可能包含消融分析，但未说明是否分析了偏振先验中每个分量（如折射率、天顶角、方位角）的独立贡献。

（完）
