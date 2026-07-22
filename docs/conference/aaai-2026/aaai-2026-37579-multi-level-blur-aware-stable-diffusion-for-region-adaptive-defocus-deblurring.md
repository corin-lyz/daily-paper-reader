---
title: Multi-Level Blur-Aware Stable Diffusion for Region-Adaptive Defocus Deblurring
title_zh: 多级模糊感知稳定扩散用于区域自适应散焦去模糊
authors: "Xiaopan Li, Yi Jiang, Shiqian Wu, Shoulie Xie, Sos Agaian"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37579/41541"
tags: ["query:cv-render"]
score: 4.0
evidence: 散焦模糊区域自适应去模糊与虚化渲染管线相关
tldr: 针对散焦模糊区域差异大、现有方法难以恢复细结构纹理的问题，提出多级模糊感知稳定扩散框架（MBSD）。通过补丁模糊标注器（PBA）分配模糊等级标签，多尺度模糊估计器（MSBE）预测软模糊概率并生成路由权重，再经模糊自适应专家混合器（BAEM）融合特征。实验表明该方法在恢复质量上优于现有方法，为计算摄影中的模糊渲染提供了可借鉴的思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 841, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1663, \"height\": 741, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 474, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1835, \"height\": 345, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 848, \"height\": 446, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1831, \"height\": 485, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1828, \"height\": 491, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1827, \"height\": 473, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 846, \"height\": 369, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 820, \"height\": 599, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37579/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 847, \"height\": 262, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37579/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1792, \"height\": 744, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37579/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 881, \"height\": 243, \"label\": \"Table\"}]"
motivation: 散焦模糊因景深不同区域差异大，现有去模糊方法难以适应区域差异并恢复细结构纹理。
method: 提出MBSD框架，包含补丁模糊标注器（PBA）、多尺度模糊估计器（MSBE）和模糊自适应专家混合器（BAEM）。
result: 在多个数据集上取得最优的去模糊质量，尤其在边缘和纹理区域表现突出。
conclusion: 区域自适应模糊感知机制显著提升散焦去模糊性能，对虚化渲染有参考价值。
---

## Abstract
Defocus blur, common in shallow depth-of-field photography, varies across image regions and is challenging to accurately estimate and restore. Existing deblurring methods often struggle to capture fine structural textures and do not effectively adapt to regional differences in blur. We propose Multi-Level Blur-Aware Stable Diffusion (MBSD), a novel framework that explicitly integrates regional blur recognition into a diffusion-based image restoration process. MBSD assigns blur-level labels to image patches using a Patch Blur Annotator (PBA), guiding a Multi-Scale Blur Estimator (MSBE) to predict soft blur probabilities and generate routing weights. These weights control a Blur-Adaptive Expert Mixer (BAEM), which adaptively combines features based on local blur severity. The features are then passed to a text-to-image diffusion model via a  cross-attention mechanism, enabling region-specific restoration. Extensive experiments on public benchmarks demonstrate that MBSD delivers superior perceptual quality while maintaining competitive PSNR and SSIM, consistently outperforming state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：散焦模糊（defocus blur）在浅景深摄影中普遍存在，其模糊程度随图像区域（如前景/背景）显著变化。现有去模糊方法多采用全局均匀处理，难以准确估计和恢复不同区域的模糊，尤其在精细结构纹理和严重模糊区域表现不佳。
- **背景**：传统方法依赖两阶段（估计模糊核+非盲反卷积），但真实模糊复杂；深度学习方法虽改进显著，但仍缺乏对局部模糊严重程度的显式建模；扩散模型（如Stable Diffusion）在图像恢复中展现潜力，但现有方法将退化视为全局一致，导致锐利区域过度平滑、严重模糊区域产生伪影。
- **意义**：提出区域自适应散焦去模糊框架，通过显式感知局部模糊等级来指导扩散模型的逐步恢复，以提升感知质量和结构保真度。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将模糊感知（blur-awareness）融入扩散模型，通过多尺度补丁级模糊等级标注、软概率估计和专家混合路由，生成区域自适应特征，并注入到Stable Diffusion的生成过程中。
- **关键技术细节**：
  - **Patch Blur Annotator (PBA)**：将图像划分为多尺度非重叠补丁，对每个补丁计算重模糊前后的高频成分SSIM作为模糊分数（Blur Score），再离散化为4个等级（轻微、轻度、中度、严重）。公式：\( M(P) = \text{SSIM}(K_L * (K_G * P), K_L * P) \)，阈值 {0.25,0.35,0.75}。
  - **Multi-Scale Blur Estimator (MSBE)**：由共享特征提取器（8个3x3卷积+ReLU）和4个尺度分类器分支组成。每个分支输出4通道软概率图（对应4个模糊等级），使用多尺度交叉熵损失 \( L_{\text{MSBE}} \) 训练。
  - **Blur-Adaptive Expert Mixer (BAEM)**：基于NAFNet，将其中每个NAFBlock替换为**Patch-level MoE Convolution Block (PMoECB)**。PMoECB根据MSBE输出的概率图生成4个专家的权重，对输入特征图按补丁进行加权卷积。额外引入保真恢复预任务（解码特征图得到恢复图像，使用MSE损失 \( L_{\text{restoration}} \)）。
  - **Blur-Aware Restoration Diffusion Model (BRDM)**：利用ControlNet将BAEM提取的模糊感知特征 \( f_{PBF} \) 注入Stable Diffusion U-Net。通过Patch-wise Blur-aware Cross-Attention (PBCA) 替代原文本交叉注意力（TCA）。文本描述由DAPE自动生成。训练采用标准噪声预测损失 \( L_{\text{noise}} \)。
- **总损失**：\( L_{\text{total}} = L_{\text{noise}} + \lambda_1 L_{\text{MSBE}} + \lambda_2 L_{\text{restoration}} \)，\( \lambda_1 = \lambda_2 = 1 \)。
- **算法流程**：输入模糊图像 → PBA生成离散标签 → MSBE预测多尺度软概率 → BAEM根据概率动态路由专家，提取模糊感知特征 → BRDM通过ControlNet注入特征 → 扩散去噪得到恢复图像。

## 3. 实验设计
- **数据集**：
  - **DPDD**：500个场景，350训练/74验证/76测试，提供成对模糊/清晰图像。
  - **RealDOF**：50个场景，Sony a7R IV拍摄，下采样至1120×1680用于评估。
  - **PixelDP**：无完整对焦真值，仅用于感知质量评价。
- **对比方法**：IFAN、Restormer、INIKNet、NRKNet、DefocusGAN、DEDDNet、P2IKT、ViTDeblur、RDDM（部分指标引用原文结果）。
- **评价指标**：
  - 全参考：PSNR、SSIM、LPIPS、DISTS。
  - 无参考：MUSIQ、MANIQA、CLIPIQA。

## 4. 资源与算力
- **GPU**：单张NVIDIA RTX A6000（48GB显存）。
- **训练配置**：Adam优化器，学习率5×10⁻⁵，batch size = 1，**训练1200k迭代**。论文未明确给出总训练时长（如天数/小时数）。
- **推理**：20步采样，使用LR embedding策略（源自SeeSR）。

## 5. 实验数量与充分性
- **定量实验**：在3个数据集上报告了10项指标对比（DPDD 10项，RealDOF 10项，PixelDP 3项无参考），覆盖主流SOTA方法。
- **定性实验**：提供了3组视觉对比图（图6-8），每组包含多个放大区域。
- **消融实验**：
  - 表2：逐步添加PBA、MSBE、BAEM、L_restoration，共4组对比，证明各组件有效性。
  - 图9：验证Blur Score的单调性与NIQE对比（100张图像，不同模糊强度）。
  - 图10-11：可视化PBA vs MSBE软概率图及最终恢复效果。
- **客观性**：指标选择全面（失真+感知），对比方法使用官方预训练模型，测试集公开，实验设置公平。消融设计合理，逐步增量验证。
- **充分性评价**：实验较为充分，涵盖多个数据集和多种场景。但仅在单GPU上训练，未报告多数据集跨域训练或微调结果，也未充分讨论失败案例。

## 6. 主要结论与发现
- MBSD在感知质量指标（LPIPS、DISTS、MUSIQ、MANIQA、CLIPIQA）上显著优于所有对比方法，同时在PSNR/SSIM上保持竞争力或领先。
- 在DPDD上，LPIPS降低至0.106（第二好为0.162），DISTS降至0.0611，感知优势明显。
- 在RealDOF和PixelDP上，MBSD同样取得最佳感知指标。
- 消融表明：软概率（MSBE）优于硬标签（PBA），保真损失进一步改善PSNR和SSIM。
- 提出的Blur Score具有单调性，比NIQE更适合模糊程度评估。

## 7. 优点
- **创新性**：首次将显式模糊感知（多尺度补丁级模糊等级）与扩散模型端到端结合，实现区域自适应去模糊。
- **模块化设计**：PBA、MSBE、BAEM各模块责任清晰，且可分别或联合消融验证。
- **自适应路由机制**：利用MoE根据局部模糊严重程度动态选择专家，提升对不同模糊区域的恢复能力。
- **高质量的感知恢复**：在生成真实感纹理和边缘方面表现突出，尤其在严重模糊区域和复杂过渡区域。
- **实验全面**：覆盖多个数据集、多种指标和消融，且公开代码和模型权重（论文提及使用公开SD-2 base）。

## 8. 不足与局限
- **SSIM局限性**：在DPDD上SSIM（0.800）略低于ViTDeblur（0.814），在RealDOF上SSIM低于P2IKT（0.787 vs 0.769），表明结构相似性上并非完全最优，可能因生成式方法引入细微纹理偏差。
- **计算资源需求**：训练使用单卡A6000、1200k迭代，batch size=1，需要大量算力和时间。推理需20步扩散采样，速度较慢。
- **对文本描述的依赖**：采用DAPE自动生成文本prompt，prompt质量可能影响结果；未对比手动提供理想prompt的情况。
- **未讨论泛化性边界**：模型仅在DPDD上训练，未在真实场景（如手机摄影、不同相机系统）系统测试；未分析对非高斯模糊、大位移模糊的适用性。
- **缺乏对失败案例的分析**：论文未展示或讨论任何恢复效果差的例子，可能隐藏了某些场景下的性能瓶颈（如极度稀疏纹理、大范围同色区域）。
- **伦理与风险**：生成模型可能引入不存在的纹理细节，在医学/监控等需要严格保真的场景中需谨慎。

（完）
