---
title: "D3-RSMDE: 40× Faster and High-Fidelity Remote Sensing Monocular Depth Estimation"
title_zh: D³-RSMDE：40倍更快且高保真的遥感单目深度估计
authors: "Ruizhi Wang, Weihan Li, Zunlei Feng, Haofei Zhang, Mingli Song, Jiayu Wang, Jie Song, Li Sun"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37970/41932"
tags: ["query:mono-depth"]
score: 9.0
evidence: 遥感单目深度估计，高速高保真
tldr: 遥感单目深度估计面临精度与效率的折衷。本文提出D³-RSMDE，先由ViT模块快速生成初步深度图，再由扩散模块细化细节，实现40倍加速同时保持高保真度。在多个遥感数据集上，该方法在速度和感知质量上均显著优于现有方法，为实时遥感深度应用铺平道路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37970/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 704, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37970/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1843, \"height\": 611, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37970/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 880, \"height\": 485, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37970/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1834, \"height\": 1171, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37970/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1843, \"height\": 570, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37970/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1826, \"height\": 591, \"label\": \"Table\"}]"
motivation: 现有遥感单目深度估计方法在速度与质量间存在严重权衡。
method: 结合ViT快速生成初始深度与扩散模型高质量细化，实现速度与保真度的平衡。
result: 在遥感深度估计基准上实现40倍加速，同时保持高保真感知质量。
conclusion: ViT与扩散的混合架构能有效解决遥感深度估计的速度-质量矛盾。
---

## Abstract
Real-time, high-fidelity monocular depth estimation from remote sensing imagery is crucial for numerous applications, yet existing methods face a stark trade-off between accuracy and efficiency. Although using Vision Transformer (ViT) backbones for dense prediction is fast, they often exhibit poor perceptual quality. Conversely, diffusion models offer high fidelity but at a prohibitive computational cost. To overcome these limitations, we propose Depth Detail Diffusion for Remote Sensing Monocular Depth Estimation (D³-RSMDE), an efficient framework designed to achieve an optimal balance between speed and quality. Our framework first leverages a ViT-based module to rapidly generate a high-quality preliminary depth map construction, which serves as a structural prior, effectively replacing the time-consuming initial structure generation stage of diffusion models. Based on this prior, we propose a Progressive Linear Blending Refinement (PLBR) strategy, which uses a lightweight U-Net to refine the details in only a few iterations. The entire refinement step operates efficiently in a compact latent space supported by a Variational Autoencoder (VAE). Extensive experiments demonstrate that D³-RSMDE achieves a notable 11.85% reduction in the Learned Perceptual Image Patch Similarity (LPIPS) perceptual metric over leading models like Marigold, while also achieving over a 40× speedup in inference and maintaining VRAM usage comparable to lightweight ViT models.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：遥感单目深度估计（RSMDE）在精度与效率之间存在严重权衡。  
  - 基于ViT的密集预测方法（如DPT、AdaBins）推理速度快，但输出深度图缺乏高频细节，感知质量差（LPIPS高）。  
  - 扩散模型（如Marigold、EcoDepth）能生成高保真深度图，但推理过程极其耗时（例如Marigold在NVIDIA 3090上需约14秒/张），难以满足实时需求。  
- **整体含义**：现有方法无法兼顾高速与高保真，亟需一种既能保持扩散模型细节生成能力，又能大幅降低计算开销的新框架。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用混合架构，将ViT的快速全局结构生成与扩散模型的精细细节恢复相结合，消除扩散模型早期耗时的结构构建阶段。  
- **关键技术细节**：  
  - **初步场景结构模块（Preliminary Scene Structuring）**：使用DPT（ViT编码器+卷积解码器），经HDN损失优化，快速生成全局一致的粗深度图，作为结构先验。  
  - **渐进式线性混合细化（PLBR, Progressive Linear Blending Refinement）**：  
    - 定义扩散调度系数：\(\bar{\alpha}_t = \epsilon^{T-1}(T-t-1)\)，其中 \(\epsilon=0.8\)，\(T=6\)。  
    - 训练时：通过线性插值 \(z_t = \bar{\alpha}_t z_0 + (1-\bar{\alpha}_t) z_c\) 生成不同粗粒度的样本。  
    - 推理时：从粗潜码 \(z_c\) 开始，每一步用轻量U-Net预测 \(\tilde{z}_{0|t}\)，然后更新 \(z_{t-1} = \bar{\alpha}_{t-1} \tilde{z}_{0|t} + (1-\bar{\alpha}_{t-1}) z_c\)，仅需少量迭代（T=6）。  
  - **VAE潜空间操作**：使用预训练的AEKL或VA VAE将细化过程压缩到低维潜空间，大幅降低计算和显存需求。  
- **算法流程**（推理）：  
  1. ViT模块 → 粗深度图 \(d_c\) → VAE编码得 \(z_c\) → 设为 \(z_{T-1}\)。  
  2. 对于 \(t = T-1\) 到 \(0\)：  
     - 模型 \(f\) 输入 \([z_x, z_t]\) 和时间嵌入 \(e_t\)，预测 \(\tilde{z}_{0|t}\)。  
     - 按PLBR规则更新 \(z_{t-1}\)。  
  3. VAE解码 \(\tilde{z}_{0|1}\) 得到最终细化的深度图 \(\tilde{d}_0\)。

## 3. 实验设计：数据集、Benchmark与对比方法

- **数据集**：使用RS3DBench中的5个遥感数据集：  
  - Japan+Korea（J&K，2,650对，30m分辨率，沿海山地）  
  - Southeast Asia（SA，7,000对，30m，平原丘陵）  
  - Mediterranean（Med，29,225对，30m，沙漠高原）  
  - Australia（Ast，1,249对，5m，平原）  
  - Switzerland（Swi，4,827对，2m，山地）  
- **评估指标**：MAE、δ₃、PSNR、LPIPS（感知质量核心指标）。  
- **对比方法**：  
  - ViT模型：AdaBins、DPT、Omnidata  
  - 扩散模型：Marigold、EcoDepth  
  - GAN模型：Pix2pix（针对遥感优化）  
- **实现细节**：  
  - ViT模块：初始学习率5e-5，权重衰减1e-4，学习率调度（patience=5，factor=0.6）。  
  - 扩散细化器：初始学习率1e-4，L1损失，T=6，ε=0.8，调度（patience=5，factor=0.5）。  
  - 随机种子固定为42，5折交叉验证生成训练/测试集。

## 4. 资源与算力

- **硬件平台**：Ubuntu 16.04，Intel CPU E5-2699 v4，NVIDIA 3090 GPU（24GB显存），125G内存，Python 3.10.6。  
- **训练与推理资源**（文中Fig. 3给出具体数值）：  
  - D³-RSMDE推理速度比Marigold快**40倍以上**，训练时间也大幅缩短。  
  - 推理和训练显存占用与轻量ViT模型（DPT、Omnidata）相当，远低于其他扩散模型。  
- **训练时长**：文中未明确给出总训练小时数，仅给出“单epoch训练时间”对比图。

## 5. 实验数量与充分性

- **实验数量**：  
  - 第4节“Quantitative Analysis”在5个数据集上比较了7种方法（含本文2个变体），报告了4项指标。  
  - “Efficiency Analysis”比较了推理时间、训练时间、推理/训练显存。  
  - “Ablation Study”进行了5组消融实验：  
    1. ViT模块 vs 标准DPT  
    2. 是否使用VAE  
    3. 不同去噪步数T（3、6、10）  
    4. 两种VAE版本（AEKL、VA VAE）  
    5. 完整模型 vs 仅ViT模块（验证扩散细化有效性）  
- **充分性评价**：  
  - 实验覆盖多地形、多分辨率、不同数据量的遥感场景，对比方法涵盖主流范式，消融实验系统验证各组件贡献。  
  - 客观性：使用LPIPS等感知指标，消除主观偏差；固定随机种子保证可复现。  
  - 公平性：所有对比模型在相同数据集上重新训练或微调（如Marigold为re-trained），避免零样本偏差。  
  - 但在“EcoDepth”性能不佳的原因分析上仅提供附录解释，未在正文充分展示。

## 6. 论文的主要结论与发现

1. **性能最优**：D³-RSMDE在LPIPS上比Marigold降低**11.85%**，在MAE上降低**13.50%**，在多个数据集上达到SOTA或第二。  
2. **效率革命性提升**：推理速度比Marigold快**40倍**以上，训练时间也大幅缩短；显存占用与轻量ViT模型持平。  
3. **关键设计有效性**：  
   - ViT+HDN生成高质量粗先验，替代扩散早期阶段。  
   - PLBR策略实现少步数（T=6）稳定细化，避免过细化（T=10导致性能下降）。  
   - VAE潜空间操作显著加速训练（+54.91%）并降低显存（-36.17%），而不牺牲精度。  
4. **混合架构成功解决速度-质量矛盾**，为高保真遥感深度估计的实时部署铺平道路。

## 7. 优点

- **方法创新性**：首次将ViT快速先验与扩散细化有机融合，PLBR策略简单高效，非马尔可夫过程避免误差累积。  
- **效率与质量双赢**：同时实现40倍加速和感知质量提升，克服了扩散模型的实际部署瓶颈。  
- **实验全面严谨**：覆盖5种遥感场景、多项指标、多个基线，消融实验设计完整。  
- **开源可复现**：固定随机种子、5折交叉验证、详细超参数设置（附录提供）。

## 8. 不足与局限

- **实验覆盖**：仅使用RS3DBench数据集，未在更多公开遥感深度基准（如MVSEC、WHU-OOI）上验证，泛化性存疑。  
- **EcoDepth异常**：同为扩散模型却表现极差（远低于Marigold），文中仅用附录简要解释（可能未针对遥感重训练充分），削弱对比公平性。  
- **训练时长大核算**：未报告总训练GPU小时数，不利于成本估算。  
- **潜在偏差**：PLBR策略依赖预定义的线性调度系数（ε=0.8，T=6），对最优超参数的选择缺乏深入灵敏度分析。  
- **应用限制**：模型针对遥感俯视图优化，未知能否推广到近视图（如街景）或视频序列。

（完）
