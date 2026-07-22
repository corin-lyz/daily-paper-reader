---
title: "SGS-3D: High-Fidelity 3D Instance Segmentation via Reliable Semantic Mask Splitting and Growing"
title_zh: SGS-3D：基于可靠语义掩码分裂与生长的高保真3D实例分割
authors: "Chaolei Wang, Yang Luo, Jing Du, Siyu Chen, Yiping Chen, Ting Han"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37931/41893"
tags: ["query:seg"]
score: 6.0
evidence: 利用深度约束的3D实例分割
tldr: 针对2D到3D提升方法因语义模糊和深度约束不足导致的实例分割误差累积问题，提出SGS-3D框架，先利用几何基元净化并分裂模糊的掩码，再将其生长为完整实例，显著提升分割保真度。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1746, \"height\": 414, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1727, \"height\": 923, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 822, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 750, \"height\": 637, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 812, \"height\": 419, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1831, \"height\": 849, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 842, \"height\": 361, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37931/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 835, \"height\": 251, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 870, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1810, \"height\": 690, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 275, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 496, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 378, \"height\": 261, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1849, \"height\": 396, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37931/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 855, \"height\": 139, \"label\": \"Table\"}]"
motivation: 现有2D-to-3D提升方法因语义模糊和深度约束不足导致实例分割精度低。
method: 提出“分裂-生长”框架：先用几何基元净化分裂语义掩码，再生长为完整实例。
result: 在多个3D场景数据集上达到最高分割精度，尤其对遮挡和薄结构更鲁棒。
conclusion: 几何先验和深度约束的结合有效提升了3D实例分割的准确性和完整性。
---

## Abstract
Accurate 3D instance segmentation is crucial for high-quality scene understanding in the 3D vision domain. However, 3D instance segmentation based on 2D-to-3D lifting approaches struggle to produce precise instance-level segmentation, due to accumulated errors introduced during the lifting process from ambiguous semantic guidance and insufficient depth constraints. To tackle these challenges, we propose Splitting and Growing reliable Semantic mask for high-fidelity 3D instance segmentation (SGS-3D), a novel "split-then-grow" framework that first purifies and splits ambiguous lifted masks using geometric primitives, and then grows them into complete instances within the scene. Unlike existing approaches that directly rely on raw lifted masks and sacrifice segmentation accuracy, SGS-3D serves as a training-free refinement method that jointly fuses semantic and geometric information, enabling effective cooperation between the two levels of representation. Specifically, for semantic guidance, we introduce a mask filtering strategy that leverages the co-occurrence of 3D geometry primitives to identify and remove ambiguous masks, thereby ensuring more reliable semantic consistency with the 3D object instances. For the geometric refinement, we construct fine-grained object instances by exploiting both spatial continuity and high-level features, particularly in the case of semantic ambiguity between distinct objects. Experimental results on ScanNet200, ScanNet++, and KITTI-360 demonstrate that SGS-3D substantially improves segmentation accuracy and robustness against inaccurate masks from pre-trained models, yielding high-fidelity object instances while maintaining strong generalization across diverse indoor and outdoor environments.

---

## 论文详细总结（自动生成）

## 中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有基于2D-to-3D提升（lifting）的3D实例分割方法，由于从2D语义掩码提升到3D点云时存在语义模糊（如视差导致的错误投影）和深度约束不足（深度传感器在无纹理/高反光表面失效，或室外场景缺乏深度信息），导致误差累积，无法生成精确的实例级分割。
- **研究动机**：期望利用2D视觉基础模型（如SAM、Grounding-DINO）的强大零样本能力，但直接投影会引入噪声。因此需要一种训练无关的细化方法，联合融合语义和几何信息，实现高保真3D实例分割。
- **整体含义**：提出了“分裂-生长”框架SGS-3D，通过先净化（净化模糊掩码）、再分裂（利用几何连续性分离粘连实例）、最后生长（特征引导合并）的流程，有效解决了误差累积问题，在室内外场景均取得SOTA性能。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：采用“分裂-生长”（split-then-grow）策略，分三个阶段处理：
  1. **点-图像映射（Point-Image Mapping）**：通过Z-buffer算法从点云和相机参数计算深度图（无需真实深度），确定每个3D点是否在图像中可见，建立点-像素对应关系。关键公式：深度缓冲 \( D_t(u,v) = \min\{z_i | p_i \in P, \pi(p_i) = (u,v)\} \)，可见性 \( V_t(i) = \mathbb{1}(\text{frustum}) \cdot \mathbb{1}(|z_i - D_t(u_i,v_i)| \leq \tau_{\text{vis}}) \)。
  2. **2D掩码提议（2D Mask Proposal）**：
     - 使用Grounding-DINO生成目标框，再通过SAM生成候选2D掩码。
     - **共生掩码过滤（Co-occurrence Mask Filtering）**：基于超点可见性，计算每个掩码与其他掩码的平均共现分数（公式4），过滤掉低分（歧义）掩码，避免错误传播。该步骤实现4倍加速。
  3. **语义引导聚合（Semantic-Guided Aggregation）**：
     - **空间连续性分裂（Spatial Continuity Splitting）**：将每个3D语义掩码通过HDBSCAN密度聚类分裂为语义-几何种子，解决外观相似但空间分离的对象误判问题。
     - **单视图特征引导生长（Single-View Feature-Guided Growing）**：亲和力分数 = 余弦相似度 × 空间重叠IoU，迭代合并相邻超点，直到亲和力不再增加。
     - **多视图渐进合并（Multi-View Progressive Merging）**：初始高重叠阈值合并可靠片段，逐渐降低阈值合并松散部分，完成跨视图融合。

### 3. 实验设计
- **数据集**：
  - 室内：ScanNet200, ScanNet++
  - 室外：KITTI-360（无深度传感器，仅RGB图像）
- **基准设置**：遵循主流协议，评估指标为mAP（50%-95% IoU，步长5%）、AP@50、AP@25。
- **对比方法**：
  - 训练无关方法：SAM3D, Open3DIS, SAI3D, SAM2Object, HDBSCAN, Felzenszwalb等；
  - 训练依赖方法：UnScene3D, Segment3D, SAM-graph等。
- **额外实验**：消融组件（CMF, SCP, FGG）、不同2D模型（Cropformer, SAM, YoloW-SAM, GD-SAM）、输入图像比例（10%, 5%, 2.5%）、共现阈值、遮挡鲁棒性、开放词汇分割应用等。

### 4. 资源与算力
- **论文未明确说明**使用的GPU型号、数量、训练时长。由于SGS-3D是**训练无关**（training-free）框架，仅需推理，计算资源需求较低（文中指出在2.5%图像输入下每场景约2.42秒，12.2倍加速于SAI3D），但未提供具体硬件平台细节。

### 5. 实验数量与充分性
- **实验数量**：共包含三大主要数据集上的主实验（表1）、消融实验（表2，3个组件+组合共5组）、2D模型对比（表5，4组）、共现阈值（表6，4组）、输入图像比例（表4，3组+Open3DIS对照）、遮挡鲁棒性（表7，8组）、开放词汇分割（表3，与多个零样本方法对比）。总计约20+个实验组。
- **充分性与客观性**：
  - **充分**：覆盖室内外、有/无深度、不同遮挡程度、不同2D基础模型、不同输入密度。
  - **公平**：与所有训练无关方法在同一协议下对比；使用GT深度时也给出了上限结果；消融实验清晰展示了每个组件的贡献。
  - **客观**：代码已开源，结果可复现。

### 6. 论文的主要结论与发现
- SGS-3D在无深度场景（KITTI-360）上比次优方法（SAI3D）mAP提升+16.4%，AP@50提升+13.5%，证明其几何-语义融合策略对缺乏深度的环境极为有效。
- 在有深度的室内场景（ScanNet200）上，无需GT深度即超越所有训练无关方法；加上GT深度后达到新天花板（mAP 34.3%）。
- 仅需2.5%的图像即可达到甚至超越Open3DIS使用10%图像的性能，且速度提升约4倍，实例数减少40%（减少过分割）。
- 开放词汇分割性能显著优于此前零样本方法（mAP 21.1% vs. SOTA 13.3%），源于更精确的实例掩码减少了语义歧义。

### 7. 优点
- **训练无关**：无需任何3D训练数据，直接利用2D基础模型，泛化性强。
- **高效**：通过共生过滤实现约4倍计算加速，仅需少量视图即可得到高质量结果。
- **鲁棒性**：对遮挡（高达50%掩码移除仍保持性能）、不同2D模型、超参数不敏感。
- **普适性**：同时适用于室内（有深度）和室外（无深度）场景，无需传感器深度。
- **创新性**：首次将“分裂-生长”思想应用于3D实例分割，有效解决语义歧义和过分割问题。

### 8. 不足与局限
- **算力资源未披露**：文中未说明具体GPU型号、内存消耗等，不利于公平复现和资源预估。
- **依赖超参数**：共现阈值 \( c_m \) 虽在一定范围内鲁棒（0.2-0.4），但极端值仍影响性能。
- **多阶段设计**：流程包含多个步骤（映射、过滤、分裂、生长、合并），端到端延迟可能较高，未讨论实时应用场景。
- **动态场景缺失**：仅在静态场景上验证，未扩展到动态（含运动物体）或流式点云数据。
- **遮挡评估局限**：模拟遮挡仅使用随机遮掩，未考虑真实传感器噪声或复杂光照变化。
- **未处理小物体**：实验中未专门分析对细小/稀疏物体的分割质量，可能受超点粒度影响。

（完）
