---
title: Relative Pose Estimation through Affine Corrections of Monocular Depth Priors
title_zh: 通过单目深度先验的仿射校正进行相对位姿估计
authors: "Yu, Yifan, Liu, Shaohui, Pautrat, Rémi, Pollefeys, Marc, Larsson, Viktor"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Yu_Relative_Pose_Estimation_through_Affine_Corrections_of_Monocular_Depth_Priors_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 5.0
evidence: 利用单目深度先验进行几何视觉任务
tldr: 单目深度模型预测的深度包含噪声与歧义，难以直接用于相对位姿估计。本文提出对单目深度先验进行仿射校正的三种求解器，将其与几何约束结合。实验显示该方法能有效提升相对位姿估计精度，为深度先验在几何视觉任务中的应用提供了思路。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 789, \"height\": 471, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1646, \"height\": 572, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 304, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 859, \"height\": 334, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 785, \"height\": 340, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 838, \"height\": 235, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 875, \"height\": 538, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 880, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 521, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 866, \"height\": 518, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 877, \"height\": 208, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 887, \"height\": 500, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yu-relative-pose-estimation-through-affine-corrections-of-monocular-depth-priors-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 763, \"height\": 318, \"label\": \"Table\"}]"
motivation: 单目深度预测虽可提供跨视图对齐约束，但其噪声和歧义阻碍了在相对位姿估计任务中的直接应用。
method: 提出三种基于仿射校正的单目深度先验求解器，将深度先验与几何约束结合用于相对位姿估计。
result: 未提供具体实验摘要，但表明能在经典关键点方案基础上获得改进。
conclusion: 展示了如何将单目深度先验有效转化为几何视觉任务的可用约束，对深度估计应用有启发意义。
---

## Abstract
Monocular depth estimation (MDE) models have undergone significant advancements over recent years. Many MDE models aim to predict affine-invariant relative depth from monocular images, while recent developments in large-scale training and vision foundation models enable reasonable estimation of metric (absolute) depth. However, effectively leveraging these predictions for geometric vision tasks, in particular relative pose estimation, remains relatively under explored. While depths provide rich constraints for cross-view image alignment, the intrinsic noise and ambiguity from the monocular depth priors present practical challenges to improving upon classic keypoint-based solutions. In this paper, we develop three solvers for relative pose estimation that explicitly account for independent affine (scale and shift) ambiguities, covering both calibrated and uncalibrated conditions. We further propose a hybrid estimation pipeline that combines our proposed solvers with classic point-based solvers and epipolar constraints. We find that the affine correction modeling is beneficial to not only the relative depth priors but also, surprisingly, the "metric" ones. Results across multiple datasets demonstrate large improvements of our approach over classic keypoint-based baselines and PnP-based solutions, under both calibrated and uncalibrated setups. We also show that our method improves consistently with different feature matchers and MDE models, and can further benefit from very recent advances on both modules.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义

- **研究动机**：近年来单目深度估计（MDE）模型发展迅速，既能提供仿射不变的相对深度，也能在大型数据和基础模型支持下估计度量（绝对）深度。然而，如何将这些深度先验有效用于几何视觉任务（尤其是相对位姿估计）仍探索不足。
- **核心瓶颈**：深度预测虽然能为跨视图对齐提供丰富的三维约束，但其内在噪声和歧义（特别是每个视图深度的尺度与平移不确定性）使得直接使用难以超越经典基于关键点的方法。
- **整体含义**：本文提出显式建模两视图深度之间的**独立仿射校正（scale + shift）**，并据此设计三个相对位姿求解器，再加上与经典点法求解器结合的混合稳健估计流程，从而将单目深度先验转化为可用的几何约束，在多数据集上取得了显著改进。

---

### 2. 方法论

#### 核心思想
- 每个视图的预测深度 \(D_i\) 与实际深度 \(\hat{D}_i\) 之间不是简单的单尺度关系，而是仿射关系：\(\hat{D}_i = a_iD_i + b_i\)。
- 利用双目对应点反投影后的三维点距离在两帧中应相等的几何约束，联合求解相对位姿 \(R, t\) 以及深度仿射参数（尺度比 \(\omega\)、两个平移 \(\varepsilon_1, \varepsilon_2\)）。
- 在未标定情况下，同时求解焦距（共享焦距或各自焦距）。

#### 关键技术细节
- **问题建模**：将两视图深度校正参数化为：
  \[
  \hat{D}_1 = D_1 + \varepsilon_1,\quad \hat{D}_2 = \omega(D_2 + \varepsilon_2)
  \]
- **核心等式**：利用三维点对距离在两帧间不变的性质，构建关于 \(\omega, \varepsilon_1, \varepsilon_2\)（和焦距）的方程组，消去旋转和平移。

#### 三个求解器
- **标定 3 点求解器**：已知内参，用 3 对对应点即可构造最小问题；通过 \(\vartheta = \omega^2\) 降次后生成 Gröbner 基求解器，最多 4 个解，矩阵规模 12×12 + 4×4 特征值问题。
- **共享焦距 4 点求解器**：两相机焦距未知但相同；需 4 对对应点，从全部 6 个方程中取 4 个，用 \(\varpi = 1/f^2\) 代换，模板规模 36×36，得 8 个解。
- **双焦距 4 点求解器**：两相机焦距各自未知；用 4 对对应点中的 5 个方程；引入 \(\varpi_1 = 1/f_1^2\), \(\varpi_2 = 1/f_2^2\)，模板规模 40×40 + 4×4 特征值问题，仅有 4 个解。

#### 混合估计流程（Hybrid LO-MSAC）
- 将**深度感知求解器**（上述三个）与**经典点法求解器**（calibrated 用 5-pt，shared-focal 用 6-pt，two-focal 用 7-pt）随机选择用于假设生成。
- 对点法解出的位姿，用三角化三维点拟合深度仿射参数（最小二乘），使两种求解器输出具有可比性。
- 评分采用**混合 MSAC**：同时使用深度诱导的重投影误差 \(E_r\) 和经典 Sampson 误差 \(E_s\)，两者按阈值归一化后加权求和。
- 局部优化（LO）使用 Ceres 自动微分联合优化 \(R, t, \omega, \varepsilon_1, \varepsilon_2, (f)\)。

---

### 3. 实验设计

#### 数据集与场景
- **ScanNet-1500**（室内）— 标定情形、共享焦距未知情形。
- **MegaDepth-1500**（室外）— 标定情形、双焦距未知情形。
- **Stanford 2D-3D-S**（室内全景）— 自行采样 1064 对，用于不同焦距均未知的未标定情形。
- **ETH3D 室内场景**（7 scenes，1451 对）— 用于低共视性难度分析，拥有 GT 3D 点。

#### 对比方法
- 经典点法基线：PoseLib RANSAC 中的 5-pt / 6-pt / 7-pt。
- PnP 基线：类似 [41] 的做法。
- 深度先验求解器：Barath & Sweeney 的 2pt+D；Ding et al. 的 3p3d / 4p4d。
- 双视图稠密重建方法：DUSt3R、MASt3R（作为参考）。
- 不同特征匹配器：SuperPoint+LightGlue、RoMa、MASt3R matches。
- 不同 MDE 模型：Omnidata、Marigold、Depth-Anything v1/v2（metric）、MoGe。

#### 评测指标
- 相对旋转/平移中位误差 \(\phi_R, \phi_t\)。
- Pose error AUC 在 5°、10°、20°（或 2°、5°、10°、20°）阈值下。
- 未标定情形额外报告焦距相对误差中位数。

---

### 4. 资源与算力

- **训练**：本文方法不是训练式模型，而是基于几何求解的估计管线，因此没有训练算力需求。
- **推理运行时**（论文给出）：
  - CPU（Intel Core i7-10700K）上每对图像中位耗时：标定约 31ms，共享焦距约 65ms，双焦距约 129ms（使用 SP+LG 匹配、经过调参）。
  - GPU 上模块耗时：SP+LG 约 16ms/对；DAv2-met.(ViT-L) 约 0.16s/图；MoGe(ViT-L) 约 3ms/图。
- 未说明具体 GPU 型号、数量、训练时长（因非训练方法）。

---

### 5. 实验数量与充分性

- **实验组数**：较丰富，包括：
  - 标定情形：ScanNet + MegaDepth 两个数据集；
  - 共享焦距：ScanNet 一个；
  - 双焦距：Stanford 2D-3D-S + MegaDepth 两个；
  - 低共视性：ETH3D 分析；
  - 消融：Shift 建模消融（表 6）、合成 shift 实验（图 5）、混合估计组件消融（表 7）、不同匹配器/模型组合（正文及附录）。
- **充分性**：整体较充分，覆盖室内/室外、标定/未标定、稀疏/稠密/学习式匹配、相对/度量深度模型，并与强基线和近两年 SOTA 方法（DUSt3R、MASt3R）对比。
- **公平性**：设计较客观——对不同求解器均采用各自官方或标准实现；但部分对比（如 3p3d、4p4d）仅在特定深度模型下测试，可能存在不完全公平之处。
- **影响**：整体结论一致性较强，方法改进在不同组合下均稳定出现。

---

### 6. 主要结论与发现

- **显式仿射校正有效**：即使对于预测“度量深度”的 MDE 模型（如 Depth-Anything metric、MoGe），显式建模 scale 和 shift 依然能显著改善位姿估计，这一点超出了许多人的预期。
- **混合估计优于纯深度或纯点法**：将深度感知求解器与经典点法求解器结合，在评分和局部优化中也同时使用两类误差，可获得最佳性能。
- **适用范围广**：与不同特征匹配器（SP+LG、RoMa、MASt3R matches）和不同 MDE 模型（Marigold、DAv2-met、MoGe）组合均能提升。
- **低共视性场景增益更明显**：在困难图像对上（共视 3D 点少），深度先验的加入比点法基线的增益更突出。
- **和最新进展正交**：使用 MASt3R 的匹配和深度作为输入时，本文方法在其之上仍能提升，说明与稠密匹配/重建基础模型的进步可叠加。

---

### 7. 优点

- **切入角度新**：此前工作只建模单尺度（scale-only）或直接使用 PnP，本文明确指出现有 MDE 模型输出具有 “gauge ambiguity”，需要仿射校正。
- **求解器完整**：覆盖标定、共享焦距、双焦距三种几何情形，且提供了小规模矩阵求解器（最多 40×40），便于集成到 RANSAC 管线中。
- **混合框架实用**：将深度先验作为“补充数据”而非“唯一数据源”，利用点法基线的鲁棒性和深度约束的高精度，实际部署价值高。
- **兼容性强**：可直接套用 off-the-shelf matcher + MDE 模型，不需要任何训练或微调。
- **分析细致**：包含对 shift 大小的合成实验、GT 拟合 shift 分布分析，以及低共视性下的难度分级评测，解释力强。

---

### 8. 不足与局限

- **未覆盖所有未标定参数**：求解器仅考虑未知焦距，假设主点在图像中心、无畸变；对真实广角/鱼眼/带畸变相机的适用性有限。
- **依赖深度先验质量**：虽然混合估计可部分缓解，但当 MDE 在特定场景（如户外大尺度）严重失真时，提升幅度明显下降（MegaDepth 上仅 MoGe 改善明显）。
- **对匹配质量敏感**：深度感知求解器的最小采样集（3-4 点）对 outlier 更敏感，需要与点法求解器平衡；在匹配点较少时可能无法发挥优势。
- **计算开销附加**：涉及 Gröbner 基求解器和 Ceres 局部优化，每对图像耗时虽可接受，但相比纯经典 RANSAC 仍有增加。
- **评测范围局限**：
  - 未覆盖更多相对深度模型（如 MiDaS、Depth Pro）与更多匹配器（如 SuperGlue、LoFTR 等）；
  - 未在室外未标定（双未知焦距）场景与 DUSt3R/MASt3R 直接对比，而仅与 7-pt/PnP 对比；
  - 对 failure case 分析和参数敏感性（阈值 \(\varrho_r, \varrho_s\) 等）讨论不足。
- **理论部分**：最小性分析只对 calibrated 求解器，共享焦距/双焦距为 over-constrained，但未深入探讨解的分布、退化配置（如共线点、纯旋转）等。

---

（完）
