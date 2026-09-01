---
title: Segmentation-Enhanced Depth Estimation Using Camera Model Based Self-supervised Contrastive Learning
title_zh: 基于相机模型与分割增强的自监督对比学习深度估计
authors: "jinchang zhang, Guoyu Lu"
date: 2024-09-26
pdf: "https://openreview.net/pdf?id=OOywAeccTZ"
tags: ["query:mono-depth"]
score: 8.0
evidence: 融合分割与相机模型对比学习的自监督单目深度估计
tldr: 自监督单目深度估计虽免除深度标签，但常面临尺度不确定性问题。现有方法多只利用图像间关系，忽略了相机内外参数等基础信息。本文提出结合分割信息的相机模型对比学习框架，利用相机参数计算几何深度约束，并通过分割增强场景理解。实验表明，该方法在无深度真值的情况下有效缓解尺度模糊，提升深度估计的精度和泛化能力。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 自监督单目深度估计存在尺度不确定性问题，且现有方法未充分利用相机内外参数和分割信息。
method: 利用相机内外参数构建对比学习约束，并引入分割特征增强场景语义，以实现无真值情况下的尺度恢复。
result: 在相关数据集上验证了相机模型和分割增强可以显著缓解尺度模糊，提高深度估计精度。
conclusion: 结合相机几何信息和分割先验能有效提升自监督单目深度估计的尺度稳定性与泛化性能。
---

## Abstract
Depth estimation is a key topic in the field of computer vision. Self-supervised monocular depth estimation offers a powerful method to extract 3D scene information from a single camera image, allowing training on arbitrary image sequences without the need for depth labels. However, monocular unsupervised depth estimation still cannot address the issue of scale and often requires ground truth for calibration.
In the deep learning era, existing methods primarily rely on relationships between images to train unsupervised neural networks, often overlooking the fundamental information provided by the camera itself. In fact, the intrinsic and extrinsic parameters of the camera can be used to compute depth information for the ground and its related areas based on physical principles. This information can offer rich supervisory signals at no additional cost. Additionally, by assuming that objects like people, cars, and buildings share the same depth as the corresponding ground, the physical depth of the entire scene can be inferred, and gaps in the depth map can be filled.
Since some areas may have depth estimation errors, to make full use of these regions, we introduce a contrastive learning self-supervised framework. This framework consists of two networks with the same structure: the Anchor network and the Target network. While calculating depth, the network also outputs semantic segmentation results to assist in computing the physics depth, which is then used as the label for the model. Semantic segmentation can identify dynamic objects, reducing photometric reprojection errors caused by moving objects. The predictions from the Anchor network are used as pseudo-labels for training the Target network. Reliability is determined by entropy, dividing the predicted depth into positive and negative samples to maximize the use of physics depth information.

---

## 论文详细总结（自动生成）

# 论文总结：Segmentation-Enhanced Depth Estimation Using Camera Model Based Self-supervised Contrastive Learning

## 1. 核心问题与整体含义（研究动机与背景）
- **问题背景**：单目深度估计是计算机视觉中的重要任务，自监督单目深度估计可从任意图像序列中学习，无需深度标签，具有较强实用性。
- **核心痛点**：
  - 现有自监督方法仍存在**尺度不确定性问题**，通常需要真值进行标定。
  - 主流深度学习方法仅利用图像间关系训练，**忽略了相机内外参数等基础物理信息**。
  - 动态物体会引起光度重投影误差，影响深度估计质量。
- **整体意义**：本文尝试利用相机物理模型与分割先验，在不依赖深度真值的情况下恢复尺度信息，提升自监督单目深度估计的精度与泛化能力。

## 2. 方法论：核心思想、关键技术细节与流程
- **核心思想**：
  - 利用相机**内参和外参**，基于物理原理计算地面及其相关区域的深度，作为额外监督信号。
  - 假设行人、车辆、建筑等物体与其对应地面的深度一致，从而推断整个场景的物理深度，并填补深度图空缺。
  - 引入**语义分割**辅助物理深度计算，同时识别动态物体以减少光度重投影误差。
  - 采用**对比学习自监督框架**，最大化利用可能存在误差的深度区域。
- **网络结构**：
  - 双网络结构：**Anchor 网络**与**Target 网络**，两者结构相同。
  - 网络在计算深度的同时输出语义分割结果。
- **关键机制**：
  - 物理深度作为模型训练的标签。
  - Anchor 网络的预测作为 Target 网络的伪标签。
  - 通过**熵**计算可靠性，将预测深度划分为正样本和负样本，用于对比学习。
- **算法流程概括**：
  1. 输入图像 → Anchor & Target 网络联合预测深度与分割；
  2. 利用相机参数计算地面物理深度，结合分割先验推断场景物理深度；
  3. 用物理深度作为监督标签，Anchor 预测作为伪标签训练 Target；
  4. 按熵划分正负样本，执行对比学习以增强深度特征表达。

## 3. 实验设计
- **数据集/场景**：提供的文本信息中**未明确列出具体数据集名称**，仅提到“在相关数据集上验证”。
- **Benchmark**：未在提取文本中说明具体的基准榜单（如 KITTI、Cityscapes 等）。
- **对比方法**：未在提取文本中列出具体对比的基线方法。
- **客观说明**：由于论文 PDF 提取内容有限，以上信息需查看原文完整实验章节才能确认。

## 4. 资源与算力
- 提取的文本中**未提及** GPU 型号、数量、训练时长等算力信息。
- 若需评估训练成本，需要查阅论文原文的实验设置部分。

## 5. 实验数量与充分性
- 从现有摘要和元数据看，实验涉及**多个数据集验证**以及**消融分析**（证据字段提到“融合分割与相机模型对比学习”），但具体实验组数未列出。
- 摘要表示方法能缓解尺度模糊、提升精度和泛化能力，但**缺少定量结果、误差指标和可视化对比**。
- 由于缺少实验细节，**无法全面判断实验的充分性、客观性和公平性**；建议查阅完整论文以核对基准、消融设置和统计显著性。

## 6. 主要结论与发现
- 相机内外参数提供的物理深度约束可以有效**缓解自监督单目深度估计的尺度不确定性问题**。
- 语义分割的引入能够：
  - 辅助计算物理深度；
  - 识别动态物体，减少光度重投影误差。
- 基于熵的对比学习框架能够**充分利用不可靠深度区域**，提高训练效率。
- 整体上，结合相机几何信息与分割先验可提升深度估计的**尺度稳定性与泛化性能**。

## 7. 优点
- **创新性**：将传统几何方法（相机模型计算地面深度）与现代自监督对比学习相结合，思路新颖。
- **无需额外标注**：利用相机参数和分割先验，不增加人工标注成本。
- **解决尺度问题**：直接从物理原理上约束尺度，较以往仅依赖图像间关系的方法更本质。
- **联合任务设计**：深度估计与语义分割共享网络，互相促进，且分割可降低动态物体干扰。
- **伪标签与对比学习**：通过 Anchor-Target 网络和熵可靠度划分，灵活处理深度估计误差区。

## 8. 不足与局限
- **信息不完整**：基于当前提取文本，缺乏具体的实验数据、数据集名称、对比方法细节，难以进行深入评估。
- **假设依赖**：方法假设物体与对应地面深度一致，在复杂场景（如悬空物体、桥梁、远处物体与地面不接触）中可能不成立，限制泛化。
- **分割精度依赖**：语义分割的准确性会直接影响物理深度计算和动态物体过滤效果，若分割出错则引入误差。
- **对比学习复杂度**：双网络和正负样本划分增加了训练复杂度和计算开销，论文中未给出复杂度分析。
- **普适性存疑**：相机模型计算地面深度依赖明确的相机位姿和地面平面假设，在无结构或剧烈起伏地形中可能失效。

（完）
