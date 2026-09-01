---
title: Benchmarking and Analyzing Monocular Geometry Estimation Models
title_zh: 单目几何估计模型的基准测试与分析
authors: "Yongtao Ge, Guangkai Xu, Zhiyue Zhao, Libo Sun, Yanlong Sun, Hao Chen, Chunhua Shen"
date: 2024-09-24
pdf: "https://openreview.net/pdf?id=jGGylopiO8"
tags: ["query:mono-depth"]
score: 9.0
evidence: 对单目几何深度基础模型进行基准分析
tldr: 单目几何估计基础模型的训练方式多样，评测难以公平比较。本文构建了公平统一的基准，对判别式与生成式模型进行系统分析。结果找出了影响零样本泛化和评测性能的关键因素，为深度估计基础模型的设计与评估提供了重要参考。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 判别式和生成式单目几何估计基础模型的训练配置和数据集各不相同，难以比较和找出关键影响因素。
method: 构建公平且强的基线，并对多种单目几何估计模型进行系统基准测试和影响因素分析。
result: 揭示了影响零样本泛化和评估性能的关键因素，为后续模型设计提供参考。
conclusion: 为单目深度与法线估计基础模型提供了统一的评测框架与深入分析。
---

## Abstract
Recent advances in discriminative and generative pretraining have yielded geometry estimation foundation models with strong generalization capabilities. While most discriminative monocular geometry estimation methods rely on large-scale finetuning data to achieve zero-shot generalization, several generative-based paradigms show the potential of achieving impressive generalization performance on unseen scenes by leveraging pre-trained diffusion models and fine-tuning on even a small-scale of synthetic training data. Frustratingly, these models are trained with different recipes on different datasets, making it hard to find out the critical factors that determine the evaluation performance. To resolve the above issue, (1) we build fair and strong baselines in a unified codebase for evaluating and analyzing the state-of-the-art (SOTA) geometry estimation models from pre-training style, finetuning data, and model architecture perspectives; (2) we thoroughly evaluate geometry models on challenging benchmarks with diverse scenes and high-quality annotations. Under the fair training and evaluation configuration, our results reveal that stochastic diffusion-based protocol is not optimal for fine-tuning generative-based geometry estimation methods. One-step finetuning and inference protocol is sufficient for generative-based depth and surface normal estimation. Besides, we find that both discriminative and generative pretraining can generalize well under small-scale fine-tuning high-quality data in scale-invariant depth estimation task. DINOv2-pretrained discriminative models achieve slightly higher performance than generative counterparts with the same small amount of synthetic data. Furthermore, we have observed that metric depth estimation requires significantly more finetuning data than scale-invariant depth estimation for learning the depth scale distribution. We hope this work will inspire future geometry estimation research in building more high-quality fine-tuning datasets and designing more powerful geometry estimation models.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- 单目几何估计（深度、表面法线等）基础模型近期发展迅速，判别式（discriminative）和生成式（generative）预训练方法都展现出较强的零样本泛化能力。
- 判别式方法通常依赖大规模微调数据，而生成式方法借助预训练扩散模型，甚至仅用少量合成数据即可取得不错的泛化效果。
- 然而，不同模型采用了不同的训练策略、数据集和架构，导致难以公平比较，也无法准确识别决定最终评测性能的关键因素。
- 因此，论文旨在构建一个统一、公平的评测框架，系统分析预训练风格、微调数据和模型架构等因素对单目几何估计性能的影响。

## 2. 论文提出的方法论

- 核心思想：在统一代码库中建立公平且强的基线模型，基于相同的训练和评测配置，对判别式与生成式几何估计方法进行对照分析。
- 关键技术细节：
  - 从“预训练风格”“微调数据”“模型架构”三个维度设计对比实验。
  - 对生成式方法，考察了随机扩散（stochastic diffusion）微调协议与一步式微调/推理（one-step finetuning and inference）协议之间的差异。
  - 对判别式方法，重点分析以 DINOv2 为代表的判别式预训练模型在不同规模微调数据下的表现。
  - 实验区分了尺度不变深度估计与度量深度估计，以单独评估模型对深度尺度分布的学习能力。
- 论文未给出具体公式或算法流程的伪代码，更多是评测协议和对照方法的系统设计。

## 3. 实验设计

- 评测基准：使用多个具有多样场景和高精度标注的挑战性 benchmark，对当前最先进的单目几何估计模型进行公平的横向评测。
- 对比方法：覆盖判别式与生成式两大类方法，包括基于扩散模型的生成式深度/法线估计方法，以及 DINOv2 预训练的判别式方法等。
- 数据集：论文提到使用了“小规模合成数据”“高质量微调数据”等配置，但未在摘要中列出具体数据集名称。
- 评测任务：包含尺度不变深度估计、度量深度估计以及表面法线估计。

## 4. 资源与算力

- 论文摘要和元数据中未明确说明使用的 GPU 型号、数量、训练时长等计算资源信息。
- 作为基准测试与分析型工作，通常需要较多训练与评估硬件，但原文未给出具体细节，因此无法总结具体算力开销。

## 5. 实验数量与充分性

- 论文开展了一系列系统实验，包括：
  - 生成式方法中不同微调协议（随机扩散 vs 一步式）的对比；
  - 判别式与生成式方法在相同小规模微调数据下的性能对比；
  - 尺度不变深度估计与度量深度估计在不同数据规模下的表现差异；
  - 不同模型架构/预训练风格的比较。
- 实验设计强调“公平训练和评测配置”，在统一代码库和相同数据条件下进行比较，因此结论具有较高的客观性和可重复性。
- 但摘要未报告具体实验数量（如消融组数、数据集个数），无法判断统计显著性；不过从结论覆盖多个维度和任务来看，实验设计较为充分。

## 6. 论文的主要结论与发现

- 随机扩散式微调协议并非生成式几何估计方法的最佳选择；一步式微调与推理协议足以支撑生成式深度和表面法线估计。
- 在小规模高质量微调数据下，判别式和生成式预训练在尺度不变深度估计任务上都能取得良好的泛化效果。
- 使用相同小规模合成数据时，DINOv2 预训练的判别式模型性能略高于生成式模型。
- 度量深度估计比尺度不变深度估计需要显著更多的微调数据，才能学习到深度尺度的分布。

## 7. 优点

- 构建统一、公平的评测框架，解决了不同模型难以直接比较的问题。
- 覆盖判别式与生成式两大技术路线，分析维度清晰（预训练风格、数据、架构）。
- 同时评估深度和表面法线，并区分尺度不变与度量深度，分析层次细致。
- 结论具有实际指导意义，例如建议生成式方法采用更简洁的一步式微调协议，可降低训练成本同时保持性能。

## 8. 不足与局限

- 摘要中未披露具体数据集、模型列表和评测基准名称，导致无法独立复现或进一步核实实验细节。
- 未提及计算资源与训练成本，不利于评估方法的经济性。
- 仅从小规模合成高质量数据场景出发，对大规模真实数据、不同数据分布下的表现缺乏讨论。
- 实验结论主要基于当前特定模型和配置，可能无法完全推广到未来更新的架构或更复杂的生成式框架。

（完）
