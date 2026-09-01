---
title: "ViPOcc: Leveraging Visual Priors from Vision Foundation Models for Single-View 3D Occupancy Prediction"
title_zh: ViPOcc：利用视觉基础模型先验进行单视图三维占用预测
authors: "Yi Feng, Yu Han, Xijing Zhang, Tanghui Li, Yanting Zhang, Rui Fan"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32308/34463"
tags: ["query:mono-depth"]
score: 4.0
evidence: 在三维占用预测中引入度量深度估计分支
tldr: 单目三维场景理解中，占用预测常缺乏实例级语义和光度一致性。ViPOcc利用视觉基础模型先验，并引入度量深度估计分支，通过逆深度对齐弥合深度分布域差。该方法在三维占用预测任务上获得更精细的结构与语义重建结果。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 516, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1818, \"height\": 704, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 856, \"height\": 585, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 827, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32308/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 862, \"height\": 502, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 856, \"height\": 347, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 864, \"height\": 351, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 879, \"height\": 308, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 389, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 894, \"height\": 522, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32308/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 858, \"height\": 221, \"label\": \"Table\"}]"
motivation: 现有单视图三维占用预测缺乏实例级语义推理和时间光度一致性。
method: 利用视觉基础模型先验，加入度量深度估计分支，并设计逆深度对齐模块来适配深度分布。
result: 在占用预测任务上取得了更精细的结构与语义重建，验证了深度分支的有效性。
conclusion: 视觉基础模型与度量深度分支可有效增强单视图三维占用预测。
---

## Abstract
Inferring the 3D structure of a scene from a single image is an ill-posed and challenging problem in the field of vision-centric autonomous driving. Existing methods usually employ neural radiance fields to produce voxelized 3D occupancy, lacking instance-level semantic reasoning and temporal photometric consistency. In this paper, we propose ViPOcc, which leverages the visual priors from vision foundation models (VFMs) for fine-grained 3D occupancy prediction. Unlike previous works that solely employ volume rendering for RGB and depth image reconstruction, we introduce a metric depth estimation branch, in which an inverse depth alignment module is proposed to bridge the domain gap in depth distribution between VFM predictions and the ground truth. The recovered metric depth is then utilized in temporal photometric alignment and spatial geometric alignment to ensure accurate and consistent 3D occupancy prediction. Additionally, we also propose a semantic-guided non-overlapping Gaussian mixture sampler for efficient, instance-aware ray sampling, which addresses the redundant and imbalanced sampling issue that still exists in previous state-of-the-art methods. Extensive experiments demonstrate the superior performance of ViPOcc in both 3D occupancy prediction and depth estimation tasks on diverse public datasets.

---

## 论文详细总结（自动生成）

## ViPOcc：利用视觉基础模型先验进行单视图三维占用预测（AAAI-2025）

### 1. 核心问题与研究动机

单视图三维占用预测是自动驾驶视觉感知中的关键任务，即从一张二维图像推断场景的完整三维几何结构。该任务本质上是**病态问题（ill-posed problem）**——同一张图像可能对应无限多种合理的三维结构，因此需要强大的先验知识来约束解空间。

现有方法的不足主要体现在三方面：

- **缺乏实例级语义推理能力**：现有的 NeRF（神经辐射场）类方法（如 BTS、KYN）通过体渲染重建三维密度场，但往往只是全局性地重建场景结构，对关键实例（如车辆、行人）的边界和细节恢复不佳。
- **时间光度一致性未充分利用**：相邻帧之间的几何与光度约束未被有效挖掘，导致跨帧的三维占用预测不一致，出现“拖尾”等伪影。
- **采样策略存在冗余与不均衡**：现有方法通常采用随机射线采样，导致大量计算资源浪费在无信息的背景区域，而关键小目标却常被遗漏。

**核心思想**：视觉基础模型（VFMs）已在大量数据上学习了丰富的视觉先验——Depth Anything V2 提供深度空间先验，Grounded-SAM 提供实例级语义先验。ViPOcc 的核心动机是将这些先验引入三维占用预测框架，通过**度量深度估计分支**和**语义引导采样**两个机制，从空间和时间两个维度同时约束三维重建，克服单视图重建的病态性。

### 2. 方法论

ViPOcc 的整体架构由**两个耦合的分支**组成：度量深度估计分支和三维占用预测分支，二者通过统一损失函数协同训练。

#### 2.1 整体框架

- 输入为相邻时刻的立体图像对（透视相机 I₀/I₁ + 鱼眼相机 I₂/I₃），以 I₀ 为主帧。
- 两个并行编码器分别提取**空间特征 F_s**（用于深度估计）和**重建特征 F_r**（用于三维占用预测）。
- 深度分支生成度量深度图；占用分支通过 MLP 预测三维密度场，再经体渲染生成 RGB 和深度图像块。
- 两条分支通过采样器、时间对齐损失和重建一致性损失紧密耦合。

#### 2.2 逆深度对齐模块（核心创新之一）

**问题**：Depth Anything V2 等 VFM 输出的伪深度图与真实度量深度之间存在显著分布偏差（尺度模糊+域差距），不能直接用于光度对齐或几何约束。

**方法**：不直接拟合深度残差，而是拟合**残差逆深度（residual inverse depth）**：

- 定义残差函数 F(x) = 1/D̂(x) − 1/D_p(x)，其中 D_p 为 VFM 伪深度，D̂ 为精炼后的度量深度。
- 选择逆深度而非深度本身的原因是：深度值范围大（0~80m），神经网络直接拟合大范围数值变化时难以收敛；而逆深度的数值范围更集中，更易拟合且能保留局部细节。
- 精炼深度计算公式：**D̂(x) = 1 / (1/D_p(x) + f(F_s, θ) + ε)**，其中 f 为卷积层，ε 为防零小常数。

#### 2.3 语义引导非重叠高斯混合采样器（SNOG Sampler，核心创新之二）

**问题**：随机均匀射线采样导致背景区域冗余采样和关键实例遗漏。

**方法**：利用 Grounded-SAM 的视觉先验实现实例感知采样：

1. 使用 Cityscapes 语义标签作为提示，经 Grounding DINO 获得实例级边界框，再由 SAM 生成精确分割掩码。
2. 对每个实例 k 提取元数据 M_k = {l_k, b_k, s_k}（边界框中心、半宽高、语义面积）。
3. 构建高斯混合 + 背景均匀分布的混合概率密度函数：
   **p(x) = (1−γ)Σ π_k·N(x|μ_k,Σ_k) + γ·U(x|s)**
   - 高斯分量均值 μ_k 设为边界框中心，协方差 Σ_k 按边界框尺寸初始化（保证约 95.5% 的采样落在框内）
   - 实例权重 π_k 在**对数空间**归一化，防止小目标采样概率趋近于零
4. 通过条件分布约束（禁止已有采样点临近区域重复采样）确保采样块**互不重叠**。

#### 2.4 损失函数

总损失：**L = λ₁·L_ta + λ₂·(L_drc + L_rgb_rc)**

- **时间对齐损失 L_ta**：利用预测的度量深度和相机位姿，将相邻帧 I₀^{t+1} 反投影变形（warp）到当前帧视角，约束变形后的图像与原始图像的光度一致性（配合边缘感知掩码 M 抑制运动物体和遮挡区域干扰）。
- **深度重建一致性损失 L_drc**：约束体渲染得到的深度图与深度分支预测的度量深度图一致，利用 D(x)·x̃ = Kp 与 ‖p‖₂ = D̂_r(x) 的几何关系建立两者间的直接约束，使占用预测和深度估计相互校准。
- **RGB 重建一致性损失 L_rgb_rc**：采用 SSIM + L1 的组合损失，约束渲染 RGB 图像块与原始 RGB 图像块一致。

### 3. 实验设计

#### 3.1 数据集

| 数据集 | 用途 | 说明 |
|--------|------|------|
| **KITTI-360** | 主训练/评估集 | 训练集 98,008 张、验证集 11,451 张、测试集 446 张；图像 192×640；深度上限 80m |
| **KITTI Raw** | 深度估计评估 | 采用 Eigen split 协议 |
| **DDAD** | 零样本泛化测试 | 用 KITTI-360 训练的权重直接测试 |

#### 3.2 评估指标

- **三维占用预测**：场景占用准确率（O_sacc）、不可见场景准确率/召回率（IE_sacc、IE_srec）、物体占用准确率（O_oacc）、不可见物体准确率/召回率（IE_oacc、IE_orec）
- **深度估计**：Abs Rel、Sq Rel、RMSE、RMSE log、阈值精度 δ<1.25^i

#### 3.3 对比方法

- **自监督深度估计**：Monodepth2、SwinDepth、Lite-Mono、SC-DepthV3 等
- **NeRF 类单视图重建**：PixelNeRF、BTS、KYN、MVBTS、KDBTS
- **消融对比**：随机采样器 vs SNOG 采样器；不同深度对齐策略

### 4. 资源与算力

论文明确给出的训练配置信息如下：

- **GPU**：NVIDIA RTX 4090 × 1（未说明是否使用多卡）
- **优化器**：Adam，初始学习率 1e-4
- **训练轮数**：25 epochs，最后 10 epochs 学习率衰减 10 倍
- **未明确披露的信息**：总训练时长、单卡 vs 多卡并行方式、内存占用等

> 总体而言，论文对算力资源的披露较为有限，仅提供了基本的训练配置信息。

### 5. 实验数量与充分性

实验设计较为全面，主要包含以下实验组：

1. **KITTI-360 场景三维占用预测对比实验**（表1）：与 Monodepth2、PixelNeRF、BTS、KYN 对比
2. **KITTI-360 物体级占用预测对比实验**（表2）：同上方法对比
3. **KITTI-360 度量深度估计对比实验**（表3）：与 BTS、KYN 及 VFM 伪深度直接对比
4. **KITTI Raw 深度估计对比实验**（表4）：与 6 种方法对比
5. **DDAD 零样本深度估计实验**（表6）：验证泛化性
6. **消融实验**（表5）：系统验证三个核心模块（逆深度对齐、SNOG 采样器、损失函数设计）各自的贡献

**充分性评估**：
- ✅ 消融实验设计科学，逐步验证每个组件的必要性，且发现了一些有价值的反直觉现象（如仅用重建一致性损失反而导致性能下降）
- ✅ 零样本测试增强了泛化性说服力
- ✅ 定性可视化（图4、图6）与定量指标互补
- ⚠️ 缺乏在更大规模数据集（如 nuScenes）上的验证，所有数据集均为 KITTI 系列（同一传感器设置），场景多样性有限
- ⚠️ 消融实验的配置组合有限，未展示不同 λ 权重组合的超参数敏感性分析

### 6. 主要结论与发现

1. **VFM 视觉先验能显著提升单视图三维重建质量**：将 Depth Anything V2 的深度先验和 Grounded-SAM 的语义先验引入训练流程，有效缓解了单视图重建的病态性。
2. **逆深度对齐是连接 VFM 深度先验和三维重建的关键技术**：直接使用伪深度或直接拟合深度残差均会导致性能下降（表5中对应配置分别降至0.86/0.91的O_sacc，低于基线），而逆深度残差拟合带来显著提升。
3. **实例感知采样优于均匀随机采样**：SNOG 采样器在不可见场景准确率和召回率上分别提升 3.1% 和 4.7%，证明关注关键实例对三维重建质量至关重要。
4. **时间对齐与重建一致性损失互补协同**：仅用单一损失效果有限甚至有害，组合后显著提升三维重建性能（IE_sacc 从 0.65 提升至 0.71，IE_srec 从 0.64 提升至 0.69）。
5. **ViPOcc 在三维占用预测和深度估计两个任务上均达到 SoTA**，并展现出良好的零样本泛化能力，验证了双分支耦合训练范式的有效性。

### 7. 方法亮点

- **创新性的“先验利用”思路**：不是简单地将 VFM 输出作为监督信号，而是设计了专门的模块（逆深度对齐 + SNOG 采样器）来**适配和引导**先验，使先验能够真正服务于三维重建任务。
- **双分支耦合的协同训练范式**：深度估计分支和三维占用预测分支互为约束——深度分支提供的时间对齐和空间几何约束提升了占用预测的时空一致性；占用预测的体渲染深度反过来约束深度分支保持几何真实感。这种双向促进关系是此前方法所不具备的。
- **逆深度残差设计的合理性**：从数值优化角度解释了为何拟合逆深度优于拟合深度本身，这一考虑体现对问题本质的深入理解。
- **对数空间归一化的采样权重**：有效解决实例面积差异悬殊时的采样不平衡问题，该设计细节有较强的实用性。
- **层次清晰的消融实验**：每个模块的贡献得到清晰验证，结论可信度高。

### 8. 不足与局限性

- **算力披露不充分**：未报告总训练时长、GPU 数量和显存占用，不利于复现和公平比较训练成本。
- **数据集覆盖有限**：仅使用 KITTI-360 和 KITTI Raw（同一系列的自动驾驶数据集），场景分布在城市道路为主，雨雪/夜间等极端场景未覆盖；DDAD 零样本测试的规模也有限。
- **VFM 依赖的固有限制**：Grounded-SAM 的实例发现依赖 Cityscapes 的固定语义类别，无法发现未知类别的新颖物体；Depth Anything V2 对极端场景（强逆光、重度遮挡）的预测质量可能影响后续对齐。
- **推理效率问题**：Grounded-SAM 的实例分割在推理阶段是否引入额外开销、双分支并行编码器的参数量与推理时延均未讨论。作者虽在结论中提及“未来开发更轻量化的框架”，说明当前方案在效率上仍有优化空间。
- **深度范围限制**：深度上限为 80m，对近距离小物体的重建精度验证了，但远距离物体的三维重建质量缺乏评估。
- **动态物体处理**：虽然有掩码机制缓解运动物体带来的干扰，但对高速运动物体的鲁棒性未做专门分析。
- **无真实三维标注的评估局限**：三维占用预测的指标本身基于几何推理构建（如将可见像素后方点视为占用），这种评估方式的客观性虽沿袭了已有工作，但始终存在一定的近似性。

（完）
