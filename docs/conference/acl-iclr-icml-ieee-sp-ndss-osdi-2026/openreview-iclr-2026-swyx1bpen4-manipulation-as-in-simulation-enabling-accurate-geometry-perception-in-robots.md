---
title: "Manipulation as in Simulation: Enabling Accurate Geometry Perception in Robots"
title_zh: 仿真般操控：为机器人实现精确几何感知
authors: "Minghuan Liu, Zhengbang Zhu, Xiaoshen Han, PengHu, Haotong Lin, Xinyao Li, Jingxiao Chen, Jiafeng Xu, Yichu Yang, Yunfeng Lin, Xinghang Li, Yong Yu, Weinan Zhang, Tao Kong, Bingyi Kang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=sWyX1BpeN4"
tags: ["query:depth-refine"]
score: 6.0
evidence: 用于深度相机的RGB引导深度增强插件
tldr: 本文针对机器人操作中深度相机精度有限和噪声大的问题，提出了相机深度模型（CDMs）作为日常深度相机的简易插件。CDMs利用RGB图像信息来校正和增强深度图，从而提升机器人对距离、尺寸、形状等几何属性的感知能力。实验表明该方法在多种机器人操作场景下显著提高了成功率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 机器人依赖2D视觉泛化差，深度相机噪声大精度低。
method: 提出Camera Depth Models插件，利用RGB图像提升深度相机精度。
result: 在机器人操作任务中显著提升成功率。
conclusion: CDMs有效增强几何感知，促进机器人操作泛化。
---

## Abstract
Modern robotic manipulation primarily relies on visual observations in a 2D color space for skill learning but suffers from poor generalization. In contrast, humans, living in a 3D world, depend more on physical properties-such as distance, size, and shape-than on texture when interacting with objects. Since such 3D geometric information can be acquired from widely available depth cameras, it appears feasible to endow robots with similar perceptual capabilities. Our pilot study found that using depth cameras for manipulation is challenging, primarily due to their limited accuracy and susceptibility to various types of noise. In this work, we propose Camera Depth Models (CDMs) as a simple plugin on daily-use depth cameras, which take RGB images and raw depth signals as input and output denoised, accurate metric depth. To achieve this, we develop a neural data engine that generates high-quality paired data from simulation by modeling a depth camera's noise pattern. Our results show that CDMs achieve nearly simulation-level accuracy in depth prediction, effectively bridging the sim-to-real gap for manipulation tasks. Notably, our experiments demonstrate, for the first time, that a policy trained on raw simulated depth, without the need for adding noise or real-world fine-tuning, generalizes seamlessly to real-world robots on two challenging long-horizon tasks involving articulated, reflective, and slender objects, with little to no performance degradation. We hope our findings will inspire future research in utilizing simulation data and 3D information in general robot policies. We release the dataset, models for various depth cameras, along with an easy-to-use guide for sim-to-real transfer at https://manipulation-as-in-simulation.github.io/.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现代机器人操作主要依赖2D彩色空间视觉信息进行技能学习，但泛化能力差。相比之下，人类在三维世界中更依赖距离、大小、形状等几何属性而非纹理。虽然深度相机可以获取3D几何信息，但日常使用的深度相机存在精度有限、噪声类型多样的问题，导致直接将其用于机器人操作很困难。
- **整体含义**：本文旨在解决深度相机噪声大、精度低的问题，实现接近仿真级别的精确几何感知，从而打通从仿真到真实世界机器人操作的泛化路径。

## 2. 方法论
- **核心思想**：将深度相机视为可校正的传感器，利用RGB图像中丰富的纹理信息来辅助校正深度图，提出Camera Depth Models（CDMs），作为日常深度相机的轻量插件。
- **关键技术细节**：
  - **输入输出**：输入RGB图像和原始深度信号，输出去噪后的精确度量深度图。
  - **数据生成**：构建神经数据引擎（neural data engine），在仿真环境中通过建模真实深度相机的噪声模式，生成高质量的RGB-D配对数据（仿真深度 + 对应真实噪声模式）。
  - **训练**：使用监督学习训练CDMs，使其学习从带噪声的原始深度+RGB到干净精确深度图的映射，无需真实世界标注。
- **流程说明**：
  1. 在仿真中部署深度相机，通过噪声模型模拟真实传感器的噪声。
  2. 利用仿真环境输出的完美深度图，结合噪声模型生成带噪声的深度图，同时保存RGB图，构成训练数据对。
  3. 训练CDMs网络，输入RGB+含噪深度，输出干净深度。
  4. 实际使用时，将CDMs插入现有深度相机管道，实时输出校正后的深度图。
  5. 机器人策略直接在仿真中生成的干净深度图上训练，然后部署到真实设备，无需额外微调。

## 3. 实验设计
- **数据集/场景**：
  - 在仿真环境中生成了大量配对数据用于训练CDMs（具体规模未在摘要中明示）。
  - 真实测试场景：两个具有挑战性的长程操作任务（涉及铰接件、反光物体、细长物体）。
- **Benchmark**：未提及标准公开benchmark，属于自建任务测试。
- **对比方法**：未明确列出对比基线，但隐含对比了直接使用原始深度相机、传统滤波方法、以及需要真实微调的策略。

## 4. 资源与算力
- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到训练了多个相机型号的CDMs并发布了模型。

## 5. 实验数量与充分性
- **实验数量**：主要呈现了在两个真实长程任务上的实验结果，以及消融实验（可能是对比有无CDMs、噪声注入等），但具体组数未详细展开。
- **充分性评价**：
  - **优点**：首次实现了在原始仿真深度（不添加噪声、不做真实微调）上训练的策略直接零差距迁移到真实世界，这一结果具有较强说服力。
  - **不足**：仅在两个特定任务上验证（涉及铰接、反光、细长物体），类型有限，缺乏大规模多样化场景（如厨房、工业装配）的测试；未与多种方法（如深度补全网络、传统滤波）进行严谨量化对比；对真实相机噪声建模的准确性依赖于仿真假设，可能无法覆盖所有真实噪声模式。

## 6. 主要结论与发现
- CDMs作为简单插件能显著提升日常深度相机的精度，达到接近仿真级别的度量深度预测能力。
- 使用CDMs校正后的深度图，可以使基于原始仿真深度训练的策略直接泛化到真实世界，在挑战性任务上性能几乎不降。
- 研究证明了利用仿真数据和3D信息构建通用机器人策略的可行性，为后续工作提供了新思路。

## 7. 优点
- **方法简单有效**：插件式设计，易于集成到现有机器人系统。
- **数据驱动**：无需真实标注，完全由仿真噪声模型生成训练数据，成本低且可扩展。
- **Sim-to-Real零差距**：首次实现了无需噪声注入或真实微调的直接迁移，简化了机器人部署流程。
- **开源**：发布了数据集、多种相机型号的模型及易用指南，促进复现和后续研究。

## 8. 不足与局限
- **实验覆盖有限**：仅在两个任务上验证，且任务类型集中在特定物体（铰接、反光、细长），对非刚体、柔软物体、透明物体等复杂情况未讨论。
- **噪声模型依赖性**：CDMs的效能高度依赖仿真中噪声模型的逼真度，若真实相机的噪声特性与建模不符，可能降低校正效果。
- **偏差风险**：训练数据完全来自仿真，可能存在材质、光照等方面与真实世界的分布偏差，对极端环境（如强光、暗光）的鲁棒性未知。
- **应用限制**：仅适用于日常深度相机（如RealSense等），对于激光雷达或立体视觉等其他深度传感器未涉及；长程任务中策略的泛化性需要更多测试。

（完）
