# AI Infrastructure Digest 2026-11-02

> Generated: 2026-11-02 01:47 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-02**

---

### **1. Ecosystem Overview**  
The AI inference and serving ecosystem is entering a pivotal phase of hardware specialization, with NVIDIA’s Blackwell (SM120) and AMD’s MI350X emerging as the new performance frontiers. Projects are rapidly adapting to next-gen architectures through targeted kernel optimizations, disaggregated inference support, and cross-platform runtime extensions. A clear divergence is forming between *high-performance engines* (vLLM, SGLang), *lightweight local runtimes* (llama.cpp), and *integrated application gateways* (Ollama, LiteLLM), each catering to distinct deployment needs. Meanwhile, Rust-based infrastructure components—particularly in vLLM and LiteLLM—are gaining momentum as foundational layers for low-latency, scalable systems.

---

### **2. Activity Comparison**

| Project       | Issues Opened (Last 24h) | PRs Merged (Last 24h) | Releases | Status |
|---------------|----------------------------|--------------------------|----------|--------|
| **vLLM**      | 8                          | 12                       | None     | Stable, focused on SM120/ROCm tuning |
| **SGLang**    | 7                          | 6                        | None     | Active bug fixes; stability under pressure |
| **llama.cpp** | 9                          | 7                        | 6        | Frequent minor releases; backend-heavy |
| **Ollama**    | 5                          | 4                        | None     | Feature-focused; security & parsing updates |
| **LiteLLM**   | 6                          | 3                        | None     | High-severity stability issues in core routing |
| **Unsloth**   | 6                          | 6                        | None     | UX/stability fixes; backend expansion |

> ✅ *Note: All projects report no breaking changes or new releases today, indicating stabilization phases post-early adoption.*

---

### **3. Model Support Race**

| New Model / Architecture         | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1-Flash**          | ✅ (SM120 tuned) | ⚠️ (dSV4 Prefill lag) | ❌ | ❌ | ❌ | ❌ |
| **MiniMax-M3.1-Flash**           | ✅ (ROCm optimized) | ✅ (MI350X support) | ❌ | ❌ | ❌ | ❌ |
| **GLM-5.3-Flash**                | ✅ (ROCm kpool opt) | ⚠️ (SM120 capture crash) | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.5 / Qwen3.8 GGUF**       | ❌ | ✅ (Prefill fusion) | ❌ | ⚠️ (Load failure) | ❌ | ❌ |
| **Apple Silicon (MLX)**          | ❌ | ⚠️ (RFC in progress) | ❌ | ❌ | ❌ | ✅ (MLX draft support) |
| **Intel XPU (MoE + SSD offload)**| ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (RFC proposed) |
| **AMD MI350X / MI355X**          | ❌ | ✅ (Full path enabled) | ❌ | ❌ | ❌ | ❌ |

> 🏆 **Leader**: **SGLang** leads in *hardware diversity*, with full support for MI350X and experimental Apple Silicon integration.  
> 🥈 **Runner-up**: **Unsloth** excels in *multi-backend accessibility*, particularly on Apple Silicon and Vulkan.  
> 🥉 **Frontier Focus**: **vLLM** dominates *Blackwell-specific optimization*, especially for Flash models.

---

### **4. Performance Frontier**

| Optimization Area               | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **KV Cache Offloading**          | ✅ (P/D, MoRI-IO) | ⚠️ (OOM under load) | ❌ | ❌ | ❌ | ❌ |
| **Batching & Throughput**        | ✅ (fused all-reduce config) | ⚠️ (Prefill gap vs vLLM) | ✅ (dflash) | ❌ | ✅ (Rust router) | ❌ |
| **Quantization (MXFP8, FP8)**    | ✅ (cuBLASLt, NVFP4 bias) | ⚠️ (FA4 regression) | ✅ (iq3_s, tiled GEMM) | ❌ | ❌ | ✅ (FP8 LoRA) |
| **Distributed Serving (P/D, Wide-EP)** | ✅ (CI enabled) | ⚠️ (dLLM roadmap) | ❌ | ❌ | ❌ | ❌ |
| **Kernel-Level Tuning (SM120)**  | ✅ (High priority) | ⚠️ (CUDA coredump risk) | ⚠️ (Hangs on Blackwell) | ❌ | ❌ | ❌ |

> 🔥 **Top Focus Areas**:  
> - **vLLM** dominates kernel-level tuning for **Blackwell-era GPUs**.  
> - **SGLang** prioritizes **long-running stability** and **distributed scalability**.  
> - **llama.cpp** leads in **low-end/NPU portability** via OpenCL/Vulkan.  
> - **LiteLLM** pushes **gateway performance** with Rust core adoption.

---

### **5. Layer Positioning**

| Project       | Primary Layer             | Key Differentiator |
|---------------|----------------------------|--------------------|
| **vLLM**      | **Inference Engine**       | Highest throughput on SM120; deep kernel customization; P/D & KV offload focus |
| **SGLang**    | **Inference Engine + Gateway** | Strong distributed serving (dLLM), multi-node HTTP routing, MoE-aware scheduler |
| **llama.cpp** | **Local Runtime / Edge**   | Cross-platform portability (Vulkan, SYCL, MLX); lightweight, embedded use cases |
| **Ollama**    | **Application Gateway / CLI** | Developer-first UX; structured output; tool calling; prompt caching |
| **LiteLLM**   | **API Gateway / Proxy**    | Multi-provider routing; cost tracking; enterprise-grade observability |
| **Unsloth**   | **Studio + Fine-Tuning UI** | Unified CLI/Studio; rich media input; MoE/CPU-GPU layering; training tools |

> 🧩 **Strategic Clusters**:  
> - **Engine Layer**: vLLM, SGLang  
> - **Runtime Layer**: llama.cpp  
> - **Gateway/Proxy Layer**: Ollama, LiteLLM  
> - **Training/UX Layer**: Unsloth

---

### **6. Trend Signals**

#### 🔍 **Key Industry Trends Extracted**:
1. **Hardware Specialization is Now Mandatory**  
   Blackwell (SM120) and MI350X are no longer future-proof—they’re live battlegrounds. vLLM and SGLang are leading in architecture-specific kernel tuning, signaling that *one-size-fits-all* inference is dead.

2. **Rust is Becoming the Foundation Layer**  
   vLLM’s Rust frontend and LiteLLM’s Rust gateway core indicate a shift toward **GIL-free, high-throughput routing** at scale—critical for agent pipelines and real-time applications.

3. **Disaggregation & Distributed Serving Are Maturing**  
   vLLM’s P/D + MoRI-IO work and SGLang’s dLLM roadmap show that *distributed inference* is moving from research to production readiness—especially for long-context agents.

4. **Security & Resource Isolation Are Rising in Priority**  
   Ollama’s per-model prompt cache budgets and LiteLLM’s `pass_through_endpoints` id stripping reflect growing awareness of *multi-tenant risks* and *request integrity* in API gateways.

5. **Edge and Heterogeneous Backends Are Expanding**  
   llama.cpp’s OpenCL/iq3_s support and Unsloth’s MLX/Vulkan enhancements show strong demand for **on-device inference**, especially on Apple Silicon and mobile NPUs.

#### 📌 **What Application Developers Should Watch**:
- **Avoid `--enforce-eager` on SM120 GPUs** until #56892 is fixed (vLLM).
- **Use Triton over FA4 on Blackwell** if running DSV4.1 or GLM-5.3-Flash (SGLang).
- **Enable per-model prompt caches** in Ollama to isolate resource usage.
- **Monitor LiteLLM’s `cost` reporting**—provider values may be wildly inaccurate.
- **Plan for Rust-powered gateways** (LiteLLM, vLLM) early—latency and scalability will matter at scale.
- **Test model loading behavior** across platforms (Unsloth, llama.cpp) due to persistent crashes on GPU/NPU edge cases.

---

> ✅ **Final Takeaway**: The ecosystem is no longer about *which engine is fastest*—it’s about **layered resilience, hardware alignment, and operational maturity**. Top-tier developers will build on **vLLM/SGLang for performance**, **LiteLLM/Ollama for developer experience**, and **Unsloth/llama.cpp for edge flexibility**—all while leveraging Rust for critical infrastructure components.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-02

---

### **1. Today's Highlights**  
The vLLM project continues to accelerate its support for **Blackwell-era hardware (SM120)**, with critical performance fixes and new optimizations targeting DeepSeek-V4.1-Flash and MiniMax-M3.1 on both NVIDIA and ROCm platforms. A major focus is on stabilizing **disaggregated inference (P/D)** and **KV cache offloading**, particularly through the NixlPushMode and MooncakeConnector integrations. Meanwhile, the Rust frontend reaches feature parity milestones, signaling growing maturity of the alternative API surface.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were introduced. The `VLLM_USE_RUST_FRONTEND=1` experimental flag remains active but not yet production-ready.

> 🔗 [GitHub Issues: No new releases](https://github.com/vllm-project/vllm/releases)

---

### **3. New Model & Hardware Support**  
- **DeepSeek-V4.1-Flash** now has targeted SM120 (RTX PRO 6000 Blackwell) performance tuning and field validation via #56700 and #56892.  
- **MiniMax-M3.1-Flash** gains ROCm-specific decode optimizations (#65032), including parallel top-k selection and MXFP8 GEMM tuning (#62509).  
- **GLM-5.3-Flash** sees ROCm kernel improvements via reduced host-device syncs (#63072) and kpool indexing optimization (#63910).  
- **ROCm support** extends to distributed inference (P/D + Wide-EP) CI enablement (#48117), with ongoing efforts to stabilize MoRI-IO PD disagg (#59116).

> 🔗 [PR #65032: ROCm Parallel Decode Top-K](https://github.com/vllm-project/vllm/pull/65032)  
> 🔗 [PR #63910: GLM-5.3-Flash ROCm KPool Optimization](https://github.com/vllm-project/vllm/pull/63910)

---

### **4. Performance & Optimization**  
- **DeepSeek-V4.1**: Fused all-reduce + mHC token cap made configurable (`--kernel-config '{"mhc_overlap_max_tokens": N}'`) to improve throughput under high concurrency (#63837).  
- **MXFP8 GEMMs** for DeepSeek-V4.1 now routed to `cuBLASLt` for large inputs (>8192 rows), yielding measurable speedups vs FlashInfer CuTe-DSL (#64815).  
- **NVFP4 CUTLASS epilogue** now includes bias computation inline, eliminating a separate kernel and reducing latency on large prefills (#61821).  
- **KV Offload**: Improved state publication logic ensures successful stores are reflected in lookup state, preventing stale overwrites (#64444).  
- **Scheduler Metrics**: Added per-step copy-on-write (CoW) KV block counter for deeper debugging of memory churn (#65038).

> 🔗 [PR #64815: cuBLASLt for Large MXFP8 GEMMs](https://github.com/vllm-project/vllm/pull/64815)  
> 🔗 [PR #61821: NVFP4 Bias in Epilogue](https://github.com/vllm-project/vllm/pull/61821)

---

### **5. Stability & Regressions**  
- **Critical Crash**: `Quark` speculative decoding fails silently when `exclude` entries (e.g., `mtp.fc.weight`) are mismatched due to incorrect weight name parsing (#65034). *No fix PR yet.*  
- **CUDA Graph Capture Failure**: On H200-NVL TP=8 with FlashInfer, fused allreduce crashes during capture; workaround ~34x slower than disabling fusion (#48071).  
- **SM120 Kernel Regression**: Extremely low decode throughput on 8x RTX PRO 6000 Blackwell with `--enforce-eager`; CUDA graphs unusable (#56892). *High-priority fix needed.*  
- **Memory Corruption**: `DiffusionGemma-26B-A4B` crashes under concurrent load due to tensor shape mismatch (`257 != 256`) in `post_norm` layer (#58035).  
- **MoE Layer Bug**: `GLM-5.x` under pipeline parallelism reuses stale top-k indices at stage start, causing incorrect outputs (#63170).

> 🔗 [Issue #65034: Quark Exclude Mismatch](https://github.com/vllm-project/vllm/issues/65034)  
> 🔗 [Issue #56892: SM120 Decode Throughput Crash](https://github.com/vllm-project/vllm/issues/56892)

---

### **6. What This Means for Application Developers**  
- **Use caution with `--enforce-eager` on SM120 GPUs**—expect degraded performance; consider disabling CUDA graphs until #56892 is resolved.  
- **For agents and long-running RLHF flows**, monitor `free_block_queue` allocation order and use `reset_prefix_cache()` carefully—consider #40244’s proposed API extension.  
- **Integrate external routers** using real-time queue signals (e.g., `NUM_REQUESTS_RUNNING/WAITING`)—request tracking via #49538 is now actively sought.  
- **Multi-modal apps**: Avoid relying on overlapping audio chunking—current implementation uses non-overlapping chunks (#49414); update your preprocessing logic accordingly.  
- **Rust frontend users**: Feature parity is progressing rapidly (#44280); expect stable adoption soon for low-latency, embedded deployments.

> 🔗 [RFC #44280: Rust Frontend Parity Roadmap](https://github.com/vllm-project/vllm/issues/44280)  
> 🔗 [Feature Request #49538: Real-Time Queue Signals](https://github.com/vllm-project/vllm/issues/49538)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-02**

---

### **1. Today's Highlights**  
The SGLang ecosystem continues to mature with a strong focus on stability and performance for next-gen hardware, particularly NVIDIA Blackwell (SM120) and AMD MI350X. Critical issues around CUDA coredumps (#26340), DeepSeek-V4.1 speculative decoding hangs (#35822), and GPU memory exhaustion under sustained load (#43486) are actively being addressed. Meanwhile, significant progress is underway in the dLLM Serving roadmap (#39499) and HiCache IPC design (#37372), signaling deeper infrastructure investments.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes observed. The latest stable image (`lmsysorg/sglang:v0.5.20`) remains in use, though `dev-dsv41` builds are actively tested.

---

### **3. New Model & Hardware Support**  
- ✅ **AMD MI350X / MI355X (gfx950)**: Added support for MiniMax-M3.1-Flash via PR #46693, enabling full inference path including attention, MoE/GEMM, and quick all-reduce kernels.  
- ✅ **Apple Silicon (MLX/Torch interoperability)**: RFC #32321 outlines a redesign leveraging Torch-owned SRT paths and exported MLX regions, paving the way for native Apple Silicon serving.  
- ✅ **Intel XPU**: RFC #46520 proposes paged-experts with Lookahead Expert Scheduling (LES) and SSD-tier offload for single-GPU MoE workloads — targeting future scalability.  
- ✅ **Ascend NPU**: Documentation improvements ongoing (PR #36090), though gaps remain in deployment clarity (Issue #47382).

---

### **4. Performance & Optimization**  
- 🚀 **Qwen3.5 Prefill Fusion**: PR #47785 fuses prefill QK norm and RoPE with contiguous Q, reducing kernel overhead on NVIDIA GPUs.  
- ⚙️ **RMSNorm JIT Tuning**: PR #47503 introduces a single SM100-tuned template for plain, fused residual-add, and FP8 per-tensor quant variants, improving consistency across backends.  
- 🔍 **Hunyuan3D Export Optimization**: PR #46956 skips redundant OBJ exports when generating GLB, cutting intermediate I/O in shape+paint workflows.  
- 📉 **DeepSeek-V4 dsv4 Prefill Throughput**: Issue #33422 reports ~2–7K tok/s vs vLLM/Marlin’s ~12.5K on 4× RTX PRO 6000 (SM120). Investigation into backend tuning and kernel coverage is ongoing.  

---

### **5. Stability & Regressions**  
| Severity | Issue | Summary | Fix Status |
|--------|------|--------|-----------|
| 🔥 High | [#26340](https://github.com/sgl-project/sglang/issues/26340) | Auto-collected CUDA coredumps from CI; recurring crashes during testing. | *Ongoing investigation; no fix yet* |
| 🔥 High | [#43486](https://github.com/sgl-project/sglang/issues/43486) | DSV4.1 Flash + DSPARK: CUDA caching allocator exhausts GPU memory under sustained decode (OOM every 15–25 min), despite only 29% KV pool usage. | *Critical bug; no fix proposed* |
| 🔥 High | [#35822](https://github.com/sgl-project/sglang/issues/35822) | EAGLE speculative decoding hangs on Ampere (A2) with `tree_speculative_sampling_target_only`. | *Reproduced; fix pending* |
| 🟡 Medium | [#42012](https://github.com/sgl-project/sglang/issues/42012) | GLM-5.3-Flash on SM120 fails at CUDA-graph capture with FA4 backend; only Triton works. | *Workaround confirmed; FA4 fix needed* |
| 🟡 Medium | [#47218](https://github.com/sgl-project/sglang/issues/47218) | DeepSeek-V4 scheduler crash when multimodal offsets become `None` under concurrent aborted streams. | *Bug confirmed; fix in progress* |

---

### **6. What This Means for Application Developers**  
- **Expect instability on Blackwell (SM120) with FA4 backend**: Use Triton as fallback for GLM-5.3-Flash and DSV4.1 until PRs land. Monitor #26340 and #42012 for updates.  
- **Optimize for high-concurrency long-context workloads carefully**: Memory leaks in CUDA caching allocator (issue #43486) may cause OOM even with low KV pool utilization—avoid max-batch decode unless patched.  
- **Leverage upcoming dLLM Serving features**: The roadmap (#39499) promises better block-diffusion support and baseline compatibility for full-sequence models—ideal for agent pipelines.  
- **Plan for multi-node deployments early**: RFC #47657 introduces per-node HTTP frontend with global `X-Data-Parallel-Rank`, enabling external routers like `llm-d` to route efficiently across nodes.  
- **Watch for Apple Silicon readiness**: If targeting M-series Macs, follow #32321 for future native MLX integration.  

> 💡 *Pro tip*: For now, stick to `v0.5.20` or `dev-dsv41` images for stability; avoid `main` if running production workloads on Blackwell or complex MoE setups.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-02**

---

### **1. Today’s Highlights**  
The latest updates focus on critical backend stability and performance improvements across Vulkan, SYCL, and OpenCL, particularly for high-end GPUs like Intel Arc B70/B60 and Qualcomm Adreno. Key progress includes initial IQ3 support in OpenCL, fixes for DMA transfers on Hexagon, and a new `dflash` optimization that improves prefill speed via device memory copies. A notable regression in MoE inference on multi-GPU SYCL systems (100–1000× slowdown) has been reported and is under investigation.

---

### **2. Releases & Breaking Changes**  
- **b12032**: Added PkgConfig support for LevelZero includes/libs in SYCL backend ([#31076](https://github.com/ggml-org/llama.cpp/pull/31076))  
- **b12031**: Initial support for `iq3_s` quantization in OpenCL with AOS/SOA and tiled GEMM variants, including adreno-specific optimizations ([#31338](https://github.com/ggml-org/llama.cpp/pull/31338))  
- **b12030**: Fixed 2D DMA stride handling on Hexagon v75+ devices where strides >24 bits caused data corruption ([#31442](https://github.com/ggml-org/llama.cpp/pull/31442))  
- **b12029**: Fixed CLS token order in InternVL models for MTMD backend ([#30886](https://github.com/ggml-org/llama.cpp/pull/30886))  
- **b12028**: Enabled Gemma 4 testing with GQA in `test-llama-archs` ([#31401](https://github.com/ggml-org/llama.cpp/pull/31401))  
- **b12027**: Enabled tensor split for LFM3 models to improve memory efficiency ([#31431](https://github.com/ggml-org/llama.cpp/pull/31431))

> ✅ *No breaking API changes detected; all are feature or bugfix releases.*

---

### **3. New Model & Hardware Support**  
- **OpenCL**: Added experimental `iq3_s` quantization support (AOS/SOA, tiled MM, adreno-optimized).  
- **Hexagon**: Full support for large-stride DMA transfers on v75+ DSPs (critical for NPU offload).  
- **MTMD**: Added support for NVIDIA Nemotron 3.5 Transcribe SALM model ([#31471](https://github.com/ggml-org/llama.cpp/pull/31471)).  
- **CUDA**: Work ongoing to support `TQ1_0` and `TQ2_0` types (Maple Preview), with kernel implementations now merged ([#31512](https://github.com/ggml-org/llama.cpp/pull/31512)).  
- **Gemma 4**: Fully integrated into test suite with GQA enabled ([#31401](https://github.com/ggml-org/llama.cpp/pull/31401)).

---

### **4. Performance & Optimization**  
- **dflash prefill speedup**: New `dflash` implementation using device copies improves prefill throughput significantly — early benchmarks show ~30% faster prefill, decode gains of ~5% ([#31506](https://github.com/ggml-org/llama.cpp/pull/31506)).  
- **MoE cache optimization**: Hybrid LRFU/MQ-like cache (`ClassLRU`) shows +6% decode gain on QFN and +7% on GLM vs SLRU, with 8% fewer expert uploads ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233)).  
- **Flash Attention**: `flash_attn_ext_rows` extension reduces unified KV-cache penalty in multi-sequence inference by enabling optimized row filtering ([#29510](https://github.com/ggml-org/llama.cpp/pull/29510)).  
- **CUDA**: SWIGLU_CLAMP kernel added for float16/fp32 tensors with strided/contiguous layouts ([#31517](https://github.com/ggml-org/llama.cpp/pull/31517)).

---

### **5. Stability & Regressions**  
- **Critical Regression (SYCL)**: Multi-GPU `--cpu-moe` + `--split-mode layer` on Intel Arc B60/B70 results in 100–1000× slower prefill (e.g., 5-token prompt takes 165s) due to CPU-bound expert scheduling ([#31510](https://github.com/ggml-org/llama.cpp/issues/31510)). No fix PR yet.  
- **Vulkan Performance Drop**: Recent builds show significant eval performance regression on RX 6600 with Qwen3.5-9B-Q5_K_M GGUF ([#24066](https://github.com/ggml-org/llama.cpp/issues/24066)) — 49 comments, still open.  
- **Crash on AMD RADV**: `draft-mtp` mode causes `DeviceLost` error during prompt processing due to excessive `llama_decode()` calls post-prefill ([#27306](https://github.com/ggml-org/llama.cpp/issues/27306)).  
- **CUDA Blackwell Hang**: RTX 5090D hangs during decode with `Qwen3.8-27B-NVFP4` model ([#27329](https://github.com/ggml-org/llama.cpp/issues/27329)).  
- **Metal OOM on M5 Max**: Gemma 4 31B crashes with “Insufficient Memory” despite 128GB unified RAM when `n_ctx` is large ([#29521](https://github.com/ggml-org/llama.cpp/issues/29521)).

---

### **6. What This Means for Application Developers**  
- **Use dflash cautiously**: While it boosts prefill speed, ensure your backend supports the `device_copy` path — avoid `draft-mtp` on HIP if you need high prefill throughput ([#31495](https://github.com/ggml-org/llama.cpp/issues/31495)).  
- **Avoid `--cpu-moe` on multi-GPU SYCL setups** until [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) is resolved — expect severe performance degradation.  
- **Upgrade to b12031+** for better OpenCL support on mobile/NPU platforms (Adreno, Hexagon); consider `iq3_s` for lower VRAM use.  
- **Monitor Metal/MacOS builds** — known OOM issues persist with large-context models; reduce `n_ctx` or use `--no-mmap` as workaround.  
- **Enable `--moe-cache-mib` only on MoE models** — draft models with dense weights will fail if MoE cache is requested ([#31497](https://github.com/ggml-org/llama.cpp/pull/31497)).  

> 🔗 **Recommended Actions**:  
> - Test `dflash` and `draft-mtp` behavior per backend (Vulkan/HIP/CUDA).  
> - Use `gguf-dump` with caution — recent audit revealed escape sequence injection risk ([#28969](https://github.com/ggml-org/llama.cpp/issues/28969)).  
> - Monitor issue #31510 closely for SYCL multi-GPU workloads.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-02**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to mature with key improvements in model parsing, prompt caching, and tool handling—especially for emerging models like Qwen3.8 and LLM-jp-4.1 that use the GPT-OSS harmony format. Critical stability fixes are underway for Gemma3:4b structured output failures and Qwen model hallucination during thinking phases, while new PRs enable per-model prompt cache budgets and better token-level metrics logging.

---

### **2. Releases & Breaking Changes**  
*No new releases were published in the last 24 hours.*  
However, several impactful changes are in flight:
- **PR #19450**: Introduces per-model `prompt_cache` budgets via API request and Modelfile config (e.g., `PROMPT_CACHE=512MiB`). This resolves long-standing limitations where all models shared a global cache budget ([#19435](https://github.com/ollama/ollama/issues/19435)).  
- **PR #19447**: Fixes `/api/embed` to respect `tokenizer.ggml.add_bos_token` and `add_eos_token`, preventing context limit errors for models disabling these tokens ([#19446](https://github.com/ollama/ollama/issues/19446)).  
- **PR #19443**: Removes `file://*` from default CORS origins to prevent local HTML files from accessing the unauthenticated API—enhancing security by requiring opt-in via `OLLAMA_ORIGINS`.

---

### **3. New Model & Hardware Support**  
- **Qwen3.8-27B Hybrid-Attention GGUFs**: Failing to load due to missing `attn_qkv/attn_gate` projections at layer 64 ([#19459](https://github.com/ollama/ollama/issues/19459)). This indicates architectural mismatch or incomplete GGUF export—likely affecting users of Huihui-AI’s ablated variants.  
- **LLM-jp-4.1 (8B/32B/33B GGUF)**: Now recognized as supporting harmony format, but requires parser updates to handle space-after-special-tokens behavior ([#18728](https://github.com/ollama/ollama/issues/18728)).  
- **Qwen3.5 0.8B Q4_K_M Quantization**: Requested for availability ([#19436](https://github.com/ollama/ollama/issues/19436))—critical for CPU inference performance. Currently only available for larger Qwen3.5 variants.

---

### **4. Performance & Optimization**  
- **Per-Model Prompt Caching**: PR #19450 enables fine-grained control over memory usage per model, allowing high-throughput applications to isolate resource-heavy models without impacting others.  
- **Token Repeat Limiting**: PR #19422 adds detection of repeated tokens (>101 consecutive) in MLX backend, preventing infinite generation loops and improving reliability on edge cases ([#16179](https://github.com/ollama/ollama/issues/16179)).  
- **Scheduler Eviction Improvements**: PR #19449 enhances runner selection during eviction by considering both memory footprint and idle duration, improving load balancing under pressure ([#19326](https://github.com/ollama/ollama/issues/19326)).  
- **Embedding Context Accuracy**: PR #19447 ensures correct context window accounting for BOS/EOS tokens, avoiding truncation in embedding pipelines.

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR |
|--------|------|--------|-------|
| 🔴 High | `gemma3:4b` returns empty JSON with structured output + long prompts | Closed | [#19427](https://github.com/ollama/ollama/issues/19427) |
| 🔴 High | Qwen models terminate entire chat when `</thinking>` is emitted | Closed | [#19424](https://github.com/ollama/ollama/issues/19424) |
| 🟡 Medium | Long replies cause `deepseek-r1`, `phi3.5`, etc. to degrade until reload | Open | [#19457](https://github.com/ollama/ollama/issues/19457) |
| 🟡 Medium | `qwen3next:layer64` missing `attn_qkv/attn_gate` projections | Open | — |
| 🟡 Medium | Generic tool parser misidentifies function names from argument strings | Open | [#19425](https://github.com/ollama/ollama/issues/19425) |
| 🟡 Low | Windows ARM64 Gemma3 vision requests time out during CPU image processing | Open | [#19444](https://github.com/ollama/ollama/issues/19444) |

> ⚠️ Note: Multiple regressions tied to **structured output**, **tool calling**, and **context management** suggest ongoing stress testing in agent workflows.

---

### **6. What This Means for Application Developers**  
- **Design for Per-Model Resource Isolation**: With per-model prompt cache budgets now being implemented, developers should define `PROMPT_CACHE` in Modelfiles or API calls to avoid interference between models in multi-tenant or agent-based systems.
- **Handle Tool Call Parsing Carefully**: Avoid relying on model-generated function names that match registered tools—use `tools:[]` explicitly and expect potential mismatches if the model emits unrecognized tags ([#19425](https://github.com/ollama/ollama/issues/19425)).
- **Enable Structured Output Safely**: For `gemma3:4b`, avoid long extraction prompts unless you can validate schema compatibility; consider fallback logic for empty JSON responses ([#19427](https://github.com/ollama/ollama/issues/19427)).
- **Monitor Memory & Context Behavior**: Long-running chats may trigger degradation in `phi3.5` and `deepseek-r1` due to cached key rescaling ([#19457](https://github.com/ollama/ollama/issues/19457)); implement periodic model reloading or context pruning.
- **Use Token Metrics for Cost Control**: The proposed opt-in token logging ([#19455](https://github.com/ollama/ollama/issues/19455)) will soon allow operators to track throughput, cache efficiency, and generated token distribution—essential for billing and SLA tracking.

---  
*Data source: [github.com/ollama/ollama](https://github.com/ollama/ollama)*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

---

### **LiteLLM Digest — 2026-11-02**

#### **1. Today's Highlights**  
The LiteLLM project is making significant progress on its dual-core architecture with the introduction of a production-ready Rust router core (#49513), enabling high-performance, low-latency inference routing while maintaining full compatibility with existing Python-based workflows. Concurrently, new PRs are actively stabilizing cross-core parity between Rust and Python in `/v1/messages` handling (#49610, #49611) and enhancing proxy security and observability through improved Redis credential management and auth enforcement.

#### **2. Releases & Breaking Changes**  
*No new releases were published in the last 24 hours.*  
However, several critical configuration and behavioral changes are underway:
- `pass_through_endpoints` now silently strips `id` from JSON-RPC 2.0 request bodies due to internal param collision (Issue #49479). This breaks compliance and requires client-side workarounds or upstream fixes.
- The `/cursor/chat/completions` endpoint fails on all streaming requests due to a missing `request` argument in `cursor_data_generator()` (Issue #35632), rendering it unusable until fixed.

#### **3. New Model & Hardware Support**  
*No new models or hardware backends were added today.*  
Notable ongoing work includes:
- Ongoing efforts to expand support for FAL.ai’s full model catalog beyond the current hardcoded list (Issue #34619).
- Improvements to tool call streaming behavior for Ollama (`ollama_chat`) providers, where `finish_reason` incorrectly reports `stop` instead of `tool_calls` (Issue #35663).

#### **4. Performance & Optimization**  
Significant performance enhancements are being delivered via the **Rust gateway core**:
- The new Rust router core (#49513) eliminates Python GIL contention and reduces latency in routing decisions.
- Router cooldowns, TPM/RPM limits, and global usage tracking are now offloaded to a Rust-backed Redis store (#49525, #49481), improving scalability and consistency across distributed deployments.
- Optimized Redis client behavior now matches `redis-py` 5.3.1 in command execution, TTL handling, and error classification (#49189), reducing silent failures during failover.

#### **5. Stability & Regressions**  
Critical stability issues reported today include:
- **[High Severity]** `pass_through_endpoints` removes `id` from JSON-RPC 2.0 requests, breaking protocol compliance (Issue #49479) — *no fix PR yet*.
- **[High Severity]** Cost tracking can overbill by up to 1,000,000× if provider-reported `usage.cost` is trusted without validation (Issue #48398) — *fix PR pending*.
- **[Medium Severity]** `/tag/update` returns 500 when updating a budgeted tag due to duplicate `budget_id` arg (Issue #46999) — *fix PR pending*.
- **[Medium Severity]** Exact-match cache is shared across virtual keys/teams, leading to incorrect response caching (Issue #49313) — *fix PR pending*.
- **[Low Severity]** `model_info` JSON missing `created_at`, `created_by`, `updated_at` causes UI to show "Unknown" despite correct DB values (Issue #35623) — *fix PR pending*.

#### **6. What This Means for Application Developers**  
Developers should **avoid using `pass_through_endpoints` with JSON-RPC 2.0 payloads** until Issue #49479 is resolved, as it will corrupt request integrity. Be cautious with cost monitoring: **do not rely on provider-reported `cost` values without validation**, especially in multi-tenant setups (Issue #48398). If you use agent tools with image inputs, be aware that default `detail:auto` pricing charges 85 tokens regardless of image size (Issue #48422). For teams deploying at scale, consider upgrading to the new Rust-powered proxy (via `--enable-rust-gateway`) for better throughput and lower latency—though this remains opt-in and still under stabilization.

> 🔗 [Issue #49479](https://github.com/BerriAI/litellm/issues/49479) | [Issue #48398](https://github.com/BerriAI/litellm/issues/48398) | [PR #49513](https://github.com/BerriAI/litellm/pull/49513)

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-02**

---

### **1. Today's Highlights**  
The Unsloth ecosystem continues to expand its multi-model and multi-backend support, with critical fixes for model loading stability on Windows and Linux, especially around GGUF, MoE, and Vulkan backends. Key improvements include better handling of GPU/CPU layer distribution in MoE models, enhanced Gemini API integration, and new CLI/Studio features for rich text and voice input. A major regression in isolated Studio installs has been addressed, restoring environment isolation guarantees.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API/config changes were published.

---

### **3. New Model & Hardware Support**  
- ✅ **Gemini API**: Full pagination support now implemented via PR [#16433](https://github.com/unslothai/unsloth/pull/16433), enabling discovery of all models including `gemini-4-argon`, `gemini-3.8-flash`, and others previously hidden behind a 20-model cap.
- ✅ **Vulkan on AMD GPUs**: PR [#16421](https://github.com/unslothai/unsloth/pull/16421) adds retry logic when `llama.cpp`’s `fit` step crashes (e.g., on RX 5700 XT), improving resilience during model loading on Windows.
- ✅ **MLX Backend**: PR [#16441](https://github.com/unslothai/unsloth/pull/16441) introduces experimental draft support for MLX-based tools, video, grammar-constrained, KV-quantized, and LoRA replies—critical for Apple Silicon users.
- ✅ **FP8 LoRA Training**: PR [#16423](https://github.com/unslothai/unsloth/pull/16423) enables `float8_training=True` on frozen 16-bit base models with LoRA, allowing mixed-precision fine-tuning without full model quantization.

---

### **4. Performance & Optimization**  
- ⚙️ **MoE Layer UX Optimization**: PR [#16429](https://github.com/unslothai/unsloth/pull/16429) hides the "MoE Layers on CPU" slider when GPU layers are set to 0—reducing confusion and unnecessary configuration overhead.
- 📈 **Memory Efficiency**: PR [#16438](https://github.com/unslothai/unsloth/pull/16438) corrects memory reporting on macOS by scaling `ru_maxrss` from bytes to mebibytes, fixing misleading peak memory readings in LTX workers.
- 🔧 **Parallelism Framework**: PR [#16407](https://github.com/unslothai/unsloth/pull/16407) introduces `UNSLOTH_PARALLEL` as a unified environment flag for DP/TP/PP/CP, enabling flexible pipeline parallel training configurations with a single command.

---

### **5. Stability & Regressions**  
- 🔥 **Critical Regression (Incident-level)**: Two duplicate issues (#16186, #16185) report that `UNSLOTH_STUDIO_HOME=<path> ./install.sh --local` silently overwrites existing baseline venvs, breaking isolation—resolved in PR [#16428](https://github.com/unslothai/unsloth/pull/16428) via improved loader baseline tracking.
- 🛑 **Model Loading Failures**: 
  - Issue [#16317](https://github.com/unslothai/unsloth/issues/16317): Stream interruption with Bearer Token + OpenAIAPI + OpenCode — likely due to auth header misprocessing.
  - Issue [#16263](https://github.com/unslothai/unsloth/issues/16263): GGUF model fails to load from ModelScope due to 404 on pinned HF revision; fixed via PR [#16433](https://github.com/unslothai/unsloth/pull/16433).
- ❌ **GPU Crashes**: 
  - Issue [#16294](https://github.com/unslothai/unsloth/issues/16294): "No available kernel" crash with MiniMax-H3 on Windows when Flash Attention is missing — resolved by PR [#16421](https://github.com/unslothai/unsloth/pull/16421) with fallback logic.
- 🔄 **UI/UX Bugs**: 
  - Issue [#16193](https://github.com/unslothai/unsloth/issues/16193): "Show in Folder" does nothing in desktop gallery — fixed via PR [#16429](https://github.com/unslothai/unsloth/pull/16429) (though not yet merged).
  - Issue [#16303](https://github.com/unslothai/unsloth/issues/16303): Custom background resets after opening chat — under investigation.

---

### **6. What This Means for Application Developers**  
- **Build robust, multi-platform agents**: Use `UNSLOTH_PARALLEL` for scalable distributed training across TP/DP/PP axes. The new capability registry simplifies complex launch setups.
- **Support diverse inference backends**: With MLX, Vulkan, and FP8 LoRA now supported, your apps can run efficiently on Apple Silicon, AMD GPUs, and low-memory devices.
- **Handle edge cases in model loading**: Be cautious with `trust_remote_code=True` models (issue [#16331](https://github.com/unslothai/unsloth/issues/16331)) and ensure custom code paths are explicitly enabled in CLI.
- **Design resilient UI workflows**: Avoid assumptions about model state persistence—users may encounter transient crashes (e.g., #16436) when switching models mid-generation. Implement retry logic and clear error messaging.
- **Leverage rich media**: The new rich text composer (PR [#16396](https://github.com/unslothai/unsloth/pull/16396)) and video attachment fixes enable richer agent interactions—ideal for multimodal applications.

> ✅ *Recommendation*: Pin to `main` or use stable builds from `v0.1.913-beta` until PRs like [#16433](https://github.com/unslothai/unsloth/pull/16433) and [#16421](https://github.com/unslothai/unsloth/pull/16421) merge into production.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*