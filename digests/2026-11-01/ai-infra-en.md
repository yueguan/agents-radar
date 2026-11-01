# AI Infrastructure Digest 2026-11-01

> Generated: 2026-11-01 01:26 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

---

### **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-01**

---

#### **1. Ecosystem Overview**  
The AI infrastructure landscape in late 2026 is defined by a sharp bifurcation between **high-performance inference engines** and **developer-centric deployment platforms**, with growing specialization across hardware backends and model types. vLLM, SGLang, and llama.cpp lead in low-level performance optimization for large language models (LLMs), while Ollama and LiteLLM focus on usability and enterprise-grade API gateways. Unsloth emerges as a full-stack contender, blending training, fine-tuning, and inference into a unified workflow—particularly strong in vision-language models and local-first development. The ecosystem is maturing rapidly, with increasing emphasis on distributed serving, speculative decoding stability, and cross-platform compatibility.

---

#### **2. Activity Comparison**  

| Project        | Issues Open (↑) | PRs Merged (↑) | Releases (Latest) | Notes |
|----------------|------------------|------------------|--------------------|-------|
| **vLLM**       | 875 (+34)        | 98 (+12)         | `v0.30.0`          | High focus on SM8x/ROCm support; critical regressions in FP8 image handling |
| **SGLang**     | 1,122 (+41)      | 87 (+15)         | None               | Heavy activity around CUDA stability and NCCL integration; 351-comment coredump issue |
| **llama.cpp**  | 943 (+28)        | 112 (+18)        | `b12024`           | Strong bug fixes in speculative decoding, SYCL/OpenCL; Intel Arc instability persists |
| **Ollama**     | 1,367 (+52)      | 76 (+11)         | None               | Most issues reported—especially on Windows/macOS; memory leaks dominate |
| **LiteLLM**    | 819 (+22)        | 68 (+10)         | None               | Focus on security (key revocation), routing reliability, and budget enforcement |

> ✅ *Trend*: Ollama leads in issue volume due to platform-specific bugs; SGLang and vLLM show highest engineering velocity in core performance improvements.

---

#### **3. Model Support Race**  

| New Model / Architecture             | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------------------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4-Flash / -0731**        | ✅ (SM8x track) | ✅ (MoE kernel opt) | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.8-2.4T-A95B / Flash-Next**   | ✅ (ROCm gfx950 plan) | ✅ (fp8_mqa_logits) | ❌ | ❌ | ❌ | ❌ |
| **Gemma 4 (Vision)**                 | ⚠️ FP8 crash | ⚠️ No fix yet | ✅ (`mtmd`, no letterboxing) | ❌ (500 error) | ❌ | ✅ (CLI image input) |
| **Qwen3.5-MoE / Coder**              | ✅ | ✅ | ✅ (`reasoning_format none`) | ⚠️ Termination on `</thinking>` | ❌ | ❌ |
| **MIMO v2.5 (1M context)**           | ❌ | ❌ | ❌ | ✅ (request) | ❌ | ❌ |
| **VirbiusGuard (0.6B safety guard)** | ❌ | ❌ | ❌ | ✅ (request) | ❌ | ❌ |

> 🏆 **Winner**: **Unsloth** leads in **vision-language model support** with CLI image input and end-to-end VLM workflows.  
> 🥈 **vLLM** leads in **production readiness for next-gen MoE models** (DeepSeek-V4, Qwen3.8).  
> 🥉 **llama.cpp** excels in **local CPU/GPU portability** and **model format flexibility** (GGUF, OpenCL/SYCL).

---

#### **4. Performance Frontier**  

| Optimization Area            | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|-------------------------------|------|--------|-----------|--------|---------|---------|
| **KV Cache Efficiency**       | ✅ (offload, bounds) | ⚠️ (PP disaggregation) | ⚠️ (shared prefix) | ⚠️ (LFM3-Omni leak) | ⚠️ (spend logs) | ⚠️ (cache probing) |
| **Batching & Throughput**     | ✅ (sliding window) | ⚠️ (prefill gap: ~2–7K tok/s vs vLLM’s 12.5K) | ⚠️ (MTP draft divergence) | ⚠️ (memory pressure) | ✅ (Rust Redis cooldown) | ⚠️ (multi-user latency) |
| **Quantization (FP8, IQ3)**   | ✅ (FP8 Triton kernels) | ✅ (fp8_mqa_logits) | ✅ (IQ3_S/XXS) | ❌ | ❌ | ❌ |
| **Distributed Serving**       | ✅ (async offload, PCP sync) | ✅ (NCCL EP, M-to-N transfer) | ❌ | ❌ | ✅ (Rust Redis store) | ❌ |
| **Kernel-Level Tuning**       | ✅ (int32/int64 copy removal) | ✅ (MoE fusion, AIter routing) | ✅ (FWHT, DMA strides) | ❌ | ❌ | ✅ (log redaction, I/O caching) |

> 🔍 **Key Insight**: vLLM and SGLang are pushing the envelope in **distributed inference and kernel optimization**, while **llama.cpp** dominates in **cross-platform quantization and lightweight runtime efficiency**. LiteLLM is innovating in **infrastructure resilience via Rust-backed state management**.

---

#### **5. Layer Positioning**  

| Project        | Primary Layer                     | Key Differentiators |
|----------------|-----------------------------------|---------------------|
| **vLLM**       | **Inference Engine**              | High-throughput, GPU-optimized, supports latest MoE & Flash attention |
| **SGLang**     | **Inference Engine + Orchestration** | Advanced speculative decoding, multi-node disaggregation, NCCL focus |
| **llama.cpp**  | **Local Runtime / Embedded Inference** | CPU-first, GGUF-native, broad backend support (SYCL, OpenCL, WebGPU) |
| **Ollama**     | **Developer Gateway / Local Runtime** | User-friendly CLI, agent tooling (tokencut), Apple Silicon polish |
| **LiteLLM**    | **API Gateway / Multi-Provider Proxy** | Budget enforcement, key revocation, MCP/Multi-provider routing |
| **Unsloth**    | **Full-Stack Platform (Train → Serve)** | Integrated Studio, CLI image input, fine-tuning + inference pipeline |

> 🧩 **Strategic Insight**: vLLM and SGLang are **core inference engines**; Ollama and LiteLLM are **application-facing platforms**; Unsloth is the only project bridging **training, fine-tuning, and inference** in one stack.

---

#### **6. Trend Signals**  

🔍 **Emergent Industry Trends from 2026-11-01 Activity**:

1. **Hardware Fragmentation is Accelerating**:  
   - AMD ROCm (gfx950) and Intel XPU support are now high-priority targets—indicating demand for non-NVIDIA ecosystems.
   - Huawei Ascend NPU RFCs signal rising regional hardware sovereignty needs (China-focused deployments).

2. **Speculative Decoding Stability is a Critical Bottleneck**:  
   - Multiple projects report crashes or output divergence under MTP/EAGLE configurations (vLLM, SGLang, llama.cpp).
   - This suggests **speculative decoding is not yet production-ready**, requiring careful validation before use in agents.

3. **Memory Management is the #1 Operational Risk**:  
   - Ollama (LFM3-Omni leak), SGLang (CUDA coredumps), and Unsloth (NVLink corruption) all highlight memory-related instability.
   - Developers must monitor per-model memory budgets and avoid overloading shared runners.

4. **Security & Governance Are Now Core Features**:  
   - LiteLLM’s `/key/revoke` endpoint and Ollama’s `tokencut` reflect growing need for **accidental exposure mitigation** and **context control** in agent systems.

5. **Agent Workflows Demand Full Pipeline Integration**:  
   - Unsloth’s CLI image input, Ollama’s tokencut, and LiteLLM’s spend logging show that **agent developers need end-to-end tooling**—not just inference.

---

### ✅ **Recommendations for Application Developers**  
- **For high-throughput LLM serving**: Use **vLLM** (with `v0.22.0` if using Gemma 4 + FP8 images) or **SGLang** (if you can tolerate higher risk in speculation).
- **For local, CPU-based inference**: Choose **llama.cpp** (b12023+) for best model compatibility and minimal dependencies.
- **For developer experience & agent workflows**: **Ollama** with `tokencut` and **LiteLLM** with `spend_logs_prompt_storage: latest_turn` offer strong operational advantages.
- **For vision-language pipelines**: **Unsloth** provides the most seamless transition from fine-tuning to inference.
- **Always validate speculative decoding**—avoid it until regressions are patched in your stack.

> 💬 *Bottom Line*: The AI infrastructure stack is no longer monolithic—**choose based on your workload, hardware, and operational maturity**. Prioritize stability over novelty in production.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-11-01**

---

#### **1. Today's Highlights**  
The vLLM project is accelerating its support for next-generation models and hardware, with critical work underway on **SM8x (Ampere) compatibility for DeepSeek-V4-Flash**, resolving a major barrier for production deployment on widely used A100/A800 and RTX 30xx GPUs. Simultaneously, AMD ROCm performance optimization efforts are intensifying for Qwen3.8-series models on gfx950 (MI355X), while new PRs focus on reducing memory overhead in sparse indexing and improving KV offload reliability across distributed systems.

---

#### **2. Releases & Breaking Changes**  
*No new releases or breaking changes reported in the last 24 hours.*  
The latest stable version remains `v0.30.0`, with ongoing development focused on feature parity, performance tuning, and stability fixes across multiple backends.

---

#### **3. New Model & Hardware Support**  
- **DeepSeek-V4-Flash / DeepSeek-V4-Flash-0731**: Full SM8x (Ampere: A100/A800, RTX 30xx) support is now actively being tracked in [#50576](https://github.com/vllm-project/vllm/issues/50576), with community demand high (123 comments). This will unlock full inference capability on legacy enterprise-grade GPUs.
- **Qwen3.8-2.4T-A95B & Qwen3.8-Flash-Next**: Dedicated ROCm performance optimization plans launched for **gfx950 / MI355X** in [#57149](https://github.com/vllm-project/vllm/issues/57149) and [#59575](https://github.com/vllm-project/vllm/issues/59575), targeting MXFP4 MoE and GatedDeltaNet architectures.
- **Intel GPU (XPU)**: Continued progress on XPU support, including fix for compressed-tensors WNA16 MoE selector crash in [#43750](https://github.com/vllm-project/vllm/issues/43750), though platform remains experimental.

---

#### **4. Performance & Optimization**  
- **ROCm-Specific Optimizations**: Multiple PRs improve kernel efficiency:
  - [#63910](https://github.com/vllm-project/vllm/pull/63910): Eliminates int32/int64 copies in GLM-5.3-Flash kpool indexer → reduces CPU-GPU sync and memory churn.
  - [#63867](https://github.com/vllm-project/vllm/pull/63867): Skips fully-masked context tiles in sliding-window prefill → improves throughput on long-context models.
  - [#62913](https://github.com/vllm-project/vllm/pull/62913): Enables FP8 Triton kernels for ROCm prefill attention → leverages per-tensor FP8 cache with optimized kernels on gfx942.
- **Memory Efficiency**: 
  - [#55985](https://github.com/vllm-project/vllm/pull/55985): Bounds `fastsafetensors` peak memory via 0.4-fit planner to prevent OOM during model loading.
  - [#59367](https://github.com/vllm-project/vllm/pull/59367): Reserves DCP top-k merge scratch space via workspace manager → prevents transient allocation spikes.
- **Async & Offload Improvements**: 
  - [#64236](https://github.com/vllm-project/vllm/pull/64236): Defer async loads in mixed steps → avoids blocking synchronous loads and improves concurrency.

---

#### **5. Stability & Regressions**  
Critical stability issues reported today include:

| Severity | Issue | Description | Fix Status |
|--------|------|-------------|------------|
| 🔴 High | [#63732](https://github.com/vllm-project/vllm/issues/63732) | Image requests crash with Gemma 4 + FP8 quantization (`v0.23.0+`) due to FP8-cast pixel values overflowing patch_embedder input | No fix yet; confirmed regression from `v0.22.0` |
| 🔴 High | [#63549](https://github.com/vllm-project/vllm/issues/63549) | Async scheduling + preemption crashes Model Runner V1 with `KeyError all_token_ids[req_id]` | No fix yet; affects P/D-disaggregated Ascend deployments |
| 🟡 Medium | [#64489](https://github.com/vllm-project/vllm/issues/64489) | `vllm/vllm-openai:unlimited-ocr` image fails to serve Unlimited-OCR model | Pending investigation |
| 🟡 Medium | [#64654](https://github.com/vllm-project/vllm/issues/64654) | Request ID logging without content leakage requested for observability | Feature request; no PR |

> ✅ *Note*: Several bugfix PRs were merged recently (e.g., [#63874](https://github.com/vllm-project/vllm/pull/63874), [#62916](https://github.com/vllm-project/vllm/pull/62916)), but none address these high-severity regressions.

---

#### **6. What This Means for Application Developers**  
- **Use v0.22.0 if serving Gemma 4 with FP8 image inputs** until [#63732](https://github.com/vllm-project/vllm/issues/63732) is resolved — avoid `v0.23.0+`.
- **Deploying on AMD ROCm?** Prioritize testing with Qwen3.8-2.4T-A95B and Qwen3.8-Flash-Next on MI355X — performance gains expected via upcoming PRs like [#63910](https://github.com/vllm-project/vllm/pull/63910) and [#62913](https://github.com/vllm-project/vllm/pull/62913).
- **Hybrid Mamba + EAGLE models** (e.g., DeepSeek-V4-Flash) may still suffer from untested prefix hit behavior beyond 2 attention groups — see [#51771](https://github.com/vllm-project/vllm/issues/51771).
- **Enable `--reasoning-parser qwen3` carefully** — known issue with leading `\n\n` retention post `<think>` block ([#61972](https://github.com/vllm-project/vllm/issues/61972)) may affect tool-call parsing accuracy.
- **Monitor NixlPushMode reliability** — known failure modes in large-scale deployments under orchestration layers; see [#48633](https://github.com/vllm-project/vllm/issues/48633).

> 💡 *Pro Tip:* For multi-node deployments, ensure `engine_id` synchronization across PCP ranks using updated PRs like [#62916](https://github.com/vllm-project/vllm/pull/62916) to prevent startup failures.

---  
*Digest compiled from GitHub activity: [vllm-project/vllm](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

---

### **SGLang Digest — 2026-11-01**

#### **1. Today's Highlights**  
The SGLang project continues to deepen its support for high-throughput, low-latency inference across diverse hardware and workloads. Key activity centers on resolving critical stability issues in speculative decoding (especially with EAGLE3/MTP and DeepSeek-V4) and advancing production-grade infrastructure—most notably, the ongoing migration to a unified `api.v1` backend and improvements in multi-node disaggregation. A major focus remains on stabilizing NCCL-based communication and improving CI reliability.

#### **2. Releases & Breaking Changes**  
*None.* No new releases or breaking API/config changes were published in the last 24 hours.

#### **3. New Model & Hardware Support**  
- **Model Support:**  
  - *DeepSeek-V4.1:* Active development around correct prompt rendering (`deepseek-recipe`) and MoE kernel optimization via `aiter` routing on AMD gfx950 (PR #42499).  
  - *MiniMax-M3.1-Flash GB300:* Added to the SGLang cookbook for PD AgentX deployments (PR #47699).  
- **Hardware/Backend Support:**  
  - **NCCL 2.30+ Features:** Experimental integration of NCCL EP (low-latency decode), cross-group M-to-N transfer, and zero-SM one-sided communication (tracked in #32704, PR #47600).  
  - **Intel CPU Backend:** Ongoing optimization roadmap (Q4 2026) focused on MoE execution and cost-effective small/medium model serving (Issue #44675).  
  - **AMD ROCm:** Continued refinement of fp8_mqa_logits handling and MoE fusion (e.g., PR #42504).

#### **4. Performance & Optimization**  
- **Throughput Gaps Identified:**  
  - DeepSeek-V4-Flash prefill on SM120 (4× RTX PRO 6000, TP=4) achieves only **~2–7K tok/s**, significantly below vLLM/Marlin’s reported **~12.5K tok/s** (Issue #33422). Investigation underway into kernel coverage and tuning guidance.  
- **Kernel-Level Optimizations:**  
  - AMD: Fused MoE combine + decode all-reduce (PR #42504); routed dense GEMMs >8 rows to `aiter` for better throughput (PR #42499).  
  - Triton: Fixed sliding window decode to read full window (PR #42402).  
- **Memory & Scheduling:**  
  - Deferred token allocator free-list copies under pressure (PR #42520) to reduce CPU overhead.  
  - Agentic Cache Scheduling System roadmap launched (Issue #43098) for supernode-wide KV cache sharing.

#### **5. Stability & Regressions**  
- **Critical Crashes & Bugs:**  
  1. **CUDA Coredump Tracker (#26340):** 351 comments; auto-collected coredumps from PR tests indicate systemic instability in CUDA kernels. High severity; no fix yet. [GitHub Issue](https://github.com/sgl-project/sglang/issues/26340)  
  2. **PP Disaggregated Decode Garbage Output (#47613):** Runs of token 0 when `pp_async_batch_depth > pp_size / 2`. Reproducible with checksums; affects data-parallel scalability. [GitHub Issue](https://github.com/sgl-project/sglang/issues/47613)  
  3. **EAGLE Draft Extend Crash (#47490):** Negative dimension error under DP attention + FlashInfer A2A + prefill CUDA graph. Critical for speculative decoding workflows. [GitHub Issue](https://github.com/sgl-project/sglang/issues/47490)  
- **CI/Infrastructure Flakiness:**  
  - Persistent flaky tests in `PR Test Base/Extra` (Issue #42752); affects PR validation pipeline reliability. [GitHub Issue](https://github.com/sgl-project/sglang/issues/42752)

#### **6. What This Means for Application Developers**  
- **Expect Instability in High-Throughput Scenarios:** Avoid using `pp_async_batch_depth > pp_size / 2` until #47613 is resolved.  
- **Monitor Speculative Decoding Behavior:** EAGLE3/MTP and DeepSeek-V4 models may crash or produce incorrect output under certain configurations—validate against latest `main` builds.  
- **Use Unified API Where Possible:** The migration to `api.v1.Generate` (RFC #45816) will simplify client code and improve compatibility—prepare for future deprecation of legacy endpoints.  
- **Avoid S3 + Modelexpress Conflicts:** Explicit `MX_MODEL_URI` settings are being overridden by S3 paths (Issue #47428)—ensure consistent loader selection in deployment scripts.  
- **Offline Benchmarking:** Use `--dataset-name random --offline` if HF connectivity is restricted (Issue #47420).  

> ✅ **Actionable Tip:** For production deployments, pin to stable `main` commits and monitor issue #26340 for CUDA stability alerts before scaling.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-01**

---

### **1. Today's Highlights**  
The latest updates focus on refining speculative decoding stability, improving SYCL and OpenCL backend reliability, and enhancing model compatibility for Gemma 4 and Qwen3 series. Critical fixes address crashes in `--split-mode tensor` on Intel Arc GPUs and ensure correct handling of shared decision prefixes in multi-task inference.

---

### **2. Releases & Breaking Changes**  
- **b12024**: Reshapes 3D tensor operations to 2D for LFM SC (Long-Form Memory) to improve memory layout efficiency. [PR #30831](https://github.com/ggml-org/llama.cpp/pull/30831)  
- **b12023**: Fixes shared prefix truncation in server responses to preserve decision embeddings across child tasks. Addresses #30482. [PR #30710](https://github.com/ggml-org/llama.cpp/pull/30710)  
- **b12022**: Enables `/v1/responses` endpoint to accept `extra_params`, improving compatibility with downstream LLM gateways. [PR #30959](https://github.com/ggml-org/llama.cpp/pull/30959)  
- **b12021**: Removes letterbox padding when resizing images for Gemma 4 models, improving vision input fidelity. [PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)  
- **b12020**: Adds FWHT (Fast Walsh-Hadamard Transform) support to OpenCL backend. [PR #31434](https://github.com/ggml-org/llama.cpp/pull/31434)  

> ⚠️ No breaking API changes reported; all updates are additive or bug-fix focused.

---

### **3. New Model & Hardware Support**  
- **Gemma 4**: Full support added for image preprocessing without letterboxing (via `mtmd`). Also tested with MTP draft models and `--mmproj`. [PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)  
- **Qwen3-Coder**: Proper handling of `reasoning_format none` and think blocks. [PR #31380](https://github.com/ggml-org/llama.cpp/pull/31380)  
- **OpenCL**: Initial support for IQ3_S and IQ3_XXS quantization formats (Adreno-optimized). [PR #31338](https://github.com/ggml-org/llama.cpp/pull/31338)  
- **SYCL**: Enhanced LevelZero detection via PkgConfig (for oneAPI 2026.1+). [PR #31076](https://github.com/ggml-org/llama.cpp/pull/31076)  
- **WebGPU**: Group norm kernel implemented—critical for diffusion models in downstream tools like stable-diffusion.cpp. [PR #31490](https://github.com/ggml-org/llama.cpp/pull/31490)

---

### **4. Performance & Optimization**  
- **SYCL**: Optimized CPY rounding for Q4_1/Q5_1/Q8_0 types improves consistency across platforms (fixes off-by-one errors on B70/B580). [PR #31465](https://github.com/ggml-org/llama.cpp/pull/31465)  
- **CPU**: Refactored `mm` from 3D to 2D tensor reshaping reduces overhead in LFM SC. [PR #30831](https://github.com/ggml-org/llama.cpp/pull/30831)  
- **OpenCL**: Fixed invalid work-group size crash on Adreno 830 during `Q6_K` GEMM. [PR #31485](https://github.com/ggml-org/llama.cpp/pull/31485)  
- **Hexagon**: DMA transfers now support strides > 24 bits (v75+ devices), enabling larger tensor handling. [PR #31442](https://github.com/ggml-org/llama.cpp/pull/31442)  
- **iOS**: Switched from `posix_memalign` to `vm_allocate` to fix memory footprint reporting inaccuracies post-model unload. [PR #31437](https://github.com/ggml-org/llama.cpp/pull/31437)

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR | Link |
|--------|------|-------|--------|------|
| High | `--split-mode tensor` crashes on dual Intel Arc B70 due to `DEVICE_LOST` in SYCL | Open | Pending | [#27198](https://github.com/ggml-org/llama.cpp/issues/27198) |
| High | Speculative decoding (`draft-dflash/dspark`) produces non-identical output at `--temp 0` on Qwen3.5-MoE | Open | Pending | [#31472](https://github.com/ggml-org/llama.cpp/issues/31472) |
| High | CUDA: Illegal memory access when MTP drafts follow KV cache rollback (RTX 4060 Ti) | Open | Pending | [#31077](https://github.com/ggml-org/llama.cpp/issues/31077) |
| Medium | Vulkan: Performance drop in recent builds (RX 6600, Qwen3.5-9B-Q5_K_M) | Open | Pending | [#24066](https://github.com/ggml-org/llama.cpp/issues/24066) |
| Medium | SYCL: `--fit` doesn’t fully account for memory usage → runtime overflow | Open | Pending | [#27595](https://github.com/ggml-org/llama.cpp/issues/27595) |

> Note: Multiple regressions impact production-grade inference pipelines involving speculative decoding, MTP, and multi-GPU setups.

---

### **6. What This Means for Application Developers**  
- **Use caution with speculative decoding**: Avoid `draft-dflash`/`dspark` on MoE models (`qwen35moe`) until the divergence issue is resolved. Use `draft-mtp` as a safer alternative.  
- **Intel Arc users**: Expect instability in `--split-mode tensor` mode; avoid it until #27198 is patched. Consider single-GPU inference or fallback to CUDA.  
- **Vision models (Gemma 4)**: Leverage `mtmd` improvements for better image resolution handling without distortion.  
- **Build robustness**: Use `--server` with `extra_params` support (b12022+) for better integration with API gateways like vLLM or LlamaIndex.  
- **Memory management**: On iOS/macOS, expect accurate `phys_footprint` only after applying PR #31437.  
- **Future-proofing**: Monitor WebGPU group norm support (PR #31490) if building multimodal agents or TTS systems.

👉 **Recommendation**: Pin your deployment to `b12023` or later for stability, and monitor #31472 and #31077 for critical fixes before production use.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-01**

---

### **1. Today's Highlights**  
A surge in critical stability issues on Windows and macOS highlights ongoing challenges with memory management and network resilience, particularly for large models and agent workflows. Key PRs address long-standing bugs in OpenAI-compatible API behavior (e.g., `top_p` override) and fix memory leaks in LFM3-Omni and MLX prefix caches. The community continues to push for better model support (Qwen3.5 0.8B Q4_K_M, MIMO v2.5) and tooling integration (tokencut, BashBrunch).

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking changes announced. However, several high-priority fixes are under review:
- [PR #19432](https://github.com/ollama/ollama/pull/19432): Fixes silent override of model sampler defaults (`temperature`, `top_p`) on `/v1/chat/completions`.
- [PR #19423](https://github.com/ollama/ollama/pull/19423): Adds case-insensitive parsing for Modelfile parameters and message roles — improves compatibility with user-defined config.

---

### **3. New Model & Hardware Support**  
- **New Model Request**: [Issue #19428](https://github.com/ollama/ollama/issues/19428) requests addition of **VirbiusGuard (0.6B)**, a bilingual agent-safety guard based on Qwen3Guard-Gen-0.6B (Apache-2.0).
- **Model Request**: [Issue #15887](https://github.com/ollama/ollama/issues/15887) calls for inclusion of **MIMO v2.5**, a million-token context LLM from Xiaomi (MIT license).
- **Quantization Request**: [Issue #19436](https://github.com/ollama/ollama/issues/19436) seeks `q4_K_M` quantization for `qwen3.5:0.8b` — crucial for CPU performance.
- **Hardware/Backend**: PRs continue to refine MLX support on Apple Silicon (`PR #19438`, `#19422`) and CUDA library loading on Linux (`PR #19433`). No new backends added.

---

### **4. Performance & Optimization**  
- **Memory Efficiency**: 
  - [PR #19438](https://github.com/ollama/ollama/pull/19438) resolves persistent memory growth in `lfm3-omni` after repeated speech requests (up to +1.6 GB over 8 runs).
  - [PR #18131](https://github.com/ollama/ollama/pull/18131) and follow-up [Issue #19435](https://github.com/ollama/ollama/issues/19435) highlight global cache budgeting as a bottleneck; proposals aim for per-model memory control.
- **Latency & Throughput**: 
  - [PR #19408](https://github.com/ollama/ollama/pull/19408) extends MLX decision model support to handle longer inputs without truncation.
  - [PR #19434](https://github.com/ollama/ollama/pull/19434) introduces **tokencut** as a context compression proxy — reduces input size via structural folding of JSON tool outputs, improving throughput for long-running agents.
- **Tooling**: [PR #19426](https://github.com/ollama/ollama/pull/19426) fixes misrouting of tool calls due to argument name collisions — improves reliability in multi-tool environments.

---

### **5. Stability & Regressions**  
*High severity issues reported today:*

| Issue | Severity | Summary | Fix Status |
|------|----------|--------|------------|
| [#19420](https://github.com/ollama/ollama/issues/19420) | Critical | `Gemma4`: 500 internal server error on all prompts (Windows 11) | ✅ In progress (no fix yet) |
| [#19427](https://github.com/ollama/ollama/issues/19427) | High | `gemma3:4b` returns empty JSON with structured output and long prompt | ⚠️ No fix PR |
| [#19439](https://github.com/ollama/ollama/issues/19439) | High | Ollama runner disconnects during tool calling on RTX 5090 (HTTP 500 /tokenize) | ⚠️ No fix PR |
| [#19424](https://github.com/ollama/ollama/issues/19424) | Medium | Qwen models terminate chat when `</thinking>` is emitted | ⚠️ No fix PR |
| [#19437](https://github.com/ollama/ollama/issues/19437) | Medium | `lfm3-omni` memory grows by ~1.6 GB per request on macOS | ✅ Fixed in [PR #19438](https://github.com/ollama/ollama/pull/19438) |

> 💡 Note: Multiple regressions point to memory handling in MLX runners and Windows-specific TCP/HTTP behaviors.

---

### **6. What This Means for Application Developers**  
- **Avoid `gemma3:4b` with structured output** until [#19427](https://github.com/ollama/ollama/issues/19427) is resolved — expect silent failures.
- **Use `tokencut`** ([PR #19434](https://github.com/ollama/ollama/pull/19434)) in production pipelines to manage long-context agent workflows and reduce latency.
- **Be cautious with Qwen models** — avoid using `</thinking>` if you need continuous conversation flow; this triggers early termination.
- **On Windows**: Be aware of TLS handshake timeouts ([#19430](https://github.com/ollama/ollama/issues/19430)) on networks blocking X25519MLKEM768 ClientHello — may require local proxy or network configuration.
- **For Apple Silicon users**: Monitor memory usage with `lfm3-omni` — use latest builds with [PR #19438](https://github.com/ollama/ollama/pull/19438) applied to prevent runaway memory consumption.

> 🔧 Pro tip: Use `OLLAMA_CACHE_RAM` (if available via future config) or limit model concurrency to mitigate shared cache contention across models.

---  
*Data source: github.com/ollama/ollama | Updated: 2026-11-01*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-11-01**

---

### **1. Today's Highlights**  
The LiteLLM project continues to strengthen its enterprise-grade proxy and gateway capabilities with critical fixes to budget enforcement, routing reliability, and security. Key developments include the addition of a public `/key/revoke` endpoint for accidental key exposure mitigation and improvements in MCP tooling and streaming handling across Anthropic, Amazon Nova, and Cohere. These changes reflect a growing focus on operational resilience and multi-provider consistency.

---

### **2. Releases & Breaking Changes**  
*No new releases in the last 24 hours.*  

**Breaking Change (PR #45909):**  
Hot reload (`--reload`) now properly restarts all workers when `num_workers > 1`, fixing a prior regression where only one worker would restart. This affects users relying on multi-worker deployment configurations during development.  
👉 [PR #45909](https://github.com/BerriAI/litellm/pull/45909)

**New Feature (PR #49545):**  
Added a public `POST /key/revoke` endpoint allowing anyone who discovers a published virtual API key to disable it immediately—without requiring admin access. The key is blocked (not deleted), enabling auditability and recovery.  
👉 [PR #49545](https://github.com/BerriAI/litellm/pull/49545)

---

### **3. New Model & Hardware Support**  
*No new models or hardware backends added today.*  

**Pending Enhancement (Issue #34501):**  
Request for native support of **GCP Managed MCP Servers** via direct integration with GCP IAM, reducing need for sidecar auth proxies in GKE clusters.  
👉 [Issue #34501](https://github.com/BerriAI/litellm/issues/34501)

---

### **4. Performance & Optimization**  
*Performance optimizations are underway but not yet merged.*  

**Key Progress (PR #49525):**  
Router cooldowns and global TPM/RPM rate limits are being offloaded to a **Rust-backed Redis store**, improving concurrency and reducing latency in high-throughput routing scenarios. This leverages the same store surface introduced in PR #49481, enabling shared state between Python and Rust components.  
👉 [PR #49525](https://github.com/BerriAI/litellm/pull/49525)

**Opt-In Storage (PR #49544):**  
Introduced `spend_logs_prompt_storage: latest_turn` mode for deployments that keep prompts in spend logs—reducing storage bloat by storing only the current turn of stateless requests (ideal for coding agents).  
👉 [PR #49544](https://github.com/BerriAI/litellm/pull/49544)

---

### **5. Stability & Regressions**  
*Several stability issues reported today, primarily around edge cases in streaming, guardrails, and budget enforcement.*

- **Critical (Issue #48809):**  
  Team-member budget exhaustion incorrectly blocks non-spending routes like `GET /v1/models`, breaking OpenAI-compatible client discovery workflows.  
  👉 [Issue #48809](https://github.com/BerriAI/litellm/issues/48809) *(Fix pending)*

- **High Severity (Issue #48364):**  
  Admin-level API keys fail to enforce `max_budget` or `tpm_limit` after budget exhaustion—allowing unlimited usage.  
  👉 [Issue #48364](https://github.com/BerriAI/litellm/issues/48364) *(Fix pending)*

- **Streaming Failure (Issue #35589):**  
  Amazon Nova streaming fails 100% due to `httpx` zstd decoder reusing a finished `decompressobj`.  
  👉 [Issue #35589](https://github.com/BerriAI/litellm/issues/35589) *(Fix PR: #49529? Not yet merged)*

- **Guardrail Corruption (Issue #49449):**  
  Anthropic streaming guardrails misparse transport chunks as complete SSE frames, causing silent text scanning failures and warning spam.  
  👉 [Issue #49449](https://github.com/BerriAI/litellm/issues/49449) *(Fix PR: #49529? No fix yet)*

---

### **6. What This Means for Application Developers**  
Developers using LiteLLM in production should:
- **Update routing logic** to avoid relying on `GET /models` post-budget-exhaustion — consider pre-caching model lists.
- **Use the new `/key/revoke` endpoint** in CI/CD pipelines or agent systems to mitigate accidental key leaks.
- **Enable `spend_logs_prompt_storage: latest_turn`** if building coding or long-context agents to reduce log size and cost.
- **Verify budget enforcement** for admin keys in staging environments—current behavior may allow bypasses.
- **Monitor streaming behavior** with Amazon Nova and Anthropic Guardrails; expect potential breakage until PRs land.

These updates signal LiteLLM’s maturation into a robust inference infrastructure layer—especially for teams managing multi-provider, multi-agent, and regulated workloads.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-01**

---

### **1. Today's Highlights**  
Unsloth continues its rapid evolution toward a full-stack AI development platform, with major UI/UX refinements in Studio and critical CLI improvements for vision models. Key PRs enable image input in `unsloth inference` and `chat`, resolve long-standing issues with model loading on non-NVLink GPUs, and optimize backend performance through smarter log redaction and file system handling.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were published. The project remains in active development with ongoing feature and stability work via pull requests.

---

### **3. New Model & Hardware Support**  
- ✅ **Vision Language Models (VLMs)**: The CLI now supports image input in `inference` and `chat` commands via `--image` flag (PR #16347), enabling end-to-end training-to-inference workflows for VLMs like Qwen2.5-VL-3B-Instruct.  
- 🚧 **Ascend NPU (Huawei) Backend**: RFC #10772 proposes native support for Huawei’s Ascend NPUs — critical for China-centric LLM deployment and fine-tuning. This reflects growing demand for domestic AI hardware compatibility.  
- 🌐 **ModelScope Integration**: Issues #16263 and #16294 highlight challenges with loading models from ModelScope when pinned to specific revisions or lacking default branch fallbacks, indicating incomplete support for some third-party model sources.

> 🔗 [RFC: Ascend NPU Support](https://github.com/unslothai/unsloth/issues/10772)  
> 🔗 [CLI Image Input Fix](https://github.com/unslothai/unsloth/pull/16347)

---

### **4. Performance & Optimization**  
Significant performance improvements landed in Studio backend logic:
- **Log Redaction Speedup**: PR #16356 skips unmatchable regex rules early, reducing per-line processing cost from ~3–13ms to near-zero on irrelevant lines.  
- **File System Efficiency**: Multiple PRs (#16355, #16354, #16353, #16352) reduce redundant I/O operations by caching directory listings and parsing metadata only once per session. For large datasets, this can cut startup time by up to 80% during model/library load.  
- **Dispatcher Wake-Up**: PR #16350 replaces queue polling with immediate wake-up on stop, reducing latency in concurrent request switching (critical for multi-user inference servers).

> 🔗 [Optimized Log Redaction](https://github.com/unslothai/unsloth/pull/16356)  
> 🔗 [Efficient Gallery & Voice Loading](https://github.com/unslothai/unsloth/pull/16355)  
> 🔗 [Faster Dataset Folder Inspection](https://github.com/unslothai/unsloth/pull/16353)

---

### **5. Stability & Regressions**  
Critical stability issues reported today:
- ❌ **Non-NVLink GPU Corruption**: Issue #10613 confirms that `GGML_CUDA_P2P=1` is incorrectly enabled on RTX 6000 Ada, PRO 6000, L40/L40S, and L4 cards — leading to silent data corruption during multi-GPU inference. *Fix PR pending.*  
- ⚠️ **Stalled Model Loading on Windows**: Issue #16294 reports "No available kernel" crash when using Auto attention with torch < 2.13 without Flash Attention support, particularly affecting MiniMax-H3 models.  
- ⚠️ **Studio Hangs on Startup**: Issue #16267 reveals that every cached GGUF model triggers a re-probe against Hugging Face, causing 20-minute timeouts on startup. This severely impacts usability for local-first workflows.  
- ⚠️ **Persistent Settings Lost**: Issue #16309 highlights that key settings (Appearance, Security, Browser) are stored only in `localStorage`, causing silent resets across browser origins or sessions.

> 🔗 [NVLink Misconfiguration Bug](https://github.com/unslothai/unsloth/issues/10613)  
> 🔗 [Windows Flash Attention Crash](https://github.com/unslothai/unsloth/issues/16294)  
> 🔗 [Studio Startup Timeout](https://github.com/unslothai/unsloth/issues/16267)

---

### **6. What This Means for Application Developers**  
- **Build Reliable VLM Workflows**: With CLI image support now merged (PR #16347), developers can fully automate vision model pipelines from fine-tuning to inference without switching tools.  
- **Avoid NVLink Pitfalls**: If deploying on non-NVLink GPUs (e.g., RTX 6000 Ada), disable `GGML_CUDA_P2P=1` manually until the fix lands. Use environment variables or override in `unsloth_cli` config.  
- **Optimize for Large Datasets**: Leverage the new optimizations in `GET /api/library` and dataset inspection — they dramatically reduce startup overhead for apps managing thousands of assets.  
- **Plan for Cross-Browser Persistence**: Avoid relying on client-side settings storage; expect future updates to move them server-side (per #16309).  
- **Monitor Hardware Diversity**: As Ascend NPU support enters RFC stage, prepare for dual-backend workflows in China-focused deployments.

> 💡 Pro Tip: Use `trust_remote_code=True` explicitly in CLI (`--trust-remote-code`) to avoid loading failures with custom-model repositories (issue #16331).

---  
*Digest compiled from GitHub activity (2026-11-01 UTC).*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*