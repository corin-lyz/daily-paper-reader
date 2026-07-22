---
title: "3DRot: 3D Rotation Augmentation for RGB-Based 3D Tasks"
title_zh: 3DRot：面向RGB三维任务的3D旋转增强
authors: "Shitian Yang, Deyu Li, Xiaoke Jiang, Lei Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=EbV2YIKU9o"
tags: ["query:vfm"]
score: 7.0
evidence: 用于深度估计等3D任务的即插即用数据增强
tldr: 本文针对RGB-based 3D任务中标注稀缺和增强工具不足的问题，提出了一种即插即用的3D旋转增强方法3DRot。该方法围绕相机光心旋转镜像图像，并同步更新相机内参和3D标注，保持投影几何一致性。在单目3D检测任务上验证了有效性，并可直接迁移到深度估计等其他3D任务，提升模型鲁棒性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: RGB三维任务标注稀少且增强工具箱有限，现有图像变换破坏几何一致性。
method: 提出围绕相机光心旋转并同步更新几何信息的即插即用增强方法。
result: 在SUN RGB-D上提升单目3D检测的IoU 3D指标。
conclusion: 3DRot是一种简单有效的数据增强手段，可推广到多种三维任务。
---

## Abstract
RGB-based 3D tasks, e.g., 3D detection, depth estimation, 3D keypoint estimation, still suffer from scarce, expensive annotations and a thin augmentation toolbox, since many image transforms, including rotations and warps, disrupt geometric consistency.
In this paper, we introduce 3DRot, a plug-and-play augmentation that rotates and mirrors images about the camera's optical center while synchronously updating RGB images, camera intrinsics, object poses, and 3D annotations to preserve projective geometry, achieving geometry-consistent rotations and reflections without relying on any scene depth.
We first validate 3DRot on a classical RGB-based 3D task, monocular 3D detection. On SUN RGB-D, inserting 3DRot into a frozen DINO-X + Cube R-CNN pipeline raises $IoU_{3D}$ from 43.21 to 44.51, cuts rotation error (ROT) from 22.91$^\circ$ to 20.93$^\circ$, and boosts $mAP_{0.5}$ from 35.70 to 38.11; smaller but consistent gains appear on a cross-domain IN10 split. \rev{Beyond monocular detection, adding 3DRot on top of the standard BTS augmentation schedule further improves NYU Depth v2 from 0.1783 to 0.1685 in abs-rel (and 0.7472 to 0.7548 in $\delta<1.25$), and reduces cross-dataset error on SUN RGB-D. On KITTI, applying the same camera-centric rotations in MVX-Net (LiDAR+RGB) raises moderate 3D AP from about 63.85 to 65.16 while remaining compatible with standard 3D augmentations. Because it operates purely through camera-space transforms, 3DRot drops into diverse RGB-based 3D tasks and multi-modal pipelines without architectural changes or depth supervision.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：RGB-based 3D任务（如单目3D检测、深度估计、3D关键点估计）面临两个主要困境：① 3D标注（如深度、3D边界框）获取成本高昂、数量稀缺；② 现有的数据增强工具箱非常薄弱，因为许多常见的图像变换（如旋转、裁剪、扭曲）会破坏图像与3D几何之间的投影一致性，导致增强后的数据无法直接用于3D监督。
- **动机**：需要一种能保持几何一致性的数据增强方法，不依赖场景深度信息，且能即插即用地应用于多种RGB三维任务。
- **整体含义**：本文提出3DRot——围绕相机光心旋转/镜像图像，并同步更新相机内参、物体姿态和3D标注，从而在不破坏投影几何的前提下生成多样化的训练样本，缓解标注稀缺问题，提升模型鲁棒性。

## 2. 方法论

### 核心思想
- 在不依赖任何场景深度信息的条件下，通过对图像进行围绕相机光心的旋转（或镜像），并相应地调整相机内参和3D标注（如物体位姿、边界框），使得变换后的图像与3D几何仍然满足透视投影关系，即保持“几何一致性”。

### 关键技术细节
- **操作步骤**：
  1. 绕相机光心（即相机坐标系原点）旋转或镜像原始RGB图像。
  2. 根据旋转角度同步更新相机内参矩阵（如焦距、主点偏移），以匹配新的图像平面。
  3. 同时更新所有3D标注：对于物体姿态（如旋转、平移）和3D边界框，应用与图像相同的旋转/镜像变换（在相机坐标系下）。
- **特点**：
  - 无需任何深度估计或场景几何先验。
  - 纯相机空间变换，不依赖点云或深度图。
  - 即插即用：可直接插入现有RGB-based 3D任务的训练管道，无需修改模型架构。

### 算法流程（文字描述）
- 输入：原始RGB图像、相机内参、物体3D标注（如中心位置、尺寸、朝向）。
- 选择旋转角度（如0°, 90°, 180°, 270°或随机角度）或镜像（水平/垂直）。
- 对图像执行旋转/镜像。
- 计算新的相机内参（旋转后主点偏移等）。
- 对每个物体，将其3D中心坐标和朝向在相机坐标系下应用同样的旋转/镜像。
- 输出：增强后的图像、更新后的内参和标注，保持投影几何一致。

## 3. 实验设计

- **任务与数据集**：
  - **单目3D检测**：SUN RGB-D 数据集（室内场景），并使用 IN10 跨域分裂（cross-domain split）评估泛化性。
  - **深度估计**：NYU Depth v2 数据集，以及跨数据集迁移到 SUN RGB-D 深度估计。
  - **多模态融合3D检测**：KITTI 数据集，在 MVX-Net（LiDAR+RGB）上测试。
- **基准模型**：
  - 单目3D检测：DINO-X + Cube R-CNN 管道（冻结特征提取器）。
  - 深度估计：BTS（标准基线），在标准增强调度基础上加入3DRot。
  - 多模态检测：MVX-Net（LiDAR+RGB）。
- **对比方法**：未明确列出其他增强方法比较，主要体现3DRot vs. 无此增强的基线表现。

## 4. 资源与算力

- 论文中**未明确提及**使用的GPU型号、数量、训练时长等算力资源。仅说明方法即插即用、计算开销小（旋转/镜像及其几何更新操作简单），但未给出具体硬件配置。

## 5. 实验数量与充分性

- **实验组数**：
  - 单目3D检测：一个主实验（SUN RGB-D）+ 一个跨域实验（IN10 split）。
  - 深度估计：一个主实验（NYU Depth v2）+ 一个跨数据集测试（SUN RGB-D）。
  - 多模态检测：一个实验（KITTI上MVX-Net）。
  - 缺少专门的消融实验（如不同旋转角度的贡献、与随机裁剪等常见增强的交互等）。
- **充分性评价**：
  - **优点**：覆盖了三个不同的RGB-based 3D任务（检测、深度估计、多模态），并包含跨域/跨数据集评估，说明方法具有一定通用性。
  - **不足**：
    - 实验数量较少，每个任务仅展示一组主要结果，没有系统地分析增强强度（旋转角度范围）的影响。
    - 没有与其他几何保持增强方法（如基于深度图的变换）进行对比。
    - 没有在更多数据集（如nuScenes、Waymo）上验证。
    - 缺乏消融实验来分离旋转和镜像各自的贡献。
  - **客观性**：结果报告了提升的具体数值（IoU 3D从43.21→44.51等），但未提供方差或显著性检验。

## 6. 主要结论与发现

- 3DRot作为一种简单的即插即用增强，能够显著提升单目3D检测性能（SUN RGB-D上IoU 3D提升1.3个百分点，旋转误差降低约2°，mAP 0.5提升约2.4个百分点）。
- 在深度估计任务上，加入到BTS标准增强调度后，NYU Depth v2上的绝对相对误差（abs-rel）从0.1783降至0.1685，阈值精度（δ<1.25）从0.7472提升至0.7548；在跨数据集（SUN RGB-D）上也降低了误差。
- 在多模态检测（KITTI上MVX-Net）中，中等难度3D AP提升约1.3个百分点（63.85→65.16），且与LiDAR增强兼容。
- 3DRot的纯相机空间变换特性使其可以无缝集成到多种RGB-based 3D任务和多模态管道中，无需架构改动或深度监督。

## 7. 优点

- **简单有效**：仅利用图像旋转/镜像和对应几何更新，原理简单，实现容易，却能带来一致且非平凡的提升。
- **即插即用**：不依赖任何深度信息，可直接插入现有训练流程，无需修改模型。
- **通用性强**：在单目3D检测、深度估计、多模态检测三种不同任务上均有效，覆盖了RGB-based 3D的核心问题。
- **保持几何一致性**：解决了传统图像增强破坏投影几何的痛点，使得增强数据可用于3D监督训练。

## 8. 不足与局限

- **实验覆盖有限**：仅三个数据集（SUN RGB-D, NYU Depth v2, KITTI），且每个任务仅一个主实验，缺乏在更多样化场景（如室外、自动驾驶大规模数据集）下的验证。
- **缺乏消融研究**：未分析旋转角度、镜像方向、组合策略等对性能的单独影响，也没有与随机裁剪、颜色抖动等常见增强的组合效应。
- **潜在偏差风险**：旋转/镜像操作可能引入特定方向的偏差（如室内场景中物体朝向分布不均），论文未讨论。
- **应用限制**：对于需要保持绝对尺度或方向不变的任务（如物理仿真），旋转可能不适用；另外，对于大旋转角度（如90°或更大），图像边缘内容丢失可能影响性能。
- **计算资源未报告**：无法评估方法的实际计算开销相对于基线增加的比例。

（完）
