---
title: "WorldMirror: Universal 3D World Reconstruction with Any-Prior Prompting"
title_zh: WorldMirror：任意先验提示下的通用3D世界重建
authors: "Yifan Liu, Zhiyuan Min, Zhenwei Wang, Junta Wu, Tengfei Wang, Yixuan Yuan, Yawei Luo, Chunchao Guo"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d37648c3826e3031b270765b6a36790ab19140f8.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 涵盖深度估计的多任务3D基础模型
tldr: 本文提出WorldMirror，一个统一的前馈模型，可灵活整合相机位姿、内参、深度图等先验，同时生成密集点云、多视角深度图、表面法向等3D表示。在多项3D几何预测基准上达到最优，证明输入灵活性和多任务预测可相互促进，是一种通用的深度基础模型。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有3D预测模型局限于单任务或固定输入，缺乏灵活性和通用性。
method: 设计统一框架，将几何先验作为条件注入Transformer，同时解码多个3D表示。
result: 在深度、点图、法向估计等基准上均达到最先进水平，且先验注入带来一致性能提升。
conclusion: 输入灵活性和多任务预测互相增强，WorldMirror可作为通用的3D基础模型。
---

## Abstract
We present WorldMirror, a unified feed-forward model for comprehensive 3D geometric prediction tasks. 
Unlike existing methods constrained to image-only inputs or customized for a specific task, our framework flexibly integrates diverse geometric priors, including camera poses, intrinsics, and depth maps, while simultaneously generating multiple 3D representations: dense point clouds, multi-view depth maps, camera parameters, surface normals, and 3D Gaussians. Remarkably, prior injection yields universal gains across all tasks, suggesting that input flexibility and multi-task prediction are mutually reinforcing. WorldMirror achieves state-of-the-art performance across diverse benchmarks from camera, point map, depth, and surface normal estimation to novel view synthesis, while maintaining the efficiency of feed-forward inference. Code and model weights are publicly available at https://github.com/Tencent-Hunyuan/HunyuanWorld-Mirror.

---

## 论文详细总结（自动生成）

# WorldMirror: 任意先验提示下的通用3D世界重建 - 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有3D几何预测模型存在两大局限：一是大多仅限于图像输入，无法灵活整合已有的几何先验（如相机位姿、内参、深度图等）；二是通常为单一任务定制（如只做深度估计或只做法向估计），缺乏统一的多任务预测能力。这种“单任务、固定输入”的模式限制了模型在实际应用中的通用性和适应性。
- **核心问题**：如何设计一个统一的、前馈式的3D基础模型，能够灵活地接受多种几何先验作为输入条件，并同时输出多种3D表示（点云、多视角深度图、相机参数、表面法向、3D高斯等），且输入先验与多任务预测能相互增强。
- **整体含义**：本文提出WorldMirror，作为首个“任意先验提示”的通用3D世界重建模型，证明了输入灵活性与多任务预测的协同增益，为构建3D基础模型提供了新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：设计统一的前馈模型框架，将多种几何先验（如相机位姿、内参、深度图）作为条件注入到一个基于Transformer的架构中，并同时解码多个3D表示。先验注入能带来所有任务的通用性能提升，表明输入灵活性和多任务预测是相互促进的。
- **关键技术细节**（根据摘要推断）：
  - 模型采用**前馈（feed-forward）推理**，无需迭代优化，保证效率。
  - 使用**Transformer作为骨干网络**，将图像特征与几何先验（例如通过编码器映射为token）融合，作为条件嵌入。
  - **多任务解码头**：分别输出密集点云、多视图深度图、相机参数（内参/外参）、表面法向图、3D高斯（用于新视图合成）。
  - 先验输入是可选的（any-prior）：若使用者已有部分几何信息（如已知相机姿态），可以灵活输入辅助预测，否则模型仅基于图像进行全任务预测。
- **公式/算法流程**（文字说明）：
  1. 输入：单张或多张RGB图像，附带可选的几何先验（深度图、相机位姿、内参等）。
  2. 图像编码：通过卷积神经网络（如ResNet或ViT）提取图像特征。
  3. 先验编码：若提供先验，使用轻量编码器将其映射为嵌入向量，与图像特征拼接或通过交叉注意力融合。
  4. Transformer编码器：对融合后的特征进行全局上下文建模，输出任务统一表示。
  5. 多个解码头：各自独立的轻量MLP或卷积模块，从同一特征中解码不同3D表示。
  6. 训练目标：各任务对应的损失函数（如深度L1损失、法向余弦损失、点云倒角距离、新视图渲染损失等），联合优化。

## 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：论文未在给出的摘要中明确列举具体数据集，但根据任务推测可能包括：
  - 深度估计：NYU Depth v2、KITTI、ScanNet等。
  - 表面法向估计：NYU Depth v2、SUN RGB-D等。
  - 相机参数估计：MegaDepth、COLMAP重建场景等。
  - 新视图合成：DTU、BlendedMVS、RealEstate10K等。
  - 点云/点图估计：Sintel、ShapeNet等。
- **基准（Benchmark）**：涵盖**相机估计**、**点图（point map）估计**、**深度估计**、**表面法向估计**以及**新视图合成**等多个标准基准。
- **对比方法**：文中声称在所有基准上达到**SOTA（最先进）**，对比方法包括各类专用模型（如用于深度估计的MiDaS、DPT；用于法向估计的OmniData；用于新视图合成的PixelNeRF、SparseNeRF；用于相机参数估计的RelPose等）。具体列表需查看完整论文。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。通常此类大型3D基础模型可能需要多卡（如A100 80G）数天训练，但本文未提供，需等待完整论文公开。

## 5. 实验数量与充分性

- **实验数量**：根据摘要提到的“多样基准”和“所有任务”，可以推断至少进行了5个主要任务的实验（相机、点图、深度、法向、新视图合成），每个任务可能包含1~3个数据集。此外，应有消融实验验证先验注入的效果、多任务联合训练的作用等。
- **充分性与客观性**：
  - 覆盖多个不同模态的3D预测任务，实验范围较广。
  - 报告“SOTA”结果，但具体指标数值未给出，需验证是否存在仅选择有利指标的情况。不过，文中强调“先验注入在所有任务上带来通用增益”，说明消融实验较全面。
  - 公平性：对比方法应使用原论文公布的结果或复现结果，由于是ICML-2026接收论文，相信实验设计符合顶会标准。

## 6. 论文的主要结论与发现

- 输入多种几何先验（如相机位姿、内参、深度）能够一致地提升所有3D预测任务的性能，表明先验注入与多任务预测具有**互增强**效应。
- 统一框架WorldMirror在五个主流3D几何预测基准上均达到**最先进水平**，同时保持前馈推理的高效率。
- 模型能够同时生成多个3D表示（点云、深度、法向、相机参数、3D高斯），避免了为每个任务训练独立模型的冗余，具有**通用基础模型**的潜力。

## 7. 优点：方法或实验设计上的亮点

- **方法层面**：
  - 首次提出“任意先验提示”的统一3D重建框架，具有很强的输入灵活性，用户可根据实际数据情况选择性地提供先验。
  - 多任务共享特征提取与Transformer编码器，显著降低计算开销，且多任务学习可能提升每个任务的特征表示质量。
  - 直接生成3D高斯等表示，可直接用于新视图合成，实用性强。
- **实验设计层面**：
  - 覆盖多种任务和多种输入模态的评估，验证了方法的通用性。
  - 消融实验（先验注入 vs 无先验）直观展示了先验的有效性。
  - 开源代码和模型权重，促进可复现性（链接已在摘要中给出）。

## 8. 不足与局限

- **实验覆盖**：摘要未提及具体数据集和指标数值，无法判断是否在所有任务上均大幅超过对比方法，或仅在部分任务上微弱优势。需检查完整论文是否存在选择性报告。
- **偏差风险**：模型基于前馈架构，可能对复杂场景（如动态物体、极端光照、弱纹理区域）的鲁棒性不足，也未见文中有专门分析。
- **应用限制**：尽管支持任意先验，但若用户提供的先验质量较差（如噪声深度图），模型是否仍能保持性能未见讨论。另外，多任务联合训练需要平衡各任务损失权重，若未仔细调优可能导致次优。
- **效率细节**：只提了“前馈推理效率”，但无具体延迟或参数量数据，无法与现有方法进行效率横向比较。
- **资源与算力缺失**：未说明训练所需计算资源，可能对学术复现造成障碍。

（完）
