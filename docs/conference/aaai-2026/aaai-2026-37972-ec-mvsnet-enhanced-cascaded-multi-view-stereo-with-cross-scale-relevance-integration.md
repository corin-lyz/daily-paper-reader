---
title: "EC-MVSNet: Enhanced Cascaded Multi-View Stereo with Cross-Scale Relevance Integration"
title_zh: "EC-MVSNet: 增强级联多视角立体与跨尺度相关性融合"
authors: "Shaoqian Wang, Jiadai Sun, Bin Fan, Qiang Wang, Bin Lu, Yuchao Dai"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37972/41934"
tags: ["query:stereo-depth"]
score: 7.0
evidence: 多视角立体深度估计与跨尺度融合，与双目深度估计相关
tldr: 针对级联多视角立体（MVS）方法跨尺度信息利用率低的问题，提出增强级联MVS框架（EC-MVSNet）。设计跨尺度特征联合构建（CFC）模块协同相邻尺度特征，提升深度估计精度。实验证明该方法在多个MVS基准上取得最优性能，其跨尺度策略可用于双目深度估计场景。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1847, \"height\": 683, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1844, \"height\": 1018, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 882, \"height\": 534, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 873, \"height\": 372, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1730, \"height\": 458, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37972/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 879, \"height\": 398, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37972/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 853, \"height\": 675, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37972/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 848, \"height\": 447, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37972/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 832, \"height\": 363, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37972/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1804, \"height\": 716, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37972/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 855, \"height\": 272, \"label\": \"Table\"}]"
motivation: 现有级联MVS方法各尺度独立运算，未充分利用跨尺度相关性。
method: 提出跨尺度特征联合构建（CFC）模块，协同融合相邻尺度特征。
result: 在DTU、Tanks and Temples等数据集上取得最先进的深度重建精度。
conclusion: 跨尺度融合显著提升MVS深度估计的准确性和鲁棒性。
---

## Abstract
Cascade-based multi-scale architectures are currently the mainstream in Multi-view Stereo (MVS), achieving a balance between computational efficiency and reconstruction accuracy. However, existing cascade MVS methods suffer from significant limitations in cross-scale information utilization, where depth estimation processes operate independently across scales without fully exploiting the rich relevance between adjacent scales. To address this fundamental limitation, we propose an Enhanced Cascade Multi-View Stereo framework (EC-MVSNet), which introduces a novel cross-scale relevance integration strategy. Specifically, we introduce a Cross-Scale Feature-based Joint Construction (CFC) module to synergistically combine features from adjacent scales to build more reliable cost volumes. Additionally, a Cross-Scale Probability-guided Enhancement (CPE) module is proposed to propagate depth probability distributions across scales to guide cost volume enhancement. Furthermore, we propose a Monocular Feature-based Refinement (MFR) module to further enhance depth prediction accuracy by leveraging monocular priors. Extensive experiments demonstrate that EC-MVSNet achieves state-of-the-art performance on multiple benchmarks, validating the effectiveness of the cross-scale integration in improving MVS reconstruction quality.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前基于级联（cascade）的多视角立体（MVS）方法虽然平衡了计算效率与重建精度，但各尺度的深度估计过程相互独立，未能充分利用相邻尺度之间丰富的相关性信息（如特征和概率分布）。这种跨尺度信息利用的局限性限制了级联框架的潜力。
- **研究动机**：为了突破这一限制，作者提出增强级联MVS框架（EC-MVSNet），通过显式融合跨尺度相关性来提升深度重建质量。该工作属于学习型MVS领域，旨在从多张图像中恢复高精度三维几何。
- **整体含义**：通过设计跨尺度特征联合构建（CFC）、跨尺度概率引导增强（CPE）和单目特征细化（MFR）三个模块，实现全面的跨尺度信息交互，显著提升重建精度，并在多个主流基准上达到最优性能。

### 2. 论文提出的方法论：核心思想、关键技术细节

#### 核心思想
在级联多尺度框架中，不仅利用前一尺度的深度范围来收缩当前尺度的搜索空间，更进一步在特征构建、代价体增强和深度细化阶段主动融合相邻尺度的信息。

#### 关键技术细节
- **特征提取器**：基于FPN网络，从参考图像和源图像中提取多尺度特征（尺度k=0,1,2,3），特征图尺寸为 \(H/2^{3-k} \times W/2^{3-k}\)。
- **CFC模块（跨尺度特征联合构建）**：
  - 在尺度k，利用当前尺度特征 \(F_k\) 和前一尺度特征 \(F_{k-1}\) 分别构建两个代价体 \(C'_k\) 和 \(C'_{k-1}\)（基于相同的深度假设）。
  - 通过子像素特征匹配（式1-2）进行可微分单应变换。
  - 采用组内相关（Group-wise correlation）和可学习权重 \(w_{i,k}\) 计算代价（式3-4）。
  - 用3D CNN处理两个代价体后聚合得到最终代价体 \(C_k\)。
- **CPE模块（跨尺度概率引导增强）**：
  - 利用前一尺度的深度概率分布图 \(P_{k-1}\)，通过线性插值采样出当前尺度深度范围 \(R_k\) 对应的局部概率分布 \(P'_{k-1}\)。
  - 将 \(P'_{k-1}\) 与当前代价体 \(C_k\) 通过3D CNN处理并拼接，再通过跳跃连接增强代价体。
- **MFR模块（单目特征细化）**：
  - 利用预训练的DepthAnythingV2模型提取参考图像的单目特征（多尺度）。
  - 将该特征与正则化后的初始概率分布拼接，通过2D CNN得到细化后的概率分布 \(P_k\)。
  - 通过Soft-argmax得到深度图 \(D_k\)。
  - 采用Mask Upsample策略，结合单目特征对深度图进行上采样和优化，生成 \(Du_k\) 传递到下一尺度。
- **损失函数**：结合交叉熵损失（监督概率分布）和L1损失（监督深度图），如式5所示。

### 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - **DTU**：室内多视角立体数据集，采用距离指标（Accuracy, Completeness, Overall，越低越好）。
  - **Tanks and Temples**：大规模场景重建基准，包含Intermediate（8个场景）和Advanced（6个场景），采用F1分数（百分比，越高越好）。
  - **ETH3D**：高分辨率多视角图像，包含Training和Testing集，采用F1、Precision、Recall。
- **基准**：在DTU上与19种方法对比（如MVSFormer++、RRT-MVS、CasMVSNet等）；在Tanks and Temples上与17种方法对比；在ETH3D上与8种方法对比。
- **对比方法**：包括经典级联方法（CasMVSNet）、近期SOTA方法（MVSFormer++、RRT-MVS、GoMVS等）。

### 4. 资源与算力

- 论文明确提到：
  - 训练硬件：两块NVIDIA GeForce RTX 3090 GPU。
  - 优化器：Adam，学习率使用OneCycleLR策略。
  - 训练流程：先在DTU训练集上训练15个epoch，后在BlendedMVS上微调10个epoch。
  - 输入图像尺寸：DTU训练时640×512；微调时768×576。
- 未提供总训练时间或显存的具体数值（但消融实验中给出了单次推理时间和内存占用）。

### 5. 实验数量与充分性

- **实验组数**：一共进行了4类主要实验：
  1. **DTU定量对比**（表1）：与19种方法对比，验证总体性能。
  2. **Tanks and Temples定量对比**（表2）：在Intermediate和Advanced集上对比，包括14个场景的F1分数。
  3. **ETH3D定量对比**（表3）：在Training和Testing集上对比。
  4. **消融实验**（表4、表5）：
     - 表4：对CFC、CPE、MFR三个模块进行逐个消融，共7种组合。
     - 表5：对CFC模块中不同特征聚合方式（低尺度、高尺度、高低、全尺度）进行消融，包括性能、时间和内存。
- **充分性与公平性**：
  - 实验覆盖了MVS领域最主要的三个基准，且与多种最新方法公平对比。
  - 消融实验系统分析了各模块贡献及聚合策略的权衡，客观展示了计算开销与精度的关系。
  - 定性可视化（图5、图6）也支持了定量结果。
  - 总体实验设计较为充分、客观。

### 6. 论文的主要结论与发现

- EC-MVSNet在DTU、Tanks and Temples、ETH3D三个基准上均达到当前最优（SOTA）性能。
  - DTU Overall指标从0.281（MVSFormer++）降至0.275。
  - Tanks and Temples Intermediate F1从68.16（RRT-MVS）提升至69.32；Advanced F1从43.29提升至44.63。
  - ETH3D上F1和Precision均为最优。
- 跨尺度相关性融合策略（CFC、CPE、MFR三个模块协同）是性能提升的关键，其中CPE模块单独使用时效果最显著（Overall 0.284）。
- 单目先验（MFR模块）进一步提升了深度图的连续性和边缘细节。

### 7. 优点

- **方法创新性**：首次在级联MVS中系统性地融合相邻尺度的特征、概率分布和单目先验，设计新颖，思路清晰。
- **实验全面性**：在三个主流基准上进行对比，并进行了详细的消融实验（模块级、策略级），验证了每个设计的有效性。
- **性能领先**：在多个数据集上取得SOTA，特别是DTU的Accuracy和Overall指标均有提升。
- **计算效率合理**：CFC模块的聚合方式经过分析，选择了在性能和效率间较优的方案（低尺度特征聚合），而非盲目追求高精度增加开销。

### 8. 不足与局限

- **应用局限**：论文仅针对多视角立体重建，未讨论其对双目立体匹配等任务的迁移性（虽然元数据提到相关但并未在方法中体现）。
- **实验覆盖**：虽然数据集较多，但缺少真实场景下动态或遮挡严重等挑战性案例的专门分析；消融实验中仅在DTU上进行，未在Tanks and Temples上做类似消融，可能削弱泛化性证明。
- **偏差风险**：单目先验模块依赖预训练的DepthAnythingV2，该模型在特定场景下可能存在偏见（如低纹理区域），论文未分析此影响。
- **计算资源**：论文未报告完整训练时间，且消融实验显示全尺度聚合会消耗大量显存（9.4GB）和时间，实际部署需权衡。
- **代码未公开**：虽然提供了GitHub链接，但截至回答时尚未验证是否完全公开。

（完）
