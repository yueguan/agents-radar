# AI Infrastructure Digest 2026-10-31

> Generated: 2026-10-31 01:46 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-10-31**

---

### **1. Ecosystem Overview**  
The AI infrastructure landscape in late 2026 is defined by a sharp bifurcation between **high-performance, low-latency serving engines** and **edge-optimized local runtimes**, with growing convergence at the gateway layer. Projects like vLLM and SGLang are pushing the boundaries of distributed inference and speculative decoding, while llama.cpp and Unsloth focus on lightweight, cross-platform execution—especially for mobile, edge, and Apple Silicon devices. Meanwhile, LiteLLM is evolving into a high-throughput routing proxy with Rust-based core ambitions, and Ollama continues to serve as a developer-friendly gateway with expanding MLX and model compatibility. The ecosystem is maturing rapidly, but stability remains a recurring concern across all layers.

---

### **2. Activity Comparison**

| Project        | Issues Open (Recent) | PRs Merged (Last 7 Days) | Release Status         |
|----------------|----------------------|----------------------------|------------------------|
| **vLLM**       | 48                   | 15                         | No new release         |
| **SGLang**     | 39                   | 12                         | No new release         |
| **llama.cpp**  | 42                   | 14                         | No formal release      |
| **Ollama**     | 37                   | 10                         | Stable: v0.40.13       |
| **LiteLLM**    | 28                   | 11                         | v1.109.0-dev.2 (beta)  |
| **Unsloth**    | 35                   | 9                          | v0.1.913-beta (released) |

> ✅ *Note: vLLM leads in issue volume due to active multi-model and disaggregated inference work; LiteLLM shows strong dev velocity with breaking changes in its architecture shift.*

---

### **3. Model Support Race**

| New Model / Architecture        | Supported By                     | Notes |
|----------------------------------|----------------------------------|-------|
| **Qwen3.8-Flash-Next (gfx950)** | vLLM, SGLang, Unsloth            | vLLM leads with ROCm-specific optimization (MXFP4 MoE + QuerySparse Attention); SGLang enables DCP + DSPARK spec-decode. |
| **DeepSeek-V4.1-Flash-1017 GGUF** | Unsloth, llama.cpp               | Unsloth adds Dynamic v3.1 support; llama.cpp fixes memory allocation issues. |
| **T-Head SAIL (Zhenwu/M890P)**   | SGLang (roadmap), Unsloth (partial) | SGLang has dedicated tracking; no upstream integration yet. |
| **Kimi-K3 (Ascend NPU)**         | SGLang, vLLM                     | SGLang enables full decode parallelism via DCP + shared compact comm; vLLM enhances MooncakeConnector. |
| **Gemma4 (E2B/E4B variants)**    | llama.cpp                        | Graph optimizations eliminate redundant computation. |
| **Clover-35B-A3B-Preview**       | llama.cpp                        | First-class GGUF conversion and hybrid attention support. |

> 🏆 **Leader**: **vLLM** maintains an edge in **multi-model and hardware diversity**, particularly for AMD/Ascend P/D disaggregation. **SGLang** is strongest in **emerging hardware** (T-Head, MI45x) and **speculative decoding** enablement.

---

### **4. Performance Frontier**

| Optimization Focus             | Leading Projects                                | Key Advances |
|----------------------------------|--------------------------------------------------|--------------|
| **KV Cache Layout & Management** | vLLM, llama.cpp, SGLang                         | vLLM unifies layouts across backends; llama.cpp introduces GPU-stored rotation constants; SGLang fuses QK-norm + RoPE. |
| **Quantization & Low-Bit Inference** | llama.cpp, vLLM, Unsloth                       | llama.cpp adds `q2_0`/`iq3`; vLLM supports FP8 DS MLA; Unsloth enables fused LoRA GEMM. |
| **Distributed Serving & Disaggregation** | vLLM (MooncakeConnector), SGLang (NIXL/MoRIIO) | vLLM advances non-CUDA P/D with gfx950/Mi355X; SGLang enables HiCache fusion. |
| **Cold Start & Warmup Speed**    | SGLang, Ollama                                   | SGLang’s `fast_warmup` cuts DeepSeek-V4.1 cold start by 3–5×; Ollama improves MLX pre-flight checks. |
| **Kernel-Level & Triton Optimization** | vLLM, SGLang, llama.cpp                         | vLLM optimizes top-k selection for SM80+; SGLang fuses kernels on ROCm; llama.cpp improves HVX permute ops. |

> 🔥 **Hotspot**: **vLLM** dominates in **distributed inference performance** and **MoE kernel efficiency**, while **SGLang** excels in **cold-start optimization** and **ROCm/AMD specialization**.

---

### **5. Layer Positioning**

| Project        | Primary Layer                | Role Summary |
|----------------|-------------------------------|--------------|
| **vLLM**       | **Inference Engine**          | High-performance, CUDA/ROCm-focused serving engine with strong MoE, spec-decoding, and P/D disaggregation support. |
| **SGLang**     | **Inference Engine + Gateway** | Multi-backend engine with strong agentic support, JIT compilation, and emerging T-Head/Ascend integration. |
| **llama.cpp**  | **Local Runtime / Edge SDK**  | Cross-platform, low-level runtime optimized for GPU (OpenCL, Vulkan), CPU (Hexagon), and WASM. Ideal for edge/mobile. |
| **Ollama**     | **Gateway / Developer CLI**   | Unified CLI and REST API gateway with model orchestration, MLX support, and agent-friendly endpoints. |
| **LiteLLM**    | **Gateway / Routing Proxy**   | High-throughput, multi-provider routing layer with Rust-core migration for latency-sensitive use cases. |
| **Unsloth**    | **Local Runtime + Training Tool** | Full-stack tool for local inference, fine-tuning (LoRA), and agent integration—blurs line between runtime and training. |

> 🔄 **Convergence Trend**: LiteLLM and Unsloth are increasingly overlapping in function—gateway capabilities meeting local training workflows.

---

### **6. Trend Signals**

#### **Industry Trends Extracted from Today’s Activity:**
1. **Disaggregation is Going Mainstream**: vLLM and SGLang are actively enabling **P/D (Prefill/Decoding) separation across non-CUDA platforms** (AMD, Ascend), signaling a move toward heterogeneous, scalable inference clusters.
2. **Rust is Becoming the Routing Core**: LiteLLM’s introduction of `RustCore` reflects a broader trend toward **performance-critical components being rewritten in Rust** for lower latency and higher throughput.
3. **Low-Bit Quantization is Maturing**: `q2_0`, `iq3`, MXFP4, and FP8 DS MLA are now production-grade features—not just experimental—indicating demand for **ultra-efficient inference** on constrained devices.
4. **Edge and Local AI is Prioritized**: llama.cpp, Unsloth, and Ollama continue to invest heavily in **Apple Silicon, Hexagon, and MLX support**, showing that local, privacy-preserving inference remains a strategic priority.
5. **Agent Workloads Are Driving Stability Needs**: Recurring crashes in structured outputs (`response_format`), tool-call parsing, and streaming finish reasons highlight that **agent reliability is now a top-tier quality metric**.

#### **What Developers Should Watch:**
- ⚠️ **Security Hygiene**: vLLM’s `/invocations` bypass bug underscores the need for strict API key enforcement—even in "default" setups.
- 🛠️ **Tool-Calling Reliability**: Expect silent failures in structured output handling until #55152 (vLLM) and #31328 (llama.cpp) are resolved.
- 🚀 **Adopt Fast Warmups Early**: Use `SGLANG_JIT_DEEPGEMM_FAST_WARMUP=1` and `draft_num_predict=0` for deterministic benchmarking.
- 🔍 **Monitor Hardware-Specific Regressions**: AMD MI308X (`glm5_next`) and Blackwell B200 (`Xid 13`) show instability under load—validate before production rollout.
- 💡 **Prepare for Hybrid Architectures**: With projects like vLLM and SGLang supporting mixed backend deployments (CUDA + ROCm + T-Head), expect more complex inference topologies in 2027.

---

> ✅ **Final Takeaway**: The AI infrastructure stack is becoming **increasingly modular, heterogeneous, and agent-driven**. Choose your stack based on **latency requirements**, **hardware constraints**, and **development velocity**—not just raw throughput.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **1. Today's Highlights**  
The vLLM project is advancing multi-model and disaggregated inference with key work on **ViT CUDA graph support**, **ROCm performance optimization for Qwen3.8-Flash-Next on gfx950/Mi355X**, and **MooncakeConnector improvements** for P/D disaggregation across non-CUDA platforms. Critical stability fixes are underway for speculative decoding, structured output handling, and security (e.g., `/invocations` bypass), while new RFCs focus on direct weight reload for RL training and consistent spec-decode token semantics.

---

### **2. Releases & Breaking Changes**  
*None* — No new releases or breaking API/config changes were published in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- **Qwen3.8-Flash-Next (gfx950 / MI355X)**: Dedicated ROCm performance optimization tracker [#59575](https://github.com/vllm-project/vllm/issues/59575) targeting `amd/Qwen3.8-Flash-Next-Quark-MXFP4` with MXFP4 MoE and QuerySparse Attention.
- **GLM-5.x (DSA)**: Fix for pipeline parallelism layer reuse bug under long-context decode ([#63170](https://github.com/vllm-project/vllm/issues/63170)).
- **Kimi K3/K2.5**: Ongoing RFCs for ViT full CUDA graph support ([#38175](https://github.com/vllm-project/vllm/issues/38175)) and MooncakeConnector enhancements ([#60523](https://github.com/vllm-project/vllm/issues/60523)).
- **AMD ROCm**: Continued CI enablement for distributed inference (Disaggregated P/D + Wide-EP) via [#48117](https://github.com/vllm-project/vllm/issues/48117).

---

### **4. Performance & Optimization**  
- **MoE Kernel Perf**: PR [#62912](https://github.com/vllm-project/vllm/pull/62912) optimizes top-k selection for SM80+ by keeping warp-level selections in registers and reducing shuffle overhead.
- **FP8 Decode Support**: PR [#64783](https://github.com/vllm-project/vllm/pull/64783) adds opt-in `fp8_ds_mla` support on Ampere GPUs using DeepSeek’s 656-byte cache layout.
- **KV Cache Layout Standardization**: PRs like [#42374](https://github.com/vllm-project/vllm/pull/42374) and [#64812](https://github.com/vllm-project/vllm/pull/64812) aim to unify KV cache layouts across backends, enabling better TP slicing and transfer efficiency.
- **Triton Kernel Optimization**: Fixes for illegal memory access on ROCm (`gfx950`) during long-context decode ([#48043](https://github.com/vllm-project/vllm/issues/48043)) and FP8 KV cache support for sparse MLA ([#57134](https://github.com/vllm-project/vllm/pull/57134)).

---

### **5. Stability & Regressions**  
- **Critical Security Bug**: The `/invocations` endpoint is enabled by default and bypasses API key auth — a potential exposure risk ([#64391](https://github.com/vllm-project/vllm/issues/64391)). *No fix PR yet.*
- **Structured Output Crash**: Guidance backend fails on tool-calling structural tags due to missing `'triggers'` key ([#55152](https://github.com/vllm-project/vllm/issues/55152)); regression in `v0.28.0`.
- **Speculative Decoding Issues**: 
  - Priority scheduling can un-schedule already-scheduled requests ([#59907](https://github.com/vllm-project/vllm/issues/59907)).
  - Drafted/accepted token semantics unclear with adaptive verification + structured outputs ([#64099](https://github.com/vllm-project/vllm/issues/64099)).
- **Model-Specific Crashes**:
  - `nvidia/Qwen3.6-35B-A3B-NVFP4` crashes with MTP/spec decoding due to CUDA device-side assert ([#44734](https://github.com/vllm-project/vllm/issues/44734)).
  - GLM-5.x PP deployment reads stale top-k indices at stage start ([#63170](https://github.com/vllm-project/vllm/issues/63170)).

---

### **6. What This Means for Application Developers**  
- **Multi-model & Low-Latency Serving**: With ongoing work on **near-zero cold starts** ([#34303](https://github.com/vllm-project/vllm/issues/34303)) and **ViT CUDA graphs** ([#38175](https://github.com/vllm-project/vllm/issues/38175)), expect faster model switching and improved multimodal throughput.
- **Production-Ready Disaggregation**: Use of **MooncakeConnector** and **NIXL/MoRIIO** is maturing for cross-platform P/D deployments — ideal for large-scale, low-latency inference on AMD/Ascend.
- **Tool-Calling Reliability**: Be cautious with `tool_choice="required"` and structured outputs; recent bugs may cause silent failures. Monitor [#55152](https://github.com/vllm-project/vllm/issues/55152) and [#58147](https://github.com/vllm-project/vllm/issues/58147) for fixes.
- **Security Hygiene**: Disable `/invocations` endpoint unless explicitly needed — it’s currently insecure by default ([#64391](https://github.com/vllm-project/vllm/issues/64391)).

> ✅ **Actionable Takeaway**: Upgrade to latest `main` if using speculative decoding, structured outputs, or hybrid quantization. Audit your tool-calling and priority scheduling logic. Avoid `v0.28.0` until #55152 and #58147 are resolved.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-10-31**

---

### **1. Today's Highlights**  
The SGLang ecosystem continues to strengthen its multi-backend support, with critical performance and stability work focused on AMD ROCm (gfx1250, MI45x), NVIDIA Blackwell (B200), and emerging T-Head PPU integration. A major regression in `SGLANG_JIT_DEEPGEMM_FAST_WARMUP` was resolved, delivering **3–5× faster cold start** for DeepSeek-V4.1 without runtime degradation — a key win for production inference latency.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API/config changes were published.

---

### **3. New Model & Hardware Support**  
- ✅ **T-Head SAIL (Zhenwu 810/810E/M890P)**: High-priority roadmap issue #37519 tracks upstreaming first-class support for T-Head’s next-gen AI accelerators.  
- ✅ **AMD gfx1250 / MI45x**: Issue #47562 tracks PR readiness for gfx1250 pinning; separate AITER pin required due to divergent layout from CDNA-based ROCm images.  
- ✅ **Kimi-K3 on Ascend NPU**: PR #40825 enables DCP + shared compact communication, unlocking decode context parallelism and DSPARK speculative decoding.  
- ⚠️ **AMD MI308X**: Still failing to load `glm5_next` models due to Transformers architecture recognition issues (#37183).  

> 🔗 [T-Head SAIL Roadmap](https://github.com/sgl-project/sglang/issues/37519) | [gfx1250 CI Tracking](https://github.com/sgl-project/sglang/issues/47562)

---

### **4. Performance & Optimization**  
- 🚀 **Cold Start Speedup**: `SGLANG_JIT_DEEPGEMM_FAST_WARMUP` now delivers **3–5× faster cold start** for DeepSeek-V4.1 (chunked-prefill 16384), with no detectable runtime cost — validated in #47397.  
- 📈 **ROCm Optimization**:  
  - PR #47052 fuses QK-norm + RoPE into a single JIT kernel on ROCm, reducing prefill overhead.  
  - PR #39071 enables fused in-place QK norm on HIP, eliminating redundant copies and RMSNorm launches.  
- 🔥 **FlashInfer Upgrades**: PR #46926 bumps FlashInfer to `0.7.2.post1`, enabling new optimizations and fixing edge cases.  
- 🎯 **AllReduce Fusion**: PR #47306 enables auto-enablement of FlashInfer AllReduce Fusion for Kimi-K3 on supported architectures.  

> 🔗 [Fast Warmup Fix](https://github.com/sgl-project/sglang/issues/47397) | [QK-Norm Fusion (ROCm)](https://github.com/sgl-project/sglang/pull/47052)

---

### **5. Stability & Regressions**  
- ⚠️ **Critical Crash on Blackwell (B200)**: Xid 13 "CTA Not Present" errors in HiCache kvcacheio transfer kernels under agentic load — **7 crashes across 4 machines**, tracked in #46203.  
- ⚠️ **CUDA Graph Capture Failure**: GLM-5.3-Flash on SM120 (RTX PRO 6000) crashes during CUDA graph capture with `fa4` backend; only `triton` works (#42012).  
- ⚠️ **CI Flakiness**: Persistent flaky tests and infrastructure failures in `PR Test Base/Extra` runs, requiring manual babysitting via `/sglang-pr-babysit` (#42752).  
- ⚠️ **Memory Leak in Diffusion Pipeline**: Nomas KV budget ignores freed PyTorch memory, leading to premature OOMs (#47515).  
- ⚠️ **Rust Server Logic Errors**:  
  - Control requests relayed without rank validation (#47511).  
  - `gather_server_endpoints` advertises loopback/unroutable addresses (#47509).  
  - Kimi-K3 fused KDA input-projection buffers replaced on weight reload (#47508).  

> 🔗 [Blackwell Crash](https://github.com/sgl-project/sglang/issues/46203) | [CUDA Graph Crash](https://github.com/sgl-project/sglang/issues/42012) | [CI Flakiness](https://github.com/sgl-project/sglang/issues/42752)

---

### **6. What This Means for Application Developers**  
- **Use `SGLANG_JIT_DEEPGEMM_FAST_WARMUP=1`** for production deployments of DeepSeek-V4.1 — expect **sub-3-minute warmups** with no performance trade-off.  
- **Avoid `fa4` on SM120 (RTX PRO 6000)** until the hybrid extend reshape bug is fixed; stick to `triton` backend.  
- **Be cautious with AMD MI308X and gfx1250**: Expect model loading issues (`glm5_next`) and incomplete AITER support; validate against pinned images.  
- **Watch for crash risks under high-throughput agentic workloads** on B200 — monitor for Xid 13 errors and consider fallback to `HiCache` disable if needed.  
- **Ensure robust error handling for tool-call parsing**, especially with `oneOf` schemas (MiniMax-M3 parser still has unresolved schema resolution bugs).  

> 🔗 [Performance Guidance](https://github.com/sgl-project/sglang/issues/47397) | [Tool-Call Parsing Bug](https://github.com/sgl-project/sglang/issues/32286)

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp Digest – 2026-10-31**

---

### **1. Today's Highlights**  
The latest development in `llama.cpp` centers on backend stability and low-level performance improvements, particularly for OpenCL and Hexagon backends. Critical fixes have been merged to stabilize GPU memory management (e.g., KV cache rotation constants on GPU), while new support for `q2_0` and `iq3` quantizations expands inference capabilities across diverse hardware. The community continues to report high-severity issues with Vulkan, SYCL, and Metal backends—especially around out-of-memory conditions and precision handling.

---

### **2. Releases & Breaking Changes**  
No formal releases were published today. However, several critical internal changes were merged:  

- **`fs_path_to_utf8()` removal** (#31424, #31423): Eliminated redundant UTF-8 path conversion functions in `common` and `rpc`, streamlining cross-platform I/O logic.  
- **Windows argv reconstruction dropped** (#31417): Now consistent with UTF-8 manifest behavior; removes legacy Windows-specific code.  
- **`common_get_path_from_env()` deleted** (#31444): Unused function removed from shared utilities.  
- **`batched tensor copy API` added** (#30881): Enables efficient bulk data movement between tensors—key for future context IO optimizations.

> 🔗 [PR #31444](https://github.com/ggml-org/llama.cpp/pull/31444) | [PR #31417](https://github.com/ggml-org/llama.cpp/pull/31417)

---

### **3. New Model & Hardware Support**  
- ✅ **OpenCL**: Initial support for `q2_0` quantization (AOS/SOA/tiled MM, noshuffle variants) via PR #31340 — enables faster, lower-precision inference on AMD/Adreno GPUs.  
- ✅ **Hexagon (Snapdragon)**: Enhanced DMA transfer support for strides > 24 bits (#31442); improved permute performance via batched HVX vgather transposes (#31446).  
- ✅ **New Models**: Added support for **Clover-35B-A3B-Preview** (DiffuSpace) including GGUF conversion, hybrid attention, and model loading (#31306).  
- ✅ **Quant Formats**: Experimental `iq3` support introduced (IQ3_S, IQ3_XXS) via PR #31338 — targeting ultra-low-bit efficiency on mobile SoCs.  
- ✅ **Gemma4**: Graph optimizations applied to E2B/E4B variants, eliminating unnecessary computation in non-KV layers (#31356).

> 🔗 [PR #31340](https://github.com/ggml-org/llama.cpp/pull/31340) | [PR #31306](https://github.com/ggml-org/llama.cpp/pull/31306) | [PR #31356](https://github.com/ggml-org/llama.cpp/pull/31356)

---

### **4. Performance & Optimization**  
- **KV Cache Rotation**: Rotation constants now kept on GPU (#31073), reducing CPU-GPU sync overhead and improving multi-device scalability.  
- **MoE Cache**: Hybrid LRFU/MQ-like cache (`ClassLRU`) improves decode speed by **+6% (QFN)** and **+7% (GLM)**, with **8% fewer uploads** vs SLRU (#31233).  
- **Hexagon**: Tiled HVX permute operations accelerate CONT/CPY kernels significantly (#31446); DMA transfers optimized for large stride scenarios (#31442).  
- **Flash Attention**: CUDA/HIP now supports native quantized KV in `flash_attn_tile` (#31234), avoiding full dequantization per step—critical for Q4/Q8 KV cache workflows.  
- **WASM SIMD**: Relaxed SIMD instructions enhance dot product performance in browser-based inference (#19590).  

> 🔗 [PR #31073](https://github.com/ggml-org/llama.cpp/pull/31073) | [PR #31233](https://github.com/ggml-org/llama.cpp/pull/31233) | [PR #31234](https://github.com/ggml-org/llama.cpp/pull/31234)

---

### **5. Stability & Regressions**  
High-priority stability concerns persist across multiple backends:

| Issue | Severity | Affected Backend | Description |
|------|----------|------------------|------------|
| [#10982](https://github.com/ggml-org/llama.cpp/issues/10982) | ⚠️ High | Vulkan (Apple Silicon) | macOS Metal outperforms Linux Vulkan despite same hardware; research ongoing into driver-level performance divergence. |
| [#24066](https://github.com/ggml-org/llama.cpp/issues/24066) | ⚠️ High | Vulkan (Linux) | Recent builds show significant eval performance drop on RX 6600 with Qwen3.5-9B-Q5_K_M.gguf. |
| [#31328](https://github.com/ggml-org/llama.cpp/issues/31328) | ❌ Critical | RPC / LFM3 | Any `response_format` (JSON schema/object) fails with `Unexpected empty grammar stack after accepting piece`. |
| [#31385](https://github.com/ggml-org/llama.cpp/issues/31385) | ⚠️ Medium | CUDA | DeepSeek-V4.1-Flash-1017 GGUF exhibits instability; likely due to new MoE or attention layout. |
| [#29465](https://github.com/ggml-org/llama.cpp/issues/29465) | ⚠️ High | Metal (macOS) | Lazy-read table mapping causes OOM in Qwen3.8-Flash-Next due to CPU-resident tensor footprint. |

> 🔗 [Issue #10982](https://github.com/ggml-org/llama.cpp/issues/10982) | [Issue #24066](https://github.com/ggml-org/llama.cpp/issues/24066) | [Issue #31328](https://github.com/ggml-org/llama.cpp/issues/31328)

---

### **6. What This Means for Application Developers**  
- **Use caution with `q2_0` and `iq3`**: While promising for edge devices, these formats are still experimental—validate correctness and latency on target hardware.  
- **Avoid `-sm tensor` on large models**: A recent crash in DeepSeek-V4.1 highlights insufficient memory pool allocation under `-sm tensor`—consider increasing `--memory` or using `--ctx-size` limits.  
- **Expect regressions in server APIs**: The `response_format` failure in LFM3 is a major blocker for JSON-structured agent outputs—monitor issue #31328 for patches.  
- **Leverage new MoE cache**: If building scalable agents with MoE models (e.g., Qwen3-MoE), switch to `ClassLRU` cache via `--moe-cache auto` for up to **7% decode improvement**.  
- **Test on heterogeneous backends**: Vulkan, SYCL, and Metal continue to show instability—use `--backend metal` or `--backend cuda` for production where possible.

> 📌 **Pro Tip**: For mobile/edge deployments, prioritize Hexagon + `iq3` or `q2_0` paths. For cloud servers, use `--moe-cache auto` and avoid `--sm tensor` unless memory budget is tight.

---  
*Data sourced from GitHub: [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*  
*Generated: 2026-10-31*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-10-31**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to mature with critical fixes for MLX-based inference stability and improved handling of multi-block `thinking` content in Anthropic-compatible endpoints. A high-priority fix has landed for MLX models skipping memory pre-flight checks, addressing VRAM overbooking on Apple Silicon systems (e.g., M5 Max). Concurrently, multiple issues around model parsing, response formatting, and Windows TCP state ambiguity highlight ongoing refinement of edge-case robustness.

---

### **2. Releases & Breaking Changes**  
*None* — No new releases or breaking changes were published in the last 24 hours. The latest stable version remains **v0.40.13**, with no migration notes or deprecations announced.

---

### **3. New Model & Hardware Support**  
- ✅ **MLX-native support expanded**: PR #19399 adds `AutoTrust` decision models (text & vision) via MLX backend, enabling local reasoning agents.  
- ✅ **Intel Macs on macOS 12 (Monterey)**: PR #19413 introduces best-effort build/run support for older Intel Macs, expanding accessibility beyond macOS 14+.  
- 🚧 **MLX model loading behavior**: PR #19419 addresses a scheduler misallocation issue where MLX models were not receiving proper memory pre-flight estimates, now fixed.

> 🔗 [PR #19413](https://github.com/ollama/ollama/pull/19413) | 🔗 [PR #19399](https://github.com/ollama/ollama/pull/19399)

---

### **4. Performance & Optimization**  
- **Memory scheduling precision improved**: PR #19327 refines KV cache estimation per layer by using minimal `head_count_kv`, reducing over-allocation errors in heterogeneous models.  
- **Speculative decoding control added**: PR #19391 enables `draft_num_predict=0` to disable speculative decoding per request, crucial for benchmarking and deterministic inference.  
- **Long prompt handling optimized**: PR #19408 extends input length limits for MLX decision models while bounding prompt expansion, preventing crashes from oversized inputs.  

> 🔗 [PR #19327](https://github.com/ollama/ollama/pull/19327) | 🔗 [PR #19391](https://github.com/ollama/ollama/pull/19391) | 🔗 [PR #19408](https://github.com/ollama/ollama/pull/19408)

---

### **5. Stability & Regressions**  
**Critical Issues Reported (High Severity):**  
- ⚠️ **Cloud API hangs / 429 timeouts**: Issue #19111 reports complete HTTP response loss on `/v1/chat/completions` with Pro plan users; requests hang 5 minutes before returning 429. *No fix PR yet.*  
- ⚠️ **Gemma4 fails on Windows 11 26H2**: Issue #19420 shows consistent 500 Internal Server Errors on fresh install. Likely related to native Windows service or dependency conflict. *No fix PR.*  
- ⚠️ **Qwen3.5 4B returns only `thinking` blocks**: Issue #18916 confirms models return empty responses despite valid `eval_count` and `done_reason`. *Blocking long-running agent workflows.*

**Medium Severity:**  
- ⚠️ **Anthropic `/v1/messages` drops early `thinking` blocks**: Issue #19415 — only the last `thinking` block retained. Fixed by PR #19416.  
- ⚠️ **MLX models skip memory pre-flight**: Issue #19417 — model booked at weights-only size, leading to runtime OOMs. Fixed by PR #19419.  
- ⚠️ **Modelfile CRLF handling retains `\r` in triple-quoted values**: Issue #19404 — corrupts system prompts on Windows. *Fix pending.*  

> 🔗 [Issue #19111](https://github.com/ollama/ollama/issues/19111) | 🔗 [Issue #19420](https://github.com/ollama/ollama/issues/19420) | 🔗 [Issue #18916](https://github.com/ollama/ollama/issues/18916)  
> 🔗 [PR #19416](https://github.com/ollama/ollama/pull/19416) | 🔗 [PR #19419](https://github.com/ollama/ollama/pull/19419) | 🔗 [Issue #19404](https://github.com/ollama/ollama/issues/19404)

---

### **6. What This Means for Application Developers**  
- **Avoid cloud usage for long-running agents** until #19111 is resolved — expect silent failures and 5-minute delays.  
- **Validate Modelfile syntax carefully** — especially if using Windows line endings (`CRLF`) or nested triple-quoted strings (#19404).  
- **Use `draft_num_predict=0` explicitly** when benchmarking or requiring deterministic generation speed (PR #19391).  
- **Ensure MLX model context sizes are realistic** — prior to v0.40.13, MLX models could be scheduled incorrectly due to missing pre-flight checks (now fixed via #19419).  
- **Do not rely on `content: []` + `tool_calls` semantics** — ensure your client handles tool calls correctly during message merging (PR #19152 clarifies this).  

> 💡 **Pro Tip**: For production use on Apple Silicon, prefer `--fit` padding and monitor available VRAM closely. Use `ollama ls` in non-interactive contexts to avoid 5-second latency (issue #15597).

---  
*Digest generated: 2026-10-31 | Source: github.com/ollama/ollama*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-10-31**

---

### **1. Today's Highlights**  
The LiteLLM proxy continues its strategic shift toward a Rust-based routing core, with PR #49120 introducing the `RustCore` for routing decisions—marking a major architectural milestone. Critical fixes were merged to ensure streaming responses properly propagate finish reasons (#47665) and maintain consistent reasoning item IDs across stream events (#49338). Additionally, a new `LITELLM_FIPS_MODE` startup gate was introduced to enforce FIPS compliance in regulated environments.

---

### **2. Releases & Breaking Changes**  
- **v1.109.0-dev.2** released with enhanced security via cosign-signed Docker images (key: [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065)).  
- **Breaking**: The `RustCore` is now opt-in via `LITELLM_RUST=true`. Future versions may default to it; migration guidance will be published post-stable rollout.  
- **New Config Flag**: `LITELLM_FIPS_MODE=true` now enforces FIPS-compliant crypto at startup (PR [#48600](https://github.com/BerriAI/litellm/pull/48600)).

---

### **3. New Model & Hardware Support**  
- **Hostinger Router** added as an OpenAI-compatible provider (PR [#44863](https://github.com/BerriAI/litellm/pull/44863)), supporting chat, messages, embeddings with pricing from Hostinger’s public catalog.  
- **Nano-GPT** now available in the UI provider dropdown (requested in [#34499](https://github.com/BerriAI/litellm/issues/34499), now resolved).  
- **Meta Model API** support remains backend-only; UI integration pending (Issue [#36164](https://github.com/BerriAI/litellm/issues/36164)).

---

### **4. Performance & Optimization**  
- **Router Core Migration**: The `RustCore` (PR [#49120](https://github.com/BerriAI/litellm/pull/49120)) enables faster, lower-latency routing with reduced memory overhead—expected to improve throughput by ~30% under high concurrency.  
- **Streaming Stability**: Fix for inconsistent ID assignment in reasoning streams ensures reliable client-side deduplication (PR [#49338](https://github.com/BerriAI/litellm/pull/49338)).  
- **Budget Window Reset**: Test suite now correctly handles edge cases on month-end dates (PR [#49372](https://github.com/BerriAI/litellm/pull/49372)).

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR |
|--------|------|--------|--------|
| 🔴 High | Streaming response ends without `finish_reason` → silent failure | Fixed | [PR #47665](https://github.com/BerriAI/litellm/pull/47665) |
| 🔴 High | `/key/update` during in-flight request drops cost from budget window | Reported | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) |
| 🟡 Medium | `websearch_interception` fails for Claude Code + Bedrock pass-through | Closed | [PR #26252](https://github.com/BerriAI/litellm/pull/26252) |
| 🟡 Medium | `reasoning_content` dropped from Mistral responses | Closed | [PR #45546](https://github.com/BerriAI/litellm/pull/45546) |
| 🟡 Medium | Perplexity search params like `date`, `language` lost in transform | Closed | [PR #45536](https://github.com/BerriAI/litellm/pull/45536) |

---

### **6. What This Means for Application Developers**  
- **Adopt `LITELLM_RUST=true` early** for improved routing performance and stability—especially in high-throughput agent systems.  
- **Update streaming clients** to handle explicit `error` terminations instead of synthetic `stop` reasons (post-PR #47665).  
- **Ensure `metadata.user_id` is preserved** through pass-through routes (fix in PR #49333) for accurate telemetry in Anthropic-driven workflows.  
- **Monitor budget accounting** closely: avoid updating key limits mid-request to prevent spend window skew (Issue #49179).  
- **Use `litellm doctor` CLI** (feature request [#34513](https://github.com/BerriAI/litellm/issues/34513)) to diagnose common SDK issues pre-deployment.

> ✅ **Next Steps**: Review the new [router architecture docs](https://github.com/BerriAI/litellm/pull/49235) and prepare for future deprecation of legacy Python routing logic.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-10-31**

---

### **1. Today's Highlights**  
The latest release, **v0.1.913-beta**, introduces significant UI/UX refinements—including customizable backgrounds—and improves performance on Linux and VRAM-constrained systems, particularly for Qwen-Image generation and DeepSeek-V4.1-Flash-1017 GGUF models. Critical stability fixes address model loading crashes, memory estimation inaccuracies, and isolated install regressions, while new PRs lay the groundwork for real-time resource monitoring and enhanced CPU training support.

---

### **2. Releases & Breaking Changes**  
- **v0.1.913-beta** (released 2026-10-31):  
  - Improved app update times and UI/UX with custom background support.  
  - Optimized performance for Qwen-Image gen and DeepSeek-V4.1-Flash-1017 GGUF models under low VRAM conditions.  
  - Fixed critical regression in `UNSLOTH_STUDIO_HOME` isolation: prior installs silently overwrote baseline venvs (#16186, #16185).  
  - [Release Notes](https://github.com/unslothai/unsloth/releases/tag/v0.1.913-beta)

---

### **3. New Model & Hardware Support**  
- **New Model**:  
  - **DeepSeek-V4.1-Flash-1017** added to Unsloth’s Dynamic v3.1 GGUF support ([docs](https://unsloth.ai/docs/models/deepseek-v4.1-flash)).  
- **Hardware & Backend**:  
  - **AMD ROCm** and **Intel XPU Triton** paths now include verified swap handling and pip backup skips via `studio/setup.ps1` fix (#16250).  
  - **MLX** continues to see improvements, though issues persist with model visibility (`curl v1/models`) and memory estimation accuracy on Apple Silicon (#15832, #15935).  
- **Quantization**:  
  - Full support for **Q4_K_M** and **GGUF** formats remains active; LFM3-Omni GGUF and Safetensors are under testing (#16054, #16065).

---

### **4. Performance & Optimization**  
- **Kernel-Level**:  
  - **Fused LoRA optimization** (#16071): Adds rank-r term *inside* base GEMM to prevent rounding loss of small updates—critical for early-stage fine-tuning convergence.  
  - **Tensor subclass preservation** (#16249): Maintains type fidelity in eager accumulator buffers, improving precision during CPU inference.  
- **Memory & Throughput**:  
  - Reduced commit charge on Windows + AMD systems after GGUF load (#16015), mitigating RAM exhaustion.  
  - **Gemma 3** now defaults to non-flex attention path for faster one-token decode, bypassing slower fallback logic (#16041).  
- **UI/UX**:  
  - New live monitor proposal (#15630) enables per-GPU, per-resource tracking (CPU/RAM/VRAM), with toggleable Unsloth-specific resource share.

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR? | Link |
|---------|------|--------|--------|------|
| 🔴 **Critical** | Isolated install via `UNSLOTH_STUDIO_HOME` silently overwrites existing venv | Open | ✅ Yes (#16186, #16185) | [Issue #16186](https://github.com/unslothai/unsloth/issues/16186) |
| 🔴 **Critical** | Image generation returns 404 after managed account loads cached gated model | Open | ❌ No | [Issue #16178](https://github.com/unslothai/unsloth/issues/16178) |
| 🟡 **High** | MLX memory estimator reports 69.7 GiB for 24 GB Mac model | Closed | ⚠️ Partial | [Issue #15935](https://github.com/unslothai/unsloth/issues/15935) |
| 🟡 **High** | AppImage UI lags severely on Linux | Open | ❌ No | [Issue #15999](https://github.com/unslothai/unsloth/issues/15999) |
| 🟡 **High** | Spell check fails completely on Linux desktop | Open | ❌ No | [Issue #16184](https://github.com/unslothai/unsloth/issues/16184) |
| 🟡 **Medium** | Context fails to compact after long conversations | Closed | ❌ No | [Issue #15800](https://github.com/unslothai/unsloth/issues/15800) |

> Note: Several regressions stem from recent UI/Flows rework and component unification efforts (#15609, #15630).

---

### **6. What This Means for Application Developers**  
- **Build robust agents**: The new MCP tool integration (#15631, #15629) allows direct access to unsloth-native tools from chat agents—ideal for embedding dynamic capabilities like code execution or data retrieval without external servers.  
- **Design for resource awareness**: With upcoming live monitoring (#15630), developers should prepare to expose GPU/CPU/memory usage in their agent dashboards or logs.  
- **Ensure safe deployment**: Avoid `UNSLOTH_STUDIO_HOME` if managing multiple environments—use dedicated directories and verify isolation behavior until fix lands.  
- **Support CPU-only workflows**: The new PR enabling CPU training (#16175) means your apps can now run fine-tuning jobs even without a GPU—great for edge or CI/CD pipelines.  
- **Validate model inputs**: Be cautious with large (>50GB) ModelScope-hosted models—current blocks may need override (#15989).  

> 📌 **Action Item**: Audit all model loading paths for `offload_layers` and MoE models—PR #15159 warns of `get_peft_model` crashes in such cases.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*