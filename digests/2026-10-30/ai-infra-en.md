# AI Infrastructure Digest 2026-10-30

> Generated: 2026-10-30 02:07 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Comparison — 2026-10-30

## 1. Ecosystem Overview

The AI infrastructure stack remains in a high-churn build-out phase: vLLM and SGLang each touched roughly 500 PRs in 24 hours, LiteLLM touched 486, and llama.cpp shipped multiple daily builds. Optimization effort is shifting from raw kernel speed toward backend parity, KV-cache locality, and agentic scheduling across ROCm, XPU, NPU, SM120/Blackwell, SYCL, Metal, and Hexagon. At the same time, stability and correctness reports—silent KV contamination, scheduler crashes, weight-sync corruption, truncated tool calls, and streaming billing at $0—show that production hardening is lagging feature velocity. The local-runtime layer is converging on quantized KV and low-bit formats, while the gateway layer is maturing around cost accounting, provider abstraction, and routing fairness. For application developers, the main signal is that model/backend/version pinning and external validation of tool calling, caching, and spend remain mandatory.

## 2. Activity Comparison

| Project | Issues updated (24h) | PRs updated (24h) | Release status |
|---|---:|---:|---|
| **vLLM** | 180 | 500 | No release; Model Runner V1 removal in progress |
| **SGLang** | 35 (30 shown) | 500 (top 20 shown) | No release; v0.5.22 operational notes |
| **llama.cpp** | n/r | n/r | Multiple builds shipped, b11978–b11998 |
| **Ollama** | n/r | n/r | No release |
| **LiteLLM** | 93 | 486 | **v1.108.0-rc.1** released |
| **Unsloth** | n/r | n/r | No release; Studio users on 0.1.911/0.1.912-beta |

*n/r = aggregate count not reported in the digest; not inferred from visible items. Counts are “touched/updated,” not necessarily open.*

**Readout:** vLLM, SGLang, and LiteLLM are the highest-churn projects today. llama.cpp is releasing continuously but did not report aggregate issue/PR counts. Ollama and Unsloth had no releases and lower reported activity, but both have active correctness and performance work.

## 3. Model Support Race

**vLLM** is strongest on datacenter accelerator + model + quantization combinations: MiniMax-M3 on ROCm, DeepSeek V4.1 MXFP8 on gfx950, Qwen3-Next/Qwen3.5 fused kernels on ROCm, Intel XPU Mooncake KV transfer, and CPU/non-Triton MRv2 enablement. It also has an online re-quantization RFC extending beyond BF16/FP16/FP32 inputs.

**SGLang** is pushing the broadest non-NVIDIA accelerator roadmap: T-Head PPU, AMD RDNA3/RDNA4 and TLX, Ascend NPU DSpark/EPLB/mixed-chunk DSA, Intel XPU prefill CUDA graphs and FP8 MoE, plus new model paths such as `sensenova_loop` and DFlash-family MoE drafts. It is ahead on NPU/AMD enablement breadth; vLLM remains ahead on CUDA/ROCm/XPU model+quant coverage in this window.

**llama.cpp** shipped new GGUF/edge architectures: Mistral “Le Chonk” and Limite 1B “Violetto,” plus OpenCL IQ2, SYCL quantized-KV DPAS attention, Hexagon UPSCALE/CONCAT, and Metal MXFP4 work. It leads on consumer/edge architecture coverage and GGUF-adjacent backends.

**Ollama** landed no new architecture in this window. It is focused on MLX backend work, OpenAI-compatibility fixes, and audio/multimodal gaps; LFM3-Omni/phonon audio support is requested but not landed.

**LiteLLM** is not a model runtime but is expanding provider/model coverage: CLF AI Gateway, Z.AI/GLM response-format and pricing support, Azure Foundry Agents v2, Vertex Claude batches, and DeepSeek V4 reasoning-effort handling. It leads on provider abstraction and gateway-level model support.

**Unsloth** did not add new architecture support; LFM3 remains requested and unsupported. Its work is concentrated on fine-tuning/quantization and Studio: EXL3 backend proposal, multi-resident GGUF models, AMD iGPU handling, Qwen3-ASR voice-context memory control, and fused NF4 decode.

**Who is ahead:** vLLM and SGLang lead datacenter serving and accelerator coverage; llama.cpp leads edge/GGUF architecture support; LiteLLM leads provider/model gateway coverage; Unsloth specializes in fine-tuning and low-bit optimization rather than new-architecture parity.

## 4. Performance Frontier

Optimization is concentrated in five areas:

- **Kernels and backend parity.** vLLM has ViT full CUDA Graph, SM80 DiffKV prefill tiling (~3.0× on 200K-context chunks), ROCm MoE top-k, and AITER FlyDSL MXFP8 GEMM (+3.7% DSv4.1 throughput). SGLang has AMD diffusion prefill paths (12.4→18 TF at 16K tokens), AMD DSA metadata reuse, NPU scale folding (~2 ms/layer), and XPU prefill graph capture. llama.cpp has Metal MXFP4 mat-vec, SYCL DPAS quantized-KV attention, Hexagon CONCAT, and OpenCL dp4a defaults. Unsloth claims 1.6–2.3× per-token speedup from fused batch-1 NF4 decode.

- **KV cache and prefix caching.** vLLM is working on NVFP4 KV, KV offload fail-fast behavior, and sleep-mode CPU backup reclamation. SGLang has HiCache, session-aware KV orchestration, DCP-aware draft KV sizing, and PD rebootstrap fixes. llama.cpp added SYCL quantized-KV attention and is tracking CPU q8_0 KV fallback. Ollama has a critical ROCm gfx1151 KV-state bleed. KV locality is now a first-order latency and correctness concern.

- **Batching, scheduling, and distributed serving.** vLLM added `--dp-prefill-delayer` for agentic multi-turn decode interruption and PP decode batch balancing. SGLang is advancing mixed-chunk DSA, AFD disaggregation, and PD rebootstrap token replay. llama.cpp has a silent hybrid LFM2 co-batching corruption bug. Ollama has parallel-session semantics issues. LiteLLM is improving least-busy routing and OTPM reservation.

- **Quantization.** vLLM covers MXFP8, NVFP4, and online re-quantization; SGLang covers FP8 MoE, MXFP4, and Ascend/NPU layouts; llama.cpp covers MXFP4, IQ2, q4_0 dp4a, and quantized KV; Ollama tracks MXFP4/ternary quant; Unsloth covers NF4, fused NF4 decode, and proposed EXL3. Low-bit support is expanding faster than quality validation.

- **Gateway and streaming overhead.** LiteLLM is removing per-chunk pydantic exceptions, reserving deployments before routing, reconciling OTPM counters, and refactoring DB/Redis access. This is CPU/routing efficiency, not GPU kernel work.

**Summary:** The frontier is not a single benchmark race. It is backend parity + kernel fusion + KV/cache management + scheduling under agent traffic. Raw token throughput remains important, but cache locality and correctness now determine production cost.

## 5. Layer Positioning

| Layer | Projects | Role |
|---|---|---|
| **High-throughput serving engines** | **vLLM, SGLang** | Datacenter inference, continuous batching, P/D disaggregation, TP/PP/DP/EP, KV management, accelerator backends. vLLM has broad CUDA/ROCm/XPU/CPU coverage; SGLang is strong on AMD/NPU scheduling and cache orchestration. |
| **Local/edge runtimes** | **llama.cpp, Ollama** | llama.cpp is the low-level GGUF runtime across CPU, Apple Metal, SYCL, OpenCL, Hexagon, CUDA, HIP. Ollama is the developer-facing local server/desktop wrapper with model management, OpenAI compatibility, and MLX work. |
| **LLM gateway** | **LiteLLM** | Provider abstraction, routing, budgets/spend, auth, observability, OpenAI-compatible proxy. It does not serve model kernels; it governs traffic and cost across backends. |
| **Training/fine-tuning** | **Unsloth** | Fine-tuning and quantization kernels, Studio workflows, low-bit inference optimizations. It overlaps with local runtimes through Studio’s llama-server/GGUF support but is differentiated by training and dataset handling. |

Boundaries are blurring—Ollama uses llama.cpp/MLX, Unsloth Studio uses `llama-server`, and LiteLLM fronts all serving layers—but the ownership is clear: **vLLM/SGLang serve, llama.cpp/Ollama run locally, LiteLLM governs, Unsloth trains and quantizes.**

## 6. Trend Signals

- **Hardware fragmentation is the new normal.** ROCm, XPU, NPU, SM120/Blackwell, SYCL, Metal, and Hexagon all have active gaps. Backend selection is not portable: SM120 has FA4/MLA/NVFP4 issues in vLLM/SGLang; sm_75 has `sgl_kernel` cubin gaps; SYCL has MoE prompt-processing regressions. Pin per-SKU known-good versions.

- **KV/prefix cache is the primary agent latency lever.** SGLang’s session-aware KV and HiCache work, vLLM’s KV offload/NVFP4 KV, llama.cpp’s quantized-KV attention, and Ollama’s KV bleed all point the same way. Agent developers should design session IDs and routing keys now to exploit cache locality later.

- **Agentic scheduling is moving into the engine.** vLLM’s DP prefill delayer and PP balancing, SGLang’s mixed-chunk and AFD work, and retraction/penalty bugs show that multi-turn agent traffic is stressing schedulers. Validate p99 inter-token latency and post-retraction determinism, not just throughput.

- **Low-bit quantization is expanding, but correctness is the constraint.** NVFP4 KV corruption, MXFP4 GGUF metadata overflow, OpenCL q4_0 quality changes, and EXL3/NF4 proposals mean teams should benchmark quality and validate model outputs, not just speed.

- **Tool calling and structured output remain brittle across the stack.** vLLM fixed nullable-schema and raw-newline cases; llama.cpp has `response_format` + tools 400s; Ollama drops Qwen3 tool turns; LiteLLM has Claude/Gemini tool-context failures; Unsloth can emit truncated tool calls as valid. Agent builders still need a validation/repair layer.

- **Gateway cost accounting is a production risk.** LiteLLM has streaming spend at $0, ignored provider cost, phantom spend from unvalidated units, and dropped spend buffers on shutdown. Realtime audio billing is also changing. Audit streaming traffic against provider invoices before enforcing budgets.

- **Release cadence is bifurcated.** llama.cpp ships multiple builds per day; vLLM/SGLang operate on high PR churn with no release; LiteLLM is on an RC; Ollama/Unsloth Studio users are on betas. Treat all as fast-moving and pin versions for production.

- **Observability is improving but needs external validation.** SGLang adds `scheduler_iterations_total`, Ollama exposes speculative-decoding metrics, and LiteLLM has spend logs—but the bugs above show internal metrics can misreport. Combine native telemetry with provider-side and client-side checks.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-30

## Today's Highlights

No new releases landed in the last 24h, but activity remains very high (180 issues and 500 PRs touched), concentrated on **multimodal graph capture**, **ROCm/XPU backend parity**, and **scheduler correctness**. The most-watched item is the ViT full CUDA Graph RFC ([#38175](https://github.com/vllm-project/vllm/issues/38175), 47 comments), which targets the remaining host-side launch overhead in vision-language serving. On the correctness side, a same-step priority eviction bug has a fix PR open ([#59963](https://github.com/vllm-project/vllm/pull/59963)), while several SM120/Blackwell kernel-instantiation gaps remain unresolved.

## Releases & Breaking Changes

No releases in the last 24h. One forward-looking compatibility note:

- **Model Runner V1 removal is in progress** ([#63373](https://github.com/vllm-project/vllm/pull/63373), referenced in [#61995](https://github.com/vllm-project/vllm/pull/61995)). MRv2 depends on Triton, so non-Triton CPU targets (Arm, IBM Z, s390x) need the fallback path in #61995 before MRv1 can be dropped. Operators on CPU-only deployments should track this.

## New Model & Hardware Support

- **MiniMax-M3 on ROCm**: avoids allocating an unused index-key cache for layers that reuse top-k indices, fixing resource registration for the existing opt-in sharing mode ([#64403](https://github.com/vllm-project/vllm/pull/64403)).
- **Intel XPU + MooncakeStoreConnector**: removes two CUDA assumptions (`torch.cuda.Event` annotation evaluation, device handling) that broke KV transfer on XPU workers ([#62546](https://github.com/vllm-project/vllm/pull/62546)).
- **CPU / non-Triton backends**: enables MRv2 by default where Triton is unavailable ([#61995](https://github.com/vllm-project/vllm/pull/61995)).
- **DeepSeek V4.1 MXFP8 on gfx950**: 32×32 block-scaled linears move from the Triton block-scaled GEMM to AITER's FlyDSL MXFP8 GEMM when AITER is enabled ([#60169](https://github.com/vllm-project/vllm/pull/60169)).
- **Qwen3-Next / Qwen3.5 on ROCm**: enables the backend-agnostic fused QK-RMSNorm + RoPE + gate Triton kernel that was previously CUDA-gated ([#51406](https://github.com/vllm-project/vllm/pull/51406)).
- **Online re-quantization from arbitrary precision** (RFC closed): extends the online quantization API beyond BF16/FP16/FP32 inputs, e.g. FP8 → MXFP4 ([#59062](https://github.com/vllm-project/vllm/issues/59062)).

## Performance & Optimization

- **DiffKV prefill tiling (SM80)**: `BLOCK_M=128` with a 64-wide KV tile yields **~3.0× speedup on 200K-context prefill chunks** on GPUs without FA3/FA4, where the previous `BLOCK_M=16` path re-loaded the entire KV tile per query position ([#61095](https://github.com/vllm-project/vllm/issues/61095)).
- **ViT full CUDA Graph** (RFC tracker): capturing the entire ViT forward to eliminate per-layer launch overhead for Qwen3-VL, Qwen3.5, GLM-V, Kimi K2.5 ([#38175](https://github.com/vllm-project/vllm/issues/38175)).
- **ROCm MoE top-k**: replaces AITER `biased_grouped_topk` with a register-resident Triton sigmoid+bias top-k for the degenerate case `topk_group >= num_expert_group`, where the LDS group scan eliminates nothing ([#63076](https://github.com/vllm-project/vllm/pull/63076)).
- **GLM-5.3-Flash on ROCm**: removes int32↔int64 casts, a `+1`/`-1` fill, and a buffer copy from the kpool sparse indexer in every MLA layer ([#63910](https://github.com/vllm-project/vllm/pull/63910)).
- **DSv4.1 on gfx950**: AITER FlyDSL MXFP8 GEMM reports **+3.7% total token throughput** ([#60169](https://github.com/vllm-project/vllm/pull/60169)).
- **Cross-DP prefill delayer** (`--dp-prefill-delayer`, opt-in): prevents a prefill on any DP rank from turning every rank's MoE step into a long mixed/eager step, reducing decode interruption under agentic multi-turn traffic ([#61834](https://github.com/vllm-project/vllm/pull/61834)).
- **PP decode batch balancing**: caps established decode requests per step at `ceil(max_num_seqs / pipeline_parallel_size)` without restricting prefill scheduling ([#50410](https://github.com/vllm-project/vllm/pull/50410)).
- **Triton attention metadata**: sizes the 3D-kernel threshold and split-K scratch from per-group `kv_cache_spec` instead of model-wide head counts, fixing slower 2D-kernel decode on heterogeneous models such as Gemma 4 ([#56996](https://github.com/vllm-project/vllm/pull/56996)).
- **Kernel micro-optimizations**: size-gated PDL for `silu_and_mul_nvfp4_quant` on SM90+ ([#64494](https://github.com/vllm-project/vllm/pull/64494)); exact-size page-locked CPU backup for level-1 sleep to stop power-of-two rounding leaks ([#64630](https://github.com/vllm-project/vllm/pull/64630)).
- **Trackers in flight**: gfx950/MI355X optimization plan for Qwen3.8-Flash-Next MXFP4 ([#59575](https://github.com/vllm-project/vllm/issues/59575)); GLM-5.3 CUDA optimization follow-up ([#57406](https://github.com/vllm-project/vllm/issues/57406)); MooncakeConnector improvements for P/D disaggregation on non-CUDA platforms ([#60523](https://github.com/vllm-project/vllm/issues/60523)); ROCm disaggregated P/D + Wide-EP CI ([#48117](https://github.com/vllm-project/vllm/issues/48117)).

## Stability & Regressions

Ranked by severity:

1. **Illegal memory access + silent corruption (high)** — Qwen3.8-27B (GDN hybrid) with PP=3 on sm_86: Xid 31 with prefix caching, gibberish output without it. Regression since v0.28.0, confirmed on v0.31.0 and v0.32.0 across both model runners. No fix PR yet ([#63505](https://github.com/vllm-project/vllm/issues/63505)).
2. **KV offload does not fail fast (high)** — P2P KV offload tier continues on peer faults instead of surfacing them, risking stalled or incorrect transfers on NIXL/InfiniBand setups ([#63602](https://github.com/vllm-project/vllm/issues/63602)).
3. **SM120 sparse-MLA kernel gap (high)** — DeepSeek-V4.1-Flash cannot run on RTX PRO 6000 (SM120): FlashInfer only instantiates a sparse-MLA decode kernel for `page_block_size=64`, but the compressed layer uses 32 ([#59203](https://github.com/vllm-project/vllm/issues/59203)).
4. **NVFP4 KV scale corruption (high)** — SM100 trtllm-gen swizzle is written unconditionally, silently corrupting the SM120 FlashInfer XQA route on RTX 5090 ([#50084](https://github.com/vllm-project/vllm/issues/50084)).
5. **Scheduler priority inversion (medium-high)** — priority scheduling can un-schedule a request already scheduled in the same step (encoder cache miss, prefix hits on unwritten KV). **Fix PR open**: [#59963](https://github.com/vllm-project/vllm/pull/59963) for [#59907](https://github.com/vllm-project/vllm/issues/59907).
6. **GLM-5.x DSA under pipeline parallelism (medium-high)** — reused layers at a stage start read stale top-k indices; hardware-independent, affects any `--pipeline-parallel-size > 1` ([#63170](https://github.com/vllm-project/vllm/issues/63170)).
7. **Speculative decoding init crash (medium)** — Gemma4 MTP drafter fails at engine init on 0.25.1 with "a and b must have same reduction dim"; regression from 0.21.0 on aarch64/GB10 ([#48848](https://github.com/vllm-project/vllm/issues/48848)).
8. **FP8 hybrid GDN + DFlash (medium)** — Qwen3.5-122B-A10B-FP8 fails with `CUBLAS_STATUS_EXECUTION_FAILED` at profile_run on nightly/0.25; 0.24.0 works ([#48477](https://github.com/vllm-project/vllm/issues/48477)).
9. **torch.compile stride mismatch (medium)** — Qwen3.5/Qwen3-Next GDN attention crashes at warmup at the `splitting_ops` boundary; recurrence of #29014 ([#50046](https://github.com/vllm-project/vllm/issues/50046)).
10. **ROCm gfx1201 AITER regression (medium)** — unified-attention kernel in v0.25.0 exceeds the LDS limit ([#48723](https://github.com/vllm-project/vllm/issues/48723)).
11. **Prefix cache miss on token boundary (low-medium)** — Kimi-K3 fails cache reuse when prompt length is exactly a 1536-token boundary ([#50235](https://github.com/vllm-project/vllm/issues/50235)).
12. **Spec-decode TP divergence (low-medium)** — adaptive DSpark verification decisions not synchronized across TP ranks; **fix PR open** ([#62769](https://github.com/vllm-project/vllm/pull/62769)).
13. **KV sizing order (low)** — batch-sharded sampling profiles `all_to_all_single` *after* the KV cache has consumed the memory budget; **fix PR open** ([#61025](https://github.com/vllm-project/vllm/pull/61025)).

Closed/resolved today: multimodal receiver cache miss ([#26195](https://github.com/vllm-project/vllm/issues/26195)), structured-output + spec-decode distribution preservation RFC ([#62818](https://github.com/vllm-project/vllm/issues/62818)), Qwen3.6-35B-A3B cross-sequence logprob drift on sm_120 ([#59764](https://github.com/vllm-project/vllm/issues/59764)), sparse MLA indexer 1 GiB buffer mis-allocation ([#60450](https://github.com/vllm-project/vllm/issues/60450)).

## What This Means for Application Developers

- **Tool calling is getting more robust for nullable schemas.** Muse, MiniCPM5, and Poolside V1 parsers previously only preserved raw argument text for exact `{"type": "string"}` schemas, corrupting `str | None` parameters ([#64539](https://github.com/vllm-project/vllm/pull/64539)); the Pythonic/Llama4 parsers now preserve raw newlines inside string arguments, including in streaming and parallel calls ([#44102](https://github.com/vllm-project/vllm/pull/44102)). If you rely on optional-string tool params, track #64539.
- **Agentic workloads get a scheduling lever.** `--dp-prefill-delayer` directly targets the multi-turn pattern where frequent prefills starve decode across data-parallel ranks ([#61834](https://github.com/vllm-project/vllm/pull/61834)). Worth benchmarking if your p99 inter-token latency degrades under bursty agent traffic.
- **Long-context prefill on Ampere-class GPUs is materially cheaper** (~3× on 200K-token chunks per [#61095](https://github.com/vllm-project/vllm/issues/61095)) — relevant if you are running 128K–200K contexts on SM80 rather than Hopper/Blackwell.
- **Blackwell / SM120 remains a sharp edge.** DeepSeek-V4.1-Flash sparse MLA and NVFP4 KV cache both have unresolved SM120 kernel gaps ([#59203](https://github.com/vllm-project/vllm/issues/59203), [#50084](https://github.com/vllm-project/vllm/issues/50084)). Pin known-good versions rather than tracking nightlies on RTX PRO 6000 / RTX 5090 for these model+quant combos.
- **P/D disaggregation on non-CUDA platforms is being hardened** (XPU Mooncake fix [#62546](https://github.com/vllm-project/vllm/pull/62546), MooncakeConnector RFC [#60523](https://github.com/vllm-project/vllm/issues/60523)). If you are designing a disaggregated deployment on ROCm/XPU, expect the connector surface to shift.
- **Sleep mode memory is now reclaimable.** Level-1 sleep previously held power-of-two-rounded pinned CPU buffers indefinitely after wake; the fix releases them by default with an opt-in `VLLM_SLEEP_RETAIN_CPU_BACKUP=1` ([#64630](https://github.com/vllm-project/vllm/pull/64630)). Relevant for multi-model hosts that hot-swap engines.
- **Benchmark clients get stricter SSE framing** — split separators and CRLF handling fixed in `StreamedResponseHandler` ([#61534](https://github.com/vllm-project/vllm/pull/61534)). If you maintain a custom load generator, mirror this to avoid miscounted tokens.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-30

Source: [sgl-project/sglang](https://github.com/sgl-project/sglang) · No releases in the last 24h · 35 issues updated (30 shown), 500 PRs updated (top 20 shown)

---

## 1. Today's Highlights

CI instability continues to dominate attention: the flaky-test tracker [#42752](https://github.com/sgl-project/sglang/issues/42752) now has 140 comments, making it the most active thread in the repo. On the correctness side, a high-severity scheduler crash in DeepSeek-V4.1-Flash multimodal serving ([#47218](https://github.com/sgl-project/sglang/issues/47218)) and a silent weight-corruption path in DFLASH draft updates ([#46806](https://github.com/sgl-project/sglang/issues/46806)) were the most consequential reports. Architecture work advanced via RFCs on model-independent parallelism ([#47170](https://github.com/sgl-project/sglang/issues/47170)) and session-aware KV orchestration ([#45189](https://github.com/sgl-project/sglang/issues/45189)), plus a broad NPU/AMD enablement push in the PR queue.

---

## 2. Releases & Breaking Changes

No new versions or tags in the last 24h. Two operational notes worth flagging:

- **Python 3.13 installs now require a Rust toolchain** on sglang 0.5.22, caused by `outlines==0.1.11 → outlines_core==0.1.26` exact pinning. Undocumented and likely to break hermetic/air-gapped builds — [#47242](https://github.com/sgl-project/sglang/issues/47242).
- **`/start_profile` with `profile_by_stage=true` and no `num_steps`** raises `TypeError: NoneType += int` and crashes every scheduler on v0.5.22 — [#47294](https://github.com/sgl-project/sglang/issues/47294).

---

## 3. New Model & Hardware Support

**Roadmaps / upstreaming plans**
- **T-Head PPU (Zhenwu 810 / 810E / M890P)** first-class support roadmap, tagged highest-priority — [#37519](https://github.com/sgl-project/sglang/issues/37519)
- **AMD RDNA3/RDNA4 (gfx1100 / gfx1201)** CI and upstreaming plan for main — [#46900](https://github.com/sgl-project/sglang/issues/46900)
- **AMD Triton Language Extension (TLX)** enablement on gfx950 / gfx1250 — [#40624](https://github.com/sgl-project/sglang/issues/40624)

**Ascend / NPU**
- DSpark compact mode for DeepSeek-V4 on Ascend, replacing uniform verify windows with SPS-policy-driven verification — [PR #38440](https://github.com/sgl-project/sglang/pull/38440)
- Dynamic expert load balancing (`--enable-eplb`) on Ascend, handling NZ expert layouts and valid-sender route counting in NPU DeepEP — [PR #47266](https://github.com/sgl-project/sglang/pull/47266)
- `--enable-mixed-chunk` support on the Ascend DSA sparse attention path (previously `NotImplementedError` for GLM-5.2-class DSA models) — [PR #47367](https://github.com/sgl-project/sglang/pull/47367)

**Intel XPU**
- Full prefill CUDA-graph capture/replay, matching the decode graph contract — [PR #30980](https://github.com/sgl-project/sglang/pull/30980)
- FP8 MoE dispatch routed through `sgl_kernel.fused_experts` for Xe2 W8A16 grouped GEMM — [PR #43994](https://github.com/sgl-project/sglang/pull/43994)

**Models**
- `sensenova_loop` native text/image/video inference — Qwen3.5-derived with shared-weight layer repetition and multi-stream hyper-connections (mHC) — [PR #47281](https://github.com/sgl-project/sglang/pull/47281)
- DFlash-family MoE FFN drafts served from HF MoE config fields (`experts.gate_up_proj` / `experts.down_proj`) — [PR #47181](https://github.com/sgl-project/sglang/pull/47181)

**Hardware caveat:** GLM-5.3-Flash on SM120 (RTX PRO 6000) has *no* working FA4 path — it crashes at CUDA-graph capture on hybrid extend reshape; Triton is the only functional backend ([#42012](https://github.com/sgl-project/sglang/issues/42012)). Separately, the `sgl_kernel` AOT wheel has a per-op cubin gap on sm_75 (`rmsnorm` fails with `cudaErrorNoKernelImageForDevice`, no soft fallback) — [#38515](https://github.com/sgl-project/sglang/issues/38515).

---

## 4. Performance & Optimization

**In-progress PRs with concrete numbers**
- **AMD diffusion prefill**: new Triton dense-attention prefill path for gfx1151. Strided SDPA views cost AOTriton heavily — `attn_fwd` runs 12.4 / 6.7 / 2.06 TF at 4K / 8K / 16K tokens, versus 12.4 → 18 TF with dense operands — [PR #46515](https://github.com/sgl-project/sglang/pull/46515); related materialization fix for long sequences — [PR #46065](https://github.com/sgl-project/sglang/pull/46065)
- **AMD DSA prefill**: reuse AITER work/reduction metadata across transformer layers during EXTEND (metadata is sequence-dependent, not layer-dependent) — [PR #39085](https://github.com/sgl-project/sglang/pull/39085)
- **NPU MiMo-V2.5-Pro**: fold `attention_value_scale` into `o_proj` weights, eliminating a per-layer `aclnnMuls` kernel (~2 ms/layer) in every decode/verify step — [PR #40890](https://github.com/sgl-project/sglang/pull/40890)
- **Rust frontend**: flatten egress logprob columns only for rows carrying them, removing per-step GIL-held work on the scheduler thread — [PR #47106](https://github.com/sgl-project/sglang/pull/47106)
- **Detokenizer**: send per-request invariant decode inputs once instead of every step (decode-id delta duplicating `output_ids`, offsets, `skip_special_tokens`, tokenizer route) — [PR #47260](https://github.com/sgl-project/sglang/pull/47260)
- **Memory**: account for DCP replication when sizing non-DSA EAGLE/MTP draft KV pools (`loc_space_scale = dcp_size` overallocation) — [PR #39799](https://github.com/sgl-project/sglang/pull/39799)
- **Observability**: new `scheduler_iterations_total` metric — [PR #46485](https://github.com/sgl-project/sglang/pull/46485)

**Trackers / regressions**
- Qwen3.8-Flash-Next on gfx950 / MI355X optimization tracker (GatedDeltaNet ×36 layers, QSA ×12, MXFP4 MoE, native NEXTN MTP) — [#44304](https://github.com/sgl-project/sglang/issues/44304)
- FlashInfer `cascade` JIT-compiles inside the first prefixed request (~4.5 s stall) on pip installs — [#46803](https://github.com/sgl-project/sglang/issues/46803)
- HiCache on hybrid GDN: host-tier hit rate collapses to zero once the evicted working set exceeds ~3 long prefixes (host mamba pool exhausted while KV remains host-resident) — [#45400](https://github.com/sgl-project/sglang/issues/45400)
- Attention-FFN disaggregation (AFD) design and staged integration plan — [#45568](https://github.com/sgl-project/sglang/issues/45568)

---

## 5. Stability & Regressions

Ranked by severity:

1. **Scheduler crash, full serving outage** — DeepSeek-V4.1-Flash multimodal path dereferences `items_offset_list = None` under concurrent aborted streams; takes down all TP/EP workers and restarts the container. No fix PR referenced. — [#47218](https://github.com/sgl-project/sglang/issues/47218)
2. **Silent weight corruption in RL / weight-sync workflows** — `update_weights_from_disk` on DFLASH loads target weights into the draft model; the request has no `selector` to prevent it. Same symptom previously reported for EAGLE3 external drafts (THUDM/slime#1177). — [#46806](https://github.com/sgl-project/sglang/issues/46806)
3. **Silent output change on retraction** — a retracted + re-prefilled request resets `frequency_penalty` / `presence_penalty` / `repetition_penalty` accumulators and the `min_new_tokens` counter to zero. Related fix in flight for the PD rebootstrap token-pop path: [PR #47366](https://github.com/sgl-project/sglang/pull/47366). — [#47245](https://github.com/sgl-project/sglang/issues/47245)
4. **SM120 attention backend failure** — FA4 crashes at CUDA-graph capture for GLM-5.3-Flash hybrid extend reshape; Triton is the only usable backend on 8× RTX PRO 6000. — [#42012](https://github.com/sgl-project/sglang/issues/42012)
5. **KV scale loader type corruption** — `LlamaModel.load_kv_cache_scales` assigns Python floats to `k_scale`/`v_scale` without updating `k_scale_float`/`v_scale_float`, breaking both registered-Parameter and tensor-consuming attention paths; host mirrors go stale. — [#47357](https://github.com/sgl-project/sglang/issues/47357)
6. **/unload_lora_adapter permanent deadlock** after parallel-sampling traffic (n > 1) — deterministic repro; closed as inactive, no fix landed. — [#36894](https://github.com/sgl-project/sglang/issues/36894)
7. **Profiler crash** — `/start_profile` with `profile_by_stage=true` and no `num_steps` crashes all schedulers on v0.5.22. — [#47294](https://github.com/sgl-project/sglang/issues/47294)
8. **EAGLE speculative decoding hang** — native Qwen3.5/3.8 MTP stuck in `tree_speculative_sampling_target_only` on Ampere (2× A2, TP=2, 100% GPU util at low power). — [#35822](https://github.com/sgl-project/sglang/issues/35822)
9. **Reasoning grammar gate never opens** when the think-end marker splits under `split_special_tokens`. — [#47205](https://github.com/sgl-project/sglang/issues/47205)
10. **Diffusion model crash** — LingBot-Video-MoE text encoding fails under default layerwise offload (tied vocab table left on CPU → device mismatch). — [#47297](https://github.com/sgl-project/sglang/issues/47297)
11. **CI health** — flaky tests and recurring NVIDIA `PR Test Base` / `PR Test Extra` infrastructure failures, tracked continuously via `/sglang-pr-babysit`. — [#42752](https://github.com/sgl-project/sglang/issues/42752)

---

## 6. What This Means for Application Developers

- **Gate 0.5.22 upgrades on your topology.** The `/start_profile` crash and Python 3.13 Rust-toolchain requirement are cheap to hit in CI. If you serve DeepSeek-V4.1-Flash with multimodal inputs and aborted-stream traffic, treat #47218 as a production-blocking risk — the failure mode is a full engine restart, not a single-request error.
- **Weight-sync pipelines need an audit.** If you do RL or online fine-tuning against a DFLASH/EAGLE draft model, verify you aren't accidentally pushing target weights into the draft. There is currently no `selector`-based guard, so isolation must be enforced at the orchestration layer.
- **Don't trust speculative decoding determinism across retraction.** Requests that get retracted and re-prefilled silently lose their penalty history and `min_new_tokens` floor. For agents relying on `repetition_penalty` or hard output-length constraints, log and validate post-retraction outputs; watch [PR #47366](https://github.com/sgl-project/sglang/pull/47366) for the related token-replay fix.
- **Backend selection is not portable.** On SM120, force Triton for GLM-5.3-Flash; on sm_75 (Turing), expect hard `cudaErrorNoKernelImageForDevice` failures for specific `sgl_kernel` ops rather than graceful torch fallback. Pin known-good backend flags per GPU SKU in your deployment manifests.
- **Cache locality is becoming the primary latency lever.** The Q4 agentic cache-scheduling roadmap ([#43098](https://github.com/sgl-project/sglang/issues/43098)), session-aware KV orchestration ([#45189](https://github.com/sgl-project/sglang/issues/45189)), and HiCache fixes all point the same direction: multi-turn agent workloads will increasingly depend on routing requests to replicas that hold their prefix KV. Design your session IDs and routing keys now so you can exploit it later.
- **Closed-loop simulation is coming for capacity planning** — the simulator RFC ([#44221](https://github.com/sgl-project/sglang/issues/44221)) targets tool-wait-aware workloads, which is the right model for agent traffic rather than fixed request traces.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-30

## 1. Today's Highlights

A heavy backend-optimization day: Metal gains a rewritten MXFP4 mat-vec kernel, SYCL lands a DPAS flash-attention path for quantized KV caches, and Hexagon picks up new UPSCALE and transposed-CONCAT optimizations — all shipped in builds b11978–b11998. Model coverage expanded with Mistral "Le Chonk" (DSA/sdpark) and the Limite 1B "Violetto" architecture, while OpenCL added initial IQ2 quantization support. The main caution flag is a cluster of open SYCL MoE prompt-processing regressions (~28–35% slower) and a silent correctness issue with co-batched hybrid LFM2 sequences.

## 2. Releases & Breaking Changes

- **b11998** — New model: Mistral "Le Chonk", extending DSA and sdpark support to the architecture. [PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376) · [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11998)
- **b11997** — Reuses the tensor staging buffer and skips the direct-io bounce buffer. Operators using `--direct-io` / mmap-disabled loads should re-check peak RSS and load latency; no config change required. [PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321) · [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11997)
- **b11988** — Limite architecture in gguf-py + converter support (Limite 1B "Violetto"). [PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433) · [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11988)
- **b11986** — OpenCL: initial IQ2 support. [PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267) · [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11986)
- **b11985** — Metal: faster MXFP4 mat-vec + full threadgroups for unary/binary/GLU. [PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075) · [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11985)
- **b11982 / b11981** — Hexagon: UPSCALE op and transposed-CONCAT DDR bandwidth work. [PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331), [PR #31277](https://github.com/ggml-org/llama.cpp/pull/31277)
- **b11980 / b11979** — SYCL: DPAS flash attention for quantized KV cache; vectorized reordered-layout dequant stores. [PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125), [PR #30976](https://github.com/ggml-org/llama.cpp/pull/30976)
- **b11978** — Test fix: `comp_padding` CUDA graph reservation. [PR #31344](https://github.com/ggml-org/llama.cpp/pull/31344)

No public API or CLI breaking changes in this window. Pending non-breaking vendor bump: cpp-httplib 0.64.0 ([PR #31400](https://github.com/ggml-org/llama.cpp/pull/31400)).

## 3. New Model & Hardware Support

- **Mistral "Le Chonk"** — new architecture wired through the DSA/sdpark paths ([PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376)).
- **Limite 1B "Violetto"** — new `Limite` GGUF arch plus converter support, so third-party quant pipelines can target it ([PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433)).
- **OpenCL IQ2 (`iq2_s`)** — AOS/SOA mat-vec, tiled mat-mul, no-shuffle GEMM/GEMV and dp4a paths tuned for Adreno ([PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267)).
- **SYCL quantized-KV flash attention** — q4_0 and q8_0 KV blocks now attended directly via DPAS for decode and small batches (1–31 query tokens) instead of scalar dequant ([PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125)).
- **Hexagon** — UPSCALE op with optimized nearest 2x/4x and bilinear 4x, non-contiguous row and dimension-collapse handling ([PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331)); DSV4_HC_PRE/POST ops in flight ([PR #30953](https://github.com/ggml-org/llama.cpp/pull/30953)).
- **Metal** — `GET_ROWS_BACK` (F32) kernel proposed ([PR #31378](https://github.com/ggml-org/llama.cpp/pull/31378)).
- **Multimodal fixes** — InternVL CLS token ordering corrected (patch 0 was receiving the CLS position embedding) ([PR #30886](https://github.com/ggml-org/llama.cpp/pull/30886)); Gemma 4 image resize switched to `PAD_NONE` to match HF preprocessing ([PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)).
- **Metal build compatibility** — the macOS 14 / M2 Ultra init failure from a `constexpr bfloat` cast in `unary.metal` is closed ([#31272](https://github.com/ggml-org/llama.cpp/issues/31272)).

## 4. Performance & Optimization

- **Metal MXFP4 mat-vec rewrite** — fp4 values decoded with an fp16 bit trick instead of a threadgroup lookup table, and packed nibbles loaded as aligned 32-bit words; kernel no longer uses threadgroup scratch for the LUT. Unary/binary/GLU kernels moved to full threadgroups ([PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)).
- **SYCL DPAS FA for quantized KV** — removes per-token scalar dequant/conversion in decode, the dominant cost for q4_0/q8_0 KV caches at short batch ([PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125)).
- **SYCL dequant stores widened** — reordered-layout Q4_0 previously did two scalar stores per `qs` byte, strided 16 elements; now vectorized. Same treatment for K-quants, IQ3 and MXFP4 ([PR #30976](https://github.com/ggml-org/llama.cpp/pull/30976)).
- **Hexagon transposed CONCAT** — doubled row block size and scratchpad double-buffering, chunked `src1` rows, padded VTCM stride to spread gather lanes, targeting DDR bandwidth utilization ([PR #31277](https://github.com/ggml-org/llama.cpp/pull/31277)).
- **Load path** — staging-buffer reuse plus direct-io bounce-buffer elimination reduces copy overhead and transient memory during model load ([PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321)).
- **CPU tiled FA with q8_0 KV (proposed)** — currently only f32/f16 KV routes into the tiled kernel; `-ctk q8_0 -ctv q8_0` falls back to the reference loop that dequantizes V once per (q row, kv token) ([PR #31399](https://github.com/ggml-org/llama.cpp/pull/31399)).
- **x86 vec_dot accumulator unrolling** — Q4_0/Q4_K/Q5_K/IQ3_S/IQ3_XXS quantized dot-product chains, aimed at token-generation latency ([PR #31061](https://github.com/ggml-org/llama.cpp/pull/31061), closed).
- **CUDA Hadamard rotation fusion** — merges sign-multiply + Hadamard transform into one kernel ahead of the quantize-to-8-bit mat-vec for prism.hadamard models ([PR #31398](https://github.com/ggml-org/llama.cpp/pull/31398)).
- **OpenCL dp4a q4_0 GEMM as default** — the f16 GEMM rounds activations to half precision, costing ~3% perplexity on gemma-4-E2B-Q4_0 vs CPU; the q8_1 dp4a path is used wherever integer dot product is available ([PR #30535](https://github.com/ggml-org/llama.cpp/pull/30535)). Small-N f32/f16 GEMV now reads each weight row once instead of padding to a 64-wide tile ([PR #30413](https://github.com/ggml-org/llama.cpp/pull/30413)).
- **MoE cache** — `--moe-cache-cpu-share auto` with CRF-based admission for CPU misses proposed ([PR #31286](https://github.com/ggml-org/llama.cpp/pull/31286)).

## 5. Stability & Regressions

Ranked by severity:

1. **[OPEN] Silent correctness: hybrid LFM2/LFM2.5 co-batching** — decoding more than one sequence in the same ubatch corrupts recurrent/short-conv state; greedy output diverges and can truncate mid-sentence. This is the highest-risk item for server deployments with `-np > 1` on hybrid models. No fix PR yet. [#29002](https://github.com/ggml-org/llama.cpp/issues/29002)
2. **[OPEN] SYCL MoE prompt-processing regression, ~28–35%** — `llama-server` prompt processing on `qwen35moe` with `-ncmoe` CPU-offloaded experts is ~35% slower in b11804 vs b11046, and not reproduced by `llama-bench`; a second report pins the XMX-ESIMD path regression to `d81d0d8fb` on Arc Pro B70. Benchmarks that don't exercise the server path will miss this. [#31262](https://github.com/ggml-org/llama.cpp/issues/31262) · [#31068](https://github.com/ggml-org/llama.cpp/issues/31068)
3. **[OPEN] DeepSeek-V4.1-Flash-1017 GGUF issues** — reported against build 11997 on CUDA with dual EPYC + RTX 6000 Pro. [#31385](https://github.com/ggml-org/llama.cpp/issues/31385)
4. **[OPEN] `qwen4exp` livelock on HIP during model load** — identical config loads on Vulkan. [#29149](https://github.com/ggml-org/llama.cpp/issues/29149)
5. **[OPEN] Intermittent `SOLVE_TRI` memory fault on gfx1200** — HIP/ROCm, reproducible under `test-backend-ops`. [#31108](https://github.com/ggml-org/llama.cpp/issues/31108)
6. **[OPEN] SYCL quantized CPY mismatch vs CPU** — `CPY` to q4_1/q5_1/q8_0 diverges from the CPU reference. [#31140](https://github.com/ggml-org/llama.cpp/issues/31140)
7. **[OPEN] SYCL `test-backend-ops` failures** — CONV_2D, CPY, ROLL, FLASH_ATTN_EXT. The FA branch has a candidate fix: gemma-interleave heuristic and quantized K/V staging blocks in `fattn-mkl.cpp` / `fattn-onednn.cpp`. [#28384](https://github.com/ggml-org/llama.cpp/issues/28384) · [PR #30761](https://github.com/ggml-org/llama.cpp/pull/30761)
8. **[OPEN] Prefix-cache invalidation on Qwen3.5+ templates** — reasoning/content whitespace isn't round-trippable, causing full re-prefill on hybrid models. Direct cost impact for agent workloads. [#30357](https://github.com/ggml-org/llama.cpp/issues/30357)
9. **[OPEN] `json-schema-to-grammar` emits raw control chars** — e.g. NUL from `"\u0000"` inside `pattern`, producing "failed to parse grammar". [#31199](https://github.com/ggml-org/llama.cpp/issues/31199)
10. **[OPEN] `response_format` + tools → 400** — the coder parser builds only the JSON-schema rule when `response_format` is set, so the tool-call branch is unreachable. Fix proposed: don't use lazy grammar when `response_format` is set ([PR #31395](https://github.com/ggml-org/llama.cpp/pull/31395)). [#31342](https://github.com/ggml-org/llama.cpp/issues/31342)
11. **[CLOSED] LFM3 `response_format` 500** — "Unexpected empty grammar stack" with `--reasoning-budget 0` + `enable_thinking: false`; fixed by ending reasoning-budget forcing on an end tag from the prompt ([PR #31374](https://github.com/ggml-org/llama.cpp/pull/31374)). [#31328](https://github.com/ggml-org/llama.cpp/issues/31328)
12. **[CLOSED] OpenCL FA softcap ordering** — mask was applied before the logit softcap, so masked entries were capped to `-softcap` and still contributed to softmax ([PR #30536](https://github.com/ggml-org/llama.cpp/pull/30536)). Also closed: unaligned MoE router ID sub-buffer abort with `CL_MISALIGNED_SUB_BUFFER_OFFSET` ([PR #30537](https://github.com/ggml-org/llama.cpp/pull/30537)).
13. **[OPEN] `json-schema-to-grammar` drops length bounds with pattern** — `minLength`/`maxLength` silently ignored when `pattern` is present ([PR #31358](https://github.com/ggml-org/llama.cpp/pull/31358)).
14. **[OPEN] CUDA arm64 Blackwell throughput regression** — between official builds b10655 and b11140, both prompt processing and generation. [#29341](https://github.com/ggml-org/llama.cpp/issues/29341)
15. **[OPEN] gemma3n/gemma4 duplicate `token_embd` allocation** — second copy allocated when loading without mmap (`-lm none`, `-lm mlock`) with tied input/output embeddings; fix proposed to create `token_embd` before the tied output ([PR #31394](https://github.com/ggml-org/llama.cpp/pull/31394)).
16. **[CLOSED] RPC single-threaded accept loop** starved all connections after the first — relevant for multi-host router setups. [#28908](https://github.com/ggml-org/llama.cpp/issues/28908)

## 6. What This Means for Application Developers

- **Don't run hybrid models with `-np > 1` in production yet.** #29002 is silent corruption, not a crash — your evals may pass on single-sequence runs and fail in a multi-tenant server. Cap parallelism at 1 for LFM2/LFM2.5 until resolved.
- **Grammar-constrained decoding has two live bugs.** If you rely on `response_format: json_schema` together with tool calling, expect 400s on some chat templates; #31395 is the PR to watch. Schemas whose `pattern` contains escaped control characters can fail grammar construction entirely (#31199). Validate your schema corpus against the current build rather than assuming prior behavior.
- **On SYCL, benchmark through `llama-server`, not `llama-bench`.** The ~35% MoE prompt-processing regression (#31262) is server-path-only and invisible to the standard bench harness. If you pin Intel builds, hold at b11046 or bisect past `d81d0d8fb`.
- **Quantized KV caches just got materially better on SYCL.** The DPAS FA kernel (#31125) makes `-ctk q8_0 -ctv q8_0` viable for decode on Intel GPUs, which is the single biggest lever for context-length-per-GPU. On CPU, q8_0 KV still falls back to the slow reference loop until #31399 lands.
- **Metal/MXFP4 and OpenCL/IQ2 widen the low-bit deployment envelope** on Apple Silicon and Adreno respectively, but OpenCL q4_0 now defaults to dp4a for quality reasons (#30535) — re-baseline perplexity if you were calibrated against f16-activation behavior.
- **Prefix-cache economics are still fragile** on Qwen3.5+ (#30357). For agent loops with long system prompts, measure actual re-prefill rates rather than assuming cache hits; whitespace emitted by the model can throw away your entire prefix.
- **Operational asks are accumulating, not shipping:** model cache list/delete ([#16393](https://github.com/ggml-org/llama.cpp/issues/16393)), independent `-ns`/`-np` control ([#22921](https://github.com/ggml-org/llama.cpp/issues/22921)), idle router worker termination ([#25570](https://github.com/ggml-org/llama.cpp/issues/25570)), and a model ADMIN API ([#30807](https://github.com/ggml-org/llama.cpp/issues/30807)) are all still open — plan around them rather than for them.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-10-30

## Today's Highlights
No new Ollama release landed in the last 24 hours; activity centered on the MLX backend and OpenAI-compatibility correctness. MLX work advanced speculative-decoding telemetry, concurrent generation, load-stall diagnostics, and repeat-loop aborts, while several OpenAI-compat bugs around reasoning/content arrays and empty responses received fix PRs. The most serious open item remains a ROCm gfx1151 KV-state bleed on Strix Halo that contaminates sequential responses.

## Releases & Breaking Changes
None. No releases published and no landed API/config migrations in the period.

## New Model & Hardware Support
- No new architecture or backend landed in this window. Active/closed model-support requests:
  - Streamed native audio answers from LFM3-Omni via `/api/chat` and `ollama run --audio` — [ollama/ollama#19396](https://github.com/ollama/ollama/issues/19396) (open)
  - Native audio/video input and audio output on Windows for models like `lfm3-omni` and `phonon` — [ollama/ollama#19390](https://github.com/ollama/ollama/issues/19390) (open)
  - `/api/chat` silently ignores the `audio` field for models advertising the `audio` capability; audio currently only works via `images` — [ollama/ollama#19378](https://github.com/ollama/ollama/issues/19378) (open)
  - Related PR: reject clips the model has no encoder for instead of passing them to the projector — [ollama/ollama#19397](https://github.com/ollama/ollama/pull/19397) (open)
  - Multimodal embedding media ordering fix for models like Gemma 4 — [ollama/ollama#19334](https://github.com/ollama/ollama/pull/19334) (open)
  - Ternary quantization for Bonsai 2 — [ollama/ollama#19388](https://github.com/ollama/ollama/issues/19388) (closed); Korean TTS support for phonon — [ollama/ollama#19295](https://github.com/ollama/ollama/issues/19295) (closed)
- GGUF loader: MXFP4 large-MoE metadata overflow on `ffn_down_exps.weight` for `gpt-oss:120b`/`latest` — [ollama/ollama#19375](https://github.com/ollama/ollama/issues/19375) and [ollama/ollama#18869](https://github.com/ollama/ollama/issues/18869) (both closed)
- Hardware-specific reports: ROCm Strix Halo gfx1151 KV bleed — [ollama/ollama#17847](https://github.com/ollama/ollama/issues/17847) (open); Windows CUDA RTX 5070 Ti warmup failure — [ollama/ollama#17380](https://github.com/ollama/ollama/issues/17380) (open)

## Performance & Optimization
- Speculative decoding: per-request metrics for drafted/accepted tokens and acceptance rate, exposed in API responses and `ollama run --verbose` — [ollama/ollama#19368](https://github.com/ollama/ollama/pull/19368) (open). MLX adds `draft_num_predict=0` to disable drafting for benchmark comparison; default remains 4 — [ollama/ollama#19391](https://github.com/ollama/ollama/pull/19391) (open).
- MLX top-k: chunked 2048-value sorting avoids the multi-pass Metal merge sort for rows >2048, targeting top-k over a 248K-token vocabulary — [ollama/ollama#19392](https://github.com/ollama/ollama/pull/19392) (closed).
- MLX concurrency: `OLLAMA_NUM_PARALLEL` support for MLX generation, sharing projection/feed-forward work while keeping attention per request — [ollama/ollama#19197](https://github.com/ollama/ollama/pull/19197) (open).
- MLX load diagnostics: progress-based stall detection and load-progress reporting restored for lazy weight loading — [ollama/ollama#17834](https://github.com/ollama/ollama/pull/17834) (open).
- Memory accounting: llama-server buffer dedupe fix for multi-model loads; a 30B dflash model was misreported as ~2.3GB in `ollama ps` and scheduler VRAM accounting — [ollama/ollama#17857](https://github.com/ollama/ollama/pull/17857) (open).
- VRAM prediction: exclude CPU-resident input tensors from GGUF-based weight estimates — [ollama/ollama#19393](https://github.com/ollama/ollama/pull/19393) (open); avoid evicting CPU-only runners to satisfy GPU memory predictions — [ollama/ollama#19284](https://github.com/ollama/ollama/pull/19284) (open).
- Chat truncation: preserve the most recent user message during context-limit truncation in multi-step tool loops — [ollama/ollama#18697](https://github.com/ollama/ollama/pull/18697) (open).
- CLI latency: `ollama ls` still takes ~5s on an interactive PTY vs ~14ms when piped, due to a pselect6 timeout — [ollama/ollama#15597](https://github.com/ollama/ollama/issues/15597) (open).

## Stability & Regressions
Ranked by severity:

1. **Critical — ROCm KV-state contamination.** On `0.32.14-rocm` / Strix Halo gfx1151, responses are contaminated by the previous unrelated request in A/B prompt tests. No fix PR in the dataset. — [ollama/ollama#17847](https://github.com/ollama/ollama/issues/17847)
2. **High — CUDA warmup failure with silent CPU fallback.** Intermittent `CUDA error: shared object initialization failed` on Windows/RTX 5070 Ti with `gpt-oss:20b`, correlated with silent `CUDA_Host`→CPU pinned-buffer fallback. No fix PR. — [ollama/ollama#17380](https://github.com/ollama/ollama/issues/17380)
3. **High — GGUF MXFP4 MoE overflow.** `read GGUF metadata ... unsupported tensor "blk.0.ffn_down_exps.weight" size overflows` on `gpt-oss:120b`. Both reports closed, but no linked fix PR in this dataset. — [ollama/ollama#19375](https://github.com/ollama/ollama/issues/19375), [ollama/ollama#18869](https://github.com/ollama/ollama/issues/18869)
4. **High — Tool-call turn dropped.** `go_template` silently drops the whole assistant turn for qwen3 tool calls (empty content, `tool_calls: null`, `eval_count > 0`); llama.cpp template parses the same generation correctly. Closed. — [ollama/ollama#19377](https://github.com/ollama/ollama/issues/19377)
5. **Medium — MLX tool calling.** `qwen3.8:27b-mlx` emits `tool_calls` only when assistant `content` is empty; a free-text preamble suppresses tool calls. Open. — [ollama/ollama#19353](https://github.com/ollama/ollama/issues/19353)
6. **Medium — OpenAI reasoning loss.** Assistant `reasoning` is dropped, and reasoning-only turns deleted, when `content` is a content-parts array. Fix PR open. — [ollama/ollama#19380](https://github.com/ollama/ollama/issues/19380), [ollama/ollama#19384](https://github.com/ollama/ollama/pull/19384)
7. **Medium — OpenAI `/v1/models` null data.** Returns `"data": null` instead of `[]` with zero models; strongly typed SDKs fail to parse. Open, no direct fix PR in the dataset. — [ollama/ollama#19381](https://github.com/ollama/ollama/issues/19381)
8. **Medium — Audio silently ignored.** `/api/chat` drops the `audio` field for audio-capable models; related encoder-validation PR open. — [ollama/ollama#19378](https://github.com/ollama/ollama/issues/19378), [ollama/ollama#19397](https://github.com/ollama/ollama/pull/19397)
9. **Medium — Parallelism semantics.** `OLLAMA_NUM_PARALLEL=1` does not prevent two concurrent sessions from decoding simultaneously. Open. — [ollama/ollama#19262](https://github.com/ollama/ollama/issues/19262)
10. **Low — Agent compaction.** Claude Code long-run auto-compaction fails with `deepseek-v4.1-flash:1017-cloud`, exhausting context. Open. — [ollama/ollama#19249](https://github.com/ollama/ollama/issues/19249)
11. **Low — MLX repeat loop.** MLX can stream the same token to the generation limit; PR aborts after 101 identical tokens. — [ollama/ollama#19398](https://github.com/ollama/ollama/pull/19398)
12. **Low — OpenAI empty responses.** Empty embedding responses serialize `data` as `null`; fix PR returns an array. — [ollama/ollama#19387](https://github.com/ollama/ollama/pull/19387)

## What This Means for Application Developers
- **OpenAI-compat clients should be defensive.** Handle `data: null` from `/v1/models` and avoid relying on assistant `reasoning` surviving content-parts round-trips until [ollama/ollama#19384](https://github.com/ollama/ollama/pull/19384) lands. Invalid content types alongside `tool_calls` are also being tightened — [ollama/ollama#19389](https://github.com/ollama/ollama/pull/19389).
- **Agent/tool-loop builders must validate turns.** The `go_template` bug ([ollama/ollama#19377](https://github.com/ollama/ollama/issues/19377)) and MLX qwen3 behavior ([ollama/ollama#19353](https://github.com/ollama/ollama/issues/19353)) can silently drop or suppress tool calls. The truncation fix ([ollama/ollama#18697](https://github.com/ollama/ollama/pull/18697)) is important for long multi-step loops.
- **Audio/multimodal is still inconsistent.** Native `audio` in `/api/chat` is not reliable; audio may only work via `images`, and #19397 will start rejecting clips the model cannot encode. Check model capabilities explicitly before sending media.
- **Pin model digests for production.** Model updates can change output quality, and rollback is not a first-class workflow — see the Qwen3:4b restore request — [ollama/ollama#19385](https://github.com/ollama/ollama/issues/19385).
- **MLX/Apple Silicon is getting more tunable.** Expect concurrency controls (`OLLAMA_NUM_PARALLEL`) and speculative-decoding knobs; use `draft_num_predict=0` for clean benchmark baselines once merged.
- **Leave memory headroom on mixed CPU/GPU hosts.** Multi-model and speculative-draft VRAM accounting fixes are in flight; current `ollama ps`/scheduler numbers may misreport, especially with draft models and CPU-resident input layers.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-30

## 1. Today's Highlights

LiteLLM published **v1.108.0-rc.1** with cosign-signed Docker images, while activity remains very high: 93 issues and 486 PRs updated in the last 24 hours. The dominant theme is **cost and spend accounting correctness**, with multiple provider-reported `usage.cost` bugs closed but several streaming-billing issues still open. Performance and architecture work is focused on reducing per-chunk streaming overhead, improving routing fairness, and extracting database/Redis access behind typed repository and transport layers.

## 2. Releases & Breaking Changes

- **v1.108.0-rc.1** released. Docker images are signed with cosign using the key introduced in commit [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065). No other migration notes were included in the provided release data.
- **Pending breaking change — realtime audio billing:** [PR #35600](https://github.com/BerriAI/litellm/pull/35600) adds OpenAI/Azure realtime translation and streamed `gpt-transcribe`, and changes per-second transcription models such as Deepgram, Groq Whisper, and Vertex Chirp to bill per second instead of at `$0`. This is marked breaking and is still open.
- **MCP auth backport for RC:** [PR #49167](https://github.com/BerriAI/litellm/pull/49167) backports #48939 to `rc/1.108.0` to restore upstream-token admission for legacy delegate MCP servers; it also bumps `ddtrace` from 4.11.0 to 4.15.6. Relevant if you run legacy MCP delegates on the RC.

## 3. New Model & Hardware Support

- **CLF AI Gateway provider:** [PR #39324](https://github.com/BerriAI/litellm/pull/39324) adds an OpenAI-compatible gateway provider with ten priced open-weight models, routed as `clf_ai_gateway/<model>`.
- **Z.AI / GLM:** [PR #48335](https://github.com/BerriAI/litellm/pull/48335) adds `response_format` and `max_completion_tokens` support for Z.AI models and pricing for GLM-5V-Turbo, including cached input.
- **Azure AI Foundry Agents v2:** [Issue #25372](https://github.com/BerriAI/litellm/issues/25372) was closed, covering Responses API support with `agent_reference` for Foundry agents.
- **Vertex Claude batch:** [PR #45715](https://github.com/BerriAI/litellm/pull/45715) routes Vertex Claude batches through Anthropic’s native Messages row shape instead of 404ing at create.
- **DeepSeek V4 reasoning effort:** [Issue #27439](https://github.com/BerriAI/litellm/issues/27439) was closed; `reasoning_effort` values such as `high` and `max` are no longer stripped for DeepSeek V4.
- **OpenAI realtime audio:** [PR #35600](https://github.com/BerriAI/litellm/pull/35600) adds realtime translation and streamed transcription on OpenAI and Azure.
- **Requested but not landed:** native support for Kimi K3, Inkling, and Tinker ([Issue #33921](https://github.com/BerriAI/litellm/issues/33921)); Soniox realtime STT through the proxy ([Issue #43677](https://github.com/BerriAI/litellm/issues/43677)).

## 4. Performance & Optimization

- **Streaming CPU overhead:** [PR #49166](https://github.com/BerriAI/litellm/pull/49166) removes repeated raised-and-caught `AttributeError`s inside pydantic on every streamed chunk. The PR reports roughly a dozen Python exceptions per chunk on OpenAI/Azure streams, so this should reduce per-chunk CPU cost.
- **Least-busy routing fairness:** [PR #47484](https://github.com/BerriAI/litellm/pull/47484) reserves the selected deployment immediately so concurrent requests do not all read the same in-flight count and pile onto one deployment.
- **OTPM reservation fix:** [PR #49054](https://github.com/BerriAI/litellm/pull/49054) caps default output-token-per-minute reservation at a quarter of `otpm` or the deployment cap, and reconciles token counters after each call. This avoids blanket 429s when model max output exceeds deployment `otpm`.
- **Cross-API bridge refactor:** [PR #46707](https://github.com/BerriAI/litellm/pull/46707) makes format bridges call native executors instead of re-entering `litellm.acompletion` / `aresponses`, avoiding duplicate hooks, logging, and retry paths per request.
- **Database and Redis layering:** A series of refactors adds per-repository FastAPI dependencies ([#48507](https://github.com/BerriAI/litellm/pull/48507), [#48592](https://github.com/BerriAI/litellm/pull/48592), [#49116](https://github.com/BerriAI/litellm/pull/49116)), a single database lifespan owner ([#48506](https://github.com/BerriAI/litellm/pull/48506)), and a typed Redis transport/store layer ([#48958](https://github.com/BerriAI/litellm/pull/48958), [#49162](https://github.com/BerriAI/litellm/pull/49162), [#49163](https://github.com/BerriAI/litellm/pull/49163), [#48956](https://github.com/BerriAI/litellm/pull/48956)). These are largely non-behavioral today but reduce connection-pool coupling and prepare for a Rust-backed Redis path.

## 5. Stability & Regressions

**Critical — billing and spend accounting**

- **Streaming traffic logged at $0:** [Issue #48937](https://github.com/BerriAI/litellm/issues/48937) — streamed chat completions are recorded with `spend = 0` when upstream reports `usage.cost: 0` alongside real cost elsewhere. Open; no fix PR shown.
- **Non-streaming provider cost ignored:** [Issue #47533](https://github.com/BerriAI/litellm/issues/47533) — provider-reported `usage.cost` ignored for OpenAI-compatible upstreams on non-streaming calls; closed.
- **Phantom spend from unvalidated cost units:** [Issue #35829](https://github.com/BerriAI/litellm/issues/35829) — provider `usage.cost` trusted verbatim as USD with no unit validation, causing astronomical spend; closed.
- **OpenRouter streaming cost loss:** [Issue #16021](https://github.com/BerriAI/litellm/issues/16021) — OpenRouter streaming no longer preserves `usage.cost`; closed.
- **Merged `n>1` streaming billing:** [Issue #38180](https://github.com/BerriAI/litellm/issues/38180) — parallel streamed completions logged and billed as one merged completion; closed.
- **Spend buffers dropped on shutdown:** [Issue #34805](https://github.com/BerriAI/litellm/issues/34805) — in-memory key/team/user/end-user spend buffers silently dropped during proxy shutdown. Open.
- **Broken budget reset without Redis:** [Issue #37261](https://github.com/BerriAI/litellm/issues/37261) — `provider_budget_config` reports `budget_reset_at` roughly 57 years ahead without Redis. Open.

**High — tool calling and protocol correctness**

- **Claude tool calling broken:** [Issue #19858](https://github.com/BerriAI/litellm/issues/19858) — Claude models via OpenRouter/Vertex with tool calling fail in LiteLLM proxy setups. Open.
- **Gemini streaming tool context corruption:** [Issue #25869](https://github.com/BerriAI/litellm/issues/25869) — `stream_chunk_builder` corrupts Gemini `server_side_tool_invocations` and `thought_signatures`, causing `400 Corrupted tool call context` on follow-up turns. Open and stale.
- **MCP legacy delegate auth regression:** [PR #49167](https://github.com/BerriAI/litellm/pull/49167) is the pending RC fix for legacy delegate MCP servers returning 401 to upstream-only callers, regressed in v1.103.0 by #40923.

**Medium — proxy and provider edge cases**

- **Cursor endpoint not logged:** [Issue #30126](https://github.com/BerriAI/litellm/issues/30126) — `/cursor/chat/completions` returns 200 and charges the provider but creates no SpendLogs/UI entry. Open.
- **Azure AI prompt-cache dialect:** [PR #45295](https://github.com/BerriAI/litellm/pull/45295) fixes chat prompt-cache format selection for `azure_ai/` deployments when `AZURE_AI_API_BASE` points at a different resource.
- **Wildcard health checks:** [PR #49060](https://github.com/BerriAI/litellm/pull/49060) probes only models a partial wildcard actually serves rather than the provider’s cheapest unrelated chat model.
- **Closed regressions:** Bedrock audio transcription 400s ([#41544](https://github.com/BerriAI/litellm/issues/41544)), valid API keys rejected after 1.82.3 ([#24680](https://github.com/BerriAI/litellm/issues/24680)), `/v2/user/info` omitting user RPM/TPM limits ([#33347](https://github.com/BerriAI/litellm/issues/33347)), Redis semantic cache failures ([#31610](https://github.com/BerriAI/litellm/issues/31610)), child-process death with multiple workers ([#18457](https://github.com/BerriAI/litellm/issues/18457)), module-import HTTP GET ([#22159](https://github.com/BerriAI/litellm/issues/22159)), and Claude empty-content sanitization ([#24498](https://github.com/BerriAI/litellm/issues/24498)).

## 6. What This Means for Application Developers

- **Do not trust streaming spend numbers yet without validation.** Several billing paths still mishandle provider-reported cost, especially streaming and OpenAI-compatible upstreams. If you rely on LiteLLM budgets or spend logs for chargeback or hard limits, audit streaming traffic against provider invoices before enforcing quotas.
- **Treat `v1.108.0-rc.1` as an RC.** Verify the cosign signature in production pipelines, and note that the MCP legacy-delegate auth fix is a pending backport ([#49167](https://github.com/BerriAI/litellm/pull/49167)) rather than confirmed in the RC artifact.
- **Avoid relying on Claude tool calling or Gemini streaming tool follow-ups in critical paths** until [#19858](https://github.com/BerriAI/litellm/issues/19858) and [#25869](https://github.com/BerriAI/litellm/issues/25869) are resolved.
- **Realtime audio billing will change.** If you use per-second STT providers, the pending realtime PR changes them from `$0` to per-second billing. Model your costs before adopting that release.
- **Provider coverage is expanding:** Z.AI/GLM, CLF AI Gateway, Azure Foundry Agents v2, Vertex Claude batches, and DeepSeek V4 reasoning controls reduce the need for custom adapters, but several are recent or still open PRs—pin versions and test provider-specific request/response shapes.
- **Bursty workloads should benefit** from least-busy reservation and OTPM reconciliation once landed, reducing 429 storms and deployment hot-spotting in high-concurrency agent workloads.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-30

## 1. Today's Highlights
No new releases landed in the last 24 hours, but the project is shipping fast in two directions: a fused batch-1 NF4 decode path that claims **1.6–2.3× per-token speedup** for 4-bit Llama ([PR #16032](https://github.com/unslothai/unsloth/pull/16032)), and a pair of fixes for Gemma 3 flex-attention fallback on Ampere/Ada-class cards ([PR #16041](https://github.com/unslothai/unsloth/pull/16041), [PR #16049](https://github.com/unslothai/unsloth/pull/16049)). Meanwhile, open regressions around context compaction in long chats ([Issue #15800](https://github.com/unslothai/unsloth/issues/15800)) and Windows/AMD commit-charge exhaustion ([Issue #16015](https://github.com/unslothai/unsloth/issues/16015)) are the main stability concerns for Studio users.

## 2. Releases & Breaking Changes
None in the last 24h. Note that Studio users are on `0.1.911-beta` / `0.1.912-beta`, and several reports today are update-related ([Issue #15866](https://github.com/unslothai/unsloth/issues/15866), [Issue #15932](https://github.com/unslothai/unsloth/issues/15932)) — pin a known-good beta if you depend on it.

## 3. New Model & Hardware Support
- **LFM3 family (1.7B)** requested; the new LiquidAI backbone is not yet supported ([Issue #15588](https://github.com/unslothai/unsloth/issues/15588), open, 10 👍).
- **EXL3 (ExLlamaV3) quantization backend** proposed as a bitsandbytes alternative: 2/3/4/6/8-bit and fractional bitrates, plus MoE quantization ([PR #7115](https://github.com/unslothai/unsloth/pull/7115), open/in progress).
- **Multiple resident GGUF models** in Studio, each in its own `llama-server` process, with model-aware request routing ([PR #10876](https://github.com/unslothai/unsloth/pull/10876), merged/closed).
- **EXL3 and AMD iGPU handling**: Studio now excludes integrated AMD graphics from Train-page VRAM totals and default GPU selection, which unblocks dual-R9700 setups ([PR #14039](https://github.com/unslothai/unsloth/pull/14039)).
- **Qwen3-ASR voice context setting** (Auto / 1K–64K) reduces per-recording allocation ([PR #15810](https://github.com/unslothai/unsloth/pull/15810)).
- **VibeVoice q4-ios GGUF removed** from Studio offerings because the bundled `audio.cpp` runtime cannot load it ([PR #16048](https://github.com/unslothai/unsloth/pull/16048)).

## 4. Performance & Optimization
- **Fused batch-1 NF4 decode for Llama**: `UNSLOTH_DECODE_FUSED_NF4=1` collapses ~12 kernel launches per decoder layer (~27–33 with LoRA) into fewer; reported **1.6–2.3× per token** in eager `generate`. Opt-in; default path unchanged ([PR #16032](https://github.com/unslothai/unsloth/pull/16032)).
- **Gemma 3 flex attention**: short prompts (17–127 tokens) could poison the process into eager `math_attention` on sm86/sm89 (L4, A10, RTX 3090/4090, A6000, L40S). Small shared-memory tiles for short no-grad calls keep compiled flex attention active ([PR #16049](https://github.com/unslothai/unsloth/pull/16049)); companion change routes generation through the non-flex path when fallback occurs ([PR #16041](https://github.com/unslothai/unsloth/pull/16041)). Concrete result cited: L4 / Gemma 3 1B / 16k context improved.
- **ASR memory**: Qwen3-ASR on an 11-second clip drops from ~8.7 GiB at 64K context to **~2.0 GiB at 1K** with the new voice-context cap ([PR #15810](https://github.com/unslothai/unsloth/pull/15810)).
- **MLX memory estimation** now prices attention score matrices for full-width f16 KV cache, where MLX actually materializes them ([PR #16021](https://github.com/unslothai/unsloth/pull/16021)); this addresses the wildly inflated estimate reported in [Issue #15935](https://github.com/unslothai/unsloth/issues/15935) (69.7 GiB for a model that loads on a 24 GB Mac).
- **Hybrid token counting** for GGUF chats: dense estimate guard plus scaled read timeout to avoid ~2-minute stalls before "Message too long" ([PR #15857](https://github.com/unslothai/unsloth/pull/15857)).

## 5. Stability & Regressions
Ranked by severity:

1. **Context never compacts in returning long conversations** — chats become non-continuable ([Issue #15800](https://github.com/unslothai/unsloth/issues/15800), open). Fix in progress via hybrid token counting ([PR #15857](https://github.com/unslothai/unsloth/pull/15857)).
2. **Truncated tool calls become valid-looking calls** — when a local model hits `max_tokens` mid tool call, the Anthropic-compatible endpoint still emits a complete, valid-argument tool call; coding agents will execute partial writes ([PR #16035](https://github.com/unslothai/unsloth/pull/16035), open — fix proposed).
3. **Windows + AMD commit-charge exhaustion** — `llama-server.exe` retains ~16 GB commit after GGUF load even when weights fit in VRAM, exhausting the page file on a 32 GB machine ([Issue #16015](https://github.com/unslothai/unsloth/issues/16015)). Mitigation PR warns on load ([PR #16046](https://github.com/unslothai/unsloth/pull/16046)).
4. **Core dumps after recent updates** — models stop responding after a few turns regardless of KV usage, and ejecting models core-dumps ([Issue #14411](https://github.com/unslothai/unsloth/issues/14411), closed).
5. **Fine-tuning data handling bugs** — "Train on completions" silently drops 80%+ of Alpaca datasets because the Alpaca template is never installed while masking looks for its markers ([Issue #15894](https://github.com/unslothai/unsloth/issues/15894), closed; duplicate [Issue #15895](https://github.com/unslothai/unsloth/issues/15895) open). Single-column `text`/`content` JSONL cannot be trained at all ([Issue #15892](https://github.com/unslothai/unsloth/issues/15892), [Issue #15901](https://github.com/unslothai/unsloth/issues/15901), closed).
6. **Anthropic passthrough ignores auto-compaction** — `/v1/messages` with oversized history returns unrecoverable 400 `exceed_context_size_error` ([Issue #15417](https://github.com/unslothai/unsloth/issues/15417), closed).
7. **MLX/GGUF model listing gaps** — LFM3 MLX models absent from `curl /v1/models` ([Issue #15832](https://github.com/unslothai/unsloth/issues/15832)); MiniCPM-V-4.7-1B Vulkan backend text generation fails ([Issue #14987](https://github.com/unslothai/unsloth/issues/14987), closed).
8. **Download race**: manifest planned from `model_info` but `snapshot_download` re-resolves `main`, so a moving branch can fetch a mismatched snapshot ([PR #16047](https://github.com/unslothai/unsloth/pull/16047), open fix).
9. **Other**: AppImage UI lag ([Issue #15999](https://github.com/unslothai/unsloth/issues/15999)), managed accounts blocked from local/loopback MCP servers despite the owner toggle ([Issue #16029](https://github.com/unslothai/unsloth/issues/16029), fix [PR #16044](https://github.com/unslothai/unsloth/pull/16044)), ModelScope source blocking >50 GB files ([Issue #15989](https://github.com/unslothai/unsloth/issues/15989)), MiniMax-M3.1 NVFP4 CPU fallback despite all 61 layers offloaded ([Issue #15409](https://github.com/unslothai/unsloth/issues/15409), closed).

## 6. What This Means for Application Developers
- **If you rely on long-running agents, treat context management as unsafe today.** Issue #15800 plus the Anthropic passthrough behavior mean client-side history trimming is still required; don't trust the server's auto-compaction. Also validate tool-call completions yourself until [PR #16035](https://github.com/unslothai/unsloth/pull/16035) lands — a truncated `Write` is currently indistinguishable from a complete one.
- **Performance wins are opt-in and model-specific.** `UNSLOTH_DECODE_FUSED_NF4=1` is worth benchmarking for single-stream 4-bit Llama serving; Gemma 3 on 3090/4090/L4-class hardware should see more consistent flex-attention behavior once #16049/#16041 merge.
- **Apple Silicon users**: memory estimates were over-reporting badly, so don't reject models based on the pre-#16021 estimator, but also budget for unfused attention score matrices that the old estimate ignored.
- **Pinning betas is advisable.** Studio desktop is the primary surface for most reported regressions (update failures, UI lag, model load failures, dataset preview), and the cadence is daily. If you serve models programmatically, prefer the Python/CLI path where the dataset and compaction issues don't apply.
- **Watch multi-model residency** ([PR #10876](https://github.com/unslothai/unsloth/pull/10876)) and EXL3 ([PR #7115](https://github.com/unslothai/unsloth/pull/7115)) if you're planning MoE or multi-tenant gateway deployments — both target exactly the routing/quantization gaps that constrain those setups today.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*