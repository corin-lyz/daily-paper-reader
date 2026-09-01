---
title: Intrinsic Image Decomposition for Robust Self-supervised Monocular Depth Estimation on Reflective Surfaces
title_zh: 基于内在图像分解的反射表面鲁棒自监督单目深度估计
authors: "Wonhyeok Choi, Kyumin Hwang, Minwoo Choi, Kiljoon Han, Wonjoon Choi, Mingyu Shin, Sunghoon Im"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32258/34413"
tags: ["query:mono-depth"]
score: 9.0
evidence: 结合内在图像分解的鲁棒自监督单目深度估计方法
tldr: 现有自监督单目深度估计依赖光度一致性损失，但在反射表面违反朗伯假设时常产生显著误差。本文提出将内在图像分解与自监督深度估计联合训练的新框架，同时估计反射率与光照分量，以缓解反射带来的匹配歧义。在相关数据集上的实验表明，该方法能够有效提升反射区域的深度精度，并保持整体估计的鲁棒性。该工作为复杂光照与镜面场景下的单目深度估计提供了可行思路。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32258/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 789, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32258/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 811, \"height\": 1098, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32258/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1630, \"height\": 492, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32258/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1475, \"height\": 542, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32258/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1474, \"height\": 356, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32258/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 878, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32258/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1656, \"height\": 604, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32258/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1661, \"height\": 518, \"label\": \"Table\"}]"
motivation: 自监督单目深度估计在反射表面上因朗伯假设失效而产生显著深度误差，影响真实场景中的可用性。
method: 提出将内在图像分解融入自监督单目深度估计框架，将输入图像分解为反射率与光照，并联合优化深度估计网络。
result: 在反射表面相关数据集上，所提方法较基线明显降低深度误差，尤其在反射区域恢复出更准确的深度。
conclusion: 通过内在图像分解缓解反射干扰，有效提升了自监督单目深度估计在非朗伯场景中的鲁棒性。
---

## Abstract
Self-supervised monocular depth estimation (SSMDE) has gained attention in the field of deep learning as it estimates depth without requiring ground truth depth maps. This approach typically uses a photometric consistency loss between a synthesized image, generated from the estimated depth, and the original image, thereby reducing the need for extensive dataset acquisition. However, the conventional photometric consistency loss relies on the Lambertian assumption, which often leads to significant errors when dealing with reflective surfaces that deviate from this model. To address this limitation, we propose a novel framework that incorporates intrinsic image decomposition into SSMDE. Our method synergistically trains for both monocular depth estimation and intrinsic image decomposition. The accurate depth estimation facilitates multi-image consistency for intrinsic image decomposition by aligning different view coordinate systems, while the decomposition process identifies reflective areas and excludes corrupted gradients from the depth training process. Furthermore, our framework introduces a pseudo-depth generation and knowledge distillation technique to further enhance the performance of the student model across both reflective and non-reflective surfaces. Comprehensive evaluations on multiple datasets show that our approach significantly outperforms existing SSMDE baselines in depth prediction, especially on reflective surfaces.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 自监督单目深度估计（SSMDE）无需真值深度图，仅依靠连续图像之间的**光度一致性损失**即可训练深度网络，显著降低了对 LiDAR 等昂贵标注数据的依赖。
- 传统 SSMDE 强烈依赖**朗伯（Lambertian）表面假设**，即假设物体表面在不同视角下亮度一致。然而室内场景中常见的**反射表面（镜子、光滑金属、玻璃等）**会严重违反这一假设，导致光度误差被错误地用于梯度反向传播，从而在反射区域产生显著深度误差。
- 已有方法尝试通过分割掩码、多网络不确定性或 3D 网格渲染来定位并修复非朗伯区域，但普遍需要额外标注或极高的计算成本，削弱了自监督方法原有的低成本优势。
- 为此，本文提出将**内在图像分解（Intrinsic Image Decomposition）**与 SSMDE 联合训练的新框架，利用分解出的“漫反射成分”与“残差成分”来定位反射区域，并在深度训练中屏蔽这些区域产生的错误梯度，从而提升反射表面的深度估计精度。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **整体框架**：包含两个分支——**内在图像分解分支**和**深度估计分支**。两个分支共享/复制深度解码器结构，在估计深度的同时预测内在图像，并通过协同训练相互促进。
- **内在图像分解建模**：
  - 采用**内在残差模型（intrinsic residual model）**，在对数空间中将图像分解为：
    \[
    \log I = \log L + \log R
    \]
    其中 \(L\) 为漫反射图像（与视角无关），\(R\) 为残差图像（随视角变化的镜面反射/高光成分）。
  - 不同于传统基于光照变化的时间序列分解方法，本文利用深度估计得到的相机位姿和视点合成（view synthesis）来对齐参考帧与源帧，从而在动态单目序列中实现自监督分解。
- **损失函数设计**：
  - **重建损失 \(L_{recon}\)**：确保分解后的漫反射+残差能重建原图。
  - **交叉重建损失 \(L_{cross}\)**：用源帧的漫反射图像经视点合成后重建参考帧，利用漫反射成分的视角不变性增强一致性。
  - **对比损失 \(L_{cts}\)**：防止模型退化（例如将漫反射全学成白色图像），促使不同图像的漫反射成分彼此可区分。
- **反射区域定位与深度训练屏蔽**：
  - 分别计算 RGB 图像对和漫反射图像对的光度误差 \(E_I\) 与 \(E_L\)。
  - 由于漫反射图像去除了与视角相关的残差成分，在反射区域其光度误差通常低于 RGB 图像误差；利用**马氏距离**构建逐像素二值掩码 \(M_R\)，标记出“非漫反射”区域。
  - 深度损失将 \(M_R\) 与原始光度误差相乘，从而在反射区域**抑制错误梯度**的传播：
    \[
    L_{depth} = M_R \odot P(I_r, I_{s2r})
    \]
  - 总损失为固有分解损失与深度损失之和：\(L_{total} = L_{itr} + L_{depth}\)。
- **知识蒸馏（可选增强）**：
  - 以常规 SSMDE 训练的模型（Baseline）和本文方法训练的模型（Baseline + Ours）作为双教师，根据反射区域掩码选择更可靠的深度作为伪真值，对学生模型进行蒸馏，进一步提升在反射与非反射表面的整体性能。

## 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - **ScanNetv2**：大规模室内 RGB-D 扫描数据集。作者使用与 3D Distillation 相同的训练三元组划分，并专门构建了 **ScanNet-Reflection**（含反射表面，测试集和验证集）和 **ScanNet-NoReflection**（几乎不含反射表面）子集来评估。
  - **7-Scenes**：由七个室内场景组成，主要用于跨数据集泛化评估，场景以非反射物体为主。
  - **Booster**：包含大量透明、镜面、高光等非朗伯表面的室内场景，用于评估方法对复杂反射表面的鲁棒性。
- **对比方法**：
  - 三个代表性 SSMDE 基线：**Monodepth2**、**HRDepth**、**MonoViT**。
  - 将本文提出的训练协议（内在分解分支 + 反射掩码）分别应用到上述基线，形成 [Baseline + Ours] 进行对比。
  - 在多阶段蒸馏比较中，还对比了 **Self-teaching**（基于不确定性的自蒸馏）和 **3D Distillation**（使用多模型集成和 3D 网格重建生成伪真值）。
- **评价指标**：标准深度估计指标，包括 Abs Rel、Sq Rel、RMSE、RMSE log、\(\delta<1.25\)、\(\delta<1.25^2\)、\(\delta<1.25^3\)。
- **训练设置**：分辨率 384×288，训练 41 个 epoch，学习率 1e-4 并按 epoch 26/36 衰减；相机位姿在训练时已知；深度范围 0.1m~10m；内在分解损失权重为 {1.0, 1.0, 0.01}，对比损失间隔 \(\delta=5.0\)。

## 4. 资源与算力

- 论文中**未明确说明**所使用的 GPU 型号、数量、训练时长或其他算力资源。
- 仅提及批量大小（Monodepth2/HRDepth 为 12，MonoViT 为 8）和训练总 epoch 数（41），但未给出实际硬件配置和运行时间。
- 这一点在总结中需明确指出：资源与算力信息缺失，无法从文本中得知训练成本。

## 5. 实验数量与充分性

- **实验规模较丰富**，包含：
  - 在 ScanNet-Reflection 测试集和验证集上的主实验（3 个基线 × 有无本文方法）。
  - 在 ScanNet-NoReflection 验证集上的泛化实验。
  - 在 7-Scenes 和 Booster 上的跨数据集实验。
  - 与多阶段方法（Self-teaching、3D Distillation）的对比。
  - 对内在分解损失中对比损失 \(L_{cts}\) 的消融实验。
- **充分性分析**：
  - 优点：数据集覆盖了反射、非反射、跨域场景，对比方法多样，且包含消融验证，整体相对充分。
  - 不足：消融实验仅针对单一基线（Monodepth2）的一个损失项进行，未对反射掩码机制、各损失权重、不同基线下的消融做更全面分析；此外缺少与其他反射区域掩码方法的直接比较（如使用分割掩码的方法成本更高但未在表内对比）。实验公平性方面，由于相机位姿已知，与同类方法（如 3D Distillation）设置一致，总体较公平。

## 6. 论文的主要结论与发现

- 提出的内在图像分解框架能够有效定位反射区域并屏蔽其错误梯度，显著提升 SSMDE 在反射表面上的深度精度。
- 在 ScanNet-Reflection 测试集上，三个基线的 Abs Rel 平均提升约 9%；验证集上平均提升约 23.51%，表明方法对反射区域的高度有效性。
- 在无反射表面上，性能几乎不下降（Monodepth2 略降 1.47%，HRDepth 甚至提升 0.42%，MonoViT 略降 1.10%），说明反射掩码定位准确，不会误伤正常区域。
- 在 Booster 数据集（大量非朗伯表面）上三个基线均有明显提升（平均改善 4.4%~10%），验证了对透明、镜面等复杂反射的鲁棒性。
- 知识蒸馏变体在多数指标上优于或接近 3D Distillation，但避免了三维网格渲染等高计算成本，说明本文方法具有更好的效率-性能权衡。

## 7. 优点

- **首次**将内在图像分解引入 SSMDE，并实现全自监督、端到端训练，无需额外标注。
- 反射区域定位完全基于光度误差的统计差异，无需分割掩码或深度真值，通用性强，可作为“即插即用”训练协议应用于多种基线架构。
- 内在分解与深度估计相互促进：深度改善对齐质量，对齐质量又提升分解效果，形成良性循环。
- 实验覆盖多类型室内场景（反射、非反射、透明/镜面），且对比了多个强基线和先进多阶段方法，证据较充分。
- 知识蒸馏方案进一步提高了性能，而不引入过多计算开销。

## 8. 不足与局限

- **资源信息缺失**：未报告 GPU 型号、数量、训练耗时等，难以评估部署和复现成本。
- **实验覆盖有限**：
  - 消融实验较简略，仅验证了 \(L_{cts}\) 的作用，未对 \(L_{cross}\)、反射掩码阈值、马氏距离等关键设计进行逐一剖析。
  - 未在室外数据（如 KITTI）上验证，方法对室外动态场景的适用性未知。
- **潜在偏差风险**：
  - 实验主要在室内扫描数据集上展开，反射表面的真值深度本身可能存在噪声（如 RGB-D 相机对反射区域测量不准），会影响评价准确性。
  - 训练时假设相机位姿已知，限制了纯视觉自监督场景下的应用。
  - 反射掩码基于像素级统计差值，极端光照或复杂反射情况下仍可能误判，导致部分区域梯度被错误屏蔽。
- **应用限制**：对高度透明物体（如玻璃）和多重反射场景的处理能力尚未明确验证；内在分解质量可能影响高光细节的保留，从而在非反射表面引入轻微性能下降。

（完）
