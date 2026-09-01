---
title: "DepthSplat: Connecting Gaussian Splatting and Depth"
title_zh: DepthSplat：连接高斯泼溅与深度估计
authors: "Xu, Haofei, Peng, Songyou, Wang, Fangjinhua, Blum, Hermann, Barath, Daniel, Geiger, Andreas, Pollefeys, Marc"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Xu_DepthSplat_Connecting_Gaussian_Splatting_and_Depth_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 6.0
evidence: 利用预训练单目深度特征提升多视深度估计，并研究深度与高斯泼溅的协同，与单目深度、深度基础模型相关。
tldr: 当前高斯泼溅与单目深度估计通常分离研究，缺乏协同。本文提出DepthSplat，利用预训练单目深度特征构造鲁棒的多视深度模型，从而获得高质量的前馈3D高斯泼溅重建；反过来，高斯泼溅重建损失可作为无监督预训练目标，在大规模多视数据上学习更强的深度模型。在ScanNet等基准上取得新高度，展示了深度估计与三维重建的相互促进。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1790, \"height\": 512, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1626, \"height\": 514, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1804, \"height\": 797, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1815, \"height\": 545, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 870, \"height\": 428, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 859, \"height\": 514, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 866, \"height\": 329, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 443, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 607, \"height\": 266, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 863, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 858, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 782, \"height\": 446, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 861, \"height\": 330, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-depthsplat-connecting-gaussian-splatting-and-depth-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 860, \"height\": 205, \"label\": \"Table\"}]"
motivation: 高斯泼溅与深度估计被孤立研究，缺乏交互，无法互相提升。
method: 引入预训练单目深度特征构造多视深度模型，并将高斯泼溅重建作为深度模型的无监督预训练目标。
result: 在ScanNet、RealEst等基准上取得目前最好效果，验证了跨任务迁移的正向作用。
conclusion: 深度估计与高斯泼溅可以相互增强，为统一几何与重建提供新思路。
---

## Abstract
Gaussian splatting and single-view depth estimation are typically studied in isolation. In this paper, we present DepthSplat to connect Gaussian splatting and depth estimation and study their interactions. More specifically, we first contribute a robust multi-view depth model by leveraging pre-trained monocular depth features, leading to high-quality feed-forward 3D Gaussian splatting reconstructions. We also show that Gaussian splatting can serve as an unsupervised pre-training objective for learning powerful depth models from large-scale multi-view posed datasets. We validate the synergy between Gaussian splatting and depth estimation through extensive ablation and cross-task transfer experiments. Our DepthSplat achieves state-of-the-art performance on ScanNet, RealEstate10K and DL3DV datasets in terms of both depth estimation and novel view synthesis, demonstrating the mutual benefits of connecting both tasks. In addition, DepthSplat enables feed-forward reconstruction from 12 input views (512x960 resolutions) in 0.6 seconds.

---

## 论文详细总结（自动生成）

# DepthSplat：连接高斯泼溅与深度估计 — 详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景问题**：**高斯泼溅（3D Gaussian Splatting, 3DGS）** 与**单目深度估计**通常被当作两个独立的任务分别研究，两者之间缺乏协同与交互。
  - 前馈式稀疏视角 3DGS 方法（如 MVSplat）依赖基于特征匹配的多视图深度估计来定位 3D 高斯中心，在**遮挡、无纹理区域、反射表面**等难以匹配的场景中性能严重退化。
  - 单目深度估计模型虽然在多样化的真实数据上表现鲁棒，但预测的深度**缺乏跨视图一致的尺度**，难以直接用于 3D 重建。
- **核心想法**：将两者连接起来，利用各自的互补优势实现**双向增强**：
  - 用预训练单目深度特征增强多视图深度估计的鲁棒性，从而提升 3DGS 重建质量；
  - 反过来，将**可微的 3DGS 渲染损失作为无监督预训练目标**，在大规模多视图数据集上预训练深度模型，无需真实深度标注。
- **整体意义**：这项工作首次系统地探索了深度估计与高斯泼溅之间的跨任务协同，为统一几何感知与三维重建提供了新的研究范式。

## 2. 方法论：核心思想、关键技术细节与算法流程

### 2.1 整体架构（双分支设计）

DepthSplat 由两个并行分支构成，通过简单的**拼接（concatenation）** 实现特征融合：

1. **多视图分支（Multi-View Branch）**：建模跨视图特征匹配信息，保证深度预测的多视图一致性。
2. **单目分支（Monocular Branch）**：提取预训练单目深度模型的鲁棒先验特征，处理难以匹配的区域。

### 2.2 多视图特征匹配

- 使用**权重共享的 ResNet** 提取各视图的降采样特征（降采样因子 $s$ 可灵活设置，如 4 或 8）。
- 采用**多视图 Swin Transformer**（6 层自注意力 + 交叉注意力）在视图间交换信息，获得多视图感知特征 $\{F_i\}_{i=1}^N$。
  - 当输入视图数 $N > 2$ 时，每个参考视图只与**最近的 2 个邻居视图**进行交叉注意力，保证计算可扩展性。
- 通过**平面扫描立体匹配（plane-sweep stereo）** 构建代价体（cost volume）：
  - 在近远平面间均匀采样 $D$ 个深度候选 $\{d_m\}_{m=1}^D$；
  - 将邻居视图特征按候选深度投影到参考视图，计算**点积相关性**；
  - 叠加所有相关性得到代价体 $C_i \in \mathbb{R}^{\frac{H}{s} \times \frac{W}{s} \times D}$。
  - 多视图时同样只对最近 2 个视图计算相关性并取平均。

### 2.3 单目深度特征提取

- 使用预训练的 **Depth Anything V2** 模型（ViT backbone，patch size 为 14）提取单目特征。
- 通过**双线性插值**将单目特征从 1/14 分辨率对齐到代价体的空间分辨率，得到 $F^i_{\text{mono}}$。

### 2.4 特征融合与深度回归

- 将单目特征与代价体在通道维度上**直接拼接**，输入 **2D U-Net** 进行深度回归。
- 对输出张量按深度维度做 **softmax** 归一化，再对深度候选进行**加权平均**得到最终深度图。
- 采用**分层匹配（hierarchical matching）** 策略：
  - 粗阶段在低分辨率（如 1/8）上构建代价体，获得粗略深度；
  - 细阶段在 2× 高分辨率（如 1/4）特征图上，在粗深度邻域内搜索构建更小范围的代价体，提升精度与效率。
  - 最终用 **DPT head** 上采样到全分辨率。

### 2.5 高斯参数预测

- 将预测的逐像素深度图通过相机参数**反投影到 3D**，作为 3D 高斯的中心位置 $\mu_j$。
- 使用额外的 **DPT head**，以拼接的图像、深度和特征信息为输入，预测每个高斯的**不透明度 $\alpha_j$、协方差 $\Sigma_j$ 和颜色 $c_j$**。
- 最终通过 3DGS 的可微渲染操作合成新视角。

### 2.6 训练损失

1. **深度估计任务**（有监督）：
   $$L_{\text{depth}} = \alpha \cdot |D_{\text{pred}} - D_{\text{gt}}| + \beta \cdot (|\partial_x D_{\text{pred}} - \partial_x D_{\text{gt}}| + |\partial_y D_{\text{pred}} - \partial_y D_{\text{gt}}|)$$
   其中 $\alpha = \beta = 20$，使用**逆深度**进行计算。

2. **视图合成任务**（自监督）：
   $$L_{\text{gs}} = \sum_{m=1}^{M} \left[ \text{MSE}(I^m_{\text{render}}, I^m_{\text{gt}}) + \lambda \cdot \text{LPIPS}(I^m_{\text{render}}, I^m_{\text{gt}}) \right]$$
   其中 $\lambda = 0.05$，$M$ 为一次前向渲染的新视角数量。

### 2.7 无监督预训练流程

- 先在 RealEstate10K 等大规模多视图数据集上，仅用**渲染损失**（无需深度真值）预训练完整模型；
- 取出其中的深度模型部分，在带真实深度标注的数据集上**微调**，显著提升深度预测精度，尤其是在域外泛化场景。

## 3. 实验设计

### 3.1 使用的数据集

| 数据集 | 用途 | 特点 |
|---|---|---|
| **TartanAir** | 深度估计训练/测试（消融） | 大规模合成数据，室内外场景，含完美 GT 深度 |
| **VKITTI2** | 深度模型微调 | 合成自动驾驶数据 |
| **ScanNet** | 深度估计 zero-shot 泛化 / benchmark | 室内真实场景 |
| **KITTI** | 深度估计 zero-shot 泛化 | 室外自动驾驶 |
| **RealEstate10K** | 3DGS 训练/测试 | 约 67K 户外视频，256×256 低分辨率 + 512×960 高分辨率 |
| **DL3DV** | 3DGS 训练/测试 | 复杂真实场景，256×448 分辨率，挑战性高 |
| **ACID** | 跨数据集泛化测试 | 自然场景视频 |

### 3.2 对比方法

- **深度估计**：DeMoN、BA-Net、DeepV2D、NeuralRecon、DRO、UniMatch 等。
- **视图合成 / 3DGS**：pixelNeRF、GPNR、AttnRend、MuRF、pixelSplat、MVSplat、TranSplat 等。
- **消融对比**：不同单目 backbone（ViT-S/B/L）、不同单目特征来源（ConvNeXt-T、Midas、DINOv2、Depth Anything V1/V2）、不同融合策略（单分支、显式尺度对齐、注意力融合、拼接）等。

### 3.3 评估指标

- **深度**：Abs Rel（相对 L1 误差）、δ1（阈值准确率）、RMSE、RMSE log。
- **视图合成**：PSNR、SSIM、LPIPS。

## 4. 资源与算力

- **GPU 类型**：4× GH200（另有单卡 A100 用于推理时间测量）。
- **训练时长**：
  - RealEstate10K 256×256：小模型 1 天，大模型 2 天；
  - DL3DV 256×448 微调：100K 迭代；
  - ScanNet 深度训练：100K 迭代。
- **推理速度**：12 张 512×960 输入视图的前馈重建仅需 **0.6 秒**；两视图 256×256 推理时间 0.050–0.079 秒（取决于模型大小）。
- **说明**：论文明确给出了 GPU 数量和训练天数，但未披露总 GPU 小时数或显存占用等更细粒度的资源数据。

## 5. 实验数量与充分性

论文进行了**非常充分且成体系**的实验，大致可分为以下组别：

1. **模型变体实验**（表 1）：3 种单目 backbone × 2 种多视图尺度设置，共 6 个模型变体，在同一设置下对比。
2. **核心组件消融**（表 2）：移除单目特征、移除代价体、替代单目特征来源（4 种替换）、替代融合策略（3 种对比），实验覆盖全面。
3. **无监督预训练实验**（表 4）：预训练 × 微调的 4 种组合，在 3 个数据集上评估。
4. **标准 benchmark 对比**（表 5–7）：ScanNet 深度、RealEstate10K 双视图 3DGS、DL3DV 2/4/6 视图 3DGS。
5. **跨数据集泛化测试**（表 8）：在 DL3DV 和 ACID 上评估跨域性能。

**充分性评价**：

- ✅ **优点**：既有组件级消融，又有系统级 benchmark 对比；既有同域评估，又有跨域泛化测试；既有定量指标，又有可视化定性对比。范围广、逻辑严谨。
- ⚠️ **客观性注意点**：所有消融实验均在作者自己的训练配置下进行，未与同类方法（如 TranSplat 的融合策略）在同一代码框架下逐一对比，部分对比依赖论文报告的数字；但作者直接与多个开源 SOTA 方法（MVSplat、pixelSplat、TranSplat 等）公平比较，整体可信度较高。

## 6. 主要结论与发现

1. **单目特征和代价体特征互补**：两者缺一不可——去除单目特征，深度 Abs Rel 从 8.46 恶化至 12.25；去除代价体，PSNR 从 26.84 跌至 23.24。
2. **更大/更强的单目模型带来持续增益**：从 ViT-S 到 ViT-L、从 1-scale 到 2-scale 分层匹配，深度和渲染指标均一致提升。
3. **3DGS 渲染损失可作为有效的无监督预训练目标**：预训练后的深度模型在域内和域外数据集上均优于随机初始化训练，在挑战性数据集（TartanAir、KITTI）上增益尤其明显。
4. **跨任务协同互惠**：更好的深度网络架构和初始化 → 更好的 3DGS 重建；3DGS 无监督预训练 → 更好的深度估计。两者形成正向循环。
5. **显著 SOTA 性能**：在 ScanNet 双视图深度估计上，Abs Rel 达 3.8（超过 UniMatch 的 5.9）；在 RealEstate10K 双视图 3DGS 上 PSNR 达 27.47（超过 MVSplat 的 26.39）；在 DL3DV 各视图数量设置下全面超越 MVSplat。
6. **效率优势**：与 LGM、GRM、GS-LRM 等依赖大规模算力与数据的方法不同，只需 4 张 GPU 训练 2 天即可达到出色效果；且随输入视图增多，推理时间增长缓慢（得益于局部最近邻匹配策略）。

## 7. 优点

- **架构简洁高效**：通过简单的特征拼接实现单目与多视图特征的融合，避免了复杂的自适应融合模块，设计优雅且效果好。
- **双向协同创新**：不仅是"深度帮助 3DGS"，还开创性地将 3DGS 渲染损失用于无监督深度预训练，形成闭环。
- **计算效率高**：分层匹配 + 局部最近邻视图选择使模型可扩展到大量输入视图，无需全局成对匹配。
- **泛化能力强**：在跨数据集（RealEstate10K → DL3DV/ACID）与 zero-shot（合成 → 真实）场景下均表现优异。
- **工程可复现性好**：代码和训练脚本已开源，模型尺寸分级（Small/Base/Large）便于不同算力条件下的使用。

## 8. 不足与局限

- **依赖相机位姿**：模型需要输入视图的相机内外参，在极端稀疏或无位姿场景中难以直接应用（作者已指明这一点，并提出 pose-free 作为未来方向）。
- **高斯数量随视图数线性增长**：像素对齐的高斯表示在大量输入视图时会产生海量高斯，影响存储与渲染效率。
- **无监督预训练收益不均**：预训练带来的增益在部分数据集（如 ScanNet）上相对有限，仅在复杂数据集上提升显著。
- **单目特征提取的额外开销**：引入大型预训练 ViT 模型（如 ViT-L）会增加推理时间和参数量，实际部署时面临精度与速度的权衡。
- **实验范围局限**：主要聚焦于室内/室外场景的 RGB 图像，未涉及动态场景、水下/低光照等极端环境；对单目特征的依赖也意味着模型性能一定程度上受限于所选的预训练模型质量。
- **消融对比存在一定局限**：与既有方法（如 TranSplat）的融合策略对比，未在完全相同的训练条件下进行公平复现比较；深度学习论文常见的"横向对比受限"问题依然存在。

（完）
