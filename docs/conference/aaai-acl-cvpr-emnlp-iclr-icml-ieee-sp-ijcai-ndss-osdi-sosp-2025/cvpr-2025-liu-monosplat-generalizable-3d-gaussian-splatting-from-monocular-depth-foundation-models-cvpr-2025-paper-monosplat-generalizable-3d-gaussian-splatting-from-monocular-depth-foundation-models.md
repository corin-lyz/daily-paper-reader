---
title: "MonoSplat: Generalizable 3D Gaussian Splatting from Monocular Depth Foundation Models"
title_zh: MonoSplat：基于单目深度基础模型的泛化三维高斯泼溅
authors: "Liu, Yifan, Fan, Keyu, Yu, Weihao, Li, Chenxin, Lu, Hao, Yuan, Yixuan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liu_MonoSplat_Generalizable_3D_Gaussian_Splatting_from_Monocular_Depth_Foundation_Models_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 4.0
evidence: 利用预训练单目深度基础模型进行三维重建
tldr: 针对泛化三维高斯泼溅在陌生场景上性能受限的问题，本文提出MonoSplat，利用预训练单目深度基础模型的丰富视觉先验，通过单目-多视角特征适配器和集成高斯预测模块，提升重建鲁棒性，实现高质量实时渲染。该工作展示了深度基础模型作为通用3D先验的潜力。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 757, \"height\": 584, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1600, \"height\": 827, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 823, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1619, \"height\": 833, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1714, \"height\": 425, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 820, \"height\": 372, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1663, \"height\": 458, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 841, \"height\": 228, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-monosplat-generalizable-3d-gaussian-splatting-from-monocular-depth-foundation-models-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 797, \"height\": 425, \"label\": \"Table\"}]"
motivation: 泛化三维高斯泼溅在陌生场景上泛化能力有限，需借助更强的单目深度先验。
method: 引入单目-多视角特征适配器与集成高斯预测模块，融合深度基础模型特征。
result: 在新场景上显著提升高斯泼溅重建质量与渲染保真度。
conclusion: 说明单目深度基础模型可作为通用3D重建的有力先验。
---

## Abstract
Recent advances in generalizable 3D Gaussian Splatting have demonstrated promising results in real-time high-fidelity rendering without per-scene optimization, yet existing approaches still struggle to handle unfamiliar visual content during inference on novel scenes due to limited generalizability. To address this challenge, we introduce MonoSplat, a novel framework that leverages rich visual priors from pre-trained monocular depth foundation models for robust Gaussian reconstruction. Our approach consists of two key components: a Mono-Multi Feature Adapter that transforms monocular features into multi-view representations, coupled with an Integrated Gaussian Prediction module that effectively fuses both feature types for precise Gaussian generation. Through the Adapter's lightweight attention mechanism, features are seamlessly aligned and aggregated across views while preserving valuable monocular priors, enabling the Prediction module to generate Gaussian primitives with accurate geometry and appearance. Through extensive experiments on diverse real-world datasets, we convincingly demonstrate that MonoSplat achieves superior reconstruction quality and generalization capability compared to existing methods while maintaining computational efficiency with minimal trainable parameters. Codes are available at \href https://github.com/CUHK-AIM-Group/MonoSplat https://github.com/CUHK-AIM-Group/MonoSplat .

---

## 论文详细总结（自动生成）

## MonoSplat 论文总结

### 1. 核心问题与整体含义（研究动机与背景）

- **背景**：三维场景重建与新视角合成是计算机视觉与图形学的核心挑战。从 SRN、NeRF 到 3D Gaussian Splatting（3DGS），场景表示方法持续演进，但传统方法普遍面临**逐场景优化耗时**和**渲染计算开销大**两大瓶颈。
- **现有方案的局限**：泛化型 3DGS 方法（如 pixelSplat、MVSplat）通过前馈网络直接预测 3D 高斯原语，免去了逐场景优化，实现了实时高质量渲染。然而，这类方法在处理**训练分布之外的陌生视觉内容和布局**时，泛化能力明显受限，原因在于其对视觉世界的理解受限于训练数据分布，且零样本域泛化能力薄弱。
- **核心问题**：如何提升泛化 3D 高斯泼溅在陌生场景上的重建质量与零样本泛化能力。
- **核心思路**：当代单目深度基础模型（如 MiDaS、Depth Anything Model）在跨域深度估计上已展现极强的视觉理解能力。本文的核心洞察是——**如果对视觉世界的深度理解是泛化重建的根本，那么利用预训练单目深度模型作为先验，应能构建广泛适用的重建框架**。据此提出 **MonoSplat**。

### 2. 方法论

#### 2.1 总体框架

MonoSplat 的输入为带相机位姿的多视角图像序列，输出为稠密 3D 高斯原语（位置 μ、不透明度 α、协方差 Σ、球谐颜色 c）。整个框架建立在**冻结的 Depth Anything Model（ViT-s 变体）** 之上，通过一个前馈网络实现从图像到高斯的直接映射。

#### 2.2 核心组件一：Mono-Multi Feature Adapter（单目-多视角特征适配器）

该模块解决的关键问题是：将单目深度模型**视图独立的单目特征**转化为**几何一致的多视角特征**。包含两个子阶段：

- **多尺度特征融合**：从冻结的深度编码器中间层提取多尺度特征，通过 **DPT 架构**渐进融合为统一特征表示 Fi，同时保留细粒度细节与全局上下文。
- **跨视角几何推理**：采用基于 Swin Transformer 的**局部窗口注意力**机制，每视角仅与其 M 个最近邻视角进行 self-attention 与 cross-attention，得到具有跨视角感知的特征 Fᵐᵛᵢ，有效建模多视角几何对应关系，同时控制计算复杂度。

此外，还从**解码器最后一层**提取单目特征 Fᵐᵒⁿᵒᵢ，该层已完成多尺度聚合与层级细化，蕴含更直接、更丰富的深度先验。

#### 2.3 核心组件二：Integrated Gaussian Prediction（集成高斯预测模块）

- **集成代价体（Integrated Cost Volume）**：采用平面扫描（plane-sweep stereo）方法，在近远边界间均匀采样 128 个深度平面，通过反投影与重投影将邻域视角特征对齐到参考视角，计算点积相关性构建初始代价体。**关键创新**在于将单目特征 Fᵐᵒⁿᵒᵢ 与多视角特征 Fᵐᵛᵢ 同时注入代价体，经 Fcost 网络细化得到集成代价体 Ĉᵢ。该集成策略在遮挡、无纹理、镜面等高难度区域中，为多视角匹配失效的情况提供了强几何先验补充。
- **高斯参数预测**：对细化后的代价体做 softmax 得到深度概率分布，加权求和得到深度图。随后通过双线性插值将深度图、单目特征与多视角特征上采样到原图分辨率，经特征细化网络 Ffeature 融合，最后分别通过 Fdepth 预测高斯位置和不透明度、Fraw 预测协方差和颜色，将所有视角的高斯原语合并为统一表示。

#### 2.4 优化目标

采用与 pixelSplat、MVSplat 一致的损失函数：`L = Lmse + 0.05 · Llpips`，即像素级 MSE 损失与感知 LPIPS 损失的加权组合，通过可微渲染管线进行端到端训练。

### 3. 实验设计

- **数据集**（三个真实世界数据集）：
  - **RealEstate10K**：YouTube 房产漫游视频室内场景（65,477 训练 / 7,289 测试序列）。
  - **ACID**：航拍自然景观数据（11,075 训练 / 1,972 测试序列）。
  - **DTU**：受控条件下的物体中心捕捉数据（16 个场景 × 4 视角），用于跨域泛化验证。
- **评价指标**：PSNR（像素精度）、SSIM（结构相似性）、LPIPS（感知相似度），以及推理速度、参数量、显存占用。统一在 256×256 分辨率下评测。
- **对比方法**（8 个基线，涵盖三大类）：
  - 光场网络类：GPNR、AttnRend（Du et al.）。
  - NeRF 类：pixelNeRF、MuRF。
  - 泛化 3DGS 类：pixelSplat、latentSplat、MVSplat、eFreeSplat。
- **实验组别**共 3 大类：
  1. **主实验**：在 RealEstate10K 和 ACID 上分别训练并测试，对比所有基线。
  2. **跨数据集泛化实验**：仅用 RealEstate10K 训练，零样本测试 ACID 与 DTU。
  3. **消融实验**：设计 6 个变体，分别验证冻结骨干、DPT、跨视角聚合、单目特征注入（整体/仅代价体/仅细化网络）的贡献。

### 4. 资源与算力

- 论文明确提到：训练使用 **单张 A100 GPU**，训练 **300,000 次迭代**，batch size 为 14。
- 推理时编码耗时 0.051 秒，显存占用 0.857 GB，模型参数量 30.3M（其中可训练参数仅 10.3M）。
- 消融实验训练 20,000 次迭代（论文正文的交互检查显示为 200k 迭代，原文表注存在矛盾）。
- **说明**：论文未明确说明训练总时长（小时数）以及完整训练所需的具体 GPU 数量。

### 5. 实验数量与充分性

- **实验数量**：共 3 组主要定量实验（2 个数据集主实验 + 跨域泛化 + 6 组消融），并配有 4 组定性可视化（新视角渲染对比、跨域泛化可视化、3D 高斯几何重建对比、消融误差分布图）。实验组数适中，但覆盖较为系统。
- **充分性分析**：
  - **优点**：跨域零样本实验设计严谨，覆盖了从室内到室外、从场景级到物体级的域间迁移；消融实验对每个关键设计决策均有验证，且包含定性误差可视化；3D 高斯几何重建可视化补充了纯 2D 指标之外的评估维度。
  - **不足**：
    - 与同为利用深度先验的并发工作 DepthSplat 未做直接对比，降低了对比的完整性。
    - 部分消融变体（如 “MF in cost volume” 与 “MF in refinement”）在 Re10k→DTU 上的 PSNR 差异较大（15.06 vs 14.93），但论文未解释原因，讨论深度不足。
    - 缺少不同输入视角数量（如 2、4、8 视角）对性能影响的敏感性分析。
    - 没有对单目特征注入引入的额外计算开销做细致的效率分解。

### 6. 主要结论与发现

- **性能领先**：MonoSplat 在 RealEstate10K（PSNR 26.68）和 ACID（PSNR 28.63）上的 PSNR、SSIM、LPIPS 三项指标均全面超过所有基线方法。
- **泛化能力强**：在 Re10k→DTU 跨域实验中，PSNR 较 MVSplat 提升 1.31dB，SSIM 提升 0.132，LPIPS 改善 0.094，域间差距越大优势越明显。
- **几何重建质量高**：在仅有光度监督的条件下，MonoSplat 生成的 3D 高斯原语显著优于 pixelSplat（存在漂浮高斯伪影）和 MVSplat（深度估计在复杂几何区域精度不足）。
- **效率优势显著**：总参数 30.3M 但可训练参数仅 10.3M，得益于冻结深度基础模型；推理显存低于 MVSplat 等对比方法。
- **消融关键结论**：
  - 保持深度基础模型**冻结**至关重要（微调导致跨域 PSNR 下降 2.61dB）。
  - DPT 多尺度融合和跨视角聚合均不可或缺。
  - 单目特征的双注入（代价体 + 细化网络）共同作用才能最大化泛化收益。

### 7. 优点

- **方法设计优雅高效**：直接复用冻结的深度基础模型，在几乎不增加可训练参数（仅 10.3M）的情况下获得大幅性能提升，体现了“基础模型优先”的设计理念，具有较高的工程实用价值。
- **视角独到**：与以往方法仅专注多视角几何推理不同，MonoSplat 揭示了**单目深度先验**在多视角重建中的独特价值——特别是在多视角匹配易失败的极近距、大视角变化、光照剧变等场景中，单目先验提供了稳健的兜底能力。
- **跨域泛化验证充分**：使用 Re10k→DTU 和 Re10k→ACID 两组零样本实验，且定性展示了模型在深度估计和 3D 高斯质量上的优势，证据链完整。
- **与单目深度模型的创新结合**：论文识别到深度基础模型解码器特征（而非编码器特征）更适合作重建先验，这是一个实用且有洞察力的工程设计决策。

### 8. 不足与局限

- **参数量与推理效率权衡**：尽管可训练参数少，但总参数量（30.3M）仍大于 MVSplat（12.0M），推理时间（0.051s）也略慢于 MVSplat（0.044s），在严格实时部署场景中可能存在瓶颈。
- **实验覆盖有限**：仅验证了三种数据集类型（室内场景级、航拍场景级、物体级），未覆盖大规模室外街景、动态场景、人像重建等更多实际应用面。
- **单目深度先验的边界**：当输入图像本身包含显著单目深度歧义（如透明物体、镜面反射、重复纹理）时，单目先验可能提供误导性信息，论文对此类失败场景未做分析。
- **对比基线时效性**：与 DeepSplat 等同步工作的对比缺失，且部分基线（如 GPNR、pixelNeRF）相对陈旧，削弱了 SOTA 对比的说服力。
- **应用限制**：方法依赖深度基础模型的特征提取能力，若深度模型本身对特定域（如医学影像、工业检测）不敏感，MonoSplat 的性能也会随之受限。

（完）
