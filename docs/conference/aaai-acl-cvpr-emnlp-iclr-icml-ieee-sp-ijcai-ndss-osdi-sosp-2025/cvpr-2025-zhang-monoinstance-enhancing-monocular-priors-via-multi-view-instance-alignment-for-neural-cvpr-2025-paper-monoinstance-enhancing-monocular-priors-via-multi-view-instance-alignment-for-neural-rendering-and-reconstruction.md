---
title: "MonoInstance: Enhancing Monocular Priors via Multi-view Instance Alignment for Neural Rendering and Reconstruction"
title_zh: MonoInstance：通过多视图实例对齐增强神经渲染与重建中的单目先验
authors: "Zhang, Wenyuan, Yang, Yixiao, Huang, Han, Han, Liang, Shi, Kanle, Liu, Yu-Shen, Han, Zhizhong"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhang_MonoInstance_Enhancing_Monocular_Priors_via_Multi-view_Instance_Alignment_for_Neural_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 5.0
evidence: 增强单目深度先验以服务多视图任务
tldr: 单目深度先验在神经渲染与重建中被广泛使用，但跨视图预测不一致且有噪声。MonoInstance通过多视图实例对齐并建模深度不确定性，筛选可靠的几何先验。该方法为多视图任务提供了增强的深度引导，有助于提升重建与渲染的准确性和一致性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1802, \"height\": 935, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 857, \"height\": 399, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 851, \"height\": 491, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1808, \"height\": 735, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 853, \"height\": 670, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1791, \"height\": 934, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 849, \"height\": 543, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1805, \"height\": 1481, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1808, \"height\": 735, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 777, \"height\": 404, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-monoinstance-enhancing-monocular-priors-via-multi-view-instance-alignment-for-neural-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 163, \"label\": \"Table\"}]"
motivation: 神经渲染和重建中广泛使用单目深度先验，但逐视图预测不一致且包含噪声，直接作为真值监督会引入误差。
method: 提出探索单目深度不确定性并利用多视图实例对齐为神经渲染和重建提供增强几何先验。
result: 未在摘要中给出具体结果，但表明能提供增强的几何先验，改善多视图一致性与重建质量。
conclusion: 为多视图任务中更有效地利用单目深度先验提供了一种通用方法。
---

## Abstract
Monocular depth priors have been widely adopted by neural rendering in multi-view based tasks such as 3D reconstruction and novel view synthesis. However, due to the inconsistent prediction on each view, how to more effectively leverage monocular cues in a multi-view context remains a challenge. Current methods treat the entire estimated depth map indiscriminately, and use it as ground truth supervision, while ignoring the inherent inaccuracy and cross-view inconsistency in monocular priors. To resolve these issues, we propose MonoInstance, a general approach that explores the uncertainty of monocular depths to provide enhanced geometric priors for neural rendering and reconstruction. Our key insight lies in aligning each segmented instance depths from multiple views within a common 3D space, thereby casting the uncertainty estimation of monocular depths into a density measure within noisy point clouds. For high-uncertainty areas where depth priors are unreliable, we further introduce a constraint term that encourages the projected instances to align with corresponding instance masks on nearby views. MonoInstance is a versatile strategy which can be seamlessly integrated into various multi-view neural rendering frameworks. Our experimental results demonstrate that MonoInstance significantly improves the performance in both reconstruction and novel view synthesis under various benchmarks.

---

## 论文详细总结（自动生成）

# MonoInstance：通过多视图实例对齐增强神经渲染与重建中的单目先验（CVPR 2025）——论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：神经渲染与三维重建（如 NeRF、3D Gaussian Splatting）通常依赖多视图 RGB 图像，但仅靠光度一致性难以恢复精细几何，存在“形状-辐射歧义”问题。为此，近年方法普遍引入单目深度、法线等先验作为额外监督。
- **问题**：单目先验存在两个固有缺陷：
  - **预测不准确**：由于域差异，单目深度估计本身有噪声。
  - **跨视图不一致**：每个视图独立预测，同一物体在不同视角下的深度不一致。
- **现有方案的不足**：
  - 当前方法将整张深度图“一视同仁”地用作真值监督，忽略了其中不可靠区域。
  - 基于 MVS 的交叉投影不确定性方法易受遮挡影响。
  - 在渲染框架内联合预测不确定性的方法（如 DebSDF）与渲染质量耦合，受渲染效果干扰，且通常针对室内场景设计，难以推广。
- **核心目标**：提出一种**通用**策略，从多视图单目深度中自动估计不确定性，从而为各类神经渲染和重建框架提供增强的几何先验，提升重建精度和新视角合成质量。
- **整体含义**：MonoInstance 将“单目深度可靠性估计”转化为“三维点云密度测量”问题，利用多视图实例对齐来揭示先验噪声，是可即插即用（plug-and-play）的通用增强模块。

## 2. 方法论

### 2.1 核心思想

- 同一物体在不同视图下的单目深度若一致，其反投影点在三维空间中会**紧密聚集**在真实表面附近；若不一致，则会**分散**形成各向异性、方差较大的点云。
- 因此，可以通过测量三维邻域内的点云密度来估计单目深度的不确定性：**密度越高，深度越可靠，不确定性越低**。
- 为避免不同物体尺度对密度估计的影响，采用**实例级**（instance-level）对齐，而非整幅图。

### 2.2 关键技术步骤

#### （1）多视图一致实例分割

- 使用预训练模型（如 [52]）获得每张图像的实例分割。
- 将不同视图中的实例两两连接构成图：若两个实例的反投影深度点云之间的 Chamfer Distance 足够小，则添加边。
- 通过图聚类算法（[55]）将各视图中的实例归并为一致的三维实例簇。
- 对室内场景，利用 GroundedSAM [53] 识别并过滤背景实例，假设无纹理区域（如墙面）的单目深度通常可靠，将其不确定性设为零。

#### （2）不确定性估计（多视图实例对齐与点密度测量）

- 将每个实例在多视图下的单目深度反投影到世界坐标系，形成融合点云。
- 先将点云下采样到固定数量（实验中为 30,000），以解耦点数量与视图数量的关系。
- 对每个实例分割区域内的像素，用 ball query [51] 计算其反投影点在邻域内的密度。
- 邻域半径定义为：
  - \( r = \text{Vol}(B_{\text{opt}}(P)) + 0.01 \)，其中 \( B_{\text{opt}}(P) \) 是融合点云的最小有向包围盒 [1]，Vol 为其体积。
- 密度在同一实例所有帧的所有查询点中归一化：
  \[
  d(p(u,v)) = \frac{d(p(u,v))}{\max_{(u,v) \in S_i} d(p(u,v))}
  \]
- 逐像素不确定性：
  \[
  U_i(u,v) = 1 - d(p(u,v))
  \]
- 对每个实例依次估计后，组装得到所有视图的完整不确定性图。

#### （3）自适应先验损失（Adaptive Prior Loss）

- 利用不确定性图 \( U \) 加权单目深度/法线与渲染深度/法线之间的差异，降低不可靠区域的监督权重，减少错误先验的负面影响。
- 总损失中，深度损失 \( L_d \) 和法线损失 \( L_n \) 采用不确定性加权。

#### （4）基于不确定性的实例掩码约束（Instance Mask Constraint）

- 对于参考视图 \( I_r \) 中高不确定性区域 \( S_i^r \) 发射的光线，将光线上的采样点 \( \{p_j\} \) 投影到邻近视图 \( I_n \)。
- 仅保留落在邻近视图实例掩码 \( S_i^n \) 内的投影点，并用这些点在 \( I_n \) 上的插值颜色及其预测不透明度 \( \alpha_j \) 渲染颜色：
  \[
  \hat{C}_{\text{sil}}^n = \sum_{j=1}^K \mathbb{1}_j \cdot I_n[\pi_n(p_j)] \alpha_j \prod_{l<j}(1-\alpha_l)
  \]
  其中 \( \mathbb{1}_j = 1 \) 当 \( \pi_n(p_j) \in S_i^n \)，否则为 0。
- 该渲染颜色与参考视图 GT 颜色计算损失，隐式约束采样点与物体表面对齐。与 Pixel Warping [9] 相比，仅累积落在目标实例掩码内的点，从而更精准。

#### （5）不确定性引导的光线采样（Uncertainty-Guided Ray Sampling）

- 将不确定性图作为采样概率，更多地关注高不确定性区域。
- 每个实例的采样像素数按其分割面积分配。
- 概率定义为：
  \[
  \text{prob}_i(u,v) = U_i(u,v) + 0.05
  \]
  其中 0.05 保证零不确定性区域仍有一定采样。

### 2.3 训练流程与损失函数

- 两阶段训练：
  1. **第一阶段**：均匀使用单目深度先验，学习场景的粗略表示；渲染低分辨率深度图对齐多视图单目深度尺度；然后计算所有实例的不确定性并组装不确定性图。
  2. **第二阶段**：将不确定性图集成到训练中，使用引导式光线采样、自适应深度损失和实例掩码约束。
- 总损失：
  \[
  L = L_{\text{color}} + \lambda_1 L_{\text{eik}} + \lambda_2 L_{\text{sil}} + \lambda_3 L_d + \lambda_4 L_n
  \]
  其中 \( L_{\text{eik}} \) 为 Eikonal 项，\( L_{\text{sil}} \) 为实例掩码约束，\( L_d, L_n \) 为自适应深度/法线损失。超参数：\( \lambda_1=0.1, \lambda_2=0.4, \lambda_3=0.5, \lambda_4=0.05 \)。
- 该方法可适配 NeRF-based 和 3DGS-based 框架（文中以 NeRF 管道为例，3DGS 差异见补充材料）。

## 3. 实验设计

### 3.1 任务与数据集

| 任务 | 数据集 | 场景/规模 | 评价指标 |
|---|---|---|---|
| 密集视图 3D 重建 | ScanNet [7] | 4 个场景（200–400 帧） | Chamfer Distance (Acc↓, Comp↓), F-score, Precision, Recall |
| 密集视图 3D 重建 | Replica [58] | 全部 8 个场景 | Acc↓, Comp↓, CD↓, Normal Consistency↑, F-score↑ |
| 稀疏视图 3D 重建 | DTU [18] | 15 个场景，每个场景 3 个视图，小重叠 | Chamfer Distance (CD)↓ |
| 稀疏新视角合成 | LLFF [39] | 8 个前向场景，3 个训练视图，8 倍降采样 | PSNR↑, SSIM↑, LPIPS↓ |

### 3.2 对比方法

- **密集视图重建**：UNISURF [45]、MonoSDF [80]、HybridNeRF（SDF-OCC-Hybrid）[33]、H2O-SDF [47]、DebSDF [71]、RS-Recon [78]。注：H2O-SDF 源码未开源，Replica 上无结果。
- **稀疏视图重建**：COLMAP [56]、SparseNeuS [30]、VolRecon [54]、ReTR [28]、NeuSurf [16]、NeuSurf†（加入单目先验）、UFORecon [41]。
- **稀疏新视角合成**：RegNeRF [42]、FreeNeRF [74]、3DGS [20]、DNGaussian [21]、FSGS [97]、COR-GS [82]。

### 3.3 实现细节

- 密集重建基于 MonoSDF [80] 代码实现。
- 单目先验默认使用 Metric3D v2 [14]，消融中对比 Omnidata [10]、GeoWizard [12]。
- 邻近视图按观测角度差异选取。
- DTU 上无需多视图一致分割，直接分割前景物体与背景，仅计算中心物体的不确定性。

### 3.4 主要实验结果

- **ScanNet**：Ours 在 F-score 上达 0.834，优于所有基线（DebSDF 0.785，RS-Recon 0.794，HybridNeRF 0.779）。
- **Replica**：Ours 在 Normal Consistency 上达 0.937，F-score 0.918（与 RS-Recon 接近），CD 0.026 与最优持平。
- **DTU**：Ours CD=1.18，优于 NeuSurf†(1.30)、UFORecon(1.43)、COLMAP(2.61) 等。
- **LLFF**：Ours PSNR=20.73, SSIM=0.731, LPIPS=0.184，均为最佳，明显优于 COR-GS (20.45/0.712/0.196)。

## 4. 资源与算力

- 论文正文**未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- 仅在实现细节中提及基于 MonoSDF 和 NeuSurf 的官方代码，未报告具体硬件环境或时间开销。
- 因此，无法从论文中获取精确的算力数据。

## 5. 实验数量与充分性

- **实验数量**：
  - 3 个主要任务（密集重建、稀疏重建、稀疏新视角合成），共 4 个数据集（ScanNet、Replica、DTU、LLFF）。
  - 消融实验 2 组：
    1. 模块消融（Base → +不确定性 → +自适应采样 → +掩码约束），在 ScanNet 上进行。
    2. 单目先验模型选择消融（Omnidata / Metric3D v2 / GeoWizard），对比 MonoSDF 与 Ours。
  - 可视化和不确定性图展示若干。
- **充分性评价**：
  - **优点**：覆盖了两种主流神经渲染框架（NeRF-based 和 3DGS-based）、密集/稀疏输入、重建/合成多种任务，且对比方法较新，实验设计较全面。
  - **不足**：
    - 消融仅在 ScanNet 上进行，未在 Replica/DTU/LLFF 上验证各模块的泛化贡献。
    - 稀疏视图重建仅在 3 视图小重叠的 DTU 子集上，未测试更多视图数或真实场景。
    - 对 3DGS 框架的适配在正文中仅给出结果，方法差异放在补充材料，正文实验细节展示有限。
    - 与 H2O-SDF 的对比不完整（Replica 缺失），可能影响公平性排名。

## 6. 主要结论与发现

- 通过将多视图单目深度反投影并做实例级对齐，点密度能有效反映深度先验的可靠性，从而得到精确的不确定性图。
- 基于不确定性加权深度/法线损失、引导光线采样以及实例掩码约束，能够显著提升高不确定性区域（如细腿、花朵、灯饰等复杂结构）的重建质量。
- MonoInstance 是通用策略，可无缝集成到 NeRF 和 3DGS 等不同多视图神经渲染框架中，并在多个基准上取得 SOTA 性能。
- 即使使用不同单目先验模型（Omnidata、Metric3D v2、GeoWizard），所提方法均能一致地超越直接使用该先验的基线（MonoSDF），证明了方法的鲁棒性和通用性。

## 7. 优点

- **创新的不确定性建模**：将单目深度不确定性从逐视图估计改为跨视图对齐后的三维点密度估计，避免了渲染质量和遮挡干扰，且直观可解释。
- **实例级处理**：利用多视图一致分割，避免不同尺度物体对密度估计的影响，使不确定性更精细。
- **通用性强**：可作为“插件”用于 NeRF、3DGS 等多种框架，并非针对单一任务设计。
- **多任务验证充分**：在重建（密集/稀疏）和合成（稀疏视角）两大方向、四个数据集上实验，且与多种最新方法对比，结果全面领先。
- **可视化清晰**：展示了不确定性图与 GT、单目深度、最终结果之间的对比，直观证明方法能捕捉先验错误区域。
- **对不准确先验的纠正能力强**：最终质量超过单目先验本身的精度，说明策略不仅“避错”还能“补全”。

## 8. 不足与局限

- **计算开销未报告**：多视图一致分割、点云下采样、球查询密度估计等额外步骤会带来时间和内存开销，论文未提供运行效率和资源消耗的量化分析。
- **依赖预训练模型**：实例分割、单目深度/法线估计均依赖大型预训练模型，其失败（如分割不准确）可能影响不确定性图的质量；对野外复杂场景的鲁棒性未知。
- **实验范围限制**：主要集中在室内（ScanNet/Replica）和单物体（DTU）场景；室外大尺度场景、动态场景未涉及。
- **稀疏视图下仅 3 视图**：DTU 实验为小重叠 3 视图，未探索更极端（如 2 视图）或更宽松（如 6 视图）情况下的表现。
- **消融不跨数据集**：模块消融仅在 ScanNet 上，无法确认各模块在其他场景中的贡献是否一致。
- **公平性隐患**：部分基线（H2O-SDF）缺少 Replica 结果，且对 NeuSurf† 的复现基于“额外单目提示”的公平设定，但仍存在实现细节差异的可能。
- **方法复杂度较高**：需要额外的分割、聚类、点云处理流程，实际部署时管线较复杂，论文对失败案例分析较少。

（完）
