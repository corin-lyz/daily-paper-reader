---
title: "MVSAnywhere: Zero-Shot Multi-View Stereo"
title_zh: MVSAnywhere：零样本多视图立体
authors: "Izquierdo, Sergio, Sayed, Mohamed, Firman, Michael, Garcia-Hernando, Guillermo, Turmukhambetov, Daniyar, Civera, Javier, Mac Aodha, Oisin, Brostow, Gabriel, Watson, Jamie"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Izquierdo_MVSAnywhere_Zero-Shot_Multi-View_Stereo_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 7.0
evidence: 跨域与跨深度范围的零样本多视图立体深度估计
tldr: 针对多视图立体在跨域场景上泛化不足、深度范围未知的问题，提出MVSAnywhere架构，利用Transformer架构并设计元数据处理方式以适应可变数量的输入视图，同时估计有效深度范围。实验显示该方法可在室内外等不同域和深度范围上实现零样本泛化，其可扩展设计有利于从稀疏到稠密视图的迁移使用，为通用多视图立体模型提供了新方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 874, \"height\": 1165, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 740, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 872, \"height\": 305, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 849, \"height\": 528, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 857, \"height\": 262, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 873, \"height\": 234, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1769, \"height\": 1350, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 277, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1786, \"height\": 1240, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 700, \"height\": 214, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 860, \"height\": 284, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-izquierdo-mvsanywhere-zero-shot-multi-view-stereo-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 858, \"height\": 387, \"label\": \"Table\"}]"
motivation: 多视图立体跨域泛化差，深度范围未知。
method: 设计Transformer架构，结合元数据并估计深度范围，实现任意场景零样本泛化。
result: 跨领域与深度范围零样本立体深度估计效果良好。
conclusion: 为通用多视图立体深度估计提供了可行架构。
---

## Abstract
Computing accurate depth from multiple views is a fundamental and longstanding challenge in computer vision.However, most existing approaches do not generalize well across different domains and scene types (e.g. indoor vs outdoor). Training a general-purpose multi-view stereo model is challenging and raises several questions, e.g. how to best make use of transformer-based architectures, how to incorporate additional metadata when there is a variable number of input views, and how to estimate the range of valid depths which can vary considerably across different scenes and is typically not known a priori? To address these issues, we introduce MVSA, a novel and versatile Multi-View Stereo architecture that aims to work Anywhere by generalizing across diverse domains and depth ranges. MVSA combines monocular and multi-view cues with an adaptive cost volume to deal with scale-related issues. We demonstrate state-of-the-art zero-shot depth estimation on the Robust Multi-View Depth Benchmark, surpassing existing multi-view stereo and monocular baselines.

---

## 论文详细总结（自动生成）

# 论文总结：MVSAnywhere: Zero-Shot Multi-View Stereo

## 1. 核心问题与整体含义

- **研究背景**：多视图立体（MVS）是 3D 视觉中的基础问题，旨在从多张已标注位姿的 RGB 图像中估计稠密深度，是三维重建、自动驾驶等应用的关键模块。
- **核心问题**：现有 MVS 方法大多针对特定场景/领域训练（如仅室内或仅驾驶场景），对训练分布之外的场景泛化能力较弱；同时，传统成本体（cost volume）方法依赖预先设定的深度范围（depth range），而实际场景的深度范围差异巨大（从室内几十厘米到室外上百米），无法预先统一设定。
- **文章目标**：引入通用 MVS 方法 **MVSA（Multi-View Stereo Anywhere）**，使其在任意场景、任意深度范围、任意源视图数量下实现 **零样本（zero-shot）** 深度估计，并具备 3D 一致性。

## 2. 方法论

- **总体架构**：MVSA 是一种基于 Transformer 的 MVS 网络，核心思路是同时利用多视图几何线索和单目图像的强语义先验，由五个部分组成：特征提取器、成本体、参考图像编码器、Mono/Multi Cue Combiner（单目/多目线索融合器）和深度解码器。
- **特征提取与成本体**：
  - 使用 ResNet18 提取参考/源图像特征。
  - 依据一组深度假设，将各源视图特征 warp 到参考视图，构建成本体。
  - 成本体不仅包含 warp 特征，还融入几何元数据（相对位姿、射线方向、深度值、可见性掩码等）。
- **参考图像编码器**：采用 Depth Anything V2 的 ViT-Base 编码器（预训练权重），提取参考图像的强单目特征，增强遮挡/弱匹配区域的鲁棒性。
- **创新点 1：视图数量无关的元数据聚合**
  - 传统方法（如 SimpleRecon）要求固定数量的源视图，实用性受限。
  - MVSA 对每个源视图，独立使用一个 MLP 处理参考视图与该源视图的元数据，输出一个匹配得分和一个权重；随后对 N 个得分做加权求和（权重经 softmax），得到成本体值。
  - 该设计天然支持任意数量的源视图，提升灵活性。
- **创新点 2：尺度无关的元数据归一化**
  - 将相对位姿信息按所有源视图的最大值归一化，使特征对场景尺度变化不敏感，适应 SfM 场景中的非度量坐标系。
  - 深度预测采用对数空间映射：  
    `D_hat = exp(log(d_min) + log(d_max / d_min) · sigmoid(x))`
- **创新点 3：自适应成本体（级联深度范围估计）**
  - 推理时，先根据相机内外参推断最小/最大可匹配深度，在对数空间均匀采样离散深度假设，构建初始成本体并预测初步深度；再用初步预测的最小/最大深度重建成本体，得到更高精度的最终深度。
  - 训练时对真实深度范围加入随机扰动，增强模型在深度范围估计不精确时的鲁棒性。
- **创新点 4：Cost Volume Patchifier（成本体补丁化）**
  - 将成本体转换为 ViT 输入 token 时，不使用简单的朴素卷积下采样，而是引入参考图像编码器低层特征（前两个 block）与成本体特征拼接，指导下采样过程，保留更多细节。
- **训练设置**：
  - 损失函数：SimpleRecon 的监督损失（对数深度 L1 + 梯度 + 法线一致性损失）。
  - 训练数据：8 个多样化的合成 MVS 数据集，涵盖室内、室外、航空、动态场景等，总计百万级图像对。

## 3. 实验设计

- **评测基准**：Robust Multi-View Depth Benchmark（RMVDB），包含 5 个多样化的多视图深度数据集：KITTI（驾驶）、ScanNet（室内）、ETH3D（建筑/场景）、DTU（桌面物体）、Tanks and Temples（大规模场景）。
- **评价指标**：绝对相对深度误差（rel ↓）和 τ 内点率（τ = 1.03³ ↑），均按像素计算。
- **对比方法（按输入条件分为 4 组）**：
  - (a) 无位姿的帧间深度方法：DeMoN、DeepV2D、MAST3R 等；
  - (b) 有位姿+已知每图像深度范围的方法：MVSNet、Vis-MVSNet、PatchmatchNet、MVSFormer++ 等；
  - (c) 单目深度方法：Depth Pro、Metric3D、UniDepthV1/V2、Depth Anything V2；
  - (d) 有位姿但无每图像深度范围的方法（MVSA 所属组）：Fast-MVSNet、MVS2D、Robust MVD Baseline、MAST3R+三角化等。
- **额外评测**：
  - RMVDB 变体实验（改进 ScanNet 关键帧选择、ETH3D 校正）。
  - ScanNet Mesh Evaluation 三维重建质量评测（completion、accuracy、chamfer、precision、recall、F-score）。
  - 消融实验（9 组）验证各模块组件有效性。
  - 位姿尺度鲁棒性实验（在补充材料中，将 ScanNet 位姿缩放 ×100）。

## 4. 资源与算力

- **论文正文与提取文本中未明确披露** GPU 型号、数量、训练时长等具体算力信息。
- 仅可推断其属于大规模训练体系：使用 8 个大型合成数据集的混合数据、ViT-Base 级模型、640×480 输入分辨率、64 个深度 bins，并结合 DAV2/DINOv2 预训练权重，应需要多卡 GPU 集群，但文中未给出细节。

## 5. 实验数量与充分性

- **实验数量**：
  - 主实验：在 5 个零样本数据集上与超过 20 种基线方法对比；
  - 消融实验：9 组，覆盖架构（ViT 大小、CNN 替代）、流程（无 bin 细化、无噪声）、特征融合（朴素 patchify、无单目特征）等多个维度；
  - 重建实验 + benchmark 变体实验 + 尺度鲁棒性实验，整体较为丰富。
- **充分性评价**：
  - **优点**：对比组划分清晰（按输入信息类型），区分公平；消融覆盖全面，能验证各核心设计的贡献；补充了 3D 重建验证，提升立体一致性结论的可信度。
  - **注意点/潜在偏差**：
    - 部分基线方法在评估数据集上训练过（表中用括号标出），导致分数虚高，但作者已明确标注，处理相对透明；
    - 单目方法（带 †）获得真实内参，且部分通过最小二乘对齐，与 MVSA 直接预测度量深度不完全可比；
    - MAST3R 变体（+三角化）依赖批量三角化，计算开销大，与 MVSA 的推理效率比较不够深入；
    - 基线选择与超参数调优是否对 MVSA 更有利（例如 MVSFormer++ 重新训练后性能提升）值得关注。

## 6. 主要结论与发现

- MVSA 在 RMVDB 上取得 **state-of-the-art 零样本深度估计结果**，在 KITTI、ScanNet、ETH3D、DTU 和 Tanks and Temples 均达到最优或次优水平，平均 rel 降至 2.7、τ 达 77.0，优于所有 MVS 和单目基线。
- 在改进版 RMVDB 上优势更明显（如 ETH3D rel：1.27），表明其在更真实评估条件下鲁棒性好。
- 在 ScanNet 网格重建评测中，虽然未在 ScanNet 上训练，MVSA 仍优于多个在 ScanNet 上训练过的方法，证实其深度预测具有更好的 3D 一致性。
- 消融实验表明：ViT-B 结构、视图/尺度无关元数据、级联深度范围细化、DAV2 初始化、预训练特征引导的 patchifier 等均为关键贡献。

## 7. 优点

- **真正的零样本设计**：在训练数据不包含任何评测数据集的前提下，跨室内/室外/驾驶/航空场景均表现优异，验证了强泛化能力。
- **架构设计新颖且实用**：
  - 视图数量无关的元数据聚合突破固定帧数限制；
  - 尺度归一化适应任意深度范围/坐标系；
  - 级联成本体解决深度范围未知问题；
  - Cost Volume Patchifier 巧妙地将单目特征融入 ViT patchification 过程，兼顾效率与细节。
- **单目+多目互补**：借助预训练单目大模型（DAV2、DINOv2）先验，增强遮挡、纹理缺失区域的深度预测质量。
- **实验全面**：同时覆盖深度精度、3D 重建质量、消融分析，结论更扎实。
- **开源友好**：提供代码和预训练模型，方便后续研究复现与扩展。

## 8. 不足与局限

- **依赖相机位姿**：MVSA 需要已知的内参和外参，无法直接用于无位姿的场景（如单张图片或未知位姿的视频序列），限制了部分应用场景。
- **未显式建模时序一致性**：虽然多视图融合带来一定一致性，但作者承认未直接训练时间一致性，连续视频深度可能出现闪烁/不一致，后续可结合视频低成本深度模型。
- **训练依赖合成数据**：全部基于合成数据训练，实时合成与真实照片的分布差距可能影响在极端真实场景的表现（虽然实验表明泛化良好）。
- **算力信息缺失**：未披露训练资源、GPU 配置、训练时长和峰值显存，不利于复现成本评估和公平的效能对比。
- **性能与速度平衡未充分讨论**：推理时间较 MAST3R+三角化明显更快（0.12s vs 0.72s），但相对于 Fast-MVSNet 等轻量方法的实时性优势，对嵌入式/移动端部署的可行性讨论不足。
- **深度范围估计基于相机几何**：在相机位姿噪声较大或极度稀疏视图（重叠极少）时，初始深度范围估计可能退化，对位姿质量有一定隐式依赖。

（完）
