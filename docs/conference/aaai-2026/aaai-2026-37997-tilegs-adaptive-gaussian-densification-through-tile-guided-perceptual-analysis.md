---
title: "TileGS: Adaptive Gaussian Densification Through Tile-Guided Perceptual Analysis"
title_zh: "TileGS: 基于瓦片引导感知分析的自适应高斯加密"
authors: "Yiwen Wang, Ran Yi, Lizhuang Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37997/41959"
tags: ["query:depth-refine"]
score: 5.0
evidence: 瓦片引导的自适应高斯加密改善边界细节，与深度精修和边缘感知方法相关
tldr: 针对3D高斯散点因像素级光度损失和固定加密策略导致边界和纹理模糊的问题，提出TileGS框架。通过瓦片级感知分析指导高斯自适应加密，以局部渲染质量为导向优化场景表示。实验结果表明该方法在保持实时性的同时显著提升渲染质量，尤其是边界细节，其思想可迁移到深度图边界精修。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37997/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 825, \"height\": 595, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37997/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 450, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37997/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 845, \"height\": 359, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37997/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1840, \"height\": 883, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37997/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 807, \"height\": 567, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37997/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1828, \"height\": 458, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37997/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 864, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37997/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 883, \"height\": 336, \"label\": \"Table\"}]"
motivation: 3DGS的加密策略未考虑结构一致性和局部感知优先级，导致边界和纹理细节不足。
method: 提出瓦片级感知引导框架，根据局部渲染质量动态调整高斯分布。
result: 在多个场景下渲染质量超越现有方法，边界细节明显改善。
conclusion: 感知引导的加密策略能有效提升3DGS的表现力。
---

## Abstract
3D Gaussian Splatting (3DGS) has become a powerful technique for real-time novel view synthesis, using explicit, end-to-end optimized 3D Gaussians to represent scenes. However, its training objective is primarily based on pixel-wise photometric loss, and its densification strategy fails to account for structural consistency and localized perceptual priorities. As a result, 3DGS struggles to capture fine textures and boundary details in underconstrained areas, leading to inefficient use of representational capacity and degraded rendering quality in critical regions.
To overcome this limitation, we introduce TileGS, a tile-wise, perceptually guided framework designed to refine scene representation based on local rendering quality. Our method features a tile-guided densification approach that performs per-tile perceptual analysis between rendered and ground-truth tiles to identify areas and Gaussians requiring refinement. Additionally, we incorporate a tile-level structural loss to enforce localized consistency during training.
TileGS is designed to be a plug-and-play framework, seamlessly integrating into existing 3DGS pipelines with minimal adjustments. Experiments across multiple datasets demonstrate that TileGS improves rendering quality while maintaining an efficient representation, showcasing its versatility and effectiveness in diverse rendering scenarios.

---

## 论文详细总结（自动生成）

# 论文《TileGS: Adaptive Gaussian Densification Through Tile-Guided Perceptual Analysis》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：3D Gaussian Splatting (3DGS) 是一种基于显式、端到端优化的3D高斯体表示场景的实时新视图合成技术。然而，其训练目标主要基于逐像素光度损失（如L1+SSIM），且其自适应密度控制（densification）策略是全局的、启发式的，仅依赖累积位置梯度阈值，未考虑结构一致性和局部感知优先级。
- **核心问题**：这种策略导致：
  - 在纹理复杂、高频细节或边界区域（如碎石轨道、背景山丘），由于RGB值相近导致梯度小，难以触发加密，造成该区域高斯稀疏、渲染模糊或出现伪影。
  - 在已良好建模的区域可能堆积冗余高斯，浪费存储和计算资源。
- **研究动机**：引入局部感知质量反馈，使密度控制更加感知驱动和空间自适应，以提升关键区域的渲染质量，同时保持高效表示。
- **整体含义**：TileGS是一个基于瓦片（tile）的感知引导框架，通过评估每个渲染瓦片与真值瓦片的结构相似性，指导高斯加密与剪枝，并引入瓦片级结构损失来增强局部一致性。该框架是即插即用的，可无缝整合到现有3DGS流水线中。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 利用3DGS渲染管线中已有的瓦片划分结构（默认16×16像素），对每个瓦片计算SSIM，作为局部渲染质量的指标。
- 基于高斯在哪些瓦片中活跃（即贡献渲染像素），以及这些瓦片的质量（SSIM是否低于阈值），动态决定哪些高斯需要加密、剪枝或保留。
- 同时，引入瓦片级结构损失作为训练辅助项，加强局部结构一致性。

### 关键技术细节

#### 2.1 局部质量评估
- 将渲染图像划分为固定大小的瓦片 \( T = \{T_1, T_2, \ldots, T_N\} \)。
- 计算每个瓦片 \( T_i \) 的SSIM得分 \( s_i = \text{SSIM}(R_i, G_i) \)，其中 \( R_i \) 为渲染瓦片，\( G_i \) 为真值瓦片。
- 若 \( s_i < \tau_{\text{SSIM}} \)（默认0.6），则标记该瓦片为“欠渲染”（under-rendered），需要进一步细化。

#### 2.2 高斯活跃度追踪
- 对每个高斯 \( g_j \) 维护两个计数器：
  - \( a_j \)：该高斯活跃（contributing to rendering）的瓦片总数；
  - \( r_j \)：该高斯活跃且瓦片为欠渲染的瓦片数。
- 每次渲染后，根据高斯覆盖的瓦片更新计数器。

#### 2.3 瓦片引导的加密决策
- 计算每个高斯的失败率 \( f_j = \frac{r_j}{a_j + \epsilon} \)。
- 加密条件：若 \( f_j > \tau_{\text{fail}} \)（默认0.999）且 \( a_j > \tau_{\text{min}} \)（默认500），则对该高斯进行加密（复制或分裂）。
- 剪枝条件：若 \( a_j < \tau_{\text{prune}} \)（默认200），则移除该高斯（因其空间影响微弱）。
- 相比原始梯度累积方法，此决策直接基于感知结果，更关注视觉质量差的区域。

#### 2.4 瓦片级结构损失
- 为每个瓦片分配权重 \( w_i = \frac{\exp(-\alpha \cdot s_i)}{\sum_j \exp(-\alpha \cdot s_j)} \)，其中 \( \alpha > 0 \) 控制对低质量瓦片的强调程度。
- 瓦片级结构损失 \( L_{\text{tile}} = \sum_{T_i \in T} w_i \cdot (1 - s_i) \)。
- 总损失 \( L_{\text{total}} = L_{\text{recon}} + \lambda_{\text{tile}} \cdot L_{\text{tile}} \)，其中 \( \lambda_{\text{tile}} = 0.2 \)。
- 该损失提供局部感知梯度，有助于保留边缘、纹理等细节。

#### 2.5 即插即用设计
- TileGS可作为独立组件，插入现有3DGS变体（如AbsGS、PixelGS、Mini-Splatting-D）的优化流程，只需添加上述追踪和损失项，无需重构网络。

## 3. 实验设计：数据集、基准、对比方法

### 数据集
- **Mip-NeRF 360**：用于大规模、有界/无界场景评估。
- **Tanks and Temples**：几何挑战性场景（如火车、庙宇等）。
- **Deep Blending**：室内外混合场景。

### 基准（评估指标）
- 渲染质量：PSNR（↑）、SSIM（↑）、LPIPS（↓）。
- 模型大小：最终点云内存占用（MB）。
- 兼容性实验还报告了FPS（推理速度）。

### 对比方法
- **3DGS**（基线）
- **AbsGS**、**PixelGS**、**Mip-Splatting**、**Mini-Splatting-D**、**ResGS**（均为近年改进的GS变体）
- 所有方法在相同硬件、相同训练迭代（30k）下重新实现以确保公平。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。
- 仅提到所有实验在相同硬件设置上进行，但未提供细节（如单卡V100、A100等）。
- 这属于实验复现性方面的不足，读者需依赖作者后续开源代码或补充材料。

## 5. 实验数量与充分性

- **数量**：论文共包含以下实验：
  - 主表（Table 1）：在三个数据集上对比6种基线，每个数据集报告4项指标。
  - 兼容性实验（Table 2）：在Deep Blending上，将TileGS嵌入PixelGS、AbsGS、Mini-Splatting-D，对比有无TileGS的PSNR、内存和FPS。
  - 超参数分析（Tile size影响）：见图5，在不同tile size下测试。
  - 消融实验（Table 3）：在Tanks and Temples上，分离瓦片引导加密（TGD）和瓦片级结构损失（TSL）的贡献。
  - 定性视觉比较（图4）：展示多场景渲染结果。
- **充分性分析**：
  - 数据集覆盖了室内外、大尺度、几何复杂等典型场景，具有代表性。
  - 对比了当前主流的GS变体，涵盖梯度重加权、频率正则化、残差加密等不同思路。
  - 消融实验验证了两个核心组件的有效性。
  - 兼容性实验证明了即插即用能力。
  - 不足之处：未提供训练耗时对比、未在不同分辨率或稀疏视图设置下测试、未与更近期的GS变体（如Mega-NeRF、GSGAN等）比较。
- **公平性**：所有基线使用官方代码复现并在相同硬件、相同训练步数下运行，指标计算统一，公平性较好。

## 6. 论文的主要结论与发现

- TileGS在三个数据集上均取得领先或竞争力的渲染质量，同时在模型大小上显著低于大多数基线（例如在Mip-NeRF 360上仅用422MB，而3DGS需734MB，PixelGS需1310MB）。
- 定性结果（图4）显示，TileGS能恢复更清晰的纹理和边界细节，减少模糊和伪影。
- 高斯分布可视化（图3）表明，TileGS促使高斯分布更均匀、覆盖更广，避免了冗余堆积。
- 消融实验表明，瓦片引导加密（TGD）主要提升感知质量（降低LPIPS），而瓦片级结构损失（TSL）有助于压缩内存和改善结构一致性，两者结合效果最佳。
- 兼容性实验证实，TileGS可有效提升其他GS变体的性能，并同时降低内存和加速推理（FPS提升）。

## 7. 优点

- **感知驱动**：将局部SSIM作为直接监督信号，弥补了梯度信号的不足，使密度控制更符合人类视觉感知。
- **即插即用**：无需修改原始GS架构，易于集成到现有流水线，实用性强。
- **高效利用资源**：通过感知引导剪枝和加密，显著降低模型大小，同时保持或提升质量。
- **结构损失设计合理**：采用自适应加权，重点关注低质量区域，避免全局平均导致细节丢失。
- **实验设计完整**：涵盖多数据集、多基线、消融、兼容性、超参数分析，验证充分。

## 8. 不足与局限

- **算力与训练效率未报告**：缺少训练时间、GPU型号等关键信息，影响复现性和实用性评估。
- **阈值依赖手工设置**：\(\tau_{\text{SSIM}}, \tau_{\text{fail}}, \tau_{\text{min}}, \tau_{\text{prune}}, \alpha, \lambda_{\text{tile}}\)等超参数均凭经验设定，未提供自动调整或鲁棒性分析。
- **应用限制**：实验仅在静态场景、密集多视图输入下进行；对于稀疏视图、动态场景、大尺度场景（如街区）的泛化性未知。
- **对比基线未覆盖**：未与最新的GS改进（如GaussianPro、SuGaR、GS-Octree等）或基于NeRF的近期方法比较，最新性存疑。
- **定性结果选择偏差风险**：视觉比较图可能选取了对TileGS有利的视角或区域，未提供大量随机视角的统计对比。
- **内存和速度权衡**：虽然模型变小，但FPS提升可能部分来自更少的高斯，但TileGS的计算额外开销（SSIM计算、活跃度追踪）未详细分析，可能在某些场景中影响实时性。

（完）
