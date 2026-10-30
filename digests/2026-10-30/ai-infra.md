# AI 基础设施日报 2026-10-30

> 生成时间: 2026-10-30 02:07 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目 AI 基础设施对比 — 2026-10-30

## 1. 生态系统概览

AI 基础设施栈仍处于高频变动的建设期：vLLM 和 SGLang 在 24 小时内各触及约 500 个 PR，LiteLLM 触及 486 个，llama.cpp 每日发布多个构建。优化重心正从单纯的内核速度转向跨 ROCm、XPU、NPU、SM120/Blackwell、SYCL、Metal 和 Hexagon 的后端一致性、KV 缓存局部性与智能体调度。与此同时，稳定性与正确性报告——静默 KV 污染、调度器崩溃、权重同步损坏、工具调用截断，以及流式计费为 $0——表明生产加固正滞后于功能迭代速度。本地运行时层正收敛于量化 KV 和低位格式，而网关层正围绕成本核算、提供商抽象和路由公平性日趋成熟。对应用开发者而言，主要信号是：模型/后端/版本锁定，以及对工具调用、缓存和支出的外部验证，仍然必不可少。

## 2. 活跃度对比

| 项目 | Issue 更新数（24h） | PR 更新数（24h） | 发布状态 |
|---|---:|---:|---|
| **vLLM** | 180 | 500 | 无发布；正在移除 Model Runner V1 |
| **SGLang** | 35（显示 30） | 500（显示前 20） | 无发布；v0.5.22 运行说明 |
| **llama.cpp** | n/r | n/r | 已发布多个构建，b11978–b11998 |
| **Ollama** | n/r | n/r | 无发布 |
| **LiteLLM** | 93 | 486 | 已发布 **v1.108.0-rc.1** |
| **Unsloth** | n/r | n/r | 无发布；Studio 用户使用 0.1.911/0.1.912-beta |

*n/r = 摘要中未报告汇总数；未从可见条目推断。计数为“触及/更新”，不一定仍处于打开状态。*

**解读：** vLLM、SGLang 和 LiteLLM 是当前变动最频繁的项目。llama.cpp 持续发布，但未报告 Issue/PR 汇总数。Ollama 和 Unsloth 无发布，报告的活动也较少，但二者都有活跃的正确性和性能工作。

## 3. 模型支持竞赛

**vLLM** 在数据中心加速器 + 模型 + 量化组合方面最强：MiniMax-M3 on ROCm、DeepSeek V4.1 MXFP8 on gfx950、Qwen3-Next/Qwen3.5 fused kernels on ROCm、Intel XPU Mooncake KV transfer，以及 CPU/非 Triton MRv2 支持。它还有一个在线再量化 RFC，将支持范围扩展到 BF16/FP16/FP32 输入之外。

**SGLang** 正在推进最广泛的非 NVIDIA 加速器路线图：T-Head PPU、AMD RDNA3/RDNA4 和 TLX、Ascend NPU DSpark/EPLB/mixed-chunk DSA、Intel XPU prefill CUDA graphs 和 FP8 MoE，以及 `sensenova_loop` 和 DFlash 系列 MoE 草稿等新模型路径。它在 NPU/AMD 支持广度上领先；在本时间窗口内，vLLM 在 CUDA/ROCm/XPU 的模型 + 量化覆盖上仍领先。

**llama.cpp** 发布了新的 GGUF/边缘架构：Mistral “Le Chonk” 和 Limite 1B “Violetto”，以及 OpenCL IQ2、SYCL 量化 KV DPAS 注意力、Hexagon UPSCALE/CONCAT 和 Metal MXFP4 工作。它在消费级/边缘架构覆盖和 GGUF 相关后端方面领先。

**Ollama** 在本时间窗口内未落地新架构。它专注于 MLX 后端工作、OpenAI 兼容性修复以及音频/多模态缺口；LFM3-Omni/phonon 音频支持已被请求但尚未落地。

**LiteLLM** 不是模型运行时，但正在扩展提供商/模型覆盖：CLF AI Gateway、Z.AI/GLM 响应格式和定价支持、Azure Foundry Agents v2、Vertex Claude 批处理，以及 DeepSeek V4 reasoning-effort 处理。它在提供商抽象和网关级模型支持方面领先。

**Unsloth** 未增加新架构支持；LFM3 仍被请求但不受支持。其工作集中在微调/量化和 Studio：EXL3 后端提案、多驻留 GGUF 模型、AMD iGPU 处理、Qwen3-ASR 语音上下文内存控制，以及融合 NF4 解码。

**谁领先：** vLLM 和 SGLang 领跑数据中心服务与加速器覆盖；llama.cpp 领跑边缘/GGUF 架构支持；LiteLLM 领跑提供商/模型网关覆盖；Unsloth 专注于微调和低位优化，而非新架构对齐。

## 4. 性能前沿

优化集中在五个方面：

- **内核与后端一致性。** vLLM 具备 ViT 完整 CUDA Graph、SM80 DiffKV prefill 分块（在 200K 上下文分块上约 3.0×）、ROCm MoE top-k 和 AITER FlyDSL MXFP8 GEMM（DSv4.1 吞吐量 +3.7%）。SGLang 具备 AMD 扩散 prefill 路径（16K tokens 时 12.4→18 TF）、AMD DSA 元数据复用、NPU 缩放折叠（约 2 ms/层）和 XPU prefill 图捕获。llama.cpp 具备 Metal MXFP4 mat-vec、SYCL DPAS 量化 KV 注意力、Hexagon CONCAT 和 OpenCL dp4a 默认启用。Unsloth 声称融合 batch-1 NF4 解码带来每 token 1.6–2.3× 加速。

- **KV 缓存与前缀缓存。** vLLM 正在处理 NVFP4 KV、KV offload 快速失败行为，以及睡眠模式 CPU 备份回收。SGLang 具备 HiCache、会话感知 KV 编排、DCP 感知 draft KV 大小配置和 PD rebootstrap 修复。llama.cpp 增加了 SYCL 量化 KV 注意力，并正在跟踪 CPU q8_0 KV 回退。Ollama 存在严重的 ROCm gfx1151 KV 状态泄漏。KV 局部性现在是一阶延迟与正确性问题。

- **批处理、调度与分布式服务。** vLLM 新增 `--dp-prefill-delayer`，用于智能体多轮解码中断和 PP 解码批次均衡。SGLang 正在推进 mixed-chunk DSA、AFD 解聚和 PD rebootstrap token 重放。llama.cpp 存在一个静默的混合 LFM2 共批处理损坏 bug。Ollama 存在并行会话语义问题。LiteLLM 正在改进 least-busy 路由和 OTPM 预留。

- **量化。** vLLM 覆盖 MXFP8、NVFP4 和在线再量化；SGLang 覆盖 FP8 MoE、MXFP4 和 Ascend/NPU 布局；llama.cpp 覆盖 MXFP4、IQ2、q4_0 dp4a 和量化 KV；Ollama 跟踪 MXFP4/三值量化；Unsloth 覆盖 NF4、融合 NF4 解码和拟议的 EXL3。低位支持扩展速度快于质量验证。

- **网关与流式开销。** LiteLLM 正在移除逐 chunk 的 pydantic 异常、在路由前预留部署、核对 OTPM 计数器，并重构 DB/Redis 访问。这是 CPU/路由效率工作，不是 GPU 内核工作。

**总结：** 前沿不是单一基准测试竞赛。它是在智能体流量下的后端一致性 + 内核融合 + KV/缓存管理 + 调度。原始 token 吞吐量仍然重要，但缓存局部性和正确性现在决定生产成本。

## 5. 分层定位

| 层级 | 项目 | 作用 |
|---|---|---|
| **高吞吐服务引擎** | **vLLM、SGLang** | 数据中心推理、连续批处理、P/D 解聚、TP/PP/DP/EP、KV 管理、加速器后端。vLLM 具有广泛的 CUDA/ROCm/XPU/CPU 覆盖；SGLang 在 AMD/NPU 调度和缓存编排方面较强。 |
| **本地/边缘运行时** | **llama.cpp、Ollama** | llama.cpp 是跨 CPU、Apple Metal、SYCL、OpenCL、Hexagon、CUDA、HIP 的底层 GGUF 运行时。Ollama 是面向开发者的本地服务器/桌面封装，具备模型管理、OpenAI 兼容性和 MLX 工作。 |
| **LLM 网关** | **LiteLLM** | 提供商抽象、路由、预算/支出、鉴权、可观测性、OpenAI 兼容代理。它不提供模型内核服务；它治理跨后端的流量和成本。 |
| **训练/微调** | **Unsloth** | 微调和量化内核、Studio 工作流、低位推理优化。它通过 Studio 的 llama-server/GGUF 支持与本地运行时重叠，但以训练和数据集处理作为差异化。 |

边界正在模糊——Ollama 使用 llama.cpp/MLX，Unsloth Studio 使用 `llama-server`，LiteLLM 为所有服务层提供前端——但归属清晰：**vLLM/SGLang 提供服务，llama.cpp/Ollama 本地运行，LiteLLM 治理，Unsloth 训练和量化。**

## 6. 趋势信号

- **硬件碎片化成为新常态。** ROCm、XPU、NPU、SM120/Blackwell、SYCL、Metal 和 Hexagon 都存在活跃缺口。后端选择不可移植：SM120 在 vLLM/SGLang 中有 FA4/MLA/NVFP4 问题；sm_75 存在 `sgl_kernel` cubin 缺口；SYCL 存在 MoE prompt-processing 回归。按 SKU 锁定已知良好版本。

- **KV/前缀缓存是智能体延迟的主要杠杆。** SGLang 的会话感知 KV 和 HiCache 工作、vLLM 的 KV offload/NVFP4 KV、llama.cpp 的量化 KV 注意力，以及 Ollama 的 KV 泄漏都指向同一方向。智能体开发者现在就应该设计会话 ID 和路由键，以便后续利用缓存局部性。

- **智能体调度正在进入引擎。** vLLM 的 DP prefill delayer 和 PP 均衡、SGLang 的 mixed-chunk 和 AFD 工作，以及撤回/惩罚 bug 表明，多轮智能体流量正在给调度器带来压力。验证 p99 token 间延迟和撤回后确定性，而不仅仅是吞吐量。

- **低位量化正在扩展，但正确性是约束。** NVFP4 KV 损坏、MXFP4 GGUF 元数据溢出、OpenCL q4_0 质量变化以及 EXL3/NF4 提案意味着团队应基准测试质量并验证模型输出，而不仅仅是速度。

- **工具调用和结构化输出在整个技术栈中仍然脆弱。** vLLM 修复了可空 schema 和原始换行符案例；llama.cpp 存在 `response_format` + tools 400s；Ollama 会丢弃 Qwen3 工具轮次；LiteLLM 存在 Claude/Gemini 工具上下文失败；Unsloth 可能将截断的工具调用输出为有效内容。智能体构建者仍然需要验证/修复层。

- **网关成本核算是一项生产风险。** LiteLLM 存在流式支出为 $0、忽略提供商成本、未验证单位导致幽灵支出，以及关闭时丢弃支出缓冲区的问题。实时音频计费也在变化。在执行预算前，对照提供商发票审计流式流量。

- **发布节奏出现分化。** llama.cpp 每天发布多个构建；vLLM/SGLang 在高 PR 变动下运行且无发布；LiteLLM 处于 RC 阶段；Ollama/Unsloth Studio 用户处于 beta 版本。将所有项目视为快速变化，并为生产固定版本。

- **可观测性正在改善，但需要外部验证。** SGLang 新增 `scheduler_iterations_total`，Ollama 暴露推测解码指标，LiteLLM 有支出日志——但上述 bug 表明内部指标可能误报。将原生遥测与提供商侧和客户端侧检查结合起来。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-30

## 今日亮点

过去 24 小时内没有新版本发布，但活动仍然非常活跃（触及 180 个 issue 和 500 个 PR），集中在**多模态计算图捕获**、**ROCm/XPU 后端功能对齐**和**调度器正确性**上。最受关注的条目是 ViT full CUDA Graph RFC（[#38175](https://github.com/vllm-project/vllm/issues/38175)，47 条评论），它针对视觉语言服务中剩余的主机侧启动开销。在正确性方面，同一步优先级驱逐 bug 已有修复 PR（[#59963](https://github.com/vllm-project/vllm/pull/59963)），而若干 SM120/Blackwell 内核实例化缺口仍未解决。

## 发布与破坏性变更

过去 24 小时内没有发布。一项前瞻性兼容性说明：

- **Model Runner V1 移除正在进行中**（[#63373](https://github.com/vllm-project/vllm/pull/63373)，在 [#61995](https://github.com/vllm-project/vllm/pull/61995) 中引用）。MRv2 依赖 Triton，因此非 Triton CPU 目标（Arm、IBM Z、s390x）需要 #61995 中的回退路径，之后才能移除 MRv1。仅 CPU 部署的运维人员应关注这一点。

## 新模型与硬件支持

- **ROCm 上的 MiniMax-M3**：避免为复用 top-k 索引的层分配未使用的 index-key 缓存，修复现有可选启用共享模式的资源注册问题（[#64403](https://github.com/vllm-project/vllm/pull/64403)）。
- **Intel XPU + MooncakeStoreConnector**：移除两个曾破坏 XPU worker 上 KV 传输的 CUDA 假设（`torch.cuda.Event` 注解求值、设备处理）（[#62546](https://github.com/vllm-project/vllm/pull/62546)）。
- **CPU / 非 Triton 后端**：在 Triton 不可用时默认启用 MRv2（[#61995](https://github.com/vllm-project/vllm/pull/61995)）。
- **gfx950 上的 DeepSeek V4.1 MXFP8**：启用 AITER 时，32×32 块缩放线性层从 Triton block-scaled GEMM 迁移到 AITER 的 FlyDSL MXFP8 GEMM（[#60169](https://github.com/vllm-project/vllm/pull/60169)）。
- **ROCm 上的 Qwen3-Next / Qwen3.5**：启用此前仅限 CUDA 的后端无关融合 QK-RMSNorm + RoPE + gate Triton 内核（[#51406](https://github.com/vllm-project/vllm/pull/51406)）。
- **从任意精度进行在线再量化**（RFC 已关闭）：将在线量化 API 扩展到 BF16/FP16/FP32 输入之外，例如 FP8 → MXFP4（[#59062](https://github.com/vllm-project/vllm/issues/59062)）。

## 性能与优化

- **DiffKV prefill 分块（SM80）**：在缺少 FA3/FA4 的 GPU 上，`BLOCK_M=128` 搭配 64 宽 KV tile 在 200K 上下文 prefill 分块上带来 **约 3.0× 加速**；此前的 `BLOCK_M=16` 路径会针对每个查询位置重新加载整个 KV tile（[#61095](https://github.com/vllm-project/vllm/issues/61095)）。
- **ViT full CUDA Graph**（RFC 跟踪项）：捕获整个 ViT 前向，以消除 Qwen3-VL、Qwen3.5、GLM-V、Kimi K2.5 的逐层启动开销（[#38175](https://github.com/vllm-project/vllm/issues/38175)）。
- **ROCm MoE top-k**：用寄存器常驻的 Triton sigmoid+bias top-k 替换 AITER `biased_grouped_topk`，适用于 `topk_group >= num_expert_group` 的退化情形，此时 LDS 分组扫描不会消除任何内容（[#63076](https://github.com/vllm-project/vllm/pull/63076)）。
- **ROCm 上的 GLM-5.3-Flash**：从每个 MLA 层的 kpool 稀疏索引器中移除 int32↔int64 转换、一次 `+1`/`-1` 填充以及一次缓冲区拷贝（[#63910](https://github.com/vllm-project/vllm/pull/63910)）。
- **gfx950 上的 DSv4.1**：AITER FlyDSL MXFP8 GEMM 报告 **总 token 吞吐量 +3.7%**（[#60169](https://github.com/vllm-project/vllm/pull/60169)）。
- **跨 DP prefill 延迟器**（`--dp-prefill-delayer`，可选启用）：防止任一 DP rank 上的 prefill 使每个 rank 的 MoE 步骤变成漫长的混合/eager 步骤，从而减少 agentic 多轮流量下的 decode 中断（[#61834](https://github.com/vllm-project/vllm/pull/61834)）。
- **PP decode 批处理均衡**：将每步已建立的 decode 请求上限设为 `ceil(max_num_seqs / pipeline_parallel_size)`，同时不限制 prefill 调度（[#50410](https://github.com/vllm-project/vllm/pull/50410)）。
- **Triton attention 元数据**：根据每组 `kv_cache_spec` 而非模型级 head 数来确定 3D 内核阈值和 split-K scratch 的大小，修复 Gemma 4 等异构模型上 2D 内核 decode 较慢的问题（[#56996](https://github.com/vllm-project/vllm/pull/56996)）。
- **内核微优化**：在 SM90+ 上为 `silu_and_mul_nvfp4_quant` 启用按大小门控的 PDL；为 level-1 sleep 使用精确大小的页锁定 CPU 备份，以阻止 2 的幂取整泄漏（[#64494](https://github.com/vllm-project/vllm/pull/64494)；[#64630](https://github.com/vllm-project/vllm/pull/64630)）。
- **进行中的跟踪项**：Qwen3.8-Flash-Next MXFP4 的 gfx950/MI355X 优化计划（[#59575](https://github.com/vllm-project/vllm/issues/59575)）；GLM-5.3 CUDA 优化后续（[#57406](https://github.com/vllm-project/vllm/issues/57406)）；非 CUDA 平台上 P/D 分离的 MooncakeConnector 改进（[#60523](https://github.com/vllm-project/vllm/issues/60523)）；ROCm 分离式 P/D + Wide-EP CI（[#48117](https://github.com/vllm-project/vllm/issues/48117)）。

## 稳定性与回归

按严重程度排序：

1. **非法内存访问 + 静默损坏（高）** — sm_86 上 PP=3 的 Qwen3.8-27B（GDN 混合）：启用前缀缓存时出现 Xid 31，不启用时输出乱码。自 v0.28.0 起回归，已在 v0.31.0 和 v0.32.0 上跨两个 model runner 确认。目前尚无修复 PR（[#63505](https://github.com/vllm-project/vllm/issues/63505)）。
2. **KV offload 不会快速失败（高）** — P2P KV offload 层级在对端故障时继续运行而非暴露故障，可能导致 NIXL/InfiniBand 配置下的传输停滞或错误（[#63602](https://github.com/vllm-project/vllm/issues/63602)）。
3. **SM120 稀疏 MLA 内核缺口（高）** — DeepSeek-V4.1-Flash 无法在 RTX PRO 6000（SM120）上运行：FlashInfer 仅为 `page_block_size=64` 实例化稀疏 MLA decode 内核，但压缩层使用 32（[#59203](https://github.com/vllm-project/vllm/issues/59203)）。
4. **NVFP4 KV scale 损坏（高）** — SM100 trtllm-gen swizzle 被无条件写入，静默损坏 RTX 5090 上的 SM120 FlashInfer XQA 路径（[#50084](https://github.com/vllm-project/vllm/issues/50084)）。
5. **调度器优先级反转（中高）** — 优先级调度可能取消调度同一 step 中已调度的请求（encoder 缓存未命中、前缀命中尚未写入的 KV）。**修复 PR 已开放**：[#59963](https://github.com/vllm-project/vllm/pull/59963) 对应 [#59907](https://github.com/vllm-project/vllm/issues/59907)。
6. **流水线并行下的 GLM-5.x DSA（中高）** — stage 起始处复用的层读取过期的 top-k 索引；与硬件无关，影响任何 `--pipeline-parallel-size > 1`（[#63170](https://github.com/vllm-project/vllm/issues/63170)）。
7. **投机解码初始化崩溃（中）** — Gemma4 MTP drafter 在 0.25.1 上引擎初始化时失败，报错 “a and b must have same reduction dim”；在 aarch64/GB10 上自 0.21.0 起回归（[#48848](https://github.com/vllm-project/vllm/issues/48848)）。
8. **FP8 混合 GDN + DFlash（中）** — Qwen3.5-122B-A10B-FP8 在 nightly/0.25 上于 profile_run 期间失败，报 `CUBLAS_STATUS_EXECUTION_FAILED`；0.24.0 正常（[#48477](https://github.com/vllm-project/vllm/issues/48477)）。
9. **torch.compile 步幅不匹配（中）** — Qwen3.5/Qwen3-Next GDN attention 在 warmup 时于 `splitting_ops` 边界崩溃；#29014 复发（[#50046](https://github.com/vllm-project/vllm/issues/50046)）。
10. **ROCm gfx1201 AITER 回归（中）** — v0.25.0 中的 unified-attention 内核超出 LDS 限制（[#48723](https://github.com/vllm-project/vllm/issues/48723)）。
11. **token 边界上的前缀缓存未命中（低中）** — 当 prompt 长度恰好为 1536 token 边界时，Kimi-K3 无法复用缓存（[#50235](https://github.com/vllm-project/vllm/issues/50235)）。
12. **投机解码 TP 分歧（低中）** — 自适应 DSpark 验证决策未在 TP rank 间同步；**修复 PR 已开放**（[#62769](https://github.com/vllm-project/vllm/pull/62769)）。
13. **KV 容量计算顺序（低）** — batch-sharded sampling 在 KV 缓存已消耗内存预算*之后*才 profile `all_to_all_single`；**修复 PR 已开放**（[#61025](https://github.com/vllm-project/vllm/pull/61025)）。

今日关闭/解决：多模态接收端缓存未命中（[#26195](https://github.com/vllm-project/vllm/issues/26195)）、结构化输出 + 投机解码分布保持 RFC（[#62818](https://github.com/vllm-project/vllm/issues/62818)）、sm_120 上 Qwen3.6-35B-A3B 跨序列 logprob 漂移（[#59764](https://github.com/vllm-project/vllm/issues/59764)）、稀疏 MLA 索引器 1 GiB 缓冲区错误分配（[#60450](https://github.com/vllm-project/vllm/issues/60450)）。

## 这对应用开发者意味着什么

- **工具调用对可为空的 schema 正变得更稳健。** Muse、MiniCPM5 和 Poolside V1 解析器此前仅对精确的 `{"type": "string"}` schema 保留原始参数文本，导致 `str | None` 参数损坏（[#64539](https://github.com/vllm-project/vllm/pull/64539)）；Pythonic/Llama4 解析器现在会保留字符串参数中的原始换行符，包括流式和并行调用（[#44102](https://github.com/vllm-project/vllm/pull/44102)）。如果你依赖可选字符串工具参数，请关注 #64539。
- **Agentic 工作负载获得一个调度手段。** `--dp-prefill-delayer` 直接针对多轮模式：频繁 prefill 会跨数据并行 rank 导致 decode 饥饿（[#61834](https://github.com/vllm-project/vllm/pull/61834)）。如果你的 p99 inter-token 延迟在突发 agent 流量下变差，值得做基准测试。
- **在 Ampere 级 GPU 上，长上下文 prefill 的成本显著降低**（根据 [#61095](https://github.com/vllm-project/vllm/issues/61095)，在 200K token 分块上约 3×）——如果你在 SM80 而非 Hopper/Blackwell 上运行 128K–200K 上下文，这一点值得关注。
- **Blackwell / SM120 仍是风险点。** DeepSeek-V4.1-Flash 稀疏 MLA 和 NVFP4 KV 缓存都存在未解决的 SM120 内核缺口（[#59203](https://github.com/vllm-project/vllm/issues/59203)、[#50084](https://github.com/vllm-project/vllm/issues/50084)）。对于这些 model+quant 组合，在 RTX PRO 6000 / RTX 5090 上请固定已知良好版本，而不是跟踪 nightly。
- **非 CUDA 平台上的 P/D 分离正在加固**（XPU Mooncake 修复 [#62546](https://github.com/vllm-project/vllm/pull/62546)，MooncakeConnector RFC [#60523](https://github.com/vllm-project/vllm/issues/60523)）。如果你正在 ROCm/XPU 上设计分离式部署，预计 connector 接口面会变化。
- **睡眠模式内存现在可回收。** level-1 sleep 此前会在唤醒后无限期保留按 2 的幂取整的页锁定 CPU 缓冲区；修复后默认释放它们，并通过 `VLLM_SLEEP_RETAIN_CPU_BACKUP=1` 选择保留（[#64630](https://github.com/vllm-project/vllm/pull/64630)）。这关系到热切换引擎的多模型主机。
- **基准测试客户端获得更严格的 SSE 分帧** — `StreamedResponseHandler` 中修复了分隔符拆分和 CRLF 处理（[#61534](https://github.com/vllm-project/vllm/pull/61534)）。如果你维护自定义负载生成器，请同步这一改动以避免 token 计数错误。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-30

来源：[sgl-project/sglang](https://github.com/sgl-project/sglang) · 过去 24 小时无新版本发布 · 35 个 issue 有更新（展示 30 个），500 个 PR 有更新（展示前 20 个）

---

## 1. 今日亮点

CI 不稳定仍在占据主要关注：flaky 测试追踪 issue [#42752](https://github.com/sgl-project/sglang/issues/42752) 目前已有 140 条评论，成为仓库中最活跃的讨论串。正确性方面，DeepSeek-V4.1-Flash 多模态服务中的高严重度调度器崩溃（[#47218](https://github.com/sgl-project/sglang/issues/47218)）以及 DFLASH draft 更新中的静默权重损坏路径（[#46806](https://github.com/sgl-project/sglang/issues/46806)）是影响最大的报告。架构工作通过关于模型无关并行（[#47170](https://github.com/sgl-project/sglang/issues/47170)）和会话感知 KV 编排（[#45189](https://github.com/sgl-project/sglang/issues/45189)）的 RFC 继续推进，此外 PR 队列中还有一轮广泛的 NPU/AMD 支持推进。

---

## 2. 发布与破坏性变更

过去 24 小时无新版本或 tag。有两点运维注意事项值得标记：

- 在 sglang 0.5.22 上，**Python 3.13 安装现在需要 Rust 工具链**，原因是 `outlines==0.1.11 → outlines_core==0.1.26` 的精确 pin。该要求未写入文档，很可能破坏封闭/气隙（air-gapped）构建 — [#47242](https://github.com/sgl-project/sglang/issues/47242)。
- **`/start_profile` 搭配 `profile_by_stage=true` 且未提供 `num_steps`** 会抛出 `TypeError: NoneType += int`，并导致 v0.5.22 上的每个 scheduler 崩溃 — [#47294](https://github.com/sgl-project/sglang/issues/47294)。

---

## 3. 新模型与硬件支持

**路线图 / 上游合入计划**
- **T-Head PPU（Zhenwu 810 / 810E / M890P）** 一等公民支持路线图，已标记为最高优先级 — [#37519](https://github.com/sgl-project/sglang/issues/37519)
- **AMD RDNA3/RDNA4（gfx1100 / gfx1201）** 面向 main 分支的 CI 与上游合入计划 — [#46900](https://github.com/sgl-project/sglang/issues/46900)
- **AMD Triton Language Extension（TLX）** 在 gfx950 / gfx1250 上的启用 — [#40624](https://github.com/sgl-project/sglang/issues/40624)

**Ascend / NPU**
- Ascend 上 DeepSeek-V4 的 DSpark compact 模式，用 SPS 策略驱动的验证替代统一的 verify 窗口 — [PR #38440](https://github.com/sgl-project/sglang/pull/38440)
- Ascend 上的动态专家负载均衡（`--enable-eplb`），处理 NZ 专家布局以及 NPU DeepEP 中的有效发送方路由计数 — [PR #47266](https://github.com/sgl-project/sglang/pull/47266)
- Ascend DSA 稀疏注意力路径支持 `--enable-mixed-chunk`（此前 GLM-5.2 级 DSA 模型会报 `NotImplementedError`）— [PR #47367](https://github.com/sgl-project/sglang/pull/47367)

**Intel XPU**
- 完整的 prefill CUDA graph 捕获/重放，与 decode graph 契约保持一致 — [PR #30980](https://github.com/sgl-project/sglang/pull/30980)
- 面向 Xe2 W8A16 grouped GEMM，将 FP8 MoE dispatch 路由到 `sgl_kernel.fused_experts` — [PR #43994](https://github.com/sgl-project/sglang/pull/43994)

**模型**
- `sensenova_loop` 原生文本/图像/视频推理 — 派生自 Qwen3.5，采用共享权重层重复与多流超连接（mHC） — [PR #47281](https://github.com/sgl-project/sglang/pull/47281)
- DFlash 系列 MoE FFN draft 从 HF MoE 配置字段（`experts.gate_up_proj` / `experts.down_proj`）提供 — [PR #47181](https://github.com/sgl-project/sglang/pull/47181)

**硬件注意事项：** 在 SM120（RTX PRO 6000）上，GLM-5.3-Flash *没有*可用的 FA4 路径 — 它会在 hybrid extend reshape 的 CUDA graph 捕获阶段崩溃；Triton 是唯一可用的后端（[#42012](https://github.com/sgl-project/sglang/issues/42012)）。另外，`sgl_kernel` 的 AOT wheel 在 sm_75 上存在逐算子 cubin 缺口（`rmsnorm` 会以 `cudaErrorNoKernelImageForDevice` 失败，且没有软回退）— [#38515](https://github.com/sgl-project/sglang/issues/38515)。

---

## 4. 性能与优化

**带具体数字的进行中 PR**
- **AMD diffusion prefill**：面向 gfx1151 的新 Triton dense-attention prefill 路径。Strided SDPA 视图会让 AOTriton 付出很高代价 — `attn_fwd` 在 4K / 8K / 16K token 下分别为 12.4 / 6.7 / 2.06 TF，而使用 dense 操作数时为 12.4 → 18 TF — [PR #46515](https://github.com/sgl-project/sglang/pull/46515)；相关的长序列 materialization 修复 — [PR #46065](https://github.com/sgl-project/sglang/pull/46065)
- **AMD DSA prefill**：在 EXTEND 期间跨 transformer 层复用 AITER work/reduction 元数据（该元数据依赖序列，而非依赖层）— [PR #39085](https://github.com/sgl-project/sglang/pull/39085)
- **NPU MiMo-V2.5-Pro**：将 `attention_value_scale` 折叠进 `o_proj` 权重，从而在每个 decode/verify 步骤中消除每层一个 `aclnnMuls` kernel（约 2 ms/层）— [PR #40890](https://github.com/sgl-project/sglang/pull/40890)
- **Rust 前端**：仅对携带 egress logprob 的行展平 egress logprob 列，从而移除 scheduler 线程上每步持有 GIL 的工作 — [PR #47106](https://github.com/sgl-project/sglang/pull/47106)
- **Detokenizer**：对每个请求中不变的 decode 输入只发送一次，而不是每步都发送（decode-id delta 会重复 `output_ids`、offsets、`skip_special_tokens`、tokenizer route）— [PR #47260](https://github.com/sgl-project/sglang/pull/47260)
- **内存**：在为 non-DSA EAGLE/MTP draft KV 池定容时计入 DCP 复制（`loc_space_scale = dcp_size` 的超额分配）— [PR #39799](https://github.com/sgl-project/sglang/pull/39799)
- **可观测性**：新增 `scheduler_iterations_total` 指标 — [PR #46485](https://github.com/sgl-project/sglang/pull/46485)

**追踪项 / 回归**
- gfx950 / MI355X 上的 Qwen3.8-Flash-Next 优化追踪（GatedDeltaNet ×36 层，QSA ×12，MXFP4 MoE，原生 NEXTN MTP）— [#44304](https://github.com/sgl-project/sglang/issues/44304)
- 在 pip 安装环境中，FlashInfer `cascade` 会在第一个带前缀请求内进行 JIT 编译（约 4.5 秒停顿）— [#46803](https://github.com/sgl-project/sglang/issues/46803)
- hybrid GDN 上的 HiCache：一旦被驱逐的工作集超过约 3 个长前缀，host 层命中率就降为零（host mamba 池耗尽，而 KV 仍驻留 host）— [#45400](https://github.com/sgl-project/sglang/issues/45400)
- Attention-FFN 解耦（AFD）设计与分阶段集成计划 — [#45568](https://github.com/sgl-project/sglang/issues/45568)

---

## 5. 稳定性与回归

按严重程度排序：

1. **调度器崩溃，整个服务中断** — DeepSeek-V4.1-Flash 多模态路径在并发中止流下解引用 `items_offset_list = None`；导致所有 TP/EP worker 宕机并重启容器。未引用修复 PR。 — [#47218](https://github.com/sgl-project/sglang/issues/47218)
2. **RL / 权重同步工作流中的静默权重损坏** — DFLASH 上的 `update_weights_from_disk` 会把目标权重加载进 draft 模型；该请求没有 `selector` 来阻止这一行为。此前 EAGLE3 外部 draft 也报告过相同症状（THUDM/slime#1177）。 — [#46806](https://github.com/sgl-project/sglang/issues/46806)
3. **撤回时的静默输出变化** — 一个被撤回并重新 prefill 的请求会把 `frequency_penalty` / `presence_penalty` / `repetition_penalty` 累加器和 `min_new_tokens` 计数器重置为零。针对 PD rebootstrap token-pop 路径的相关修复正在进行：[PR #47366](https://github.com/sgl-project/sglang/pull/47366)。 — [#47245](https://github.com/sgl-project/sglang/issues/47245)
4. **SM120 注意力后端故障** — 对于 GLM-5.3-Flash 的 hybrid extend reshape，FA4 会在 CUDA graph 捕获时崩溃；在 8× RTX PRO 6000 上 Triton 是唯一可用后端。 — [#42012](https://github.com/sgl-project/sglang/issues/42012)
5. **KV scale 加载器类型损坏** — `LlamaModel.load_kv_cache_scales` 将 Python float 赋给 `k_scale`/`v_scale`，却未更新 `k_scale_float`/`v_scale_float`，同时破坏 registered-Parameter 和使用 tensor 的注意力路径；host 镜像会过期。 — [#47357](https://github.com/sgl-project/sglang/issues/47357)
6. **/unload_lora_adapter 永久死锁** 在并行采样流量（n > 1）后出现 — 可确定性复现；已作为不活跃关闭，无修复落地。 — [#36894](https://github.com/sgl-project/sglang/issues/36894)
7. **Profiler 崩溃** — 在 v0.5.22 上，`/start_profile` 在 `profile_by_stage=true` 且未提供 `num_steps` 时会使所有 scheduler 崩溃。 — [#47294](https://github.com/sgl-project/sglang/issues/47294)
8. **EAGLE 投机解码挂起** — 在 Ampere（2× A2，TP=2，低功耗下 GPU 利用率 100%）上，原生 Qwen3.5/3.8 MTP 卡在 `tree_speculative_sampling_target_only`。 — [#35822](https://github.com/sgl-project/sglang/issues/35822)
9. **推理语法门始终不会打开**：当 think-end 标记在 `split_special_tokens` 下被拆分时会出现。 — [#47205](https://github.com/sgl-project/sglang/issues/47205)
10. **Diffusion 模型崩溃** — LingBot-Video-MoE 文本编码在默认 layerwise offload 下失败（tied vocab 表留在 CPU → 设备不匹配）。 — [#47297](https://github.com/sgl-project/sglang/issues/47297)
11. **CI 健康度** — flaky 测试以及反复出现的 NVIDIA `PR Test Base` / `PR Test Extra` 基础设施故障，通过 `/sglang-pr-babysit` 持续追踪。 — [#42752](https://github.com/sgl-project/sglang/issues/42752)

---

## 6. 对应用开发者的意义

- **请在你的拓扑上对 0.5.22 升级设门禁。** `/start_profile` 崩溃和 Python 3.13 的 Rust 工具链要求在 CI 中很容易触发。如果你以多模态输入和中止流流量来服务 DeepSeek-V4.1-Flash，请把 #47218 视为会阻塞生产的风险 — 其故障模式是整个引擎重启，而不是单个请求错误。
- **权重同步流水线需要审计。** 如果你针对 DFLASH/EAGLE draft 模型做 RL 或在线微调，请确认没有意外把目标权重推入 draft。当前没有基于 `selector` 的防护，因此必须在编排层强制隔离。
- **不要信任跨撤回场景的投机解码确定性。** 被撤回并重新 prefill 的请求会静默丢失惩罚历史和 `min_new_tokens` 下限。对于依赖 `repetition_penalty` 或硬性输出长度约束的 agent，请记录并校验撤回后的输出；关注 [PR #47366](https://github.com/sgl-project/sglang/pull/47366) 中相关的 token 重放修复。
- **后端选择不可移植。** 在 SM120 上，GLM-5.3-Flash 必须强制使用 Triton；在 sm_75（Turing）上，特定 `sgl_kernel` 算子会硬报 `cudaErrorNoKernelImageForDevice` 失败，而不会优雅回退到 torch。请在部署 manifest 中按 GPU SKU 固定已知可用的后端 flag。
- **缓存局部性正成为首要的延迟杠杆。** Q4 agentic 缓存调度路线图（[#43098](https://github.com/sgl-project/sglang/issues/43098)）、会话感知 KV 编排（[#45189](https://github.com/sgl-project/sglang/issues/45189)）以及 HiCache 修复都指向同一方向：多轮 agent 工作负载将越来越依赖把请求路由到持有其前缀 KV 的副本。现在就开始设计 session ID 和路由键，以便日后利用这一点。
- **面向容量规划的闭环仿真即将到来** — 模拟器 RFC（[#44221](https://github.com/sgl-project/sglang/issues/44221)）面向能感知工具等待的工作负载，这比固定请求轨迹更适合 agent 流量。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 摘要 — 2026-10-30

## 1. 今日亮点

今天是后端优化密集的一天：Metal 获得重写的 MXFP4 mat-vec 内核，SYCL 落地了面向量化 KV 缓存的 DPAS flash-attention 路径，Hexagon 则新增 UPSCALE 与转置 CONCAT 优化——这些均已随构建 b11978–b11998 发布。模型覆盖范围扩展至 Mistral “Le Chonk”（DSA/sdpark）和 Limite 1B “Violetto” 架构，同时 OpenCL 添加了初步的 IQ2 量化支持。主要警示是一组未解决的 SYCL MoE prompt-processing 回归（约慢 28–35%），以及共批处理的混合 LFM2 序列存在静默正确性问题。

## 2. 发布与破坏性变更

- **b11998** — 新模型：Mistral “Le Chonk”，将 DSA 与 sdpark 支持扩展到该架构。[PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376) · [发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11998)
- **b11997** — 复用张量暂存缓冲区，并跳过 direct-io 回弹缓冲区。使用 `--direct-io` / 禁用 mmap 加载的运维人员应重新检查峰值 RSS 和加载延迟；无需更改配置。[PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321) · [发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11997)
- **b11988** — gguf-py 中的 Limite 架构 + 转换器支持（Limite 1B “Violetto”）。[PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433) · [发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11988)
- **b11986** — OpenCL：初步支持 IQ2。[PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267) · [发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11986)
- **b11985** — Metal：更快的 MXFP4 mat-vec + 针对 unary/binary/GLU 的完整线程组。[PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075) · [发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11985)
- **b11982 / b11981** — Hexagon：UPSCALE 算子以及转置 CONCAT 的 DDR 带宽优化工作。[PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331)、[PR #31277](https://github.com/ggml-org/llama.cpp/pull/31277)
- **b11980 / b11979** — SYCL：面向量化 KV 缓存的 DPAS flash attention；向量化重排布局的反量化存储。[PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125)、[PR #30976](https://github.com/ggml-org/llama.cpp/pull/30976)
- **b11978** — 测试修复：`comp_padding` CUDA graph 预留。[PR #31344](https://github.com/ggml-org/llama.cpp/pull/31344)

本时间窗口内没有公共 API 或 CLI 破坏性变更。待处理的非破坏性供应商版本升级：cpp-httplib 0.64.0（[PR #31400](https://github.com/ggml-org/llama.cpp/pull/31400)）。

## 3. 新模型与硬件支持

- **Mistral “Le Chonk”** — 新架构已接入 DSA/sdpark 路径（[PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376)）。
- **Limite 1B “Violetto”** — 新增 `Limite` GGUF 架构及转换器支持，使第三方量化流水线可以面向该架构（[PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433)）。
- **OpenCL IQ2（`iq2_s`）** — 面向 Adreno 调优的 AOS/SOA mat-vec、分块 mat-mul、no-shuffle GEMM/GEMV 与 dp4a 路径（[PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267)）。
- **SYCL 量化 KV flash attention** — 对于 decode 和小批量（1–31 个 query token），q4_0 与 q8_0 KV 块现在通过 DPAS 直接执行 attention，而不再进行标量反量化（[PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125)）。
- **Hexagon** — UPSCALE 算子，支持优化的最近邻 2x/4x 和双线性 4x、非连续行与维度折叠处理（[PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331)）；DSV4_HC_PRE/POST 算子仍在开发中（[PR #30953](https://github.com/ggml-org/llama.cpp/pull/30953)）。
- **Metal** — 提出 `GET_ROWS_BACK`（F32）内核（[PR #31378](https://github.com/ggml-org/llama.cpp/pull/31378)）。
- **多模态修复** — 修正 InternVL CLS token 排序（此前 patch 0 会收到 CLS 位置嵌入）（[PR #30886](https://github.com/ggml-org/llama.cpp/pull/30886)）；Gemma 4 图像缩放切换为 `PAD_NONE`，以匹配 HF 预处理（[PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)）。
- **Metal 构建兼容性** — 由 `unary.metal` 中 `constexpr bfloat` 类型转换导致的 macOS 14 / M2 Ultra 初始化失败已关闭（[#31272](https://github.com/ggml-org/llama.cpp/issues/31272)）。

## 4. 性能与优化

- **Metal MXFP4 mat-vec 重写** — fp4 值改用 fp16 位技巧解码，而不再使用线程组查找表；打包的 nibble 以对齐的 32 位字加载；内核不再为 LUT 使用线程组暂存区。Unary/binary/GLU 内核已迁移至完整线程组（[PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)）。
- **面向量化 KV 的 SYCL DPAS FA** — 在 decode 中移除逐 token 的标量反量化/转换，这是短 batch 下 q4_0/q8_0 KV 缓存的主要开销（[PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125)）。
- **SYCL 反量化存储加宽** — 重排布局的 Q4_0 此前每个 `qs` 字节执行两次标量存储，跨步为 16 个元素；现在已向量化。K-quants、IQ3 与 MXFP4 也做了同样处理（[PR #30976](https://github.com/ggml-org/llama.cpp/pull/30976)）。
- **Hexagon 转置 CONCAT** — 将行块大小翻倍，并采用 scratchpad 双缓冲、分块 `src1` 行、填充 VTCM 跨步以分散 gather lane，目标是提升 DDR 带宽利用率（[PR #31277](https://github.com/ggml-org/llama.cpp/pull/31277)）。
- **加载路径** — 复用暂存缓冲区，并消除 direct-io 回弹缓冲区，从而降低模型加载期间的拷贝开销和瞬时内存（[PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321)）。
- **使用 q8_0 KV 的 CPU 分块 FA（提议中）** — 目前只有 f32/f16 KV 会进入分块内核；`-ctk q8_0 -ctv q8_0` 会回退到参考循环，该循环会针对每个（q 行、kv token）对 V 反量化一次（[PR #31399](https://github.com/ggml-org/llama.cpp/pull/31399)）。
- **x86 vec_dot 累加器展开** — 针对 Q4_0/Q4_K/Q5_K/IQ3_S/IQ3_XXS 的量化点积链，目标是降低 token 生成延迟（[PR #31061](https://github.com/ggml-org/llama.cpp/pull/31061)，已关闭）。
- **CUDA Hadamard 旋转融合** — 对于 prism.hadamard 模型，将符号乘法 + Hadamard 变换合并为一个内核，置于量化到 8 位 mat-vec 之前（[PR #31398](https://github.com/ggml-org/llama.cpp/pull/31398)）。
- **OpenCL dp4a q4_0 GEMM 成为默认** — f16 GEMM 会将激活值舍入到半精度，在 gemma-4-E2B-Q4_0 上相比 CPU 造成约 3% 的困惑度损失；在具备整数点积的地方会使用 q8_1 dp4a 路径（[PR #30535](https://github.com/ggml-org/llama.cpp/pull/30535)）。Small-N f32/f16 GEMV 现在每行权重只读取一次，而不再填充到 64 宽的分块（[PR #30413](https://github.com/ggml-org/llama.cpp/pull/30413)）。
- **MoE 缓存** — 提出 `--moe-cache-cpu-share auto`，并基于 CRF 对 CPU 未命中进行准入控制（[PR #31286](https://github.com/ggml-org/llama.cpp/pull/31286)）。

## 5. 稳定性与回归

按严重程度排序：

1. **[OPEN] 静默正确性问题：混合 LFM2/LFM2.5 共批处理** — 在同一个 ubatch 中解码多个序列会破坏 recurrent/short-conv 状态；贪心输出会发散，并可能在句子中途截断。对于在混合模型上使用 `-np > 1` 的服务器部署，这是风险最高的项。目前尚无修复 PR。[#29002](https://github.com/ggml-org/llama.cpp/issues/29002)
2. **[OPEN] SYCL MoE prompt-processing 回归，约 28–35%** — 在 b11804 中，使用 `-ncmoe` 将专家卸载到 CPU 后，`llama-server` 在 `qwen35moe` 上的 prompt 处理比 b11046 慢约 35%，且 `llama-bench` 无法复现；第二份报告将 Arc Pro B70 上的 XMX-ESIMD 路径回归定位到 `d81d0d8fb`。不经过服务器路径的基准测试会漏掉这个问题。[#31262](https://github.com/ggml-org/llama.cpp/issues/31262) · [#31068](https://github.com/ggml-org/llama.cpp/issues/31068)
3. **[OPEN] DeepSeek-V4.1-Flash-1017 GGUF 问题** — 针对 build 11997 在 CUDA 上、双 EPYC + RTX 6000 Pro 环境报告。[#31385](https://github.com/ggml-org/llama.cpp/issues/31385)
4. **[OPEN] `qwen4exp` 在 HIP 上模型加载期间发生 livelock** — 相同配置在 Vulkan 上可正常加载。[#29149](https://github.com/ggml-org/llama.cpp/issues/29149)
5. **[OPEN] gfx1200 上间歇性 `SOLVE_TRI` 内存故障** — HIP/ROCm，在 `test-backend-ops` 下可复现。[#31108](https://github.com/ggml-org/llama.cpp/issues/31108)
6. **[OPEN] SYCL 量化 CPY 与 CPU 不一致** — 向 q4_1/q5_1/q8_0 执行 `CPY` 时与 CPU 参考实现出现偏差。[#31140](https://github.com/ggml-org/llama.cpp/issues/31140)
7. **[OPEN] SYCL `test-backend-ops` 失败** — CONV_2D、CPY、ROLL、FLASH_ATTN_EXT。FA 分支有一个候选修复：`fattn-mkl.cpp` / `fattn-onednn.cpp` 中的 gemma-interleave 启发式与量化 K/V 暂存块。[#28384](https://github.com/ggml-org/llama.cpp/issues/28384) · [PR #30761](https://github.com/ggml-org/llama.cpp/pull/30761)
8. **[OPEN] Qwen3.5+ 模板上的前缀缓存失效** — reasoning/content 中的空白字符无法往返保持一致，导致混合模型上完整重新 prefill。这会直接影响 agent 工作负载的成本。[#30357](https://github.com/ggml-org/llama.cpp/issues/30357)
9. **[OPEN] `json-schema-to-grammar` 输出原始控制字符** — 例如 `pattern` 内的 `"\u0000"` 会产生 NUL，从而导致 “failed to parse grammar”。[#31199](https://github.com/ggml-org/llama.cpp/issues/31199)
10. **[OPEN] `response_format` + tools → 400** — 设置 `response_format` 时，coder parser 只构建 JSON-schema 规则，因此 tool-call 分支不可达。提议的修复：当设置 `response_format` 时不要使用 lazy grammar（[PR #31395](https://github.com/ggml-org/llama.cpp/pull/31395)）。[#31342](https://github.com/ggml-org/llama.cpp/issues/31342)
11. **[CLOSED] LFM3 `response_format` 500** — 在使用 `--reasoning-budget 0` + `enable_thinking: false` 时出现 “Unexpected empty grammar stack”；通过在遇到 prompt 中的结束标签时结束 reasoning-budget 强制来修复（[PR #31374](https://github.com/ggml-org/llama.cpp/pull/31374)）。[#31328](https://github.com/ggml-org/llama.cpp/issues/31328)
12. **[CLOSED] OpenCL FA softcap 顺序** — mask 在 logit softcap 之前应用，因此被 mask 的条目会被截到 `-softcap`，并且仍参与 softmax（[PR #30536](https://github.com/ggml-org/llama.cpp/pull/30536)）。同样已关闭：未对齐的 MoE router ID 子缓冲区中止，报 `CL_MISALIGNED_SUB_BUFFER_OFFSET`（[PR #30537](https://github.com/ggml-org/llama.cpp/pull/30537)）。
13. **[OPEN] `json-schema-to-grammar` 在存在 pattern 时丢弃长度边界** — 当 `pattern` 存在时，`minLength`/`maxLength` 被静默忽略（[PR #31358](https://github.com/ggml-org/llama.cpp/pull/31358)）。
14. **[OPEN] CUDA arm64 Blackwell 吞吐量回归** — 在官方构建 b10655 与 b11140 之间，prompt 处理和生成均受影响。[#29341](https://github.com/ggml-org/llama.cpp/issues/29341)
15. **[OPEN] gemma3n/gemma4 重复分配 `token_embd`** — 在输入/输出嵌入绑定（tied）且不使用 mmap 加载（`-lm none`、`-lm mlock`）时，会分配第二份副本；提议的修复是在共享输出之前创建 `token_embd`（[PR #31394](https://github.com/ggml-org/llama.cpp/pull/31394)）。
16. **[CLOSED] RPC 单线程 accept 循环** 在第一个连接之后导致所有连接饥饿 — 这对多主机 router 部署有参考意义。[#28908](https://github.com/ggml-org/llama.cpp/issues/28908)

## 6. 这对应用开发者意味着什么

- **暂时不要在生产环境中使用 `-np > 1` 运行混合模型。** #29002 是静默数据损坏，而不是崩溃——你的评估在单序列运行中可能通过，但在多租户服务器中会失败。在问题解决前，将 LFM2/LFM2.5 的并行度上限设为 1。
- **语法约束解码存在两个仍未解决的 bug。** 如果你依赖 `response_format: json_schema` 并结合工具调用，请预期某些 chat 模板会返回 400；#31395 是值得关注的 PR。`pattern` 中包含转义控制字符的 schema 可能完全无法构建 grammar（#31199）。请针对当前构建验证你的 schema 语料库，而不是假设此前行为仍然成立。
- **在 SYCL 上，请通过 `llama-server` 而不是 `llama-bench` 做基准测试。** 约 35% 的 MoE prompt-processing 回归（#31262）仅出现在服务器路径，标准 bench harness 看不到。如果你固定使用 Intel 构建，请停留在 b11046，或二分排查越过 `d81d0d8fb`。
- **量化 KV 缓存在 SYCL 上刚获得实质性提升。** DPAS FA 内核（#31125）使 `-ctk q8_0 -ctv q8_0` 在 Intel GPU 上可用于 decode，这是提升每 GPU 上下文长度的最大单一杠杆。在 CPU 上，q8_0 KV 仍会回退到缓慢的参考循环，直到 #31399 合入。
- **Metal/MXFP4 与 OpenCL/IQ2 分别拓宽了 Apple Silicon 和 Adreno 上的低比特部署边界**，但 OpenCL q4_0 现在出于质量原因默认使用 dp4a（#30535）——如果你此前根据 f16 激活行为做过校准，请重新建立困惑度基线。
- **前缀缓存的经济性在 Qwen3.5+ 上仍然脆弱**（#30357）。对于带有长系统提示的 agent 循环，请测量实际的重新 prefill 率，而不是假设缓存命中；模型输出的空白字符可能使你的整个前缀失效。
- **运维需求在不断累积，但尚未交付：** 模型缓存列出/删除（[#16393](https://github.com/ggml-org/llama.cpp/issues/16393)）、独立的 `-ns`/`-np` 控制（[#22921](https://github.com/ggml-org/llama.cpp/issues/22921)）、空闲 router worker 终止（[#25570](https://github.com/ggml-org/llama.cpp/issues/25570)）以及模型 ADMIN API（[#30807](https://github.com/ggml-org/llama.cpp/issues/30807)）都仍处于开放状态——请按它们尚不可用来做规划，而不是指望它们。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-30

## 今日亮点
过去 24 小时内没有新的 Ollama 版本发布；活动集中在 MLX 后端和 OpenAI 兼容性正确性上。MLX 工作推进了投机解码遥测、并发生成、加载停滞诊断和重复循环中止，而若干围绕 reasoning/content 数组与空响应的 OpenAI 兼容性 bug 收到了修复 PR。最严重的未决项仍是 Strix Halo 上的 ROCm gfx1151 KV 状态串扰，它会污染连续响应。

## 版本发布与破坏性变更
无。该时段内未发布版本，也没有已落地的 API/配置迁移。

## 新增模型与硬件支持
- 本窗口内没有新增架构或后端落地。活跃/已关闭的模型支持请求：
  - 通过 `/api/chat` 和 `ollama run --audio` 流式返回 LFM3-Omni 的原生音频回答 — [ollama/ollama#19396](https://github.com/ollama/ollama/issues/19396)（开放）
  - 在 Windows 上为 `lfm3-omni` 和 `phonon` 等模型提供原生音频/视频输入与音频输出 — [ollama/ollama#19390](https://github.com/ollama/ollama/issues/19390)（开放）
  - 对声明具备 `audio` 能力的模型，`/api/chat` 会静默忽略 `audio` 字段；音频目前只能通过 `images` 使用 — [ollama/ollama#19378](https://github.com/ollama/ollama/issues/19378)（开放）
  - 相关 PR：拒绝模型没有对应编码器的片段，而不是将其传给 projector — [ollama/ollama#19397](https://github.com/ollama/ollama/pull/19397)（开放）
  - 针对 Gemma 4 等模型的多模态嵌入媒体排序修复 — [ollama/ollama#19334](https://github.com/ollama/ollama/pull/19334)（开放）
  - Bonsai 2 的三元量化 — [ollama/ollama#19388](https://github.com/ollama/ollama/issues/19388)（已关闭）；phonon 的韩语 TTS 支持 — [ollama/ollama#19295](https://github.com/ollama/ollama/issues/19295)（已关闭）
- GGUF 加载器：`gpt-oss:120b`/`latest` 在 `ffn_down_exps.weight` 上的 MXFP4 大 MoE 元数据溢出 — [ollama/ollama#19375](https://github.com/ollama/ollama/issues/19375) 和 [ollama/ollama#18869](https://github.com/ollama/ollama/issues/18869)（均已关闭）
- 特定硬件报告：ROCm Strix Halo gfx1151 KV 串扰 — [ollama/ollama#17847](https://github.com/ollama/ollama/issues/17847)（开放）；Windows CUDA RTX 5070 Ti 预热失败 — [ollama/ollama#17380](https://github.com/ollama/ollama/issues/17380)（开放）

## 性能与优化
- 投机解码：为草稿/接受 token 数和接受率提供按请求指标，并在 API 响应和 `ollama run --verbose` 中暴露 — [ollama/ollama#19368](https://github.com/ollama/ollama/pull/19368)（开放）。MLX 增加 `draft_num_predict=0` 以在基准比较中禁用 drafting；默认仍为 4 — [ollama/ollama#19391](https://github.com/ollama/ollama/pull/19391)（开放）。
- MLX top-k：对 2048 个值进行分块排序，避免对大于 2048 的行使用多轮 Metal 归并排序，目标是 248K token 词表上的 top-k — [ollama/ollama#19392](https://github.com/ollama/ollama/pull/19392)（已关闭）。
- MLX 并发：为 MLX 生成支持 `OLLAMA_NUM_PARALLEL`，共享 projection/feed-forward 工作，同时按请求保留 attention — [ollama/ollama#19197](https://github.com/ollama/ollama/pull/19197)（开放）。
- MLX 加载诊断：恢复惰性权重加载的基于进度的停滞检测和加载进度报告 — [ollama/ollama#17834](https://github.com/ollama/ollama/pull/17834)（开放）。
- 内存核算：修复多模型加载时 llama-server 的缓冲区去重问题；一个 30B dflash 模型在 `ollama ps` 和调度器 VRAM 核算中被误报为约 2.3GB — [ollama/ollama#17857](https://github.com/ollama/ollama/pull/17857)（开放）。
- VRAM 预测：在基于 GGUF 的权重估算中排除驻留 CPU 的输入张量 — [ollama/ollama#19393](https://github.com/ollama/ollama/pull/19393)（开放）；避免为满足 GPU 内存预测而驱逐仅 CPU 的 runner — [ollama/ollama#19284](https://github.com/ollama/ollama/pull/19284)（开放）。
- 聊天截断：在多步工具循环中因上下文限制而截断时，保留最近一条用户消息 — [ollama/ollama#18697](https://github.com/ollama/ollama/pull/18697)（开放）。
- CLI 延迟：在交互式 PTY 中 `ollama ls` 仍耗时约 5s，而通过管道时约 14ms，原因是 pselect6 超时 — [ollama/ollama#15597](https://github.com/ollama/ollama/issues/15597)（开放）。

## 稳定性与回归
按严重程度排序：

1. **严重 — ROCm KV 状态污染。** 在 `0.32.14-rocm` / Strix Halo gfx1151 上，A/B prompt 测试中的响应会被前一个无关请求污染。数据集中没有修复 PR。 — [ollama/ollama#17847](https://github.com/ollama/ollama/issues/17847)
2. **高 — CUDA 预热失败并静默回退到 CPU。** Windows/RTX 5070 Ti 上使用 `gpt-oss:20b` 时偶发 `CUDA error: shared object initialization failed`，与静默的 `CUDA_Host`→CPU pinned-buffer 回退相关。没有修复 PR。 — [ollama/ollama#17380](https://github.com/ollama/ollama/issues/17380)
3. **高 — GGUF MXFP4 MoE 溢出。** `gpt-oss:120b` 上出现 `read GGUF metadata ... unsupported tensor "blk.0.ffn_down_exps.weight" size overflows`。两份报告均已关闭，但本数据集中没有关联的修复 PR。 — [ollama/ollama#19375](https://github.com/ollama/ollama/issues/19375), [ollama/ollama#18869](https://github.com/ollama/ollama/issues/18869)
4. **高 — 工具调用回合被丢弃。** `go_template` 对 qwen3 工具调用静默丢弃整个 assistant 回合（空 content、`tool_calls: null`、`eval_count > 0`）；llama.cpp 模板能正确解析同一生成结果。已关闭。 — [ollama/ollama#19377](https://github.com/ollama/ollama/issues/19377)
5. **中 — MLX 工具调用。** `qwen3.8:27b-mlx` 仅在 assistant `content` 为空时发出 `tool_calls`；自由文本前导内容会抑制工具调用。开放。 — [ollama/ollama#19353](https://github.com/ollama/ollama/issues/19353)
6. **中 — OpenAI reasoning 丢失。** 当 `content` 是 content-parts 数组时，assistant `reasoning` 会被丢弃，仅含 reasoning 的回合会被删除。修复 PR 开放。 — [ollama/ollama#19380](https://github.com/ollama/ollama/issues/19380), [ollama/ollama#19384](https://github.com/ollama/ollama/pull/19384)
7. **中 — OpenAI `/v1/models` 数据为 null。** 零模型时返回 `"data": null` 而不是 `[]`；强类型 SDK 无法解析。开放，数据集中没有直接修复 PR。 — [ollama/ollama#19381](https://github.com/ollama/ollama/issues/19381)
8. **中 — 音频被静默忽略。** `/api/chat` 对支持音频的模型丢弃 `audio` 字段；相关编码器校验 PR 开放。 — [ollama/ollama#19378](https://github.com/ollama/ollama/issues/19378), [ollama/ollama#19397](https://github.com/ollama/ollama/pull/19397)
9. **中 — 并行语义。** `OLLAMA_NUM_PARALLEL=1` 不能阻止两个并发会话同时解码。开放。 — [ollama/ollama#19262](https://github.com/ollama/ollama/issues/19262)
10. **低 — Agent 压缩。** Claude Code 长时间运行的自动压缩在 `deepseek-v4.1-flash:1017-cloud` 上失败，导致上下文耗尽。开放。 — [ollama/ollama#19249](https://github.com/ollama/ollama/issues/19249)
11. **低 — MLX 重复循环。** MLX 可能将同一 token 流式输出到生成上限；PR 在出现 101 个相同 token 后中止。 — [ollama/ollama#19398](https://github.com/ollama/ollama/pull/19398)
12. **低 — OpenAI 空响应。** 空嵌入响应将 `data` 序列化为 `null`；修复 PR 返回数组。 — [ollama/ollama#19387](https://github.com/ollama/ollama/pull/19387)

## 这对应用开发者意味着什么
- **OpenAI 兼容客户端应保持防御性。** 处理 `/v1/models` 返回的 `data: null`，并在 [ollama/ollama#19384](https://github.com/ollama/ollama/pull/19384) 落地前，不要依赖 assistant `reasoning` 在 content-parts 往返中保留。与 `tool_calls` 并存时的无效内容类型也正在收紧 — [ollama/ollama#19389](https://github.com/ollama/ollama/pull/19389)。
- **Agent/工具循环构建者必须校验回合。** `go_template` 缺陷（[ollama/ollama#19377](https://github.com/ollama/ollama/issues/19377)）和 MLX qwen3 行为（[ollama/ollama#19353](https://github.com/ollama/ollama/issues/19353)）可能静默丢弃或抑制工具调用。截断修复（[ollama/ollama#18697](https://github.com/ollama/ollama/pull/18697)）对长多步循环很重要。
- **音频/多模态仍不一致。** 在 `/api/chat` 中，原生 `audio` 并不可靠；音频可能只能通过 `images` 使用，而 #19397 将开始拒绝模型无法编码的片段。发送媒体前请显式检查模型能力。
- **生产环境请固定模型 digest。** 模型更新可能改变输出质量，而回滚并非一等工作流 — 参见 Qwen3:4b 恢复请求 — [ollama/ollama#19385](https://github.com/ollama/ollama/issues/19385)。
- **MLX/Apple Silicon 正变得更可调。** 预计会有并发控制（`OLLAMA_NUM_PARALLEL`）和投机解码开关；合并后使用 `draft_num_predict=0` 获得干净的基准基线。
- **在 CPU/GPU 混合主机上保留内存余量。** 多模型和投机 draft 的 VRAM 核算修复正在进行；当前 `ollama ps`/调度器数字可能误报，尤其是在使用 draft 模型和驻留 CPU 的输入层时。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 — 2026-10-30

## 1. 今日亮点

LiteLLM 发布了 **v1.108.0-rc.1**，其 Docker 镜像已使用 cosign 签名；项目活跃度依然很高：过去 24 小时内有 93 个 issue 和 486 个 PR 更新。主要主题是**成本与花费核算的正确性**：多个由提供商上报的 `usage.cost` bug 已关闭，但仍有若干流式计费 issue 未解决。性能与架构工作聚焦于降低流式响应每个 chunk 的开销、提升路由公平性，并将数据库/Redis 访问抽取到带类型的 repository 与 transport 层之后。

## 2. 发布与破坏性变更

- **v1.108.0-rc.1** 已发布。Docker 镜像使用提交 [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 中引入的密钥通过 cosign 签名。所提供的发布数据中未包含其他迁移说明。
- **待处理的破坏性变更 — 实时音频计费：** [PR #35600](https://github.com/BerriAI/litellm/pull/35600) 新增 OpenAI/Azure 实时翻译和流式 `gpt-transcribe`，并将 Deepgram、Groq Whisper、Vertex Chirp 等按秒计费的转写模型从 `$0` 改为按秒计费。此变更被标记为破坏性，且仍处于打开状态。
- **面向 RC 的 MCP 认证回移：** [PR #49167](https://github.com/BerriAI/litellm/pull/49167) 将 #48939 回移到 `rc/1.108.0`，以恢复旧版 delegate MCP 服务器的上游 token 准入；同时将 `ddtrace` 从 4.11.0 升级到 4.15.6。如果你在 RC 上运行旧版 MCP delegate，此变更与你相关。

## 3. 新模型与硬件支持

- **CLF AI Gateway 提供商：** [PR #39324](https://github.com/BerriAI/litellm/pull/39324) 新增一个兼容 OpenAI 的网关提供商，包含十个已定价的开放权重模型，路由为 `clf_ai_gateway/<model>`。
- **Z.AI / GLM：** [PR #48335](https://github.com/BerriAI/litellm/pull/48335) 为 Z.AI 模型新增 `response_format` 和 `max_completion_tokens` 支持，并新增 GLM-5V-Turbo 的定价，包括缓存输入。
- **Azure AI Foundry Agents v2：** [Issue #25372](https://github.com/BerriAI/litellm/issues/25372) 已关闭，涵盖 Foundry agents 通过 `agent_reference` 支持 Responses API。
- **Vertex Claude 批处理：** [PR #45715](https://github.com/BerriAI/litellm/pull/45715) 将 Vertex Claude 批处理通过 Anthropic 原生 Messages 行结构路由，而不是在创建时返回 404。
- **DeepSeek V4 推理力度：** [Issue #27439](https://github.com/BerriAI/litellm/issues/27439) 已关闭；对于 DeepSeek V4，`high` 和 `max` 等 `reasoning_effort` 值不再被剥离。
- **OpenAI 实时音频：** [PR #35600](https://github.com/BerriAI/litellm/pull/35600) 在 OpenAI 和 Azure 上新增实时翻译与流式转写。
- **已请求但尚未落地：** 对 Kimi K3、Inkling 和 Tinker 的原生支持（[Issue #33921](https://github.com/BerriAI/litellm/issues/33921)）；通过代理支持 Soniox 实时 STT（[Issue #43677](https://github.com/BerriAI/litellm/issues/43677)）。

## 4. 性能与优化

- **流式 CPU 开销：** [PR #49166](https://github.com/BerriAI/litellm/pull/49166) 移除了每个流式 chunk 中 pydantic 内部反复抛出并捕获的 `AttributeError`。该 PR 报告称，在 OpenAI/Azure 流上每个 chunk 大约会产生十几个 Python 异常，因此这应能降低每个 chunk 的 CPU 成本。
- **最不繁忙路由公平性：** [PR #47484](https://github.com/BerriAI/litellm/pull/47484) 会立即预留所选 deployment，使并发请求不会都读取相同的 in-flight 计数并全部堆积到同一个 deployment 上。
- **OTPM 预留修复：** [PR #49054](https://github.com/BerriAI/litellm/pull/49054) 将默认的每分钟输出 token 预留上限设为 `otpm` 的四分之一或 deployment 上限，并在每次调用后校准 token 计数器。这样可避免当模型最大输出超过 deployment 的 `otpm` 时出现一刀切的 429。
- **跨 API 桥接重构：** [PR #46707](https://github.com/BerriAI/litellm/pull/46707) 让格式桥接调用原生 executor，而不是重新进入 `litellm.acompletion` / `aresponses`，从而避免每个请求重复执行 hooks、日志记录和重试路径。
- **数据库与 Redis 分层：** 一系列重构新增了按 repository 划分的 FastAPI 依赖（[#48507](https://github.com/BerriAI/litellm/pull/48507)、[#48592](https://github.com/BerriAI/litellm/pull/48592)、[#49116](https://github.com/BerriAI/litellm/pull/49116)）、单一数据库生命周期所有者（[#48506](https://github.com/BerriAI/litellm/pull/48506)），以及带类型的 Redis transport/store 层（[#48958](https://github.com/BerriAI/litellm/pull/48958)、[#49162](https://github.com/BerriAI/litellm/pull/49162)、[#49163](https://github.com/BerriAI/litellm/pull/49163)、[#48956](https://github.com/BerriAI/litellm/pull/48956)）。这些变更目前大多不改变行为，但降低了连接池耦合，并为 Rust 后端的 Redis 路径做准备。

## 5. 稳定性与回归

**严重 — 计费与花费核算**

- **流式流量被记录为 $0：** [Issue #48937](https://github.com/BerriAI/litellm/issues/48937) — 当上游报告 `usage.cost: 0`，同时又在其他地方报告真实成本时，流式 chat completions 会被记录为 `spend = 0`。状态为打开；未显示修复 PR。
- **非流式提供商成本被忽略：** [Issue #47533](https://github.com/BerriAI/litellm/issues/47533) — 对于兼容 OpenAI 的上游，在非流式调用中由提供商上报的 `usage.cost` 被忽略；已关闭。
- **未校验成本单位导致的幽灵花费：** [Issue #35829](https://github.com/BerriAI/litellm/issues/35829) — 提供商的 `usage.cost` 未经单位校验就被原样信任为 USD，导致出现天文数字的花费；已关闭。
- **OpenRouter 流式成本丢失：** [Issue #16021](https://github.com/BerriAI/litellm/issues/16021) — OpenRouter 流式不再保留 `usage.cost`；已关闭。
- **`n>1` 流式计费被合并：** [Issue #38180](https://github.com/BerriAI/litellm/issues/38180) — 并行流式 completions 被记录并计费为一个合并后的 completion；已关闭。
- **关闭时丢失花费缓冲区：** [Issue #34805](https://github.com/BerriAI/litellm/issues/34805) — 代理关闭期间，内存中的 key/team/user/end-user 花费缓冲区被静默丢弃。状态为打开。
- **无 Redis 时预算重置异常：** [Issue #37261](https://github.com/BerriAI/litellm/issues/37261) — 在没有 Redis 的情况下，`provider_budget_config` 报告的 `budget_reset_at` 大约超前 57 年。状态为打开。

**高 — 工具调用与协议正确性**

- **Claude 工具调用损坏：** [Issue #19858](https://github.com/BerriAI/litellm/issues/19858) — 通过 OpenRouter/Vertex 使用工具调用的 Claude 模型在 LiteLLM 代理配置中失败。状态为打开。
- **Gemini 流式工具上下文损坏：** [Issue #25869](https://github.com/BerriAI/litellm/issues/25869) — `stream_chunk_builder` 会损坏 Gemini 的 `server_side_tool_invocations` 和 `thought_signatures`，导致后续轮次出现 `400 Corrupted tool call context`。状态为打开且已停滞。
- **MCP 旧版 delegate 认证回归：** [PR #49167](https://github.com/BerriAI/litellm/pull/49167) 是针对旧版 delegate MCP 服务器对仅上游调用方返回 401 的待处理 RC 修复；该问题在 v1.103.0 中由 #40923 引入回归。

**中 — 代理与提供商边界情况**

- **Cursor 端点未记录日志：** [Issue #30126](https://github.com/BerriAI/litellm/issues/30126) — `/cursor/chat/completions` 返回 200 并向提供商产生费用，但不会创建 SpendLogs/UI 条目。状态为打开。
- **Azure AI prompt 缓存方言：** [PR #45295](https://github.com/BerriAI/litellm/pull/45295) 修复了当 `AZURE_AI_API_BASE` 指向不同资源时，`azure_ai/` deployment 的 chat prompt 缓存格式选择。
- **通配符健康检查：** [PR #49060](https://github.com/BerriAI/litellm/pull/49060) 仅探测部分通配符实际提供的模型，而不是提供商最便宜的无关 chat 模型。
- **已关闭的回归：** Bedrock 音频转写返回 400（[#41544](https://github.com/BerriAI/litellm/issues/41544)）、1.82.3 后有效 API key 被拒绝（[#24680](https://github.com/BerriAI/litellm/issues/24680)）、`/v2/user/info` 遗漏用户 RPM/TPM 限制（[#33347](https://github.com/BerriAI/litellm/issues/33347)）、Redis 语义缓存失败（[#31610](https://github.com/BerriAI/litellm/issues/31610)）、多 worker 下子进程死亡（[#18457](https://github.com/BerriAI/litellm/issues/18457)）、模块导入时发起 HTTP GET（[#22159](https://github.com/BerriAI/litellm/issues/22159)），以及 Claude 空内容清理（[#24498](https://github.com/BerriAI/litellm/issues/24498)）。

## 6. 对应用开发者的意义

- **在没有校验前，不要信任流式花费数字。** 若干计费路径仍会错误处理提供商上报的成本，尤其是流式请求和兼容 OpenAI 的上游。如果你依赖 LiteLLM 预算或花费日志进行成本分摊或硬性限制，请在执行配额前将流式流量与提供商发票进行核对。
- **将 `v1.108.0-rc.1` 视为 RC。** 在生产流水线中验证 cosign 签名，并注意 MCP 旧版 delegate 认证修复是待处理的回移（[#49167](https://github.com/BerriAI/litellm/pull/49167)），尚未确认包含在 RC 制品中。
- **在关键路径中避免依赖 Claude 工具调用或 Gemini 流式工具后续轮次**，直到 [#19858](https://github.com/BerriAI/litellm/issues/19858) 和 [#25869](https://github.com/BerriAI/litellm/issues/25869) 得到解决。
- **实时音频计费将发生变化。** 如果你使用按秒计费的 STT 提供商，待处理的实时 PR 会将其从 `$0` 改为按秒计费。在采用该版本前请先建模计算成本。
- **提供商覆盖范围正在扩大：** Z.AI/GLM、CLF AI Gateway、Azure Foundry Agents v2、Vertex Claude 批处理和 DeepSeek V4 推理控制减少了对自定义适配器的需求，但其中若干是近期功能或仍处于打开的 PR——请固定版本，并测试特定提供商的请求/响应形状。
- **突发型工作负载应当会受益**于最少繁忙预留和 OTPM 校准：这些机制落地后可减少高并发 agent 工作负载中的 429 风暴和 deployment 热点。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-30

## 1. 今日亮点
过去 24 小时内没有新版本发布，但项目正沿两个方向快速推进：一条融合式 batch-1 NF4 解码路径，宣称对 4-bit Llama 实现 **每 token 1.6–2.3× 加速**（[PR #16032](https://github.com/unslothai/unsloth/pull/16032)）；以及两项针对 Ampere/Ada 级显卡上 Gemma 3 flex-attention 回退问题的修复（[PR #16041](https://github.com/unslothai/unsloth/pull/16041)、[PR #16049](https://github.com/unslothai/unsloth/pull/16049)）。与此同时，长对话中上下文压缩相关的未解决回归（[Issue #15800](https://github.com/unslothai/unsloth/issues/15800)）和 Windows/AMD commit-charge 耗尽（[Issue #16015](https://github.com/unslothai/unsloth/issues/16015)）是 Studio 用户面临的主要稳定性问题。

## 2. 版本发布与破坏性变更
过去 24 小时内无。注意 Studio 用户当前使用 `0.1.911-beta` / `0.1.912-beta`，今天有多份报告与更新相关（[Issue #15866](https://github.com/unslothai/unsloth/issues/15866)、[Issue #15932](https://github.com/unslothai/unsloth/issues/15932)）——如果你依赖某个版本，请固定到一个已知可用的 beta。

## 3. 新模型与硬件支持
- **LFM3 系列（1.7B）** 已被请求支持；新的 LiquidAI 主干架构尚未支持（[Issue #15588](https://github.com/unslothai/unsloth/issues/15588)，开放中，10 👍）。
- **EXL3（ExLlamaV3）量化后端** 被提议作为 bitsandbytes 的替代方案：支持 2/3/4/6/8-bit 和分数比特率，以及 MoE 量化（[PR #7115](https://github.com/unslothai/unsloth/pull/7115)，开放中/进行中）。
- **Studio 中多个常驻 GGUF 模型**，每个模型位于独立的 `llama-server` 进程中，并具备模型感知的请求路由（[PR #10876](https://github.com/unslothai/unsloth/pull/10876)，已合并/已关闭）。
- **EXL3 与 AMD 核显处理**：Studio 现在会将 AMD 集成显卡排除在训练页面的 VRAM 总量和默认 GPU 选择之外，这解除了双 R9700 配置的阻塞（[PR #14039](https://github.com/unslothai/unsloth/pull/14039)）。
- **Qwen3-ASR 语音上下文设置**（Auto / 1K–64K）可减少每次录音的内存分配（[PR #15810](https://github.com/unslothai/unsloth/pull/15810)）。
- **VibeVoice q4-ios GGUF 已从 Studio 提供列表中移除**，因为捆绑的 `audio.cpp` 运行时无法加载它（[PR #16048](https://github.com/unslothai/unsloth/pull/16048)）。

## 4. 性能与优化
- **Llama 的融合式 batch-1 NF4 解码**：`UNSLOTH_DECODE_FUSED_NF4=1` 将每个解码器层约 12 次内核启动（启用 LoRA 时约 27–33 次）合并为更少次数；在 eager `generate` 中报告 **每 token 1.6–2.3×**。需手动启用；默认路径不变（[PR #16032](https://github.com/unslothai/unsloth/pull/16032)）。
- **Gemma 3 flex attention**：短 prompt（17–127 tokens）可能使进程在 sm86/sm89（L4、A10、RTX 3090/4090、A6000、L40S）上陷入 eager `math_attention`。为短时无梯度调用使用小型共享内存块，可让编译后的 flex attention 保持生效（[PR #16049](https://github.com/unslothai/unsloth/pull/16049)）；配套修改会在发生回退时让生成走非 flex 路径（[PR #16041](https://github.com/unslothai/unsloth/pull/16041)）。文中引用的具体结果：L4 / Gemma 3 1B / 16k 上下文得到改善。
- **ASR 内存**：在 11 秒片段上，启用新的语音上下文上限后，Qwen3-ASR 从 64K 上下文下的约 8.7 GiB 降至 **1K 下约 2.0 GiB**（[PR #15810](https://github.com/unslothai/unsloth/pull/15810)）。
- **MLX 内存估算** 现在会为全宽 f16 KV cache 计入注意力分数矩阵的内存开销，也就是 MLX 实际将这些矩阵物化的位置（[PR #16021](https://github.com/unslothai/unsloth/pull/16021)）；这解决了 [Issue #15935](https://github.com/unslothai/unsloth/issues/15935) 中报告的严重虚高估算（一个能在 24 GB Mac 上加载的模型被估成 69.7 GiB）。
- **GGUF 对话的混合 token 计数**：稠密估算保护加上按比例调整的读取超时，避免在出现 “Message too long” 前卡住约 2 分钟（[PR #15857](https://github.com/unslothai/unsloth/pull/15857)）。

## 5. 稳定性与回归
按严重程度排序：

1. **再次进入长对话时上下文从不压缩** —— 对话变得无法继续（[Issue #15800](https://github.com/unslothai/unsloth/issues/15800)，开放中）。正在通过混合 token 计数修复（[PR #15857](https://github.com/unslothai/unsloth/pull/15857)）。
2. **被截断的工具调用变成看似有效的调用** —— 当本地模型在工具调用中途达到 `max_tokens` 时，Anthropic 兼容端点仍会发出一个完整且参数有效的工具调用；编码代理会执行部分写入（[PR #16035](https://github.com/unslothai/unsloth/pull/16035)，开放中 —— 已提出修复）。
3. **Windows + AMD commit-charge 耗尽** —— 即使权重能放入 VRAM，`llama-server.exe` 在加载 GGUF 后仍保留约 16 GB 的 commit，耗尽 32 GB 机器上的页面文件（[Issue #16015](https://github.com/unslothai/unsloth/issues/16015)）。缓解 PR 会在加载时发出警告（[PR #16046](https://github.com/unslothai/unsloth/pull/16046)）。
4. **近期更新后出现核心转储** —— 无论 KV 使用情况如何，模型几轮后停止响应，且卸载模型会触发核心转储（[Issue #14411](https://github.com/unslothai/unsloth/issues/14411)，已关闭）。
5. **微调数据处理 bug** —— “Train on completions” 会静默丢弃 80%+ 的 Alpaca 数据集，因为从未安装 Alpaca 模板，而掩码处理却在查找其标记（[Issue #15894](https://github.com/unslothai/unsloth/issues/15894)，已关闭；重复的 [Issue #15895](https://github.com/unslothai/unsloth/issues/15895) 开放中）。单列 `text`/`content` JSONL 完全无法训练（[Issue #15892](https://github.com/unslothai/unsloth/issues/15892)、[Issue #15901](https://github.com/unslothai/unsloth/issues/15901)，已关闭）。
6. **Anthropic 透传忽略自动压缩** —— 历史记录过大时，`/v1/messages` 会返回不可恢复的 400 `exceed_context_size_error`（[Issue #15417](https://github.com/unslothai/unsloth/issues/15417)，已关闭）。
7. **MLX/GGUF 模型列表缺失** —— LFM3 MLX 模型未出现在 `curl /v1/models` 中（[Issue #15832](https://github.com/unslothai/unsloth/issues/15832)）；MiniCPM-V-4.7-1B Vulkan 后端文本生成失败（[Issue #14987](https://github.com/unslothai/unsloth/issues/14987)，已关闭）。
8. **下载竞态**：清单基于 `model_info` 规划，但 `snapshot_download` 会重新解析 `main`，因此移动中的分支可能拉取到不匹配的快照（[PR #16047](https://github.com/unslothai/unsloth/pull/16047)，修复开放中）。
9. **其他**：AppImage UI 卡顿（[Issue #15999](https://github.com/unslothai/unsloth/issues/15999)），托管账户尽管所有者开关已打开，仍被阻止访问本地/回环 MCP 服务器（[Issue #16029](https://github.com/unslothai/unsloth/issues/16029)，修复 [PR #16044](https://github.com/unslothai/unsloth/pull/16044)），ModelScope 源阻止大于 50 GB 的文件（[Issue #15989](https://github.com/unslothai/unsloth/issues/15989)），MiniMax-M3.1 NVFP4 即使全部 61 层都已卸载仍回退到 CPU（[Issue #15409](https://github.com/unslothai/unsloth/issues/15409)，已关闭）。

## 6. 这对应用开发者意味着什么
- **如果你依赖长时间运行的代理，目前应将上下文管理视为不安全。** Issue #15800 加上 Anthropic 透传行为意味着客户端侧的历史裁剪仍然是必需的；不要信任服务端的自动压缩。在 [PR #16035](https://github.com/unslothai/unsloth/pull/16035) 落地前，也要自行校验工具调用是否完整——当前被截断的 `Write` 与完整调用无法区分。
- **性能收益需手动启用且因模型而异。** `UNSLOTH_DECODE_FUSED_NF4=1` 值得针对单流 4-bit Llama 服务进行基准测试；一旦 #16049/#16041 合并，3090/4090/L4 级硬件上的 Gemma 3 应会看到更一致的 flex-attention 行为。
- **Apple Silicon 用户**：内存估算此前严重高报，因此不要基于 #16021 之前的估算器拒绝模型，但也要为旧估算忽略的未融合注意力分数矩阵预留预算。
- **建议固定 beta 版本。** Studio 桌面端是大多数已报告回归的主要发生面（更新失败、UI 卡顿、模型加载失败、数据集预览），且发布节奏为每日。如果你以编程方式提供模型服务，优先使用 Python/CLI 路径，那里数据集和上下文压缩问题不适用。
- **关注多模型常驻**（[PR #10876](https://github.com/unslothai/unsloth/pull/10876)）和 EXL3（[PR #7115](https://github.com/unslothai/unsloth/pull/7115)），如果你计划部署 MoE 或多租户网关——两者正好针对当前制约这些设置的路由/量化缺口。

---

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*