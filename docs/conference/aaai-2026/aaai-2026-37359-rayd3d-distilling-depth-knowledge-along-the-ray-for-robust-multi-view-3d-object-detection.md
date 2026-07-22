---
title: "RayD3D: Distilling Depth Knowledge Along the Ray for Robust Multi-View 3D Object Detection"
title_zh: RayD3D：沿射线方向蒸馏深度知识实现鲁棒多视图3D目标检测
authors: "Rui Ding, Zhaonian Kuang, Zongwei Zhou, Meng Yang, Xinhu Zheng, Gang Hua"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37359/41321"
tags: ["query:lite-vision"]
score: 6.0
evidence: 深度知识蒸馏用于多视图3D检测
tldr: 多视图3D检测深度预测不准，跨模态蒸馏会引入LiDAR密度等无关信息。论文提出RayD3D，沿射线方向蒸馏纯深度知识，避免无关噪声，显著提升深度预测鲁棒性。该方法可迁移到其他像素级任务。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37359/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 880, \"height\": 652, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37359/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1647, \"height\": 404, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37359/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1827, \"height\": 811, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37359/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 863, \"height\": 348, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1758, \"height\": 494, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1680, \"height\": 682, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1689, \"height\": 396, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 839, \"height\": 230, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 831, \"height\": 230, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37359/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 877, \"height\": 189, \"label\": \"Table\"}]"
motivation: 跨模态深度蒸馏会引入与深度无关的LiDAR密度噪声。
method: 沿相机到物体真实位置的射线方向蒸馏纯深度知识。
result: 在多个自动驾驶数据集上深度预测和检测性能提升。
conclusion: 沿射线方向蒸馏可有效传递纯净深度信息，提升多视图3D检测鲁棒性。
---

## Abstract
Multi-view 3D detection with bird’s eye view (BEV) is crucial for autonomous driving and robotics, but its robustness in real-world is limited as it struggles to predict accurate depth values. A mainstream solution, cross-modal distillation, transfers depth information from LiDAR to camera models but also unintentionally transfers depth-irrelevant information (e.g. LiDAR density). To mitigate this issue, we propose RayD3D, which transfers crucial depth knowledge along the ray: a line projecting from the camera to true location of an object. It is based on the fundamental imaging principle that predicted location of this object can only vary along this ray, which is finally determined by predicted depth value. Therefore, distilling along the ray enables more effective depth information transfer. More specifically, we design two ray-based distillation modules. Ray-based Contrastive Distillation (RCD) incorporates contrastive learning into distillation by sampling along the ray to learn how LiDAR accurately locates objects. Ray-based Weighted Distillation (RWD) adaptively adjusts distillation weight based on the ray to minimize the interference of depth-irrelevant information in LiDAR. For validation, we widely apply RayD3D into three representative types of BEV-based models, including BEVDet, BEVDepth4D, and BEVFormer. Our method is trained  on clean NuScenes, and tested on both clean NuScenes and RoboBEV with a variety types of data corruptions. Our method significantly improves the robustness of all the three base models in all scenarios without increasing inference costs, and achieves the best when compared to recently released multi-view and distillation models.

---

## 论文详细总结（自动生成）

# RayD3D：沿射线方向蒸馏深度知识实现鲁棒多视图3D目标检测 详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：多视图3D目标检测（尤其是基于鸟瞰图BEV的方法）在自动驾驶和机器人领域至关重要，但在真实场景中因深度预测不准导致鲁棒性严重下降。例如，当数据受到雾、雪等污染时，深度精度（MATE）从0.72恶化至1.00，导致检测指标NDS从37.20骤降至6.06。
- **现有方案局限**：主流解决方案——跨模态蒸馏（将LiDAR的深度知识迁移至相机模型）虽然有效，但会无意中传递与深度无关的信息（如LiDAR点云密度、反射强度等），反而干扰相机模型自身的定位能力。现有方法（如BEVDistill、DistillBEV等）简单地让相机特征模仿LiDAR特征或要求师生同构，未能有效聚焦纯深度信息。
- **核心动机**：基于成像原理——物体在图像中的预测位置只能沿着相机到物体真实位置的射线（ray）变动，而最终位置由深度值决定。因此，**沿射线方向蒸馏深度知识**，可以更纯粹地传递深度信息，避免无关噪声。

## 2. 方法论

### 2.1 核心思想
- 提出 **RayD3D** 框架，利用射线先验（ray prior）进行跨模态蒸馏。将LiDAR和相机的BEV特征统一在BEV空间中，沿射线进行知识迁移，使得相机模型学习到LiDAR精确的深度定位能力。

### 2.2 关键技术细节

#### (1) 总体框架
- 包含三个组件：LiDAR教师网络（CenterPoint）、相机学生网络（如BEVDet等）、两个射线蒸馏模块。
- 先训练并冻结教师网络，再训练学生网络，推理时仅用学生网络，不增加额外推理成本。
- 将BEV特征图均匀划分为 \(N_{\text{ray}}\) 条射线。

#### (2) Ray-based Contrastive Distillation (RCD)
- **目的**：学习LiDAR如何沿射线区分准确与不准确的位置。
- **正负样本构建**：在每条射线上，正样本为物体前景区域中相机和LiDAR对应位置的特征对；负样本通过高斯采样策略从正样本附近的不准确位置抽取（保证相机特征相似但深度位置不同），并加入教师网络中的负样本以增加多样性。
- **损失函数**：包含学生网络和教师网络的双向对比损失，公式如下：
  \[
  L_{\text{RCD}} = -\log\left( \frac{1}{N_{\text{ray}}} (L_{\text{RCD}}^S + L_{\text{RCD}}^T) \right)
  \]
  其中：
  \[
  L_{\text{RCD}}^S = \sum_{i=1}^{N_{\text{ray}}} \frac{\exp(L_{i,j} \cdot C_{i,j} / \tau)}{\sum_{k=1}^{N_{\text{neg}}+1} \exp(L_{i,j} \cdot C_{i,k} / \tau) + \xi}
  \]
  \(L_{i,j}, C_{i,j}\) 分别代表LiDAR和相机在正样本位置的特征，\(C_{i,k}\) 为负样本，\(\tau\) 温度参数，\(\xi\) 防止分母为零。

#### (3) Ray-based Weighted Distillation (RWD)
- **目的**：根据每条射线上相机与LiDAR特征分布的差异自适应调节蒸馏权重，减少深度无关信息干扰。
- **权重计算**：
  - 生成空间注意力图 \(S_C, S_L\)（对通道平均后softmax）。
  - 计算每条射线i的KL散度：\(A_i = \text{KL}(S_L^i, S_C^i)\)。差异大则增加权重（传递更多深度信息），差异小则降低权重（避免干扰）。
  - 将权重映射到整个BEV网格，并通过对同一物体内所有射线取最大值保证物体内部权重一致。
  - 背景区域乘以缩放因子 \(s\) 以降低影响。
- **损失函数**：
  \[
  L_{\text{RWD}} = \frac{1}{H \times W} \sum_{h,w} \omega[h,w] \cdot |L[h,w] - C[h,w]|
  \]
  其中 \(\omega\) 为权重图。

#### (4) 总损失
- 结合RCD、RWD、学生网络原始检测损失 \(L_{\text{SRC}}\) 以及响应蒸馏损失 \(L_{\text{RES}}\)（使学生输出接近教师）：
  \[
  L = L_{\text{RCD}} + L_{\text{RWD}} + L_{\text{SRC}} + L_{\text{RES}}
  \]

## 3. 实验设计

### 3.1 数据集与场景
- **训练**：NuScenes清洁数据（700训练场景）。
- **测试**：
  - 清洁NuScenes验证集（150场景）和测试集（150场景）。
  - **RoboBEV**（包含8类数据污染：亮度、低光、雾、雪、运动模糊、颜色量化、相机崩溃、帧丢失），以平均鲁棒率 \(mRR\) 评估（\(mRR = \frac{1}{N} \sum (NDS_i / NDS_{\text{clean}})\)）。

### 3.2 Baseline 与对比方法
- **三种代表型BEV模型**：BEVDet（LSS型）、BEVDepth4D（时序型）、BEVFormer（Transformer型），均使用ResNet-50骨干。另含BEVDepth4D-r101（ResNet-101）。
- **对比方法**：
  - 多视图检测模型：PETR、BEVerse、Sparse4D、SoloFusion、DualBEV。
  - 跨模态蒸馏模型：Unidistill、DistillBEV、VexKD、TiG-BEV、LabelDistill。
- **公平性**：教师网络统一使用CenterPoint；所有对比方法在相同设置（FP16、官方代码）下复现结果。

## 4. 资源与算力
- 文中明确说明：训练24 epochs在8×3090 GPU上，训练时间略有增加，但推理时间不变。未提供具体时长或GPU内存消耗。算力规模属于常规学术级（8卡3090）。

## 5. 实验数量与充分性

### 5.1 实验分组
- **主实验**（Table 1）：3个基础模型在清洁nuScenes和RoboBEV上的性能，完整涵盖8种污染类型。
- **与最新模型对比**（Table 2）：与7种多视图模型和6种蒸馏模型对比，结果包含NDS/mAP和mRR。
- **训练+测试均带污染**（Table 3）：模拟8种污染训练后测试RoboBEV，验证对“可见”污染的鲁棒性。
- **消融实验**（Table 4-6）：
  - RCD和RWD模块的独立和联合效果。
  - RCD中负样本采样策略（随机 vs 高斯 vs 在教师中采样）。
  - RWD中特征差异计算策略（余弦相似度、JS散度、KL散度）。

### 5.2 充分性与客观性
- **优点**：实验覆盖了主流BEV检测类型，污染种类全面（8种），对比方法涵盖近年代表性工作，消融实验验证了各模块必要性。公平性方面：设置相同骨干、图像分辨率、训练超参数，且对比结果直接引用RoboBEV官方或复现。
- **不足**：仅使用NuScenes单一数据集（文中提及未来扩展到跨城市设置），未在Waymo等更大规模数据集上验证。教师网络仅使用CenterPoint（未对比不同LiDAR检测器）。随机种子固定为0，方差声明<0.2 NDS，但未提供多轮重复实验统计。

## 6. 主要结论与发现
- **性能提升**：在清洁nuScenes上，NDS/mAP提升3~6%（如BEVDet从37.4/29.6升至41.9/32.7）；在RoboBEV中，平均鲁棒率mRR提升2~6%。
- **泛化性**：方法适用于三种不同类型的BEV模型，无论是否显式预测深度（如BEVFormer无显式深度）。
- **对比优势**：超越所有现有的跨模态蒸馏方法（包括使用LiDAR+相机融合教师的Unidistill、VexKD），且仅使用纯LiDAR教师。
- **关键洞察**：沿射线方向蒸馏能有效过滤深度无关信息，对比学习比简单特征模仿更有效。

## 7. 优点
- **创新性**：首次将射线先验（ray prior）引入跨模态蒸馏，直观且物理合理。
- **设计巧妙**：
  - RCD通过高斯采样构建负样本，强制模型区分沿射线的精确/不精确位置。
  - RWD用KL散度自适应调节权重，避免过拟合LiDAR特征。
  - 两个模块可以独立或联合使用，兼容主流BEV架构。
- **实用性**：训练时增加少量时间，推理时零开销，适合实际部署。
- **鲁棒性验证充分**：不仅验证了未见的污染，还验证了见过污染下的表现，全面展示鲁棒性。

## 8. 不足与局限
- **数据集单一**：仅基于nuScenes，未在Waymo、Argoverse等更大或跨城市数据集上验证，泛化性存疑。
- **教师网络固定**：仅使用CenterPoint，未探索不同LiDAR检测器（如VoxelNet、PointPillars）作为教师的影响。
- **鲁棒性上限**：极端污染（如Camera Crash）下性能仍大幅下降（Table 1中BEVDet+Ours的mRR仅为29.5%），说明当图像信息严重缺失时深度蒸馏弥补能力有限。
- **超参数敏感性**：温度τ、采样σ、背景缩放因子s等未进行系统消融或提供敏感性分析。
- **实验统计**：未提供多次运行的标准差或置信区间，仅声明方差<0.2 NDS，可能难以评估统计显著性。
- **应用限制**：方法依赖LiDAR在训练时的存在，不适用于仅有相机传感器的场景；且需要BEV空间统一，对非BEV架构（如纯透视投影方法）不直接适用。

（完）
