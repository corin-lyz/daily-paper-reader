---
title: "Depth Pro: Sharp Monocular Metric Depth in Less Than a Second"
title_zh: Depth Pro：不到一秒生成锐利的单目度量深度
authors: "Alexey Bochkovskiy, Amaël Delaunoy, Hugo Germain, Marcel Santos, Yichao Zhou, Stephan Richter, Vladlen Koltun"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=aueXfY0Clv"
tags: ["query:mono-depth"]
score: 10.0
evidence: 零样本度量单目深度基础模型，边界锐利且速度快
tldr: 现有单目深度估计方法难以同时获得度量精度、锐利边界与实时速度。本文提出Depth Pro基础模型，利用高效多尺度视觉Transformer并结合真实与合成数据训练，可在0.3秒内生成2.25百万像素的度量深度图，且无需相机内参。其预测边界细节丰富，并配套专用边界精度评估指标，为零样本真实场景深度估计设立了新标杆。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有零样本深度模型难以兼顾度量精度、边界锐度与运行速度，且依赖相机内参。
method: 提出高效多尺度视觉Transformer，结合真实与合成数据联合训练，并设计边界精度评估指标。
result: 0.3秒生成2.25百万像素锐利度量深度图，边界细节突出，速度与精度俱佳。
conclusion: 该方法为快速、高精度、零样本单目度量深度估计提供了新的基础模型。
---

## Abstract
We present a foundation model for zero-shot metric monocular depth estimation. Our model, Depth Pro, synthesizes high-resolution depth maps with unparalleled sharpness and high-frequency details. The predictions are metric, with absolute scale, without relying on the availability of metadata such as camera intrinsics. And the model is fast, producing a 2.25-megapixel depth map in 0.3 seconds on a standard GPU. These characteristics are enabled by a number of technical contributions, including an efficient multi-scale vision transformer for dense prediction, a training protocol that combines real and synthetic datasets to achieve high metric accuracy alongside fine boundary tracing, dedicated evaluation metrics for boundary accuracy in estimated depth maps, and state-of-the-art focal length estimation from a single image. Extensive experiments analyze specific design choices and demonstrate that Depth Pro outperforms prior work along multiple dimensions. We release code & weights at https://github.com/apple/ml-depth-pro

---

## 论文详细总结（自动生成）

# 深度论文分析总结：Depth Pro——不到一秒生成锐利的单目度量深度

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的零样本单目深度估计（zero-shot monocular depth estimation）方法在以下三个维度难以同时取得良好表现：
  - **度量精度**：预测的深度值是否具有真实物理尺度（metric/absolute scale）。
  - **边界锐度**：深度图能否保留清晰的物体边界和高频细节。
  - **运行速度**：是否能在实际应用中快速推理。
  
  此外，许多现有方法依赖相机内参等元数据作为输入，限制了它们在真实场景中的泛化能力。

- **研究背景**：单目深度估计是计算机视觉的基础任务，广泛应用于三维重建、自动驾驶、AR/VR、图像编辑等场景。随着基础模型范式的兴起，构建一个可以在任意图像上进行零样本度量深度预测的通用模型，具有重要研究价值和实践意义。

- **整体含义**：Depth Pro 的目标是成为一个**零样本、度量、高速、高分辨率、边界锐利**的单目深度估计基础模型，在不依赖相机内参的前提下，同时达到此前方法无法兼顾的精度、清晰度和速度。

## 2. 方法论：核心思想与关键技术

论文提出的 Depth Pro 模型，其方法论可概括为以下核心技术贡献：

- **高效多尺度视觉 Transformer（Efficient Multi-Scale Vision Transformer）**：
  - 采用专门为稠密预测任务设计的多尺度视觉 Transformer 架构，能够同时编码全局上下文和高频空间细节，实现从图像到深度图的端到端映射。
  - 强调网络设计的“高效性”，使得在保持高分辨率输出的同时，推理速度快至 0.3 秒。

- **联合训练协议（Training Protocol Combining Real and Synthetic Datasets）**：
  - 将**真实数据**和**合成数据**联合用于训练。
  - 真实数据用于保证度量精度的绝对尺度和真实场景泛化能力。
  - 合成数据（如渲染的虚拟场景）用于提供密集、精确的深度标签，从而增强边界追踪和几何细节的学习效果。

- **专用边界精度评估指标（Dedicated Evaluation Metrics for Boundary Accuracy）**：
  - 论文指出传统深度评估指标（如 RMSE、相对误差）对边界区域的误差不敏感。
  - 为此设计并提出了专门用于评估深度图边界精度的指标，用于量化模型在物体边缘处预测的锐利程度。

- **单图像焦距估计（State-of-the-Art Focal Length Estimation from a Single Image）**：
  - 模型能够从单张图像中直接估计焦距，达到当时最先进水平（SOTA），从而在不提供相机内参的情况下实现度量深度预测。

- **整体算法流程（文字描述）**：
  1. 输入一张 RGB 单目图像；
  2. 通过多尺度视觉 Transformer 对图像进行稠密特征提取；
  3. 网络同时输出（或通过内部子网络估计）焦距参数；
  4. 基于估计的焦距和特征，解码出具有绝对尺度的度量深度图（2.25 百万像素分辨率）；
  5. 整个过程在标准 GPU 上约 0.3 秒完成。

## 3. 实验设计

- **数据集与场景**：
  - 论文所述内容中并未具体列出全部数据集名称。但从摘要和元数据信息推断，实验涵盖了：
    - **真实数据集**：用于评估零样本泛化能力和度量精度，包括常见的室内外深度基准。
    - **合成数据集**：用于训练，以提供高质量、像素级精确的深度真值。
  - 具体的数据集列表、评测基准名称（如 NYU-Depth-v2、KITTI、ScanNet 等）在提供的文本中**未详细说明**。

- **Benchmark**：
  - 论文明确提到使用了 **边界精度专用评估指标**，并进行了与先前进方法的系统性对比。
  - 零样本度量深度估计的评估基准应是领域内的标准基准组合，但具体细节未在给定文本中详述。

- **对比方法**：
  - 论文中明确表示 “Depth Pro outperforms prior work along multiple dimensions”，即与多类先前方法在多维度上进行了对比。
  - 具体对比了哪些基线模型（如 MiDaS、DPT、ZoeDepth、Metric3D 等）在给定文本中未逐一列出。

## 4. 资源与算力

- **算力信息**：提供的论文文本中**未明确说明**训练所使用的 GPU 型号、数量、训练时长等具体算力信息。
- **唯一提及的硬件约束**：推理速度为 “在标准 GPU 上 0.3 秒生成 2.25 百万像素深度图”，但“标准 GPU”的具体型号未说明。
- **需查阅原始论文的附录或实验设置部分**才能获取详细的训练资源信息。

## 5. 实验数量与充分性

- **实验数量**：
  - 摘要中使用了 “Extensive experiments” 的描述，表明进行了大量实验。
  - 明确提到的实验类型包括：对具体设计选择（specific design choices）的分析实验，以及针对边界精度指标的评估实验。
  - 具体实验组数（如消融实验的数量、数据集数量、对比实验数量）在给定文本中**无法获知**。

- **充分性与客观性**：
  - 从文本描述来看，实验设计覆盖了“与先前方法对比”和“设计选择分析”两条主线，能够较为客观地验证方法的有效性。
  - 由于本文仅为摘要级文本，无法判断是否覆盖了所有关键数据集、是否进行了充分统计检验、是否考虑不同传感器分布偏差等，因此**完整性无法完全确认**。但作为 ICLR 2025 录取论文，可以合理预期实验经过了同行评审。

## 6. 主要结论与发现

- **核心结论**：Depth Pro 是首个在**零样本**场景下同时实现**度量精度**、**锐利边界**、**高分辨率（2.25MP）**和**快速推理（0.3秒）**的单目深度估计基础模型。
- **技术发现**：
  - 多尺度视觉 Transformer 架构可以有效兼顾全局上下文与高频细节。
  - 真实与合成数据联合训练是同时获得度量精度与边界质量的关键。
  - 提出的边界精度指标能够发现并量化现有深度模型的边界缺陷，为后续研究提供了新的评测工具。
  - 单图像焦距估计可以替代相机内参，提升部署灵活性。
- **实证结论**：在多个维度上优于先前方法，尤其是在边界锐度和速度-精度权衡上取得了显著优势。

## 7. 优点

- **多维度同步突破**：在度量精度、边界细节、速度三者之间取得了此前未有的平衡，是该领域的重要进展。
- **零依赖相机内参**：通过在单图像中估计焦距，大幅提升了模型的通用性和实用性，降低了部署门槛。
- **高质量边界输出**：针对边界不清晰这一普遍痛点专门设计训练策略和评估指标，具有较强方法论价值。
- **设计完备的评估体系**：提出专用边界指标，弥补了传统指标的盲区，有助于更客观地反映模型质量。
- **基础模型范式**：代码和权重开源，具备作为未来研究基座（foundation model）的影响力潜力。
- **推理速度快**：0.3 秒生成 2.25 百万像素深度图，为实时应用提供了可能。

## 8. 不足与局限

- **实验细节不透明**：在提供的文本中，未列出具体数据集名称、对比方法列表、评测基准的详细信息，导致实验覆盖范围的判断受限；需查阅完整论文补充。
- **算力信息缺失**：未报告训练所需的 GPU 资源、耗时、参数量等关键资源信息，难以评估方法的训练成本可持续性。
- **泛化边界未明确讨论**：虽然声称零样本能力强，但对于极端光照、透明物体、反光表面、传感器域差异等常见深度估计挑战场景的鲁棒性未在摘要中展开。
- **焦距估计的误差传播风险**：依赖单图像焦距估计的精度；若焦距估计失败，将对最终度量深度造成系统性偏差，论文未在摘要中说明其鲁棒性边界。
- **评估指标未成标准**：提出的边界精度指标虽好，但其有效性和广泛接受度尚需领域内进一步验证。
- **合成数据依赖**：联合训练协议中合成数据的使用虽然有效，但合成到真实的域差异、对特定场景（如复杂室内布局）的适用性仍需更深入讨论。

（完）
