---
title: "A Paradigm Shift in High-Resolution Depth Estimation Using SPAD-Based LiDAR Histograms: From Signal Filtering to Lightweight Similarity Learning"
title_zh: 基于SPAD LiDAR直方图的高分辨率深度估计范式转变：从信号滤波到轻量相似性学习
authors: "Minsung Lee, Seo Hyun Kim, Yeonsu Park, Hyeongseok Seo, Jongmin Lee"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37513/41475"
tags: ["query:mono-depth"]
score: 8.0
evidence: 从ToF LiDAR直方图中进行轻量相似性学习实现深度估计
tldr: 针对传统直方图深度估计在平衡性能和计算成本上的局限，以及现有深度学习方法难以在边缘设备部署的问题，提出将ToF深度估计从信号滤波范式转变为轻量相似性学习范式。该方法通过轻量网络学习直方图间的相似性，避免了复杂的物理建模。实验表明，该方法在保持高分辨率深度精度的同时，大幅降低了计算开销，适合在移动端等资源受限设备上实时运行。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 866, \"height\": 747, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 882, \"height\": 284, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 885, \"height\": 835, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 870, \"height\": 566, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1812, \"height\": 748, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1842, \"height\": 402, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1811, \"height\": 395, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1829, \"height\": 589, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37513/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1813, \"height\": 830, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37513/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 872, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37513/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 863, \"height\": 416, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37513/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 191, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37513/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 870, \"height\": 106, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37513/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 205, \"label\": \"Table\"}]"
motivation: 现有直方图深度估计算法难以在性能和计算开销间取得平衡，且深度学习模型过大无法边缘部署。
method: 提出将深度估计重构为轻量相似性学习问题，利用神经网络学习直方图特征之间的相似性来回归深度值。
result: 在保持高分辨率深度精度的同时，显著降低计算成本，适用于边缘硬件。
conclusion: 为移动设备上高效深度估计提供了新的轻量范式。
---

## Abstract
Accurate and efficient depth estimation from time-of-flight (ToF) LiDAR is essential for autonomous systems operating in real-world environments. However, traditional histogram-based depth estimation (HBDE) algorithms face fundamental limitations in balancing depth performance and computational cost, and they struggle under signal-induced pile-up distortion. While deep learning has shown promise, existing neural network-based methods rely on large models that are impractical for deployment on edge hardware. To bridge this critical gap, we propose a paradigm shift in histogram-based ToF estimation, reframing depth estimation from signal filtering to lightweight similarity learning. Instead of attempting to correct the distorted signal, our approach learns a specialized metric where the measure of similarity between the distorted histogram and a reference pulse is the temporal shift itself. The resulting 57.61 KB model, over 215.2 times smaller than state-of-the-art deep learning approaches, achieves real-time performance (106.27 fps) on an FPGA. It delivers superior accuracy across nearly all signal-noise conditions, including 2.21 cm RMSE at severe pile-up scenarios, significantly outperforming conventional methods while remaining practical for on-device deployment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：传统基于直方图的深度估计算法（HBDE）在精度与计算成本之间难以平衡，且在强信号导致的“堆积失真”（pile-up distortion）下性能严重下降。现有深度学习方法虽精度高，但模型庞大，无法部署在FPGA等边缘硬件上，存在“高性能”与“可部署性”之间的关键缺口。
- **整体含义**：本文提出一种范式转变——从**信号滤波**转向**轻量相似性学习**。不再尝试校正畸变信号，而是学习一种专用度量，使得畸变直方图与参考脉冲之间的相似性直接反映时间偏移（即深度）。由此得到的模型仅57.61 KB，比现有最优深度学习模型小215.2倍，可在FPGA上实现106.27 fps的实时推理，并在严重堆积场景下仍保持2.21 cm的RMSE，为资源受限设备上的高精度深度估计提供了可行方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
- 将ToF估计重构为**相似性学习**问题：学习一个度量函数，使得畸变直方图与参考脉冲之间的特征差异直接回归出时间偏移（ToF）。
- 采用**孪生网络（Siamese Network）**结构，两个分支共享权重，分别处理参考脉冲和实测直方图，提取鲁棒的特征，然后通过特征差回归ToF。

### 关键技术细节
- **输入**：64-bin精细直方图（分辨率6.25 cm，时间间隔416.66 ps）及粗ToF（CToF）辅助值。
- **网络架构**（LITOFNET）：
  - 每个分支：两个卷积层 + 平均池化 + ReLU激活。
    - 第一层：输入通道1，输出14通道，kernel=12，stride=3，无padding。
    - 第二层：输入14通道，输出42通道，kernel=3，stride=2，padding=2。
    - 每个卷积后接平均池化（kernel=2，stride=2），以保留信号分布特征。
  - 展平后特征向量h₁和h₂，计算差值 h₂ - h₁，送入全连接回归头输出最终ToF估计值 ˆt_ToF。
- **损失函数**：均方误差（MSE） L = (ˆt_ToF - t_ToF)²。
- **训练细节**：
  - 优化器：Adam，学习率3.87×10⁻⁴，batch size=100。
  - 超参数通过Optuna调优。
  - 最大训练500 epoch，早停耐心100。
  - 权重初始化：Kaiming初始化。
- **数据生成**：利用仿真框架，根据实际激光脉冲波形、噪声、反射率、死时间等参数，生成4M个精细直方图样本（含堆积失真），其中3M训练，91K验证。
- **硬件优化**：
  - 预计算静态参考脉冲的分支输出（因共享权重）。
  - 采用bfloat16格式（精度与效率的最佳折中）。
  - FPGA流水线设计，实现0.84 μs/像素的推理。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

### 实验场景与数据集
1. **非均匀反射率平面墙的模拟实验**：192像素模拟墙，四段交替10%和90%反射率，距离10 m附近，模拟中等至严重堆积。
2. **真实世界日间单点扫测**：1 m×1 m朗伯目标（10%反射率），5~58 m，94.6 klux日光下，每点1000次采集。
3. **真实世界夜间室外测量**：商用SPAD LiDAR模块（SV110），采集不同距离（~12 m到~60 m）和反射率下的数据。
4. **Middlebury数据集**（Scharstein & Pal 2007; Hirschmuller & Scharstein 2007）：8个场景，评估多种信噪比组合。

### Benchmark与对比方法
- **传统HBDE方法**：Buller & Wallace 2007, Niclass et al. 2014, Okino et al. 2020, Gyongy et al. 2020。
- **堆积校正方法**：Rapp et al. 2019, Heide et al. 2018。
- **深度学习方法**：Shin et al. 2016, Lindell et al. 2018, Rapp & Goyal 2017。
- 注意：Peng et al. 2023和Yu et al. 2025因代码未公开未参与对比。

## 4. 资源与算力

- **论文中明确说明**：所有真实数据使用商用SPAD LiDAR模块（SV110）采集，FPGA为Xilinx Kintex-7；CPU推理使用Intel i9-14900k，GPU为NVIDIA RTX 4090。
- **训练算力**：未明确提及训练所用GPU数量、型号、训练时长。仅提到训练使用3M模拟样本，最大500 epoch，早停机制。因此无法准确评估训练开销，但模型极小（57.61 KB），推断训练应较为轻量。

## 5. 实验数量与充分性

- **共进行了四大部分实验**：
  - 平面墙堆积模拟（4组反射率区域，192像素）。
  - 日间单点扫测（50+个距离点，每个1000次采集）。
  - 夜间室外真实数据（4个代表性点）。
  - Middlebury数据集（8个场景，多种信噪比组合，共9+5=14种条件）。
- **消融实验**：
  - 网络组件消融：去掉第二卷积层、隐藏层等，验证各组件必要性（表3）。
  - 数据精度消融：bfloat16 vs float32 vs 定点8bit（表4）。
- **充分性评价**：实验覆盖了模拟与真实、近场与远场、强光与弱光、均匀与非均匀反射率等多种关键场景；对比了多种传统和深度方法；消融设计合理。但缺少与其他紧凑型网络（如MobileNet）的对比，且只在一个FPGA型号上验证部署，略有限制。

## 6. 论文的主要结论与发现

- **范式转变有效**：将深度估计从信号滤波转为轻量相似性学习，可同时实现高精度和低计算成本。
- **极小的模型**：57.61 KB，比SOTA深度模型小215.2倍，可在FPGA达到106.27 fps实时处理。
- **强鲁棒性**：在严重堆积（1000:50信号-噪声比）下RMSE仅2.21 cm；在非均匀反射率墙上误差0.023 m，远优于其他方法（0.248–0.344 m）。
- **通用性**：在Middlebury数据集上，所有信噪比条件下均取得最优或次优结果，性能稳定。
- **实际部署可行**：bfloat16精度在准确度和硬件效率间取得最佳平衡；FPGA实现0.84 μs/像素。

## 7. 优点

- **创新性**：首次将Siamese网络应用于ToF直方图深度估计，通过相似性学习隐式处理堆积失真，避免了复杂的物理建模。
- **极致轻量**：模型大小仅57.61 KB，是同类方法中最小之一，适合边缘部署。
- **实验充分且客观**：包含模拟、真实室内外、标准数据集，并在多个条件下与大量方法公平对比。
- **硬件验证**：不仅提出算法，还完成了FPGA实现并报告了实际吞吐量和延迟。
- **数据合成方法**：基于实际激光脉冲波形和物理参数（死时间、光子统计）生成逼真训练数据，涵盖了噪声、反射率、距离的广泛组合，增强了泛化能力。

## 8. 不足与局限

- **训练数据依赖模拟**：虽模拟参数贴近真实，但未在论文中提供真实-模拟迁移的消融实验，可能存在域差异。
- **对比方法完整性**：未对比Peng et al. (2023)和Yu et al. (2025)等近期深度学习方法（代码不可用），无法完全反映最新SOTA水平。
- **硬件仅限于单款FPGA**：未比较在其他边缘设备（如Jetson、ASIC）上的表现。
- **未讨论多物体遮挡或运动模糊**：实验场景相对理想（静态目标、单一反射面），真实复杂环境可能带来新挑战。
- **未报告训练成本**：训练时间、GPU型号、能耗等未提及，不利于评估离线训练开销。
- **消融实验范围有限**：未探索不同卷积层数、核大小、激活函数等对性能的影响，仅固定一个最优配置。

（完）
