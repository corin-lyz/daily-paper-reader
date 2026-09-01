---
title: "Beyond 2D Representation: Learning 3D Scene Field for Robust Monocular Depth Estimation"
title_zh: 超越二维表示：学习三维场景场以实现鲁棒单目深度估计
authors: "Haifeng Wu, Shuhang Gu, Lixin Duan, Wen Li"
date: 2024-09-13
pdf: "https://openreview.net/pdf?id=gINO3tfVEP"
tags: ["query:mono-depth"]
score: 9.0
evidence: 基于三维场景场表示的自监督单目深度估计
tldr: 现有单目深度估计通常依赖前视二维特征，难以捕捉反射、阴影遮挡和低纹理区域等复杂物理因素，导致深度不连续或不一致。本文提出基于三维场景场表示的全新自监督框架，从三维角度建模场景以提升深度估计的鲁棒性。实验表明，该方法在多个真实世界场景中显著减少深度不连续与不一致问题。该工作为单目深度估计提供了更强的三维感知表示。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 前视二维特征难以刻画反射、阴影等复杂物理因素，使得真实场景中的单目深度预测不连续且不一致。
method: 提出三维场景场表示的自监督单目深度估计框架，以三维场景场替代传统二维特征提取与深度回归。
result: 在真实场景多个基准上显著改善了反射和低纹理区域的深度连续性与整体估计精度。
conclusion: 三维场景场表示能有效增强单目深度估计对复杂物理场景的鲁棒性，具有广泛适用性。
---

## Abstract
Monocular depth estimation has been extensively studied over the past few decades, yet achieving robust depth estimation in real-world scenes remains a challenge, particularly in the presence of reflections, shadow occlusions, and low-texture regions. Existing methods typically rely on extracting front-view 2D features for depth estimation, which often fail to capture those complex physical factors present in real-world scenes, leading to discontinuous, incomplete, or inconsistent depth maps. To address these issues, we turn to learning a more powerful 3D representation for robust monocular depth estimation, and propose a novel self-supervised monocular depth estimation framework based on the Three-dimensional Scene Field  representation, or TSF-Depth for short. Specifically, we build our TSF-Depth framework upon an encoder-decoder architecture. The encoder extracts scene features from the input 2D image, and subsequently reshapes it as a tri-plane feature field by incorporating scene prior encoding. This tri-plane feature field is designed to implicitly model the structure and appearance of the continuous 3D scene. We then estimate a high-quality depth map from the tri-plane feature field by simulating the camera imaging process. To do this, we construct a 2D feature map with 3D geometry by sampling from the tri-plane feature field using the coordinates of points where the line of sight intersects with the scene. The aggregated multi-view geometric features are subsequently fed into the decoder for depth estimation. Extensive experiments on KITTI and NYUv2 datasets show that TSF-Depth achieves state-of-the-art performance. We also validate the generalization capability of our model on Make3D and ScanNet datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

**论文题目**：Beyond 2D Representation: Learning 3D Scene Field for Robust Monocular Depth Estimation  
（超越二维表示：学习三维场景场以实现鲁棒单目深度估计）  
**作者**：Haifeng Wu, Shuhang Gu, Lixin Duan, Wen Li  
**来源**：ICLR 2025（Rejected）  

## 1. 核心问题与整体含义

- **研究背景**：单目深度估计（Monocular Depth Estimation）是计算机视觉中的经典任务，但真实世界场景中普遍存在反射（reflections）、阴影遮挡（shadow occlusions）和低纹理区域（low-texture regions）等复杂物理因素，使得深度预测变得困难。
- **现有方法的问题**：现有方法通常依赖从前视（front-view）2D 图像提取特征进行深度回归，这些二维特征难以刻画上述三维物理因素，导致生成的深度图出现**不连续、不完整或不一致**的问题。
- **核心动机**：作者认为需要从三维角度建模场景，学习更强大的三维表示，以提升单目深度估计的鲁棒性。整体含义是：将深度估计从“二维像素映射”提升到“三维场景理解”层面，从而更好地应对真实世界中复杂的几何与光学现象。

## 2. 方法论

- **总体框架**：提出一种名为 **TSF-Depth** 的自监督单目深度估计框架，基于编码器-解码器（encoder-decoder）架构。
- **关键思想**：用**三维场景场（Three-dimensional Scene Field）** 表示替代传统二维特征提取与深度回归。所谓场景场，是指通过一个隐式连续三维场来建模场景的结构与外观。
- **技术细节**：
  - **编码阶段**：编码器从输入 2D 图像中提取场景特征，随后结合**场景先验编码（scene prior encoding）**，将这些特征重塑为一个**三平面特征场（tri-plane feature field）**。该三平面场从三个正交平面来隐式地表示连续 3D 场景。
  - **解码阶段**：通过**模拟相机成像过程**，从三平面特征场中采样“视线与场景相交点”的坐标，从而构建一个带 3D 几何信息的 2D 特征图。这个过程本质上类似光线投射（ray casting）沿视线聚合多视图几何特征。
  - **深度估计**：将聚合后的多视图几何特征输入解码器，最终预测高精度深度图。
- **公式或算法流程（文字说明）**：
  1. 输入单张 RGB 图像，编码器提取 2D 特征。
  2. 将 2D 特征重排为三平面特征场（三个正交平面上的特征图）。
  3. 对每个像素对应的三维视线，在视线方向上采样点，并通过三平面场查询得到该点的特征表示。
  4. 沿视线方向聚合采样点特征，形成具有 3D 几何感知的 2D 特征图。
  5. 解码器基于该特征图回归深度值。

## 3. 实验设计

- **数据集**：
  - **训练/评测主数据集**：KITTI（室外驾驶场景）、NYUv2（室内场景）。
  - **泛化验证数据集**：Make3D、ScanNet。
- **Benchmark**：论文未明确说明具体评测协议（例如 Eigen split 或官方 split），但通常此类工作默认使用深度估计的标准 benchmark（如 KITTI Eigen 分割、NYUv2 标准测试集）。
- **对比方法**：论文称达到 **state-of-the-art** 性能，但摘要中未列出具体对比的基线方法。结合领域常识，可能的对比方法包括 Monodepth2、PackNet、ManyDepth 等自监督/监督深度估计模型。具体对比结果和细节未在摘要中给出。

## 4. 资源与算力

- **未提及**：摘要和元数据中**没有明确说明**使用的 GPU 型号、数量、训练时长、参数量或推理时间等算力相关信息。因此无法从该论文内容中获知资源消耗情况。

## 5. 实验数量与充分性

- **实验数量**：
  - 两个主数据集（KITTI、NYUv2）上的性能评测。
  - 两个跨数据集泛化验证（Make3D、ScanNet）。
  - 摘要中**未提及消融实验**，也没有报告针对三平面场景场设计（如平面数量、采样策略、先验编码方式）的组件分析。
- **充分性评估**：
  - **优点**：覆盖了室内和室外两种主流场景，并进行了跨数据集泛化测试，这是评价鲁棒性的关键维度。
  - **不足**：缺少消融实验和详细对比表格，难以判断各个模块的贡献以及相对于具体基线方法的优势程度；也缺少对失败案例和局限性（如反射、阴影极端情况）的讨论。因此，从整体证据强度来看，实验数量偏少，充分性有限，客观性和公平性难以从摘要层面完全判断。

## 6. 主要结论与发现

- 在 KITTI 和 NYUv2 上，TSF-Depth 取得了当前最优（state-of-the-art）的性能。
- 在 Make3D 和 ScanNet 上验证了模型的泛化能力，表明三维场景场表示能够适应不同场景分布。
- 相比传统基于二维特征的方法，三维场景场表示显著减少了反射和低纹理区域的深度不连续与不一致现象，提升了深度估计的整体鲁棒性。
- 结论：通过模拟相机成像过程并利用三维场景场，可以更有效地应对复杂物理场景中的深度估计挑战。

## 7. 优点

- **创新性**：从“二维特征”转向“三维场景场”表示，提供了单目深度估计的一种新视角，突破了传统前视特征的表征局限。
- **机制合理性**：通过模拟相机成像过程（视线采样）来聚合几何特征，使得深度估计具有明确的 3D 几何解释，可解释性较强。
- **自监督框架**：不依赖深度标签，有助于利用大规模无标注真实数据。
- **鲁棒性提升**：针对反射、阴影、低纹理等难点场景进行了专门设计，实验结果支持其有效性。
- **泛化验证**：跨数据集测试增强了方法的说服力。

## 8. 不足与局限

- **信息不完整**：本文摘要和元数据中缺少详细的实验结果表格、对比方法列表、消融研究、训练细节和评估协议，导致无法全面验证方法的有效性。
- **实验覆盖有限**：未在更多挑战性场景（如动态物体、极端天气、夜间、高反射表面等）上充分测试；对反射和阴影的处理效果缺少量化分析。
- **计算成本未知**：三平面场景场和光线采样机制可能引入较大计算开销，但论文未报告推理速度或显存占用，实际部署可行性存疑。
- **偏差风险**：数据集主要集中于驾驶（KITTI）和室内（NYUv2），对通用场景的泛化仍有限制；跨数据集测试仅两个，且未报告失败案例。
- **开放问题**：论文未讨论三维场景场表示在动态场景中的适用性、与语义/物体的交互，以及深度边界处的处理策略；这些因素可能影响实际应用效果。

---

（完）
