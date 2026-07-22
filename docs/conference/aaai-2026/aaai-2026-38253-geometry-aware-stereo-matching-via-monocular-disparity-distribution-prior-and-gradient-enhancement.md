---
title: Geometry-Aware Stereo Matching via Monocular Disparity Distribution Prior and Gradient Enhancement
title_zh: 基于单目视差分布先验和梯度增强的几何感知立体匹配
authors: "Junze Zhang, Luoxi Jing, Yuanyuan Wang, Xueqi Li, Guoli Yang, Songchang Jin, Chunping Qiu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38253/42215"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 利用单目先验的立体匹配
tldr: 针对立体匹配在遮挡、细节和反射区域缺乏相关先验的问题，提出GEAStereo网络，通过将单目视差分布先验自适应融入代价体积构建单目-立体融合体积，并引入梯度特征增强几何结构感知，有效改善了不适定区域的匹配质量。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38253/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 467, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38253/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 877, \"height\": 496, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38253/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1837, \"height\": 827, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38253/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 855, \"height\": 428, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38253/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 875, \"height\": 461, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1800, \"height\": 169, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1800, \"height\": 574, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 402, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 362, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 817, \"height\": 496, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 807, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38253/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 880, \"height\": 493, \"label\": \"Table\"}]"
motivation: 现有立体匹配方法在遮挡和细纹理区域缺乏相关性先验，导致匹配不准确。
method: 提出单目-立体融合体积（MSFV），将单目视差分布先验融入立体代价体积，并利用梯度特征增强细节。
result: 在多个公开数据集上达到领先性能，尤其在遮挡和反射区域表现优异。
conclusion: 利用单目先验有效提升立体匹配的鲁棒性和精度。
---

## Abstract
Stereo matching recovers 3D scene information based on the correlation between corresponding pixels. Despite impressive progress, existing methods lack sufficient correlation priors in ill-posed regions such as occlusions, detailed and reflective regions. In this paper, we propose Geometry Aware Stereo Matching Network (GEAStereo) to enhance geometric structure perception and address this issue. We adaptively incorporate the Monocular Disparity Distribution Prior into the stereo cost volume, building Mono-Stereo Fusion Volume (MSFV), which effectively captures global geometric structures and rectifies the correlation information in ill-posed regions. Furthermore, we introduce rich detail information from gradient features and construct a Detail-Aware Volume (DAV) by aggregating the group-wise cost volume under the guidance of gradient spatial attention, thus enhancing the correlation modeling in detailed structures. Jointly, MSFV and DAV provide rich correlation priors for disparity iterative optimization. Experimental results show that our method achieves competitive results on the ETH3D and KITTI2015 benchmarks. Compared with the state-of-the-art methods, our method demonstrates stronger performance in zero-shot generalization.

---

## 论文详细总结（自动生成）

# 论文总结：基于单目视差分布先验和梯度增强的几何感知立体匹配

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：立体匹配在遮挡、无纹理/低纹理、反射及细节丰富的区域（即“不适定区域”）中，代价体积难以捕获可靠的匹配相关性，导致误匹配。
- **现有方法不足**：1）传统成本聚合与迭代优化方法均依赖代价体积，但在不适定区域缺乏有效相关先验；2）近期工作（如 DEFOM-Stereo、MonSter）引入单目深度先验（如 DepthAnythingV2）来提供结构信息，但忽略了代价体积本身在这些区域仍然缺失或错误，且单目深度与视差存在跨场景尺度对齐误差，限制了零样本泛化能力。
- **本文目标**：提出 GEAStereo 网络，通过**单目视差分布先验**（而非深度）自适应修正代价体积的相关性，同时利用**梯度特征**增强细节结构感知，从而提升不适定区域的匹配精度和零样本泛化能力。

## 2. 方法论

### 核心思想
- 构建两个增强的体积来提供丰富的相关先验：
  - **单目-立体融合体积 (MSFV)**：将单目视差分布先验融入立体代价体积，修正全局几何结构。
  - **细节感知体积 (DAV)**：利用梯度特征和空间注意力增强代价体积中的细节建模。
- 由 DAV 回归初始视差，再通过双核 GRU 迭代优化。

### 关键技术细节
- **单目视差分布先验体积 (MDPV)**：从左图特征经 Trap Block 解码器生成，输出 `H/4 × W/4 × D_max` 的分布概率，避免尺度映射错误；通过 soft argmin 得到“单目视差”用于中间监督。
- **单目-立体融合模块 (MPF)**：
  - 分别处理 MDPV 和立体代价体积得到特征 `F_mono`, `F_stereo`。
  - 交叉模态注意力 (CMA)：以 `F_mono` 为 query，`F_stereo` 为 key/value，利用线性归一化降低复杂度，捕获全局像素关联。
  - 遗忘门滤波 (FGF)：计算 `FGF = SiLU(Linear(C_stereo))` 并与融合特征相乘，保留准确匹配、抑制错误先验。
  - 输出 MSFV `C_fusion`。
- **细节感知体积 (DAV)**：
  - 构建分组相关体积 `C_gp` 和梯度相关体积 `C_gd`（利用梯度特征的内积）。
  - 两者拼接后通过轻量级 3D UNet 聚合，过程中引入**梯度空间注意力**：对左梯度图进行通道池化，经 7×7 卷积和 sigmoid 得到空间权重，加权中间代价体积，增强边缘和细节区域。
- **迭代优化**：DAV 经 soft argmin 得到初始视差 `d0`；双核 GRU 迭代更新视差残差。
- **损失函数**：视差损失（Smooth L1 + 加权L1）、单目视差损失（Smooth L1）、梯度损失（对初始/最终视差图计算梯度并监督）。

## 3. 实验设计

- **训练集**：Scene Flow 数据集，预训练 200k 迭代，batch size 8。
- **评估基准与数据集**：
  - Scene Flow：EPE 指标。
  - KITTI2015：D1-bg, D1-all 等。
  - ETH3D：Bad0.5, Bad1.0, Bad2.0, Avgerr。
  - KITTI2012 反射区域：Out-3/4/5 等。
  - 零样本：直接在 Middlebury（half/quarter）和 ETH3D 上测试，不微调。
- **对比方法**：
  - 主流方法：RAFT-Stereo, ACVNet, CREStereo, IGEV, Selective-IGEV, MC-Stereo, Mocha-Stereo, IGEV++, AIO-Stereo 等。
  - 使用深度先验的方法：DEFOM-Stereo, MonSter。
- **评价指标**：EPE、像素误差率（Bad 0.5/1.0/2.0/3px）、D1 等。

## 4. 资源与算力

- **文中说明**：
  - 框架：PyTorch，AdamW 优化器，one-cycle 学习率，初始学习率 2e-4。
  - 预训练迭代：Scene Flow 上 200k 次，batch size 8。
- **未明确的信息**：GPU 型号与数量、单次训练时长、推理速度等未提及。

## 5. 实验数量与充分性

- **主要实验组**：
  - 四个数据集（Scene Flow, KITTI2015, ETH3D, KITTI2012 反射区域）的定量比较。
  - 边缘/非边缘区域分析（使用 Canny 算子划分 Scene Flow 测试集）。
  - 零样本泛化（Middlebury, ETH3D）对比。
  - 消融实验：整体框架（基线+DAV+MSFV）、MSFV 消融（去掉单目损失、去掉 MDPV、不同融合策略、去掉遗忘门）等。
- **充分性评价**：实验覆盖主流基准和常见困难场景；消融设计细致，验证了每个组件的必要性；与 SOTA 方法在相同指标下比较，公平性较好。但缺乏实时性分析、更大规模数据集（如 Sintel）或真实应用场景（如自动驾驶）的鲁棒性测试。

## 6. 主要结论与发现

- GEAStereo 在 Scene Flow 上 EPE 0.40，超越 Selective-IGEV（0.44）和 DEFOM-Stereo（0.42），且参数量仅为 DEFOM 的 4%。
- 在 ETH3D 上，多数指标达到 SOTA，如 Non-Occ 区域 Bad0.5 从 2.91（AIO-Stereo）降至 1.94。
- 在 KITTI2015 上，背景非遮挡区域 D1 降低至 1.16。
- 在反射区域（KITTI2012）和边缘区域（Scene Flow）均显著优于基线。
- 零样本泛化：在 Middlebury 和 ETH3D 上，误差率比之前最佳方法降低 18%-70%，展示了强大的跨场景适应能力。
- 消融实验证实 MSFV 和 DAV 的独立贡献：MSFV 利用单目先验修正代价体积，DAV 利用梯度细节提升边缘精度。

## 7. 优点

- **创新先验形式**：采用单目**视差分布**（而非深度），避免了深度-视差尺度的跨场景对齐问题，有利于零样本泛化。
- **融合机制设计**：MPF 通过线性注意力捕获全局关联，并结合遗忘门保留准确匹配信息，有效抑制错误先验。
- **细节增强**：梯度空间注意力引导的 DAV 能恢复细微结构，在边缘区域表现突出。
- **参数高效**：参数量仅 14.84M，远低于 DEFOM-Stereo（382.62M）等依赖基础模型的方法，便于部署。
- **全面实验**：覆盖常规、不适定区域、零样本等多种场景，消融充分，对比公正。

## 8. 不足与局限

- **计算资源不透明**：未报告 GPU 型号、训练时长、推理速度或内存占用，难以评估实际部署成本。
- **先验质量依赖**：MDPV 的质量依赖训练数据中纹理与视差的关联，若训练集与测试集分布差异极大，先验可能失效。
- **极端场景局限**：虽然改善了不适定区域，但在严重遮挡、镜面反射极强或极度低纹理区域，仍可能无法完全消除误匹配。
- **实验覆盖有限**：未在自动驾驶常用数据集如 DrivingStereo、KITTI2012 全量上评估；未测试与实时性相关的帧率指标。
- **未与近期基于 Transformer 的 SOTA 全面对比**：如 STTR、SGM-Nets 等未在文中提及，可能缺少部分竞争基线。
- **消融仅在 Scene Flow 上**：缺乏在真实数据集上（如 KITTI）验证消融结果的鲁棒性。

（完）
