---
title: "Video Depth Anything: Consistent Depth Estimation for Super-Long Videos"
title_zh: Video Depth Anything：超长视频的一致深度估计
authors: "Chen, Sili, Guo, Hengkai, Zhu, Shengnan, Zhang, Feihu, Huang, Zilong, Feng, Jiashi, Kang, Bingyi"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Chen_Video_Depth_Anything_Consistent_Depth_Estimation_for_Super-Long_Videos_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 基于Depth Anything V2改进时空头实现视频深度一致性
tldr: 针对Depth Anything在视频中时间不一致的问题，本文提出Video Depth Anything，在Depth Anything V2基础上替换为高效时空头部，实现数分钟超长视频的高质量一致深度估计，同时兼顾计算效率。该工作显著拓展了深度基础模型在视频任务中的实用性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1801, \"height\": 699, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1757, \"height\": 430, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 838, \"height\": 508, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 833, \"height\": 279, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 873, \"height\": 312, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1685, \"height\": 1025, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 839, \"height\": 500, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1627, \"height\": 362, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1629, \"height\": 252, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 715, \"height\": 405, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 783, \"height\": 308, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 924, \"height\": 365, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chen-video-depth-anything-consistent-depth-estimation-for-super-long-videos-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 786, \"height\": 188, \"label\": \"Table\"}]"
motivation: 单目深度基础模型在视频上存在时间抖动，现有方法仅适用于短时长视频且效率受限。
method: 基于Depth Anything V2设计高效时空变换头，在保持效率的同时提升时间一致性。
result: 在超长视频上实现高质量一致深度估计，并优于现有视频深度方法。
conclusion: 为深度基础模型的视频应用提供了简单高效的扩展方案。
---

## Abstract
Depth Anything has achieved remarkable success in monocular depth estimation with strong generalization ability. However, it suffers from temporal inconsistency in videos, hindering its practical applications. Various methods have been proposed to alleviate this issue by leveraging video generation models or introducing priors from optical flow and camera poses. Nonetheless, these methods are only applicable to short videos (10 seconds) and require a trade-off between quality and computational efficiency. We propose Video Depth Anything for high-quality, consistent depth estimation in super-long videos (over several minutes) without sacrificing efficiency. We base our model on Depth Anything V2 and replace its head with an efficient spatial-temporal head. We design a straightforward yet effective temporal consistency loss by constraining the temporal depth gradient, eliminating the need for additional geometric priors. The model is trained on a joint dataset of video depth and unlabeled images, similar to Depth Anything V2. Moreover, a novel key-frame-based strategy is developed for long video inference. Experiments show that our model can be applied to arbitrarily long videos without compromising quality, consistency, or generalization ability. Comprehensive evaluations on multiple video benchmarks demonstrate that our approach sets a new state-of-the-art in zero-shot video depth estimation. We offer models of different scales to support a range of scenarios, with our smallest model capable of real-time performance at 30 FPS.

---

## 论文详细总结（自动生成）

## 一、核心问题与研究动机

- **研究背景**：单目深度估计领域取得了显著进展，特别是以 Depth Anything / Depth Anything V2 为代表的深度基础模型，在图像深度估计上展现出强大的**泛化能力**（能在零样本条件下适应多样化场景）和**丰富细节还原能力**。然而，这类模型是为静态图像设计的，当直接应用于视频时会出现严重的**时间不一致问题**（如闪烁、运动模糊导致的深度跳动）。
- **现有方法的局限**：为解决该问题，已有工作主要分为两类：
    1. 基于**光流或相机位姿**等几何先验的优化方法（如 NVDS），其性能高度依赖光流/位姿的准确性，误差易被放大；
    2. 基于**视频扩散模型**的方法（如 ChronoDepth、DepthCrafter、DepthAnyVideo），虽然细节丰富，但计算开销大、推理慢，且受限于训练窗口长度，仅能处理**短时长视频（通常 <10 秒）**，在长视频中会产生窗口间的跳跃和闪烁。
- **核心问题**：能否设计一种模型，既**完整继承现有深度基础模型**（如 Depth Anything V2）的泛化能力和细节精度，又能在**任意长度的视频**中实现时间上一致的深度估计，同时保持**计算高效**？
- **本文目标**：基于以上问题，提出 Video Depth Anything（VDA），在**不引入任何几何先验或视频生成先验**的前提下，实现超长视频（数分钟）的高质量、一致深度估计，并在空间精度、时间一致性和推理效率三方面同时达到最优。

---

## 二、方法论

本文方法建立在 Depth Anything V2 之上，由以下三个关键组件构成：

### 1. 网络架构：时空头（Spatial-Temporal Head, STH）

- 保持 Depth Anything V2 的 **编码器（Encoder）冻结不动**，以避免有限的视频数据破坏已学好的表征。
- 用一个新的**时空头（STH）** 替换原本的 DPT 头，结构上保留 DPT 的主要框架，但插入若干个**时间注意力层（Temporal Layer）**。
- 每个时间注意力层包含多头自注意力（SA）和前馈网络（FFN），**只沿时间维度**计算自注意力，使不同帧的同一空间位置能进行信息交互；同时使用**绝对位置编码**来编码帧间的时间顺序关系。
- 输入时，将视频帧的时间维与 batch 维合并，使图像编码器可以同时处理图片和视频数据；视频数据中 N 为帧数，单张图片时 N=1。
- 仅在**低分辨率特征层**插入时间层，以控制额外计算开销。

### 2. 损失函数：时间梯度匹配损失（Temporal Gradient Matching Loss, TGM）

- 先分析现有方法的不足：光流扭曲损失（OPW）假设相邻帧对应点的深度不变，但这在相机移动时会失效（汽车向前行驶时前方物体距离显然变近）。
- 提出的稳定误差（SE）损失：假设相邻帧对应点的深度**变化量**应与真值的变化量一致，而非深度本身不变：
  - \(L_{SE} = \frac{1}{N-1}\sum_{i=1}^{N-1} \left\| |\hat{d}_i - d_i| - |\hat{g}_i - g_i| \right\|_1\)，但该损失仍需光流。
- 进一步简化，**放弃光流匹配**，直接对同一图像坐标处的相邻帧深度值做差，比较预测帧间差与真值帧间差的一致性，即 TGM 损失：
  - \(L_{TGM} = \frac{1}{N-1}\sum_{i=1}^{N-1} \left\| |d_{i+1} - d_i| - |g_{i+1} - g_i| \right\|_1\)
  - 为避免边缘突变和动态物体干扰，仅在 \(|g_{i+1} - g_i| < 0.05\) 的区域计算该损失。
- 总损失为：\(L_{all} = \alpha L_{TGM} + \beta L_{ssi}\)，其中 \(L_{ssi}\) 是 MiDaS 提出的尺度与平移不变损失，负责约束单帧空间结构。

### 3. 长视频推理策略：关键帧引用 + 重叠插值

- **问题**：模型训练窗口有限（N=32），直接拼接不同窗口会产生跳变；仅用重叠区域做仿射对齐则会产生累积尺度漂移。
- **关键帧引用（Key-frame Referencing）**：每个新的推理片段由未来帧、与前一片段重叠的帧（\(T_o=8\)）、以及从前一片段中**按间隔 \(\Delta_k=12\) 子采样**出的关键帧（\(T_k=2\)）拼接成 32 帧。这使得当前窗口能携带更早的尺度/平移信息，抑制尺度漂移并保证全局一致性。
- **深度片段缝合（Depth Clip Stitching）**：对重叠的 \(T_o\) 帧，在前后两个窗口的预测之间做**线性插值**过渡（权重从 1 线性衰减到 0），保证窗口拼接处的深度平滑连续。

---

## 三、实验设计

### 1. 视频深度估计评估

- **数据集**：使用 5 个覆盖面广的基准数据集：
  - 室内：ScanNet、NYUv2、Bonn
  - 室外：KITTI
  - 野外/合成：Sintel（约 50 帧/段）
- **评估协议**：每个视频最多评估 500 帧（远超 DepthCrafter 的 110 帧），并在附录中提供 110 帧的对比结果。
- **指标**：
  - 空间精度：AbsRel（越低越好）、δ1（越高越好）
  - 时间一致性：TAE（Temporal Alignment Error，重投影误差，越低越好）
- **对比方法**：
  - 单图方法：Depth Anything V2（DAv2-L）、NVDS + DAv2-L（将 NVDS 基础模型替换为 DAv2）
  - 视频方法：NVDS、ChronoDepth、DepthCrafter、DepthAnyVideo（注意：DepthAnyVideo 最大支持 192 帧，因此仅在 Sintel 上报告指标）

### 2. 单图像深度估计评估

- **数据集**：KITTI、Sintel、NYUv2、ETH3D、DIODE（与 Depth Anything V2 一致）。
- **对比方法**：DepthCrafter、DepthAnyVideo、DAv2-L。

### 3. 长视频定量评估

- 从 Bonn、ScanNet 中选 10 个场景、NYUv2 中选 8 个场景，每段 500 帧。
- 在帧数长度为 110、192、300、400、500 下分别评测，对比 DepthCrafter（110 帧窗口）和 DepthAnyVideo（192 帧窗口）。

### 4. 推理延迟对比

- 在单张 A100 GPU，分辨率 518×518 下测量各模型平均单帧推理时间。

### 5. 消融实验

- **时间损失函数**（TartarAir 和 VKITTI）：比较 VideoAlign、VideoAlign+SSI、OPW+SSI、SE+SSI、TGM+SSI。
- **推理策略**：Baseline（无重叠）、OA（重叠+尺度平移对齐）、OI（重叠+插值）、OI+KR（重叠插值+关键帧引用）。
- **窗口大小**：16、32、48。
- **训练策略**：仅视频数据集 vs 视频+图像数据集联合蒸馏。

---

## 四、资源与算力

- 文中**未明确说明训练所用的 GPU 型号、数量及训练时长**。
- 仅在推理延迟部分提到使用 **NVIDIA A100 GPU**（单卡，分辨率 518×518）。
- 训练数据规模提及如下：
  - 视频数据：730K 帧（带标注）
  - 图像数据：0.62M 张（无标签，用于自蒸馏）
  - 对比方案 DepthCrafter 训练使用超过 10M 帧，DepthAnyVideo 使用 6M 帧，本文所用数据量明显更少。

---

## 五、实验数量与充分性

- **实验组数**：论文报告了相当充分的实验：
  - 5 个视频基准数据集上的零样本评估；
  - 5 个图像基准上的零样本评估；
  - 3 个数据集上、5 种不同帧长度的长视频对比；
  - 推理延迟对比（含 6 个模型）；
  - 4 组消融实验（损失函数、推理策略、窗口大小、训练策略），每组内部还有多个变体。
- **充分性与客观性分析**：
  - **优点**：评估数据集覆盖室内/室外/合成场景，空间和时间指标并重；长视频评估达到 500 帧/段，明显优于此前工作的评估规模；除定量指标外还提供了空间-时间剖面图（temporal profile）等定性证据，结论较为扎实。
  - **潜在不足**：
    - 500 帧（约 20 秒）的评估长度对于“super-long videos”（数分钟）这一宣称而言仍偏短，文中虽在示例中给出 196 秒定性展示，但定量指标缺乏分钟级视频的支持；
    - 消融实验未采用最终模型（使用 VDA-S、窗口 16、且未加图像蒸馏），消融结论与最终设置之间的一致性需谨慎对待；
    - 对比方法（尤以扩散类方法）的公平性受限于其本身窗口长度限制，不同模型的可比场景范围并不完全对等。

---

## 六、主要结论与发现

1. **时间一致性和空间精度可以兼得**：Video Depth Anything 在 5 个视频基准中的 4 个取得空间精度 SOTA，并在所有数据集上取得时间一致性最优。
2. **超长视频适用性**：相比现有方法（最大仅能处理 110~192 帧），本文模型可处理数分钟长的视频，且指标随帧数增加几乎不衰减。
3. **计算效率突出**：VDA-L 在 A100 上平均单帧仅 67ms（约为 DAv2-L 的 1.1 倍），远超扩散类方法（DepthCrafter 910ms、DepthAnyVideo 159ms）；VDA-S 仅 9.1ms，可达 30 FPS 以上实时运行。
4. **图像能力几乎无损**：在单图像零样本评估中，VDA-L 与 DAv2-L 性能持平（仅 DIODE 上 δ1 下降 0.002），证明视频训练的加入未破坏原有图像泛化能力。
5. **关键帧引用策略有效抑制尺度漂移**：在 7320 帧的自采视频上，OI+KR 相比 OA 显著保持了全局尺度一致性。

---

## 七、方法亮点与设计优点

- **极简而有效的时间一致性约束**：TGM 损失不需要任何光流、相机位姿或视频生成先验，从根本上避免了外部几何估计带来的误差累积；通过阈值过滤动态物体和深度边缘，进一步提升了训练的稳定性。其思路优雅、实现成本极低。
- **轻量级时空建模**：只在 DPT 头的低分辨率特征层插入时间注意力，且冻结编码器，使额外计算开销极低（VDA-L 相比 DAv2-L 仅慢 ~7ms），同时避免了数据不足时破坏预训练表征的风险。
- **联合训练策略保留泛化能力**：视频数据与 0.62M 无标签图像联合训练，利用教师模型生成伪标签进行自蒸馏，在提升视频一致性的同时保持了 DAv2 的图像零样本能力。
- **实用的长视频推理方案**：关键帧引用+重叠插值思路简单、可解释性强，相比全局对齐方式避免了累积漂移，且不需要修改模型本身。
- **多尺度模型支持**：提供 ViT-Small 和 ViT-Large 两种规模，兼顾实时应用与高质量需求。

---

## 八、不足与局限

1. **长视频定量评估仍有限**：虽然定性结果展示了 196 秒甚至更长的视频，但定量指标仅覆盖最多 500 帧（约 20 秒），与“super-long videos”的承诺存在差距；对于更长时间的累积漂移和内存性能没有系统评估。
2. **训练数据域受限**：在 Sintel（合成电影风格）数据集上精度不及 DepthCrafter，作者归因于训练集中缺少类似焦距的电影域数据，说明模型对域差异敏感。
3. **仅支持相对深度（affine-invariant）**：延续了 Depth Anything V2 的设置，模型不输出度量深度，这在某些下游任务（如机器人导航、AR 遮挡）中需要额外的尺度恢复步骤。
4. **对动态场景的局限**：TGM 损失通过阈值忽略深度突变区域来规避动态物体问题，但这是缓解策略而非彻底解决方案，快速运动物体或遮挡切换场景下的深度一致性仍有待验证。
5. **消融实验与最终模型不一致**：消融实验使用的是 VDA-S、窗口 16、无图像蒸馏的配置，而最终模型为 VDA-L（或带蒸馏的 VDA-S）、窗口 32；因此消融结论在量化上不能完全外推到最终模型。
6. **缺失训练算力报告**：未披露 GPU 型号、数量、训练时长、能耗等信息，影响复现投入的估计。
7. **定性评估存在主观性**：图 5 中的“红框更接近 GT”等定性判断具有一定主观选择偏差，缺乏用户研究或多者独立评估。

---

（完）
