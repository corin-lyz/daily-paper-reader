---
title: "Depth Any Camera: Zero-Shot Metric Depth Estimation from Any Camera"
title_zh: Depth Any Camera：任意相机的零样本度量深度估计
authors: "Guo, Yuliang, Garg, Sparsh, Miangoleh, S. Mahdi H., Huang, Xinyu, Ren, Liu"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Guo_Depth_Any_Camera_Zero-Shot_Metric_Depth_Estimation_from_Any_Camera_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 10.0
evidence: 零样本度量深度基础模型，扩展到任意相机与广角/全景
tldr: 现有深度基础模型在大视场如鱼眼和全景相机上难以保持度量精度。本文提出Depth Any Camera（DAC），将透视图训练模型扩展至多种相机类型，仅用透视图像训练即可零样本泛化到鱼眼与360度相机。该方法有效利用既有三维数据，为大视场相机提供高精度度量深度估计。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 821, \"height\": 460, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 865, \"height\": 389, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 858, \"height\": 408, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1723, \"height\": 848, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 811, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1700, \"height\": 1442, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1835, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1496, \"height\": 745, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 835, \"height\": 609, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-guo-depth-any-camera-zero-shot-metric-depth-estimation-from-any-camera-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 840, \"height\": 612, \"label\": \"Table\"}]"
motivation: 深度基础模型对大视场相机（鱼眼、360度）的零样本度量深度估计精度不足。
method: 设计可扩展到任意视场角的框架，通过透视模型训练并适配非透视相机，无需专门数据。
result: 在鱼眼和360度相机上实现无缝零样本度量泛化，超越现有方法。
conclusion: 为任意相机提供了一种通用的零样本度量深度估计框架。
---

## Abstract
While recent depth foundation models exhibit strong zero-shot generalization, achieving accurate metric depth across diverse camera types--particularly those with large fields of view (FoV) such as fisheye and 360-degree cameras--remains a significant challenge. This paper presents Depth Any Camera (DAC), a powerful zero-shot metric depth estimation framework that extends a perspective-trained model to effectively handle cameras with varying FoVs. The framework is designed to ensure that all existing 3D data can be leveraged, regardless of the specific camera types used in new applications. Remarkably, DAC is trained exclusively on perspective images but generalizes seamlessly to fisheye and 360-degree cameras without the need for specialized training data. DAC employs Equi-Rectangular Projection (ERP) as a unified image representation, enabling consistent processing of images with diverse FoVs. Its key components include a pitch-aware Image-to-ERP conversion for efficient online augmentation in ERP space, a FoV alignment operation to support effective training across a wide range of FoVs, and multi-resolution data augmentation to address resolution disparities between training and testing. DAC achieves state-of-the-art zero-shot metric depth estimation, improving delta-1 accuracy by up to 50% on multiple fisheye and 360-degree datasets compared to prior metric depth foundation models, demonstrating robust generalization across camera types.

---

## 论文详细总结（自动生成）

```markdown
# 论文深度总结：Depth Any Camera（DAC）

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：单目深度估计是自动驾驶、AR/VR、机器人等应用的基础任务。近年来，深度基础模型（如 Metric3Dv2、UniDepth、Depth Anything）在**零样本度量深度估计**上表现出很强的泛化能力。
- **核心问题**：现有模型大多基于**透视相机（pinhole/perspective）** 训练，在应对**大视场（FoV）相机**——特别是**鱼眼相机**和**360度全景相机**时，性能显著下降。主要挑战包括：
  1. 如何用统一的相机模型表示不同 FoV；
  2. 如何仅利用透视训练数据，泛化到只有大 FoV 相机才能观测到的数据空间；
  3. 如何在统一空间中处理不同 FoV 导致的训练图像尺寸差异；
  4. 如何解决训练与测试阶段分辨率不一致的问题。
- **论文意义**：提出 **Depth Any Camera（DAC）** 框架，使**仅用透视图像训练**的模型能够零样本泛化到鱼眼、360度等任意相机类型，从而让已有的海量透视 3D 数据在大 FoV 相机的新应用中继续发挥作用，无需重新采集或专门标注大 FoV 数据。

## 2. 方法论

### 2.1 总体思路
- 以 **Equi-Rectangular Projection（ERP，等距柱状投影）** 作为统一的图像表示空间，将各种相机类型（透视、鱼眼、360度）的输入统一转换到 ERP 空间中训练与推理。
- 训练时仅使用透视图像，但通过**在线数据增强**模拟大 FoV 相机中特有的高畸变区域，从而让模型“看到”这些透视数据中原本不存在的观测模式。

### 2.2 关键技术组件
1. **Pitch-Aware Image-to-ERP 转换（俯仰角感知的图像到 ERP 转换）**
   - 基于 **Gnomonic Projection（日晷投影/测地投影）** 的闭式映射，将输入图像像素坐标与球面坐标（经纬度）对应起来。
   - 核心公式：
     \[
     x_t = \frac{\cos\phi \sin(\lambda - \lambda_c)}{\cos c}, \quad
     y_t = \frac{\cos\phi_c \sin\phi - \sin\phi_c \cos\phi \cos(\lambda - \lambda_c)}{\cos c}
     \]
     其中 \(\cos c = \sin\phi_c\sin\phi + \cos\phi_c\cos\phi\cos(\lambda - \lambda_c)\)。
   - 利用 **grid sampling（网格采样）**：在目标 ERP patch 内均匀采样网格点，通过公式反向映射到输入图像的浮点坐标，再插值取像素值，从而高效完成转换。
   - **关键创新**：将 ERP patch 中心纬度 \(\phi_c\) 与相机俯仰角关联。训练时，根据相机俯仰角（可估计或已知）将透视图像投影到 ERP 的不同纬度区域，从而**模拟大 FoV 相机才能看到的高畸变区域**，显著提升零样本泛化能力。
   - 此外，可在归一化图像坐标上直接进行缩放、旋转、平移等在线增强，代价很低。

2. **FoV Alignment（视场对齐）**
   - 不同训练数据（如 HM3D 中 FoV 变化很大）在 ERP 空间中的投影区域大小差异巨大，若使用固定 patch 裁剪，要么丢失内容，要么在背景 padding 上浪费大量计算。
   - 方法：根据实际相机 FoV 和预定义 ERP patch 的垂直 FoV，计算一个特定的缩放因子 \(s_* = \frac{\text{Fov}_c}{\text{Fov}_e}\)，将每个样本缩放到统一的目标 ERP patch 大小，最大化内容保留、最小化计算浪费，提升训练效率与质量。

3. **Multi-Resolution Training（多分辨率训练）**
   - 训练 patch 与测试图像分辨率不一致会导致注意力模块性能下降。
   - 方法：将每个 ERP patch 缩放到额外两个较低分辨率（如原分辨率的 0.7 和 0.4），与原始分辨率组成三个 batch 一起训练，损失求和，使模型学习尺度等变特征，适应更广泛的测试分辨率。

### 2.3 模型与训练细节
- 网络架构：选用 **iDisc** 作为基线网络（含 cross-attention 和 self-attention），使用 **SIlog 损失函数**。
- 深度表示：使用**到相机中心的欧氏距离**而非 Z-buffer 深度，因为后者在球面投影下会产生错误的小深度值。

## 3. 实验设计

### 3.1 训练数据集（均为透视图像）
- **室内**：HM3D-tiny（310K 张，Recon 低质量，FoV 36°–124°，俯仰角变化大）、Taskonomy-tiny（300K 张，真实高质量，FoV 45°–75°）、Hypersim（54K 张，仿真高质量，FoV 60°）。
- **室外**：DDAD（80K 张，FoV 45°–60°）、LYFT（50K 张，FoV 20°–60°）。
- 室内与室外分别训练独立模型，不混合数据。

### 3.2 零样本测试数据集（大 FoV）
- **360° 全景**：Matterport3D、Pano3D-GV2。
- **鱼眼**：ScanNet++（FoV 150°）、KITTI360（FoV 180°）。
- 另外在 NYUv2、KITTI 上补充了透视数据测试（见补充材料）。

### 3.3 评测指标
- 度量深度指标：\(\delta_1\)、\(\delta_2\)、\(\delta_3\)（阈值精度）、Abs Rel、RMSE、log10。

### 3.4 对比方法
- **Metric3Dv2**（透视规范相机模型基础模型，Dinov2 backbone）
- **UniDepth**（网络内估算并转换相机内参的更近期基础模型）
- **iDisc**（网络基线，使用 Metric3D 训练管线重训，控制网络架构变量）

### 3.5 实现参数
- ERP 全图高 \(H_{ERP}=1400\)，ERP patch 大小 500×700。
- 室内外均使用 10° 纬度增强；室内额外 10° 旋转增强。
- ResNet101 backbone 训练 60k 迭代，batch size 48；Swin-L/DINOv2 训练 120k 迭代，batch size 48。

## 4. 资源与算力

- **论文正文未明确说明**使用的 GPU 型号、数量及具体训练时长（小时数）。
- 仅能间接推断：训练迭代数为 60k（ResNet101）或 120k（Swin-L/DINOv2），batch size 均为 48；训练数据总量为 670K（室内）/130K（室外）张透视图像，且使用多分辨率三 batch 同时训练，因此算力需求较高。
- 结论：**算力信息缺失**，无法复现能耗或硬件需求。

## 5. 实验数量与充分性

- **主要对比实验**：在 4 个大 FoV 测试集（Matterport3D、Pano3D-GV2、ScanNet++、KITTI360）上与 3 个基线方法对比，每个数据集报告 6 个指标，共 4 组核心对比实验。
- **消融实验**：
  - 逐一去除 Pitch-Aware ERP、Pitch Aug、FoV Align、Multi-Reso，在 Pano3D-GV2 和 ScanNet++ 上进行验证（补充材料还包含 Matterport3D 完整版）。
  - 对比 iDisc-cnn 与 iDisc，验证注意力模块的作用。
  - 分别在不同训练数据集（HM3D、Taskonomy、Hypersim）上单独训练并测试，分析数据特点影响。
- **补充实验**：NYUv2、KITTI 上的透视数据集评估；Swin-L backbone 结果；完整指标表（Supplemental Table 5/6/7）。
- **充分性评价**：
  - **优点**：覆盖室内/室外、全景/鱼眼、多种训练数据、多骨干网络，并有较为系统的消融，实验设计较为全面。
  - **不足**：部分消融只报告了 \(\delta_1\) 和 A.Rel，核心指标不全；没有报告训练数据混合后的消融（如不同数据比例）；没有与更多面向 360 度的专门方法对比（如 PanoFormer、OmniFusion 等，仅在 Related Work 中提到）；没有分析推理时间或模型参数量；室外实验改进幅度较小，但论文给出了合理解释。

## 6. 主要结论与发现

- DAC 在**所有大 FoV 测试集**上均达到 state-of-the-art 零样本性能，在室内 360° 和鱼眼数据集上 \(\delta_1\) 精度相对次优方法提升 **高达 50%**，即使使用更轻的 ResNet101 backbone 和更小的训练数据。
- 在室外 KITTI360 上，DAC 相对 Metric3Dv2/UniDepth 显著提升，但相对同网络配置的 iDisc 改进有限，原因是室外训练数据俯仰角变化小，模拟高畸变区域能力受限，且 KITTI360 的 LiDAR 评估点集中在低畸变区域。
- **Pitch-Aware ERP 转换是核心助推器**：能有效模拟大 FoV 相机可见的高畸变区域，而深度网络本身难以外推到该数据域（UniDepth 即失败案例）。
- 当训练数据本身（如 HM3D）包含较大俯仰角变化时，额外的 Pitch Aug 收益下降；对俯仰角变化小的数据集，Pitch Aug 仍然重要。
- FoV Align 和 Multi-Reso Training 对 360° 图像泛化尤为关键。
- 不同训练数据对泛化的贡献不同：HM3D 对 360°（Pano3D-GV2）更好（因 FoV 和俯仰覆盖广），而 Hypersim 对鱼眼（ScanNet++）更好（因图像质量高）；混合数据集可进一步提升泛化能力。

## 7. 优点（方法与实验亮点）

- **视角创新**：将“透视数据训练→任意相机泛化”问题统一到 ERP 空间，解决了大 FoV 相机数据稀缺的根本瓶颈。
- **几何驱动的训练增强**：利用 Gnomonic 网格采样与俯仰角信息，实现高效且物理合理的畸变模拟，避免依赖深度学习网络自行外推。
- **工程友好**：FoV Alignment 在统一 patch 下最大化内容利用率，Multi-Resolution 训练解决实际部署中的分辨率差异，方法可直接适配不同网络骨干（ResNet101、Swin-L、DINOv2）。
- **实验公平性**：与 iDisc 在同一网络架构、同一训练数据下对比，隔离了网络结构的影响；对无法适应任意分辨率的基线方法，报告两种分辨率下的较优结果，保证公平。
- **实用价值**：保证已采集的透视 3D 数据在新相机类型上仍可使用，降低数据重采成本。

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量、训练时长，影响复现和资源评估。
- **室外大 FoV 性能提升有限**：在 KITTI360 上相对于同网络 iDisc 的增益较小；且 KITTI360的评价点集中在低畸变区域，削弱了对比的区分度。
- **依赖相机参数与俯仰角**：训练/推理时需要使用相机内参、畸变参数和俯仰角（或估计值），在无法获得精确标定的场景中性能可能受限。
- **训练数据仍为透视图像**：虽然能零样本泛化到大 FoV，但实验显示训练数据的俯仰角覆盖和图像质量对泛化影响很大；对于极端大 FoV 场景（如>180° 或超广角）的泛化上限未充分验证。
- **缺少与 360 度专门模型（如 PanoFormer、OmniFusion）的直接对比**，这些模型虽针对特定域，但可作为更强基线。
- **消融实验报告不完整**：部分消融只列出 \(\delta_1\) 和 A.Rel，未展示全部指标（如 RMSE、log10）；补充材料中的完整结果未纳入主文分析。
- **未讨论推理效率、模型大小、实时性**，对实际部署（如机器人/自动驾驶）的约束考虑不足。

（完）
```
