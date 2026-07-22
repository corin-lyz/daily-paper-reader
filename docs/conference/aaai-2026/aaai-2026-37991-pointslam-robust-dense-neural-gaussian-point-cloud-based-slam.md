---
title: "PointSLAM++: Robust Dense Neural Gaussian Point Cloud-based SLAM"
title_zh: "PointSLAM++: 基于鲁棒神经高斯点云的稠密SLAM系统"
authors: "Xu Wang, Boyao Han, Xiaojun Chen, Ying Liu, Ruihui Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37991/41953"
tags: ["query:depth-refine"]
score: 4.0
evidence: RGB-D SLAM使用神经高斯表示处理深度噪声和结构一致性，与深度精修相关
tldr: 面向实时重建中深度噪声导致定位和结构不稳定的问题，提出PointSLAM++系统。采用分层约束神经高斯表示保持结构关系，渐进式位姿优化降低深度传感器噪声。实验表明该方法在定位精度和建图质量上优于现有SLAM方法，其深度优化策略可借鉴于深度图精修。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37991/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 851, \"height\": 664, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37991/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1823, \"height\": 632, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37991/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 867, \"height\": 414, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37991/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 345, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37991/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1834, \"height\": 769, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37991/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 379, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37991/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 834, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37991/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 371, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37991/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 877, \"height\": 182, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37991/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 876, \"height\": 221, \"label\": \"Table\"}]"
motivation: 现有SLAM在深度噪声下难以保持结构一致性和鲁棒位姿估计。
method: 引入分层约束神经高斯表示和渐进式位姿优化，动态调整高斯节点分布。
result: 在多个基准数据集上实现更低的定位误差和更高的重建质量。
conclusion: 神经高斯表示能有效应对深度噪声提升SLAM鲁棒性。
---

## Abstract
Real-time 3D reconstruction is crucial for robotics and augmented reality, yet current simultaneous localization and mapping(SLAM) approaches often struggle to maintain structural consistency and robust pose estimation in the presence of depth noise. This work introduces PointSLAM++, a novel RGB-D SLAM system that leverages a hierarchically constrained neural Gaussian representation to preserve structural relationships while generating Gaussian primitives for scene mapping. It also employs progressive pose optimization to mitigate depth sensor noise, significantly enhancing localization accuracy. Furthermore, it utilizes a dynamic neural representation graph that adjusts the distribution of Gaussian nodes based on local geometric complexity, enabling the map to adapt to intricate scene details in real time. This combination yields high-precision 3D mapping and photorealistic scene rendering. Experimental results show PointSLAM++ outperforms existing 3DGS-based SLAM methods in reconstruction accuracy and rendering quality, demonstrating its advantages for large-scale AR and robotics.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究动机
- **问题背景**：实时三维重建在机器人、AR/VR中至关重要，但现有SLAM系统在深度传感器噪声干扰下，难以同时保持结构一致性和鲁棒位姿估计。
- **现有方法局限**：
  - 传统RGB-D SLAM（如KinectFusion）跟踪稳定但重建细节粗糙。
  - 神经隐式方法（如iMAP、NICE-SLAM）计算成本高、扩展性差。
  - 3D高斯溅射（3DGS）SLAM（如MonoGS、SplaTAM）存在三大挑战：过度依赖精确深度（噪声下性能骤降）、高斯球过拟合（忽略局部结构导致冗余）、视角依赖弱（插值能力差，对大视角变化不鲁棒）。
- **研究目标**：设计一个实时RGB-D SLAM系统，能够应对深度噪声，实现高精度建图与真实感渲染，并具备大尺度场景适应性。

## 2. 方法论：PointSLAM++系统核心思想与技术细节
### 2.1 整体框架
- 输入：RGB-D图像序列。输出：相机轨迹与稠密神经高斯地图。
- 流程：每帧通过下采样和反投影生成点云，使用GICP+ORB特征估计初始位姿，然后通过分层锚点机制生成神经高斯，经特殊高斯光栅化渲染，并在建图阶段持续优化MLP。

### 2.2 渐进式位姿优化（Progressive Pose Optimization, PPO）
- **目的**：消除深度噪声导致的定位错误，提升鲁棒性。
- **步骤**：
  1. **GICP粗配准**：对前后帧点云进行刚体注册，最小化距离误差，得到初始位姿估计。
  2. **ORB特征辅助精细配准**：利用多尺度金字塔提取ORB特征并融合深度，通过点-面约束进一步优化位姿，初始化为GICP结果以加快收敛。
  3. **全局BA优化**：联合优化相机位姿和3D地图点，最小化重投影误差，使用Huber损失和深度先验，在SE(3)流形上参数化，采用Levenberg-Marquardt求解。
  4. **追踪失败恢复**：当跟踪丢失时，利用PnP+RANSAC进行全局特征匹配重定位。

- **关键公式**：重投影误差目标函数 \( \min_{\xi} \sum_{k} \rho(e_k^T \Sigma_k^{-1} e_k) \)，其中 \( e_k = x_k - \pi(K T X_k) \)，\( T = \exp(\xi^\wedge) \)。

### 2.3 神经高斯表示（Neural-Gaussian Representation）
- **锚点生成**：将SLAM跟踪中稳定的ORB特征点云（结合深度数据）转化为神经锚点，每个锚点包含位置 \( p_v \)、局部特征 \( f_v \)、缩放因子 \( l_v \)、可学习偏移 \( O_v \)。
- **两级锚点机制**：
  - **主锚点**：由ORB特征生成，不可分裂或删除，以较小学习率优化，保证位姿估计稳定性。
  - **次锚点**：基于深度数据和小体素产生，通过体素梯度检测（平均梯度 \( \nabla_g \) 超过阈值 \( \tau_g \) 则新增）动态添加或删除，用于刻画无纹理区域的细节。
- **神经高斯属性预测**：每个锚点通过MLP网络 \( F_a \) 生成 \( k \) 个神经高斯，属性包括位置、不透明度 \( \alpha \)、四元数 \( q \)、缩放 \( s \)、颜色 \( c \)。位置由锚点位置和可学习偏移与缩放计算得到。
- **视角依赖环境补偿**：将归一化视角向量 \( v \) 与锚点外观特征 \( \hat{f}_a \) 通过嵌入MLP融合，得到增强的上下文特征 \( f'_a \)，显式建模视角变化带来的光照与反射变化，提升跨视角渲染一致性。

### 2.4 建图与更新策略
- **关键帧选择**：基于几何一致性（点云平均距离）动态选取，新增高斯点仅添加到非重叠区域。
- **地图更新**：优化多损失函数（颜色L1、SSIM、深度L1），并加入尺度正则化项控制高斯椭球趋向各向同性，平衡跟踪精度与渲染真实感。
- **建图损失**：\( L_{\text{mapping}} = \lambda_{I1} L_1(I, I_{gt}) + \lambda_{I2} L_{\text{D-SSIM}}(I, I_{gt}) + \lambda_D L_1(D, D_{gt}) \)。

## 3. 实验设计
### 3.1 数据集与基准
- **Replica**：高精度合成RGB-D数据，用于基础验证。
- **TUM-RGBD**：真实室内场景，含有运动模糊、深度噪声，标准SLAM跟踪精度评估数据集。
- **ScanNet++**：大规模真实室内数据，相机位姿间隔大、深度图有误差，测试鲁棒性与大尺度能力。

### 3.2 对比方法
- 神经隐式方法：NICE-SLAM、Point-SLAM
- 高斯SLAM方法：SplaTAM、MonoGS、Photo-SLAM、GS-ICP SLAM
- 所有方法使用官方代码复现，确保公平。

### 3.3 评估指标
- 渲染质量：PSNR↑、SSIM↑、LPIPS↓
- 定位精度：ATE RMSE↓（绝对轨迹误差）

### 3.4 消融实验
- 去掉渐进式位姿优化（PPO）→ 仅用GICP导致定位显著恶化（ATE从1.08cm升至17.33cm）。
- 去掉神经高斯表示（NeuGS）与视角补偿（VDC）→ 渲染指标明显下降（PSNR从26.16降至24.06）。
- 仅去掉VDC → PSNR 25.36，证实视角补偿对跨视角渲染关键。
- 消融实验数据来自TUM-RGBD平均结果。

## 4. 资源与算力
- **硬件**：Intel Xeon Silver 4314 CPU + 单张Nvidia RTX 3090 GPU。
- **软件**：基于PyTorch实现。
- **效率**：ScanNet++上，建图时间8分13秒，跟踪时间1分钟，FPS 3.33，GPU内存占用10.11 GB（对比SplaTAM 20.32 GB，MonoGS 23.76 GB）。未提及训练总时长或具体迭代轮数。

## 5. 实验数量与充分性
- **实验数量**：包含三大数据集上的定量对比（表1-3）、效率与内存对比（表4）、消融研究（表5，4组变体）。此外有定性渲染对比图（图1,5）。
- **充分性**：
  - 覆盖多种场景（合成/真实、小/大尺度、干净/含噪）。
  - 对比方法涵盖主流神经隐式与高斯SLAM，且复现官方代码。
  - 消融实验验证核心模块贡献。
  - 但缺少动态场景、室外场景、单目或双目模式的实验；未报告统计显著性检验；消融仅在TUM上进行，未在Replica或ScanNet++上重复。

## 6. 主要结论与发现
- PointSLAM++在渲染质量（PSNR、SSIM、LPIPS）和定位精度（ATE）上显著优于所有对比方法，尤其在深度噪声大、相机运动快的场景（TUM、ScanNet++）中优势明显。
- 渐进式位姿优化有效抑制深度噪声对定位的干扰，比纯GICP方法ATE降低超过90%。
- 神经高斯表示与视角补偿联合提升了重建细节和跨视角一致性。
- 系统在扫描Net++上实现了稳定的跟踪与高质量重建，而GS-ICP SLAM等方法失败。
- 效率方面：比神经隐式方法（SplaTAM、MonoGS）更快、更省显存，但比纯几何方法（GS-ICP SLAM）慢。

## 7. 优点
- **方法创新点**：
  - 首次将分层约束神经高斯表示引入SLAM，结合稳定主锚点与动态次锚点，同时保证定位精度和细节建模。
  - 渐进式多阶段位姿优化，从粗到精融合几何与视觉线索，并内嵌重定位机制。
  - 显式的视角依赖补偿大大提升了渲染对光照变化的鲁棒性。
- **实验设计**：
  - 使用三个代表性数据集，覆盖理想、真实、大尺度场景。
  - 与多种先进方法公平对比（复现官方代码）。
  - 消融充分，验证每个关键模块的贡献。
- **性能**：在多个指标上实现SOTA，尤其在深度噪声场景下鲁棒性突出。

## 8. 不足与局限
- **计算成本**：虽然比神经隐式方法高效，但比纯几何方法（GS-ICP SLAM）慢（FPS 3.33 vs 2.84? 实际GS-ICP SLAM 2.84更低？但论文称“更快”可能有误，需核实：实际上GS-ICP SLAM FPS 2.84，本文FPS 3.33，更快）。但整体训练时间仍较长，未来需要提升效率以部署在移动设备。
- **实验覆盖**：
  - 未测试动态场景（如人群、移动物体）下的表现。
  - 仅评估RGB-D模式，未扩展到单目或双目。
  - 仅限室内数据集，室外大场景（如KITTI）未见评测。
  - 消融实验仅在TUM数据集上，数据量小（3个序列的平均），可能不足以代表所有场景。
- **潜在偏差**：使用ORB特征可能对低纹理区域依赖不足，次锚点生成依赖深度数据，深度缺失时效果可能下降。
- **应用限制**：系统未提及闭环检测和全局优化，长轨迹漂移可能累积。

（完）
