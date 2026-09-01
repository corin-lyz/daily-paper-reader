---
title: Predicting High-precision Depth on Low-Precision Devices Using 2D Hilbert Curves
title_zh: 使用二维希尔伯特曲线在低精度设备上预测高精度深度
authors: "Mykhail Uss, Ruslan Yermolenko, Oleksii Shashko, Olena Kolodiazhna, Ivan Safonov, Volodymyr Savin, Yoonjae Yeo, Seowon Ji, Jaeyun Jeong"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=fwQH7M5DRM"
tags: ["query:mono-depth"]
score: 8.0
evidence: 面向资源受限设备的低比特精度深度预测
tldr: 深度神经网络在低端设备上受限于计算复杂度，往往需要低比特量化，但低比特难以表达高动态范围深度。本文提出将高动态范围深度编码为希尔伯特曲线上两个低动态范围分量，并训练全精度网络直接预测这些分量，从而在不增加推理开销的前提下恢复高精度深度，适用于单目和双目深度预测。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 低端设备上低比特深度网络难以表达高动态范围深度，精度受限。
method: 利用二维希尔伯特曲线将高动态范围深度分解为两个低动态范围分量进行预测。
result: 在低比特运行条件下恢复了高精度深度，显著提升低资源设备上的深度预测质量。
conclusion: 为移动端低精度深度估计提供了一种新颖的编码-恢复方案。
---

## Abstract
Dense depth prediction deep neural networks (DNN) have achieved impressive results for both monocular and binocular data but they are limited by high computational complexity, restricting their use on low-end devices. For better on-device efficiency and hardware utilization, weights and activations of the DNN should be converted to low-bit precision. However, this precision is not sufficient for representing high dynamic range depth. In this paper, we aim to overcome this limitation and restore high-precision depth from low-bit precision predictions. To achieve this, we propose to represent high dynamic range depth as two low dynamic range components of a Hilbert curve, and to train the full precision DNN to directly predict the latter. For on-device deployment, we use standard quantization methods and add a post-processing step that reconstructs depth from the Hilbert curve components predicted in low-bit precision. Extensive experiments demonstrate that our method increases bit precision of predicted depth by up to three bits with little computational overhead. We also observe a positive side effect of quantization error reduction by up to five times. Our method enables effective and accurate depth prediction with DNN weights and activations quantized to eight bit precision.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心矛盾**：稠密深度预测 DNN（单目与双目）虽然精度高，但计算复杂度极大，严重限制了其在低端设备（移动端、嵌入式设备）上的部署。
- **低比特量化的困境**：为了在资源受限设备上实现高效推理，模型权重和激活值需量化为低比特（如 8-bit）。然而，低比特精度不足以表达深度估计中的**高动态范围**（高精度、连续、数值范围大的深度值），导致输出深度质量严重下降。
- **论文目标**：在不增加设备端计算开销的前提下，从低比特精度的网络预测中**恢复出高精度深度**，从而兼顾低比特推理的高效性与深度输出的高精度。

## 2. 方法论

- **核心思想**：将高动态范围深度信息重新编码为**两个低动态范围分量**，使每个分量都能被低比特精度有效表达；网络不再直接预测深度值，而是预测这两个分量；推理后通过后处理步骤重建高精度深度。
- **关键技术细节**：
  - 使用 **二维 Hilbert 曲线（2D Hilbert Curves）** 作为编码工具，将深度值映射到二维空间，分解为两个低动态范围分量。
  - 训练时采用**全精度网络**直接回归这两个分量，避免低比特训练带来的梯度不稳定问题。
  - 部署时使用**标准量化方法**（权重和激活量化到低比特），由于两个分量均为低动态范围，低比特表示足够精确。
  - 后处理阶段将预测的两个分量组合，重建出高精度、高动态范围的深度图，其额外计算开销极小。
- **算法流程**：
  1. 离线：对训练数据的高动态范围深度值进行 Hilbert 曲线编码，生成两个低动态范围分量作为监督标签。
  2. 训练：全精度 DNN 以这两个分量为输出目标进行回归训练。
  3. 量化部署：对训练好的模型进行标准低比特量化。
  4. 推理后处理：设备端执行量化模型得到两个低动态范围分量，通过 Hilbert 曲线解码与重建，得到高精度深度。

## 3. 实验设计

- **任务与场景**：覆盖**单目深度估计**（如室内/室外场景）与**双目（立体）深度估计**两类任务。
- **数据集与 Benchmark**：原文摘要未明确列出具体数据集名称，但根据 ICML 相关工作惯例，单目通常涉及 KITTI、NYU-Depth-v2 等；双目通常涉及 Scene Flow、KITTI Stereo 等。
- **对比方法**：未在摘要中详列，合理推断对比了：
  - 全精度稠密深度预测基线模型；
  - 直接对深度输出进行低比特量化的标准方案；
  - 现有的其他低比特部署策略。

## 4. 资源与算力

- **原文未明确说明**所用的 GPU 型号、数量、训练时长等算力信息。
- 摘要中只提到"little computational overhead"（额外计算开销小），但这指的是推理阶段的后处理开销，而非训练资源。
- 由于仅有摘要级别的文本可供分析，训练资源信息属于论文中未披露的内容，这一点应客观指出。

## 5. 实验数量与充分性

- **实验数量**：文中描述为大量实验（"Extensive experiments"），至少涵盖单目与双目两个任务、不同量化配置下的评估。
- **充分性分析**：
  - 在给定信息下，实验覆盖了方法的核心目标——证明低比特条件下深度精度的恢复能力。
  - 证明了两个关键指标：深度比特精度最多提升 **3 bits**、量化误差最多降低 **5 倍**。
  - 但在可获取的内容中，未展示详细的各数据集逐项结果、消融实验对 Hilbert 曲线选择的验证、与更多既有低比特方法的对照等，因此在当前信息范围内无法完全评判实验的全面性与公平性。

## 6. 主要结论与发现

- 提出的 Hilbert 曲线分量编码-恢复方法，能在**权重和激活量化到 8-bit** 的条件下实现高效且准确的深度预测。
- 相比直接低比特预测深度，本方法将输出深度的有效精度提升至多 **3 bits**。
- 额外观察到一个正面效应：深度预测的**量化误差降低了最多 5 倍**。
- 后处理阶段计算开销极小，适合资源受限设备的实际部署。

## 7. 优点

- **思路新颖**：通过二维 Hilbert 曲线将高动态范围深度分解为两个低动态范围分量，巧妙绕开了低比特位数与高动态范围之间的根本矛盾。
- **即插即用**：训练端只需改标签与输出层，部署端仅增加一个轻量后处理步骤，即可适配现有标准量化流程，工程落地价值高。
- **通用性强**：方法同时对单目和双目深度估计有效，不针对特定网络结构设计。
- **实验证明力强**：在精度提升和量化误差两个维度上给出了明确的量化收益，结论清晰。

## 8. 不足与局限

- **信息不完整**：当前可获取内容仅包括摘要层面，缺少方法细节、完整实验表格与实现细节，无法进行更深入的技术审查。
- **数据集覆盖不完全清楚**：文本未列出具体数据集名称、规模及是否涵盖多样化的真实世界场景，泛化能力难以下定论。
- **消融实验未知**：未说明是否验证了 Hilbert 曲线相对其他空间填充曲线（如 Z-order 曲线）的优越性，也不清楚分量分配的比例对结果的影响。
- **适用条件限制**：方法依赖"低动态范围分量可被低比特充分表达"这一前提，对于极端近/远距离或非连续深度分布是否仍然鲁棒，尚缺乏证据。
- **偏差风险**：可能因未与更多近期 SOTA 低比特方法对比，或未在标准的移动端芯片上实测推理延迟与能耗，而存在评价偏差。

（完）
