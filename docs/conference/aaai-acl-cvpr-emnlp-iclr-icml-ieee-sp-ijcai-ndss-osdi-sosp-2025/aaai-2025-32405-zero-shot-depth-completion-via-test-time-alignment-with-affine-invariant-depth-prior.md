---
title: Zero-shot Depth Completion via Test-time Alignment with Affine-invariant Depth Prior
title_zh: 利用仿射不变深度先验与测试时对齐的零样本深度补全
authors: "Lee Hyoseok, Kyeong Seon Kim, Kwon Byung-Ki, Tae-Hyun Oh"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32405/34560"
tags: ["query:mono-depth"]
score: 6.0
evidence: 基于仿射不变深度先验与测试时对齐的零样本深度补全
tldr: 深度补全任务依赖先验知识，现有学习方法学到的先验多局限于训练域，难以泛化到域外场景。本文提出零样本深度补全方法，将预训练深度扩散模型作为仿射不变深度先验，并在测试时通过优化循环与度量尺度的稀疏深度测量对齐。该方法在不需要重新训练的情况下即可适应新场景，实验证明其对域外数据具有更强的鲁棒性和补全质量。该工作为深度补全的零样本泛化提供了有效范式。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1819, \"height\": 445, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 807, \"height\": 390, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1821, \"height\": 524, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 879, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1840, \"height\": 360, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1750, \"height\": 478, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1651, \"height\": 510, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32405/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 793, \"height\": 443, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32405/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 870, \"height\": 494, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32405/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1647, \"height\": 471, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32405/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32405/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 364, \"label\": \"Table\"}]"
motivation: 现有深度补全方法依赖训练域先验，对域外场景泛化能力差，难以零样本适应。
method: 利用预训练深度扩散模型作为仿射不变深度先验，并通过测试时优化将先验与稀疏度量深度对齐。
result: 在零样本设置下对域外稀疏深度测量实现高质量补全，明显优于依赖训练域的已有方法。
conclusion: 结合仿射不变深度先验与测试时对齐，可实现深度补全的零样本泛化。
---

## Abstract
Depth completion, predicting dense depth maps from sparse depth measurements, is an ill-posed problem requiring prior knowledge. 
Recent methods adopt learning-based approaches to implicitly capture priors, but the priors primarily fit in-domain data and do not generalize well to out-of-domain scenarios. To address this, we propose a zero-shot depth completion method composed of an affine-invariant depth diffusion model and test-time alignment. We use pre-trained depth diffusion models as depth prior knowledge, which implicitly understand how to fill in depth for scenes. Our approach aligns the affine-invariant depth prior with metric-scale sparse measurements, enforcing them as hard constraints via an optimization loop at test-time. Our zero-shot depth completion method demonstrates generalization across various domain datasets, achieving up to a 21% average performance improvement over the previous state-of-the-art methods while enhancing spatial understanding by sharpening scene details. We demonstrate that aligning a monocular affine-invariant depth prior with sparse metric measurements is a sufficient strategy to achieve domain-generalizable depth completion without relying on extensive training datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

**问题背景**：深度补全（Depth Completion）旨在从稀疏深度测量（如 LiDAR、SLAM 输出）中恢复稠密、度量尺度的深度图。这是一个典型的不适定问题（ill-posed problem），必须依赖先验知识（如 RGB 图像引导、场景结构先验）来约束解空间。

**核心痛点**：
- 现有学习方法（监督式或无监督式）主要通过在特定数据集上训练来隐式地学习深度亲和力（depth affinity），但这类先验严重耦合于训练域（in-domain），一旦遇到域外场景（out-of-domain），性能显著退化。
- 已有的测试时自适应（TTA）方法（如 BNAdapt、CoTTA、ProxyTTA）虽然能在测试时做一定调整，但其底层基础模型本身的泛化能力有限，在域差距较大时仍然力不从心。

**本文意义**：作者提出一种**零样本（zero-shot）深度补全**方法，不依赖任何特定深度补全数据集的训练，而是利用**预训练的深度扩散基础模型**作为通用深度先验，通过**测试时对齐**将其与度量尺度的稀疏测量融合，从而实现跨域泛化。核心论断是：**将单目仿射不变深度先验与稀疏度量测量对齐，足以实现域泛化的深度补全**，而无需依赖大规模配对训练数据。

---

## 2. 方法论

### 2.1 整体框架
- 使用预训练深度扩散模型（**Marigold** 或 **DepthFM**）作为仿射不变深度先验（affine-invariant depth prior）。
- 在测试时，通过**优化循环**将稀疏度量深度作为**硬约束**注入扩散采样过程，引导生成满足测量约束且保持结构先验的稠密深度图。

### 2.2 关键公式与算法流程

**（1）引导采样框架（Guided Sampling）**：
- 扩散模型的得分函数 sθ(xt) 提供先验梯度（prior term）。
- 通过式 (5) 将似然梯度加入反向采样过程，解决逆问题：
  \[
  \hat{s}_\theta(x_t, t, y) = s_\theta(x_t, t) + w \nabla_{x_t}\|y - A(x_0(x_t))\|_2^2
  \]
  其中 A 为二值测量矩阵，y 为稀疏深度，w 为权重。

**（2）测试时对齐（Test-time Alignment with Hard Constraints）**：
- 采用 latent diffusion 框架，在潜在空间 z 中操作（式 6）。
- 以 z0(zt)（通过 Tweedie 公式从 zt 估计）作为可优化变量进行优化（式 7）。
- 优化后的 ẑ0 通过式 (8) 重新添加噪声映射回中间潜在变量 ẑt，保证噪声水平匹配当前时间步。
- 整体形成"优化 → 重新加噪 → 继续采样"的交替迭代过程（图 4），有效保证测量一致性并避免次优解。

**（3）仿射不变先验的兼容性分析**：
- 作者通过实证验证：预训练扩散模型生成的仿射不变深度分布足以覆盖归一化度量深度空间（式 10），因此该先验可直接用于度量深度补全任务。

**（4）基于先验的离群点过滤（Prior-based Outlier Filtering，Algorithm 1）**：
- 使用超像素分割（SLIC）将仿射不变深度图划分为局部区域。
- 每个区域内用 RANSAC 进行鲁棒的线性最小二乘拟合，将仿射深度映射到度量深度。
- 偏差超过阈值 τ 的稀疏点被识别为离群点并过滤，提高引导信号的可靠性。

**（5）损失函数**：
- **稀疏深度一致性损失 L_depth**：L1 损失，强制执行测量约束。
- **局部平滑损失 L_smooth**：保留深度先验中的局部平滑特性。
- **相对结构相似性损失 R-SSIM（式 11）**：去除 SSIM 中的亮度项，仅保留结构对比项，实现跨域结构迁移，保持细节锐度。

---

## 3. 实验设计

### 3.1 数据集与 Benchmark
- **室内**：NYUv2、SceneNet、VOID
- **室外**：Waymo、nuScenes、KITTI Depth Completion
- 室内外全覆盖，跨越合成与真实场景、不同传感器类型和深度范围。

### 3.2 对比方法
- **测试时自适应方法**：BNAdapt、CoTTA、ProxyTTA（底层模型为 CostDCNet，在 KITTI DC 上训练）
- **无监督深度补全方法**：Self-S2D、VOICED、ScaffNet、KBNet、SPTR（均在各 benchmark 的训练集上进行了域内训练）
- **采样方法消融**：Naïve（无引导）、Guided（Bansal et al. 的标准引导）、Ours（硬约束测试时对齐）

### 3.3 评估指标
- RMSE（均方根误差）和 MAE（平均绝对误差），单位：米。

---

## 4. 资源与算力

论文中**未明确说明**训练所使用的 GPU 型号、数量或训练时长。这主要是因为本文方法的核心优势在于**无需训练**——仅在测试时进行优化，因此不存在大规模训练算力需求。但测试时推理开销较大（如表 2 所示），使用 Marigold（50 步）约需 101 秒/样本，使用 DepthFM（1 步）约需 16 秒/样本。文中提到可用 Flow Matching / Consistency Models 加速，但具体算力配置未披露。

---

## 5. 实验数量与充分性

### 5.1 实验组数
- **域泛化对比**（表 1）：4 个数据集 × 4 种方法 + 2 个基础模型变体，共约 20+ 组结果。
- **效率评估**（表 2）：3 种配置下的推理时间和精度对比。
- **无监督方法对比**（表 3）：在 KITTI DC 和 VOID 上与 5 种无监督方法对比，含 2 种过滤策略。
- **消融实验**（表 4）：3 个组件（测试时对齐、R-SSIM、离群点过滤）× 2 个数据集的系统消融。
- **定性实验**：图 1、5、6、7 展示了在 nuScenes、NYU、VOID 上的可视化结果。

### 5.2 充分性与客观性评价
- **充分性**：实验覆盖室内/室外、合成/真实、晴天/极端天气等多维场景；消融实验完整验证了每个设计组件的有效性；对比对象包含监督、无监督、TTA 三大类方法，较全面。
- **公平性**：与 TTA 方法对比时，所有方法均使用相同的预训练基础模型（CostDCNet）和相同的测试时预算；与无监督方法对比时，本文使用了更弱的设置（无多视图、无域内训练），对比是公平甚至偏向于基线方法的。
- **潜在注意点**：表 3 中，在 KITTI DC 上使用"手动过滤"（各 benchmark 自带的过滤方法）时效果显著优于本文自动过滤（1.198 vs 1.413），说明自动过滤算法在户外场景仍有改进空间。

---

## 6. 主要结论与发现

1. **仿射不变深度先验 + 测试时对齐 = 域泛化深度补全**：作者的实证验证充分支持了这一假设——预训练深度扩散模型已隐式掌握了足够的深度结构与空间理解能力，只需与度量测量对齐即可完成补全。
2. **硬约束优于软引导**：通过优化循环将稀疏测量作为硬约束（而非简单的梯度引导），显著提升了测量一致性和最终精度（消融实验中 RMSE 从 2.113 降至 1.610）。
3. **结构正则化至关重要**：R-SSIM 损失有效防止了深度先验中的结构信息被稀释，在保持场景细节（如物体边界、结构形状）方面发挥关键作用。
4. **跨域性能显著领先**：在所有域外数据集上，本文方法均达到最佳或次佳性能，较 ProxyTTA 平均提升最高可达 21%。
5. **模型通用性**：方法可应用于不同的深度扩散基础模型（Marigold、DepthFM），说明框架本身具有泛用性。

---

## 7. 优点

- **范式创新**：首次将"深度基础模型先验 + 测试时优化"引入深度补全领域，开辟了零样本深度补全的新路径，摆脱了传统方法对大规模配对训练数据的依赖。
- **无需训练**：全程零训练参数，所有适配均发生在测试时，可直接部署于任意域的新场景。
- **硬约束设计合理**：相比软引导，硬约束保证了测量值在最优解中的一致性，且"优化 z0 后重新加噪"的技巧有效避免了潜在空间优化与噪声水平的失配问题。
- **鲁棒性设计完整**：离群点过滤算法（RANSAC + 超像素 + 仿射先验）解决了实际传感器噪声问题，提高了方法的工程实用性。
- **跨域细节保持**：R-SSIM 损失是 SSIM 的精妙改进，去掉了依赖绝对值域信息的亮度项，使得结构信息可跨域传递，该设计在深度域迁移场景中具有通用价值。
- **实验扎实**：多场景、多基线、多消融，在 AAAI 标准下实验支撑充分。

---

## 8. 不足与局限

- **推理速度慢**：作者也坦诚指出了这一局限。由于在测试时对扩散采样的每个步骤执行优化循环（Marigold 基础下 101 秒/样本），远达不到实时应用要求，限制了实际工程部署能力。加速方向（一致性模型等）仅是展望，未做验证。
- **计算资源未披露**：未说明测试时优化的具体硬件配置，使得运行时间的可复现性和公平性难以独立评估。
- **离群点过滤算法仍有改进空间**：在 KITTI DC 上，手动过滤（benchmark 自带的预处理方法）效果明显优于本文的自动过滤算法（RMSE 降低约 18%），说明基于超像素 + RANSAC 的方法在复杂户外场景中仍会遗漏或误删有效测量点。
- **依赖基础模型的先验质量**：方法的最终性能强烈依赖所选的深度扩散基础模型（Marigold / DepthFM）的泛化能力和深度质量；若基础模型在某一极度特殊的场景类别上失败，本文框架可能无能为力。
- **测试时优化的超参数敏感性**：优化迭代次数、损失权重、RANSAC 阈值 τ、超像素数量 N 等超参数需要针对不同传感器配置或场景特点做一定调适，可能影响实际使用中的鲁棒性。
- **缺少极端场景的验证**：如夜间无光环境、强反射表面、透明物体等实际工程中的困难场景未被专门测试，深度先验在这些场景中的表现未知。

---

（完）
