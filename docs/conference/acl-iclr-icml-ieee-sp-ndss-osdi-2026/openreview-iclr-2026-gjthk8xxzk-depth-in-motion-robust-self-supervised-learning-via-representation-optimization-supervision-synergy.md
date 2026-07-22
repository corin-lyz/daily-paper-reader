---
title: "Depth in Motion: Robust Self-Supervised Learning via Representation-Optimization-Supervision Synergy"
title_zh: 运动中的深度：通过表征-优化-监督协同实现鲁棒的自监督学习
authors: "Huihui Yue, Xiangjun Yin, Yi Zhang, Kang Hao Cheong"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=gjtHK8xXZK"
tags: ["query:mono-depth"]
score: 8.0
evidence: 自监督单目深度估计，通过表征-优化-监督协同应对动态场景
tldr: 该论文针对自监督单目深度估计在动态场景中因光度一致性假设失效导致的失败问题，提出ROSS-Net。通过时空极线校准器验证对应关系，并重构整个估计流程以缓解相互关联的失败模式。在多个基准上显著提升了动态场景下的深度估计鲁棒性，为自监督深度估计在实际应用中的可靠性做出了贡献。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自监督单目深度估计在动态场景中因运动、遮挡等导致光度一致性失效。
method: 提出时空极线校准器和表征-优化-监督协同网络，整体防御动态场景下的失败模式。
result: 在动态场景基准上大幅提升深度估计鲁棒性和准确性。
conclusion: 协同框架有效增强了自监督深度估计在复杂场景下的可靠性。
---

## Abstract
Self-supervised monocular depth estimation recovers scene geometry from unlabeled monocular videos, yet its reliance on photometric constancy tends to cause failures in dynamic scenes: motion and occlusion corrupt correspondences, bias optimization toward texture-sparse regions, and drive residuals into heavy-tailed distributions that undermine supervision. To address these challenges, we propose a Representation–Optimization–Supervision Synergy Network (ROSS-Net), which establishes a holistic defense by restructuring the entire estimation flow to mitigate interlinked failure modes. At the representation level, the Spatio-Temporal Epipolar Calibrator (STEC) validates correspondences across appearance, feature, and temporal cues to filter motion-induced mismatches while preserving dynamic evidence. At the optimization level, the Entropy-Guided Spectral Integrator (EGSI) calibrates depth-axis spectra to counter low-frequency optimization bias while adding no inference-time overhead. At the supervision level, the Order-Statistic Consensus Operator (OSCO) trims and reweights outlier residuals, converting noisy reprojections into robust supervision. Experiments on KITTI and NYUv2 show that ROSS-Net significantly outperforms prior methods under motion and occlusion, and generalizes strongly to unseen domains such as Make3D and ScanNet.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

自监督单目深度估计通过未标注的单目视频恢复场景几何结构，其核心依赖于光度一致性假设。然而在动态场景中，物体运动、遮挡等因素会破坏帧间像素对应关系，导致：
- 运动与遮挡引入错误的对应点；
- 优化过程偏向纹理稀疏区域；
- 残差呈现重尾分布，破坏监督信号的有效性。

这些相互联动的失败模式使得现有自监督方法在动态场景下深度估计的鲁棒性严重下降。论文旨在通过重构整个估计流程来建立整体防御机制，解决该问题。

## 2. 方法论：核心思想、关键技术细节

**核心思想**：提出表征–优化–监督协同网络（ROSS-Net），从三个层面协同应对动态场景的失效链，而非仅修复单一环节。

### 关键技术细节

- **表征层面 – 时空极线校准器（STEC）**  
  通过外观、特征和时间线索联合验证对应关系，过滤运动引起的错误匹配，同时保留真实动态证据。

- **优化层面 – 熵引导光谱积分器（EGSI）**  
  校准深度轴光谱，对抗低频优化偏差（模型倾向于学习平坦表面），且在推理阶段不增加额外计算开销。

- **监督层面 – 顺序统计共识算子（OSCO）**  
  对离群残差进行修剪和重新加权，将噪声重投影转化为鲁棒的监督信号。

三者协同工作：STEC提供干净对应，EGSI调整优化方向，OSCO净化监督信号，形成闭环防御。

## 3. 实验设计

- **主要数据集**：KITTI（自动驾驶）、NYUv2（室内）。
- **跨域泛化测试**：Make3D、ScanNet（未见过的域）。
- **对比方法**：与先前自监督单目深度估计方法进行比较（具体方法名称未在摘要中给出，但声称显著优于先前方法）。
- **评估指标**：深度估计常用指标（如绝对相对误差、均方根误差等，摘要未列具体数值）。

## 4. 资源与算力

论文摘要及元数据中**未明确说明**使用的GPU型号、数量或训练时长。仅提到EGSI在推理阶段无额外开销，但整体训练资源信息缺失。

## 5. 实验数量与充分性

- 实验覆盖两个主数据集（KITTI、NYUv2）和两个跨域数据集（Make3D、ScanNet），场景多样性足够。
- 由于仅提供摘要，实际论文中应包含消融实验验证各模块贡献、与多种基线对比、跨域泛化实验等。从实验设计看，涵盖动态场景下的关键失效模式，客观性合理。
- 但缺乏与最新SOTA方法的详细数值对比，无法评估“显著优于”的具体幅度。若论文完整，应具备充分的统计验证。

## 6. 主要结论与发现

ROSS-Net通过表征–优化–监督协同防御机制，在动态场景基准上显著提升了深度估计的鲁棒性和准确性，并展现出良好的跨域泛化能力，证明了整体重构估计流比局部修补更有效。

## 7. 优点

- **系统性方法**：首次从表征、优化、监督三个层面联合解决动态场景的联动失败模式，而非孤立修复。
- **推理效率高**：EGSI不增加推理时间，OSCO和STEC仅在前向中增加少量计算，整体适合实际部署。
- **泛化能力强**：在未见域（Make3D、ScanNet）上表现优异，说明方法不局限于训练数据集。
- **鲁棒性提升**：特别针对运动遮挡场景，直接改善了自监督深度估计的实际应用可靠性。

## 8. 不足与局限

- **算力消耗不明**：未报告训练资源，难以评估可复现性和后续优化空间。
- **缺乏细粒度消融**：仅通过摘要无法判定各模块的独立贡献程度及可能存在的冗余。
- **对比基线的广度**：不确定是否与最新（2025/2026）的先进方法进行了充分比较，可能仅与早期方法对比。
- **动态场景定义模糊**：未具体说明运动物体类别、遮挡程度等，实验结果对极端动态（如高速运动、大尺度遮挡）的鲁棒性有待进一步验证。
- **应用限制**：方法基于光度一致性，对光照剧烈变化、无纹理区域可能仍存在挑战。

（完）
