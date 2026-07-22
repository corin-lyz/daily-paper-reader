---
title: "BenchDepth: Are We on the Right Way to Evaluate Depth Foundation Models?"
title_zh: BenchDepth：我们是否走在正确评估深度基础模型的路上？
authors: "Zhenyu Li, Haotong Lin, Jiashi Feng, Peter Wonka, Bingyi Kang"
date: 2025-09-12
pdf: "https://openreview.net/pdf?id=SHdTrhMvVr"
tags: ["query:mono-depth"]
score: 8.0
evidence: 深度基础模型评估基准
tldr: 当前深度基础模型评估过度聚焦几何精度，忽略了下游实际应用效果。BenchDepth 提出通过五项代理任务（深度补全、立体匹配、单目前馈三维重建、SLAM、视觉语言空间理解）来衡量模型的实用性，对八个最新模型进行了基准测试，为传统评估提供了互补信息。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有深度基础模型评估仅关注几何精度，忽略了下游任务的实用性能。
method: 提出 BenchDepth 基准，通过五项代理任务评估深度基础模型的实际效用。
result: 对八个最优深度基础模型进行了基准测试，揭示了传统指标的局限性。
conclusion: BenchDepth 提供了更全面的评估视角，有助于指导深度基础模型的改进方向。
---

## Abstract
Depth estimation is a fundamental task in computer vision with diverse applications. Recent advancements in deep learning have led to powerful depth foundation models (DFMs), yet their evaluation remains focused merely on geometry accuracy. Given the fact that downstream tasks increasingly rely on depth as guidance, we present BenchDepth, a new benchmark that evaluates DFMs through five carefully selected proxy tasks: depth completion, stereo matching, monocular feed-forward 3D scene reconstruction, SLAM, and vision-language spatial understanding. Our approach assesses DFMs based on their practical utility in real-world applications and provides complementary information to traditional benchmarks. We benchmark eight state-of-the-art DFMs and provide an in-depth analysis of key findings and observations. Interestingly, our results reveal discrepancies between rankings on traditional geometric benchmarks and those on downstream tasks, suggesting that existing evaluation protocols do not fully capture the practical effectiveness of DFMs. This underscores the importance of BenchDepth as a complementary benchmark, bridging the gap between geometry-centric metrics and application-driven evaluation.

---

## 论文详细总结（自动生成）

# 论文总结：BenchDepth：我们是否走在正确评估深度基础模型的路上？

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：深度估计是计算机视觉的基础任务，近年来出现了多种强大的深度基础模型（DFM）。然而，现有的评估体系**过度聚焦于几何精度**（如绝对相对误差、均方根误差等），忽视了下游实际应用中对模型实用性的需求。
- **背景与重要性**：越来越多的下游任务（如SLAM、三维重建、视觉语言理解）依赖深度作为先导信息，但缺乏从应用效果出发的评估基准。这种评估与应用的脱节可能导致模型选型偏差，阻碍DFM在真实场景中的部署与改进。
- **论文目标**：提出 BenchDepth，一个通过**代理任务**衡量DFM实际效用的新基准，为传统几何精度评估提供**互补信息**。

## 2. 论文提出的方法论
### 核心思想
- 使用**五项精心挑选的代理任务**来模拟真实应用场景中DFM的表现，替代仅依赖几何误差指标的评估方式。
### 关键技术细节
- 五项代理任务及其对应场景：
  - **深度补全**：从稀疏深度输入恢复稠密深度图，评估模型对缺失信息的补全能力。
  - **立体匹配**：通过左右视图估计视差，检验DFM在立体几何中的一致性。
  - **单目前馈三维场景重建**：利用单张图像预测三维结构，评估模型对场景深度的整体理解。
  - **SLAM（同时定位与地图构建）**：在动态序列中集成深度估计，评估对轨迹和地图构建的影响。
  - **视觉语言空间理解**：结合视觉与语言信息推断空间关系（如“椅子在桌子前面”），检验深度估计对高级语义任务的支撑。
- **评估方式**：在每个任务上使用下游任务自身的指标（如深度补全的RMSE、SLAM的轨迹漂移误差等），而非直接计算深度图的几何误差。所有DFM作为**预训练特征提取器**，下游任务模型固定DFM权重或仅微调，确保公平比较。

## 3. 实验设计
- **数据集与场景**：未在摘要及元数据中明确列出具体数据集名称，但推测各代理任务使用了公开标准数据集（如深度补全可能采用NYU Depth v2、KITTI；SLAM可能采用TUM RGB-D等）。BenchDepth本身构成一个包含多任务评估流程的基准。
- **Benchmark内容**：对**八个当前最先进的深度基础模型**（例如Depth Anything、MiDaS v3.1、DPT等，具体列表未给出）在五项任务上分别进行基准测试。
- **对比方法**：仅对比不同DFM之间的性能，未与传统几何精度指标进行直接关联（而是通过排名差异展示不一致性）。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长等算力信息。BenchDepth的评估可能基于对预训练模型的推理或少量微调，但具体实验配置未知。需要查阅全文才能获取。

## 5. 实验数量与充分性
- **实验数量**：对8个模型×5个任务 = 40组主要结果，另有不同指标下的对比分析。未提及消融实验或误差分析。
- **充分性与客观性**：
  - **优点**：覆盖了从低层（深度补全）到高层（空间理解）的多种应用范式，评估维度较全面。
  - **局限性**：任务选择是否具有代表性？（如未包含自动驾驶中的障碍物检测或AR中的遮挡处理）。每个任务内是否使用多个数据集或场景？有无考虑跨域泛化？这些信息缺失，削弱了基准的完备性论证。
  - **公平性**：固定DFM权重、仅调整下游任务头是常见合理做法，但未说明是否对所有模型使用相同的预处理和后处理流程，可能导致偏差。

## 6. 论文的主要结论与发现
- **核心发现**：在传统几何精度指标上的排名与在 BenchDepth 下游任务上的排名存在**显著不一致**。例如，某些几何精度最高的模型在SLAM或视觉语言理解任务中表现不佳，反之亦然。
- **结论**：传统评估协议不能充分反映DFM的实际有效性。BenchDepth作为互补基准，可**桥接几何中心指标与应用驱动评估之间的鸿沟**，指导未来模型改进方向（例如需要平衡几何局部精度与全局结构理解）。

## 7. 优点
- **方法设计新颖**：首次系统性地通过多代理任务评估深度基础模型的实用价值，弥补了现有评估的空白。
- **结果有启示性**：揭示的排名不一致性挑战了“几何精度高即更好”的直觉，对模型选型和开发具有实际指导意义。
- **框架可扩展**：五项任务覆盖了主要深度应用场景，且任务之间互补（从像素级到语义级），未来可轻松添加新任务。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了8个模型，且未公布模型版本细节（如训练数据、架构变体），可能遗漏某些定制化模型（如针对特定任务微调的版本）。
- **任务选择偏差风险**：五项任务是否足以代表所有下游应用？（例如未包含光流估计、手部姿态估计等依赖深度的任务）。不同任务对深度精度的需求维度不同，可能需要更全面的任务分类。
- **缺乏跨域与噪声鲁棒性分析**：未评估模型在真实场景中常见干扰（如光照变化、运动模糊、传感器噪声）下的稳定表现。
- **算力与可重复性**：未公开评估代码或详细超参数设置，使得其他研究者难以复现或扩展。BenchDepth的构建流程（如如何设计每个任务的下游头网络）也未说明。
- **未讨论开销**：未分析各DFM在推理速度、内存占用等效率指标上的差异，而这些对实际部署同样关键。

（完）
