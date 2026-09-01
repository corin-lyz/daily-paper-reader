---
title: "Stereo Anywhere: Robust Zero-Shot Deep Stereo Matching Even Where Either Stereo or Mono Fail"
title_zh: Stereo Anywhere：在单目或双目各自失效处仍鲁棒的零样本深度立体匹配
authors: "Bartolomei, Luca, Tosi, Fabio, Poggi, Matteo, Mattoccia, Stefano"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Bartolomei_Stereo_Anywhere_Robust_Zero-Shot_Deep_Stereo_Matching_Even_Where_Either_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 零样本双目立体匹配结合单目深度基础模型先验，针对弱纹理、遮挡等挑战，契合双目深度主题。
tldr: 现有立体匹配在弱纹理、遮挡和非朗伯表面等场景容易失效，而单目深度先验可提供补充线索。本文提出Stereo Anywhere，用双分支结构同时利用立体几何约束与单目深度基础模型，并通过新的代价体融合机制处理上述挑战。仅用合成数据训练，在多个基准的零样本泛化测试中达到当前最佳，甚至能应对立体与单目单独都失败的情形。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1418, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1783, \"height\": 553, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1870, \"height\": 247, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1655, \"height\": 642, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1651, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 805, \"height\": 573, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1786, \"height\": 251, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1817, \"height\": 405, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1789, \"height\": 543, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bartolomei-stereo-anywhere-robust-zero-shot-deep-stereo-matching-even-where-either-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 849, \"height\": 339, \"label\": \"Table\"}]"
motivation: 立体匹配在弱纹理和遮挡下易失效，单目深度先验可提供补充但未被有效利用。
method: 构建双分支框架融合立体几何与单目深度视觉基础模型，并设计新颖代价体融合机制。
result: 仅合成训练即在多个基准零样本测试中达到SOTA，包含光学幻觉数据集MonoTrap上的验证。
conclusion: 立体与单目先验互补可显著提升零样本鲁棒性，是深度估计的重要方向。
---

## Abstract
We introduce Stereo Anywhere, a novel stereo-matching framework that combines geometric constraints with robust priors from monocular depth Vision Foundation Models (VFMs). By elegantly coupling these complementary worlds through a dual-branch architecture, we seamlessly integrate stereo matching with learned contextual cues. Following this design, our framework introduces novel cost volume fusion mechanisms that effectively handle critical challenges such as textureless regions, occlusions, and non-Lambertian surfaces. Through our novel optical illusion dataset, MonoTrap, and extensive evaluation across multiple benchmarks, we demonstrate that our synthetic-only trained model achieves state-of-the-art results in zero-shot generalization, significantly outperforming existing solutions while showing remarkable robustness to challenging cases such as mirrors and transparencies.

---

## 论文详细总结（自动生成）

# 详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：立体匹配（Stereo Matching）在以下两类典型场景中表现不佳：
  - **跨域泛化能力有限**：现有的深度立体模型虽然能在合成数据上取得较好效果，但从合成域迁移到真实场景时性能显著下降。根本原因之一是立体匹配训练数据的“数量与多样性”远不及其他视觉任务（如单目深度估计）。
  - **固有匹配难点**：大范围无纹理区域（如室内墙面）、遮挡区域，以及非朗伯表面（镜子、玻璃等透明或反射材质）都会使传统立体匹配的“像素对应→三角化”基本假设失效，导致深度估计出现严重错误。
- **关键洞见**：与立体匹配形成鲜明对比的是，**单目深度估计领域**近年来借助海量训练数据（百万级样本）和Vision Foundation Models（VFMs）取得了重大突破，例如Depth Anything系列。这类模型依赖上下文线索而非像素匹配，因此**天然对无纹理区域、非朗伯表面和遮挡具有更强的鲁棒性**。
- **研究目标**：作者提出，既然单目深度VFM恰好能缓解立体匹配的痛点，而立体匹配的几何约束又恰好能弥补单目深度估计的固有缺陷（如**尺度模糊**和**透视错觉**），那么将二者**互补地融合**起来，有望构建一个“在任何地方都可用”的鲁棒零样本立体匹配框架。
- **论文含义**：提出了名为 **Stereo Anywhere** 的新型立体匹配框架，通过双分支架构将立体几何约束与单目深度VFM先验优雅地耦合，实现即使在“立体单独失效或单目单独失效”的场景下依然保持准确深度估计的能力。

## 2. 方法论：核心思想、关键技术细节与算法流程

### 总体框架

Stereo Anywhere 以 RAFT-Stereo 的**迭代式立体匹配架构**为基础，整体由三个主要阶段组成：

1. **特征提取（Feature Extraction）**
2. **相关性体金字塔构建（Correlation Pyramids Building）**
3. **迭代式视差估计（Iterative Disparity Estimation）**

### 2.1 特征提取

- **图像特征**：使用预训练的共享特征编码器分别处理左、右视图（$I_L, I_R$），得到特征图 $F_L, F_R$，用于构建立体相关性体。
- **上下文特征**：使用与图像特征编码器结构相同、但**针对单目深度图重新训练**的上下文编码器，处理经对齐后的参考单目深度图 $\hat{M}_L$，以提取强几何先验信息。

### 2.2 双分支相关性体构建

论文的核心创新在于同时构建**两个互补的相关性体金字塔**：

- **立体相关性体（Stereo Correlation Volume）**：基于左右图像特征 $F_L, F_R$ 的点积构建，编码跨视图的图像相似度：
  
  $$(V_S)_{ijk} = \sum_h (F_L)_{hij} \cdot (F_R)_{hik}, \quad V_S \in \mathbb{R}^{\frac{H}{4} \times \frac{W}{4} \times \frac{W}{4}}$$
  
  该体在非朗伯表面上可能不可靠（因为特征匹配失效）。

- **单目相关性体（Monocular Correlation Volume）**：基于左右单目深度估计 $M_L, M_R$ 的**法线图** $\nabla_L, \nabla_R$ 的点积构建：
  
  $$(V_M)_{ijk} = \sum_h (\nabla_L)_{hij} \cdot (\nabla_R)_{hik}, \quad V_M \in \mathbb{R}^{\frac{H}{4} \times \frac{W}{4} \times \frac{W}{4}}$$
  
  由于法线图不含纹理，$V_M$ 信息量有限，因此引入**相对深度先验**生成左右分割掩码 $M_L, M_R$（8个类别），利用掩码将 $V_M$ 分段（$V_{M_n}$），再送入3D CNN正则化模块 $\phi_A$ 聚合，得到聚合后的单目体 $V'_M$，最终通过两个浅层3D卷积层分别生成体积 $V_{DM}$（用于视差估计）和 $V_{CM}$（用于置信度估计）。

### 2.3 可微分单目深度缩放（Differentiable Monocular Scaling）

- **目标**：单目VFM输出的是**仿射不变深度**（或任意尺度深度），必须将其对齐到立体视差空间才能与立体信息融合。
- **实现**：
  - 通过 softargmax 从 $V_{DM}$ 中提取左/右粗视差图 $\hat{D}_L, \hat{D}_R$；
  - 从 $V_{CM}$ 中利用**信息熵**计算置信度图 $\hat{C}_L, \hat{C}_R$（低熵即单峰清晰代价曲线代表高置信度）；
  - 使用 SoftLRC 算子剔除遮挡像素的置信度；
  - 利用**可微分加权最小二乘法**联合估计尺度 $\hat{s}$ 和偏移 $\hat{t}$：
  
  $$\min_{\hat{s}, \hat{t}_{L,R}} \left\| \sqrt{\hat{C}} \odot \left[(\hat{s}M + \hat{t}) - \hat{D}\right] \right\|_F$$
  
  - 最终得到可与立体视差空间直接比较的缩放后视差图 $\hat{M}_L, \hat{M}_R$。

### 2.4 体增强与体截断（Volume Augmentations & Truncation）

- **动机**：仅凭 SceneFlow 训练数据，网络无法学会“何时信任立体信息、何时信任单目信息”。
- **三种体增强策略**：
  1. **Volume Rolling**：对 $V_{DM}$ 或 $V_S$ 的最后一个维度（视差方向）进行随机滚动操作；
  2. **Volume Noising**：向体添加均匀随机噪声；
  3. **Volume Zeroing**：在零视差位置施加高斯型抑制曲线，使该部分代价失效。
- **额外的单目增强**：训练时随机用归一化的真值深度替代单目VFM输出。
- **体截断（关键创新）**：针对镜面（mirror）场景——立体匹配在镜面上容易预测过远的视差（因为镜子看起来像通向另一个空间的窗口），作者设计了一个**截断掩码** $T$，判定条件为：单目预测视差大于立体预测视差且单目置信度高，或单目置信度高但立体置信度低。当 $T > 0.98$ 时，使用以单目预测视差为中心的 sigmoid 曲线**截断立体相关性体**，只保留不“穿透”镜面的立体匹配代价。

### 2.5 迭代式视差估计

- 扩展 RAFT-Stereo 的 Multi-GRU 更新算子，引入**第二个查找算子**，从单目体积 $V_{DM}$ 中提取额外相关性特征 $G_M$；
- 将立体特征 $G_S$ 和单目特征 $G_M$ 共同输入编码器，与当前视差估计特征拼接后送入 ConvGRU，迭代优化视差；
- 继承凸上采样模块将最终视差恢复到全分辨率。
- 公式原型可表示为：
  
  $$D = \phi_S(I_L, I_R, M_L, M_R)$$

### 2.6 训练损失

- 迭代模块：带指数递增权重的 L1 损失（继承自 RAFT-Stereo）；
- $\hat{D}_L, \hat{D}_R, \hat{M}_L, \hat{M}_R$：L1 损失；
- $\hat{C}_L, \hat{C}_R$：二值交叉熵（BCE）损失。

## 3. 实验设计：数据集、基准与对比方法

### 3.1 训练数据

- **唯一训练数据**：SceneFlow 合成数据集（约 39k 对立体图像，含稠密真值视差）。
- **单目VFM**：Depth Anything v2（Large 权重，仅在 HyperSim 合成数据上训练），作为**冻结的**单目先验来源。

### 3.2 评估数据集

| 类别 | 数据集 | 说明 |
|------|--------|------|
| 标准零样本泛化 | Middlebury 2014（半分辨率） | 高分辨率室内场景，15对图像 |
| | Middlebury 2021 | 24对图像 |
| | ETH3D | 27个低分辨率室内/外场景 |
| | KITTI 2012 / 2015 | 室外驾驶场景，稀疏LiDAR真值 |
| 非朗伯表面 | Booster（四分之一分辨率） | 228对高分辨率室内图像，含镜子/透明表面 |
| | LayeredFlow（八分之一分辨率） | 400对含透明物体的图像，稀疏真值 |
| 单目失效场景 | **MonoTrap（新提出）** | 26个含透视错觉场景，LiDAR真值，专门用于挑战单目深度估计 |

### 3.3 对比方法

- **立体匹配SOTA**：RAFT-Stereo、PSMNet、GMStereo、ELFNet、PCVNet、DLNR、Selective-RAFT、Selective-IGEV、IGEV-Stereo、NMRF等（均使用作者提供的零样本预训练权重）。
- **单目深度估计**：Depth Anything v2、Depth Pro（在MonoTrap数据集上对比）。

### 3.4 评估指标

- 立体：平均视差误差（Avg.）和 bad > τ 像素百分比（τ = 1/2/3/5 等），分别计算 All（全部像素）、Noc（非遮挡）和 Occ（遮挡像素）；
- 单目（MonoTrap）：AbsRel、RMSE 和 δ < 1.05 分数。

## 4. 资源与算力

- **GPU**：单张 NVIDIA A100 GPU。
- **训练时长**：3 个 epoch。
- **学习率**：1e-4，AdamW 优化器。
- **批大小**：2 张图像（裁剪尺寸 320×640）。
- **其他细节**：GRU 迭代次数训练时固定为 12，推理时增至 32。
- **说明**：论文未详细披露更多算力信息（如总训练wall-clock时间、VFM推理开销等）。

## 5. 实验数量与充分性分析

### 实验概况

论文包含**四大类实验**，整体较为丰富：

1. **消融实验**（Table 1）：从基线（RAFT-Stereo）出发，逐步添加上下文、再训练、法线相关性体/缩放深度、体增强与截断共 5 个变体（A→E），在 Booster 和 Middlebury 2014 上验证每个组件的贡献。共 5 组消融对比。
2. **零样本泛化**（Table 2）：在 5 个标准数据集上，与 10 种 SOTA 立体模型对比。
3. **非朗伯零样本泛化**（Table 3）：在 Booster 和 LayeredFlow 上与 10 种 SOTA 对比；额外加入在 Booster 训练集上微调后的在线排行榜对比（带有 DKT-RAFT 参考）。
4. **MonoTrap 基准**（Table 4）：与单目模型（Depth Anything v2、Depth Pro）及 RAFT-Stereo 基线对比。

### 充分性评价

- **优点**：实验覆盖全面，同时考察了“立体失败场景”（非朗伯/弱纹理）和“单目失败场景”（透视错觉），验证了方法的双向鲁棒性；零样本协议规范，符合领域惯例（仅用 SceneFlow 训练）；消融设计合理，能清晰归因每个模块的贡献；定性结果（Figure 4/5/6）提供了直观证据。
- **不足之处**：
  - **场景覆盖偏差**：测试集偏重室内场景（Middlebury、Booster、LayeredFlow、MonoTrap），室外动态场景仅依靠 KITTI；缺乏更具挑战性的户外非朗伯场景（如车载玻璃、水洼反光等）。
  - **消融数量有限**：没有对“体增强三种策略”分别消融，而是合并为一个整体（D→E），无法区分 Rolling/Noising/Zeroing 各自的贡献。
  - **单目VFM消融缺失**：未验证不同VFM（如 Depth Anything v1、Metric3D、Depth Pro）替换对最终性能的影响，对 VFM 选择的敏感性未知。
  - **公平性注意**：MonoTrap 上对比单目模型时使用 RANSAC 对齐（†）与最小二乘对齐两套方案，但不同对齐策略下指标差异较大，说明单目模型在该数据集上的绝对数值需谨慎解读。
  - **模型效率未报告**：对比方法均为 SOTA 精度导向模型，但论文未报告推理时间或参数量，对于“零样本鲁棒性”之外的实用性讨论不足。

## 6. 主要结论与发现

- **Stereo Anywhere 在零样本迁移中全面超越 SOTA**：仅用 SceneFlow 合成数据训练，在 Middlebury 2014 上 bad-2 All 达到 6.96%（比第二名 DLNR 低约 2.5%），在 KITTI 2015 上 bad-3 达到 3.93%（突破 4% 大关）。
- **遮挡区域改善显著**：Occ 指标在所有数据集上以明显优势胜出（Middlebury 2014 相对第二名提升约 6%），证明单目先验有效弥补了立体匹配在遮挡区域的病态问题。
- **非朗伯表面鲁棒性大幅增强**：Booster 上 bad-2 从 RAFT-Stereo 的 17.84% 降至 9.01%；LayeredFlow 上 bad-1 从 89.21% 降至 81.83%（虽然绝对数值仍高，但相对改善显著）。在 Booster 训练集微调后在线排行榜上达到第一。
- **立体几何约束可“纠偏”单目VFM的透视错觉**：在 MonoTrap 数据集上，单目模型（Depth Anything v2、Depth Pro）严重受骗（AbsRel 高达 53.46%），而 Stereo Anywhere 的 AbsRel 仅 3.50%，优于纯立基线 RAFT-Stereo（5.01%）。
- **核心结论**：单目深度VFM的先验与立体几何约束之间存在良好的互补性，融合二者可以显著提升零样本深度估计的鲁棒性和泛化能力，形成“1+1>2”的效果。

## 7. 方法优点与亮点

1. **思路新颖**：将单目深度VFM作为“第二目”引入立体匹配框架，而非简单后处理或伪标签，是一种架构级别的深度融合。
2. **设计精妙**：
   - 利用**法线图**构建单目相关性体，将仿射深度转化为局部几何表示，天然对尺度不敏感；
   - **可微分缩放模块**联合优化左右两侧的尺度与偏移，既完成单目→立体坐标对齐，又保持了左右一致性；
   - **体截断机制**是针对镜面场景精心设计的启发式先验，简单而高效。
3. **数据高效**：仅需少量合成数据（SceneFlow）训练即可实现跨域鲁棒性，标注成本极低。
4. **双向鲁棒**：同时针对立体失效（非朗伯/遮挡）和单目失效（透视错觉）进行了验证，实证了两个模态的互补性。
5. **新数据集贡献**：提出 MonoTrap 光学错觉立体数据集（26 个场景），为未来研究提供了新的评估基准。
6. **开源精神**：提供了项目主页（stereoanywhere.github.io），便于复现与后续研究。

## 8. 不足与局限

1. **依赖外部VFM的可靠性**：Stereo Anywhere 的单目分支强依赖 Depth Anything v2 的预测质量。虽然论文展示了在 MonoTrap 上能抑制 VFM 错误，但在更广泛的不常见场景（如极端光照、传感器特异性伪影）中，VFM 的错误可能更难被识别和拒绝。
2. **非朗伯表面绝对性能仍不理想**：LayeredFlow 上 bad-1 仍高达 81.83%，虽然相对提升显著，但绝对精度距离实际应用仍有巨大差距；遮挡区域的错误率（Occ 约 20~29%）也仍偏高。
3. **MonoTrap 规模较小**：26 个场景相对有限，且均为室内受控场景，视角与内容多样性不足，可能无法全面反映真实世界中透视错觉的复杂性。
4. **训练与推理开销**：需要同时运行单目VFM（Large 模型）和立体网络，推理时 GRU 迭代 32 次，对实时性要求高的场景（如自动驾驶、AR）可能不够友好；论文未报告推理速度。
5. **VFM 选择缺乏消融**：未分析不同单目VFM（不同架构、不同训练数据）对最终性能的敏感性，方法的泛化边界尚不明确。
6. **针对特定场景的手工设计**：体截断操作是基于“镜面导致立体视差偏大”这一特定经验的启发式，对更复杂的非朗伯场景（如半透明物体、多重反射）可能不够通用。
7. **室内/外分布偏差**：训练和多数评估集中在室内场景，室外复杂动态环境的验证不足；单目VFM 在室外远景的尺度模糊也可能影响缩放模块的稳定性。

（完）
