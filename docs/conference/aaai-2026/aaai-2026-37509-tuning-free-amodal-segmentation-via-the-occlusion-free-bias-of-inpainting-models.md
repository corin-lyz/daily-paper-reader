---
title: Tuning-Free Amodal Segmentation via the Occlusion-Free Bias of Inpainting Models
title_zh: 利用修复模型的无遮挡偏差实现免调参的共形分割
authors: "Jae Joong Lee, Bedrich Benes, Raymond A. Yeh"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37509/41471"
tags: ["query:seg"]
score: 7.0
evidence: 利用修复基础模型实现免调参的共形分割
tldr: 针对现有共形分割方法依赖监督数据和合成数据、泛化性差的问题，本文提出一种免调参方法，利用扩散修复基础模型的无遮挡偏差进行共形分割。该方法无需微调，直接利用预训练模型的偏置生成高质量共形掩膜。在多个基准上，该方法在零样本设置下优于现有方法，展示了利用基础模型隐式知识的优势。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 459, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 859, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1822, \"height\": 242, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 849, \"height\": 496, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 845, \"height\": 505, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1812, \"height\": 746, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 874, \"height\": 565, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37509/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 877, \"height\": 440, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37509/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 379, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37509/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 229, \"label\": \"Table\"}]"
motivation: 现有共形分割方法依赖标注数据，零样本方法泛化能力不足。
method: 利用扩散修复模型的无遮挡偏差，通过无需微调的推理实现共形分割。
result: 在零样本设置下达到领先性能，证明了基础模型隐式知识的有效性。
conclusion: 修复基础模型的无遮挡偏差可有效用于共形分割，无需额外训练。
---

## Abstract
Amodal segmentation is an image-based algorithm that aims to predict masks for both visible and occluded parts of objects. Existing methods typically rely on supervised learning with annotated amodal masks or synthetic data. The effectiveness of these methods relies heavily on the quality of the datasets. This dependence can unintentionally restrict their generalization capabilities due to insufficient diversity and size. Although existing zero-shot methods perform well on their reported datasets, their performance does not necessarily transfer to other datasets. We propose a tuning-free approach that re-purposes diffusion-based inpainting foundation models for amodal segmentation. Our approach is motivated by the “occlusion-free bias” of inpainting models, i.e., the inpainted objects tend to be complete and without occlusions. We reconstruct the occluded regions of an object via inpainting and then apply segmentation, all without additional training or fine-tuning. Experiments on five datasets, three previously unreported, demonstrate the generalizability of our approach. On average, our approach achieves 5.3% more accurate masks in mIoU compared to the publicly available state-of-the-art, pix2gestalt.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

**研究动机与背景**：  
共形分割（Amodal Segmentation）旨在预测物体完整掩膜（包括被遮挡部分），在自动驾驶、机器人规划等领域有重要应用。现有方法主要依赖监督学习，需要大量人工标注或合成数据来获得共形掩膜。然而，人工标注困难且不一致，合成数据存在域差异，导致模型泛化能力受限。已有的零样本方法（如 pix2gestalt）在其报告数据集上表现良好，但迁移到新数据集时性能显著下降。

**本文贡献**：  
提出一种**免调参（tuning-free）** 的共形分割方法，利用预训练的扩散修复基础模型（如 Stable Diffusion、Flux）的“无遮挡偏差”（occlusion-free bias）——即修复模型倾向于生成完整的物体而非被遮挡的物体。该方法无需任何共形数据训练或微调，实现零样本共形分割，并在多个数据集上展现出优于现有监督方法的泛化能力。

---

## 2. 方法论

**核心思想**：  
利用扩散修复模型在给定合理修复区域时更倾向于生成完整物体的特性。通过设计合适的条件图像和修复区域，引导模型在遮挡区域“生成”被遮挡的部分，再对修复后图像进行常规分割（使用 SAM），得到共形掩膜。

**关键技术细节**：

- **泄漏条件（Leakage Conditioning）**：  
  针对无文本提示的情况，提出“软修复”方法，在标准扩散修复采样中引入泄漏项，使模型不严格保留非修复区域，允许掩膜外像素发生变化，从而支持掩膜外推。更新公式（7）：  
  \(\hat{x}_t = s \cdot (M \odot \tilde{x}_t + (1-M) \odot x_t) + (1-s) \cdot x_t\)  
  其中 \(s=0.3\) 控制泄漏强度。

- **条件图像设计**：  
  背景部分：从物体可见像素的颜色直方图中采样，并施加高斯模糊，避免边缘对比度突变；  
  物体部分：对可见像素添加部分高斯噪声（\(x_{\text{obj}} = s \cdot \epsilon + (1-s) \cdot I\)），保持物体特征。

- **修复区域设计**：  
  基于可见掩膜 \(V\) 提取轮廓，计算所有轮廓的最小凸多边形作为修复区域 \(M\)，使扩散模型可以在包含可能遮挡区域的空间内生成。

- **前后处理**：  
  使用预训练的分割模型 SAM 从修复后图像中提取共形掩膜，支持多扩散模型后端（SD1.5, SD2, SDXL, Flux）。

**算法流程**：  
输入图像 \(I\) + 可见掩膜 \(V\) → 构造条件图像 \(x\)（对象部分加噪声，背景采样填充） → 构造修复区域 \(M\)（凸包） → 使用扩散模型进行软修复（迭代去噪，结合泄漏） → 使用 SAM 提取掩膜作为共形预测。

---

## 3. 实验设计

**数据集**：共5个，涵盖真实世界和合成场景：
- COCO-A（Zhu et al., 2017a）：手动标注共形掩膜，13k 实例。
- BSDS-A（Zhu et al., 2017b）：手动标注，200 张图像。
- KINS（Qi et al., 2019）：基于KITTI的手动标注，7k 图像。
- FishBowl（Tangemann et al., 2021）：合成鱼缸场景，1k 视频。
- SAILVOS（Hu et al., 2019）：基于GTA-V的合成数据集，26k 图像、507k 物体。

**基准方法**：
- **pix2gestalt**（Ozguroglu et al., 2024）：监督学习的 SOTA（公开代码）。
- **Amodal Wild**（Zhan et al., 2024）：两阶段方法。
- **Inpaint-SDXL**：基线（代码未公开）。
- **SAM / SAM2**：强模态分割基线。
- **Xu et al. (2024)** 的免调参方法：受限于83类，无法在所有数据集上比较。

**对比设置**：遵循 pix2gestalt 的零样本评估协议，采用平均交并比（mIoU）作为指标，并按遮挡率（<50%）报告子集结果。

---

## 4. 资源与算力

- **硬件**：NVIDIA RTX 4090（24GB VRAM）。
- **量化**：Flux 模型使用 8-bit 量化以降低显存。
- **推理效率**（与 pix2gestalt 对比）：
  - SD2：推理时间 0.3 秒（快 19 倍），内存效率高 4.1 倍。
  - SDXL：虽然比 SD2 慢，但依然比 pix2gestalt 更高效（约 4.8 倍推理加速，1.4 倍显存效率）。
- **训练时间**：本方法无需训练，故未报告训练时长。

---

## 5. 实验数量与充分性

- **定量实验**：在全部5个数据集上报告 mIoU，并在3个新数据集（KINS、FishBowl、SAILVOS）上额外分析，共5组对比实验。
- **消融实验**（表2）：在COCO-A上分别移除泄漏条件、背景设计、修复区域设计，验证各组件贡献（MioU下降37.6%、5.6%、6.2%）。
- **稳定性分析**（图4）：对比 pix2gestalt 在已知与未知数据集上的性能差距（26.9% vs. 8.5%）。
- **定性比较**（图6、7、8）：展示多模型结果及掩膜外推能力。
- **计算效率对比**：推理时间和显存消耗。
- **公平性**：严格遵循现有零样本设置，与公开 SOTA 对比；但缺少与 Amodal Wild 的直接定量比较（代码未完全公开），且 Inpaint-SDXL 基线无法复现。

**评价**：实验设计较为全面，覆盖多种数据分布、多种扩散模型、多角度消融，结论可信。但缺少对极端遮挡或小物体的深入分析，且未与最新工作（如视频共形分割）对比。

---

## 6. 主要结论与发现

- 提出的免调参方法在全部5个数据集上平均 mIoU 为 71.0%，超过监督 SOTA pix2gestalt（65.7%），提升 5.3%。
- 在三个新数据集（KINS、FishBowl、SAILVOS）上平均提升 12.1%（21.2%、1.9%、14.3%），表明更强的泛化能力。
- 多个扩散模型后端（SD1.5、SD2、SDXL、Flux）均有效，其中 SDXL 表现最佳。
- 泄漏条件、背景设计和修复区域设计均为关键组件；缺少任一个均导致显著性能下降。
- 本方法在推理速度和显存消耗上均优于 pix2gestalt。

---

## 7. 优点

- **免训练零样本**：无需任何共形标注数据或微调，直接利用大规模预训练基础模型的隐式知识。
- **无类别限制**：不预设物体类别，可处理开放世界中的任意物体。
- **设计巧妙**：泄漏条件使修复区域可外推，背景颜色分布匹配避免伪影，凸包区域自然覆盖可能遮挡区域。
- **计算效率高**：比监督方法更快、更省显存，适合实际部署。
- **泛化性强**：在多种数据分布（真实/合成、不同遮挡率）上表现稳定，优于依赖特定合成数据集的监督方法。

---

## 8. 不足与局限

- **在原始报告数据集上仍略弱于 pix2gestalt**：在 COCO-A 和 BSDS-A 上，pix2gestalt 的 mIoU 更高（分别为 82.9% vs. 82.7%，80.8% vs. 75.6%），表明在分布匹配的数据上监督方法仍有优势。
- **依赖可见掩膜和 SAM**：方法输入需要物体可见掩膜（可由 SAM 等提供），且最终分割依赖 SAM，可能受限于 SAM 的开集分割能力。
- **修复不确定性**：扩散模型生成的修复内容可能不准确，尤其在高度遮挡或复杂场景下，导致掩膜预测错误（例如作者在图6中展示了 pix2gestalt 的幻觉，但本方法也可能产生类似问题）。
- **未做时间一致性**：仅处理单帧图像，未考虑视频中遮挡一致性的时序建模。
- **未与最新方法全面对比**：缺少与 AmodalWild、SAM-based amodal 方法的定量比较（代码未放出），且未在更多挑战数据集（如 MP3D-Amodal）上验证。
- **消融实验仅在单一数据集（COCO-A）上进行**：组件重要性可能在不同数据集上有变化，结论的泛化性有待更多验证。

---

（完）
