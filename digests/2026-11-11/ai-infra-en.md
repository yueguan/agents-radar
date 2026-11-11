# AI Infrastructure Digest 2026-11-11

> Generated: 2026-11-11 02:13 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-11**

---

### **1. Ecosystem Overview**  
The AI inference infrastructure landscape in late 2026 is characterized by intense specialization, cross-platform acceleration, and heightened security awareness. Projects are converging on high-performance serving for large models across heterogeneous hardware—especially AMD ROCm (gfx950/MI355X), NVIDIA Blackwell (SM120), and Intel XPU—while also deepening integration with agent workflows, multimodal inputs, and speculative decoding. A significant shift toward secure-by-design deployment practices is evident following the LiteLLM v1.107.0 compromise, signaling maturing DevOps standards. Simultaneously, local runtimes like llama.cpp and Ollama continue to push boundaries in lightweight, embedded inference, enabling edge and mobile use cases.

---

### **2. Activity Comparison**

| Project       | Issues Open | PRs Merged | Release Status         |
|---------------|-------------|------------|------------------------|
| **vLLM**      | 48          | 14         | Stable: `v0.33.0rc1.dev992` |
| **SGLang**    | 62          | 12         | Patch release (`v0.5.23`) with regression |
| **llama.cpp** | 51          | 10         | No new release; internal changes |
| **Ollama**    | 38          | 10         | RC: `v0.40.17-rc0` — stable update |
| **LiteLLM**   | 58          | 9          | Security patch: `v1.110.0-rc.1`, LTS `v1.108.5` |
| **Unsloth**   | 47          | 8          | Beta: `0.1.916-beta` — no breaking changes |

> ✅ **Insight**: SGLang leads in issue volume (indicating active development and debugging), while LiteLLM and Ollama show strong release momentum focused on stability and security. vLLM remains the most stable in terms of API continuity.

---

### **3. Model Support Race**

| New Model / Architecture             | Supported By                          | Notes |
|--------------------------------------|---------------------------------------|-------|
| **Qwen3.8-Flash-Next (MXFP4)**      | vLLM, SGLang, Unsloth                | vLLM & SGLang have dedicated kernel tuning via AITER/FlyDSL |
| **DeepSeek-V4.1-Flash**             | vLLM, SGLang, Ollama (via MLX)        | SGLang adds DSA KV cache defaults; vLLM optimizes decode batching |
| **Kimi-K3/K3.1 Vision**             | vLLM, Ollama, Unsloth                | Ollama enables video input via MLX; vLLM supports metadata-only images |
| **MiniMax-H3-Image**                | Unsloth (new)                         | First major image generation addition |
| **Mistral-Large-4**                 | SGLang (full support)                 | Includes MoE, MLA, YaRN, Pixtral multimodal |
| **Nemotron 3.5 Bolt 74B-A9B**       | Ollama (MLX backend)                  | Supports 3-bit quantization and expert config |
| **TTS Models (zonos3-mini)**        | llama.cpp                             | Custom Jinja-based text normalization added |
| **AMD gfx950 / MI355X**             | vLLM, SGLang, llama.cpp               | vLLM leads with GEMM/kernel tuning; SGLang sets default FP8 KV cache |

> 🏆 **Leaderboard**:  
> - **vLLM** wins in **ROCm optimization depth** and **model-specific kernel tuning**.  
> - **SGLang** leads in **multi-model coverage**, especially Mistral-Large-4 and Qwen4-Exp.  
> - **Ollama** dominates **agent-ready multimodal support** (video + structured reasoning).  
> - **Unsloth** excels in **UI/tooling integration** and **local model portability**.

---

### **4. Performance Frontier**

| Optimization Focus           | Leading Projects                     | Key Developments |
|-------------------------------|--------------------------------------|------------------|
| **KV Cache Efficiency**       | vLLM, SGLang, llama.cpp              | FP8_e4m3 defaults (SGLang), fused dequant (llama.cpp), split-K BF16 router (vLLM) |
| **Batching & Throughput**     | vLLM, SGLang                         | Split-K GEMM (vLLM), mHC mix in BF16x3 (SGLang) |
| **Quantization & Fused Kernels** | vLLM, llama.cpp, SGLang            | Fused Q8_0/Q4_0 KV dequant (llama.cpp), MXFP4 kernels (vLLM/SGLang) |
| **Distributed Serving**       | vLLM, SGLang                         | NIXL/P/D connector fixes (vLLM), Mooncake cache invalidation |
| **Speculative Decoding**      | Ollama, SGLang, vLLM                 | Metrics reporting (Ollama), draft acceptance drop (Unsloth), latency regression (SGLang) |
| **Kernel-Level Tuning**       | vLLM (AITER), SGLang (FlyDSL)        | GFX950-specific GEMMs, warp split-K FlashInfer |

> 🔥 **Frontier Hotspots**:  
> - **ROCm (gfx950)** is now a primary target for kernel-level optimization (vLLM/SGLang).  
> - **FP8 and MXFP4** are becoming standard for memory-sensitive deployments.  
> - **Speculative decoding** remains unstable across platforms—regressions in SGLang and divergence bugs in llama.cpp highlight ongoing challenges.

---

### **5. Layer Positioning**

| Project       | Primary Layer                  | Role Summary |
|---------------|-------------------------------|--------------|
| **vLLM**      | **Serving Engine**            | High-throughput, low-latency inference engine with advanced batching and kernel tuning. Targets datacenter-scale deployment. |
| **SGLang**    | **Serving Engine + Gateway**  | Hybrid runtime with built-in orchestration, agent support, and multi-backend routing. Strong focus on distributed systems and speculative decoding. |
| **llama.cpp** | **Local Runtime / Embedded**  | Lightweight, CPU/GPU-accelerated inference engine ideal for edge devices, mobile, and offline environments. Emphasis on binary efficiency and low-level control. |
| **Ollama**    | **Gateway + Local Runtime**   | Developer-friendly CLI/UI layer with model management, tool calling, and agent scaffolding. Bridges local execution with cloud-like ease-of-use. |
| **LiteLLM**   | **API Gateway / Orchestration** | Multi-provider abstraction layer with cost tracking, rate limiting, and observability. Critical for enterprise LLM pipelines. |
| **Unsloth**   | **Agent Studio / UX Platform** | End-to-end agent development environment with UI, reverse proxy support, and model integration tools. Focuses on developer experience and deployment security. |

> 📊 **Strategic Differentiation**:  
> - **Engineers** choosing between vLLM/SGLang should prioritize throughput vs. flexibility.  
> - **Product teams** building agents will favor Ollama or Unsloth for rapid prototyping.  
> - **Enterprises** need LiteLLM for cost visibility and provider agnosticism.

---

### **6. Trend Signals**

#### **Emerging Industry Trends (from 2026-11-11 activity):**
1. **Hardware Diversification Accelerates**: AMD ROCm (gfx950/MI355X) is no longer niche—now receiving first-party kernel optimizations from vLLM and SGLang.
2. **FP8 & MXFP4 Are Becoming Standard**: All major projects now support or default to FP8 KV cache, signaling a shift toward memory-efficient inference at scale.
3. **Security Is Non-Negotiable**: The LiteLLM v1.107.0 incident underscores that supply-chain hygiene is now central to infrastructure trust. Signed containers and audit trails are expected.
4. **Speculative Decoding Still Fragile**: Despite widespread adoption, performance regressions (SGLang), divergence bugs (llama.cpp), and accuracy drops (Unsloth) reveal immature implementation maturity.
5. **Multimodal Workflows Go Mainstream**: Video input (Ollama), vision models (Unsloth), and image search (LiteLLM) are now production-ready features—not experimental.
6. **Developer Experience Is the New Differentiator**: Projects like Ollama and Unsloth are winning not through raw performance, but through seamless tooling, UI consistency, and reliable model loading.

#### **Actionable Guidance for Application Developers:**
- ✅ **Prioritize security**: Use `ghcr.io/berriai/litellm:lts` and verify image signatures via cosign.
- ✅ **Validate speculative decoding outputs**, especially with quantized models—expect inconsistencies.
- ✅ **Leverage FP8 and MXFP4** where available (vLLM/SGLang) for memory savings without sacrificing throughput.
- ✅ **Use Ollama’s `OLLAMA_NUM_PARALLEL`** to reduce first-token latency in agent workloads.
- ✅ **Test on AMD/Intel Arc GPUs**—known instability exists in ROCm and Vulkan drivers.
- ✅ **Avoid v0.5.23 of SGLang on Hopper**—significant decode latency regression reported.

---

> **Final Note**: The AI inference stack is no longer just about speed—it’s about **reliability, security, and usability**. The leaders today aren’t just fastest—they’re most resilient, best-integrated, and safest to deploy at scale.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM Digest – 2026-11-11**

---

### **1. Today’s Highlights**  
The vLLM project continues its aggressive optimization push for AMD ROCm hardware, with multiple high-impact performance PRs targeting gfx950 (MI355X) and MI350X platforms—particularly for Qwen3.8-Flash-Next and DeepSeek-V4.1 models. Critical stability fixes were merged for NIXL P/D disaggregation and MooncakeConnector cache invalidation, improving reliability in distributed inference scenarios. New CI support for Intel XPU engines expands cross-platform compatibility.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes observed. The project remains stable at v0.33.0rc1.dev992+g1648fefab, with ongoing focus on internal optimizations rather than user-facing updates.

---

### **3. New Model & Hardware Support**  
- **ROCm (gfx950 / MI355X)**: Expanded support for `amd/Qwen3.8-2.4T-A95B-Quark-MXFP4` and `Qwen3.8-Flash-Next-Quark-MXFP4`, with dedicated GEMM and kernel tuning via AITER's FlyDSL.
- **Intel XPU**: Enabled `v1/engine` tests in Intel CI (`#66926`), marking a step toward full XPU integration.
- **Kimi-K3/K3.1**: Added support for metadata-only image inputs in EPD (`#66925`) and updated B300 AgentX presets (`#66433`).
- **Multi-modality**: PyNvVideoCodec now includes keyframe-aware sampling for efficient video preprocessing (`#66073`).

> 🔗 [PR #66926](https://github.com/vllm-project/vllm/pull/66926) | [PR #66925](https://github.com/vllm-project/vllm/pull/66925) | [PR #66073](https://github.com/vllm-project/vllm/pull/66073)

---

### **4. Performance & Optimization**  
- **ROCm/GFX950**: Split-K BF16 router GEMM introduced for DeepSeek-V4.1 decode batches (22 ≤ M ≤ 256), replacing hipBLASLt to improve throughput under concurrent expert execution (`#65749`).
- **GLM-5.3**: Added FlashInfer warp split-K and `ll_bf16` GEMMs to low-latency table on SM107, reducing latency for smaller token counts (`#65706`).
- **MiniMax-M2.7**: Ongoing efforts to address TP4+EP throughput regression observed in v0.29–0.32 (`#63705`).
- **Batch-invariant mode**: Fixed inefficient re-computation of linear layers by routing through Humming instead of raw Triton (`#66838`).
- **DSpark/Muse Spark**: Fused draft norms and Markov sampling on NVIDIA SM100, yielding -4.4% TPOT latency improvement (`#66764`).
- **CI/Build**: PR time-to-signal reduced from ~73 min to target ~30 min via build optimization (`#66411`).

> 🔗 [PR #65749](https://github.com/vllm-project/vllm/pull/65749) | [PR #65706](https://github.com/vllm-project/vllm/pull/65706) | [PR #66764](https://github.com/vllm-project/vllm/pull/66764) | [PR #66838](https://github.com/vllm-project/vllm/pull/66838)

---

### **5. Stability & Regressions**  
- **Critical**: DFlash acceptance fails at TP > 1 for MiMo-V2.6-Flash-MOPD (`#66387`). This impacts multi-node inference pipelines using DFlash drafter.
- **High Severity**: MooncakeConnector cache not cleared on encoder reset causes stale embeddings in subsequent requests (`#66846`). Fix merged.
- **Moderate**: NIXL P/D connector stalls due to lost peer connections; fixed via re-handshake logic (`#66795`).
- **Regression**: Qwen3.5-122B-A10B-FP8 accuracy drops after commit `0fae39a` (`#65906`). Investigation underway.
- **Minor**: First boot of new `(max-num-batched-tokens, max-num-seqs)` shape allocates ~5% smaller KV cache than subsequent boots (`#54383`).

> 🔗 [Issue #66387](https://github.com/vllm-project/vllm/issues/66387) | [PR #66846](https://github.com/vllm-project/vllm/pull/66846) | [PR #66795](https://github.com/vllm-project/vllm/pull/66795) | [Issue #65906](https://github.com/vllm-project/vllm/issues/65906)

---

### **6. What This Means for Application Developers**  
- **For production deployments on AMD GPUs**: Expect improved throughput and stability for Qwen3.8-series and DeepSeek-V4.1 models on MI355X/gfx950. Use `--kv-cache-dtype fp8_e4m3` with AITER’s sparse MLA kernel for memory savings (`#66036`).
- **For agents using structured outputs**: Be aware of ~2x decode throughput regression since `#45424`; monitor impact if using grammar-based parsing.
- **For distributed systems**: Ensure you’re on latest vLLM version to avoid known issues with Mooncake/NIXL connectors during restarts or prefill resets (`#66846`, `#66795`).
- **For model developers**: If deploying hybrid GDN/Mamba models with heterogeneous TP, verify `MooncakeConnector` compatibility (`#53078` is still open but tracked).
- **For CI/CD pipelines**: Build times are improving—expect faster feedback cycles (~30 min signal time targeted).

> ✅ **Actionable Tip**: Use `VLLM_BATCH_INVARIANT=1` only when necessary; recent fix (`#66838`) avoids costly repeated unpacking in quantized GPTQ layers.

---  
*Data source: [vllm-project/vllm GitHub](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-11**

---

### **1. Today's Highlights**  
The SGLang project continues to strengthen its multi-GPU and cross-architecture support, with critical performance improvements for AMD ROCm (gfx950/gfx1201) and Blackwell (SM120) backends. Key work includes optimizing DSA KV cache defaults on ROCm, enabling FP8 for Mamba host pools in HiCache, and addressing flaky CI infrastructure that impedes PR validation. A major regression in decode latency on Hopper (SM90) was reported post-v0.5.22, highlighting ongoing tuning challenges.

---

### **2. Releases & Breaking Changes**  
*No new releases detected in the last 24 hours.*  
However, a notable **performance regression** has been observed: upgrading from `v0.5.22` to `v0.5.23` causes **33–45% higher per-step decode time** at concurrency 12 on Hopper (SM90), particularly under DSPARK speculative decoding. Developers should monitor this issue ([#49336](https://github.com/sgl-project/sglang/issues/49336)) before upgrading.

---

### **3. New Model & Hardware Support**  
- **AMD ROCm (gfx950/gfx1201)**: DeepSeek-V4.1-Flash and Qwen4-Exp now have dedicated optimization tracking ([#49327](https://github.com/sgl-project/sglang/issues/49327)).  
- **Mistral-Large-4**: Full model support added, including MoE, MLA, DSA sparse attention, YaRN, and multimodal vision via Pixtral ([#47271](https://github.com/sgl-project/sglang/pull/47271)).  
- **Qwen4-Exp**: Integrated into HiCache with unified Engram store support ([#46248](https://github.com/sgl-project/sglang/pull/46248)).  
- **NVFP4 KV**: Continued rollout on SM120/B200; initial PRs merged ([#29913](https://github.com/sgl-project/sglang/issues/29913)).

---

### **4. Performance & Optimization**  
- **AMD ROCm**:  
  - Default DSA KV cache now set to `fp8_e4m3` on gfx950 ([#48996](https://github.com/sgl-project/sglang/pull/48996)).  
  - Faster MQA logits decode via padded Q + group-strided launch on MI355X ([#48649](https://github.com/sgl-project/sglang/pull/48649)).  
  - 12-head MLA decode and 8-token verify now run on fp8-Q ASM ([#45320](https://github.com/sgl-project/sglang/pull/45320)).  
- **NVIDIA (SM120/Blackwell)**:  
  - mHC mix in BF16x3 for DeepSeek-V4.1 decode batches improves throughput ([#48456](https://github.com/sgl-project/sglang/pull/48456)).  
- **HiCache & Mooncake**:  
  - Prefetching SSD-resident KV into DRAM during queueing reduces latency ([#49696](https://github.com/sgl-project/sglang/pull/49696)).  
  - Mamba host pool size now configurable via `--hicache-mamba-size-gb` ([#47867](https://github.com/sgl-project/sglang/pull/47867)).  

---

### **5. Stability & Regressions**  
- **Critical Regression**: `DeepSeek-V4.1-Flash` decode latency increased by **33–45%** after v0.5.22 → v0.5.23 upgrade on SM90 ([#49336](https://github.com/sgl-project/sglang/issues/49336)).  
- **Crash on FA4 Backend**: GLM-5.3-Flash on SM120 crashes during CUDA graph capture with hybrid extend reshape — only Triton backend works ([#42012](https://github.com/sgl-project/sglang/issues/42012)).  
- **Cache Hit-Rate Dip**: Reproducible drop in prefix cache hit rate under L2/L3 pressure with HiCache + Mooncake ([#47481](https://github.com/sgl-project/sglang/issues/47481)).  
- **Missing BOS Token**: Rust server returns no beginning-of-sequence token when tokenizer expects `add_bos_token` ([#49519](https://github.com/sgl-project/sglang/issues/49519)).  
- **CI Flakiness**: High comment count on [flaky test tracker](https://github.com/sgl-project/sglang/issues/42752) indicates systemic CI instability affecting PR validation.

---

### **6. What This Means for Application Developers**  
- **Use caution with v0.5.23+** on Hopper GPUs — expect significant decode slowdowns unless you’re using non-speculative decoding.  
- **Leverage AMD optimizations** if deploying on MI355X or gfx950: enable `fp8_e4m3` KV cache and use `--fp4-gemm-backend aiter|triton` for MXFP4 models.  
- **Optimize agent workflows** using HiCache + Mooncake: prefetching is now enabled during queuing, improving L3 offload efficiency.  
- **Avoid FA4 backend on SM120** for GLM-5.3-Flash due to known crash; stick with Triton.  
- **Monitor CI stability** — frequent flaky tests may delay integration; consider pinning to stable commits until [#42752](https://github.com/sgl-project/sglang/issues/42752) is resolved.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-11**

---

### **1. Today’s Highlights**  
The latest updates focus on critical CUDA stability fixes, including reduced VMM pool allocation to prevent OOM errors on high-memory GPUs and improved graph reuse handling. Significant progress continues in backend robustness with new SYCL optimizations and Vulkan error mitigation, while model support expands with kimi K3.1 vision and TTS model additions.

---

### **2. Releases & Breaking Changes**  
No new tagged releases were published in the last 24 hours. However, notable internal changes include:  
- **CUDA**: Halved VMM pool reservation (from 16GB → 8GB) for GPUs with unified memory to avoid `cuMemAddressReserve` failures ([#30542](https://github.com/ggml-org/llama.cpp/pull/30542)).  
- **CUDA**: Moved graph and concurrent stream state out of `common.cuh` to reduce rebuild overhead ([#32001](https://github.com/ggml-org/llama.cpp/pull/32001)).  
- **SYCL**: Introduced cached device ID in `set_main_device` to avoid repeated mutex locking ([#32013](https://github.com/ggml-org/llama.cpp/pull/32013)).

> ⚠️ *Developers using large GPU memory pools or frequent graph reuse should verify their CUDA config aligns with these changes.*

---

### **3. New Model & Hardware Support**  
- ✅ **Vision Models**: Added support for *kimi K3.1 vision* via `mtmd` backend ([#31980](https://github.com/ggml-org/llama.cpp/pull/31980)).  
- ✅ **TTS Models**: Added `zonos3-mini` TTS model with custom text normalization via Jinja ([#32020](https://github.com/ggml-org/llama.cpp/pull/32020)).  
- ✅ **Backends**:  
  - **CANN**: Fixed Flash Attention NaNs by passing attention mask correctly ([#31590](https://github.com/ggml-org/llama.cpp/pull/31590)).  
  - **SYCL**: Improved FP16 build support proposal ([#31678](https://github.com/ggml-org/llama.cpp/issues/31678)).  
  - **Vulkan**: Continued work on Adreno driver compatibility ([#28635](https://github.com/ggml-org/llama.cpp/issues/28635), [#30649](https://github.com/ggml-org/llama.cpp/issues/30649)).  
- ✅ **Quantization**: Work ongoing for support of `PQ2_0` (type 142) and `PTQ1_0` (type 143) used by Ternary-Bonsai-2 ([#29058](https://github.com/ggml-org/llama.cpp/issues/29058)).

---

### **4. Performance & Optimization**  
- **CUDA**: Fused Q8_0/Q4_0 KV dequant into MMA flash-attention kernels, eliminating intermediate f16 materialization and reducing scratch memory use ([#31802](https://github.com/ggml-org/llama.cpp/pull/31802)).  
- **HIP/ROCm**: Fixed BLAS workspace allocation by installing a fixed 32MB buffer instead of per-call allocation, improving stability and performance ([#31838](https://github.com/ggml-org/llama.cpp/pull/31838)).  
- **Vulkan**: Proposed optimization to fold token verification and GQA head processing into single row tiles, enabling better coalescing ([#30190](https://github.com/ggml-org/llama.cpp/pull/30190)).  
- **CPU**: Shared compute buffer across subcontexts improves efficiency in multi-threaded inference ([#31945](https://github.com/ggml-org/llama.cpp/pull/31945)).

> 💡 *Expect measurable latency reduction in MoE and quantized models when using fused KV dequant or optimized backends.*

---

### **5. Stability & Regressions**  
Critical issues reported today:  
1. **Speculative Decoding Divergence** (Bug): Greedy sampling produces different output between speculative and vanilla runs when target model is quantized (e.g., Q4_K_M); works fine on bf16 ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)) — *40 comments, 3 likes*.  
2. **Segfault in Speculative Decoding** (Critical): Crashes during Llama-3.1-405B-Instruct + Llama-3.1-8B draft model setup ([#9949](https://github.com/ggml-org/llama.cpp/issues/9949)) — *17 comments, 0 likes*.  
3. **SYCL TDR Resets on Dual Arc Pro B70**: GPU driver resets due to DFlash2 draft model load ([#28778](https://github.com/ggml-org/llama.cpp/issues/28778)) — *12 comments*.  
4. **Vulkan Driver Crash on Adreno 840**: `vkGetDeviceFaultInfoEXT` crashes during full offload of 27B TQ1_0 model ([#30649](https://github.com/ggml-org/llama.cpp/issues/30649)) — *4 comments*.  

> 🔍 *No PRs directly address these yet; developers using speculative decoding or mobile Vulkan backends should proceed with caution.*

---

### **6. What This Means for Application Developers**  
- Use `GGML_CUDA_POOL_DEBUG=1` for debugging CUDA memory issues — helpful for diagnosing OOM scenarios ([#31919](https://github.com/ggml-org/llama.cpp/pull/31919)).  
- If running MoE models on CPU with `--cpu-moe`, be aware of potential slowdowns on dual-GPU SYCL setups ([#31510](https://github.com/ggml-org/llama.cpp/issues/31510)).  
- For production systems using speculative decoding, validate outputs carefully—especially with quantized targets—due to known divergence bugs ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)).  
- Consider upgrading to ROCm 6.3 if using HIP builds to ensure correct sync behavior and mask width handling ([#31617](https://github.com/ggml-org/llama.cpp/pull/31617)).  
- The UI now pulls brand assets from `llama-brand`, ensuring consistent visual identity across deployments ([#32039](https://github.com/ggml-org/llama.cpp/pull/32039)).

> 📌 *Stay vigilant with backend-specific edge cases—especially on Intel Arc, Adreno, and CANN platforms—and test critical paths under real-world loads.*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-11**

---

### **1. Today's Highlights**  
The latest release, **v0.40.17-rc0**, brings critical improvements to model runner stability on Vulkan-based GPUs and enhances CLI resilience during restarts. Key developments include MLX backend support for video input and speculative decoding metrics reporting—signaling deeper integration with high-performance inference workflows. Additionally, the UI now dynamically respects model-level thinking capabilities via `/api/show`, enabling consistent behavior across clients.

---

### **2. Releases & Breaking Changes**  
- **v0.40.17-rc0** (latest):  
  - ✅ Improved ChatGPT CLI restore and graceful restart logic ([#19583](https://github.com/ollama/ollama/pull/19583))  
  - ✅ Enhanced OpenCode settings via `thinking` metadata in `cmd/launch` ([#19409](https://github.com/ollama/ollama/pull/19409))  
  - ✅ Optimized memory mapping (`mmap`) for Vulkan-integrated GPUs ([#19409](https://github.com/ollama/ollama/pull/19409))  
  > *Note: No breaking API changes reported; backward compatibility preserved.*

---

### **3. New Model & Hardware Support**  
- **MLX Backend**:  
  - Added support for **video input** (MP4/MOV) in LFM3-VL, LFM3-Omni, and Qwen3.5/3.6 models via `images` field ([#19348](https://github.com/ollama/ollama/pull/19348))  
  - Added support for **Nemotron 3.5 Bolt 74B-A9B** with per-layer expert config, mixed-precision stacking, and 3-bit quantization ([#19229](https://github.com/ollama/ollama/pull/19229))  
- **Vulkan Backend**:  
  - Improved mmap usage on integrated GPUs for better memory efficiency  
- **New Capabilities**:  
  - MLX models now report `"video"` capability in `/api/show` response  
  - Decision models (e.g., Liquid d1, Kev, H2O Lightning) supported under `/v1/systemone` proposal ([#19639](https://github.com/ollama/ollama/issues/19639))

---

### **4. Performance & Optimization**  
- **Speculative Decoding Metrics**:  
  - Per-request draft acceptance rates, drafted vs. accepted tokens now exposed in API responses and `--verbose` output ([#19368](https://github.com/ollama/ollama/pull/19368))  
- **Parallel Inference**:  
  - MLX now supports concurrent generation via `OLLAMA_NUM_PARALLEL` (default = 1), reducing first-token latency for multi-request workloads ([#19197](https://github.com/ollama/ollama/pull/19197))  
- **Model Loading**:  
  - `ollama load MODEL` command added to pre-warm models without starting a session ([#16207](https://github.com/ollama/ollama/pull/16207))  
- **Memory Management**:  
  - MLX KV-cache leak fix: memory no longer accumulates per request; now released between calls ([#17875](https://github.com/ollama/ollama/issues/17875) — closed)

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix/PR | Link |
|--------|------|--------|--------|------|
| ⚠️ High | Windows RTX 5090: Runner disconnects during tool calling → HTTP 500 /tokenize | Open | No PR | [#19439](https://github.com/ollama/ollama/issues/19439) |
| ⚠️ High | AMD Radeon 780M Vulkan regression: `ErrorDeviceLost` on larger models | Closed | Fixed in 0.32.10 | [#17748](https://github.com/ollama/ollama/issues/17748) |
| ⚠️ Medium | Intel Iris Xe iGPU: `ErrorOutOfDeviceMemory` on qwen2.5:14b in 0.33.3+ | Closed | Downgrade workaround | [#18531](https://github.com/ollama/ollama/issues/18531) |
| ⚠️ Medium | OLLAMA_KEEP_ALIVE=300s too short for long interaction cycles | Open | No PR | [#19624](https://github.com/ollama/ollama/issues/19624) |
| 🟡 Low | `-v`/`--version` shows misleading "warning" messages | Closed | Patched | [#19613](https://github.com/ollama/ollama/issues/19613) |

> **Note**: Critical regressions persist on **Windows + RTX 5090** and **AMD 780M Vulkan**, indicating GPU-specific stress points in recent runners.

---

### **6. What This Means for Application Developers**  
- **Build smarter agents**: Use `OLLAMA_NUM_PARALLEL` to reduce latency in multi-user or batch scenarios (MLX only).  
- **Leverage structured reasoning**: The dynamic thinking toggle via `/api/show` enables UI consistency across models—no more hardcoded name checks.  
- **Integrate multimodal data**: Video input is now available in MLX backends—ideal for agent systems requiring visual context.  
- **Monitor speculative decoding performance**: Use new metrics to tune draft depth and evaluate real-world speedups.  
- **Avoid common pitfalls**: Be cautious with large payloads (>32 MiB) in Cloud `/api/chat` (undocumented limit); use `load` command to warm models preemptively.  

> 🔧 **Pro Tip**: For stable inference on Intel/AMD integrated GPUs, consider pinning to Ollama <0.33.3 until Vulkan issues are resolved.  

---  
*Data sourced from [github.com/ollama/ollama](https://github.com/ollama/ollama) — updated 2026-11-11.*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

---

### **1. Today's Highlights**  
LiteLLM’s latest activity centers on critical security remediation following a high-profile compromise of version `1.107.0`, which was removed from PyPI and Docker registries due to a credential-stealing trojan embedded in a lookalike repository. Concurrently, the project continues its architectural modernization with unified container images, Helm charts, and Terraform workloads—all underpinned by enhanced security via cosign signing. A key focus remains on improving cost tracking accuracy and stream reliability across providers like Anthropic, Vertex AI, and Bedrock.

---

### **2. Releases & Breaking Changes**  
- **v1.110.0-rc.1**, **v1.109.0**, **v1.108.5 (LTS)**: Released with improved security posture and consistent cosign signatures. All images are signed using the key from [commit `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065).  
  - **Action Required**: Users must verify image integrity using `cosign verify` before deployment.  
  - **Security Note**: The compromised `v1.107.0` has been yanked from all public registries; avoid any reference to it.  
  - **LTS Update**: `ghcr.io/berriai/litellm:lts` now points to `v1.108.5`—recommended for production stability.

> 🔐 [Verify Docker Image Signature](https://docs.sigstore.dev/cosign/overview/)  
> 📦 [Release v1.110.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.110.0-rc.1)  
> 💡 [LTS Tag Guidance](https://github.com/BerriAI/litellm#lts)

---

### **3. New Model & Hardware Support**  
No new models or hardware backends were added in this cycle. However, notable improvements include:
- **Bedrock Knowledge Base Search Enhancement** ([PR #51063](https://github.com/BerriAI/litellm/pull/51063)): Supports image-based queries and returns image hits when using multimodal embedding models.
- **Mistral Native Token Counting** ([PR #50653](https://github.com/BerriAI/litellm/pull/50653)): Integrates Mistral’s official `/responses/input_tokens` endpoint for precise token billing and context window enforcement.

---

### **4. Performance & Optimization**  
- **Streaming Latency Reduction**: PR [#51033](https://github.com/BerriAI/litellm/pull/51033) ensures streams terminate at the terminal event rather than waiting for upstream connection close—reducing perceived latency by up to ~30s in long-running responses (e.g., Claude Code).
- **TPM Rate Limiter Fix**: PR [#49675](https://github.com/BerriAI/litellm/pull/49675) addresses premature TPM exhaustion during upstream 429s, restoring proper rate-limiting behavior under concurrent load.
- **SSE Keepalive Handling**: PR [#49711](https://github.com/BerriAI/litellm/pull/49711) prevents silent failure after keepalive ping by ensuring upstream errors are propagated in-stream.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR | Notes |
|------|----------|--------|--------|-------|
| [#47826](https://github.com/BerriAI/litellm/issues/47826) | ⚠️ Critical | Closed | N/A | Malicious `v1.107.0` package removed from PyPI/Docker. **All users must upgrade immediately.** |
| [#47340](https://github.com/BerriAI/litellm/issues/47340) | 🛑 High | Open | N/A | `tpm_limit` never resets in-memory; requires manual `/key/update`. |
| [#51029](https://github.com/BerriAI/litellm/issues/51029) | 🟡 Medium | Open | N/A | OTel `gen_ai.response.model` shows alias instead of actual model used. |
| [#49675](https://github.com/BerriAI/litellm/issues/49675) | 🛑 High | Open | N/A | TPM counter outlives window under 429s — leads to false throttling. |
| [#48937](https://github.com/BerriAI/litellm/issues/48937) | 🟡 Medium | Closed | PR [#50957](https://github.com/BerriAI/litellm/pull/50957) | Streamed costs logged as $0 despite real usage. Fixed in-sidecar spend collector. |

> ✅ **Fixed**: Cost logging (`#48937`) and malformed requests (`#50937`) have resolved PRs merged.

---

### **6. What This Means for Application Developers**  
- **Security First**: Immediately audit your deployments for `v1.107.0` or older versions. Use `ghcr.io/berriai/litellm:lts` for stable, secure production use.
- **Cost Accuracy**: If you rely on custom pricing, `base_model`, or cross-provider cost tracking, ensure your config aligns with recent fixes—especially around `response_cost` and `spend` logging (e.g., `#47675`, `#47908`).
- **Stream Reliability**: For agents using streaming APIs (especially with Anthropic/Claude), expect more predictable termination behavior thanks to PRs like `#51033`.
- **Future-Proofing**: Leverage the new unified Docker/Helm/Terraform workflows ([PRs #43332, #43330, #43358](https://github.com/BerriAI/litellm/pulls?utf8=%E2%9C%93&q=43332)) for simpler CI/CD and infrastructure-as-code setups.
- **Guardrails & Tools**: Be aware that `custom_code` regex flags and `MCP tool` guardrail visibility remain limited—check [PR #51097](https://github.com/BerriAI/litellm/pull/51097) for upcoming support.

🔧 **Recommended Actions**:  
- Upgrade to `v1.108.5-lts` or later.  
- Audit virtual keys and spending configurations.  
- Enable OpenTelemetry and validate `gen_ai.response.model` alignment in traces.  

---  
*Digest generated: 2026-11-11 | Source: [GitHub – BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-11**

---

### **1. Today's Highlights**  
The Unsloth ecosystem continues to mature with focused improvements in Studio’s usability and security, particularly around reverse proxy support, UI consistency, and model integration. Key PRs address critical rendering bugs (e.g., glass theme mangles), improve TTS/audio model visibility, and enable secure custom domain hosting via Cloudflare Tunnel—essential for enterprise-grade deployments. A major fix now ensures `max_tokens` is correctly respected on vLLM/SGLang tool calls, resolving a context-length-related 400 error.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking configuration changes were published. The latest desktop version remains **0.1.916-beta**, with no migration notes issued.

---

### **3. New Model & Hardware Support**  
- ✅ **MiniMax-H3-Image** added to Studio’s image generation pipeline (`MiniMaxAI/MiniMax-H3-Image`) with support for text-to-image and multi-reference edits (up to 16 images). [PR #17453](https://github.com/unslothai/unsloth/pull/17453)  
- ✅ **Yandex** integrated as a primary web search engine fallback tier, improving resilience against DDGS anti-bot throttling. [PR #17447](https://github.com/unslothai/unsloth/pull/17447)  
- ✅ **RPM package** requested for Linux distributions (Fedora/RHEL/CentOS), signaling growing demand for native packaging. [Issue #17445](https://github.com/unslothai/unsloth/issues/17445)  

> *Note: No new GPU backend support (e.g., new ROCm/NVIDIA hardware) was introduced today.*

---

### **4. Performance & Optimization**  
- 🚀 **Context handling optimization**: Studio now properly respects `max_tokens` limits when making tool requests to vLLM and SGLang, preventing `context_length_exceeded` errors even when prompt + max_tokens exceeds window size. [PR #17426](https://github.com/unslothai/unsloth/pull/17426)  
- ⏱️ **Model loading efficiency**: `push_to_hub_gguf()` now correctly embeds user-defined tags into the README card, improving discoverability of custom quantizations. [PR #17438](https://github.com/unslothai/unsloth/pull/17438)  
- 💡 **Dynamic GGUF calibration**: Active community proposal to expand Ukrainian language coverage in calibration datasets for improved dynamic quantization fidelity. [Issue #17371](https://github.com/unslothai/unsloth/issues/17371)

---

### **5. Stability & Regressions**  
- 🔴 **Critical UI bug**: Glass theme causes severe UI mangled rendering upon model selection. Fixed in [PR #17417](https://github.com/unslothai/unsloth/pull/17417) (closed).  
- 🔴 **Security vulnerability**: Seeded admin password exposed in proxied pages if loopback request carries `X-Forwarded-*` headers. Patched in [PR #17454](https://github.com/unslothai/unsloth/pull/17454).  
- 🔴 **Crash on AMD GPUs**: PyTorch segfaults on RX 6700 XT (gfx1031) due to incorrect RDNA 2 detection in `unsloth-rocm` Docker image. [Issue #17147](https://github.com/unslothai/unsloth/issues/17147) — *no fix yet*.  
- 🔴 **Local model load failure**: Image generation models fail to load on Intel Arc 140V. [Issue #17137](https://github.com/unslothai/unsloth/issues/17137) — *pending investigation*.  
- 🟡 **Tool use regression**: Qwen3.8-Flash-Next UD 3.1 GGUFs show draft acceptance drop from ~74% to 56–66% under real workloads due to mismatched MTP heads. [Issue #17402](https://github.com/unslothai/unsloth/issues/17402) — *planned head alignment pending*.

---

### **6. What This Means for Application Developers**  
- **Secure deployment**: Use [PR #17435](https://github.com/unslothai/unsloth/pull/17435) to serve Unsloth Desktop behind allowlisted reverse proxies (e.g., Cloudflare Tunnel) without exposing credentials.  
- **Reliable tooling**: Ensure your app uses `max_tokens` safely by validating context length bounds—especially when integrating with vLLM/SGLang backends.  
- **Model portability**: Leverage `push_to_hub_gguf(tags=...)` to tag and document custom GGUFs properly; avoid silent failures due to missing README metadata.  
- **UI/UX caution**: Avoid enabling glass themes in production-facing builds until [PR #17417](https://github.com/unslothai/unsloth/pull/17417) is rolled out.  
- **Hardware-aware design**: Test on AMD and Intel Arc GPUs—known instability exists in ROCm and Intel driver stacks. Consider fallbacks for these platforms.

> For agents and fine-tuned apps: Be cautious with decision models loaded via API—they may still appear in chat pickers despite being excluded from inference. [PR #17451](https://github.com/unslothai/unsloth/pull/17451) addresses this gap.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*