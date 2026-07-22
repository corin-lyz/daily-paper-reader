---
title: Edge Consistency for 4D Gaussian Splatting in Dynamic Scene Rendering
title_zh: 动态场景渲染中4D高斯溅射的边缘一致性
authors: "Boya Shi, Thomas N Guan, Yi Xiaodong"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37847/41809"
tags: ["query:depth-refine"]
score: 7.0
evidence: 通过边缘一致性正则化从单目输入实现动态场景中清晰边界的渲染
tldr: 动态场景渲染中尖锐边缘保持和时序一致性是难点。本文提出Edge4DGS，利用高斯原语与凸包结合的混合几何表示增强边界建模，并引入基于光流的边缘一致性正则化，使高斯分布对齐物体真实轮廓。从稀疏单目输入即可实时渲染出具有清晰边缘和时序连贯的动态场景，可用于深度图边缘精修和高质量渲染。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37847/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 723, \"height\": 481, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37847/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1838, \"height\": 479, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37847/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1821, \"height\": 2196, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37847/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 883, \"height\": 477, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37847/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1825, \"height\": 656, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37847/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 350, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37847/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 343, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37847/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 871, \"height\": 375, \"label\": \"Table\"}]"
motivation: 现有动态场景渲染方法难以保持锐利边缘和时间一致性。
method: 提出混合几何表示（高斯原语+凸包）和边缘一致性正则化，结合光流引导分布对齐。
result: 从稀疏单目输入实现实时渲染，保持清晰边缘和时序连贯。
conclusion: 为动态场景边缘精修和高质量渲染提供了有效技术。
---

## Abstract
Existing dynamic scene rendering methods often struggle to preserve sharp edges and maintain temporal consistency. To address these challenges, we introduce Edge 4D Gaussian Splatting (Edge4DGS), a real-time rendering framework that renders fine-grained geometry from sparse monocular inputs in dynamic scenes. Edge4DGS proposes a hybrid geometric representation that augments Gaussian primitives with convex hulls, enabling accurate modeling of hard surfaces and complex boundaries. To enhance spatial precision, we introduce edge consistency regularization leveraging optical flow, guiding Gaussian distributions to align with true object contours. To enforce temporal coherence, we extend the regularization from discrete time steps to continuous unit intervals, enabling accurate motion modeling and reducing flickering artifacts. A two-stage coarse-to-fine optimization further improves geometric fidelity while preserving computational efficiency. Extensive experiments on monocular and multi-view motion datasets demonstrate that Edge4DGS achieves real-time, high-resolution rendering and consistently surpasses state-of-the-art methods, reducing LPIPS by 56.25%.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：从稀疏单目输入渲染动态3D场景时，现有方法（如NeRF系列和3D Gaussian Splatting扩展）难以同时保持锐利边缘与时间一致性。NeRF因密集光线采样无法实时渲染；3DGS虽可实时，但高斯原语固有平滑性导致边缘模糊，且动态场景下时序漂移产生闪烁 artifact。
- **意义**：在电影级VFX、AR/VR等对视觉质量敏感的应用中，边缘细节与时间稳定性至关重要。Edge4DGS旨在从稀疏输入实现实时、高保真、边缘清晰的动态场景渲染。

### 2. 方法论：核心思想、关键技术细节、公式/算法流程
- **核心思想**：提出**混合几何表示**（Gaussian primitives + 可微分凸包 convex hulls）以精确建模硬表面和复杂边界；引入**边缘一致性正则化**（基于光流）将高斯分布对齐至真实物体轮廓；将该正则化从离散时间步扩展到**连续单位时间间隔**以增强时间连贯性；采用**两阶段粗到细优化**先拟合全局结构再精细边缘。
- **关键技术细节**：
  - **混合表示**：每个凸包由一组支撑半平面（half-planes）定义，通过平滑最大函数（log-sum-exp）实现可微分的内部-外部指示，支持梯度传播。
  - **变形场**：使用MLP预测每个高斯原语在4D空间中的位置、旋转、尺度变化，保持物理一致性。
  - **边缘一致性正则化**：对于像素 i，在时间间隔 δt 内，惩罚光流变化：\( L_{4D}^{Edge} = \sum_{\delta t} \sum_i \|flow_i(t+\delta t) - flow_i(t)\|^2 \)。
  - **损失函数**：\( L_{Edge4DGS} = L_{4D}^{Edge} + \lambda_1 L_1 + \lambda_2 L_{ssim} + \lambda_3 L_{tv} \)（\(L_{tv}\)为网格正则化）。
  - **两阶段优化**：粗阶段仅优化高斯原语学习整体几何与运动；细阶段联合优化凸包与高斯，恢复精细边缘。
- **算法流程**：输入稀疏点云→通过HexPlane分解时空→特征融合→变形网络预测高斯运动→投影到2D并构建可微分凸包→可微分栅格化→计算损失反向传播。

### 3. 实验设计
- **数据集与场景**：
  - **Plenoptic Video Dataset**：真实世界动态场景，17-20个GoPro多视角视频，分辨率1352×1014，含火焰、阴影等挑战。
  - **D-NeRF Dataset**：单目动态序列，每场景50-200训练图像、20测试图像，分辨率800×800。
- **Benchmark**：与NeRF和3DGS变体对比，包括DyNeRF、StreamRF、HyperReel、NeRFPlayer、K-Planes、MixVoxels、MSTH、Deformable3DGS、RealTimEdge4DGS等。
- **对比方法**：全文列举了15种以上的SOTA方法，覆盖隐式、显式、混合方法。

### 4. 资源与算力
- 单张 **NVIDIA RTX 3090 GPU**（24GB显存），PyTorch实现。
- 训练时长：Plenoptic数据集约**72分钟**，D-NeRF数据集约**13分钟**；渲染速度约**40 FPS**（Plenoptic）、**110 FPS**（D-NeRF）。
- 所有实验在同一单卡环境下完成，未提及多卡并行。

### 5. 实验数量与充分性
- **量化实验**：两个主要数据集各给出完整指标（PSNR/SSIM/LPIPS/Training Time/FPS），如表1、表2。
- **消融实验**：在D-NeRF上进行了7组消融，分别验证凸包模块、边缘正则化（无时间间隔）、带时间间隔的完整正则化、以及四种训练策略（联合/交替/渐进分辨率/两阶段），详见表3。
- **定性结果**：提供可视化对比（图3、图4、图5），展示边缘细节和光流一致性。
- **公平性**：与已有方法在相同数据集、相同评价指标下对比，并且报告了训练时间与FPS，便于公平比较效率。
- **结论**：实验覆盖了多视角与单目场景、静态/动态区域，消融全面，结果充分支持核心创新点。

### 6. 论文的主要结论与发现
- Edge4DGS在Plenoptic数据集上PSNR达35.33dB（比最佳基线高约4.5dB），LPIPS降低56.25%；在D-NeRF上PSNR达40.00dB，SSIM 0.99，均为SOTA。
- 混合几何表示显著改善边缘锐度；连续时间间隔的正则化比离散约束更有效地减少闪烁。
- 两阶段粗到细优化优于联合训练或交替训练，在保证精度的同时降低复杂度。
- 实现实时渲染（40-110 FPS），训练效率与现有高斯方法相当或更优。

### 7. 优点
- **方法创新性**：首次将可微分凸包引入4D高斯溅射，填补高斯表示无法准确建模硬边界的空白。
- **正则化设计**：基于光流的边缘一致性从时间步扩展到连续间隔，同时促进空间与时间连贯性，无需增加点云数量。
- **工程实现**：所有凸包操作在自定义CUDA内核中实现，保持端到端可微且高效。
- **实验全面**：在多视角与单目数据集上均验证，消融实验覆盖每个模块，结果统计显著且可视化清晰。
- **实用性**：稀疏输入、单GPU训练、实时渲染，适合实际应用。

### 8. 不足与局限
- **实验覆盖**：仅测试了两个基准数据集（Plenoptic Video和D-NeRF），缺乏对更大规模或更具挑战性场景（如复杂人体运动、遮挡严重、光照剧变）的评估。
- **偏差风险**：光流引导需要预先计算（论文使用RAFT），光流质量不佳时可能引入误差；正则化依赖于光流真值，而RAFT预测本身存在局限，未讨论该依赖对结果的影响。
- **应用限制**：凸包初始化使用Fibonacci球采样，对点云密度和质量较敏感；对于极其复杂的拓扑（如烟雾、半透明物体），凸包表示可能不如纯高斯或隐式表示灵活。
- **超参数敏感性**：λ1、λ2、λ3等权重根据验证集设定，文中未给出具体数值或详细敏感性分析。
- **与SOTA的公平性**：对比方法中部分结果来自原始论文，但未统一使用相同的后处理或硬件环境（如DyNeRF使用8块GPU训练），可能影响对比公平性。

（完）
