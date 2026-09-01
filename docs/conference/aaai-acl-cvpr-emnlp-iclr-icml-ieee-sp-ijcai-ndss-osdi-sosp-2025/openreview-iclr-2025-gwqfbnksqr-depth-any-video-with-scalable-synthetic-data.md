---
title: Depth Any Video with Scalable Synthetic Data
title_zh: 基于可扩展合成数据的任意视频深度估计
authors: "Honghui Yang, Di Huang, Wei Yin, Chunhua Shen, Haifeng Liu, Xiaofei He, Binbin Lin, Wanli Ouyang, Tong He"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=gWqFbnKsqR"
tags: ["query:mono-depth"]
score: 7.0
evidence: 利用可扩展合成数据训练视频深度模型
tldr: 视频深度估计长期受限于真值数据缺乏。本文提出可扩展的合成数据管线，从虚拟环境采集4万段5秒带精确深度标注的视频。同时利用视频扩散模型的先验，配合旋转位置编码和流匹配增强灵活性。最终模型能够处理可变长度的真实视频，显著提升深度一致性与可靠性。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 视频深度估计因缺乏一致且可扩展的真值数据而长期受限，难以在真实视频上获得可靠结果。
method: 构建可扩展的合成数据管线，采集4万段带精确深度标注的视频，并基于视频扩散模型结合旋转位置编码与流匹配进行训练。
result: 生成的视频深度模型可处理灵活长度和真实世界视频，显著提升一致性和可靠性。
conclusion: 为视频深度估计提供了大规模合成数据与生成式先验结合的解决方案。
---

## Abstract
Video depth estimation has long been hindered by the scarcity of consistent and scalable ground truth data, leading to inconsistent and unreliable results. In this paper, we introduce Depth Any Video, a model that tackles the challenge through two key innovations. First, we develop a scalable synthetic data pipeline, capturing real-time video depth data from diverse virtual environments, yielding 40,000 video clips of 5-second duration, each with precise depth annotations. Second, we leverage the powerful priors of generative video diffusion models to handle real-world videos effectively, integrating advanced techniques such as rotary position encoding and flow matching to further enhance flexibility and efficiency. Unlike previous models, which are limited to fixed-length video sequences, our approach introduces a novel mixed-duration training strategy that handles videos of varying lengths and performs robustly across different frame rates—even on single frames. At inference, we propose a depth interpolation method that enables our model to infer high-resolution video depth across sequences of up to 150 frames. Our model outperforms all previous generative depth models in terms of spatial accuracy and temporal consistency. The code and model weights are open-sourced.

---

## 论文详细总结（自动生成）

# 基于可扩展合成数据的任意视频深度估计（Depth Any Video）— 论文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：视频深度估计（video depth estimation）是计算机视觉中的基础任务，广泛应用于三维重建、自动驾驶、机器人导航等场景。
- **核心问题**：该领域长期受限于**一致且可扩展的真值（ground truth）数据匮乏**，导致现有模型在真实视频上的深度估计结果不一致、不可靠。
- **整体含义**：本文旨在通过**大规模合成数据**与**生成式视频扩散模型先验**的结合，突破视频深度估计的数据瓶颈，实现面向任意真实世界视频的高质量、时间一致性深度估计。

## 2. 论文提出的方法论

- **核心思想**：将“可扩展合成数据”与“视频扩散模型生成先验”相结合，解决真实视频深度估计的数据与泛化难题。
- **技术细节一：可扩展合成数据管线**
  - 从多种虚拟环境中**实时采集视频深度数据**。
  - 构建了包含 **40,000 段 5 秒时长视频片段**的数据集，每段均带有**像素级精确深度标注**。
- **技术细节二：生成式视频扩散模型先验**
  - 利用视频扩散模型强大的生成先验来处理真实世界视频，提升模型对真实场景的泛化能力。
  - 引入 **旋转位置编码（rotary position encoding）** 增强模型对视频时序位置信息的建模能力。
  - 引入 **流匹配（flow matching）** 技术提升训练的灵活性与推理效率。
- **技术细节三：混合时长训练策略**
  - 提出 novel 的 **mixed-duration training strategy**，使模型能够处理**可变长度**的视频序列。
  - 在不同帧率下均表现稳健，甚至可处理**单帧图像**（即静态深度估计）。
- **技术细节四：推理时的深度插值方法**
  - 提出一种深度插值方法，使模型能够对长达 **150 帧** 的序列进行**高分辨率视频深度估计**，突破固定长度输入的限制。

## 3. 实验设计

- **训练数据**：自建的 40,000 段 5 秒合成视频数据集，来自多种虚拟环境。
- **测试场景**：真实世界视频（real-world videos），涵盖不同长度与帧率的视频序列。
- **Benchmark**：与所有先前的生成式深度模型（generative depth models）进行对比。
- **对比方法**：所有 prior 的生成式深度估计模型。
- **评估指标**：空间精度（spatial accuracy）与时间一致性（temporal consistency）。
- **说明**：摘要中未列出具体 benchmark 数据集名称（如 NYUv2、KITTI 等）及详细指标数值，需查看全文获取更多细节。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**训练所使用的 GPU 型号、数量、训练时长等具体算力信息。
- 如需了解算力配置，需要查阅论文全文的 implementation details 部分。

## 5. 实验数量与充分性

- **实验组数**：从摘要信息来看，论文至少包含以下实验：
  - 与所有先前生成式深度模型的**整体性能对比**（空间精度 + 时间一致性）。
  - 对不同视频长度与帧率的**鲁棒性验证**（含单帧输入场景）。
  - 对 150 帧长序列的**高分辨率推理效果验证**。
- **充分性评估**：
  - 整体对比实验覆盖了主要同类方法，结论具有参考价值。
  - 但由于摘要未提供消融实验（如对合成数据规模、旋转位置编码、流匹配、混合时长策略进行逐一验证）的明确信息，在判断各技术模块独立贡献方面，需依赖全文内容。
  - 由于未提及具体基准数据集与指标数值，客观定量的公平性需要阅读全文后评估。

## 6. 论文的主要结论与发现

- 提出的 Depth Any Video 模型在**空间精度**和**时间一致性**上均**优于所有先前的生成式深度模型**。
- 可扩展合成数据与生成式视频扩散先验的结合，能够有效解决真实视频深度估计的数据瓶颈问题。
- 混合时长训练策略与深度插值方法使模型突破了固定长度输入的限制，可在多种帧率和长度下灵活运行，大幅提升了模型的实用性与部署灵活性。
- 代码与模型权重已开源，便于后续研究与复现。

## 7. 优点

- **数据创新**：构建了大规模、精确标注的合成视频深度数据集（4 万段 5 秒视频），填补了视频深度领域可扩展真值数据的空白。
- **方法创新**：首次将生成式视频扩散模型先验系统性地引入视频深度估计，结合旋转位置编码与流匹配，兼具性能与效率。
- **灵活性强**：支持任意长度、不同帧率的视频输入，甚至可处理单帧图像，突破以往模型的固定长度限制。
- **长序列能力**：通过深度插值方法支持长达 150 帧的高分辨率视频深度推理，实用价值高。
- **开源贡献**：代码和模型权重开放，可复现性强，利于领域后续研究。

## 8. 不足与局限

- **算力信息缺失**：未说明训练所需 GPU 资源与耗时，难以评估方法的复现成本与资源门槛。
- **实验细节有限**：摘要中未提供具体 benchmark 数据集的名称与详细量化指标，暂无法充分验证其在公开标准数据集上的绝对表现水平。
- **潜在偏差风险**：合成数据虽规模庞大，但虚拟环境与真实世界之间仍存在 domain gap，模型在极具挑战性的真实场景（如极端光照、动态物体遮挡、透明表面等）上的表现需要进一步验证。
- **应用限制**：深度插值方法虽支持 150 帧，但更长的视频序列是否仍能保持时间一致性尚未在摘要中说明；对高分辨率视频的推理效率与显存占用也待评估。
- **创新性评估**：合成数据与扩散先验的结合虽有新意，但各部分（合成数据训练、扩散先验）均为已有技术方向，实际工程价值需在更多下游任务中进行验证。

（完）
