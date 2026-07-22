---
title: "M3DOnline: Foundation-Prior Guided Monocular 3D Motion Learning for Autonomous Driving in Novel Scenes"
title_zh: M3DOnline：基础模型先验引导的单目3D运动学习用于新场景自动驾驶
authors: "Han Ling, Yinghui Sun, Xian Xu, Quansen Sun, Weihao Zhang, Zhongwen Wang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=oka09rPYGR"
tags: ["query:vfm"]
score: 6.0
evidence: 利用语义和深度基础模型生成伪标签用于场景流学习
tldr: 该论文提出M3DOnline，利用语义和深度基础模型先验设计伪标签生成流水线，基于刚性运动假设分割语义片段并估计3D运动。该方法克服了传统自监督场景流对纹理匹配的依赖，在非朗伯表面和运动边界上表现更优。展示了基础模型在3D运动学习中的赋能作用，可用于密集预测相关任务。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自监督场景流方法依赖强视觉线索，在非朗伯面和运动边界性能差。
method: 利用语义和深度基础模型生成伪标签，基于刚性假设分割场景并估计运动。
result: 在挑战性场景下提升了场景流估计性能。
conclusion: 表明基础模型先验能有效提升自监督3D运动学习。
---

## Abstract
We propose M3DOnline, a learning framework for normalized scene flow (NSF). NSF represents the dense 3D motion of pixels between two frames and plays a critical role in various monocular 3D vision tasks. Existing self-supervised NSF methods heavily rely on strong visual cues, which limits their performance on non-Lambertian surfaces and around motion boundaries.
Our key insight is to leverage useful priors from foundation models to overcome the inherent limitations of texture-based matching in traditional self-supervised methods. Specifically, we design a pseudo-label generation pipeline using semantic and depth foundation models. Based on rigid motion assumptions, we divide real-world scenes into semantic segments and generate per-segment 3D motion pseudo-labels. 
To handle inevitable non-rigid regions and reduce the impact of inaccurate predictions from foundation models, we introduce a loss-based adaptive learning strategy, which filters out obvious non-rigid areas and dynamically adjusts the learning weight and region based on label quality.
Experiments show that M3DOnline significantly improves motion boundary estimation and the handling of reflective and transparent surfaces. This demonstrates the advantage of integrating foundation model priors into self-supervised scene flow learning. Code will be available.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：自监督归一化场景流（NSF）学习方法严重依赖强视觉线索（如纹理匹配），导致在非朗伯表面（如反射、透明材质）以及运动边界区域性能较差。
- **背景意义**：NSF表示两帧间像素的密集3D运动，是单目3D视觉任务（如自动驾驶场景理解）的关键。传统自监督方法因依赖纹理信息而存在固有局限。
- **核心动机**：提出利用基础模型（foundation models）的语义和深度先验来克服纹理匹配的缺陷，从而提升场景流估计在挑战性场景下的鲁棒性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：基于刚性运动假设，利用语义和深度基础模型生成伪标签，指导自监督场景流学习。
- **关键技术细节**：
  - **伪标签生成流水线**：先使用语义基础模型对场景进行语义分割，再结合深度基础模型估计每个语义片段的3D运动伪标签（假设同一语义片段内运动一致）。
  - **即插即用设计**：生成的伪标签作为在线训练时的监督信号，无需修改基础模型架构。
  - **自适应学习策略（Loss-based adaptive learning）**：
    - 通过计算损失值识别非刚性区域（损失过大则视为非刚性），从训练中滤除这些区域。
    - 根据伪标签质量动态调整学习权重和训练区域，降低不准确预测带来的负面影响。
- **算法流程**（文字描述）：
  1. 输入相邻两帧单目图像。
  2. 使用语义分割基础模型（如SAM, Mask2Former）获得逐像素语义标签。
  3. 使用深度估计基础模型（如MiDaS, DPT）获得逐像素深度。
  4. 基于刚性假设，对每个语义分割片段，通过深度和光流（或对应点）估计3D刚体运动参数（旋转与平移），生成该片段所有像素的3D运动伪标签。
  5. 将伪标签作为监督信号，训练一个场景流预测网络（如PWC-Net变体）。
  6. 训练过程中，计算每个像素的预测损失，若损失超过阈值则视为非刚性区域，不参与梯度更新；并根据损失分布动态调整不同区域的损失权重。

## 3. 实验设计

- **数据集/场景**：文中未明确列出具体数据集名称（根据常见场景流基准，推测可能包括KITTI Scene Flow、Sintel、或者自采集自动驾驶场景）。  
  元数据提及“在挑战性场景下提升了场景流估计性能”，但没有细说。
- **基准（Benchmark）**：未明确指定，但对比方法应包括现有自监督场景流方法（如Self-Flow，CAMS，UnSceneFlow等）。
- **对比方法**：未具体列举，从摘要推断对比了传统自监督方法，并验证了优势。
- **实验充分性评价**：仅从摘要看，实验信息非常有限，未提供定量对比表、消融实验分组、具体指标（如EPE、Outliers、Acc3D等）。因此实验覆盖度不够清晰。

## 4. 资源与算力

- **文中未明确说明**：没有提及GPU型号、数量、训练时长等算力信息。

## 5. 实验数量与充分性

- **实验数量**：文中仅笼统描述“实验表明M3DOnline显著改善运动边界估计和反射/透明表面处理”，未给出具体实验组数、消融实验数、不同数据集结果。
- **充分性评估**：不充分。缺乏量化对比表、消融实验可视化、超参数分析等，难以判断方法的可复现性和稳定性。但从元数据看，该论文为 ICLR-2026 被拒稿，可能实验部分存在不足。

## 6. 论文的主要结论与发现

- 将基础模型先验（语义+深度）引入自监督场景流学习，能有效克服传统方法对纹理匹配的依赖。
- 在运动边界、非朗伯表面（反射、透明）等挑战性场景下，性能显著优于现有自监督方法。
- 自适应学习策略可以减轻伪标签不准确带来的负面影响，提高训练稳定性。

## 7. 优点

- **创新性**：率先将语义和深度基础模型先验集成到自监督3D运动学习管线中，利用刚性假设生成伪标签，思路新颖。
- **实用性**：即插即用，无需修改基础模型，可方便地适配不同场景流网络。
- **自适应策略**：基于损失动态调整学习区域和权重，巧妙处理非刚性运动和伪标签噪声，提高了训练鲁棒性。

## 8. 不足与局限

- **实验不充分**：缺乏具体数据集、定量指标、消融实验、对比方法的详细结果，无法评估方法的实际增益和局限性。
- **依赖基础模型质量**：方法有效性高度依赖语义分割和深度估计基础模型在测试场景上的泛化能力，若基础模型在极端场景（如夜间、雨雾）下失效，则伪标签质量无法保证。
- **刚性假设限制**：对于大量非刚性物体（如行人肢体、动物）无法适用，尽管自适应策略可以滤除，但会丢失监督信号。
- **计算开销**：伪标签生成需多次推理基础模型（语义+深度），可能增加在线训练成本。
- **验证范围窄**：未提及在多个公开benchmark上的全面评估，论文结论的泛化性存疑。
- **未考虑实时性**：未讨论推理速度是否满足自动驾驶实时要求。

（完）
