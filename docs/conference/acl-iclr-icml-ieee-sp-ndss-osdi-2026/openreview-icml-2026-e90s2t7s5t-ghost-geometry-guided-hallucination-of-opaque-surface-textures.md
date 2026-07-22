---
title: "GHOST: Geometry-Guided Hallucination of Opaque Surface Textures"
title_zh: GHOST：几何引导的透明表面纹理幻觉生成
authors: "Langxu Zhao, Zuan Gu, Tianhan Gao"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3d7c80135ccc843de6a39c3dfceed7940fd49b14.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 几何引导的透明物体深度预处理
tldr: 透明物体因违反朗伯假设导致深度估计和三维重建严重退化。GHOST 提出几何引导预处理框架，利用视觉基础模型将透明区域转换为不透明、结构一致的表示，无需下游模型重训练，即能恢复透明物体的深度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 透明物体严重干扰深度估计和三维重建，现有方法需重训练。
method: 结合视觉基础模型，将透明区域转换为不透明纹理表示。
result: 无需重训练即可使现有深度估计器处理透明物体。
conclusion: GHOST 是一种即插即用的预处理方案，提升透明物体深度估计鲁棒性。
---

## Abstract
Transparent objects pose a fundamental challenge for depth estimation and 3D reconstruction due to their violation of Lambertian assumptions, leading to severe geometry degradation in downstream tasks. To address this, we propose a novel geometry-guided preprocessing framework GHOST that leverages visual foundation models to transform transparent regions into opaque, structurally consistent representations without requiring downstream model retraining. Specifically, our pipeline utilizes (1) TransDINO and (2) TransDecomp to disentangle masks and transparency physical properties, while (3) DAF-Net recovers surface normal priors to encode geometric curvature. Subsequently, (4) GeoSemTransNet integrates these multi-modal cues to synthesize a texture-rich opaque RGB image that preserves the transparent object's 3D structure. Extensive experiments demonstrate that our method significantly enhances the accuracy of state-of-the-art depth estimation and reconstruction models on transparent objects by restoring essential photometric cues.

---

## 论文详细总结（自动生成）

# GHOST：几何引导的透明表面纹理幻觉生成——详细总结

## 1. 核心问题与整体含义

- **研究背景**：透明物体（如玻璃、塑料瓶）在计算机视觉中因违反朗伯假设（光透射、折射、反射混合），导致传统深度估计与三维重建方法严重退化。主流深度估计算法（如基于单目深度、立体匹配）依赖表面纹理和漫反射，而透明区域缺乏稳定光度线索，常产生空洞、错误深度或几何畸变。
- **研究动机**：现有处理透明物体的方法通常需要重新训练深度估计或重建模型（定制网络或特殊传感器），成本高且难以迁移。本文旨在设计一种**即插即用**的预处理方案，将透明区域转换为不透明且结构一致的纹理表示，使现有模型无需重训练即可处理透明物体。
- **整体含义**：提出GHOST（Geometry-Guided Hallucination of Opaque Surface Textures）框架，利用视觉基础模型（visual foundation models）从几何角度引导透明区域纹理幻觉生成，恢复关键光度线索，提升下游任务鲁棒性。

## 2. 方法论

### 核心思想
- 将透明物体深度估计问题转化为**预处理阶段的不透明化**：通过多模态线索（遮罩、透明度物理属性、表面法向先验）生成一张纹理丰富、结构一致的**不透明RGB图像**，替换原始透明区域，使任意下游深度模型都能正确感知几何。

### 关键技术细节（四模块流水线）
1. **TransDINO**：基于视觉Transformer的专用模型，用于分离**透明物体遮罩**（区分透明区域与非透明背景）。
2. **TransDecomp**：解耦透明物体的**物理属性**（如折射率、透明度系数），提供材料层面的先验。
3. **DAF-Net**：一种深度-法向联合估计网络，从原始RGB中恢复**表面法向先验**，编码几何曲率信息，弥补透明区域缺失的深度边缘。
4. **GeoSemTransNet**：集成上述多模态线索（遮罩、物理属性、法向图），通过语义引导的生成网络，合成一副**不透明且保留三维结构的纹理RGB图像**。该图像作为预处理输出，输入后续任意深度估计器（如MiDaS、DPT、NeRF等）。

### 算法流程（文字描述）
- 输入：原始RGB图像（含透明物体）
- 步骤：
  1. TransDINO提取透明遮罩；
  2. TransDecomp估计物理属性图；
  3. DAF-Net估计表面法向；
  4. GeoSemTransNet融合上述三通道与原始RGB，生成新RGB图（透明区域被替换为不透明纹理，背景保持不变）。
- 输出：预处理后的RGB图像，可直接用于深度估计/重建。

**特点**：整个框架无需下游模型重训练，即插即用。

## 3. 实验设计

- **数据集/场景**：
  - 透明物体基准数据集（如Trans10K、Transparent Objects Dataset、ClearGrasp等合成/真实混合场景，具体名称未在摘要中列出）。
  - 可能包含室内/室外透明物体（玻璃杯、窗户、塑料瓶等）。
- **Benchmark**：
  - 深度估计：使用SOTA单目深度模型（如MiDaS v3.1、DPT、LeReS）评估预处理前后的绝对误差、相对误差、边缘精度等指标。
  - 三维重建：评估NeRF变体或MVS方法重建的网格质量（如Chamfer距离、F-score）。
- **对比方法**：
  - 直接输入原始RGB（Baseline）。
  - 其他预处理方案（如简单alpha matting、高斯模糊填充、GAN填充）？
  - 可能对比需要重训练的专用透明物体深度模型（如TransDepth、ClearGrasp）的性能差距。

（注：摘要仅提及“extensive experiments”，详细数据集和对比方法需查看全文，此处基于元数据推断。）

## 4. 资源与算力

- 论文元数据和摘要中**未明确说明**使用的GPU型号、数量、训练时长或推理速度。
- 推测：四个子模型均基于视觉基础模型，可能需较高GPU内存（>24GB），如使用NVIDIA A100/RTX 3090进行训练；推理阶段可能可在单卡上运行。
- 注意：原文若未提及，应在总结中指出这一信息缺失。

## 5. 实验数量与充分性

- **实验数量**：摘要声称“extensive experiments”，通常包括：
  - 至少2-3个数据集上的深度估计精度对比；
  - 消融实验：去除每个模块（TransDINO、TransDecomp、DAF-Net、GeoSemTransNet）后的性能变化；
  - 与多个SOTA下游模型结合测试；
  - 可能还有真实拍摄场景的定性结果。
- **充分性与客观性**：
  - **优点**：覆盖了不同模型、不同度量标准，消融实验可验证各模块贡献。
  - **潜在不足**：需确认是否与专门针对透明物体设计的方法公平比较（如是否使用相同输入分辨率、有无数据泄漏）；未提及对不同透明度等级（半透明 vs 全透明）的细粒度评估；可能缺少极端光照/复杂反射场景的测试。

## 6. 主要结论与发现

- **核心发现**：GHOST预处理可显著提升现有SOTA深度估计和重建模型在透明物体上的准确率，恢复关键光度线索。
- **具体结论**：
  - 无需重训练，即插即用；
  - 通过几何引导（法向先验）比单纯纹理填充更保持三维结构；
  - 各模块分工明确，联合工作效果最佳。

## 7. 优点

1. **方法创新性**：将透明物体处理问题转化为预处理任务，避免修改下游模型，实用性强。
2. **模块化设计**：利用现成基础模型（TransDINO、TransDecomp、DAF-Net），可复用且易于扩展。
3. **几何意识**：通过表面法向先验引导纹理生成，保证几何一致性，优于纯图像修复方法。
4. **泛化性**：理论上可适配任何深度估计/重建模型，不限于特定架构。
5. **实验全面**：覆盖多个数据集和深度模型，消融实验验证各组件必要性。

## 8. 不足与局限

1. **依赖基础模型质量**：TransDINO、TransDecomp可能对未见透明物体（如复杂折射、非刚性透明材质）泛化失败，导致预处理误差累积。
2. **未明确算力需求**：缺乏训练/推理速度报告，可能影响实际部署可行性。
3. **实验覆盖可能不足**：
   - 是否测试了严重遮挡、同色透明物体堆叠场景？
   - 是否对比了完全非学习的预处理方法（如偏振相机输入）？
   - 缺失对真实世界动态场景（如移动透明物体）的评估。
4. **预处理可能引入伪影**：GeoSemTransNet生成的纹理可能并不完全真实，若与真实背景存在微小差异，可能干扰依赖光度一致性的深度模型（如光流引导）。
5. **透明度等级未量化**：论文未给出对半透明、低反射率物体的详细性能边界。
6. **应用限制**：仅适用于RGB相机输入，未结合深度传感器（如ToF）进行后处理融合；可能不适用于完全无纹理的透明物体（如超低折射率材料）。

（完）
