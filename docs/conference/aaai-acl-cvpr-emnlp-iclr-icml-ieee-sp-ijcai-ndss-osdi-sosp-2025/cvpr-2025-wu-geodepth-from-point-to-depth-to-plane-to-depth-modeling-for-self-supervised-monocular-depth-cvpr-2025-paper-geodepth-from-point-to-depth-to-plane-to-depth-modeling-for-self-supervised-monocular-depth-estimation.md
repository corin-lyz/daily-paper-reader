---
title: "GeoDepth: From Point-to-Depth to Plane-to-Depth Modeling for Self-Supervised Monocular Depth Estimation"
title_zh: GeoDepth：从点到平面建模的自监督单目深度估计
authors: "Wu, Haifeng, Gu, Shuhang, Duan, Lixin, Li, Wen"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wu_GeoDepth_From_Point-to-Depth_to_Plane-to-Depth_Modeling_for_Self-Supervised_Monocular_Depth_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 自监督单目深度估计，利用平面建模提升深度连续性与准确性。
tldr: 现有自监督单目深度估计将每个像素独立预测，导致同一区域内深度值突变和伪影。本文提出GeoDepth，将复杂3D场景表示为大小不同的平面集合，每个平面用平面法向等参数刻画，从而在自监督框架中生成更准确且连续的深度图。该方法缓解了深度跳变问题，并在自监督单目深度估计中展现出更优的几何一致性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1743, \"height\": 975, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1797, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 776, \"height\": 266, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 848, \"height\": 171, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1775, \"height\": 467, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 843, \"height\": 376, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1810, \"height\": 1203, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1793, \"height\": 871, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-geodepth-from-point-to-depth-to-plane-to-depth-modeling-for-self-supervised-monocular-depth-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 214, \"label\": \"Table\"}]"
motivation: 逐点深度估计容易产生区域内深度突变和伪影，缺乏几何连续性约束。
method: 提出以平面为单位建模3D场景，利用平面参数（如法向）生成连续深度图。
result: 在自监督单目深度估计中产生更精确、连续的深度图，减少深度跳变伪影。
conclusion: 从点级建模升级到平面级建模，可显著提升深度图质量与几何一致性。
---

## Abstract
Self-supervised monocular depth estimation has long been treated as a point-wise prediction problem, where the depth of each pixel is usually estimated independently. However, artifacts are often observed in the estimated depth map, e.g., depth values for points located in the same region may jump dramatically. To address this issue, we propose a novel self-supervised monocular depth estimation framework called GeoDepth, where we explore the intrinsic geometric representation in 3D scenes for producing accurate and continuous depth maps. In particular, we model the complex 3D scene as a collection of planes with varying sizes, where each plane is characterized by a unique set of parameters, namely planar normal (indicating plane orientation) and planar offset (defining the perpendicular distance from the camera center to the plane). Under this modeling, points in the same plane are enforced to share a unique representation and their depth variations related only to pixel coordinates, thus this geometric relationship can be exploited to regularize the depth variations of these points. To this end, we design a structured plane generation module that introduces spatio-temporal geometric cues and the plane uniqueness principle to recover the correct scene plane representation. In addition, we develop a depth discontinuity module to identify depth discontinuity regions and subsequently optimize them. Our experiments on the KITTI and NYUv2 datasets demonstrate that GeoDepth achieves state-of-the-art performance, with additional tests on Make3D and ScanNet validating its generalization capabilities.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **研究背景**：自监督单目深度估计旨在从单张图像预测逐像素深度图，仅依赖视频或立体图像进行训练，避免了昂贵的人工标注。
- **现有方法的核心缺陷**：主流方法采用**点级（point-to-depth）建模**，将每个像素视为独立点并单独估计深度。这种做法忽视了像素之间的几何关联，导致深度图在无纹理区域或同一平面区域内出现**深度跳变和伪影**（例如同一面墙上不同像素的深度值突变）。
- **研究目标**：论文提出一种**几何诱导**的建模新思路，挖掘 3D 场景的内在结构（平面表示），从而生成**更准确、连续、平滑**的深度图，并提升跨数据集的泛化能力。

## 2. 方法论：GeoDepth 框架

### 2.1 核心思想：平面到深度（Plane-to-Depth）建模

- 将复杂 3D 场景解构为**不同大小的平面集合**，每个平面由一组唯一参数刻画：
  - **平面法向（planar normal）**：指示平面的朝向；
  - **平面偏移（planar offset）**：定义相机中心到平面的垂直距离。
- 根据几何关系，同一平面内的 3D 点满足统一的点法式方程，其深度值由平面参数和像素坐标共同决定：

> d = o / (nᵀ K⁻¹ p̃)（式 1）

- 扩展到整幅图像，深度图可以由预测的平面法向 N 和平面偏移 O 经**几何映射**计算得到（式 2）。相比逐点回归，这种建模方式从原理上保证了**共面点的深度值连续变化**，消除了同一区域内深度的突兀跳变。

### 2.2 结构化平面生成模块（Structured Plane Generation, SPG）

SPG 模块从**空间**与**时间**两个维度恢复正确的平面表示，并引入平面唯一性约束：

1. **平面法向对齐**：
   - 用预测深度恢复点云，通过相邻点的叉积计算局部法向（depth-to-normal）；
   - **空间对齐**：用深度导出的法向约束网络预测的法向；
   - **时间对齐**：借助相邻帧之间的相机位姿，将邻帧法向变换到当前视角后做一致性约束。
2. **平面偏移对齐**：
   - 由深度和法向反推"伪偏移"，再做空间对齐；
   - 对邻帧偏移进行 warping 与插值，保证时域一致性。
3. **平面唯一性对齐**：
   - 对法向和偏移施加一阶梯度稀疏约束，迫使共面点共享统一的平面参数。

### 2.3 深度不连续感知模块（Depth Discontinuity Awareness, DDA）

- 深度不连续往往出现在**低纹理区域**——此处光度误差较小，难以提供有效监督；
- DDA 模块利用**自适应阈值**（光度误差的自适应均值）识别潜在的不连续区域，并仅在这些区域施加平面约束，避免对整体深度图的过度平滑；
- 该模块不增加额外参数，却显著提升了边界区域和低纹理区域的深度质量。

### 2.4 自监督训练

- 总损失 = 通用自监督损失（最小光度损失 + 边缘感知平滑损失）+ 结构化平面生成损失（包含法向对齐、偏移对齐和唯一性对齐，由 DDA 掩码加权）。

## 3. 实验设计

### 3.1 数据集与评估指标

| 场景 | 数据集 | 用途 |
|------|--------|------|
| 室外 | KITTI（Eigen split） | 训练与评测（39,180 训练序列） |
| 室外 | Make3D | 跨数据集泛化测试（134 张图） |
| 室内 | NYUv2 | 训练与评测（654 张测试图） |
| 室内 | ScanNet | 跨数据集泛化测试（533 张测试图） |

- 评估指标：Abs Rel、Sq Rel、RMSE、RMSE log、δ<1.25/δ<1.25²/δ<1.25³。

### 3.2 对比方法

- 室外对照：Monodepth2、HR-Depth、CADepth-Net、DIFFNet、Lite-Mono、Dynamo-Depth、Swin-Depth、SC-DepthV3 等十余种自监督方法（含基于立体、单目及混合训练模式的方法）；
- 室内对照：MovingIndoor、StructDepth、PLNet、P2Net、F2Depth、IFMNet 等专注室内场景的自监督方法。

## 4. 资源与算力

- 论文在**实现细节**中仅提及使用 **NVIDIA RTX 3090 GPU**（未说明具体数量和并行方式），以及 PyTorch 框架；
- 训练轮数：室外 20 epochs，室内 40 epochs；批量大小分别为 12 和 16；
- 论文**未详细说明训练时长、GPU 数量、能耗等具体算力开销**，这一点在原文中交代不足。

## 5. 实验数量与充分性

- **实验覆盖较全面**，共包含四大类实验：
  1. 室内/室外**主实验**（KITTI、NYUv2，与 SOTA 定量 + 定性对比）；
  2. **跨数据集泛化实验**（KITTI→Make3D、NYUv2→ScanNet）；
  3. **消融实验**（Baseline → +P2D → +P2D+SPG → +P2D+SPG+DDA 完整模型）；
  4. 定性可视化展示（图 5、图 6）。
- **公平性考量**：
  - 论文明确说明采用简单架构（仅借用 RA-Depth 的 DepthNet/PoseNet），刻意回避了数据增强、Transformer 骨干等 SOTA 技巧，以突出**建模本身**的贡献；
  - 消融实验与 Baseline 相比参数增量极小，说明性能提升主要来自建模策略而非模型容量。
- **不足**：消融实验仅在 KITTI 上完成（单数据集）；未提供与采用先进骨干网络的最新方法的直接对比；对 SPG 内部各子模块（空间/时间对齐、唯一性约束）的单独贡献未做逐项拆解。

## 6. 主要结论与发现

- 将自监督单目深度估计从“逐点独立回归”范式升级为**“平面结构化建模”**范式，能够从几何原理上消除深度不连续问题；
- GeoDepth 在 KITTI 与 NYUv2 上均取得 SOTA 结果：
  - KITTI：RMSE 4.381，Abs Rel 0.100（单目训练模式）——甚至优于部分使用立体/混合训练的方法；
  - NYUv2：RMSE 0.520，Abs Rel 0.134；
- 在两个跨数据集泛化实验（Make3D、ScanNet）中均超过所有既有方法，说明平面建模学到的几何表示具有**更强的可迁移性**；
- 消融实验证实：平面到深度建模（P2D）、结构化平面生成（SPG）和深度不连续感知（DDA）三个组件层层递进、均带来有效增益。

## 7. 优点与亮点

- **理论上有新意**：基于几何代数中的"平面唯一性表征原理"，将平面法向与偏移联合建模，打破了现有方法仅用表面法向的局限；
- **原理性的改进**：与其工程化地打补丁，GeoDepth 从几何基石出发重定义了深度估计的建模方式，立意清晰；
- **轻量高效**：核心模块（SPG、DDA）几乎不引入额外参数，性能提升主要来自建模策略本身，颇具说服力；
- **泛化能力突出**：室内外两个跨数据集测试均表现优异，验证了方法的鲁棒性；
- **结果可视化清晰**：定性对比图直观展示了本文方法在平面区域和边缘上的优势。

## 8. 不足与局限

- **低纹理场景依赖阈值**：DDA 模块依赖光度误差自适应阈值，在光照剧烈变化、反光表面或运动模糊等特殊场景下，阈值选取可能存在偏差风险；
- **动态物体问题未专门处理**：自监督深度估计普遍面临动态物体（如行人、车辆）的干扰，论文采用的最小光度损失虽有一定缓解作用，但并未提出针对性策略；
- **实验覆盖仍有提升空间**：消融仅在 KITTI 单数据集上完成；SPG 内部各组件（空间对齐、时间对齐、唯一性对齐）未做独立消融，各损失权重（γ、φ、ω）也未见敏感性分析；
- **对比范围受限**：作者主观省略了与 RA-Depth、MonoViT 等结合先进增强/骨干网络方法的直接对比，互补的结果仅在补充材料中提及，一定程度上削弱了与最新 SOTA 的可比性；
- **架构选型的双刃剑**：为突出核心建模贡献而刻意采用简单架构，虽有利于归因分析，但也意味着其与最强基线组合后的上限尚未在正文中充分展示。

（完）
