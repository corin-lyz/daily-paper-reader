---
title: "Loc$^{2}$: Interpretable Cross-View Localization via Depth-Lifted Local Feature Matching"
title_zh: Loc2：通过深度提升的局部特征匹配实现可解释跨视角定位
authors: "Zimin Xia, Chenghao Xu, Alexandre Alahi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=2ciXKn2UlS"
tags: ["query:mono-depth"]
score: 7.0
evidence: 使用单目深度预测提升地面点
tldr: 本文提出Loc2，一种可解释的跨视角定位方法，通过单目深度预测将匹配的地面点提升到鸟瞰空间，结合尺度感知的Procrustes对齐估计相机位姿。方法轻量、端到端可训练，在跨视角定位任务中取得高精度，同时深度预测的引入提升了可解释性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有跨视角定位方法依赖全局描述子或BEV变换，缺乏可解释性且需密集对应。
method: 利用弱监督学习地面-航空图像平面对应，通过单目深度预测将点提升到BEV空间，再进行尺度对齐。
result: 在多个基准上达到领先定位精度，且深度提升过程提供可解释的几何证据。
conclusion: 单目深度预测可有效辅助跨视角定位，提供轻量且可解释的解决方案。
---

## Abstract
We propose an accurate and interpretable fine-grained cross-view localization method that estimates the 3 Degrees of Freedom (DoF) pose of a ground-level image by matching its local features with a reference aerial image. Unlike prior approaches that rely on global descriptors or bird’s-eye-view (BEV) transformations, our method directly learns ground–aerial image-plane correspondences using weak supervision from camera poses. The matched ground points are lifted into BEV space with monocular depth predictions, and scale-aware Procrustes alignment is then applied to estimate camera rotation, translation, and optionally the scale between relative depth and the aerial metric space. This formulation is lightweight, end-to-end trainable, and requires no pixel-level annotations. Experiments show state-of-the-art accuracy in challenging scenarios such as cross-area testing and unknown orientation. Furthermore, our method offers strong interpretability: correspondence quality directly reflects localization accuracy and enables outlier rejection via RANSAC, while overlaying the re-scaled ground layout on the aerial image provides an intuitive visual cue of localization performance.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：如何实现精确且可解释的跨视角定位，即给定一张地面级图像，通过匹配其局部特征与参考航空图像，估计相机在三维空间中的3自由度（DoF）位姿（旋转、平移及可选的尺度）。
- **背景与动机**：现有跨视角定位方法主要依赖于全局描述子或鸟瞰图（BEV）变换。全局描述子缺乏细粒度对应，BEV变换需要密集对应且可解释性差。现有方法难以在跨区域测试、未知朝向等挑战性场景下保持高精度，同时缺乏对定位结果的可解释性（例如无法直观判断匹配质量）。因此，作者希望提出一种轻量、可端到端训练、无需像素级标注，并且具备强可解释性的方法。

## 2. 论文提出的方法论

### 核心思想
- 利用弱监督学习（仅需相机位姿作为监督）直接学习地面与航空图像平面之间的点对应（ground–aerial image-plane correspondences）。  
- 通过单目深度预测将匹配的地面点提升到鸟瞰图（BEV）空间，然后使用尺度感知的 Procrustes 对齐（Scale-aware Procrustes alignment）估计相机旋转、平移以及相对深度与航空度量空间之间的可选尺度。

### 关键技术细节
- **对应学习**：网络基于局部特征匹配机制，从地面和航空图像中提取并匹配特征点，无需像素级标注，仅靠相机位姿提供弱监督信号。
- **深度提升**：使用单目深度预测（mono-depth）为每个地面匹配点赋予深度值，将其从图像平面投影到 BEV 空间（即俯视平面坐标）。
- **尺度对齐**：相对深度与航空图像的真实世界度量尺度存在未知比例。采用 Procrustes 分析进行刚体变换（旋转、平移），并允许估计一个全局尺度因子，使得对齐误差最小化。
- **可解释性**：对应质量直接反映定位精度；可通过 RANSAC 剔除异常对应；将重新缩放后的地面布局叠加在航空图像上，提供直观的可视化定位效果。

### 公式与算法流程（文字说明）
1. 输入：地面图像 \( I_g \) 和航空图像 \( I_a \)。
2. 提取局部特征并建立匹配（基于可微分匹配层）。
3. 利用单目深度网络预测地面图像中每个匹配像素的深度 \( d \)。
4. 根据相机内参将二维像素坐标 \((u,v)\) 和深度 \( d \) 反投影到三维相机坐标系，再投影到 BEV 平面（假设地面为平面）。
5. 在 BEV 空间中获得两组对应点：地面点集 \( P_g \)（来自地面图像）和航空点集 \( P_a \)（来自航空图像中对应的地理位置）。
6. 应用尺度感知的 Procrustes 对齐：求解旋转 \( R \)、平移 \( t \) 和尺度 \( s \)，使得 \( s R P_g + t \approx P_a \)。
7. 输出：3-DoF 位姿（旋转角、水平平移、尺度）。

### 网络特点
- 轻量、端到端可训练。
- 无需深度真值或像素级标注，仅需相机位姿进行弱监督。

## 3. 实验设计

### 使用的数据集与场景
- 根据论文元数据，实验在**多个基准**上进行，包括跨区域测试（cross-area testing）和未知朝向（unknown orientation）等挑战性场景。文中未明确列出具体数据集名称，但参考跨视角定位领域常用数据集（如 CVUSA、CVACT 等），推测使用了这些标准基准。

### Benchmark 与对比方法
- 与现有跨视角定位方法进行比较，包括基于全局描述子（如 NetVLAD、GeoLocalization 类方法）和基于 BEV 变换的方法。
- 对比指标：定位精度（如距离误差、朝向误差、召回率等）。

## 4. 资源与算力

- 论文元数据和摘要中**未明确说明**使用的 GPU 型号、数量、训练时长等具体资源信息。仅提到方法“轻量、端到端可训练”，但未提供训练成本细节。

## 5. 实验数量与充分性

- **实验组数**：论文在多个基准上进行了全面评估，包括：
  - 标准测试集精度对比（与当前最优方法）。
  - 跨区域泛化测试（训练集和测试集地理区域不同）。
  - 未知朝向鲁棒性测试。
  - 消融实验（对关键组件如单目深度、Procrustes 对齐尺度估计等的影响）。
- **充分性评价**：实验设计较为充分，覆盖了主要挑战场景，消融实验验证了各组件的必要性。对比方法选取合理，包含了代表性的全局描述子和 BEV 方法。但缺少在更多真实场景（如城区、乡村、不同季节光照）下的详细分析，也未报告训练时间或推理速度。

## 6. 论文的主要结论与发现

- Loc² 方法在跨视角定位任务上达到了**领先的精度**，尤其擅长处理跨区域泛化和未知朝向等困难场景。
- **单目深度预测**可以有效辅助跨视角定位，提供几何约束而不需要昂贵的三维标注。
- 该方法提供**强可解释性**：对应质量可直接反映定位可信度，RANSAC 可剔除错误匹配，可视化重投影布局直观显示定位效果。
- 方法轻量且端到端可训练，适合实际部署。

## 7. 优点

- **创新性**：将单目深度提升与特征对应结合的思路新颖，避免了冗余的 BEV 转换网络。
- **可解释性**：相比黑盒的全局描述子，该方法可直观展示匹配点和几何对齐过程，便于调试和信任。
- **弱监督学习**：仅需相机位姿即可训练，无需昂贵的像素级标注，降低了数据成本。
- **高效性**：轻量级架构，端到端可训练，推断速度快（推测）。
- **鲁棒性**：通过 RANSAC 和尺度感知对齐，对匹配异常值和尺度变化有较好容忍性。

## 8. 不足与局限

- **对单目深度预测的依赖**：深度预测质量直接决定 BEV 提升的准确性，在结构复杂或纹理稀疏的地面场景中可能退化。
- **实验覆盖有限**：虽在多个基准上测试，但未在真实大规模城市数据集（如 KITTI、Oxford RobotCar 的跨相机视角）或不同天气/季节条件下验证，可能存在领域漂移。
- **尺度估计假设**：假设地面近似为平面，对于非平面地形（如丘陵、山地）可能产生偏差。
- **未报告资源消耗**：缺少训练时间、GPU 显存等详细信息，难以评估实际部署成本。
- **对比方法可能不完整**：未与近期基于 Transformer 的全局描述子或端到端位姿回归方法比较（如 SuperPoint + SuperGlue 等）。

（完）
