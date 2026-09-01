---
title: "MonoMixer: Marrying Convolution and Vision Transformer for Efficient Self-Supervised Monocular Depth Estimation"
title_zh: MonoMixer：融合卷积与视觉Transformer的高效自监督单目深度估计
authors: "(PDF |   Details)"
date: 2025-08-01
pdf: "https://www.ijcai.org/proceedings/2025/0085.pdf"
tags: ["query:mono-depth"]
score: 8.0
evidence: 结合卷积与Transformer的高效自监督单目深度估计
tldr: 自监督单目深度估计在效率与精度之间需要更好平衡。本文提出MonoMixer，将卷积与视觉Transformer以混合方式融合，用于高效自监督深度估计。该方法在保持较高精度的同时显著降低计算开销，适合资源受限场景。
source: IJCAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-85/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 514, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-85/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1726, \"height\": 1037, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-85/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1831, \"height\": 529, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-85/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1738, \"height\": 444, \"label\": \"Figure\"}, {\"url\": \"assets/figures/ijcai-2025-accepted/ijcai-2025-85/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 842, \"height\": 280, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1812, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 788, \"height\": 260, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 823, \"height\": 239, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 839, \"height\": 251, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 864, \"height\": 168, \"label\": \"Table\"}, {\"url\": \"assets/tables/ijcai-2025-accepted/ijcai-2025-85/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 787, \"height\": 323, \"label\": \"Table\"}]"
motivation: 现有自监督单目深度估计模型往往计算量大，难以在效率与精度间取得平衡。
method: 设计卷积与视觉Transformer的混合架构，结合局部与全局特征提取，实现高效自监督训练。
result: 在多个基准上以更低的计算成本达到有竞争力的精度。
conclusion: 为高效自监督单目深度估计提供了一种轻量而有效的混合架构方案。
---

## Abstract
No abstract is available.

---

## 论文详细总结（自动生成）

# MonoMixer：融合卷积与视觉Transformer的高效自监督单目深度估计

## 1. 核心问题与研究动机

- **背景**：单目深度估计（MDE）在自动驾驶、AR、机器人导航等领域具有基础性意义。全监督方法依赖昂贵的LiDAR真值标注，而自监督方法仅利用单目视频序列的几何约束即可训练，成本更低、更受关注。
- **现有挑战**：
  - 纯CNN方法受限于局部感受野，难以推理全局空间关系，对复杂场景的整体布局感知能力不足。
  - Transformer方法（如MonoViT、MonoFormer等）虽能捕获长程依赖，但其MHSA（多头自注意力）的二次复杂度导致参数量与延迟大幅上升，难以部署到边缘设备。
  - 已有轻量级混合架构（如Lite-Mono）仍与重量级网络存在精度差距。
- **核心目标**：设计一种**参数量极小、推理速度快**的CNN-Transformer混合架构，在自监督单目深度估计中同时取得优异的精度与效率。

## 2. 方法论：MonoMixer架构

整体采用编码器-解码器结构（DepthNet）+ 轻量级PoseNet，三大核心模块如下：

### 2.1 全局引导Transformer（G2T）块——捕获全局上下文
- 采用MBConv块构建五阶段特征金字塔，提取多尺度局部特征。
- 对各阶段特征做平均池化（统一到相同空间尺寸）后沿通道维度拼接，得到**全局语义Token G**。
- 在Transformer块中，以局部特征 \(F_n\) 作为Query、全局Token G作为Key/Value，进行**跨注意力（Cross-Attention）**计算，以线性复杂度将全局信息注入各尺度局部特征：
  \[
  Q_o = \text{Softmax}\left(\frac{Q_f K_g^\top}{\sqrt{d}}\right)V_g
  \]
- 随后通过FFN（两个1×1卷积+GELU）与BatchNorm进行特征精炼。
- **关键优势**：相比MHSA，跨注意力避免了二次计算复杂度，同时为浅层特征补充了足够的全局语义信息。

### 2.2 细节增强（DA）块——增强局部细节
- 针对CNN卷积操作固有局部性导致物体细节丢失、且无法建模场景拓扑结构的问题，引入**图卷积（GCN）**。
- DA块包含三个分支：水平混合、垂直混合、线性投影分支。以水平方向为例：
  1. **像素聚类（Pixel Cluster）**：通过可学习变换矩阵 \(W_{p1}\) 将像素特征映射到图域，每个节点代表一个隐式的视觉中心。
  2. **信息传播（Information Propagation）**：利用单层图卷积进行传播，公式为 \(Y_h = \sigma(((E - A_{g1})B_h)W_{g1})\)，其中 \(E - A_{g1}\) 实现了拉普拉斯平滑。
  3. **节点投影（Node Projection）**：使用 \(W_{p1}\) 的转置将图域特征映射回几何域。
- 最后将水平、垂直、线性投影三个分支的输出逐元素相加并经1×1卷积融合，实现对物体边界和细长结构的精细恢复。

### 2.3 自调制通道注意力（SMCA）块——建模通道间全局依赖
- 将全局语义Token G重塑为 \(R^{C \times HW}\)，通过通道池化得到全局通道图Q，线性映射生成K和V。
- 计算Q与K^T的矩阵乘法，得到全局上下文分数S。
- 将S分为g组后，通过**MLPMixer**（两段MLP：第一段实现组间信息交换，第二段进行通道混合）对通道特征进行更新与调制。
- 经Softmax生成通道注意力图 \(A_c\)，最终输出为 \(G' = V \odot A_c\)。

### 2.4 训练目标
- 采用图像重建框架：利用预测深度 \(D_t\)、相对位姿 \(P_{t\to s}\) 和相机内参K，将源图像 \(I_s\) 渲染为合成目标图像 \(\hat{I}_t\)。
- 损失函数包括：
  - 光度重建损失（SSIM + L1）
  - 边缘感知平滑损失
  - 总损失 \(L = L_d + \alpha L_s\)

## 3. 实验设计

### 3.1 数据集与评估基准
| 数据集 | 用途 | 训练方式 |
|--------|------|----------|
| **KITTI** | 主基准，评估深度估计精度 | 单目视频训练（存在M和M*两种输入分辨率：640×192和1024×320） |
| **Make3D** | 零样本跨数据集泛化评估 | 仅在KITTI上训练，直接测试 |
| **Cityscapes** | 跨场景泛化评估 | 从零在Cityscapes上训练并测试 |

- **评估指标**：7项标准指标——AbsRel、SqRel、RMSE、RMSElog（越低越好），以及δ<1.25、δ<1.25²、δ<1.25³（越高越好）。

### 3.2 对比方法（共12种）
Monodepth2、HR-Depth、Lite-HR-Depth、MonoViT、MonoFormer、ROIFormer、Lite-Mono、DaCCN、R-MSFMX3、AQUANet、SQLdepth、RPrDepth，涵盖CNN、Transformer、混合架构以及轻量级方法。

## 4. 资源与算力

- 论文明确说明：使用**单张NVIDIA RTX 3090 GPU**训练，共训练**20个epoch**，batch size为**12**，优化器为Adam（β1=0.9, β2=0.999）。
- 推理效率评估在**NVIDIA TITAN Xp**和**Jetson Xavier**（边缘设备）上进行。
- 模型规模：**3.9M参数，4.1G FLOPs**，在TITAN Xp上推理速度2.6ms，在Jetson Xavier上10.5ms。
- 未说明训练总时长、能耗等更细粒度的计算资源消耗指标。

## 5. 实验数量与充分性分析

### 5.1 实验规模
- **主实验**：KITTI上两种分辨率（640×192 / 1024×320）的各指标对比。
- **泛化实验**：Make3D零样本测试（4项指标）、Cityscapes从零训练测试（4项指标）。
- **效率对比**：6种模型的参数量、FLOPs、两种硬件上的推理延迟对比。
- **整体消融**：移除G2T/DA/SMCA块各自的效果（表5）。
- **核心模块内部分量消融**（表6）：
  - G2T：无Transformer / 用自注意力替代跨注意力
  - DA：分别移除水平分支、垂直分支、恒等分支
  - SMCA：移除MLPMixer

### 5.2 充分性评价
**总体充分且客观**，理由如下：
- 数据集覆盖了主benchmark（KITTI）、跨域泛化（Make3D）、复杂城市场景（Cityscapes），角度全面。
- 消融实验覆盖了每个核心模块的功能验证及模块内部设计的必要性验证（共10组消融），逻辑清晰、归因明确。
- 与12种代表性方法进行对比，并报告了标准7项指标，同设置下公平对照。
- 在边缘设备（Jetson Xavier）上实测速度，贴近实际部署场景。

**但存在以下可拓展之处**：
- 消融实验只报告了单分辨率（640×192）的结果，未验证在高分辨率下的模块贡献是否一致。
- 未以图表形式展示训练收敛曲线或训练耗时，读者难以评估训练效率的优劣。
- 效率对比中DIFFNet仅出现在测速表而未出现在精度对比表中，跨表对比的完整性略有不足。

## 6. 主要结论与发现

- **精度大幅领先**：在KITTI上，MonoMixer在640×192下取得AbsRel=0.081、RMSE=4.039、δ<1.25=0.923；在1024×320下进一步达到AbsRel=0.076、RMSE=4.018、δ<1.25=0.929，大幅超过MonoFormer、SQLdepth等SOTA方法。
- **极致轻量化**：仅3.9M参数，比MonoFormer（23.9M）减少约**95%**，比SQLdepth（34.0M）少约88%，此外推理速度在所有对比模型中也是最快（TITAN Xp上2.6ms）。
- **每个模块均有效**：
  - G2T块贡献最大（移除后AbsRel从0.081升至0.099），验证了全局上下文对自监督深度估计的关键作用；
  - 跨注意力优于自注意力（0.086 vs 0.091），证明设计选择正确；
  - DA块仅增加0.004M参数即可带来显著精度提升，验证了图推理对局部细节增强的有效性；
  - SMCA块在几乎不增加参数的前提下提升全部指标。
- **跨数据集泛化能力强**：Make3D上AbsRel=0.282、Cityscapes上AbsRel=0.103，均优于同期SOTA。

## 7. 优点

- **架构设计上的创新与实用性兼顾**：三个模块（G2T、DA、SMCA）各有明确针对的问题（全局语义、局部细节、通道依赖），分工清晰，可借鉴到其他密集预测任务。
- **计算复杂度控制出色**：通过平均池化构造统一的全局Token实现跨尺度信息交互，将Transformer计算量压缩至近线性复杂度，打破了Transformer难以轻量化部署的瓶颈。
- **效率-精度平衡优异**：在参数量、FLOPs、延迟、精度四个维度上全面胜过现有方法，具备实际部署价值。
- **定性结果验证充分**：展示了在道路标志杆、近距离运动物体等挑战场景下的可视化对比，直观体现DA块对细长结构的增强效果。
- **消融实验严谨**：不仅做了模块级消融，还深入到模块内部组件级验证（如分支移除、自注意力替代、MLPMixer移除等），证据链完整。

## 8. 不足与局限

- **实验多样性有限**：消融实验仅在KITTI单分辨率下进行，未覆盖高分辨率、不同天气/光照条件（夜晚、雨天）以及非道路场景（室内）等。
- **输入分辨率未多样化**：尽管在两种分辨率上做了评测，但没有展示模型对更大分辨率（如1920×1080）的扩展能力。
- **未提及局限与失败案例**：论文没有讨论模型在哪些场景下会失效（如无纹理区域、反射表面、遮挡严重的动态场景等）。
- **训练细节披露不足**：未报告单epoch训练时间、总训练成本、内存占用等，复现者难以精确预估资源需求。
- **与全监督方法未做对比**：未报告在标准深度评估协议下与全监督方法（如BTS、LapDepth等）的差距，对模型天花板位置的定位不够完整。
- **表1中MonoMixer在640×192设置中的RMSElog（0.068）和δ<1.25²（0.984）明显偏离同类指标分布，疑似排版/打印错误**，但论文未对此做说明，可能影响读者对该行数据的信任度。

（完）
