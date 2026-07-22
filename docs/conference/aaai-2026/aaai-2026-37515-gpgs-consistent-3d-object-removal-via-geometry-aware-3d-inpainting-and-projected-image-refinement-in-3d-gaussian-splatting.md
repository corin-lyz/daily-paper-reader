---
title: "GPGS: Consistent 3D Object Removal via Geometry-Aware 3D Inpainting and Projected Image Refinement in 3D Gaussian Splatting"
title_zh: "GPGS: 基于几何感知3D修补和投影图像精修的持续3D物体移除"
authors: "Yongjoon Lee, Donghyeon Cho"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37515/41477"
tags: ["query:depth-refine"]
score: 4.0
evidence: 粗到细的几何感知修补以实现一致3D移除
tldr: 2D修补模型进行3D物体移除时存在几何不准确和多视角不一致的问题。GPGS基于3D高斯泼溅框架，利用点云补全模型和粗到细推理策略实现几何感知的3D修补，并通过投影图像精确保留多视角一致性。在多个数据集上验证了其优越性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 839, \"height\": 393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1626, \"height\": 737, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 830, \"height\": 363, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 423, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 811, \"height\": 436, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1750, \"height\": 559, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37515/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1746, \"height\": 716, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37515/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 239, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37515/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 240, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37515/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 877, \"height\": 259, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37515/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 246, \"label\": \"Table\"}]"
motivation: 现有2D修补进行3D物体移除时几何恢复不准确且多视角不一致。
method: 采用预训练点云补全模型和粗到细推理实现几何感知3D修补。
result: 在3D物体移除任务中实现了更准确的几何修复和多视角一致性。
conclusion: 结合3D几何先验和粗到细策略可有效提升3D修补的鲁棒性。
---

## Abstract
Object removal in 3D space is a key technology for immersive applications such as virtual reality (VR), augmented reality (AR), and the metaverse. While recent approaches have attempted to address this task using 2D inpainting models, they often suffer from two major limitations: (1) inaccurate geometric restoration in the removed regions, and (2) visual inconsistency across multiple viewpoints. To address these challenges, we propose GPGS, a novel pipeline built upon the 3D Gaussian Splatting (3DGS) framework. First, we perform geometry-aware 3D inpainting by leveraging a pre-trained point cloud completion model and a coarse-to-fine inference strategy, enabling accurate restoration of unseen 3D structures. Next, we introduce a projected image refinement method that improves the appearance of novel-view projections by addressing view-dependent artifacts such as brightness shifts and texture misalignments. GPGS further enhances overall scene consistency through fine-tuning of the original 3DGS scene using the refined multi-view images. Experimental results show that our GPGS makes geometrically accurate and visually coherent outputs, even in challenging 360° panoramic scenes, significantly outperforming existing methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：在3D场景中移除物体是VR/AR/元宇宙等沉浸式应用的关键技术。现有基于2D修补模型的方法存在两大局限：一是移除区域几何恢复不准确，二是多视角间视觉不一致。
- **背景**：3D Gaussian Splatting（3DGS）被广泛用于场景重建和编辑。已有方法（如AuraFusion、Infusion）尝试使用2D修补，但在360°全景场景中，由于视场角宽、遮挡复杂，修补结果常出现几何失真和跨视角差异。
- **目标**：提出一种能在360°场景中实现几何准确、视觉一致且自然的物体移除方法。

## 2. 方法论：核心思想与关键技术细节
- **整体框架**：基于3DGS，包含三个阶段：物体分离 → 几何感知3D修补 → 投影图像精修。最后用精修后的多视角图像微调原始3DGS场景。

### 2.1 物体分离
- 利用输入二进制掩码对3DGS进行额外训练，使物体对应的高斯点颜色接近1（白色），背景接近0。通过比较颜色差异识别物体点并去除，得到分离后的场景 $G_s$。
- 手动选择参考视图，用SAM获取最优掩码，再用LeftRefill修补未见过区域，生成高质量参考图像。

### 2.2 几何感知3D修补（核心创新）
- **动机**：传统2D深度修补或深度对齐方法在小误差时会被放大，导致投影不准。本文直接在3D空间用点云补全模型Point-MAE完成缺失几何。
- **方法**：
  - 从 $G_s$ 渲染图像并反投影得到3D点云。
  - 采用粗到细策略：先裁剪包含未见过区域的宽点云，逐步增加未见过区域比例，使Point-MAE能从相邻中心点估计缺失点。重复细阶段直至未见过区域深度覆盖达70%，剩余用插值填充。
  - 在场景数据上微调Point-MAE（用Chamfer距离损失训练），使其适应特定场景几何。

### 2.3 投影图像精修
- **问题**：投影会忽略视角相关亮度变化，且参考视图像素数可能不足，造成空白或纹理失真。
- **步骤**：
  - **亮度校正**：在LAB颜色空间中，对未见过区域的L通道进行均值-标准差转移，使其与周围区域亮度匹配。
  - **纹理精修网络**：5层CNN，输入投影结果，输出残差。训练时从未见过区域的周围区域提取16×16 patch，使用对空间不对中鲁棒的频率分布损失（FDL）进行训练。同时拼接坐标和图像索引嵌入以增强泛化。
  - **微调**：用精修后的多视图图像微调 $G_s$，并在未见过区域添加初始高斯点加速恢复。

## 3. 实验设计
- **数据集**：
  - **COR-NeRF**：11个360°无界场景，每场景150-200张训练图像（含物体）及等量测试图像（不含物体），提供GT。
  - **360-USID**：7个360°无界场景，每场景180-200张训练图像，30-40张测试图像，并预先提供参考图像。
- **评价指标**：PSNR、SSIM、LPIPS、FID（均在物体掩码边界框内测量）。
- **对比方法**：Gaussian Grouping、Infusion、Gscream、AuraFusion。均为SOTA方法。实验中为公平比较，给对手提供优越条件（如为AuraFusion提供正确未见过区域掩码，将其2DGS改为RaDe-GS以适应COR-NeRF数据）。

## 4. 资源与算力
- 论文明确说明：所有实验使用单张 RTX 3090 GPU。未报告训练时长或迭代次数（仅提及物体分离阶段训练300迭代，其他阶段具体迭代次数未给出）。

## 5. 实验数量与充分性
- **数量**：在两个数据集上对比4种方法，并进行了两组消融实验（几何修补方法对比、精修各组件消融）。总计6~7组定量实验及大量定性对比。
- **充分性**：实验覆盖了主流360°移除场景，消融设计合理，分别验证了几何感知3D修补和投影精修的有效性。但未在更广泛数据集（如Forward-facing场景）上验证；未进行超参数敏感性分析或实时性测试。对比实验通过提供对手有利条件（如手动掩码）保证了公平性，但主观评估（用户研究）缺失。

## 6. 主要结论与发现
- GPGS在几何准确性和多视角一致性上显著优于现有方法。
- 几何感知3D修补比2D深度修补投影更结构更精确，尤其能保持参考图像的结构。
- 投影精修中的亮度转移和基于FDL的纹理恢复对提升感知质量（LPIPS、FID）至关重要。
- 在PSNR指标上GPGS并非最优（略低于AuraFusion），但其他感知指标全面领先，说明其更注重视觉一致性而非像素级相似度。

## 7. 优点
- **创新性**：首次将3D点云补全（Point-MAE）引入3DGS物体移除，替代传统的2D深度修补，从根本上解决了投影几何误差放大问题。
- **方法设计**：粗到细策略解决了Point-MAE无法直接填充无中心点区域的问题；投影精修网络结合亮度转移和FDL损失，有效处理了视角相关伪影。
- **实验公平性**：对比时给对手提供手动掩码等优越条件，使结果更具说服力。
- **通用性**：提出的物体分离方法和精修网络可适配不同3DGS变体（如RaDe-GS），不依赖于特定架构。

## 8. 不足与局限
- **依赖手动操作**：参考视图选择和未见过区域掩码需人工标注，自动化程度不足。
- **计算开销**：Point-MAE的粗到细推理需要多次迭代，且需对场景数据进行微调，训练时间未报告，但推测较长。
- **实验覆盖**：仅在360°无界场景上验证，未测试前向场景或室内小场景；未与基于扩散模型的新方法（如RefFusion）直接比较。
- **PSNR劣势**：像素级精度不如某些方法，可能因投影精修引入了轻微偏差。
- **应用限制**：场景需提供物体掩码，无法直接处理非分割场景；对动态物体或复杂光照可能不鲁棒。

（完）
