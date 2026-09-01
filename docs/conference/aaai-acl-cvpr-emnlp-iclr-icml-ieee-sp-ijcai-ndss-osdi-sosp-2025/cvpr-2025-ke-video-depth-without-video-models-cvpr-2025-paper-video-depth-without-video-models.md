---
title: Video Depth without Video Models
title_zh: 无需视频模型的视频深度估计
authors: "Ke, Bingxin, Narnhofer, Dominik, Huang, Shengyu, Ke, Lei, Peters, Torben, Fragkiadaki, Katerina, Obukhov, Anton, Schindler, Konrad"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Ke_Video_Depth_without_Video_Models_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 基于单图像深度基础模型的视频深度估计
tldr: 视频深度估计常依赖视频基础模型，但训练和推理成本高，逐帧应用单目模型又缺乏时序一致性。本文提出在不使用视频模型的情况下，通过融入时序一致性约束来提升单图像深度估计器在视频上的表现。实验表明该方法有效减弱闪烁并保持精度，提供了一种轻量的视频深度估计方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1797, \"height\": 629, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1819, \"height\": 654, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 783, \"height\": 479, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1808, \"height\": 715, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1812, \"height\": 403, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1812, \"height\": 816, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1798, \"height\": 448, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 701, \"height\": 215, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ke-video-depth-without-video-models-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 870, \"height\": 250, \"label\": \"Table\"}]"
motivation: 单帧深度估计在视频上逐帧应用会忽略时序连续性，而视频基础模型训练和推理代价高。
method: 提出在单图像深度估计器基础上引入时序一致性机制，无需训练视频模型即可获得稳定的视频深度。
result: 在保持精度的同时显著提升时序一致性，并避免视频基础模型的复杂拼接。
conclusion: 为视频深度估计提供了一种轻量高效的替代方案，可复用单帧深度基础模型。
---

## Abstract
Video depth estimation lifts monocular video clips to 3D by inferring dense depth at every frame. Recent advances in single-image depth estimation, brought about by the rise of large foundation models and the use of synthetic training data, have fueled a renewed interest in video depth. However, naively applying a single-image depth estimator to every frame of a video disregards temporal continuity, which not only leads to flickering but may also break when camera motion causes sudden changes in depth range. An obvious and principled solution would be to build on top of video foundation models, but these come with their own limitations; including expensive training and inference, imperfect 3D consistency, and stitching routines for the fixed-length (short) outputs. We take a step back and demonstrate how to turn a single-image latent diffusion model (LDM) into a state-of-the-art video depth estimator. Our model, which we call RollingDepth, has two main ingredients: (i) a multi-frame depth estimator that is derived from a single-image LDM and maps very short video snippets (typically frame triplets) to depth snippets. (ii) a robust, optimization-based registration algorithm that optimally assembles depth snippets sampled at various different frame rates back into a consistent video. RollingDepth is able to efficiently handle long videos with hundreds of frames and delivers more accurate depth videos than both dedicated video depth estimators and high-performing single-frame models. Project page: https://rollingdepth.github.io.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与研究动机

- 视频深度估计的目标是为视频每一帧预测稠密深度，并保证时序上的一致性，从而支撑机器人、AR、媒体制作等应用。
- 传统 SfM / 多视图重建方法在真实开放视频中容易失效，因为其要求相机运动、静态背景、纹理和光照条件都较为理想。
- 近年单图深度估计取得很大进展，尤其是基于 Stable Diffusion、DINOv2 等基础模型的方法，如 Marigold、Depth Anything 等。但直接在视频上逐帧应用单图模型会忽略时序连续性，导致深度闪烁、漂移，并且在深度范围突变（如前景进入视野、相机转向窗口）时容易出错。
- 另一条路线是使用视频扩散模型（如 Stable Video Diffusion）来估计视频深度，但这类方法训练和推理成本高、固定输出长度（通常约 100 帧）、需要拼接短片段，且三维一致性不完美。
- 本文的核心思路是：不设计视频模型，而是将单图像 latent diffusion model（LDM）扩展为视频深度估计器，在保持单图模型精度的同时获得视频级时序一致性，并能高效处理数百甚至上千帧的长视频。

## 2. 方法论

- 论文提出 **RollingDepth**，由三个核心部分组成：

### 2.1 多帧片段深度估计器

- 以 Marigold 的单目深度 LDM 为基座，将其扩展为可处理短片段（通常为 3 帧）的多帧深度估计器。
- 具体做法是修改自注意力层：将一个 snippet 中所有帧的 token 拼接为同一个序列，使注意力机制能够同时建模空间与时间交互。这种方式不同于视频扩散模型的因子化时空注意力，能够灵活处理不同时间间隔的帧。
- 重新训练模型预测**逆深度（inverse depth）**，并采用“逐片段归一化”（使用第 2 和第 98 百分位数），使模型能理解并正确处理视频中深度范围的快速变化。
- 训练片段长度随机取 1、2 或 3 帧，并保证帧间存在足够重叠的视野。

### 2.2 从片段到视频的整体装配

- 使用 **Dilated Rolling Kernel** 构建多尺度重叠片段。
  - 对 3 帧 snippet，设 dilation rate（帧间距）为 g、stride 为 h，则取帧序列 (x_{i-g}, x_i, x_{i+g})。
  - 通过多个 dilation rate（如 {1, 10, 25}）捕捉不同时间尺度上的上下文。
- 每个 snippet 独立预测得到自己的深度片段，但各自有任意的尺度 s_k 和偏移 t_k。
- 使用基于优化的 **Depth Co-alignment** 全局对齐：对每个 snippet 估计一对尺度与偏移，最小化所有重叠帧上不同深度图之间的 L1 损失，并加入正则化；随后对每帧取所有重叠预测的像素级均值，得到全局一致的深度视频。
- 该对齐问题通过梯度下降求解，初始化 s_k=1, t_k=0。

### 2.3 可选的扩散精化

- 将对齐后的深度视频编码到潜在空间，并加入中等强度的噪声（对应扩散过程 T/2 时刻）。
- 再用同一个 snippet LDM 进行反向去噪，并逐步降低 dilation rate（从 6 到 1），实现从粗到细的时间精化。
- 每步去噪后对重叠的 latent 进行平均，以在片段之间传播信息。
- 这一步能增强高频细节，对全局深度布局影响很小，但会增加推理时间。

## 3. 实验设计

### 3.1 数据集与 Benchmark

- 训练数据：
  - **TartanAir**：合成视频数据集，人工筛选 18 个场景、369 个序列。
  - **Hypersim**：逼真单图像数据集，365 个场景，作为 1 帧片段使用。
- 评测数据（零样本）：
  - **PointOdyssey**（250 帧）：合成动态场景，包含独立运动的物体。
  - **ScanNet v2**（90 帧）：室内静态场景，取测试集 100 个序列，前 270 帧并以 3 倍降采样。
  - **Bonn RGBD**（110 帧）：室内动态人物场景，取 5 个动态场景的约 110 帧。
  - **DyDToF**（200 帧和 100 帧两个子集）：含人物和动物等运动物体的真实感合成数据。

### 3.2 对比方法与评价指标

- 单帧方法：Marigold、DepthAnything、DepthAnythingV2。
- 视频方法：NVDS、ChronoDepth、DepthCrafter。
- 评价协议：将预测深度与真值在整段视频上拟合一个共同的尺度与偏移（仿射不变深度评价扩展），使用 AbsRel（越小越好）和 δ1 精度（越大越好）。

### 3.3 主要结果

- RollingDepth 在 PointOdyssey、ScanNet、DyDToF 上均显著优于所有对比方法；
- 在 Bonn 上仅次于 DepthCrafter；
- 定性结果展示了 RollingDepth 能保留精细细节、恢复正确场景布局，并在时间轴上无明显闪烁或漂移。

## 4. 资源与算力

- 论文明确说明：训练在 **4 块 NVIDIA A100 GPU** 上进行，batch size 为 32，约 18,000 次迭代，约 **2 天**收敛。
- 推理设置：
  - 片段长度固定为 3，dilation rates 为 {1, 10, 25}，每个 snippet 做 1 步推理。
  - 对齐优化使用 Adam，2000 步梯度下降。
  - 精化阶段从扩散轨迹 T/2 出发，做 10 步去噪。
  - 快速版本（fp16，dilation {1,25}，无精化）在 250 帧、768×432 视频上约 81 秒；对比 ChronoDepth 约 121 秒、DepthCrafter 约 284 秒。

## 5. 实验数量与充分性

- 实验整体较充分：
  - 4 个数据集、多种长度和动态程度不同的零样本 benchmark；
  - 与 3 个单帧方法、3 个视频方法做定量和定性对比；
  - 提供时序轮廓可视化、逐帧误差曲线和误差图；
  - 提供两组消融实验：
    1. dilation rate 的选择（{1} vs {1,25} vs {1,10,25}）；
    2. 组件的有效性（co-alignment 与 refinement 的开/关组合）。
- 不足与客观性分析：
  - 消融实验只在 PointOdyssey 的 10 个序列和 ScanNet 的 20 个序列上进行，规模相对有限；
  - 对精化阶段的噪声强度、去噪步数、片段长度（如 2 帧或 4 帧）等没有系统消融；
  - 时序平滑度指标在正文中被省略，放到补充材料中，因此主表只给出了深度精度指标；
  - 对比视频方法时，它们各自的训练数据和推理设置可能不完全对齐，但总体上这一比较已经是该方向常用的标准做法。

## 6. 主要结论与发现

- 通过“单图 LDM + 短片段多帧扩展 + 全局对齐 + 可选精化”，无需视频模型即可得到最先进的视频深度估计结果。
- 多尺度片段采样（尤其是高 dilation rate）能有效利用长时间上下文，显著降低深度误差，并对全局对齐至关重要。
- Depth co-alignment 是系统中最关键的部分，承担了将不同尺度/偏移的片段融合为一致视频的主要工作；refinement 对指标提升有限，但能明显改善视觉细节。
- RollingDepth 很好地平衡了逐帧精度和时序一致性，能够处理数百帧的长视频，且在动态场景、深度范围剧烈变化的场景中优于基于视频扩散模型的方法。

## 7. 优点

- **无需视频模型**：在单个图像 LDM 基础上实现视频级深度估计，避免大型视频扩散模型的训练与推理成本。
- **长视频友好**：通过 sliding snippet 和全局对齐，内存占用恒定，可处理数百甚至上千帧视频。
- **灵活的多尺度时间上下文**：dilated rolling kernel 允许以不同帧率采样片段，既能捕捉相邻帧平滑性，也能捕捉长期结构。
- **模块化设计**：每个组件（片段估计、对齐、精化）均可替换或关闭，便于与未来更强的单图模型或对齐方法结合。
- **在实验上全面领先**：在多个动态与静态数据集上同时超越单帧方法和专用视频深度方法，且具有较低推理开销。

## 8. 不足与局限

- **依赖全局对齐优化**：co-alignment 需要额外的优化步骤，其效果高度依赖重叠片段之间的一致性和 dilation 设置；在极低纹理、遮挡严重或深度范围频繁突变的视频上，对齐全过程的鲁棒性仍有风险。
- **训练数据规模有限**：只用了 TartanAir 和 Hypersim 两个合成数据集，场景多样性不如视频基础模型的数据规模，可能限制某些开放世界场景的泛化。
- **可选的精化仍增加成本**：refinement 需要额外的去噪步骤，虽然指标提升不大，但对需要高频细节的应用来说，推理时间会显著增加。
- **对运动物体的显式建模不足**：方法主要依赖多帧注意力和全局对齐，没有显式建模光流或动态物体运动，因此在快速运动、遮挡和运动模糊严重时可能退化。
- **评价指标侧重精度**：主文没有在 benchmark 表中量化时间一致性，时序平滑度只在补充材料中呈现，因此对“时序一致性”这一核心贡献的量化证据相对较弱。

（完）
