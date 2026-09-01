---
title: Efficient Depth Estimation for Unstable Stereo Camera Systems on AR Glasses
title_zh: 面向AR眼镜不稳定双目相机系统的高效深度估计
authors: "Liu, Yongfan, Kwon, Hyoukjun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liu_Efficient_Depth_Estimation_for_Unstable_Stereo_Camera_Systems_on_AR_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 8.0
evidence: 面向未校正双目图像的高效深度估计，契合手机双摄/小基线的双目深度主题。
tldr: 该文面向AR眼镜实时双目深度估计中的延迟瓶颈，指出传统校正和代价体计算远超模型推理耗时。作者设计单应矩阵预测网络配合校正位置编码，完全去除预处理，同时用分组逐点操作替代代价体，充分利用GPU/NPU加速。实验表明该方法在未校正图像上兼具低延迟和鲁棒性，为不稳定相机系统的实时深度估计提供了可行的轻量方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 732, \"height\": 224, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 867, \"height\": 203, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 414, \"height\": 318, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 628, \"height\": 310, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 817, \"height\": 427, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1782, \"height\": 532, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 642, \"height\": 535, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1780, \"height\": 555, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 759, \"height\": 685, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 841, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1807, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1808, \"height\": 227, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 162, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1197, \"height\": 193, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-efficient-depth-estimation-for-unstable-stereo-camera-systems-on-ar-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 590, \"height\": 171, \"label\": \"Table\"}]"
motivation: AR眼镜实时深度估计中，预处理校正和非机器学习代价体计算延迟高，成为实时处理的瓶颈。
method: 提出单应矩阵预测网络与校正位置编码去除校正预处理，并用分组逐点计算替代代价体以适配ML加速硬件。
result: 方法在未校正图像上实现低延迟且鲁棒的深度估计，满足AR实时需求。
conclusion: 通过消除校正和简化代价体，实现了高效稳定的双目深度估计，适合AR等实时场景。
---

## Abstract
Stereo depth estimation is a fundamental component in augmented reality (AR), which requires low latency for real-time processing. However, preprocessing such as rectification and non-ML computations such as cost volume require significant amount of latency exceeding that of an ML model itself, which hinders the real-time processing required by AR. Therefore, we develop alternative approaches to the rectification and cost volume that consider ML acceleration (GPU and NPUs) in recent hardware. For pre-processing, we eliminate it by introducing homography matrix prediction network with a rectification positional encoding (RPE), which delivers both low latency and robustness to unrectified images. For cost volume, we replace it with a group-pointwise convolution-based operator and approximation of cosine similarity based on layernorm and dot product. Based on our approaches, we develop MultiHeadDepth (replacing cost volume) and HomoDepth (MultiHeadDepth + removing pre-processing) models. MultiHeadDepth provides 11.8-30.3% improvements in accuracy and 22.9-25.2% reduction in latency compared to a state-of-the-art depth estimation model for AR glasses from industry. HomoDepth, which can directly process unrectified images, reduces the end-to-end latency by 44.5%. We also introduce a multi-task learning method to handle misaligned stereo inputs on HomoDepth, which reduces the AbsRel error by 10.0-24.3%. The overall results demonstrate the efficacy of our approaches, which not only reduce the inference latency but also improve the model performance. Our code is available at https://github.com/UCI-ISA-Lab/MultiHeadDepth-HomoDepth

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **应用背景**：立体深度估计（Stereo Depth Estimation）是增强现实（AR）的核心基础组件，支撑着新视角渲染、遮挡推理、世界锁定、AR 物体尺度确定等下游任务。AR 眼镜将双目相机分别安装在镜框两侧，因此双目深度估计是 AR 场景中的主流方案。
- **核心挑战**：AR 应用要求实时处理（端到端延迟需低于 100ms），但 AR 眼镜属于算力受限的可穿戴设备，实现低延迟面临严峻挑战。
- **具体瓶颈**：
  - **预处理（Preprocessing）**阶段——包括相机标定和立体校正（rectification），在主流 AR 深度估计模型 ARGOS 中占总延迟的 **30.2%**。如果相机内外参未知，求解外参可能额外产生 200–2000ms 延迟。
  - **代价体积（Cost Volume）**计算——属于非 ML 计算，在 ARGOS 中占总延迟的 **29.3%**。其大量逐像素范数与除法运算无法充分利用 GPU/NPU 中高度优化的矩阵乘法硬件。
- **核心意义**：论文旨在通过**消除立体校正预处理**和**用硬件友好的算子替换代价体积**两条路径，显著降低 AR 眼镜场景下双目深度估计的端到端延迟，同时保持甚至提升模型精度。

## 2. 论文提出的方法论

### 2.1 核心思想
论文提出两个互补的模型：
- **MultiHeadDepth**：针对代价体积进行优化，用硬件友好的"多头代价体积"替换传统代价体积。
- **HomoDepth**：在 MultiHeadDepth 基础上进一步消除预处理，通过单应矩阵估计头 + 2D 校正位置编码（RPE），直接接受未校正的立体图像输入。

### 2.2 代价体积优化——MultiHeadDepth

**(1) 近似余弦相似度（LND 方法）**
- 传统代价体积的余弦相似度计算为：`D_cos(a,b) = a·b / (|a||b|)`，其中分子（点积）是硬件友好的矩阵乘法，但分母（逐像素范数计算及除法）效率低下。
- 论文采用 **2D LayerNorm + 点积（LayerNorm-Dot product, LND）** 来近似余弦相似度：先用 LayerNorm 对左右特征分别归一化，再直接做点积。这样大幅减少了昂贵的范数和除法运算。
- LayerNorm 的额外好处是提供了"缓冲区"，可以融入其他编码信息（如位置编码）。LND 之后还增加了一层可学习权重层来调节相似度近似效果。

**(2) 多头代价体积（Multi-head Cost Volume）**
- 借鉴多头注意力的思想，将输入激活沿通道维度分为多个头，每个头独立计算相似度，再通过**分组逐点卷积（group-pointwise convolution）**组合输出。
- 相比传统代价体积，优势包括：
  1. 计算复杂度更低；
  2. 计算被替换为高度优化的 group-pointwise Conv（GPU/NPU 友好）；
  3. 多头机制提供更多感知视角，有利于精度提升。
- 算法流程：对左右特征做 LayerNorm → 对右特征按每个 disparity 偏移执行 roll 操作 → 按头分组做点积相似度 → 逐点卷积融合。

### 2.3 预处理消除——HomoDepth

**(1) 单应矩阵（Homography Matrix）**
- 基于 3D 投影推导，同一世界点 Q 在左右图像中的投影点 ql 与 qr 满足关系：`qr = (dl/dr) · H_{l→r} · ql`，其中 `H_{l→r}` 是 3×3 平面单应矩阵。当物体位于相机中心平面或距离较远时，`dl/dr ≈ 1`，此时单应矩阵可近似反映双目图像间的位置关系。
- 与传统需要已知相机参数的 MVSNet 不同，本文面向**相机外参未知且动态变化**的 AR 眼镜场景。

**(2) 单应矩阵估计头（Homography Estimation Head）**
- 设计一个小型 CNN 单应矩阵估计头，与深度估计共享编码器（common encoder），通过多任务学习联合训练，仅增加极小的额外计算量。

**(3) 2D 校正位置编码（Rectification Positional Encoding, RPE）**
- 传统校正会因边缘裁剪造成信息丢失，因此作者不进行实际图像校正，而是将单应矩阵转化为位置编码注入网络。
- 具体做法：对左图使用标准 2D 正弦位置编码 `PE(ql)`，对右图使用经单应矩阵变换后的位置编码 `RPE(qr) := PE(H_{l→r} · ql)`。这样来自同一世界点的左右像素会获得相似的位置编码，增强了立体匹配的准确性。
- RPE 可集成进多头代价体积中，与 LayerNorm 输出相加，使网络同时利用语义相似性和位置相似性。

**(4) 多任务训练**
- 使用**同方差不确定性（homoscedastic uncertainty）**加权两个任务的损失：
  - 深度损失 `L_D`：SmoothL1 损失 + 多尺度梯度损失（沿用 ARGOS）；
  - 单应损失 `L_H`：对单应矩阵进行加权（对数值较小的角度相关元素放大权重，权重 w=50），使用 Frobenius 范数衡量误差；
  - 联合损失：`L = L_H/(2σ_H²) + L_D/(2σ_D²) + log(σ_H·σ_D)`，其中 σ_H、σ_D 为可训练的不确定性参数。

## 3. 实验设计

### 3.1 数据集
| 数据集 | 场景 | 图像类型 | 立体质量 | 基线 |
|---|---|---|---|---|
| SceneFlow | 合成场景（驾驶/飞行物体） | 合成渲染 RGB | 已校正 | 静态，1.0m |
| ADT（Aria Digital Twin） | 日常第一人称视角生活场景 | 真实相机 RGB + 灰度鱼眼 | 已校正 | 静态，12.8cm |
| DTU | 机器人臂旋转扫描物体 | 真实相机 RGB | **未校正** | 动态，7.8–14.9cm |
| Middlebury 2014 | 高分辨率立体数据集 | 真实图像 | 已校正 | —（仅测试） |

- 作者还构造了两个额外的难度变体用于消融实验：
  - **DTU df**：在 DTU 基础上引入焦距缩放和裁剪，模拟动态焦距 + 动态相对位置。
  - **SceneFlow persp**：对 SceneFlow 施加透视变换，模拟 AR 眼镜弯曲导致的相机失准。

### 3.2 评估平台
- 主评估平台：Intel i7-12700H CPU + Nvidia RTX 3070 Ti 笔记本 GPU。
- 边缘设备：Nvidia Jetson Orin Nano 开发套件、Snapdragon 8+ Gen 1 手机平台（使用 SNPE 编译）。
- 训练平台：Nvidia RTX 4090 24GB GPU（单卡）。

### 3.3 对比方法
- MobileStereoNet-2D / MobileStereoNet-3D（WACV 2022）
- Dynamic-Stereo（CVPR 2023）
- **Argos**（CVPR 2023，工业级 AR 眼镜 SOTA 模型，主要对比对象）
- Selective-Stereo（CVPR 2024）

### 3.4 评估指标
- AbsRel（绝对相对误差）、D1（误差率）、RMSE（均方根误差），均为越低越好。

## 4. 资源与算力

- **训练硬件**：论文明确提到使用 Nvidia RTX 4090 24GB GPU 进行模型训练（单卡）。
- **训练配置**：批量大小为 10，使用 Adam 优化器，无学习率调度器。先以 1e-4 基础学习率训练并选最优 epoch，之后以 4e-4 微调。
- **未明确说明的信息**：论文**未披露**具体训练时长、GPU 数量、总训练轮数（epoch 数）、能耗等细节。这属于实验报告中的信息缺口。

## 5. 实验数量与充分性

论文包含了较为全面的实验体系：

1. **主精度对比实验**：在 SceneFlow、ADT、DTU、Middlebury 四个数据集上与 5 种 SOTA 方法对比，测试了模型的泛化能力。
2. **量化实验（INT8）**：使用 Qualcomm Aimet 工具进行后训练量化，对比量化前后精度与延迟，验证模型在实际部署中的适用性。
3. **消融/鲁棒性实验**：在 DTU、DTU df、SceneFlow persp 三种场景下对比 Argos（有/无预处理）、MultiHeadDepth（有/无预处理）、HomoDepth（无预处理），验证各模块的贡献。
4. **边缘设备延迟实验**：在 Jetson Orin Nano 和 Snapdragon 8+ Gen 1 上对比 Argos、MultiHeadDepth、HomoDepth 的延迟。
5. **模型复杂度对比**：对比参数量、FLOPs、CPU/GPU 延迟与精度的整体关系。

**充分性评价**：
- 实验覆盖了合成数据（SceneFlow）、真实 AR 眼镜数据（ADT）、真实未校正数据（DTU）、高分辨率标准数据集（Middlebury），覆盖面较广；
- 消融实验设计合理，能逐步验证代价体积优化和预处理消除的各自贡献；
- 在多个硬件平台（笔记本 CPU/GPU、边缘 GPU、移动 SoC）上均评测了延迟，增强了结论的可信度；
- 对比方法包含从轻量级到 SOTA 的多类方案，且遵循了与 Argos 一致的训练/测试协议（如 Middlebury 用 SceneFlow 训练后直接测试）。
- 总体来看实验较为充分、客观。但以下因素需注意：ADT 数据集作者人为筛选了 145/236 个场景（剔除了含人的场景），可能在某种程度上影响对真实 AR 场景的泛化结论；HomoDepth 在已校正数据集（SceneFlow/ADT）上的表现如何，论文未给出系统对比。

## 6. 论文的主要结论与发现

- **MultiHeadDepth 的优越性**：相比 Argos，在精度上提升 **11.8%–30.3%**（AbsRel 等指标），在延迟上降低 **22.9%–25.2%**（CPU/GPU），且参数量和 FLOPs 显著低于 Selective-Stereo（仅用其 1.2% 的 FLOPs 就达到相近精度）。
- **HomoDepth 的端到端优势**：在未校正/失准的立体输入场景下，HomoDepth 无需任何预处理即可直接处理原始图像，相比"预处理 + 模型"的完整流程降低端到端延迟 **44.5%**（具体数据：CPU 延迟从 811.0ms 降至 761.2ms vs 1068.3ms 的完整预处理流水线；GPU 延迟从 109.0ms 降至 84.5ms vs 312.5ms 的完整流水线）。
- **INT8 量化有效性**：量化后精度仅下降 3.1%–14.7%，但延迟降低 7.7%–43.4%，MultiHeadDepth 在量化后仍全面优于 Argos。
- **多任务学习的价值**：引入同方差不确定性加权多任务训练后，HomoDepth 在失准输入上的 AbsRel 误差额外降低 **10.0%–24.3%**。
- **RPE 的有效性**：2D 校正位置编码能有效向网络传递立体图像的几何对齐信息，使网络无需实际校正图像即可感知左右图的相对位置关系。

## 7. 优点

- **问题定位精准**：通过对 SOTA 模型 ARGOS 的延迟分解，清晰量化了预处理和代价体积两大瓶颈，研究动机非常扎实。
- **方法设计巧妙且互补**：
  - 用 LayerNorm+点积近似余弦相似度，既保留语义匹配能力又契合硬件加速特性；
  - 引入多头机制提升网络感知能力；
  - 用"单应矩阵 + 位置编码"替代传统几何校正，避免了校正带来的信息丢失（边缘裁剪），且能适应动态变化的外参。
- **普适性**：论文的方法不仅适用于 ARGOS，可推广到任何含代价体积或在线校正的双目深度估计模型。
- **工程落地意识强**：同时考虑了 INT8 量化、多种边缘硬件平台（Jetson Orin Nano、Snapdragon）、实际 AR 眼镜数据集（ADT），实验设计紧贴真实部署需求。
- **代码开源**：提供了完整代码仓库，便于复现和后续研究。

## 8. 不足与局限

- **训练细节不透明**：未报告训练 epoch 数、完整训练时长、能耗、显存占用等关键资源信息，影响可复现性和对部署成本的评估。
- **HomoDepth 的适用边界**：
  - 依赖单应矩阵估计的准确性——在极端视角变化、大景深变化或近距离物体主导的场景中（`dl/dr ≈ 1` 假设不成立），其有效性需要进一步验证。
  - 论文主要展示 HomoDepth 在未校正场景（DTU、DTU df、SceneFlow persp）上的优势，但在已校正的标准数据集（如 SceneFlow、ADT、Middlebury）上，HomoDepth 与 MultiHeadDepth 的精度对比未系统给出，可能掩盖了单应估计误差带来的精度损失。
- **ADT 数据集使用偏差**：仅筛选了无人的 145 个场景，而真实 AR 应用中人物是常见元素；此外 ADT 右图为灰度鱼眼图像（经校正后复制到 3 通道），与 DTU 等普通 RGB 双目输入存在域差距，跨数据集泛化结论可能受影响。
- **实时性目标尚未完全达成**：即使在论文优化后，CPU 端延迟仍约 600–760ms，GPU 端约 84–216ms；论文引言提到的"100ms 以下"目标仅在部分条件下达到（如 HomoDepth 在 RTX 3070 Ti 上为 84.5ms），在 Jetson Orin Nano 等更贴近 AR 眼镜的边缘设备上的 CPU 延迟仍高达 6 秒级别，说明离真正的轻量级实时部署仍有距离。
- **对比公平性**：HomoDepth 的单应估计头依赖 DTU 提供的真实外参计算 homography ground truth，而真实场景中未必能获得准确的监督信号来训练该头，论文对无标签场景下的适用方案讨论不足。
- **数据集多样性有限**：虽然使用了多个数据集，但真实 AR 眼镜数据（ADT）与未经校正的真实双目数据（DTU）是分离的，缺乏"真实 AR 眼镜 + 天然未校正"的直接测试数据；DTU 是机器人臂扫描场景而非真实头戴设备场景。

（完）
