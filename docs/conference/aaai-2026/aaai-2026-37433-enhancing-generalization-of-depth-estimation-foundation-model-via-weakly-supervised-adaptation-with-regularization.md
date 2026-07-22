---
title: Enhancing Generalization of Depth Estimation Foundation Model via Weakly-Supervised Adaptation with Regularization
title_zh: 通过弱监督自适应与正则化增强深度估计基础模型的泛化能力
authors: "Yan Huang, Yongyi Su, Xin Lin, Le Zhang, Xun Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37433/41395"
tags: ["query:mono-depth"]
score: 10.0
evidence: 深度估计基础模型，Depth Anything系列，零样本泛化
tldr: 针对深度估计基础模型在未见领域泛化不足的问题，WeSTAR提出参数高效的弱监督自训练适应框架，结合密集自训练目标与正则化。在多个下游数据集上，该方法在不牺牲零样本能力的前提下显著提升了深度预测的准确性，为深度基础模型的实际部署提供了有效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37433/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 873, \"height\": 606, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37433/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1553, \"height\": 893, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37433/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1828, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37433/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 463, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1820, \"height\": 436, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 441, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1820, \"height\": 401, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 883, \"height\": 332, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 838, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37433/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 809, \"height\": 263, \"label\": \"Table\"}]"
motivation: 深度估计基础模型（如Depth Anything）在零样本场景表现出色，但仍可在有少量下游数据时进一步提升。
method: 提出WeSTAR框架，采用弱监督自训练与正则化，参数高效地适应目标领域。
result: 在多个跨域基准上，WeSTAR显著提升了深度估计的准确性和鲁棒性，优于直接微调方法。
conclusion: 弱监督自训练与正则化能有效增强深度基础模型在未知领域的泛化能力。
---

## Abstract
The emergence of foundation models has substantially advanced zero-shot generalization in monocular depth estimation (MDE), as exemplified by the Depth Anything series. However, given access to some data from downstream tasks, a natural question arises: can the performance of these models be further improved? To this end, we propose WeSTAR, a parameter-efficient framework that performs \textbf{We}akly supervised \textbf{S}elf-\textbf{T}raining \textbf{A}daptation with \textbf{R}egularization, designed to enhance the robustness of MDE foundation models in unseen and diverse domains. We first adopt a dense self-training objective as the primary source of structural self-supervision. To further improve robustness, we introduce semantically-aware hierarchical normalization, which exploits instance-level segmentation maps to perform more stable and multi-scale structural normalization. Beyond dense supervision, we introduce a cost-efficient weak supervision in the form of pairwise ordinal depth annotations to further guide the adaptation process, which enforces informative ordinal constraints to mitigate local topological errors. Finally, a weight regularization loss is employed to anchor the LoRA updates, ensuring training stability and preserving the model's generalizable knowledge. Extensive experiments on both realistic and corrupted out-of-distribution datasets under diverse and challenging scenarios demonstrate that WeSTAR consistently improves generalization and achieves state-of-the-art performance across a wide range of benchmarks.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文文本，以下是对该论文《Enhancing Generalization of Depth Estimation Foundation Model via Weakly-Supervised Adaptation with Regularization》的详细中文总结。

---

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：尽管像 Depth Anything 系列这样的单目深度估计（MDE）基础模型在零样本跨域泛化方面表现出色，但在实际部署中，当拥有少量来自下游任务的数据时，如何利用这些数据在不牺牲模型原有泛化能力的前提下，进一步提升模型性能，仍是一个未解难题。
- **研究背景与动机**：
    - 现有模型的零样本预测在分布偏移严重的场景下（如恶劣天气、传感器噪声）仍不完美。
    - 直接进行微调（全参数微调或朴素的自训练）存在三大挑战：
        1.  **伪标签不可靠**：深度估计是回归任务，生成可靠的伪标签比分类任务更困难，错误标签会误导模型（确认偏差）。
        2.  **改进空间有限**：由于预训练模型本身具有很强的几何理解能力，仅靠密集自训练（ST）在清洁数据上带来的增益微乎其微。
        3.  **灾难性遗忘**：过度的自适应可能导致模型过拟合到特定目标域，丢失预训练中学到的通用知识。
- **整体含义**：本文旨在提出一种安全、高效、鲁棒的参数高效自适应框架，以增强深度估计基础模型在未见过的、多样化的目标域（尤其是困难场景）中的泛化能力。

### 2. 论文提出的方法论

#### 2.1 核心思想
提出了一个名为 **WeSTAR** (Weakly-supervised Self-Training Adaptation with Regularization) 的框架。该框架通过**协同设计**的方式，将密集自训练（ST）、成本低廉的弱监督信号（成对深度排序）、语义感知归一化和参数更新正则化结合起来，实现稳定且高效的模型适应。

#### 2.2 关键技术细节

1.  **密集自训练（Dense Self-Training）**：
    -   **结构**：使用经典的教师-学生模型架构，教师模型通过指数移动平均（EMA）从学生模型权重更新。
    -   **流程**：对无标签数据应用弱增强（$T_w$）输入教师模型，生成伪标签（$d^*$）；对同一数据应用强增强（$T_s$）输入学生模型，输出预测（$d$）。
    -   **损失计算**：计算两者在语义感知层次归一化（SA-HDN）后的平均绝对误差（MAE）。

2.  **语义感知层次归一化（Semantic-Aware Hierarchical Depth Normalization, SA-HDN）**：
    -   **解决的问题**：传统归一化（全局）或内容无关的网格归一化（HDN）会忽略语义上下文，或割裂同一物体，导致统计量不稳定。
    -   **方法**：利用外部分割基础模型（如 SAM2）实时生成实例级分割掩码（$M_k$），为每个像素定义包含全局上下文（$C_{global}$）和实例上下文（$C_{ins}^k$）的多层级上下文。归一化公式为 $\Phi(d_p, C_p) = (d_p - t(C_p))/(s(C_p) + \epsilon)$，其中 $t$ 和 $s$ 分别指中位数和中位数绝对偏差。
    -   **损失函数**：$L_{st}(p) = \frac{1}{|C_p|} \sum_{c \in C_p} |sg(\Phi(d^*_p, c)) - \Phi(d_p, c)|$。

3.  **弱监督适应（Weakly-Supervised Adaptation）**：
    -   **动机**：引入与模型伪标签独立的稀疏、强约束的标签信号，以纠正局部拓扑错误，缓解确认偏差。
    -   **方法**：在少量图像上标注成对深度顺序关系（如点A比点B远）。使用基于间隔的排序损失（Margin Ranking Loss）来强制约束这些关系。公式为 $L_{weak} = \sum \ell(\hat{d}^+_{jn}, \hat{d}^-_{jn}, l_{jn})$，其中对有序与相等关系分别进行边界损失和绝对值损失约束。

4.  **鲁棒适应与正则化（Robust Adaptation with LoRA and Regularization）**：
    -   **LoRA**：在编码器的自注意力层注入低秩矩阵（$U, V$），仅更新这些参数。这大幅降低计算开销，并抑制过拟合和灾难性遗忘。
    -   **权重正则化（Weight Regularization, L2 Loss on LoRA weights）**：
        -   **动机**：即使使用 LoRA，在严重域偏移或噪声伪标签下，模型仍可能发生确认偏差。
        -   **方法**：增加一项损失 $L_{reg} = \sum_{U_t, V_t} \alpha \|U_t V_t\|_2^2$，惩罚学生模型中LoRA权重相对于初始值（通常为0）的大幅偏离。
        -   **作用**：如同一个“锚”，确保模型参数更新只在使用域数据强烈迫使时发生，稳定训练，防止遗忘先验知识。

#### 2.3 算法流程
1.  加载预训练MDE基础模型（如 Depth Anything V2）作为初始化。
2.  **冻结** 原始模型参数，在注意力层添加可训练的 **LoRA** 低秩矩阵。
3.  在无标签目标域数据上进行自训练：教师生成伪标签 -> 学生通过强力增强预测 -> 计算 SA-HDN 后的 MAE 损失 $L_{st}$。
4.  在有弱标签（成对深度）数据上计算弱监督排序损失 $L_{weak}$。
5.  计算 LoRA 参数的权重正则化损失 $L_{reg}$。
6.  总损失函数：$L = \lambda_{st} L_{st} + \lambda_w L_{weak} + \lambda_r L_{reg}$，并反向传播更新模型的 LoRA 参数。
7.  通过 EMA 更新教师模型。

### 3. 实验设计

- **使用的数据集与场景**：
    - **清洁、真实数据集**：NYU-V2, KITTI, Sintel, DIODE, NuScenes (夜间版), DrivingStereo (晴天/多云/雾/雨)。
    - **带腐蚀的基准数据集**：NYU-C, KITTI-C, DIODE-C, Sintel-C。这些基准对原始数据施加了6种不同类别的最高严重程度腐蚀，模拟极端域偏移。
    - 每个数据集都划分为独立的训练和测试集，仅在训练集上自适应，在测试集上评估。
- **Benchmark**：主要基于 `δ1` 指标（越高越好）和 `AbsRel` 指标（越低越好）。
- **对比方法**：
    - **零样本基线**：原始预训练模型（Source）。
    - **源-自由域自适应方法 (SFDA)**：TTT++, TTAC, FR, SSA。
    - **自监督方法**：iBOT*（基于ViT的特征表示学习）。
    - **弱监督方法**：SGRL (结构引导排序损失)。
- **公平性**：所有对比方法均使用相同的预训练骨干网络（Depth Anything V2 和 MiDaS v3.1）。

### 4. 资源与算力

- 文中明确说明，所有实验均在 **单块 NVIDIA RTX 3090 GPU** 上，使用 **批处理大小为 4** 进行训练。
- 这体现了WeSTAR框架的高效性，不需要大规模计算资源。

### 5. 实验数量与充分性

- **实验数量**：内容丰富，包含以下主要部分：
    1.  **主实验结果（表1, 2, 3）**：在9种不同真实/腐蚀数据集上进行对比，评估了 WeSTAR 与7种其他方法（包括Source基线）在 δ1 和 AbsRel 指标上的表现。
    2.  **消融实验（表4, 5, 6）**：系统地分析了单个组件（ST, WS, WR）、不同微调模块（全参数 vs. 编码器 vs. 解码器 vs. LoRA）和不同归一化方法（Global vs. HDN vs. SA-HDN）的影响。
    3.  **训练稳定性分析（图4）**：展示了不同方法在训练过程中 δ1 和 AbsRel 性能的演化曲线，论证了 WeSTAR 的稳定性。
    4.  **定性结果（图3）**：提供了多个场景的可视化深度图对比。
- **充分性与客观性**：
    - **充分**：实验覆盖了清洁、腐蚀（多种类型）、不同天气等多个真实且具有挑战性的场景，消融实验完整。
    - **客观公平**：对比方法全面，且明确指出了所有方法均使用相同骨干网络。实验设置（超参数、训练策略）也进行了说明，控制变量较好。
    - **不足**：对于弱标签的标注成本或需要多少弱标签数据达到最佳性能，文中只提及数量极少（具体数目未说明），没有深入探讨弱标签量与性能的边际效应。此外，对 SAM2 这一外部模型的依赖性在推理阶段增加了额外开销，实验未讨论端到端推理速度或内存占用。

### 6. 主要结论与发现

- **主要结论**：所提出的 **WeSTAR** 框架能**持续、稳定**地提升深度估计基础模型在 **严重域偏移**（如数据腐蚀、夜间、雾天）下的泛化能力，在大多数基准上达到了 **最先进的性能 (SOTA)**。
- **关键发现**：
    - **弱监督与自训练互补**：密集自训练（提供全局结构）与稀疏的弱监督顺序标签（提供强序数约束、局部纠错）配合使用，效果优于单独使用。
    - **语义感知归一化的有效性**：SA-HDN 相比全局归一化和内容无关的 HDN，能更准确地匹配多尺度几何结构，显著提升性能。
    - **权重正则化保证稳定性**：LoRA 权重上的正则化是关键，能有效抑制由于噪声伪标签和稀疏弱监督导致的训练不稳定和几何扭曲。
    - **参数高效性**：LoRA 微调优于全参数微调，既能避免过拟合/遗忘，又能在大多数场景下取得更好或更稳定的性能。

### 7. 优点

1.  **方法设计巧妙**：将自训练、弱监督、语义信息注入和参数正则化有机结合，形成一个协同效应很强的整体，针对性地解决了现有微调方案的每个痛点。
2.  **实践价值高**：提出了一种极易在现实中部署的方案：可以在少量、低成本的弱标注（标注成本远低于密深度图）帮助下，高效提升模型在特定且困难域的表现。
3.  **鲁棒性强**：模型不仅在干净数据上保持或提升，更在各类严重腐蚀数据上展现出了巨大的增益，这对于真实世界应用至关重要。
4.  **实验设计扎实**：使用了多样化的数据集、严苛的腐蚀基准和全面的对比方法，实验设置公平，分析详尽，有力支撑了结论。

### 8. 不足与局限

1.  **外部模型依赖**：SA-HDN 严重依赖 SAM2 等外部分割模型生成实例掩码。这不仅引入了额外计算成本和延迟，也意味着整体流程的性能上限受限于分割模型。在域漂移目标域中，SAM2 本身的泛化能力也是一个潜在风险。
2.  **弱标签成本的不确定性**：虽然论文称弱标签是“成本低廉”的，但并未系统研究达到最佳效果时需要哪些类型的弱标签（如每张图标注多少对）、标注的难度如何、或者更少标注下的性能退化曲线，对实际应用中的资源配置指导意义不足。
3.  **实验硬件规模**：虽然单卡 3090 能运行是优势，但也意味着实验仅在有限的数据规模（批处理大小=4）下进行，对于更大规模的骨干网络或训练数据，其性能缩放规律未知。
4.  **理论深度有限**：方法主要是工程实践驱动的组合创新，缺乏深入的理论分析来解释为何权重正则化能如此有效，或者语义信息是如何在数学上帮助深度归一化的。
5.  **仅评估了性能指标，未讨论推理速度**：添加了 LoRA 和 SAM 后，模型推理时的参数和计算量相比原始模型有多大变化，文中没有提及。

（完）
