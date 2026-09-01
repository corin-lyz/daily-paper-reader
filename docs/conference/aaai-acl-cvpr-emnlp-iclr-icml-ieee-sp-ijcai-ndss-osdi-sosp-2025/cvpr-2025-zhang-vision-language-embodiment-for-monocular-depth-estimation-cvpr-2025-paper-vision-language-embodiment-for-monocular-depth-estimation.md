---
title: Vision-Language Embodiment for Monocular Depth Estimation
title_zh: 视觉语言具身化用于单目深度估计
authors: "Zhang, Jinchang, Lu, Guoyu"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhang_Vision-Language_Embodiment_for_Monocular_Depth_Estimation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 视觉语言具身化的单目深度估计
tldr: 单目深度估计存在固有不确信性，现有模型常忽略相机自身信息。该文提出将相机模型及其物理特性具身到深度学习模型中，仅利用相机内参即可与道路环境实时交互计算具身场景深度。结合 RGB 图像特征后，模型在无需额外设备的情况下提升深度估计精度。该思路为单目深度估计引入新的感知维度。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1711, \"height\": 699, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 813, \"height\": 517, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 819, \"height\": 294, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 832, \"height\": 514, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 829, \"height\": 164, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 832, \"height\": 239, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 835, \"height\": 247, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 835, \"height\": 472, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 835, \"height\": 302, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhang-vision-language-embodiment-for-monocular-depth-estimation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 844, \"height\": 213, \"label\": \"Table\"}]"
motivation: 现有单目深度模型只依赖图像间关系，忽略相机内参所蕴含的物理信息，导致重建存在不确定性和误差。
method: 将相机模型与物理特性嵌入深度网络，利用相机内参实时计算具身场景深度，并与 RGB 特征融合进行深度估计。
result: 在道路等环境交互中能够仅凭相机内参实现实时深度计算，结合 RGB 特征后提升深度估计精度。
conclusion: 相机模型的具身化可以有效补充单目深度估计的感知信息，并支持无需额外设备的实时应用。
---

## Abstract
Depth estimation is a core problem in robotic perception and vision tasks, but 3D reconstruction from a single image presents inherent uncertainties. Current depth estimation models primarily rely on inter-image relationships for supervised training, often overlooking the intrinsic information provided by the camera itself. We propose a method that embodies the camera model and its physical characteristics into a deep learning model, computing embodied scene depth through real-time interactions with road environments. The model can calculate embodied scene depth in real-time based on immediate environmental changes using only the intrinsic properties of the camera, without any additional equipment. By combining embodied scene depth with RGB image features, the model gains a comprehensive perspective on both geometric and visual details. Additionally, we incorporate text descriptions containing environmental content and depth information as priors for scene understanding, enriching the model's perception of objects. This integration of image and language -- two inherently ambiguous modalities -- leverages their complementary strengths for monocular depth estimation. The real-time nature of the embodied language and depth prior model ensures that the model can continuously adjust its perception and behavior in dynamic environments. Experimental results show that the embodied depth estimation method enhances model performance across different scenes.

---

## 论文详细总结（自动生成）

# 视觉语言具身化用于单目深度估计（CVPR 2025）

## 1. 论文的核心问题与整体含义

- 单目深度估计是机器人感知、自动驾驶、AR/3D重建中的核心任务，但单张图像到三维空间的映射是病态问题（ill-posed），存在固有不确定性。
- 现有深度估计模型主要依赖图像之间的特征关系进行监督训练，往往忽略了相机自身的内外参、高度、焦距等物理信息。
- 核心动机：将相机模型及其物理特性“具身”（embody）到深度学习模型中，使模型能利用相机内参直接与道路环境实时交互，计算可靠的场景深度先验，并结合语言描述进一步提升深度估计精度。
- 整体含义：提出一种全新的单目深度估计范式——通过物理具身 + 语言先验，弥补纯视觉方法在几何约束上的不足，且无需额外传感器。

## 2. 方法论

### 2.1 核心思想

- 利用针孔相机模型，在“道路为平面”的假设下，根据相机内外参和高度，实时计算像素级绝对深度——即“具身场景深度”（Embodied Scene Depth）。
- 将具身场景深度与 RGB 图像特征通过交叉注意力融合，提供几何与视觉互补信息。
- 引入包含环境内容和深度信息的文本描述作为语言先验，通过变分自编码器（VAE）建模场景布局的潜分布，指导深度解码。

### 2.2 关键技术细节

**（1）相机模型具身深度计算**

- 将世界坐标点投影到相机坐标系再到图像平面，用线性相机模型公式（式1-6）描述。
- 已知相机高度 h（即 yc=h），在平面假设下可直接解出每个像素对应的三维点坐标 (xc, yc, zc)，进而计算到相机的距离。
- 利用语义分割模型识别道路区域，得到“Embodied Road Depth”；推广到其他平坦表面（地面、人行道等）得到“Embodied Ground Depth”。
- 对垂直物体（车辆、行人、树木）假设其深度等于所接触地面的深度，向上扩展得到“Extended Embodied Ground Depth”。
- 对空洞区域使用 Telea 图像修复算法填补，得到稠密的“Embodied Scene Depth”。

**（2）深度引导的文本变分自编码器（Depth-Guided Text VAE）**

- 使用 ExpansionNet-v2 生成图像语义描述。
- 根据语义分割和具身场景深度，计算每个物体的深度值及排序，生成形如“This object seems to be {depth} and ranks as the {r}-th farthest in distance”的深度描述。
- 将语义描述与深度描述合并，送入 CLIP 文本编码器，再由 MLP 预测潜变量分布的均值 μ̂ 和标准差 σ̂。
- 用重参数化技巧采样：**ẑ = μ̂ + ε·σ̂**（ε~N(0,1)），随后由深度解码器生成深度图。
- 使用 KL 散度损失将潜在分布正则化到标准高斯分布，使用尺度不变对数损失（SiLog Loss）进行监督。

**（3）具身驱动的特征融合与条件采样**

- RGB 图像和具身场景深度分别经过 Transformer/Restormer 编码器提取特征。
- 通过交叉注意力机制交换 query，融合两类特征：F_df = softmax(Q_r K_d^T/√d) V_d，F_rf = softmax(Q_d K_r^T/√d) V_r，再拼接。
- 条件采样器根据融合特征预测噪声向量 ε̃，替代标准高斯噪声，采样潜变量 **z̃ = μ̂ + ε̃·σ̂**，经深度解码器输出最终深度图。
- 文本 VAE 与图像条件采样器采用交替训练（类似 Wordepth），共享深度解码器权重。

## 3. 实验设计

### 3.1 数据集与场景

- **KITTI**：采用 Eigen split，23,158 张训练图像，697 张测试图像，遵循 [15] 的裁剪方式，上采样预测深度至 ground truth 分辨率。
- **DDAD（Dense Depth for Autonomous Driving）**：更具挑战性的自动驾驶数据集，包含多个摄像头视角和更大深度范围，150 个训练场景（每相机 12,650 张），50 个测试场景（每相机 3,950 张），使用前向、后向、左前、右前四个视角评估。

### 3.2 对比方法

- **KITTI 上**对比了 Eigen、DORN、VNL、BTS、Adabins、P3Depth、DepthFormer、NeWCRFs、iDisc、URCDC、Wordepth、ECoDepth、MiM、Trap Attention、Depth Anything、Metric3D v2、UniDepth、LightedDepth、AFNet 等 SOTA 模型。
- **DDAD 上**对比了 PackNet-SAN、BTS、DepthFormer、PixelFormer、NeWCRF、BinsFormer、Adabins、iDisc 等。

### 3.3 其他实验

- 具身深度的误差分析（图3、表1）：评估 Embodied Surface/Road/Ground/Extended Ground/Scene Depth 相对于稀疏 LiDAR ground truth 的误差分布。
- 不同语义分割模型的影响（表2）：DeeperLab、AUNet、Panoptic-DeepLab、UPSNet、EfficientPS 以及 KITTI ground truth 分割，验证深度计算对分割模型鲁棒。
- KITTI 五天数据的具身深度误差统计（表3）。
- 消融实验（表6）：逐步添加 Embodied Scene Depth（ESD）、Embodied Feature Fusion（EFF）、Text Description（TD）、Depth Description（DD），共5组设置。

## 4. 资源与算力

- 论文中**未明确说明**使用的 GPU 型号、数量、训练时长等算力资源。
- 仅在致谢中提到资助项目（NSF Awards），未提供任何与计算资源相关的细节。
- 因此，无法从文中获取训练成本的具体数据，可复现性和资源需求信息不足。

## 5. 实验数量与充分性

- **实验数量较丰富**：
  - 两大数据集（KITTI、DDAD）上的标准评估；
  - 具身深度的详细误差分析（不同语义分割、不同日期数据）；
  - 5组消融实验，逐步验证各模块贡献。
- **充分性分析**：
  - 在自动驾驶场景下对比了大量 SOTA 方法，指标全面（AbsRel, SqRel, RMSE, RMSE log, δ<1.25 等），结果显著领先（KITTI AbsRel 0.0251 vs 之前最好 0.039；RMSE 1.654 vs 1.712）。
  - 消融实验证明每个组件都有正贡献，实验设计相对严谨。
  - **局限**：实验仅覆盖室外自动驾驶场景（KITTI/DDAD），未在室内或其他类型数据集（如 NYUv2）验证泛化；也没有与同类方法对比推理速度和计算开销；对基准的公平性（如是否使用相同的预训练、是否在相同实验条件下重训练对比方法）未给出详细说明。

## 6. 主要结论与发现

- 将相机模型具身化后，可以通过纯物理计算实时获得高精度的道路/平面区域深度，例如 KITTI 上 Embodied Road Depth 超过 99% 像素误差小于 10%。
- 结合 RGB 特征和语言先验，单目深度估计精度显著提升：KITTI 上 AbsRel 降至 0.0251、RMSE 降至 1.654；DDAD 上 AbsRel 降至 0.145、RMSE 降至 8.673，全面优于已有 SOTA。
- 文本描述中的深度信息（物体相对排名和绝对深度值）为模型提供了有效的尺度先验，弥补了纯视觉的歧义。
- 具身场景深度作为几何先验，能有效约束潜空间采样，防止预测不合理的深度值。

## 7. 优点

- **创新性强**：首次在单目深度估计中系统性地将相机物理模型“具身化”，将传统几何先验与深度学习方法有机结合。
- **无需额外设备**：仅依赖相机内参和实时语义分割就能计算稠密深度先验，实用性强。
- **多模态互补**：同时利用图像、物理深度、语言三种信息，且通过 VAE 和交叉注意力机制合理建模。
- **实验结果全面且领先**：在两个主流自动驾驶数据集上均取得 SOTA，消融实验清楚展示各模块的贡献。
- **对分割模型鲁棒**：表2显示不同语义分割模型对深度计算影响很小，增强了方法可靠性。

## 8. 不足与局限

- **平面假设限制**：方法强依赖“地面为平面”的假设，对于非平坦地形（坡道、颠簸路面）误差会明显增大（表1中 Embodied Scene Depth 的 ±5% 误差仅 38.88%，远低于道路区域的 80%）。
- **语义分割依赖**：虽然不同分割模型影响较小，但分割错误仍可能导致深度先验错误，特别是在复杂场景中。
- **文本描述质量依赖外部模块**：使用 ExpansionNet-v2 生成描述，若描述不准确或遗漏物体，语言先验可能无效。
- **实验覆盖不足**：仅测试了自动驾驶场景（KITTI、DDAD），未涉及室内场景、低纹理环境、动态物体等，泛化性有待验证。
- **缺乏算力与运行效率报告**：未给出训练成本、推理时间或模型参数量的对比，不利于评估实际部署成本。
- **公平性问题**：对比方法中部分（如 Depth Anything、Metric3D）可能使用更大规模预训练或额外数据，与本文训练设置不完全一致，结果解释需谨慎。

（完）
