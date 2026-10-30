# AI Infrastructure Digest 2026-10-30

> Generated: 2026-10-30 08:16 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

---

### **1. Ecosystem Overview**  
The AI inference infrastructure landscape in late 2026 is defined by a convergence of high-throughput optimization, heterogeneous hardware support, and growing complexity in agent-driven workloads. Projects like vLLM and SGLang are pushing the envelope in distributed serving and speculative decoding, while llama.cpp and Unsloth focus on low-level efficiency and edge deployment. LiteLLM emerges as a critical unifying layer for cost-aware, multi-provider routing, and Ollama continues to bridge developer accessibility with production-grade features. The ecosystem is increasingly fragmented by backend specialization—NVIDIA CUDA, ROCm/HIP, Apple Silicon, and emerging NPUs—driving intense competition in kernel-level tuning and memory management.

---

### **2. Activity Comparison**

| Project       | Issues Open (Last 7d) | PRs Merged (Last 7d) | Releases (Last 24h) | Breaking Changes |
|---------------|------------------------|------------------------|----------------------|------------------|
| **vLLM**      | 38                     | 15                     | None                 | None             |
| **SGLang**    | 42                     | 12                     | None                 | None             |
| **llama.cpp** | 39                     | 18                     | `b12001`, `b12000`   | Yes (`b12000`)   |
| **Ollama**    | 35                     | 10                     | None                 | None             |
| **LiteLLM**   | 31                     | 14                     | `v1.109.0-dev.2`     | Yes              |
| **Unsloth**   | 33                     | 11                     | None                 | Yes (beta change)|

> ✅ *Note: High activity across all projects reflects mature, rapidly evolving ecosystems. llama.cpp and LiteLLM stand out with recent releases; LiteLLM leads in breaking changes due to API evolution.*

---

### **3. Model Support Race**

| New Model / Architecture | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------------------------|------|--------|-----------|--------|---------|---------|
| **Qwen3-VL / Qwen3.5**   | ✅ (ViT CUDA graphs) | 🚧 (in progress) | ❌ | ❌ | ❌ | ❌ |
| **Kimi K3 / Kimi K2.5**  | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ (MLX/LoRA) |
| **DeepSeek-V4.1**        | ✅ (ROCm, TP4 tuning) | ✅ (HiCache/DSPark) | ✅ (FP8/OCP) | ❌ | ✅ (xAI/grok-4.3) | ❌ |
| **LFM3-Omni / LFM3-1.7B**| ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (active dev) |
| **Metis-1.6**            | ❌ | ❌ | ✅ (hybrid Mamba-GQA) | ❌ | ❌ | ❌ |
| **Mimo V2.6**            | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Soniox STT (stt-rt-v5)**| ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

> 🏆 **Leaderboard**:  
> - **llama.cpp** leads in *low-level model diversity* (GGUF, Hexagon, OpenCL).  
> - **Unsloth** is ahead in *emerging agent models* (LFM3 series).  
> - **Ollama** dominates *user-facing model adoption* via easy pull commands.  
> - **LiteLLM** excels in *multi-provider compatibility* for new backends (xAI, Azure, Fireworks).

---

### **4. Performance Frontier**

| Optimization Focus         | vLLM                  | SGLang               | llama.cpp           | Ollama             | LiteLLM              | Unsloth             |
|----------------------------|------------------------|-----------------------|----------------------|--------------------|-----------------------|----------------------|
| **KV Cache & Offload**     | ✅ Tiered, sleep mode   | ✅ HiCache + DCP      | ✅ MoE cache sharing | ⚠️ KV state leak (ROCm) | ❌ Streaming cost loss | ✅ Auto-compaction |
| **Speculative Decoding**   | ✅ Stable (draft staleness) | ✅ HiCache + DCP     | ✅ Test suite added | ❌ Draft crash (issue #47218) | ❌ Tool call corruption | ❌ Not supported |
| **Quantization**           | ✅ MXFP4, MoE padding   | ✅ AMD monokernel     | ✅ OCP FP8 E4M3      | ❌ Ternary request    | ✅ Token count fix    | ❌ No quantizer API  |
| **Distributed Serving**    | ✅ PP, TP, FlashInfer   | ✅ Multi-node scheduler | ❌ Limited          | ❌ No scaling        | ✅ Provider routing   | ❌ No distributed    |
| **Kernel-Level Tuning**    | ✅ DiffKV, FlashInfer   | ✅ DSpark, AMD kernels| ✅ Metal MXFP4       | ❌ GPU init error    | ❌ No kernel control  | ❌ Vulkan experimental|

> 🔥 **Frontier Leaders**:  
> - **vLLM** leads in *high-throughput distributed inference*.  
> - **llama.cpp** leads in *cross-backend kernel innovation* (Metal, OpenCL, RDNA4_M).  
> - **SGLang** leads in *agentic performance* (HiCache, DCP, scheduler metrics).

---

### **5. Layer Positioning**

| Project       | Primary Layer               | Key Differentiator |
|---------------|-----------------------------|--------------------|
| **vLLM**      | **Inference Engine**        | Industry standard for large-scale, high-throughput serving; optimized for NVIDIA GPUs and distributed setups. |
| **SGLang**    | **Inference Engine + Agent Orchestration** | Built for agentic systems; strong focus on speculative decoding, session-aware caching, and scheduling observability. |
| **llama.cpp** | **Local Runtime / Edge Inference** | Designed for CPU/GPU/edge devices; enables offline, lightweight inference with minimal dependencies. |
| **Ollama**    | **Developer Gateway / Local CLI** | User-friendly interface for local inference; acts as a local LLM server with model lifecycle management. |
| **LiteLLM**   | **Gateway / Cost Proxy**    | Multi-provider routing layer with unified billing, security, and guardrails—critical for enterprise cost control. |
| **Unsloth**   | **Agent-Focused Runtime + UI** | Combines inference engine with MLX vision support, long-context compaction, and developer-facing tools for real-time agents. |

> 📌 *This reflects a clear stratification: engines (vLLM/SGLang), runtime (llama.cpp), gateways (LiteLLM), and application platforms (Ollama/Unsloth).*

---

### **6. Trend Signals**

#### **Emerging Trends from Today’s Activity**:
1. **Agent-Centric Optimization**: HiCache, DCP, speculative decoding, and context compaction dominate discussions—indicating that *agent workflows* are now the primary workload driver.
2. **Heterogeneous Hardware Push**: ROCm, AMD Monokernels, Apple Silicon, and NPU support are no longer niche—they’re core development priorities across all major projects.
3. **Cost Transparency Crisis**: Silent cost tracking failures in LiteLLM and Ollama reveal growing urgency around *financial accountability* in multi-provider inference.
4. **Edge & Mobile Expansion**: llama.cpp’s support for Hexagon, OpenCL, and RDNA4_M signals a shift toward *deploying LLMs beyond data centers*.
5. **Stability Over Features**: Despite rapid feature growth, *crash fixes and regression resolution* are prioritized—especially around memory safety, KV cache, and streaming.

#### **What Application Developers Should Watch**:
- **Avoid silent fallbacks**: Monitor for CUDA/ROCm crashes or CPU fallbacks (e.g., Ollama issue #17380).
- **Validate cost logging**: Never assume `usage.cost` is accurate—audit responses from OpenRouter, Gemini, and xAI.
- **Prioritize stability in agentic flows**: Use `SGLANG_JIT_DEEPGEMM_FAST_WARMUP` and avoid speculative decoding until crashes are resolved.
- **Plan for cross-platform portability**: Leverage llama.cpp for ARM/Adreno/Apple Silicon, and LiteLLM for multi-cloud cost control.
- **Prepare for breaking changes**: LiteLLM’s `v1.109.0-dev.2` and vLLM’s upcoming releases may require client updates.

> ✅ **Final Recommendation**: Build with **vLLM or SGLang** for scalable inference, **LiteLLM** for cost visibility, **llama.cpp** for edge deployment, and **Unsloth** for agent-native UIs—while treating all outputs as potentially unstable until core regressions are patched.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-10-30**

#### **1. Today's Highlights**  
The vLLM project continues to deepen its support for next-generation multimodal and high-throughput models, with critical work on ViT CUDA graph optimization (#38175) and speculative decoding stability (#62818). Key performance improvements landed for DeepSeek-V4.1 on ROCm (#63847, #59802), while new PRs address memory safety in sleep mode (#64630) and benchmark correctness (#64703).

#### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were introduced.

#### **3. New Model & Hardware Support**  
- **Model Support**:  
  - Full CUDA graph support for ViT encoders in multimodal models (Qwen3-VL, GLM-V, Kimi K2.5, Qwen3.5) via [RFC #38175](https://github.com/vllm-project/vllm/issues/38175).  
  - Added pipeline parallelism support for **DiffusionGemma** ([PR #52015](https://github.com/vllm-project/vllm/pull/52015)).  
- **Hardware & Backend**:  
  - **ROCm (gfx950 / MI355X)**: Performance optimizations for `amd/Qwen3.8-Flash-Next-Quark-MXFP4` ([Issue #59575](https://github.com/vllm-project/vllm/issues/59575)), including persistent_topk kernel support ([PR #63847](https://github.com/vllm-project/vllm/pull/63847)).  
  - **NVIDIA SM120 (RTX PRO 6000 Blackwell)**: Fix for FlashInfer sparse-MLA decode kernel missing `page_block_size=32` instantiation ([Issue #59203](https://github.com/vllm-project/vllm/issues/59203)).

#### **4. Performance & Optimization**  
- **Attention & Kernel**:  
  - On SM80, `DiffKV` prefill tiling with `BLOCK_M=128 + KV tile 64` achieves **3.0x speedup** on 200K-context chunks ([Issue #61095](https://github.com/vllm-project/vllm/issues/61095)).  
  - For **DeepSeek-V4.1-Flash**, TP4 concurrency-1 decode step reduced via specialized kernel tuning ([PR #64364](https://github.com/vllm-project/vllm/pull/64364)).  
- **Memory & Offload**:  
  - Level-1 sleep mode now uses exact-size, reusable pinned CPU backup to reduce host memory overhead by up to **2x** ([PR #64630](https://github.com/vllm-project/vllm/pull/64630)).  
  - Eliminate redundant Q/K/V copies in GLM-5.3-Flash prefill convolution by producing dense output directly ([PR #61614](https://github.com/vllm-project/vllm/pull/61614)).  
- **Quantization**:  
  - Reduced MXFP4 MoE padding on SM90 for better memory efficiency ([PR #56697](https://github.com/vllm-project/vllm/pull/56697)).

#### **5. Stability & Regressions**  
- **Critical Crashes**:  
  - **Qwen3.8-27B (GDN hybrid)** crashes with illegal memory access (Xid 31) under prefix caching on sm_86; regression since v0.28.0 ([Issue #63505](https://github.com/vllm-project/vllm/issues/63505)). *No fix PR yet.*  
  - **GLM-5.x (DSA)** under PP > 1 exhibits stale top-k index reuse due to layer re-use at stage start ([Issue #63170](https://github.com/vllm-project/vllm/issues/63170)). *No fix PR yet.*  
- **Other Issues**:  
  - **Kimi-K3** prefix cache fails when prompt length is exactly 1536 tokens ([Issue #50235](https://github.com/vllm-project/vllm/issues/50235)).  
  - **P2P KV offload tier** does not fail fast on peer faults ([Issue #63602](https://github.com/vllm-project/vllm/issues/63602)).  
  - **Tiered KV offload** promotes all waiting requests, filling primary DRAM pool ([Issue #49902](https://github.com/vllm-project/vllm/issues/49902)).

#### **6. What This Means for Application Developers**  
- Use **CUDA graphs** for multimodal models (Qwen3-VL, Kimi) to unlock full inference acceleration—ensure you’re on a recent nightly build with ViT graph support.  
- Avoid `page_block_size=32` on **SM120** GPUs until the FlashInfer kernel is patched ([#59203](https://github.com/vllm-project/vllm/issues/59203)).  
- If using **speculative decoding with structured outputs**, be cautious of draft staleness issues—validate your grammar logic carefully.  
- For **multi-GPU or disaggregated inference**, monitor GPU memory usage per rank (tracked in [#49816](https://github.com/vllm-project/vllm/issues/49816)) and consider enabling per-GPU metrics in future releases.  
- **Benchmarking**: Always ensure `--base-url` points to server root without `/v1` suffix ([PR #64698](https://github.com/vllm-project/vllm/pull/64698)) to avoid silent 404 errors.

> 🔗 **Key Resources**:  
> - [vLLM GitHub Issues](https://github.com/vllm-project/vllm/issues)  
> - [vLLM PRs](https://github.com/vllm-project/vllm/pulls)  
> - [vLLM Docs – Benchmark CLI](https://github.com/vllm-project/vllm/blob/main/docs/benchmarking/cli.md)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-10-30**

---

### **1. Today's Highlights**  
The SGLang project continues to strengthen its support for advanced inference patterns, with critical fixes to HiCache and DSpark performance on AMD/Ascend hardware, and active work on session-aware KV cache orchestration for agentic systems. A major focus remains on CI stability and flaky test resolution, as highlighted by the top-ranked issue #42752 tracking recurring infrastructure failures in PR testing workflows.

---

### **2. Releases & Breaking Changes**  
*None* — No new releases or breaking API/config changes were published in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- ✅ **AMD ROCm (gfx950 / MI355X)**: Ongoing optimization for `Qwen3.8-Flash-Next` via Issue #44304; integration of `--enable-amd-monokernel` runtime switches now supports key DeepSeek-V4.1 optimizations.
- ✅ **T-Head SAIL (Zhenwu 810/810E/M890P)**: Roadmap milestone reached with Issue #37519 — upstreaming first-class support is now a highest-priority feature.
- ✅ **NPU (Ascend A3/DV4)**: Active development for DSpark compact mode (#38440), dynamic expert load balancing (#47266), and M-RoPE kernel fixes (#44506).
- ✅ **Apple Silicon (M-series)**: Continued improvements to HiCache and unified radix cache handling (#43897, #47422).

---

### **4. Performance & Optimization**  
- 🔥 **DeepSeek-V4.1 Cold Start**: `SGLANG_JIT_DEEPGEMM_FAST_WARMUP` enables **3–5x faster cold start** without detectable runtime degradation at chunked-prefill 16384 (#47397).
- 🚀 **HiCache + DCP on AMD**: PR #45205 enables HiCache for DSA DCP with packed MTP drafts, reducing redundant prefills under high concurrency (critical for agentic workloads).
- ⚙️ **Scheduler Metrics**: PR #46485 adds `scheduler_iterations_total`, improving observability for scheduling efficiency monitoring.
- 📊 **MFU/MBU Bandwidth Fix**: PR #47416 corrects overcounting in decode read bandwidth estimation, improving accuracy of hardware utilization metrics.

---

### **5. Stability & Regressions**  
- ⚠️ **Critical Scheduler Crash (DeepSeek-V4)**: Issue #47218 reports scheduler crash when multimodal offsets become `None` during concurrent aborted streams — affects full TP/EP workers and triggers container restarts. *No fix PR yet.*
- ⚠️ **HiCache Host-Tier Hit Drop**: Issue #45400 shows host-tier hits drop to zero when evicted working set exceeds ~3 long prefixes due to Mamba pool exhaustion. *Impact: degraded cache reuse in agentic scenarios.*
- ⚠️ **CUDA Graph Capture Failure (GLM-5.3-Flash)**: FA4 backend crashes on SM120 (RTX PRO 6000) during hybrid extend reshape; only Triton works (#42012). *High severity for flash attention users.*
- ⚠️ **CI Flakiness**: Issue #42752 tracks persistent CI infrastructure failures across `PR Test Base/Extra` runs — blocking PR validation efforts. *No immediate fix identified.*

---

### **6. What This Means for Application Developers**  
Developers building agentic systems should prioritize upgrading to `lmsysorg/sglang:dev-dsv41` or later to benefit from HiCache+DCP optimizations and improved scheduler stability. Use `SGLANG_JIT_DEEPGEMM_FAST_WARMUP=1` to reduce cold-start latency in production deployments. For multi-modal or high-concurrency applications, monitor for potential crashes during stream abortion (issue #47218) and consider disabling speculative decoding if encountering `None` offset errors. On AMD/Ascend platforms, enable `--enable-amd-monokernel` and `--enable-eplb` for optimal MoE and draft performance. Stay vigilant about CI instability — expect delays in merging PRs until #42752 is resolved.  

👉 [Issue #42752 – CI Flakiness](https://github.com/sgl-project/sglang/issues/42752)  
👉 [PR #47397 – Fast Warmup](https://github.com/sgl-project/sglang/pull/47397)  
👉 [PR #45205 – HiCache + DCP on AMD](https://github.com/sgl-project/sglang/pull/45205)

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-10-30**

---

### **1. Today's Highlights**  
The latest release `b12001` introduces critical ROCm/HIP support for RDNA4_M (gfx1170/gfx1171), aligning low-level GPU defines with the architecture’s 128b WMMA capabilities. Concurrently, OCP FP8 E4M3 quantization support lands in `b12000`, enabling efficient inference on models like *Qwen3.8-27B-NVFP4*—a major step toward broader FP8 adoption across heterogeneous backends.

---

### **2. Releases & Breaking Changes**  
- **`b12001`**: Adds dedicated `RDNA4_M` family definition for gfx1170/gfx1171 under ROCm/HIP, correcting misalignment between hardware capabilities and kernel behavior. [PR #31373](https://github.com/ggml-org/llama.cpp/pull/31373)  
- **`b12000`**: Enables full OCP FP8 E4M3 support with proper scale materialization and conversion fixes for NVFP4→FP8 fallbacks. [PR #27512](https://github.com/ggml-org/llama.cpp/pull/27512)  
- **Migration Note**: Users upgrading from prior versions should re-convert models using `--quantize` if relying on FP8 E4M3 or NVFP4 → FP8 pathing.

---

### **3. New Model & Hardware Support**  
- **Models**:  
  - Added support for **Limite 1B - Violetto** via GGUF arch and conversion tooling. [PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433)  
  - Initial support for **Metis-1.6** (Praxis ~2.94B, Logos ~10.2B) with hybrid Mamba-2 + GQA attention and fixed-depth recurrence. [PR #31134](https://github.com/ggml-org/llama.cpp/pull/31134)  
  - **Mistral Le Chonk** model extended with DSA and sdpark optimizations. [PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376)  

- **Hardware & Backends**:  
  - **ROCm/HIP**: Full RDNA4_M (gfx1170/gfx1171) targeting. [PR #31373](https://github.com/ggml-org/llama.cpp/pull/31373)  
  - **OpenCL**: Initial `iq2_s` support with AOS/SOA, tiled MM, DP4A (Adreno), and narrow batch optimization. [PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267)  
  - **Hexagon**: Basic `UPSCALE` ops with vectorized staging via `vgather`. [PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331)  
  - **Metal**: Optimized MXFP4 mat-vec kernel using fp16 bit trick and aligned 32-bit loads; full threadgroup utilization for unary/binary/GLU. [PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)  

---

### **4. Performance & Optimization**  
- **Kernel-Level**: Metal MXFP4 mat-vec now avoids lookup tables and uses aligned word loads, reducing latency. [PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)  
- **Memory**: Reuse of tensor staging buffer skips direct-I/O bounce buffers, improving CPU-GPU data transfer efficiency. [PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321)  
- **Speculative Decoding**: Test suite added to validate speculative decoding methods (MTP, DFlash, LiLiCorr). [PR #31027](https://github.com/ggml-org/llama.cpp/pull/31027)  
- **Quantization**: OCP FP8 E4M3 enables higher throughput on supported hardware; early benchmarks show up to 1.5x speedup over NVFP4 on Qwen3.8-27B.  
- **SYCL/XMX-ESIMD**: PR #31195 proposes optimized IQ quantization decoding and XMX kernels for Intel Battlemage GPUs—expected to close ~35% MoE prompt processing gap. [PR #31195](https://github.com/ggml-org/llama.cpp/pull/31195)

---

### **5. Stability & Regressions**  
- **Critical**: `Metal` backend fails to initialize on macOS 14 / M2 Ultra due to `constexpr bfloat cast` error in `unary.metal`. [Issue #31272](https://github.com/ggml-org/llama.cpp/issues/31272)  
- **Severe**: LFM3 models fail on any `response_format` (JSON schema/object) with "Unexpected empty grammar stack after accepting piece: 6 (30)". [Issue #31328](https://github.com/ggml-org/llama.cpp/issues/31328)  
- **High**: CUDA regression on RTX 5090 Laptop (sm_120): qwen35 decodes at only 76% of roofline vs. 86% on RTX 4090 (Linux). [Issue #28196](https://github.com/ggml-org/llama.cpp/issues/28196)  
- **Moderate**: SYCL BF16 conversion not supported on Arrow Lake iGPU. [Issue #31029](https://github.com/ggml-org/llama.cpp/issues/31029)  
- **Fixes in Progress**: PR #31406 addresses KV cache rotation for reused layers; PR #31368 prevents invalid rollbacks past ubatch snapshots.

---

### **6. What This Means for Application Developers**  
- **Deployers**: Prioritize `b12000+` builds when using FP8 E4M3 or OCP quantizations—especially for Qwen3.8 and similar models. Avoid older versions where NVFP4 → FP8 conversion may fail silently.  
- **Edge & Mobile**: Hexagon (`UPSCALE`) and OpenCL (`iq2_s`) support expands deployment options on ARM SoCs and Adreno devices.  
- **Multi-GPU & Cloud**: Use `--moe-cache-cpu-share auto` (in PR #31286) to dynamically manage MoE expert memory across CPU/GPU tiers—ideal for large-scale inference.  
- **Stability Warnings**: Avoid LFM3 with `response_format` until fix is merged. For Metal users on Apple Silicon, consider downgrading to `b11997` or patching manually.  
- **Future-Proofing**: Monitor PRs around `moe-cache` (SLRU → ClassLRU) and speculative decoding test coverage—these will impact scalability and cost efficiency in agent systems.

> 🔗 **Official Website**: [https://llama.app](https://llama.app)  
> 📌 **Attestations**: [GitHub Attestations (61557038)](https://github.com/ggml-org/llama.cpp/attestations/61557038)  
> 🍏 **Apple Silicon Builds**: [macOS Apple Silicon (arm64)](https://github.com/ggml-org/llama.cpp/releases/tag/b12001)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-10-30**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to expand its agent and multimodal capabilities, with key contributions focused on tool call robustness, OpenAI compatibility fixes, and support for new models like *Mimo V2.6* and *JEV*. Critical stability issues persist in ROCm (gfx1151 iGPU) and CUDA environments, including memory bleed and silent fallbacks, while user demand for a cross-platform GUI app remains strong.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours.  
- **Note**: Version `0.40.13` is currently in use (per Issue #19383), but no formal release notes or migration guidance were published recently.

---

### **3. New Model & Hardware Support**  
- ✅ **New Model Support**:  
  - [`dhiltgen/mimo-v2.6`](https://github.com/ollama/ollama/pull/19400): Added MLX and GGUF support for MiMo V2.6. Available via `ollama pull dhiltgen/mimo-v2.6`.  
  - [`dhiltgen/jev`](https://github.com/ollama/ollama/pull/19399): Initial support for JEV decision and vision models via MLX, including prompt encoding and API routing.  
- 🚧 **Hardware/Backend Updates**:  
  - ROCm backend now under scrutiny for GPU state leakage on AMD Strix Halo (gfx1151 iGPU) — see [Issue #17847](https://github.com/ollama/ollama/issues/17847).  
  - Ternary quantization support requested for Bonsai 2 ([#19388](https://github.com/ollama/ollama/issues/19388)), indicating growing interest in advanced quantization schemes.

---

### **4. Performance & Optimization**  
- 🔧 **Model Loading & Memory Management**:  
  - PR [#19284](https://github.com/ollama/ollama/pull/19284): Prevents spurious eviction of CPU-only runners (e.g., embedding models) during GPU memory prediction — improves efficiency on mixed-workload systems.  
  - PR [#17615](https://github.com/ollama/ollama/pull/17615): Fixes mismatch in KV cache accounting between GraphSize and PredictServerVRAM — prevents inaccurate memory predictions.  
- ⏱️ **Latency & Streaming**:  
  - PR [#19396](https://github.com/ollama/ollama/issues/19396): Proposal for native streamed audio output from LFM3-Omni model — enables real-time TTS responses without external processing.  
  - PR [#19390](https://github.com/ollama/ollama/issues/19390): Request for native video/audio input/output support on Windows — critical for multimodal agent workflows.

---

### **5. Stability & Regressions**  
**Critical**:  
- [Issue #17847](https://github.com/ollama/ollama/issues/17847): **ROCm (gfx1151 iGPU) leaks KV state across requests** — response content from prior request contaminates current one. High severity; impacts inference correctness. No fix PR yet.  
- [Issue #17380](https://github.com/ollama/ollama/issues/17380): Intermittent `CUDA error: shared object initialization failed` during warmup, silently falling back to CPU — leads to performance degradation and hard-to-debug behavior.  

**High Impact**:  
- [Issue #19385](https://github.com/ollama/ollama/issues/19385): Users unable to roll back models after update (e.g., Qwen3:4b) — urgent need for version rollback mechanism.  
- [Issue #19375](https://github.com/ollama/ollama/issues/19375): GGUF metadata read failure due to tensor size overflow (`ffn_down_exps.weight`) in large MoE models (e.g., gpt-oss:120b) — blocks model loading.  

**Open PRs Addressing Stability**:  
- [#17566](https://github.com/ollama/ollama/pull/17566): Token budget control for thinking — prevents runaway reasoning.  
- [#17564](https://github.com/ollama/ollama/pull/17564): Prevents handing over incomplete tool calls — crucial for agent reliability.

---

### **6. What This Means for Application Developers**  
- **Agent Development**: Expect increased fragility in tool call handling (especially with `qwen3.8:27b-mlx` and `qwen3.5:4b`), where `tool_calls` are dropped if assistant generates non-empty text first ([#19353](https://github.com/ollama/ollama/issues/19353)). Use `content: ""` + `tool_calls` explicitly until fixes land.  
- **OpenAI Compatibility**: Be cautious with `content: []` arrays — they may cause `tool_calls` to be moved incorrectly ([#19152](https://github.com/ollama/ollama/pull/19152)). Also, `/v1/models` returns `null` instead of `[]` when no models exist — breakages possible in strongly-typed SDKs ([#19381](https://github.com/ollama/ollama/issues/19381)).  
- **Multimodal Workflows**: Native audio/video support is highly anticipated ([#19390](https://github.com/ollama/ollama/issues/19390)) — consider building around LFM3-Omni with streaming TTS proposal ([#19396](https://github.com/ollama/ollama/issues/19396)) as a workaround.  
- **Deployment Strategy**: Avoid using ROCm on gfx1151 hardware until [issue #17847](https://github.com/ollama/ollama/issues/17847) is resolved. On CUDA, monitor for silent CPU fallbacks during warmup ([#17380](https://github.com/ollama/ollama/issues/17380)).

> 🔗 **Stay Updated**: Follow key issues and PRs at [github.com/ollama/ollama](https://github.com/ollama/ollama).

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

---

### **1. Today's Highlights**  
LiteLLM’s latest updates focus on critical cost accounting accuracy, streaming reliability, and improved support for emerging models like Kimi K3 and Soniox real-time STT. Key fixes address silent spend loss in OpenAI-compatible upstreams (`#47533`, `#38507`) and corrupted tool call contexts in Gemini streaming (`#25869`). The proxy now preserves `x-litellm-applied-guardrails` only for actual runs (`#48472`) and correctly reports live key spend in Prometheus gauges (`#49199`).

---

### **2. Releases & Breaking Changes**  
- **v1.109.0-dev.2** released with enhanced security: all Docker images are now signed via [cosign](https://docs.sigstore.dev/cosign/overview/) using the key from [commit `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065).  
- **Breaking change**: `/v1/responses` streaming now emits named SSE events (e.g., `event: content`) instead of `[DONE]`—clients relying on `[DONE]` must update their logic ([PR #47980](https://github.com/BerriAI/litellm/pull/47980)).  
- **Breaking change**: Database-only `general_settings` keys are now ignored unless managed by the settings API ([PR #48967](https://github.com/BerriAI/litellm/pull/48967)).

---

### **3. New Model & Hardware Support**  
- **Native support requested**: Kimi K3 (Moonshot AI), Inkling (Thinking Machines Lab), and Tinker platform ([Issue #33921](https://github.com/BerriAI/litellm/issues/33921)).  
- **Soniox real-time streaming STT (stt-rt-v5)** added to proxy request list ([Issue #43677](https://github.com/BerriAI/litellm/issues/43677)).  
- **Gemini Live session setup** now sent at first content, ensuring `session.update` is applied ([PR #39864](https://github.com/BerriAI/litellm/pull/39864)).  
- **Azure AI Foundry grok-4-1**, **xAI grok-4.3**, and **Fireworks DeepSeek V4.1** tool requests now counted per provider billing ([PR #49074](https://github.com/BerriAI/litellm/pull/49074)).

---

### **4. Performance & Optimization**  
- **Latency & throughput**: Vertex AI pass-through auth now runs once per call (was twice), reducing latency and improving budgeting fidelity ([PR #48942](https://github.com/BerriAI/litellm/pull/48942)).  
- **Token counting accuracy**: Tool call token counts now align with Azure and xAI billing (previously off by up to 300 tokens) — crucial for context window control ([PR #49074](https://github.com/BerriAI/litellm/pull/49074)).  
- **Deployment caching**: SAP provider deployments now use process-wide caching keyed on `(base_url, resource_group)` for faster resolution ([PR #48597](https://github.com/BerriAI/litellm/pull/48597)).

---

### **5. Stability & Regressions**  
| Severity | Issue | Impact | Fix Status |
|---------|------|--------|------------|
| 🔴 Critical | Cost tracking fails silently for OpenRouter Responses API (`call_type=aresponses`) — spend logged as $0 despite real usage ([#38507](https://github.com/BerriAI/litellm/issues/38507)) | Real cost not reflected in budgets; financial audit risk | ❌ Not yet fixed |
| 🔴 Critical | Provider-reported `usage.cost` trusted verbatim without unit validation → phantom spend ([#35829](https://github.com/BerriAI/litellm/issues/35829)) | Can cause catastrophic overbilling if upstream returns non-USD values | ❌ Not yet fixed |
| 🟡 High | Streaming drops upstream `usage` when final chunk has non-empty `choices` → cached_tokens lost, full input billed ([#36168](https://github.com/BerriAI/litellm/issues/36168)) | Inaccurate cost reporting for cached responses | ✅ PR open: [#49069](https://github.com/BerriAI/litellm/pull/49069) |
| 🟡 High | `stream_chunk_builder` corrupts Gemini `server_side_tool_invocations` → "Corrupted tool call context" on follow-up turns ([#25869](https://github.com/BerriAI/litellm/issues/25869)) | Breaks agentic workflows using tool calls + web search | ✅ PR open: [#49061](https://github.com/BerriAI/litellm/pull/49061) |
| 🟡 Medium | In-memory spend buffers dropped on proxy shutdown ([#34805](https://github.com/BerriAI/litellm/issues/34805)) | Risk of data loss during restarts or crashes | ✅ PR open: [#49199](https://github.com/BerriAI/litellm/pull/49199) |

---

### **6. What This Means for Application Developers**  
- **Audit your cost tracking**: If using OpenRouter, OmniRoute, or any OpenAI-compatible gateway, verify that `response_cost` reflects real usage — current versions may log $0 due to unhandled `usage.cost` in non-streaming mode ([#47533](https://github.com/BerriAI/litellm/issues/47533)).  
- **Avoid silent overbilling**: Never assume `usage.cost` is USD — implement client-side validation until [#35829](https://github.com/BerriAI/litellm/issues/35829) is resolved.  
- **Update streaming clients**: If you rely on `[DONE]` events from `/v1/responses`, migrate to named event handling (`event: done`) immediately ([PR #47980](https://github.com/BerriAI/litellm/pull/47980)).  
- **Use updated tooling**: For agents with tool calls + web search on Gemini, expect breakage until [#49061](https://github.com/BerriAI/litellm/pull/49061) lands — avoid `stream=True` until patched.  
- **Secure secrets**: Use `secrets_from_file` via [`#28994`](https://github.com/BerriAI/litellm/issues/28994) for Docker secrets or Kubernetes integrations.  

> ✅ **Recommendation**: Upgrade to `v1.109.0-dev.2` and validate cost logging, especially for high-volume or budget-sensitive workloads.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-10-30**

---

### **1. Today's Highlights**  
The Unsloth project continues to expand its support for emerging LLM architectures, with active development on the LFM3 series and improved handling of vision-language models via MLX. Key PRs focus on memory estimation accuracy (especially for Apple Silicon), UI stability across tabs and platforms, and enhanced context management for long conversations—critical for real-world agent applications.

---

### **2. Releases & Breaking Changes**  
*No new releases in the past 24 hours.*  
However, ongoing changes in `main` may affect users upgrading from v0.1.911-beta to v0.1.912-beta:  
- Model loading behavior has changed subtly, particularly for GGUF and safetensors models on MLX (see [PR #15945](https://github.com/unslothai/unsloth/pull/15945)).  
- The `/v1/models` endpoint now correctly lists auto-mapped safetensors checkpoints after a fix ([PR #15945](https://github.com/unslothai/unsloth/pull/15945)), resolving prior invisibility issues.

---

### **3. New Model & Hardware Support**  
- ✅ **LFM3 Series**: Active support for `LFM3-1.7B` and `LFM3-Omni` is under development ([Issue #15588](https://github.com/unslothai/unsloth/issues/15588), [PR #16053](https://github.com/unslothai/unsloth/pull/16053)).  
- 📌 **MLX Vision Models**: Improved handling of long-context vision chats on Apple Silicon ([PR #16079](https://github.com/unslothai/unsloth/pull/16079)) and support for GGUF LoRAs in image generation ([PR #16053](https://github.com/unslothai/unsloth/pull/16053)).  
- 🔧 **Vulkan Backend**: Experimental support for MiniCPM-V-4.7-1B on Vulkan (AMD GPUs) reported, though rendering remains inconsistent ([Issue #14987](https://github.com/unslothai/unsloth/issues/14987)).  
- 💻 **Windows + AMD**: Users report issues with model loading and high RAM commitment during GGUF load ([Issue #16015](https://github.com/unslothai/unsloth/issues/16015)).

---

### **4. Performance & Optimization**  
- ⚙️ **Memory Estimation Fix**: MLX memory estimator now accounts for full-width attention scores even without quantization ([PR #16021](https://github.com/unslothai/unsloth/pull/16021)), correcting overestimates (e.g., 69.7 GiB reported vs. 24 GB actual GPU capacity — [Issue #15935](https://github.com/unslothai/unsloth/issues/15935)).  
- 🚀 **Context Management**: Fixes applied to prevent context overflow and enable automatic compaction in long chats ([PR #15900](https://github.com/unslothai/unsloth/pull/15900), [Issue #15800](https://github.com/unslothai/unsloth/issues/15800)).  
- 🔄 **Pipeline Parallelism**: GradScaler sync fixes ensure consistent gradient scaling across pipeline stages in multi-GPU training ([PR #16070](https://github.com/unslothai/unsloth/pull/16070)).

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Related PR |
|------|----------|--------|------------|
| Core dumps during model ejecting (LLaMA/Server logs missing) | Critical | Closed | [Issue #14411](https://github.com/unslothai/unsloth/issues/14411) |
| AppImage UI lag on Linux | High | Open | [Issue #15999](https://github.com/unslothai/unsloth/issues/15999) |
| Text duplication across chat tabs | Medium | Closed | [PR #15795](https://github.com/unslothai/unsloth/pull/15795) |
| Dolphin Venice Edition not rendered properly in UI | Medium | Closed | [Issue #15858](https://github.com/unslothai/unsloth/issues/15858) |
| LFM3-Omni video input fails on Windows + AMD | Medium | Open | [Issue #16065](https://github.com/unslothai/unsloth/issues/16065) |
| LFM3-Omni Safetensors fails to load | Medium | Open | [Issue #16054](https://github.com/unslothai/unsloth/issues/16054) |

> Note: Several regressions stem from recent UI and backend refactors; fixes are being prioritized in Studio and MLX pipelines.

---

### **6. What This Means for Application Developers**  
- **Use case planning**: For production agents requiring long-context continuity (e.g., legal or research assistants), rely on `auto-compaction` and monitor context size via updated memory tracking tools ([Issue #16055](https://github.com/unslothai/unsloth/issues/16055)).  
- **Model selection**: Prioritize `LFM3` and `Dolphin` models on Apple Silicon using MLX—expect better performance but verify compatibility with local GGUF/LoRA workflows.  
- **Deployment caution**: Avoid ModelScope downloads >50GB until [PR #15994](https://github.com/unslothai/unsloth/pull/15994) is merged. Use direct Hugging Face links instead.  
- **Debugging tip**: If models crash mid-conversation, check `coredumpctl list` and ensure logs are enabled—recent core dumps lack traceability ([Issue #14411](https://github.com/unslothai/unsloth/issues/14411)).  

> ✅ **Recommendation**: Monitor `unslothai/unsloth#main` for upcoming v0.1.912 release candidates—several stability and memory fixes are queued.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*