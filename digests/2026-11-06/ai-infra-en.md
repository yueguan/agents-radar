# AI Infrastructure Digest 2026-11-06

> Generated: 2026-11-06 02:47 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-06**

---

### **1. Ecosystem Overview**

The AI inference and serving ecosystem in late 2026 is characterized by rapid specialization, hardware diversification, and increasing convergence between local runtime, model serving, and agent orchestration layers. Projects are no longer competing on broad feature parity but instead carving out distinct niches: vLLM dominates high-throughput, GPU-optimized inference; SGLang pushes the envelope in speculative decoding and MoE-aware scheduling; llama.cpp leads in cross-platform portability and low-level kernel control; Ollama continues to serve as a developer-friendly gateway for local agents; LiteLLM acts as the universal API layer for multi-provider workflows; and Unsloth focuses on memory efficiency and training-integrated deployment. The rise of AMD ROCm, Intel XPU, and Apple MLX support signals a shift toward heterogeneous compute, while safety, determinism, and correctness remain critical concerns across all stacks.

---

### **2. Activity Comparison**

| Project       | Open Issues | Open PRs | Recent Release? | Status |
|---------------|-------------|----------|------------------|--------|
| **vLLM**      | 873         | 429      | No (v0.32.0 under scrutiny) | Stable but critical SM120 crash |
| **SGLang**    | 654         | 387      | No               | CI instability blocks progress |
| **llama.cpp** | 528         | 294      | No               | High-severity stability bugs |
| **Ollama**    | 589         | 212      | No               | Critical Vulkan/MLX regressions |
| **LiteLLM**   | 476         | 234      | ✅ v1.108.1      | Security-hardened release |
| **Unsloth**   | 412         | 187      | No               | UI and memory issues dominate |

> *Note: Issue counts reflect active, unmerged items with severity tags (e.g., 🔴 High). PR volume correlates with innovation velocity, though SGLang and vLLM show higher engineering throughput despite CI bottlenecks.*

---

### **3. Model Support Race**

| New Model / Architecture        | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| `Qwen3.8-Flash-Next` (AMD)       | ✅   | ✅     | ✅        | ✅     | —       | —       |
| `Qwen3.8-2.4T-A95B` (AMD)        | ✅   | —      | —         | —      | —       | —       |
| `Hindsight hs-decide-1-flash`    | —    | —      | —         | ✅     | —       | —       |
| `GLM-5.3-Flash` (Ascend A3/A5)   | —    | ✅     | —         | —      | —       | —       |
| `xing4_0` (MoE + Vision)         | —    | —      | ✅        | —      | —       | —       |
| `MiniMax-H3`, `Lumina-Image-2.0` | —    | —      | —         | —      | —       | ✅      |
| `nemotron-3.5-asr-streaming`     | ❓   | —      | —         | —      | —       | —       |
| `Intel XPU` (xccl2, paged experts)| ✅   | ✅     | —         | ⬆️ (Request) | —       | —       |
| `Apple Silicon (MLX)`            | —    | —      | —         | ✅     | —       | ✅      |

> **Leaderboard**:  
> - **vLLM** leads in **NVIDIA & AMD ROCm** support with deep optimization for Qwen3.8 series.  
> - **SGLang** excels in **cross-architecture innovation**, especially with Ascend, DFlash/DSpark MoE drafts, and Intel XPU.  
> - **Unsloth** is uniquely focused on **image generation and diffusion training** integration.  
> - **Ollama** wins in **agent-centric model adoption** with `hs-decide-1-flash` and optimized `qwen3.8-flash-next`.

---

### **4. Performance Frontier**

| Optimization Focus          | vLLM                     | SGLang                   | llama.cpp             | Ollama              | LiteLLM                 | Unsloth                  |
|------------------------------|--------------------------|--------------------------|------------------------|---------------------|-------------------------|--------------------------|
| **KV Cache Management**      | ✅ FlashInfer GDN, R3 capture | ✅ Sliding-window retention | ✅ LRFU/MQ cache (moe-cache) | —                   | —                       | —                        |
| **Batching & Throughput**    | ✅ Pipeline flexibility, DCP | ⚠️ Prefill gap vs vLLM     | ✅ Hybrid batching     | —                   | ✅ Pinning for caching  | ⚠️ Batch-of-1 RAG inefficiency |
| **Quantization**             | ✅ int8_per_token_head, MXFP4 | ✅ IQ4_NL JIT kernels     | ✅ IQ4_NL scalar MMQ   | ✅ NVFP4 (Qwen3.8)  | —                       | —                        |
| **Distributed Serving**      | ✅ MoE expert parallelism | ✅ DCP prefill routing     | —                      | —                   | ✅ Rate limit accuracy  | ✅ PP aux loss handling  |
| **Kernel-Level Optimization**| ✅ In-place SSM, A2A merge | ✅ Unified TMA copy kernel | ✅ `mul_mm` (Metal)   | —                   | ✅ Overhead metric      | ✅ W4A16 auto step protection |

> **Frontier Summary**:  
> - **vLLM** leads in **scalable, large-model serving** with advanced caching and pipeline control.  
> - **SGLang** pioneers **MoE-aware speculation and distributed routing**.  
> - **llama.cpp** dominates **low-level, platform-specific kernel tuning**.  
> - **Unsloth** innovates in **diffusion training memory planning** and **in-process tool calls**.  
> - **LiteLLM** focuses on **gateway-level observability and cost control**.

---

### **5. Layer Positioning**

| Project       | Primary Layer               | Key Differentiator                                                                 |
|---------------|-----------------------------|-----------------------------------------------------------------------------------|
| **vLLM**      | **High-Performance Serving Engine** | Optimized CUDA kernels, MoE expert parallelism, direct weight reload for RL.      |
| **SGLang**    | **Speculative Decoding Engine**   | Advanced draft serving, DFlash/DSpark integration, MoE routing simulation.        |
| **llama.cpp** | **Local Runtime & Portability**   | Cross-platform, CPU/GPU/Vulkan/Metal/WebGPU support, GGUF paging, low-memory design. |
| **Ollama**    | **Agent-Focused Gateway**         | Developer-first UX, local model hub, agent-ready models (e.g., `hs-decide`).       |
| **LiteLLM**   | **Universal API Gateway**         | Multi-provider routing, structured output, S3/Batch security, streaming reliability. |
| **Unsloth**   | **Training-Integrated Runtime**   | Diffusion training memory planning, context-aware rendering, RAG ingestion.       |

> **Positioning Insight**: The stack is becoming vertically integrated—developers now pick tools based on their role: **engineers** use vLLM/SGLang for scale; **builders** use Ollama for speed; **operators** use LiteLLM for consistency; **researchers** use Unsloth for training fidelity.

---

### **6. Trend Signals**

#### **Emerging Industry Trends (from 2026-11-06 activity):**
1. **Hardware Heterogeneity is Now Standard**: AMD ROCm, Intel XPU, Huawei Ascend, Apple MLX, and even older APUs (Renoir) are being actively supported — indicating that no single vendor dominates.
2. **MoE is Moving from Research to Production**: Multiple projects (vLLM, SGLang, llama.cpp) now support MoE expert parallelism, routed expert capture (R3), and lookahead scheduling — signaling production readiness.
3. **Speculative Decoding is Becoming Complex**: Drafting now includes N-gram tail insertion, sliding-window KV retention, and full MoE routing — moving beyond simple beam search.
4. **Security & Correctness Are Non-Negotiable**: Signed Docker images (LiteLLM), deterministic top-k (llama.cpp), and silent failure fixes (Ollama, vLLM) reflect growing maturity.
5. **Agent-Centric Design is Driving Adoption**: Models like `hs-decide-1-flash` and tool call reliability fixes highlight a shift from general-purpose LLMs to **specialized decision-making components**.

#### **What Application Developers Should Watch:**
- ✅ **Adopt vLLM or SGLang for high-throughput, long-context inference** — but validate Blackwell SM120 behavior.
- ✅ **Use Ollama for local agent systems** — especially with `hs-decide-1-flash` and `qwen3.8-flash-next`.
- ✅ **Leverage LiteLLM for secure, multi-provider pipelines** — prioritize Cosign verification.
- ⚠️ **Avoid speculative decoding until `#25618` (llama.cpp) and `#33185` (SGLang) are resolved** — expect divergence.
- 🛑 **Do not deploy on AMD Renoir iGPU (Vulkan)** or Intel Arc without testing — known crashes (`#19558`, `#31560`).
- 🔮 **Monitor Unsloth’s `context_parallel` and `Context Window Quick Boost` features** — they may redefine long-context interaction.

---

> **Final Note**: The AI infrastructure landscape is no longer about "which engine is fastest" — it's about **layered composition**. The future belongs to developers who strategically combine vLLM (for speed), SGLang (for speculation), LiteLLM (for routing), Ollama (for agents), and Unsloth (for training-to-inference continuity).

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-11-06**

#### **1. Today's Highlights**  
The vLLM project continues to advance its support for next-generation hardware and inference patterns, with significant progress on **ROCm/AMD performance optimization** for Qwen3.8-Flash-Next and Qwen3.8-2.4T-A95B models. A major focus is on **direct weight reloading** for RL training, with multiple PRs refining the mechanism to avoid silent failures and ensure consistency across model updates. Meanwhile, **Rust frontend parity** remains a top priority, now with 54 community discussions highlighting growing interest in low-latency, systems-level integration.

#### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API changes were published. The `v0.32.0` release remains under scrutiny due to unresolved issues (e.g., #59203, #62383), particularly around block size and CUDA memory access crashes on SM120 Blackwell GPUs.

#### **3. New Model & Hardware Support**  
- ✅ **AMD ROCm Support**: Deepened optimization for `amd/Qwen3.8-Flash-Next-Quark-MXFP4` and `amd/Qwen3.8-2.4T-A95B-Quark-MXFP4` on gfx950 / MI355X, including performance tracking via #57149 and #59575.  
- ✅ **Intel XPU**: PR #64818 introduces `xccl2` backend for distributed communication on Intel GPUs, replacing deprecated `xccl`.  
- ✅ **NVIDIA Nemotron 3.5 ASR Streaming**: Feature request #47455 calls for native support of `nemotron-3.5-asr-streaming-0.6b`, a multilingual streaming ASR model.  
- ✅ **MoE Expert Parallelism**: PR #61897 enables routed experts capture (R3) with pipeline parallelism in Model Runner V2, critical for large-scale MoE serving.

#### **4. Performance & Optimization**  
- 🚀 **FlashInfer GDN Prefill State Management**: PR #64498 enables in-place reading/writing of SSM states in cache pool, reducing 4–5 kernel launches per layer for hybrid attention models like Qwen3.5/3.8.  
- 🚀 **Tokenizer Pool Efficiency**: PR #65674 reduces unnecessary deep-copying by only duplicating the `_tokenizer` backend, improving thread safety without overhead.  
- 🚀 **DCP & MLA Decode Optimization**: PR #64994 up-projects MLA decode output before A2A merge, cutting data transfer from 514 to 130 bf16 values per head (Kimi-K3, 512/128 heads).  
- 🚀 **Pipeline Parallelism Flexibility**: PR #63106 adds support for arbitrary pipeline stage partitions in DeepSeek-V4.1, enabling finer-grained control over KV state sharing across PP ranks.  
- 🔧 **Scratch Memory Reservations**: PR #61584 reserves attention scratch across all microbatch slots upfront, preventing workspace growth during warmup on ROCm.

#### **5. Stability & Regressions**  
- ⚠️ **Critical Crash on SM120 (Blackwell)**: Issue #62383 reports a **CUDA illegal memory access crash** when processing single prefill steps exceeding 262,144 tokens in Qwen4Exp (Qwen3.8-Flash-Next), confirmed in v0.32.0 but not v0.31.1.  
- ⚠️ **KV Cache Corruption with int8_per_token_head + Prefix Caching**: Issues #50702 and #50749 report silent corruption of first generated tokens under high load, affecting Gemma-4 hybrid models on Triton.  
- ⚠️ **Rust Frontend Bug**: Issue #44280 highlights incomplete feature parity in the Rust frontend (enabled via `VLLM_USE_RUST_FRONTEND=1`), with 54 comments reflecting community demand for full functionality.  
- ⚠️ **Memory Profiling Gap**: Issue #54775 reveals that KDA/gated-delta-rule chunked-scan buffers are unaccounted for in startup memory profiling, causing OOM at high `--max-num-batched-tokens`.  
- ✅ **Fixes in Progress**: PRs #66010 (MoE perm indices fix), #65722 (AITER FA skip bug), and #65326 (level-2 sleep wake-up race) address critical correctness and stability concerns.

#### **6. What This Means for Application Developers**  
- **Use caution with v0.32.0** — avoid `--max-model-len` near 262k tokens on Blackwell (SM120) until #62383 is resolved.  
- **Leverage direct weight reload (`VLLM_RELOAD_MODE=direct`)** for RL fine-tuning workflows; ensure models opt-in via #66002 to prevent silent errors.  
- **Optimize for AMD ROCm** using latest Qwen3.8 variants — expect improved throughput on MI355X via staged optimizations (#57149, #59575).  
- **Enable `--dcp-comm-backend a2a`** and `--use-cuda-multiprocessor` for MoE and DCP workloads to reduce inter-node bandwidth.  
- **Avoid `int8_per_token_head` with prefix caching** on hybrid models until #50702/#50749 are patched.  
- **Monitor Rust frontend adoption** — while experimental, it’s gaining traction and may soon be production-ready.

> 🔗 [GitHub vLLM Issues](https://github.com/vllm-project/vllm/issues) | [PRs](https://github.com/vllm-project/vllm/pulls)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-06**

---

### **1. Today's Highlights**  
The SGLang project continues to accelerate its hardware and model specialization, with new PRs advancing MoE draft serving for DFlash/DSpark and optimizing AMD ROCm paths for GLM-5.3 and Qwen3. Critical CI stability remains a top concern, as flaky tests (#42752) and infrastructure failures persist across multiple platforms—particularly on Xeon (SPR), H20, and Mooncake RDMA setups. Meanwhile, developers are actively refining speculative decoding workflows, including N-gram tail insertion and sliding-window KV retention for DFLASH drafts.

---

### **2. Releases & Breaking Changes**  
*No new releases in the past 24 hours.*  
No API or configuration-breaking changes reported.

---

### **3. New Model & Hardware Support**  
- **DFlash/DSpark MoE Draft Serving**: PR #48763 adds support for serving MoE FFN drafts in DFlash and DSpark families, enabling SpecForge-trained models to leverage expert routing during speculation.
- **AMD ROCm Optimization**: PRs #47052 (Qwen3: fused QK-norm + RoPE via JIT kernel), #48505 (DSA DCP prefill routing by wire bytes), and #46486 (GLM-5 decode MLA chain on gfx950) improve performance on MI355X and other ROCm-enabled GPUs.
- **Ascend A3/A5 Support**: PR #43305 enables GLM-5.3-Flash on Huawei Ascend A3/A5 NPUs, expanding cross-architecture inference coverage.
- **Apple Silicon Redesign**: RFC #32321 proposes a Torch-owned SRT path with exported MLX regions for Apple Silicon, aiming for better memory management and interoperability.
- **Intel XPU Paged Experts**: RFC #46520 outlines support for paged experts with Lookahead Expert Scheduling (LES) and SSD-tier offload on iGPU, targeting scalable MoE inference.

---

### **4. Performance & Optimization**  
- **DeepSeek-V4 Prefill Throughput Gap**: Issue #33422 reports ~2–7K tok/s vs. ~12.5K on vLLM/Marlin on 4× RTX PRO 6000 Blackwell (SM120). Investigation ongoing; potential kernel-level optimization needed.
- **GLM-5.3-Flash on Hopper (SM90)**: Tracking issue #45373 highlights ongoing kernel optimizations; PoC work underway by @BBuf.
- **MoE Routing Simulation**: RFC #44697 proposes a profiling tool to simulate prefill imbalance using GPU-faithful replay—critical for load-balancing improvements.
- **HiCache TMA Copy Kernel**: PR #48786 introduces a unified layout TMA copy kernel to reduce overhead in high-throughput caching scenarios.
- **Diffusion Caching**: PR #48122 caches MiniMax-H3 reference and keyframe rows per image, reducing redundant encoding on repeated requests.

---

### **5. Stability & Regressions**  
- **Critical CI Flakiness**: Issue #42752 tracks recurring CI failures in NVIDIA `PR Test Base/Extra` runs due to flaky tests and unstable infrastructure. This has triggered CI Maintenance Mode (#21065), pausing normal PR validation until resolved.
- **Mooncake RDMA Failures**: Multiple issues report persistent "Memory region not registered" errors on 8-gpu-H20 runners (#48206) and ROCm disaggregation tests (#45849).
- **Xeon SPR OOM Kills**: Issue #48411 confirms OOM kills during KV allocation on `sglang-spr08/18/19`, affecting server startup on Intel Xeon Scalable (SPR) systems.
- **Model-Specific Bugs**: 
  - GLM-5.3-Flash crashes at startup under pipeline parallelism (#36906)
  - DeepSeek-V4 long-context prefill fails with illegal memory access in DSA indexer (`topk_v1.cuh:348`) — #37892
  - EAGLE DSA misses `KVIndexTranslator` in FP8 MHA prefix reads — #38029

> ✅ *Fix PRs exist for some regressions (e.g., #48446 for sampling_params duplication), but systemic CI instability remains unresolved.*

---

### **6. What This Means for Application Developers**  
- **Expect delays in PR merges** due to active CI maintenance mode — prioritize testing on local machines or staging environments.
- **Use caution with speculative decoding on MoE models**; verify draft routing behavior (e.g., DFLASH sliding-window KV retention via #48785) and monitor for correctness bugs like `reasoning_effort` mismapping (#33185).
- **Leverage emerging hardware-specific optimizations**: Use ROCm paths for GLM/Qwen3 on AMD, and expect future gains from Intel XPU and Ascend support.
- **Avoid relying on auto-generated model registry imports**; PR #48080 highlights that eager model loading hides dependency failures and increases startup latency (~2s).
- **Monitor context-length handling**: Issue #48557 reveals inconsistent HTTP error codes (`200` vs `400`) when context exceeds limits—ensure client-side logic accounts for this divergence.

👉 [View all open issues](https://github.com/sgl-project/sglang/issues) | [Browse recent PRs](https://github.com/sgl-project/sglang/pulls)

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-06**

---

### **1. Today's Highlights**  
The latest updates focus on critical stability fixes for speculative decoding and GPU backend correctness, particularly in Vulkan (Intel Arc) and Metal (Apple Silicon). Notable improvements include deterministic behavior in CUDA top-k selection and enhanced support for IQ4_NL quantization on Vulkan, enabling better compatibility with emerging model formats.

---

### **2. Releases & Breaking Changes**  
No new releases were published today. However, the following changes are **breaking in practice** due to behavioral shifts:  
- **Vulkan**: The `FA dequant` path now correctly checks combined K+V scratch size (`#30487`), which may trigger runtime errors if insufficient memory is allocated — a regression fix that could break older configurations. [PR #30487](https://github.com/ggml-org/llama.cpp/pull/30487)  
- **Metal**: The introduction of `mul_mm` for `32 <= K < 64` improves performance but may require reconfiguration of `n_gpu_layers` when using MTP or MoE models. [PR #30982](https://github.com/ggml-org/llama.cpp/pull/30982)

---

### **3. New Model & Hardware Support**  
- **Model Support**: Added full support for `xing4_0`, including MoE, MTP, MLA, and vision tower inference via `mmproj`. [PR #31589](https://github.com/ggml-org/llama.cpp/pull/31589)  
- **Hardware Backends**:  
  - **Vulkan**: Added `IQ4_NL scalar MMQ` matmul kernel for GPUs without cooperative matrices (e.g., Intel Arc). [PR #31764](https://github.com/ggml-org/llama.cpp/pull/31764)  
  - **Metal**: Extended `mul_mm` usage to smaller K ranges (32–64), improving efficiency on Apple Silicon. [PR #30982](https://github.com/ggml-org/llama.cpp/pull/30982)  
  - **WebGPU**: Added `COUNT_EQUAL` support for I32 inputs and I64 output, enabling faster token counting in `ggml-opt`. [PR #31578](https://github.com/ggml-org/llama.cpp/pull/31578)  
- **Quantization**: `YaRN` now supports `truncate=false` for non-integer correction interval boundaries. [PR #30206](https://github.com/ggml-org/llama.cpp/pull/30206)

---

### **4. Performance & Optimization**  
- **Speculative Decoding**: The `moe-cache` PR introduces a hybrid LRFU/MQ-like cache (`ClassLRU`) that reduces expert uploads by **8%** while boosting decode speed by **+6% (QFN)** and **+7% (GLM)**. [PR #31233](https://github.com/ggml-org/llama.cpp/pull/31233)  
- **CUDA**: Updated radix Top-K sorting to make tied selections deterministic (earliest wins), improving consistency across runs. [PR #31084](https://github.com/ggml-org/llama.cpp/pull/31084)  
- **Vulkan**: Disabled subgroup arithmetic on Qualcomm Adreno GPUs to prevent miscompilation; maintains performance parity on other vendors. [PR #30826](https://github.com/ggml-org/llama.cpp/pull/30826)  
- **Memory Efficiency**: `llama-server` now uses reference-counted checkpoints instead of deep copies, reducing memory overhead during session restores. [PR #30137](https://github.com/ggml-org/llama.cpp/pull/30137)

---

### **5. Stability & Regressions**  
Critical issues reported today:  
1. **Non-deterministic results on Intel Arc Pro B70 (Battlemage)** with Vulkan + `KHR_coopmat` enabled (`#31560`). Same input yields different outputs — **high severity**, no fix yet. [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560)  
2. **CPU F16 dot product overflow on Apple M4 Max** (`#31734`). Accumulation in F16 leads to `inf` values under certain conditions — affects ARM builds without SVE. [Issue #31734](https://github.com/ggml-org/llama.cpp/issues/31734)  
3. **CUDA illegal memory access** during MTP draft generation after KV cache rollback (`#31077`). Occurs on RTX 4060 Ti; requires immediate attention. [Issue #31077](https://github.com/ggml-org/llama.cpp/issues/31077)  
4. **Gemma 4 + MTP crash on startup** due to `GGML_ASSERT(ubatch->embd_state)` (`#30698`). Affects HIP backend users. [Issue #30698](https://github.com/ggml-org/llama.cpp/issues/30698)  

> ⚠️ **Note**: Multiple speculative decoding bugs remain unconfirmed but have high comment counts (e.g., `#25618` with 38 comments).

---

### **6. What This Means for Application Developers**  
- **Use caution with speculative decoding** (`draft-mtp/draft-dspark`) on quantized models — expect divergence from vanilla inference until `#25618` is resolved.  
- **Ensure Vulkan drivers are updated**, especially on Intel Arc devices, to avoid nondeterminism (`#31560`). Consider disabling `KHR_coopmat` temporarily if instability persists.  
- **Upgrade to recent HEAD** for improved checkpoint handling and reduced memory overhead in long-running servers (`#30137`).  
- **Avoid CPU-pinned buffers** for large models like Qwen3.8-Flash-Q8_0 (`#29932`) on multi-node setups — use `--no-pinned` or offload more layers to GPU.  
- **Monitor for regressions** in `ggml-cuda` and `ggml-metal` kernels — performance gains may come at the cost of increased sensitivity to hardware-specific quirks.  

> ✅ **Pro Tip**: Use `--spec-type draft-mtp --n-gpu-layers=auto` with `--n-cpu-moe` only if you’ve verified stable operation — offloaded MoE paths still show idle GPU waits (`#25859`).  

---  
*Data source: [github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-06**

---

### **1. Today's Highlights**  
The Ollama ecosystem continues to expand its support for advanced inference workflows, with critical stability fixes for MLX and Vulkan backends now in flight. Notably, PRs addressing silent truncation issues in responses (`#19501`) and `null`-returning `/v1/models` (`#19539`) have been merged, improving compatibility with OpenAI-compliant clients. New model support for Hindsight’s `hs-decide-1-flash` (`#19555`) and enhanced Qwen3.8 Flash Next memory efficiency (`#18078`) signal growing focus on agent-centric, low-latency reasoning.

---

### **2. Releases & Breaking Changes**  
*None reported in the last 24 hours.*  
However, **PR #19539** has been merged to fix a breaking change in OpenAI compatibility: `/v1/models` now returns an empty array `[]` instead of `null` when no models are installed, aligning with OpenAI spec and preventing failures in strongly-typed SDKs (e.g., .NET/Java). [GitHub PR #19539](https://github.com/ollama/ollama/pull/19539)

---

### **3. New Model & Hardware Support**  
- **New Model Support**:  
  - Added support for **Hindsight’s `hs-decide-1-flash`** decision model via `#19555`, enabling integration into agent systems requiring fast, lightweight choice-making logic. [GitHub PR #19555](https://github.com/ollama/ollama/pull/19555)  
- **Hardware & Backend Progress**:  
  - **Intel Arc GPU Support** remains a high-priority feature request (#1590), with 76 comments and 96 upvotes as of today — indicating strong community demand.  
  - **AMD Renoir iGPU (Ryzen 4700U)** users face crashes during model load under Vulkan backend (`#19558`), highlighting ongoing instability on older APUs.  
  - **Apple M4 Max (MLX)** continues to show nuanced behavior, including tool call loss (`#19359`) and stop sequence ignoring (`#19552`).

---

### **4. Performance & Optimization**  
- **Memory Efficiency**:  
  - `#18078` introduces NVFP4 quantization optimizations for `qwen3.8-flash-next:125b-a6b-nvfp4` and `mlx` variants, preserving BF16 precision in key layers (QSA projections, non-expert MTP path) while using MXFP8 elsewhere — reducing memory pressure without sacrificing long-generation fidelity.  
- **Latency & Throughput**:  
  - No new performance regressions or benchmarks reported today. However, **issue #18968** reports a 25–30% drop in tokens/sec across all models from v0.40.1 to v0.40.13+, suggesting potential regression in recent builds that warrants investigation.  
- **Parallelism**:  
  - PR `#17144` re-enables parallel requests for `qwen35`/`qwen35moe` now that upstream llama.cpp crash is resolved, potentially boosting throughput for multi-user workloads.

---

### **5. Stability & Regressions**  
| Severity | Issue | Description | Fix Status |
|---------|------|-------------|------------|
| 🔴 High | [#19558](https://github.com/ollama/ollama/issues/19558) | `llama-server` crashes with `NTSTATUS 0xe06d7363` on AMD Renoir iGPU (Vulkan backend) at model load — affects all models, even small ones (~380MB). | ❌ Open; critical for Linux/Windows APU users |
| 🔴 High | [#19551](https://github.com/ollama/ollama/issues/19551) | MLX runner ignores `stop_sequences`, causing infinite generation despite client-side constraints. | ✅ **PR #19552** submitted and ready for review |
| 🟡 Medium | [#19549](https://github.com/ollama/ollama/issues/19549) | Gemma 4 silently drops incomplete tool call arguments (missing opening `<|"|>`) — leads to missing `tool_calls`. | ✅ **PR #19556** proposed to repair trailing delimiters |
| 🟡 Medium | [#19492](https://github.com/ollama/ollama/issues/19492) | Responses API silently truncates oversized input even when `truncation=false` — breaks context integrity. | ✅ **PR #19501** merged to forward `Truncation` option correctly |

---

### **6. What This Means for Application Developers**  
- **Agent Builders**: Use `hs-decide-1-flash` via `#19555` to offload lightweight decision logic directly in your local stack, reducing reliance on remote APIs.  
- **Tool Call Reliability**: Be cautious with `qwen3.8:27b-mlx` and `gemma4` — both exhibit known issues with `tool_calls` and partial argument handling. Always validate output structure and use `stop_sequences` carefully until `#19552` lands.  
- **Client Compatibility**: Ensure your app handles `GET /v1/models` returning `[]` instead of `null` — this is now standard per merged fix (`#19539`).  
- **Deployment Stability**: Avoid AMD Renoir iGPUs (Ryzen 4700U) with Vulkan backend until `#19558` is resolved. For Intel Arc users, expect delays in native GPU acceleration until `#1590` progresses.  
- **Model Versioning**: Note that model digests may shift unexpectedly after re-pulls (`#19518`) — verify hashes before relying on immutable references.

---  
*Digest generated from GitHub activity: ollama/ollama — 2026-11-06*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-11-06**

---

### **1. Today's Highlights**  
The latest release, **v1.108.1**, introduces critical security hardening with signed Docker images via Cosign, verified against commit `e82a3c6`. Key improvements include enhanced streaming reliability for OpenRouter and Anthropic clients, better handling of structured outputs on AWS Bedrock (Opus 5.5, Sonnet 5, etc.), and fixes for token counting, S3 presigned URL parsing, and model catalog consistency. These changes strengthen both security and correctness in production inference pipelines.

---

### **2. Releases & Breaking Changes**  
- **v1.108.1**: All Docker images are now signed using [Cosign](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065). Verify signatures using `cosign verify` to ensure image integrity.  
- **Breaking**: PR #50318 introduces a new model catalog fetch mechanism for Codex — the proxy now pulls the model list from the *exact* Codex release’s GitHub repo instead of a static fallback. This ensures version parity but may require updates to client-side expectations if relying on outdated catalogs.  
- **Breaking**: PR #47496 restricts S3 bucket access in Bedrock batch jobs to only those explicitly allowed by admin config — previously, any bucket name could be used. Clients must now align with configured buckets.

---

### **3. New Model & Hardware Support**  
- **Bedrock Native Structured Output**: PR #50221 enables native JSON schema output for `anthropic/claude-opus-5-5`, `fable-5.1`, `sonnet-5`, and `haiku-5.5`—replacing fallback tool-based routing. This improves cost efficiency (20–40% cheaper) and latency while maintaining fidelity.  
- **OpenRouter Video API**: PR #50319 adds first-class support for video generation fields (`first_frame`, `reference_image`, `duration`, `seed`, etc.) under `/v1/videos`, enabling direct use of OpenRouter’s standard spec without provider-specific wrappers.  
- **ModelScope Image Generation**: PR #48558 resolves async image generation issues by switching from synchronous polling to async task waiting, making end-to-end image editing work correctly across ModelScope endpoints.

---

### **4. Performance & Optimization**  
- **Latency & Overhead**: PR #49920 enhances `litellm_overhead_latency_metric` by measuring per-chunk processing overhead during streaming requests—improving observability of gateway-level latency.  
- **Routing Efficiency**: PR #50321 ensures `/v1/responses` requests with prompt caching stay pinned to the same deployment, preventing cache misses due to load-balancer drift.  
- **Rate Limiting Accuracy**: PR #50320 corrects RPM/TPM enforcement in `usage-based-routing-v2` by counting limits once per deployment, avoiding premature 429s caused by over-counting.  
- **TTS Latency Reduction**: PR #50237 routes Cloud TTS audio to regional endpoints (e.g., `eu` or `us`) instead of global ones, reducing cross-region egress and improving compliance in EU/GDPR environments.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR | Notes |
|------|----------|--------|--------|-------|
| #50156: OpenRouter stream emits empty post-finish choice | High | Open | N/A | Causes incorrect usage tracking and breaks downstream parsers. |
| #49711: Anthropic SSE keepalive masks upstream failures | High | Open | N/A | Once a `ping` is sent, errors can’t be surfaced properly — risk of silent failure. |
| #47327: `image_detail="original"` not supported in token counter | Medium | Open | N/A | Blocks pre-call validation; requires manual patch until fixed. |
| #50173: Vertex AI Gemini `/v1/images/edits` returns 500 with no image | Medium | Open | N/A | Should return 400; regression after prior fix (#43787). |
| #48813: OCI Gemini ignores all but last system message | High | Closed | #48813 | Breaks agent workflows relying on multi-system-message patterns. |
| #47046: Vertex/Gemini reorders messages with non-prefix `cache_control` | High | Open | N/A | Can cause logic errors in agents relying on conversation order. |

> ✅ **Fixed in v1.108.1**: Several stability issues resolved including `token_counter` raising on `input_audio`, and `reasoning_effort="none"` being ignored on default-on Claude models (PR #40188, #47924).

---

### **6. What This Means for Application Developers**  
- **Security First**: Always verify Docker image signatures using Cosign. Ensure your CI/CD pipeline includes `cosign verify` steps to prevent supply-chain attacks.  
- **Streaming Reliability**: If using Anthropic or OpenRouter with long-running streams, avoid relying on `sse_keepalive_ping_interval_seconds` unless you handle downstream error propagation manually. Monitor for silent failures.  
- **Agent Design**: Be cautious with `system` messages in Vertex AI and OCI Gemini — earlier ones may be silently dropped. Use `cache_control` only at the start of conversations.  
- **Cost Control**: Leverage native structured output on Bedrock (via PR #50221) to reduce costs and improve performance. Avoid forcing JSON tools where unnecessary.  
- **S3 Security**: If using Bedrock batch jobs, update your SDK calls to respect admin-configured S3 buckets. Do not assume arbitrary bucket names will be accepted.  
- **Tooling**: For agents using MCP tools, expect improved error propagation (PR #49359), but monitor for missing `is_error` flags in edge cases.

👉 **Recommended Actions**: Update to v1.108.1 immediately for security and stability. Audit SSO, S3, and streaming configurations. Review model catalogs if using Codex or custom providers.  

🔗 Full details: [GitHub Issues](https://github.com/BerriAI/litellm/issues) | [Pull Requests](https://github.com/BerriAI/litellm/pulls)

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-06**

---

### **1. Today's Highlights**  
The Unsloth team continues to advance core inference and training optimizations, with critical fixes for GPU memory management and pipeline parallelism across CPU/GPU backends. Key PRs address OOM risks in diffusion training (MiniMax-H3, Lumina-Image-2.0) and improve context handling for long conversations. Meanwhile, UI stability improvements target Linux WebKitGTK lag and floating panel behavior.

---

### **2. Releases & Breaking Changes**  
*None*  
No new releases were published in the last 24 hours. No breaking API or config changes reported.

---

### **3. New Model & Hardware Support**  
- ✅ **Lumina-Image-2.0**: Added measured activation pricing and residency tracking for stable-diffusion.cpp on 16–24 GB GPUs ([PR #16808](https://github.com/unslothai/unsloth/pull/16808)).  
- ✅ **MiniMax-H3**: Improved memory planning to prevent OOM kills during rendering on L4 cards by accounting for host reserve ([PR #16849](https://github.com/unslothai/unsloth/pull/16849)).  
- ✅ **Integrated GPU GGUF loading**: New support for memory-mapped oversized GGUF models via disk paging ([PR #16857](https://github.com/unslothai/unsloth/pull/16857)) — crucial for systems like RTX Spark N1X.  
- ✅ **MLX In-Process Tool Calls**: Fixed incorrect tool call formatting for GLM, MiniMax, Kimi, DeepSeek, and Seed-OSS models on macOS ([PR #16824](https://github.com/unslothai/unsloth/pull/16824)).

---

### **4. Performance & Optimization**  
- ⚡ **Diffusion Training Speedups**:  
  - Eliminated per-micro-step host syncs in LoRA training ([PR #16763](https://github.com/unslothai/unsloth/pull/16763)), reducing overhead.  
  - Enabled W4A16 auto step protection on sm_100-class cards (B200, GB200, B300), improving stability without performance loss.  
- ⚡ **RAG Ingestion Efficiency**:  
  - Identified batch-of-1 embedding inefficiency: ~99 ms/chunk vs. 2.07 ms when properly batched ([Issue #16715](https://github.com/unslothai/unsloth/issues/16715)). Fix pending.  
- ⚡ **Long Chat Rendering**:  
  - Reduced scrolling lag on Linux by parking offscreen content behind placeholders while preserving React state ([PR #16854](https://github.com/unslothai/unsloth/pull/16854)).  
- ⚡ **Pipeline Parallelism**:  
  - Fixed silent fallback of chunked cross entropy on CPU backend ([PR #16846](https://github.com/unslothai/unsloth/pull/16846)); now runs on last stage correctly.  
  - Prevented MoE router aux loss accumulation across micro-batches under `UNSLOTH_PP_SIZE` ([PR #16822](https://github.com/unslothai/unsloth/pull/16822)).

---

### **5. Stability & Regressions**  
- 🔥 **Critical UI Lag in Long Conversations**:  
  Reported in both web and desktop apps; caused by unbounded style/layout computation on long chats ([Issue #16603](https://github.com/unslothai/unsloth/issues/16603)).  
- 🔥 **Glass Theme Bugs**:  
  - Background blur applied globally despite intended transparency ([Issue #16820](https://github.com/unslothai/unsloth/issues/16820)).  
  - Inconsistent theme application across UI elements ([Issue #16770](https://github.com/unslothai/unsloth/issues/16770)).  
- 🔥 **Image Generation Fallback to CPU on Windows**:  
  Despite RTX 3060 GPU, image generation falls back to CPU/RAM ([Issue #16803](https://github.com/unslothai/unsloth/issues/16803)).  
- 🔥 **Download Resume Failure**:  
  Downloads do not resume after interruption ([Issue #16810](https://github.com/unslothai/unsloth/issues/16810)).  
- 🛠️ **Fixes in Progress**:  
  - Memory plan refinement for DiT forward headroom ([PR #16805](https://github.com/unslothai/unsloth/pull/16805)).  
  - Floating browser window repositioning and resizing improvements ([PR #16859](https://github.com/unslothai/unsloth/pull/16859)).

---

### **6. What This Means for Application Developers**  
- **Build robust agents with dynamic context handling**: The new `Context Window Temporary Quick Boost` feature proposal ([Issue #16860](https://github.com/unslothai/unsloth/issues/16860)) signals future support for adaptive context scaling — ideal for long-running agents.  
- **Leverage native memory-aware training**: Use `UNSLOTH_PP_SIZE` safely with MoE models thanks to improved aux loss handling ([PR #16822](https://github.com/unslothai/unsloth/pull/16822)).  
- **Design for low-memory environments**: The new integrated GPU GGUF paging support ([PR #16857](https://github.com/unslothai/unsloth/pull/16857)) enables deployment on devices without dedicated VRAM.  
- **Prioritize UI performance on Linux**: Use `placeholders` for offscreen chat content to avoid WebKitGTK stalls ([PR #16854](https://github.com/unslothai/unsloth/pull/16854)).  
- **Monitor RAG ingestion efficiency**: Avoid per-document embedding bottlenecks by batching at the corpus level until fix lands ([Issue #16715](https://github.com/unslothai/unsloth/issues/16715)).  

> 💡 *Recommendation*: For production deployments, enable `context_parallel` only on GPU-capable hosts — it’s silently disabled on CPU due to current limitations ([PR #16802](https://github.com/unslothai/unsloth/pull/16802)).

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*