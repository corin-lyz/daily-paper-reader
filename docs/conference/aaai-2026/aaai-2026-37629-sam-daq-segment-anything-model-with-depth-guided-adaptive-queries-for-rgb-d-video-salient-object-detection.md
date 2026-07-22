---
title: "SAM-DAQ: Segment Anything Model with Depth-guided Adaptive Queries for RGB-D Video Salient Object Detection"
title_zh: SAM-DAQ：基于深度引导自适应查询的SAM用于RGB-D视频显著目标检测
authors: "Jia Lin, Xiaofei Zhou, Jiyuan Liu, Runmin Cong, Guodao Zhang, Zhi Liu, Jiyong Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37629/41591"
tags: ["query:vfm"]
score: 6.0
evidence: 用深度引导改进SAM基础模型用于视频分割
tldr: 针对RGB-D视频显著目标检测任务，现有SAM直接应用面临人工提示依赖、内存消耗大等问题。本文提出SAM-DAQ，通过深度引导自适应查询机制将深度与时间线索无缝集成到SAM2中，无需手动标注，大幅降低了顺序适配器和记忆注意力的计算负担。实验表明该方法在多个基准上取得优异性能，为视频分割中利用深度先验提供了新思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37629/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1834, \"height\": 517, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37629/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1723, \"height\": 995, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37629/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1465, \"height\": 978, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37629/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 847, \"height\": 366, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1813, \"height\": 609, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1538, \"height\": 305, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 859, \"height\": 223, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 847, \"height\": 259, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 261, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37629/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 381, \"label\": \"Table\"}]"
motivation: 直接应用SAM到RGB-D视频显著目标检测存在依赖人工提示、高内存消耗和计算负担三大挑战。
method: 提出深度引导自适应查询机制，将深度和时间线索集成到SAM2中，实现无需手动提示的端到端检测。
result: 在多个RGB-D视频显著目标检测基准上达到领先性能，同时降低计算开销。
conclusion: 深度引导的自适应查询有效提升了基础模型在视频分割任务中的适用性和效率。
---

## Abstract
Recently segment anything model (SAM) has attracted widespread concerns, and it is often treated as a vision foundation model for universal segmentation. Some researchers have attempted to directly apply the foundation model to the RGB-D video salient object detection (RGB-D VSOD) task, which often encounters three challenges, including the dependence on manual prompts, the high memory consumption of sequential adapters, and the computational burden of memory attention. To address the limitations, we propose a novel method, namely Segment Anything Model with Depth-guided Adaptive Queries (SAM-DAQ), which adapts SAM2 to pop-out salient objects from videos by seamlessly integrating depth and temporal cues within a unified framework. Firstly, we deploy a parallel adapter-based multi-modal image encoder (PAMIE), which incorporates several depth-guided parallel adapters (DPAs) in a skip-connection way. Remarkably, we fine-tune the frozen SAM encoder under prompt-free conditions, where the DPA utilizes depth cues to facilitate the fusion of multi-modal features. Secondly, we deploy a query-driven temporal memory (QTM) module, which unifies the memory bank and prompt embeddings into a learnable pipeline. Concretely, by leveraging both frame-level queries and video-level queries simultaneously, the QTM module can not only selectively extract temporal consistency features but also iteratively update the temporal representations of the queries. Extensive experiments are conducted on three RGB-D VSOD datasets, and the results show that the proposed SAM-DAQ consistently outperforms state-of-the-art methods in terms of all evaluation metrics.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究任务**：RGB-D 视频显著目标检测（RGB-D VSOD），旨在同时利用深度信息（空间结构）和时间信息（帧间依赖）从视频中检测最吸引人的物体。
- **现有挑战**：直接应用视觉基础模型 SAM2 到该任务面临三大障碍：
  - **依赖人工提示**：SAM2 推理时需要手工标注的点、框等提示，而 VSOD 任务无法提供。
  - **顺序适配器的高内存消耗**：以往采用顺序式适配器（adapter）微调编码器，反向传播需穿越整个编码器，导致显存激增。
  - **记忆注意力的计算负担**：SAM2 的记忆注意力机制需要在当前帧特征与大型记忆库之间做大量相关性计算，复杂度高。
- **本文目标**：将 SAM2 适配到 RGB-D VSOD，通过深度和时间线索的有机整合，实现**无人工提示、低内存、低计算**的显著目标检测。

## 2. 论文提出的方法论

### 核心思想
提出 **SAM-DAQ**（Segment Anything Model with Depth-guided Adaptive Queries），在冻结的 SAM2 编码器基础上，设计两个关键模块：

- **PAMIE（并行适配器多模态图像编码器）**：用于多模态融合与高效微调。
- **QTM（查询驱动时间记忆模块）**：用于时序建模并替代手工提示。

### 关键技术细节

#### （1）PAMIE
- **结构**：在每个 Hiera 块后并行插入轻量适配器（DPA，Depth-guided Parallel Adapter），以跳跃连接方式绕过原编码器。
- **RGB 流**：将当前层 RGB 特征与对应层深度特征拼接后送入适配器，输出与 Hiera 块输出相加。
- **深度流**：深度图像先经线性投影对齐到 RGB 空间，然后同样经并行适配器。
- **效果**：无需解冻编码器即可融合多模态信息，且梯度仅需流经轻量适配器，大幅降低训练显存。
- **公式示意**（Eq.1–2）：
  - 深度适配器：\(\tilde{F}_D^{i-1} = \text{Adapter}(F_D^{i-1}), \quad F_D^i = \text{Hiera}_i(F_D^{i-1}) + \text{DS}(\tilde{F}_D^{i-1})\)
  - RGB 适配器：\(\tilde{F}_{RGB}^{i-1} = \text{Adapter}(\text{Cat}(F_{RGB}^{i-1}, F_D^{i-1})), \quad F_{RGB}^i = \text{Hiera}_i(F_{RGB}^{i-1}) + \text{DS}(\tilde{F}_{RGB}^{i-1})\)
- **输出**：通过 FPN 生成三级图像嵌入 \(E_I = \{E_i^I\}_{i=2}^4\)，并对最高层使用中间监督。

#### （2）QTM（查询驱动时间记忆模块）
- **设计动机**：消除手工提示和大内存库，用可学习查询统一时序建模与提示生成。
- **查询类型**：
  - **帧级查询** \(Q_f\)：与当前帧最高层图像嵌入 \(E_4^I\) 交互，提取显著相关帧嵌入。
  - **视频级查询** \(Q_v\)：通过交叉注意力与帧嵌入交互，生成增强后的视频级查询 \(\tilde{Q}_v\)，再与 \(E_4^I\) 元素相乘得到可学习嵌入 \(E_L\)。
- **更新机制**：利用当前帧的记忆特征（由记忆编码器和线性投影得到）通过交叉注意力、自注意力、FFN 迭代更新视频级查询，用于下一帧。
- **优势**：避免手动提示，且将记忆交互从 O(T×h×w) 降低到 O(N_v×N_f)，N_v、N_f 远小于帧尺寸。

#### 损失函数
- 总损失 \(L_{\text{total}} = L_{\text{pred}} + \alpha L_{\text{inter}}\)，分别监督最终预测图和中间语义嵌入（对最高级图像嵌入使用 BCE 损失）。

## 3. 实验设计

- **数据集**：三个 RGB-D VSOD 基准数据集：
  - RDVS（4087 帧，57 个视频）
  - ViDSOD-100（9362 帧，100 个视频）
  - DViSal（7117 帧，237 个视频，仅使用有标签帧）
- **评估指标**：E-measure (\(E_\xi\))、S-measure (\(S_\alpha\))、F-measure (\(F_\beta\))、MAE（越低越好）。
- **对比方法**：11 种 SOTA 方法，包括 HRTransNet、PICRNet、DVSOD、LSTA、DPA、DCTNet+、ATFNet、MDSAM、SAM2-UNet、MFENet、KAN-SAM。
- **结果**：SAM-DAQ 在所有三个数据集上均取得最高分（如 RDVS 上 \(E_\xi=0.913\)，ViDSOD-100 上 \(E_\xi=0.918\)），显著优于第二名。

## 4. 资源与算力

- **GPU**：单张 NVIDIA RTX-3090（24 GB 显存）。
- **训练时长**：约 3 小时。
- **配置**：输入分辨率 1024×1024，每视频随机采样 10 帧，batch size=1，迭代 2000，优化器 AdamW（lr=0.0001, weight decay=0.05）。
- **参数量**：总参数量约 237.9 M，可训练参数量仅 19.2 M（由于冻结 SAM 编码器），训练显存仅 21.0 GB（远低于顺序适配器的 ~92 GB 和 LoRA 的 ~95 GB）。

## 5. 实验数量与充分性

- **主要对比**：在 3 个数据集上对比 11 种方法，结果全部列于表 1，定性对比见图 3（4 个场景）。
- **消融实验**：共 5 组（表 2–6，图 4）：
  - PAMIE 消融：去除深度投影仪、替换并行适配器为顺序/LoRA、去除多模态 → 表 2。
  - 可学习嵌入生成策略（稀疏/稠密/两者）→ 表 3。
  - 查询数量（视频级 1~15、帧级 1~60）→ 图 4。
  - 查询隐藏维度（32/64/128/256）→ 表 4。
  - QTM 更新机制（无、SAM2 原有记忆库、乘法/加法更新）→ 表 5。
  - 中间监督层级（仅 E4、E3+E4 等）→ 表 6。
- **充分性评价**：实验设计全面，涵盖了模块贡献、超参数影响和不同组件替代方案的比较。对比方法涵盖近期 SOTA，结果统计显著，消融实验验证了每个设计选择的必要性。客观公平。

## 6. 论文的主要结论与发现

- SAM-DAQ 在三个 RGB-D VSOD 数据集上全面超越现有 SOTA，验证了将基础模型（SAM2）适配到该任务的有效性。
- PAMIE 通过并行适配器实现冻结编码器的高效微调，显存消耗仅为顺序适配器的 1/4 左右，且多模态融合显著提升精度。
- QTM 用可学习查询替代手工提示和大记忆库，不仅降低了计算复杂度，还通过帧级与视频级查询的交叉注意力有效捕获时序一致性。
- 深度先验（depth projector + DPA）在 RGB-D 融合中起到关键作用。

## 7. 优点：方法或实验设计上的亮点

- **高效微调**：并行适配器设计使反向传播绕过重编码器，大幅降低显存，同时保持精度。
- **免提示集成**：QTM 首次将记忆库和提示嵌入统一为可学习查询，无需手工标注即可驱动 SAM2 解码器。
- **多模态与时间联合建模**：在单一框架内同步处理深度融合和时序建模，避免了分步处理的子优问题。
- **轻量化**：仅需单卡 3090 在 3 小时内完成训练，可解释性强，实用价值高。

## 8. 不足与局限

- **数据集覆盖有限**：仅使用三个 RGB-D VSOD 数据集，未在更大规模或更多样化场景（如真实世界噪声深度图、快速运动场景）上验证。
- **多目标场景**：当前 QTM 主要聚焦单显著目标，论文未来工作提到需优化多目标视频分割。
- **对深度质量的依赖**：深度投影仪和 DPA 依赖深度图质量，极端噪声或缺失深度时性能可能下降（文中未讨论）。
- **长视频处理**：实验中每视频仅采样 10 帧，长视频的长期依赖建模能力有待进一步验证。
- **对比方法偏少**：虽然对比 11 种方法，但部分方法（如 DCTNet+）为 2024 年发表，未与更近期的 2025 年方法（除 MFENet、KAN-SAM 外）充分对比。
- **未提供失败案例分析**：缺乏对模型失败场景的定性分析，可能掩盖潜在缺陷。

（完）
