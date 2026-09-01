---
title: "Buffer Anytime: Zero-Shot Video Depth and Normal from Image Priors"
title_zh: Buffer Anytime：基于图像先验的零样本视频深度与法线估计
authors: "Kuang, Zhengfei, Zhang, Tianyuan, Zhang, Kai, Tan, Hao, Bi, Sai, Hu, Yiwei, Xu, Zexiang, Hasan, Milos, Wetzstein, Gordon, Luan, Fujun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Kuang_Buffer_Anytime_Zero-Shot_Video_Depth_and_Normal_from_Image_Priors_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 从图像先验进行零样本视频深度估计
tldr: 针对视频深度与法线标注稀缺的问题，提出零样本训练框架Buffer Anytime。它结合单图像深度/法线模型、光流平滑约束和轻量时序注意力，无需成对视频标注。在Depth Anything V2与Marigold-E2E-FT上应用后，视频时序一致性显著提升，精度保持良好，为零样本视频几何估计提供了新途径。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1752, \"height\": 1076, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 863, \"height\": 635, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 854, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1765, \"height\": 892, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1806, \"height\": 1620, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1797, \"height\": 1388, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1788, \"height\": 407, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1666, \"height\": 363, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kuang-buffer-anytime-zero-shot-video-depth-and-normal-from-image-priors-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 716, \"height\": 300, \"label\": \"Table\"}]"
motivation: 视频深度和法线数据难以获取，现有方法依赖成对视频标注数据。
method: 利用单图像深度/法线先验，结合光流平滑和轻量时序注意力架构，以零样本训练方式估计视频几何缓冲。
result: 在Depth Anything V2和Marigold-E2E-FT等模型上显著提升时序一致性且保持精度。
conclusion: 为视频几何估计提供了一种无需成对标注的零样本训练策略。
---

## Abstract
We present Buffer Anytime, a framework for estimation of depth and normal maps (which we call geometric buffers) from video that eliminates the need for paired video--depth and video--normal training data. Instead of relying on large-scale annotated video datasets, we demonstrate high-quality video buffer estimation by leveraging single-image priors with temporal consistency constraints. Our zero-shot training strategy combines state-of-the-art image estimation models based on optical flow smoothness through a hybrid loss function, implemented via a lightweight temporal attention architecture. Applied to leading image models like Depth Anything V2 and Marigold-E2E-FT, our approach significantly improves temporal consistency while maintaining accuracy. Experiments show that our method not only outperforms image-based approaches but also achieves results comparable to state-of-the-art video models trained on large-scale paired video datasets, despite using no such paired video data.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究意义（背景与动机）

- 视频级几何缓冲（geometric buffer）估计，即从单目RGB视频中估计逐帧深度图和表面法线图，是计算机视觉的长期基础问题，对具身AI、3D/4D重建与生成、自动驾驶等应用至关重要。
- 当前主流方法存在两个主要瓶颈：
  - **单图像模型**（如Depth Anything V2、Marigold-E2E-FT）逐帧独立预测，缺乏帧间一致性，导致视频预测结果在时序上不稳定、出现闪烁或漂移。
  - **视频专用模型**（如ChronoDepth、DepthCrafter）虽然时序一致性好，但严重依赖大规模成对的视频-几何标注数据，数据获取成本极高。
- 本文的核心洞察是：几何缓冲区估计任务不同于文本/图像条件生成任务——输入RGB视频与输出结果在结构上高度对齐，单图像模型在相似输入条件下更易产生一致内容。因此，**能否仅利用已有的高质量单图像先验，在不使用任何成对视频标注的情况下，将图像模型升级为时间一致、精度高、且与视频专用模型可比的视频模型？** 本文给出了肯定回答。

## 2. 方法论：核心思想与技术细节

### 2.1 核心思想

- 提出 **Buffer Anytime** 框架，一种零样本（zero-shot）训练策略——这里的“零样本”特指**不使用任何视频-几何真值配对数据**，而非传统意义上处理未见类别。
- 核心思路：将单图像深度/法线先验（来自冻结的图像基础模型）与光流驱动的时序平滑约束相结合，通过轻量时序注意力机制实现帧间一致性，同时保持逐帧精度。

### 2.2 混合损失函数

**总损失：**

\[
L = \omega_{\text{reg.}} \cdot L_{\text{depth/normal}} + L_{\text{stable}}
\]

- **正则化损失（\(L_{\text{depth}}/L_{\text{normal}}\)）**：约束视频模型的每帧输出与冻结的单图像模型预测保持一致，确保逐帧精度不退化。
  - 深度任务采用仿射不变相对损失（affine-invariant relative loss），对预测深度做中位数平移和尺度归一化后计算L2距离。
  - 法线任务直接对骨架模型的潜在表示（latent）应用L2损失。
  - 实际训练时从视频中**随机采样一帧**计算正则化损失，大幅提升训练效率。
- **光流稳定化损失（\(L_{\text{stable}}\)）**：利用预训练光流模型（GMFlow）建立相邻帧像素对应关系，最小化双向重投影误差，增强时序一致性。

\[
L_{\text{stable}} = \frac{1}{2HW}\sum_x |I_k(x) - I_{k+1}(O_{k\to k+1}(x))|_1 + \frac{1}{2HW}\sum_x |I_{k+1}(x) - I_k(O_{k+1\to k}(x))|_1
\]

### 2.3 光流掩码机制

为消除光流预测误差对训练的干扰，设计了两种滤波策略：

1. **环回校验（Cycle Validation）**：仅保留满足 \(\|O_{k\to k+1}(O_{k+1\to k}(x)) - x\|_2 \leq \tau_c\) 的像素，过滤遮挡和错误匹配。
2. **边缘掩码（Edge Mask）**：对预测深度图执行Canny边缘检测，排除边缘附近像素（曼哈顿距离≤3像素）的损失计算，避免几何边界处的光流误差导致过度惩罚。

### 2.4 模型架构

针对两种不同的图像基础模型设计了两种时序适配架构：

- **深度估计模型（基于Depth Anything V2）**：
  - 冻结ViT骨干，仅微调轻量级refinement网络。
  - 在相邻fusion层之间注入**3个时序Transformer块**（结构类似AnimateDiff，含时序注意力+投影层）。
  - ViT骨干完全脱离梯度流，降低显存开销，支持长视频序列。
- **法线估计模型（基于Marigold-E2E-FT）**：
  - 冻结扩散U-Net的空间层和自编码器。
  - 在空间层之间插入时序层，实现潜在空间时序建模。
  - 所有时序块采用**零初始化（zero-init）**，训练开始时模型行为与图像模型完全一致，保证训练稳定性。
- 训练过程中使用**延迟反向传播**（deferred back-propagation）技巧（借鉴ARF），将法线模型的潜在图划分为4帧块，逐块前向和反向传播，解决显存瓶颈。

## 3. 实验设计与基准

### 3.1 深度估计实验

- **基准**：沿用DepthCrafter提供的视频深度评估协议——对全视频求解一个全局仿射变换对齐真值，再计算指标。
- **数据集**：ScanNet（室内静态）、KITTI（室外街景）、Bonn（室内动态）。
- **指标**：AbsRel（精度）、δ1（25%阈值准确率）、OPW（光流加权时序平滑误差）。
- **对比方法**：
  - 视频方法（有视频监督）：ChronoDepth、NVDS、DepthCrafter
  - 单图像方法：Marigold、Marigold-E2E-FT、Depth Anything V2

### 3.2 法线估计实验

- **基准**：由于缺少现成视频法线基准，作者基于DSINE的图像级指标自建评估协议。
- **数据集**：Sintel（合成动态场景）、ScanNet（室内真实场景）——每场景均匀采样32帧作为测试序列。
- **指标**：Mean/Median角度误差（度）、11.25°内像素百分比、OPW时序平滑指标。
- **对比方法**：DSINE、Lotus、Marigold、Marigold-E2E-FT

### 3.3 消融实验

- 在KITTI深度估计上进行4组消融：
  1. 正则化权重 \(\omega_{\text{reg.}} = 0.1\)（降低）
  2. 正则化权重 \(\omega_{\text{reg.}} = 3\)（提高）
  3. 去除光流掩码（no mask）
  4. 使用全部帧计算正则化损失（all frames）而非单帧

## 4. 资源与算力

- **GPU**：NVIDIA H100（80GB） × **24块**
- **总batch size**：24（即每GPU batch=1）
- **训练数据**：约20万段视频，每段128帧，分辨率252×420至308×546
- **训练时长**：约**1天**（20,000次迭代）
- **序列长度**：深度110帧 / 法线32帧（受显存限制）
- **优化器**：AdamW，深度学习率1e-4、法线学习率1e-5
- 论文对算力细节披露充分，可复现性良好。

## 5. 实验数量与充分性

- **总量**：深度估计3个跨域数据集（室内静态/室外/室内动态）+ 法线估计2个数据集（合成/真实）+ 4组消融，覆盖面较完整。
- **客观性评估**：
  - 深度实验对视频监督方法和单图像方法均有对比，且注明了与DepthCrafter论文的引用指标差异（*号标注），透明度高。
  - 法线实验指标丰富，包含三种图像级指标+时序指标。
  - 消融实验验证了权重选择、掩码必要性、帧采样策略三个设计维度。
- **缺口**：
  - 法线估计缺乏专门的视频法线基准，自建协议的可推广性有待检验。
  - 消融实验未包含时序层数量、零初始化必要性、光流模型选择等架构级别分析。
  - 深度与法线实验之间缺少同一数据集的交叉验证。
  - 定性可视化展示有限，未与ChronoDepth等最新视频模型进行全面对比。

## 6. 主要结论与发现

- Buffer Anytime在**扫描网和KITTI上精度与时序平滑性全面超越Depth Anything V2基线**，AbsRel分别从0.135→0.123和0.140→0.119，OPW分别从0.121→0.076和0.089→0.038。
- 在Bonn（室内动态场景）上对单图像基线也有显著改进，且在δ1指标上超过了DepthCrafter（0.925 vs 0.971的可比区间内提升至0.925）。
- 与依赖大规模成对视频标注训练的DepthCrafter在三个数据集上结果**可比甚至部分指标更优**，且在推理速度上远快于DepthCrafter（33s vs 270s）。
- 法线估计方面，**时序一致性（OPW）显著改善**（Sintel: 0.152→0.065，ScanNet: 0.092→0.069），逐帧精度基本保持（Sintel Mean角度误差微增0.56°、ScanNet上略有提升）。
- 单帧正则化损失与全帧正则化效果几乎一致，从经验上验证了单图像先验在提供逐帧约束方面的充分性。

## 7. 亮点与优势

- **无需视频-几何配对数据**：是首个仅利用图像先验+光流约束实现与视频监督方法性能可比的视频深度/法线估计框架，大幅降低数据成本。
- **通用性强**：框架本身模型无关，可迁移到不同结构的图像基础模型（判别式ViT和扩散U-Net均适用）。
- **计算效率高**：冻结大部分骨干网络、只训练轻量时序层，训练时间仅约1天；推理速度相比视频扩散方法提升8倍以上。
- **设计精巧的损失工程**：光流掩码（环回校验+边缘掩码）有效应对光流误差这一核心难题；零初始化时序块保证训练稳定性；延迟反向传播解决长序列显存瓶颈。
- **零样本策略的概念创新**：将“零样本”引入视频几何估计，从数据监督范式转向先验+约束驱动的范式，对视频反转（video inversion）类问题有启示意义。

## 8. 不足与局限性

- **依赖图像模型先验**：当单图像主干完全失效时（极端光照、罕见物体、严重遮挡等），模型无法自我纠正。
- **光流约束的局部性**：光流平滑只覆盖相邻帧，无法捕捉跨长距离的时序关系——例如物体短暂离开画面再重新进入时，深度估计可能不一致。
- **法线实验缺少专用基准**：现有评估基于图像级指标+自定义时序指标，缺乏与视频法线专用方法的直接对比。
- **训练视频分辨率偏低**：训练数据分辨率约252×420至308×546，对高分辨率视频的泛化能力未充分验证。
- **消融范围有限**：未验证不同光流模型的影响、时序层位置和数量的最优配置、超参数τc的敏感性等。
- **部署成本**：即使骨干冻结，法线模型的扩散架构推理仍需多步去噪，相比轻量判别模型仍显笨重。
- **作者自述局限**：未来方向包括引入少量视频监督的混合训练、开发3D空间中的跨帧一致性损失，以避免光流时序约束的局部性限制。

（完）
