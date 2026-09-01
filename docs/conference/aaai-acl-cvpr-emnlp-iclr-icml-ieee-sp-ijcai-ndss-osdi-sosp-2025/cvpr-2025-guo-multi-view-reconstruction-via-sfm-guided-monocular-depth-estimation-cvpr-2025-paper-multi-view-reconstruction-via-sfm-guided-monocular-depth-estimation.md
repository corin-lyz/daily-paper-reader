---
title: Multi-view Reconstruction via SfM-guided Monocular Depth Estimation
title_zh: 基于SfM引导的单目深度估计的多视图重建
authors: "Guo, Haoyu, Zhu, He, Peng, Sida, Lin, Haotong, Yan, Yunzhi, Xie, Tao, Wang, Wenguan, Zhou, Xiaowei, Bao, Hujun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Guo_Multi-view_Reconstruction_via_SfM-guided_Monocular_Depth_Estimation_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 面向多视图重建的SfM引导单目深度估计
tldr: 针对现有学习型多视图重建依赖匹配导致显存消耗高、稀疏视角下易失败的问题，提出Murre流水线：先用SfM恢复场景全局点云，再以此引导条件扩散模型进行单目深度估计，从而生成多视图几何。实验表明该方法在稀疏视角场景下重建鲁棒且质量高，相比依赖匹配的方法降低了显存需求，并为多视图重建提供了新的单目深度引导范式。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1809, \"height\": 694, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1806, \"height\": 674, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 845, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 918, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1803, \"height\": 875, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 365, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-multi-view-reconstruction-via-sfm-guided-monocular-depth-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 851, \"height\": 193, \"label\": \"Table\"}]"
motivation: 现有学习型多视图重建依赖匹配，显存消耗高，稀疏视角下易失败。
method: 提出Murre，先通过SfM恢复全局点云，再引导条件扩散模型生成单目深度，完成多视图重建。
result: 在稀疏视角和多视图重建基准上取得稳健的高质量重建结果。
conclusion: 验证了SfM引导单目深度估计作为多视图重建鲁棒方案的有效性。
---

## Abstract
This paper aims to reconstruct the scene geometry from multi-view images with strong robustness and high quality. Previous learning-based methods incorporate neural networks into the multi-view stereo matching and have shown impressive reconstruction results. However, due to the reliance on matching across input images, they typically suffer from high GPU memory consumption and tend to fail in sparse view scenarios. To overcome this problem, we develop a new pipeline, named Murre, for multi-view geometry reconstruction of 3D scenes based on SfM-guided monocular depth estimation. For input images, Murre first recover the SfM point cloud that captures the global scene structure, and then use it to guide a conditional diffusion model to produce multi-view metric depth maps for the final TSDF fusion. By predicting the depth map from a single image, Murre bypasses the multi-view matching step and naturally resolves the issues of previous MVS-based methods. In addition, the diffusion-based model can easily leverage the powerful priors of 2D foundation models, achieving good generalization ability across diverse real-world scenes. To obtain multi-view consistent depth maps, our key design is providing effective guidance on the diffusion model through the SfM point cloud, which is a condensed form of multi-view information, highlighting the scene's salient structure, and can be readily transformed into point maps to drive the image-space estimation process. We evaluate the reconstruction quality of Murre in various types of real-world datasets including indoor, streetscapes, and aerial scenes, surpassing state-of-the-art MVS-based and implicit neural reconstruction-based methods. The code and supplementary materials are available at https://zju3dv.github.io/murre/.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究动机

- **问题背景**：多视图 3D 重建旨在从多张已知相机位姿的图像中恢复准确的三维场景几何，在机器人、自动驾驶、虚拟现实等领域有广泛应用。
- **现有方法的困境**：
  - 传统 MVS 与隐式神经重建方法依赖多视图光度一致性优化，在低纹理区域易出现歧义，且重建速度慢。
  - 学习型 MVS 方法（如 MVSNet、IGEV-MVS）虽引入数据驱动的场景先验，但存在三大挑战：
    1. 在 3D 空间中聚合特征构建代价体（cost volume），**GPU 显存消耗极高**，限制了重建分辨率；
    2. 隐式依赖多视图匹配的归纳偏置，**在稀疏视角场景下容易失败**（大量区域无法跨视图匹配）；
    3. 训练依赖高质量 3D 真值数据，而此类数据稀缺，导致**泛化能力受限**。
- **核心思路**：提出 Murre 流水线——**绕过多视图匹配步骤**，改用 SfM（运动恢复结构）引导的单目深度估计来完成多视图重建，从根源上解决显存与稀疏视角问题。

## 2. 方法论

### 2.1 核心思想

- 将 **SfM 点云**（多视图信息的浓缩形式，体现场景显著全局结构）作为显式中间表示，注入到基于扩散模型的单目深度估计器中，从而生成**多视图一致且具有准确尺度**的深度图，最终通过 TSDF 融合重建场景几何。

### 2.2 算法流程

1. **SfM 稀疏重建**：对输入多视图图像运行 SfM 方法（训练与推理默认使用 Detector-free SfM（DF-SfM），推理也可用 COLMAP），得到全局稀疏点云。
2. **深度条件构建**：
   - 将 SfM 点云按可见性投影到每个视图，得到稀疏深度图；
   - 通过 **KNN（k 近邻）插值**对稀疏深度图进行稠密化（k=3，按距离倒数加权平均）；
   - 额外计算一个**距离图**，记录每个像素到最近有效 SfM 深度的欧氏距离，作为网络区分"原始值"与"插值"像素的置信度信号；
   - 对深度做归一化：先剔除每视图深度值上下 2% 的离群点，再将范围扩大至 0.8 倍～1.2 倍，以保证覆盖全图深度范围。
3. **扩散模型深度估计**：
   - 基于 **Stable Diffusion v2** 的潜在扩散框架，冻结 VAE，仅微调 U-Net；
   - 输入条件：RGB 图像 latent code + 稠密化 SfM 深度图的 latent code + 直接缩放到 latent 分辨率的距离图；
   - 前向过程对真实深度 latent 添加高斯噪声：$d_t = \sqrt{\bar{\omega}_t} d_0 + \sqrt{1 - \bar{\omega}_t} \varepsilon$；
   - 训练损失为噪声预测的 MSE：$L = \|\varepsilon - \hat{\varepsilon}\|^2$；
   - 推理采用测试时集成（5 次推理取逐像素中位数）。
4. **深度对齐与多视点融合**：
   - 将预测深度反归一化回 SfM 深度尺度后，用 **RANSAC 线性回归**（估计最优尺度与平移参数）进一步与 SfM 稀疏深度对齐，以消除尺度不一致；
   - 最终通过 TSDF 融合或点云融合得到场景几何。

## 3. 实验设计

### 3.1 训练数据集

- **Hypersim**：465 个室内合成场景，保留 345 个场景、56,077 帧，分辨率下采样至 384×512；
- **3D Ken Burns**：23 个大规模合成场景（每个含两个序列），保留 12 个序列、30,309 帧，中心裁剪至 384×512；
- 总计约 **86.4K 训练帧**，全部经 DF-SfM 处理获得稀疏点云。

### 3.2 评估数据集与基准

| 场景类型 | 数据集 | 评估规模 | 指标 |
|---------|--------|---------|------|
| 物体级 | DTU | 15 个场景（3 视图） | Chamfer 距离（mm） |
| 室内 | ScanNet | 4 个场景 | F-score（5cm 阈值） |
| 室内 | Replica | 3 个场景 | F-score（5cm 阈值） |
| 街道 | Waymo | 32 个静态序列 | 深度 RMSE |
| 航拍 | UrbanScene3D | 2 个场景（覆盖 10⁶ m²） | 定性/补充材料 |

### 3.3 对比方法

- **传统 MVS**：COLMAP、RealityCapture；
- **隐式神经重建**：VolSDF、MonoSDF、NeuralRecon、SimpleRecon；
- **单目深度估计**：Marigold、Depth-Anything、Depth-Anything v2、Metric3D；
- **学习型 MVS**：MVSNet、IGEV-MVS、DUSt3R。

### 3.4 主要结果

- **DTU 上平均 Chamfer 距离为 1.42 mm，超越所有对比方法**，显著优于 MonoSDF（1.86）、MVSNet（2.38）、IGEV-MVS（3.50）、DUSt3R（2.81）、Marigold（5.46）等，且训练数据量（86.4K）远小于 Depth-Anything（1.5M）等方法。

## 4. 资源与算力

- **训练硬件**：8 张 NVIDIA Tesla A100 GPU；
- **训练配置**：batch size 128，训练 50 个 epoch，约 **25 小时**；
- **优化器**：Adam，学习率 3×10⁻⁵；
- **推理硬件**（消融实验）：单张 A6000 GPU；
- 文中明确给出了训练算力配置；消融实验中的推理速度也基于单卡 A6000 进行测试。

## 5. 实验数量与充分性评估

- **实验组数**：约 3 组主要对比实验（DTU 定量、多数据集定性）+ 3 组消融实验：
  1. **精度-速度权衡**：改变去噪步数（10/1）、集成次数（5/1）、对齐方式（RANSAC/最小二乘/无对齐），共 6 种配置测试；
  2. **SfM 方法鲁棒性**：COLMAP、DF-SfM（LoFTR）、DF-SfM（DKM）三种 SfM 的对比，含纹理丰富与纹理缺失场景的可视化分析；
  3. **深度条件设计**：KNN 的 k 值（0/1/3）× 是否使用距离图，共 6 种配置。
- **充分性评价**：
  - **优点**：评估场景覆盖全面（物体、室内、街道、航拍），跨数据集验证泛化性；消融设计针对核心设计要素，能清晰证明各组件（KNN 稠密化、距离图、SfM 方法选择、对齐策略）的贡献。
  - **不足**：每类数据集的测试场景数量较少（如 ScanNet 仅 4 个场景、Replica 仅 3 个场景、UrbanScene3D 仅 2 个场景），统计显著性有限；Waymo 上仅报告深度 RMSE（因缺 3D 真值），未直接评估重建几何质量；部分对比方法（如 NeuralRecon、SimpleRecon）仅在其训练数据集上有良好表现，跨数据集对比的公平性有一定局限。

## 6. 主要结论与发现

- **核心论断**：SfM 点云作为显式条件信号，能有效引导扩散模型生成多视图一致的度量深度图，从而为多视图重建提供一种绕开匹配的新范式。
- **有效性验证**：仅用少量合成数据微调 Stable Diffusion，即可在多种真实场景（物体、室内、街道、航拍）中获得优于 SOTA MVS 与隐式神经重建方法的重建质量。
- **鲁棒性发现**：Murre 对 SfM 方法的选择具有鲁棒性，只要 SfM 能生成合理的稀疏点云即可；KNN 稠密化与距离图条件对性能提升至关重要（k=3 + 距离图：F-score 0.853 vs 无稠密化无距离图：0.528）。
- **灵活性发现**：通过 LCM 蒸馏可实现单步去噪，推理速度大幅提升（0.84s/视角），且重建质量仅轻微下降（F-score 0.853→0.828），提供了精度与速度的可调节权衡。

## 7. 优点

- **范式创新**：首次系统性地将 SfM 先验注入扩散式单目深度估计以解决多视图重建问题，绕开了显存密集的多视图匹配，从根本上缓解稀疏视角困难。
- **泛化能力强**：借助 Stable Diffusion 的 2D 基础模型先验，仅用 86.4K 合成数据微调即可泛化到多样化的真实场景，大幅降低对大规模 3D 真值数据的需求。
- **设计巧妙**：SfM 点云既是多视图信息的"压缩表示"，又能自然投影为图像空间稀疏深度，与单目估计过程高度契合；距离图为网络提供了区分原始/插值像素的显式信号。
- **工程实用性强**：支持多种 SfM 方法（DF-SfM/COLMAP）、多种融合方式（点云融合/TSDF 融合），且推理速度可通过步数与集成数灵活调节，兼顾质量与效率。

## 8. 不足与局限

- **对 SfM 的依赖**：必须预先运行 SfM 获取相机位姿与稀疏点云，在极端情形（如仅两张重叠极小的视图）下 SfM 无法工作，重建流程完全失效。
- **仅支持静态场景**：方法假设场景是静态的，难以处理包含运动物体的场景（如街道上的车辆、行人），限制了动态环境的应用。
- **实验覆盖有限**：各数据集的测试场景数量偏少，统计可靠性有待加强；Waymo 等数据集缺少 3D 几何真值，只能以深度误差间接评估。
- **对齐步骤的依赖风险**：虽然 RANSAC 对齐提升了精度，但当 SfM 点云质量较差（如 COLMAP 在低纹理区域产生极稀疏、含噪点云）时，深度预测误差仍较显著。
- **作者自述的改进方向**：未来可结合 DUSt3R 类方法处理极端输入情形，引入跟踪方法处理动态场景。

（完）
