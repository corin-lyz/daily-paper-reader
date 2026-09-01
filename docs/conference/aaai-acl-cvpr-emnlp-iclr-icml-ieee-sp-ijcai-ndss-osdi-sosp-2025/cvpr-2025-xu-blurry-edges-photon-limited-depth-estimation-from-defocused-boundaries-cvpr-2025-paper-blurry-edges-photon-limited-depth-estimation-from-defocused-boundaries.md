---
title: "Blurry-Edges: Photon-Limited Depth Estimation from Defocused Boundaries"
title_zh: Blurry-Edges：从散焦边界进行光子受限深度估计
authors: "Xu, Wei, Wagner, Charles James, Luo, Junjie, Guo, Qi"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Xu_Blurry-Edges_Photon-Limited_Depth_Estimation_from_Defocused_Boundaries_CVPR_2025_paper.pdf"
tags: ["query:neural-bokeh"]
score: 6.0
evidence: 从散焦边界估计深度，与模糊渲染相关
tldr: 光子受限图像中，散焦模糊受噪声影响难以准确估计深度。本文提出Blurry-Edges表示，显式存储边界、颜色和平滑度等补丁信息，并设计深度网络预测该表示，再通过闭式深度-散焦关系解算深度。实验表明该方法在低光散焦场景中能稳健恢复物体深度，为散焦测距与虚化渲染提供新工具。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1814, \"height\": 456, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 710, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 540, \"height\": 312, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1471, \"height\": 583, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 681, \"height\": 556, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 873, \"height\": 145, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 805, \"height\": 280, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1818, \"height\": 749, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1819, \"height\": 441, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 459, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-xu-blurry-edges-photon-limited-depth-estimation-from-defocused-boundaries-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1641, \"height\": 509, \"label\": \"Table\"}]"
motivation: 光子受限图像中的散焦深度估计对噪声十分敏感，传统DfD难以直接应用。
method: 提出Blurry-Edges补丁表示，用深度网络从一对不同散焦图像预测表示，并推导闭式深度关系。
result: 在光子受限的散焦图像上实现了稳健的物体深度估计，优于基线方法。
conclusion: 为低光照散焦场景的深度估计提供了新的表示与推理框架，对计算摄影有启发。
---

## Abstract
Extracting depth information from photon-limited, defocused images is challenging because depth from defocus (DfD) relies on accurate estimation of defocus blur, which is fundamentally sensitive to image noise. We present a novel approach to robustly measure object depths from photon-limited images along the defocused boundaries. It is based on a new image patch representation, Blurry-Edges, that explicitly stores and visualizes a rich set of low-level patch information, including boundaries, color, and smoothness. We develop a deep neural network architecture that predicts the Blurry-Edges representation from a pair of differently defocused images, from which depth can be calculated using a closed-form DfD relation we derive. The experimental results on synthetic and real data show that our method achieves the highest depth estimation accuracy on photon-limited images compared to a broad range of state-of-the-art DfD methods.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景与动机**：深度来自散焦（Depth from Defocus, DfD）是一种无需主动光源即可恢复物理深度的技术，适合 AR/VR、手机、手表、微型机器人等空间受限平台。传统 DfD 依赖图像空间导数或散焦模糊程度作为深度线索，而散焦信息本身对噪声极为敏感，尤其在光子受限（低光照、高噪声）场景下，现有方法通常假设输入图像噪声较低，难以直接应用。
- **核心问题**：如何在强噪声、低照度条件下，从一对不同散焦程度的图像中稳健估计深度。
- **整体思路**：本文提出一种新的图像块表示 **Blurry-Edges**，显式建模图像块的“颜色、边界位置、边界平滑度”三类低级信息，并用深度神经网络从一对不同散焦图像中预测该表示，再通过推导得到的闭式 DfD 公式解算深度。
- **意义**：将 DfD 的适用范围拓展到光子受限环境，为低光照散焦深度估计与计算摄影提供了新工具；同时该表示可同时输出边界图、去噪彩色图、锐化/重聚焦图像等多种副产品。

## 2. 方法论：核心思想、技术与公式

### 2.1 核心思想
- 散焦模糊可建模为高斯 PSF 卷积；一对不同光焦度（ρ+、ρ−）下拍摄的图像，同一物理边界在两张图中的“模糊平滑度”不同。
- 若能准确估计同一边界在两幅图像中的平滑度（η+、η−），即可抵消场景自身纹理锐度的影响，从而解析地求解深度。
- 难点在于“平滑度”在有噪声时难以直接测量。因此作者设计了一个参数化图像块表示 Blurry-Edges，将局部结构限制为若干“楔形”（wedge）的叠加，使噪声下的结构估计更加稳健。

### 2.2 Blurry-Edges 表示
- 每个图像块用 𝑙 个垂直堆叠、部分遮挡、颜色恒定的楔形表示：
  \[
  \Psi = (\{p_i, \theta_i, c_i, \eta_i, i=1,\dots,l\}, c_0)
  \]
  - \(p_i\)：楔形顶点位置；
  - \(\theta_i\)：楔形起始/终止角度；
  - \(c_i\)：楔形颜色；
  - \(\eta_i\)：楔形边界平滑度；
  - \(c_0\)：背景颜色。
- 通过 α-clipping 形式渲染彩色图：
  - \(c(x;\Psi)=\sum_{i=0}^l c_i \alpha_{l\to i}(x;\Psi)\)。
- 由该表示可生成：
  - 边界中心图 \(b(x;\Psi,\delta)\)；
  - 无色噪声影响的彩色图 \(c(x;\Psi)\)；
  - 色彩导数图 \(c'(x;\Psi)\)（经 Sobel 滤波），用于突出边界平滑度。

### 2.3 闭式深度-散焦关系
- 假设高斯 PSF、薄透镜模型，以及场景纹理可视为“理想阶跃函数与高斯核卷积”，深度 \(z\) 与两幅图中的边界平滑度 \(\eta_+\)、\(\eta_-\) 满足：
  \[
  z(\eta_+,\eta_-) = \frac{2\Sigma^2 s^2(\rho_- - \rho_+)}
       {\eta_+^2 - \eta_-^2 - \Sigma^2 s^2(\rho_+ - \rho_-)(\rho_+ + \rho_- - 2)}
  \]
- 该公式可对每一对对应的边界平滑度直接求解深度，避免了显式估计场景纹理锐度。

### 2.4 网络架构与算法流程
- **输入**：一对不同光焦度下拍摄的静态场景图像 \(I_+, I_-\)。
- **局部阶段（Local stage）**：
  - 将图像划分为重叠块（patch），每个块用基于卷积残差网络的 CNN 独立预测 Blurry-Edges 的部分参数（顶点、角度、平滑度）；
  - 颜色参数通过岭回归（ridge regression）从预测的 α-map 和输入图像联合估计；
  - 该阶段损失包含颜色误差、平滑度误差、边界定位误差三项。
- **全局阶段（Global stage）**：
  - 使用 Transformer Encoder 对所有块的 Blurry-Edges 表示进行全局精修；
  - 强制同一位置的两幅输入图共享相同的楔形位置与颜色（defocus consistency），仅平滑度不同；
  - 同时促进相邻块之间在边界图、彩色图、色彩导数图、深度上的一致性；
  - 训练损失共 7 项，覆盖颜色、边界位置、平滑度、一致性等。
- **输出**：
  - 全局边界中心图 \(B(x)\)；
  - 全局彩色图 \(C(x)\)（可生成锐化/重聚焦结果）；
  - 全局稀疏深度图 \(Z(x)\)（沿边界中心）；
  - 全局置信度图 \(F(x)\)，用于过滤不可靠边界与深度。

## 3. 实验设计

### 3.1 训练数据
- 训练集/验证集：自合成的“基本几何形状”图像（方形、圆形、三角形），每物体深度在 0.75 m–1.18 m 内随机取值。
- 图像噪声：采用 Poisson-Gaussian 噪声模型：
  \[
  I(x) = \mathrm{Poisson}(\alpha I^*(x)) + \mathrm{Gaussian}(0,\sigma^2)
  \]
  - 光子水平 \(\alpha \in [180,200]\)，读出噪声标准差 \(\sigma=2\)。
- 光焦度：\(\rho_-=10.0\,\mathrm{m^{-1}}\)，\(\rho_+=10.2\,\mathrm{m^{-1}}\)。
- 训练/验证场景数量：8000 / 2000。
- 局部阶段随机裁剪 16000 / 4000 个含边界的 patch；全局阶段使用完整图像。

### 3.2 测试数据与基准
- 避免使用 NYUDv2 等 RGBD 数据集，因为无法提供真实的散焦边界渲染所需遮挡背景信息。
- 测试集：从 MS-COCO 选取前景物体、从 Painting 数据集选取背景，组合成 200 个包含复杂纹理、复杂边界形状、连续深度变化的场景。
- 渲染遵循 Guo et al. 的框架，使用插值 PSF 模拟平滑变化的散焦和 alpha-clipping 生成真实深度边界。
- 图像分辨率：147×147 像素；更大分辨率（如 587×587）通过分块处理测试。

### 3.3 对比方法
- 解析方法：Focal Track、Tang et al.；
- 学习方法：PhaseCam3D、DefocusNet、DFV-DFF、DEReD；
- 这些方法均使用相同训练数据重新训练，以适应噪声图像。
- 本文变体：
  - **Ours**：稀疏深度图；
  - **Ours-W**：由 Blurry-Edges 渲染得到的稠密深度图；
  - **Ours-PP**：用 U-Net 对稀疏深度图后处理得到的稠密深度图。

### 3.4 评测指标
- \(\delta_1, \delta_2, \delta_3\)（相对误差阈值内的像素比例）、RMSE、AbsRel。

### 3.5 真实实验
- 搭建了带可变形透镜的原型相机，采集低光、不同散焦的图像对/图像栈；
- 与对比方法在真实场景中进行定性/定量比较，参考深度由人工测量生成。

## 4. 资源与算力

- 论文明确说明：“The training and testing are performed on an NVIDIA GeForce RTX A5000 graphics card with 24 GB of memory”。
- 未说明：
  - GPU 数量（似乎仅使用单张 RTX A5000）；
  - 具体训练时间（epoch 数有：局部阶段 1000 epoch、全局阶段 350 epoch，但未给耗时）；
  - 是否使用多卡分布式训练、显存占用峰值等。

## 5. 实验数量与充分性

- **主要实验类型**：
  1. 合成测试集上的定量/定性对比（表 3、图 8）；
  2. 真实原型相机图像上的对比（图 9）；
  3. patch 大小消融实验（11×11、21×21、31×31，表 2、图 7）；
  4. 大分辨率图像分块处理测试（图 8b）；
  5. 稠密化变体对比（Ours-W、Ours-PP）。
- **充分性评价**：
  - 优点：对比方法涵盖解析和深度学习方法，且均用相同训练数据重训，比较相对公平；同时提供稀疏和稠密的多种输出；真实实验验证了泛化性。
  - 不足：消融实验较单一，仅考察了 patch size，未系统消融损失项数量、楔形数 \(l\)、网络两阶段的作用等；训练数据只包含基本几何形状，虽然测试集更复杂，但真实场景数量有限；没有与更近期、面向暗光的深度估计方法进行对比。

## 6. 主要结论与发现

- 提出的 Blurry-Edges 表示能够在强噪声（比以往 DfD 工作高 4 倍以上的噪声标准差）下稳健恢复边界平滑度。
- 从预测的平滑度对使用闭式公式可解析求解深度，显著优于现有 DfD 方法：
  - 稀疏深度（Ours）：RMSE 5.281 cm vs 最佳对比 Tang et al. 6.737 cm；
  - 稠密深度（Ours-PP）：RMSE 3.992 cm vs 最佳对比 DefocusNet 6.092 cm；
  - 真实实验中视觉质量和定量误差也明显优于其他方法。
- 模型仅用简单几何形状训练即可泛化到复杂纹理的真实/合成图像，无需微调。
- Blurry-Edges 同时可用于生成无噪声彩色图、边界图和锐化/重聚焦图像，具有多功能性。

## 7. 优点

- **鲁棒性**：核心创新在于将局部结构建模为 Blurry-Edges，显著降低噪声对散焦估计的影响，能处理 4 倍以上的噪声水平，这是以往 DfD 方法无法做到的。
- **解析可解释性**：深度求解有闭式公式，而非纯黑盒回归，增强了可靠性与可解释性。
- **多任务输出**：一个表示同时支持深度、边界、去噪彩色图、锐化/重聚焦，适用面广。
- **训练数据简单**：仅用基本几何图形训练即可泛化到复杂真实场景，说明表示学习具有很强的归纳偏置。
- **公平对比**：对基线方法进行重训，并在相同噪声/数据条件下比较。

## 8. 不足与局限

- **稀疏性**：深度只在边界处估计，稠密深度需要额外后处理（Ours-PP）或基于楔形的渲染填充，纹理内部无法直接获得深度。
- **模型复杂度**：固定 \(\ell=2\) 个楔形，patch size 为 21×21；对更复杂交界结构表达能力有限。
- **训练数据单一**：只使用三种基本几何形状；虽然测试集更复杂，但真实场景数量有限，泛化边界尚未充分探索。
- **消融不足**：对损失项、楔形数、网络结构、噪声强度变化等缺少系统消融。
- **硬件假设**：需要可变形透镜或能改变光焦度的光学系统，且需静态场景；对动态场景不适用。
- **计算资源信息不完整**：未报告训练时长、GPU 数量，难以评估复现成本和扩展性。
- **应用限制**：深度范围/光学参数设定在 0.75–1.18 m 附近，更远/更近场景的适用性未充分验证。

（完）
