# AI Infrastructure Digest 2026-11-10

> Generated: 2026-11-10 04:17 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

---

### **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-10**

---

#### **1. Ecosystem Overview**  
The AI infrastructure landscape in late 2026 is defined by a sharp bifurcation between **high-performance inference engines** (vLLM, SGLang) and **developer-friendly gateways/local runtimes** (Ollama, llama.cpp, LiteLLM), with Unsloth emerging as a hybrid tooling layer bridging local execution and agent workflows. The rollout of NVIDIA’s Blackwell SM120 GPUs has intensified hardware-specific tuning efforts, particularly around MoE models, FlashInfer workspace optimization, and CUDA graph stability. Meanwhile, multi-model support, structured outputs, and agentic capabilities are driving convergence in API compatibility and deployment flexibility across the stack.

---

#### **2. Activity Comparison**

| Project       | Issues Open | PRs Merged (Last 24h) | Releases (Last 24h) | Breaking Changes |
|---------------|-------------|-------------------------|------------------------|------------------|
| **vLLM**      | 187         | 9                       | None                   | None             |
| **SGLang**    | 234         | 5                       | None                   | Yes (v0.5.23)    |
| **llama.cpp** | 321         | 12                      | 10 (b12233–b12242)     | Yes (API fix)    |
| **Ollama**    | 198         | 4                       | 1 (v0.40.16)           | Yes (VRAM fix)   |
| **LiteLLM**   | 267         | 6                       | 1 (v1.108.4 LTS)       | Yes (rate limiter) |
| **Unsloth**   | 172         | 6                       | None                   | None             |

> *Note: High issue volume reflects active debugging of next-gen hardware (SM120, gfx1151). SGLang and llama.cpp show strongest momentum in PR velocity.*

---

#### **3. Model Support Race**  
| New Model / Architecture | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1-Flash**  | ✅ (SM120 issues) | ⚠️ (High regression) | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.8-Flash-Next**   | ✅ (gfx950/MI355X) | ✅ (ROCm fixes) | ✅ (WebGPU/Metal) | ✅ (memory gain) | ✅ (cost map) | ✅ (FLUX.3) |
| **NVFP4 MoE (Blackwell)**| ⚠️ (Critical bugs) | ⚠️ (FA4 crash) | ❌ | ❌ | ❌ | ✅ (pre-Blackwell fallback) |
| **Gemma 4 (MTMD audio)** | ❌ | ❌ | ✅ (real-time stream) | ❌ | ❌ | ❌ |
| **Ripple Preview (65B)** | ❌ | ❌ | ❌ | ✅ (new model) | ❌ | ❌ |
| **FLUX.3**               | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (strided attention) |

> **Winner**: *Ollama* leads in new model availability (Ripple Preview), while *llama.cpp* dominates edge device support (Metal, WebGPU, SpacemiT). *Unsloth* shows early strength in pre-Blackwell and ROCm model resilience.

---

#### **4. Performance Frontier**  
Optimization efforts are increasingly concentrated in five key areas:
- **KV Cache & Memory Management**: vLLM reduces FlashInfer workspace by 92% (394 → 32 MiB), boosting capacity; SGLang fixes `UnifiedRadixCache` use-after-free.
- **Quantization & Kernel Efficiency**: vLLM pushes Humming backend to replace Marlin; llama.cpp adds MXFP4 on SpacemiT; SGLang optimizes DSpark for Hopper.
- **Batching & Parallelism**: Ollama enables `OLLAMA_NUM_PARALLEL` for MLX; LiteLLM improves streaming reuse via choice object caching.
- **Distributed Serving**: SGLang and vLLM lead in multi-GPU coordination (ROCm/gfx950, SM120), though both face stability regressions.
- **Kernel-Level Tuning**: All projects are targeting narrow matrix ops (Vulkan, Intel GPU), attention kernels (FA4 vs Triton), and speculative decoding efficiency.

> **Top Focus**: *vLLM* and *SGLang* are most advanced in kernel-level optimizations for SM120 and ROCm, but trade-offs in correctness persist.

---

#### **5. Layer Positioning**  

| Project       | Primary Layer                     | Role Summary |
|---------------|-----------------------------------|--------------|
| **vLLM**      | **Inference Engine**              | High-throughput, low-latency serving; dominant in cloud-scale LLM deployments. |
| **SGLang**    | **Inference Engine + Agent Runtime** | Combines engine features with agent orchestration; strong in speculative decoding and multi-GPU workloads. |
| **llama.cpp** | **Local Runtime / Edge Inference** | Optimized for CPU/GPU/edge devices; ideal for offline, privacy-sensitive, or heterogeneous environments. |
| **Ollama**    | **Developer Gateway / Local Runtime** | User-first interface with model library and CLI; bridges dev experience and local inference. |
| **LiteLLM**   | **LLM Gateway / Orchestration Layer** | Multi-provider routing, cost tracking, rate limiting — critical for production API stacks. |
| **Unsloth**   | **Agent Tooling & UX Layer**       | Focuses on image gen, model management, and UI polish; positions itself as a "local agent studio." |

> **Strategic Differentiation**: LiteLLM and Ollama serve as abstraction layers; vLLM/SGLang are performance-critical engines; llama.cpp and Unsloth target edge/local autonomy.

---

#### **6. Trend Signals**  
Key industry trends emerging from today’s activity:
- **Hardware-Specific Fragmentation**: SM120 (Blackwell) and gfx1151 (AMD Strix Halo) are becoming distinct performance domains—no single project is fully stable across both. Developers must validate on target hardware early.
- **Quantization Wars**: vLLM’s shift from Marlin to Humming signals a move toward unified, modern backends supporting MXFP4/FP8 across architectures.
- **Agent-Centric Optimization**: Ripple Preview, DeepSeek-V4 speculative decoding, and sd.cpp image generation highlight growing demand for end-to-end agent pipelines.
- **Cost & Compliance Visibility**: LiteLLM’s per-customer RPM/TPM limits and accurate cost tracking reflect rising need for financial governance in multi-tenant systems.
- **Security at the Edge**: Use-after-free vulnerabilities (SGLang #49361) and VRAM accounting bugs (Ollama #19631) underscore that reliability is now a compliance concern.

> **Actionable Guidance for DevOps & Engineers**:
> - Avoid `--enforce-eager` on SM120 until vLLM resolves #56892.
> - Do not upgrade SGLang to `v0.5.23` if using speculative decoding on Hopper.
> - Validate audio, vision, and structured output pipelines on llama.cpp or Ollama before production.
> - Use `litellm v1.108.4` with caution: recheck all `model_rpm_limit` settings post-upgrade.
> - Monitor Unsloth’s `sd.cpp` retry logic for robustness in constrained environments.

---

*Prepared by the AI Systems Analysis Team | 2026-11-10*

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-11-10**

---

#### **1. Today's Highlights**  
The vLLM project is intensifying its focus on **Blackwell SM120 support**, with critical performance and correctness issues reported for DeepSeek-V4.1-Flash and NVFP4 MoE models on RTX PRO 6000 Blackwell GPUs. A major RFC (#65560) proposes replacing Marlin with the more modern, architecture-agnostic **Humming quantization backend** across NVIDIA architectures. Meanwhile, ROCm/AMD efforts are advancing with targeted optimizations for Qwen3.8-Flash-Next and gfx950/MI355X platforms.

---

#### **2. Releases & Breaking Changes**  
*None.* No new releases were published in the past 24 hours. No breaking changes or migration notes announced.

---

#### **3. New Model & Hardware Support**  
- **SM120 (RTX PRO 6000 Blackwell)**: Critical support gaps identified for `DeepSeek-V4.1-Flash` and `NVFP4 MoE` models; CUDA graphs unusable, decode throughput severely degraded when `--enforce-eager` is used.  
- **ROCm/gfx950 / MI355X**: Active optimization campaigns underway for `Qwen3.8-Flash-Next-Quark-MXFP4` and `Qwen3.8-2.4T-A95B-Quark-MXFP4` ([Issue #57149](https://github.com/vllm-project/vllm/issues/57149), [#59575](https://github.com/vllm-project/vllm/issues/59575)).  
- **NVIDIA Nemotron 3.5 ASR Streaming**: Feature request filed to support `nemotron-3.5-asr-streaming-0.6b` ([Issue #47455](https://github.com/vllm-project/vllm/issues/47455)).

---

#### **4. Performance & Optimization**  
- **SM120 FlashInfer Workspace Sizing**: Reducing workspace from 394 MiB to 32 MiB increased KV-cache capacity by **21.27%** under same memory budget ([Issue #63399](https://github.com/vllm-project/vllm/issues/63399)).  
- **Speculative Decoding Efficiency**: PR #66534 introduces early top-k/p cutoff application during target logits load—potentially reducing verification overhead.  
- **Quantization Backend Shift**: RFC #65560 advocates replacing legacy **Marlin** with **Humming**, which supports newer formats (MXFP4, MXFP8, FP8) and all NVIDIA architectures including Hopper and Blackwell.  
- **MoE Kernel Unification**: RFC #37753 seeks to unify MoE "oracles" via class-based design for better maintainability and extensibility.  

---

#### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|-------|-------------|------------|
| ⚠️ High | [Issue #56892](https://github.com/vllm-project/vllm/issues/56892) | Extreme decode throughput drop on SM120 with `--enforce-eager`; CUDA graphs unusable | ❌ No fix yet |
| ⚠️ High | [Issue #51525](https://github.com/vllm-project/vllm/issues/51525) | Explicit `VLLM_CUTLASS` NVFP4 MoE selection silently returns empty completions on SM120 | ❌ No fix yet |
| ⚠️ High | [Issue #66141](https://github.com/vllm-project/vllm/issues/66141) | ROCm `aiter` MLA kernel returns uncorrelated output for `gqa_ratio=16`, `decode_qlen=3–4` | ✅ Partial fix in PR #66710 |
| ⚠️ Medium | [Issue #53670](https://github.com/vllm-project/vllm/issues/53670) | EAGLE/MTP prefix-cache last-block drop causes 1,648-token recompute per hit (~30–40% batch throughput loss) | ❌ In progress |
| ⚠️ Medium | [Issue #66070](https://github.com/vllm-project/vllm/issues/66070) | Qwen3.5 vision RoPE angle precision differs from HF (reproduction available) | ❌ No fix |

---

#### **6. What This Means for Application Developers**  
- **Avoid `--enforce-eager` on SM120 GPUs** until #56892 is resolved—expect severe performance degradation. Use eager mode only if necessary, and monitor for silent failures.  
- **Use Humming instead of Marlin** where possible: expect better performance and broader hardware coverage, especially on Hopper/Blackwell. Watch for #65560’s rollout.  
- **Validate model outputs rigorously** when using speculative decoding or MoE models—known correctness issues exist in both MTP and NVFP4 paths.  
- **Enable metrics tracking** (`--enable-mfu-metrics`) to monitor weight layout efficiency, especially when using custom quantization configs ([PR #65143](https://github.com/vllm-project/vllm/pull/65143)).  
- **For AMD deployments**, ensure your CI/CD pipelines test on gfx950/MI355X hardware—performance gains are being actively pursued, but stability remains a focus.

--- 

*Data source: github.com/vllm-project/vllm | Updated: 2026-11-10*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

---

### **SGLang Digest — 2026-11-10**

#### **1. Today's Highlights**  
The SGLang project continues to deepen its support for next-generation hardware, with critical stability fixes for SM120 (Blackwell) and AMD gfx1151 devices, including crashes in attention backends and use-after-free vulnerabilities. Key performance work includes optimizations for Qwen3.8-27B on Hopper via DSpark, and a major regression fix for DeepSeek-V4 prefill throughput on SM120. The community is actively addressing flaky CI infrastructure and testing bottlenecks across diverse platforms.

#### **2. Releases & Breaking Changes**  
*No new releases were published in the last 24 hours.*  
However, **v0.5.23** has been identified as introducing a **33–45% decode latency regression** at concurrency 12 on Hopper (SM90), specifically when using `use_deep_select_decode`. This impacts users relying on speculative decoding with DeepSeek-V4.1-Flash models — upgrade caution advised pending patch resolution ([#49336](https://github.com/sgl-project/sglang/issues/49336)).

#### **3. New Model & Hardware Support**  
- **AMD gfx1151 (Strix Halo)**: Critical bug fixes for `decode_attention_fwd(..., has_mla=True)` and deterministic inference (`--enable-deterministic-inference`) now under review ([#48967](https://github.com/sgl-project/sglang/issues/48967), [#48968](https://github.com/sgl-project/sglang/issues/48968)).  
- **Cambricon MLU Backend**: A roadmap issue tracks initial integration efforts ([#45615](https://github.com/sgl-project/sglang/issues/45615)), with no PRs yet merged.  
- **Kimi-K3.1-Pro MI35x**: Documentation updated to align inference recipes with InferenceX benchmarks ([#49453](https://github.com/sgl-project/sglang/pull/49453)).  
- **SM120 (RTX 50-series Blackwell)**: Several issues highlight runtime failures during model loading and CUDA graph capture, particularly with GLM-5.3-Flash and DeepSeek-V4 ([#42012](https://github.com/sgl-project/sglang/issues/42012), [#49209](https://github.com/sgl-project/sglang/issues/49209)).

#### **4. Performance & Optimization**  
- **DeepSeek-V4 Pre-fill Throughput**: Reports indicate **~2–7K tok/s** on 4× RTX PRO 6000 (SM120) vs. **~12.5K tok/s** on vLLM/Marlin — suggesting significant tuning or kernel optimization opportunities ([#33422](https://github.com/sgl-project/sglang/issues/33422)).  
- **Qwen3.8-27B on Hopper**: A regression in `v0.5.23` increased per-step decode time by **33–45%** at concurrency 12 across 8K–64K context lengths — impacting speculative decoding workflows ([#49336](https://github.com/sgl-project/sglang/issues/49336)).  
- **Diffusion Optimization**: PRs aim to improve SANA-Video 2.0 and Qwen-Image-Edit performance by offloading RoPE computation to FlashInfer kernels and enabling breakable CUDA graphs ([#41501](https://github.com/sgl-project/sglang/pull/41501), [#49448](https://github.com/sgl-project/sglang/pull/49448)).  
- **AMD GFX950**: Optimizations for fused indexer logits and top-k selection in MTP verify rows are in progress ([#47276](https://github.com/sgl-project/sglang/pull/47276)).

#### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|--------|------|-------------|------------|
| 🔴 High | [#49336](https://github.com/sgl-project/sglang/issues/49336) | **Decode latency regression** in `v0.5.23` (+33–45%) on Hopper with DSpark | No fix PR yet |
| 🔴 High | [#42012](https://github.com/sgl-project/sglang/issues/42012) | **FA4 attention backend crash** on SM120 during hybrid extend reshape; only Triton works | No fix PR yet |
| 🔴 High | [#49361](https://github.com/sgl-project/sglang/issues/49361) | **Use-after-free in UnifiedRadixCache** exposing tenant KV data — potential security risk | No fix PR yet |
| 🟡 Medium | [#49133](https://github.com/sgl-project/sglang/issues/49133) | Breakable prefill CUDA graph produces different logprobs than eager mode | No fix PR yet |
| 🟡 Medium | [#48411](https://github.com/sgl-project/sglang/issues/48411) | Xeon SPR servers OOM-killed during KV allocation | No fix PR yet |

> *Note: Flaky CI infrastructure (#42752) remains a blocker for PR validation, affecting development velocity.*

#### **6. What This Means for Application Developers**  
- **Avoid `v0.5.23`** if running speculative decoding (DSpark) on Hopper — expect up to **45% higher decode latency** at high concurrency. Use `v0.5.22` or earlier until fixed.  
- **Do not deploy on SM120 (Blackwell) with FA4 backend** for models like GLM-5.3-Flash — it crashes during hybrid extend. Stick to Triton until resolved.  
- **Be cautious with `UnifiedRadixCache`** — a confirmed use-after-free vulnerability could expose sensitive user data. Monitor [#49361](https://github.com/sgl-project/sglang/issues/49361) for patches.  
- For **agent applications**, consider enabling breakable CUDA graphs and paged KV caches (via PR #49451) to scale long-context generation efficiently.  
- **Multi-GPU inference on AMD** requires careful attention to `has_mla=True` and deterministic mode — these may fail silently or produce NaNs. Validate on gfx1151 before production rollout.

---  
*Digest generated from GitHub data: [sgl-project/sglang](https://github.com/sgl-project/sglang)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-10**

---

### **1. Today's Highlights**  
The latest updates focus on refining OpenAI-compatible API parity, particularly for structured outputs and audio generation via the `/v1/audio/speech` endpoint. Critical fixes address JSON schema handling in `response_format`, ensuring consistent behavior across models. New kernel optimizations for Apple Metal (AMD eGPUs), WebGPU, and Vulkan improve performance on narrow matrix operations and reduce memory overhead.

---

### **2. Releases & Breaking Changes**  
- **New releases**: `b12242` through `b12233` were pushed in the last 24 hours.  
  - [b12242](https://github.com/ggml-org/llama.cpp/releases/tag/b12242): Fixes lazy grammar handling when `response_format` is set with tools (#31956).  
  - [b12240](https://github.com/ggml-org/llama.cpp/releases/tag/b12240): Adds support for converting `text.format.json_schema` to completions in the Responses API (#26145).  
  - [b12238](https://github.com/ggml-org/llama.cpp/releases/tag/b12238): Introduces OpenAI-compatible `/v1/audio/speech` endpoint with WAV output and PCM rounding alignment (#31763).  
  - [b12237](https://github.com/ggml-org/llama.cpp/releases/tag/b12237): Enables real-time audio stream processing for Gemma 4 models via MTMD (#31965).  
- **API Stability Note**: The server now correctly handles top-level `json_schema` in `response_format`, fixing prior nesting-only expectations (#28697).

---

### **3. New Model & Hardware Support**  
- **Model Support**:  
  - Gemma 4 input audio stream (MTMD) now supported (#31965).  
  - Qwen3-TTS speaker encoder and PocketTTS vocoder now fully supported via WebGPU (`PAD_REFLECT_1D`, `COL2IM_1D`) (#31991, #31990).  
- **Hardware & Backend Additions**:  
  - **Apple Metal**: SIMD width-aware kernels now handle AMD eGPUs (RX 580, Polaris) with 64-wide wavefronts (#31994).  
  - **Hexagon**: Added `SSM_SCAN` op support for Android and WoS devices (#31921).  
  - **SpacemiT CPU**: Enabled MXFP4 dense repack, IME2 acceleration, Q4_1 32x256 dispatch, and `GATED_DELTA_NET` routing via RVV (#31776, #31929, #31897).  
  - **WebGPU**: Expanded operator coverage with `PAD_REFLECT_1D`, `COL2IM_1D`, and improved padding tests (#31877, #31991).  
- **Quantization**: External PLE file loading now supported via `--ple` alias (#31185).

---

### **4. Performance & Optimization**  
- **Kernel Optimizations**:  
  - Vulkan: New "narrow" tile configs tuned for N ≤ 32 improve matrix multiplication efficiency (#31990).  
  - Intel GPU: Pre-fill FlashAttention kernel added for Intel platforms, improving prefill throughput (#29357).  
  - SYCL: `GGML_SYCL_Q4_0_U4_GEMM` environment variable enables faster Q4_0 weight loading by avoiding f16 conversion overhead (benchmarks show up to 1.2ms reduction on large matrices) (#31993).  
- **Memory Efficiency**:  
  - `ggml-zdnn`: Now accepts non-ZDNN tensors and invalidates compute tensors after each operation, reducing stale state risks (#31824).  
  - `mtmd_helper_resampler`: Improves audio resampling precision and reduces latency in streaming scenarios (#31964).  

---

### **5. Stability & Regressions**  
- **Critical Issues**:  
  - **KV Cache Truncation** (#11970): High-severity bug causing incorrect KV cache truncation during `/v1/chat/completions` — impacts model correctness; currently open with 50 comments.  
  - **SYCL MoE Logprobs Nondeterminism** (#31206): Identical requests yield different `logprobs` despite fixed seed — affects reproducibility in agents; reported on iGPU + MoE.  
- **Other Notable Bugs**:  
  - GGML crash due to unexpected graph reallocation (`ggml_backend_sched_alloc_splits`) on Linux (#28753).  
  - ROCm/GFX1151 wrong logits for long prompts (#28211).  
  - Adreno 830 Vulkan pipeline creation failure (`VK_ERROR_UNKNOWN`) due to shaderc/NDK SPIR-V divergence (#28635).  
- **Fixes in Progress**: No PRs directly resolving these high-impact issues yet.

---

### **6. What This Means for Application Developers**  
- **Structured Outputs**: You can now reliably use `response_format: json_schema` at the top level without wrapper nesting — ideal for agent tool calling.  
- **Audio Integration**: The new `/v1/audio/speech` endpoint enables full voice-to-text pipelines with real-time audio streams (e.g., for Voice Agents or Realtime Transcription).  
- **Cross-Platform Deployment**: With Metal (AMD eGPU), SpacemiT RISC-V, and WebGPU improvements, you can deploy low-latency inference on diverse edge devices.  
- **Caution**: Avoid using `--temp 0` with speculative decoding (`draft-dflash/dspark`) on Qwen3.5-MoE models — output may diverge from autoregressive baseline (#31472).  
- **Configuration Tip**: Use `GGML_SYCL_Q4_0_U4_GEMM=1` if working with Q4_0 quantized models on SYCL backends to avoid unnecessary F16 conversion overhead.

> 🔗 Full details: [GitHub - llama.cpp](https://github.com/ggml-org/llama.cpp)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-10**

---

### **1. Today's Highlights**  
The latest release, **v0.40.16**, introduces *Ripple Preview* — a 65B dense model optimized for agentic coding from Ricursive Intelligence, now available via `ollama run ripple-preview`. Critical fixes address VRAM accounting and GPU splitting issues that were causing models to be incorrectly evicted or misallocated across GPUs. These changes significantly improve reliability on multi-GPU and memory-constrained systems.

---

### **2. Releases & Breaking Changes**  
- **v0.40.16**:  
  - Introduces [Ripple Preview](https://ollama.com/library/ripple-preview), a 65B dense model tailored for autonomous coding agents.  
  - Fixes critical VRAM prediction and GPU splitting bugs affecting model loading and eviction behavior ([PR #19632](https://github.com/ollama/ollama/pull/19632), [Issue #19631](https://github.com/ollama/ollama/issues/19631)).  
  - Improves memory efficiency of `Qwen3.8 Flash Next` by ~27 GiB on Apple Silicon (M-series chips).  

> 🔗 [GitHub Release v0.40.16](https://github.com/ollama/ollama/releases/tag/v0.40.16)

---

### **3. New Model & Hardware Support**  
- **New Model**:  
  - [`ripple-preview`](https://ollama.com/library/ripple-preview) – 65B parameter dense model designed for complex reasoning and code generation in agent workflows.  
- **Hardware & Backend**:  
  - Improved support for **Apple Silicon (M-series)** with reduced memory footprint for Qwen3.8 Flash Next.  
  - Continued refinement of **ROCm** and **Vulkan (AMD RADV)** performance, though some regressions remain reported (e.g., #19201).  
  - Note: Vulkan-only systems still trigger unnecessary CUDA driver downloads (#19474); no fix yet.

> 🔗 [Ripple Preview Model Page](https://ollama.com/library/ripple-preview)

---

### **4. Performance & Optimization**  
- **Memory Efficiency**:  
  - `Qwen3.8 Flash Next` uses **~27 GiB less VRAM** on Apple Silicon, enabling larger context handling on M-series devices.  
- **Parallelism & Throughput**:  
  - PR #19197 enables concurrent generation for MLX models via `OLLAMA_NUM_PARALLEL`, improving first-token latency for parallel workloads.  
  - PR #17144 lifts the hard block on parallel requests for `qwen35` and `qwen35moe` after upstream llama.cpp stability fixes.  
- **Prompt Processing**:  
  - Ongoing optimization for prompt caching on AMD GPUs; slowdowns reported (~30×) post-first request with cache enabled (#19201).

> 🔗 [PR #19197 (MLX concurrency)](https://github.com/ollama/ollama/pull/19197) | [PR #17144 (Qwen35 parallelism)](https://github.com/ollama/ollama/pull/17144)

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR | Notes |
|---------|------|--------|--------|-------|
| ⚠️ High | VRAM accounting & GPU splitting errors cause model eviction/load failure | Fixed | [#19632](https://github.com/ollama/ollama/pull/19632) | Resolves multiple linked issues (#19463, #18679, etc.) |
| ⚠️ High | `OLLAMA_KEEP_ALIVE=300` too short for long user sessions | Open | — | Users report disconnection during reflection phase (Issue #19624) |
| ⚠️ High | `OLLAMA_LOAD_TIMEOUT=300` too short for large responses | Open | — | Similar pattern to #19624; affects interactive use (Issue #19616) |
| ⚠️ Medium | Windows updater crashes when >512 processes exist | Fixed | [#19625](https://github.com/ollama/ollama/pull/19625) | Due to incorrect `EnumProcesses` byte-to-ID conversion |
| ⚠️ Medium | Incorrect VRAM usage shown by `ollama ps` vs actual `rocm-smi` | Open | — | Discrepancy up to 23GB (Issue #17251) |
| ⚠️ Low | `--version` shows unexpected warnings | Closed | [#19613](https://github.com/ollama/ollama/pull/19613) | Now only outputs version number |

> 🔗 [Issue #19631 (VRAM fixes)](https://github.com/ollama/ollama/issues/19631)

---

### **6. What This Means for Application Developers**  
- **Agent Builders**: Ripple Preview is now viable for production-grade agentic workflows requiring deep code reasoning. Use `ollama run ripple-preview` to prototype.  
- **Multi-Model Deployments**: The VRAM and GPU splitting fixes stabilize multi-GPU and low-memory environments — crucial for edge inference and containerized agents.  
- **Configuration Caution**: Increase `OLLAMA_KEEP_ALIVE` and `OLLAMA_LOAD_TIMEOUT` beyond default 5 minutes if your app handles long conversations or batched tool calls (open issues #19624, #19616).  
- **Future-Proofing**: Enable `OLLAMA_NUM_PARALLEL` cautiously — while supported for MLX and Qwen3.5 variants, ensure sufficient GPU memory per concurrent request.  
- **Image Embedding Gap**: No `/api/embeddings` image support yet — consider using external vision encoders until PR #4296 is implemented.

> 🔗 [Feature Request: Image Embeddings API](https://github.com/ollama/ollama/issues/4296)  
> 🔗 [Feature Request: Dark Mode](https://github.com/ollama/ollama/issues/1645)

---  
*Prepared for infrastructure engineers by the AI Systems Analysis Team | 2026-11-10*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest – 2026-11-10

---

### **1. Today's Highlights**

The latest LTS release, **v1.108.4**, stabilizes critical cost tracking and rate limiting behavior across multi-provider deployments. Key fixes address long-standing issues in token accounting (e.g., `response_cost` being null), TPM/RPM enforcement (now correctly applied at half the configured value), and proper handling of `vector_store_ids` in Anthropic requests. A new feature enables per-model RPM/TPM limits for customers, enhancing granular budget control.

---

### **2. Releases & Breaking Changes**

- **v1.108.4 (LTS)** – Released today with stability and security improvements. Use `ghcr.io/berriai/litellm:lts` to stay on the latest stable patch.
  - [Release Notes](https://github.com/BerriAI/litellm/releases/tag/v1.108.4)
  - [Verify Signature](https://github.com/BerriAI/litellm/blob/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub)

> ⚠️ **Migration Note**: The v3 rate limiter fix (#34140) means that existing `model_rpm_limit` settings may now behave as if halved — review all team-level limits post-upgrade.

---

### **3. New Model & Hardware Support**

- ✅ **Glimpse-1.0** added to Together AI cost map (`together_ai/elorian-labs/glimpse-1.0`)  
  [PR #50830](https://github.com/BerriAI/litellm/pull/50830)

- ✅ **Vercel AI Gateway** now supported as a Decisions provider  
  [PR #50642](https://github.com/BerriAI/litellm/pull/50642)

- ✅ **AWS Bedrock Converse API** now supports `performanceConfig.latency="optimized"`  
  [PR #50809](https://github.com/BerriAI/litellm/pull/50809)

- ✅ **S3-based media support** extended: s3:// URLs now properly routed to Bedrock Converse for images, documents, and video  
  [PR #50577](https://github.com/BerriAI/litellm/pull/50577)

---

### **4. Performance & Optimization**

- 🔥 **Streaming Efficiency Boost**: Reuses upstream `choice` objects in HTTP handlers for providers like DeepSeek, Mistral, Groq, Anthropic, and Vertex AI — reducing CPU overhead per streamed token.  
  [PR #50786](https://github.com/BerriAI/litellm/pull/50786)

- 📈 **Token Counting Accuracy**: Custom-named deployments (e.g., Azure AI Foundry) now use base model’s tokenizer for correct cost tracking instead of default fallbacks.  
  [PR #50734](https://github.com/BerriAI/litellm/pull/50734)

- 📊 **Cost Tracking Fix**: Prompt-only calls now send accurate token counts to provider APIs (e.g., Gemini, Vertex AI), avoiding underreporting.  
  [PR #50783](https://github.com/BerriAI/litellm/pull/50783)

---

### **5. Stability & Regressions**

| Issue | Severity | Status | Fix PR |
|------|----------|--------|--------|
| `AnthropicException 400`: extra `vector_store_ids` input | High | Open (#23741) | Not yet resolved |
| TPM limit not rolling with in-memory cache | Critical | Open (#47340) | No fix yet |
| v3 rate limiter enforces RPM/TPM at half config | Critical | Closed (#34140) | ✅ Resolved |
| Cost tracking fails when model priced under different provider | High | Open (#47675) | Pending |
| JSON-RPC `id` stripped by `pass_through_endpoints` | Medium | Open (#49479) | No fix |
| Gemma/TTS `/v1/audio/speech` fails due to missing `voice` param | Medium | Open (#20078) | Pending |
| RAG file ingestion returns LiteLLM-managed ID + uses team credentials | Low | Closed (#50459) | ✅ Fixed |

> 💡 **Note**: Several high-severity issues remain open, particularly around cost accuracy and rate limiting consistency across providers.

---

### **6. What This Means for Application Developers**

- **Budgeting & Cost Visibility**: With `per-model tpm/rpm limits` now available per customer (PR #50797), you can enforce tighter financial controls in multi-tenant apps. Ensure your deployment configs reflect this change.

- **Avoid Silent Token Undercounting**: If using custom-named models (e.g., Azure AI Foundry), verify that `base_model` is set correctly to avoid incorrect cost reporting.

- **Be Cautious with Rate Limits**: After upgrading to v1.108.4+, re-evaluate `model_rpm_limit` and `tpm_limit` values — they are now enforced correctly, not halved.

- **Guardrails & Streaming**: Be aware that `Presidio` guardrail doesn’t restore PII in `tool_calls[].function.arguments` (Issue #31950). Implement client-side logic or use `output_parse_pii` carefully.

- **Use S3 URLs Safely**: For image/document ingestion via Bedrock, ensure `s3://` paths are used directly — previously rejected silently.

> 🛠️ **Actionable Tip**: Run `cosign verify` on the `lts` image to ensure integrity, especially in production environments.  
> 🔗 [Cosign Verification Guide](https://github.com/BerriAI/litellm/blob/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub)

---  
*Digest generated: 2026-11-10 | Source: GitHub – BerriAI/litellm*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-10**

---

### **1. Today's Highlights**  
Unsloth continues to advance its AI infrastructure stack with a focus on stability, UX polish, and multi-GPU/ROCm performance. Key developments include critical fixes for VRAM management in `sd.cpp`, improved handling of GGUF reloads, and refinements to diffusion pipeline state tracking—especially on ROCm platforms. Meanwhile, new PRs address UI consistency, model management QoL, and cross-platform reliability (Windows/macOS/Linux), signaling deeper maturity ahead of v0.2.0.

---

### **2. Releases & Breaking Changes**  
None. No new releases or breaking changes were published in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- **ROCm (gfx1151)**: Enhanced support for layer-splitting and attention kernel probing across multiple cards via PR #17347 and #17341.  
- **Pre-Blackwell GPUs**: PR #17343 introduces smarter checkpoint routing—official NVFP4 fine-tunes now only fallback to INT8 on older hardware if explicitly matched, reducing unnecessary quantization.  
- **FLUX.3**: Expanded support for strided cuDNN attention verification per card (#17341) and joint block quantization sharing (#17337).  
- **Image Generation (sd.cpp)**: Improved resilience under low VRAM conditions with retry logic when `sd-server` refuses renders (#17339).

> 🔗 [PR #17343](https://github.com/unslothai/unsloth/pull/17343) | [PR #17341](https://github.com/unslothai/unsloth/pull/17341)

---

### **4. Performance & Optimization**  
- **VRAM Efficiency**: PR #17345 ensures GGUF reloads occur *only after* VRAM is confirmed free post-stop, preventing partial CPU loading and speculative decode waste.  
- **Kernel-Level Improvements**:  
  - PR #17347 applies SDPA guards only to cards that actually participate in a layer split, reducing redundant GPU overhead.  
  - PR #17340 restores `torch.cuda.tunable` state on ROCm after pipeline unload—preventing spurious GEMM misconfigurations.  
- **Memory & Latency**: PR #17339 implements retry logic for failed image renders under VRAM constraints, improving success rate on 8GB RTX 3070 systems.

> 📈 *Expected impact*: ~15–30% reduction in failed image generation attempts on constrained setups; improved stability during long-running inference sessions.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR | Notes |
|------|----------|--------|--------|-------|
| Image gen falls back to CPU on Windows 11 (RTX 3060) | High | Open | None | Affects desktop app users; reproducible on Win but not M1 Mac. |
| Linux forced to use CPU instead of GPU | High | Open | None | Seen on Manjaro KDE; likely tied to CUDA driver detection or runtime binding. |
| `Regenerate title` copies part of last message | Medium | Open | None | UX bug in chat flow affecting content integrity. |
| Custom Folder browser returns 403 for managed accounts | Medium | Open | None | Security boundary issue in Studio; affects enterprise deployments. |
| Video upload blocked on custom OpenAI-compatible connections | Medium | Open | None | Blocks data ingestion in Studio workflows. |

> ⚠️ **Note**: While several high-severity issues are open, no fix PRs were merged today. The team is actively addressing platform-specific regressions.

---

### **6. What This Means for Application Developers**  
- **For agent developers**: Expect more reliable behavior when chaining image generation (`sd.cpp`) and LLM inference—especially on multi-GPU or ROCm setups. Use `UNSLOTH_REUSE_CACHED_PICKLE_WEIGHTS=0` + `UNSLOTH_PREQUANT_PREFER_SAFETENSORS=1` to enforce safe weight loading (#17342).  
- **For tooling integrators**: Be cautious with managed accounts and custom API endpoints—permissions and folder access are still inconsistent across environments (#17292, #17295).  
- **For UI builders**: The upcoming rework of visual pickers (#17164, #17344) will simplify theme customization and improve keyboard navigation—ideal for accessible design.  
- **For deployment engineers**: Monitor VRAM usage closely—reloads may still fail silently without full memory release. Consider implementing retry logic at the client level until #17345 is fully adopted.

> 💡 Pro tip: Use `--auto-fit off` selectively in sd.cpp builds where auto-splitting fails due to memory mismatch (#17346).

---  
*Digest compiled from GitHub activity: unslothai/unsloth @ 2026-11-10*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*