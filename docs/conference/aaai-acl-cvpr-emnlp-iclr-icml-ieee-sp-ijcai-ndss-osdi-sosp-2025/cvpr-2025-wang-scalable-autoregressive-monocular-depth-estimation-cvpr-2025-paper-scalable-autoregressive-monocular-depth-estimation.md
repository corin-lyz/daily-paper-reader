---
title: Scalable Autoregressive Monocular Depth Estimation
title_zh: 可扩展的自回归单目深度估计
authors: "Wang, Jinhong, Liu, Jian, Tang, Dongqi, Wang, Weiqiang, Li, Wentong, Chen, Danny, Chen, Jintai, Wu, Jian"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wang_Scalable_Autoregressive_Monocular_Depth_Estimation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 自回归模型用于单目深度估计
tldr: 现有单目深度估计通常采用回归或分类范式，缺乏可扩展的序列化建模。本文将深度图视为token集合，设计低到高分辨率的自回归目标与从粗到细的序数回归离散化，形成新的深度自回归模型DAR。该方法在KITTI与NYU Depth v2上以明显优势刷新最先进精度，展示了自回归范式在单目深度估计中的潜力。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 776, \"height\": 587, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 835, \"height\": 604, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1530, \"height\": 836, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 810, \"height\": 342, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 849, \"height\": 312, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1486, \"height\": 721, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1485, \"height\": 646, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 852, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1560, \"height\": 929, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1610, \"height\": 855, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 288, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-scalable-autoregressive-monocular-depth-estimation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 870, \"height\": 228, \"label\": \"Table\"}]"
motivation: 单目深度估计需要更灵活且可扩展的建模方式，以更好利用全局与局部结构。
method: 将深度图按分辨率分块token化，用补丁级掩码实现低到高分辨率自回归，并以序数回归实现从粗到细的深度区间离散化。
result: 在KITTI和NYU Depth v2上取得最先进结果，超越现有方法。
conclusion: 自回归预测范式可有效提升单目深度估计的精度与可扩展性。
---

## Abstract
This paper proposes a new autoregressive model as an effective and scalable monocular depth estimator. Our idea is simple: We tackle the monocular depth estimation (MDE) task with an autoregressive prediction paradigm, based on two core designs. First, our depth autoregressive model (DAR) treats the depth map of different resolutions as a set of tokens, and conducts the low-to-high resolution autoregressive objective with a patch-wise casual mask. Second, our DAR recursively discretizes the entire depth range into more compact intervals, and attains the coarse-to-fine granularity autoregressive objective in an ordinal-regression manner. By coupling these two autoregressive objectives, our DAR establishes new state-of-the-art (SOTA) on KITTI and NYU Depth v2 by clear margins. Further, our scalable approach allows us to scale the model up to 2.0B and achieve the best RMSE of 1.799 on the KITTI dataset (5% improvement) compared to 1.896 by the current SOTA (Depth Anything). DAR further showcases zero-shot generalization ability on unseen datasets. These results suggest that DAR yields superior performance with an autoregressive prediction paradigm, providing a promising approach to equip modern autoregressive large models (e.g., GPT-4o) with depth estimation capabilities. Project page: https://depth-ar.github.io/

---

## 论文详细总结（自动生成）

# 论文总结：可扩展的自回归单目深度估计

## 1. 核心问题与整体含义

- **研究动机**：单目深度估计（MDE）通常采用“编码器-解码器”的回归或分类范式，这类方法在模型规模扩展和跨数据集泛化方面存在瓶颈。自回归（AR）模型在语言、图像生成等任务中展现出强大的可扩展性和零样本能力，但如何将 MDE 转化为自回归序列预测尚缺乏系统研究。
- **核心思路**：本文提出**深度自回归模型（DAR）**，利用 MDE 中两个天然有序性质——**深度图分辨率**（低→高）和**深度值粒度**（粗→细）——将其转化为两个并行的自回归目标，从而以“预测下一分辨率令牌图+递归细化深度区间”的方式完成深度估计。
- **意义**：首次将自回归范式引入单目深度估计，在 KITTI 和 NYU Depth v2 上取得 SOTA，并展示出类似 LLM 的 Scaling Law，为在大模型中集成深度估计能力提供了新途径。

## 2. 方法论

- **整体框架**：给定输入图像 I，DAR 逐步生成多尺度深度图 {D̃₁, D̃₂, ..., D̃_K}，每一步条件于前一步的预测，最终深度为 D̃_K。过程按联合概率 p(D̃₁,...,D̃_K)=∏ pθ(D̃_k|D̃₁..D̃_{k-1}) 建模。
- **分辨率自回归目标**：
  - 使用 **DAR Transformer**（decoder-only 结构，含 MSA、LN、MCA）逐步预测更高分辨率的令牌图。
  - 每一步将上一步令牌图 `r_{k-1}` 上采样作为输入 `y_k`，在 **patch-wise causal mask** 约束下，仅允许当前令牌与“此前所有前缀令牌”及“当前令牌图内部”交互，实现低到高分辨率生成。
  - 图像特征（来自 ViT 编码器）通过 Cross-Attention 作为条件控制生成。
- **粒度自回归目标**：
  - **Multiway Tree Bins（MTBin）**：初始将深度范围均匀分为 N=16 个 bin；根据上一步预测所在 bin，扩展至相邻 bin 后递归划分成更细的子 bin，得到新候选深度中心 `c_k`。公式表示为：`L = (b_{k-1}^{t+2} - b_{k-1}^{t-1})/N`，`b_k^i = b_{k-1}^{t-1} + (i-1)L`。
  - **Bins Injection**：将 MTBin 生成的候选中心经 3×3 卷积投影为特征，通过 **ConvGRU** 注入 DAR Transformer 输出令牌，实现粒度信息对深度特征生成的引导。
  - 最终深度由 Softmax 概率与候选深度中心的线性组合得到：`D̃_k(x)=Σ c_i^k · p_i^k(x)`。
- **损失函数**：多尺度缩放不变损失（Scale-Invariant Loss），对所有 K 步预测的深度图（上采样到真值分辨率）求和计算。

## 3. 实验设计

- **数据集**：
  - **NYU Depth V2**：室内，深度 0-10m，24,231 训练图 / 654 测试图。
  - **KITTI**：室外，深度 0-80m，42,949 训练图 / 1,000 验证图。
  - **SUN RGB-D**：仅用于零样本泛化测试（5,050 测试图）。
- **对比方法**：覆盖传统 CNN（Eigen）、序数回归（DORN、AdaBins）、Transformer（DPT、NeWCRFs、BinsFormer、PixelFormer）、扩散模型（EcoDepth）、大规模预训练方法（ZoeDepth、Depth Anything）等 20+ 方法。
- **实验设置**：
  - 主干网络统一为 ViT-L（与 Depth Anything 相同），公平比较。
  - 模型规模：DAR-Small (440M)、DAR-Base (1B)、DAR-Large (2B)。
  - 评估指标：Abs Rel、RMSE、Sq Rel、log10、δ1/δ2/δ3。

## 4. 资源与算力

- **文中披露**：使用 **8 块 NVIDIA A100 GPU** 训练，DAR-Base 每个 epoch 约 **30 分钟**，共训练 **25 个 epoch**。
- **未明确说明**：不同规模模型（Small/Large）的具体训练时间、总 GPU 时长、数据加载/预处理细节等未给出。
- 模型参数量较大（最大 2.0B），训练资源需求较高，但文中未提供 FLOPs 或推理耗时对比。

## 5. 实验数量与充分性

- **定量实验**：两大标准数据集（NYU、KITTI）上的完整指标对比，覆盖几乎所有 SOTA 方法，对比充分。
- **零样本实验**：在 SUN RGB-D 上验证跨数据集泛化能力，与 Depth Anything 等对比。
- **消融实验**：Table 5 给出了基线、+MTBins+BI、+DAR（双目标）、+Scale Up 四组对比，验证各模块贡献；另有 bin 数量 N 的消融（见补充材料）和逐步可视化。
- **充分性评估**：
  - **优点**：对比方法全面、主干统一、指标多样，结论可信。
  - **不足**：消融实验较简略，未单独分离“分辨率自回归”和“粒度自回归”的效果；零样本仅测试了 SUN RGB-D 一个数据集；未与其他自回归式深度估计方法（如 Ord2Seq）直接对比。

## 6. 主要结论与发现

- DAR 在 **NYU Depth v2** 上达到 0.205 RMSE 和 0.982 δ1，在 **KITTI** 上达到 1.799 RMSE（比 Depth Anything 的 1.896 提升约 5%），均为新 SOTA。
- **可扩展性**：模型从 440M 扩展到 2.0B，性能持续提升，呈现 Scaling Law。
- **零样本能力**：仅用 NYU 训练，在 SUN RGB-D 上超越 Depth Anything 等，说明自回归范式具有良好泛化性。
- 双自回归目标（分辨率+粒度）协同作用优于单一目标，验证了自回归建模在 MDE 中的有效性。

## 7. 优点

- **创新性**：首次系统地将自回归范式引入单目深度估计，开辟新方向。
- **方法论设计**：巧妙利用了深度图的分辨率和粒度两个有序属性，并设计 patch-wise causal mask 和 MTBin 策略实现双目标建模。
- **公平性与可靠性**：与 Depth Anything 使用相同主干和相近参数量进行对比，结果有说服力。
- **可扩展性验证**：从 440M 到 2B 的多个规模实验，符合 Scaling Law，为后续研究提供参考。
- **应用潜力**：可直接嵌入 GPT-4o 等自回归多模态大模型，赋予其深度感知能力。

## 8. 不足与局限

- **边界模糊**：作者承认多步渐进生成可能导致深度边界不够锐利，细节清晰度受影响。
- **计算成本高**：自回归 Transformer 参数量大，训练和推理开销高，实际部署受限。
- **消融不深入**：未分别量化“分辨率自回归”和“粒度自回归”各自的独立贡献，也未测试不同 K 值或 bin 数量对性能的影响。
- **零样本验证有限**：仅一个未见数据集（SUN RGB-D），且集中于室内场景，对室外跨域泛化验证不足。
- **对比范围**：未与最新基于扩散的模型（如 Marigold）或更高效的轻量级方法比较推理效率。
- **依赖预训练骨干**：性能部分依赖于 ViT-L 在 ImageNet 等数据上的预训练，未证明完全从头训练的效果。

（完）
