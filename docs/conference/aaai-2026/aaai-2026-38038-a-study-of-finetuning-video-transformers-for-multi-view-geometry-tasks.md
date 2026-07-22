---
title: A Study of Finetuning Video Transformers for Multi-view Geometry Tasks
title_zh: 微调视频Transformer用于多视图几何任务的研究
authors: "Huimin Wu, Kwang-Ting Cheng, Stephen Lin, Zhirong Wu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38038/42000"
tags: ["query:vfm"]
score: 4.0
evidence: 微调视频基础模型用于多视图几何任务
tldr: 研究视觉Transformer微调用于光流等多视图几何任务，发现通用视频预训练模型可少量适应即可迁移至几何任务，为视觉基础模型用于密集预测任务提供参考。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38038/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1638, \"height\": 678, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38038/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1657, \"height\": 630, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 870, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 841, \"height\": 322, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 587, \"height\": 216, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 586, \"height\": 288, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 882, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 878, \"height\": 849, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 877, \"height\": 848, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 877, \"height\": 389, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38038/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 897, \"height\": 781, \"label\": \"Table\"}]"
motivation: 现有光流等方法需要定制架构和任务专用预训练，成本高昂。
method: 在视频基础模型后添加线性解码器，并迭代微调进行几何推理。
result: 仅使用简单架构即可达到与最先进方法相当的性能，迁移效率高。
conclusion: 通用视频预训练模型可作为多视图几何任务的有效基础，简化流程。
---

## Abstract
This paper presents an investigation of vision transformer learning for multi-view geometry tasks, such as optical flow estimation, by fine-tuning video foundation models. Unlike previous methods that involve custom architectural designs and task-specific pretraining, our research finds that general-purpose models pretrained on videos can be readily transferred to multi-view problems with minimal adaptation. The core insight is that general-purpose attention between patches learns temporal and spatial information for geometric reasoning. We demonstrate that appending a linear decoder to the Transformer backbone produces satisfactory results, and iterative refinement can further elevate performance to state-of-the-art levels. This conceptually simple approach achieves top cross-dataset generalization results for optical flow estimation with end-point error (EPE) of 0.69, 1.78, and 3.15 on the Sintel clean, Sintel final, and KITTI datasets, respectively.  Our method additionally establishes a new record on the online test benchmark with EPE values of 0.79, 1.88, and F1 value of 3.79. Applications to 3D depth estimation and stereo matching also show strong performance, illustrating the versatility of video-pretrained models in addressing geometric vision tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：多视图几何任务（如光流估计、立体匹配、深度估计）传统上依赖复杂的定制架构和任务专用预训练，例如光流主流方法包含卷积茎、Transformer成本体积、循环神经网络迭代细化等复杂流水线。现有研究（如CroCo）尝试简化但依赖特定的预训练任务和精心收集的数据集，缺乏通用性。
- **核心问题**：能否利用通用视频基础模型（预训练于视频数据），仅通过少量微调就高效迁移到多视图几何任务，从而简化流程、提升泛化能力？
- **整体含义**：探索视频预训练Transformer中跨帧自注意力机制是否已蕴含几何匹配所需的时间与空间信息，验证通用模型在低层几何推理上的潜力。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 直觉：视频预训练Transformer中的跨帧自注意力不仅能捕获高级语义，还能学到丰富的对应关系，适用于细粒度几何匹配。
- 方法：采用通用视频基础模型作为编码器，以最小化修改（线性解码器或迭代细化）适应下游几何任务，命名为**GeoViT**。

### 关键技术细节

**（1）预训练视频基础模型的适配**
- 将预训练的3D ViT（如MAE st）调整为两帧输入任务。
- 空间位置编码：插值到所需输入尺寸。
- 时间位置编码：预训练时通常为8帧，将其分为两半，各半平均后分别对应源图像和目标图像。
- 3D patch embedding：沿时间维度求和，转换为2D patch embedding。
- 最终token表示为：2D patch embedding + 2D位置编码 + 时间编码。

**（2）简单线性解码**
- 直接在每个输出patch后接线性层，回归几何属性（如光流）。
- 无需复杂成本体积或专门设计，即可取得不错性能（Sintel final EPE 2.0，优于当时SOTA SAMFlow 2.11）。

**（3）迭代细化解码（核心改进）**
- 受RAFT启发，将预测分解为残差序列，通过循环迭代逐步修正。
- 公式：  
  \(\Delta g_t = F_{dec}(F_{enc}(I_1, \text{warp}(I_2, g_{t-1})), g_{t-1})\)  
  \(g_t = g_{t-1} + \Delta g_t\)
- **关键创新**：去除成本体积，直接用图像扭曲（warping）代替。解码器采用ConvGRU。
- 损失函数：加权L1损失，后期预测权重更高（\(\gamma^{T-t}\)，\(\gamma=0.9\)）。
- 对于深度估计：将深度转换为像素位移，使用相机参数，再转换回深度。

### 算法流程（文字说明）
1. 输入源图像 \(I_1\) 和目标图像 \(I_2\)。
2. 对目标图像基于上一预测 \(g_{t-1}\) 进行扭曲，使输入对对应残差。
3. 将源图像和扭曲后目标图像送入预训练3D ViT编码器提取特征。
4. 解码器（ConvGRU）接收源图像特征和当前预测，输出残差 \(\Delta g_t\)。
5. 累加得到新预测 \(g_t\)。
6. 重复步骤2-5共T次（默认6次）。

## 3. 实验设计：数据集、benchmark、对比方法

### 光流估计
- **训练数据**：FlyingChairs（Chairs）→ FlyingThings（Things）→ 联合微调（Sintel、KITTI-2015、HD1K）。
- **评估数据集**：Sintel（clean/final）、KITTI-2015（训练集和测试集）。
- **基准**：Sintel benchmark、KITTI benchmark（在线服务器）。
- **对比方法**：RAFT、GMA、FlowFormer、FlowFormer++、SAMFlow、DPFlow等20余种SOTA。

### 立体匹配
- **训练数据**：Scene Flow → 联合数据集（Scene Flow + TartanAir + Sintel Stereo + CREStereo + InStereo2K + ETH3D）。
- **评估数据集**：ETH3D Stereo测试集。
- **指标**：bad 1.0, bad 2.0, bad 4.0。
- **对比方法**：GANet、AANet、CFNet、RAFT-Stereo、CREStereo、GMStereo、MonSter。

### 深度估计
- **训练数据**：RGBD-SLAM + SUN3D + Scenes11联合训练。
- **评估数据集**：DeMoN测试集（分别测试RGBD-SLAM、SUN3D、Scenes11）。
- **指标**：Abs Rel, Sq Rel, RMSE, RMSE log。
- **对比方法**：DeMoN、DeepMVS、DPSNet、IIB、GMDepth。

## 4. 资源与算力

论文明确提到：**“our largest model can be trained on just 8 V100 GPUs for at most 2 weeks using techniques such as gradient checkpointing.”**  
具体训练阶段：
- 光流“C+T”：Chairs 40K步（batch size 8, 368×496），Things 400K步（batch size 8, 384×768）。
- 后续微调：联合数据集200K步（batch size 8, 288×960），Sintel微调5K步（batch size 8, 416×1024）。
- 立体匹配：两阶段各100K步（batch size 8）。
- 深度估计：100K步（batch size 8, 448×576）。
未说明每个阶段的具体GPU数及时长，但总体资源合理。

## 5. 实验数量与充分性

- **实验数量**：共包含：
  - 光流跨数据集泛化实验（表1）。
  - 光流在线benchmark（表3、表4）。
  - 消融实验：预训练方案对比（表2a）、迭代步数（表2b）、模型尺寸（表2c）、解码器设计（表2d）。
  - 立体匹配benchmark（表5）。
  - 深度估计三个数据集（表6）。
  - 定性结果对比（图2）。
- **充分性判断**：
  - 消融覆盖全面：预训练模型、尺寸、迭代步数、解码方式均被分析。
  - 对比方法广泛，涵盖主流和最新SOTA。
  - 跨任务验证（光流、立体、深度），体现通用性。
  - 使用标准benchmark在线评估，减少过拟合偏差。
  - **客观公平**：尽管未控制所有变量（如数据量、模型大小差异），但论文明确承认“fair comparisons is challenging”，并主要强调概念简单性。整体实验设计合理、结果可信。

## 6. 论文的主要结论与发现

- **核心发现**：通用视频预训练Transformer（尤其MAE st）可被简单微调后高效处理多视图几何任务，性能超越复杂定制方法。
- **关键优势**：
  - 光流：在Sintel clean/final和KITTI上取得SOTA，EPE分别降低21%、9.6%，F1降低11.2%。
  - 立体匹配：在ETH3D上bad 2.0和bad 4.0最优。
  - 深度估计：在SUN3D上3/4指标最优，在RGBD-SLAM和Scenes11上也有竞争力。
- **消融揭示**：
  - 视频预训练（MAE st）优于纯图像预训练（MAE）和强调语义的模型（MVD、InternVideo、UMT），说明时空对应线索对低层几何任务更重要。
  - 模型越大效果越好。
  - 迭代6步足够，更多步数收益边际递减。
  - 图像扭曲法优于成本体积查询法。

## 7. 优点：方法或实验设计上的亮点

1. **概念简单性**：直接利用通用视频预训练模型，仅添加线性头或轻量ConvGRU，无需成本体积等复杂组件。
2. **跨任务统一性**：同一架构适用于光流、立体匹配、深度估计，且均达SOTA或可比性能。
3. **强泛化能力**：跨数据集泛化（C+T直接测Sintel/KITTI）优于以往方法。
4. **实验全面**：包含多任务、多消融、在线benchmark验证，结论稳健。
5. **资源效率**：最大模型仅需8张V100训练2周，相比一些大规模预训练模型更可行。

## 8. 不足与局限

1. **实验覆盖**：
   - 只测试了MAE st、MAE、MVD、InternVideo、UMT等，未覆盖其他视频基础模型（如VideoMAE v2、TimeSformer等）。
   - 深度估计仅在DeMoN数据集上评估，未在更复杂的真实场景（如NYUv2、KITTI深度）测试。
2. **偏差风险**：
   - 训练数据多为合成数据集（Chairs、Things、Scene Flow），真实场景泛化性可能受限。
   - 迭代细化中图像扭曲假设几何变换可微，对于大位移或遮挡区域可能不准确。
3. **应用限制**：
   - 方法依赖预训练模型，若预训练数据领域差异大，效果可能下降。
   - 迭代推理增加计算成本（虽已控制，但实时应用仍需优化）。
4. **公平性**：论文承认“controlling factors such as data size and model size for fair comparisons is challenging”，未统一所有对比方法的数据使用量。
5. **缺少理论分析**：未深入解释视频预训练为何能学到几何对应，更多是经验验证。

（完）
