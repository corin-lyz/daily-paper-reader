---
title: "PanDA: Towards Panoramic Depth Anything with Unlabeled Panoramas and Mobius Spatial Augmentation"
title_zh: "PanDA: 面向全景的Depth Anything——利用无标注全景和莫比乌斯空间增强"
authors: "Cao, Zidong, Zhu, Jinjing, Zhang, Weiming, Ai, Hao, Bai, Haotian, Zhao, Hengshuang, Wang, Lin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Cao_PanDA_Towards_Panoramic_Depth_Anything_with_Unlabeled_Panoramas_and_Mobius_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 将Depth Anything扩展到全景深度估计
tldr: 针对Depth Anything在全景图像上因球面畸变性能未知的问题，提出PanDA方法，利用无标注全景数据和莫比乌斯空间增强训练深度基础模型。系统评估了全景表示、相机位置和空间变换三类因素。实验表明PanDA显著提升全景深度估计精度，为深度基础模型在非透视场景的泛化提供了新的训练与评估范式。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1713, \"height\": 756, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 856, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 866, \"height\": 481, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 310, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 305, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 853, \"height\": 461, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 857, \"height\": 395, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 860, \"height\": 634, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 856, \"height\": 475, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1806, \"height\": 749, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 859, \"height\": 563, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 329, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 865, \"height\": 441, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 850, \"height\": 372, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1798, \"height\": 633, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 861, \"height\": 613, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 860, \"height\": 613, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 861, \"height\": 366, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cao-panda-towards-panoramic-depth-anything-with-unlabeled-panoramas-and-mobius-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 862, \"height\": 348, \"label\": \"Table\"}]"
motivation: Depth Anything模型在透视图像上具备强大的零样本深度估计能力，但在全景图像上因球面畸变表现未知，尚待系统分析。
method: 提出PanDA，结合无标注全景数据和莫比乌斯空间增强策略，对Depth Anything进行全景自适应训练与评估。
result: 实验表明PanDA在全景深度估计上显著优于原始Depth Anything，并揭示了表示、相机位置与空间变换的影响。
conclusion: 为深度基础模型在全景场景的泛化提供了一套有效的增强与训练方案，扩展了Depth Anything的应用范围。
---

## Abstract
Recently, Depth Anything Models (DAMs) - a type of depth foundation models - have demonstrated impressive zero-shot capabilities across diverse perspective images. Despite its success, it remains an open question regarding DAMs' performance on panorama images that enjoy a large field-of-view (180x360) but suffer from spherical distortions. To address this gap, we conduct an empirical analysis to evaluate the performance of DAMs on panoramic images and identify their limitations. For this, we undertake comprehensive experiments to assess the performance of DAMs from three key factors: panoramic representations, 360 camera positions for capturing scenarios, and spherical spatial transformations. This way, we reveal some key findings, e.g., DAMs are sensitive to spatial transformations. We then propose a semi-supervised learning (SSL) framework to learn a panoramic DAM, dubbed PanDA. Under the umbrella of SSL, PanDA first learns a teacher model by fine-tuning DAM through joint training on synthetic indoor and outdoor panoramic datasets. Then, a student model is trained using large-scale unlabeled data, leveraging pseudo-labels generated by the teacher model. To enhance PanDA's generalization capability, Mobius transformation-based spatial augmentation (MTSA) is proposed to impose consistency regularization between the predicted depth maps from the original and spatially transformed ones. This subtly improves the student model's robustness to various spatial transformations, even under severe distortions. Extensive experiments demonstrate that PanDA exhibits remarkable zero-shot capability across diverse scenes, and outperforms the data-specific panoramic depth estimation methods on two popular real-world benchmarks.

---

## 论文详细总结（自动生成）

# PanDA: 面向全景的Depth Anything——利用无标注全景和莫比乌斯空间增强

## 1. 论文的核心问题与整体含义

**研究动机：**
- Depth Anything Models（DAMs）作为深度估计基础模型，在透视图像上已展现出卓越的零样本泛化能力。
- 然而，全景图像（Panorama）具有大视场（180°×360°）和球面畸变（spherical distortions）两大特性，DAMs在全景图像上的性能表现尚不清楚。
- 全景深度估计数据集（如Matterport3D、Stanford2D3D等）通常场景受限（多为室内），且标注成本高昂，导致现有全景深度估计方法难以泛化到开放世界场景。

**核心问题：**
1. DAMs在全景图像上的表现如何？哪些因素影响其性能？
2. 如何将DAMs的能力有效迁移到全景深度估计任务中，同时保持其零样本泛化能力并增强对球面空间变换的鲁棒性？

**整体含义：**
本文填补了深度基础模型从透视图像扩展到全景图像的研究空白，通过系统性的实证分析揭示DAMs在全景任务中的不足，并提出了一套基于半监督学习的解决方案——**PanDA**，使其成为真正意义上的全景深度基础模型（Panoramic Depth Foundation Model）。

---

## 2. 方法论

### 2.1 核心思想

采用**半监督学习（SSL）**范式，结合少量标注全景数据和大量无标注全景数据：

- 首先微调DAM v2作为教师模型，在合成数据集上学习全景深度先验；
- 再利用教师模型为大规模无标注真实全景数据生成伪标签，训练学生模型；
- 提出**莫比乌斯变换空间增强（MTSA）**，通过一致性正则化增强模型对球面空间变换的鲁棒性。

### 2.2 技术细节与关键组件

#### （1）基于LoRA的教师模型微调
- 使用**Low-Rank Adaptation (LoRA)**微调DAM v2的编码器，避免全量微调带来的灾难性遗忘，保留DAM原有的零样本能力。
- 在合成室内数据集**Structured3D**和室外数据集**Deep360**上联合训练。
- 深度归一化处理：$\hat{d} = \frac{d - d_2}{d_{98} - d_2}$，其中$d_2$和$d_{98}$分别表示有效深度值的2%和98%分位数，输出裁剪到[0.01, 1]区间。

#### （2）赤道感知块归一化损失（EPNL）
- **核心思想**：全景图像中极区（polar regions）畸变严重，全局归一化会压制赤道区域（equator regions）的结构细节。
- **实现方式**：
  - 在预测和真值深度图上随机裁剪K个patch；
  - 使用高斯分布采样补丁，使其集中在赤道附近；
  - 在每个patch内部进行深度归一化后计算L1损失；
  - 同时保证patch在左右边界处连续，保持球面一致性。
- **损失公式**：$L_{EPNL}(d_{x_i}, d_i) = \frac{1}{M}\frac{1}{K}\sum_{i=1}^{M}\sum_{j=1}^{K}|N_j(d_{x_i}) - N_j(d_i)|$

#### （3）半监督训练与学生模型
- **伪标签生成**：教师模型对无标注全景数据预测深度，将输入分辨率提高2倍以增强结构细节；使用SegFormer检测天空区域并将深度置为1.0（最远值）。
- **总损失函数**：$L_{SSL} = L_S + L_P + \lambda_C L_C + \lambda_M L_M$
  - $L_S$：标注数据上的监督损失（SILog + Gradient + EPNL）
  - $L_P$：无标注数据上的伪标签损失
  - $L_C$：颜色增强一致性损失（color jittering等强增强）
  - $L_M$：MTSA一致性损失

#### （4）莫比乌斯变换空间增强（MTSA）
- **数学基础**：莫比乌斯变换是球面上唯一的共形（conformal）且双射（bijective）变换，可统一描述垂直旋转（vertical rotation）和缩放（zoom）两种球面空间变换。
- **实现流程**：
  1. 对输入全景图$u_i$施加莫比乌斯变换$\mathcal{M}(\cdot)$，得到变换后的图像$u_i^m$；
  2. 将$u_i$和$u_i^m$分别输入学生模型，获得深度预测$d_{u_i}$和$d_{u_i}^m$；
  3. 将相同的变换$\mathcal{M}(\cdot)$作用于$d_{u_i}$，与$d_{u_i}^m$计算SILog一致性损失。
- **训练参数**：默认垂直旋转角$\theta$均匀采样自[-10°, 10°]，缩放因子$s$采样自[1.0, 1.5]。
- **作用机制**：MTSA使模型在训练时接触到更多畸变严重的样本，迫使模型学习更鲁棒的球面特征表示，不仅提升变换后图像的深度精度，也改善了原始全景图像的深度估计质量。

---

## 3. 实验设计

### 3.1 使用的数据集

| 数据集 | 类型 | 场景 | 样本数 | 用途 |
|--------|------|------|--------|------|
| Structured3D | 合成 | 室内 | 18,298 | 教师模型训练（标注） |
| Deep360 | 合成 | 室外 | 2,100 | 教师模型训练（标注） |
| ZInD | 真实 | 室内 | 54,034 | 学生模型训练（无标注） |
| 360+x | 真实 | 室内/室外 | 47,956 | 学生模型训练（无标注） |
| Matterport3D | 真实 | 室内 | — | 评估基准 |
| Stanford2D3D | 真实 | 室内 | — | 评估基准 |

### 3.2 对比方法

**零样本透视深度方法：**
- Marigold（基于Stable Diffusion）
- DAM v1（ViT-S/B/L）
- DAM v2（ViT-S/B/L）

**全景深度估计SOTA方法：**
- BiFuse、BiFuse++、UniFuse、HoHoNet、PanoFormer、ACDNet、HRDFuse、S2Net、EGFormer、Elite360D、Depth Anywhere

### 3.3 评估设置

- **评估指标**：AbsRel↓、RMSE↓、δ₁↑、δ₂↑、δ₃↑
- **训练分辨率**：504×1008；**评估分辨率**：512×1024
- **变换评估**：垂直旋转角（10°、20°）和缩放级别（2.0、3.0）
- **零样本评估**：直接测试未见过的真实室内场景
- **微调评估**：在Matterport3D和Stanford2D3D上微调后评测

---

## 4. 资源与算力

论文中关于算力的信息**部分明确**：

- **GPU类型**：A800 GPU（论文明确提及）
- **超参数**：学习率1e-4（Adam优化器），batch size = 4，教师模型训练20 epochs，学生模型训练4 epochs，真实数据集微调30 epochs
- **未说明的内容**：论文**未明确提及**GPU的具体数量、总训练时长（天数/小时数）、模型参数量等细节

---

## 5. 实验数量与充分性

### 已开展的实验

1. **三大实证分析实验**（第3节）：
   - 五种全景表示对比（ERP、CP、TP、HS、VS）× 不同DAM骨干网络
   - 三种相机位置/高度对比实验
   - 11个垂直旋转角度 + 10个缩放级别的空间变换鲁棒性测试

2. **零样本对比实验**：2个数据集 × 3种方法 × 3种骨干 × 5种变换条件

3. **SOTA全景方法对比**：2个数据集 × 11种全景深度估计方法

4. **消融实验**：
   - EPNL有效性：2种骨干 × 2个数据集 × 3组对比（Baseline / +RPNL / +EPNL）
   - SSL损失：5组损失组合对比（Ls / Ls+Lp / Ls+Lp+Lm / Ls+Lp+Lc / Ls+Lp+Lc+Lm）

### 充分性评估

**优点：**
- 分析维度全面（表示、相机位置、空间变换三大因素）
- 消融实验设计层次清晰，逐步验证各组件贡献
- 定量与定性结合，提供误差图、深度梯度图等多角度可视化

**不足之处：**
- 零样本评估仅覆盖**室内场景**（Matterport3D、Stanford2D3D），缺少真实**室外场景**的零样本量化评估（虽然训练集包含室外合成数据）
- 消融实验仅在Matterport3D上报告RMSE指标，未报告多个数据集上的完整指标
- 与透视图像零样本方法对比时，DAM v1/v2使用的微调版本不同（NYU/Hypersim），对比设置的统一性有待加强
- 真实场景的定性结果展示较多，但定量分析集中于室内

---

## 6. 主要结论与发现

### 6.1 实证分析三大发现
1. **ERP表示最佳**：尽管畸变更大，但ERP输入保持了连续完整的语义内容，优于CP、TP、HS、VS等无畸变表示。
2. **极区主导会损害性能**：当相机位置低（如放地面），极区物体占比大，会严重影响赤道区域的深度预测精度。
3. **DAMs对空间变换鲁棒性差**：垂直旋转和缩放均导致性能显著下降，旋转超过约40°后性能急剧恶化。

### 6.2 方法有效性结论
- **PanDA显著超越原始DAM**：零样本设置下，PanDA相对DAM v2在RMSE上降低14%~56%，特别是在大幅旋转和缩放条件下优势更明显。
- **PanDA优于所有SOTA全景方法**：在Matterport3D上，PanDA-L的RMSE达0.3305（此前最佳HRDFuse为0.4433）；在Stanford2D3D上，PanDA-L的RMSE达0.2540（此前最佳HRDFuse为0.3106）。
- **EPNL有效**：相比随机patch归一化（RPNL），EPNL在多数指标上更优，验证了赤道优先采样策略的有效性。
- **MTSA是模型鲁棒性的关键**：消融实验表明，加入MTSA后RMSE显著下降（从0.6678降至0.5781@10°旋转），而单独的color augmentation作用甚微。

---

## 7. 优点

1. **系统性的实证分析先行**：在三方面因素（表示、相机位置、空间变换）上进行深入评估，发现DAMs的局限性，为后续方法设计提供了坚实的实验依据，这比直接提出方法更有学术价值。
2. **创新的MTSA方案**：利用莫比乌斯变换的数学性质（共形、双射），统一处理旋转和缩放变换，设计简洁而有效。MTSA不仅提升了变换鲁棒性，还改善了原始条件的深度精度。
3. **半监督训练框架合理**：借助教师模型生成伪标签 + 对学生模型施加一致性正则，有效利用了大规模无标注全景数据，符合深度基础模型的发展趋势。
4. **EPNL针对全景特性设计**：将赤道区域优先与局部归一化结合，解决了全局归一化细节被压制的问题，设计思路清晰，且在ERP左右边界保持连续性。
5. **LoRA微调策略**：在保留DAM零样本能力的同时适应全景域，避免了过拟合和灾难性遗忘。
6. **实验充分详实**：多种骨干网络（ViT-S/B/L）、多个基准数据集、多种变换条件下的全面评估，并提供了大量可视化结果。

---

## 8. 不足与局限

1. **零样本评估场景覆盖有限**：定量评估主要集中在室内场景（Matterport3D、Stanford2D3D），虽然目标是"开放世界"（open-world），但缺乏真实室外全景深度基准（如Panoptic 3D或Depth360等）的量化对比。
2. **算力细节不透明**：未明确说明训练所需的GPU数量、总训练时长，影响可复现性评估。
3. **伪标签质量依赖教师模型**：半监督学习中伪标签错误可能被放大；虽然使用了SegFormer检测天空区域，但其他区域的伪标签噪声问题未深入讨论。
4. **数据偏差风险**：无标注数据以室内为主（ZInD为室内），360+x虽包含室外但占比不明。学生模型可能对室外场景存在域偏差。
5. **MTSA覆盖的变换类型有限**：仅包含垂直旋转和缩放，未考虑水平旋转（虽然等价于水平滚动）、球面平移等其他莫比乌斯变换形式。
6. **消融实验报告不完整**：SSL消融仅在Matterport3D数据集上报告RMSE指标，缺少多数据集、多指标的全面消融。
7. **与深度基础模型的对比还不够广泛**：未与Depth Anything v2的Metric版本、Metric3D等最新深度基础模型进行全景性能对比。
8. **实际应用限制**：训练分辨率504×1008不高，面对高分辨率全景（如8K）可能存在性能下降；推理速度未讨论，可能难满足实时应用需求。

**（完）**
