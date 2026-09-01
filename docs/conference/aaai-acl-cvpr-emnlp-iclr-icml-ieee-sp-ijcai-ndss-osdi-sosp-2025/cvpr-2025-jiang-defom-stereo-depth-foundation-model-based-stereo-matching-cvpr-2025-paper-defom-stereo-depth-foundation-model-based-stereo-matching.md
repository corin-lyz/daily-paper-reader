---
title: "DEFOM-Stereo: Depth Foundation Model Based Stereo Matching"
title_zh: DEFOM-Stereo：基于深度基础模型的立体匹配
authors: "Jiang, Hualie, Lou, Zhiqiang, Ding, Laiyan, Xu, Rui, Tan, Minglang, Jiang, Wenjie, Huang, Rui"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Jiang_DEFOM-Stereo_Depth_Foundation_Model_Based_Stereo_Matching_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 9.0
evidence: 基于深度基础模型的立体匹配
tldr: 双目匹配在遮挡和无纹理区域容易失效，单目相对深度模型却具备强大的泛化能力。DEFOM-Stereo将单目深度基础模型融入循环立体匹配框架，在特征提取阶段融合CNN与深度特征，并在更新阶段使用深度先验。实验证明该方法显著改善遮挡和无纹理区域的视差估计精度，为鲁棒的双目度量深度估计提供了新路径。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 900, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 855, \"height\": 175, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1711, \"height\": 770, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1769, \"height\": 590, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 416, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1777, \"height\": 709, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1632, \"height\": 521, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jiang-defom-stereo-depth-foundation-model-based-stereo-matching-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1804, \"height\": 610, \"label\": \"Table\"}]"
motivation: 现实中的遮挡与无纹理区域使纯双目匹配难以获得准确视差，而单目相对深度模型具有很好的泛化能力。
method: 在循环立体匹配框架中集成单目相对深度基础模型，构建结合CNN与DEFOM的特征编码，并在更新阶段引入深度先验。
result: 实验表明融合深度基础模型可显著提升遮挡和无纹理区域的立体匹配精度与鲁棒性。
conclusion: 为双目深度估计提供了一种借助单目深度基础模型增强匹配的新框架，同时服务于度量深度估计任务。
---

## Abstract
Stereo matching is a key technique for metric depth estimation in computer vision and robotics. Real-world challenges like occlusion and non-texture hinder accurate disparity estimation from binocular matching cues. Recently, monocular relative depth estimation has shown remarkable generalization using vision foundation models. Thus, to facilitate robust stereo matching with monocular depth cues, we incorporate a robust monocular relative depth model into the recurrent stereo-matching framework, building a new framework for depth foundation model-based stereo-matching, DEFOM-Stereo. In the feature extraction stage, we construct the combined context and matching feature encoder by integrating features from conventional CNNs and DEFOM. In the update stage, we use the depth predicted by DEFOM to initialize the recurrent disparity and introduce a scale update module to refine the disparity at the correct scale. DEFOM-Stereo is verified to have much stronger zero-shot generalization compared with SOTA methods. Moreover, DEFOM-Stereo achieves top performance on the KITTI 2012, KITTI 2015, Middlebury, and ETH3D benchmarks, ranking 1^ st on many metrics. In the joint evaluation under the robust vision challenge, our model simultaneously outperforms previous models on the individual benchmarks, further demonstrating its outstanding capabilities.

---

## 论文详细总结（自动生成）

# DEFOM-Stereo：基于深度基础模型的立体匹配（论文总结）

## 1. 论文核心问题与整体含义

- **研究动机**：立体匹配是计算机视觉与机器人中获得度量深度（metric depth）的关键技术，但真实世界中的**遮挡（occlusion）**和**无纹理（non-texture）区域**使得仅依赖双目匹配线索难以获得准确的视差估计。
- **背景**：近年来，单目相对深度估计（monocular relative depth estimation）借助视觉基础模型（如 Depth Anything V2）表现出极强的零样本泛化能力和跨场景鲁棒性，能够对反光、无纹理、曝光异常等困难区域给出较好的相对深度预测。
- **核心思路**：将鲁棒的单目相对深度基础模型（DEFOM）集成到循环立体匹配框架（RAFT-Stereo）中，用单目深度线索辅助双目匹配，构建更鲁棒的立体匹配模型 **DEFOM-Stereo**。
- **意义**：为双目度量深度估计提供了一条利用深度基础模型增强匹配能力的新路径，同时缓解了纯双目匹配在挑战性场景下的失效问题。

## 2. 方法论

### 2.1 总体框架
- 以 **RAFT-Stereo** 为基线，集成 **Depth Anything V2**（作为深度基础模型 DEFOM）。
- 框架分为两个阶段：
  1. **特征提取与视差初始化阶段**：用 DEFOM 增强特征编码，并用其预测的深度图初始化循环视差。
  2. **循环更新阶段**：先执行**尺度更新（Scale Update, SU）**，再进行传统的**增量更新（Delta Update, DU）**。

### 2.2 组合特征提取（Combined Feature Extraction）
- **组合特征编码器（Combined Feature Encoder）**：将 DEFOM 中 DPT 头输出的融合特征图，经卷积块对齐通道后，与普通 CNN 特征图**逐元素相加**，得到组合匹配特征。
- **组合上下文编码器（Combined Context Encoder）**：将 DPT 中 Reassemble 4/8/16 三个尺度的特征分别经卷积对齐后，与 CNN 上下文特征相加，构成多尺度组合上下文。
- 关键设计：使用一个**可训练的 DPT 头**（而非固定的预测深度用的 DPT），以提供更灵活的特征用于融合。

### 2.3 相关体构建与查找
- 构建全对相关体（all-pair correlation volume）金字塔：\(C^1 \in \mathbb{R}^{h \times w \times w}\)，通过最后一维的 1D 平均池化构建多层金字塔。
- **传统金字塔查找（Pyramid Lookup, PL）**：采样当前视差及其邻域的相关值，搜索范围上限为 128 像素。
- **提出的尺度查找（Scale Lookup, SL）**：仅作用于最细层相关体，对视差乘以一组尺度因子 \(\{1,2,4,6,8,10,12,16\}/8\) 进行采样，并同时采样相邻像素（±1）的相关值，共采样 24 个相关值。SL 覆盖全图搜索范围，解决了 PL 搜索范围有限的问题。

### 2.4 单目深度初始化
- 深度基础模型输出的是相对深度，与真实视差存在未知的尺度和偏移。
- 初始化公式：
  \[
  d_0 = \eta \cdot w \cdot \frac{z}{\max(z) + \epsilon}
  \]
  其中 \(\eta\) 为控制比例（设为 1/2），\(w\) 为图像宽度，\(\epsilon = 0.05\) 为小偏置。

### 2.5 尺度更新（Scale Update, SU）
- 由于 DEFOM 预测的深度存在**区域内尺度不一致**问题（尤其在合成数据集如 Scene Flow 上严重），需要恢复逐像素的正确尺度。
- SU 模块使用 ConvGRU 预测一个稠密尺度图 \(s\)，更新方式为：
  \[
  d_n = s \cdot d_{n-1}
  \]
- SU 模块替换了原来 RAFT-Stereo 中以 0 初始化视差的做法，在 DU 之前执行，DU 再基于 SU 的结果恢复局部细节。

### 2.6 损失函数
- 与 RAFT 系列一致，对每次迭代的视差预测施加指数递增权重的 L1 损失：
  \[
  L = \sum_{i=1}^{N} \gamma^{N-i} \| d_{gt} - d_i \|_1, \quad \gamma = 0.9
  \]

## 3. 实验设计

### 3.1 数据集与 Benchmark
- **预训练**：Scene Flow 合成数据集（320×736 裁剪）。
- **零样本泛化测试**：KITTI 2012、KITTI 2015、Middlebury（full/half/quarter）、ETH3D 的训练集。
- **官方榜单评估**：KITTI 2012、KITTI 2015、Middlebury、ETH3D 四个标准立体匹配 leaderboard。
- **联合评估**：Robust Vision Challenge（RVC），同时评估 KITTI 2015、Middlebury、ETH3D 三个基准。
- **微调数据**：针对不同 benchmark 使用不同的混合数据集，包括 Tartan Air、CREStereo Dataset、Scene Flow、Falling Things、InStereo2k、CARLA HR-VS、Sintel Stereo、virtual KITTI 2、3D Ken Burns、IRS、Booster 等。

### 3.2 对比方法
- **零样本泛化**：DSMNet、RAFT-Stereo、DLNR、IGEV-Stereo、Selective-RAFT、Selective-IGEV、NMRF-Stereo、Mocha-Stereo 等。
- **KITTI 榜单**：ACVNet、PCWNet、LaC+GANet、CREStereo、IGEV-Stereo、MC-Stereo、DN+ACVNet、Selective-IGEV、NMRF-Stereo、LoS、GANet+ADL、MoCha-Stereo、IGEV++、StereoBase、ViTAStereo 等。
- **Middlebury/ETH3D 榜单**：HITNet、RAFT-Stereo、CREStereo、CroCo-Stereo、GMStereo、IGEV-Stereo、DLNR、LoS、IGEV++、Selective-IGEV 等。
- **RVC 评估**：2018/2020/2022 年 RVC 的冠亚军模型及近年模型（UCFNet RVC、LoS RVC）。

### 3.3 模型变体
- DEFOM-Stereo（ViT-S）：使用小尺寸 ViT 骨干。
- DEFOM-Stereo（ViT-L）：使用大尺寸 ViT 骨干。
- DEFOM-Stereo RVC：使用 ViT-S 的轻量版本用于联合评估。

## 4. 资源与算力

- **GPU 型号**：NVIDIA RTX 4090。
- **GPU 数量**：文中**未明确说明**使用了多少张 GPU。
- **训练时长**：文中**未明确给出**具体训练时长。
- **训练配置**：AdamW 优化器，one-cycle 学习率调度，初始学习率 2e-4，batch size 8，更新迭代数（SU+DU）训练时 18 次、评估时 32 次，SU 固定 8 次。
- **训练步数**：Scene Flow 预训练 200k 步；不同 benchmark 微调步数不等（如 KITTI 微调 50k，Middlebury 两阶段 200k+100k，ETH3D 两阶段 300k+90k，RVC 两阶段 200k+100k）。

## 5. 实验数量与充分性

- **实验组数**：
  - 零样本泛化评估：4 个数据集（KITTI 2012/2015、Middlebury 三个分辨率、ETH3D），对比 9 种以上方法。
  - 官方榜单评估：4 个标准 benchmark，对比 15 种以上已发表 SOTA 方法。
  - RVC 联合评估：3 个基准，对比 7 种以上 RVC 模型。
  - 两个规模（ViT-S/ViT-L）的模型变体。
  - 消融研究（文中提及在补充材料中，未在主文中展示）。
- **充分性评价**：
  - **优点**：零样本泛化评估覆盖多个主流真实数据集，对比方法全面且使用统一的 Scene Flow 预训练模型，公平性较好；官方榜单评估具有客观性和权威性；RVC 评估进一步验证了模型的联合泛化能力。
  - **不足**：消融实验仅在补充材料中，主文未展示各组件（如 SU、SL、组合编码器）的独立贡献量化分析；未对比同类"深度基础模型+立体匹配"的最新工作（如 Stereo Anything、MonSter、FoundationStereo，仅在相关工作提及）。

## 6. 主要结论与发现

1. **DEFOM 特征融合有效**：将深度基础模型的 DPT 特征简单与 CNN 特征相加，即可显著提升匹配特征和上下文特征的质量。
2. **深度初始化+尺度更新显著提升效果**：将 DEFOM 预测的相对深度作为视差初始值，并通过 SU 模块恢复稠密尺度，有效解决了深度尺度不一致问题。
3. **零样本泛化大幅增强**：仅用 Scene Flow 预训练，DEFOM-Stereo (ViT-L) 相比 RAFT-Stereo 在 KITTI 上误差降低约 13%，ETH3D 降低约 28%，Middlebury 降低超过一半；在 Middlebury-full 上比此前最佳（DLNR）误差降低 29% 以上。
4. **多个官方榜单排名第 1**：在 KITTI 2012、KITTI 2015、Middlebury、ETH3D 上均取得多项指标第 1（如 KITTI 2015 的 D1-fg、D1-all，Middlebury 的 Bad 2.0 (all) 等）。
5. **RVC 联合评估全面最优**：在 KITTI 2015 和 Middlebury 上大幅超越此前所有 RVC 模型，如 Middlebury 上比次优模型提升约 25%。
6. **对遮挡区域改善更明显**：Middlebury 上 all 像素评估的提升幅度大于 non-occluded 像素，表明深度先验对遮挡区域有更强的辅助作用。

## 7. 优点

- **方法简洁有效**：没有复杂的注意力适配器，简单的特征加性融合 + 尺度更新模块即可取得显著收益。
- **挖掘了深度基础模型的新用法**：不仅利用其特征，还利用其深度预测作为先验，并通过尺度更新解决尺度不一致问题。
- **尺度查找（SL）设计精巧**：克服了传统金字塔查找搜索范围受限的问题，实现了全图范围内的全局匹配。
- **泛化性能突出**：相比于其他后期改进 RAFT-Stereo 的方法（如 Selective-IGEV、Mocha-Stereo 等），DEFOM-Stereo 在跨数据集零样本评估中全面领先。
- **实验验证扎实**：涵盖零样本、官方榜单、联合评估三个层面，且在多个基准上取得领先排名。
- **视觉对比直观**：示例图展示了在反光、无纹理、曝光异常等困难场景下的明显改进。

## 8. 不足与局限

- **算力信息不完整**：未说明 GPU 数量、训练时长、模型参数量等关键资源信息，不利于复现和成本评估。
- **消融实验未在主文展示**：各组件（如 SU 的迭代次数、SL 的尺度设计、组合编码器的替代方案等）的消融结果放在补充材料，主文缺乏对设计选择的充分论证。
- **运行时间无详细对比**：虽然表格中列出了一些方法的运行时间，但 DEFOM-Stereo 使用了 ViT 骨干和额外的 DEFOM 推理，计算开销可能较大，文中未给出明确的推理时间与效率分析。
- **与同类方法的对比不足**：同样将深度基础模型融入立体匹配的同期工作（Stereo Anything、MonSter、FoundationStereo）仅在相关工作提及，未进行实验对比。
- **对 DEFOM 的依赖风险**：模型性能高度依赖深度基础模型的预测质量；当单目深度估计在特定场景（如极端视角、特殊传感器）失效时，模型可能受到影响。
- **评估偏向特定基准**：主要评估集中于驾驶、室内、高分辨率等标准数据集，未覆盖更多样化的应用场景（如水下、低光照、多视角大基线等）。

（完）
