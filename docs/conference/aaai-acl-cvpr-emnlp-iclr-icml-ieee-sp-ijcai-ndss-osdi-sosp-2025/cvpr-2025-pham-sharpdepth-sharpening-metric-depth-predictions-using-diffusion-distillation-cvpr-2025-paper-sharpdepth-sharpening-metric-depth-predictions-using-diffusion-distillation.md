---
title: "SharpDepth: Sharpening Metric Depth Predictions Using Diffusion Distillation"
title_zh: SharpDepth：利用扩散蒸馏锐化度量深度预测
authors: "Pham, Duc-Hai, Do, Tung, Nguyen, Phong, Hua, Binh-Son, Nguyen, Khoi, Nguyen, Rang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Pham_SharpDepth_Sharpening_Metric_Depth_Predictions_Using_Diffusion_Distillation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 利用扩散蒸馏提升单目度量深度图的边界锐度
tldr: 判别式深度估计方法具有度量精度但边界平滑，生成式方法边界锐利但仅为相对深度。本文提出SharpDepth，通过扩散蒸馏将两者优势结合，实现单目度量深度估计的锐利边界保持。该方法在保持度量精度的同时显著提升细节与边缘质量，为高质量深度估计提供了新思路。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1773, \"height\": 808, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 786, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 784, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1574, \"height\": 662, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1585, \"height\": 1177, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 344, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 867, \"height\": 459, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1783, \"height\": 606, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1789, \"height\": 614, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pham-sharpdepth-sharpening-metric-depth-predictions-using-diffusion-distillation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 880, \"height\": 535, \"label\": \"Table\"}]"
motivation: 判别式方法度量准确但深度图过度平滑，生成式方法边界清晰却缺乏度量精度。
method: 通过扩散蒸馏将生成式边界锐度注入判别式度量深度模型，联合训练保留细节与绝对尺度。
result: 在度量准确度和边界清晰度两方面均取得显著改善，优于单一类型方法。
conclusion: 扩散蒸馏可有效融合两类深度估计方法的优势，获得锐利且度量的深度图。
---

## Abstract
We propose SharpDepth, a novel approach to monocular metric depth estimation that combines the metric accuracy of discriminative depth estimation methods (e.g., Metric3D, UniDepth) with the fine-grained boundary sharpness typically achieved by generative methods (e.g., Marigold, Lotus). Traditional discriminative models trained on real-world data with sparse ground-truth depth can accurately predict metric depth but often produce over-smoothed or low-detail depth maps. Generative models, in contrast, are trained on synthetic data with dense ground truth, generating depth maps with sharp boundaries yet only providing relative depth with low accuracy. Our approach bridges these limitations by integrating metric accuracy with detailed boundary preservation, resulting in depth predictions that are both metrically precise and visually sharp. Our extensive zero-shot evaluations on standard depth estimation benchmarks confirm SharpDepth's effectiveness, showing its ability to achieve both high depth accuracy and detailed representation, making it well-suited for applications requiring high-quality depth perception across diverse, real-world environments.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：单目度量深度估计中，判别式方法与生成式方法存在明显的能力割裂：
  - 判别式方法（如 Metric3D、UniDepth）在真实数据上用稀疏 GT 训练，**度量精度高（绝对尺度准确）**，但输出深度图往往**过度平滑、缺乏细粒度边界细节**。
  - 生成式方法（如 Marigold、Lotus）在合成数据上用稠密 GT 训练，能生成**边界锐利、细节丰富**的深度图，但仅提供**相对（仿射不变）深度，且精度有限**。
- **研究目标**：提出一种方法，将两类方法的优势融合——既能保持判别式模型的**度量精度（绝对深度尺度）**，又能获得生成式模型的**高保真边界锐度**。
- **核心思想**：通过**扩散蒸馏（Diffusion Distillation）**，利用预训练的扩散深度模型作为教师，对一个基于扩散架构的深度锐化器进行训练，从而将生成式细节注入判别式度量深度估计的结果中。
- **方法论要点**：SharpDepth 不依赖任何 GT 深度数据，仅使用预训练模型（UniDepth 与 Lotus）进行“无真值微调”，直接在真实图像上训练。
- **关键技术细节**：
  1. **差异图（Difference Map）**：将 UniDepth 生成的度量深度与 Lotus 生成的仿射不变深度归一化对齐后，逐像素计算绝对差异，高差异区域视为需要锐化的不确定区域。
  2. **噪声感知门控（Noise-aware Gating）**：基于差异图对度量深度潜变量进行加权融合，在高差异区域注入更多高斯噪声，低差异区域保持相对干净，从而引导扩散模型聚焦于需细化的区域。
  3. **训练损失**：
     - **Score Distillation Sampling (SDS) 损失**：将预训练扩散深度模型作为教师，蒸馏其输出分布，增强深度细节；
     - **噪声感知重建损失（Noise-aware Reconstruction Loss）**：以差异图加权，约束输出接近初始度量深度，防止尺度漂移。
  - 总目标为：`L_total = λ_SDS * L_SDS + λ_recons * L_recons`。
- **训练策略**：训练过程中使用 EMA 模型替代固定的教师模型，逐步迭代更新差异图，使模型持续适应自身提升后的输出。

### 3. 实验设计：数据集 / 场景、benchmark、对比方法
- **训练数据**：使用约 90,000 张真实图像（约相当于判别式模型训练数据的 1/150），涵盖 Pandaset、Waymo、ArgoVerse2、ARKit、Taskonomy、ScanNetv2 等室内外多相机类型数据集。
- **测试数据**：
  - 真实数据集：KITTI、NYUv2、ETH3D、Diode、Booster、nuScenes、iBims；
  - 合成数据集：Sintel、UnrealStereo4K、Spring。
- **评估指标**：
  - 深度精度：δ1、绝对相对误差（A.Rel）、RMSE；
  - 细节/锐度：Depth Boundary Error（DBE）及合成数据集上的 Pseudo-DBE（PDBE），包括边缘准确率与完成率。
- **对比方法**：
  - 度量深度方法：UniDepth、Metric3Dv2、ZoeDepth、DepthAnythingV2-Metric、PatchRefiner、UniDepth-aligned Lotus；
  - 相对深度方法（需 GT 对齐）：Marigold、Lotus、BetterDepth。

### 4. 资源与算力
- 文中明确说明：在 **2 块 A100 40GB GPU** 上训练约 **1.5 天**，共 **13,000 次迭代**，batch size 为 1，梯度累积 16 步，优化器使用 Adam（学习率 1e-6）。

### 5. 实验数量与充分性
- **实验数量**：
  - 主实验包含 6 个真实数据集和 4 个合成数据集的零样本泛化评估；
  - 深度精度与边界锐度分别有独立的评测表（Tab. 1、Tab. 2）；
  - 消融实验覆盖噪声感知门控的输入形式、SDS 损失与重建损失的贡献、教师模型选择、在线/离线模型更新等 8 个设置（Tab. 3）；
  - 同时提供了定性结果（深度图对比、点云重建效果）和教师模型可视化比较。
- **充分性与客观性**：
  - 实验覆盖室内、室外、真实、合成，以及高精度和锐度两个维度，整体较全面；
  - 对比方法包含判别式与生成式主流模型，并额外设置了 naive 对齐基线（UniDepth-aligned Lotus）以验证差异图引导的必要性；
  - 消融实验设计较严谨，各模块贡献清晰；
  - 需要指出的是，零样本评测中部分生成式方法需 GT 对齐才能评估，与 SharpDepth 等不需对齐的方法放在同一表中，可能存在一定不公平性（作者也对此做了标注和区分）。

### 6. 论文的主要结论与发现
- SharpDepth 能在保持与判别式方法相当的度量精度的同时，显著提升深度图的边界锐度与细节表现，在深度精度和 DBE 锐度两个维度上取得较好的平衡（Pareto frontier）。
- 无 GT 训练策略有效，仅使用预训练模型即可在真实数据上训练，且训练数据量远小于主流方法。
- 通过差异图引导的噪声感知门控机制，以及 SDS + 重建损失组合，是成功融合两类方法优势的关键。
- 定性实验表明，SharpDepth 能恢复细长结构（如栅栏、电线杆）和复杂物体轮廓，点云重建质量优于 UniDepth。

### 7. 优点
- **创新性**：首次将扩散蒸馏用于“度量深度锐化”，有效弥合判别式与生成式方法的鸿沟。
- **免 GT 训练**：无需任何真实深度标注，仅依赖预训练模型间的差异图即可训练，降低数据依赖。
- **模块设计精细**：噪声感知门控、差异化加噪、EMA 迭代更新、重建损失加权等设计均有明确动机和消融支撑。
- **实验充分**：多数据集、多指标、多基线、多消融，验证了方法的泛化性和合理性。
- **实际应用价值高**：在需要高质量深度感知的场景（如三维重建、AR/VR）中，其生成锐利且度量准确的深度图有明确实用意义。

### 8. 不足与局限
- **精度略低于当前最先进的判别式模型**：例如在 KITTI 上，SharpDepth 的 δ1 略低于 UniDepth（97.3 vs 97.9），度量精度并非最优；
- **对初始度量深度模型的依赖**：需要依赖 UniDepth（或类似方法）的初始预测，若初始模型较差，锐化效果可能受限；
- **推理开销大**：基于扩散模型，相比纯判别式前馈模型，生成式锐化的推理速度可能显著更慢，文中未报告推理耗时；
- **训练数据仍具一定规模**：虽然仅为判别式模型的 1/100~1/150，但仍需 90,000 张图像，且依赖多个大规模数据集；
- **DBE 指标在合成数据集上的适配性问题**：PDBE 需用 Canny 边缘检测近似，与真实深度边缘存在偏差；
- **差异图的计算与对齐依赖经验性归一化**：仿射深度对齐到度量深度采用最小二乘，可能对极端场景鲁棒性有限。

（完）
