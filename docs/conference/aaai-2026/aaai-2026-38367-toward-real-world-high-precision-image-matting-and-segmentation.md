---
title: Toward Real-World High-Precision Image Matting and Segmentation
title_zh: 面向现实世界的高精度图像抠图与分割
authors: "Haipeng Zhou, Zhaohu Xing, Hongqiu Wang, Jun Ma, Ping Li, Lei Zhu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38367/42329"
tags: ["query:matting"]
score: 8.0
evidence: 深度感知蒸馏用于高精度抠图
tldr: 针对现有抠图算法在现实场景中泛化能力差、难以处理精细结构的问题，本文提出前景一致学习模型FCLM。通过引入深度感知蒸馏策略，将深度知识迁移至抠图分支，有效提升了发丝等边缘细节的预测精度。在多个公开数据集上，FCLM在PSNR和MSE指标上均达到最优，验证了深度先验对抠图质量的显著增益。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 868, \"height\": 349, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 796, \"height\": 530, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 879, \"height\": 255, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 528, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1676, \"height\": 1066, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38367/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 727, \"height\": 324, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 773, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1830, \"height\": 370, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 854, \"height\": 322, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 854, \"height\": 231, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1830, \"height\": 771, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38367/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 855, \"height\": 201, \"label\": \"Table\"}]"
motivation: 现有抠图和分割方法依赖合成数据，泛化到真实场景效果差，且对发丝等精细结构处理不足。
method: 提出前景一致学习模型FCLM，包含深度感知蒸馏策略，利用深度知识提升抠图精细度。
result: 在多个数据集上取得最优PSNR和MSE，证明了深度先验对抠图质量的有效提升。
conclusion: 深度感知蒸馏能够有效提升真实场景下高精度抠图的性能。
---

## Abstract
High-precision scene parsing tasks, including image matting and dichotomous segmentation, aim to accurately predict masks with extremely fine details (such as hair). Most existing methods focus on salient, single foreground objects. While interactive methods allow for target adjustment, their class-agnostic design restricts generalization across different categories. Furthermore, the scarcity of high-quality annotation has led to a reliance on inharmonious synthetic data, resulting in poor generalization to real-world scenarios. To this end, we propose a Foreground Consistent Learning model, dubbed as FCLM, to address the aforementioned issues. Specifically, we first introduce a Depth-Aware Distillation strategy where we transfer the depth-related knowledge for better foreground representation. Considering the data dilemma, we term the processing of synthetic data as domain adaptation problem where we propose a domain-invariant learning strategy to focus on foreground learning. To support interactive prediction, we contribute an Object-Oriented Decoder that can receive both visual and language prompts to predict the referring target. Experimental results show that our method quantitatively and qualitatively outperforms state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **任务**：高精度图像抠图（matting）和二值分割（dichotomous segmentation），需要精确预测发丝等极细粒度结构。
- **现有挑战**：
  - 大多数方法针对显著的前景单物体，缺乏对不同类别的泛化能力。
  - 真实高质量标注数据稀缺，依赖合成的“不和谐”数据（前景与背景简单拼接），导致模型在真实场景中性能急剧下降（如表1所示：P3M-Net在合成数据上训练后评估真实数据时SAD从8.73升至12.33）。
  - 现有交互方法（如SAM）是类无关的，无法理解语义；多阶段方法（如MAM）复杂且易传播误差。
- **研究动机**：解决合成数据与真实数据的域差异，实现高精度的、可交互的、语义感知的抠图与分割。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **整体框架**：Foreground Consistent Learning Model (FCLM)，包含三个核心模块：
  1. **深度感知蒸馏（Depth-Aware Distillation, DAD）**：
     - 教师模型：Depth-Anything V2（Large）；学生模型：DINOV2（用于分割）或DINOV2（用于抠图）。
     - 核心思想：利用深度图区分前景/背景，将深度知识从教师转移到学生特征层。
     - 具体做法：对教师特征使用两个meta-net（含上下文token ε）分别投影到前景和背景；根据深度图阈值δ生成权重d+（前景）和d-（背景），然后分别对前景和背景特征计算KL散度蒸馏损失（公式3）。避免简单蒸馏带来的噪声，实现更精细的引导。
  2. **前景一致域适应（Foreground Consistent Domain Adaptation, FCDA）**：
     - 将合成数据视为域适应问题：输入一对共享前景的图像（A为合成，B为真实或另一合成），鼓励模型学习域不变的前景特征。
     - 对抗学习：引入域判别器h(·)，通过梯度反转层（GRL）使编码器产生混淆域的特征（损失L_adv，公式4）。
     - 令牌交换策略：随机交换A和B之间相同位置的视觉令牌（25%比例），引入隐式域扰动，防止训练崩溃。
     - 最优传输损失（OT Loss，公式8）：对前景令牌计算余弦相似度代价矩阵，使用Sinkhorn算法最小化A、B前景特征分布的距离，进一步对齐域不变特征。背景不进行对齐。
  3. **面向对象的解码器（Object-Oriented Decoder）**：
     - 支持视觉提示（点、框、涂鸦）和文本提示（通过CLIP文本编码器）。
     - 采用轻量级像素解码器和Transformer解码器（类似Mask2Former/SAM），将提示嵌入和可学习的查询与层次化图像特征交互，直接预测高精度掩码。
- **优化目标**：总损失L = L_kd + L_adv + L_OT + L_head（L_head对抠图任务使用L1+Laplacian损失，对二值分割使用BCE+IoU损失）。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - 抠图任务：HIM2K-Natural（人体实例抠图）、RefMatte-RW（多物体抠图，带文本标注）。
  - 分割任务：DIS-5K（高精度二值分割），包含验证集DIS-VD和整体测试集DIS-TE。使用BG20K背景图像生成合成训练数据。
- **基准指标**：
  - 抠图：IMQ系列（MSE、MAD、Grad、Conn）、SAD、MSE、Grad、Conn。
  - 分割：maxFβ、Fwβ、M、Sα、Emϕ、HCEγ。
- **对比方法**：
  - 抠图：InstMatt、CLIPMat、MatAny、MAM、SmartMatting等。
  - 分割：IS-Net、PGNet、HitNet、HQ-SAM、Pi-SAM、MVANet、DiffDIS、Gerpercept等。
- **设置**：训练时输入共享前景的图像对，测试时仅使用学生模型和解码器；文本提示模板“a photo of {CLS}.”（RefMatte有自带文本标注）。

## 4. 资源与算力
- 文中明确说明：所有实验使用 **4×Nvidia 4090 GPUs**，优化器为AdamW，学习率恒定为1e-5。未提及训练总时长或迭代数。

## 5. 实验数量与充分性
- **主实验**：在两个抠图数据集（HIM2K、RefMatte）和一个分割数据集（DIS-5K的VD和TE）上进行定量与定性比较，验证了SOTA性能。
- **消融实验**：
  - 蒸馏策略（表4）：逐步验证无KD、标准KD、单分支前景蒸馏、双分支前景蒸馏、加背景蒸馏的影响，共6组。
  - 域适应损失（表5）：无对齐、仅L_adv、仅L_OT、联合L_adv+L_OT，共4组。
  - 提示类型（表6）：点、框、文本对比，共3组。
- **可视化**：t-SNE展示域适应前后前景特征的对齐效果（图6），以及多组定性结果（图4、图5）。
- **充分性评价**：实验覆盖了抠图和分割两个子任务，消融实验系统，且所有比较均在公开基准上进行，使用了统一背景数据集（BG20K）合成训练数据，确保公平。但消融仅在RefMatte数据集上进行，未在HIM2K或DIS-5K上重复；未讨论不同合成数据比例或不同阈值δ的影响。总体较为充分，但略有限制。

## 6. 论文的主要结论与发现
- 将合成数据中的不一致性视为域适应问题，通过前景一致学习（FCDA）结合对抗损失和最优传输，能有效提升真实场景泛化能力。
- 深度感知蒸馏（DAD）利用深度图作为先验，显著改善前景/背景特征分离，提升发丝等精细结构预测。
- 支持多种提示的统一解码器简化了交互流程，实现了语义感知的多实例预测。
- FCLM在多个公共数据集上取得SOTA结果：HIM2K上IMQ四项指标均第一；RefMatte上MSE(0.010)和SAD(21.31)最低；DIS-5K上maxFβ(0.924/0.922)和M(0.025/0.026)最优。

## 7. 优点：方法或实验设计的亮点
- **创新性**：
  - 首次将合成数据的高精度抠图/分割问题建模为域适应，并设计前景一致学习。
  - 深度感知蒸馏巧妙利用深度图分离前景/背景蒸馏，比传统蒸馏更有效。
  - 令牌交换策略简单有效，防止对抗训练崩溃。
  - 统一解码器支持多模态提示，无需复杂多阶段pipeline。
- **实验设计**：
  - 使用公共基准和统一背景数据集，可复现性强。
  - 消融实验逐步验证各组件贡献，逻辑清晰。
  - 定量+定性+可视化（t-SNE）全面评估。
- **开放性**：代码开源在GitHub。

## 8. 不足与局限
- **实验覆盖局限**：
  - 消融实验仅在RefMatte上进行，未在HIM2K或DIS-5K上验证，可能降低结论的普适性。
  - 阈值δ（0.25）的选取未做敏感性分析。
  - 未探索不同合成数据比例或不同教师模型尺寸的影响。
- **偏差风险**：
  - 仅使用Depth-Anything V2作为教师模型，可能依赖其性能；其他深度估计模型未比较。
  - 文本提示仅使用CLIP，未尝试更强的大语言模型。
- **应用限制**：
  - 训练时需要配对图像（共享前景），限制了训练数据来源。
  - 推理时虽然只需单张图像，但需要预先选择提示方式；对于无提示输入，需初始化为零嵌入，可能影响效果。
  - 计算资源需求较高（4×4090），训练时间未披露。

（完）
