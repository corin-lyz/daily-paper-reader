---
title: Synthetic-to-Real Self-supervised Robust Depth Estimation via Learning with Motion and Structure Priors
title_zh: 运动与结构先验引导的合成到真实自监督鲁棒深度估计
authors: "Yan, Weilong, Li, Ming, Li, Haipeng, Shao, Shuwei, Tan, Robby T."
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Yan_Synthetic-to-Real_Self-supervised_Robust_Depth_Estimation_via_Learning_with_Motion_and_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 自监督单目深度估计，提升户外复杂天气鲁棒性
tldr: 户外白天、雨天、夜间等条件下单目深度估计的泛化困难且缺乏标注数据。本文提出首个合成到真实的鲁棒深度估计框架，在合成适配阶段利用成本体积内的运动与结构先验迁移真实知识。所提方法有效应对光照与天气变化，在多个户外基准上提升自监督深度估计的鲁棒性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 861, \"height\": 352, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1785, \"height\": 1037, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1585, \"height\": 701, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1765, \"height\": 831, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 853, \"height\": 194, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 819, \"height\": 542, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 572, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1541, \"height\": 555, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yan-synthetic-to-real-self-supervised-robust-depth-estimation-via-learning-with-motion-and-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1667, \"height\": 375, \"label\": \"Table\"}]"
motivation: 户外复杂天气下自监督单目深度估计泛化差，且难以获得真实恶劣环境标注。
method: 提出合成到真实适配框架，在成本体积中引入运动与结构先验迁移知识，合成数据训练后自适应真实场景。
result: 在昼夜、雨夜等多样化户外条件下取得鲁棒性提升，超越此前方法。
conclusion: 为复杂户外环境的自监督深度估计提供了有效的合成到真实迁移方案。
---

## Abstract
Self-supervised depth estimation from monocular cameras in diverse outdoor conditions, such as daytime, rain, and nighttime, is challenging due to the difficulty of learning universal representations and the severe lack of labeled real-world adverse data.Previous methods either rely on synthetic inputs and pseudo-depth labels or directly apply daytime strategies to adverse conditions, resulting in suboptimal results.In this paper, we present the first synthetic-to-real robust depth estimation framework, incorporating motion and structure priors to capture real-world knowledge effectively. In the synthetic adaptation, we transfer motion-structure knowledge inside cost volumes for better robust representation, using a frozen daytime model to train a depth estimator in synthetic adverse conditions.In the innovative real adaptation which targets to fix synthetic-real gaps, models trained earlier identify the weather-insensitive regions with a designed consistency-reweighting strategy to emphasize valid pseudo-labels.We further introduce a new regularization by gathering explicit depth distribution prior to constrain the model facing real-world data.Experiments show that our method outperforms the state-of-the-art across diverse conditions in multi-frame and single-frame settings. We achieve improvements of 7.5% in AbsRel and 4.3% in RMSE on average for nuScenes and Robotcar datasets (daytime, nighttime, rain). In zero-shot evaluation on DrivingStereo (rain, fog), our method generalizes better than previous ones. Our code will be released soon.

---

## 论文详细总结（自动生成）

## 论文详细总结

### 1. 核心问题与研究动机

- **问题背景**：自监督单目深度估计在户外多样条件下（白天、夜晚、雨天、雾天等）面临两大挑战：
  - 不同环境退化模式差异大，难以学习统一且鲁棒的表征；
  - 真实世界中的恶劣天气数据缺乏标注，难以直接监督训练。
- **现有方法的不足**：
  - 有些方法依赖合成数据与伪深度标签，但仅从深度伪标签学习难以捕获跨条件的通用表征（特征异质性高）；
  - 有些方法直接将白天训练策略套用于恶劣条件，违反光度一致性假设；
  - 单纯在合成数据上训练存在合成到真实域差距（synthetic-to-real gap），导致泛化受限。
- **本文目标**：提出首个**合成到真实（Synthetic-to-Real）**的自监督鲁棒深度估计框架，通过利用运动与结构先验，使模型能有效适应真实世界中的多种恶劣环境。

---

### 2. 方法论

#### 2.1 总体思路

框架包含两个核心阶段（如图 2 所示）：

1. **合成适配（Synthetic Adaptation, SA）**：在合成恶劣条件下训练模型，将白天域知识通过成本体积（cost volume）迁移到恶劣条件；
2. **真实适配（Real Adaptation, RA）**：进一步利用真实恶劣数据微调模型，缩小合成与真实域之间的差距。

#### 2.2 基础：白天自监督深度模型

- 采用 **Manydepth** 作为白天基线，其使用多帧成本体积提供几何引导；
- 白天深度网络 $\Phi_{D-day}$ 和位姿网络 $\Phi_{P-day}$ 通过自监督方式在白天数据上预训练；
- 成本体积 $cv$ 通过特征点积（而非差值）构建，提升鲁棒性：
  $$cv = \Phi_{D-day}(I_t) \cdot W(\Phi_{D-day}(I_{t-1}), T, d, K)$$

#### 2.3 合成适配阶段（SA）

- 使用 GAN 或扩散模型 $\Phi_{aug}$ 将白天图像转换为多种合成恶劣条件图像 $I_{C_i-syn}$；
- 采用教师-学生蒸馏方式，用冻结的白天模型产出的伪深度标签 $D_{day}$ 监督合成域深度网络 $D_{syn}$：
  $$L_d = \frac{1}{HW}\sum \frac{|D_{day} - D_{syn}|}{D_{syn}}$$
- **关键创新**：除了深度损失外，同时蒸馏**成本体积**（motion-structure 空间）：
  $$L_{cv} = \frac{1}{chw}\sum |cv_{day} - cv_{syn}|$$
  同时用 $L_T$ 约束位姿；
- 总损失：$L_{syn} = \alpha_1 L_d + \alpha_2 L_{cv} + \alpha_3 L_T$；
- 动机：成本体积天然过滤冗余信息，保留显式的运动与结构特征，是比原始特征更优的迁移空间。

#### 2.4 真实适配阶段（RA）

- **一致性重加权（Consistency Reweighting）**：
  - 使用白天模型 $\Phi_{D-day}$ 和合成域模型 $\Phi_{D-syn}$ 分别对真实恶劣图像预测深度 $D_{day}$ 与 $D_{syn}$；
  - 计算一致性置信度图：
    $$C_{cst} = e^{-\beta \frac{|D_{syn} - D_{day}|}{D_{syn}}}$$
    $$W_{cst} = C_{cst} + \epsilon$$
  - 重加权后的深度监督损失：
    $$L_{cd} = \frac{1}{HW}\sum W_{cst} \frac{|D_{real} - D_{syn}|}{D_{real}}$$
  - 目的：强调两模型一致的区域（更可靠的伪标签），弱化不一致区域（伪标签噪声）。

- **结构先验约束（Structure Prior Constraint）**：
  - 观察到夜间/雨天预测深度分布与白天分布存在系统性差异（夜间偏向近平面、雨天偏向远平面）；
  - 将白天训练集上的深度预测分布 $Dist_{day}$ 作为参考分布，使用**可微直方图**计算深度分布；
  - 通过 KL 散度约束恶劣条件下深度分布向白天分布对齐：
    $$L_{KL}^{dis} = \sum_{i=1}^{N} P_{adv}(i) \cdot \log \frac{P_{adv}(i)}{P_{day}(i)}$$
  - 总损失：$L_{real} = \alpha_1 L_{cd} + \alpha_2 L_{KL}^{dis} + \alpha_3 L_{cv} + \alpha_4 L_T$。

#### 2.5 推理阶段

- 可工作于**单帧**（输入同一帧两次）和**多帧**（输入连续帧）两种模式；
- 无需额外传感器（如雷达）。

---

### 3. 实验设计

#### 3.1 数据集与评估场景

| 数据集 | 条件 | 用途 |
|---|---|---|
| **nuScenes** | 白天、夜间、雨天 | 训练 + 测试（单帧和多帧评估） |
| **Robotcar** | 白天、夜间 | 训练 + 测试（单帧评估） |
| **DrivingStereo** | 雨天、雾天 | 零样本测试（zero-shot，仅测试） |

- 评估指标：AbsRel、SqRel、RMSE、δ1；
- 深度范围：nuScenes 为 0.1–80m，Robotcar 为 0.1–50m，DrivingStereo 为 0.1–80m。

#### 3.2 对比方法

- 单帧方法：Monodepth2、RNW、md4all-AD、robust-depth、md4all-DD、DM-MDE、DeFeatNet、ADIDS、WSGD 等；
- 多帧方法：Manydepth、Manydepth + md4all 伪标签（自设基线 [33]+[8]）；
- 额外信息方法：R4Dyn（使用雷达）。

#### 3.3 主要结果

- **nuScenes**：单帧模式相比 SOTA（md4all、DM-MDE）在 AbsRel 上平均提升 4.8%、SqRel 提升 16.8%、RMSE 降低 4.2%；多帧模式下也超过雷达辅助的 R4Dyn；
- **Robotcar**：单帧模式下 AbsRel 平均提升 7.8%、SqRel 提升 6.5%、RMSE 降低 2.7%；
- **DrivingStereo**（零样本）：雨天条件下 AbsRel 提升 3.0%，雾天条件下同样优于对比方法；
- 总体：在 nuScenes 和 Robotcar 上较 SOTA 平均改进 **AbsRel 7.5%**、**RMSE 4.3%**。

#### 3.4 消融实验

在 nuScenes 夜间和雨天条件下对以下模块进行了消融：

- SA（仅深度伪标签蒸馏）→ +Lcv → +RA（仅伪标签）→ +一致性重加权 CR → +结构先验约束 $L_{KL}^{dis}$；
- 额外对比了直接蒸馏特征空间（$L_{feat}$），证明成本体积蒸馏优于直接特征蒸馏；
- 结果显示每个模块都能带来逐步的性能提升，验证了设计的有效性。

---

### 4. 资源与算力

- 论文 **未明确说明** 所用的 GPU 型号、数量及具体训练时长；
- 仅提及：$\Phi_{day}$ 和 $\Phi_{syn}$ 各训练 20 epochs，$\Phi_{real}$ 由 $\Phi_{syn}$ 初始化训练 10 epochs，学习率 $1\times10^{-4}$，每 5 epochs 衰减 0.5；
- 基础网络统一使用 ResNet 架构以保证对比公平；
- 文中未说明具体硬件环境，因此无法评估实际算力开销。

---

### 5. 实验数量与充分性

- **数据集覆盖**：3 个公开数据集、覆盖 5 种条件（白天、夜间、雨天、雾天），较为全面；
- **评估模式**：单帧和多帧两种设置均有覆盖；
- **对比方法**：与 10+ 种现有 SOTA 方法对比，包含基于雷达的方法和多种 UDA 方法；
- **消融实验**：包含核心模块的逐步消融（Tab. 4），并设计了公平基线 [33]+[8]；
- **零样本泛化**：在 DrivingStereo 上验证了模型未见过数据的泛化能力；
- **充分性评价**：
  - **优点**：实验覆盖了主要恶劣天气场景，多数据集、多模式、多指标的评估体系完整；消融实验清晰揭示每个模块的贡献；
  - **不足**：缺少在极端恶劣条件下（如大雪、雾霾中的夜间）的测试；缺少对不同深度范围设置（如只预测近处或远处）的敏感性分析；消融实验仅报告了夜间和雨天，未报告白天条件下的消融结果；缺乏对伪标签噪声水平的定量估计。

---

### 6. 主要结论

- 本文提出的合成到真实（Synthetic-to-Real）两阶段适配框架能有效提升自监督单目深度估计在多样恶劣条件下的鲁棒性；
- **成本体积是比原始特征更优的知识迁移空间**——天然过滤冗余信息、保留运动-结构特征；
- **一致性重加权策略**能有效从合成域模型和白天模型的预测中筛选可靠伪标签，缓解合成-真实域差距；
- **白天深度分布作为结构先验**能有效约束真实恶劣条件下的深度预测分布，改善系统性偏差（夜间偏近、雨天偏远）；
- 模型在多帧和单帧设置下均取得 SOTA 性能，且在零样本评估中展现出较强泛化能力，说明该框架学到了更具通用性的深度表征。

---

### 7. 优点

1. **首次提出"合成到真实"的完整学习流水线**，不再是仅在合成数据上训练，而是在真实数据上进一步适配；
2. **探索了成本体积这一辅助空间**用于知识蒸馏，理论动机清晰（过滤冗余、保留运动和结构信息），实验验证优于直接特征蒸馏；
3. **一致性重加权策略设计巧妙**——利用两个冻结模型的预测一致性自动加权伪标签，类似"交叉验证"的思想，有效降低伪标签噪声；
4. **结构先验约束具有洞察性**：通过分析不同条件下的深度分布差异（夜间偏近、雨天偏远），引入可微直方图 + KL 散度的显式分布约束；
5. **不依赖额外传感器**：在 nuScenes 上，仅凭单目视觉便超过了使用雷达信息的 R4Dyn；
6. **与公平设计的基线 [33]+[8] 相比**，验证了性能来源是训练策略本身，而非多帧骨干结构的优势；
7. 定性结果展示了直观场景（夜间道路指示牌、雨天车辆模糊、雾天轮胎痕迹、雨天反光）说明模型实际处理问题的能力。

---

### 8. 不足与局限

1. **无条件类型的全面覆盖**：未涉及雪天、沙尘、极端暴雨等场景；夜间+雨天的组合场景也未单独评估；
2. **依赖合成数据生成模型**：SA 阶段依赖 GAN/扩散模型的质量，如果生成的合成恶劣数据域差距过大，会影响后续迁移效果；文中未讨论对生成模型质量的敏感性；
3. **结构先验是"静态"的**：论文使用整个训练集的白天深度分布作为固定参考，未考虑场景类别（如高速路 vs 城市街道）的分布差异；KL 散度对齐可能在某些场景（如隧道）产生次优约束；
4. **一致性重加权的假设局限**：假设两模型在"真实可靠区域"一致的逻辑虽合理，但对于两个模型都失败的共同盲区，一致性策略无法识别和纠正；
5. **资源与可复现信息不足**：未报告具体 GPU 配置和训练耗时，给复现带来一定困难；
6. **消融实验有限**：仅展示夜间和雨天两种条件下的消融，白天和其他极端条件下的消融未给出；
7. **深度范围受限**：所有实验限制在 3.5–80m（或 50m），对近处（<3m）和远处（>80m）的预测质量未作说明，实际自动驾驶中近处感知同样关键。

---

（完）
