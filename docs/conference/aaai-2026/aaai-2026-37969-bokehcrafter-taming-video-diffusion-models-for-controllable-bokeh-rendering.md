---
title: "BokehCrafter: Taming Video Diffusion Models for Controllable Bokeh Rendering"
title_zh: "BokehCrafter: 驯服视频扩散模型实现可控散景渲染"
authors: "Qiwen Wang, Liao Shen, Jiaqi Li, Tianqi Liu, Huiqiang Sun, Zihao Huang, Yachuan Huang, Xianrui Luo, Zhiguo Cao"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37969/41931"
tags: ["query:neural-bokeh"]
score: 9.0
evidence: 使用视频扩散模型从全聚焦输入生成可控散景效果
tldr: 现有散景渲染依赖视差图和焦距等专业输入，且视频散景时序一致性差。本文提出BokehCrafter，首个视频扩散散景框架，从全聚焦视频输入生成时序连贯且美观的散景效果。采用双流注意力机制，结合参考图像分支和渲染分支，用户只需简单条件（如焦点位置和模糊大小）即可控制。实验表明生成效果优于现有方法，为移动端虚化渲染提供了新途径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37969/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1838, \"height\": 543, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37969/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1833, \"height\": 692, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37969/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1846, \"height\": 552, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37969/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1844, \"height\": 924, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37969/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1842, \"height\": 576, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37969/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1847, \"height\": 460, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37969/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 888, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37969/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 346, \"label\": \"Table\"}]"
motivation: 现有散景渲染依赖多输入且视频时序不一致。
method: 提出视频扩散框架，通过双流注意力机制从全聚焦输入生成可控散景。
result: 生成时序连贯、视觉效果佳的散景视频，超越先前方法。
conclusion: 为视频散景渲染提供了无需视差图的可控生成方案。
---

## Abstract
Bokeh is used in photography to emphasize the selected subject by smoothly blurring the out-of-focus region with appealing highlights. While recent advances have achieved impressive results in rendering realistic blur, existing frameworks typically rely on disparity maps and bokeh-relevant inputs (e.g., focal distance and blur size), and face significant challenges in video bokeh rendering due to limited temporal consistency. In this paper, we propose BokehCrafter, the first video diffusion framework that generates temporally coherent and visually pleasing bokeh effects from all-in-focus video inputs under user-friendly input conditions. Specifically, we leverage a dual-stream attention mechanism, integrating a reference image branch and a rendering instruction branch. We propose a Bokeh Image Extraction (BIE) module and a CLIP-based text encoder to extract image and text features, respectively, whose outputs are fused via a Text-Image Fusion (TIF) module to enable fine-grained and controllable bokeh rendering. To support the novel capabilities of our model, we construct Video Bokeh Scenes (VBS), a large-scale dataset containing a wide variety of bokeh videos with corresponding rendering instructions, across various scenes and rendering settings. Extensive experiments demonstrate that our method significantly outperforms state-of-the-art methods in both bokeh rendering quality and temporal consistency.

---

## 论文详细总结（自动生成）

# 论文总结：BokehCrafter: 驯服视频扩散模型实现可控散景渲染

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有散景渲染方法（无论是传统算法还是神经渲染）普遍依赖输入视差图以及焦距、模糊大小等专业参数；且在视频散景渲染中缺乏时序一致性建模，导致帧间闪烁和伪影。
- **意义**：本文首次将视频散景渲染任务建模为条件去噪扩散过程，提出 BokehCrafter 框架，仅需用户提供一条文本指令（如“保持前景清晰，对背景施加较大模糊”）和一张参考散景图片，即可从全聚焦视频生成时序连贯、视觉美观的散景效果，无需视差图或专业参数，降低使用门槛。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：基于 Latent Diffusion Model，将全聚焦视频编码为潜空间特征，引入双流注意力机制融合参考图像散景特征与文本语义，指导 U-Net 去噪过程，生成散景视频。
- **关键模块**：
  - **Bokeh Image Extraction (BIE)**：使用 CLIP 图像编码器提取参考图像的密集视觉特征，再通过 Q-Former（可学习查询token）聚焦散景相关属性（模糊模式、强度），输出紧凑的散景特征 F_bok。
  - **CLIP Text Encoder**：冻结的 CLIP 文本编码器提取指令语义特征 F_txt。
  - **Text-Image Fusion (TIF)**：采用双流交叉注意力，分别处理 F_txt 和 F_bok，与 U-Net 中间特征相加融合，实现细粒度可控渲染。
- **训练与推理策略**：
  - **Reference Content Decoupling (RCD)**：训练时参考图像来自不同场景（而非同场景），避免过拟合，提高泛化。
  - **条件丢弃（Condition Dropout）**：5% 样本丢弃图像条件、5% 丢弃文本条件、5% 同时丢弃，增强鲁棒性。
  - **双条件无分类器引导**：推理时同时使用无条件、仅图像条件、全条件三个噪声预测进行组合（公式 6），引导强度由 λ_I 和 λ_T 控制。
- **公式**：训练目标为常规噪声预测损失（公式 3）；推理时的噪声预测：  
  \( \tilde{\epsilon}_\theta = \epsilon_\theta(z_t, \phi, \phi) + \lambda_I[\epsilon_\theta(z_t, c_I, \phi) - \epsilon_\theta(z_t, \phi, \phi)] + \lambda_T[\epsilon_\theta(z_t, c_I, c_T) - \epsilon_\theta(z_t, c_I, \phi)] \)

## 3. 实验设计
- **数据集**：自建 Video Bokeh Scenes (VBS) 数据集，源自 IRS 和 TartanAir 的合成场景，包含 6100 个场景（训练 5760、测试 300、验证 40），每个场景按 5 个焦距 × 3 个模糊程度共 15 种散景变体，总计约 91.5k 视频（1.4M 帧）。同时为每个视频生成指令池（通过 GPT-4o），采样使用。
- **Benchmark**：VBS 测试集（按小、中、大模糊程度分别评估）。此外，收集 30 段真实世界视频（1024×576）用于用户研究。
- **对比方法**：RVR†（带权重归一化）、SteReFo、DeepLens、MPIB、BokehMe、VBR。所有对比方法均使用视差图和散景参数，而 BokehCrafter 仅使用文本+参考图。
- **评价指标**：PSNR、SSIM、LPIPS（渲染质量）；帧间差分均方根（时序一致性，公式 8）；用户研究（成对偏好）。

## 4. 资源与算力
- 训练配置：8 张 A100 GPU，初始学习率 1×10⁻⁵，batch size 8，训练步数 50K。采样使用 DDIM 及双条件 CFG。
- 推理时需多步去噪，计算成本较高（作者在局限性中指出）。

## 5. 实验数量与充分性
- **主要实验**：
  - 定量比较（表1）：在 VBS 测试集上按三种模糊程度分别报告各项指标，且均优于所有对比方法。
  - 定性比较（图4）：展示多帧结果，BokehCrafter 生成更清晰的焦点和更美观的高光。
  - 真实视频比较（图5）：与 VBR 对比，显示更优的焦点过渡和光斑效果。
  - 消融实验（表2）：分别移除 RCD、BIE、TIF，各组件均带来明显性能下降。
  - 用户研究（表3）：91 人对 30 段真实视频进行成对偏好选择，BokehCrafter 在所有对比中胜出（最低 62.35%，最高 84.76%）。
- **充分性评价**：实验覆盖合成基准、真实场景、用户主观评估，消融验证充分。对比方法类型全面（经典+神经+视频专用）。但仅在自建数据集上定量评估（因现有视频散景数据集 SVB 未公开），缺乏公开基准对比。

## 6. 主要结论与发现
- BokehCrafter 在散景渲染质量和时序一致性上显著优于现有方法，且无需输入视差图或复杂参数，仅需文本指令和参考图像。
- 用户研究证实，人类更偏爱 BokehCrafter 的结果（偏好度 62%–85%）。
- 消融实验证实 RCD、BIE、TIF 各组件均属必要。

## 7. 优点
- **方法创新**：首次将视频扩散模型用于散景渲染，以直观的用户输入（文本+参考图）替代传统专业参数，降低了使用门槛。
- **数据贡献**：构建了大规模、多样化的视频散景数据集 VBS（6.1k 场景，91.5k 视频），并附有文本指令，可推动后续研究。
- **模块设计**：BIE 模块有效解耦场景内容与散景风格；TIF 融合多模态条件；双条件 CFG 提供灵活控制。
- **实验全面**：覆盖合成数据定量、真实数据定性、用户主观评价，消融实验验证各组件贡献。

## 8. 不足与局限
- **计算成本高**：作为视频扩散模型，推理需要多步去噪，限制实时或低功耗应用。作者提及计划探索高效采样策略。
- **数据集局限性**：VBS 为合成场景（来源于 IRS、TartanAir），可能存在与真实世界的域差距；且未在公开视频散景基准（如 SVB）上评估（因不可用）。
- **对比公平性**：对比方法均额外输入视差图和参数，而 BokehCrafter 仅靠文本和单张参考图，虽然输入更简单，但可能因信息量差异导致不公平（作者未讨论）。
- **应用限制**：依赖参考图像的散景风格，用户可能需从专家参考库选择；文本指令对复杂散景效果（如动态变焦、特定光圈形状）的描述能力有限。
- **未报告失败案例**：未讨论在极端深度不连续、快速运动或遮挡场景下的表现。

（完）
