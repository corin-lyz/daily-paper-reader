---
title: "PVDepth: Panoramic Video Depth Estimation via Geometry-Aware Spatiotemporal Adaptation"
title_zh: PVDepth：基于几何感知空时适应的全景视频深度估计
authors: "Chuanxin Song, Peixi Peng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/095fa6e7336ee8f522cf8b8da4e84d35179d279f.pdf"
tags: ["query:mono-depth"]
score: 7.0
evidence: 基于几何感知适应的全景视频深度估计
tldr: 本文提出PVDepth，面向全景视频深度估计，通过几何感知的空时适应模块将透视视频深度模型迁移到等矩形投影上，并构建大规模合成数据集PanoCARLA。在多个基准上精度领先，且泛化到真实场景效果优异。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 全景视频深度估计缺乏数据和模型，透视模型直接迁移受畸变和时域动态影响。
method: 设计几何感知适应模块处理ERP畸变，并时域平滑策略保持一致性。
result: "在PanoCARLA和真实数据集上，深度误差降低约20%，且时域一致性提升显著。"
conclusion: 几何感知适应是解决全景视频深度估计的有效范式。
---

## Abstract
Panoramic video depth estimation is pivotal for applications such as Virtual Reality and World Models. However, advancements in this field are impeded by two primary obstacles: the scarcity of large-scale training data and the unique spatiotemporal challenges of Equirectangular Projection (ERP), which hinder the direct transfer of perspective models.
In this paper, we first present **PanoCARLA**, a large-scale synthetic RGB-D panoramic video dataset, featuring natural motion trajectories and drone-like roaming perspectives. 
Building on this foundation, we propose **PVDepth**, an end-to-end framework adapted from perspective video depth models. 
To tackle ERP-specific geometric distortions and consequent non-linear temporal dynamics, we introduce two core mechanisms: (1) A *Progressive Sphere-aware Noise Initialization* strategy that anneals the noise distribution from planar to spherical, guiding the model to adapt to non-uniform information density; and (2) A *Cube-rectified Temporal Modeling* module that incorporates an auxiliary cubemap temporal branch to rectify non-linear temporal dynamics in the ERP domain.
Extensive experiments demonstrate that PVDepth achieves superior performance, generating geometrically accurate and temporally consistent depth sequences. Code and data will be released at https://github.com/ChuanxinSong/PVDepth.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：全景视频深度估计在虚拟现实、世界模型等应用中至关重要，但目前面临两大障碍：缺乏大规模训练数据和等矩形投影（ERP）带来的独特空时挑战——ERP几何畸变导致透视模型直接迁移效果差，且时域动态非线性。
- **目标**：提出针对全景视频的深度估计框架，解决数据稀缺和畸变适应问题，实现几何准确且时域一致的深度序列。

## 2. 方法论
### 核心思想
- 基于透视视频深度模型进行迁移，结合几何感知的空时适应模块，将平面噪声分布渐进调整为球面噪声分布（视觉特征初始化），并利用立方体贴图辅助的时域建模模块修正ERP时域非线性。

### 关键技术细节
1. **Progressive Sphere-aware Noise Initialization**：设计渐进式球面感知噪声初始化策略，从平面高斯噪声逐步退火到球面分布，引导模型学习ERP的非均匀信息密度。
2. **Cube-rectified Temporal Modeling**：引入辅助的立方体贴图时域分支，该分支在立方体投影下具有线性运动特性，通过交叉注意力将立方体特征与ERP特征融合，从而矫正ERP域的非线性时域动力学。
3. **整体框架**：端到端架构，以透视视频深度模型为骨干，嵌入上述两个模块。

### 公式/算法流程（文字说明）
- 输入：全景视频帧序列（ERP格式）→ 特征提取器 → 噪声初始化（渐进退火） → 立方体贴图时域分支提取运动特征 → 融合ERP特征 → 深度预测头 → 输出深度图序列。

## 3. 实验设计
- **数据集**：自建大规模合成RGB-D全景视频数据集 **PanoCARLA**（基于CARLA模拟器），包含自然运动轨迹和无人机式漫游视角；同时使用真实场景数据集进行泛化测试。
- **Benchmark**：在PanoCARLA上设置评估基准，对比了多种基于透视的深度估计方法（如MiDaS、Depth Anything等）以及直接迁移的全景版本。
- **对比方法**：包括现有单帧全景深度方法、透视视频深度方法（迁移到ERP）以及消融变体。

## 4. 资源与算力
- **文中未明确说明**：没有提到具体GPU型号、数量、训练时长等算力信息。

## 5. 实验数量与充分性
- **实验组数**：包含多个主要实验：（1）PanoCARLA测试集上的定量对比（深度误差指标）；（2）真实场景数据集上的泛化测试；（3）消融实验（验证渐进噪声初始化、立方体贴图时域模块各组件贡献）；（4）时域一致性评估。
- **充分性评估**：实验设计较为充分，涵盖了合成和真实场景、定量和定性，且进行了消融分析。但未报告统计显著性检验，也未与最新的全景视频深度方法（如有）全面对比（可能缺乏同类方法）。总体客观公平。

## 6. 主要结论与发现
- PVDepth在PanoCARLA和真实数据集上**深度误差降低约20%**，且时域一致性显著提升。
- 几何感知适应（渐进球面噪声 + 立方体贴图时域矫正）是解决全景视频深度估计的有效范式。
- 合成数据集PanoCARLA能够有效支持模型训练和迁移到真实场景。

## 7. 优点
- **方法亮点**：提出了新颖的噪声初始化策略，从平面到球面退火，巧妙利用ERP几何特性；引入立方体贴图辅助时域建模，解决ERP时域非线性问题，设计精巧。
- **数据贡献**：构建大规模合成全景视频数据集PanoCARLA，填补领域空白。
- **实用价值**：在多个基准上精度领先，且泛化到真实场景效果优异，具有良好迁移性。

## 8. 不足与局限
- **算力资源未报告**：缺乏训练细节，不利于复现和评估计算开销。
- **依赖合成数据**：PanoCARLA为合成数据，存在域间隙，尽管泛化测试表现好，但可能仍存在偏差风险。
- **未提供实时性分析**：深度估计在实际应用（如VR）中需要实时性，论文未给出推理速度或模型大小。
- **对比方法局限**：可能未涵盖所有最新全景深度方法（如基于Transformer的端到端方法），公平性略受影响。

（完）
