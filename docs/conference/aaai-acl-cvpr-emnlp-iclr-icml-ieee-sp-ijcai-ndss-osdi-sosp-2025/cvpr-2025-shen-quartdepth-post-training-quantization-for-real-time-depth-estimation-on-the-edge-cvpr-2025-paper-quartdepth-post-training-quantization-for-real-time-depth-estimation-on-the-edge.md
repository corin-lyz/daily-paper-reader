---
title: "QuartDepth: Post-Training Quantization for Real-Time Depth Estimation on the Edge"
title_zh: QuartDepth：面向边缘实时深度估计的后训练量化
authors: "Shen, Xuan, Ma, Weize, Liu, Jing, Yang, Changdi, Ding, Rui, Wang, Quanyi, Ding, Henghui, Niu, Wei, Wang, Yanzhi, Zhao, Pu, Lin, Jun, Gu, Jiuxiang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Shen_QuartDepth_Post-Training_Quantization_for_Real-Time_Depth_Estimation_on_the_Edge_CVPR_2025_paper.pdf"
tags: ["query:mono-depth"]
score: 9.0
evidence: 面向边缘设备实时深度估计的后训练量化
tldr: 高精度的基础深度模型在 ASIC 等边缘设备上部署困难。QuartDepth 提出后训练量化方案，将权重和激活量化到 4 位，显著降低模型大小和计算量。在保持深度估计精度的同时，实现了面向边缘设备的实时推理。这为轻型移动端深度模型提供了高效压缩路径。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 873, \"height\": 502, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 356, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 291, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 291, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 865, \"height\": 450, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 870, \"height\": 280, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 879, \"height\": 425, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 875, \"height\": 427, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1815, \"height\": 1322, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 864, \"height\": 854, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shen-quartdepth-post-training-quantization-for-real-time-depth-estimation-on-the-edge-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1802, \"height\": 354, \"label\": \"Table\"}]"
motivation: 单目深度估计模型计算和内存需求大，难以部署到资源受限的边缘设备和 ASIC 上。
method: 采用后训练量化，将深度模型的权重和激活量化为 4 位，并结合硬件加速实现边缘端高效推理。
result: 在显著减少模型尺寸和计算成本的同时，保持深度估计精度，实现边缘设备上的实时运行。
conclusion: 量化压缩是推动深度模型在移动端和嵌入式设备落地的重要手段，QuartDepth 提供了一套可行方案。
---

## Abstract
Monocular Depth Estimation (MDE) has emerged as a pivotal task in computer vision, supporting numerous real-world applications. However, deploying accurate depth estimation models on resource-limited edge devices, especially Application-Specific Integrated Circuits (ASICs), is challenging due to the high computational and memory demands. Recent advancements in foundational depth estimation deliver impressive results but further amplify the difficulty of deployment on ASICs. To address this, we propose QuartDepth which adopts post-training quantization to quantize MDE models with hardware accelerations for ASICs. Our approach involves quantizing both weights and activations to 4-bit precision, reducing the model size and computation cost. To mitigate the performance degradation, we introduce activation polishing and compensation algorithm applied before and after activation quantization, as well as a weight reconstruction method for minimizing errors in weight quantization. Furthermore, we design a flexible and programmable hardware accelerator by supporting kernel fusion and customized instruction programmability, enhancing throughput and efficiency. Experimental results demonstrate that our framework achieves competitive accuracy while enabling fast inference and higher energy efficiency on ASICs, bridging the gap between high-performance depth estimation and practical edge-device applicability. Code: https://github.com/shawnricecake/quart-depth

---

## 论文详细总结（自动生成）

# QuartDepth：面向边缘实时深度估计的后训练量化（CVPR 2025）详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题背景**：单目深度估计（Monocular Depth Estimation, MDE）是计算机视觉中的关键任务，广泛应用于机器人感知、自动驾驶、虚拟现实和三维重建等领域。尽管最近以 Metric3D、Depth Anything 为代表的基础模型在精度和泛化性上取得了显著突破，但其复杂的架构和巨大的计算量使其很难部署到资源受限的边缘设备，尤其是专用集成电路（ASIC）上。
- **核心矛盾**：高精度 MDE 基础模型在 ASIC 上存在**计算量大、内存占用高**的双重瓶颈，难以满足实时推理和低功耗的实际需求；而传统模型压缩方法（如剪枝、蒸馏、架构搜索）主要降低模型复杂度，量化则能同时实现模型小型化和推理加速，是为数不多能同时解决容量与速度问题的技术手段。
- **整体含义**：本文提出 QuartDepth——一个面向 ASIC 的**后训练量化（PTQ）框架**，将 MDE 模型的权重和激活同时量化为 4-bit（W4A4）或 4-bit 权重 + 8-bit 激活（W4A8），在保持深度估计精度的前提下，使模型能够在边缘设备上实现实时推理和更高能效，弥合了高性能深度估计与边缘设备实用部署之间的差距。

## 2. 方法论：核心思想、关键技术细节与算法流程

### 2.1 整体流程

QuartDepth 的量化流程分为三个主要步骤：
1. **激活抛光（Activation Polishing）**：先对激活进行 LogNP 抛光，将异常分布平滑为利于量化的形式；
2. **激活损失补偿（Activation Loss Compensation）**：对权重进行更新，补偿激活量化引入的输出误差；
3. **权重重建（Weight Reconstruction）**：对更新后的权重进行量化，最小化二阶权重量化误差。

### 2.2 关键技术细节

**（1）LogNP 激活抛光**

- 通过逐通道可视化分析，作者发现 MDE 模型解码器中存在大量极端离群值，且不同通道的离群值范围差异大，导致 per-tensor 量化困难。
- 对每个激活元素 x，LogNP 抛光函数定义为：

  \[
  \Phi(x, \omega) = \text{sign}(x) \cdot [\log_2(|x| + \omega) - \log_2(\omega)]
  \]

  其中 ω 为逐通道的抛光因子，按校准集上第 ε 百分位（ε 取 95）计算得到。
- 在激活反量化后，执行逆变换（unpolishing）恢复原始范围。该额外延迟开销由硬件并发执行隐藏。

**（2）激活损失补偿**

- 激活量化后存在量化误差，QuartDepth 在权重量化**之前**逐层更新权重以补偿该误差。
- 目标是最小化 \(\| Wx - (W + \Delta W)\hat{x} \|_2^2\)，通过梯度置零获得闭式解：

  \[
  \Delta W^* = -W(x - \hat{x})\hat{x}^T(\hat{x}\hat{x}^T)^{-1}
  \]

  当 \(\hat{x}\hat{x}^T\) 不可逆时采用 dampening 技术。

**（3）权重重建**

- 利用泰勒展开近似量化引起的损失变化：\(L(w+\Delta w) - L(w) \simeq \frac{1}{2}\Delta w^T H_w \Delta w\)，并假设预训练模型已收敛（梯度为零）。
- 使用 KFAC 近似 Fisher 信息矩阵作为 Hessian 的近似：\(F_l = G_l \otimes A_l\)，其中 \(G_l\) 为梯度二阶矩，\(A_l\) 为激活二阶矩，从而将复杂度从参数的平方降为可管理的量级。
- 采用 AdaRound 学习舍入参数 v，优化目标为：
  \[
  \min_v \sum_{l=1}^L (w^{(l)} - \hat{w}^{(l)})^T F_l (w^{(l)} - \hat{w}^{(l)}) + \vartheta h(v)
  \]
  仅需 32 张校准图像。

**（4）硬件设计**

- 设计了灵活可编程的 ASIC 加速器，包含 Dispatch（指令分发）、Load/Store（DMA）、矩阵乘法单元（MMU）和向量计算单元（VCU）。
- 支持 W4A4 和 W4A8 专用计算内核，实现外部内存带宽的充分利用；FPU 通过多项式近似实现 log2/exp 等非线性函数，仅 3-5 个时钟周期，误差仅为 2-5 ULP。
- 支持 **内核融合** 和 **自定义指令编程**，中间结果直接在片上 SRAM 传递，无需写回 DDR；数据搬运、矩阵计算和向量计算可完全流水线重叠执行。

## 3. 实验设计：数据集、基准与对比方法

### 3.1 模型与配置

- **模型**：Metric3D（ViT-Small、ViT-Large、ViT-Giant 三个骨干）和 Depth Anything（ViT-Large 骨干）。
- **量化配置**：W4A8 和 W4A4 两种权重/激活位宽组合，采用逐通道非对称量化。

### 3.2 数据集

- **标定集**：NYUv2（室内）和 KITTI（室外）训练集各随机采样 32 张。
- **评估集**：
  - 室内：NYUv2、SUN RGB-D、iBims-1、HyperSim；
  - 室外：KITTI、vKITTI2、DIODE。
- **评估指标**：AbsRel（绝对相对误差）、δ1/δ2/δ3（阈值精度）、RMSE（均方根误差）、Silog（度量深度损失）。

### 3.3 对比方法

- 传统 PTQ 方法：minmax、EMA、percentile；
- 学习型/优化型方法：OBS（仅权重量化）、AdaRound、BrecQ。

### 3.4 硬件实验

- 使用 RTL 实现硬件设计，Design Compiler 在商用 28nm CMOS 工艺下综合，频率 1GHz，PrimeTime PX 测试功耗，DDR 带宽 19.2 GB/s（RTL 仿真）。

## 4. 资源与算力

- **论文未明确说明**训练/标定所使用的 GPU 型号、数量及具体时长。
- 从方法描述可推断：仅使用 32 张校准图像，权重重建阶段采用 batch size = 1、学习率 4e-5、warm up 0.2、weight decay 0.01、drop rate 0.5、20,000 次迭代，计算开销相对较小。
- 硬件实验：Design Compiler 在 28nm CMOS 工艺下综合，频率 1GHz，面积分别为 Float32：29.22 mm²、W4A8：23.94 mm²、W4A4：24.35 mm²。

## 5. 实验数量与充分性评估

- **实验组数**：核心定量实验主要包含三组大表：① Metric3D 两个骨干在 NYUv2/KITTI 上的精度结果；② Depth Anything 在 7 个数据集上的泛化结果；③ ASIC 硬件延迟/能效结果（3 种分辨率 × 3 种配置 × 2 个骨干）。此外还包含 ViT-Giant 结果（附录）、校准样本数量消融、硬件核心数消融以及可视化对比。
- **充分性**：整体实验覆盖了**模型规模（Small/Large/Giant）、量化位宽（W4A4/W4A8）、场景类型（室内/室外）、数据集多样性（7 个以上）**，且对比方法涵盖传统与先进 PTQ 方法，较全面。
- **客观性/公平性**：对比方法在相同设置下进行，采用逐通道非对称量化保持一致性；Depth Anything 实验中与仅量化权重的 OBS 方法对比，体现出方法综合优势。不过，部分对比方法（如 OBS）属于 weight-only，与 W4A8/W4A4 的直接对比略欠对等。

## 6. 主要结论与发现

1. **量化可行性**：QuartDepth 在 W4A8 配置下对 ViT-Large 骨干几乎无损（如 NYUv2 AbsRel 0.071 vs 原模型 0.067）；在 W4A4 配置下仍能保持较高精度（如 δ1 达 0.932 vs Float32 的 0.972），显著优于现有 PTQ 方法。
2. **激活离群值是主要瓶颈**：MDE 解码器的极端离群分布是量化精度下降的主因，LogNP 抛光能有效将其平滑为量化友好的分布。
3. **硬件加速效果显著**：相比 Float32，W4A4 在 ViT-Small 256×256 分辨率下可获得最高 3.5 倍加速（FPS 从 13.02 提升至 26.11）和 3.7 倍能效提升；ViT-Large 1024 分辨率下加速 4.9 倍、能效提升 5.0 倍。
4. **校准效率高**：仅 32 张校准样本即可使性能趋于稳定，适用于数据获取受限的边缘场景。
5. **硬件资源利用率高**：量化模型对核心数不敏感，在较少硬件资源下仍保持高效率和低延迟。

## 7. 优点

1. **算法-硬件协同设计**：将量化算法与 ASIC 架构紧密结合（如用可编程向量计算阵列隐藏 LogNP 开销），不是孤立的软件算法或硬件设计。
2. **问题针对性强**：深入分析了 MDE 基础模型的逐通道离群分布特性，LogNP 抛光的设计直击该特定任务的核心痛点。
3. **两阶段量化纠错**：激活补偿与权重重建两个阶段相互衔接，系统性地分别处理了激活量化和权重量化的误差来源。
4. **硬件设计灵活性高**：支持内核融合、并发执行和自定义指令，使不同数值格式（Float32/INT4/INT8）无缝协作，适应多种量化配置。
5. **实验覆盖全面**：涵盖从 ViT-Small 到 ViT-Giant 的模型规模、从室内到室外的数据集类型，以及精度和硬件性能的双维评估。

## 8. 不足与局限

1. **硬件验证深度有限**：论文依赖 RTL 仿真和综合工具评估延迟与功耗，未提及在真实 FPGA 原型或流片（tape-out）芯片上的物理验证，实用性证据仍偏仿真层面。
2. **算力信息缺失**：未披露标定/重建阶段所用的 GPU 型号、数量和能耗，复现时难以估算算法端整体环境开销。
3. **校准集规模限制**：仅使用 32 张图像进行标定，虽然在文中证明已够用，但在更大规模、更多样化的部署场景下可能影响泛化稳健性。
4. **位宽探索范围有限**：仅实验了 W4A8 和 W4A4（权重最低为 4-bit），未深入探索 W2/W3 等更低比特，也未讨论混合精度量化的潜力。
5. **部分对比不够对等**：OBS 为仅权重量化方法，与 W4A8/W4A4 的对比在严格意义上不完全公平。
6. **硬件泛化性存疑**：硬件设计针对特定量化配置和模型优化，对非量化层、动态形状输入或未来新架构的适应能力未充分探讨。
7. **应用范围局限**：只在深度估计模型上验证，未证明方法能否推广到其他密集预测任务（如分割、光流）或其他视觉基础模型。

（完）
