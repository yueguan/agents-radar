# AI Infrastructure Digest 2026-11-09

> Generated: 2026-11-09 01:56 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-09**

---

### **1. Ecosystem Overview**  
The AI inference infrastructure landscape in late 2026 is defined by aggressive hardware specialization, convergence of serving engines with low-level kernel optimizations, and growing maturity in agent-native tooling. NVIDIA’s Blackwell (SM120/121) has become the primary focus for performance tuning across vLLM, SGLang, and llama.cpp, while AMD ROCm and Intel XPU are gaining traction through experimental support and MoE/FP8 enhancements. The ecosystem is rapidly shifting from monolithic inference backends toward modular, composable stacks—where gateways like LiteLLM orchestrate diverse engines, and local runtimes (e.g., llama.cpp) enable edge deployment. With increasing emphasis on speculative decoding, long-context stability, and multi-modal readiness, production-grade reliability is now a core differentiator.

---

### **2. Activity Comparison**

| Project       | Open Issues | Open PRs | Recent Release? | Status Notes |
|---------------|-------------|----------|------------------|--------------|
| **vLLM**      | 47          | 32       | No               | High-severity regressions on SM120; active kernel fixes |
| **SGLang**    | 52          | 29       | No               | Critical hangs in long-context inference; PD fully enabled |
| **llama.cpp** | 39          | 28       | No               | Fixing speculative decoding divergence & NaN issues |
| **Ollama**    | 43          | 31       | No               | AMD Vulkan regression persists; per-model config now merged |
| **LiteLLM**   | 46          | 38       | ✅ `v1.110.0-dev.2` | High-priority billing/streaming bugs under fix |
| **Unsloth**   | 37          | 26       | No               | ROCm segfaults and Intel Arc load failures remain critical |

> *Note: All projects report no new stable releases today. LiteLLM leads in release activity with a dev build.*

---

### **3. Model Support Race**

| New Model / Architecture        | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **NVIDIA SM120 (Blackwell)**     | ✅ (active debug) | ✅ (full FP8/MoE) | ✅ (Hopper/Q6_K opt) | ❌ | ❌ | ❌ |
| **AMD ROCm (Octave KV, 3-bit)**  | ✅ (experimental) | ⚠️ (limited) | ❌ | ❌ | ❌ | ❌ |
| **Intel XPU (MoE, Qwen3.5)**     | ✅ (ongoing port) | ❌ | ❌ | ❌ | ❌ | ✅ (fp64 fix) |
| **Qwen3-MoE / Qwen3.8-Flash-Next** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **FLUX.3 / Qwen-Image-2.1**      | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (enhanced CFG/dynamic pairing) |
| **Rockchip NPU (rknpu)**         | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Ascend 950 NPU / DSA**         | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **TopxAI / Kitani**              | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

> 🔥 **Winner: SGLang** — Leads in multi-hardware model support (RTX PRO 5000, Ascend), with full integration of emerging models like Longcat2.0 and Qwen3-MoE.  
> 🏆 **Runner-up: Ollama** — Best-in-class edge support via Rockchip NPU and Docker patching for MLX/Vulkan.

---

### **4. Performance Frontier**

| Optimization Focus           | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|-------------------------------|------|--------|-----------|--------|---------|---------|
| **KV Cache Efficiency**       | ✅ Octave KV (3-bit), lease renewal | ✅ LIM/CopySFA offload | ❌ | ✅ per-layer sizing | ❌ | ❌ |
| **MoE Kernel Fusion**         | ✅ TRT-LLM skip padding, RFC unify oracles | ✅ FlashInfer cuTile default | ✅ device-side scheduler | ❌ | ❌ | ❌ |
| **Speculative Decoding**      | ⚠️ draft-mtp determinism | ✅ MTP prefill graphs | ✅ fixed cache misalignment | ❌ | 🔴 streaming loss | ❌ |
| **Kernel-Level Tuning**       | ✅ CUDA graph stability, SM120 kernels | ✅ FlashInfer b12x warp-MMA | ✅ TMA/WGMMA Hopper | ❌ | ❌ | ✅ Triton JIT specialization |
| **Distributed Serving**       | ✅ Prefix caching, hybrid attention | ✅ Prefill-Decode Disaggregation | ❌ | ❌ | ✅ Router + Redis | ❌ |
| **Quantization Flexibility**  | ✅ Humming backend proposal | ✅ online W4A4 MoE | ✅ IQ2 speedup | ❌ | ❌ | ❌ |

> 💡 **Trend**: vLLM and SGLang are converging on **kernel-specific optimizations** (FP8, MoE, CUDA graphs), while **llama.cpp** excels in **low-level runtime robustness** (deterministic Top-K, memory leak fixes). **LiteLLM** focuses on **observability and cost control**, not raw throughput.

---

### **5. Layer Positioning**

| Project       | Primary Layer             | Key Differentiator |
|---------------|----------------------------|--------------------|
| **vLLM**      | Serving Engine (GPU)       | Deep hardware-specific kernels; MoE + prefix caching leader |
| **SGLang**    | Serving Engine + Agent Stack | Prefill-Decode Disaggregation (PD); multi-backend orchestration |
| **llama.cpp** | Local Runtime (CPU/GPU)    | Cross-platform portability; speculative decoding correctness |
| **Ollama**    | Gateway + CLI Runtime      | Per-model configuration, edge/NPU support, embeddable Modelfiles |
| **LiteLLM**   | LLM Gateway / Proxy Layer    | Multi-provider routing, budget tracking, stream reliability |
| **Unsloth**   | Diffusion + LLM Runtime    | First-render optimization; INT8 on T4; VAE efficiency |

> 🔍 **Positioning Insight**:  
> - **vLLM/SGLang** dominate high-throughput GPU inference.  
> - **Ollama/LiteLLM** serve as the "glue" layer between apps and engines.  
> - **llama.cpp/Unsloth** target developers needing cross-platform, deterministic, or edge execution.

---

### **6. Trend Signals**

#### **Emerging Industry Trends (from 2026-11-09 Activity):**
1. **Hardware Specialization Is Now Mandatory**: SM120/GB10 issues dominate bug reports — performance is no longer abstracted. Developers must choose engines aligned with their hardware stack.
2. **Long-Context Stability Is a Production Barrier**: Multiple projects report hangs at 1M tokens (SGLang), prompt truncation (vLLM), and memory corruption (llama.cpp). This signals that **long-context inference is still fragile outside lab conditions**.
3. **Agent Systems Demand Precision**: Speculative decoding divergence (llama.cpp), incorrect usage accounting (LiteLLM), and streaming leaks (SGLang) directly impact agent reliability — a red flag for production use.
4. **Multi-Hardware Support Is Becoming a Competitive Moat**: Projects with broad NPU/ROCm/XPU support (Ollama, SGLang) are gaining ground in embedded and enterprise edge deployments.
5. **Quantization Flexibility > Monolithic Backends**: The proposed shift to **Humming** (vLLM) and **IQ2 pattern caching** (llama.cpp) reflects a move toward **configurable, extensible quantization** rather than vendor lock-in.

#### **What Application Developers Should Watch:**
- ✅ **Avoid `v0.33.0` (vLLM)** and **v0.5.23 (SGLang)** if using Blackwell or long contexts — both have known critical regressions.
- ✅ **Monitor `#50125` (LiteLLM)** and `#25618` (llama.cpp): These affect **cost accuracy** and **output consistency** in agents.
- ✅ **Enable `num_parallel` (Ollama)** and `--enable-mfu-metrics` (vLLM) for better resource planning.
- ✅ **Test speculative decoding paths rigorously** — especially with quantized models (Q4_K_M, etc.) and non-BF16 targets.

> 📌 **Bottom Line**: The era of “plug-and-play” inference is over. Choose your stack based on **hardware alignment, long-context stability, and agent-grade correctness**, not just peak throughput.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-09

---

### **1. Today's Highlights**  
The vLLM project is advancing deep hardware-specific optimizations for NVIDIA Blackwell (SM120) and AMD ROCm, with critical work on CUDA graph stability, MoE kernel fusion, and multi-modality support. Key focus areas include fixing decode throughput regressions on RTX PRO 6000 Blackwell GPUs, improving prefix caching robustness across hybrid attention models, and enabling native 3-bit KV cache (Octave KV) for AMD GPUs.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API/config changes were introduced.

---

### **3. New Model & Hardware Support**  
- ✅ **NVIDIA SM120 (Blackwell)**: Active debugging and optimization for `DeepSeek-V4.1-Flash`, `Qwen3.8-Flash-Next`, and `Gemma-4` on 8x RTX PRO 6000 Blackwell (via #56892, #65139, #62776).  
- ✅ **AMD ROCm**: Experimental support for **Octave KV (3-bit KV cache)** via #59773; improvements to FP8 MoE fallback logic (#66350).  
- ✅ **Multi-modality**: RFC for full ViT CUDA graph support in multimodal models (e.g., Qwen3-VL, Kimi K2.5) — #38175.  
- ✅ **Streaming ASR**: Proposal to add support for **NVIDIA Nemotron 3.5 ASR Streaming (0.6b)** — #47455.  
- ✅ **Intel XPU**: Ongoing porting of Qwen3.5 models and MoE decode fusion to eager mode — #64687.

---

### **4. Performance & Optimization**  
- 🔥 **SM120 Decode Throughput Fix**: Critical performance regression on `DeepSeek-V4.1-Flash` with extremely low decode throughput on SM120 due to broken CUDA graphs — prioritized fix underway (#56892).  
- 🚀 **MoE Optimization**: PR #65972 skips unnecessary DP padding rows in TRT-LLM monolithic MoE kernels, reducing compute overhead when using vLLM’s top-k routing.  
- ⚙️ **KV Cache Efficiency**: PR #66443 introduces lease renewal and expiration handling in NIXL KV connector to avoid stalled transfers; improves reliability under high load.  
- 📈 **MoE Kernel Unification**: RFC #37753 aims to unify MoE "oracles" into a class-based structure for better maintainability and extensibility.  
- 💡 **Quantization Flexibility**: RFC #65560 proposes replacing Marlin with **Humming** as the default quantization backend across NVIDIA architectures (Hopper/Blackwell), supporting INT4/INT8 GPTQ/AWQ, FP8, MXFP8/MXFP4, etc.  

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Status |
|--------|------|-------------|--------|
| 🔴 High | #65139 | **CUDA illegal memory access** at MTP verify step on `v0.33.0` (SM120, NVFP4 MoE, fp8 KV, chunked prefill, prefix caching, MTP spec decode) — regression from `v0.32.0` | Open |
| 🔴 High | #54173 | **CUBLAS_STATUS_INTERNAL_ERROR / illegal memory access** in GDN path with prefix caching on GB10 (sm_121); `--no-async-scheduling` ineffective | Open |
| 🔴 High | #62776 | Prompt content beyond `indexer_budget` (~2K tokens) is invisible in `Qwen3.8-Flash-Next` on GB10 — model ignores input past 2K | Open |
| 🟡 Medium | #54308 | Qwen3-Next GDN decode collapses to repeated token (`!!!!!!...`) on non-Blackwell ROCm GPUs with `fp32 SSM cache` | Open |
| 🟡 Medium | #64874 | “Workspace is locked” AssertionError in `rocm_fp8_paged_mqa_logits` under high concurrency on GLM-5.3-Flash | Closed |
| 🟡 Medium | #32588 | Segment-level timestamps offset by ~0.5s per segment in long audio transcriptions (Whisper) | Closed |

> *Note: Several high-severity issues are tied to SM120/GB10 hardware and recent v0.33.0 release — developers using these platforms should monitor updates closely.*

---

### **6. What This Means for Application Developers**  
- If you're deploying **large MoE models (e.g., DeepSeek-V4.1, Qwen3.8-Flash-Next)** on **NVIDIA Blackwell (SM120)**, expect performance degradation until fixes land — use `--enforce-eager` temporarily if needed.  
- **Prefix caching with hybrid attention** (e.g., DSpark + EAGLE) remains unstable on complex models with ≥3 attention groups — test carefully and consider disabling until #51771 is resolved.  
- For **multi-modal inference**, prepare for upcoming ViT CUDA graph support (RFC #38175) to improve vision encoder throughput.  
- **AMD users** should track Octave KV (3-bit KV cache) progress (#59773) for potential 2–3x memory savings in long-context scenarios.  
- Avoid `llguidance` dependency if using older glibc (< 2.31) — it’s now unconditionally required in `v0.33.0` (Issue #66345).  
- Use `--enable-mfu-metrics` to get accurate weight byte estimates for custom quantization configs — now supported via #65143.

> 🔗 [GitHub Issues](https://github.com/vllm-project/vllm/issues) | [PRs](https://github.com/vllm-project/vllm/pulls)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

---

### **SGLang Digest — 2026-11-09**

#### **1. Today’s Highlights**  
The SGLang project continues to deepen its support for next-generation hardware, with major progress on **Blackwell (SM12x) kernel optimizations**, including new FP8 and MoE dispatch logic for RTX PRO 5000 and GB10 GPUs. Critical stability fixes address persistent hangs in long-context inference (e.g., GLM-5.3-Flash at 1M tokens) and CUDA coredumps from test workflows. A key roadmap initiative—**Prefill-Decode Disaggregation (PD)**—is now fully operational across diverse model families and parallel strategies.

#### **2. Releases & Breaking Changes**  
*No new releases or breaking changes reported in the last 24 hours.*  
However, ongoing work in `#49173` highlights a regression in DeepSeek-V4 prefill performance on GB10 due to incorrect SM120/SM121 indexing in DSV4.1, which may require patching before v0.5.24.

#### **3. New Model & Hardware Support**  
- **Hardware**: Full support for **RTX PRO 5000 (SM120)** via tuned FP8 GEMM configs (`#45263`) and **Ascend 950 NPU** with explicit compressor state handling (`#49000`).  
- **Models**: Added **Longcat2.0** support for Ascend arch22 NPU (`#47917`), and **Qwen3-MoE** now correctly handles quantized router states (`#49185`).  
- **Backends**: Native **LIM/CopySFA KV offload** added for Ascend DSA (`#47383`); **FlashInfer cuTile NVFP4 MoE** is being prioritized as default on SM12x (`#49123`).  
- **Quantization**: Expanded **FP8 MoE support on XPU** (`#43994`) and **online W4A4 MoE** on Ascend (`#32602`).

#### **4. Performance & Optimization**  
- **DeepSeek-V4 Prefill**: On 4× RTX PRO 6000 (SM120, TP=4), throughput is ~2–7K tok/s vs. vLLM/Marlin’s 12.5K — prompting investigation into backend tuning (`#33422`).  
- **CUDA Graphs**: Fix for `HybridLinearAttnBackend` failure during full prefill graph capture (`#48593`), enabling stable graph reuse.  
- **Kernel Optimizations**:  
  - FlashInfer b12x warp-MMA used for block-fp8-as-MXFP8 on SM12x (`#45262`) improves dense linear performance.  
  - Triton FA3 kernel on H200 shows 1.5–3% slower forward than flash-attn hopper build (`#47734`).  
  - Intel AMX attention kernels optimized by moving SWA index translation into Triton (`#48861`).  

#### **5. Stability & Regressions**  
| Issue | Severity | Status | Notes |
|------|----------|--------|-------|
| [`#48964`](https://github.com/sgl-project/sglang/issues/48964) | Critical | Open | Engine state corruption after 1M-token request on GLM-5.3-Flash (v0.5.23); v0.5.22 is clean. |
| [`#33549`](https://github.com/sgl-project/sglang/issues/33549) | Critical | Closed | Decode hang at ~245K context on 8×H20; watchdog kills server. |
| [`#49133`](https://github.com/sgl-project/sglang/issues/49133) | High | Open | Breakable prefill CUDA graphs produce different logprobs vs eager mode (Qwen3.8-27B-NVFP4). |
| [`#49173`](https://github.com/sgl-project/sglang/issues/49173) | High | Open | Regression in DSV4.1 prefill on GB10 due to dropped SM120/SM121 from indexer — likely causing 8× slowdown. |
| [`#26340`](https://github.com/sgl-project/sglang/issues/26340) | High | Open | Auto-collected CUDA coredumps from CI — indicates underlying runtime instability. |

#### **6. What This Means for Application Developers**  
- **For high-throughput inference**: Prioritize **SM12x-specific kernels** (e.g., FlashInfer cuTile MoE, tuned FP8) and avoid `--cuda-graph-config {"prefill": "full"}` on hybrid attention models until `#48593` is resolved.  
- **For long-context apps**: Avoid v0.5.23 if using >100K token inputs; use v0.5.22 or patch `#48964`. Monitor `#49173` for regressions in DeepSeek-V4 prefill.  
- **For agent systems**: Enable `--hicache-mamba-size-gb` (`#47867`) to control Mamba host pool size and prevent cache misses. Use `#48963` to optimize shared prompt caching in batched prefill.  
- **For multi-modal apps**: Ensure image placeholders are properly escaped to avoid 500 errors (`#37579`), and defer media cleanup in streaming sessions (`#49207`).

> 🔗 *Full issue tracking: [sgl-project/sglang Issues](https://github.com/sgl-project/sglang/issues)*  
> 🔗 *PRs in flight: [sgl-project/sglang PRs](https://github.com/sgl-project/sglang/pulls)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-09**

---

### **1. Today's Highlights**  
The latest updates focus on speculative decoding stability, MoE optimization, and GPU backend robustness. Critical fixes address deterministic Top-K selection in CUDA/Metal backends and NaN persistence in recurrent states, while new PRs accelerate IQ2 quantization and enable device-side MoE scheduling for SYCL. These changes improve reliability for production inference pipelines.

---

### **2. Releases & Breaking Changes**  
No new tagged releases were published today. However, several high-impact changes landed via PRs:
- **`ggml` radix Top-K now ensures deterministic tied selections** across CUDA and Metal backends ([#31084](https://github.com/ggml-org/llama.cpp/pull/31084)) — affects `--spec-type draft-mtp` behavior.
- **MoE expert cache CPU wait fix**: Prevents unnecessary blocking during cached expert access ([#31888](https://github.com/ggml-org/llama.cpp/pull/31888)).
- **Speculative decoding no longer applies MoE cache to draft model**, reducing misalignment risk ([#31497](https://github.com/ggml-org/llama.cpp/pull/31497)).

> 🔔 *Developers using speculative decoding with quantized models (e.g., Q4_K_M) should verify output consistency after these changes.*

---

### **3. New Model & Hardware Support**  
- **Apple Silicon (arm64)**: Continued support with optimizations for SpacemiT IME2 MXFP4 dense repack ([#31776](https://github.com/ggml-org/llama.cpp/pull/31776)).
- **Intel Arc B-series (Vulkan)**: Ongoing work addressing persistent output corruption under long-context RAG prompts ([#31828](https://github.com/ggml-org/llama.cpp/issues/31828)); workaround `GGML_VK_DISABLE_COOPMAT` still recommended.
- **SYCL Backend**: New device-side MoE scheduler added ([#30156](https://github.com/ggml-org/llama.cpp/pull/30156)), enabling GPU-native routing for sparse MoE models.
- **Quant Formats**: 
  - BF16 and `q8_0` VXE support in `llamafile` ([#31866](https://github.com/ggml-org/llama.cpp/pull/31866)).
  - IQ2 quantization speedup via pattern caching ([#31910](https://github.com/ggml-org/llama.cpp/pull/31910)).

---

### **4. Performance & Optimization**  
- **Hopper (H100) Q6_K prefill path**: Native kernel optimized using TMA and WGMMA for faster dense quantized inference ([#31916](https://github.com/ggml-org/llama.cpp/pull/31916)).
- **CUDA MoE Upload Stream**: Optional separate stream for H2D expert copies reduces contention; diagnostics via `GGML_CUDA_UPLOAD_STREAM=1` ([#31850](https://github.com/ggml-org/llama.cpp/pull/31850)).
- **IQ2 Quant Creation Speedup**: Caching repeated patterns improves creation time across multiple models without changing output ([#31910](https://github.com/ggml-org/llama.cpp/pull/31910)).
- **Vulkan**: Added fast path for `d_state=64` in SSM_SCAN kernel (critical for Zamba2-style models) ([#31818](https://github.com/ggml-org/llama.cpp/pull/31818)).

---

### **5. Stability & Regressions**  
Critical issues reported today include:
- **Speculative decoding divergence** under greedy sampling (`temperature=0`) when target is quantized (e.g., Q4_K_M), matching only on BF16 targets ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618), 39 comments).
- **Persistent NaN in recurrent state** due to multiply-by-zero reset logic — can cause silent degradation over time ([#31913](https://github.com/ggml-org/llama.cpp/pull/31913)).
- **Memory leak** with dual Intel B70 SYCL backend during token generation ([#27845](https://github.com/ggml-org/llama.cpp/issues/27845), 23 comments).
- **Vulkan output corruption** on Intel Arc B-series even with `GGML_VK_DISABLE_COOPMAT` workaround ([#31828](https://github.com/ggml-org/llama.cpp/issues/31828)).

> ✅ *Fixes exist for `NaN` and `Top-K` determinism; speculative decoding issue remains open but actively researched.*

---

### **6. What This Means for Application Developers**  
- **Use `--spec-type draft-mtp` with caution** on quantized models — ensure outputs match vanilla runs by testing with `bf16` as baseline.
- **Enable `GGML_CUDA_POOL_DEBUG=1`** for debugging CUDA memory fragmentation in complex deployments.
- **Leverage device-side MoE scheduling** (SYCL) or separate upload streams (CUDA) for better throughput in large MoE models.
- **Avoid long-context RAG flows on Intel Arc B-series Vulkan** until #31828 is resolved; consider fallback to `GGML_VK_DISABLE_COOPMAT`.
- **Implement custom tool schema enforcement** via PRs like [#31612](https://github.com/ggml-org/llama.cpp/pull/31612) and [#31915](https://github.com/ggml-org/llama.cpp/pull/31915) for reliable JSON tool calls in Gemma 4/Qwen3.

> 📌 *Monitor #25618 and #31913 closely — they directly impact correctness in agent systems relying on speculative decoding and long-running sessions.*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-09**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to evolve with key improvements in model loading efficiency and backend support, particularly for emerging hardware like Rockchip NPUs and NVIDIA MLX/Vulkan. Critical bug fixes include a typo in `gemma4:12b-it-qat` metadata and a regression affecting AMD Radeon 780M Vulkan users. Developers are increasingly requesting granular control over concurrency and load timeouts—indicating growing adoption in production-grade AI workflows.

---

### **2. Releases & Breaking Changes**  
*No new releases in the last 24 hours.*  
However, ongoing work on per-model configuration is nearing completion:
- **PR #9546** (merged) adds `num_parallel` support in Modelfiles to override global `OLLAMA_NUM_PARALLEL`, enabling fine-grained concurrency control per model.
- **PR #16711** introduces `PARAMETER env` in Modelfiles to set environment variables per model—critical for isolating runtime behavior across models.

> 🔗 [PR #9546](https://github.com/ollama/ollama/pull/9546) | [PR #16711](https://github.com/ollama/ollama/pull/16711)

---

### **3. New Model & Hardware Support**  
- ✅ **Rockchip NPU (rknpu)**: PR #19480 adds native backend support for RK3588/RK3576 SoCs on Linux, enabling offload of prefill and MoE operations via GGUF models.
- ✅ **NVIDIA MLX/Vulkan Docker**: PR #18592 patches missing libraries/configs in Docker containers, restoring functionality for `vulkan` and `mlx` runners on NVIDIA GPUs.
- 🚧 **OSADIA Axiom Architecture**: Issue #19611 requests official support for Next GenAi Mexico’s proprietary neural architecture, signaling rising interest in custom inference backends.

> 🔗 [PR #19480](https://github.com/ollama/ollama/pull/19480) | [PR #18592](https://github.com/ollama/ollama/pull/18592) | [Issue #19611](https://github.com/ollama/ollama/issues/19611)

---

### **4. Performance & Optimization**  
- **KV Cache Sizing**: PR #19327 improves VRAM estimation by sizing KV cache per layer based on actual head counts—preventing over-allocation in heterogeneous models (e.g., MoE).
- **Memory Parsing**: PR #19324 ensures repeated buffer lines within a single load are aggregated, avoiding loss of critical data during model initialization.
- **Benchmarking Reliability**: PR #19600 enhances `cmd/bench` to fail incomplete runs and truncate reused output files—improving consistency in automated performance testing.

> 🔗 [PR #19327](https://github.com/ollama/ollama/pull/19327) | [PR #19324](https://github.com/ollama/ollama/pull/19324) | [PR #19600](https://github.com/ollama/ollama/pull/19600)

---

### **5. Stability & Regressions**  
| Severity | Issue | Summary | Fix Status |
|--------|------|--------|----------|
| ⚠️ High | #17748 ([AMD Vulkan regression](https://github.com/ollama/ollama/issues/17748)) | `radv/amdgpu: Not enough memory for command submission` on Radeon 780M post v0.32.10 | ❌ Open |
| ⚠️ Medium | #19596 ([Typo in gemma4:12b-it-qat](https://github.com/ollama/ollama/issues/19596)) | `toos` instead of `tools` in metadata → tool call failure | ✅ Fixed (PR pending) |
| ⚠️ Medium | #19604 ([403 Forbidden behind proxy](https://github.com/ollama/ollama/issues/19604)) | Pull fails even when `curl` works; corporate Blue Coat CA issue | ❌ Open |
| ⚠️ Low | #19613 ([`--version` shows warnings](https://github.com/ollama/ollama/issues/19613)) | Unexpected "could not connect" warning on version check | ❌ Open |

> 🔗 [Issue #17748](https://github.com/ollama/ollama/issues/17748) | [Issue #19596](https://github.com/ollama/ollama/issues/19596) | [Issue #19604](https://github.com/ollama/ollama/issues/19604)

---

### **6. What This Means for Application Developers**  
- **Concurrency Control Is Now Per-Model**: With `num_parallel` in Modelfiles, you can now run high-concurrency models (e.g., LLMs) alongside low-latency embedding models without resource contention. Use this to optimize throughput in multi-model services.
- **Longer Load Times? Increase Timeout**: The default `OLLAMA_LOAD_TIMEOUT=300s` is too short for complex models or slow storage (Issue #19616). Adjust it in your deployment configs if you see premature load failures.
- **Security & Exposure Risks**: Exposing Ollama via public proxies (e.g., Cloudflare) without API auth invites abuse (Issue #19595). Always gate access with authentication or IP whitelisting.
- **Embedding Dimension Discovery**: Use `ollama show <model>` to query dimensions (PR #19612 clarifies docs)—essential for vector database integration.

> 🔗 [PR #19612](https://github.com/ollama/ollama/pull/19612) | [Issue #19616](https://github.com/ollama/ollama/issues/19616) | [Issue #19595](https://github.com/ollama/ollama/issues/19595)

---  
*Digest generated: 2026-11-09 | Source: github.com/ollama/ollama*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-11-09**

---

### **1. Today's Highlights**  
The LiteLLM ecosystem continues to mature with significant improvements in observability and reliability for production-grade LLM proxy deployments. Key updates include enhanced budget tracking, improved stream handling for Bedrock and DeepSeek, and new provider integrations (Kitani, TopxAI). Critical fixes address streaming data loss, incorrect usage accounting, and security vulnerabilities in off-origin URL handling.

---

### **2. Releases & Breaking Changes**  
- **v1.110.0-dev.2**: Released today with updated Docker image signing via cosign. Verified using key from [commit `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065).  
  - *Note*: This is a development release; no breaking changes expected in the stable branch yet.  
  - Ensure you verify signatures before deployment: [cosign verification guide](https://docs.sigstore.dev/cosign/overview/).

---

### **3. New Model & Hardware Support**  
- ✅ **TopxAI** added as an officially supported OpenAI-compatible provider via [PR #41919](https://github.com/BerriAI/litellm/pull/41919), enabling cost tracking and routing for 11 text models.  
- ✅ **Kitani** now integrated as a first-class provider ([PR #50656](https://github.com/BerriAI/litellm/pull/50656)), supporting open-weight models via OpenAI-compatible API endpoints.  
- 🔧 **DeepSeek Fetch Tool** support added ([PR #50618](https://github.com/BerriAI/litellm/pull/50618)) alongside existing Search tool — critical for agent workflows using external data retrieval.

> *No new hardware backends (CUDA/ROCm/Metal/CPU) or quantization formats introduced today.*

---

### **4. Performance & Optimization**  
- 🚀 **Router optimization**: PR [#50641](https://github.com/BerriAI/litellm/pull/50641) reduces hot-path overhead by skipping unused deployment reads and caching Anthropic capability probes — directly improving latency under high concurrency.  
- ⚡ **Redis efficiency**: PR [#31880](https://github.com/BerriAI/litellm/pull/31880) eliminates unnecessary Redis writes for keys without rate limits, reducing I/O load at scale.  
- 📊 **Spend logging perf**: PR [#49556](https://github.com/BerriAI/litellm/pull/49556) adds regression test coverage for key worker cancellation and Redis replay scenarios, preventing double-charging during failures.

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR |
|--------|------|--------|-------|
| 🔴 High | Streaming usage severely undercounted despite correct non-streaming counts (`#36114`) | Open | In progress – root cause in stream aggregation layer |
| 🔴 High | `/v1/messages` stream on Bedrock keeps generating after client disconnects (`#50514`) | Open | Fixed in [PR #50657](https://github.com/BerriAI/litellm/pull/50657) |
| 🔴 High | `output_config` causes failure with Xiaomi MiMo models (`#24549`) | Closed | Patched in dev branch |
| 🟡 Medium | `ModelNotMappedError` raised without context (`#47922`) | Open | No fix yet – needs better error reporting |
| 🟡 Medium | Windows wheel ships tiktoken cache with CRLF line endings (`#41855`) | Open | Requires network access on import – workaround needed |

> *Critical bugs affecting billing accuracy and stream integrity are being actively addressed.*

---

### **6. What This Means for Application Developers**  
- **Use caution with `max_budget` and `rate_limit` settings**: The `#50125` bug shows budget checks can admit calls past limit — validate your thresholds carefully in production.  
- **Ensure proper stream handling when using Bedrock + agents**: The fix for `#50514` via [PR #50657](https://github.com/BerriAI/litellm/pull/50657) should be applied if you're using detached-drain aborts with `ANTHROPIC_MESSAGES_MAX_DETACHED_STREAM_DRAINS=0`.  
- **Leverage new providers immediately**: Integrate Kitani and TopxAI with minimal config changes — ideal for cost-optimized inference on open-weight models.  
- **Enable `turn_off_message_logging` + `redact_message_content`**: Use [PR #50622](https://github.com/BerriAI/litellm/pull/50622) to protect PII in debug logs while retaining full observability.  
- **Monitor spend metrics closely**: Discrepancies between Prometheus/Grafana and LiteLLM UI (`#26611`) indicate potential gaps in data aggregation — consider reconciling via `SpendLogDelivery` scans.

> *For agent developers: Avoid `output_config` with Xiaomi MiMo models until v1.110.0 is released. Test streamed responses through DeepSeek’s `Fetch` tool path thoroughly.*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-09**

---

### **1. Today's Highlights**  
Unsloth continues its deep optimization push for diffusion and LLM inference, with critical fixes to fp8/INT8 performance on Turing GPUs (T4) and Intel Arc A-series, alongside robust improvements in model loading reliability across diverse hardware. Key PRs include enabling INT8 on T4 via `UNSLOTH_DIFFUSION_INT8_GEMM_RACE`, resolving float64 support issues on Intel XPU, and stabilizing first-render compilation for Qwen-Image-2.1 and FLUX.3.

---

### **2. Releases & Breaking Changes**  
*No new releases or breaking changes detected in the last 24 hours.*  
Existing behavior remains unchanged; no migration notes required.

---

### **3. New Model & Hardware Support**  
- ✅ **Intel Arc A-series (Arc 140V)**: Fixes now address model load failures due to missing `float64` support (`PR #17070`).  
- ✅ **Turing GPUs (T4, sm75)**: INT8 quantization is now supported via `UNSLOTH_DIFFUSION_INT8_GEMM_RACE=0` (`PR #17167`) — previously refused outright.  
- ✅ **Qwen-Image-2.1 & FLUX.3**: Enhanced compatibility with dynamic CFG pairing (`PR #17177`) and improved handling of bf16 weights on fp16-only devices (`PR #17173`).  
- ✅ **MiniMax-H3 Sol VAE**: Optimized device reads and window mask logic (`PR #17085`, `#17100`) to reduce host-GPU sync overhead.

---

### **4. Performance & Optimization**  
- 🚀 **INT8 GEMM/GEMV**: Fixed race conditions in unlisted GPU cards (`PR #17084`, `#17106`) and optimized tiled memory usage for grouped linear layers.  
- ⚙️ **Triton Kernel Specialization**: Wan fused blocks now compile one binary per sequence length (`PR #17101`), reducing redundant JIT compilation.  
- 🔍 **Memory Efficiency**: Reduced device-to-host reads by 192× in MiniMax-H3 VAE (`PR #17100`) and eliminated redundant KV cache reads in Qwen-Image-2.1 (`PR #17118`).  
- 💡 **FlashInfer Prebuilds**: Installed prebuilt kernels into vLLM engine env (`PR #17063`) to eliminate JIT compilation delay on first launch.

---

### **5. Stability & Regressions**  
- ⚠️ **Critical**: `unsloth-rocm` crashes on gfx1031 (RX 6700 XT) after misidentifying RDNA 2 → segfaults on first GPU op (`Issue #17147`). *Fix pending.*  
- ⚠️ **High**: Image generation models fail to load on Intel Arc 140V (`Issue #17137`). *Root cause linked to fp64 support (`PR #17070`).*  
- ⚠️ **Medium**: FP8 denoisers hang on first render under torch < 2.12 (`Issue #17166`). *Fixed in `PR #17166`.*  
- ⚠️ **Low**: UI inconsistencies in Appearance tab (duplicate settings, picker auto-close) (`Issues #17161`, `#17162`, `#17163`, `#17169`). *PRs in progress.*

---

### **6. What This Means for Application Developers**  
- **Enable INT8 on T4 and older GPUs** via `UNSLOTH_DIFFUSION_INT8_GEMM_RACE=0` — unlock efficient inference on legacy hardware.  
- **Avoid `tool_calls=None` in datasets** — use `PR #17176` to safely handle null tool calls in Llama 3.x/GPT-OSS templates.  
- **Expect more stable diffusion pipelines** on Intel Arc and AMD ROCm with recent fixes; test with `FLUX.3` and `Qwen-Image-2.1` using updated config.  
- **Monitor UI stability**: Multiple appearance-related bugs may affect user experience in Studio — consider reporting or contributing fixes.  

> 🔗 [GitHub Issues](https://github.com/unslothai/unsloth/issues) | [GitHub PRs](https://github.com/unslothai/unsloth/pulls)

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*