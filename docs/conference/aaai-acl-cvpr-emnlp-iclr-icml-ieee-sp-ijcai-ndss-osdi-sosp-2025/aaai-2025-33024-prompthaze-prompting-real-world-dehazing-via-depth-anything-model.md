---
title: "PromptHaze: Prompting Real-world Dehazing via Depth Anything Model"
title_zh: PromptHaze：通过Depth Anything模型提示真实世界去雾
authors: "Tian Ye, Sixiang Chen, Haoyu Chen, Wenhao Chai, Jingjing Ren, Zhaohu Xing, Wenxue Li, Lei Zhu"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33024/35179"
tags: ["query:mono-depth"]
score: 6.0
evidence: 利用Depth Anything模型生成深度提示用于去雾
tldr: 针对真实图像去雾中背景恢复和细节保留困难、且缺少成对数据的问题，提出PromptHaze方法，利用Depth Anything模型生成深度提示，通过逐提示迭代策略逐步恢复背景，并支持可控去雾强度。多个真实去雾基准实验表明该方法优于现有方法，能恢复更真实背景和清晰细节，该策略显著提升密集雾区细节恢复能力，展示了深度基础模型在图像恢复中的新应用价值。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 394, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 850, \"height\": 525, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 872, \"height\": 566, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1758, \"height\": 767, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 810, \"height\": 426, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33024/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1656, \"height\": 760, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33024/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 791, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33024/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 263, \"label\": \"Table\"}]"
motivation: 真实世界去雾缺少成对数据，现有方法难以恢复背景和细节。
method: 使用Depth Anything模型产生深度提示，逐提示迭代引导去雾网络渐进恢复背景。
result: 在真实去雾基准上取得更真实的背景恢复和细节保留效果。
conclusion: 深度基础模型可作为提示源提升去雾性能，拓展了深度模型的应用范围。
---

## Abstract
Real-world image dehazing remains a challenging task due to the diverse nature of haze degradation and the lack of large-scale paired datasets. Existing methods based on hand-crafted priors or generative priors struggle to recover accurate backgrounds and fine details from dense haze regions. In this work, we propose a novel paradigm, PromptHaze, for real-world image dehazing via the depth prompt from the Depth Anything model. By employing a prompt-by-prompt strategy, our method iteratively updates the depth prompt and progressively restores the background through a dehazing network with controllable dehazing strength. Extensive experiments on widely-used real-world dehazing benchmarks demonstrate the superiority of PromptHaze in recovering authentic backgrounds and fine details from various haze scenes, outperforming state-of-the-art methods across multiple quality metrics.

---

## 论文详细总结（自动生成）

# PromptHaze：通过Depth Anything模型提示真实世界去雾——论文详细总结

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **问题背景**：真实世界图像去雾是一项极具挑战性的任务。雾天退化形式多样（不均匀雾层、低光照、颜色畸变、噪声等），且**缺乏大规模真实成对数据**（即同一场景的有雾/无雾图像对），导致现有方法难以在真实场景中有效工作。
- **现有方法的不足**：
  - **基于手工先验的方法**（如暗通道先验、颜色衰减先验、非局部先验）：难以适应真实世界中多样化的退化场景。
  - **基于合成数据训练的深度学习方法**（如AECR-Net、FFA-Net、Dehamer等）：在合成基准上PSNR/SSIM很高，但对真实雾图泛化能力差。
  - **基于生成式先验的方法**（如RIDCP）：在浓雾区域难以恢复清晰的背景和细节，且训练过程繁琐。
- **核心洞察**：作者发现，**Depth Anything模型**在常见雾天场景下能够提供相当稳定和准确的深度估计，而深度信息与雾的退化程度（透射率 t(x) = e^{βd(x)}）有直接物理关联。因此，利用深度基础模型的深度提示（depth prompt）来辅助真实去雾是一个逻辑自然的思路。
- **整体含义**：论文提出了一种全新范式 **PromptHaze**，首次将基础模型（Depth Anything）的深度信息引入真实世界去雾，通过“逐提示（prompt-by-prompt）”迭代策略逐步恢复背景，实现高质量真实去雾。

## 2. 论文提出的方法论

### 2.1 核心思想

- 利用 Depth Anything 模型提取的**深度提示特征**作为辅助条件，引导去雾网络恢复背景。
- 采用**可控去雾强度系数 α**，使网络能够分步、渐进式地去雾。
- 采用 **Prompt-by-Prompt 迭代去雾策略**：用上一次去雾结果重新提取更准确的深度提示，再继续进行去雾，形成“去雾→更新深度提示→再去雾”的良性循环。

### 2.2 在线雾天数据生成管道（Online Haze Data Generation Pipeline）

- 受 RIDCP 启发，使用 500 张带深度图的干净图像，在线合成带多种复杂退化的有雾图像，公式为：

  **I_syn(x) = JPEG( P( J_gt(x)^γ + N , e^{βd(x)}, A + ΔA ) )**

  其中：
  - γ：亮度调整因子
  - N：高斯噪声
  - d(x)：深度图（离线估计）
  - β：雾密度系数
  - A：全局大气光值，ΔA 控制大气光的颜色偏差
  - JPEG(·)：模拟 JPEG 压缩伪影

- **合成可控目标 Iα**：为了实现可控去雾强度，构造中间目标：

  **Iα = α * J_gt + (1 - α) * I_syn**

  其中 α ∈ [0.4, 1.0]，间隔 0.1，训练时随机采样。

### 2.3 可控去雾强度的模型训练

- **深度提示特征**：提取 Depth Anything 模型解码器头部之前的最后一层特征作为深度提示 P_I，以更全面地保留深度特征信息。
- **雾相关参数回归学习**：利用合成管道中已知的雾密度 β 和大气光值 A，通过池化层和 MLP 组成的输出头进行回归预测，促使编码器学习雾相关表征。
- **雾相关参数投影（AdaLN）**：将池化后的雾相关参数嵌入通过线性投影，以自适应层归一化（Adaptive Layer Normalization）的方式注入解码器每个块中：

  **AdaLN(x, e) = e_s * LayerNorm(x) + e_b**

### 2.4 Prompt-by-Prompt 迭代去雾策略

- 训练完成后，推理阶段采用迭代策略：
  1. 对当前（雾图或部分去雾）图像提取深度提示。
  2. 用可控去雾模型以一定的 α 进行去雾。
  3. 用去雾结果更新深度提示，增大 α 继续去雾。
  4. 重复上述过程，逐步细化深度提示和恢复背景。

### 2.5 训练损失

- **重建损失**：Charbonnier 损失，约束输出接近 Iα。
- **对比正则化损失（CR Loss）**：锚点为 Jα，正样本为 Iα，负样本为 I_syn，提升泛化能力。
- **雾相关参数回归损失**：L1 损失，约束预测的 β 和 A 接近真值。
- **总损失**：L_total = L_rec + λ_cr * L_cr + λ_reg * L_reg，其中 λ_cr = 0.5，λ_reg = 0.2。

## 3. 实验设计

### 3.1 数据集

- **训练数据**：与 RIDCP 相同的 500 张干净图像（带深度图），通过在线管道实时合成有雾数据。
- **测试/评估数据**：
  - **RTTS 数据集**：包含 4000+ 张真实雾图，场景、时间、分辨率、退化类型多样，是公认的真实去雾 benchmark。
  - **Fattal 数据集**：31 张经典真实雾图，用于视觉定性比较。

### 3.2 对比方法

- **经典合成数据去雾网络**：MSBDN、FFA-Net、Dehamer
- **领域自适应/半监督方法**：DAD（基于图像翻译的域自适应）、PSD（基于物理先验的半监督）
- **生成式先验方法**：RIDCP（最先进的真实去雾方法）

### 3.3 评估指标

- 由于真实雾图无真值，采用**非参考质量评估指标**：
  - **FADE**（雾密度评估，越低越好）
  - **BRISQUE**（无参考图像质量，越低越好）
  - **NIMA**（神经图像美学评估，越高越好）
- **用户研究（User Study）**：从 RTTS 中选 80 张图，邀请 8 名志愿者，基于残雾程度、颜色自然度、噪声和伪影三个标准投票选出最佳结果。

## 4. 资源与算力

- **GPU**：4 块 NVIDIA RTX 4090
- **框架**：PyTorch
- **优化器**：AdamW（β=0.9, 0.999），batch size = 7
- **学习率**：初始 2×10⁻⁴，采用余弦退火
- **训练迭代**：15K iterations
- **模型参数量**：去雾模型 64.7M；Depth Anything 为 small 版本（24.8M）
- **数据增强**：水平翻转、随机缩放裁剪（256×256）、45° 和 90° 旋转
- **特别说明**：文中未明确提及总训练时长；此外，作者指出由于需要多次推理，**方法的推理速度不适合实时应用**。

## 5. 实验数量与充分性

### 5.1 实验概况

| 实验类型 | 数量/范围 |
|---------|----------|
| 定量对比（RTTS） | 6 种对比方法 × 3 个指标 + 用户研究 |
| 定性对比（RTTS） | 视觉比较多种方法 |
| 定性对比（Fattal） | 视觉比较 DAD、RIDCP、PSD 与本文方法 |
| 消融实验 | 4 组配置 |

### 5.2 消融实验的 4 组配置

1. **w/o Depth Prompt**（去掉深度提示）：BRISQUE 19.712 / NIMA 4.1041 → 深度提示对质量提升至关重要。
2. **w/o Contrastive Regularization**（去掉对比正则化）：BRISQUE 16.824 / NIMA 4.5190 → 对比正则化提升对比度和清晰度。
3. **w/o Prompt-by-Prompt Dehazing Strategy**（去掉迭代策略）：BRISQUE 16.791 / NIMA 4.8437 → 迭代策略有效提升整体质量。
4. **w/o Haze-Relevant Parameters Projection**（去掉雾参数投影）：BRISQUE 15.944 / NIMA 4.8141 → 雾参数投影进一步改善视觉效果。

### 5.3 实验充分性与公平性评价

- **优点**：
  - 在公认的真实去雾基准（RTTS）上进行了定量和定性评估，而非仅依赖合成数据集。
  - 与 RIDCP 使用相同的训练数据设置，对比相对公平。
  - 用户研究增加了主观评价的维度。
  - 消融实验覆盖了四个关键组件，验证了各模块贡献。
- **不足之处**：
  - 消融实验仅报告了 BRISQUE 和 NIMA 两个指标，缺少 FADE 和用户研究在第 2-4 组消融上的结果。
  - 定量评估仅使用无参考指标（因真实数据无真值），这些指标有一定局限性。
  - 论文提及“更多消融在补充材料中”，主文实验数量有限。
  - FADE 指标上略低于 PSD，作者解释为 PSD 倾向于过增强，但这也在一定程度上说明方法并非在所有指标上全面占优。

## 6. 论文的主要结论与发现

- **主要结论**：PromptHaze 在 RTTS 和 Fattal 等真实去雾基准上取得了领先性能，在 BRISQUE、NIMA 指标上明显优于现有方法（BRISQUE 14.624 vs. RIDCP 的 18.782；NIMA 5.1141 vs. 4.4267），用户研究中更以 0.512 的选票率大幅领先第二名（RIDCP 的 0.162）。
- **核心发现**：
  1. **Depth Anything 模型作为基础模型，可以为图像去雾提供强大支持**。在常见雾天场景下，其零样本深度估计能力稳定可靠。
  2. **深度提示 + 迭代策略有效缓解了浓雾导致的深度估计误差**。通过逐步更新深度提示，形成了深度模型与去雾网络的互补循环交互。
  3. **可控去雾强度（α）使渐进式、可调节的去雾成为可能**，优于传统一次性输出固定结果的方法。

## 7. 优点

- **方法创新性强**：首次将 Depth Anything 基础模型引入真实去雾领域，开辟了“深度提示 + 去雾”的新研究方向，且是首批探索基础模型用于真实图像恢复的工作之一。
- **物理合理性**：深度信息与大气散射模型中透射率直接相关，从物理机制上讲利用深度先验引导去雾是合理的。
- **解决真实场景数据缺乏问题**：在线雾天数据生成管道在训练中动态生成多样化的有雾数据，大大提升模型对复杂真实退化场景的泛化能力。
- **可控式设计**：通过 α 系数实现可调节的去雾强度，实用性较强。
- **迭代策略巧妙**：Prompt-by-Prompt 策略缓解了厚雾区深度估计不准的问题，形成一个“去雾—更新提示—再去雾”的闭环，思路优雅且有效。
- **实验验证较完善**：在多种真实数据集上进行了定量、定性、用户研究和消融实验，且与多种类型方法对比。
- **写作清晰**：动机明确（图1 直观展示 Depth Anything 在雾图上的深度估计能力），示意图信息丰富。

## 8. 不足与局限

- **伪影与颜色畸变问题**：在处理极不均匀的浓雾场景时，可能会在前景区域产生不自然的伪影和颜色失真（论文图 7 中作者坦诚了这一局限）。
- **推理速度慢，不适合实时应用**：Prompt-by-Prompt 策略需要多次（迭代）推理，计算开销大。
- **模型参数量较大**：去雾网络 64.7M + Depth Anything 24.8M，总参数量接近 90M，部署成本较高。
- **定量评估依赖无参考指标**：由于真实雾图缺乏真值，无法使用 PSNR/SSIM 等参考指标；FADE/BRISQUE/NIMA 这类无参考指标在评价偏好上可能与人的感知存在偏差。
- **消融实验报告不完整**：主文仅给出了 BRISQUE 和 NIMA 两个指标，未覆盖全部指标和用户研究；FADE 指标上未超越 PSD（虽然作者给出合理解释）。
- **训练数据规模较小**：仅用 500 张干净图像合成训练数据，虽然在线生成缓解了数据不足问题，但可能仍限制了对极端雾况的覆盖。
- **深度估计误差的累积风险**：虽然迭代策略缓解了这一问题，但在极端浓雾或低光照条件下，Depth Anything 的深度估计仍可能不准确，影响最终去雾效果。
- **应用范围有限**：方法主要面向静态图像去雾，在视频去雾（需时域一致性）或实时系统中的应用受限。

---

（完）
