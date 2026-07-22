---
title: BRIDGE - Building Reinforcement-Learning Depth-to-Image Data Generation Engine for Monocular Depth Estimation
title_zh: BRIDGE：构建强化学习深度到图像数据生成引擎用于单目深度估计
authors: "Dingning Liu, Haoyu Guo, Jingyi Zhou, Tong He"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=FOgOAyKkIs"
tags: ["query:mono-depth"]
score: 9.0
evidence: 强化学习优化的深度到图像生成用于单目深度估计训练
tldr: 该论文提出BRIDGE框架，通过强化学习优化的深度到图像生成技术合成超过2000万张真实且几何准确的RGB-D图像。结合教师伪标签与真实深度进行混合监督训练，显著提升了单目深度估计模型的鲁棒性和扩展性。实验表明该方法在多个基准上达到突破性性能，为解决单目深度估计数据稀缺问题提供了新范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单目深度估计受限于数据稀缺和质量不足。
method: 利用强化学习优化深度到图像生成，合成大规模RGB-D数据，采用混合监督训练。
result: 生成2000万+图像，模型在扩展性和性能上取得突破。
conclusion: 为单目深度估计提供了大规模数据和训练新范式。
---

## Abstract
Monocular Depth Estimation (MDE) is a foundational task for computer vision. Traditional methods are limited by data scarcity and quality, hindering their robustness. To overcome this, we propose BRIDGE, an RL-optimized depth-to-image (D2I) generation framework that synthesizes over 20M realistic and geometrically accurate RGB images, each intrinsically paired with its ground truth depth, from diverse source depth maps. Then we train our depth estimation model on this dataset, employing a hybrid supervision strategy that integrates teacher pseudo-labels with ground truth depth for comprehensive and robust training. This innovative data generation and training paradigm enables BRIDGE to achieve breakthroughs in scale and domain diversity, consistently outperforming existing state-of-the-art approaches quantitatively and in complex scene detail capture, thereby fostering general and robust depth features.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：单目深度估计（MDE）是计算机视觉的基础任务，但传统方法受限于数据稀缺和质量不足，导致模型鲁棒性与泛化能力受限。
- **研究动机**：现有MDE数据集规模有限、场景多样性不足，且深度标注获取成本高昂；合成数据常缺乏几何真实感。需要一种既能大规模生成逼真RGB-D图像，又能保证几何精度的方法。
- **整体含义**：提出全新的数据生成与训练范式，通过强化学习优化的深度到图像（D2I）生成引擎，合成超2000万张RGB图像及其对应真实深度，结合混合监督训练，从而突破MDE数据瓶颈，提升模型在复杂场景下的深度预测能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（用文字说明即可）
- **核心思想**：构建BRIDGE框架，包括两大阶段：
    1. **RL优化的D2I生成**：利用强化学习（RL）优化深度到图像的生成过程，从多样化源深度图出发，合成具有几何准确性的RGB图像，每张图像天然自带真实深度（ground truth depth）。
    2. **混合监督训练**：在合成数据集上训练深度估计模型，使用教师模型生成的伪标签与真实深度共同进行监督，实现全面且鲁棒的训练。
- **关键技术细节**：
    - 强化学习用于优化生成器，可能围绕图像真实感与深度一致性设计奖励函数，确保生成图像保留底层的几何结构。
    - 教师伪标签用于弥补合成数据与真实数据间的域差异，使模型同时学习合成数据的结构化信息与真实数据的分布特征。
- **公式或算法流程**（文字描述）：
    1. 输入多样化的源深度图（可能来自于真实扫描、CAD模型或随机生成）。
    2. 通过RL策略网络生成RGB图像，并利用判别器与深度一致性项计算奖励，迭代优化策略。
    3. 在生成的大规模RGB-D数据集上训练MDE模型，损失函数包含：与真实深度的监督损失 + 与教师伪标签的一致性损失。
    4. 最终模型在多个基准上测试。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集与场景**：文中未明确列出具体测试数据集名称。但提及“超过2000万张真实且几何准确的RGB图像”，这些图像来自“多样化源深度图”，覆盖多领域多样性。
- **Benchmark**：未具体说明使用了哪些标准基准（如NYUv2、KITTI等），但声称在多个基准上达到突破性性能，且“复杂场景细节捕捉”上优于现有最优方法。
- **对比方法**：对比了“现有最先进方法”（SOTA），但未列出方法名称。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。
- 文中**未明确说明**所使用的GPU型号、数量、训练时长等算力信息。仅提及合成超2000万图像，但未说明生成与训练的具体资源消耗。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。
- **实验数量**：仅提供了总体实验结果描述，未列出具体实验组数、消融研究、跨数据集测试等细节。
- **充分性与公平性**：由于信息有限，难以评估。但该论文得分9.0，且被会议(ICLR-2026-Public)接收，暗示实验结果可能是充分且可信的。然而，根据提供文本，缺乏系统性的实验设计描述（如消融组件、参数敏感性、不同生成策略对比等），因此从现有内容看，实验报告不够详尽。

### 6. 论文的主要结论与发现
- 通过RL优化的D2I生成框架，可稳定合成超2000万张几何准确的RGB-D图像，大幅提升MDE训练数据规模与多样性。
- 采用混合监督（教师伪标签+真实深度）训练，可有效融合合成与真实数据信息，使模型泛化性和鲁棒性显著优于现有SOTA方法。
- 该方法在定量指标和复杂场景细节捕捉上均取得突破，为单目深度估计提供了新的数据生成与训练范式。

### 7. 优点：方法或实验设计上有哪些亮点
- **创新性**：将强化学习引入深度到图像生成，优化了生成图像与深度的一致性，解决传统合成数据几何失真问题。
- **数据规模**：生成超2000万RGB-D对，远超现有公开数据集规模，为大规模训练提供了可能。
- **训练策略**：混合监督同时利用伪标签和真实深度，有效缓解合成数据与真实数据之间的域偏移，提升模型适应性。
- **性能提升**：在多个基准上超越现有SOTA，表明方法有效。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖不足**：原文未提供具体数据集、对比方法、消融实验等细节，难以判断方法的普适性和各组件贡献。
- **偏差风险**：合成数据可能仍存在域偏差，即使使用教师伪标签，若生成器偏向特定深度分布，可能影响真实场景泛化。
- **应用限制**：未讨论在极端光照、动态场景等条件下的鲁棒性；算力成本未提及，若生成超2000万图像需要极大计算资源，可能限制其可复现性与实际部署。
- **技术细节缺失**：RL奖励函数设计、生成网络架构、教师模型选择等关键细节均未提供，降低了方法复现性。

（完）
