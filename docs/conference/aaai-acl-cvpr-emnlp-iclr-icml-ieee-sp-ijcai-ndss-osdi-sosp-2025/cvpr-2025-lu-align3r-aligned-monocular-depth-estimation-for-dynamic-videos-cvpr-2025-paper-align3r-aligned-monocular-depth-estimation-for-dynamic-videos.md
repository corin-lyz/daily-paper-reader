---
title: "Align3R: Aligned Monocular Depth Estimation for Dynamic Videos"
title_zh: Align3R：面向动态视频的对齐单目深度估计
authors: "Lu, Jiahao, Huang, Tianyu, Li, Peng, Dou, Zhiyang, Lin, Cheng, Cui, Zhiming, Dong, Zhen, Yeung, Sai-Kit, Wang, Wenping, Liu, Yuan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Lu_Align3R_Aligned_Monocular_Depth_Estimation_for_Dynamic_Videos_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 面向动态视频的对齐单目深度估计
tldr: 现有单目深度估计方法在单帧上表现良好，但难以在动态视频中保持深度一致。Align3R 提出利用 DUSt3R 模型，以估计的单目深度为先验，对不同时刻的深度图进行对齐，得到时域一致的视频深度。该方法避免了视频扩散模型的昂贵训练，并且能够输出具有尺度信息的深度。实验表明其在高动态场景下的一致性和精度优于已有方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1739, \"height\": 800, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1808, \"height\": 813, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1492, \"height\": 1514, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 171, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 725, \"height\": 303, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1746, \"height\": 432, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 873, \"height\": 207, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1789, \"height\": 377, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1478, \"height\": 388, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lu-align3r-aligned-monocular-depth-estimation-for-dynamic-videos-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 761, \"height\": 197, \"label\": \"Table\"}]"
motivation: 单目深度估计在单帧上效果很好，但缺乏帧间一致性；已有视频扩散方法训练成本高且只能得到尺度不变深度。
method: 微调 DUSt3R 模型，将估计的单目深度作为附加输入，对不同时间步的深度图进行对齐，生成时间一致的视频深度。
result: 在动态视频数据集上实现更一致的深度估计，避免昂贵训练，并支持尺度信息恢复。
conclusion: 利用几何对齐模型可高效解决视频深度一致性问题，为动态场景深度估计提供轻量方案。
---

## Abstract
Recent developments in monocular depth estimation methods enable high-quality depth estimation of single-view images but fail to estimate consistent video depth across different frames. Recent works address this problem by applying a video diffusion model to generate video depth conditioned on the input video, which is training-expensive and can only produce scale-invariant depth values without camera poses. In this paper, we propose a novel video-depth estimation method called Align3R to estimate temporal consistent depth maps for a dynamic video. Our key idea is to utilize the recent DUSt3R model to align estimated monocular depth maps of different timesteps. First, we fine-tune the DUSt3R model with additional estimated monocular depth as inputs for the dynamic scenes. Then, we apply optimization to reconstruct both depth maps and camera poses. Extensive experiments demonstrate that Align3R estimates consistent video depth and camera poses for a monocular video with superior performance than baseline methods.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：单目深度估计方法（如 Depth Anything V2、Depth Pro）在单帧图像上已能取得高质量结果，但在动态视频中难以维持跨帧的尺度一致性，导致深度序列出现闪烁伪影。
- **现有方案局限**：
  - 传统视频深度对齐方法（如 Robust-CVD）依赖光流或图像匹配，迭代优化耗时长（数小时以上），且难以处理大动态运动或大相机运动。
  - 基于视频扩散模型的方法（如 DepthCrafter、ChronoDepth）能生成较一致的视频深度，但训练成本高、只能处理固定长度的视频片段，且只能输出尺度不变的深度，不提供相机位姿，难以直接支撑 4D 重建、3D 追踪等下游任务。
- **研究含义**：需要一个既能保持视频深度时间一致性、又能恢复尺度信息、还能同时估计相机位姿的轻量级方案。

## 2. 论文提出的方法论

**核心思想**：将单目深度估计器（提供细节）与 DUSt3R（提供跨帧几何一致性）相结合，通过微调 DUSt3R 模型，使其接受单目深度估计作为额外输入，从而在动态视频中预测对齐的成对点图，再通过优化求解全局一致的深度图和相机位姿。

**关键技术细节**：

- **深度图的 3D 化处理**：将单目深度估计结果反投影为 3D 点图（需要相机内参；Depth Pro 可预测焦距，Depth Anything V2 使用固定焦距），并对每个轴的数值范围归一化到 [−1,1] 以稳定训练。
- **点图 ViT 编码器**：使用一个新增的 ViT 对点图进行 patch embedding 和自注意力处理，提取多层级特征，然后通过零卷积（Zero-Conv）逐层注入 DUSt3R 的解码器，避免破坏预训练模型的原始特征分布。
- **微调策略**：冻结 DUSt3R 的编码器，仅微调解码器和新增的点图 ViT；损失函数沿用 DUSt3R 的归一化点图 L2 损失。
- **深度滤波**：过滤掉 400 米以上的远距离深度值（如天空），避免远点主导训练损失，强调近处物体的预测精度。
- **推理优化**：沿用 DUSt3R 的全局对齐优化（式 1），通过极小化跨帧点图投影深度与预测深度的加权 L2 距离，同时求解深度图与相机位姿。
- **分层优化（Hierarchical Optimization）** ：应对长视频（>30 帧）显存不足的问题——将视频分为 M 帧短片段，先对关键帧做全局对齐初始化，再对每个片段做局部对齐，显著降低显存与时间开销。

## 3. 实验设计

**评估数据集**：

- **视频深度估计**：真实数据集 Bonn（5 个视频，各约 110 帧）、TUM dynamics（8 个场景，各 50 帧）；合成数据集 Sintel（23 个视频）、PointOdyssey 验证集（15 个场景，各 110 帧）、SceneFlow/FlyingThings3D 测试集（44 个场景），另有 DAVIS 定性展示。
- **静态/自动驾驶场景补充评估**：ScanNetV2、NYU-v2、KITTI。
- **相机位姿估计**：TUM dynamics（30/90 帧）、Bonn（30 帧）、ScanNetV2、Sintel（14 个序列）。

**对比方法**：

- 单帧深度：Depth Anything V2、Depth Pro。
- 视频深度：ChronoDepth、DepthCrafter。
- 联合深度与位姿：DUSt3R、MonST3R（同期的并发工作）。
- 位姿专用方法：DROID-SLAM、DPVO、COLMAP、Robust-CVD、CasualSAM。

**评估指标**：Abs Rel、δ<1.25（全视频统一尺度和偏移对齐后计算）；位姿用 ATE、RTE、RRE。

## 4. 资源与算力

- **训练阶段**：6 块 RTX 4090 GPU，batch size = 12，训练约 20 小时（50 个 epoch），每 epoch 含 27,750 对训练样本（来自 5 个合成数据集）。
- **推理阶段**：论文明确提到长视频（>30 帧）在 4090 上原始 DUSt3R 优化会显存溢出，因此设计了分层优化策略来降低显存需求。

## 5. 实验数量与充分性

- **实验数量**：相当充分。覆盖 6 个动态视频数据集 + 3 个静态/驾驶数据集（深度），5 个数据集（位姿），共 15 组以上定量对比；另有 4 组消融实验（表 5、表 6）和多组定性可视化。
- **消融覆盖**：
  - 微调策略：全模型 vs 最后 4 层 vs 全部解码器。
  - 深度融合方式：不使用深度 vs 直接拼接 vs ViT 编码注入。
  - 推理优化：分层优化 vs 原始优化 vs MonST3R 的窗口处理。
- **公平性**：所有基线均采用官方实现，并按每序列统一尺度和偏移对齐后重新评估，评价协议统一；位姿评估中 DROID-SLAM、DPVO 等使用了真值内参，而本文方法未使用该条件，结论方向一致。
- **客观性判断**：实验设计总体客观公平，但在 TUM/Bonn 等相机运动较缓和的室内场景，本文方法相对 Depth Pro 的增益有限，作者也坦诚指出在全局对齐中可能损失部分细节；ATE 指标在 Sintel 上略逊于 MonST3R。

## 6. 论文的主要结论与发现

- Align3R 在全部 5 个动态视频数据集的所有深度指标上优于 DUSt3R 和 MonST3R，同时显著优于单帧深度方法。
- 在相机位姿估计上，Align3R 的 RTE 和 RRE 在所有数据集上均为最优或次优；ATE 在真实数据集上最优，仅在合成 Sintel 上略逊 MonST3R。
- 在静态场景（ScanNetV2、NYU-v2）和自动驾驶场景（KITTI）中，本文方法仍保持竞争力，说明在动态数据上微调并未严重损害静态场景泛化能力。
- 通过将单目深度先验注入 DUSt3R 解码器而非直接拼接，既保持了预训练模型的特征分布，又显著提升了点图细节恢复能力，证明了"预组合（pre-combination）"优于"后优化（post-optimization）"。

## 7. 优点

- **轻量且高效**：只需学习预测成对点图，远易于视频扩散模型的生成式训练。
- **同时输出深度与位姿**：可直接支撑 4D 重建和 3D 追踪等下游任务，优于只能输出尺度不变深度的扩散方法。
- **细节与一致性的兼得**：融合单目深度的高频细节与 DUSt3R 的跨帧几何约束，比 MonST3R（仅微调原模型）恢复更多深度细节。
- **工程化设计**：零卷积注入、深度归一化、远点滤波、分层优化等细节设计，保证了训练的稳定性和推理的可扩展性。
- **消融完整**：对微调策略、融合方式、优化策略均有充分的消融验证。

## 8. 不足与局限

- **依赖单目深度先验质量**：单目深度估计的误差会传播至点图预测和全局优化；若单目深度在特定场景下失效，方法的深度上限将受影响。
- **数据域偏差**：微调数据全部来自合成数据集（SceneFlow、VKITTI、TartanAir、Spring、PointOdyssey），虽然加入了静态数据混合，但合成到真实的泛化差距未深入讨论。
- **室内低动态场景增益有限**：在 Bonn/TUM 等场景，Depth Pro 原始输出已较好，Align3R 的提升幅度明显小于高动态场景，且全局对齐可能损失部分细节。
- **位姿 ATE 并非全面最优**：在 Sintel 上 ATE 稍逊 MonST3R，说明轨迹全局漂移控制仍有改进空间。
- **训练成本仍不低**：虽然推理时优化较传统方法快很多，但 6×4090 训练 20 小时对一般研究者仍有门槛。
- **未见开放世界/零样本泛化测试**：未涉及 TanksAndTemples、ETH3D 等户外大场景零样本评估，泛化性论证主要局限在评测的几类场景中。

（完）
