---
title: "SpikeStereoNet: A Brain-Inspired Framework for Stereo Depth Estimation from Spike Streams"
title_zh: SpikeStereoNet：基于脉冲流的脑启发立体深度估计框架
authors: "Zhuoheng Gao, Yihao Li, Jiyao Zhang, Rui Zhao, Tong Wu, Hao Tang, Zhaofei Yu, Hao Dong, Guozhang Chen, Tiejun Huang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=lPMPFeioCZ"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 基于脉冲相机和脑启发框架的立体深度估计
tldr: 本文针对传统帧相机在快速变化场景中立体深度估计困难的问题，提出了一种脑启发的SpikeStereoNet框架，直接从原始脉冲流估计立体深度。该模型融合两视角脉冲流，并通过递归脉冲神经网络迭代优化深度估计。同时构建了大规模合成和真实脉冲流数据集。实验表明该方法在高速场景下优于传统方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 传统帧相机在高速场景下立体深度估计困难，脉冲相机提供新途径但缺乏专用算法。
method: 提出基于递归脉冲神经网络的立体深度估计框架，直接处理原始脉冲流。
result: 在高速场景数据集上取得优于传统方法的深度估计精度。
conclusion: SpikeStereoNet为脉冲相机立体深度估计提供了有效方案。
---

## Abstract
Conventional frame-based cameras often struggle with stereo depth estimation in rapidly changing scenes. In contrast, bio-inspired spike cameras emit asynchronous events at microsecond-level resolution, providing an alternative sensing modality. However, existing methods lack specialized stereo algorithms and benchmarks tailored to the spike data. To address this gap, we propose SpikeStereoNet, a brain-inspired framework to estimate stereo depth directly from raw spike streams. The model fuses raw spike streams from two viewpoints and iteratively refines depth estimation through a recurrent spiking neural network (RSNN) update module. To benchmark our approach, we introduce a large-scale synthetic spike stream dataset and a real-world stereo spike dataset with dense depth annotations. SpikeStereoNet outperforms existing methods on both datasets by leveraging spike streams' ability to capture subtle edges and intensity shifts in challenging regions such as textureless surfaces and extreme lighting conditions. Furthermore, our framework exhibits strong data efficiency, maintaining high accuracy even with substantially reduced training data.

---

## 论文详细总结（自动生成）

# SpikeStereoNet 论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：传统基于帧的相机在快速变化场景中（如高速运动、微秒级变化）进行立体深度估计非常困难，容易产生运动模糊、曝光不足等问题。
- **背景**：生物启发的脉冲相机（spike camera）以微秒级分辨率异步发射脉冲事件，能够捕捉高速动态场景的细节，为立体深度估计提供了新的传感模态。然而，目前缺乏专门针对脉冲流的立体深度估计算法和基准数据集。
- **目标**：提出 **SpikeStereoNet**，一种脑启发的框架，直接从原始脉冲流估计立体深度，填补该领域的方法和基准空白。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用递归脉冲神经网络（RSNN）处理原始脉冲流，通过迭代更新模块逐步精细化深度估计，同时融合两个视角的脉冲信息。
- **关键技术细节**：
  - **输入**：直接接收左右两个视角的原始脉冲流，无需转换为传统帧或事件。
  - **特征融合**：将两路脉冲流通过时空特征提取网络融合，保留微秒级的时间分辨率和边缘信息。
  - **递归更新模块**：采用递归脉冲神经网络，通过时间步递归迭代，不断修正深度图，类似于传统立体匹配中的代价聚合与优化过程。
  - **损失函数**：文中未明确，但通常使用深度误差度量（如 L1 或 Huber loss）。
- **公式/算法流程**（文字说明）：
  1. 左右脉冲流同时输入特征提取网络，获得多尺度时空特征。
  2. 构建初始视差代价体。
  3. 通过递归脉冲神经网络（RSNN）对代价体进行多次迭代优化，每次迭代结合前一时刻的深度估计和当前脉冲特征。
  4. 输出最终视差图或深度图。

## 3. 实验设计
- **数据集**：
  - **大规模合成脉冲流数据集**：基于虚拟场景渲染的脉冲流，具有密集深度标注。
  - **真实世界立体脉冲数据集**：使用真实的脉冲相机采集，并标定密集深度真值（可能通过激光雷达或其他传感器）。
- **Benchmark**：在两个数据集上评估深度估计精度（如端点误差、像素精度等），并与现有方法对比。
- **对比方法**：摘要未列出具体方法名称，但提到“existing methods”，可能包括传统立体匹配算法、基于事件的立体方法、以及由帧到深度的深度学习模型。对比结果显示 SpikeStereoNet 在两个数据集上均优于这些方法。

## 4. 资源与算力
- **信息缺失**：论文摘要及元数据中**未提及**使用的 GPU 型号、数量、训练时长或硬件配置。无法评估所需算力。

## 5. 实验数量与充分性
- **实验数量**：仅在两个数据集上进行了主要性能对比，未提及消融实验、模块分析、超参数敏感性等详细实验。
- **充分性**：由于信息有限，无法判断实验是否充分。但摘要指出模型在无纹理表面和极端光照下表现更好，且数据效率高（减少训练数据仍保持高精度），暗示可能进行了数据量缩减实验。缺乏消融实验细节，实验覆盖度偏低。

## 6. 主要结论与发现
- **性能优势**：在高速场景下，SpikeStereoNet 的深度估计精度明显优于基于帧的传统方法和已有脉冲处理方法。
- **场景鲁棒性**：脉冲流能捕获传统帧难以记录的细微边缘和强度变化，在纹理缺失和极端光照区域仍有较好表现。
- **数据效率**：使用较少训练数据即可达到高精度，表明模型泛化能力强。

## 7. 优点（亮点）
- **脑启发框架**：直接处理原始脉冲流，避免转换损失时间信息，符合生物视觉处理机制。
- **首个专用基准**：构建了大规模合成和真实脉冲立体数据集，填补了该领域的数据空白，便于后续研究。
- **递归脉冲神经网络设计**：迭代优化策略更接近人类立体视觉的感知-调整过程，且脉冲神经元低功耗特性适合未来硬件实现。
- **数据效率高**：降低了对大规模标注数据的依赖，有利于实际应用。

## 8. 不足与局限
- **实验覆盖不足**：缺少对模型不同组件的消融实验、计算效率分析、实时性评估等。
- **资源信息缺失**：未提供训练和推理的算力需求，难以判断部署可行性。
- **应用限制**：仅针对立体深度估计，未推广到多视图或运动恢复结构；脉冲相机硬件尚未普及，实际应用场景受限。
- **偏差风险**：真实数据集可能仅在受控环境下采集，泛化到野外极端噪声场景尚需验证。
- **对比方法可能偏少**：未列出详细对比方法与 baseline，无法确认比较的公平性与全面性。

（完）
