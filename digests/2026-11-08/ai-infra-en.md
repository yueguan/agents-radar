# AI Infrastructure Digest 2026-11-08

> Generated: 2026-11-08 01:46 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-08**

---

### **1. Ecosystem Overview**  
The AI inference and serving ecosystem is entering a phase of high-performance specialization, driven by next-generation hardware (NVIDIA Blackwell, Apple Silicon, Intel Arc) and complex model architectures (MoE, Mamba2, diffusion). Projects are diverging in focus: vLLM and SGLang push the envelope on throughput and speculative decoding for LLMs; llama.cpp consolidates low-level portability across heterogeneous backends; Ollama and LiteLLM act as accessible gateways for developers; while Unsloth targets advanced fine-tuning and multimodal workflows. Stability remains a critical concern, especially on new hardware, with multiple high-severity regressions reported across projects.

---

### **2. Activity Comparison**

| Project       | Issues Open (Today) | PRs Merged (Today) | Release Status         |
|---------------|---------------------|--------------------|------------------------|
| **vLLM**      | 7                   | 5                  | v0.33.0 under regression; recommend v0.32.0 for production |
| **SGLang**    | 9                   | 6                  | No new release; stabilization phase ahead of tuning campaigns |
| **llama.cpp** | 6                   | 4                  | b12197–b12196 released; minor updates, no major version bump |
| **Ollama**    | 8                   | 3                  | No new release; breaking changes in review (e.g., Gemma 4 rendering) |
| **LiteLLM**   | 5                   | 4                  | v1.110.0-dev.1 (dev), v1.108.3 LTS stable; recommended for prod |
| **Unsloth**   | 5                   | 4                  | No release; stable with no breaking changes |

> ✅ *Trend*: vLLM and SGLang show highest engineering velocity, reflecting active optimization for cutting-edge inference workloads. LiteLLM leads in stability and release hygiene with LTS support.

---

### **3. Model Support Race**

| New Model / Architecture        | Supported By                          | Notes |
|----------------------------------|----------------------------------------|-------|
| **NVIDIA Nemotron 3.5 ASR Streaming (0.6b)** | ✅ vLLM (`#47455`) | First project to ship real-time speech support |
| **Qwen3.5-122B-A10B + MTP speculative decode** | ✅ vLLM (`PR #52244`) | Hybrid GDN prefix-cache hits enabled |
| **Mamba2 ReplaySSM speculative decode (Triton backend)** | ✅ vLLM (`#66318`), SGLang (`#49110`) | End-to-end support now available |
| **MageVL-Cowork (multimodal image+text)** | ✅ llama.cpp (`#31819`) | Full `mtmd` integration |
| **Ripple model (NoPE, layer fusion)** | ✅ Ollama (`PR #19592`) | Experimental but well-integrated |
| **Gemma 4 tool call fixes (content order, tags)** | ✅ Ollama (`#19594`, `#19574`) | Critical agent workflow stability |
| **DeepSeek-V4.1-Flash (Blackwell)** | ⚠️ SGLang & vLLM (optimization underway) | High-throughput performance still unstable on SM120 |
| **Apple Silicon MLX runner (CPU + Windows)** | ✅ Ollama (`#19366`) | Enables non-Metal deployment — key differentiator |

> 🏆 **Winner**: **Ollama** leads in *developer-facing* model accessibility and *cross-platform* support.  
> 🚀 **Leader in cutting-edge model/feature adoption**: **vLLM** and **SGLang** are fastest at integrating new architectures like Mamba2 and MoE on Blackwell.

---

### **4. Performance Frontier**

| Optimization Focus           | Primary Projects                     | Key Developments |
|-------------------------------|--------------------------------------|------------------|
| **KV Cache Efficiency**       | vLLM, SGLang                         | Triton-backed Mamba2 speculative decode; persistent_topk on ROCm |
| **Batching & Parallelism**    | SGLang, vLLM                         | Multi-node PCP/PP; AgentX S19 tuning (+35.6% output throughput) |
| **Quantization & Memory**     | vLLM, llama.cpp                      | Humming kernel migration (vLLM); `get_weight_byte_size()` for MFU; SYCL memory safety |
| **Kernel-Level Fusion**       | SGLang, llama.cpp                    | RoPE reduction (75%), fused residuals (Anima), SDPA fusions (Alchemist) |
| **Distributed Serving**       | vLLM, SGLang                         | Multi-rank engine sync issues (`#62916`, `#41449`) highlight ongoing challenges |
| **Long Context Throughput**   | SGLang, vLLM                         | GLM-5.3-Flash + AgentX: +26.3% total throughput on B300 |

> 🔥 **Hotspot**: Kernel fusion and attention optimizations dominate activity—especially in SGLang and vLLM. The race is on for sub-millisecond latency per token on long-context prompts.

---

### **5. Layer Positioning**

| Project       | Core Layer               | Role in Stack                              | Differentiator |
|---------------|--------------------------|--------------------------------------------|----------------|
| **vLLM**      | Serving Engine           | High-throughput, low-latency inference on NVIDIA | Optimized for SM120/MoE; Triton/TensorRT backend focus |
| **SGLang**    | Serving Engine + Gateway | Unified inference orchestration with multi-modal support | Strong in diffusion, long context, DFLASH MLA |
| **llama.cpp** | Local Runtime            | CPU/SYCL/Vulkan/WebGPU execution; portable inference | Best-in-class cross-platform support; lightweight |
| **Ollama**    | Gateway / Developer UX   | Simplified local deployment and CLI experience | MLX runner expansion, concurrent generation, tool calling fixes |
| **LiteLLM**   | LLM Gateway / Abstraction | Unified API for multi-provider routing and observability | Security hardening, budget tracking, OpenTelemetry |
| **Unsloth**   | Training/Fine-Tuning     | Fast training, RAG, diffusion pipelines      | `.safetensors`-only loading, Ulysses CP, VAE VRAM optimization |

> 📊 **Strategic Insight**: The stack is bifurcating—**engine-focused** (vLLM/SGLang) vs. **developer-friendly abstraction** (Ollama/LiteLLM) vs. **specialized runtime** (llama.cpp) vs. **training pipeline** (Unsloth).

---

### **6. Trend Signals**

1. **Hardware-Specific Instability Is the New Normal**:  
   Multiple critical bugs on **NVIDIA Blackwell (SM120)** (vLLM, SGLang) and **Intel Arc B-series** (llama.cpp) indicate that driver/compiler/backend maturity lags behind silicon advances. **Action**: Avoid bleeding-edge hardware in production until patches land.

2. **Speculative Decoding Is Now Mainstream**:  
   Mamba2 support via Triton backend (vLLM/SGLang), hybrid GDN caching (vLLM), and DFLASH MLA draft serving (SGLang) signal that speculative decoding is no longer experimental—it’s a core performance lever.

3. **Security & Predictability Are Priorities**:  
   - **Unsloth** adopting `.safetensors`-only loading  
   - **LiteLLM** enforcing type safety and secure Docker signing  
   - **llama.cpp** fixing deterministic SYCL operations  
   → **Shift from "fast" to "reliable and reproducible"**.

4. **Agent Workflows Are Driving Fixes**:  
   Tool calling crashes (Ollama), message ordering bugs (Ollama), silent worker kills (SGLang), and parsing errors (LiteLLM) all stem from agent use cases. **Agents are now the primary stress test** for infrastructure.

5. **Open Source Is Becoming Production-Grade**:  
   With LTS releases (LiteLLM), security scanning (cosign), and detailed error logging (LiteLLM, vLLM), open-source inference stacks are maturing rapidly—no longer just for prototyping.

> ✅ **Developer Action Plan**:  
> - Use **v0.32.0** for vLLM on Blackwell until `#65139` is fixed.  
> - Leverage **Ollama’s `OLLAMA_NUM_PARALLEL`** and **SGLang’s AgentX tuning** for scalable agent systems.  
> - Adopt **`.safetensors`** and **`lts` tags** for production reliability.  
> - Monitor **long-context memory usage** and **CUDA graph behavior** closely—especially on GB10 and Arc GPUs.

---

*Report compiled: 2026-11-08 | Sources: GitHub activity across vLLM, SGLang, llama.cpp, Ollama, LiteLLM, Unsloth*

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-11-08**

#### **1. Today’s Highlights**  
The vLLM project continues to accelerate its support for NVIDIA Blackwell (SM120) and ROCm, with critical performance and correctness fixes targeting high-end inference workloads. Key developments include progress on speculative decoding compatibility for Mamba2 via Triton backend support, a major shift toward replacing Marlin with the more modern Humming kernel across NVIDIA architectures, and urgent bug fixes for NVFP4 MoE models on SM120.

#### **2. Releases & Breaking Changes**  
*None in the last 24 hours.*  
However, **v0.33.0** remains under scrutiny due to a regression in `b12x` attention causing CUDA illegal memory access during MTP speculative decode (`#65139`). Users on SM120 with NVFP4 MoE + fp8 KV + chunked prefill should consider downgrading to v0.32.0 until the fix lands.

#### **3. New Model & Hardware Support**  
- ✅ **NVIDIA Blackwell (SM120)**: Active development and testing on RTX PRO 6000 Blackwell Max-Q (8x96GB), with multiple issues reported around CUDA graph capture, MoE kernels, and attention kernels.
- ✅ **ROCm (gfx950, gfx1100)**: Ongoing improvements including persistent_topk support, decoder CUDA graph enablement for Whisper, and fallback logic for unsupported AITER FP8 MoE paths.
- 🚀 **New Model Support**:  
  - Added experimental support for **NVIDIA Nemotron 3.5 ASR Streaming (0.6b)** (`#47455`) — useful for multilingual real-time speech applications.  
  - **Qwen3.5-122B-A10B** now supports hybrid GDN prefix-cache hits under MTP speculative decoding (`PR #52244`).

#### **4. Performance & Optimization**  
- 🔥 **Triton Backend for Mamba2 ReplaySSM Speculative Decode**: PR `#66318` enables speculative decoding on both FlashInfer and Triton backends for Mamba2, enabling faster standard and speculative decode via input caching (`#47572`).
- ⚙️ **Humming Kernel Replacement**: PR `#65560` tracks the migration from legacy Marlin to Humming across all NVIDIA architectures (Hopper/Blackwell), which will unify quantization paths and improve performance on newer GPUs.
- 💡 **Batch-Invariant Mode Optimization**: Issue `#65569` highlights that `enable_batch_invariant_mode` routes Linear layers to Triton unnecessarily on Hopper/Blackwell, despite cuBLAS already being batch-invariant — potential perf gain pending resolution.
- 📊 **Quantization Metrics**: PR `#65143` adds `get_weight_byte_size()` to `QuantizationConfig`, improving MFU/MBU estimation accuracy for custom quantized models.

#### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|-------|-------------|------------|
| Critical | `#65139` | CUDA illegal memory access at first MTP verify step (v0.33.0 regression on SM120) | ❌ Unresolved; blocks stable deployment on Blackwell |
| High | `#56892` | Extremely low decode throughput on SM120 with DeepSeek-V4.1-Flash and `--enforce-eager` | ❌ Urgent; CUDA graphs unusable |
| High | `#51525` | NVFP4 MoE with `VLLM_CUTLASS` produces empty completions on SM120 | ❌ Silent failure; affects model output integrity |
| Medium | `#65592` | `"max_tokens": null` bypasses platform caps silently | ✅ Fixed in PR `#66333` (pending merge) |

> 🔗 [Issue #65139](https://github.com/vllm-project/vllm/issues/65139) | 🔗 [Issue #56892](https://github.com/vllm-project/vllm/issues/56892)

#### **6. What This Means for Application Developers**  
- If you’re deploying **DeepSeek-V4.1-Flash**, **Qwen3.5**, or **Nemotron-3.5-ASR** on **Blackwell GPUs (SM120)**, avoid `--enforce-eager` and use v0.32.0 until `#65139` and `#56892` are resolved.  
- Use **speculative decoding with Mamba2**? Enable `ReplaySSM` via Triton (`#66318`) — it’s now supported end-to-end.  
- For **quantized MoE models (NVFP4/MXFP8)**, expect instability on SM120 unless using `--moe-backend flashinfer`.  
- **Custom quantization** users should implement `get_weight_byte_size()` to ensure accurate MFU reporting (`#65143`).  
- **Multi-node deployments** with PCP/PP may fail due to engine ID sync issues (`#62916`) — monitor for multi-rank startup failures.  

> ✅ **Pro Tip**: Pin your vLLM version to `v0.32.0` for production stability on Blackwell until `#65139` is patched. Track `#65560` for future performance gains via Humming kernel adoption.

---  
*Digest compiled from GitHub activity: [vLLM Project](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-08**

---

### **1. Today's Highlights**  
The SGLang project continues to strengthen its multi-modal and long-context inference capabilities, with key performance improvements for diffusion models and DeepSeek-V4 on Blackwell GPUs. Critical stability fixes address CUDA coredumps (#26340), silent worker crashes during long prefill (>40k tokens), and a high-severity bug in speculative decoding that caused rank deadlocks under mixed request batches.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were introduced. The project remains focused on stabilization and optimization ahead of upcoming model-specific tuning campaigns.

---

### **3. New Model & Hardware Support**  
- **Blackwell (SM120/SM121) support**: Active optimization work for DeepSeek-V4 and GLM-5.3-Flash on 4× RTX PRO 6000 (GB10) and DGX Spark (GB10).  
- **Apple Silicon (MLX)**: RFC #32321 outlines a redesign for Torch-owned SRT path with exported MLX regions—key step toward native Apple Silicon serving.  
- **Multi-modal expansion**: PRs #49110 and #49107 enable DFLASH MLA draft serving on TRTLLM_MLA/CUTEDSL_MLA backends and fix streaming tool call handling for DeepSeek V3.1.  
- **Quantization**: Continued refinement of W4A8 MoE quantization (PR #38573) and MXFP4 on ROCm (PR #47908).

> 🔗 [RFC: Apple Silicon Serving Redesign](https://github.com/sgl-project/sglang/issues/32321)  
> 🔗 [DFLASH MLA Draft Support](https://github.com/sgl-project/sglang/pull/49110)

---

### **4. Performance & Optimization**  
- **Diffusion acceleration**:  
  - PR #48442 reduces RoPE kernel launches by 75% (from 57.6k → 7.2k) for HiDream-O1-Image via bit-exact reuse.  
  - PR #48394 fuses gated residuals in Anima, cutting 8,400 kernels per 50-step CFG request.  
  - PR #48122 caches MiniMax-H3 reference/keyframes per image, avoiding redundant encoding.  
- **Long-context throughput**:  
  - PR #49071 reports **+26.3% total throughput** and **+35.6% output throughput** on B300 TP4 for GLM-5.3-Flash with AgentX S19 at C64.  
- **Kernel-level optimizations**:  
  - PR #45891 moves packed prefill top-k to CUDA for DSA, reducing overhead on long contexts (GB300).  
  - PR #48895 fuses DCP8 indexer top-k for decode, improving efficiency in Kimi-K3.1 DCP.  
  - PR #45807 fuses BF16 prefill indexer score epilogue for DeepSeek-V4.1, eliminating unnecessary BF16 intermediates.

> 🔗 [B300 AgentX Tuning Results](https://github.com/sgl-project/sglang/pull/49071)  
> 🔗 [RoPE Kernel Reduction in HiDream-O1-Image](https://github.com/sgl-project/sglang/pull/48383)

---

### **5. Stability & Regressions**  
- **Critical (High Severity)**:  
  - #26340: **CUDA coredumps** from `pr-test.yml`—auto-collected across runs; impacts CI reliability and debugging. No fix yet.  
  - #41449: **Deadlock in DSpark + TP** when batching grammar-constrained requests with others—ranks spin in collective. *Fix PR pending*.  
  - #36941: **Silent worker kill** during long prefill (>40k tokens) due to unified memory exhaustion (no OOM trace). Affects GB10 systems.  
- **Medium Severity**:  
  - #48593: Full prefill CUDA graph capture fails for hybrid linear attention models (IndexError in `_replay_metadata`).  
  - #47734: sglang-kernel FA3 uses suboptimal MmaPV_is_RS=false for hdim=128 non-causal on H200 (1.5–3% slower than flash-attn).  
  - #33422: DeepSeek-V4 prefill throughput (~2–7K tok/s) lags behind vLLM/Marlin (~12.5K tok/s) on SM120—tuning guidance needed.

> 🔗 [CUDA Coredump Tracker](https://github.com/sgl-project/sglang/issues/26340)  
> 🔗 [Worker Crash on Long Prefill](https://github.com/sgl-project/sglang/issues/36941)

---

### **6. What This Means for Application Developers**  
- **Use caution with long-context prompts (>40k tokens)**: Monitor unified memory usage on GB10 systems; avoid full prefill CUDA graphs if using hybrid attention backends (e.g., `HybridLinearAttnBackend`).  
- **Leverage recent diffusion optimizations**: Enable image-prefix sharding (ERNIE-Image) and cache reuse (MiniMax-H3, Ref2VA) to reduce latency and GPU load.  
- **Avoid mixed request batching** with grammar constraints in DSpark + TP setups until #41449 is resolved.  
- **Expect improved throughput** with tuned configurations for GLM-5.3-Flash on B300 (AgentX) and DeepSeek-V4 on Blackwell—check PR #49071 for benchmarked settings.  
- **Monitor CI flakiness** (#42752): Flaky tests may affect merge confidence; use `/sglang-pr-babysit` workflows carefully.

> ✅ **Actionable Tip**: Use `--cuda-graph-config '{"prefill": {"backend": "full"}}'` only after verifying it doesn’t trigger #48593 on hybrid models.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp Digest – 2026-11-08**

---

### **1. Today's Highlights**  
The latest updates focus on robustness and performance across SYCL, Vulkan, and Hexagon backends, with critical fixes for deterministic behavior in `CPY` operations (SYCL), memory safety in device buffer allocation (SYCL), and improved support for Intel Arc GPUs via cooperative matrix handling. Notably, the `mtmd` module now fully respects image token limits and adds support for the MageVL-Cowork model, enhancing multimodal inference capabilities.

---

### **2. Releases & Breaking Changes**  
- **b12197**: Added automatic loading of `libhtp` on Windows without legacy environment variables (`FASTRPC_MANAGE_DSP_LIBRARY_PATH`, `GGML_HEXAGON_LIBHTP_DIR`) — eliminates manual setup for Hexagon devices.  
  🔗 [PR #31781](https://github.com/ggml-org/llama.cpp/pull/31781)  
- **b12196**: Fixed exact rounding in SYCL `CPY` conversions to Q4_1/Q5_1/Q8_0, resolving non-deterministic output mismatches with CPU reference.  
  🔗 [PR #31465](https://github.com/ggml-org/llama.cpp/pull/31465)  
- **b12195**: SYCL now refuses device buffers exceeding available VRAM, preventing GPU crashes due to overcommitment.  
  🔗 [PR #31285](https://github.com/ggml-org/llama.cpp/pull/31285)  

> ⚠️ **Migration Note**: Users relying on large `--ngl` values on SYCL may need to adjust `GGML_VK_SUBALLOCATION_BLOCK_SIZE` or limit `--split-mode` if encountering out-of-memory errors.

---

### **3. New Model & Hardware Support**  
- **Model Support**:  
  - Added full support for **MageVL-Cowork** model in `mtmd` (multimodal text+image).  
    🔗 [Issue #31819](https://github.com/ggml-org/llama.cpp/issues/31819)  
  - Improved compatibility with **Decision 2.0 GGUF** and **SystemOne API** (via feature request).  
    🔗 [Issue #30161](https://github.com/ggml-org/llama.cpp/issues/30161)  

- **Hardware & Backend**:  
  - Enhanced **Intel Arc B-series** Vulkan support: Fixes nondeterminism and output corruption under long-context prompts.  
    🔗 [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560)  
  - **Hexagon** now supports non-ASCII paths and long directory names; absolute skel path resolution added.  
    🔗 [PR #31781](https://github.com/ggml-org/llama.cpp/pull/31781)  
  - **WebGPU**: Implemented `group_norm` kernel, enabling better tensor ops on WebGPU platforms.  
    🔗 [PR #31490](https://github.com/ggml-org/llama.cpp/pull/31490)

---

### **4. Performance & Optimization**  
- **SYCL**:  
  - Fused SDPA enabled for **Alchemist (dg2_g10-g12)** at `head_dim=64`, unlocking optimized Flash Attention for Qwen2/Qwen2.5 families.  
    🔗 [PR #31574](https://github.com/ggml-org/llama.cpp/pull/31574)  
  - Added MKL-based flash attention for **Flint/gpt-oss**, achieving **1.87x speedup** at prompt shape (tg128).  
    🔗 [PR #31536](https://github.com/ggml-org/llama.cpp/pull/31536)  
  - Vectorized RMS Norm + fused ADD/RMS_NORM/MUL improves Qwen3.8-27B Q3_K_P inference by **+3.4% throughput** (tg128).  
    🔗 [PR #31753](https://github.com/ggml-org/llama.cpp/pull/31753)  

- **CPU**:  
  - Switched default `src1` type to F32 in CPU backend for `GGML_TYPE_F16`, reducing precision loss in matrix multiplications.  
    🔗 [PR #31854](https://github.com/ggml-org/llama.cpp/pull/31854)  

- **Memory**:  
  - `moe-cache`: Now supports `--moe-cache-cpu-share auto` to dynamically offload MoE misses to CPU, balancing load and reducing GPU pressure.  
    🔗 [PR #31286](https://github.com/ggml-org/llama.cpp/pull/31286)

---

### **5. Stability & Regressions**  
- **Critical**:  
  - **Vulkan**: Nondeterministic results on **Intel Arc Pro B70 (BMG G31)** with `KHR_coopmat` enabled — same input yields different outputs.  
    🔗 [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560) *(No fix PR yet)*  
  - **Vulkan**: Output corruption persists on **Arc B-series** even after disabling `GGML_VK_DISABLE_COOPMAT`.  
    🔗 [Issue #31828](https://github.com/ggml-org/llama.cpp/issues/31828) *(Workaround incomplete)*  

- **High Severity**:  
  - **Metal**: Draft-MTP allocates up to **~15 GB host memory** during 96k-token sessions (vs ~5 GB without speculative decoding).  
    🔗 [Issue #30174](https://github.com/ggml-org/llama.cpp/issues/30174) *(Fix pending)*  
  - **Server**: Hangs indefinitely when loading a ~19GB model split across two RPC backends.  
    🔗 [Issue #31847](https://github.com/ggml-org/llama.cpp/issues/31847) *(Regression in multi-backend setup)*  

- **Medium**:  
  - **CPU**: Q8_0 dot product sign error when operand is `-128` (non-VNNI systems).  
    🔗 [Issue #29351](https://github.com/ggml-org/llama.cpp/issues/29351) *(Fix PR: #31854)*

---

### **6. What This Means for Application Developers**  
- ✅ **Use `--moe-cache-cpu-share auto`** for scalable MoE models on constrained GPU memory.  
- 🛠 **Avoid `GGML_VK_SUBALLOCATION_BLOCK_SIZE=4 GiB`** unless explicitly needed — it’s a workaround for suballocation fragmentation (see [Issue #27734](https://github.com/ggml-org/llama.cpp/issues/27734)).  
- ⚠️ **Disable `KHRCOOPMAT` on Intel Arc B-series** until [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560) is resolved to avoid incorrect outputs.  
- 📦 **For web apps using WebGPU**: The new `group_norm` kernel enables more complete model execution pipelines.  
- 🔄 **For server deployments**: Be cautious with `cache_prompt=false` tasks — they can cause memory leaks if not properly cleared (see [PR #30188](https://github.com/ggml-org/llama.cpp/pull/30188)).  
- 💡 **Enable `--image-min/max-tokens`** only after confirming `mtmd` projectors are updated — older builds ignore these flags ([PR #31864](https://github.com/ggml-org/llama.cpp/pull/31864)).

> 📌 **Best Practice**: Always test model loading and inference stability across multiple hardware targets, especially when using speculative decoding or multi-backend setups.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-08**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to expand its support for advanced inference workflows, with key contributions in MLX runner enhancements and model parser fixes. Notably, a new PR introduces ripple preview support and concurrent generation for MLX models, while critical rendering and parsing issues affecting Gemma 4 and DeepSeek3 are being addressed. A high-severity bug on Windows RTX 5090 impacting tool calling has been reported, underscoring ongoing challenges with GPU driver compatibility.

---

### **2. Releases & Breaking Changes**  
*No new releases were published in the last 24 hours.*  
However, several breaking changes are under active review:  
- **PR #19594**: Fixes incorrect content ordering in Gemma 4 assistant messages when `content` and `tool_calls` coexist — a regression that could break agent logic relying on message structure. [Link](https://github.com/ollama/ollama/pull/19594)  
- **PR #19574**: Addresses stray `<channel|>` close tags leaking into output during Gemma 4 inference, which may corrupt structured outputs. [Link](https://github.com/ollama/ollama/pull/19574)

---

### **3. New Model & Hardware Support**  
- **MLX Runner Expansion (PR #19593, #19590, #19567)**:  
  - Adds **Windows support** and **CPU backend** for MLX runner via PR #19366. This enables full-text-to-speech inference (e.g., `phonon:en`) on non-Metal systems. [Link](https://github.com/ollama/ollama/pull/19366)  
  - Introduces **dense Nemotron and Luciole tokenizer support** in MLX, enabling direct loading of Safetensors checkpoints. [Link](https://github.com/ollama/ollama/pull/19567)  
- **Ripple Model Integration (PR #19592)**: Adds experimental **Ripple model**, renderer, and parser with NoPE (non-periodic embedding) support and layer fusion optimizations. [Link](https://github.com/ollama/ollama/pull/19592)  
- **Concurrent Generation (PR #19197)**: Enables multiple parallel generation requests via `OLLAMA_NUM_PARALLEL`, improving latency for multi-user workloads. Default remains 1; memory overhead scales per request. [Link](https://github.com/ollama/ollama/pull/19197)

---

### **4. Performance & Optimization**  
- **VRAM Estimation Improvements (PR #18866)**: Fixes four miscounts between `ollama ps` and `nvidia-smi` by accurately estimating KV cache usage from GGUF metadata. Critical for large-context models where over-allocation leads to context window truncation. [Link](https://github.com/ollama/ollama/pull/18866)  
- **Benchmark Tool Reliability (PR #19600)**: Ensures `cmd/bench` fails on incomplete runs and prevents file corruption by truncating reused output files — essential for automated performance testing pipelines. [Link](https://github.com/ollama/ollama/pull/19600)  
- **Memory Efficiency Gains**: MLX’s fused MLP activation, value blending, and attention gating reduce redundant computation and improve throughput per token.

---

### **5. Stability & Regressions**  
- **Critical**:  
  - **#19439 (Open)**: Windows RTX 5090 crashes during tool calling with HTTP 500 /tokenize errors. Reproduced with Muse Glimmer 30B and Q models via Open WebUI. High risk for production agents using tool calling. [Link](https://github.com/ollama/ollama/issues/19439)  
  - **#17748 (Open)**: AMD Radeon 780M Vulkan backend fails on larger models post-0.32.10 due to `ErrorDeviceLost`. Confirmed regression on Linux. [Link](https://github.com/ollama/ollama/issues/17748)  

- **High Priority**:  
  - **#19596 (Open)**: Typo in `gemma4:12b-it-qat` Modelfile (`toos` instead of `tools`) breaks tool call parsing. Immediate fix needed for official model usability. [Link](https://github.com/ollama/ollama/issues/19596)  
  - **#17638 (Open)**: `gpt-oss:20b` rejects its own generated tool calls with "error parsing tool call" — indicates internal serialization flaw. [Link](https://github.com/ollama/ollama/issues/17638)  

- **Moderate**:  
  - **#18825 (Closed)**: `embeddinggemma-2:740m` fails on Linux due to missing MLX runtime — documentation gap. [Link](https://github.com/ollama/ollama/issues/18825)  
  - **#19597 (Open)**: Cloud `/api/chat` returns 400 on payloads >32 MiB (undocumented limit), disrupting image-heavy chat apps. [Link](https://github.com/ollama/ollama/issues/19597)

---

### **6. What This Means for Application Developers**  
- **Build robust agents with tool calling**: Avoid `gemma4:12b-it-qat` until the `toos` typo is fixed (#19596). Use alternative models or patch the Modelfile locally.  
- **Deploy on heterogeneous hardware**: The MLX CPU+Windows support (PR #19366) enables broader deployment across desktops and laptops, including Apple Silicon and Intel GPUs.  
- **Optimize for concurrency**: Leverage `OLLAMA_NUM_PARALLEL` (PR #19197) to reduce first-token latency in multi-user applications, but monitor VRAM usage carefully.  
- **Handle large inputs cautiously**: Be aware of undocumented 32 MiB limits in Ollama Cloud (`/api/chat`) — consider chunking or preprocessing base64 images.  
- **Monitor stability on newer GPUs**: Avoid RTX 5090 and AMD 780M on current stable versions until regressions are patched. Use `--no-mlx` or fallback backends if needed.  

> *Recommendation*: Pin to v0.40.13 or earlier for production stability until #19439 and #17748 are resolved. Track PRs #19594 and #19574 for immediate fixes in message rendering.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

---

### **1. Today's Highlights**  
The LiteLLM ecosystem continues to strengthen its support for Anthropic’s advanced reasoning and tooling workflows, with critical fixes addressing streaming reliability, budget tracking, and model-specific translation inconsistencies. A key focus on security and observability is evident in new CI checks for UI localization, enhanced OpenTelemetry span flushing, and improved logging of stream failures.

---

### **2. Releases & Breaking Changes**  
- **v1.110.0-dev.1**: Introduced with verified Docker image signing via [cosign](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065). This release includes pre-production improvements for security and stability, but is not recommended for production use.  
  🔗 [GitHub Release v1.110.0-dev.1](https://github.com/BerriAI/litellm/releases/tag/v1.110.0-dev.1)  
- **v1.108.3 (LTS)**: Now the latest stable LTS release. Users should pull `ghcr.io/berriai/litellm:lts` to ensure long-term support and patch updates.  
  🔗 [GitHub Release v1.108.3](https://github.com/BerriAI/litellm/releases/tag/v1.108.3)

> ✅ **Migration Note**: If using `lts` tag, no action needed—image will auto-update to latest patch. For production systems, avoid `dev` releases.

---

### **3. New Model & Hardware Support**  
No new models or hardware backends were added in the past 24h. However, ongoing work includes:
- **Azure AI Foundry Agents v2** support via `agent_reference` (tracked in #25372).
- **Vertex AI TTS** now respects `vertex_location` in endpoint construction (fix in progress, see #50219).

> 🔧 *Note*: Bedrock Converse now supports direct S3 object URLs (via PR #50577), enabling efficient ingestion of images, documents, and videos without prior download.

---

### **4. Performance & Optimization**  
- **Streaming Reliability**: PR #50597 introduces better error logging by capturing mapped exception classes during stream failures (e.g., `AnthropicError` → `MidStreamFallbackError`).  
  🔗 [PR #50597](https://github.com/BerriAI/litellm/pull/50597)
- **Timeout Control**: PR #50496 adds `total_timeout` as an opt-in bound for non-streaming requests, allowing finer-grained control over end-to-end latency without breaking existing behavior.  
  🔗 [PR #50496](https://github.com/BerriAI/litellm/pull/50496)
- **Type Safety & Build Speed**: PR #50579 eliminates 1,160 `basedpyright Any` errors across 182 files, improving type-checking robustness and developer tooling performance.  
  🔗 [PR #50579](https://github.com/BerriAI/litellm/pull/50579)

---

### **5. Stability & Regressions**  
Top regressions reported today (ranked by severity):

| Issue | Description | Fix Status | GitHub Link |
|------|-------------|------------|-------------|
| [#24498](https://github.com/BerriAI/litellm/issues/24498) | Empty system message sanitization returns `[System: Empty message content sanitised...]` for Claude models | Closed | [Issue #24498](https://github.com/BerriAI/litellm/issues/24498) |
| [#49179](https://github.com/BerriAI/litellm/issues/49179) | `/key/update` during in-flight request drops cost from `BudgetWindowSpend`, causing incorrect spending tracking | Closed | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) |
| [#49969](https://github.com/BerriAI/litellm/issues/49969) | Replayed `reasoningText.signature` sent to non-Anthropic Bedrock models (e.g., GLM 5.3) causes Bad Request | Closed | [Issue #49969](https://github.com/BerriAI/litellm/issues/49969) |
| [#50065](https://github.com/BerriAI/litellm/issues/50065) | `tools/list` hangs 18–50s after `notifications/initialized` on legacy clients post-v1.108.0 | Open | [Issue #50065](https://github.com/BerriAI/litellm/issues/50065) |
| [#49998](https://github.com/BerriAI/litellm/issues/49998) | Upstream failure on `/v1/messages` returns 503 “auth DB unreachable” instead of 500 | Closed | [Issue #49998](https://github.com/BerriAI/litellm/issues/49998) |

> ⚠️ **Critical**: The `budget_limits` race condition (#49179) could lead to inaccurate billing if keys are updated mid-request — recommend upgrading to v1.108.3 or later.

---

### **6. What This Means for Application Developers**  
- **Use `lts` tag** (`ghcr.io/berriai/litellm:lts`) for production systems to benefit from consistent, tested patches.
- **Avoid `dev` builds** unless actively testing new features; they may contain unverified changes.
- **Handle reasoning blocks carefully** when routing between Claude and non-Anthropic models (e.g., GLM, Mistral) — signature mismatches can cause rejection (see #49969).
- **Monitor `/v1/messages` response consistency**, especially when using tool loops or agent frameworks — recent fixes address misrouting and state corruption (e.g., #48384, #50070).
- **Enable `total_timeout`** in high-load environments to prevent runaway requests and improve SLA adherence.
- **Validate input types** for `extra_headers` and `tool_choice` — malformed inputs now return 400 instead of crashing (PR #50581).

> 🛠️ **Pro Tip**: Use PR #50593 to enforce localization best practices in dashboard UIs — avoid hardcoding English strings in JSX.

---  
*Digest generated: 2026-11-08 | Source: [BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-08**

---

### **1. Today's Highlights**  
The latest activity centers on critical performance and stability improvements in diffusion, RAG, and context parallelism workflows. Key PRs include optimized VAE memory usage in Studio’s LTX video generation pipeline and fixes for GPU memory divergence during MoE training on CPU backends. A major UI/UX enhancement enables `.safetensors`-only weight loading across all model types, improving security and consistency.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were published. The project remains stable with no migration requirements.

---

### **3. New Model & Hardware Support**  
- **New**: Added support for `.safetensors`-only loading of image, video, and text-encoder weights via [PR #17061](https://github.com/unslothai/unsloth/pull/17061). This eliminates reliance on legacy pickle formats (`.pt`, `.pth`) and enhances security.
- **Hardware**: Expanded Ulysses context parallelism support to **Gemma 4**, **DeepSeek V2/V3 MLA**, and **V4 sink attention** models via [PR #15883](https://github.com/unslothai/unsloth/pull/15883) and [PR #15882](https://github.com/unslothai/unsloth/pull/15882).
- **Backend**: Added Intel OneAPI environment sourcing option (`setvars.sh`) for SYCL-enabled llama.cpp builds ([PR #17048](https://github.com/unslothai/unsloth/pull/17048)).

---

### **4. Performance & Optimization**  
- **RAG Ingestion**: Identified a 99ms/chunk latency overhead due to per-document embedding processing; batched embeddings now improve throughput by ~48x ([Issue #16715](https://github.com/unslothai/unsloth/issues/16715)).
- **Diffusion VAE**: Reduced VRAM overhead in LTX-2/2.5 tiled decoding by keeping only one open tile band active per GPU card ([PR #17060](https://github.com/unslothai/unsloth/pull/17060)). Saves up to **2.3 GiB VRAM** on SDXL.
- **MoE Training**: Fixed divergent router auxiliary loss on CPU backend when gradient accumulation > 1 ([Issue #17054](https://github.com/unslothai/unsloth/issues/17054)), restoring training stability.
- **Context Parallelism**: Eval loss is now correctly normalized per sequence, not multiplied by `cp_size` ([PR #17049](https://github.com/unslothai/unsloth/pull/17049)).

---

### **5. Stability & Regressions**  
- **Critical**: Image generation falls back to CPU/RAM on Windows 11 with RTX 3060 despite GPU availability ([Issue #16803](https://github.com/unslothai/unsloth/issues/16803)). *Fix pending.*
- **High Severity**: Search tool missing from UI (repeated issue, third occurrence) — impacts agent workflow reliability ([Issue #16706](https://github.com/unslothai/unsloth/issues/16706)).
- **Medium**: Studio crashes on LAN bind attempts with IP address override ([Issue #17017](https://github.com/unslothai/unsloth/issues/17017)).
- **UI Bugs**: Model Hub rendering issues (overlapping content, misplaced images) reported on desktop app ([Issue #17004](https://github.com/unslothai/unsloth/issues/17004)); search icon blurred ([Issue #16962](https://github.com/unslothai/unsloth/issues/16962)).
- **Fixes Merged**: Resolved nul file creation on Windows Git Bash ([PR #17010](https://github.com/unslothai/unsloth/pull/17010)) and improved conditional stream reuse in FLUX.3 modulation heads ([PR #17059](https://github.com/unslothai/unsloth/pull/17059)).

---

### **6. What This Means for Application Developers**  
Developers should **migrate to `.safetensors`-only weight loading** immediately—this is now the default path and improves security and predictability. For **multi-GPU inference pipelines**, ensure that MoE models are not run under `UNSLOTH_FORCE_CPU=1` with high gradient accumulation steps unless patched. When building **agent systems using RAG**, consider batching document ingestion at the corpus level to avoid per-chunk latency penalties.  

For **vision applications**, expect reduced VRAM pressure in video generation workflows thanks to tiled VAE optimizations. However, be cautious with **large video uploads (>64MB)**—a feature request exists to increase limit ([Issue #17062](https://github.com/unslothai/unsloth/issues/17062)), so plan for chunking in production. Finally, **avoid relying on the search tool** until the recurring bug is resolved, and use CLI/API alternatives where possible.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*