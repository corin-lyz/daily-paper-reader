---
title: Improved Monocular Depth Prediction Using Distance Transform Over Pre-semantic Contours with Self-supervised Neural Networks
title_zh: 基于预语义轮廓距离变换的自监督单目深度预测改进
authors: "Hariat, Marwane, Manzanera, Antoine, Filliat, David"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Hariat_Improved_Monocular_Depth_Prediction_Using_Distance_Transform_Over_Pre-semantic_Contours_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 直接改进自监督单目深度预测
tldr: 针对自监督单目深度估计在低纹理区域因光度损失导致深度预测模糊的问题，提出在预语义轮廓上施加距离变换来增强均匀区域的空间判别信息，并联合估计轮廓、深度与自运动。在KITTI等基准上的实验表明，该方法提高了低纹理区域的深度精度与训练效率，距离变换通过放大均匀区域差异使损失函数更有效引导网络学习，为自监督MDE提供了有效的增强手段。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1808, \"height\": 447, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1804, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1815, \"height\": 868, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 766, \"height\": 486, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 861, \"height\": 824, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 285, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 861, \"height\": 258, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 236, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hariat-improved-monocular-depth-prediction-using-distance-transform-over-pre-semantic-contours-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 855, \"height\": 225, \"label\": \"Table\"}]"
motivation: 自监督单目深度估计在低纹理区域深度模糊，需增强空间信息。
method: 对预语义轮廓做距离变换生成增强输入，联合估计轮廓、深度和自运动。
result: 在基准上实验显示低纹理区域深度精度提升，训练更高效。
conclusion: 距离变换增强空间信息可显著提升自监督单目深度估计性能。
---

## Abstract
Monocular depth estimation (MDE) with self-supervised training approaches struggles in low-texture areas, where photometric losses may lead to ambiguous depth predictions. To address this, we propose a novel technique that enhances spatial information by applying a distance transform over pre-semantic contours, augmenting discriminative power in low texture regions. Our approach jointly estimates pre-semantic contours, depth and ego-motion. The pre-semantic contours are leveraged to produce new input images, with variance augmented by the distance transform in uniform areas. This approach results in more effective loss functions, enhancing the training process for depth and ego-motion. We demonstrate theoretically that the distance transform is the optimal variance-augmenting technique in this context. Through extensive experiments on KITTI and Cityscapes, our model demonstrates robust performance, surpassing conventional self-supervised methods in MDE.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：单目深度估计（Monocular Depth Estimation, MDE）只需要单张 RGB 图像即可预测深度，相比立体视觉和 LiDAR 等传感器方案成本更低、实时性更好，是自动驾驶、机器人导航和 VR/AR 的重要基础模块。
- **核心问题**：**自监督 MDE** 通常采用“结构从运动”框架，利用深度网络和 ego-motion 网络重建相邻帧，并通过**光度损失（photometric loss，如颜色差异 + SSIM）**作为监督信号。然而在**低纹理区域**，多个像素的局部颜色和结构特征非常相似，导致光度损失区分度不足，产生**深度预测歧义或模糊**。
- **核心设想**：如果能向低纹理区域引入“既具有区分性、又满足帧间恒常性”的空间信息，就能缓解匹配歧义，提升深度与 ego-motion 的训练效果。
- **整体意义**：论文提出将**预语义轮廓（pre-semantic contours）的距离变换（distance transform）**作为“方差增强”输入，联合估计轮廓、深度和 ego-motion，从而改善自监督 MDE 在低纹理区域的性能，并给出理论解释。

---

## 2. 提出的方法论

### 2.1 核心思想

- 利用边缘/轮廓信息构造**距离变换图**：
  \[
  \delta_\Omega(x) = d(x, \partial \Omega)
  \]
  即每个像素到最近轮廓边界的距离。
- 将距离变换图与 RGB 图像拼接成“方差增强图像”，参与相邻帧的重投影和光度重建，使均匀区域具有更强的判别信号。
- 论文通过定理 1 证明：在 Eikonal 约束 \(\|\nabla f\| = 1\) 下，距离变换是在满足恒常性假设前提下“最大化方差”的最优函数。
- 通过定理 2 证明：基于距离变换的损失函数具有 Lipschitz、光滑性和强凸性（加正则后），从而带来更好的收敛性和泛化性质。

### 2.2 整体框架

- 包含三个网络：
  - **Depth network \(D_\theta\)**：预测深度图。
  - **Ego-motion network \(T_\alpha\)**：预测相邻帧之间的相机位姿。
  - **Edge network \(E_\delta\)**：预测预语义轮廓。
- Depth 和 Edge 网络共享 ResNet encoder，Ego-motion 使用独立的 ResNet encoder。

### 2.3 预语义轮廓估计

- 从深度图和表面法向量构造伪标签：
  - 利用深度/法向拉普拉斯的零交叉（zero-crossing）检测语义边界。
  - 结合归一化梯度幅值生成加权伪标签 \(w_d(p)\) 和 \(w_n(p)\)。
- 使用加权损失 + 对比损失（binary cross-entropy） + 防止退化的正则项 \(E(p)^2\) 训练边缘网络。
- 后处理包括：迟滞阈值、非极大值抑制、形态学闭合、轮廓过滤和闭合，最终得到细轮廓 \(\hat{C}(I)\)。

### 2.4 利用距离变换增强图像

- 根据细轮廓计算距离变换图 \(\delta_I(p)\)。
- 将距离变换图作为额外通道与 RGB 拼接，形成“方差增强图像”。
- 重建损失分为：
  - 标准光度损失（颜色 + SSIM）：\(\mathcal{L}_{photo}\)
  - 距离变换重建损失（L1 损失）：\(\mathcal{L}_{dist}\)
  - 边缘感知平滑损失：\(\mathcal{L}_s = \mathcal{L}_{s1} + \mathcal{L}_{s2}\)
- 最终深度损失：
  \[
  \mathcal{L}_{depth} = \mathcal{L}_{dist} + \mathcal{L}_{photo} + \mathcal{L}_s
  \]
- 扩展：可将距离变换映射到 \(n\) 维随机游走表示（\(RW^n\)），在保持恒常性的前提下进一步增加方差；实验中使用 \(n=3\) 效果最好。

---

## 3. 实验设计

### 3.1 数据集与场景

- **KITTI**：城市、乡村、高速等户外场景；采用 Eigen 数据划分，训练集 39,810 张，测试集 697 张。
- **Cityscapes**：城市街道动态场景；训练集 22,973 张，测试集 1,525 张；边缘检测在 500 张验证帧上评估。
- **Waymo**：大规模自动驾驶数据，包含夜间、不同天气等；使用 1,000 个前视视频序列中的 100,000 对图像训练，1,500 对图像评估。
- **NYUv2**：室内场景；使用官方划分，302 个训练序列，654 张稠密标注测试图。
- **ScanNet**：大规模室内 RGB-D 数据集；用于 **zero-shot 跨域泛化评估**。

### 3.2 Benchmark 与对比方法

- 深度估计在 KITTI、Cityscapes、Waymo、NYUv2/ScanNet 上进行评估。
- 对比方法包括：
  - **Monodepth2**
  - **Guizilini et al.**
  - **FeatDepth（Shu et al.）**
  - **MonoViT**
  - **HR-Depth、RA-Depth、DIFFNet**
  - **Lego（论文原版和复现版 Lego*）**
  - **Struct2Depth、LearnK、Li et al.、CoopNet**
  - **MonoIndoor++、IndoorDepth、GLNet** 等。
- 边缘检测评估使用 **ODS、OIS、AP** 指标，与 LEGO 对比。

### 3.3 主要实验类型

1. **深度估计定量比较**：KITTI/Cityscapes 上与多种自监督方法对比。
2. **域内/跨域实验**：Waymo 域内评估、Cityscapes→KITTI 跨域评估、NYUv2→ScanNet 零样本评估。
3. **消融实验**：分析 \(L_{dist}\)、\(L_s\)、\(E_\theta\)、\(RW^3\) 等组件的影响。
4. **轮廓估计评估**：在 Cityscapes 上评估边缘/轮廓质量。
5. **恒常性假设验证**：在 KITTI MOTS 上跟踪同一实例点，比较不同特征的归一化时间方差。
6. **Flow 与 Odometry 评估**：在附录中报告了光流和位姿估计结果，均优于基线 CoopNet。

---

## 4. 资源与算力

- 论文明确说明：使用 **单张 NVIDIA RTX A5000 GPU**。
- 训练框架：PyTorch。
- 优化器：Adam，\(\beta_1 = 0.99\)，\(\beta_2 = 0.999\)。
- 训练配置：batch size 4，训练 30 epochs，初始学习率 \(10^{-4}\)，20 epochs 后降至 \(10^{-5}\)。
- 训练时长：约 **12 小时**。
- 其他细节：编码器使用 ResNet-50，预训练权重为 ImageNet；输入分辨率默认 256×832。
- 需要指出：文中没有提及总 FLOPs、模型参数量、多卡训练配置、推理延迟或显存占用。

---

## 5. 实验数量与充分性

### 5.1 实验数量与覆盖范围

- 实验覆盖 **5 个公开数据集**，包含户外、室内、自动驾驶、多天气场景，并同时进行了 **域内、跨域和零样本评估**，覆盖面较广。
- 进行了**多组消融实验**，不是单一结果展示。
- 额外设计了**轮廓检测评估**和**恒常性假设量化验证**，从多角度支持方法有效性。
- 结合理论证明（距离变换的最优性和损失函数的收敛性质），形成了“理论 + 消融 + 大规模比较”的较完整证据链。

### 5.2 充分性评价

- **优点**：实验设计较为全面，对比了多种 SOTA 方法，并在同一 backbone（ResNet-50、ViT）下进行了分组对比，具有一定的公平性。
- **不足/风险**：
  - 论文未报告多次运行的均值/标准差，也没有显著性检验，无法判断指标差异的统计稳定性。
  - 部分对比结果来自原论文或复现结果（如 `Lego*`），可能存在复现偏差。
  - 使用预训练 DinoV2 编码器得到的改进结果，并未充分说明该方法与其他基于 DinoV2 的方法是否完全公平。
  - 没有在类别极不平衡、极端遮挡、低光等极端挑战场景下进行专门失败分析。
  - 部分理论断言依赖附录中的证明，但主文中仅给出结论，需依赖附录验证。

---

## 6. 主要结论与发现

- 距离变换是**在恒常性假设下增加低纹理区域方差的最优方法**，并具有良好的收敛性质。
- 基于距离变换的方差增强图像能显著改善自监督 MDE，尤其是低纹理区域和物体边界。
- 联合边缘/轮廓估计与深度估计可以互相促进：深度伪标签提升轮廓质量，轮廓又通过平滑损失改善深度。
- 在 KITTI、Cityscapes、Waymo、NYUv2 和 ScanNet 上均取得优于多数自监督方法的性能。
- 该距离变换损失也能直接用于其他框架（如 Lego*），说明其**通用性和可迁移性**较好。
- 相比基于深度特征（FeatDepth）的增强方法，距离变换在帧间恒常性上表现更好，时间方差更低。

---

## 7. 优点与亮点

- **理论性强**：用优化和凸分析证明距离变换的“最大方差”性质和损失函数的收敛性质，而非仅凭启发式。
- **方法简洁有效**：不需要额外的传感器或监督标注；距离变换计算简单，可作为额外通道或损失项插入现有自监督框架。
- **联合学习设计合理**：深度、法向、边缘、自运动共享信息，互相促进。
- **实验丰富**：覆盖多数据集、多任务（深度、轮廓、光流、位姿），并设置了跨域和零样本评估。
- **公平性考虑**：在相同 backbone 下进行对比，并针对不同方法做了分组比较。
- **对低纹理问题的针对性明确**：既指出了传统光度损失的病态性，又给出了可解释的解决路径。

---

## 8. 不足与局限

- **依赖轮廓质量**：距离变换直接依赖预语义轮廓；如果轮廓不完整或错误，会引入噪声并影响深度重建。
- **额外网络与后处理开销**：需要训练额外的边缘网络，并包含迟滞阈值、形态学闭合等后处理流程，增加了系统复杂度。
- **理论假设较理想化**：理论推导基于凸形状、光滑边界和仿射变换等理想条件，与实际场景中的复杂遮挡、非刚性运动和非凸物体存在差距。
- **实验统计不足**：没有报告多次实验的方差、置信区间，多数结果可能只来自单次训练；无法排除随机性影响。
- **数据集偏向**：主要验证自动驾驶与室内场景，对自然场景、水下、低光、快速运动等“in-the-wild”场景覆盖有限。
- **缺少部署相关指标**：未报告推理时间、模型参数量、实际运行内存等，难以判断实际应用中的资源占用。
- **未提供代码和预训练模型**：可复现性受到一定限制（论文中未明确说明是否开源）。
- **距离变换的“恒常性”仍受动态物体和遮挡影响**：尽管方法改善了这一问题，但并未完全解决所有病态情况。

---

（完）
