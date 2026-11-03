# AI Infrastructure Digest 2026-11-03

> Generated: 2026-11-03 03:41 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-03**

---

### **1. Ecosystem Overview**  
The AI inference infrastructure landscape in late 2026 is marked by rapid convergence toward high-throughput, low-latency, and multi-modal serving for next-generation MoE and hybrid architectures. Projects are increasingly focused on hardware-specific optimizations (especially SM12x/Blackwell), speculative decoding efficiency, and cross-platform portability. A clear divide emerges between mature, production-ready engines (vLLM, Ollama) and rapidly evolving, innovation-driven platforms (SGLang, LiteLLM, Unsloth). The rise of Rust-based backends and GPU-resident LRU caching signals a shift toward performance-critical, memory-safe systems designed for agent-scale workloads.

---

### **2. Activity Comparison**

| Project | Issues Open | PRs Merged (Last 24h) | Release Status |
|--------|--------------|--------------------------|----------------|
| **vLLM** | 587 | 389 contributors → 927 commits | v0.33.0rc3 (final pre-release) |
| **SGLang** | 462 | 1,007 PRs from 257 contributors | v0.5.23 (stable release) |
| **llama.cpp** | 315 | 3 critical fixes merged | No new tag; active development |
| **Ollama** | 412 | 22 PRs (mostly stability) | v0.40.14-rc0 (RC) |
| **LiteLLM** | 298 | 12 major PRs (Rust migration) | No new release; beta phase |
| **Unsloth** | 276 | 6 PRs (UI/memory focus) | No new release |

> ✅ **Insight**: SGLang leads in community momentum and contributor velocity; vLLM shows strong engineering output ahead of final release. LiteLLM is pivoting to foundational architecture change with Rust.

---

### **3. Model Support Race**

| New Model / Architecture | Supported By | Notes |
|--------------------------|--------------|-------|
| **Reflection Beam-1.0 (501B MoE, 23B active)** | ✅ vLLM | First to support this scale; uses DFlash2 drafter |
| **Recursive Flint (40B MoE)** | ✅ vLLM | Also supported in SGLang and llama.cpp |
| **Yandex AliceAI-Foundation-80B-A3B** | ✅ vLLM | Added via #57963 |
| **Muse Spark Open / Beam-1.0** | ✅ SGLang | Full cookbook support; vision-language ready |
| **Clover MoE (AR-only)** | ✅ llama.cpp | GGUF-native, causal hybrid attention |
| **LFM3-Audio-2B** | ✅ Unsloth | Multimodal TTS chat via loader.ts |
| **Qwen3.8-Flash-Next** | ✅ All (vLLM/SGLang/llama.cpp/Ollama) | But FP8/KV cache issues persist across all |

> 🏆 **Winner**: **vLLM** and **SGLang** lead in cutting-edge model coverage — particularly for large MoE and multimodal models. **vLLM** has the edge in raw scale and architectural readiness.

---

### **4. Performance Frontier**

| Optimization Focus | Leading Projects | Key Advances |
|--------------------|------------------|--------------|
| **KV Cache & Offloading** | vLLM, SGLang, llama.cpp | Async lookup publication (#57474), HiSparse offload (Ascend), GPU-resident MoE LRU (#29949) |
| **Batching & Throughput** | vLLM, SGLang | Multi-sequence flash attention (`flash_attn_ext_rows`), live-KV replay (SM12x) |
| **Speculative Decoding** | vLLM, SGLang | Zero-bubble handoff (#64531), adaptive strategies in dev |
| **Quantization & Memory** | llama.cpp, Ollama | Q4_K_M gap closed except 0.8B; FP8 dequant fix (PR #16156) |
| **Distributed Serving** | SGLang, LiteLLM | Hybrid DSA KV pools, Bedrock Converse model routing |
| **Kernel-Level Speed** | SGLang, vLLM | Fused-A GEMM (K=256), inverse RoPE fused into `wo_a` path |

> 🔥 **Trend**: Optimization is shifting from generic throughput to **hardware-aware, context-sensitive scheduling**, especially around **MoE expert management**, **FP8 precision control**, and **asynchronous state handling**.

---

### **5. Layer Positioning**

| Project | Primary Layer | Role Summary |
|--------|---------------|--------------|
| **vLLM** | **Inference Engine** | High-performance, scalable CUDA engine; core for cloud/GPU inference |
| **SGLang** | **Multi-Backend Inference Engine + Gateway** | Unifies execution across NPU/ROCm/CUDA; gateway-like abstraction layer |
| **llama.cpp** | **Local Runtime / Embedded Engine** | CPU/GPU hybrid, low-footprint, ideal for edge and local deployment |
| **Ollama** | **Model Gateway / Local Server** | User-friendly CLI/API layer; bridges local models and cloud services |
| **LiteLLM** | **API Gateway / Proxy Layer** | Unified API facade; enables cost tracking, provider switching, streaming reliability |
| **Unsloth** | **Agent UI + Runtime Orchestrator** | Developer-facing frontend with model loading, memory management, and multimodal UX |

> 📌 **Positioning Insight**:  
> - **vLLM/SGLang** dominate at the *inference engine* layer.  
> - **LiteLLM/Ollama** serve as *gateway/proxy* layers for API standardization.  
> - **llama.cpp/Unsloth** specialize in *local/runtime* deployment and developer experience.

---

### **6. Trend Signals**

#### 🔍 **Key Industry Trends Extracted:**
1. **SM12x (Blackwell) is the new frontier** — multiple regressions (hangs, low throughput) in vLLM and SGLang signal that Blackwell adoption is accelerating but still unstable. Production use remains risky.
2. **Rust is becoming the performance benchmark** — LiteLLM’s sub-1ms overhead goal and vLLM’s experimental Rust frontend indicate a strategic pivot toward memory safety and latency reduction.
3. **MoE scaling is no longer theoretical** — Reflection Beam-1.0 and Recursive Flint support confirm MoE models are entering production-grade inference pipelines.
4. **Multimodal integration is maturing** — Muse Spark Open, Beam-1.0, and LFM3-Audio-2B show consistent progress in vision/audio support beyond text.
5. **Memory management is central** — GPU-resident LRU caches, dynamic sizing (Unsloth), and VRAM overprediction fixes highlight memory pressure as a top constraint.

#### 💡 **What Developers Should Watch:**
- **Avoid v0.33.0rc3 in production** until SM12x hangs and batch-invariance bugs are resolved (vLLM).
- **Monitor LiteLLM’s Rust migration** — expect near-term performance gains and potential breaking changes.
- **Leverage SGLang’s unified backend** for multi-hardware deployments (NPU/ROCm/CUDA).
- **Use Ollama’s `--cloud` flag** to discover models without pull overhead.
- **Build agent resilience around silent session cancellation** (Unsloth, Ollama) and non-deterministic decoding (vLLM).

> ✅ **Final Recommendation**: Prioritize **stability over novelty** for production systems. Use **vLLM v0.32.0** or **Ollama v0.40.13** for reliable inference. Reserve RC versions for testing only.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM Digest – 2026-11-03**

---

### **1. Today's Highlights**  
The vLLM project released **v0.33.0rc3**, marking a major milestone with 927 commits from 389 contributors (122 new). This release introduces support for cutting-edge models including *Reflection Beam-1.0* (501B MoE, 23B active), *Recursive Flint* (40B MoE), and *Yandex AliceAI-Foundation-80B-A3B*, expanding the ecosystem for high-throughput inference on next-gen architectures. Critical stability improvements are underway, especially around batch invariance and deterministic decoding.

---

### **2. Releases & Breaking Changes**  
- **v0.33.0rc3**: Final pre-release candidate for v0.33.0, now available via PyPI and Docker images.  
  - [GitHub Release](https://github.com/vllm-project/vllm/releases/tag/v0.33.0rc3)  
- No breaking API changes reported; backward compatibility maintained.  
- New `VLLM_USE_RUST_FRONTEND=1` flag remains experimental but is feature-parity tracked via #44280.

---

### **3. New Model & Hardware Support**  
#### **New Models Added**  
- **Reflection Beam-1.0** (501B MoE, 23B active) with DFlash2 drafter (#62074)  
- **Yandex AliceAI-Foundation-80B-A3B** (#57963)  
- **Recursive Flint** (40B MoE, 2.5B active) (#62938)  
- **Muse Spark** (new model added to supported list)  

#### **Hardware & Backend Support**  
- **SM12x (Blackwell)**: Ongoing critical fixes for DeepSeek-V4-Flash/DSv4.1 on RTX PRO 6000 Blackwell (SM120) and DGX Spark (GB10, SM12.1), where hangs and low decode throughput persist (#56892, #50576).  
- **ROCm (gfx950 / MI355X)**: Performance optimization tracking for Qwen3.8-Flash-Next (#59575)  
- **aarch64 (ARM64)**: Still lacks sm_121 support on NVIDIA DGX Spark / Acer GN100 (#36821) — not yet resolved.

---

### **4. Performance & Optimization**  
- **Batch Invariant Determinism**: High-priority work continues; multiple PRs (e.g., #65127, #64493) validate seed ranges and tokenizer behavior to ensure reproducibility under `torch.compile` and CUDA graphs (#27433, #39096).  
- **KV Cache & Offloading**:  
  - Async lookup result publication per request group improves scheduling efficiency (#57474).  
  - Admission reservation caching reduces overhead in async KV loads (#65285).  
- **Speculative Decoding**: Zero-bubble draft token handoff for structured outputs optimized (#64531), improving pipeline utilization.  
- **Prefix Caching**: Fixes for truncation issues (e.g., #47349) and inconsistent completions (#5543) remain open.  
- **Rust Frontend**: Performance parity roadmap ongoing (#44280); initial benchmarks show promise but gaps remain.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR? | Link |
|------|----------|--------|---------|------|
| `DeepSeek-V4-Flash` hangs after ~6 requests with `cudagraph_mode=FULL_AND_PIECEWISE + chunked prefill` (SM12.x) | Critical | Open | ❌ | [#40969](https://github.com/vllm-project/vllm/issues/40969) |
| Non-deterministic greedy decoding on `Qwen3.8-Flash-Next` near `indexer_budget` | High | Open | ❌ | [#54521](https://github.com/vllm-project/vllm/issues/54521) |
| `DeepSeek-V4-Flash` hangs silently on RTX 5080 (SM120) | Critical | Open | ❌ | [#46625](https://github.com/vllm-project/vllm/issues/46625) |
| MTP speculative decoding crash with long sequences (FP8-Qwen3.6-27B-FP8) | High | Open | ❌ | [#40756](https://github.com/vllm-project/vllm/issues/40756) |
| `vLLM v1` prefix caching causes first request to differ at `temperature=0` | Medium | Open | ❌ | [#40896](https://github.com/vllm-project/vllm/issues/40896) |

> ⚠️ Multiple regressions tied to **Blackwell (SM12x)** and **batch invariance** remain unresolved. These impact production deployments using FP8, MoE, or speculative decoding.

---

### **6. What This Means for Application Developers**  
- **Use caution with v0.33.0rc3 in production** — while new models like Reflection Beam-1.0 and Recursive Flint unlock massive MoE scaling, Blackwell (SM12x) support is still unstable. Avoid `cudagraph_mode=FULL_AND_PIECEWISE` and `chunked_prefill` on GB10/RTX 5080 until #56892 and #40969 are resolved.  
- **Enable `--enable-prefix-caching` only if you can verify determinism** — known issues with non-deterministic output and truncation persist (#54521, #47349).  
- **Leverage the Rust frontend (`VLLM_USE_RUST_FRONTEND=1`) for low-latency services**, but expect incomplete features (#44280).  
- **Monitor for `temperature=0` drift** when using MoE models (especially Qwen3.8-Flash-Next) — consider disabling `persistent_topk` or using `--disable-async-scheduling` as workaround.  
- **For multi-model serving or cold start scenarios**, track #34303 (CUDA checkpoint/restore) — it promises near-zero cold starts on v1 engine.  

> ✅ **Recommendation**: Pin to `v0.32.0` for stable inference until v0.33.0 final release clears key SM12x and batch-invariance bugs. Use CI/CD pipelines to test against nightly builds for early detection of regressions.

---  
*Digest generated from GitHub data: [vLLM Project](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-03**

---

### **1. Today's Highlights**  
The SGLang ecosystem saw a major release with **v0.5.23**, driven by 1,007 PRs from 257 contributors—highlighting intense community engagement. Key developments include the addition of **Muse Spark Open** and **Beam-1.0** as new supported LLM/VLM models, alongside significant progress in NPU (Ascend) and AMD ROCm support. Critical stability work continues on flaky CI infrastructure and speculative decoding reliability.

---

### **2. Releases & Breaking Changes**  
- **v0.5.23**: Released today with extensive improvements across inference engine, model serving, and hardware backends. No breaking API changes reported; migration guidance available via [release notes](https://github.com/sgl-project/sglang/releases/tag/v0.5.23).  
- **RFC: Migration to shared `api.v1.Generate` backend** ([#45816](https://github.com/sgl-project/sglang/issues/45816)) — Unifying generation logic under a single schema is underway; impacts internal routing and processor components but not user-facing APIs yet.

---

### **3. New Model & Hardware Support**  
- **New Models Added**:  
  - **Muse Spark Open** (LLM/VLM): Full support added with dedicated cookbook [here](https://docs.sglang.io/cookbook/autoregressive/Meta/MuseSparkOpen).  
  - **Beam-1.0** (LLM/VLM): Supported via [cookbook](https://docs.sglang.io/cookbook/autoregressive/Beam/Beam1_0).  
- **Hardware & Backend Expansions**:  
  - **NPU (Ascend)**: DSV4.1 support for Ascend A5 (`#47660`), HiCache L3 support for DeepSeek-V4 (`#39784`), and hybrid DSA KV pool integration (`#43533`).  
  - **AMD ROCm**: Performance optimization tracking for Qwen3.8-Flash-Next on gfx950 / MI355X (`#44304`).  
  - **Intel XPU**: RFC proposal for paged experts with Lookahead Expert Scheduling (LES) and SSD-tier offload (`#46520`).  

---

### **4. Performance & Optimization**  
- **Kernel-Level Improvements**:  
  - SM12x FA4 decode now uses live KV lengths during graph replay to avoid idle splits (`#47815`).  
  - Fused-A GEMM kernel relaxes K alignment from 1024 to 256 for better compatibility with MLA models (`#47898`).  
  - Inverse RoPE fused into `wo_a` FP8 quantization path for DeepSeek-V4, reducing memory bandwidth usage (`#43786`).  
- **Memory & Offload Optimizations**:  
  - HiSparse KV offload for Ascend DSA serving with LIM selection and CopySFA attention (`#47383`).  
  - FLUX.2 Klein reference K/V cached across denoising steps (reduces recomputation overhead) (`#47953`).  
- **Diffusion Pipeline Enhancements**:  
  - DM05 action inference added (`#48056`), prefix K/V buffer ownership optimized in Cosmos-Dreams rollout (`#46613`).

---

### **5. Stability & Regressions**  
- **Critical Flakiness**:  
  - **[CI] Flaky tests and infrastructure failures** (`#42752`) — 165 comments; recurring failures in `PR Test Base/Extra` runs block merges. High priority for maintainers.  
- **Crashes & Correctness Bugs**:  
  - **Mamba cache allocation failure kills engine** when all states are locked (`#43204`); fix pending.  
  - **GLM-5.3-Flash NEXTN crashes on first request** due to out-of-bounds embedding gather (`#37548`); resolved in patch.  
  - **Qwen3.8-Flash-Next FP8 KV cache crash** with QSA sparse attention (dtype mismatch) (`#36545`); fixed.  
  - **Greedy decoding non-reproducible across restarts** with DFLASH + FlashInfer autotune (`#39597`).  
- **Minor but Notable Issues**:  
  - Hybrid Mamba + EAGLE: Memory error misreports `--mem-fraction-static` when Mamba state fills memory (`#47830`).  
  - `<image>` literal in input aborts requests due to token/data mismatch (`#43979`).

---

### **6. What This Means for Application Developers**  
- **Build robust agents**: Expect improved speculative decoding stability with adaptive strategies in development (`#23705`) and better MoE scalability via Mooncake Elastic EP (`#47859`).  
- **Leverage multi-modal models**: Muse Spark Open and Beam-1.0 offer new options for vision-language tasks—check their cookbooks for setup guidance.  
- **Optimize for cost and latency**: Use `fused-a-gemm`, `live-KV-length` replay, and `K/V caching` features to reduce inference costs on Hopper (SM120) and B200 systems.  
- **Avoid pitfalls**: Watch for known issues around FP8/KV cache dtype mismatches, greedy reproducibility, and image token handling until fixes land.  
- **Prepare for future migrations**: The `api.v1.Generate` unification will streamline server architecture—monitor [#45816](https://github.com/sgl-project/sglang/issues/45816) for updates.

> 🔗 *Full context: [GitHub repo](https://github.com/sgl-project/sglang)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp Digest – 2026-11-03**

---

### **1. Today's Highlights**  
The latest development in `llama.cpp` centers on **multi-sequence flash attention optimization** and **MoE inference improvements**, with key PRs enhancing unified KV cache efficiency across CUDA, Vulkan, and SYCL backends. A major focus is on **MoE expert caching** and **GPU-resident LRU policies**, addressing scalability bottlenecks for large mixture-of-experts models. Additionally, new support for **Clover MoE** and **Pocket TTS without voice cloning** expands the ecosystem’s flexibility.

---

### **2. Releases & Breaking Changes**  
No new tagged releases were published today. However, several **critical fixes and behavioral changes** were merged:  
- `b12063`: Introduced `dflash` device-based hidden feature copying (#31506) — improves multi-device offloading performance.  
- `b12059`: Enhanced `flash_attn_ext_rows` for more efficient unified KV handling in multi-sequence inference (#29510).  
- `b12058`: Fixed crash in `imatrix` when using `--nextn` (#31522).  

> 🔗 [GitHub Commit b12063](https://github.com/ggml-org/llama.cpp/commit/b12063), [PR #29510](https://github.com/ggml-org/llama.cpp/pull/29510)

---

### **3. New Model & Hardware Support**  
- ✅ **Clover MoE (AR-only)**: Added full GGUF conversion, model loading, and causal hybrid attention support (#31306).  
- ✅ **Pocket TTS without voice cloning**: Now supports predefined voice states via backbone KV state (#31429).  
- ✅ **DFlash (Laguna S 2.1)**: Feature request opened for native DFlash support (#26669) — expected to be prioritized soon.  
- ✅ **Apple Silicon (arm64)**: Updated macOS/iOS builds available via GitHub Releases for recent commits.  
- ✅ **Intel Arc B70/B60 (SYCL)**: Continued optimizations for SYCL backend on Intel GPUs; see issue #31510 (performance regression reported).

> 🔗 [PR #31306](https://github.com/ggml-org/llama.cpp/pull/31306), [Issue #26669](https://github.com/ggml-org/llama.cpp/issues/26669)

---

### **4. Performance & Optimization**  
- **Multi-sequence flash attention**: The `flash_attn_ext_rows` extension reduces overhead in unified KV cache by avoiding per-sequence filtering (#29510). Benchmarks show up to **~30% faster prefill latency** on large batch sizes.  
- **Vulkan + Intel FA kernel**: Optimized prefill performance on Intel platforms via dedicated shaders (#29357).  
- **SYCL DPAS flash attention**: Added support for quantized KV cache (q4_0/q8_0) on Xe2 platforms (B580/B70), enabling faster decode and small batches (#31125).  
- **MoE expert cache (GPU-resident LRU)**: Ongoing enhancement (#29949) aims to run cache decisions directly on GPU compute graph — critical for scalable MoE inference.  
- **CUDA graph retention fix**: Prevents premature eviction of CUDA graphs under high concurrency, reducing recompilation cost (#30662).

> 🔗 [PR #29510](https://github.com/ggml-org/llama.cpp/pull/29510), [PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125), [PR #30662](https://github.com/ggml-org/llama.cpp/pull/30662)

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|--------|------|------------|-----------|
| ⚠️ High | [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) | Multi-GPU (`--split-mode layer`) + `--cpu-moe` on Intel Arc B60: **100–1000× slower prefill (165s for 5 tokens)** | Open |
| ⚠️ High | [#27845](https://github.com/ggml-org/llama.cpp/issues/27845) | Memory leak with `-sm tensor` on dual Intel B70 SYCL backend | Closed (unconfirmed) |
| ⚠️ Medium | [#30697](https://github.com/ggml-org/llama.cpp/issues/30697) | `--moe-cache-mib` crashes on Vulkan (RX 6900 XT) | Open |
| ⚠️ Medium | [#31538](https://github.com/ggml-org/llama.cpp/issues/31538) | Segmentation fault on Windows MSVC build | Open |
| ⚠️ Low | [#27910](https://github.com/ggml-org/llama.cpp/issues/27910) | RTX 5090 display loss during Qwen3.8-27B Q6_K inference | Closed |

> 🔗 [Issue #31510](https://github.com/ggml-org/llama.cpp/issues/31510), [Issue #27845](https://github.com/ggml-org/llama.cpp/issues/27845)

---

### **6. What This Means for Application Developers**  
- **Deploying MoE models at scale?** Monitor the ongoing work on **GPU-resident MoE LRU caching** (#29949) — this will enable efficient expert management without CPU thrashing.  
- **Using multi-GPU setups?** Avoid `--split-mode layer` + `--cpu-moe` on Intel Arc GPUs until #31510 is resolved; consider `--split-mode row` or `--gpu-layers` tuning.  
- **Building agent systems?** Leverage new **Pocket TTS voice support without cloning** (#31429) for lightweight voice synthesis pipelines.  
- **Optimizing inference latency?** Enable `flash_attn_ext_rows` where supported (CUDA/Vulkan/SYCL) — it significantly improves throughput in batched, multi-sequence scenarios.  
- **Handling tool calls?** Update to latest code to benefit from **namespaced tools in responses API** (#31568) and **JSON argument order tolerance** (#30193).

> 🔗 [PR #31568](https://github.com/ggml-org/llama.cpp/pull/31568), [PR #30193](https://github.com/ggml-org/llama.cpp/pull/30193)

---  
*Digest generated: 2026-11-03 | Source: [github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-03**

---

### **1. Today's Highlights**  
Ollama v0.40.14-rc0 addresses critical stability fixes, including a deadlock workaround in CI and improved OpenAI-compatible response handling. New support for Rockchip NPU backend and enhanced model loading diagnostics signal growing hardware diversity. Multiple issues highlight ongoing challenges with cloud access, multimodal vision inference on GPU, and quantization gaps—particularly for Qwen3.5 0.8B.

---

### **2. Releases & Breaking Changes**  
- **v0.40.14-rc0**:  
  - Fixed buildx deadlock in CI via pinning (PR #19373)[github.com/ollama/ollama/pull/19373]  
  - Preserved readable `refusal` content in OpenAI-compatible responses (PR #19192)[github.com/ollama/ollama/pull/19192]  
  - Improved sorting of long top-k rows in chunks for mlx backend (PR #19373)[github.com/ollama/ollama/pull/19373]  

> ⚠️ No breaking changes in API or config; this is a release candidate focused on stability and correctness.

---

### **3. New Model & Hardware Support**  
- **Rockchip NPU (rknpu)**: Native backend support added for RK3588/RK3576 SoCs on Linux (PR #19480)[github.com/ollama/ollama/pull/19480] — enables offloading of prefill and MoE operations to NPU while keeping single-token decode on CPU.  
- **MiMo V2.6**: Experimental support added via `dhiltgen/mimo-v2.6` (PR #19400)[github.com/ollama/ollama/pull/19400] — available for testing with `ollama pull`.  
- **Qwen3.8-27B hybrid-attention GGUFs**: Now under investigation (Issue #19459)[github.com/ollama/ollama/issues/19459] — currently fails due to missing `attn_qkv/attn_gate` projections in layer 64.

---

### **4. Performance & Optimization**  
- **VRAM Overprediction Fix**: PR #19463[github.com/ollama/ollama/issues/19463] identifies that `PredictServerVRAM` overestimates KV cache usage by **4–8x** for hybrid models (e.g., Qwen3.5/3.8), ignoring `OLLAMA_KV_CACHE_TYPE`. This causes unnecessary eviction of models with `keep_alive=-1`, impacting memory efficiency on high-end systems.  
- **MLX CUDA Library Loading**: PR #19433[github.com/ollama/ollama/pull/19433] resolves dependency resolution issues by bundling CUDA libraries in-process, improving reliability on Linux systems with complex GPU environments.  
- **Unix Domain Sockets**: Feature request #19464[github.com/ollama/ollama/issues/19464] seeks IPC support for reduced latency and isolation on Unix-like systems.

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|------|-------------|------------|
| 🔴 High | Cloud Access Failure (#19461)[github.com/ollama/ollama/issues/19461] | Paid users unable to access cloud models despite confirmed payment; no response from support channels. | ❌ Open — critical UX/monetization blocker |
| 🔴 High | Vision Inference on NVIDIA GPU (#19482, #19483)[github.com/ollama/ollama/issues/19482,19483] | `qwen2.5vl:3b` crashes with "token repeat limit reached" when vision encoder runs on GPU; works on CPU. | ❌ Open — regression in multimodal inference |
| 🟡 Medium | Qwen3.5 0.8B Q4_K_M Missing (#19436)[github.com/ollama/ollama/issues/19436] | Only Qwen3.5 0.8B lacks `q4_K_M` quantization, limiting CPU performance gains. | ✅ Requested — not yet implemented |
| 🟡 Medium | Token Decoding Bug (#18785)[github.com/ollama/ollama/issues/18785] | `"python"` without leading space decodes as empty string → silent text loss in output. | ✅ Closed — fix merged |

---

### **6. What This Means for Application Developers**  
- **Cloud Users**: If you’re subscribed to Ollama Cloud, report payment/access issues directly via [hello@ollama.com](mailto:hello@ollama.com) — community support channels are unreliable.  
- **Multimodal Apps**: Avoid using `qwen2.5vl:3b` with GPU-based vision encoders until #19482 is resolved — fall back to CPU-only inference or use alternative models.  
- **Performance Tuning**: For CPU-heavy workloads, prefer `q4_K_M` quantizations (available for all Qwen3.5 except 0.8B); consider importing custom GGUFs if needed.  
- **Model Deployment**: Use `ollama ls --cloud` (PR #19153)[github.com/ollama/ollama/pull/19153] to discover cloud models without pulling them first — improves workflow efficiency.  
- **Local-First Agents**: Consider integrating tools like **DGC** (PR #19469)[github.com/ollama/ollama/pull/19469], **Ultimate Assistant**, or **Herald** (PR #19477)[github.com/ollama/ollama/pull/19477] for local-first agent orchestration.

---  
*Digest generated from GitHub data: github.com/ollama/ollama — 2026-11-03*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

---

### **1. Today's Highlights**  
The LiteLLM project is advancing its core infrastructure with a major push toward a **Rust-based migration**, aiming to achieve sub-1ms overheads and significantly improve performance and memory safety. This initiative, detailed in [Issue #31263](https://github.com/BerriAI/litellm/issues/31263), marks a pivotal shift in the project’s long-term architecture. Meanwhile, several critical fixes have landed or are under review, including resolution of streaming data loss in `/v1/messages`, improved cost tracking for dynamic key updates, and enhanced support for Baidu Qianfan as an OpenAI-compatible provider.

---

### **2. Releases & Breaking Changes**  
*No new releases were published in the last 24 hours.*  

However, significant changes are underway:  
- **Rust Migration (In Progress)**: A foundational rewrite targeting ultra-low-latency inference (sub-1ms overhead) via Rust. See [Issue #31263](https://github.com/BerriAI/litellm/issues/31263) and associated blog: [LiteLLM Rust Launch](https://docs.litellm.ai/blog/litellm-rust-launch). Early beta sign-up available [here](https://docs.google.com/forms/d/e/1FAIpQLSecWdOjkzjEson2UiZpD...).  
- **License Clarity**: A PR ([#34241](https://github.com/BerriAI/litellm/issues/34241)) seeks to clarify MIT vs. Enterprise feature boundaries to improve compliance and transparency.

---

### **3. New Model & Hardware Support**  
- ✅ **Baidu Qianfan (ModelBuilder)**: Added as a fully supported OpenAI-compatible provider via [PR #49777](https://github.com/BerriAI/litellm/pull/49777). Now supports custom deployments, proper `api_key` handling, and correct model routing.  
- 🚧 **LM Studio Context Window Syncing**: [PR #49597](https://github.com/BerriAI/litellm/pull/49597) adds native detection of actual context limits from LM Studio’s API, improving accuracy for locally hosted models with custom instance IDs.  
- 🔧 **Bedrock Converse Models**: Updated cost map entries to reflect true 262,144-token input windows for Kimi K2.5, Qwen3 Coder Next, Qwen3 Next 80B, and Nemotron Nano 3 30B ([PR #49768](https://github.com/BerriAI/litellm/pull/49768)).

---

### **4. Performance & Optimization**  
- ⚡ **Rust Migration (Core Initiative)**: Targeted at achieving **sub-1ms overhead** across the proxy layer. Expected benefits include reduced latency, lower memory footprint, and improved scalability under high concurrency.  
- 💡 **Streaming Efficiency**: Fixes ensure no data loss during content-block transitions (e.g., `thinking` → `text`) on Bedrock and Claude Code models ([Issue #25214](https://github.com/BerriAI/litellm/issues/25214), [Issue #30765](https://github.com/BerriAI/litellm/issues/30765)).  
- 📈 **Cost Accuracy**: PRs like [#49625](https://github.com/BerriAI/litellm/pull/49625) now correctly apply custom deployment pricing even when provider mismatch occurs, preventing revenue leakage.

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|--------|------|-------------|------------|
| 🔴 High | [Issue #49675](https://github.com/BerriAI/litellm/issues/49675) | v3 rate limiter incorrectly resets TPM window after upstream 429s, leading to false rate limit violations | *Open* – impacts throttling accuracy |
| 🔴 High | [Issue #38907](https://github.com/BerriAI/litellm/issues/38907) | Proxy strips `delta.reasoning_items` from streaming responses on fast-serialization path | *Closed* – fixed in [PR #49402](https://github.com/BerriAI/litellm/pull/49402) |
| 🟡 Medium | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) | `/key/update` during in-flight request drops cost from budget window total | *Closed* – fix merged |
| 🟡 Medium | [Issue #49479](https://github.com/BerriAI/litellm/issues/49479) | `pass_through_endpoints` strips `id` from JSON-RPC 2.0 requests, breaking spec compliance | *Open* – requires fix |
| 🟡 Medium | [Issue #49617](https://github.com/BerriAI/litellm/issues/49617) | Langfuse OTel omits SystemOne answers and token totals | *Open* – affects observability |

---

### **6. What This Means for Application Developers**  
- **Expect better performance and reliability** with upcoming Rust-based proxy—ideal for low-latency agent systems and high-throughput APIs.  
- **Ensure your code handles streaming correctly**—recent fixes address missing first chunks after reasoning blocks, which could break agent state continuity.  
- **Leverage new providers like Baidu Qianfan** directly via OpenAI-compatible endpoints without manual workarounds.  
- **Use `litellm config-validate` and `token-count` CLI tools** (in progress via [PR #34772](https://github.com/BerriAI/litellm/pull/34772)) for pre-deployment validation and offline cost estimation.  
- **Avoid configuration pitfalls**: Be mindful that `budget_limits` updates mid-request may not reflect immediately in window spend; use stable key configurations where possible.

> ✅ **Pro Tip**: Monitor [GitHub Issues](https://github.com/BerriAI/litellm/issues) and [PRs](https://github.com/BerriAI/litellm/pulls) closely—this ecosystem is rapidly evolving, especially around streaming correctness and enterprise-grade observability.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-03**

---

### **1. Today's Highlights**  
The Unsloth project continues its rapid evolution with key enhancements to model serving and user experience, particularly around multimodal support and UI/UX polish. Notably, the team has merged PRs enabling LFM3-Audio-2B chat via the multimodal loader and improved TTS audio model sizing for GPU memory constraints. Critical stability fixes were also introduced, including a fix for silent cancellation of running chats during model loading.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API/config changes observed.

---

### **3. New Model & Hardware Support**  
- ✅ **LFM3-Audio-2B**: Added support for text-to-speech inference via `studio/frontend/src/multimodal/loader.ts` (PR #16243). This model accepts audio input and responds in text, expanding Unsloth’s multimodal capabilities.
- 🟡 **Vulkan on AMD RX 5700 XT (Windows 10)**: Users report inability to use Vulkan backend despite driver compatibility (Issue #16395). No fix yet; likely a backend-specific kernel or driver binding issue.
- 🔧 **FP8 Quantization**: PR #16156 addresses FP8 weight conversion mismatches when saving Qwen3 MoE models, ensuring proper 16-bit compatibility after dequantization.

> 🔗 [PR #16243](https://github.com/unslothai/unsloth/pull/16243) | [Issue #16395](https://github.com/unslothai/unsloth/issues/16395)

---

### **4. Performance & Optimization**  
- ⚙️ **TTS Model Sizing**: PR #16514 introduces dynamic sizing of audio.cpp TTS models based on measured peak memory usage, allowing smaller models to run on GPUs even when previously blocked by "No single GPU has enough free memory" errors.
- 💡 **Memory Efficiency**: PR #16476 refactors startup progress tests to be independent of host scheduling, improving reliability in CI/CD pipelines (especially Python 3.13).
- 📈 **KV Cache Growth**: PR #16191 improves eager KV cache growth logic by considering both graph-stored and fresh cache states, reducing unnecessary over-allocation (e.g., from 1,536 → 3,072 rows instead of 2,048).

> 🔗 [PR #16514](https://github.com/unslothai/unsloth/pull/16514) | [PR #16476](https://github.com/unslothai/unsloth/pull/16476) | [PR #16191](https://github.com/unslothai/unsloth/pull/16191)

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|------|-------------|------------|
| 🔴 High | **#16479** | Loading a model silently cancels active chats; unloading refuses without `force_cancel_active` | ❌ Open |
| 🔴 High | **#16436** | Loading a second model during generation fails with misleading "memory not released" error | ❌ Open |
| 🟡 Medium | **#16484** | Auto-unload on idle broken (not triggering) | ❌ Open |
| 🟡 Medium | **#14487** | Persistent memory leak on Windows (Drive C loses 200–400 MB per session) | ❌ Open |
| 🟡 Medium | **#15932** | Post-update models fail to load (e.g., DeepSeek V4.1 Flash GGUF) | ❌ Open |

> 🔗 [Issue #16479](https://github.com/unslothai/unsloth/issues/16479) | [Issue #16436](https://github.com/unslothai/unsloth/issues/16436) | [Issue #16484](https://github.com/unslothai/unsloth/issues/16484)

---

### **6. What This Means for Application Developers**  
- **Build robust model workflows**: Be cautious when switching models during active generations—active sessions may be canceled silently (Issue #16479), so implement client-side polling or retry logic with `force_cancel_active`.
- **Leverage new multimodal APIs**: The LFM3-Audio-2B integration enables voice-input AI agents; use the `multimodal/loader.ts` pattern for future audio-enabled models.
- **Optimize for memory**: Use `idle auto-unload` settings carefully—current behavior is inconsistent (Issue #16484). Consider using `--auto-unload-timeout` CLI flags if available.
- **Watch for quantization edge cases**: When saving FP8 models, ensure your pipeline handles `.activation_scale` metadata correctly (PR #16156); otherwise, downstream matching may fail.
- **Prepare for CPU-only fallbacks**: Audio/TTS models now better detect usable GPU memory (PR #16514), but developers should still handle CPU fallback gracefully.

> 🔗 [PR #16514](https://github.com/unslothai/unsloth/pull/16514) | [Issue #16479](https://github.com/unslothai/unsloth/issues/16479)

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*