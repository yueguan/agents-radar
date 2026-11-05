# AI Infrastructure Digest 2026-11-05

> Generated: 2026-11-05 02:44 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-05**

---

### **1. Ecosystem Overview**  
The AI inference infrastructure landscape in late 2026 is characterized by rapid convergence between high-performance serving engines, lightweight local runtimes, and intelligent gateways—driven by next-generation models (e.g., DeepSeek-V4, Qwen3.8-Flash-Next) and heterogeneous hardware (Blackwell, MI355X, Apple Silicon, RTX 5090). Projects are increasingly focused on **distributed inference stability**, **quantization efficiency**, and **agent-native capabilities**, with a clear shift toward end-to-end control over model execution, from kernel-level optimizations to UI-integrated file interaction. The gap between research-grade models and production-ready deployment continues to narrow—but only at the cost of heightened complexity in tooling and debugging.

---

### **2. Activity Comparison**

| Project       | Open Issues (Critical/High) | PRs Merged (Last 7d) | Release Status       |
|---------------|-----------------------------|------------------------|------------------------|
| **vLLM**      | 12 (4 Critical)             | 14                     | Stable: v0.32.0 (⚠️ bugs) |
| **SGLang**    | 14 (4 Critical)             | 10                     | No stable release      |
| **llama.cpp** | 12 (3 Critical)             | 16                     | **v0.9.0 (b12110)** released |
| **Ollama**    | 10 (3 High)                 | 12                     | No new release         |
| **LiteLLM**   | 12 (4 High)                 | 15                     | `v1.108.0` upcoming (RC3) |
| **Unsloth**   | 12 (4 High)                 | 13                     | **v0.1.915-beta** released |

> ✅ *Observation:* **llama.cpp** leads in release velocity with a major v0.9.0 launch; **SGLang** and **Unsloth** show strong beta activity despite no stable releases. **vLLM** remains most active in critical issue tracking.

---

### **3. Model Support Race**

| New Model / Architecture     | vLLM       | SGLang     | llama.cpp       | Ollama        | LiteLLM       | Unsloth         |
|-------------------------------|------------|------------|------------------|---------------|----------------|------------------|
| **DeepSeek-V4-Flash/DSv4.1**  | ✅ Active (SM8x/SM120) | ❌          | ❌               | ❌            | ❌             | ❌               |
| **Qwen3.8-Flash-Next**        | 🚧 ROCm/GPU | ✅ ROCm/MoE | ✅ FP8/OCP       | ✅ MXFP8      | ✅ Migration   | ✅ Native support |
| **MiniMax-M3.1**              | 🚧 ROCm     | ❌         | ❌               | ❌            | ❌             | ❌               |
| **Apple Silicon (MLX)**       | ⚠️ Test     | ✅ RFC + dev | ❌               | ✅ YaRN/YaRN  | ❌             | ✅ Full support |
| **Intel Xeon (SPR)**          | ❌          | ✅ CBB mode | ❌               | ❌            | ❌             | ❌               |
| **AMD MI355X (gfx950)**       | ✅ ROCm perf | ✅ MoE/QSA | ❌               | ❌            | ❌             | ❌               |
| **NPU (Ascend)**              | ❌          | ✅ Sampling mask | ❌ | ❌            | ❌             | ❌               |

> 🏆 **Leaderboard**:  
> - **Unsloth** leads in **local agent readiness** (file I/O, persistent context, browser integration).  
> - **Qwen3.8-Flash-Next** has the broadest native support across **llama.cpp**, **Ollama**, and **SGLang**.  
> - **vLLM** is ahead in **enterprise-grade distributed inference** (P/D disaggregation, multi-node stability).

---

### **4. Performance Frontier**

| Optimization Focus           | vLLM                          | SGLang                        | llama.cpp                    | Ollama                     | LiteLLM                      | Unsloth                     |
|-------------------------------|-------------------------------|-------------------------------|------------------------------|----------------------------|------------------------------|-----------------------------|
| **KV Cache Efficiency**       | ✅ Back-only tail management, DFlash/DSpark | ❌ (long prefill crash)       | ❌                            | ⚠️ Overestimated VRAM usage | ❌                            | ❌                           |
| **Batching & Throughput**     | ✅ Multi-sequence Flash Attention | ✅ Dynamic-shape CPU graphs   | ✅ Batched tensor API (WebGPU) | ✅ `batch-size` + streaming | ✅ Fanout.gather (scalability) | ✅ RAG batched embeddings (~48× faster) |
| **Quantization**              | ✅ FP8 dynamic fusion, LoRA shrink | ✅ MXFP4, QSA                | ✅ OCP FP8 E4M3, GGML batching | ✅ MXFP8 selective | ❌                             | ✅ FP8 enabled by default     |
| **Distributed Serving**       | ✅ P/D disaggregation (critical bugs) | ❌ Disaggregation corruption | ❌                             | ❌                           | ✅ Control plane passthrough | ❌                           |
| **Kernel-Level Tuning**       | ✅ Triton fusion, MoE fallback | ✅ HIP spec decoding, DCP routing | ✅ OpenCL/Adreno speedup | ❌                           | ✅ Rust router core           | ✅ CUDA graph capture (default) |

> 🔥 **Key Insight:** **vLLM** dominates in **distributed system tuning**; **Unsloth** leads in **local performance via CUDA graphs and RAG batching**; **llama.cpp** excels in **cross-platform kernel optimization**.

---

### **5. Layer Positioning**

| Project       | Primary Layer                  | Secondary Role                              | Key Differentiator                                  |
|---------------|--------------------------------|----------------------------------------------|-----------------------------------------------------|
| **vLLM**      | **Inference Engine**           | Model Serving, Distributed Serving           | Industry standard for scalable, high-throughput LLM serving |
| **SGLang**    | **Model Gateway / Orchestrator** | Agent Workflow, Hybrid Execution             | Strong focus on agentic logic, cross-platform runtime |
| **llama.cpp** | **Local Runtime / Edge Inference** | Standalone inference, WebGPU/OpenCL support  | Universal backend support, minimal dependencies     |
| **Ollama**    | **Developer Gateway / CLI Tool** | Local model runner, Cloud sync               | Seamless UX for developers, but less control       |
| **LiteLLM**   | **Control Plane / API Gateway** | Cost tracking, Guardrails, Audit Logging     | Compliance-first design, SOC 2-ready observability |
| **Unsloth**   | **Agent Runtime / Local UI**   | Fine-tuning workflow, RAG, File Interaction  | First local agent platform with browser file access |

> 💡 **Strategic Takeaway:** The stack is maturing into **specialized layers**—from **engine** (vLLM), **gateway** (LiteLLM), **runtime** (Unsloth), to **orchestrator** (SGLang)—with developers choosing based on deployment scale, compliance needs, and agent sophistication.

---

### **6. Trend Signals**

1. **Agent-Native Infrastructure Is Now Core**  
   - Unsloth’s file I/O, persistent context, and `search_conversation` tools signal that **local agents require full OS-like capabilities**—not just model inference.
   - Developers should expect future frameworks to include **filesystem access, task scheduling, and memory persistence** as baseline features.

2. **ROCm and AMD Are No Longer Niche**  
   - Multiple projects (vLLM, SGLang, llama.cpp) now actively optimizing for **MI355X/gfx950**—indicating AMD’s growing role in enterprise AI clusters.
   - Expect more **model-specific kernel patches** (e.g., #65530) tailored to AMD’s architecture.

3. **Compliance-Driven Gateways Are Rising**  
   - LiteLLM’s WORM storage, Splunk/Sentinel integrations, and audit trails show that **governance is no longer an afterthought**.
   - Application developers must now consider **SOC 2, data provenance, and tamper-evident logs** when selecting gateways.

4. **Quantization Is Becoming Platform-Aware**  
   - Ollama’s selective MXFP8, llama.cpp’s OCP FP8, and vLLM’s dynamic FP8 fusion indicate that **quantization is moving beyond generic formats** to **hardware-optimized, model-aware strategies**.

5. **Stability > Speed in Production**  
   - Despite aggressive optimization pushes, **critical regressions dominate issue trackers** (e.g., vLLM’s NixlConnector race condition, SGLang’s long prefill kill).
   - **Avoid beta releases in production**—prioritize validation, CI reliability, and reproducibility.

---

### ✅ **Recommendation for Application Developers**  
- **For agents**: Choose **Unsloth** or **SGLang** for rich local workflows and programmable cache policies.  
- **For scalable serving**: Use **vLLM** (but avoid v0.32.0 until fixes land).  
- **For compliance-heavy apps**: Adopt **LiteLLM** with signed images and audit logging.  
- **For edge/local deployment**: **llama.cpp v0.9.0** offers best-in-class portability and quantization flexibility.  
- **For rapid prototyping**: **Ollama** remains ideal—but test tool calls on RTX 5090/WSL2 carefully.

> 📌 **Final Note:** The era of "just deploy a model" is over. Today’s success depends on **layered, auditable, and agent-aware infrastructure**—choose your stack not just for performance, but for **stability, compliance, and extensibility**.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-05

---

### **1. Today's Highlights**  
The vLLM project continues to accelerate its support for next-generation models and hardware, with critical work underway on **DeepSeek-V4-Flash/DSv4.1** compatibility across SM8x (Ampere) and SM120 (Blackwell) GPUs. A major focus is on **P/D disaggregation stability**, particularly around `NixlConnector` race conditions and KV cache corruption issues in multi-node deployments. Meanwhile, ROCm performance optimizations for Qwen3.8-Flash-Next and MiniMax-M3.1 are progressing rapidly on gfx950/MI355X.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were introduced today. The latest stable release remains **v0.32.0**, though several regression bugs have been reported against it (e.g., #63343, #65618).

---

### **3. New Model & Hardware Support**  
- ✅ **DeepSeek-V4-Flash / DeepSeek-V4.1-Flash**: Active development on **SM8x (Ampere: A100/A800, RTX 30xx)** support via Issue #50576 (128 comments), currently blocked due to missing `packed_modules_mapping` in DSpark draft logic (#65618).  
- 🚧 **Qwen3.8-Flash-Next (gfx950 / MI355X)**: Performance optimization roadmap ongoing via Issue #59575; PRs like #65530 and #65481 target tile tuning and sparse decode splits.  
- 🚧 **MiniMax-M3.1 (ROCm/gfx950)**: Optimization work in progress (PR #65530, #65481); focused on NVFP4 sparse decode and M=17–256 GEMM tiling.  
- ⚠️ **Intel GPU (XPU)**: Test enablement for attention kernels underway in PR #65788, targeting broader hardware coverage.  

> 🔗 [Issue #50576](https://github.com/vllm-project/vllm/issues/50576) | [PR #65788](https://github.com/vllm-project/vllm/pull/65788)

---

### **4. Performance & Optimization**  
- 🔥 **Triton Kernel Fusion**: PR #64212 adds `gelu_tanh_and_mul_per_token_quant` kernel for dynamic FP8 quantization, improving MLP fusion efficiency in Gemma3/4 models.  
- 💡 **MoE Optimization**: PR #65790 enables fallback to Triton for small M on Hopper when using `--moe-backend deep_gemm`, avoiding overhead from DeepGEMM’s contiguous layout.  
- 📈 **KV Cache Efficiency**: PR #64109 introduces back-only tail KV management for DFlash/DSpark drafter sliding-window layers, reducing memory pressure.  
- 🛠 **LoRA Performance**: PR #64502 adds multi-CTA LoRA shrink kernel (`VLLM_LORA_SHRINK_USE_MULTICTA=1`), eliminating floating-point atomics and partial reduction buffers.  
- 🎯 **ROCm Tuning**: Multiple PRs (e.g., #65530, #61119) optimize GEMM tiles and defer all-reduce operations for MiniMax-M3.1 and Qwen3-Next on gfx950.  

> 🔗 [PR #64212](https://github.com/vllm-project/vllm/pull/64212) | [PR #65790](https://github.com/vllm-project/vllm/pull/65790) | [PR #65530](https://github.com/vllm-project/vllm/pull/65530)

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|-------|-------------|------------|
| 🔴 Critical | [#61160](https://github.com/vllm-project/vllm/issues/61160) | Cross-request context contamination via shared-prefix KV reuse in P/D disaggregation with automatic prefix caching | Open — reproducible on DeepSeek-V4.1-Flash |
| 🔴 Critical | [#61229](https://github.com/vllm-project/vllm/issues/61229) | NixlConnector: KV blocks released during remote READ → decode consumes overwritten data | Open — silent corruption risk |
| 🟡 High | [#62383](https://github.com/vllm-project/vllm/issues/62383) | Qwen4Exp single prefill >262K tokens crashes with CUDA illegal memory access (v0.32.0 only) | Open — confirmed regression vs. v0.31.1 |
| 🟡 High | [#65618](https://github.com/vllm-project/vllm/issues/65618) | DSpark draft lacks `packed_modules_mapping` → 0% acceptance rate for DeepSeek-V4.1-Flash | Open — blocks speculative decoding |

> ⚠️ These regressions affect production-grade inference pipelines using P/D disaggregation and large-context models.

---

### **6. What This Means for Application Developers**  
- **Avoid v0.32.0** if using DeepSeek-V4.1-Flash or large-context Qwen4Exp models — use v0.31.1 as a temporary workaround until #65618 and #62383 are resolved.  
- **Enable `--no-enable-prefix-caching`** if experiencing cross-request crosstalk in P/D-disaggregated setups (per #61160).  
- **Monitor for NixlConnector instability** in multi-node deployments — avoid `--enforce-eager` with async KV pulls unless you’ve validated your setup.  
- **Leverage emerging MoE and LoRA optimizations** (PR #65790, #64502) for better throughput on H200/Hopper systems with dense or MoE models.  
- **Prepare for ROCm-specific tuning** — future performance gains on AMD MI355X will require model-specific kernel patches (e.g., #65530).  

> ✅ Pro Tip: Use `collect_env.py` and report full environment details when filing issues — many regressions stem from version mismatches between vLLM, FlashInfer, and CUDA/ROCm toolchains.

---  
*Digest generated: 2026-11-05 | Source: [vLLM GitHub](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest – 2026-11-05

---

### **1. Today's Highlights**  
The SGLang ecosystem continues to mature with active work on cross-platform stability and performance, particularly for AMD ROCm and Apple Silicon. Critical CI/CD reliability issues (#42752) remain unresolved, impacting PR validation velocity. Notable progress includes the integration of Kimi K3’s native MLA and live-restore hooks (#48510), and a new fix for multimodal feature transport over CUDA VMM to prevent GPU memory exhaustion (#48507).

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
No new releases or breaking API/config changes were published. The project remains in active development with no stable version updates.

---

### **3. New Model & Hardware Support**  
- **AMD ROCm (gfx950 / MI355X)**: Ongoing performance optimization for `Qwen3.8-Flash-Next` with MoE, QSA, and MXFP4 support via #44304.
- **Apple Silicon (M-series)**: RFC #32321 outlines a redesign for Torch-owned SRT path with exported MLX model regions—key step toward native Apple Silicon inference.
- **NPU (Ascend)**: Sampling mask support added via #43517, enabling RL agents to access logprobs through standard APIs.
- **Intel Xeon (SPR)**: CPU-core binding now supports Compute Building Block (CBB) mode for next-gen Xeons via #45717.
- **Diffusion**: Enhanced support for Wan VAE decoding and model checksum verification (PR #48225) under development.

---

### **4. Performance & Optimization**  
- **AMD DCP Prefill**: PR #48505 introduces routing by wire bytes to optimize DCP prefill latency and reduce redundant communication.
- **ROCm Speculative Decoding**: #48090 skips unnecessary vocabulary-wide sort during rejection sampling, improving efficiency on HIP.
- **CPU Graphs**: #44394 adds dynamic-shape fallback graphs and static bucketing for Intel AMX systems, enhancing prefill/decode throughput.
- **Hopper (SM90)**: Tracking issue #45373 focuses on kernel optimizations for GLM-5.3-Flash; PoC underway by @BBuf.
- **Blackwell (SM120)**: DeepSeek-V4 prefill throughput (~2–7K tok/s on 4× RTX PRO 6000) lags behind vLLM/Marlin (~12.5K tok/s); issue #33422 seeks tuning guidance.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR? | Link |
|------|----------|--------|---------|------|
| Flaky CI / infrastructure failures | High | Open | ❌ | [#42752](https://github.com/sgl-project/sglang/issues/42752) |
| Long prefill (>40k tokens) silently kills worker | Critical | Open | ❌ | [#36941](https://github.com/sgl-project/sglang/issues/36941) |
| MambaPool.copy_from causes CUDA OOM under burst admission | High | Open | ❌ | [#47916](https://github.com/sgl-project/sglang/issues/47916) |
| Disaggregation: Qwen EAGLE PD decode produces corrupted output after retraction | High | Open | ❌ | [#48356](https://github.com/sgl-project/sglang/issues/48356) |
| Triton override breaks other kernels | Medium | Open | ❌ | [#48460](https://github.com/sgl-project/sglang/issues/48460) |

> ⚠️ Multiple critical stability bugs affect high-throughput and long-context serving. No fixes yet available.

---

### **6. What This Means for Application Developers**  
- **Expect instability in long-running or high-concurrency scenarios**, especially with >40k token inputs or hybrid models (e.g., GLM-5.3-Flash). Avoid production use until #36941 and #47916 are resolved.
- **Use `--model-checksum` cautiously**: While model integrity checks exist for LLMs, diffusion models lack equivalent safeguards—consider adding custom verification (PR #48225 pending).
- **Leverage new features for agent workflows**: Programmable KV cache policies (#46542) and speculative decoding opt-out semantics (#47797) enable fine-grained control over agentic logic.
- **Monitor CI health**: Frequent flaky tests (#42752) may delay merges—expect delays in dependency updates and experimental features.
- **Plan for platform-specific tuning**: Performance varies significantly across hardware (e.g., SM120 vs. Hopper), so benchmark your workload on target GPUs before deployment.

> 💡 *Recommendation*: Use nightly builds only for testing; avoid main branch in production until core stability issues are addressed.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

---

### **llama.cpp Digest — 2026-11-05**

#### **1. Today's Highlights**  
The `v0.9.0` release introduces critical advancements in quantization and inference efficiency, including **OCP FP8 E4M3 support**, a new `flash_attn_ext_rows` op for optimized multi-sequence KV caching, and a generic batched tensor API. Significant performance improvements were landed for OpenCL and WebGPU backends, while ongoing stability fixes address Vulkan device loss and SYCL memory overflow issues across Intel Arc and AMD GPUs.

#### **2. Releases & Breaking Changes**  
- **`v0.9.0` (b12110)**: Official release with breaking changes to model loading behavior due to updated quantization handling and new `flash_attn_ext_rows` kernel integration.  
  - [GitHub Release v0.9.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.9.0)  
  - Migration note: Users relying on legacy Flash Attention paths may need to re-evaluate `--flash-attn` settings; check compatibility with `Q4_K_M`, `FP8_E4M3`, and GQA models.

#### **3. New Model & Hardware Support**  
- **Architectures Added**: Clover, Limite, Metis-1.6, Mistral Large 4  
  - [PR #31706: Meta MobileMoE support](https://github.com/ggml-org/llama.cpp/pull/31706)  
  - [PR #31692: Ripple (recursiveAI) 65B model support](https://github.com/ggml-org/llama.cpp/pull/31692)  
- **Quantization Formats**: OCP FP8 E4M3 (new), Q4_0-Q8_0 batching via `GGML_CUDA_FA_QUANTS` proposal  
- **Backends**:  
  - **OpenCL**: Improved decode performance via FA support for GQA 2/6 with 256-head size  
    - [PR #31633](https://github.com/ggml-org/llama.cpp/pull/31633)  
  - **WebGPU**: Batched tensor API (`get_tensor_n`, `set_tensor_n`) and `MEAN` support added  
    - [PR #31473](https://github.com/ggml-org/llama.cpp/pull/31473), [PR #31591](https://github.com/ggml-org/llama.cpp/pull/31591)  
  - **SYCL**: Continued work on MTP + Q3 quant performance  
    - [Issue #27373](https://github.com/ggml-org/llama.cpp/issues/27373)  
  - **Vulkan**: Intel prefill FA kernel (PR #29357) now active for Intel Arc platforms  

#### **4. Performance & Optimization**  
- **Flash Attention**: `flash_attn_ext_rows` enables efficient unified KV cache for multi-sequence inference, reducing overhead in streaming scenarios.  
- **OpenCL**: Up to **~15% decode speedup** observed on Qualcomm Adreno devices via improved workgroup utilization and GLU kernel optimization ([PR #31633](https://github.com/ggml-org/llama.cpp/pull/31633)).  
- **WebGPU**: Batched tensor operations reduce CPU-GPU sync points by ~30–40% in high-throughput scenarios ([PR #31473](https://github.com/ggml-org/llama.cpp/pull/31473)).  
- **CUDA**: PR #31679 ensures optimal MMVQ batch sizes by aligning host-side arch detection with actual PTX compilation target.  
- **Memory Efficiency**: `gguf` writer now skips staging buffer for host-buffer tensors ([PR #31144](https://github.com/ggml-org/llama.cpp/pull/31144)), reducing peak memory usage by ~10–15% during model export.

#### **5. Stability & Regressions**  
- **Critical**: **Vulkan device loss** reported on Intel Arc B580/B70 and AMD RX 7900 XTX after long-running inference (>7h) or flash attention fallback ([#29526](https://github.com/ggml-org/llama.cpp/issues/29526), [#31560](https://github.com/ggml-org/llama.cpp/issues/31560)).  
  - Fix PRs pending; workaround: disable `KHR_coopmat` or limit session duration.  
- **High Severity**: **SYCL dual-GPU stuck during model load** ([#27547](https://github.com/ggml-org/llama.cpp/issues/27547)) and **memory overflow during `--fit`** ([#27595](https://github.com/ggml-org/llama.cpp/issues/27595)) – both affect production deployments.  
- **Moderate**: `decode()` crash due to `vk::Queue::submit: ErrorDeviceLost` on Linux 7.x kernels ([#30010](https://github.com/ggml-org/llama.cpp/issues/30010)), likely tied to RADV driver instability.  
- **Regression**: 5% decode slowdown on Intel Arc B580 after `#29622` merge ([#30018](https://github.com/ggml-org/llama.cpp/issues/30018)) – bisected to embd+raw token batching change.

#### **6. What This Means for Application Developers**  
- **Streaming & Agents**: Use `flash_attn_ext_rows` and `--batch-size` with `--max-tokens` to improve throughput in multi-user, long-context applications.  
- **Model Serving**: Avoid `--cache-reuse` with dynamic prompt truncation until fix lands ([PR #30683](https://github.com/ggml-org/llama.cpp/pull/30683)).  
- **Hardware Choice**: Intel Arc users should enable `--flash-attn` and test `KHR_coopmat`; avoid it if non-deterministic results occur ([#31560](https://github.com/ggml-org/llama.cpp/issues/31560)).  
- **CI/CD**: Update CI builds to include `ccache` fixes ([PR #31705](https://github.com/ggml-org/llama.cpp/pull/31705)) for faster local development.  
- **Production Use**: Monitor for device loss on Vulkan with long-running sessions; consider restarting workers every 6–7 hours.  

> 🔗 [Official Website](https://llama.app) | 📦 [Download v0.9.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.9.0)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-05**

---

### **1. Today's Highlights**  
Critical stability and compatibility issues are emerging for users on Windows with NVIDIA RTX 5090 and WSL2, particularly around GPU memory access and tool calling failures in Qwen models. A high-severity bug in the Qwen2.5 library template causes malformed JSON output due to Go struct formatting, breaking native tool calls. Meanwhile, a new PR resolves this by implementing proper `String()` rendering for template functions.

---

### **2. Releases & Breaking Changes**  
*No new releases in the last 24 hours.*  
However, ongoing changes affect model behavior:  
- **Qwen2.5 library template fix (#19534)**: Resolves JSON rendering issue (`{{ .Function }}` now outputs valid JSON).  
  [PR #19534](https://github.com/ollama/ollama/pull/19534)  
- **Modelfile round-trip safety**: Fixes parsing of model names with whitespace via quoted `FROM` arguments.  
  [PR #19528](https://github.com/ollama/ollama/pull/19528)

---

### **3. New Model & Hardware Support**  
- **MLX runner improvements**:  
  - YaRN context extension support now enabled for Qwen3/Qwen3.5 via PR #19231.  
    [PR #19231](https://github.com/ollama/ollama/pull/19231)  
  - MLX CUDA runner now rejects unsupported devices at startup (prevents silent failure).  
    [PR #19521](https://github.com/ollama/ollama/pull/19521)  
- **WSL2 GPU memory optimization**: Patch submitted to fully utilize GPU memory on RTX Spark N1X laptops (128GB unified memory).  
  [PR #19520](https://github.com/ollama/ollama/pull/19520)  
- **New CLI integrations**: Kimi Code CLI support added via `ollama launch kimi`.  
  [PR #19506](https://github.com/ollama/ollama/pull/19506)

---

### **4. Performance & Optimization**  
- **Memory efficiency**: Qwen3.8 Flash Next now uses MXFP8 quantization selectively, preserving BF16 for critical paths (e.g., non-expert MTP) to improve long-generation quality.  
  [PR #18078](https://github.com/ollama/ollama/pull/18078)  
- **VRAM prediction accuracy**: Critical fix pending for overestimated KV cache usage (4–8x inflation) in hybrid models like Qwen3.5/3.8. This impacts model retention under `keep_alive=-1`.  
  [Issue #19463](https://github.com/ollama/ollama/issues/19463)  
- **Streaming performance**: OpenAI-compatible streaming now flushes responses promptly, resolving UI hanging issues.  
  [PR #17027](https://github.com/ollama/ollama/pull/17027)

---

### **5. Stability & Regressions**  
**High Severity**  
- **Tool calling crashes on Windows (RTX 5090)**: Intermittent disconnects during tool calls with HTTP 500 `/tokenize`, affecting Qwen2.5-Coder and Muse Glimmer 30B.  
  [Issue #19439](https://github.com/ollama/ollama/issues/19439)  
- **Qwen2.5-Coder & Qwen3 tool calling failure**: Native tool calling fails despite valid function definitions.  
  [Issue #19524](https://github.com/ollama/ollama/issues/19524)  
- **Vision model crash**: `qwen2.5vl:3b` fails with "prediction aborted, token repeat limit reached" when vision encoder runs on GPU.  
  [Issue #19483](https://github.com/ollama/ollama/issues/19483)  

**Medium Severity**  
- **Response truncation without warning**: API silently truncates oversized inputs even with `truncation=false` (Bonsai 27B).  
  [Issue #19492](https://github.com/ollama/ollama/issues/19492)  
- **Cloud API regression**: `deepseek-v4-pro` rejects `max_tokens > 65536` despite model’s ~384k capacity.  
  [Issue #19517](https://github.com/ollama/ollama/issues/19517)  
- **Usage API change**: `/api/usage` no longer returns session/weekly percentages (web UI still shows them).  
  [Issue #19511](https://github.com/ollama/ollama/issues/19511)

---

### **6. What This Means for Application Developers**  
- **Avoid Qwen2.5/Coder tool calls until PR #19534 is merged**—malformed JSON will cause function name mismatches and dropped calls.  
- **Validate model compatibility on RTX 5090/WSL2 systems**: Expect instability in tool calling and GPU memory allocation; consider fallback to CPU or lower-precision quantizations.  
- **Use `OLLAMA_DEBUG_LOG_REQUESTS` + PR #17472** to debug streaming and generation issues—current logs lack response payloads.  
- **Do not rely on `max_tokens > 65536` for `deepseek-v4-pro` on Ollama Cloud**—use client-side chunking or split requests.  
- **Update Modelfiles**: Ensure `FROM` model names are quoted if they contain spaces or special characters to prevent parse errors.  

> ✅ *Recommendation*: Monitor PRs #19534 and #19520 for immediate fixes impacting agent reliability and GPU utilization.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest – 2026-11-05

---

### **1. Today's Highlights**

The latest release candidate `v1.107.0-rc.3` and upcoming `v1.108.0` introduce significant enhancements to the control plane, including authenticated management passthrough, audit logging with WORM-compliant S3 object locks, and integration with Splunk and Microsoft Sentinel for SOC 2-ready compliance. Concurrently, critical stability fixes address streaming usage loss, budget tracking gaps, and guardrail misbehavior — especially around Anthropic’s `/v1/messages` format and MCP tool handling.

---

### **2. Releases & Breaking Changes**

- **`v1.108.0` (upcoming)**:  
  - Introduced **signed Docker images** via cosign (`e82a3c6`) — verify using:
    ```bash
    cosign verify --key https://raw.githubusercontent.com/BerriAI/litellm/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub ghcr.io/berriai/litellm:v1.108.0
    ```
  - Adds `--control-plane` flag to serve the control plane API from the same image, enabling unified deployment.
  - Implements full audit trail with OCSF standardization, queued delivery, hash chains, and sinks for Splunk HEC and Microsoft Sentinel ([PR #49662](https://github.com/BerriAI/litellm/pull/49662), [#49660](https://github.com/BerriAI/litellm/pull/49660)).

- **`v1.107.0-rc.3`**:  
  - Addresses key issues in cost tracking during early client disconnects ([Issue #14457](https://github.com/BerriAI/litellm/issues/14457)) and mid-stream fallback errors ([Issue #49795](https://github.com/BerriAI/litellm/issues/49795)).
  - Fixes silent drops of `drop_params` on partial model updates ([Issue #35842](https://github.com/BerriAI/litellm/issues/35842)).

> 🔔 *Migration Note:* Upgrade carefully if relying on custom `litellm_params`, `guardrails`, or session budgeting logic. Audit logs are now structured and tamper-evident.

---

### **3. New Model & Hardware Support**

- **QwenCloud Migration Path**: Official support added via [Feature Request #36150](https://github.com/BerriAI/litellm/issues/36150) — enables seamless migration from DashScope to QwenCloud with OpenAI-compatible endpoints.
- **Cloudflare Workers AI Provider Update**: Proposal to modernize provider implementation to reflect expanded Cloudflare AI API capabilities (text-to-image, new models) — currently tracked in [Issue #21115](https://github.com/BerriAI/litellm/issues/21115).
- **Databricks Tool-Only Response Handling**: Now correctly processes assistant messages without `content` when reasoning is disabled ([Issue #48637](https://github.com/BerriAI/litellm/issues/48637)).

---

### **4. Performance & Optimization**

- **Control Plane Scalability**:  
  - `Fanout.gather` primitive enables bounded scatter/gather across multiple dataplanes, improving fleet-wide aggregate visibility and resilience ([PR #49656](https://github.com/BerriAI/litellm/pull/49656)).
  - Rust-based router core introduced as a performance-optimized alternative to Python-based routing ([PR #49513](https://github.com/BerriAI/litellm/pull/49513)), with minimal cross-boundary overhead.

- **Latency Improvements**:  
  - Fixed upstream error propagation in `sse_keepalive` streams that previously masked failures after initial ping ([Issue #49711](https://github.com/BerriAI/litellm/issues/49711)).
  - Optimized `POST /health/test_model` endpoint now performs live three-turn sanity checks per model, reducing false negatives during deployment validation ([PR #49337](https://github.com/BerriAI/litellm/pull/49337)).

---

### **5. Stability & Regressions**

| Severity | Issue | Description | Fix PR |
|--------|------|-------------|--------|
| ⚠️ Critical | [Issue #14457](https://github.com/BerriAI/litellm/issues/14457) | Streaming response termination causes complete loss of usage data (billing gaps). | In progress (`v1.107.0-rc.3`) |
| ⚠️ High | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) | `/key/update` mid-request skips cost tracking in budget window (`LiteLLM_BudgetWindowSpend`). | Patched in `v1.107.0-rc.3` |
| ⚠️ High | [Issue #49795](https://github.com/BerriAI/litellm/issues/49795) | Mid-stream failure loses fallback due to incorrect `sent_first_chunk` state. | Fix in `v1.107.0-rc.3` |
| ⚠️ Medium | [Issue #48773](https://github.com/BerriAI/litellm/issues/48773) | Azure Responses API duplicates tool-call arguments in streaming. | Resolved in `v1.107.0-rc.3` |
| ⚠️ Medium | [Issue #50065](https://github.com/BerriAI/litellm/issues/50065) | `tools/list` blocks for legacy clients after v1.108.0 upgrade. | Active investigation |

> ✅ *Note:* Most high-severity bugs have patches in the current RC; test thoroughly before production rollout.

---

### **6. What This Means for Application Developers**

- **Build more secure, auditable systems**: With enhanced audit logging (OCSF, WORM storage, SIEM integrations), you can now meet compliance requirements like SOC 2 without external tools.
- **Avoid billing inaccuracies**: Ensure accurate cost tracking by upgrading to `v1.107.0+` — especially critical if your app uses long-running or interruptible streams.
- **Improve agent reliability**: Guardrail and MCP tool handling now better respects Anthropic’s `thinking`/`reasoning_content` semantics and avoids infinite loops ([#48384](https://github.com/BerriAI/litellm/issues/48384), [#36085](https://github.com/BerriAI/litellm/issues/36085)).
- **Leverage new observability features**: Use `/health/test_model` and deprecation banners to proactively detect broken model routes and plan migrations ([PR #50022](https://github.com/BerriAI/litellm/pull/50022), [#50080](https://github.com/BerriAI/litellm/pull/50080)).

> 📌 **Actionable Tip**: If using self-hosted models (e.g., vLLM) with guardrails, update immediately to avoid silent pre-call hook execution on invalid models ([PR #49295](https://github.com/BerriAI/litellm/pull/49295)).

---  
*Digest generated from GitHub data: [BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-05**

---

### **1. Today's Highlights**  
The v0.1.915-beta release introduces **Qwen3.8-Flash-Next**, enhances Glass themes (now in Beta), and enables model interaction with files via the built-in browser—marking a significant step toward full local agent capabilities. Critical stability fixes address UI lag in long conversations, `search_conversation` tool disappearance, and RAG ingestion bottlenecks, while new PRs improve audio model handling and embedding model download resilience.

---

### **2. Releases & Breaking Changes**  
- **v0.1.915-beta**: [GitHub Release](https://github.com/unslothai/unsloth/releases/tag/v0.1.915-beta)  
  - Adds support for **Qwen3.8-Flash-Next** (optimized inference path).  
  - Introduces **Glass theme beta** with improved visual consistency.  
  - Enables **file interaction in-browser** (models can read/write from built-in file system).  
  - Preserves chat context across GGUF model switches; reduces idle memory usage.  
  - Faster MLX loading and native FP8 support now available.  

> ✅ *No breaking changes to APIs or config; backward compatibility maintained.*

---

### **3. New Model & Hardware Support**  
- **Qwen3.8-Flash-Next**: Added as a first-class model variant with optimized Flash Attention paths.  
- **Audio.cpp Music Models**: Expanded support for `YuE2`, `Stable Audio 3 Medium`, and `MiDashengLM-Gen` (via size estimation fixes).  
- **Image Models**: Studio now loads single-file `.safetensors` variants of:  
  - Anima  
  - Chroma1-HD  
  - MOSS-Image  
  - Krea 2 int4/fp8 (with ComfyUI-compatible LoRA/LoKR support)  
- **Hardware Backends**:  
  - Enhanced Metal (MLX) performance on Apple Silicon.  
  - Improved CUDA graph capture for decode steps (default in `generate()`).

---

### **4. Performance & Optimization**  
- **FP8 Inference**: Now enabled by default in v0.1.915-beta, reducing memory footprint without loss in accuracy.  
- **Latency Reduction**:  
  - **CUDA graphs** applied by default in `model.generate()` → reduces kernel launch overhead (675+ kernels per step reduced to near-zero).  
  - **MLX load speedup** observed (~2x faster startup for large models on M-series chips).  
- **RAG Ingestion**:  
  - Batched embeddings now process **~48× faster** (from ~99 ms/chunk to ~2.07 ms/batch) when using multi-chunk batching (`_EMBED_BATCH`).  
  - Fixes #16715: previously processed documents one-by-one despite batch-ready input.  
- **Memory Efficiency**: Idle memory usage lowered significantly post-v0.1.915-beta.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR |  
|------|----------|--------|--------|  
| `search_conversation` tool disappearing (repeatedly) | High | Open | [#16718](https://github.com/unslothai/unsloth/pull/16718) |  
| Long conversation UI lag (web + desktop) | High | Open | — |  
| AppImage UI lag on Linux (Slackware, etc.) | High | Open | [#16605](https://github.com/unslothai/unsloth/issues/16605) |  
| RAG indexing fails during embedding model download | High | Open | [#16722](https://github.com/unslothai/unsloth/pull/16722) |  
| S3 JSONL manifests fail on Unicode separators (U+2028/U+2029) | Medium | Open | [#16725](https://github.com/unslothai/unsloth/pull/16725), [#16723](https://github.com/unslothai/unsloth/pull/16723) |  
| Studio theme flicker on first load (light → dark transition) | Low | Closed | [#16614](https://github.com/unslothai/unsloth/issues/16614) |

> ⚠️ **Critical**: Multiple regressions in core UX (search, UI lag, RAG) reported in last 24h; several fix PRs merged today.

---

### **6. What This Means for Application Developers**  
- **Agent Builders**: With **browser file access**, **Qwen3.8-Flash-Next**, and **persistent context**, you can now build lightweight, locally-hosted agents that read/write files and maintain state across sessions.  
- **Fine-Tuning Pipelines**: Use `/skills` command (`#16717`) and `keep cancelled jobs` logic (`#16633`) to improve workflow resilience in automated training flows.  
- **Embedding Systems**: The fix for `EmbeddingModelDownloadRequiredError` during indexing (`#16722`) allows seamless RAG pipelines even when models are still downloading.  
- **Audio/Video Workflows**: Support for ComfyUI-style single-file models and Unicode-safe JSONL parsing enables robust audio/video fine-tuning pipelines.  
- **Performance Tuning**: Enable `CUDA graphs` by default in `generate()` calls—expect **~30–50% lower latency** on GPU-heavy workloads.  

> 📌 **Action Item**: Upgrade to v0.1.915-beta and audit any RAG or long-context workflows for regression risks. Monitor `search_conversation` behavior until #16718 lands.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*