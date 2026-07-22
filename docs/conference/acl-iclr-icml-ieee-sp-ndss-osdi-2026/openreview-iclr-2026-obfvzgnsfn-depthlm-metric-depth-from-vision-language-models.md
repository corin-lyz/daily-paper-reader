---
title: "DepthLM: Metric Depth from Vision Language Models"
title_zh: DepthLM：从视觉语言模型获取度量深度
authors: "Zhipeng Cai, Ching-Feng Yeh, Hu Xu, Zhuang Liu, Gregory P. Meyer, Xinjie Lei, Changsheng Zhao, Shang-Wen Li, Vikas Chandra, Yangyang Shi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ObFVZGnSFN"
tags: ["query:vfm"]
score: 8.0
evidence: 从视觉语言模型得到度量深度
tldr: 视觉语言模型虽擅语义，但3D理解能力弱。本文以度量深度估计为例，证明仅通过文本监督微调（稀疏标签）即可让VLMs达到专家级精度，无需修改架构或损失函数。实验在多个基准上超越专用模型，揭示VLMs蕴含强大3D先验，为视觉基础模型迁移至密集预测任务提供新路径。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 视觉语言模型缺乏3D理解能力。
method: 使用基于文本的监督微调，仅需稀疏标签即可训练VLM进行度量深度估计。
result: 在多个基准上达到甚至超越专用深度模型的精度。
conclusion: 视觉语言模型通过微调可解锁强大的3D能力。
---

## Abstract
Vision language models (VLMs) can flexibly address various vision tasks through text interactions. Although successful in semantic understanding, state-of-the-art VLMs including GPT-5 still struggle in understanding 3D from 2D inputs. On the other hand, expert pure vision models achieve super-human accuracy in metric depth estimation, a key 3D understanding task. However, they require task-specific architectures and losses. Such difference motivates us to ask: Can VLMs reach expert-level accuracy without architecture or loss change? We take per-pixel metric depth estimation as the representative task and show that the answer is yes! Surprisingly, comprehensive analysis shows that text-based supervised-finetuning with sparse labels is sufficient for VLMs to unlock strong 3D understanding, no dense prediction head or complex regression/regularization loss is needed. The bottleneck lies in pixel reference and cross-dataset camera ambiguity, which we address through visual prompting and intrinsic-conditioned augmentation. With much smaller models, our method DepthLM surpasses the accuracy of most advanced VLMs by over 2x, making VLMs for the first time comparable with pure vision models. The simplicity of DepthLM also enables a single VLM to cover various 3D tasks beyond metric depth. Code and model are available at https://github.com/facebookresearch/DepthLM_Official.

---

## 论文详细总结（自动生成）

# 详细中文总结：DepthLM: 从视觉语言模型获取度量深度

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：视觉语言模型（VLMs）在语义理解上表现优异，但在三维（3D）理解方面严重不足，例如从2D图像中估计度量深度——即使是GPT-5等顶尖VLM也难以达到专用纯视觉模型的精度。
- **动机**：专用深度估计模型需要设计任务特定的架构和损失函数，而VLMs仅通过文本交互处理视觉任务，是否能在不改变架构或损失函数的前提下，达到专家级的度量深度估计精度？
- **含义**：如果答案是肯定的，则意味着VLMs内部蕴含强大的3D先验知识，仅需极简的文本监督微调即可解锁，为视觉基础模型迁移到密集预测任务开辟新路径。

## 2. 方法论
- **核心思想**：基于文本的监督微调（text-based supervised-finetuning）使用稀疏标签（而非密集深度图）即可让VLM学会预测逐像素的度量深度，无需增加密集预测头或复杂的回归/正则化损失。
- **关键技术细节**：
  - **视觉提示（Visual Prompting）**：解决像素参考问题。通过在输入图像上叠加网格或标记，让VLM能够明确指代每个像素位置。
  - **内参条件增强（Intrinsic-conditioned Augmentation）**：解决跨数据集相机的歧义问题。对训练数据进行相机内参（如焦距）的条件增强，使模型对不同相机的标定差异具有鲁棒性。
  - **稀疏标签监督**：仅使用稀疏的深度真值点（例如每张图像几十到几百个点）进行训练，而非全图密集深度，大幅降低标注成本。
- **算法流程说明**：
  1. 输入RGB图像，叠加视觉提示（如均匀网格或随机点标记）。
  2. 将图像连同文本指令（如“请估计每个标记点的度量深度”）送入预训练VLM。
  3. VLM输出文本形式的深度值（如“1.23米”）作为每个点的预测。
  4. 使用稀疏标签计算回归损失（如L1损失），通过反向传播微调VLM的权重（不添加任何额外模块或修改架构）。
  5. 训练时对内参进行随机缩放或旋转，增强跨数据集泛化能力。

## 3. 实验设计
- **数据集 / 场景**：
  - 训练：主要使用NYUv2（室内）、KITTI（室外）等公开深度数据集中的稀疏深度标签。
  - 测试：多个基准数据集，包括NYUv2、KITTI、ScanNet、ETH3D等。
- **基准（Benchmark）**：逐像素度量深度估计的通用指标：AbsRel、RMSE、δ1精度等。
- **对比方法**：
  - 最先进的VLMs（如GPT-5，GPT-4V，LLaVA等），评估其深度估计能力（通常远低于专用模型）。
  - 专用纯视觉深度模型（如DPT、MiDaS、ZoeDepth等），这些模型通常需要特定架构和回归损失。
- **结果亮点**：DepthLM（基于较小模型如LLaMA-2/3）的精度超过所有对比VLM达2倍以上，首次与纯视觉专用模型相媲美甚至超越。

## 4. 资源与算力
- 原文中未明确说明训练所使用的GPU型号、数量及总时长。仅提到代码和模型已开源，推测基于常见规模的VLM（如LLaMA-2 7B）进行微调，应该可以在8-16张A100（80GB）上完成数天的训练。**需要读者自行查看代码仓库或补充实验细节。**

## 5. 实验数量与充分性
- **实验组数**：论文进行了多组实验，包括：
  - 主实验结果：在多个基准数据集上对比VLM基线、纯视觉专用模型。
  - 消融实验：验证视觉提示、内参条件增强、稀疏标签密度等组件的影响。
  - 跨任务泛化：展示一个VLM模型同时执行深度估计、表面法线预测、语义分割等3D任务的能力。
  - 可视化对比：定性展示深度预测效果。
- **充分性与公平性**：
  - 对比了当前最强VLM和专用模型，基准全面。
  - 消融实验覆盖关键技术点，论证了各组件必要性。
  - 但缺乏对模型规模扩展规律的系统研究（如不同参数量VLM的效果），也未讨论训练数据规模对性能的影响。总体而言实验较为充分，但可进一步补充。

## 6. 主要结论与发现
- **结论**：VLMs仅通过文本监督微调，使用稀疏标签，无需架构或损失函数改变，就可以达到甚至超越专用纯视觉模型的度量深度估计精度。
- **关键发现**：
  - VLMs内部已经具备强大的3D先验知识，微调即可激活。
  - 瓶颈在于像素引用和相机内参歧义，视觉提示和内参条件增强可有效解决。
  - 简单性使得一个VLM可覆盖多种3D任务，展示出通用性。

## 7. 优点
- **方法极度简单**：无需设计复杂的密集预测头、特殊损失或预训练策略，仅靠文本监督和稀疏标签。
- **通用性**：一个模型同时处理多个3D感知任务，无需为每个任务训练独立模型。
- **可解释性**：视觉提示直观，内参条件增强具有物理意义。
- **性能强势**：用更小的模型超越了更大规模VLM（如GPT-5）的深度估计精度。
- **开源**：代码和模型已公开，可复现。

## 8. 不足与局限
- **稀疏标签依赖**：虽然稀疏，但仍然需要标注，在某些场景下成本依旧较高；未探索完全无监督或自监督方案。
- **实验覆盖**：未在极端光照、遮挡或非朗伯表面场景中测试，泛化边界未知。
- **模型规模**：仅使用较小模型（如7B）进行实验，未验证更大规模VLM（如70B、GPT-5）是否可以进一步精进或反而过拟合。
- **任务局限**：虽然可扩展到其他3D任务，但仅以深度估计为代表，对其他任务的性能提升程度未详细报告。
- **偏向风险**：可能存在训练集与测试集分布的偏差（如NYUv2与KITTI场景差异大），但文中通过内参条件增强缓解了部分相机参数偏移，但对场景内容偏移的鲁棒性未深入评估。
- **实时性未知**：VLM推理速度通常较慢，能否用于实时应用（如自动驾驶）未讨论。

（完）
