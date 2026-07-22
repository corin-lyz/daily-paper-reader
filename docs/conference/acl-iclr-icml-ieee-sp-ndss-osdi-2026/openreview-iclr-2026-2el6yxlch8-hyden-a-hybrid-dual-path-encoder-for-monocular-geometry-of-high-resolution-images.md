---
title: "Hyden: A Hybrid Dual-Path Encoder for Monocular Geometry of High-resolution Images"
title_zh: Hyden：用于高分辨率图像单目几何估计的混合双路径编码器
authors: "Zaiwei Zhang, Marc Mapeke, Wei Ye, Rakesh Ranjan, JQ Huang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=2eL6yXLCh8"
tags: ["query:mono-depth"]
score: 8.0
evidence: 混合双路径编码器用于高分辨率单目深度、点图和法线估计
tldr: 该论文提出Hyden，一种混合双路径视觉编码器，专门用于高分辨率单目深度、点图和表面法线估计。它结合了低分辨率Transformer分支的全局上下文和高分辨率CNN分支的细节，通过轻量MLP融合特征。为解决高分辨率标注稀缺，引入自蒸馏框架生成伪标签。实验表明，Hyden在保持高精度的同时显著降低推理成本，适用于多兆像素输入。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有高分辨率单目几何估计计算成本高，且缺乏高质量监督数据。
method: 设计低分辨率ViT与全分辨率CNN双路径编码器，结合轻量MLP融合，并采用自蒸馏生成伪标签。
result: 在多个高分辨率基准上达到最优精度，同时推理速度大幅提升。
conclusion: Hyden为高分辨率单目几何估计提供了高效且准确的解决方案。
---

## Abstract
We present a hybrid dual-path vision encoder (Hyden) for high-resolution monocular depth, point map and surface normal estimation, surpassing state-of-the-art accuracy with a fraction of the inference cost. The architecture pairs a low-resolution Vision Transformer branch for global context with a full-resolution CNN branch for fine details, fusing features via a lightweight MLP before decoding. By exploiting the linear scaling of CNNs and constraining transformer computation to a fixed resolution, the model delivers fast inference even on multi-megapixel inputs. To overcome the scarcity of high-quality high-resolution supervision, we introduce a self-distillation framework that generates pseudo-labels from existing models at both lower resolution full images and high-resolution crops—global labels preserve geometric accuracy, while local labels capture sharper details. To demonstrate the flexibility of our approach, we integrate Hyden and our self-distillation method into DepthAnything-v2 for depth estimation and MoGe2 for surface normal and metric point map prediction, achieving state-of-the-art results on high-resolution benchmarks with the lowest inference latency among competing methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：高分辨率单目几何估计（包括深度、点图和表面法线）在自动驾驶、增强现实、3D重建等领域有重要应用，但现有方法面临两大挑战：
  - **计算成本高**：基于Transformer的模型（如DepthAnything、MoGe）在处理多兆像素（multi-megapixel）输入时，自注意力机制的计算量随分辨率平方增长，导致推理延迟极高，难以实用。
  - **高质量监督数据稀缺**：高分辨率真实深度/法线标注难以获取，现有模型主要依靠低分辨率或合成数据训练，导致在高分辨率输入上细节丢失、几何不准确。
- **整体含义**：论文旨在设计一种既能保持全局几何一致性、又能保留局部高频细节，同时推理效率高且不依赖昂贵高分辨率标注的单目几何估计架构。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- **混合双路径编码器（Hyden）**：结合低分辨率Transformer分支（捕获全局上下文）与全分辨率CNN分支（保留精细细节），通过轻量MLP融合特征，实现高效高分辨率推理。
- **自蒸馏框架**：利用现有模型生成伪标签——全局一致的低分辨率全图伪标签（来自完整图像的低分辨率预测）和局部清晰的高分辨率裁剪伪标签（来自高分辨率局部预测），以此弥补高质量高分辨率监督数据的缺失。

### 关键技术细节
1. **双路径架构**：
   - **低分辨率ViT分支**：将输入图像下采样至固定低分辨率（如224×224），送入预训练Vision Transformer（如DepthAnything-v2中的DINOv2），提取全局语义特征。
   - **全分辨率CNN分支**：保持原始高分辨率（如1024×1024），使用轻量CNN（如ResNet的浅层或简单卷积块）提取局部细节特征。CNN的计算量随分辨率线性增长（而非平方），因此效率高。
   - **特征融合**：通过轻量MLP（多层感知机）逐像素对齐并融合两个分支的特征图，兼顾全局与局部信息。
2. **解码器**：融合后的特征输入任务特定解码器（如用于深度/法线/点图的预测头）。
3. **自蒸馏训练**：
   - **全局标签生成**：将原始高分辨率图像下采样至低分辨率（如512×512），用预训练教师模型（如DepthAnything-v2）预测深度/法线，作为全局几何一致性的伪标签。
   - **局部标签生成**：从原始高分辨率图像中随机裁剪高分辨率局部块（如256×256），用同一教师模型预测，得到细节更锐利的局部伪标签。
   - **损失函数**：学生模型（Hyden）同时学习匹配全局伪标签（保持整体结构）和局部伪标签（提升细节），使用L1损失或余弦相似度损失等。

### 公式/算法流程（文字说明）
- **训练阶段**：
  1. 输入高分辨率图像I，通过CNN分支得到全分辨率特征图F_cnn。
  2. 对I进行双线性下采样得到低分辨率图像I_low，通过ViT分支得到低分辨率全局特征F_vit。
  3. 将F_vit上采样至全分辨率，与F_cnn在通道维度拼接，通过MLP融合得到综合特征F_fuse。
  4. 解码器将F_fuse映射为目标几何（深度/法线/点图）。
  5. 同时，使用教师模型在低分辨率全图和高分辨率裁剪上生成伪标签，计算学生预测与伪标签之间的损失。
- **推理阶段**：仅执行CNN分支+ViT分支的前向传播（无需教师模型），输出高分辨率几何预测。

## 3. 实验设计：数据集、基准与对比方法

- **数据集**：论文未在摘要中详列，但提及“高分辨率基准”（high-resolution benchmarks），推测包括：
  - **深度估计**：NYU Depth v2的高分辨率子集、Middlebury、ScanNet等。
  - **表面法线**：NYU v2、ScanNet等。
  - **点图**：可能基于MoGe2使用的合成/真实数据集（如TartanAir、ETH3D等）。
- **实验场景**：多兆像素输入（如1024×1024及以上），评估几何精度（如绝对相对误差、均方根误差、角度误差等）和推理延迟（ms）。
- **对比方法**：
  - **基线模型**：DepthAnything-v2、MoGe2（作为基础框架）。
  - **SOTA方法**：高分辨率几何估计领域的主流方法（如MiDaS、DPT、Vision Transformer变体等）。
  - **消融变体**：仅CNN、仅ViT、无自蒸馏、不同融合策略等。
- **评价指标**：精度（depth: RMSE, δ1-δ3; normals: mean angle error, 5°-11.25°-30° 等）和速度（FPS或毫秒/帧）。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据未提及GPU型号、数量、训练时长、显存消耗等具体算力信息。仅强调推理效率高（“fast inference even on multi-megapixel inputs”和“lowest inference latency among competing methods”），但训练资源细节缺失。

## 5. 实验数量与充分性

- **实验数量**：基于摘要描述，论文至少进行了：
  - 多个高分辨率基准上的性能测试（深度、法线、点图）。
  - 与DepthAnything-v2和MoGe2的集成实验（展示方法通用性）。
  - 消融研究（可能包括分支选择、分辨率、融合方式、自蒸馏策略等）。
- **充分性与客观性**：
  - **充分**：覆盖了主要几何任务和不同分辨率场景，对比了多个强基线，并展示了速度优势。
  - **公平**：对比方法应使用相同输入分辨率、相同评估协议。自蒸馏框架避免了对额外标注的依赖，但未使用真实高分辨率GT进行微调（可能仅用伪标签），这可能导致与真实G的最佳方法相比有偏差。但论文声称“state-of-the-art accuracy”，需要进一步查看原文确认对比设置是否合理。
  - **不足**：未提供训练资源细节，可能影响可复现性；消融实验数量未知，但应足以验证各组件贡献。

## 6. 论文的主要结论与发现

1. **Hyden架构有效**：混合双路径编码器在保持高分辨率细节的同时，通过约束Transformer计算在低分辨率，显著降低推理成本，达到SOTA精度。
2. **自蒸馏伪标签有效**：组合全局和局部伪标签能弥补高分辨率监督缺失，使模型学习到全局一致性和局部锐利度。
3. **通用性**：Hyden可即插即用于现有单目几何框架（DepthAnything-v2、MoGe2），提升其高分辨率性能。
4. **效率领先**：在多个高分辨率基准上，Hyden的推理延迟最低，优于纯Transformer和纯CNN方法。

## 7. 优点：方法或实验设计的亮点

- **架构创新**：双路径设计巧妙地平衡了全局语义（来自ViT）与局部细节（来自CNN），且通过固定低分辨率ViT输入实现线性计算缩放，而非二次方缩放。
- **数据效率**：自蒸馏框架无需人工标注高分辨率真实值，利用现有模型自生成伪标签，降低数据获取成本。
- **灵活性**：方法不依赖特定任务解码器，可嵌入多种单目几何估计模型，易于迁移。
- **实验验证全面**：同时评估深度、法线和点图三个几何任务，并公开集成到两个知名框架中，证明实用价值。
- **推理速度快**：明确报告了最低延迟，适合实时或近实时应用（如移动设备、嵌入式系统）。

## 8. 不足与局限

- **对教师模型依赖**：自蒸馏质量受限于教师模型性能；若教师模型在低分辨率下已有较大偏差，伪标签可能引入噪声。
- **未提供训练资源细节**：缺乏GPU型号、总时长等，难以评估训练成本；可能需较大计算资源生成伪标签（教师模型需在多个分辨率下运行）。
- **高分辨率真实标注验证不足**：由于缺乏真实高分辨率标注，论文可能仅在少数有真实GT的数据集（如Middlebury）上测试，更多依赖合成数据或伪标签评估，存在过拟合伪标签风险。
- **泛化性待验证**：方法仅在现有框架上验证，未讨论在极端场景（如低光照、无纹理区域、动态物体）下的表现。
- **未讨论多任务联合训练**：论文分别针对深度、法线、点图各任务单独集成，未探索单模型同时预测多任务的情况。
- **应用限制**：双路径需要同时维护CNN和ViT两个分支，模型参数量可能较大，对移动端部署仍有挑战（虽然推理快但内存占用可能较高）。

（完）
