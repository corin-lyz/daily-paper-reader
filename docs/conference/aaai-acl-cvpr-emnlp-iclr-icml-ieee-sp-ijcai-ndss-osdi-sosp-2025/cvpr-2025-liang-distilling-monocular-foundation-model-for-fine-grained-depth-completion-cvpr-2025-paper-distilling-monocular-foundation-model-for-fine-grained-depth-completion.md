---
title: Distilling Monocular Foundation Model for Fine-grained Depth Completion
title_zh: 蒸馏单目基础模型用于细粒度深度补全
authors: "Liang, Yingping, Hu, Yutao, Shao, Wenqi, Fu, Ying"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liang_Distilling_Monocular_Foundation_Model_for_Fine-grained_Depth_Completion_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 5.0
evidence: 利用单目基础模型作为教师网络指导深度补全
tldr: 深度补全稀疏激光雷达输入时缺少稠密监督，本文提出两阶段知识蒸馏框架，首先利用单目深度基础模型和网格重建生成多样化训练数据并模拟激光雷达扫描，再将精细几何知识蒸馏到深度补全网络，从而在无稠密标签情况下显著提升细粒度深度补全性能。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1721, \"height\": 376, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1546, \"height\": 561, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1635, \"height\": 458, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1806, \"height\": 640, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1792, \"height\": 813, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 779, \"height\": 709, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 852, \"height\": 326, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1423, \"height\": 889, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 851, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 854, \"height\": 436, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liang-distilling-monocular-foundation-model-for-fine-grained-depth-completion-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 203, \"label\": \"Table\"}]"
motivation: 稀疏深度传感器标签难以提供稠密监督，限制深度补全学习细节几何。
method: 利用单目深度基础模型生成稠密伪标签，并通过两阶段蒸馏传递几何知识。
result: 在公开数据集上实现更精细的深度补全效果，超越现有方法。
conclusion: 展示了单目基础模型可作为深度补全的有效教师。
---

## Abstract
Depth completion involves predicting dense depth maps from sparse LiDAR inputs, a critical task for applications such as autonomous driving and robotics. However, sparse depth annotations from sensors limit the availability of dense supervision, which is necessary for learning detailed geometric features. To overcome this limitation, we propose a two-stage knowledge distillation framework that leverages powerful monocular foundation models to provide dense supervision for depth completion. In the first stage, we introduce a pre-training strategy that generates diverse training data from natural images to distill geometric knowledge to depth completion. Specifically, we simulate LiDAR scans by utilizing monocular depth and mesh reconstruction, thereby creating training data without requiring ground-truth depth. Nonetheless, monocular depth estimation suffers from inherent scale ambiguity in real-world settings. To address this, in the second stage, we employ a scale- and shift-invariant loss (SSI Loss) to learn real-world scales when fine-tuning on real-world datasets. Our two-stage distillation framework enables depth completion models to harness the strengths of monocular foundation models. Experimental results show that models trained with our two-stage distillation framework achieve top-ranked performance on the KITTI benchmark, demonstrating improvements in both quantitative and qualitative metrics.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义

- **任务背景**：深度补全（Depth Completion）旨在从稀疏的 LiDAR 深度测量和 RGB 图像中预测稠密深度图，广泛应用于自动驾驶、机器人和增强现实等场景。
- **核心挑战**：真实场景中 LiDAR 提供的深度标注非常稀疏（如 KITTI 仅覆盖约 5% 像素），即便通过多帧融合也仅能覆盖约 20%，导致深度补全模型难以获得稠密监督，无法学习精细的几何细节，尤其在复杂户外场景中预测结果往往不完整、碎片化。
- **核心思路**：利用单目深度估计基础模型（如 Depth Anything V2）可以生成稠密但尺度模糊的深度图，本文提出一种**两阶段知识蒸馏框架**，将单目基础模型的几何知识迁移到深度补全网络，以弥补稠密监督的缺失。
- **整体意义**：该框架首次系统性地将单目基础模型作为深度补全的“教师”，在无需额外稠密真实标注的情况下显著提升深度补全的精细度和完整性，并在 KITTI 官方榜单上获得第一名。

### 2. 论文提出的方法论

#### 2.1 整体框架
- 采用两阶段训练策略：
  - **第一阶段（预训练）**：利用未标注的自然图像，通过单目深度估计和网格重建模拟 LiDAR 扫描，生成合成训练数据，从零预训练深度补全模型。
  - **第二阶段（微调）**：在带稀疏真实标注的数据集上微调，结合 L1 监督损失和尺度-平移不变损失（SSI Loss）实现稠密知识蒸馏，同时学习真实世界尺度。

#### 2.2 第一阶段：数据生成与预训练
- 使用预训练单目深度模型 `f_mo`（Depth Anything V2）对无标注图像 `I_un` 预测稠密深度 `D_syn`。
- 随机采样相机内参矩阵 `K`（焦距 `fx, fy` 和主点 `cx, cy`），将深度图转化为三维点云 `P`。
- 利用三维点云通过泊松表面重建等方法重建网格 `M`。
- 模拟 LiDAR 传感器，从原点向网格发射射线，记录交点距离，生成模拟稀疏深度图 `D_simu`。
- 深度补全模型 `fθ` 以 `(I_un, D_simu)` 为输入，预测稠密深度 `D_f`，使用 L1 损失与 `D_syn` 对齐进行预训练：
  ```
  L_pre = |D_f - D_syn|
  ```
- 该阶段使模型学习到多样的几何结构和相对深度关系，增强泛化能力。

#### 2.3 第二阶段：微调与尺度对齐
- 使用真实数据集中的稀疏深度标注 `D_sparse` 计算监督损失：
  ```
  L_sup = M * |D_f - D_sparse|
  ```
  其中 `M` 为有效掩码。
- 同时利用单目模型生成的稠密深度 `D_m` 进行蒸馏，但为解决单目深度固有的尺度和平移模糊，引入 SSI Loss：
  ```
  L_SSIL = min_{s,b} |D_f - (s * D_m + b)|
  ```
  其中 `s` 和 `b` 为最优尺度和平移参数，使预测与单目稠密深度在全局对齐。
- 额外引入多尺度梯度匹配正则项 `L_reg`，保留深度不连续处的锐利度，具体为计算 `R = D_f - D_m` 的梯度差分在多尺度下的均值。
- 最终总损失为三者之和。

### 3. 实验设计

#### 3.1 数据集与基准
- **预训练数据**：混合 5 个公开无标注数据集，约 36 万张图像：
  - COCO（118,287 张，室内外）
  - Google Landmarks（117,576 张，室外）
  - Nuscenes（93,475 张，室外）
  - Cityscapes（19,998 张，室外）
  - DAVIS（10,581 张，室内外）
- **微调与测试基准**：
  - **KITTI Depth Completion Benchmark**：标准户外自动驾驶深度补全基准，包含约 9.3 万训练样本，测试集为 1,000 张由官方服务器评估，主要指标 RMSE（排名依据），另含 MAE、iRMSE、iMAE。
  - **NYU Depth V2**：室内 RGB-D 数据集，使用 RMSE、REL、δ1.25 等指标。

#### 3.2 对比方法
- 与多种主流深度补全方法比较，包括 S2D、CSPN、DeepLiDAR、CSPN++、GuideNet、FCFR、ACMNet、NLSPN、PointDC、RigNet、DySPN、BEV@DC、CFormer、LRRU、TPVD、ImprovingDC、BP-Net 等。

#### 3.3 实验内容
1. **主 benchmark 比较**：KITTI 和 NYUv2 上的定量结果，并展示视觉对比和 3D 点云重建对比。
2. **消融实验**：
   - 去掉第一阶段预训练（w/o Pre-train）
   - 去掉第二阶段 SSI Loss（w/o SSI Loss）
3. **网络架构泛化性测试**：将本方法应用于 LRRU、CFormer、BP-Net 三种不同架构，验证通用性。
4. **零样本跨域测试**：在未见过的 ScanNet、DDAD、VOID1500 数据集上进行测试，评估泛化能力。
5. **应用验证**：将完成的稠密深度输入 Droid-SLAM，对比仅使用稀疏 LiDAR 点的重建质量。

#### 3.4 主要结果
- KITTI 排行：RMSE 678.12 mm，优于所有对比方法，排名第一。
- NYUv2：RMSE 0.085，δ1.25 99.7%，达到最佳或相当水平。
- 消融表明：去掉预训练 RMSE 上升 4.22 mm；去掉 SSI Loss 性能下降。
- 本方法应用于三种不同网络均能带来提升。
- 零样本测试中，在所有数据集上均优于基线，尤其 DDAD 上 RMSE 从 8.903 降至 7.766。

### 4. 资源与算力

- 文中明确提到训练环境为 **4 块 NVIDIA RTX A100 GPU**。
- 第一阶段预训练从零开始训练 **60 万迭代**（600K iterations）。
- 第二阶段微调 **30 万迭代**（300K iterations）。
- 采用 OneCycle 学习率策略，Batch Size 为 16，优化器 AdamW，权重衰减 0.05，梯度裁剪 0.1，EMA 衰减 0.9999。
- **未明确说明**总训练时长、具体天数或 GPU 小时数，也未提及单次实验的耗时。

### 5. 实验数量与充分性

- **实验数量**：较为丰富，包括 2 个标准 benchmark 的对比、3 组消融、3 种网络架构的适配测试、3 个跨域数据集零样本测试，以及 1 个 SLAM 应用实验。
- **充分性评价**：
  - 优点：核心组件均有消融验证；在官方 KITTI 排行榜上评估，结果客观可信；跨域测试增强了方法泛化性的证据；网络架构兼容性测试说明方法不依赖于特定模型。
  - 不足：消融实验只针对“有无预训练”和“有无 SSI Loss”两个维度，未对不同数据生成策略参数（如采样内参范围、网格重建方法、LiDAR 模拟密度）进行敏感性分析；对比方法中未包含最新 ICCV/ECCV 2024 之后所有 SOTA；预训练数据量虽然大，但未评估各数据集单独贡献；训练迭代次数等超参数也未做系统搜索。

### 6. 论文的主要结论与发现

- 提出了一种两阶段知识蒸馏框架，有效利用单目基础模型为深度补全提供稠密监督，缓解了稀疏真实标签导致的学习瓶颈。
- 第一阶段的数据生成策略可以在完全不需要真实深度标注的情况下，从大规模自然图像中学习几何先验。
- 第二阶段 SSI Loss 有效解决了单目深度尺度模糊问题，使模型在保持稠密细节的同时对齐真实世界尺度。
- 该方法在 KITTI 官方排行榜取得第一名，在 NYUv2 上取得最佳或接近最佳精度，并表现出良好的跨域零样本泛化能力和对多种网络架构的适应性。
- 应用在 SLAM 中可以显著改善重建质量，验证了方法在真实下游任务中的价值。

### 7. 优点

- **创新性**：首次将单目深度基础模型系统地引入深度补全训练，形成紧凑的两阶段蒸馏范式，思路简单而有效。
- **数据效率**：预训练阶段完全使用无标注图像，降低了对人工稠密标注的依赖，符合真实场景中标注稀缺的现状。
- **理论严谨性**：SSI Loss 的引入针对单目深度固有缺陷，从数学上允许预测与伪标签之间存在未知的尺度和平移变换，同时保留了相对深度结构。
- **实验全面**：从主基准、消融、网络适配、跨域泛化到下游应用均有覆盖，且 KITTI 结果来自官方评估，说服力强。
- **可复现性**：代码开源，便于后续研究者复现和改进。

### 8. 不足与局限

- **依赖教师模型质量**：单目基础模型的伪标签本身存在误差，尤其在半遮挡、低纹理或极度复杂场景下，可能将错误信息传递给深度补全网络，且论文未对教师模型选择进行消融（仅使用 Depth Anything V2）。
- **数据生成模拟与真实 LiDAR 存在偏差**：泊松重建和射线模拟难以完全复现真实 LiDAR 的噪声、遮挡和多回波特性，可能影响模型在真实传感器上的迁移能力。
- **SSI Loss 仅对齐全局尺度和平移**：无法解决单目深度中的局部几何畸变或区域不一致问题，对逐像素对齐的细粒度监督仍有局限。
- **实验范围有限**：仅在 KITTI 和 NYUv2 两个基准上验证，虽然做了跨域零样本，但真实场景的多样性（不同LiDAR型号、天气、光照）未充分覆盖；同时缺少对模型推理速度和计算开销的分析。
- **算力需求较高**：预训练 600K + 微调 300K 迭代，在 4 台 A100 上运行，对一般研究团队可能成本较高，论文未提供更轻量的训练方案。
- **未深入分析失败案例**：缺少对复杂场景下失败模式的定性定量讨论，不利于进一步诊断方法边界。

（完）
