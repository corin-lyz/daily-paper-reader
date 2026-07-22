---
title: "TSPE-GS: Probabilistic Depth Extraction for Semi-Transparent Surface Reconstruction via 3D Gaussian Splatting"
title_zh: TSPE-GS：基于3D高斯溅射的半透明表面概率深度提取
authors: "Zhiyuan Xu, Min Nan, Yuhang Guo, Tong Wei"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38130/42092"
tags: ["query:mono-depth"]
score: 4.0
evidence: 半透明表面重建，深度提取
tldr: 针对3D高斯溅射在重建半透明表面时面临的深度歧义问题（一个像素对应多个表面），TSPE-GS提出概率深度提取方法，为每个像素建模多模态深度分布。实验在合成和真实场景中显著提升了半透明物体的几何重建质量，扩展了3D高斯溅射的应用范围。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 839, \"height\": 706, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 827, \"height\": 620, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 831, \"height\": 642, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1745, \"height\": 717, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 821, \"height\": 524, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38130/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 866, \"height\": 521, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38130/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 803, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38130/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 832, \"height\": 497, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38130/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38130/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 465, \"height\": 391, \"label\": \"Table\"}]"
motivation: 3D高斯溅射在重建半透明表面时因单深度假设失败，需要处理深度歧义。
method: 提出概率深度提取方法，建模每个像素的多模态深度分布，从高斯溅射中解析多个表面。
result: 在半透明物体重建上，TSPE-GS取得了更精确的几何结构，优于现有3D高斯方法。
conclusion: 概率深度建模能有效解决半透明表面的重建难题，拓展了3D高斯溅射的实用性。
---

## Abstract
3D Gaussian Splatting-based geometry reconstruction is regarded as an excellent paradigm due to its favorable trade-off between speed and reconstruction quality. However, such 3D Gaussian-based reconstruction pipelines often face challenges when reconstructing semi-transparent surfaces, hindering their broader application in real-world scenes. The primary reason is the assumption in mainstream methods that each pixel corresponds to one specific depth—an assumption that fails under semi-transparent conditions where multiple surfaces are visible, leading to depth ambiguity and ineffective recovery of geometric structures. To address these challenges, we propose TSPE-GS (Transparent Surface Probabilistic Extraction for Gaussian Splatting), a novel probabilistic depth extraction approach that uniformly samples transmittance to model the multi-modal distribution of opacity and depth per pixel, replacing the previous single-peak distribution that caused depth confusion across surfaces. By progressively fusing truncated signed distance functions, TSPE-GS separately reconstructs distinct external and internal surfaces in a unified framework. Our method can be easily generalized to other Gaussian-based reconstruction pipelines, effectively extracting semi-transparent surfaces without requiring additional training overhead. Extensive experiments on both public and self-collected semi-transparent datasets, as well as opaque object datasets, demonstrate that TSPE-GS significantly enhances reconstruction accuracy for semi-transparent surfaces while maintaining reconstruction quality in opaque scenes.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于3D高斯溅射（3D Gaussian Splatting, 3DGS）的几何重建管线在处理半透明表面时面临深度歧义问题。主流方法假设每个像素只有一个可见表面，因此采用期望深度或中值深度作为单一深度值。但在半透明场景中，一个像素可能对应多个表面（如玻璃容器及其内部的物体），单一深度值会落在真实表面之间，导致重建失败。
- **研究动机**：半透明表面重建在机器人操作（如抓取透明玻璃杯、从塑料袋中取物）、增强现实、自动驾驶等领域有重要应用。现有3DGS方法虽然能表示半透明场景，但深度提取方法未能充分利用其表达能力。作者希望充分挖掘3DGS中的视见度-深度映射信息，实现多层表面的同时精确重建。
- **整体含义**：提出一种概率深度提取方法TSPE-GS，通过对每个像素的深度分布建模为多模态分布（而非单峰），并利用多峰值检测和渐进式TSDF融合，实现对半透明表面和内部被遮挡物体的高质量重建。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 将3DGS渲染过程中的深度与透射率关系建模为累积分布函数（CDF），进而导出深度概率密度函数（PDF）。在无半透明表面时，PDF呈单峰高斯形状；当半透明表面存在时，PDF呈现多峰（高斯混合）形状，每个峰值对应一个可见表面。
- **统一表面深度假设**：任意像素的深度分布可建模为（单模态或多模态）分布，其局部极大值直接对应于物理表面的深度。
- 基于此假设，通过检测PDF的峰值提取各层表面深度，再通过渐进式TSDF融合分别重建外层和内层表面。

### 关键技术细节
1. **概率深度建模**：
   - 定义深度d与透射率T的关系：\( T_d = \prod_{d'=0}^{d} (1 - \alpha_{d'}) \)，其中\(\alpha_d\)为深度d处的不透明度。
   - 导出概率密度函数：\( p(d) = -\frac{dT_d}{dd} \approx \alpha_d T_d \)。
   - 在非透明场景中，p(d)为单峰；在透明场景中为多峰。
   - 实现时，均匀采样N个透射率阈值\(T' \in \{k/N | k=1,2,...,N\}\)，对每个像素计算对应阈值下的最大深度\(D'_p = \max_{g: T_{p,g} < T'} d_{p,g}\)，然后对所有像素的深度样本做核密度估计得到分布。

2. **峰值检测**：
   - 为了降低计算开销，作者论证了：对于同一表面类型的像素，其深度分布经过平均后仍保持多峰特性。因此，可以在全体像素的聚合分布上进行峰值检测，而不需要逐像素检测。
   - 对聚合分布采用局部极大值检测，设定峰值分数阈值以平衡捕捉真实表面与去除虚假峰值。

3. **渐进式TSDF融合**：
   - 从多个峰值深度图（多层深度）中，先重建最外层表面，将其对应体素“冻结”（即不再参与后续更新）；再重建内层表面，避免外层不准确深度干扰内层重建。
   - 伪代码见Algorithm 1（深度提取）和附录E（渐进融合）。

### 算法流程（文字说明）
```
1. 对每个像素，从排序后的高斯原语中获取透射率T_g和深度d_g。
2. 对每个预定义的透射率阈值T'，找到第一个满足T_g < T'的高斯原语，将其深度作为D'_p。
3. 对所有像素所有阈值的深度样本进行核密度估计，得到聚合深度PDF。
4. 检测PDF的局部峰值，并筛选出高于阈值的峰值作为各层表面深度。
5. 对每个峰值对应的深度图，逐一进行TSDF融合：先融合外层，冻结体素，再融合内层。
6. 从TSDF体积中提取三角网格。
```

## 3. 实验设计

### 数据集
- **半透明数据集**：
  - **αSurf**（公开数据集，Wu et al. 2025）：包含多个半透明或薄物体场景。
  - **Bottleship**（自建数据集）：包含被半透明表面（如塑料袋、玻璃）遮挡的物体。
- **不透明数据集**：
  - **DTU**（Aanæs et al. 2016）：经典多视角立体数据集。
  - **BlendedMVS**（Yao et al. 2020）：大场景多视角数据集。

### Benchmark / 对比方法
- **NeRF-based**: Mip-NeRF 360, NeuS, HFS, Neuralangelo, NeRRF.
- **3DGS-based**: 2DGS, PGSR, GOF, GSF, RaDeGS, TSGS等。
- 所有方法使用相同的TSDF融合后端（统一评价口径）。

### 实验数量与内容
- **半透明场景定量**：表1（αSurf，8个场景，Chamfer距离），表2（Bottleship，8个场景）。
- **不透明场景定量**：表3（BlendedMVS，8个场景）。
- **消融与分析**：峰值阈值灵敏度分析（图6），可视化比较（图5）。
- 附加分析：在附录中讨论了不同方法兼容性（如2DGS不适用）。

## 4. 资源与算力

**文中未明确说明使用的GPU型号、数量或训练时长。** 仅在实现部分提到“严格遵循基线方法的超参数设置”，未提及具体算力资源。推测其计算开销主要来自额外的深度采样（1-2分钟）和核密度估计，相比3DGS本身训练时间可以忽略。但缺乏具体数字。

## 5. 实验数量与充分性

- **数量**：共3张主要定量表（αSurf 8场景、Bottleship 8场景、BlendedMVS 8场景）+ 多种方法的广泛对比（10+种基线）。
- **充分性**：
  - 覆盖半透明和不透明两种场景，验证了方法的泛化能力。
  - 在αSurf和Bottleship上均展示了相对于原始基线的显著提升（如RaDeGS + TSPE平均CD从0.97→0.82，表1）。
  - 超参数分析（图6）展示了峰值阈值的选择稳健性。
  - 但缺少对更多半透明类型（如薄雾、玻璃与液体混合）的测试，以及模拟噪声的鲁棒性实验。
- **客观性**：使用统一的TSDF后端，对比公平；代码已开源。

## 6. 论文的主要结论与发现

- 3DGS的深度分布（PDF）在不透明场景呈单峰，在半透明场景呈多峰，每个峰值对应一个可见表面。
- 现有期望深度和中值深度均无法准确表示多层表面，而峰值检测可以。
- TSPE-GS作为插件可无缝集成到多种3DGS管线中，显著提升半透明表面重建精度（如RaDeGS + TSPE在αSurf上平均CD降低15%以上），且不损害不透明场景性能。
- 渐进式TSDF融合有效避免内外层深度间的干扰。

## 7. 优点

- **创新性**：首次在3DGS框架中识别并解决半透明表面遮挡物体的深度歧义问题，提出基于PDF多模态建模的通用解决方案。
- **简洁高效**：无需额外训练，仅需在渲染后增加少量计算（1-2分钟完成采样），即可获得多层深度。
- **强扩展性**：可作为即插即用模块应用于多种3DGS方法（如RaDeGS、2DGS等），且不依赖特定网络结构。
- **统一假设**：提出的“统一表面深度假设”理论清晰，实验验证了其普适性。
- **实验全面**：在多个公共/自建数据集上对比了多种方法，包括半透明和不透明场景，结果可信。

## 8. 不足与局限

- **依赖底层3DGS的拟合质量**：如果基础3DGS方法无法准确表示场景几何（如2DGS在Bottleship上表现差），TSPE-GS效果也会受限。
- **超参数敏感性**：峰值检测阈值需要根据场景调整（文中给出通用范围，但未提供自动选择方法）。
- **计算开销未精确量化**：虽然提到额外时间1-2分钟，但未与基线训练/推理总时间对比，也未报告峰值检测的复杂度。
- **半透明场景覆盖有限**：仅测试了αSurf和Bottleship，缺乏更复杂半透明材质（如半透明液体、多层玻璃、浑浊介质）的验证。
- **未讨论极端情况**：当半透明表面和内部物体深度接近时，峰值可能重叠，方法可能失效。
- **未与更细粒度的多深度估计方法比较**：如光场相机、多焦点深度等（可能超出论文范畴）。

（完）
