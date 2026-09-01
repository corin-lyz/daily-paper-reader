---
title: "DiffSQL: Leveraging Diffusion Model for Zero-Shot Self-Supervised Monocular Depth Estimation"
title_zh: DiffSQL：利用扩散模型进行零样本自监督单目深度估计
authors: "(PDF |   Details)"
date: 2025-08-01
pdf: "https://www.ijcai.org/proceedings/2025/0981.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 基于扩散模型的零样本自监督单目深度估计
tldr: 针对自监督单目深度估计依赖大量无标注视频且泛化不足的问题，本文提出DiffSQL，利用扩散模型的强大生成先验，在无需真值标签的情况下实现零样本自监督单目深度估计。该方法旨在提升模型对未知场景的泛化能力，展示了扩散模型在几何估计任务中的潜力。
source: IJCAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 1053, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1772, \"height\": 685, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1785, \"height\": 774, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 268, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 844, \"height\": 599, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1836, \"height\": 504, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-981/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 855, \"height\": 539, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1830, \"height\": 361, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1826, \"height\": 341, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 884, \"height\": 319, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 885, \"height\": 267, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 846, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-981/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 846, \"height\": 225, \"label\": \"Table\"}]"
motivation: 自监督单目深度估计在缺少标签时泛化能力受限，需要更强的先验。
method: 利用扩散模型构建自监督训练框架，实现零样本深度估计。
result: 在零样本场景下取得有竞争力的深度估计性能。
conclusion: 为自监督深度估计开辟了使用扩散模型的新途径。
---

## Abstract
No abstract is available.

---

## 论文详细总结（自动生成）

# DiffSQL：利用扩散模型进行零样本自监督单目深度估计 — 论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **任务背景**：单目深度估计旨在从单张 RGB 图像预测逐像素深度，广泛应用于自动驾驶、增强现实和机器人等领域。
- **现有痛点**：
  - 监督学习依赖 LiDAR 等传感器获取的稀疏深度真值，采集成本高、耗时长，且在新场景中泛化能力受限。
  - 自监督方法（如 Monodepth2、SQLdepth）虽摆脱了对真值标签的依赖，但：
    - 现有基于 CNN 的主干网络（如 ResNet）缺乏足够的空间几何特征提取能力；
    - SQLdepth 的 Self Query Layer（SQL）依赖固定数量的查询对象，且查询特征缺乏语义与几何信息，导致对远处和小物体细节的建模不足，零样本泛化能力有限。
- **核心动机**：扩散模型（如 Stable Diffusion）具备强大的语义提取与几何特征学习能力，将其引入自监督深度估计，可在无标签条件下增强特征表示，提升零样本泛化能力。

## 2. 方法论：核心思想、关键技术细节

### 2.1 总体框架（DiffSQL）

DiffSQL 由三部分构成（见图 2）：

- **(1) DepthNet**：混合卷积-扩散特征编码器，处理输入帧，产生多尺度视觉嵌入，并通过自注意力机制融合扩散模型的粗粒度输出，最终生成深度图。
- **(2) PoseNet**：传统位姿估计子网络，计算相邻帧之间的变换矩阵 T，仅在训练时用于视图合成（可微形变）。
- **(3) 可微图像形变**：基于深度图 D 和相对位姿 T，将参考帧像素映射到当前帧以计算重投影损失。

### 2.2 扩散编码器（Diffusion Encoder for Feature Augmentation）

- **动机**：作者通过 k-means（k=8）对比 ResNet50 与扩散模型提取的特征图（图 4），发现 ResNet50 在远处和小物体上丢失大量结构信息，而扩散模型能更有效地学习几何结构。
- **具体做法**：
  - 输入图像经 VAE 编码器下采样至 (h/8, w/8)，ResNet 模块保持该分辨率处理，自注意力模块在 (h×w/64) 分辨率处理后再 resize 至 (h/8, w/8)。
  - 设计特征融合模块：对局部特征（ResNet）与全局特征（Self-Attention）分别用 1×1 卷积统一通道，通道拼接后再用 1×1 卷积降低维度；该融合在 encoder、latent、decoder 三个阶段均执行，并带跳连。
  - 在第 4 次下采样阶段，将 SD 特征与 U-Net-CNN 主干特征沿通道维度拼接，进一步丰富空间几何细节。

### 2.3 动态自查询层（Dynamic Self Query Layer）

- **核心思想**：替代 ViT 中的自注意力向量，改用预训练 Stable Diffusion 提取的特征作为粗粒度查询对象（因其全局语义更强）；并引入动态查询选择机制，使查询自适应于输入特征分布。
- **数学流程**：
  - 全局上下文向量：`g = (1/L) Σ H_i,j`
  - 调整向量：`g_adjusted = f_adjust(g)`（轻量网络，如 1×1 卷积）
  - 动态查询：`Q_dynamic = Q_base + g_adjusted`
  - 注意力打分：`A_i,j = Q_dynamic,i ⊤ · H_i,j`
  - Softmax 归一化后加权求和得到输出查询向量
  - 构建自代价体积 V：`V_i,j,k = Q_dynamic,i ⊤ · S_j,k`
  - 深度分布计算：通过 MLP 聚合 softmax(V) 加权特征，生成深度分布向量 b
  - 最终深度：对 D-plane 体积做 plane-wise softmax 得到概率图 p，再按深度 bin 中心加权求和：`d̃ = Σ c(b_i)·p_i`
- **复杂度优势**：通过粗粒度查询对象，避免了 O(h²×w²) 的高复杂度自代价体积计算。

### 2.4 损失函数

- **光度误差**：`pe(I_a, I_b) = α·(1-SSIM)/2 + (1-α)·|I_a - I_b|₁`
- **边缘感知平滑损失**：`L_s = |∂x d*|·e^{-|∂x I|} + |∂y d*|·e^{-|∂y I|}`
- **自动掩码**：μ 过滤静止像素和低纹理区域，基于时间光度差异
- **总损失**：`L = μ·L_photo + λ·L_s`

## 3. 实验设计：数据集、Benchmark 与对比方法

### 3.1 数据集

- **KITTI**（训练与主测试）：
  - 61 个驾驶场景，图像分辨率 1242×375
  - 训练：39,810 个单目三元组；验证：4,424 帧
  - 测试：697 帧原始 LiDAR 数据 + 652 帧改进真值（[Uhrig et al., 2017]）
  - 采用 Eigen benchmark 划分，评估指标包括 AbsRel、SqRel、RMSE、RMSElog、δ<1.25/1.25²/1.25³
- **Make3D**（零样本泛化测试）：
  - 使用 KITTI 预训练权重直接测试，中心裁剪 2:1 长宽比
  - 指标：AbsRel、SqRel、RMSE、log10

### 3.2 对比方法

- **自监督方法**：Monodepth2、SQLdepth、PackNet-SfM、HR-Depth、ManyDepth、MonoDiffusion、CADepthNet、AQUANet、Dynamic Depth 等
- **监督方法**（仅 Make3D 表格）：Monodepth、Zhou (M)、DDVO

### 3.3 主要实验结果

- **KITTI 标准测试**（表 1）：DiffSQL（单目训练）AbsRel=0.096，优于 SQLdepth（0.097）；SqRel=0.698（SQLdepth 为 0.718），相对提升 2.79%；MS 训练时 AbsRel=0.094，全面优于对比方法。
- **KITTI 改进真值**（表 2）：AbsRel=0.067（单目）、0.065（MS），均优于 SQLdepth。
- **Make3D 零样本**（表 3）：AbsRel=0.310，优于 SQLdepth（0.314），SqRel 从 3.374 降至 3.013，RMSE 从 7.285 降至 7.019。

## 4. 资源与算力

- **论文中未明确说明**所使用的 GPU 型号、数量、训练时长等计算资源信息。
- 仅在方法描述中提及设计了降维策略以"减少计算开销"，但未给出具体 FLOPs、参数量或推理时间的定量分析。

## 5. 实验数量与充分性

### 实验组数（总计约 6 组核心实验）

1. **KITTI Eigen benchmark 主实验**（表 1）：9 种方法对比，单目 + MS 两种训练模式
2. **KITTI 改进真值实验**（表 2）：9 种方法对比，两种训练模式
3. **Make3D 零样本实验**（表 3）：6 种方法对比
4. **消融实验 1**（表 4）：特征融合模块（ResNet18/50 ± SD）
5. **消融实验 2**（表 5）：SD 模型不同层（Mid Block、Up Block(1)、Up Block(0)）的影响
6. **消融实验 3**（表 6）：动态查询 vs 固定查询 vs 无查询

### 充分性评估

- **优点**：覆盖了标准 benchmark、改进真值、跨数据集零样本测试以及 3 组关键消融，实验设计较为完整；对比方法覆盖面广，包含多种主流自监督方法。
- **不足**：
  - 消融实验中每个设置仅报告单一数值，未见多次运行的标准差或统计显著性检验；
  - 缺少与近年来其他生成式/扩散模型深度估计方法的直接对比（如与更多基于扩散的 MonoDiffusion 之外的方法比较）；
  - 未报告推理速度、参数量、显存占用等效率指标，无法全面评估实际部署价值；
  - 零样本仅测试了 Make3D 一个数据集，场景多样性有限（均为道路/室外场景）。

## 6. 主要结论与发现

- DiffSQL 在 KITTI 标准基准上 **AbsRel 较 SQLdepth 相对提升 1.03%，SqRel 相对提升 2.79%**，且在所有指标上全面优于 SQLdepth。
- 扩散模型（Stable Diffusion）提取的**全局语义特征能有效增强 CNN 对远处和细小物体的几何结构感知**。
- **动态查询机制**比固定查询更能适应输入特征分布，显著提升深度图精度。
- **Up Block (0) 层的特征最适合作为粗粒度查询对象**，优于 Mid Block 和 Up Block (1)。
- 在 Make3D 零样本测试中，DiffSQL 表现出**更强的跨域泛化能力**，生成的深度图更清晰、细节更准确（图 7）。

## 7. 优点

- **方法创新性**：
  - 首次将 Stable Diffusion 的生成先验与自监督深度估计相结合，提出 plug-and-play 的扩散增强特征融合模块；
  - 创新性地将 SD 的注意力特征替代 ViT 特征作为粗粒度查询对象，并引入动态查询选择机制，突破固定查询数量的限制。
- **实验说服力**：通过 k-means 特征可视化（图 4）直观展示了扩散模型相比 ResNet 在几何特征上的优势，为方法动机提供实证支撑。
- **性能优异性**：在 KITTI 上超越当前最优的 SQLdepth，且零样本泛化表现突出。
- **模块化设计**：扩散增强模块为 plug-and-play 设计，可方便地适配到其他自监督框架中。

## 8. 不足与局限

- **算力与效率不透明**：未报告训练/推理时间、GPU 资源、模型参数量，而引入 Stable Diffusion 编码器可能带来显著的计算开销，实际部署可行性存疑。
- **零样本评估场景单一**：仅在 Make3D 上验证跨域泛化，缺乏对室内场景（如 NYUv2）、不同天气/光照条件等更广泛分布的测试。
- **消融实验深度有限**：未深入分析融合位置、通道数、查询数量 K 等超参数的影响；未报告实验结果的可重复性（如随机种子、方差）。
- **对比公平性**：部分对比方法（如 Dynamic Depth）使用不同输入帧数（-1,0），测试分辨率与输入条件不完全一致，可能存在轻微不公平比较。
- **理论分析不足**：未解释为什么扩散模型的 Up Block (0) 层特征最优的深层原因，缺乏对注意力机制的可解释性分析。
- **未探讨失败案例**：未分析模型在极端场景（如强烈反光、运动模糊、无纹理区域）下的失效模式。

（完）
