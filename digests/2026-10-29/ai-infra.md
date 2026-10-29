# AI 基础设施日报 2026-10-29

> 生成时间: 2026-10-29 14:08 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目 AI 基础设施报告 — 2026-10-29

## 1. 生态概览

今天的活动显示，生态系统正将 **缓存与状态管理的正确性** 而非原始吞吐量作为主战场：vLLM、SGLang、llama.cpp 和 LiteLLM 都有围绕 KV/前缀缓存语义、权重传输验证和支出核算的开放或进行中工作。模型支持由 **混合/MoE 与投机解码架构** 主导（Kimi-K3、DeepSeek-v4.1、Qwen3.5/3.8、Mistral-Large-4-1T、GLM-5.3、LFM3），后端则从 NVIDIA 扩展到 ROCm RDNA3/4、Intel SYCL、Qualcomm Hexagon、Ascend NPU 和 T-Head PPU。发布节奏分化明显：llama.cpp 发布每日构建，LiteLLM 在 24 小时内发布了三个版本，而 vLLM 和 SGLang 处于评审与分诊模式，没有发布。最清晰的系统性风险信号是：在高严重度列表中，**静默失败**——跨请求状态泄漏、空操作权重更新、`$0` 支出日志、被截断的训练数据集——数量超过显式崩溃。

## 2. 活动对比

计数反映 24 小时摘要中出现的不同条目，而非仓库总量。

| 项目 | 层级 | 引用的 Issue 数 | 引用的 PR 数 | 发布状态 |
|---|---|---|---|---|
| **vLLM** | 数据中心推理引擎 | ~13 | ~17 | 24 小时内无发布；以评审/分诊为主 |
| **SGLang** | 数据中心推理引擎 | ~27 | ~18 | 24 小时内无发布；CI 不稳定阻碍合并 |
| **llama.cpp** | 本地/边缘运行时 | ~18 | ~21 | b11971–b11982（12 个每日构建） |
| **Ollama** | 本地运行时 + 分发 | ~17 | ~10 | **v0.40.13**（LFM3 跨平台） |
| **LiteLLM** | LLM 网关 | ~22 | ~17 | **3 个发布**：1.108.0-rc.1、1.106.6、1.105.7 |
| **Unsloth** | 微调 + Studio | ~15 | ~17 | **v0.1.912-beta** |

**解读这些计数：** SGLang 的 Issue 数量偏高，部分原因是结构性的（如 #42752 不稳定测试、#34510 PD 统一等长期跟踪项），而非新缺陷。LiteLLM 的发布速度是组内最高，但每次发布仅做 cosign 签名且没有 changelog，这会把验证负担转移给运维人员。llama.cpp 的十二个构建表明其合并流水线稳定，而非功能里程碑。

## 3. 模型支持竞赛

**领先者——数据中心推理引擎。** vLLM 和 SGLang 在前沿架构支持上明显领先：

- **vLLM：** Muse Spark NVFP4（混合 MegaMoE/TRT-LLM 内核）、Mistral-Large-4-1T-A52B 配方、Kimi-K3 DSpark 采样（ROCm）、Qwen3-Next/3.5 融合 QK-norm+RoPE+gate、DeepSeek-v4.1 稀疏 MLA（gfx942）、Nemotron VL 设备侧预处理、OpenVLA chat-template 回退。
- **SGLang：** `sensenova_loop` 多模态（衍生自 Qwen3.5）、DFlash + Nemotron Bolt E4B GGUF、面向 DeepSeek-V4 的 Ascend NPU DSpark、Kimi-K3 Quark FP8 KDA in-proj + MLA absorb、Wan2.2/Wan-Animate `convrot_int8`、GLM-5.3-Flash（SM90）和 Qwen3.8-Flash-Next（gfx950）跟踪项。同时推进 AMD **消费级** RDNA3/RDNA4（gfx1100/1101/1200/1201）与 T-Head PPU。

**领先者——非主流/边缘后端。** llama.cpp 在非主流目标的广度上无可匹敌：**RDNA4_M（gfx1170/1171）** WMMA 目标、**TML Inkling** 架构，带 banded Flash Attention、**DiffusionGemma** 扩散支持、**Spark2.5 DSpark** 草稿模型，以及 Qualcomm Hexagon（UPSCALE、分块 IM2COL）、OpenCL/Adreno q4_K GEMM 和 SYCL 量化 KV FA。

**领先者——开箱即用的消费级可用性。** Ollama 的 **LFM3 系列**（文本/音频/全模态）已在 v0.40.13 中跨平台支持 Linux 和 Windows；一个开放 PR 新增 **Windows + CPU 上的 MLX runner**。

**落后——微调。** Unsloth 仍将 **LFM3-1.7B 支持列为开放请求**（需求最高项，10 👍）；已有部分 MLX LFM3 支持，但模型未出现在 `/v1/models` 中。

**网关层。** LiteLLM 不发布内核，但新增了 **Claude 的 Vertex AI 批处理**、**DashScope 图像编辑 + TTS**、**hosted_vllm 模型发现**，以及两个新提供商（**CLF AI Gateway**、**eu/jev**）。

## 4. 性能前沿

优化工作集中在五个领域，并明显从原始 FLOPs 转向缓存与内存：

**KV 缓存（最大集群）。** vLLM 有三个开放的 PR 修复分层卸载正确性（#62615、#64444、#61717），以及一个 hybrid-GDN 前缀缓存 MTP 缺陷（#52244）；TurboQuant 的 8 位以下 KV（#38171）是需求最高的未合并功能（52 条评论，129 👍），但在 Ampere 混合 MoE 故障（#40124）之后仍以 Hopper 为中心。llama.cpp 合入了**直接在 q4_0/q8_0 KV 块上运行的 SYCL DPAS flash attention**（b11980），消除了 decode 阶段的 dequant/convert。SGLang 记录了 hybrid GDN 上 **HiCache 主机层命中率崩塌**（#45400），并计划实现**超节点范围的 L1/L2 KV 共享与缓存位置感知路由**（#43098）。

**内核。** vLLM：带 PDL 的单遍 Triton RMSNorm（#64290）、ROCm 融合 QK-norm+RoPE+gate（#51406）、gfx942 上的稀疏 MLA 预填充（#60102）。SGLang：面向 bf16 SSM 状态池的 AITER Gluon `chunk_kda`（#47036）、Kimi-K3 上的 FP8 KDA in-proj + MLA absorb（#44362）。llama.cpp：面向 Q4_0/K-quants/IQ3/MXFP4 的向量化 dequant 存储（b11979）、Hexagon HVX IM2COL 与转置 CONCAT DDR 带宽（b11976/b11981）。Unsloth：bitsandbytes 4 位 GEMM 分发绕过（#15971）、面向 batch-1 decode 的整步 CUDA graph（#15976，需主动启用）。

**批处理 / 调度。** vLLM MRV2 采样器 fp32 logits 拷贝增长（#62560）；SGLang 低并发下的步级 CUDA graph + 融合 EAGLE 验证（#46211、#46210）；llama.cpp ClassLRU 混合 MoE 缓存报告 **QFN 上 decode +6%、GLM 上 +7%、专家上传减少 8%**（#31233）。

**量化。** vLLM 在 SM120（RTX PRO 6000 Blackwell）上的 MXFP4 后端选择仍有未解决缺口（#31085、#30135）；SGLang 的 `convrot_int8` 已针对 Wan2.2 验证；llama.cpp MoE `--moe-cache-cpu-share auto` 配合 CRF 准入（#31286）。

**分布式服务。** vLLM NCCL 权重传输在 XPU/禁用 PyNCCL 时静默空操作（#57022）；SGLang 在 Ray 下的 PD 分离当前损坏，等待 #47282；llama.cpp `--split-mode tensor` 在第二个请求时崩溃（#29466）。

## 5. 层级定位

这六个项目占据四个不同层级，今天的活动可以清晰地映射到这些层级：

- **数据中心推理引擎——vLLM、SGLang。** 通过 TP/EP 进行高吞吐批量推理、分离式 prefill/decode、量化 KV、投机解码。其工作主要围绕缓存正确性、权重生命周期和多 rank 协调。它们在前沿模型 day-0 支持和 AMD/加速器覆盖面上竞争。

- **本地/边缘运行时——llama.cpp。** 基于 GGUF，支持 CPU/混合/GPU 卸载，后端覆盖广泛（CUDA、ROCm、SYCL、Hexagon、OpenCL、Metal）。专注于量化格式和非 NVIDIA 芯片的内核级优化，并持续引入新架构支持（Inkling、DiffusionGemma）。

- **本地运行时 + 分发——Ollama。** 将 llama.cpp/MLX 封装在分发与用户体验层之后。其竞争面在于开箱即用的模型可用性（LFM3）、OpenAI API 兼容性和跨平台覆盖——而非内核创新。

- **LLM 网关——LiteLLM。** 位于上述所有层之上的路由器：提供商抽象、虚拟密钥、预算、支出日志、协议转换（OpenAI ↔ Anthropic ↔ Vertex ↔ Responses API）。其路线图项 #31263（**目标为低于 1ms 开销的 Rust 网关**）是跟踪器中信号最强的架构押注。

- **训练/微调——Unsloth。** LoRA/QLoRA 训练加 Studio；它也有 MLX 推理路径，但主要层级是微调。其 Issue 主要集中在数据集格式化、内存估算和 Studio 用户体验，而非服务吞吐。

**含义：** 2026 年的生产栈通常组合全部四层——例如，用 vLLM 或 SGLang 提供服务，用 LiteLLM 作为网关，用 Ollama/llama.cpp 支持本地/开发和边缘，用 Unsloth 做适配。这种分层意味着许多“bug”实际上是跨层的：LiteLLM 的 `$0` 流式支出与提供商上报的 `usage.cost` 相互作用；Ollama 的 `/v1/models` 返回 `null` 会破坏强类型网关客户端。

## 6. 趋势信号

**1. 静默失败模式是主要风险类别。** 四个项目中最高严重度的事项全部是静默的：llama.cpp HIP Gated Delta Net 泄漏循环状态并逐字重放先前提示（#29092）；vLLM NCCL 权重传输报告成功却移动零字节（#57022）；LiteLLM 流式命中日志记录 `$0`（#48937）；Unsloth 在 completions 模式下屏蔽超过 80% 的 Alpaca 行（#15894/#15895）。**关注：** 建立显式验证（传输后校验和、行数审计、工具调用存在性检查），而不是信任 HTTP 200。

**2. 投机解码已成熟，但尚不可组合。** MTP/EAGLE/DFlash 已广泛发布，但组合缺陷正在浮现：vLLM hybrid-GDN 前缀缓存在 MTP 下未命中（#52244）；SGLang 通过 `update_weights_from_disk` 发生静默草稿权重损坏（#46806）；llama.cpp 的 draft-MTP 上下文按 `n_ctx()` 而非 `n_ctx_seq()` 设定大小（#28433）。**关注：** 在 MTP *开启* 而非仅关闭时对缓存命中率做基准测试。

**3. 混合/MoE 架构已成为新常态——并且打破了原有假设。** GDN、DSA、mamba、MoE 索引大小设定（Inkling 中的 `int64_t`）以及专家卸载缓存出现在每个引擎中。以 Hopper 为中心的代码路径在 Ampere 上仍然失败（vLLM #40124）。**关注：** 按架构族而非仅按模型大小做容量规划。

**4. 加速器碎片化正在加速，而非收敛。** AMD 数据中心（gfx942/gfx950）如今已是一等公民；消费级 RDNA3/4 正在推进，但**并非默认安全**（SGLang #47254——RDNA 启动到 `aiter`，而后者没有 RDNA 内核）；Intel SYCL、Qualcomm Hexagon、OpenCL/Adreno、Ascend NPU 和 T-Head PPU 都有活跃工作。**关注：** vLLM 在 Strix Halo APU 上仍比 llama.cpp 单流慢约 6–7×（#34579）；批量吞吐是当前 AMD/APU 的唯一切入点。

**5. 缓存感知路由是下一个网关战场。** SGLang 正计划超节点范围的 L1/L2 KV 共享（#43098）和 PD 单协议统一（#34510）；LiteLLM 已根据 `supports_web_search`（#49005）和 prompt-cache 方言（#45295）进行路由。**关注：** 如果你今天自建网关，可能是在重建 SGLang/LiteLLM 将在 2–3 个季度内占据的东西。

**6. OpenAI 兼容性在边缘处仍是泄漏的抽象。** Ollama 返回 `"data": null` 而非 `[]`（#19381），对 content-parts 数组丢弃 reasoning（#19380），并在 `tool_calls` 旁静默接受无效内容类型（#19389）。SGLang 的 Kimi-K3 在组合 tools + JSON schema 时返回 400，而 glm47/DeepSeek **静默丢弃工具调用并产生幻觉**（#47167）。**关注：** 验证工具调用的*存在性*，而不仅仅是状态码；在强类型 SDK 中为列表字段增加 null 容忍。

**7. 冷启动与启动延迟是一等生产成本。** SGLang FlashInfer `cascade` JIT 在首个带前缀请求内编译约 4.5s（#46803）；LiteLLM 多 worker 代理因 uvicorn 的 5s 健康检查而陷入崩溃循环（#45039，已修复）；Unsloth 正在系统性削减 Studio 启动时间（#15943、#15947）。**关注：** 在自动扩缩/缩容到零的部署中，用一个带前缀的假请求预热。

**8. Rust 重写正在向网关和前端热路径收敛。** vLLM 的 Rust 前端（#64492）和 LiteLLM 的低于 1ms Rust 网关（#31263）都指向同一目标：Python 无法承受的控制面开销。**关注：** 下一个数量级的延迟收益将来自网关/前端层，而非内核。

**9. 供应链与气隙环境可运维性支持不足。** LiteLLM 在启动时下载 Prisma 二进制（#33167，自 7 月起开放，8 👍）；Unsloth 的 py3.13 安装通过 `outlines_core` pin 需要 Rust 工具链（#47242）。**关注：** 对于受监管部署，内置二进制并固定 Python 版本——不要依赖运行时拉取。

**10. 过期关闭并不等于修复。** SGLang 今天明确将五个以 `inactive` 标签关闭的 Issue（FP8 KV logprob 不匹配、DCP OOB、HiCache 写穿缺口、PD `KVTransferError`）标记为**过期关闭，而非已修复**。**关注：** 在分诊回归时检查 `closed_reason`——GitHub 默认视图会夸大解决情况。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 摘要 — 2026-10-29

## 今日亮点
过去 24 小时内没有新版本发布；活动主要集中在 PR 评审和 issue 分类处理上。核心主题是 **KV-cache/offload 正确性** —— 四个独立的 bugfix PR 分别针对分层缓存命中上报、驱逐竞态，以及 MTP 投机解码下的 prefix-cache 行为。与此同时，面向 Kimi-K3、Qwen3.5 和 DeepSeek-v4.1 的 ROCm 性能工作正在推进，模型支持方面则继续推进 Muse Spark NVFP4 和 Mistral-Large-4-1T-A52B recipe。

## 新模型与硬件支持
- **[PR #64525][1] — 面向 Muse Spark NVFP4 的 Hybrid MegaMoE/TRT-LLM 执行路径**（NVIDIA）。新的模型路径，将 MegaMoE 与 TensorRT-LLM kernel 结合以支持 NVFP4 权重。
- **[PR #64524][2] — Muse Spark 的 DeepGEMM commit 升级**（已关闭/已合并）。上述改动所需的 kernel 依赖更新。
- **[PR #64527][3] — Mistral-Large-4-1T-A52B recipe。** 面向 1T 参数 / 52B 激活 MoE 的部署 recipe。
- **[PR #64031][4] — ROCm 上的 Kimi-K3 DSpark Markov 采样。** 将词表上的采样跨 TP rank 分片，每个 draft position 仅一次 launch。
- **[PR #51406][5] — ROCm：面向 Qwen3-Next/Qwen3.5 的融合 QK-norm + RoPE + gate Triton kernel**，此前仅在 CUDA 上开放。
- **[PR #60102][6] — ROCm gfx942：将稀疏 MLA prefill（≥512 个 query row）路由到 AITER `pa_prefill_sparse`**，用于 DeepSeek-v4.1。
- **[PR #57924][7] — Nemotron VL：将图像/视频归一化移至 device**，消除关键路径上的 CPU 预处理。
- **[PR #64362][8] — OpenVLA chat-template 回退**，修复通过 `/v1/chat/completions` 服务 `openvla/openvla-7b` 时出现的 HTTP 400。
- **[Issue #31085][9] / [Issue #30135][10] — SM120（RTX PRO 6000 Blackwell）NVFP4/MXFP4 支持缺口。** SM120 在 MXFP4 后端选择中仍未被识别，被迫回退到 Marlin；功能请求仍开放，尚无关联的修复 PR。
- **[Issue #38175][11] — RFC 跟踪项：为 ViT 编码器实现完整 CUDA Graph**（Qwen3-VL、Kimi K2.5、GLM-V），目标是解决多模态服务中的逐层 launch 开销。
- **[Issue #30679][12] — RFC：用 `torch.accelerator` 替换 `torch.cuda`**，以打通非 CUDA 后端；仍开放，26 条评论。

## 性能与优化
- **[PR #64290][13] — 针对 fp32（Gemma）权重的单遍 Triton RMSNorm（带 PDL）。** 已在 GB300 上以 Qwen3.8-27B BF16、TP=1、MTP=3 进行端到端测试；目前为 draft 状态，尚未就绪。
- **[PR #64031][4] — ROCm 上的 Kimi-K3（TP>1）：** rank-local 的 logits `lm_head` 分片（不做 base-logits all-gather），加上每个 position 交换一条带 tag 的 peer record —— 从而将 all-gather 从采样热路径中移除。
- **[PR #62560][14] — MRV2 在负载下的内存增长。** 采样器每一步都会分配 `[num_reqs, vocab]` logits 的 fp32 副本；在 `capture_model()` 的 `empty_cache()` 之后，每个新的最大 batch size 都会触发新分配。修复方案会在 capture 后预热采样器。
- **[PR #60102][6] — gfx942 稀疏 MLA prefill** 对长查询 prefill 卸载到 AITER；较短调用仍走树内 kernel。
- **[Issue #59575][15] — 面向 Qwen3.8-Flash-Next 的 MI355X / gfx950 优化跟踪项**（GatedDeltaNet ×36 层、QuerySparse Attention ×12、HyperConnection 门控残差、MXFP4 MoE）；相关工作通过 PR 逐步合入。
- **[Issue #57406][16] — GLM-5.3 优化计划**，延续 GLM-5.2 的 `glm_moe_dsa` 工作；CUDA 相关项仍开放。
- **值得跟踪的未解决回归：** [Issue #34579][17]（AMD Strix Halo APU 上的 GPT-OSS-120B：7.5 tok/s，而 llama.cpp 为 50 tok/s）以及 [Issue #38887][18]（RTX 4090 上 Gemma 4 E4B 在强制 `TRITON_ATTN` 回退下约 9 tok/s，v0.19.0）。两者均已停滞，且没有修复 PR。

## 稳定性与回归
按严重程度排序：

1. **[PR #64141][19] — Triton attention 在创建 scale-view 期间覆盖共享 KV cache。** 内联 scale-cache 填充可能破坏已写入的缓存内容；存在正确性/数据损坏风险。修复 PR 已开放，对应 #63944。
2. **[PR #57022][20] — NCCL 权重传输静默空操作。** 当 NCCL 无法加载（例如 XPU）或 `VLLM_DISABLE_PYNCCL=1` 时，`PyNcclCommunicator` 会设置 `disabled=True`，并且每个 collective 都提前返回 —— 一轮传输报告成功，但实际搬运零字节，导致 worker 停留在旧权重上。修复 PR 已开放。
3. **[PR #52244][21] — MTP 投机解码下 Hybrid GDN prefix-cache 未命中**（Qwen3.5-122B-A10B）。缓存 prompt 从未达到预期深度，且长度是 hash 单元整数倍的 prompt 完全无法命中（1072-token GDN page）。修复 PR 已开放，需要 rebase。
4. **[PR #62615][22] / [PR #64444][23] / [PR #61717][24] — KV-offload 分层正确性问题簇。** 上报的 primary HIT 可能在 `prepare_load()` 之前被 secondary-tier promotion 驱逐；成功的 store 未发布到 lookup state；`AsyncLookupManager` 会为后续请求固定一个已解析的“absent”判定，从而隐藏其他实例写入的块。三个修复均处于开放状态。
5. **[Issue #36489][25] — Qwen3.5 397B FP8 在 A100 上意外崩溃**（v0.17.0），运行一段时间后可复现；仍开放且停滞，无关联修复。
6. **[Issue #40124][26] — TurboQuant + hybrid MoE（Qwen3.6-35B-A3B）在 Ampere SM 80–86 上异常**，有 13 个社区补丁；已关闭，若干路径中仍残留 Hopper+ 假设。
7. **[Issue #30142][27] — 异步调度下 `assert request.num_output_placeholders >= 0` 失败**（v0.12.0，8×H200）；已关闭。
8. **[Issue #26195][28] — 多模态服务在大 batch size 下 `MultiModalReceiverCache` 中缺失缓存项**；已关闭。
9. **[PR #64492][29] — Rust 前端拒绝 content 为 null/省略/空的 assistant 轮次**，与 Python 前端“返回 400 或接受”的行为不一致；修复 PR 已开放。

## 这对应用开发者意味着什么
- **分层 KV cache 在边缘场景下仍不可完全信任。** 如果你运行多实例或基于文件系统/对象存储的 KV offload，三个开放 PR 修复了未命中和过早驱逐问题。在依赖跨实例 prefix 复用来降低成本之前，请固定使用包含 #62615、#64444 和 #61717 的构建。
- **投机解码 + prefix caching 可能悄无声息地拉高 TTFT。** 在启用 MTP 的 Qwen3.5 级 hybrid GDN 模型上，与 hash 单元边界对齐的 prompt 可能完全无法获得缓存收益（#52244）。请在启用 MTP 的情况下基准测试缓存命中率，而不只是在关闭 MTP 时测试。
- **在分离式部署中验证权重更新确实已传播。** NCCL 空操作 bug（#57022）意味着一次 rollout 可能看起来成功，实际却仍在服务旧权重 —— 如果你依赖动态权重刷新，值得在传输后显式做 checksum。
- **Rust 前端正在接近功能对齐，但在边缘用例上仍落后于 Python**（#64526 中的 `return_prompt_text`、#64492 中的 assistant 轮次处理）。请根据实际消息形态来把控迁移，尤其是 tool-calling 对话记录。
- **TurboQuant KV 量化是需求最高的功能**（#38171 上有 52 条评论、129 👍），但仍未合入，且 Ampere hybrid-MoE 的故障（#40124）表明相关代码路径仍以 Hopper 为中心。目前应将 SM 80–86 上的 sub-8-bit KV cache 视为不受支持。
- **AMD 容量规划：** Kimi-K3 和 Qwen3.8-Flash-Next 优化正逐步落地到 gfx950/MI355X，但 APU（Strix Halo）上的单流吞吐仍落后 llama.cpp 约 6–7× —— vLLM 在该平台仍是面向批量吞吐的方案，而不是低延迟方案。

[1]: https://github.com/vllm-project/vllm/pull/64525
[2]: https://github.com/vllm-project/vllm/pull/64524
[3]: https://github.com/vllm-project/vllm/pull/64527
[4]: https://github.com/vllm-project/vllm/pull/64031
[5]: https://github.com/vllm-project/vllm/pull/51406
[6]: https://github.com/vllm-project/vllm/pull/60102
[7]: https://github.com/vllm-project/vllm/pull/57924
[8]: https://github.com/vllm-project/vllm/pull/64362
[9]: https://github.com/vllm-project/vllm/issues/31085
[10]: https://github.com/vllm-project/vllm/issues/30135
[11]: https://github.com/vllm-project/vllm/issues/38175
[12]: https://github.com/vllm-project/vllm/issues/30679
[13]: https://github.com/vllm-project/vllm/pull/64290
[14]: https://github.com/vllm-project/vllm/pull/62560
[15]: https://github.com/vllm-project/vllm/issues/59575
[16]: https://github.com/vllm-project/vllm/issues/57406
[17]: https://github.com/vllm-project/vllm/issues/34579
[18]: https://github.com/vllm-project/vllm/issues/38887
[19]: https://github.com/vllm-project/vllm/pull/64141
[20]: https://github.com/vllm-project/vllm/pull/57022
[21]: https://github.com/vllm-project/vllm/pull/52244
[22]: https://github.com/vllm-project/vllm/pull/62615
[23]: https://github.com/vllm-project/vllm/pull/64444
[24]: https://github.com/vllm-project/vllm/pull/61717
[25]: https://github.com/vllm-project/vllm/issues/36489
[26]: https://github.com/vllm-project/vllm/issues/40124
[27]: https://github.com/vllm-project/vllm/issues/30142
[28]: https://github.com/vllm-project/vllm/issues/26195
[29]: https://github.com/vllm-project/vllm/pull/64492

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-29

## 1. 今日亮点

CI 可靠性是目前仓库里最强的信号：flaky 测试跟踪 issue（#42752）的评论数已膨胀到 137 条，今天的多个 PR 也被失败的 `PR Test Base` / `PR Test Extra` 运行挡住。路线图方面，Q4 prefill-decode 分离规划（#43699）和 PD 单协议层统一跟踪 issue（#34510）都在推进，同时还有正式支持 AMD 消费级 RDNA3/RDNA4 显卡的具体动作（#30599、#46900、#47254）。过去 24 小时出现了两个值得关注的新正确性类 bug：DeepSeek-V4.1-Flash 在多模态流被中止时的调度器崩溃（#47218），以及通过 `update_weights_from_disk` 更新 DFlash draft 模型时发生的静默权重损坏（#46806）。

## 2. 发布与破坏性变更

过去 24 小时没有发布、API 破坏性变更或迁移说明。两个与依赖相关的变更正在进行中：

- **[Chore] 将 transformers 从 5.19.0 升级到 5.21.0** — 适配 chat-template 镜像、rope 兼容性测试、Nemotron-H 配置默认值、Transformers 后端 TP 风格、MiniCPM-V 4.6 图像尺寸（[PR #47204](https://github.com/sgl-project/sglang/pull/47204)）。
- **Python 3.13 安装需要 Rust 工具链** — 这一点隐藏且未文档化，根源是 `outlines==0.1.11 → outlines_core==0.1.26` 的精确 pin。需要先做打包修复，3.13 才能变得无摩擦（[#47242](https://github.com/sgl-project/sglang/issues/47242)）。

## 3. 新模型与硬件支持

- **AMD 消费级 RDNA3/RDNA4（gfx1100/1101、gfx1200/1201）** — 总括跟踪 issue 以及将 ROCm 从数据中心 Instinct（`gfx942`/`gfx950`）扩展出去的支持计划（[#30599](https://github.com/sgl-project/sglang/issues/30599)）；`main` 上的 CI + 上游合入计划（[#46900](https://github.com/sgl-project/sglang/issues/46900)）。
- **[Fix][AMD] 将消费级 RDNA 默认设为 Triton attention** — 在 `main` 上，ROCm 默认后端是 `aiter`，它没有 RDNA kernel，因此使用默认 flag 的 RDNA 服务器根本起不来（首次 decode CUDA graph 捕获失败）。此 PR 让消费级卡开箱即可启动（[PR #47254](https://github.com/sgl-project/sglang/pull/47254)）。
- **T-Head PPU / SAIL（Zhenwu 810、810E、M890P）** — 一等公民式上游支持的最高优先级路线图项（[#37519](https://github.com/sgl-project/sglang/issues/37519)）。
- **`sensenova_loop` 多模态** — 面向一个 Qwen3.5 衍生模型的原生文本/图像/视频推理，具备共享权重层重复和多流超连接（mHC）（[PR #47281](https://github.com/sgl-project/sglang/pull/47281)）。
- **DFlash + Nemotron Bolt E4B GGUF** — 为 `nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-4GB` 使用其 BF16 GGUF drafter 做投机解码；原生配置支持以 GGUF 存储的普通 DFlash checkpoint（[PR #47279](https://github.com/sgl-project/sglang/pull/47279)）。
- **Ascend NPU：面向 DeepSeek-V4 的 DSpark 紧凑模式** — 在投机解码中用 SPS 策略选择的窗口替代统一验证窗口 / eager 后处理（[PR #38440](https://github.com/sgl-project/sglang/pull/38440)）；**A5 显式状态 bring-up + PD 上按位置寻址的 C4 状态**（[PR #45723](https://github.com/sgl-project/sglang/pull/45723)）。
- **量化：** `convrot_int8` 已在 GB200/H100 上针对 Wan2.2 A14B（T2V/I2V）和 Wan-Animate-2 14B 验证（[PR #46048](https://github.com/sgl-project/sglang/pull/46048)）。
- **Transformers 后端模型：** 还有一个新的多模态模型待加入，但其身份在 PR 描述中被隐去（[PR #47271](https://github.com/sgl-project/sglang/pull/47271)）。

## 4. 性能与优化

- **SM120 上的 DeepSeek-V4 prefill 可能性能严重不足。** 用户报告在 4× RTX PRO 6000（TP=4、PCIe、无 NVLink）上约为 2–7K tok/s，而相同硬件上 vLLM/marlin 约为 12.5K tok/s；怀疑存在 kernel 覆盖缺口，但尚未确认。如果你在 Blackwell 工作站卡上服务 DSV4，值得跟踪（[#33422](https://github.com/sgl-project/sglang/issues/33422)）。
- **[Perf][AMD] gfx950 KDA prefill extend 走 AITER Gluon `chunk_kda`** — 覆盖现有 HIP gfx950 kernel 遗漏的 bf16 SSM 状态池路径（`--mamba-ssm-dtype bfloat16`，用于 Kimi-K3 MI355X recipe），因为后者只接受 fp32 状态池（[PR #47036](https://github.com/sgl-project/sglang/pull/47036)）。
- **[Perf][AMD] Kimi-K3：Quark FP8 KDA in-proj + MLA absorb 走 FP8 kernel** — Kimi-K3 Quark checkpoint 中的注意力权重是按通道量化的；该改动让 KDA in-projection 和 MLA absorb 走 FP8 kernel，而不是先 upcast（[PR #44362](https://github.com/sgl-project/sglang/pull/44362)）。
- **低并发下的投机解码延迟** — 可选的 step 级 CUDA graph，加上用于 EAGLE v2 链的融合 verify/relay 路径，目标是在 1–4 请求、topk-1 时减少 host 工作和各阶段图之间的小 launch（[PR #46211](https://github.com/sgl-project/sglang/pull/46211)）；megakernel 工作增加了 DSA/MLA 绑定视图、EAGLE draft 捕获检查以及早期 draft embedding 共享（[PR #46210](https://github.com/sgl-project/sglang/pull/46210)）。
- **Diffusion：** 重放 LTX head-gate 和 RMSNorm launch，以恢复 #46109/#46110 带来的 eager host-dispatch 回归，同时不回退它们的 GPU 收益（[PR #47270](https://github.com/sgl-project/sglang/pull/47270)）；修复了单 GPU Qwen-Image-2.1-Turbo VAE tiling 中的彩色接缝，现已关闭（[PR #47092](https://github.com/sgl-project/sglang/pull/47092)）。
- **活跃的模型特定性能跟踪项：** Hopper SM90 上的 GLM-5.3-Flash（[#45373](https://github.com/sgl-project/sglang/issues/45373)）、gfx950/MI355X 上的 Qwen3.8-Flash-Next（[#44304](https://github.com/sgl-project/sglang/issues/44304)）。
- **启动延迟：** 在 pip 安装场景下，FlashInfer `cascade` 会在首个带前缀请求内触发 JIT 编译（约 4.5 秒）——这是 serverless/自动扩缩容部署的冷启动税（[#46803](https://github.com/sgl-project/sglang/issues/46803)）。
- **Router/缓存路线图：** Agentic Cache Scheduling（Q4）提议在超节点范围内共享 L1/L2 KV，并跨引擎进行缓存位置感知路由（[#43098](https://github.com/sgl-project/sglang/issues/43098)）。

## 5. 稳定性与回归

按严重程度排序：

1. **调度器崩溃会拖垮整个服务容器** — DeepSeek-V4.1-Flash：在并发中止流的情况下，多模态 embedding 路径中的 `items_offset_list` 为 `None`，导致所有 TP/EP worker 被杀。镜像 `lmsysorg/sglang:dev-dsv41`。暂无可见的修复 PR（[#47218](https://github.com/sgl-project/sglang/issues/47218)）。
2. **无界分配导致整个 TP 组 OOM 崩溃** — DeepSeek-V4.1-Flash + DSPARK：`SparsePrefillWorkspace` 无界增长（[#41076](https://github.com/sgl-project/sglang/issues/41076)）。
3. **RL/微调工作流中的静默权重损坏** — `update_weights_from_disk` 会把*目标模型*的权重加载进 DFLASH draft，因为没有 `selector` 来区分；引入 `selector` 时移除了旧的 `disable_draft_model` 逃生通道。此前在 EAGLE3 外部 draft 上也报告过相同症状。暂无修复 PR（[#46806](https://github.com/sgl-project/sglang/issues/46806)）。
4. **工具 + JSON-schema 格式无法组合使用** — Kimi-K3 返回 HTTP 400；glm47 和 DeepSeek 会静默丢弃工具调用并幻觉出一个答案，破坏 `Codex --output-schema`（[#47167](https://github.com/sgl-project/sglang/issues/47167)）。
5. **混合 GDN 模型上的 HiCache：host 层命中率崩塌** — 一旦被逐出的工作集超过约 3 个长前缀，host mamba 池就被耗尽，而 KV 仍驻留在 host 上，于是命中率降到零（[#45400](https://github.com/sgl-project/sglang/issues/45400)）。
6. **GLM-5.3 FP8 推理在满载 KV cache 并持续负载后挂起**（TP8、EAGLE、H200）（[#45781](https://github.com/sgl-project/sglang/issues/45781)）；范围更广的 GLM-5.3 bug 跟踪项仍有约 8 个未勾选项（[#37524](https://github.com/sgl-project/sglang/issues/37524)）。
7. **Ampere 上的 EAGLE 原生 MTP 采样挂起** — 在 `tree_speculative_sampling_target_only` 中空转，GPU util 100% / 低功耗（2× A2、TP=2、fp8_e5m2 KV）（[#35822](https://github.com/sgl-project/sglang/issues/35822)）。
8. **`sgl_kernel` 在 sm_75 上 AOT 逐算子 cubin 缺失** — `rmsnorm` 以 `cudaErrorNoKernelImageForDevice` 失败，而 `silu_and_mul` / `gelu_and_mul` 可以工作；没有回退到 torch/Triton 的软回退路径（[#38515](https://github.com/sgl-project/sglang/issues/38515)）。
9. **API 表面正确性：** Responses API 将一次 Codex 回合重放为三个 assistant block，等于教模型在工具调用前结束回合（[#42110](https://github.com/sgl-project/sglang/issues/42110)）。
10. **CI/合并队列健康度** — flaky 测试和 NVIDIA `PR Test Base`/`PR Test Extra` 基础设施故障跟踪在 #42752（137 条评论）；与 #17050 互补（[#42752](https://github.com/sgl-project/sglang/issues/42752)）。

**正在修复：** `stop_regex` 窗口边界现在按 UTF-8 字节而不是码点计数，因此匹配被拆分到比边界更多 token 的情况不再一路跑到 `max_new_tokens`（[PR #47280](https://github.com/sgl-project/sglang/pull/47280)）；Anthropic endpoint 接受不带 `budget_tokens` 的 `thinking.type=enabled`（DeepSeek 风格客户端通过 `output_config.effort` 传入 effort），此前会返回 400（[PR #45364](https://github.com/sgl-project/sglang/pull/45364)）；PD bootstrap server 现在先于 prefill scheduler 启动，修复了 `RayEngine`/`--use-ray` 下的 Decode handshake failed … Aborted by AbortReq 问题（[PR #47282](https://github.com/sgl-project/sglang/pull/47282)）；Mooncake store RPC 现在携带按请求的 context，用于端到端 tracing（[PR #37976](https://github.com/sgl-project/sglang/pull/37976)）。

**关于关闭状态的提醒：** 今天有几个 issue 以 `inactive` 标签移至 CLOSED——H100 上索引 96 处的 FP8 KV-cache logprob 不匹配（[#25790](https://github.com/sgl-project/sglang/issues/25790)）、GLM-5.3 上的 DCP virtual-loc index-K OOB（[#36886](https://github.com/sgl-project/sglang/issues/36886)）、`--enable-dp-attention` 下 MiMo-V2.5 音频挂起（[#37059](https://github.com/sgl-project/sglang/issues/37059)）、HiCache chunked-prefill 写穿缺口（[#33714](https://github.com/sgl-project/sglang/issues/33714)），以及 PD `KVTransferError` decode-instance-dead（[#37022](https://github.com/sgl-project/sglang/issues/37022)）。这些是过期关闭，不是已修复——不要把它们当作已解决。

## 6. 这对应用开发者意味着什么

- **结构化输出 + 工具调用是目前最棘手的地方。** 如果你的 agent 依赖 `tool_choice` 与 `response_format`/JSON schema 一起使用，预计 Kimi-K3 会返回 400，glm47/DeepSeek 会静默丢失工具调用——这是最糟的失败模式，因为模型会编造答案而不是报错。在你的客户端中校验工具调用是否存在，而不只是看 HTTP 状态码（#47167）。
- **在 RL/FP 流水线中不要盲目信任权重更新端点。** 在 #46806 合入前，一次 `update_weights_from_disk` 调用可能用目标权重覆盖你的 draft 模型，并产生退化但不报错的输出。固定一个已知良好的版本，或在更新后验证 draft 配置。
- **Ray 下的 PD 分离目前是坏的**（`503 Prefill server not fully registered yet` → 握手中止）。如果你在 `--use-ray` 上跑分离式 prefill，先等等，或应用 PR #47282。
- **AMD Radeon 正变得可用，但还不是默认安全。** 在 PR #47254 合入前，使用默认 flag 的消费级 RDNA 卡无法启动，因为 ROCm 默认使用 `aiter`，而它没有 RDNA kernel；请显式选择 Triton attention 后端。
- **冷启动预算：** 在 pip 安装场景下，FlashInfer `cascade` 会给首个请求增加约 4.5 秒 JIT（#46803）。如果你使用自动扩缩容或缩容到零，请用一个带前缀的 dummy 请求预热。
- **Python 3.13 用户：** 在镜像构建中准备 Rust 工具链，或留在 3.12，直到 `outlines_core` 的 pin 放宽（#47242）。
- **值得纳入规划的路线图信号：** 超节点范围 KV cache 共享 + 缓存位置感知路由（#43098），以及 PD 单协议层统一（#34510），都指向 SGLang 将掌控更多跨引擎路由层——如果你今天正在它前面自建网关，这一点与你相关。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 摘要 — 2026-10-29

## 1. 今日亮点

过去 24 小时以后端优化为主：SYCL 合入了可直接在量化 KV 块上运行的 DPAS flash-attention 内核，Hexagon 则新增了向量化 UPSCALE、分块 IM2COL，并提升了转置 CONCAT 的带宽——这些均位于 b11971–b11982 发布窗口内。稳定性方面，目前最严重的未关闭报告是 HIP/ROCm 融合 Gated Delta Net 算子：在复用的服务器槽位上，它会跨请求泄漏循环状态，这对生产服务构成正确性风险。语法/JSON-schema 处理也受到关注，针对 LFM3 `response_format` 500 错误的修复 PR 已经开放。

## 2. 发布与破坏性变更

本批次没有显式 API 移除或破坏配置的变更。值得注意的行为修复与加固包括：

- **b11982** — hexagon：支持 UPSCALE 算子，优化了最近邻 2x/4x 与双线性 4x 路径、非连续行以及基于 gather 的暂存 ([#31331](https://github.com/ggml-org/llama.cpp/pull/31331))
- **b11981** — hexagon：通过更大的行块、双缓冲 scratchpad 和带填充的 VTCM 步长，提升转置 CONCAT 中的 DDR 带宽利用率 ([#31277](https://github.com/ggml-org/llama.cpp/pull/31277))
- **b11980** — SYCL：面向量化 KV cache 的 DPAS flash attention 内核 ([#31125](https://github.com/ggml-org/llama.cpp/pull/31125))
- **b11979** — SYCL：针对 Q4_0、K-quants、IQ3、MXFP4 的向量化重排布局反量化存储 ([#30976](https://github.com/ggml-org/llama.cpp/pull/30976))
- **b11978** — tests：修复 `comp_padding` CUDA graph 预留 ([#31344](https://github.com/ggml-org/llama.cpp/pull/31344))
- **b11977** — ggml-openvino：在动态路径上正确切片 SCALE 的 VIEW 输入 ([#31334](https://github.com/ggml-org/llama.cpp/pull/31334))
- **b11976** — Hexagon：基于块的 HVX 并行 IM2COL，支持多线程输出列块 ([#31050](https://github.com/ggml-org/llama.cpp/pull/31050))
- **b11972** — common/mtmd：按模型层数限制控制向量层索引，并对 M-RoPE 位置循环进行钳制——**输入校验加固，建议服务器更新** ([#31336](https://github.com/ggml-org/llama.cpp/pull/31336))
- **b11971** — OpenCL：面向 Adreno 的分块 q4_K GEMM，用于 batch > 1 时的分块 q4_K LM head ([#30412](https://github.com/ggml-org/llama.cpp/pull/30412))

**待定的配置语义变更：** [PR #31371](https://github.com/ggml-org/llama.cpp/pull/31371) 将不再让 `n_ctx_slot()` 无条件把每槽位上下文钳制到 `n_ctx_train`，该行为目前会使 YaRN/`--rope-scaling` 的上下文扩展失效。如果你依赖超出训练上下文的 RoPE scaling，请关注此 PR。

## 3. 新模型与硬件支持

- **Spark2.5 DSpark draft 模型** — 支持转换与推理，并导出内嵌的 `draft_model.*` 权重用于投机解码 ([#31372](https://github.com/ggml-org/llama.cpp/pull/31372))
- **DSA / dspark 扩展至新模型** — draft/投机路径上的后续工作 ([#31376](https://github.com/ggml-org/llama.cpp/pull/31376))
- **ROCm/HIP：RDNA4_M 系列** — 新增带 128b WMMA 的 `gfx1170`/`gfx1171` 目标，与现有 RDNA4 不同 ([#31373](https://github.com/ggml-org/llama.cpp/pull/31373))
- **TML Inkling 架构** — 转换器、图构建以及带状 Flash Attention 内核；在某些算子中使用 `int64_t` 以避免 MoE 索引溢出。进行中 ([#25731](https://github.com/ggml-org/llama.cpp/pull/25731))
- **DiffusionGemma** — 初步的扩散模型支持，包含 `llama-diffusion-cli` 与实时可视化。进行中 ([#24423](https://github.com/ggml-org/llama.cpp/pull/24423))
- **后端能力扩展** — Hexagon 新增 UPSCALE 与并行 IM2COL；OpenCL/Adreno 新增分块 q4_K GEMM；ggml-openvino 修复 SCALE view 语义。这些变更减轻了 [#14909](https://github.com/ggml-org/llama.cpp/issues/14909) 中跟踪的算子回退压力（开放，57 条评论）。

## 4. 性能与优化

- **SYCL 量化 KV flash attention（b11980）：** 解码和小批量（1–31 个 query token）现在可直接在 q4_0 与 q8_0 KV 块上运行 FA，消除了标量反量化/转换工作。这对 Intel GPU 上长上下文、内存受限的服务是直接收益 ([#31125](https://github.com/ggml-org/llama.cpp/pull/31125))
- **SYCL 反量化存储（b11979）：** 此前的重排布局 Q4_0 内核每个 work-item 处理一个 `qs` 字节，并执行两次相隔 16 个元素的标量存储；现在存储被加宽，K-quants、IQ3 和 MXFP4 也采用相同处理 ([#30976](https://github.com/ggml-org/llama.cpp/pull/30976))
- **Hexagon IM2COL（b11976）：** 基于块的 HVX 跨线程并行化，包含填充修复和常见路径循环展开——面向 Snapdragon NPU 上的视觉/卷积工作负载 ([#31050](https://github.com/ggml-org/llama.cpp/pull/31050))
- **Hexagon 转置 CONCAT（b11981）：** 将行块大小加倍并采用 scratchpad 双缓冲，以提高 DDR 带宽利用率 ([#31277](https://github.com/ggml-org/llama.cpp/pull/31277))
- **OpenCL/Adreno q4_K GEMM（b11971）：** 将分块 dp4a q4_K GEMM 拆分到独立文件，并加入 dp4a 能力检查；有利于 batch > 1（prompt 处理） ([#30412](https://github.com/ggml-org/llama.cpp/pull/30412))
- **MoE 缓存重构：** ClassLRU 混合方案（类似 LRFU/MQ，带衰减的使用历史）报告称，在热缓存下相比 SLRU，**QFN 上解码 +6%、GLM 上 +7%，专家上传减少 8%** ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233))；配套 PR 新增 `--moe-cache-cpu-share auto`，并为 CPU miss 引入 CRF 准入 ([#31286](https://github.com/ggml-org/llama.cpp/pull/31286))
- **后端采样内存：** 主机端概率/候选缓冲区现在按绑定采样器链的最大输出宽度分配，而不是按词表宽度 ([#29576](https://github.com/ggml-org/llama.cpp/pull/29576))
- **Hopper 上的 CUDA graphs：** 当内核的线程块簇维度变化时重新实例化 graph；cuBLAS F16 GEMM 会根据 `m` 选择不同的簇形状（例如 m=1200 时为 5,1,1，而 m=1040 时为 2,2,1），同时共享同一个内核 ([#31284](https://github.com/ggml-org/llama.cpp/pull/31284))

## 5. 稳定性与回归

按严重程度排序：

1. **跨请求状态污染（严重，开放）：** HIP/ROCm 融合 Gated Delta Net 算子在复用的服务器槽位上会跨请求携带循环状态（qwen35/qwen35moe）；较早 prompt 的文本会逐字出现在后续补全中。暂未见修复 PR——在 ROCm 上可通过每请求独立槽位或使用非混合模型缓解 ([#29092](https://github.com/ggml-org/llama.cpp/issues/29092))
2. **CUDA graphs 在 RTX 5090 Laptop / sm_120 上挂起 GPU channel（开放）：** 出现 RC watchdog + Xid 8；`GGML_CUDA_DISABLE_GRAPHS=1` 是完整的规避方案 ([#27330](https://github.com/ggml-org/llama.cpp/issues/27330))
3. **语法 / JSON-schema 正确性问题组：**
   - LFM3：任何 `response_format`（`json_schema` 或 `json_object`）都会返回 HTTP 500 “Unexpected empty grammar stack”；**修复 PR 已开放** ([#31328](https://github.com/ggml-org/llama.cpp/issues/31328), [#31374](https://github.com/ggml-org/llama.cpp/pull/31374))
   - `json-schema-to-grammar` 会把原始控制字符（例如 `\u0000` 产生的 NUL）逐字复制到 `pattern` 中 → “failed to parse grammar” ([#31199](https://github.com/ggml-org/llama.cpp/issues/31199), [#28697](https://github.com/ggml-org/llama.cpp/pull/28697) 处理 direct-schema 形式)
   - JSON schema 中的大型 `enum` 会导致语法构建呈超线性增长，使一个核心满载数十秒——构成服务端 DoS ([#29457](https://github.com/ggml-org/llama.cpp/issues/29457))
4. **SYCL prompt 处理回归：** 在 CPU offload 的 MoE 专家（`-ncmoe`）下，`llama-server` 在 b11804 相比 b11046 慢约 35%；`llama-bench` 未能复现，表明问题出在 server 路径 ([#31262](https://github.com/ggml-org/llama.cpp/issues/31262))
5. **macOS 14 / M2 Ultra 上 Metal 初始化失败：** `unary.metal` 中的 constexpr bfloat 转换 ([#31272](https://github.com/ggml-org/llama.cpp/issues/31272))
6. **`--split-mode tensor` 崩溃：** 在 2x/3x Tesla P100 + CUDA 环境下，第二个请求触发 `GGML_ASSERT(bcj.nodes[i])` ([#29466](https://github.com/ggml-org/llama.cpp/issues/29466))
7. **投机解码缺陷：** draft-mtp 上下文按 `llama_n_ctx()` 而非 `llama_n_ctx_seq()` 设置大小，在大 `--ctx-size` 下会在解码入口杀死 server ([#28433](https://github.com/ggml-org/llama.cpp/issues/28433))；mtmd 图像块会在 draft KV cache 中留下位置空洞 → 使用 `draft-dflash` 时 HTTP 500 ([#27408](https://github.com/ggml-org/llama.cpp/issues/27408))；sidecar MTP draft 失败并报 `blk.0.attn_q not found` ([#29345](https://github.com/ggml-org/llama.cpp/issues/29345))；另有报告称投机解码整体性能下降 ([#31220](https://github.com/ggml-org/llama.cpp/issues/31220))
8. **SYCL CPY 正确性：** 拷贝到 q4_1/q5_1/q8_0 的结果与 CPU 参考不一致 ([#31140](https://github.com/ggml-org/llama.cpp/issues/31140))
9. **安全：** 可通过 `?q=` WebUI 查询参数进行指令注入 ([#25790](https://github.com/ggml-org/llama.cpp/issues/25790))；通过嵌套重复触发的 GBNF 栈溢出（`llama_grammar_advance_stack`）现已关闭 ([#18988](https://github.com/ggml-org/llama.cpp/issues/18988))——b11972 的边界加固是相关的后续工作
10. **Server 生命周期：** 初始化期间 Metal OOM 仍报告“model loaded”并绑定端口，随后每个请求都返回 500——已作为 stale 关闭 ([#27309](https://github.com/ggml-org/llama.cpp/issues/27309))；在 b8638→b9016 server 重写后，Jetson Orin NX 在开始服务前挂起 ([#29499](https://github.com/ggml-org/llama.cpp/issues/29499))

## 6. 对应用开发者的意义

- **Intel GPU 上的长上下文成本更低了。** 借助 q4_0/q8_0 KV 上的 DPAS FA，解码密集型工作负载可以保留量化 KV cache，而无需回退到反量化再转换，直接降低多轮 agent 的内存压力。请在你的 SKU 上验证：SYCL 同时存在约 35% 的 prompt 处理回归，且与 `-ncmoe` 相关 ([#31262](https://github.com/ggml-org/llama.cpp/issues/31262))。
- **将语法约束输出视为加固面。** 如果你接受用户提供的 schema，那么 `response_format` 500 错误 ([#31328](https://github.com/ggml-org/llama.cpp/issues/31328)) 和超线性 `enum` 编译 ([#29457](https://github.com/ggml-org/llama.cpp/issues/29457)) 都可能被攻击者触达。限制 schema 大小、预编译静态语法，并为服务器设置请求超时。
- **推理预算 + `enable_thinking: false` 是脆弱的组合。** llama.cpp 会强制插入一个 `</think>`，可能与模板插入的标签冲突；修复正在评审中 ([#31374](https://github.com/ggml-org/llama.cpp/pull/31374))。在合并前，避免同时使用 `--reasoning-budget 0` 与 `response_format`。
- **在 [#29092](https://github.com/ggml-org/llama.cpp/issues/29092) 解决之前，不要将混合模型请求共置在共享 ROCm 槽位上**——其故障模式是静默的跨租户 prompt 泄漏，这是最糟糕的一类服务 bug。
- **Blackwell 笔记本运维人员：** 在 sm_120 上主动设置 `GGML_CUDA_DISABLE_GRAPHS=1`，直到 [#27330](https://github.com/ggml-org/llama.cpp/issues/27330) 落地；该故障是 GPU channel 硬挂起，而不是慢路径。
- **MoE 部署应评估 `moe-cache`。** ClassLRU 工作 ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233), [#31286](https://github.com/ggml-org/llama.cpp/pull/31286)) 显示，在热缓存下可获得个位数百分比的解码提升，且专家上传明显减少——对于 PCIe 流量成为瓶颈的 CPU offload MoE 很有意义。
- **升级到 b11982 或更高版本以获得输入加固。** [b11972](https://github.com/ggml-org/llama.cpp/pull/31336) 中的控制向量与 M-RoPE 边界检查，堵住了一类畸形张量名导致的崩溃，这对任何加载用户提供 GGUF 的人都相关。

---

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-29

## 今日亮点
Ollama 发布了 **v0.40.13**，将 Liquid AI **LFM3** 系列——包括 `lfm3-audio`（TTS）和 `lfm3-omni`——引入 Linux 和 Windows，此前仅支持 Apple silicon。Issue tracker 显示正在积极强化 **OpenAI 兼容语义**，针对 assistant reasoning 丢失、`null` 与 `[]` 响应字段以及无效 content-type 处理的修复正在进行中。与此同时，若干高严重级别的 MLX 和工具调用回归仍未关闭，而一个为 **MLX runner 添加 Windows/CPU 支持**的 PR 表明后端正在更广泛地收敛。

---

## 发布与破坏性变更

- **v0.40.13 — LFM3 登陆 Linux 和 Windows**  
  `lfm3`、`lfm3-audio` 和 `lfm3-omni` 现可在 Linux、Windows 和 Apple silicon 上运行。此前这些模型在非 Apple 平台上会失败，并报错“this model requires MLX support.”  
  [Release v0.40.13](https://github.com/ollama/ollama/releases/tag/v0.40.13) · [LFM3 model](https://ollama.com/library/lfm3) · [lfm3-audio](https://ollama.com/library/lfm3-audio) · [lfm3-omni](https://ollama.com/library/lfm3-omni)

- **MLX 运行时要求文档缺失（已关闭）**  
  用户在 Linux 上拉取 `lfm3` 时遇到不清晰的 MLX runtime 错误；该要求此前未文档化。现已关闭，平台支持已在 v0.40.13 落地。  
  [Issue #19369](https://github.com/ollama/ollama/issues/19369) · [Issue #19343](https://github.com/ollama/ollama/issues/19343)

- **CI/Metal 规避措施（无 API 影响）**  
  固定 Buildx 以避免 CI 死锁；为旧版 macOS 上的 Metal `unary` 库编译回归打临时补丁。  
  [PR #19373](https://github.com/ollama/ollama/pull/19373) · [PR #19372](https://github.com/ollama/ollama/pull/19372)

---

## 新模型与硬件支持

- **LFM3 系列跨平台** — 文本、音频（TTS）和 omni 模型现已在 Linux 和 Windows 上受支持，不再仅限于 Apple silicon。  
  [Release v0.40.13](https://github.com/ollama/ollama/releases/tag/v0.40.13)

- **MLX runner：原生 Windows + CPU 后端（开放 PR）** — 为 MLX runner 增加 Windows 执行能力，并在 Metal/CUDA 之外提供 CPU 后端，同时启用完整 TTS 推理（例如 `phonon:en`）。尚未合并/发布。  
  [PR #19366](https://github.com/ollama/ollama/pull/19366)

- **视觉能力上报已修正（已关闭）** — 仅带音频投影器的模型（例如 Voxtral Mini）不再错误声明 `vision`。这对基于能力的路由很重要。  
  [PR #19328](https://github.com/ollama/ollama/pull/19328)

- **已请求但未落地** — Windows 上的原生音频/视频输入与音频输出（[#19390](https://github.com/ollama/ollama/issues/19390)）；Bonsai 2 的三值量化（[#19388](https://github.com/ollama/ollama/issues/19388)，已关闭）。

---

## 性能与优化

本时间窗口未发布吞吐量或延迟基准数据。相关基础设施工作：

- **Runner 驱逐竞态已修复（已关闭）** — 防止已标记驱逐的 runner 在 loaded-runner 快速路径上被并发请求“复活”；处理 scheduler/session-duration 相关问题。  
  [PR #17416](https://github.com/ollama/ollama/pull/17416)

- **为默认启用 flash-attention 的架构提供显式设置（开放）** — 针对 GPT-OSS Q8 在长上下文下的崩溃；在部分 offload 下 `auto` flash-attention 被禁用。主要是稳定性问题，并对长上下文服务的显存/延迟有影响。  
  [PR #17477](https://github.com/ollama/ollama/pull/17477)

- **截断时保留最新用户消息（开放）** — 修复多步工具循环中截断删除当前用户查询、导致 `500: no user query found` 的问题。  
  [PR #18697](https://github.com/ollama/ollama/pull/18697)

---

## 稳定性与回归

按严重程度排序：

1. **`gpt-oss:120b` 加载失败 — GGUF 元数据 / MXFP4 tensor 大小溢出**（开放）  
   在 v0.40.12 上出现 `unsupported tensor "blk.0.ffn_down_exps.weight" size overflows`；同类溢出此前已在 v0.40.1 上通过 `gpt-oss:latest` 报告。会完全阻断大型 MoE MXFP4 模型。  
   [Issue #19375](https://github.com/ollama/ollama/issues/19375) · [Issue #18869](https://github.com/ollama/ollama/issues/18869)

2. **MLX 缓存丢失 + Claude Desktop 出现 HTTP 500 重试**（开放，0.40.11–0.40.12）  
   在配置 256K 上下文的 M4 Max 上，初始约 17K token 的预填充出现首次提示缓慢、重试和缓存丢失。是 #18474 的后续。  
   [Issue #19359](https://github.com/ollama/ollama/issues/19359)

3. **Qwen3 工具调用被 `go_template` 丢弃**（已关闭）  
   带 `tools` 的 `/api/chat` 返回空的 assistant 轮次（`content: ""`、`tool_calls: null`、`eval_count > 0`）；llama.cpp 的模板能正确解析同一生成结果。这对 agent 循环很严重。  
   [Issue #19377](https://github.com/ollama/ollama/issues/19377)

4. **`qwen3.8:27b-mlx` 在自由文本内容后从不发出 `tool_calls`**（开放）  
   仅当 `content` 为空时工具调用才有效；破坏了常见的“先声明再调用”agent 模式。  
   [Issue #19353](https://github.com/ollama/ollama/issues/19353)

5. **`ollama show --modelfile` 输出无法解析**（开放）  
   包含 `"""` 加空白字符的值会生成 `ollama create` 拒绝的 Modelfile — 破坏模型导出/往返工作流。  
   [Issue #19342](https://github.com/ollama/ollama/issues/19342)

6. **`deepseek-v4.1-flash:1017-cloud` 下 Claude Code 自动压缩失败**（开放）  
   长时间运行无法自动压缩，循环于“Goal not yet met”，并消耗 token。  
   [Issue #19249](https://github.com/ollama/ollama/issues/19249)

7. **`OLLAMA_NUM_PARALLEL=1` 未串行化请求**（开放，需要信息）  
   两个并发会话仍会执行；这对依赖串行化的内存受限部署有问题。  
   [Issue #19262](https://github.com/ollama/ollama/issues/19262)

8. **macOS 27：应用窗口和菜单栏图标始终不出现**（已关闭）  
   服务器和 CLI 可用；Electron/CoreUI bundle 查找失败（`com.electron.ollama`）。  
   [Issue #19370](https://github.com/ollama/ollama/issues/19370)

9. **OpenAI 兼容正确性问题**（开放，修复进行中）  
   - 当 `content` 为 content-parts 数组时，assistant 的 `reasoning` 被丢弃，且仅含 reasoning 的轮次被删除 — [Issue #19380](https://github.com/ollama/ollama/issues/19380)，修复 [PR #19384](https://github.com/ollama/ollama/pull/19384)  
   - 零模型时 `/v1/models` 返回 `"data": null` 而非 `[]` — [Issue #19381](https://github.com/ollama/ollama/issues/19381)，修复 [PR #19387](https://github.com/ollama/ollama/pull/19387)（也覆盖空 embeddings）  
   - 无效 content type 与 `tool_calls` 一起被静默接受 — 修复 [PR #19389](https://github.com/ollama/ollama/pull/19389)

10. **原生 `/api/chat` 忽略 `audio` 字段**（开放）  
    声明 `audio` 能力的模型（例如 `lfm3-omni`）只能通过 `images` 接受音频；`audio` 字段被静默丢弃且不报错。  
    [Issue #19378](https://github.com/ollama/ollama/issues/19378)

---

## 这对应用开发者意味着什么

- **音频/omni 现已跨平台，但 API 仍然别扭。** 从 v0.40.13 起，`lfm3-audio`/`lfm3-omni` 可在 Linux 和 Windows 上运行，但通过 `/api/chat` 的原生音频输入会被忽略（[#19378](https://github.com/ollama/ollama/issues/19378)）。目前请使用 `images` 字段，并在构建生产级音频流水线前持续关注该问题。

- **OpenAI 兼容客户端需要防御性解析。** 在待定修复落地前，空结果时 `/v1/models` 和 `/v1/embeddings` 可能返回 `"data": null`，且 content-parts 数组会丢失 reasoning 字段。若使用 LangChain、LiteLLM 或带类型的 SDK，请固定 Ollama 版本，并为列表字段增加 null 容忍。修复：[#19387](https://github.com/ollama/ollama/pull/19387)、[#19384](https://github.com/ollama/ollama/pull/19384)、[#19389](https://github.com/ollama/ollama/pull/19389)。

- **工具调用可靠性因版本和模型而异。** `go_template` 空轮次 bug 已关闭，但 MLX Qwen3.8 的行为（[#19353](https://github.com/ollama/ollama/issues/19353)）意味着 agent 应容忍 `tool_calls` 缺失，并避免在 MLX 构建上要求工具调用前必须有前置文本。在可选择的情况下考虑使用 llama.cpp 模板路径。

- **固定模型 digest；没有回滚机制。** 询问如何回退模型更新的用户（[#19385](https://github.com/ollama/ollama/issues/19385)）没有原生机制可用。请在部署中固化按 digest 固定的 manifest，而不是依赖 `:latest`。

- **不要依赖 `OLLAMA_NUM_PARALLEL=1` 实现内存隔离**，直到 [#19262](https://github.com/ollama/ollama/issues/19262) 解决；如果需要严格串行，请在网关/队列层强制执行串行化。

- **Windows/CPU 上的 MLX 即将到来但尚未发布。** [#19366](https://github.com/ollama/ollama/pull/19366) 将大幅拓宽 MLX 模型可用范围，包括 TTS；合并后应规划容量测试。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 文摘 — 2026-10-29

## 1. 今日亮点

LiteLLM 在 24 小时内发布了三个版本——1.108 线上的一个 RC，加上 1.106 和 1.105 上的补丁版本——发布说明均仅包含 cosign 镜像签名验证说明，因此从发布正文中无法获得变更日志细节。问题跟踪器中的主要主题是**成本核算**：今天有五份独立报告描述了在流式、非流式和预算重置路径中，支出日志记录为 `$0` 或出现幽灵支出。与此同时，一项与安全相关的修复已排入 Vertex AI Live WebSocket 透传（[#47716](https://github.com/BerriAI/litellm/pull/47716)），而 Rust 网关迁移主工单（[#31263](https://github.com/BerriAI/litellm/issues/31263)）仍是获赞最多的未关闭事项（21 👍，32 条评论）。

## 2. 发布与破坏性变更

**已发布（过去 24 小时）** — 所有镜像均使用 [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 中引入的 cosign 密钥签名：

| 版本 | 备注 |
|---|---|
| [v1.108.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.108.0-rc.1) | 下一个小版本的候选发布版 |
| [v1.106.6](https://github.com/BerriAI/litellm/releases/tag/v1.106.6) | 1.106 线上的补丁 |
| [v1.105.7](https://github.com/BerriAI/litellm/releases/tag/v1.105.7) | 1.105 线上的补丁 |

**进行中的破坏性变更（打开的 PR，`!` = 破坏性——请规划迁移窗口）：**

- [#47762](https://github.com/BerriAI/litellm/pull/47762) — `uv.lock` 和官方 Docker 镜像从 OpenAI SDK 2.33.0 → **SDK 3**（自 #44927 起已允许 `openai<4`；langchain-openai 1.1.14 和 semantic-router 此前将锁定版本保持在 3 以下）。
- [#47716](https://github.com/BerriAI/litellm/pull/47716) — `/vertex_ai/live` 现在强制执行模型级密钥访问控制；违规时以 `1008` 关闭套接字。
- [#49058](https://github.com/BerriAI/litellm/pull/49058) — 现在对 Claude Code 和 Codex 流量遵循 `litellm_settings.drop_params: false`；**携带不支持参数的请求将开始返回 400**，而不是被静默剥离（该回归引入于 v1.103.0）。
- [#34889](https://github.com/BerriAI/litellm/pull/34889) — GitHub Copilot OAuth token 直接发送到 Copilot；ghe.com 共享登录必须在升级前设置 `GITHUB_COPILOT_API_BASE`，并且调用方提供的 `api_base` 在未启用 `drop_params` 时会返回 400。
- [#48929](https://github.com/BerriAI/litellm/pull/48929) — `hosted_vllm/` 会按原样转发工具 schema 中的 `additionalProperties` 和 `strict: true`；vLLM 现在将强制执行此前被静默丢弃的 schema。

## 3. 新模型与硬件支持

- **vLLM 模型发现** — [PR #40066](https://github.com/BerriAI/litellm/pull/40066)：带 `check_provider_endpoint: true` 的 `hosted_vllm/*` 通配部署现在即使没有 API key 也会在 `/v1/models` 上列出服务器的模型，因此 Claude Code 等客户端可以针对自托管 vLLM 填充其模型选择器。
- **Claude 的 Vertex AI 批处理** — [PR #45715](https://github.com/BerriAI/litellm/pull/45715)：Vertex 上的 Claude 现在通过托管批处理路由运行（此前在创建时返回 404），将每一行序列化为原生 Anthropic Messages 行，并返回带用量和成本的 OpenAI 形状批处理行。
- **DashScope 图像编辑 + TTS** — [PR #48998](https://github.com/BerriAI/litellm/pull/48998)：面向 Qwen 图像和 TTS 模型的 `/v1/images/edits`（此前返回 500）和 `/v1/audio/speech`（此前返回 404）。
- **新提供商：CLF AI Gateway** — [PR #39324](https://github.com/BerriAI/litellm/pull/39324)：OpenAI 兼容网关，通过预付额度提供 GLM、Kimi、DeepSeek 和 Qwen 开放权重模型；定价映射中包含九个模型。
- **新提供商：eu/jev decision API** — [PR #46875](https://github.com/BerriAI/litellm/pull/46875)：欧盟托管的决策 API，具有 TypeSafe 请求格式、虚拟密钥认证、RPM 限制，并按 token 用量计费。
- **Azure AI Foundry Agents v2**（带 `agent_reference` 的 Responses API）— [Issue #25372](https://github.com/BerriAI/litellm/issues/25372) 已关闭。

## 4. 性能与优化

- **Rust 网关迁移** — [Issue #31263](https://github.com/BerriAI/litellm/issues/31263)：这是“最快、最轻量 AI Gateway”的主工单，目标是**低于 1ms 的开销**。Beta 测试者可通过链接表单报名；这是跟踪器中信号最强的路线图事项。
- **OTPM 预留爆炸问题已修复** — [PR #49054](https://github.com/BerriAI/litellm/pull/49054)：当部署设置了 `otpm` 且请求省略 `max_tokens` 时，路由器会预留模型的最大输出量。一个最大输出 128k、OTPM 为 50k 的 Claude 部署，即使桶为空，也会对此类每个请求返回 **429**。现在默认预留量已设置上限，并在调用后对 token 计数器进行对账。
- **Worker 启动崩溃循环** — [PR #45039](https://github.com/BerriAI/litellm/pull/45039)：在 CPU 受限的主机上，多 worker 代理会因 `"Child process died"` 陷入崩溃循环，因为每个 worker 在响应 uvicorn 的 **5s** 首次健康检查 ping 之前都会加载整个 LiteLLM。现在 `litellm` 命令会推迟重型导入，直到该 ping 得到响应之后。
- **Azure SDK 客户端复用忽略超时** — [PR #44204](https://github.com/BerriAI/litellm/pull/44204)：使用共享凭据的 Azure 调用复用了同一个缓存的 SDK 客户端，而不考虑每次调用的超时/重试设置，因此一个短超时批处理会污染该资源上之后的所有批处理；SDK 与 LiteLLM 重试叠加后，一个失败调用可能产生 **16 次上游尝试**。
- **缓存流的保真度/正确性** — [PR #32096](https://github.com/BerriAI/litellm/pull/32096)：流式缓存命中会按实时流顺序重放，保留推理内容、引用和音频，而不是丢弃它们并在文本之前发出工具调用。
- **提示缓存方言解析** — [PR #45295](https://github.com/BerriAI/litellm/pull/45295)：`azure_ai/` 部署现在从部署的 `api_base` 推导缓存格式，而不是使用代理全局的 `AZURE_AI_API_BASE`，从而为 Foundry 部署恢复 OpenAI 格式缓存。
- **感知 Web 搜索的路由** — [PR #49005](https://github.com/BerriAI/litellm/pull/49005)：路由器现在会为 Anthropic/Gemini/带日期的 OpenAI 搜索工具以及 `web_search_options` 优先选择 `supports_web_search: true` 的部署，避免浪费提供商调用和回退延迟。

## 5. 稳定性与回归

**严重级别 1 — 支出/预算正确性（系统性问题簇）：**

- [#48937](https://github.com/BerriAI/litellm/issues/48937) **OPEN** — 当上游在别处报告真实成本的同时报告 `usage.cost: 0` 时，流式 `/v1/chat/completions` 记录为 `$0`。报告称影响*所有流式流量*：`LiteLLM_SpendLogs` 中 `spend = 0` 且 `cost_breakdown.input_cost = 0.0`。**尚无修复 PR。**
- [#47533](https://github.com/BerriAI/litellm/issues/47533) **CLOSED** — 对于 OpenAI 兼容上游（`custom_llm_provider: openai`），在*非流式*调用中忽略了提供商报告的 `usage.cost`；尽管上游报告了成本，`response_cost = 0`。
- [#47675](https://github.com/BerriAI/litellm/issues/47675) **OPEN** — 当 `custom_llm_provider=anthropic`，但定价行位于 `model_prices_and_context_window.json` 中的其他提供商下时，`response_cost` 为 null，支出为 `$0`。
- [#35829](https://github.com/BerriAI/litellm/issues/35829) **CLOSED** — 提供商的 `usage.cost` 被原样信任为 USD，未做单位校验 → 非 USD 计价的值导致天文数字般的幽灵支出。
- [#34805](https://github.com/BerriAI/litellm/issues/34805) **OPEN** — 内存中的支出缓冲区（`update_spend`、`update_daily_tag_spend`）在代理关闭时被静默丢弃；apscheduler 任务不会刷新。
- [#37261](https://github.com/BerriAI/litellm/issues/37261) **OPEN** — 在没有 Redis 的情况下，`provider_budget_config` 报告的 `budget_reset_at` **约在 57 年后**，因此月度预算永远不会重置。

**严重级别 2 — 安全 / 访问控制：**

- [#47716](https://github.com/BerriAI/litellm/pull/47716) **OPEN, fix ready** — 作用域限定到特定模型的密钥本可在 `/vertex_ai/live` 上打开*任意* Vertex AI Live 模型，因为模型只在认证后的第一条 websocket 消息中命名。修复会根据调用方访问权限进行校验，并以 `1008` 关闭。内部副本为 [#40078](https://github.com/BerriAI/litellm/pull/40078)。
- [#28906](https://github.com/BerriAI/litellm/issues/28906) **CLOSED** — 编辑可复用凭据时，可能会用掩码占位符（`sk-p****cdef`）覆盖真实密钥，因为 `/credentials` 返回的掩码值会预填充编辑弹窗。

**严重级别 3 — 正确性 / 协议转换：**

- [#48773](https://github.com/BerriAI/litellm/issues/48773) **CLOSED** — Responses 桥接在流式 `/v1/chat/completions`（`azure/responses`）中重复工具调用参数：第一个 delta 中包含完整参数 JSON，随后又逐片段重新发送。
- [#19858](https://github.com/BerriAI/litellm/issues/19858) **OPEN since Jan** — 通过 Kilo Code / Roo Code 使用时，OpenRouter 和 Vertex Sonnet 4.5 上的 Claude 工具调用损坏。长期存在，6 条评论。
- [#41963](https://github.com/BerriAI/litellm/issues/41963) **OPEN** — `/v1/responses` 在分发给提供商之前不会将纯字符串 `input` 规范化为列表；严格提供商拒绝该输入。
- [#46095](https://github.com/BerriAI/litellm/issues/46095) **OPEN** — OpenRouter 流式块报告的是*请求的*路由器名称（`openrouter/auto`），而不是实际服务的模型（`deepseek/deepseek-v4.1-flash`）。
- [#45546](https://github.com/BerriAI/litellm/issues/45546) **CLOSED** — Mistral 的 `_strip_output_only_fields` 会从 assistant 消息中删除 `reasoning_content`/`thinking_blocks`，而不是按照 Mistral 文档将它们作为 `thinking` 块重放。
- [#47814](https://github.com/BerriAI/litellm/issues/47814) **CLOSED** — TPM 预留使用 `str(item)` 序列化 Responses 输入，将 base64 图像载荷计为文本，并使 token 估算大致**翻倍**。
- [#48449](https://github.com/BerriAI/litellm/issues/48449) **CLOSED** — Decisions 转换会将省略的概率具体化为 `0.0`，混淆了“缺失”与“报告为零”。
- [#42111](https://github.com/BerriAI/litellm/issues/42111) **OPEN** — `/v1/audio/speech` 对 `openrouter/` TTS slug 返回 500：*“Unable to map the custom llm provider=openrouter to a known provider.”*
- [#24498](https://github.com/BerriAI/litellm/issues/24498) **CLOSED** — Claude 模型会间歇性返回 `[System: Empty message content sanitised to satisfy protocol]`。

**严重级别 4 — 可运维性：**

- [#33167](https://github.com/BerriAI/litellm/issues/33167) **OPEN, 8 👍, stale** — 自 v1.92.0 起，LiteLLM 在启动时下载 Prisma 二进制文件，并在**隔离/出站受限网络**中失败。对受监管部署影响很大；尚未关联修复 PR。
- [#30126](https://github.com/BerriAI/litellm/issues/30126) **OPEN, stale** — `/cursor/chat/completions` 返回 200 且提供商已计费，但**不会创建 SpendLogs 或 Logs UI 条目**（v1.88.1）。
- [#28994](https://github.com/BerriAI/litellm/issues/28994) **OPEN** — 没有与 `os.environ/` 等价的机制来从文件读取密钥（Docker secrets、K8s 挂载卷）。3 👍。

## 6. 这对应用开发者意味着什么

- **目前不要信任流式流量的支出日志。** [#48937](https://github.com/BerriAI/litellm/issues/48937) 加上已关闭的 [#47533](https://github.com/BerriAI/litellm/issues/47533) 和 [#35829](https://github.com/BerriAI/litellm/issues/35829) 描述了同一个故障模式：上游报告的成本被丢弃或被错误信任，`$0` 传播到密钥/团队/最终用户预算中。如果你根据 `max_budget` 或 `model_max_budget` 限制用量，在信任强制执行之前，请与提供商发票对账，并在修复落地前优先使用非流式或客户端 token 计费。另请注意，最终用户的按模型预算此前已被报告为损坏（[#31842](https://github.com/BerriAI/litellm/issues/31842)）。
- **预算重置可能会静默地永不触发**（没有 Redis 时，[#37261](https://github.com/BerriAI/litellm/issues/37261)），并且**正在处理中的支出会在重启时丢失**（[#34805](https://github.com/BerriAI/litellm/issues/34805)）。对于任何与计费相关的事项，请运行 Redis 并优雅地排空代理。
- **固定到某个补丁版本，并在升级前阅读带 `!` 的 PR。** 24 小时内三个版本意味着 1.105/1.106/1.108 线都在变动。OpenAI SDK 3 升级（[#47762](https://github.com/BerriAI/litellm/pull/47762)）是会在传递依赖冲突中显现的那个（langchain-openai、semantic-router）。
- **选择加入的提供商功能持续落地，而不会破坏你的配置** — DashScope 图像编辑/TTS、Claude 的 Vertex 批处理、hosted_vllm 模型列表以及 CLF/eu-jev 提供商都是增量添加的。如果你在代理后面运行自托管 vLLM，[#40066](https://github.com/BerriAI/litellm/pull/40066) 值得采用，这样编码代理无需假密钥即可枚举你的模型。
- **隔离部署应使用低于 v1.92.0（即不包含该 Prisma 行为）的版本，或预烘焙二进制文件。** [#33167](https://github.com/BerriAI/litellm/issues/33167) 自 7 月以来一直处于 OPEN 状态，有 8 👍，且没有关联修复；启动时网络出站在受限环境中是硬性阻塞。
- **关注 Rust 网关。** [#31263](https://github.com/BerriAI/litellm/issues/31263) 的目标是低于 1ms 的网关开销。如果你今天正在为代理延迟预算做架构设计，请将 Python 网关的开销视为已知上限，并据此决定 beta 参与规模。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-29

来源：[github.com/unslothai/unsloth](https://github.com/unslothai/unsloth)

---

## 1. 今日亮点

Unsloth 发布了 **v0.1.912-beta**（“Trick or Treat + Optimizations”），在万圣节 UI 主题之外，还打包了更快的首次启动、更快的 Linux 工具调用、更快的模型加载，以及对兼容微调任务的批量训练。今天大量 PR 都来自 `danielhanchen` 的性能优化系列，目标包括 Studio 启动、RAG/压缩扫描，以及面向 batch-1 解码的可选整步 CUDA graph。稳定性方面，最严重的问题是 “Train on completions” 掩码中的**静默 Alpaca 数据集截断**（[#15894](https://github.com/unslothai/unsloth/issues/15894) / [#15895](https://github.com/unslothai/unsloth/issues/15895)），以及 macOS 15.0.1 上的**更新后模型加载回归**（[#15932](https://github.com/unslothai/unsloth/issues/15932)）。

---

## 2. 发布与破坏性变更

- **[v0.1.912-beta — "Trick or Treat + Optimizations"](https://github.com/unslothai/unsloth/releases)** — 十月季节性 UI（万圣节主题行、诡异问候语、蜘蛛/蝙蝠），外加首次启动加速、Linux 工具调用加速和模型加载改进。还支持将兼容的微调任务一起训练（“multi-L…”——发布说明在来源中被截断；看起来是 multi-LoRA / 多任务批处理）。未记录到 API 或配置层面的破坏性变更。
- **升级警告：** [#15932](https://github.com/unslothai/unsloth/issues/15932) 报告一个此前正常的模型在 macOS 15.0.1 上更新到 0.1.912-beta *之后*无法加载；[#15866](https://github.com/unslothai/unsloth/issues/15866) 报告更新本身失败。目前尚无修复 PR——建议受影响的 macOS/MLX 用户固定使用 0.1.911-beta，直到问题完成分诊。

---

## 3. 新模型与硬件支持

- **[#15588 — Support LFM3-1.7B](https://github.com/unslothai/unsloth/issues/15588)**（OPEN，10 👍）— 请求加入 LiquidAI LFM3 系列；是今天这批请求中需求最高的模型请求。
- **[#15832 — LFM3 MLX models not listed by `curl v1/models`](https://github.com/unslothai/unsloth/issues/15832)** — MLX 路径上已有部分 LFM3 支持，但模型未出现在 OpenAI 兼容的模型列表中，导致外部客户端无法发现。
- **[#15954 — [MLX] Neural Engine Prefill toggle](https://github.com/unslothai/unsloth/pull/15954)** — 针对 MLX 模型的稠密 SwiGLU MLP prefill，可选启用 ANE/GPU 拆分，并与现有 Int8 Prefill 开关并列接入。
- **[#15831 — Python 3.15 / ROCm 10.0](https://github.com/unslothai/unsloth/issues/15831)** — 请求提供 ROCm 10.0 wheels，因为 PyTorch 2.15.0 已发布它们；目前尚无承诺。
- **[#14457 — make `unsloth doctor` MLX-aware on Apple Silicon](https://github.com/unslothai/unsloth/pull/14457)** — 当前 CLI 诊断只探测 CUDA/ROCm，在基于 MLX 的 Mac 上会误报失败。
- 今天在 bug/修复中涉及的模型覆盖：MiniCPM-V-4.7-1B（Vulkan，[#14987](https://github.com/unslothai/unsloth/issues/14987)，已关闭）、Qwen-Image-2.1-Turbo（GGUF/Safetensors，[#15735](https://github.com/unslothai/unsloth/issues/15735)，已关闭）、Wan2.1-T2V-1.3B 和 FLUX.2-klein-9B（低内存首次加载修复，见 §4）、MiniMax-H3（融合 QKV，见 §4）、通过 MLX 的 Dolphin Venice Edition（[#15858](https://github.com/unslothai/unsloth/issues/15858)）。

---

## 4. 性能与优化

**解码 / 内核**
- **[#15976 — Whole-step CUDA graph for batch-1 decode](https://github.com/unslothai/unsloth/pull/15976)** — 通过 `UNSLOTH_DECODE_CUDA_GRAPHS_FULL=1` 选择启用；将 attention 核心捕获进 graph 内，而不是在其周围分段。默认行为不变。
- **[#15971 — bitsandbytes 4-bit GEMM via unsloth_zoo direct call](https://github.com/unslothai/unsloth/pull/15971)** — 在 `fast_linear_forward` 中，每次 `matmul_4bit` 调用绕过约 50 µs 的 bitsandbytes Python 调度；影响 T4 fp16 batch 5–16 默认路径和 `UNSLOTH_BNB_GEMM_4BIT=1`。

**Studio 启动与 UI**
- **[#15943 — Load chat page on first use](https://github.com/unslothai/unsloth/pull/15943)** — 从每个首屏的启动 bundle 中移除静态导入的 `ChatPage`（线程、Markdown、推理、工具渲染）。
- **[#15947 — Skip OXC validator `npm ci` when tree unchanged](https://github.com/unslothai/unsloth/pull/15947)** — 当文件树未变化时，跳过 OXC 校验器的 `npm ci`，消除每次 setup/update 运行中的冗余安装。

**RAG / 索引**
- **[#15949 — Match linked-folder paths against `.gitignore` runs](https://github.com/unslothai/unsloth/pull/15949)** — 在 30 秒重扫描中，将逐规则 `re.match` 替换为 run 匹配；此前一个 145 行的 `.gitignore` 会对每个文件/目录产生约 90 次正则调用。
- **[#15950 — Literal-led blank-line collapsing regex](https://github.com/unslothai/unsloth/pull/15950)** — `re.sub(r"\n{3,}")` 没有字面前缀，导致在非结构化 seed 分块期间必须在每个字符偏移处尝试匹配（约 10 ns/字符）。
- **[#15972 — Reuse lexical passes across compaction/prune runs](https://github.com/unslothai/unsloth/pull/15972)** — 性能剖析显示，在 2 万个归档时，查询编码 + KNN 只占压缩召回的 1–4%；成本在于关键词搜索和分支过滤。
- **[#15942 — Share one edge index for Recipe Studio connection status](https://github.com/unslothai/unsloth/pull/15942)** — 让 Recipe Studio 连接状态共享一个边索引，停止逐节点订阅完整的 edge 数组和 config map。

**低内存 / OOM 修复**
- **[#15975 — Stage fp32 video encoder on device when dense cast won't fit host RAM](https://github.com/unslothai/unsloth/pull/15975)** — 当 dense cast 无法放入主机 RAM 时，将 fp32 视频编码器分阶段放到设备上；Wan2.1-T2V-1.3B 在 12.7 GB T4 主机上首次加载会因 OOM 被终止（22.7 GB fp32 UMT5 分片 → 13.x GB fp16 cast）。
- **[#15974 — Download weights before the small-host placement decision](https://github.com/unslothai/unsloth/pull/15974)** — 冷缓存下 FLUX.2-klein-9B 在 12.7 GB 主机上加载会因 OOM 被终止；热缓存下第二次加载可在约 60 秒内完成渲染。
- **[#15956 — MiniMax-H3 unfused Q/K/V share one ConvRot rotation and act quant](https://github.com/unslothai/unsloth/pull/15956)**（CLOSED）— 融合为单个 int8 `to_qkv` 仅在去噪器被固定常驻时适用；所有显存低于约 57 GB 的卡（48 GB Ada、A100-40、L4、4090/5090）都会采用流式放置。

---

## 5. 稳定性与回归

按严重程度排序。

1. **“Train on completions”中的静默数据集截断（HIGH）。** [#15894](https://github.com/unslothai/unsloth/issues/15894)（CLOSED）/[#15895](https://github.com/unslothai/unsloth/issues/15895)（OPEN）— 在任何 instruct 模型上，一个 272 行的 Alpaca JSONL 会有 80%+ 被丢弃：Alpaca 模板从未安装，但掩码仍在搜索其标记。已在两个不相关的模型家族、Windows 11、RTX 5060 Laptop、v0.1.911-beta 和 main 上复现。训练期间的静默数据丢失是这里最糟糕的故障模式——请审计 completion-only 数据集。
2. **更新到 0.1.912-beta 后模型无法加载（HIGH）。** [#15932](https://github.com/unslothai/unsloth/issues/15932) — macOS 15.0.1，模型在更新前数周一直正常。暂无修复 PR。
3. **MLX 内存估算器严重高估（HIGH）。** [#15935](https://github.com/unslothai/unsloth/issues/15935) — 对一个能在 24 GB Mac 上正常加载的模型报告 69.7 GiB；很可能阻止原本有效的加载/放置决策。
4. **单列数据集不可用（MEDIUM-HIGH）。** [#15892](https://github.com/unslothai/unsloth/issues/15892) — `{"text": "..."}` JSONL 被报告为 `Format: unknown`，AI Assist 拒绝处理，点击 Start 静默返回预览。[#15901](https://github.com/unslothai/unsloth/issues/15901) — AI Assist 无法表达“无需列映射”，因此总是在单列数据上失败并丢弃原因。
5. **更新路径失败（MEDIUM）。** [#15866](https://github.com/unslothai/unsloth/issues/15866) — Unsloth 无法从桌面应用更新。
6. **MLX 模型渲染/发现（MEDIUM）。** [#15858](https://github.com/unslothai/unsloth/issues/15858) Dolphin Venice Edition 在 UI 内渲染不正确，但在其他应用中正常；[#15832](https://github.com/unslothai/unsloth/issues/15832) LFM3 MLX 模型缺失于 `/v1/models`。
7. **UI 缺陷（LOW）。** [#15856](https://github.com/unslothai/unsloth/issues/15856) 图片拖拽重排在 Fedora 44 上拒绝自己的 drop；[#15782](https://github.com/unslothai/unsloth/issues/15782) 聊天 textarea 内容在标签页之间重复；[#15735](https://github.com/unslothai/unsloth/issues/15735)（CLOSED）Qwen-Image Safetensors 缺失于 “On Device”；[#14987](https://github.com/unslothai/unsloth/issues/14987)（CLOSED）MiniCPM-V-4.7-1B 的 Vulkan 生成失败。

---

## 6. 这对应用开发者意味着什么

- **在信任仅 completions 的运行之前，先审计微调数据。** [#15894](https://github.com/unslothai/unsloth/issues/15894) 是一个*静默*正确性 bug——loss 曲线看起来合理，而你 80% 的 Alpaca 行却被掩掉了。请在训练产物中验证行数，而不只是预览面板，并在修复前优先使用显式 chat 模板，而不是推断的 Alpaca 格式。
- **MLX/Apple 部署请固定版本。** 0.1.912-beta 在 macOS 上有一个未解决的加载回归，外加一个在统一内存上可能高报约 3 倍的内存估算器。如果你依赖 Mac 推理/训练，请固定 0.1.911-beta。
- **Decode CUDA graph 现在可以在整步粒度上测试。** `UNSLOTH_DECODE_CUDA_GRAPHS_FULL=1`（[#15976](https://github.com/unslothai/unsloth/pull/15976)）为选择启用且默认关闭——可在不改变生产行为的情况下，为单流 batch-1 服务做基准测试。
- **视频/扩散模型的低显存服务正在积极改进。** [#15956](https://github.com/unslothai/unsloth/pull/15956) 中的流式放置行为，以及 [#15974](https://github.com/unslothai/unsloth/pull/15974)/[#15975](https://github.com/unslothai/unsloth/pull/15975) 中的冷缓存 OOM 修复，意味着低于 24 GB 的卡将越来越多地*仅在*模型已缓存时才能工作——在受限主机上应规划一次预热加载。
- **启动延迟正在下降。** 懒加载聊天页面（[#15943](https://github.com/unslothai/unsloth/pull/15943)）和跳过冗余安装（[#15947](https://github.com/unslothai/unsloth/pull/15947)）缩短了嵌入式 Studio 部署的首请求时间；如果你将 Unsloth Studio 作为本地 sidecar 交付，这一点很重要。
- **网关/发现缺口仍然存在。** 如果你用 OpenAI 兼容路由前置 Unsloth，`/v1/models` 对 MLX 服务的模型尚不可靠（[#15832](https://github.com/unslothai/unsloth/issues/15832)）；请维护显式的模型 allowlist，而不是依赖运行时发现（另见 [#15521](https://github.com/unslothai/unsloth/issues/15521) 中跟踪的上游 [opencode#59557](https://github.com/anomalyco/opencode/issues/59557)）。
- **值得关注：** 一个 Evals 标签页，通过 Studio 自带的 llama-server 对 GGUF 运行 `lm-evaluation-harness`（[#12521](https://github.com/unslothai/unsloth/pull/12521)）；Flows 的 benchmark/loop 节点，可在留出行上自动评分微调（[#15941](https://github.com/unslothai/unsloth/pull/15941)）；LoRA 运行的神经元激活捕获（[#5217](https://github.com/unslothai/unsloth/pull/5217)）；以及重建的按 GPU Live 监控（[#15630](https://github.com/unslothai/unsloth/pull/15630)）。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*