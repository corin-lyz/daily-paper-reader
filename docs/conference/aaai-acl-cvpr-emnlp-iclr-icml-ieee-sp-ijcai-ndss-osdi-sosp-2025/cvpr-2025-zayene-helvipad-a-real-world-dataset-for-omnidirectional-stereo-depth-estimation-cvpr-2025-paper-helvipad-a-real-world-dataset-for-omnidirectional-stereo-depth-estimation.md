---
title: "HELVIPAD: A Real-World Dataset for Omnidirectional Stereo Depth Estimation"
title_zh: HELVIPAD：面向全方位立体深度估计的真实世界数据集
authors: "Zayene, Mehdi, Endres, Jannik, Havolli, Albias, Corbière, Charles, Cherkaoui, Salim, Kontouli, Alexandre, Alahi, Alexandre"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zayene_HELVIPAD_A_Real-World_Dataset_for_Omnidirectional_Stereo_Depth_Estimation_CVPR_2025_paper.pdf"
tags: ["query:stereo-depth"]
score: 6.0
evidence: 提供真实世界双目深度估计数据集及精确标签
tldr: 针对全方位立体深度估计数据缺乏的问题，提出Helvipad数据集，包含40K视频帧的室外及室内多样场景，使用双360度相机和LiDAR采集并提供精确深度与视差标签。还通过深度补全增加标签密度，并基准测试了多种立体深度模型，数据覆盖拥挤室内外与多样光照条件，可支持全景立体方法的训练与评估，为全方位立体深度估计研究提供了重要数据资源。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1788, \"height\": 661, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 851, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1584, \"height\": 432, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1813, \"height\": 560, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 823, \"height\": 407, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1804, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1790, \"height\": 872, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1820, \"height\": 688, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1806, \"height\": 344, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zayene-helvipad-a-real-world-dataset-for-omnidirectional-stereo-depth-estimation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 179, \"label\": \"Table\"}]"
motivation: 全方位立体深度估计缺少真实世界数据集。
method: 用双360度相机和LiDAR采集40K帧，构建带精确深度标签的数据集。
result: 提供丰富场景数据集，并完成主流立体深度模型的基准评估。
conclusion: 填补了全方位立体深度数据空白，促进相关研究。
---

## Abstract
Despite progress in stereo depth estimation, omnidirectional imaging remains underexplored, mainly due to the lack of appropriate data. We introduce Helvipad, a real-world dataset for omnidirectional stereo depth estimation, featuring 40K video frames from video sequences across diverse environments, including crowded indoor and outdoor scenes with various lighting conditions. Collected using two 360deg cameras in a top-bottom setup and a LiDAR sensor, the dataset includes accurate depth and disparity labels by projecting 3D point clouds onto equirectangular images. Additionally, we provide an augmented training set with an increased label density by using depth completion. We benchmark leading stereo depth estimation models for both standard and omnidirectional images. The results show that while recent stereo methods perform decently, a challenge persists in accurately estimating depth in omnidirectional imaging. To address this, we introduce necessary adaptations to stereo models, leading to improved performance.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：全方位（Omnidirectional / 360°）立体深度估计面临真实世界数据严重匮乏的问题，限制了该方向深度学习方法的训练与评估。
- **研究背景**：
  - 移动机器人（自动驾驶、医疗、农业等）需要精确的 3D 环境表示来导航和交互。
  - 传统 LiDAR 虽然精度高、覆盖 360°，但点云稀疏、远距离物体数据不足、部署成本高。
  - 基于立体相机的深度估计更易获取，但现有数据集（如 KITTI、nuScenes、Waymo）多为前视或有限视场，难以覆盖周围完整环境。
  - 360° 相机能提供完整视场，但其等距柱状投影（equirectangular）存在显著球面畸变，使常规立体匹配模型难以直接应用。
- **本文贡献**：构建了 HELVIPAD 数据集——首个面向真实世界、带像素级深度标签的全方位立体视频数据集，并针对球面几何提出模型适配方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 2.1 数据采集
- 自建采集平台：两台 Ricoh Theta V 360° 相机，采用**上-下（top-bottom）垂直布局**，基线距离 19.1 cm，30 fps 采集；搭配 Ouster OS1-64 LiDAR（垂直视场 42.4°，10 Hz），安装在下方相机下方 45.0 cm 处。
- 共采集 29 段视频序列，覆盖室内（走廊、大厅）和室外（广场、停车场、人行道）等多种场景，以及白天/夜晚等不同光照条件。

### 2.2 LiDAR 到 360° 图像的映射
- 利用棋盘格选取 LiDAR 点云与图像中的对应关键点。
- 通过初始旋转/平移将 LiDAR 坐标转换到相机坐标系，再转为球坐标 `(r, θ, φ)`，投影到等距柱状图像上。
- 使用 **BFGS 优化算法**最小化投影点与图像标注点之间的总误差，获得最优外参 `(R_opt, T_opt)`。
- 对齐精度评估：随机点平均误差 1.7 像素，边缘等困难区域平均误差 8.0 像素。
- 球面视差（spherical disparity）计算公式（角度制，单位为度）：

\[
d = \arctan\left(\frac{\sin(\theta_b)}{r_{\text{bottom}} / B_{\text{camera}} - \cos(\theta_b)}\right)
\]

其中 θ_b 为极角，r_bottom 为底部相机深度，B_camera 为基线。

### 2.3 深度补全（Depth Completion）
- **目的**：LiDAR 点云稀疏，初始标签仅覆盖约 12% 像素；深度补全后提升至 **61%**，用于构建增强训练集。
- 流程：
  1. **时间聚合**：将当前帧点云与前后各 4 帧点云聚合，增加有效点数（机器人速度慢、LiDAR 帧率高，误差可忽略；对快速移动物体会有限制）。
  2. **插值**：在球面网格上对查询点取 k 近邻，按球面距离倒数加权平均估计深度。
  3. **滤波**：基于加权方差（不确定性）和近邻平均距离（是否缺乏邻近点）排除不可靠插值点，保证标签质量。

### 2.4 针对 360° 成像的立体匹配模型适配（360-IGEV-Stereo）
- **极角图输入（Polar Angle Map）**：将极角编码为特征图，与图像特征在 bottleneck（1/32 分辨率）和 context 特征（1/4 分辨率）处拼接，帮助模型感知随极角变化的畸变。
- **循环填充（Circular Padding）**：沿水平方向用图像右/左边缘像素填充左/右侧，充分利用 360° 图像的左右连续性。
- 以上适配应用于 IGEV-Stereo，形成 **360-IGEV-Stereo** 模型。

## 3. 实验设计

### 3.1 数据集与基准划分
- 总规模：39,553 帧配对立体图像，分辨率 **1920 × 512**，含深度图与视差图标签。
- 划分：
  - 训练/验证集：20 段序列（室外 13、室内 5、夜间室外 2），共 29,407 帧。
  - 测试集：6 段序列（室外 3、室内 2、夜间室外 1），共 10,146 帧，并人工检查确保与训练集场景不重叠。
- 深度范围：0.5 m 至 225 m，平均深度 8.1 m（室内 5.4 m，室外白天/夜晚 9.2 m）。

### 3.2 对比方法（Baselines）
| 方法 | 类型 |
|---|---|
| PSMNet | 常规立体匹配（rectilinear） |
| IGEV-Stereo | 常规立体匹配，当前 SOTA |
| 360SD-Net | 全方位立体匹配（top-bottom 设置） |
| 360-IGEV-Stereo | 本文提出的适配模型 |

### 3.3 评估指标
- **Disparity / Depth MAE、RMSE、MARE**（平均绝对误差、均方根误差、平均绝对相对误差）
- **LRCE**（Left-Right Consistency Error，评估全向图像左右边界一致性）

## 4. 资源与算力

- 论文明确提到：所有模型在**单张 NVIDIA A100 GPU** 上训练，使用尽可能大的 batch size。
- **未明确说明**：GPU 数量、训练时长、总计算成本、能耗等细节均未在正文中提供。

## 5. 实验数量与充分性

### 主要实验组
1. **基准对比实验**：4 种方法在 HELVIPAD 测试集上的完整指标对比。
2. **深度补全作为数据增强的消融实验**：比较原始标签 vs. 增强标签训练对 4 种方法的影响。
3. **模型适配消融实验**：针对 360-IGEV-Stereo，分别移除循环填充、移除光度数据增强，验证其效果。
4. **跨场景泛化实验**：在不同训练子集（Indoor、Outdoor、Indoor+Outdoor、All）上训练，在室内、室外、夜间室外三种条件下测试，评估模型泛化能力。
5. **定性可视化**：展示深度补全效果以及不同模型预测的视差图对比。

### 充分性评价
- 实验设计较为全面：既包含方法横向对比，也覆盖数据增强、几何适配、跨场景泛化等多个维度。
- 公平性较好：所有方法在同一数据集、同一评估协议下训练和测试。
- 不足之处：基线方法数量有限（仅 4 个）；深度补全管线本身未做详细消融（如不同 k 值、阈值敏感性）；未与更多近期全向深度估计方法（如基于 transformer 的方法）对比。

## 6. 论文的主要结论与发现

- **360-IGEV-Stereo 在全部指标上取得最优结果**（如深度 MAE 1.720 m，深度 MARE 0.130，LRCE 0.388），显著优于常规 IGEV-Stereo 和 360SD-Net。
- 深度补全增强训练集能稳定提升各方法性能，对较老模型（PSMNet、360SD-Net）收益更大，对先进模型（IGEV-Stereo、360-IGEV-Stereo）也有积极帮助。
- 循环填充能大幅降低 LRCE（从 1.153 降至 0.388），改善图像边界处的深度连续性；光度增强提升光照泛化能力。
- 跨场景泛化分析显示：全方位方法（360SD-Net、360-IGEV-Stereo）比常规方法在跨场景（尤其室内→夜间室外）时具有更好的泛化能力；使用包含多样化场景的训练数据对鲁棒性至关重要。
- 常规先进立体模型在全向图像上仍面临畸变挑战，但通过极角输入和循环填充等针对性适配，可以取得明显的性能提升。

## 7. 优点

- **数据稀缺性突破**：HELVIPAD 是第一个真实世界、含像素级深度标签的全方位立体数据集，直接填补领域空白。
- **采集设置合理**：采用 top-bottom 双 360° 相机布局，避免左右布局的遮挡问题，硬件成本相对可控。
- **标签质量较高**：LiDAR 投影平均误差仅 1.7 像素，并提供完整的标定和优化流程。
- **深度补全管线有效**：将标签密度从 12% 提升至 61%，且误差低，为训练提供了显著更密集的监督信号。
- **方法适配简洁有效**：极角图 + 循环填充的方案对模型改动小、参数开销低，却带来一致且明显的性能提升。
- **实验分析细致**：包含跨场景泛化、夜间场景、消融分析等多维度评估，展示了方法的鲁棒性和局限性。

## 8. 不足与局限

- **算力信息缺失**：未报告训练时间、GPU 数量、能耗等关键计算成本，限制了方法的可复现性和实际部署评估。
- **LiDAR 视场有限**：Ouster OS1-64 垂直视场仅 42.4°，无法覆盖相机完整垂直范围，天空和部分场景区域始终缺少标签；论文通过裁剪图像区域回避了该问题。
- **深度补全的限制**：时间聚合对快速移动物体（如汽车）会产生错误标签；插值和滤波依赖启发式阈值，缺乏系统敏感性分析。
- **遮挡未显式处理**：LiDAR-相机基线虽小，但论文声明未处理遮挡效应，可能在近景或复杂几何场景带来误差。
- **模型覆盖不全**：对比方法仅包含 4 种，未与更多近期全向深度估计工作（如 PanoFormer、基于 transformer 的方法）比较。
- **数据规模有限**：40K 帧虽具开创性，但相比驾驶数据集（如 Waymo 1M 帧）仍较小；且所有场景来自同一大学校园，跨地域、跨文化泛化能力未验证。
- **部署场景单一**：数据集面向动态人类环境，但未包含极端天气、重度遮挡、高动态车辆等复杂交通场景。

（完）
