# AI Infrastructure Digest 2026-11-07

> Generated: 2026-11-07 02:20 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-07**

---

### **1. Ecosystem Overview**  
The AI inference and serving ecosystem is entering a phase of hyper-specialization, with projects diverging along hardware, model architecture, and deployment pattern lines. vLLM and SGLang are pushing the boundaries of high-throughput, low-latency inference on next-gen GPUs (AMD MI355X, NVIDIA SM120), while llama.cpp and Ollama emphasize cross-platform portability and local runtime efficiency. LiteLLM consolidates as the de facto gateway for multi-provider orchestration, enabling agent workflows across diverse backends. The emergence of DSA (Dynamic Sparse Attention), FP8 MLA, MoE sharding, and disaggregated prefill/decode patterns signals a shift toward intelligent resource allocation rather than brute-force scaling.

---

### **2. Activity Comparison**

| Project      | Issues Open (Last 24h) | PRs Merged (Last 24h) | Release Status       |
|--------------|------------------------|------------------------|----------------------|
| **vLLM**     | 6                      | 8                      | None                 |
| **SGLang**   | 9                      | 7                      | None                 |
| **llama.cpp**| 5                      | 6                      | b12180 (minor)       |
| **Ollama**   | 7                      | 1                      | None                 |
| **LiteLLM**  | 5                      | 5                      | v1.108.2 (LTS) ✅    |
| **Unsloth**  | 6                      | 5                      | v0.1.916-beta (beta) |

> 🔍 *Insight:* LiteLLM leads in stability-driven releases, while vLLM and SGLang dominate in active development velocity. Ollama shows reduced PR throughput despite critical stability issues—indicating potential bottleneck in triage or contributor engagement.

---

### **3. Model Support Race**

| New Model / Architecture         | Supported By                          | Key Differentiators |
|----------------------------------|----------------------------------------|---------------------|
| **Kimi-K3.1 / Kimi-K3.1-Pro**    | SGLang ✅, llama.cpp ✅, Unsloth ✅      | SGLang: full MoE sharding + HiSparse; llama.cpp: DSA-based attention; Unsloth: video continuation support |
| **DeepSeek-V4/V4.1**             | vLLM ✅, SGLang ✅, Ollama ✅            | vLLM: ROCm optimization + MegaMoE locality; SGLang: PD disaggregation |
| **NVIDIA Nemotron 3.5 ASR Streaming** | vLLM ✅ (feature request)           | Only project with explicit ASR streaming support |
| **GLM-5.3-Flash / GLM-5.3**      | SGLang ✅, Ollama ✅                    | SGLang: reasoning corruption bug; Ollama: MLX backend experimental |
| **Cosmos3-Nano Video Continuation** | Unsloth ✅                            | Exclusive to Unsloth; enables video extension from frame boundaries |
| **Ascend NPU Diffusion**         | SGLang (RFC) ✅                         | Proposing MindIE-SD integration — unique to SGLang’s diffusion roadmap |

> 🏆 **Leaderboard**: **SGLang** leads in architectural innovation (MoE sharding, HiCache, DFlash), followed closely by **vLLM** in hardware-specific optimizations. **llama.cpp** excels in open-source model availability and GPU portability.

---

### **4. Performance Frontier**

| Optimization Focus          | Leading Projects                     | Key Developments |
|------------------------------|---------------------------------------|------------------|
| **KV Cache & Memory Bandwidth** | vLLM ✅, SGLang ✅, llama.cpp ✅       | vLLM: bounded FP8→BF16 scratch conversion (+10.1%); SGLang: HiCache pool control; llama.cpp: ClassLRU MoE cache |
| **Speculative Decoding**     | vLLM ✅, SGLang ✅                     | vLLM: `load_plan` API abstraction; SGLang: side-stream context projection + one-kernel verify |
| **Quantization & Precision** | vLLM ✅, llama.cpp ✅, LiteLLM ✅       | vLLM: FP8 MLA on gfx1250; llama.cpp: small-n GEMV variants; LiteLLM: MCP per-server schema caching |
| **Distributed Serving**      | vLLM ✅, SGLang ✅                     | vLLM: direct weight reload hardening; SGLang: PD disaggregation, pipeline-parallel prefill |
| **Kernel-Level Optimization**| vLLM ✅, SGLang ✅, llama.cpp ✅        | vLLM: FlashInfer prefill; SGLang: radix-4 router fusion; llama.cpp: Vulkan CoopMat support |

> 💡 **Trend**: The frontier has shifted from raw throughput to **context-aware, memory-efficient execution** — especially for long-context and multimodal models.

---

### **5. Layer Positioning**

| Project      | Primary Layer                | Role Summary |
|--------------|-------------------------------|-------------|
| **vLLM**     | **Serving Engine**            | High-performance, CUDA-optimized inference engine; targets cloud-scale deployments |
| **SGLang**   | **Serving Engine + Gateway**  | Specialized for speculative decoding and agent workflows; supports multiple backends via unified API |
| **llama.cpp**| **Local Runtime / Edge Engine** | CPU/GPU-native inference with minimal dependencies; ideal for offline/local deployment |
| **Ollama**   | **Gateway + Local Runtime**   | User-friendly CLI/API layer; bridges models to apps via lightweight server; strong focus on developer experience |
| **LiteLLM**  | **API Gateway / Orchestration** | Multi-provider routing, cost control, structured output handling; acts as middleware for agents |
| **Unsloth**  | **Agent UI + Runtime Platform** | Full-stack tooling for agents and multimodal workflows; includes studio, plugins, and visualization |

> 🧩 **Strategic Insight**: The stack is clearly bifurcating: **engine-level** (vLLM/SGLang), **runtime-level** (llama.cpp/Ollama), **orchestration-level** (LiteLLM), and **agent UX-level** (Unsloth).

---

### **6. Trend Signals**

- **Hardware Specialization Is Accelerating**: AMD MI355X and Intel Arc B70 support are now key differentiators — vLLM and SGLang lead here, but llama.cpp and Ollama are catching up.
- **MoE & Disaggregation Are Mainstream**: Both vLLM and SGLang have made significant progress in MoE sharding, TP/DP decomposition, and prefill/decode separation — essential for scaling large models.
- **Agent Workflows Demand Integrated Tooling**: Unsloth’s Claude Agent SDK, LiteLLM’s structured output fixes, and SGLang’s reasoning parser improvements signal that **tool calling reliability** is now a core requirement.
- **Stability > Speed in Production**: Despite rapid feature growth, all projects report critical regressions (memory corruption, crashes, silent data loss). Developers must prioritize **LTS versions**, **observability tools**, and **local testing**.
- **Security & Compliance Are Rising**: LiteLLM now includes prompt redaction, token usage masking, and sessionless cost tracking — reflecting growing demand for auditability.

> 🚨 **Developer Action Items**:
> - Use **v1.108.2 (LiteLLM LTS)** immediately for agent stability.
> - Avoid `OLLAMA_NUM_PARALLEL` with multimodal models until fixed.
> - Monitor `--hicache-mamba-size-gb` and `VLLM_RELOAD_MODE=direct` for silent failures.
> - Leverage **benchmarking tools** (Unsloth, vLLM SIG Roadmaps) before deploying new models at scale.

---

✅ **Final Takeaway**: The AI infrastructure landscape is no longer about choosing "the fastest" engine — it's about selecting the right **layered, resilient, and observable stack** tailored to your deployment profile: cloud-scale (vLLM/SGLang), edge-local (llama.cpp/Ollama), agent-native (Unsloth), or multi-cloud (LiteLLM).

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM Digest – 2026-11-07**

---

### **1. Today's Highlights**  
The vLLM project continues to accelerate its support for next-generation hardware and speculative decoding workflows, with key PRs landing for **ROCm optimization on DeepSeek-V4/V4.1**, **Kimi-K3 FP8 MLA support on gfx1250**, and **direct weight reload hardening**. Critical bugfixes address memory safety issues in quantized MoE models and FP8 KV cache handling, while progress on the Rust frontend and `load_plan` API signals deeper engine abstraction.

---

### **2. Releases & Breaking Changes**  
None. No new releases or breaking changes were published in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- ✅ **DeepSeek-V4/V4.1** now receives ROCm-specific performance optimizations via `stacked context WKV projections` ([PR #66212](https://github.com/vllm-project/vllm/pull/66212)) and **opt-in locality placement for MegaMoE weights** ([PR #64835](https://github.com/vllm-project/vllm/pull/64835)).  
- ✅ **Kimi-K3 (FP8 MLA)** gains full support on **gfx1250 (AMD MI355X)** with dedicated prefill/decode kernels ([PR #66208](https://github.com/vllm-project/vllm/pull/66208)).  
- ✅ **NVIDIA Nemotron 3.5 ASR Streaming (0.6b)** is now supported as a feature request ([Issue #47455](https://github.com/vllm-project/vllm/issues/47455)), enabling multilingual streaming speech recognition.  
- ✅ **MiMo-V2 omni** video prompt labeling corrected to reflect actual frame timestamps ([PR #66201](https://github.com/vllm-project/vllm/pull/66201)).

---

### **4. Performance & Optimization**  
- 🔥 **Native FlashInfer prefill** optimized with bounded FP8 → BF16 scratch conversion, improving input-token throughput by up to **10.1%** on SM120 ([PR #64942](https://github.com/vllm-project/vllm/pull/64942)).  
- 🚀 **MoE performance**: TRT-LLM monolithic MoE kernels now skip DP padding rows using vLLM’s top-k routing, reducing unnecessary computation ([PR #65972](https://github.com/vllm-project/vllm/pull/65972)).  
- ⚙️ **Encoder CUDA graphs** refined to use one replay per batch with finer-grained budgeting, avoiding oversubscription and latency spikes ([PR #65227](https://github.com/vllm-project/vllm/pull/65227)).  
- 📈 **ReplaySSM** remains under RFC ([Issue #47572](https://github.com/vllm-project/vllm/issues/47572)), aiming to reduce decode kernel memory bandwidth pressure in Mamba2/GDN models.

---

### **5. Stability & Regressions**  
- ❌ **Critical Memory Corruption**: `Qwen3.8-27B-GDN` with PP=3 on sm_86 exhibits **illegal memory access (Xid 31)** due to prefix caching regression since v0.28.0 — confirmed in v0.31.0 and v0.32.0 ([Issue #63505](https://github.com/vllm-project/vllm/issues/63505)). *No fix PR yet.*  
- ❌ **MoE Output Non-Determinism**: Marlin MoE output depends on intra-expert token ordering — a serious correctness issue affecting reproducibility ([Issue #52525](https://github.com/vllm-project/vllm/issues/52525)). *PR pending.*  
- ⚠️ **TP Worker Hang**: TP=8 setup with DeepSeek-V4-Pro + speculative decoding causes `EngineDeadError` due to RPC timeout during sampling ([Issue #41530](https://github.com/vllm-project/vllm/issues/41530)). *High severity; no fix PR.*  
- 🛠 **Bugfixes landed today**:  
  - Fixed incorrect FP4 scale group handling during TP splits ([PR #66213](https://github.com/vllm-project/vllm/pull/66213)).  
  - Corrected dequant identity dtype mismatch in activation path ([PR #66209](https://github.com/vllm-project/vllm/pull/66209)).  
  - Fixed misaligned packed weight transfers in NCCL/IPC paths ([PR #65487](https://github.com/vllm-project/vllm/pull/65487)).

---

### **6. What This Means for Application Developers**  
- If you're deploying **Kimi-K3**, **DeepSeek-V4**, or **GLM-5.3** on AMD GPUs, ensure you’re using recent builds with ROCm patches ([PR #66212](https://github.com/vllm-project/vllm/pull/66212), [PR #64835](https://github.com/vllm-project/vllm/pull/64835)) to avoid performance cliffs.  
- For **speculative decoding at scale**, avoid `--max-model-len` values that trigger the `Qwen3.8-27B` illegal memory access until the fix lands.  
- Use `VLLM_RELOAD_MODE=direct` with caution — recent PRs enforce opt-in for safe reloading ([PR #66002](https://github.com/vllm-project/vllm/pull/66002)) to prevent silent model corruption.  
- Consider enabling `enable_batch_invariant_mode` on Hopper/Blackwell, but be aware it currently routes Linear layers to Triton unnecessarily ([Issue #65569](https://github.com/vllm-project/vllm/issues/65569)).  

> 🔗 **Pro Tip**: Monitor [vLLM SIG Roadmaps](https://github.com/vllm-project/vllm/issues/61024) for Q4 2026 plans around Rust frontend parity, load plan standardization, and BYOG benchmarking.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-11-07

---

### **1. Today's Highlights**  
The SGLang project continues to accelerate its hardware and model specialization, with major performance optimizations for Kimi-K3.1 on AMD and Blackwell GPUs, including MoE layer sharding and fused kernels. Critical CI stability issues—particularly flaky tests and CUDA coredumps—are being actively tracked, while new PRs focus on refining speculative decoding, diffusion inference, and cross-platform support (XPU, Apple Silicon).

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- **Kimi-K3.1**: Full model support added via [PR #48414](https://github.com/sgl-project/sglang/pull/48414), including DFlash speculative decoding, PD disaggregation, HiSparse, and pipeline-parallel prefill.  
- **AMD (MI355X/MI45x)**: Performance improvements for Kimi-K3.1 via [PR #43126](https://github.com/sgl-project/sglang/pull/43126) and [PR #44362](https://github.com/sgl-project/sglang/pull/44362), enabling FP8 KDA in-proj absorption and radix-4 router fusion.  
- **Intel XPU (Battlemage)**: [PR #48787](https://github.com/sgl-project/sglang/pull/48787) enables W8A16 weight-only FP8 execution using `sgl_kernel.fp8_linear` for better performance.  
- **Apple Silicon (MLX)**: [RFC #32321](https://github.com/sgl-project/sglang/issues/32321) outlines a redesign to support Torch-owned SRT path with exported MLX model regions.  
- **Ascend NPU**: [RFC #48582](https://github.com/sgl-project/sglang/issues/48582) proposes integrating MindIE-SD as an optional acceleration library for SGLang Diffusion.

---

### **4. Performance & Optimization**  
- **Kimi-K3.1 (AMD)**: MoE front/back sharding reduces memory bandwidth pressure; [PR #48584](https://github.com/sgl-project/sglang/pull/48584) shows up to **~1.5x speedup** by avoiding redundant data reads across TP ranks.  
- **DeepSeek-V4 (SM120)**: Throughput remains suboptimal (~2–7K tok/s vs ~12.5K in vLLM/Marlin) on 4× RTX PRO 6000 — [Issue #33422](https://github.com/sgl-project/sglang/issues/33422) highlights need for SM120 kernel optimization.  
- **Speculative Decoding**: Multiple PRs optimize critical path:  
  - [PR #48948](https://github.com/sgl-project/sglang/pull/48948): Side-stream context projection + one-kernel verify commit reduces latency.  
  - [PR #48949](https://github.com/sgl-project/sglang/pull/48949): Vocabulary-tiled final-token pass improves small-batch efficiency.  
- **Diffusion Inference**: [PR #48903](https://github.com/sgl-project/sglang/pull/48903) fixes floating-point drift in `kandinsky6-sr` decode parallel test, ensuring reproducibility.  
- **HiCache**: [PR #47867](https://github.com/sgl-project/sglang/pull/47867) introduces `--hicache-mamba-size-gb` to pin Mamba host pool size, preventing cache misses due to undersized pools.

---

### **5. Stability & Regressions**  
- **CUDA Coredump Tracker (#26340)**: 356 comments; auto-collected from `pr-test.yml`. High-severity issue affecting CI stability — no fix yet.  
- **Flaky CI Tests (#42752)**: 186 comments; recurring failures in NVIDIA `PR Test Base/Extra` runs during babysitting workflows.  
- **Critical Bugs**:  
  - GLM-5.3-Flash reasoning output degenerates into repeated `!` under multi-tool prompts ([#36669](https://github.com/sgl-project/sglang/issues/36669)).  
  - `/v1/chat/completions` silently drops `cache_salt` field when passed through model gateway ([#48793](https://github.com/sgl-project/sglang/issues/48793)).  
  - Reasoning parser incorrectly splits on quoted `</think>` tags, truncating CoT content ([#48791](https://github.com/sgl-project/sglang/issues/48791)).  
- **Regression in Diffusion Suite**: `multimodal-gen-test-2-gpu` fails due to tensor mismatch after test inclusion ([#48892](https://github.com/sgl-project/sglang/issues/48892)) — fixed in [PR #48903](https://github.com/sgl-project/sglang/pull/48903).  

---

### **6. What This Means for Application Developers**  
- **Expect higher throughput on Kimi-K3.1 and DeepSeek-V4** with recent optimizations, but monitor SM120 performance gaps.  
- Use `--hicache-mamba-size-gb` to avoid silent HiCache misses in hybrid models.  
- Avoid `cache_salt` in `/v1/chat/completions` if routing through the model gateway — it will be dropped.  
- For agent systems using complex tool calling, test GLM-5.3-Flash carefully due to known reasoning corruption.  
- Enable `--enable-layernorm-sp` and use `--layernorm-sp-min-tokens` to optimize extend passes on large models.  
- Watch CI health: expect flakiness in PR reviews; consider running local tests before submission.  

> 🔗 *For real-time updates, follow: [SGLang GitHub Issues](https://github.com/sgl-project/sglang/issues)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-07**

---

### **1. Today's Highlights**  
The latest updates focus on expanding hardware support for Intel Arc GPUs (Vulkan/CoopMat), introducing the new **Kimi-K3.1-Pro** model with DSA-based attention, and advancing disaggregated inference capabilities via `llama-server`’s `--prefill-budget-busy` and prefill/decode separation. Key performance improvements landed for AMD RDNA4 (gfx1201) and OpenCL/MoE quantizations, while stability fixes address critical crashes in UTF-8 handling and GPU memory management.

---

### **2. Releases & Breaking Changes**  
No breaking changes or major version releases were announced today. However, **b12180** introduces a new model (`Kimi-K3.1-Pro`) and includes several backend optimizations. The `--cache-disk` feature request (#20697) remains closed but actively discussed—no immediate API change expected.

> 🔗 [GitHub Release b12180](https://github.com/ggml-org/llama.cpp/releases/tag/b12180)

---

### **3. New Model & Hardware Support**  
- ✅ **New Model**: `Kimi-K3.1-Pro` added with **DSA (Dynamic Sparse Attention)** replacing full-attention MLA layers; features top-k masking and 32-head × 128 context support.
  > 🔗 [PR #31779: Add Kimi-K3.1-Pro](https://github.com/ggml-org/llama.cpp/pull/31779)
- 🚀 **Hardware**: Enhanced support for **Intel Arc Pro B70 (BMG G31)** and **AMD Strix Halo (gfx1151/gfx1201)** via Vulkan/ROCm.
  - Vulkan now detects ReBAR support to avoid BAR allocation failures (#31658).
  - ROCm/HIP builds now use fixed 32MB BLAS workspace instead of per-call allocation (#31838).
- 🖥️ **Backends**: OpenCL now supports `repeat` operations for `f16`, `i32`, `i16`, and `bf16` (#31782). WebGPU adds batched tensor APIs (`get_tensor_n` / `set_tensor_n`) (#31473).

---

### **4. Performance & Optimization**  
- ⚡ **Flash Attention (FA)**: Optimized Intel FA kernel for prefill on Vulkan (#29357); future gains expected from `readonly` buffer declarations (#31833).
- 💾 **MoE Cache**: Hybrid LRFU/MQ-like cache (`ClassLRU`) introduced (#31233), improving expert routing efficiency on large MoE models.
- 📈 **Quantization & Spec Verification**: Small-n GEMV variants added for `q8_0`, `q4_k`, and `q5_1` to improve speculative decoding accuracy (#31831).
- 🧠 **CPU Optimization**: KleidiAI SME2 kernels now used for CPU Flash Attention (#29394); Kronecker product support expanded to non-power-of-two dims (#28490).
- 🛠️ **Graph Optimization**: Gemma 4 E2B/E4B models see massive prefill speedups via layer pruning and prompt token elimination (#31356).

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR? | Notes |
|------|----------|--------|---------|-------|
| [#31560](https://github.com/ggml-org/llama.cpp/issues/31560) | Critical | Open | ❌ | Intel Arc B70: KHR_coopmat causes nondeterministic results (F16/Q8_0) |
| [#27638](https://github.com/ggml-org/llama.cpp/issues/27638) | High | Open | ❌ | Vulkan ANV: Flash Attention fallback → O(N²) PP degradation + device loss |
| [#31538](https://github.com/ggml-org/llama.cpp/issues/31538) | Medium | Open | ❌ | iGPU: CPU-resident layers consume device memory via Vulkan host buffers |
| [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) | High | Open | ❌ | SYCL: `--cpu-moe -sm layer` stalls at 40,000 MiB MoE cache |
| [#27725](https://github.com/ggml-org/llama.cpp/issues/27725) | Medium | Open | ❌ | Memory leak during eval (CUDA) |

> 🔍 *Note:* Several regressions are tied to recent flash attention and MoE optimizations — users should monitor test suites before production deployment.

---

### **6. What This Means for Application Developers**  
- **Disaggregated Inference is Now Feasible**: With `server : disaggregated prefill/decode support` (#21266) gaining traction, developers can now architect scalable, low-latency agents using separate prefill/decode nodes.
- **Hybrid Caching Strategy**: Use `--cache-ram` + `--cache-idle-slots` cautiously—current implementation has race conditions under concurrent load (#27148). Consider disabling until fix lands.
- **Model Choice Matters**: Kimi-K3.1-Pro’s DSA architecture enables faster long-context inference—ideal for high-throughput RAG pipelines.
- **Avoid Unstable Backends**: Avoid `KHR_coopmat` on Intel Arc B70 (issue #31560); prefer `--no-coopmat` until driver fixes.
- **Optimize for MoE**: Use `ClassLRU` cache and `--split-mode layer` carefully—avoid `--cpu-moe` on multi-GPU setups due to severe performance drops (#31510).

> 📌 **Actionable Tip**: For real-time apps, enable `--prefill-budget-busy` (#30992) to prevent idle GPU cycles during high-load prefill bursts.

---  
*Digest generated from GitHub data: [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# **Ollama Digest – 2026-11-07**

---

### **1. Today's Highlights**  
Critical stability issues emerged today affecting Vulkan backend users on AMD and Intel GPUs, with `llama-server` crashing at model load due to unhandled C++ exceptions (e.g., `NTSTATUS 0xe06d7363`, `device lost`). A regression in CUDA 13 support also caused "unspecified launch failure" crashes during long grammar-constrained generations. Meanwhile, a high-priority fix was merged to prevent VRAM leaks when `keep_alive` expires and runners fail to terminate.

---

### **2. Releases & Breaking Changes**  
None reported in the last 24 hours.

---

### **3. New Model & Hardware Support**  
- **MLX Backend**: Added experimental support for dense Nemotron/Luciole Safetensors models (`#19566`) — though current MLX runner does not recognize the `NemotronForCausalLM` architecture.  
- **CUDA 13.5 + SM 103**: ARM64 support expanded with new CUDA 13.5 and compute capability 103 (SM 103) added via `#19580`.  
- **Vulkan Backends**: Ongoing work continues to stabilize Vulkan on AMD Renoir (gfx90c), Intel Arc 130V (Lunar Lake), and RX 5500 (gfx1010) devices — see `#19558`, `#19589`, `#19158`.

---

### **4. Performance & Optimization**  
- **MLX Concurrency**: PR `#19197` enables concurrent generation via `OLLAMA_NUM_PARALLEL` for MLX models, improving first-token latency by reducing idle time between requests.  
- **Memory Efficiency**: PR `#18078` optimizes Qwen3.8 Flash Next on MLX by retaining BF16 precision for key layers while using MXFP8 quantization elsewhere, balancing quality and memory usage.  
- **Token Streaming**: Fix for whitespace handling after `</think>` in DeepSeek3 parser (`#19580`) improves output formatting consistency in streaming responses.

---

### **5. Stability & Regressions**  
| Severity | Issue | Impact | Status | PR / Link |
|--------|------|--------|--------|----------|
| 🔴 Critical | `llama-server` crashes on AMD Renoir iGPU (Ryzen 4700U) with `NTSTATUS 0xe06d7363` | Model load fails on all models; affects Vulkan backend | Open | [Issue #19558](https://github.com/ollama/ollama/issues/19558) |
| 🔴 Critical | `gemma4:12b` crashes on Intel Arc 130V (Lunar Lake) iGPU with `ggml_vulkan: device lost` | GPU offload fails post-model upload; regression since 0.40.x | Open | [Issue #19158](https://github.com/ollama/ollama/issues/19158) |
| 🔴 Critical | `cuda_v13`: Long grammar-constrained generation causes “CUDA error: unspecified launch failure” → Xid 79 (GPU off bus) | High-risk crash under sustained load | Open | [Issue #17026](https://github.com/ollama/ollama/issues/17026) |
| 🟡 High | `ollama serve` fails to terminate idle `llama-server` runners at `keep_alive` expiry | VRAM leak, leading to out-of-memory errors on subsequent loads | Fixed | [PR #19581](https://github.com/ollama/ollama/pull/19581) |
| 🟡 Medium | `OLLAMA_NUM_PARALLEL` causes image processing failures in multi-modal models (e.g., `llava`, `moondream`) | Request hang/freeze under parallelism | Open | [Issue #4165](https://github.com/ollama/ollama/issues/4165) |
| 🟡 Low | TLS handshake timeout on networks dropping X25519MLKEM768 ClientHello | Blocks `ollama pull` on privacy-focused or restrictive networks | Open | [Issue #19430](https://github.com/ollama/ollama/issues/19430) |

---

### **6. What This Means for Application Developers**  
- **Avoid CUDA 13.5** on NVIDIA GPUs if running long-form, grammar-constrained generations — use `cuda_v12` as fallback until `#17026` is resolved.  
- **Do not enable `OLLAMA_NUM_PARALLEL`** with multi-modal models until `#4165` is fixed — this may cause silent hangs or resource exhaustion.  
- **Monitor GPU memory carefully** on AMD/Intel integrated GPUs (Renoir, Lunar Lake) — Vulkan backend instability may lead to unexpected process crashes and persistent VRAM locks.  
- **Integrate observability tools early**: New community integrations like **HomeLab Monitor** (`#19586`) and **AI Meter** (`#19582`) provide real-time visibility into model load, VRAM, and token throughput — ideal for production monitoring.  
- **Expect transient API issues** on macOS: `/api/chat` intermittently fails with “broken pipe” after long replies (`#19293`) — implement retry logic with jitter.  

> ✅ **Actionable Tip**: Use `ollama stop all` (requested in `#6987`) to manage model state more reliably until implemented. Track progress via GitHub issue links above.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-11-07**

---

### **1. Today's Highlights**  
The latest LTS release, **v1.108.2**, stabilizes critical streaming and tooling behavior across Anthropic, Gemini, and Bedrock backends. Key fixes address corruption in `server_side_tool_invocations`, `reasoning_effort` misbehavior on Claude models, and a high-severity bug where encrypted reasoning was dropped during Azure-to-Responses routing. These improvements are vital for agent reliability and structured output fidelity.

---

### **2. Releases & Breaking Changes**  
- **v1.108.2 (LTS)**: Released as the new stable baseline. Use `ghcr.io/berriai/litellm:lts` to stay updated.  
  🔗 [Release Notes](https://github.com/BerriAI/litellm/releases/tag/v1.108.2)  
- **Backported Fix**: #50456 patches encrypted reasoning round-trip failure in `/v1/messages` for `chatgpt/*` models behind Anthropic, ensuring stateful tool calling works end-to-end.  
  🔗 [PR #50456](https://github.com/BerriAI/litellm/pull/50456)

> ✅ **Migration Note**: If using `chatgpt/*` models with tool calls and reasoning, upgrade to v1.108.2 or later to avoid silent state loss.

---

### **3. New Model & Hardware Support**  
- **NVIDIA NIM**: Catalog refresh now includes newly released NVIDIA NIM models (e.g., `nvidia/llama3-8b-instruct-nim`).  
  🔗 [PR #50439](https://github.com/BerriAI/litellm/pull/50439)  
- **Vertex AI Claude Partner Models**: Support for `vertex_ai/claude-*` now serializes `system` and `tools` before `messages` in request body — aligning with upstream expectations.  
  🔗 [PR #49645](https://github.com/BerriAI/litellm/pull/49645)  
- **Cohere Embeddings**: `COHERE_API_BASE` is now respected for embedding requests (previously ignored).  
  🔗 [Issue #50248](https://github.com/BerriAI/litellm/issues/50248), [PR #50248](https://github.com/BerriAI/litellm/pull/50248)

---

### **4. Performance & Optimization**  
- **Latency Reduction**: SAP GenAI Hub deployment URL discovery overhead (~870ms per call) addressed via caching in `litellm.acompletion()`.  
  🔗 [Issue #47096](https://github.com/BerriAI/litellm/issues/47096), [PR #47096](https://github.com/BerriAI/litellm/pull/47096)  
- **Memory Efficiency**: MCP (Model Context Protocol) now supports opt-in per-server memory of upstream tool schemas, reducing redundant API calls on sessionless requests.  
  🔗 [PR #50493](https://github.com/BerriAI/litellm/pull/50493)  
- **Streaming Efficiency**: Ollama streaming now assigns unique `index` per tool call, preventing JSON argument fusion.  
  🔗 [PR #49183](https://github.com/BerriAI/litellm/pull/49183)

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | PR / Fix |
|--------|------|--------|---------|
| 🚨 High | `stream_chunk_builder` corrupts `Gemini` `server_side_tool_invocations` → "Corrupted tool call context" on follow-up turns | Closed | [PR #50492](https://github.com/BerriAI/litellm/pull/50492) |
| 🚨 High | Encrypted reasoning dropped in Azure-to-Responses detour (`/azure/responses/...`) | Closed | [PR #50024](https://github.com/BerriAI/litellm/pull/50024) |
| 🟡 Medium | `reasoning_effort="none"` omits thinking instead of disabling it on default-on Claude models | Closed | [PR #50492](https://github.com/BerriAI/litellm/pull/50492) |
| 🟡 Medium | `Bedrock Converse` replays `signature` from `reasoningText` to non-Anthropic models → `Bad Request` | Closed | [PR #49969](https://github.com/BerriAI/litellm/pull/49969) |
| 🟡 Medium | `langfuse_otel` fails to serialize `Message` objects → missing token usage and trace data | Fixed | [PR #46845](https://github.com/BerriAI/litellm/pull/46845) |

> ⚠️ **Critical Note**: All major stability issues today were resolved via PRs merged or closed within 24h. Upgrade to v1.108.2 immediately.

---

### **6. What This Means for Application Developers**  
- **Agents & Tool Chains**: Ensure `reasoning_effort="none"` is properly enforced on Claude models; use v1.108.2 to avoid silent state retention.  
- **Structured Output Reliability**: Streaming tool calls with `Gemini`, `Anthropic`, or `Ollama` now preserve correct `index` and `arguments` — no more fused JSON payloads.  
- **Security & Compliance**: Guardrails now properly redact buffered stream content ([PR #50489](https://github.com/BerriAI/litellm/pull/50489)), and prompt storage can be disabled per key/team ([PR #50135](https://github.com/BerriAI/litellm/pull/50135)).  
- **Cost Control**: The cost map now includes `-latest` model aliases, improving budget accuracy for dynamic model routing.

🔧 **Action Item**: Update your LiteLLM proxy and SDK to **v1.108.2 (LTS)** to benefit from all stability and feature fixes. Use `ghcr.io/berriai/litellm:lts` for guaranteed compatibility.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-07**

---

### **1. Today's Highlights**  
The v0.1.916-beta release delivers major performance improvements across Unsloth, including faster typing and search in long chats, accelerated image/video loading, and support for two-GPU generation. UI enhancements now include per-chat separate browsers and floating window controls. Key stability fixes address glass theme inconsistencies, long-conversation lag, and model selection drift during multi-agent workflows.

---

### **2. Releases & Breaking Changes**  
- **v0.1.916-beta**: Released with large performance fixes and new UI features.  
  🔗 [GitHub Release](https://github.com/unslothai/unsloth/releases/tag/v0.1.916-beta)  
- **Breaking Change (in progress)**: `transformers` 5.23 renames `gradient_checkpointing` to `activation_checkpointing`, which affects behavior in unsloth’s Trainer logic. Requires upstream audit.  
  🔗 [Issue #16667](https://github.com/unslothai/unsloth/issues/16667)

---

### **3. New Model & Hardware Support**  
- **Cosmos3-Nano Video Continuation**: Added ability to extend video clips from last or first frames.  
  🔗 [PR #16741](https://github.com/unslothai/unsloth/pull/16741)  
- **Cosmos3 256p + Frame Range Support**: Now aligns with NVIDIA’s official model card specs; includes per-model frame rate picker.  
  🔗 [PR #16737](https://github.com/unslothai/unsloth/pull/16737)  
- **Claude Agent Integration (Experimental)**: Connects to Anthropic’s Agent SDK via local CLI login—no API key required. Supports Claude Pro and Max plans.  
  🔗 [PR #16963](https://github.com/unslothai/unsloth/pull/16963)  
- **Krea 2 Turbo Image Generation**: Now supported on Windows desktop (note: crashes reported under certain conditions).  
  🔗 [Issue #16892](https://github.com/unslothai/unsloth/issues/16892)

---

### **4. Performance & Optimization**  
- **Image Load Memory Management**: Studio now returns freed heap memory to OS after image loads. Reduces host RAM pressure.  
  🔗 [PR #16934](https://github.com/unslothai/unsloth/pull/16934)  
- **Diffusion Autotune Cache**: On warm restarts, compiled diffusion kernels no longer re-benchmark previously tuned Triton reductions (saving ~9.5s on L4).  
  🔗 [PR #16981](https://github.com/unslothai/unsloth/pull/16981)  
- **RAG Ingestion Bottleneck**: Currently embeds documents one at a time (batch-of-1), causing ~99ms/chunk vs. 2.07ms when batched.  
  🔗 [Issue #16715](https://github.com/unslothai/unsloth/issues/16715)  
- **Benchmarks Page (Under Development)**: Proposed tabular sweeps of speculative decoding, KV cache types, and RAM offload settings on GGUF models.  
  🔗 [PR #11808](https://github.com/unslothai/unsloth/pull/11808)

---

### **5. Stability & Regressions**  
| Severity | Issue | Status | Fix PR |
|---------|------|--------|--------|
| Critical | Glass theme causes full background blur (desktop/web) | Closed | [Issue #16820](https://github.com/unslothai/unsloth/issues/16820) |
| High | Long conversations cause UI lag (web + desktop) | Closed | [Issue #16603](https://github.com/unslothai/unsloth/issues/16603) |
| High | `search_conversation` tool disappeared (3rd recurrence) | Open | [Issue #16706](https://github.com/unslothai/unsloth/issues/16706) |
| High | Unsloth Studio backend crashes on Krea 2 Turbo load (Windows, `0xc0000005`) | Open | [Issue #16892](https://github.com/unslothai/unsloth/issues/16892) |
| Medium | DiffusionGemma multi-GPU prompts >1792 tokens abort with `GGML_ASSERT n_ubatch >= n_tokens` | Open | [Issue #16925](https://github.com/unslothai/unsloth/issues/16925) |
| Medium | Crash on Blender MCP startup due to MCP SDK 2.3.0 incompatibility | Closed | [Issue #16878](https://github.com/unslothai/unsloth/issues/16878) |

---

### **6. What This Means for Application Developers**  
- **Multi-Model Workflows**: Expect improved consistency in model selection persistence when switching between agents. Use per-model extended parameter settings (proposed in [#7589](https://github.com/unslothai/unsloth/issues/7589)) for fine-grained control.  
- **UI/UX Considerations**: Avoid relying on `localStorage` for account-level preferences—new sync mechanisms are being rolled out. Use server-side storage instead.  
- **Agent Development**: The experimental Claude Agent integration enables local, key-free agent deployment—ideal for privacy-sensitive apps.  
- **Performance**: Optimize RAG pipelines by batching document chunks early. Monitor for regression in long-context handling.  
- **Extensibility**: With the proposed Plugin Store ([#10801](https://github.com/unslothai/unsloth/issues/10801)) and VS Code extension ([#10340](https://github.com/unslothai/unsloth/issues/10340)), future integrations will be more modular and IDE-native.

> ✅ **Actionable Takeaway**: If building agent systems, test multi-model state persistence and consider leveraging the upcoming benchmarking tools for model tuning.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*