---
title: "MonSter: Marry Monodepth to Stereo Unleashes Power"
title_zh: MonSter：融合单目深度与立体匹配以释放深度估计潜力
authors: "Cheng, Junda, Liu, Longliang, Xu, Gangwei, Wang, Xianqi, Zhang, Zhaoxing, Deng, Yong, Zang, Jinliang, Chen, Yurui, Cai, Zhipeng, Yang, Xin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Cheng_MonSter_Marry_Monodepth_to_Stereo_Unleashes_Power_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 联合单目深度先验与立体匹配，迭代提升两者性能
tldr: 针对立体匹配在遮挡和无纹理区域难以获取可靠匹配的问题，提出MonSter双分支架构，将单目深度估计与立体匹配相互耦合。该方法通过置信度引导选取可靠立体线索估计单目深度尺度平移，并用显式单目深度先验增强立体匹配的困难区域。实验表明，迭代式互增强使单目先验从粗粒度目标结构细化到像素级几何，显著提升整体深度精度。该工作展示了单目与立体信息互补的巨大潜力。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1716, \"height\": 595, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 827, \"height\": 537, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 831, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1624, \"height\": 556, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1510, \"height\": 809, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 828, \"height\": 752, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1580, \"height\": 147, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1739, \"height\": 674, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 860, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 246, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 864, \"height\": 594, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1571, \"height\": 372, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-monster-marry-monodepth-to-stereo-unleashes-power-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 875, \"height\": 301, \"label\": \"Table\"}]"
motivation: 立体匹配在遮挡与无纹理区域缺乏足够匹配线索，而单目深度估计可提供先验，但二者尚未有效融合。
method: 提出MonSter双分支架构，用置信度引导交替优化单目深度尺度恢复与立体匹配，并利用单目先验增强困难区域匹配。
result: 在公开数据集上验证了迭代互增强机制能持续细化深度先验，并显著提升遮挡与无纹理区域的深度估计精度。
conclusion: 单目深度与立体匹配的迭代互补可同时改善两者的性能，为混合深度估计提供了新范式。
---

## Abstract
Stereo matching recovers depth from image correspondences. Existing methods struggle to handle ill-posed regions with limited matching cues, such as occlusions and textureless areas. To address this, we propose MonSter, a novel method that leverages the complementary strengths of monocular depth estimation and stereo matching. MonSter integrates monocular depth and stereo matching into a dual-branch architecture to iteratively improve each other. Confidence-based guidance adaptively selects reliable stereo cues for monodepth scale-shift recovery, and utilizes explicit monocular depth priors to enhance stereo matching at ill-posed regions. Such iterative mutual enhancement enables MonSter to evolve monodepth priors from coarse object-level structures to pixel-level geometry, fully unlocking the potential of stereo matching. As shown in Fig.2, MonSter ranks 1st across five most commonly used leaderboards --- SceneFlow, KITTI 2012, KITTI 2015, Middlebury, and ETH3D. Achieving up to 49.5% improvements over the previous best method (Bad 1.0 on ETH3D). Comprehensive analysis verifies the effectiveness of MonSter in ill-posed regions. In terms of zero-shot generalization, MonSter significantly and consistently outperforms state-of-the-art methods across the board. Code will be released upon acceptance.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

立体匹配通过图像对应关系恢复深度，在遮挡、无纹理、重复纹理、反光表面、细结构和远距离物体等不适定区域，由于缺乏足够的匹配线索，现有方法（包括基于代价滤波和基于迭代优化的方法）仍容易出现误匹配。论文指出，虽然单目深度估计不存在匹配问题，能够提供互补的结构先验，但其预测结果存在严重的尺度与平移歧义，且包含噪声，难以直接用于像素级融合。因此，论文提出 MonSter，将单目深度估计与立体匹配耦合到一个双分支架构中，通过迭代式互增强，使单目先验从粗粒度物体级结构逐步细化到像素级几何，从而有效解决立体匹配在不适定区域的困难，充分挖掘立体匹配的潜力。

## 2. 论文提出的方法论

- **核心思想**：将立体匹配分解为“单目相对深度估计”与“逐像素尺度-平移恢复”两个子问题，利用单目深度先验弥补立体匹配的不足，同时利用可靠立体线索修正单目深度的尺度歧义。
- **总体架构**：MonSter 由三个部分组成：
  1. **单目深度分支**：采用冻结的 DepthAnythingV2（ViT-large 编码器 + DPT 解码器）提取相对深度。
  2. **立体匹配分支**：基于 IGEV 构建 Geometry Encoding Volume，并沿用 ConvGRU 迭代优化；立体分支与单目分支共享冻结的 ViT 编码器，通过特征转移网络得到多尺度金字塔特征。
  3. **互细化模块**：包含两个关键子模块：
     - **全局尺度-平移对齐**：利用最小二乘法全局对齐逆单目深度与初始立体视差，将对齐后的单目视差称为“monocular disparity”。
     - **立体引导对齐（SGA）**：基于置信度（利用特征残差图）选择可靠立体线索，通过条件引导 ConvGRU 更新单目视差的逐像素残差平移。
     - **单目引导细化（MGR）**：对称地，利用对齐后的单目视差作为条件，结合立体/单目几何特征与残差图，通过独立的 ConvGRU 更新立体视差。
- **迭代融合**：经过 N1 次立体迭代得到初始视差后，再进行 N2 轮 SGA/MGR 交替细化，最终输出立体视差。
- **损失函数**：使用 L1 损失，对单目分支和立体分支的每一次迭代输出按指数权重（γ=0.9）加权监督。

## 3. 实验设计

- **数据集与基准**：
  - 训练：主要在 Scene Flow 上预训练；对于 ETH3D 和 Middlebury，按现有 SOTA 方法构建 Basic Training Set（BTS），包含 Scene Flow、CREStereo、Tartan Air、Sintel Stereo、FallingThings、InStereo2k 等。
  - 测试：五个常用基准——Scene Flow、KITTI 2012、KITTI 2015、Middlebury、ETH3D。
- **对比方法**：GwcNet、LEAStereo、ACVNet、GANet、RAFT-Stereo、CREStereo、IGEV、CroCo-Stereo、DLNR、Selective-IGEV、LoS、NMRF-Stereo、CFNet 等。
- **实验类型**：
  - 五个基准上的性能对比；
  - 不适定区域专门评估：KITTI 2012 反光区域、Scene Flow 边缘/非边缘区域、KITTI 2015 背景区域；
  - 零样本泛化实验：仅用 Scene Flow 训练，以及用 Scene Flow + CREStereo + TartanAir 混合训练，在两个设置下分别测试 KITTI、Middlebury、ETH3D；
  - 消融实验：验证 MGR、SGA、特征共享、单目深度模型替换、迭代次数对性能与运行时间的影响。

## 4. 资源与算力

论文未明确说明使用的 GPU 数量、总训练时长。仅在实现细节中提到使用 NVIDIA RTX 3090 GPU，batch size 为 8，预训练 200k 步，KITTI 微调 50k 步。此外，报告了模型参数量和推理时间：单目分支 335.3M、立体分支 12.6M、SGA/MGR 8.2M，总计 356.1M 参数；在 Scene Flow 测试集上推理时间约 0.64 秒（IGEV 为 0.37 秒）。论文未提供训练耗时与 GPU 数量的具体细节。

## 5. 实验数量与充分性

- **实验数量**：较为丰富，包括 5 个基准测试、多个不适定区域专项评估、两种训练设置下的零样本泛化测试、6 组消融实验（含模块级和模型替换级）以及不同迭代次数的对比。
- **充分性**：总体上充分。消融实验验证了每个核心模块的贡献（MGR vs 直接卷积融合、SGA 的作用、特征共享的作用），也验证了方法对不同单目模型的通用性。基准对比覆盖了经典与 SOTA 方法，结果全部来自官方 leaderboard 或对应论文，较为客观。零样本实验设计了单一数据集训练和混合数据集训练两种设置，增强说服力。
- **公平性**：对比时遵循现有方法的数据集划分和训练协议；消融实验控制了参数数量（Mono+Conv 与 MGR 参数量相当），对比较为公平。但论文未报告训练标准差的多次运行结果，也未提供显著性检验。

## 6. 论文的主要结论与发现

- MonSter 在 Scene Flow、KITTI 2012、KITTI 2015、Middlebury 和 ETH3D 五个公开基准上均排名第一，相比之前的 SOTA 最高提升 49.5%（ETH3D Bad 1.0）。
- 在反光、边缘、无纹理、远距离等不适定区域，MonSter 显著优于基线 IGEV 及专门设计处理这些区域的 Selective-IGEV 等方法。
- 在零样本泛化场景中，MonSter 表现出最佳的跨域迁移能力；当训练数据混入更多仿真数据后，性能进一步提升。
- 消融实验表明：基于置信度引导的 SGA 比直接卷积融合更能有效恢复单目深度的逐像素尺度-平移；MGR 比直接特征拼接融合更优；特征共享带来额外提升。
- 仅需 4 次迭代（N1=N2=2），MonSter 即可超过需 32 次迭代的 IGEV 的精度，体现了较好的准确率-速度平衡。
- 与不同单目深度模型（DepthAnythingV1、MiDaS）结合均有效，说明方法具有通用性。

## 7. 优点

- **方法设计理念创新**：将单目深度与立体匹配从简单拼接提升到迭代互增强，有效解决尺度歧义与不适定区域问题。
- **置信度引导机制**：SGA 通过特征残差图计算置信度，选择性使用可靠立体线索修正单目视差，避免噪声引入。
- **系统性与通用性**：可与任意主流单目深度模型集成，且共享 ViT 编码器使立体分支获得大规模预训练特征，提升泛化能力。
- **实验全面且结果突出**：覆盖五个主流基准、多种不适定区域、零样本泛化多个设置，且性能全面领先，说服力强。
- **公开代码**：提供开源实现，便于复现与后续研究。

## 8. 不足与局限

- **计算开销较大**：引入 ViT-large 单目分支使推理时间从 0.37s 增加到 0.64s，模型总参数量达 356.1M（虽然小于 CroCo-Stereo，但仍比常规立体方法重得多），可能限制在实时或移动平台上的部署。
- **未提供训练资源细节**：缺少 GPU 数量、训练时长等具体信息，难以评估训练成本。
- **实验统计稳健性欠缺**：未报告多次运行的标准差或显著性检验，结果可能受单次训练随机性影响。
- **特定场景覆盖有限**：不适定区域实验仅涉及反光、边缘、纹理缺失和远距离场景，未涵盖雨雾、低光照、透明物体等更多真实挑战。
- **依赖预训练单目模型**：性能受单目模型质量影响，当前配置依赖 DepthAnythingV2 的庞大网络；若替换为轻量模型，性能增益可能下降。
- **应用限制**：论文聚焦于双目光学图像的匹配，未讨论与雷达、ToF 等其他传感器的融合，也未明确给出针对实际产品（如无人机）的端到端速度优化方案。

（完）
