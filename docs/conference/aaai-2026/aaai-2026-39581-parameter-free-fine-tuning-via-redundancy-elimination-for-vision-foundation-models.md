---
title: Parameter-Free Fine-tuning via Redundancy Elimination for Vision Foundation Models
title_zh: 通过冗余消除的视觉基础模型无参数微调
authors: "Jiahuan Long, Tingsong Jiang, Wen Yao, Yizhe Xiong, Zhengqin Xu, Shuai Jia, Hanqing Liu, Chao Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39581/43542"
tags: ["query:vfm"]
score: 9.0
evidence: 视觉基础模型的无参数微调方法，可迁移到密集预测任务
tldr: 现有视觉基础模型微调需要更新大量参数，即使参数高效方法仍需修改数千权重。本文发现SAM中存在冗余通道，提出无参数微调方法，通过基于模的通道选择算法挑选并重用预训练特征，无需任何参数更新即可适配下游任务。在多个分割和密集预测数据集上，该方法不仅保持甚至超越传统微调性能，为VFM的高效迁移提供了全新思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39581/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 837, \"height\": 675, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39581/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1848, \"height\": 576, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39581/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1746, \"height\": 596, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39581/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 886, \"height\": 320, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39581/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1808, \"height\": 828, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39581/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 867, \"height\": 156, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39581/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1778, \"height\": 670, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39581/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1812, \"height\": 737, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39581/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 704, \"height\": 305, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39581/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 792, \"height\": 263, \"label\": \"Table\"}]"
motivation: 视觉基础模型微调通常需大量参数更新，计算成本高。
method: 提出无需参数更新的微调方法，利用通道选择算法消除冗余，重用预训练特征。
result: 在多个下游任务上性能持平或超越传统微调，且无需更新参数。
conclusion: 实现了视觉基础模型的高效零成本迁移，适用于密集预测任务。
---

## Abstract
Vision foundation models (VFMs) have demonstrated remarkable capabilities in learning universal visual representations. However, adapting these models to downstream tasks conventionally requires parameter updates, with even parameter-efficient fine-tuning methods necessitating the modification of thousands to millions of weights. In this paper, we investigate the redundancies in the segment anything model (SAM) and then propose a novel parameter-free fine-tuning method. Unlike traditional fine-tuning methods that adjust parameters, our method emphasizes selecting, reusing, and enhancing pre-trained features, offering a new perspective on fine-tuning foundation models. Specifically, we introduce a channel selection algorithm based on the model's output difference to identify redundant and effective channels. By selectively replacing the redundant channels with more effective ones, we filter out less useful features and reuse more task-irrelevant features to downstream tasks, thereby enhancing the task-specific feature representation. Experiments on both out-of-domain and in-domain datasets demonstrate the efficiency and effectiveness of our method in different vision tasks (e.g., image segmentation, depth estimation and image classification). Notably, our approach can seamlessly integrate with existing fine-tuning strategies (e.g., LoRA, Adapter), further boosting the performance of already fine-tuned models. Moreover, since our channel selection involves only model inference, our method significantly reduces GPU memory overhead.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：视觉基础模型（VFMs，如SAM、SAM2、DINOv2）在下游任务适配时，通常需要更新大量参数（即使是参数高效微调PEFT，也需修改数千至数百万权重），计算成本和内存开销高。
- **动机**：作者发现SAM中部分特征通道对下游任务是冗余甚至有害的——实验中手动置零某些通道后性能不降反升（如PerSeg数据集上通道216置零后mIoU从50.6提升至52.7），说明存在显著特征冗余。
- **整体含义**：提出一种**全新视角**——不更新任何参数，而是通过**选择、重用、增强**预训练特征来适配下游任务，实现“无参数微调”。

## 2. 方法论：核心思想、关键技术细节与算法流程
### 核心思想
- 基于“输出差异”识别冗余通道和有效通道，将冗余通道的特征替换为有效通道的特征（成对替换），从而增强任务相关表示。

### 关键技术细节
1. **通道选择算法**：
   - 定义一个替换对组合 \(P = \{(i,j)_1, (i,j)_2, \ldots, (i,j)_k\}\)，表示将第 \(i\) 通道的特征替换为第 \(j\) 通道的特征。
   - 目标函数：\(P^* = \arg\max_P \text{mIoU}(S, P)\)，即最大化下游数据集 \(S\) 上的mIoU。
2. **降低搜索开销的三个策略**：
   - **基于输出差异筛选**：对每个可能的替换对 \((i,j)\)，计算替换后输出相对于原始输出的准确率差异 \(\Delta\text{Acc}_{i \to j}\)，存储为字典 \(D\)；只保留Top \(N\) 个替换对（文中 \(N=10\)），构造 \(D_{\text{topN}}\)，然后枚举 \(D_{\text{topN}}\) 中所有非空组合，找到最优组合 \(P^*\)。此举将推理次数从 \(2^{C^2}\) 降至 \(C^2 + 2^N - 1\)。
   - **样本缩减**：仅从训练集中随机选取50张图像作为“搜索数据集”。
   - **特征存储**：预存编码器输出的特征图，后续修改特征后直接传入解码器，避免重复编码。
3. **实际推理**：根据 \(P^*\) 对每个图像的特征图做通道替换，其余模型参数完全不变。

## 3. 实验设计
### 数据集与场景
- **图像分割**（9个数据集）：
  - 自然图像：COCO、VOC2012、PerSeg
  - 医学图像：ISIC16、BUSI、KVASIR
  - 伪装检测：CAMO、COD10K、CHAME
- **深度估计**：NYUv2（使用DINOv2）
- **图像分类**：CIFAR-10（使用DINOv2）

### Benchmark与对比方法
- **基线模型**：SAM（ViT-B/L/H）、SAM2（Hiera-T/S/B+/L）、DINOv2（ViT-S/B）
- **对比方法**（8种）：
  - 部分参数微调：Decoder-only、Encoder-only、MedSAM
  - 参数高效微调：SAMed、SAM-COBOT、SAM-Adapter、SAM-PARSER、DoRA
- 主要采用mIoU作为分割指标，深度估计采用MSE、AbsRel、δ1，分类采用准确率。

## 4. 资源与算力
- **硬件**：4张NVIDIA RTX 4090 GPU
- **训练配置**：25个epoch，Adam优化器，初始学习率 \(1 \times 10^{-4}\)，权重衰减 \(5 \times 10^{-5}\)，batch size = 1（每张卡? 实际四卡可能并行，但未明确说明）
- **搜索过程**：仅用50张图像，只进行推理，无需反向传播
- **GPU内存**：本方法仅需11.1 GB（ViT-B, 1024×1024, batch size=4），远低于其他方法（如Decoder-only需13.7 GB，Encoder-only需34.6 GB）
- **注**：论文未给出具体训练或搜索总时长。

## 5. 实验数量与充分性
- **实验数量**：丰富且系统。
  - 主表（表2）：SAM/SAM2各backbone在9个分割数据集上的性能，共7个模型×9数据集 = 63组结果。
  - 增强已有方法（表3）：将本方法应用于8种微调策略，共8×9=72组结果（部分缺失除外）。
  - 消融实验（图4）：在不同替换对数量下测试性能（COCO、CAMO、KVASIR）。
  - 计算开销对比（表4）：5种方法（加上本方法）比较GPU内存和参数量。
  - 扩展任务（表5）：深度估计和分类共4组结果。
  - 定性结果（图3）：多场景可视化比较。
- **充分性与公平性**：覆盖自然、医学、伪装三大领域，多种backbone和多种微调基线，消融探究关键超参数，对比方法均使用公开标准实现。结论可靠。
- **潜在偏差**：搜索数据集仅50张图像，可能对极端小样本或长尾分布不够鲁棒；但此设计出于效率考量。

## 6. 主要结论与发现
1. **有效性**：本方法在所有SAM/SAM2 backbone上均提升分割mIoU（平均提升+5.14~+11.46），尤其在自然图像（如VOC提升+17.45）上效果显著。
2. **即插即用**：可无缝集成到已有微调模型中，进一步提升性能（如SAM-PARSER +4.43 mIoU，SAMed +1.16 mIoU）。
3. **特征分析**：有效通道特征具有清晰边缘和结构，冗余通道特征模糊、噪声大；部分通道具有跨数据集一致性。
4. **通用性**：不仅适用于分割，在深度估计（NYUv2）和图像分类（CIFAR）上也有效，且无需修改任务特定头。
5. **低开销**：完全无参数，仅需推理，GPU内存远低于其他微调方法。

## 7. 优点
- **创新性**：首次提出“无参数微调”范式，仅通过通道替换即可实现适配，思路新颖。
- **简洁有效**：算法直观，无需修改模型结构、无需训练参数，实现简单。
- **兼容性强**：可与任何微调方法叠加使用，进一步提升性能。
- **计算效率**：搜索过程只需推理，无反向传播，GPU内存极低。
- **实验充分**：覆盖多种VFM、多种任务、多种数据集，验证了泛化能力。

## 8. 不足与局限
- **搜索依赖小样本**：搜索数据集仅50张图像，若下游数据集极度不平衡或样本量极小，搜索质量可能下降。
- **未验证其他VFM**：仅测试SAM、SAM2、DINOv2（属于ViT-like），未应用于CLIP、DALL-E等生成模型或更大规模VFM。
- **通道独立性假设**：替换策略假设各通道独立，未建模通道间复杂交互；理论上可能存在更优的非独立替换。
- **任务范围有限**：虽然扩展了深度估计和分类，但主要实验集中于分割，更广泛的下游任务（如检测、VQA）尚未验证。
- **搜索开销**：虽然已大幅降低，但 \(C^2 + 2^N\) 次推理在通道数极大（如1024）时仍可能耗时；未讨论实际所需时间。
- **缺乏理论分析**：为何某些通道跨数据集一致有效/冗余？缺乏深入的理论解释。

（完）
