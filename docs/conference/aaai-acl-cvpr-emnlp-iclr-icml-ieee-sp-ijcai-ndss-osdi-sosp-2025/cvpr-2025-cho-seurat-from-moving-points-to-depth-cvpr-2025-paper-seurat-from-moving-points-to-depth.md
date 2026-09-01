---
title: "Seurat: From Moving Points to Depth"
title_zh: Seurat：从运动点到深度
authors: "Cho, Seokju, Huang, Jiahui, Kim, Seungryong, Lee, Joon-Young"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Cho_Seurat_From_Moving_Points_to_Depth_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 利用跟踪点轨迹从单目视频预测相对深度
tldr: 针对单目视频深度估计因缺少立体线索而存在歧义的问题，提出Seurat方法，使用现成点跟踪模型获取2D轨迹，再利用空间和时间Transformer处理轨迹并直接推断相对深度随时间的变化。在TAPVid-3D基准上展示了稳健的零样本性能，该方法无需训练新特征即可从纯轨迹信息中恢复相对深度顺序，验证了从运动点轨迹估计相对深度的有效性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1704, \"height\": 470, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 957, \"height\": 644, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 1410, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1803, \"height\": 437, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1794, \"height\": 936, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 622, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 859, \"height\": 238, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 870, \"height\": 130, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 865, \"height\": 144, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cho-seurat-from-moving-points-to-depth-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 873, \"height\": 178, \"label\": \"Table\"}]"
motivation: 单目视频缺乏立体线索，深度估计存在歧义。
method: 使用点跟踪模型捕获2D轨迹，用空间和时间Transformer推断相对深度变化。
result: 在TAPVid-3D基准上零样本性能稳健。
conclusion: 运动点轨迹可作为单目相对深度估计的有效线索。
---

## Abstract
Accurate depth estimation from monocular videos remains challenging due to ambiguities inherent in single-view geometry, as crucial depth cues like stereopsis are absent. However, humans often perceive relative depth intuitively by observing variations in the size and spacing of objects as they move. Inspired by this, we propose a novel method that infers relative depth by examining the spatial relationships and temporal evolution of a set of tracked 2D trajectories. Specifically, we use off-the-shelf point tracking models to capture 2D trajectories. Then, our approach employs spatial and temporal transformers to process these trajectories and directly infer depth changes over time. Evaluated on the TAPVid-3D benchmark, our method demonstrates robust zero-shot performance, generalizing effectively from synthetic to real-world datasets. Results indicate that our approach achieves temporally smooth, high-accuracy depth predictions across diverse domains.

---

## 论文详细总结（自动生成）

## Seurat: From Moving Points to Depth 论文深度解析

### 1. 核心问题与研究动机

- **问题背景**：单目视频深度估计长期受制于**单视图几何的高度歧义性**。关键深度线索（如双目视差）在单目设定下完全缺失，而现有多数方法依赖大规模标注数据、强特征骨干网络或额外传感器来弥补这一缺口。
- **核心观察**：人类在观看视频时，能仅凭物体运动过程中**大小和间距的变化**直觉判断远近——这一现象说明运动本身蕴含丰富的深度线索。
- **灵感来源**：结构光 3D 扫描通过投射已知图案并观察其形变来恢复深度；作者提出类比观点——在单目视频中，**被跟踪 2D 点的轨迹模式**本质上就是“自然投射的图案”，其空间分布随时间的变化可揭示场景的 3D 结构。
- **核心主张**：仅从 2D 点轨迹出发，无需立体/多视角装置、额外传感器、预训练特征骨干或大规模真实标注数据，即可推断相对深度随时间的变化——这一视角在以往工作中未被系统验证过。

### 2. 方法论

#### 2.1 核心思想

- 给定 T 帧单目视频，用现成点跟踪器（CoTracker / LocoTrack）提取 N 条轨迹 {pᵢ} 及遮挡状态 {vᵢ}。
- 模型预测每条轨迹 i 在帧 t 相对参考帧 t₀ 的**深度比** rᵢ,ₜ = dᵢ,ₜ / dᵢ,ₜ₀，而非绝对深度。
- 最终通过与现成的单目深度估计器（ZoeDepth / DepthPro）进行**分段尺度匹配**，将相对深度比恢复为度量深度。

#### 2.2 理论基础

- 引入针孔相机模型下的**投影密度与深度关系推导**。地表面积为 A_surface 的局部面片，其投影面积满足：
  - A_image = (f/dₜ)² · A_surface · cos θₜ
- 由此推出深度比与投影点密度的关系式：
  - rₜ = dₜ/dₜ₀ = √(ρ_imageᵗ / ρ_imageᵗ₀) · √(cos θₜ₀ / cos θₜ)
- 作者指出，**确定性的手算方案**因需要精确的旋转角度与局部刚性假设，在实际中不可靠（实验表 4 证实）；故改用 Transformer 隐式学习这些几何关系，对运动动态更具鲁棒性。

#### 2.3 网络架构（双分支 Transformer）

| 模块 | 作用 |
|---|---|
| **支持轨迹分支** | 处理 24×24 均匀网格采样的密集轨迹，捕捉全场景运动上下文。交替堆叠时间注意力+空间注意力层，编码全局运动信息。 |
| **查询轨迹分支** | 处理用户/数据集给定的稀疏查询轨迹；每条查询轨迹**独立处理**，避免查询点分布不均造成的偏置。通过交叉注意力从支持分支注入全局运动信息。 |
| **回归头** | 两个分支末端各接深度比预测头，迭代细化预测结果。 |

**设计精髓**：双分支解耦——支持轨迹的全局上下文只通过交叉注意力传递给查询分支，防止稀疏查询分布污染场景运动建模。

#### 2.4 滑动窗口训练与推理

- 整段视频一起处理会导致长程运动模式复杂、支持轨迹移出画面，预测不稳定。
- 采用**窗口长度 W=8、窗口步长 S** 的滑动窗口方案；查询轨迹跨窗口持续存在，支持轨迹每个窗口重新初始化。
- **窗口式对数深度比损失**：预测每个窗口内相对于窗口首帧的 log 深度比，使用 L1 损失，对绝对尺度不变，聚焦时间相对变化。
- 推理时将跨窗口的对数深度比累加再取指数，恢复全序列深度比。

### 3. 实验设计

- **基准与数据集**：
  - **训练**：Kubric MOVi-F 合成数据集生成器生成 90,000 个训练样本，纯合成数据，无预训练骨干。
  - **评估**：**TAPVid-3D minival 基准**，涵盖三个场景——Aria（自我中心视角）、DriveTrack（驾驶场景）、PStudio（工作室多视角捕捉，含形变）。
- **评估指标**：3D-AJ（位置+遮挡综合精度）、APD（深度自适应阈值的平均位置精度，δ=1,2,4,8,16）、TC（时间一致性，预测与真值加速度的 L2 距离）。
- **对比方法**：
  - 逐帧深度估计器路线：ZoeDepth、DepthPro 分别搭配 CoTracker / LocoTrack，将 2D 轨迹通过每帧深度图反投影到 3D。
  - 视频深度估计器路线：DepthCrafter、ChronoDepth 搭配相同点跟踪器。
  - 刻意规避了在评估数据上训练过的模型（如使用 Waymo 训练的 UniDepth），保证公平性。
- **评价协议**：表 1 按轨迹独立缩放评估时间深度变化精度；表 2 使用视频级单尺度+单偏移全局对齐，评估整段视频深度一致性——两种协议从不同角度检验方法性能。

### 4. 资源与算力

- **GPU**：8 × NVIDIA RTX 3090
- **批次大小**：每 GPU 1，总批次 8
- **训练步数**：100,000 步
- **优化器**：AdamW，学习率 5×10⁻⁴，权重衰减 1×10⁻⁵，1000 步预热，线性衰减
- **模型规模**：L=2 层 Transformer，隐藏维度 384，8 个注意力头
- **说明**：论文未披露具体训练时长（小时数），仅给出步数与硬件配置；整体算力需求在同类方法中属于中等偏轻量级。

### 5. 实验数量与充分性评估

| 实验类型 | 内容 | 作用 |
|---|---|---|
| 主实验（表 1） | 3 个数据集 × 3 指标 × 多基线 | 验证方法有效性 |
| 主实验（表 2） | 仿射不变深度对齐评估 | 对比视频深度估计器 |
| 消融 A（表 3 左） | 双分支设计、滑动窗口、窗口式损失逐项剔除 | 验证核心设计贡献 |
| 消融 B（表 3 右） | 层数 L=1/2/3/4 | 确定最佳模型容量 |
| 基线对比（表 4） | 手工实现密度公式（Eq. 4） vs 本文方法 | 证明学习方式优于确定性计算 |
| 输入分析（表 5） | 加入 RGB 纹理补丁 | 检验纹理是否有助于深度推理 |
| 对照实验（表 6） | 简单高斯平滑 vs Seurat | 排除“仅在做平滑”的质疑 |

**充分性分析**：
- 实验覆盖全面：从架构消融到输入模态分析再到与朴素平滑的对照，逻辑链条完整。
- 零样本泛化验证扎实：纯合成训练 → 真实数据集评估，覆盖室内外、自我中心、驾驶、形变等多种场景。
- 公平性较好：刻意选用与评估集无重叠的 MDE 模型做基线，避免数据泄漏。
- 潜在不足：所有评估局限于 TAPVid-3D 单一基准；未与更近期的轨迹到 3D 方法（如 TrackTo4D）做定量比较。

### 6. 主要结论与发现

- **轨迹包含可提取的深度线索**：从 2D 运动轨迹可直接推断相对深度变化，性能显著超越“点跟踪+单帧深度估计”的朴素组合。
- **零样本泛化能力强**：仅用合成数据训练，无需真实深度标注，即可在真实世界数据集获得领先效果。
- **时间一致性优势突出**：尤其在 DriveTrack 数据集上，TC 指标比基线提升超过 40 倍——说明轨迹方法天然比逐帧深度估计器更具时序稳定性。
- **每个设计决策都至关重要**：双分支解耦（+4.3 3D-AJ）、滑动窗口（+9.2）、窗口式日志比损失（+3.1）均显著贡献性能。
- **简单平滑无法替代真正的几何一致性**：高斯平滑反而损坏精度，证明 Seurat 学到的是时序几何关系而非表面平滑。
- **纹理信息反而有害**：加入 RGB 补丁导致性能下降，提示轨迹几何信息量已足够，额外纹理可能引入合成域过拟合。

### 7. 优点

- **新颖的研究视角**：将结构光思想类比到点轨迹分析，为单目深度估计开辟了一条不依赖外观特征的新路线。
- **简洁且高效**：无需任何预训练视觉骨干（不依赖 DINOv2 或 Stable Diffusion 等大模型），架构轻量，训练成本可控。
- **理论支撑明确**：从针孔相机的投影密度公式出发提供数学动机，同时用实验证明纯手工计算不可行、学习方法是必要路径。
- **双分支解耦设计巧妙**：网格支持轨迹提供全局上下文，查询轨迹保持独立——既避免偏置又保持全局感知。
- **零样本泛化令人信服**：从合成到真实的跨越验证了轨迹几何线索的域不变性，这一强泛化能力具有实际应用价值。
- **评估严谨**：排除与基准数据重叠的 MDE 模型，采用两种评估协议，全程关注位置精度与时间一致性双重维度。

### 8. 不足与局限

- **强依赖点跟踪质量**：输入完全依赖上游 2D 点跟踪器；跟踪失败或漂移时，深度预测质量无从保证，论文未分析跟踪误差对深度精度的鲁棒性边界。
- **仅输出相对深度比**：无法独立输出度量深度，必须与现代 MDE 模型配合做尺度匹配；最终精度耦合于 MDE 的空间深度质量。
- **基准覆盖单一**：全部量化评估集中在 TAPVid-3D；与其他 monocular video depth 方法的标准评测集（如 Sintel、KITTI 深度基准）缺乏交叉验证。
- **与最新轨迹方法缺少对比**：未与 SpatialTracker、TrackTo4D 等轨迹到 3D 的近期工作做定量比较，方法的相对定位不够完整。
- **支持轨迹窗口化重初始化**：每窗口重新初始化支持网格，长视频中跨窗口的一致性依赖查询轨迹的拼接；快速运动或频繁遮挡下可能累积误差。
- **缺少失败案例分析**：论文未讨论在何种场景（如严重遮挡、极端运动模糊、低纹理区域）下方法会失效。
- **纹理加入反而降性能**：暗示模型对合成域外观过拟合的风险，真实应用中若需扩展域可能需要额外的域自适应措施。
- **查询轨迹间缺乏交互**：每条查询轨迹被独立处理，无法利用查询点之间的空间关系（如表 3 消融 II 所示，联合处理虽有偏置但可能包含有用信息）。

---

（完）
