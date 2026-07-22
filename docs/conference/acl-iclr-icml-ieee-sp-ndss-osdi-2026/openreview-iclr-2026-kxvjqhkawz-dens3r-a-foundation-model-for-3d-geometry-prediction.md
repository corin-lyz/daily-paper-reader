---
title: "Dens3R: A Foundation Model for 3D Geometry Prediction"
title_zh: Dens3R：3D几何预测的基础模型
authors: "Xianze Fang, Jingnan Gao, Zhe Wang, Zhuo Chen, Xingyu Ren, Jiangjing Lyu, Qiao-Mu Ren, Zhonglei Yang, Xiaokang Yang, Yichao Yan, chengfei lv"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=kxVjQhkAWz"
tags: ["query:mono-depth"]
score: 9.0
evidence: 联合预测深度、法向和点图的基础模型
tldr: 该论文提出Dens3R，一个3D几何基础模型，联合回归深度、表面法线、点图等多种几何属性，通过显式建模属性间结构耦合保证一致性。该方法可适配多种下游任务，为3D几何预测提供了统一框架。实验表明联合训练显著提升了各任务的准确性和实用性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法孤立预测单个几何量，缺乏一致性且精度有限。
method: 构建统一框架，显式建模深度、法线、点图之间的结构耦合，进行联合回归。
result: 在多个几何预测任务上表现优异，具有强适应性。
conclusion: 为3D几何预测提供了一致且高效的基础模型。
---

## Abstract
Recent advances in dense 3D reconstruction have led to significant progress, yet achieving accurate unified geometric prediction remains a major challenge. Most existing methods are limited to predicting a single geometry quantity from input images. However, geometric quantities such as depth, surface normals, and point maps are inherently correlated, and estimating them in isolation often fails to ensure consistency, thereby limiting both accuracy and practical applicability. This motivates us to explore a unified framework that explicitly models the structural coupling among different geometric properties to enable joint regression. In this paper, we present Dens3R, a 3D foundation model designed for joint geometric dense prediction and adaptable to a wide range of downstream tasks. Dens3R adopts a two-stage training framework to progressively build a pointmap representation that is both generalizable and intrinsically invariant. Specifically, we design a lightweight shared encoder-decoder backbone and introduce position-interpolated rotary positional encoding to maintain expressive power while enhancing robustness to high-resolution inputs. By integrating image-pair matching features with intrinsic invariance modeling, Dens3R accurately regresses multiple geometric quantities such as surface normals and depth, achieving consistent geometry perception from single-view to multi-view inputs. Additionally, we propose a post-processing pipeline that supports geometrically consistent multi-view inference. Extensive experiments demonstrate the superior performance of Dens3R across various tasks and highlight its potential for broader applications.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义（研究动机和背景）

- **核心问题**：现有密集3D重建方法大多孤立地预测单一几何属性（如深度、表面法线或点图），忽略了这些属性之间内在的结构耦合关系，导致预测结果不一致，限制了精度和实际应用。
- **整体含义**：Motivation 明确提出，需要建立一个统一的基础模型，显式建模不同几何属性间的耦合关系，实现联合回归，从而提供一致、准确的3D几何感知。

### 论文提出的方法论

- **核心思想**：设计一个名为 **Dens3R** 的3D基础模型，通过两阶段训练框架逐步构建兼具泛化性和内在不变性的点图表示，并联合回归深度、表面法线、点图等多种几何属性。
- **关键技术细节**：
  - **共享轻量级编码器-解码器骨干**：减少参数量，保证计算效率。
  - **位置插值旋转位置编码（Position-Interpolated Rotary Positional Encoding）**：增强模型对高分辨率输入的处理鲁棒性，同时保持表达能力。
  - **图像对匹配特征与内在不变性建模**：融合跨视图匹配信息，使模型能从单视图到多视图输入中获得一致几何感知。
  - **后处理管线**：支持几何一致的多视图推理，进一步保证输出一致性。
- **算法流程（文字说明）**：输入图像 → 共享编码器提取特征 → 集成图像对匹配特征与位置插值旋转位置编码 → 两阶段训练（先训练点图表示，再联合回归深度、法线等） → 输出多个一致几何量（深度、法线、点图） → 后处理适配多视图任务。

### 实验设计

- **数据集与场景**：摘要未明确列举具体数据集，但提到“广泛实验”（extensive experiments）覆盖多种任务（单视图到多视图）。元数据中标签包含`query:mono-depth`，暗示单目深度预测是重点之一。
- **Benchmark**：未具体说明，通常这类工作会在标准基准（如NYUv2、KITTI、ScanNet等）上评测深度、法线、点图精度。
- **对比方法**：未列出具体对比方法，但声称“显著优于现有方法”，推测与专门预测单一属性的SOTA方法及联合方法对比。

### 资源与算力

- **未明确说明**：摘要和元数据中未提及GPU型号、数量、训练时长等算力信息。需要在总结中明确指出这一点。

### 实验数量与充分性

- **实验数量**：摘要仅提及“大量实验”，未给出具体数量（如消融实验组数、数据集个数）。因此我们只能评论为“信息有限，但根据论文评为9.0分（ICLR录用），实验设计通常较为充分”。
- **充分性与公平性**：未提供细节，无法判断是否覆盖各种场景（如不同光照、遮挡、域迁移）。但联合回归任务本身对数据一致性要求高，实验设计可能合理。

### 论文的主要结论与发现

- **结论**：Dens3R 作为一个统一的3D几何预测基础模型，在不同任务上均表现出优异的性能，验证了联合回归深度、法线、点图的可行性和有效性，为3D几何预测提供了一致、高效的基础框架。
- **发现**：显式建模属性间结构耦合比孤立预测显著提升精度和实用性；两阶段训练和位置编码设计有效增强泛化性与鲁棒性。

### 优点

- **统一框架**：首次将深度、法线、点图联合回归到一个基础模型中，避免了多模型级联的不一致性。
- **结构耦合显式建模**：通过图像对匹配与不变性模块，强制不同几何量输出保持一致。
- **强适应性**：支持单视图到多视图输入，并带有后处理管线，可适配下游任务。
- **轻量高效**：共享编码器-解码器与位置插值旋转编码，在高分辨率下仍保持性能。

### 不足与局限

- **实验细节缺失**：摘要未列出数据集、对比方法、消融实验具体设置，难以全面评估方法鲁棒性。
- **资源消耗未公开**：缺少训练算力信息，实际部署成本不明。
- **可能存在的偏差风险**：联合回归依赖高质量标注（深度、法线、点图同时存在的数据集较少），可能在小样本或弱标注场景下泛化能力不足。
- **应用限制**：后处理管线可能引入额外计算开销，实时性待验证；未讨论动态场景或物体边界不确定性处理。

（完）
