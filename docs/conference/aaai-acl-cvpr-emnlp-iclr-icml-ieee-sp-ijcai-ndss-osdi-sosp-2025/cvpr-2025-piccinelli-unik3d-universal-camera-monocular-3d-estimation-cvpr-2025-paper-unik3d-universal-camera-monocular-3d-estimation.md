---
title: "UniK3D: Universal Camera Monocular 3D Estimation"
title_zh: UniK3D：任意相机的通用单目3D估计
authors: "Piccinelli, Luigi, Sakaridis, Christos, Segu, Mattia, Yang, Yung-Hsu, Li, Siyuan, Abbeloos, Wim, Van Gool, Luc"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Piccinelli_UniK3D_Universal_Camera_Monocular_3D_Estimation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 面向任意相机模型的单目3D估计，可输出准确度量重建，契合度量深度与零样本泛化需求。
tldr: 现有单目3D估计通常假设针孔相机或已校正图像，限制了在鱼眼、全景等真实场景中的应用。本文提出UniK3D，首次实现任意相机模型的单目3D估计，引入球形3D表示更好地解耦相机与场景几何，并能在无约束相机模型下重建准确的度量3D结构。该工作显著提升了真实世界场景中的通用性和上下文保留能力。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 856, \"height\": 702, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1706, \"height\": 573, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1709, \"height\": 1301, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 783, \"height\": 569, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1820, \"height\": 614, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 669, \"height\": 243, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 215, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-piccinelli-unik3d-universal-camera-monocular-3d-estimation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 792, \"height\": 180, \"label\": \"Table\"}]"
motivation: 传统单目3D估计依赖针孔或校正图像假设，无法适应鱼眼、全景等真实相机。
method: 引入球形3D表示，结合与模型无关的相机参数表示，实现任意相机下的度量重建。
result: 在多种相机模型上实现准确的度量3D重建，摆脱了对相机模型假设的依赖。
conclusion: 统一任意相机模型可大幅提升单目3D估计的通用性和实用性。
---

## Abstract
Monocular 3D estimation is crucial for visual perception. However, current methods fall short by relying on oversimplified assumptions, such as pinhole camera models or rectified images. These limitations severely restrict their general applicability, causing poor performance in real-world scenarios with fisheye or panoramic images and resulting in substantial context loss. To address this, we present UniK3D, the first generalizable method for monocular 3D estimation able to model any camera. Our method introduces a spherical 3D representation which allows for better disentanglement of camera and scene geometry and enables accurate metric 3D reconstruction for unconstrained camera models. Our camera component features a novel, model-independent representation of the pencil of rays, achieved through a learned superposition of spherical harmonics. We also introduce an angular loss, which, together with the camera module design, prevents the contraction of the 3D outputs for wide-view cameras. A comprehensive zero-shot evaluation on 13 diverse datasets demonstrates the state-of-the-art performance of UniK3D across 3D, depth, and camera metrics, with substantial gains in challenging large-field-of-view and panoramic settings, while maintaining top accuracy in conventional pinhole small-field-of-view domains. Code and models are available at github.com/lpiccinelli-eth/unik3d

---

## 论文详细总结（自动生成）

# UniK3D：任意相机的通用单目3D估计（CVPR 2025）论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：单目 3D 估计是视觉感知的核心任务，广泛应用于自动驾驶、机器人导航和 3D 建模。然而，现有方法普遍依赖过度简化的假设：
  - 仅支持**针孔相机模型**（如 UniDepth、DepthPro）；
  - 或需要**已校正/矫正图像**（如 Metric3D 依赖 GT 矫正参数）；
  - 预测视差或 log-depth 在**视场角（FoV）超过 180°** 时在数学上不适定。
- **核心痛点**：
  - 鱼眼、全景等真实世界相机具有强非线性畸变，现有方法在这些场景下性能严重退化、丢失大量上下文信息；
  - 现有方法无法在训练中有效学习通用相机估计，输出空间本身存在固有局限（如 depth 表示在大 FoV 下失效）。
- **本文目标**：提出 **UniK3D**——首个**相机无关（camera-universal）**的单目度量 3D 估计框架，无需任何相机内参或预处理，即可从单张图像对任意相机（针孔、鱼眼、全景）进行准确的度量 3D 重建。

## 2. 方法论

### 2.1 核心思想
- **双重球形化**：
  1. **输出空间球形化**：用**径向距离（radius）**替代传统垂直深度（depth），使物体投影尺寸与半径呈单值函数关系，解决大视角下深度表示的病态问题，并增强 xy 平面附近的数值稳定性。
  2. **相机表示球形化**：用**球谐函数（Spherical Harmonics, SH）**基直接建模像素的光线束（pencil of rays），摆脱对具体相机模型（针孔、Mei、UCM 等）的依赖。

### 2.2 关键技术细节
- **相机内部表示**（Angular Module）：
  - 从编码器 class tokens 预测 15 个 SH 系数 + 3 个域参数（主点 2 个 + 水平 FoV 1 个，垂直 FoV 由水平 FoV 按方形像素假设推算），共 **19 个参数**；
  - 光线重建公式：`C = F⁻¹_B{H} = Σ_l Σ_m H_lm · B_lm(θ, φ)`，其中 B_lm 为勒让德多项式形式的 SH 基函数；
  - SH 基保证输出的**连续性、可微性**，且具有高稀疏性（3 阶基仅需 15 个系数），可紧凑表示大多数相机内参。
- **径向模块**（Radial Module）：
  - 编码器提取多分辨率 dense features，经 Transformer Decoder（4 个并行层，每分辨率一个）以相机角度信息为条件进行调制，再经上采样输出 log-radius，最后指数化为半径。
- **防止分布收缩（FoV Contraction）**——三个关键策略：
  1. **非对称角度损失（Asymmetric Angular Loss）**：基于分位数回归，`Lα_AA(θ̂, θ*) = α·Σ|θ̂−θ*|(θ̂>θ*) + (1−α)·Σ|θ̂−θ*|(θ̂≤θ*)`；对极角 θ 用 α=0.7，对方位角 φ 用 α=0.5（即标准 L1），最终角度损失 `L_A = β·L⁰·⁷_AA(θ) + (1−β)·L⁰·⁵_AA(φ)`，β=0.75。相比简单的数据重平衡，该方法直接高效地克服了训练数据中针孔小 FoV 占主导导致的宽角欠表示问题。
  2. **增强相机条件机制**：采用**静态（不可学习）正弦编码**注入相机光线；采用**课程学习**策略，以概率 `1 − tanh(s/10⁵)` 逐步从 GT 相机过渡到预测相机；对输入 Radial Module 的相机输出进行**梯度截断（stop-gradient）**，模拟外部信息流；禁用 cross-attention 中的 LayerScale，防止条件短路。
  3. **梯度缩放**：Angular Module 到 class tokens 的梯度乘以 0.1，以平衡相机与径向梯度的量级差异。
- **总损失**：`L_total = L_A + η·L_rad + γ·L_conf`，其中 η=2、γ=0.1；径向损失为预测与 GT log-radius 的 L1 损失，置信度损失为 detached 径向损失与预测置信度 Σ 的 L1 损失。

### 2.3 算法流程（文字描述）
1. 输入单张 RGB 图像 → ViT 编码器提取多分辨率特征 F 与 class tokens T；
2. Angular Module 从 T 预测 SH 系数与域参数 → 逆
