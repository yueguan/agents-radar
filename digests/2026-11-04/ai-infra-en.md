# AI Infrastructure Digest 2026-11-04

> Generated: 2026-11-04 02:40 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# **Cross-Project AI Infrastructure Ecosystem Report – 2026-11-04**

---

### **1. Ecosystem Overview**

The AI inference and serving landscape in late 2026 is characterized by intense specialization, rapid hardware acceleration, and a growing divide between high-performance distributed engines and lightweight, developer-friendly gateways. Projects are converging on key capabilities—batch-invariant inference, speculative decoding, multi-GPU disaggregation, and FP8/quantized model support—while diverging in target use cases: from edge mobile inference (llama.cpp) to large-scale agent orchestration (SGLang, LiteLLM). The rise of heterogeneous backends (ROCm, MLX, Blackwell SM120) has intensified competition, with performance gains now tightly coupled to low-level kernel optimization and memory management. Meanwhile, the emergence of multimodal video input and real-time TTS streaming signals a shift toward rich, interactive AI applications.

---

### **2. Activity Comparison**

| Project       | Issues Open (Last 24h) | PRs Merged (Last 24h) | Release Status        |
|---------------|------------------------|------------------------|------------------------|
| **vLLM**      | 5                      | 7                      | None                   |
| **SGLang**    | 9                      | 5                      | None                   |
| **llama.cpp** | 6                      | 5                      | `b12094`, `b12091`     |
| **Ollama**    | 3                      | 4                      | `v0.40.15-rc0`         |
| **LiteLLM**   | 4                      | 4                      | `v1.109.0-rc.1`        |
| **Unsloth**   | 7                      | 3                      | `v0.1.914-beta`        |

> ✅ *Observation:* SGLang and vLLM lead in active issue volume, indicating deep engineering focus on stability and scalability. Ollama and LiteLLM are driving release velocity, reflecting strong productization momentum. Unsloth’s beta release shows aggressive feature delivery despite UI stability concerns.

---

### **3. Model Support Race**

| New Model / Architecture      | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|-------------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1 (ROCm)**      | ✅   | ✅     | ❌        | ❌     | ❌      | ✅      |
| **Nemotron 3.5 ASR Streaming**| 🚧   | ❌     | ❌        | ❌     | ❌      | ❌      |
| **Nemotron 3.5 Transcribe**   | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **Metis-1.6**                 | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **xing4_0**                   | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **MiMo V2.6 (MLX/GGUF)**     | ❌   | ❌     | ❌        | ✅     | ❌      | ❌      |
| **Qwen3-VL / LFM3-VL (Video)**| ❌   | ❌     | ❌        | ✅ (macOS) | ❌    | ✅      |

> 🏆 **Winner:** *Ollama* leads in multimodal user-facing model support, especially for Apple Silicon video input.  
> 🔥 **Innovation Leader:** *Unsloth* integrates cutting-edge models (Qwen3-VL, Llama 4) into its FastModel pipeline with CUDA graph acceleration.  
> ⚠️ **Gap:** No project yet supports **Nemotron 3.5 ASR Streaming**—a potential opportunity for gateway or fine-tuning frameworks.

---

### **4. Performance Frontier**

| Optimization Focus          | vLLM                     | SGLang                  | llama.cpp               | Ollama                | LiteLLM               | Unsloth               |
|------------------------------|--------------------------|-------------------------|-------------------------|-----------------------|-----------------------|------------------------|
| **KV Cache & Memory**        | ✅ Cache-aware admission, TPOT backoff | ✅ HiCache sharding | ✅ `--moe-cache-mib` (buggy) | ✅ MLX prefix cache fix | ✅ Redis circuit breaker | ✅ LoRA folding (I/O) |
| **Batching & Inference**     | ✅ Batch-invariant mode, sparse indexing | ✅ PD disaggregation, DCP-size fixes | ✅ `--sm tensor` (CUDA) | ✅ Concurrent generation | ✅ Streamed TTS | ✅ CUDA graphs (3–7x speedup) |
| **Quantization & Kernels**   | ✅ NVFP4, BF16 fusion, ROCm | ✅ DeepGEMM (SM120), Gluon kernels | ✅ NVFP4 GGUF, Q2_0, Hexagon | ✅ GGUF verification | ✅ Cosign-signed images | ✅ FP8 LoRA |
| **Distributed Serving**      | ✅ Sharded prefill (ROCm) | ✅ PD + HiCache across nodes | ❌ | ❌ | ✅ Proxy-level routing | ❌ |
| **Speculative Decoding**     | ✅ Draft-MTP (with caveats) | ✅ DFLASH (crash-prone) | ✅ Draft-MTP (correctness bug) | ❌ | ✅ Fallback logic | ✅ FastModel CUDA graphs |

> 🔍 **Trend:** High-end performance is increasingly defined by **kernel-level optimizations** (e.g., DeepGEMM, Gluon, CUDA graphs) and **distributed memory coordination** (HiCache, TPOT backoff). Unsloth stands out with **3–7x decode speedups via CUDA graphs**, while SGLang and vLLM push boundaries in **PD disaggregation** and **scalable KV management**.

---

### **5. Layer Positioning**

| Project       | Primary Layer              | Key Differentiator                                  | Target User                          |
|---------------|----------------------------|----------------------------------------------------|--------------------------------------|
| **vLLM**      | High-performance engine    | Batch-invariant inference, ROCm scaling            | Enterprise inference teams           |
| **SGLang**    | Multi-backend inference engine | PD disaggregation, Blackwell/ROCm optimization | Large-scale agent platforms          |
| **llama.cpp** | Local runtime (CPU/GPU)    | Cross-platform, lightweight, mobile support        | Devs, edge, embedded systems         |
| **Ollama**    | Developer gateway + CLI    | Simple UX, local-first, multimodal video input     | Solo devs, prototyping, agents       |
| **LiteLLM**   | API proxy & orchestration  | Multi-provider routing, soft budgeting, security  | Enterprises, internal tooling        |
| **Unsloth**   | Agent-focused runtime      | FastModel, CUDA graphs, Flows editor                | AI agents, real-time workflows       |

> 🎯 **Strategic Insight:** vLLM/SGLang dominate **infrastructure layer**; Ollama/LiteLLM control **developer access layer**; Unsloth targets **agent execution layer** with performance-first design.

---

### **6. Trend Signals**

#### **Key Trends Extracted:**
1. **Heterogeneous Hardware Acceleration**: ROCm, AMD MI350X/P, Intel XPU, Apple MLX, and NVIDIA Blackwell are no longer niche—they’re central to performance gains. vLLM and SGLang are leading in ROCm integration.
2. **Disaggregation is Production-Ready**: SGLang confirms that **Prefill-Decode Disaggregation (PD)** is stable across TP/PP/CP/DCP—ideal for low-latency, high-throughput inference at scale.
3. **Security & Governance in Gateways**: LiteLLM’s cosign-signed images and `soft_budget` alerts signal a maturing focus on **observability, compliance, and cost control** in AI APIs.
4. **Agent-Centric Optimization**: Unsloth’s CUDA graphs, Ollama’s video input, and SGLang’s HiCache show that **real-time agent workflows** demand end-to-end optimization—not just model quality.
5. **Multimodal First**: Video input (Ollama), audio streaming (LiteLLM), and vision models (SGLang/vLLM) are now standard, not experimental.

#### **What Developers Should Watch:**
- **Avoid `--enable-prefix-caching` in disaggregated setups** (vLLM Issue #61160).
- **Do not use `draft-mtp` with quantized models until #25618 is fixed** (llama.cpp).
- **Monitor Redis failover behavior** (LiteLLM PR #49884)—critical for production proxies.
- **Prepare for API unification** (SGLang RFC #45816, LiteLLM `callback_settings`).
- **Prioritize security hygiene**: Use signed Docker images (LiteLLM) and validate `AuthorizationError` in streaming clients.

> ✅ **Final Takeaway:** The infrastructure stack is shifting from “just faster inference” to **secure, scalable, agent-ready platforms**. Choose based on your layer: **engine (vLLM/SGLang), gateway (Ollama/LiteLLM), or agent runtime (Unsloth)**—and align your stack with the emerging standards in **disaggregation, security, and multimodal readiness**.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

---

### **vLLM Digest — 2026-11-04**

#### **1. Today's Highlights**  
The vLLM project continues to deepen its support for **batch-invariant inference**, with Issue #27433 (104 comments) driving optimization efforts to eliminate non-determinism in LLM serving—critical for reproducible, production-grade inference. Meanwhile, significant progress is being made on **ROCm and multi-modality performance**, including a key PR (#65555) that shards DeepSeek-V4.1 sparse-indexer prefill rows across TP ranks on AMD GPUs, improving scalability.

#### **2. Releases & Breaking Changes**  
None reported in the last 24 hours. No new releases or breaking API/config changes observed.

#### **3. New Model & Hardware Support**  
- ✅ **DeepSeek-V4.1**: Full ROCm support now includes sharded sparse-indexer prefill rows via PR #65555, enabling better scaling on MI350X/P systems.  
- ✅ **NVIDIA Nemotron 3.5 ASR Streaming** (nemotron-3.5-asr-streaming-0.6b): Added as a feature request (Issue #47455), targeting multilingual streaming speech-to-text use cases.  
- ✅ **GLM-5.3-Flash**: ROCm-specific improvements via PR #63070, defaulting to breakable CUDA graphs for better compilation success.  
- 🚧 **PaliGemma**: Deprecation proposed via PR #65409 due to incorrect multimodal output behavior (causal attention vs. required bidirectional prefix LM).  

> 🔗 [PR #65555](https://github.com/vllm-project/vllm/pull/65555) | [PR #63070](https://github.com/vllm-project/vllm/pull/63070) | [PR #65409](https://github.com/vllm-project/vllm/pull/65409)

#### **4. Performance & Optimization**  
- **+13.8% decode throughput** on Qwen3.8-Flash-Next-NVFP4 with context-aware sparse LM head (PR #59740), with up to +23.3% gains when combined with full NVFP4 quantization.  
- **BF16 projection fusion** added opt-in for SM89 (RTX 4090) prefill kernels via PR #65344, targeting Qwen3.5-4B MoE MLPs.  
- **Cache-aware admission ordering** (PR #54625) improves efficiency by prioritizing requests with cached prefixes—reducing cold-start latency in high-throughput scenarios.  
- **KV pool saturation mitigation**: PR #64460 introduces automatic admission backoff to prevent decode TPOT inflation (~2.3x) under high concurrency.  

> 🔗 [PR #59740](https://github.com/vllm-project/vllm/pull/59740) | [PR #65344](https://github.com/vllm-project/vllm/pull/65344) | [PR #54625](https://github.com/vllm-project/vllm/pull/54625) | [PR #64460](https://github.com/vllm-project/vllm/pull/64460)

#### **5. Stability & Regressions**  
- **Critical**: `enable_adaptive_verification` rejected on SM120 (Blackwell) GPUs (Issue #63343), blocking use of advanced verification features.  
- **Severe**: Cross-request context contamination via shared-prefix KV reuse in PD disaggregation (Issue #61160), leading to content leakage between users—fixed by disabling prefix caching (`--no-enable-prefix-caching`).  
- **High severity**: Silent garbage output in Confidential Computing mode (TDX guest) due to stale UVA views (Issue #57224); workaround available via `VLLM_USE_V2_MODEL_RUNNER=0`.  
- **ROCm**: `Workspace is locked` assertion in `rocm_fp8_paged_mqa_logits` under high decode concurrency (Issue #64874), affecting GLM-5.3-Flash.  
- **Other**: Qwen3.8-Flash-Next-FP8 fails to load on ROCm due to missing `weight_scale` in AMD embedding layer (Issue #58688).  

> 🔗 [Issue #63343](https://github.com/vllm-project/vllm/issues/63343) | [Issue #61160](https://github.com/vllm-project/vllm/issues/61160) | [Issue #57224](https://github.com/vllm-project/vllm/issues/57224) | [Issue #64874](https://github.com/vllm-project/vllm/issues/64874) | [Issue #58688](https://github.com/vllm-project/vllm/issues/58688)

#### **6. What This Means for Application Developers**  
- **Use batch-invariant mode carefully**: Ensure your workload aligns with deterministic inference expectations—see Issue #27433 for ongoing optimization.  
- **Avoid prefix caching in disaggregated setups** if you’re serving sensitive or mixed-content traffic—use `--no-enable-prefix-caching` until #61160 is resolved.  
- **Leverage ROCm gains**: If using AMD MI350X/P hardware, enable shard-based sparse indexing (PR #65555) and breakable CUDA graphs (PR #63070) for better DeepSeek-V4.1 performance.  
- **Watch for memory issues**: High-concurrency decode workloads may hit workspace locks—implement backoff logic or adjust queue scheduling.  
- **Prepare for PaliGemma deprecation**: Migrate away from PaliGemma if using multimodal pipelines; no fix timeline exists yet.  

> 🔗 [RFC #27433](https://github.com/vllm-project/vllm/issues/27433) | [Issue #61160](https://github.com/vllm-project/vllm/issues/61160) | [PR #65555](https://github.com/vllm-project/vllm/pull/65555)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-04**

---

### **1. Today's Highlights**  
The SGLang ecosystem continues to solidify its position as a high-performance, multi-backend inference engine with major strides in **Prefill-Decode Disaggregation (PD)** stability and **Blackwell (SM120)** kernel optimization. Key developments include the rollout of `SGLANG_SIMULATE_ACC_LEN` seed consistency fixes, improvements in AMD ROCm support for Qwen3.8-Flash-Next, and active work on **Mooncake HiCache** integration for distributed storage across nodes. Critical CI reliability issues remain under investigation, with flaky test tracking ongoing.

---

### **2. Releases & Breaking Changes**  
*No new releases or breaking changes were published in the last 24 hours.*  
However, ongoing efforts to unify the backend API via **[RFC: Complete migration to shared api.v1 Generate](https://github.com/sgl-project/sglang/issues/45816)** are progressing — this will standardize generation logic across `sgl-processor`, `sgl-router`, and the Rust server. Developers should prepare for upcoming schema changes in `api.v1`.

---

### **3. New Model & Hardware Support**  
- ✅ **AMD ROCm (gfx950 / MI355X):** Active performance optimization for **Qwen3.8-Flash-Next**, including Gluon kernel fusion for GLM-5 decode chains ([PR #46486](https://github.com/sgl-project/sglang/pull/46486)) and fix for `--dcp-size > 1` crash on MI300X ([Issue #38709](https://github.com/sgl-project/sglang/issues/38709)).  
- ✅ **Intel XPU:** RFC proposal for **paged-experts with Lookahead Expert Scheduling (LES)** and SSD tier support for iGPU MoE offload ([Issue #46520](https://github.com/sgl-project/sglang/issues/46520)).  
- ✅ **NVIDIA Blackwell (SM120):** Native block32 DeepGEMM enabled for DeepSeek-V4.1 FP8 (`--fp8-gemm-backend deep_gemm`) and optimizations for DSV4 prefill throughput ([PR #47210](https://github.com/sgl-project/sglang/pull/47210)).  
- ✅ **New Models:** Added native support for **Nemotron-3-Embed-1B-BF16** ([PR #45885](https://github.com/sgl-project/sglang/pull/45885)) and expanded Mooncake Engram storage for **Qwen4-Exp** ([PR #47673](https://github.com/sgl-project/sglang/pull/47673)).

---

### **4. Performance & Optimization**  
- 🔥 **DeepSeek-V4.1 on SM120:** Significant progress in prefill optimization via `deep_gemm` backend; expected throughput improvement from ~2–7K tok/s to near **12.5K tok/s** (matching vLLM/Marlin) pending tuning ([Issue #33422](https://github.com/sgl-project/sglang/issues/33422)).  
- 🚀 **Qwen3.8-Flash-Next on AMD:** GLM-5 decode MLA chain now runs as Gluon kernels on gfx950, reducing kernel launch overhead ([PR #46486](https://github.com/sgl-project/sglang/pull/46486)).  
- ⚡ **Memory Efficiency:** PRs focused on reducing redundant MoE copies during EAGLE/MTP startup ([PR #42510](https://github.com/sgl-project/sglang/pull/42510)) and optimizing `vectorized_gather_kernel` OOB checks ([Issue #37936](https://github.com/sgl-project/sglang/issues/37936)).  
- 💾 **Storage Layer:** Mooncake HiCache now supports **node-sharded Engram storage**, enabling scalable, distributed L2/L3 caching across compute nodes ([PR #47673](https://github.com/sgl-project/sglang/pull/47673)).

---

### **5. Stability & Regressions**  
⚠️ **Critical Crashes & Bugs:**  
- **CUDA Coredump Tracker (#26340)**: 354 comments — recurring coredumps in `pr-test.yml` workflows indicate underlying GPU driver/runtime instability, likely tied to speculative decoding or PD disaggregation. *No fix yet.*  
- **DFLASH + PD Disaggregation Crash (#36140)**: DFLASH speculative decoding fails when decode runs in PD mode due to missing disagg wiring. *Fix pending.*  
- **ROCm `--dcp-size > 1` Crash (#38709)**: Unpacks 2 values from 1-value return in `forward_absorb_rocm_core`. *Reproduced on MI300X.*  
- **HiCache Backup Failure (#48145)**: Mooncake HiCache backup fails for Qwen3.8-Flash-Next-FP8 model. *High priority.*  
- **VAE Decode Time Spikes (#48196)**: Wan VAE decode time spikes 2–10x on 1-gpu-5090 runners after migration to `5090-sm*` runners — impacting multimodal pipelines.

🟢 **Flaky CI Infrastructure**: Issue #42752 tracks persistent flakiness in NVIDIA `PR Test Base/Extra` runs, hampering PR validation. *Work in progress.*

---

### **6. What This Means for Application Developers**  
- **Use PD Disaggregation with Confidence**: The Q4 roadmap confirms PD is now stable across TP/PP/CP/DCP/DP-attention and supports HiCache and speculative decoding — ideal for large-scale, low-latency inference.  
- **Optimize for Blackwell (SM120)**: Enable `--fp8-gemm-backend deep_gemm` for DeepSeek-V4.1 and other models to unlock near-peak throughput. Monitor prefill benchmarks closely.  
- **Avoid DFLASH in PD Mode**: Until #36140 is fixed, avoid using DFLASH speculative decoding with `--disaggregation-mode decode`. Use DSPARK or EAGLE instead.  
- **Prepare for API Unification**: Start aligning tooling with the evolving `api.v1.Generate` schema (tracked in #45816). Expect deprecation of legacy endpoints.  
- **Handle Multimodal Edge Cases**: Be cautious with raw `<|image|>` placeholders in prompts — they can cause 500 errors if not properly paired with image data ([Issue #37579](https://github.com/sgl-project/sglang/issues/37579)). Use `oss-harmony` for offline serving ([PR #48267](https://github.com/sgl-project/sglang/pull/48267)).  

👉 **Actionable Tip**: If running on AMD, verify your model’s compatibility with ROCm 7.2 and monitor `--dcp-size` limits until #38709 is resolved.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp Digest – 2026-11-04**

---

### **1. Today's Highlights**  
The latest development cycle brings significant performance gains for speculative decoding on Hexagon and CUDA backends, with a 19% improvement in prompt evaluation throughput via optimized `--sm tensor` communication. New support for **Metis-1.6**, **Nemotron 3.5 Transcribe**, and **xing4_0** models expands the ecosystem for multimodal and specialized inference workloads. Critical stability fixes address crashes in MTP speculative decoding and GPU memory management.

---

### **2. Releases & Breaking Changes**  
- **b12094**: Hexagon backend optimization: faster `TOP_K` sampling via tile skipping and block scan (`#31279`).  
- **b12091**: Speculative decoding now correctly saves/restores MTP hidden state; simplifies checkpointing (`#30141`).  
- **b12087**: Added support for **Nemotron 3.5 Transcribe** model architecture (`#31471`).  
- **b12089**: Full support for **Metis-1.6** models via new architecture registration and conversion scripts (`#31134`).  
- **b12088**: Enabled **NVFP4 LFM3 GGUF** format support (`#31608`).  

> 📌 *No breaking API changes detected. All updates are additive or performance-focused.*

---

### **3. New Model & Hardware Support**  
| Model / Architecture | Backend | Status | PR/Issue |
|----------------------|--------|--------|---------|
| **Metis-1.6** | CPU/GPU | ✅ Supported | [#31134](https://github.com/ggml-org/llama.cpp/pull/31134) |
| **Nemotron 3.5 Transcribe** | CPU/GPU | ✅ Supported | [#31471](https://github.com/ggml-org/llama.cpp/pull/31471) |
| **xing4_0** | CPU/GPU | ✅ Supported (new arch) | [#31589](https://github.com/ggml-org/llama.cpp/pull/31589) |
| **Hexagon (Snapdragon 7 Gen 4)** | HMX | ⚠️ Partial support | Issue [#29473](https://github.com/ggml-org/llama.cpp/issues/29473) |
| **AMD gfx1151 (ROCm 10.0+)** | HIP | ✅ CI testing enabled | PR [#31567](https://github.com/ggml-org/llama.cpp/pull/31567) |

> 🔧 *New quant formats: NVFP4 (Q4_K_M/Q5_K_M), Q2_0 (Adreno OpenCL), and F16/F32 for LIGHTNING_INDEXER (Hexagon).*

---

### **4. Performance & Optimization**  
- **Hexagon**: `TOP_K` sampling speedup by skipping irrelevant tiles and replacing bitonic sort with `top_k_scan()` — reduces latency per token.  
- **CUDA**: `--sm tensor` prompt eval improved by **+19% t/s** over stock (mean) without PPL degradation (`#30956`).  
- **SYCL**: `fast_ggml_sycl_comm_exchange()` optimizes inter-device communication — closes gap between `--sm layer` and `--sm tensor`.  
- **OpenCL**: Improved `q2_0 GEMV` kernel for Adreno GPUs (4–8 output rows per thread) — enhances low-bit quant performance.  
- **WebGPU**: Batched tensor APIs (`get_tensor_n`, `set_tensor_n`) added to improve batching efficiency (`#31473`).  

> 💡 *These optimizations are critical for high-throughput speculative decoding and large-context inference.*

---

### **5. Stability & Regressions**  
| Issue | Severity | Affected Backends | Status | Fix PR |
|------|----------|-------------------|--------|--------|
| [Bug #25618](https://github.com/ggml-org/llama.cpp/issues/25618): Draft-MTP greedy output divergence on quantized targets | High | CUDA/SYCL | Open | No fix yet |
| [Bug #31586](https://github.com/ggml-org/llama.cpp/issues/31586): CUDA crash loading DFlash with `--sm tensor` | High | CUDA | Open | In progress |
| [Bug #31064](https://github.com/ggml-org/llama.cpp/issues/31064): SYCL segfault on oneAPI 2026.2.0 | Medium | SYCL | Open | PR [#31605](https://github.com/ggml-org/llama.cpp/pull/31605) (docker fix) |
| [Bug #30869](https://github.com/ggml-org/llama.cpp/issues/30869): Compile failure on AMD CDNA2 (gfx90a) | Medium | HIP | Open | No PR |
| [Bug #29893](https://github.com/ggml-org/llama.cpp/issues/29893): VRAM spike with `--moe-cache-mib` on Vulkan | Medium | Vulkan | Open | No fix |

> ⚠️ **Critical**: The MTP speculation correctness issue (#25618) affects quantized models under greedy sampling — may lead to inconsistent outputs in production agents.

---

### **6. What This Means for Application Developers**  
- **Use `--spec-type draft-mtp` safely only on BF16 or non-quantized targets until #25618 is resolved** — avoid it with Q4_K_M, Q5_K_M, etc.  
- **Enable `--sm tensor` for better throughput on multi-GPU setups** — now up to 19% faster than before.  
- **Leverage new model support**: Deploy **Metis-1.6**, **Nemotron 3.5 Transcribe**, and **xing4_0** models using standard CLI flags.  
- **Optimize mobile inference** via Hexagon + LIGHTNING_INDEXER support (check device compatibility).  
- **Avoid `--moe-cache-mib` on Vulkan** due to memory regression. Use `--cache-ram` instead.  
- **Upgrade Docker images** if using Intel SYCL — PR [#31605](https://github.com/ggml-org/llama.cpp/pull/31605) fixes runtime crashes.  

> 🛠️ *For production systems: Monitor `llama-server` logs for `GGML_ASSERT` crashes during MTP/Draft loading. Apply patches from `b12094+` and test with your target quantization scheme.*  

🔗 **Official Website**: [https://llama.app](https://llama.app)  
🔗 **Latest Release**: [b12094](https://github.com/ggml-org/llama.cpp/releases/tag/b12094)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-04**

---

### **1. Today's Highlights**  
The latest release, **v0.40.15-rc0**, introduces critical improvements for Apple Silicon (MLX) performance and adds support for the new **MiMo V2.6** model. Major focus areas include video input capabilities on MLX, enhanced tokenization APIs, and fixes for authorization handling in streaming responses. These updates significantly improve multimodal inference reliability and developer tooling.

---

### **2. Releases & Breaking Changes**  
- **v0.40.15-rc0**: Released with key fixes and feature enhancements.  
  - *MLX prefix cache memory management* now avoids excessive swap usage on 32GB Apple Silicon systems ([#18131](https://github.com/ollama/ollama/issues/18131)).  
  - *MiMo V2.6* model support added via `./ollama pull dhiltgen/mimo-v2.6` ([#19400](https://github.com/ollama/ollama/pull/19400)).  
- **API Improvements**:  
  - Streaming `401 Unauthorized` errors now correctly propagate `AuthorizationError` even when response body is malformed ([#19489](https://github.com/ollama/ollama/issues/19489), fixed in [#19491](https://github.com/ollama/ollama/pull/19491)).  
  - New `/api/tokenize` and `/api/detokenize` endpoints enable token counting without model loading ([#19498](https://github.com/ollama/ollama/pull/19498)).

---

### **3. New Model & Hardware Support**  
- **New Models**:  
  - **MiMo V2.6** (MLX/GGUF) – supports multi-modal reasoning with improved instruction-following and context handling.  
  - **AutoTrust** (MLX) – experimental decision model for trust-aware agent workflows ([#19399](https://github.com/ollama/ollama/pull/19399)).  
- **Hardware & Backend Enhancements**:  
  - **MLX Video Input Support** added for macOS: Qwen3-VL, LFM3-VL, and LFM3-Omni models now accept MP4/MOV clips via `images` field ([#19348](https://github.com/ollama/ollama/pull/19348)).  
  - **CUDA/NVIDIA GPU** support confirmed for vision models (Qwen2.5vl:3b) — previously failed due to kernel mismatch on older GPUs ([#19482](https://github.com/ollama/ollama/issues/19482)).  
  - **Linux/Windows Video Input** still pending; currently only works on macOS MLX ([#19503](https://github.com/ollama/ollama/issues/19503)).

---

### **4. Performance & Optimization**  
- **Model Download Speed**:  
  - GGUF blob verification during download now occurs incrementally across parallel HTTP ranges, eliminating redundant post-download hashing ([#19505](https://github.com/ollama/ollama/pull/19505)).  
  - Reduces latency by ~30–50% for large models (e.g., 70B+).  
- **Memory Efficiency**:  
  - MLX prefix cache no longer over-allocates on Apple Silicon (32GB RAM), preventing heavy swapping during agent workloads ([#18131](https://github.com/ollama/ollama/issues/18131)).  
- **Concurrency**:  
  - MLX models now support concurrent generation via `OLLAMA_NUM_PARALLEL` setting ([#19197](https://github.com/ollama/ollama/pull/19197)), reducing first-token latency for batched requests.

---

### **5. Stability & Regressions**  
| Issue | Severity | Status | Fix PR |
|------|----------|--------|--------|
| **Scheduler deadlock** during model eviction + concurrent load | Critical | Open | No fix yet ([#17408](https://github.com/ollama/ollama/issues/17408)) |
| **Tool call truncation returns HTTP 500** | High | Open | Fixed in [#19494](https://github.com/ollama/ollama/pull/19494) |
| **Incorrect VRAM estimation** for large context windows | Medium | Open | No fix yet ([#10128](https://github.com/ollama/ollama/issues/10128)) |
| **NVIDIA Pascal GPU CUDA error** in v0.40.13 | High | Open | No fix yet ([#19485](https://github.com/ollama/ollama/issues/19485)) |
| **HumanNumber rounding past unit boundary** | Low | Open | Fixed in [#19500](https://github.com/ollama/ollama/pull/19500), [#19499](https://github.com/ollama/ollama/pull/19499) |

> ⚠️ **Critical Note**: Scheduler deadlock remains unresolved and may cause complete service hang under high concurrency.

---

### **6. What This Means for Application Developers**  
- **Agents & Tools**: Use `OLLAMA_NUM_PARALLEL` to reduce latency in multi-request flows; ensure `num_predict` is set above expected tool call length to avoid HTTP 500s ([#19494](https://github.com/ollama/ollama/pull/19494)).  
- **Multimodal Apps**: Leverage video input support on macOS MLX for Qwen3-VL and LFM3-VL models — ideal for RAG or visual agents.  
- **CI/CD & Automation**: Use `--json` flag in `list` and `ps` commands for scriptable model management ([#17349](https://github.com/ollama/ollama/pull/17349)).  
- **Security & Reliability**: Always validate `AuthorizationError` in streaming clients; use `/api/tokenize` to precompute prompt lengths and avoid silent truncation ([#19498](https://github.com/ollama/ollama/pull/19498)).  
- **Avoid Pitfalls**: Do not assume `truncation=false` prevents input shortening — it’s now honored only if explicitly passed via `openai` compatibility layer ([#19501](https://github.com/ollama/ollama/pull/19501)).

---  
*Data source: [github.com/ollama/ollama](https://github.com/ollama/ollama)*  
*Digest generated: 2026-11-04*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM Digest – 2026-11-04**

---

### **1. Today's Highlights**  
The latest release candidate `v1.109.0-rc.1` introduces enhanced security via cosign-signed Docker images and advances in proxy-level configuration, including support for per-key callback settings and soft budget alerts for internal users. Key stability fixes address critical issues in streaming behavior (e.g., `flex_unavailable` error mapping) and state management during Redis failovers, while ongoing work improves internationalization readiness across the Admin UI.

---

### **2. Releases & Breaking Changes**  
- **`v1.109.0-rc.1`**: Released with cosign signature verification tied to commit [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065). All Docker images are now signed — verify using [cosign](https://docs.sigstore.dev/cosign/overview/).  
- **Breaking Change (Proxy)**: `callback_settings` can now be referenced by name from virtual keys (`config.yaml`), enabling dynamic credential injection without hardcoding secrets. See PR [#49995](https://github.com/BerriAI/litellm/pull/49995).  
- **New Feature**: Internal users now support `soft_budget`, triggering alerts at threshold (Slack/email/webhook) before spending hits hard limits. PR [#49975](https://github.com/BerriAI/litellm/pull/49975).

---

### **3. New Model & Hardware Support**  
- **Cloudflare Workers AI**: Audio and image generation support added via `cloudflare` provider (PRs [#35055](https://github.com/BerriAI/litellm/pull/35055), [#35056](https://github.com/BerriAI/litellm/pull/35056)).  
- **Bedrock Converse Claude**: Improved handling of `output_config.effort` and structured output routing (PR [#49990](https://github.com/BerriAI/litellm/pull/49990)).  
- **Google GenAI**: Forwarding of client headers in `generate_content` endpoints now preserved (PR [#49972](https://github.com/BerriAI/litellm/pull/49972)).

---

### **4. Performance & Optimization**  
- **Streaming TTS**: Text-to-speech now streams audio as it arrives via `/v1/audio/speech` (PR [#46222](https://github.com/BerriAI/litellm/pull/46222)), reducing latency for long clips (e.g., ElevenLabs) by up to ~65% on initial playback.  
- **Token Budgeting**: The v3 rate limiter now more accurately tracks TPM usage under upstream 429s; fix pending in PR [#49675](https://github.com/BerriAI/litellm/pull/49675).  
- **Admin UI Build**: Pre-built UI artifacts are now used in Docker builds, cutting CI time by ~40% (PR [#29092](https://github.com/BerriAI/litellm/pull/29092)).

---

### **5. Stability & Regressions**  
- **Critical**: Redis circuit breaker stays `HALF_OPEN` indefinitely after primary failover due to non-connectivity errors (e.g., `READONLY`). Fixes PR in progress: [#49884](https://github.com/BerriAI/litellm/pull/49884).  
- **High Severity**: OpenAI `flex_unavailable` error returns 500 instead of 429, breaking fallback logic. Fixed in PR [#49736](https://github.com/BerriAI/litellm/pull/49736).  
- **Medium Severity**: `TZ=Asia/Calcutta` crashes proxy startup if `tzdata-legacy` not installed (Ubuntu 24.04+). Fix PR: [#49813](https://github.com/BerriAI/litellm/pull/49813).  
- **Minor**: HuggingFace embeddings ignore `timeout` parameter (PR [#47299](https://github.com/BerriAI/litellm/pull/47299)); fixed in `main`.

---

### **6. What This Means for Application Developers**  
- **Enhanced Observability & Control**: Use `soft_budget` and per-key `callback_settings` to enforce cost governance and trace logs without re-deploying.  
- **Better Streaming UX**: Leverage real-time TTS streaming and improved error handling (e.g., 429 vs 500) for resilient agent workflows.  
- **Avoid Pitfalls**: Ensure `tzdata-legacy` is installed in production environments when using non-standard timezones.  
- **Future-Proofing**: Prepare for i18n-ready UI components (PRs [#49991](https://github.com/BerriAI/litellm/pull/49991), [#49994](https://github.com/BerriAI/litellm/pull/49994), etc.)—text will soon be translatable.  

> **Actionable Tip**: Update to `v1.109.0-rc.1` and validate your proxy config against the new `callback_settings` and `soft_budget` patterns ahead of GA.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

---

### **Unsloth Digest — 2026-11-04**

---

#### **1. Today's Highlights**  
The v0.1.914-beta release introduces the *Glass Theme*, enhanced personalization (backgrounds, greetings), and faster generation with expanded multi-GPU and CPU training support. Key performance work includes CUDA graph decoding for FastModel variants (3–7x speedup on T4) and FP8 LoRA integration. Meanwhile, UI stability remains a focus—critical regressions in chat persistence, model loading, and AppImage startup are under active review.

---

#### **2. Releases & Breaking Changes**  
- **v0.1.914-beta** ([Release Notes](https://github.com/unslothai/unsloth/releases/tag/v0.1.914-beta))  
  - Added Glass theme and customizable chat backgrounds/greetings.  
  - Introduced CPU training and expanded multi-GPU training options.  
  - Performance improvements in generation latency and model loading.  
  - *Note:* Update failures reported on macOS and Linux AppImage; users may need to force restart or clear cache.

---

#### **3. New Model & Hardware Support**  
- **New Backends:**  
  - ROCm support now explicitly tested and validated via `PR #16609` ([#16609](https://github.com/unslothai/unsloth/pull/16609)), ensuring stable behavior on AMD gfx1151 (Strix Halo) and Windows ROCm.  
  - Apple MLX support confirmed on macOS 26.6.2 ([Issue #16561](https://github.com/unslothai/unsloth/issues/16561)).  
- **New Models:**  
  - Kimi-3.1-pro is missing from Model Hub Connect despite being available elsewhere ([Issue #16586](https://github.com/unslothai/unsloth/issues/16586)); suggests backend sync issue.  
  - Qwen3-VL, Llama 4, Mistral 3, Granite, Phi-4, OLMo 2, Cohere 2, GLM-4, and Qwen3-MoE now benefit from CUDA graph decoding ([PR #16587](https://github.com/unslothai/unsloth/pull/16587)).

---

#### **4. Performance & Optimization**  
- **3–7x decode speedup** on T4 GPUs via CUDA graph compilation for FastModel models (Gemma 3, Qwen3-VL, Llama 4, etc.) — bitwise equivalent to eager mode ([PR #16587](https://github.com/unslothai/unsloth/pull/16587)).  
- **FP8 LoRA** enables fp8 forward and input gradient computation on frozen 16-bit base layers while preserving exact precision for saving/merging ([PR #16423](https://github.com/unslothai/unsloth/pull/16423)).  
- **LoRA folding**: 16-bit + LoRA models achieve **1.3–1.4x decode speedup** by temporarily folding LoRA weights into base weights during long generations ([PR #16576](https://github.com/unslothai/unsloth/pull/16576)).  
- **Memory optimization**: `adapter_config.json` now read once per inference worker load ([PR #16604](https://github.com/unslothai/unsloth/pull/16604)), reducing I/O overhead.

---

#### **5. Stability & Regressions**  
| Issue | Severity | Status | Link |  
|------|----------|--------|------|  
| AppImage fails to start on Slackware64 due to GLib/AppIndicator symbol lookup error | Critical | Open | [#16605](https://github.com/unslothai/unsloth/issues/16605) |  
| UI lag in long conversations (web + desktop) | High | Open | [#16603](https://github.com/unslothai/unsloth/issues/16603) |  
| AppImage Unsloth Studio has very laggy UI | Medium | Open | [#15999](https://github.com/unslothai/unsloth/issues/15999) |  
| Auto unload on idle broken | Medium | Closed | [#16484](https://github.com/unslothai/unsloth/issues/16484) |  
| Loading model silently cancels running chats | High | Closed | [#16479](https://github.com/unslothai/unsloth/issues/16479) |  
| `search_conversation` tool disappeared (2nd time) | Medium | Open | [#16602](https://github.com/unslothai/unsloth/issues/16602) |  

> ✅ *Fixes in progress*: PRs addressing model load conflicts (`#16581`, `#16479`) and background image persistence (`#16574`) are under review.

---

#### **6. What This Means for Application Developers**  
- **Build more performant agents**: Leverage CUDA graphs and FP8 LoRA for 3–7x faster inference without sacrificing model fidelity—ideal for real-time agent workflows.  
- **Enable cross-platform deployment**: With improved ROCm and MLX support, your apps can run efficiently on AMD and Apple Silicon hardware.  
- **Avoid silent state loss**: Be cautious when loading/unloading models—current behavior cancels active generations silently; implement explicit `force_cancel_active` checks.  
- **Design for long sessions**: Long conversations trigger UI lag—optimize state management or use context compaction early.  
- **Extend Flows pipelines**: The new Flows node editor (in PRs like `#14730`, `#15604`) enables declarative AI pipelines, benchmarking, and agent orchestration—future-proof your app logic.

---  
*Digest generated: 2026-11-04 | Source: [unslothai/unsloth GitHub](https://github.com/unslothai/unsloth)*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*