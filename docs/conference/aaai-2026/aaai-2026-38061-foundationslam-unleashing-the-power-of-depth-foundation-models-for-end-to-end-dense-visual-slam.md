---
title: "FoundationSLAM: Unleashing the Power of Depth Foundation Models for End-to-End Dense Visual SLAM"
title_zh: "FoundationSLAM: 释放深度基础模型在端到端稠密视觉SLAM中的威力"
authors: "Yuchen Wu, Jiahe Li, Fabio Tosi, Matteo Poggi, Jin Zheng, Xiao Bai"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38061/42023"
tags: ["query:mono-depth"]
score: 9.0
evidence: 利用深度基础模型进行单目深度和位姿估计
tldr: 现有基于光流的单目SLAM方法缺乏几何一致性，导致跟踪和建图精度有限。FoundationSLAM提出利用深度基础模型引导几何推理，通过混合光流网络产生几何感知对应，并引入双一致光束平差层联合优化位姿与深度，实现鲁棒且稠密的SLAM。实验表明该方法在多个基准上取得了领先的准确性和鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 618, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1818, \"height\": 626, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 1016, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1813, \"height\": 730, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 555, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38061/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 882, \"height\": 457, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 826, \"height\": 259, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1830, \"height\": 508, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1829, \"height\": 565, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 877, \"height\": 423, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 882, \"height\": 529, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 876, \"height\": 141, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38061/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 882, \"height\": 432, \"label\": \"Table\"}]"
motivation: 现有的基于光流的单目稠密SLAM方法缺乏几何一致性，导致跟踪和建图精度受限。
method: 提出混合光流网络生成几何感知对应，并设计双一致光束平差层联合优化多视角下的位姿与深度。
result: 在多个SLAM基准上实现了领先的跟踪和建图精度。
conclusion: 利用深度基础模型有效提升了单目稠密SLAM的几何一致性和鲁棒性。
---

## Abstract
We present FoundationSLAM, a learning-based monocular dense SLAM system that addresses the absence of geometric consistency in previous flow-based approaches for accurate and robust tracking and mapping.
Our core idea is to bridge flow estimation with geometric reasoning by leveraging the guidance from foundation depth models. 
To this end, we first develop a Hybrid Flow Network that produces geometry-aware correspondences, enabling consistent depth and pose inference across diverse keyframes. 
To enforce global consistency, we propose a Bi-Consistent Bundle Adjustment Layer that jointly optimizes keyframe pose and depth under multi-view constraints. Furthermore, we introduce a Reliability-Aware Refinement mechanism that dynamically adapts the flow update process by distinguishing between reliable and uncertain regions, forming a closed feedback loop between matching and optimization.
Extensive experiments demonstrate that FoundationSLAM achieves superior trajectory accuracy and dense reconstruction quality across multiple challenging datasets, while running in real-time at 18 FPS, demonstrating strong generalization to various scenarios and practical applicability of our method.

---

## 论文详细总结（自动生成）

# 论文总结：FoundationSLAM

## 1. 核心问题与整体含义（研究动机和背景）

现有基于光流的单目稠密SLAM系统（如DROID-SLAM）虽然在许多场景取得了良好效果，但存在**缺乏几何一致性**的根本缺陷：光流估计仅在2D图像空间进行，不感知底层场景几何结构，导致在无纹理、重复纹理、反射等困难区域出现不一致的匹配；同时，优化过程未显式施加多视图几何约束，累积误差会降低位姿精度和重建质量。作者提出**利用深度基础模型（foundation depth models）的强几何先验来引导光流估计**，并设计双向一致性优化框架，从而提升SLAM的几何一致性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

**核心思想**：将几何先验嵌入到光流估计与联合优化中，形成“感知→优化→反馈”的闭环系统。三个关键组件：

- **Hybrid Flow Network（混合光流网络）**  
  双分支架构：  
  - 几何先验分支（冻结的FoundationStereo的FeatureNet）提供稳定几何特征；  
  - 任务适应分支（可训练的CNN）适配SLAM场景。  
  特征融合后输入Flow GRU迭代预测光流更新和置信度。

- **Bi-Consistent Bundle Adjustment Layer（双一致光束平差层）**  
  联立两个残差：  
  - 流一致残差 \(L_{\text{flow}} = \|u_{\text{proj}} - (u_i + F_{i\to j})\|_1\)；  
  - 几何一致残差 \(L_{\text{geo}} = \|u_{\text{back}}^i - u_i\|\)，通过双向投影确保多视图几何一致。  
  最终损失 \(L_{\text{BA}} = \sum \omega L_{\text{flow}} + (1-\omega)L_{\text{geo}}\)，采用Gauss-Newton联合优化深度和位姿。

- **Reliability-Aware Refinement（可靠性感知精化）**  
  构建像素级可靠性掩码 \(M_i(u) = M_{\text{edge}} \cdot M_{\text{node}}\)：  
  - 边缘可靠性 \(M_{\text{edge}}\)：基于前向投影残差（阈值 \(\tau_{\text{edge}}=5\)）；  
  - 节点可靠性 \(M_{\text{node}}\)：基于多视图几何残差均值（阈值 \(\tau_{\text{node}}=5\)）。  
  可靠区域：使用局部相关体积进行高效精化；不可靠区域：屏蔽相关特征，仅依赖几何上下文进行修正。

**算法流程**（以迭代t为例）：  
1. 构建4D相关体积；  
2. 乘以可靠性掩码；  
3. 特征聚合后输入Flow GRU预测光流更新；  
4. 更新光流；  
5. 执行两次BA优化（更新深度和位姿）；  
6. 重复多轮直至收敛。

## 3. 实验设计

- **数据集**：TUM-RGBD（9个序列）、EuRoC MAV（11个序列）、ETH3D-SLAM、7Scenes、TNT（定性展示）。  
- **Benchmark**：跟踪精度使用ATE RMSE；建图质量使用Accuracy（准确度）、Completion（完整度）、Chamfer Distance（倒角距离）。  
- **对比方法**：ORB-SLAM3、DeepV2D、DeepFactors、DPV-SLAM、DPV-SLAM++、GO-SLAM、DROID-SLAM、VGGT-SLAM、MASt3R-SLAM。涵盖经典稀疏、匹配密集、NeRF/3DGS混合、以及基于基础模型的最新方法。

## 4. 资源与算力

- **训练**：8块NVIDIA RTX 4090 GPU，batch size=8，训练300K步（约5天），使用AdamW优化器及OneCycleLR调度。  
- **测试**：单块RTX 4090，输入分辨率512×384，运行速度约18 FPS（采用ViT-S作为backbone，基础模型编码在半分辨率下运行）。

## 5. 实验数量与充分性

共进行了**3组主要实验**：
- **跟踪评估**：在TUM-RGBD（9序列）、EuRoC（11序列）、ETH3D-SLAM上进行ATE比较，覆盖多种难度场景（快速运动、低纹理、反射、动态模糊）。  
- **建图评估**：在7Scenes（seq-01）和EuRoC（VICON房间）上对比Accuracy、Completion、Chamfer Distance。  
- **消融实验**：在EuRoC上分析了3个组件（Bi-BA、Node-geo、Edge-flow）的贡献，共6组对比。  
此外还有定性对比（TNT数据集）和速度对比。实验设计较为全面，对比方法覆盖主流范式，且消融实验验证了每个模块的有效性。但建图评估仅选用了部分序列，未在全数据集上进行，可能存在一定偏差。整体充分、客观。

## 6. 主要结论与发现

- FoundationSLAM在**四个基准的跟踪精度**上均达到或超过SOTA，尤其在TUM-RGBD的7/9序列、EuRoC全序列中取得最佳ATE。  
- **建图质量**在7Scenes和EuRoC上均优于DROID-SLAM、MASt3R-SLAM和VGGT-SLAM，Chamfer距离最低。  
- 消融实验表明：Bi-Consistent BA层显著提升几何一致性；可靠性感知精化在反射、低纹理等挑战区域有效改善光流和重建。  
- 系统在保证实时性（18 FPS）的同时实现了高性能，证明了深度先验与闭环优化的实用性。

## 7. 优点

- **创新性**：首次将深度基础模型（FoundationStereo）的几何先验以双分支融合方式嵌入SLAM光流网络，并设计双向一致性BA和可靠性反馈闭环，形成紧密耦合的端到端框架。  
- **实验严谨**：对比方法多样，消融实验完整，定量+定性结合，尤其可视化展示了光流精化效果和几何一致性提升。  
- **效率与性能平衡**：使用ViT-S和半分辨率编码，在保持高精度的同时达到18 FPS实时运行，具备实际部署潜力。  
- **泛化能力**：在跨域（TartanAir训练→真实世界测试）、灰度图像（EuRoC）、快速运动等条件下均表现优异。

## 8. 不足与局限

- **未明确讨论局限性**：论文未设置“Limitations”部分，可能忽略以下问题——  
  - 对预训练深度模型（FoundationStereo）的强依赖：若测试场景与训练数据分布差异巨大（如极端光照、缺乏纹理），先验可能失效。  
  - 建图评估仅选用了2个数据集的少量序列，未在全部序列（如TUM的完整场景）上评价，结论泛化性有待验证。  
  - 无动态场景处理机制：未讨论移动物体对光流和BA的影响。  
  - 仅支持单目/RGB-D输入，未扩展到立体或惯性SLAM。  
  - 消融实验仅在EuRoC上进行，未在TUM或ETH3D上验证，可能遗漏不同场景下的交互效应。  
- **实验偏差风险**：训练数据TartanAir为合成数据，虽然泛化到真实场景，但合成与真实的域差距可能影响部分真实场景性能（如TUM中某些序列ATE不如MASt3R-SLAM）。  
- **计算资源较高**：需要8×4090训练5天，对普通研究团体有一定门槛。

（完）
