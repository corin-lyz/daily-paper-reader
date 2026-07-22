---
title: Generalized Geometry Encoding Volume for Real-time Stereo Matching
title_zh: 广义几何编码体用于实时立体匹配
authors: "Jiaxin Liu, Gangwei Xu, Xianqi Wang, Chengliang Zhang, Xin Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37654/41616"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 实时立体匹配结合深度感知特征
tldr: 实时立体匹配方法泛化性不足，而立体基础模型推理缓慢。论文提出GGEV，利用单目基础模型提取深度感知特征，并引入深度感知动态代价聚合模块，在保持实时性的同时实现强泛化能力。在多个数据集上表现出色。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 521, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1744, \"height\": 855, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 330, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 877, \"height\": 330, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 856, \"height\": 591, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 709, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 873, \"height\": 454, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37654/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 860, \"height\": 311, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37654/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 778, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37654/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 848, \"height\": 403, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37654/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1628, \"height\": 861, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37654/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1743, \"height\": 392, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37654/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 814, \"height\": 383, \"label\": \"Table\"}]"
motivation: 现有实时立体匹配方法泛化性差，而立体基础模型推理慢。
method: 提取深度感知特征编码域不变结构先验，用深度感知动态代价聚合模块自适应融合。
result: 在多个数据集上实现强泛化且保持实时性。
conclusion: 结合单目基础模型的深度先验能有效提升立体匹配的泛化性。
---

## Abstract
Real-time stereo matching methods primarily focus on enhancing in-domain performance but often overlook the critical importance of generalization in real-world applications. In contrast, recent stereo foundation models leverage monocular foundation models (MFMs) to improve generalization, but typically suffer from substantial inference latency. To address this trade-off, we propose Generalized Geometry Encoding Volume (GGEV), a novel real-time stereo matching network that achieves strong generalization. We first extract depth-aware features that encode domain-invariant structural priors as guidance for cost aggregation. Subsequently, we introduce a Depth-aware Dynamic Cost Aggregation (DDCA) module that adaptively incorporates these priors into each disparity hypothesis, effectively enhancing fragile matching relationships in unseen scenes. Both steps are lightweight and complementary, leading to the construction of a generalized geometry encoding volume with strong generalization capability. Experimental results demonstrate that our GGEV surpasses all existing real-time methods in zero-shot generalization capability, and achieves state-of-the-art performance on the KITTI 2012, KITTI 2015, and ETH3D benchmarks.

---

## 论文详细总结（自动生成）

# Generalized Geometry Encoding Volume for Real-time Stereo Matching 论文总结

## 1. 核心问题与整体含义
- **研究动机**：实时立体匹配方法（如RT-IGEV、HITNet）在域内性能优秀，但零样本泛化到未见场景时表现脆弱，尤其在遮挡、无纹理、重复模式、薄结构区域。而近年立体基础模型（如FoundationStereo、MonSter）借助单目基础模型（MFMs）提升泛化，却因使用繁重backbone（如ViT-L）和复杂迭代机制导致推理延迟极高，无法满足实时应用需求。
- **整体含义**：论文旨在设计一种既能保持实时性（<100ms）又具有强泛化能力的立体匹配网络，通过轻量级方式将MFMs的深度先验融入代价聚合过程，实现“广义几何编码体”（GGEV）。

## 2. 方法论
- **核心思想**：利用冻结的深度基础模型（Depth Anything V2 Small）提取多尺度深度特征作为结构先验，通过选择性通道融合（SCF）与纹理特征融合为深度感知特征；再提出深度感知动态代价聚合（DDCA）模块，根据视差假设与深度特征的亲和力动态生成卷积核，自适应增强不同视差平面的脆弱匹配关系，最终构建泛化性强的几何编码体。
- **关键技术细节**：
  1. **多线索特征提取**：
     - 纹理特征编码器：MobileNetV2（ImageNet预训练）提取左右图像多尺度特征。
     - 深度特征编码器：冻结的Depth Anything V2 Small，仅从左图提取深度特征。
     - 选择性通道融合（SCF）：1×1卷积融合纹理和深度特征，保持结构细节。
  2. **代价体积构建**：基于1/4分辨率纹理特征构建分组相关代价体积（Group-wise Correlation Volume）。
  3. **深度感知动态代价聚合（DDCA）**：
     - 对每个视差假设平面，计算其与深度特征图的亲和矩阵（Affinity Matrix），生成动态卷积核。
     - 使用多组（G组）动态卷积核分别处理通道分组，同时采用大小卷积核组合捕捉低高频信息。
     - 过程公式：(2)-(6) 定义Q、K、Ag、Mg以及动态卷积操作。
  4. **初始视差回归**：对聚合后的代价体积应用soft-argmin得到1/4分辨率初始视差。
  5. **深度感知迭代优化**：使用GRU迭代更新，初始化隐状态来自深度特征；利用深度特征辅助空间上采样恢复全分辨率。
- **损失函数**：Smooth L1（初始视差）+ L1（迭代视差，衰减因子γ=0.9）。

## 3. 实验设计
- **数据集**：
  - 训练：Scene Flow（合成）；部分实验额外加入CREStereo、TartanAir（合成）。
  - 零样本测试：KITTI 2012、KITTI 2015、Middlebury（quarter）、ETH3D（真实场景）。
  - 微调：KITTI 2012+2015混合、ETH3D（使用多数据集混合）。
- **基准（Benchmark）**：
  - KITTI 2012/2015：官方leaderboard的2-pixel/3-pixel error、D1-all等。
  - ETH3D：Bad 0.5/1.0/2.0、AvgErr。
  - 零样本泛化：统一使用阈值error（KITTI 3px、Middlebury 2px、ETH3D 1px）。
- **对比方法**：
  - 准确性方法：RAFT-Stereo、IGEV、Selective-IGEV、FoundationStereo、MonSter等。
  - 实时方法：DeepPrunerFast、AANet、HITNet、CoEx、Fast-ACVNet、RT-IGEV、BANet-3D等。
  - 域泛化方法：FC-GANet、DEFOM-Stereo。

## 4. 资源与算力
- **GPU型号**：NVIDIA RTX 3090（单卡或多卡未明确说明）。
- **训练时长**：未明确给出具体小时数；仅提到使用AdamW优化器，学习率one-cycle调度，训练步数200k（消融实验）或更长时间（最终模型）。
- **参数**：全模型（ViT-S backbone）可训练参数3.68M，推理时间47ms（1248×384分辨率）；若用ViT-L则110ms。
- **说明**：论文未披露GPU数量和总训练时长。

## 5. 实验数量与充分性
- **实验组数**：至少包括：
  1. **零样本泛化**：Table 1（单数据集和多数据集训练，对比10余种方法）。
  2. **KITTI 2012/2015 benchmark**：Table 2，对比14种方法。
  3. **ETH3D benchmark**：Table 3，对比7种方法。
  4. **消融实验**：Table 4，逐步验证DFE、SCF、DCA各模块，包含参数量和速度。
  5. **反射区域评估**：Table 5（KITTI 2012 Ill-posed区域）。
  6. **迭代次数影响**：Figure 7，对比2/4/6/8迭代。
  7. **定性比较**：Figure 1,5,6,8展示多场景可视化。
- **充分性与公平性**：
  - 实验覆盖多种场景（室内/室外、合成/真实、反射面、薄结构等）。
  - 对比方法广泛，包括最新实时方法（如RT-IGEV、BANet-3D）和精度导向方法。
  - 消融实验控制了可训练参数量、推理速度等变量，公平性良好。
  - 不足：未比较在更多域泛化数据集（如Middlebury full、复杂光照）下的表现，且未与最新立体基础模型（如MonSter）在实时性约束下直接比较（因其非实时）。

## 6. 主要结论与发现
- GGEV在零样本泛化上显著超越所有现有实时方法：相比RT-IGEV，在KITTI 2012上误差降低29%，KITTI 2015降低16%，Middlebury降低16%，ETH3D降低51%。
- 在KITTI 2012/2015和ETH3D benchmark上，GGEV在所有实时方法中达到最佳，部分指标甚至超过非实时方法。
- 使用相同ViT-S backbone，GGEV推理速度比DEFOM-Stereo快5倍以上，且泛化性能相当。
- 深度先验（Depth Anything V2）有效增强脆弱匹配关系，动态卷积聚合比统一处理更有效。
- 在反射（Ill-posed）区域，GGEV超越所有实时方法，甚至超过RAFT-Stereo等非实时方法。

## 7. 优点
- **创新性**：首次将MFMs的深度特征以轻量方式融入实时立体匹配的代价聚合，而非直接用于代价体积构建或尺度对齐，避免了scale-shift问题。
- **模块设计**：DDCA动态生成卷积核，针对不同视差平面自适应响应，结构清晰且计算高效（仅增加0.03M参数）。
- **效率与性能平衡**：ViT-S版本仅47ms，满足实时要求，同时泛化性能接近昂贵方法。
- **可迁移性**：提出的SCF和DDCA模块可即插即用，未来可扩展至其他立体匹配框架。
- **实验充分**：涵盖多种基准、零样本、消融、反射区域等，验证全面。

## 8. 不足与局限
- **依赖预训练深度模型**：冻结的Depth Anything V2可能在某些未见场景（如极端照明、特殊材质）下提供不准确的深度先验，从而误导聚合；未分析失败案例。
- **实时性受深度特征提取限制**：虽然ViT-S轻量，但提取深度特征仍需8ms，若使用更重backbone（如ViT-L）则实时性下降（110ms，超出100ms实时线）。
- **未探索视频时间一致性**：当前仅处理单帧，实际自动驾驶/机器人场景需高效时间融合。
- **仅使用合成数据预训练**：虽然泛化强，但若训练数据更贴近真实分布可能进一步提升；未尝试与真实数据混合训练的影响。
- **缺乏与最先进基础模型（如FoundationStereo、MonSter）的直接公平速度对比**：这些方法虽非实时，但论文仅定性比较数值，未在同一硬件下测量速度。
- **潜在偏差**：ETH3D benchmark训练集混合了多数据集，可能引入数据泄露风险（尽管论文遵循CREStereo设置）。

（完）
