---
title: "BokehFlow: Depth-Free Controllable Bokeh Rendering via Flow Matching"
title_zh: "BokehFlow: 基于流匹配的无深度可控散景渲染"
authors: "Yachuan Huang, Xianrui Luo, Qiwen Wang, Liao Shen, Jiaqi Li, Huiqiang Sun, Zihao Huang, Wei Jiang, Zhiguo Cao"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37431/41393"
tags: ["query:neural-bokeh"]
score: 9.0
evidence: 基于流匹配的无深度可控散景渲染
tldr: 现有的可控散景渲染方法通常依赖准确的深度图，限制了其应用范围。BokehFlow提出一种无需深度输入的框架，基于流匹配直接合成散景效果，并利用交叉注意力机制实现语义级别的控制。该方法在保持高效率和真实感的同时，避免了深度估计的误差，适用于移动摄影等场景。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37431/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1639, \"height\": 863, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37431/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1689, \"height\": 727, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37431/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1662, \"height\": 860, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37431/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1710, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37431/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 833, \"height\": 640, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 859, \"height\": 182, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1810, \"height\": 457, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 812, \"height\": 182, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 852, \"height\": 184, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 848, \"height\": 220, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37431/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 874, \"height\": 302, \"label\": \"Table\"}]"
motivation: 现有可控散景渲染方法依赖深度图，限制了通用性和效率。
method: 提出基于流匹配的无深度框架，通过交叉注意力实现语义控制。
result: 无需深度输入即可生成高质量、可控的散景效果，效率优于传统方法。
conclusion: 流匹配方法可有效替代深度依赖，实现灵活且高效的散景渲染。
---

## Abstract
Bokeh rendering simulates the shallow depth-of-field effect in photography, enhancing visual aesthetics and guiding viewer attention to regions of interest. Although recent approaches perform well, rendering controllable bokeh without additional depth inputs remains a significant challenge. Existing classical and neural controllable methods rely on accurate depth maps, while generative approaches often struggle with limited controllability and efficiency. In this paper, we propose BokehFlow, a depth-free framework for controllable bokeh rendering based on flow matching. BokehFlow directly synthesizes photorealistic bokeh effects from all-in-focus images, eliminating the need for depth inputs. It employs a cross-attention mechanism to enable semantic control over both focus regions and blur intensity via text prompts. To support training and evaluation, we collect and synthesize four datasets. Extensive experiments demonstrate that BokehFlow achieves visually compelling bokeh effects and offers precise control, outperforming existing depth-dependent and generative methods in both rendering quality and efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有可控散景渲染方法严重依赖准确的深度图，但在真实拍摄场景中深度图往往不准确或不可用。同时，已有的生成式方法（如扩散模型）虽能生成散景效果，但受限于采样速度慢、控制能力弱（如无法控制聚焦区域）。
- **研究动机**：提出一种不需要深度输入的、语义可控的、高效的散景渲染框架，以提升在移动摄影等实际场景中的适用性和用户体验。
- **整体含义**：BokehFlow首次将流匹配（Flow Matching）应用于散景渲染，通过从全聚焦图像到散景图像的直接分布传输，实现了无需深度图的、基于文本提示的聚焦区域和模糊强度控制，在渲染质量和效率上优于现有依赖深度的方法。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：将散景渲染建模为从全聚焦图像到散景图像的确定性分布传输过程，利用流匹配框架学习直线、高效的传输路径，避免扩散模型的高计算开销。同时，通过交叉注意力机制将文本语义控制嵌入模型，实现无需深度的语义级控制。
- **关键技术细节**：
  - **潜在流匹配**：使用VAE将图像编码到潜在空间（8倍下采样，4通道），在潜在空间进行流匹配，降低计算成本。
  - **直接传输路径**：不同于传统从高斯噪声到目标分布的传输，BokehFlow直接从全聚焦图像潜在表示 \( z_A \) 出发，传输到散景潜在表示 \( z_B \)。训练时网络预测残差向量 \( \phi_1(z_B) - \phi_t(z_B) \)，实现可变尺度的线性传输，提高采样效率和空间一致性。
  - **Bokeh控制适配器（BCA）**：使用预训练CLIP文本编码器将用户提示（如“聚焦前景，模糊强度30”）编码为控制嵌入 \( z_C \)。通过交叉注意力层将 \( z_C \) 注入到流匹配网络的向量场回归中，其中散景特征 \( z_B \) 作为Query，\( z_C \) 作为Key和Value。这样替代了依赖深度的模糊半径公式，实现语义驱动控制。
  - **知识迁移**：利用预训练模型（如Stable Diffusion、Marigold、DepthFM）初始化U-Net，特别是从深度预测模型（DepthFM）初始化能增强深度感知能力，提升边界渲染质量。
- **算法流程**：
  - 训练：输入全聚焦图像和散景图像对，VAE编码得到 \( z_A \)、\( z_B \)，添加噪声后用流匹配模型学习预测残差向量，BCA将文本控制嵌入注入。
  - 推理：从全聚焦图像潜在 \( z_A \) 开始，数值积分ODE（单步或少数步）得到散景潜在 \( z_B \)，再经VAE解码得到散景图像。

## 3. 实验设计

- **数据集**：
  - 训练集：Control Bokeh Dataset (CBD) — 来自ReDWeb的3500个场景，每张全聚焦图像用不同焦距和模糊强度渲染出10种散景，共35000对图像，分辨率约1024×768。
  - 评估集：
    - **CBD**（合成数据集）
    - **EBB400**（真实世界DSLR拍摄数据集，400对，来自EBB!，分辨率约1536×1024）
    - **GenPhotoBokeh**（合成数据集，1000个文本驱动场景，来自Generative Photography，用于与文本到图像方法比较）
    - **IB30**（用户研究数据集，30张iPhone 15 Pro拍摄的真实图像）
- **基准**：对比三类方法：
  - 经典渲染：SteReFo、VDSLR、DrBokeh
  - 神经渲染：DeepLens、MPIB、BokehMe
  - 生成方法：Generative Photography（仅在GenPhotoBokeh上比较）
- **评估指标**：SSIM、PSNR、LPIPS整体指标；边缘指标SSIM_eg、PSNR_eg（基于Sobel边缘提取+膨胀掩码）；推理时间。

## 4. 资源与算力

- 论文明确说明：训练在单个NVIDIA RTX A6000 GPU上完成，训练迭代10K步，初始学习率3e-5，3K步后衰减到3e-7。未说明训练总时长（小时数），但指出实验重复多次结果稳定。推理时间在CBD数据集上为0.461秒（Ours），显著低于大部分对比方法。

## 5. 实验数量与充分性

- 主要定量实验：在CBD和EBB400数据集上，对比了7种方法（SteReFo、VDSLR、DrBokeh、DeepLens、MPIB、BokehMe、Ours），报告了PSNR/SSIM/LPIPS及边缘指标和推理时间。
- 额外比较：在GenPhotoBokeh上与Generative Photography对比（PSNR/SSIM/LPIPS及边缘指标）。
- 消融实验：3个策略的消融（传输策略、BCA交叉注意力、初始化策略），每个实验都在CBD数据集上报告了4个指标。
- 用户研究：在IB30数据集上，50名志愿者对Ours与5个基线（DrBokeh、SteReFo、MPIB、BokehMe、iPhone 15 Pro）进行偏好选择，结果Ours被显著偏好（69%~83%）。
- 总体评价：实验覆盖了合成/真实、不同类别方法、用户主观评价，消融验证了各个关键组件，设计较为充分和客观。但未在更多多样性场景（如极端光照、物体运动）中测试，边缘指标仅基于简单Sobel算子，可能存在偏差。

## 6. 论文的主要结论与发现

- BokehFlow在无需深度输入的情况下，实现了与依赖深度方法相当甚至更优的渲染质量，尤其在边缘区域优势明显。
- 流匹配的直线传输路径比从噪声开始的扩散路径更高效，单步采样即可合成高质量散景，推理速度提升约6倍（对比Generative Photography）。
- Bokeh控制适配器（BCA）通过文本提示实现了聚焦区域和模糊强度的直观语义控制，优于传统深度基参数控制。
- 从深度预测模型初始化（如DepthFM）比从图像生成模型初始化（如SD2.1）更有利于散景渲染的边界保真。
- 用户研究表明BokehFlow在感知质量上被多数用户偏好，甚至优于iPhone 15 Pro的硬件渲染效果。

## 7. 优点

- **创新性**：首次将流匹配用于散景渲染，设计直接传输路径，避免深度依赖，实现高效、可控生成。
- **控制灵活性**：通过自然语言提示即可控制聚焦区域与模糊强度，无需物理参数调整，用户友好。
- **效率高**：单步采样，推理时间显著低于扩散模型和部分传统方法。
- **鲁棒性**：在深度不可靠的真实场景中表现优于依赖深度的方法。
- **实验全面**：包含合成/真实数据集、多种基线、消融及用户研究，验证了方法有效性。
- **开源友好**：基于公开预训练模型（CLIP、DepthFM、Diffusers库），可复现性高。

## 8. 不足与局限

- **控制粒度有限**：当前聚焦区域控制限于离散区域（前景/背景），未实现连续焦距控制。
- **未处理运动物体**：工作集中在静态图像，未考虑视频场景中的时序一致性。
- **数据集局限性**：CBD基于ReDWeb的深度图渲染，可能未覆盖真实弱光或复杂遮挡场景；EBB400只有400对，规模较小。
- **边缘评估简单**：使用Sobel算子提取边缘，可能无法捕捉复杂纹理边界，存在偏差风险。
- **未与最新大模型对比**：如未对比Bokeh Diffusion等同期工作（虽提及但未在定量实验中纳入）。
- **潜在偏见**：用户研究样本量（52人）有限，且图像来自iPhone 15 Pro，可能偏好特定风格。
- **算力信息不足**：仅说明单GPU训练，未提供精确训练时间，难以评估可复现成本。

（完）
