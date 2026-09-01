---
title: "Lotus: Diffusion-based Visual Foundation Model for High-quality Dense Prediction"
title_zh: Lotus：面向高质量密集预测的扩散式视觉基础模型
authors: "Jing He, Haodong Li, Wei Yin, Yixun Liang, Leheng Li, Kaiqiang Zhou, Hongbo Zhang, Bingbing Liu, Ying-Cong Chen"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=stK7iOPH9Q"
tags: ["query:mono-depth"]
score: 8.0
evidence: 基于扩散模型的零样本密集预测视觉基础模型
tldr: 预训练文生图扩散模型能为密集预测提供强大的零样本泛化先验，但原始扩散公式并不适配。Lotus 系统分析后发现，预测噪声的参数化方式对密集预测有害，多步噪声过程也非必要。基于此，它改为直接预测目标图并简化扩散过程，在保持高质量输出的同时提升效率。该方法可泛化到深度等密集预测任务，为基础模型提供了新思路。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法直接套用图像生成的扩散公式来预测密集任务，参数化方式与多步去噪过程不利于高质量零样本预测。
method: 系统分析密集预测与图像生成的差异，将扩散模型改为直接预测目标图，并减少去噪步数以提升质量和效率。
result: 在多个零样本密集预测任务上获得高质量结果，同时显著降低计算开销。
conclusion: 重新设计扩散公式可使预训练先验更有效地服务于密集预测，为基础模型在深度等任务上的应用奠定基础。
---

## Abstract
Leveraging the visual priors of pre-trained text-to-image diffusion models offers a promising solution to enhance zero-shot generalization in dense prediction tasks. However, existing methods often uncritically use the original diffusion formulation, which may not be optimal due to the fundamental differences between dense prediction and image generation. In this paper, we provide a systemic analysis of the diffusion formulation for the dense prediction, focusing on both quality and efficiency. And we find that the original parameterization type for image generation, which learns to predict noise, is harmful for dense prediction; the multi-step noising/denoising diffusion process is also unnecessary and challenging to optimize. Based on these insights, we introduce $\textbf{Lotus}$, a diffusion-based visual foundation model with a simple yet effective adaptation protocol for dense prediction. Specifically, Lotus is trained to directly predict annotations instead of noise, thereby avoiding harmful variance. We also reformulate the diffusion process into a single-step procedure, simplifying optimization and significantly boosting inference speed. Additionally, we introduce a novel tuning strategy called detail preserver, which achieves more accurate and fine-grained predictions. Without scaling up the training data or model capacity, Lotus achieves SoTA performance in zero-shot depth and normal estimation across various datasets. It also enhances efficiency, being significantly faster than most existing diffusion-based methods. Lotus' superior quality and efficiency also enable a wide range of practical applications, such as joint estimation, single/multi-view 3D reconstruction, etc.

---

## 论文详细总结（自动生成）

# Lotus 论文总结

## 1. 核心问题与整体含义

- **研究动机**：预训练文生图扩散模型包含丰富的视觉先验，能够为深度估计、法线估计等密集预测任务带来较强的零样本泛化能力。然而，已有方法往往不加调整地直接沿用图像生成语境下的扩散过程，忽略了两类任务（图像生成 vs. 稠密预测）的本质差异。
- **核心问题**：论文系统考察了扩散公式在密集预测中的应用，发现两个关键不适配点：其一，图像生成中采用的"预测噪声"参数化方式对密集预测有害；其二，多步加噪/去噪的扩散过程对密集预测既不必要，也增加了优化难度。
- **整体含义**：重新设计扩散公式，使预训练先验更高效、更高质量地服务于密集预测，为构建覆盖深度等任务的视觉基础模型提供了新思路。

## 2. 论文提出的方法论

- **核心思想**：以预训练扩散大模型的视觉先验为基础，但摒弃"生成导向"的原始扩散公式，改为针对密集预测特性设计轻量、直接的适配方案。
- **关键技术细节**：
  - **直接预测注释图而非噪声**：训练模型直接输出目标注释（如深度图、法线图），避免在扩散过程中学习噪声所引入的有害方差，从而获得更稳定的优化信号和更准确的预测结果。
  - **单步扩散过程**：将原始多步去噪链条缩减为单步过程，大幅简化优化复杂度，显著提升推理速度，同时仍能有效地利用预训练模型的先验。
  - **Detail Preserver（细节保持器）**：一种新的调优策略，在利用先验的同时保留精细结构信息，使预测结果更具细节精度。
- **算法流程**（文字描述）：输入图像 → 编码到扩散模型的特征空间 → 基于预训练的视觉先验，单步直接解码为所需的密集注释图；训练过程引入 Detail Preserver 策略以增强细节保持能力，而不是像传统扩散模型那样逐步去噪。

## 3. 实验设计

- **数据集/场景**：论文在零样本深度估计和法线估计任务上进行了跨多个数据集的评估，涵盖多样化的真实场景；同时展示了联合估计、单视图/多视图三维重建等实际应用场景。
- **Benchmark**：以零样本密集预测（深度、法线）为主基准，对比模型在该基准上的泛化能力和精度。
- **对比方法**：与现有扩散式密集预测方法比较（多数为套用原始扩散公式的方法），论文没有在摘要层面详细列出各具体方法名称，但声称在多个数据集上均达到了 SoTA 水平。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- 仅提到"没有扩大训练数据或模型容量"即可取得 SoTA 结果，暗示其方法在相对有限的资源下也具有竞争力；但具体训练成本仍需查阅全文。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，至少包括：
  - 多数据集上的零样本深度评估；
  - 多数据集上的零样本法线评估；
  - 对扩散参数化方式（噪声预测 vs. 直接预测）、扩散步数等进行的分析/消融实验；
  - 多类应用展示（联合估计、单/多视图三维重建）。
- **充分性评判**：实验维度覆盖了核心对比（方法有效性）、机理验证（为什么原始扩散公式有害）、效率验证（推理速度）以及泛化性验证（多种下游应用），整体设计较全面。但由于摘要未披露具体数据集名称、指标数值和对比方法细节，**其公平性与充分程度还需要结合全文实验章节来确认**。

## 6. 论文的主要结论与发现

- 图像生成中"预测噪声"的参数化方式对密集预测有害，应改为直接预测目标注释。
- 多步加噪/去噪过程对密集预测并非必需，单步设计可以在保证高质量的同时显著加速推理。
- Detail Preserver 能在不增加数据或模型规模的情况下进一步提升细节精度。
- 重新设计扩散公式后，Lotus 在零样本深度、法线估计上取得 SoTA 性能，并且比多数现有扩散式方法快得多，验证了扩散式基础模型用于密集预测的实用性。

## 7. 优点

- **分析驱动的方法设计**：不盲从图像生成的扩散范式，而是先系统诊断问题，再基于洞察重构模型，思路清晰且具有普适指导意义。
- **质量与效率兼得**：通过单步化设计大幅降低推理成本，突破扩散模型在密集预测中一直以来的效率短板。
- **创新组件 Detail Preserver**：在不增加数据/模型规模的情况下提升细粒度精度，实用价值较高。
- **应用范围广**：除基础深度/法线任务外，还开放了联合估计与三维重建等下游应用，展示了作为视觉基础模型的潜力。

## 8. 不足与局限

- **算力信息缺失**：全文（摘要层面）没有交代训练所需的 GPU 资源和时长，不利于读者评估其训练门槛和可复现成本。
- **完整实验细节不透明**：摘要中未列出具体数据集、指标数值、对比方法及实现细节；零样本性能的置信度（如误差分布、失败场景）有待全文补充。
- **潜在偏差风险**：论文主要基于文生图预训练扩散模型，其先验可能偏向自然图像分布，对域外场景（如医学影像、极端深度范围）的泛化能力如何，摘要未做讨论。
- **应用限制**：虽然演示了三维重建等应用，但对时间开销、内存占用和实际部署条件未给出量化评估；单步改造是否在所有密集预测子任务上都显著优于多步扩散，也有待进一步验证。

（完）
