# AI Infrastructure Digest 2026-10-29

> Generated: 2026-10-29 14:08 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Report — 2026-10-29

## 1. Ecosystem Overview

Today's activity shows the ecosystem converging on **correctness of caching and state management** rather than raw throughput as the primary battleground: vLLM, SGLang, llama.cpp, and LiteLLM all have open or in-flight work on KV/prefix cache semantics, weight-transfer verification, and spend accounting. Model enablement is dominated by **hybrid/MoE and speculative-decoding architectures** (Kimi-K3, DeepSeek-v4.1, Qwen3.5/3.8, Mistral-Large-4-1T, GLM-5.3, LFM3), with backends broadening beyond NVIDIA to ROCm RDNA3/4, Intel SYCL, Qualcomm Hexagon, Ascend NPU, and T-Head PPU. Release cadence diverges sharply: llama.cpp ships nightly builds, LiteLLM shipped three releases in 24h, while vLLM and SGLang are in review-and-triage mode with no releases. The clearest systemic risk signal is that **silent failures** — state leakage across requests, no-op weight updates, `$0` spend logs, truncated training datasets — outnumber loud crashes in the high-severity lists.

## 2. Activity Comparison

Counts reflect distinct items surfaced in the 24h digest, not full repo totals.

| Project | Layer | Issues referenced | PRs referenced | Release status |
|---|---|---|---|---|
| **vLLM** | Datacenter serving engine | ~13 | ~17 | None in 24h; review/triage-heavy |
| **SGLang** | Datacenter serving engine | ~27 | ~18 | None in 24h; CI flakiness blocking merges |
| **llama.cpp** | Local/edge runtime | ~18 | ~21 | b11971–b11982 (12 nightly builds) |
| **Ollama** | Local runtime + distribution | ~17 | ~10 | **v0.40.13** (LFM3 cross-platform) |
| **LiteLLM** | LLM gateway | ~22 | ~17 | **3 releases**: 1.108.0-rc.1, 1.106.6, 1.105.7 |
| **Unsloth** | Fine-tuning + Studio | ~15 | ~17 | **v0.1.912-beta** |

**Reading the counts:** SGLang's high issue volume is partly structural (long-running trackers like #42752 flaky-test, #34510 PD unification) rather than new defects. LiteLLM's release velocity is the highest of the group but each release is cosign-only with no changelog, which shifts verification burden to operators. llama.cpp's twelve builds indicate a steady merge pipeline rather than feature milestones.

## 3. Model Support Race

**Leading — datacenter serving engines.** vLLM and SGLang are clearly ahead on frontier architecture enablement:

- **vLLM:** Muse Spark NVFP4 (hybrid MegaMoE/TRT-LLM kernels), Mistral-Large-4-1T-A52B recipe, Kimi-K3 DSpark sampling on ROCm, Qwen3-Next/3.5 fused QK-norm+RoPE+gate, DeepSeek-v4.1 sparse MLA on gfx942, Nemotron VL device-side preprocessing, OpenVLA chat-template fallback.
- **SGLang:** `sensenova_loop` multimodal (Qwen3.5-derived), DFlash + Nemotron Bolt E4B GGUF, Ascend NPU DSpark for DeepSeek-V4, Kimi-K3 Quark FP8 KDA in-proj + MLA absorb, Wan2.2/Wan-Animate `convrot_int8`, GLM-5.3-Flash (SM90) and Qwen3.8-Flash-Next (gfx950) trackers. Also pushing AMD **consumer** RDNA3/RDNA4 (gfx1100/1101/1200/1201) and T-Head PPU.

**Leading — exotic/edge backends.** llama.cpp is unmatched on breadth of non-mainstream targets: **RDNA4_M (gfx1170/1171)** WMMA targets, **TML Inkling** architecture with banded Flash Attention, **DiffusionGemma** diffusion support, **Spark2.5 DSpark** draft models, plus Qualcomm Hexagon (UPSCALE, blocked IM2COL), OpenCL/Adreno q4_K GEMM, and SYCL quantized-KV FA.

**Leading — turnkey consumer availability.** Ollama’s **LFM3 family** (text/audio/omni) went cross-platform to Linux and Windows in v0.40.13; an open PR adds **MLX runner on Windows + CPU**.

**Trailing — fine-tuning.** Unsloth still has **LFM3-1.7B support as an open request** (highest-demand item, 10 👍); partial MLX LFM3 support exists but models are missing from `/v1/models`.

**Gateway layer.** LiteLLM doesn't ship kernels, but added **Vertex AI batch for Claude**, **DashScope image edits + TTS**, **hosted_vllm model discovery**, and two new providers (**CLF AI Gateway**, **eu/jev**).

## 4. Performance Frontier

Optimization effort is concentrated in five areas, with a notable shift toward caching and memory over raw FLOPs:

**KV cache (largest cluster).** vLLM has three open PRs fixing tiered-offload correctness (#62615, #64444, #61717) and a hybrid-GDN prefix-cache MTP bug (#52244); TurboQuant sub-8-bit KV (#38171) is the most-requested unmerged feature (52 comments, 129 👍) but remains Hopper-centric after the Ampere hybrid-MoE breakage (#40124). llama.cpp landed **SYCL DPAS flash attention directly on q4_0/q8_0 KV blocks** (b11980), which eliminates dequant/convert for decode. SGLang documents **HiCache host-tier hit-rate collapse** on hybrid GDN (#45400) and is planning **supernode-wide L1/L2 KV sharing with cache-location-aware routing** (#43098).

**Kernels.** vLLM: single-pass Triton RMSNorm with PDL (#64290), ROCm fused QK-norm+RoPE+gate (#51406), sparse MLA prefill on gfx942 (#60102). SGLang: AITER Gluon `chunk_kda` for bf16 SSM state pools (#47036), FP8 KDA in-proj + MLA absorb on Kimi-K3 (#44362). llama.cpp: vectorized dequant stores for Q4_0/K-quants/IQ3/MXFP4 (b11979), Hexagon HVX IM2COL and transposed-CONCAT DDR bandwidth (b11976/b11981). Unsloth: bitsandbytes 4-bit GEMM dispatch bypass (#15971), whole-step CUDA graph for batch-1 decode (#15976, opt-in).

**Batching / scheduling.** vLLM MRV2 sampler fp32 logits copy growth (#62560); SGLang step-level CUDA graph + fused EAGLE verify at low concurrency (#46211, #46210); llama.cpp ClassLRU hybrid MoE cache reports **+6% decode on QFN, +7% on GLM, 8% fewer expert uploads** (#31233).

**Quantization.** vLLM open gaps on SM120 (RTX PRO 6000 Blackwell) MXFP4 backend selection (#31085, #30135); SGLang `convrot_int8` validated for Wan2.2; llama.cpp MoE `--moe-cache-cpu-share auto` with CRF admission (#31286).

**Distributed serving.** vLLM NCCL weight-transfer silently no-ops on XPU/disabled PyNCCL (#57022); SGLang PD disaggregation under Ray is currently broken pending #47282; llama.cpp `--split-mode tensor` crash on 2nd request (#29466).

## 5. Layer Positioning

The six projects occupy four distinct layers, and today's activity maps cleanly onto them:

- **Datacenter serving engines — vLLM, SGLang.** High-throughput batched inference with TP/EP, disaggregated prefill/decode, quantized KV, speculative decoding. Their work is dominated by correctness of caching, weight lifecycle, and multi-rank coordination. They compete on frontier-model day-0 support and AMD/accelerator breadth.

- **Local/edge runtime — llama.cpp.** GGUF-based, CPU/hybrid/GPU offload, wide backend coverage (CUDA, ROCm, SYCL, Hexagon, OpenCL, Metal). Focused on kernel-level optimization for quantized formats and non-NVIDIA silicon, plus a steady stream of new architecture support (Inkling, DiffusionGemma).

- **Local runtime + distribution — Ollama.** Wraps llama.cpp/MLX behind a distribution and UX layer. Its competitive surface is turnkey model availability (LFM3), OpenAI API compatibility, and cross-platform coverage — not kernel innovation.

- **LLM gateway — LiteLLM.** A router above all of the above: provider abstraction, virtual keys, budgets, spend logs, protocol translation (OpenAI ↔ Anthropic ↔ Vertex ↔ Responses API). Its roadmap item #31263 (**Rust gateway targeting sub-1ms overhead**) is the highest-signal architectural bet in the tracker.

- **Training/fine-tuning — Unsloth.** LoRA/QLoRA training plus Studio; it also has MLX inference paths but its primary layer is fine-tuning. Its issues are dominated by dataset formatting, memory estimation, and Studio UX rather than serving throughput.

**Implication:** a production stack in 2026 typically composes all four — e.g., vLLM or SGLang for serving, LiteLLM as gateway, Ollama/llama.cpp for local/dev and edge, Unsloth for adaptation. The layering means many “bugs” are actually cross-layer: LiteLLM’s `$0` streaming spend interacts with provider-reported `usage.cost`; Ollama’s `/v1/models` returning `null` breaks typed gateway clients.

## 6. Trend Signals

**1. Silent failure modes are the dominant risk class.** The top-severity items across four projects are all silent: llama.cpp HIP Gated Delta Net leaks recurrent state and replays prior prompts verbatim (#29092); vLLM NCCL weight transfer reports success while moving zero bytes (#57022); LiteLLM streaming hits log `$0` (#48937); Unsloth masks out 80%+ of Alpaca rows in completions mode (#15894/#15895). **Watch:** build explicit verification (post-transfer checksums, row-count audits, tool-call presence checks) rather than trusting HTTP 200.

**2. Speculative decoding is mature but not yet composable.** MTP/EAGLE/DFlash ship broadly, but composition bugs are surfacing: vLLM hybrid-GDN prefix-cache misses under MTP (#52244); SGLang silent draft-weight corruption via `update_weights_from_disk` (#46806); llama.cpp draft-MTP context sized from `n_ctx()` instead of `n_ctx_seq()` (#28433). **Watch:** benchmark cache hit rates with MTP *on*, not just off.

**3. Hybrid/MoE architectures are the new normal — and they break assumptions.** GDN, DSA, mamba, MoE index sizing (`int64_t` in Inkling), and expert-offload caches appear across every engine. Hopper-centric code paths still fail on Ampere (vLLM #40124). **Watch:** capacity-plan by architecture family, not just model size.

**4. Accelerator fragmentation is accelerating, not consolidating.** AMD datacenter (gfx942/gfx950) is first-class now; consumer RDNA3/4 is in flight but **not default-safe** (SGLang #47254 — RDNA boots to `aiter`, which has no RDNA kernels); Intel SYCL, Qualcomm Hexagon, OpenCL/Adreno, Ascend NPU, and T-Head PPU all have active work. **Watch:** vLLM on Strix Halo APU is still ~6–7× slower than llama.cpp single-stream (#34579); batched throughput is the only AMD/APU play today.

**5. Cache-aware routing is the next gateway battleground.** SGLang is planning supernode-wide L1/L2 KV sharing (#43098) and PD single-protocol unification (#34510); LiteLLM already routes on `supports_web_search` (#49005) and prompt-cache dialect (#45295). **Watch:** if you build your own gateway today, you may be rebuilding what SGLang/LiteLLM will own in 2–3 quarters.

**6. OpenAI-compatibility remains a leaky abstraction at the edges.** Ollama returns `"data": null` instead of `[]` (#19381), drops reasoning for content-parts arrays (#19380), and silently accepts invalid content types alongside `tool_calls` (#19389). SGLang’s Kimi-K3 returns 400 when combining tools + JSON schema, while glm47/DeepSeek **silently drop tool calls and hallucinate** (#47167). **Watch:** validate tool-call *presence*, not just status codes; add null-tolerance to list fields in typed SDKs.

**7. Cold-start and startup latency is a first-class production cost.** SGLang FlashInfer `cascade` JIT compiles ~4.5s inside the first prefixed request (#46803); LiteLLM multi-worker proxies crash-looped on uvicorn’s 5s health ping (#45039, fixed); Unsloth is systematically trimming Studio startup (#15943, #15947). **Watch:** pre-warm with a dummy prefixed request on autoscaled/scale-to-zero deployments.

**8. Rust rewrites are converging on gateway and frontend hot paths.** vLLM’s Rust frontend (#64492) and LiteLLM’s sub-1ms Rust gateway (#31263) both target the same target: control-plane overhead that Python can’t hold. **Watch:** the gateway/frontend layer is where the next order-of-magnitude latency wins will come from, not the kernels.

**9. Supply-chain and air-gap operability is under-served.** LiteLLM downloads Prisma binaries at startup (#33167, open since July, 8 👍); Unsloth’s py3.13 install requires a Rust toolchain via an `outlines_core` pin (#47242). **Watch:** for regulated deployments, bake binaries and pin Python versions — don’t rely on runtime fetches.

**10. Stale closures are not fixes.** SGLang explicitly flags five issues closed with the `inactive` label today (FP8 KV logprob mismatch, DCP OOB, HiCache write-through gap, PD `KVTransferError`) as **stale-closed, not fixed**. **Watch:** when triaging regressions, check `closed_reason` — the GitHub default view overstates resolution.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-29

## Today's Highlights
No releases landed in the last 24 hours; activity is concentrated in PR review and issue triage. The dominant theme is **KV-cache/offload correctness** — four separate bugfix PRs target tiered-cache hit reporting, eviction races, and prefix-cache behavior under MTP speculative decoding. In parallel, ROCm performance work for Kimi-K3, Qwen3.5 and DeepSeek-v4.1 is advancing, and model enablement continues with Muse Spark NVFP4 and a Mistral-Large-4-1T-A52B recipe.

## New Model & Hardware Support
- **[PR #64525][1] — Hybrid MegaMoE/TRT-LLM execution for Muse Spark NVFP4** (NVIDIA). New model path combining MegaMoE with TensorRT-LLM kernels for NVFP4 weights.
- **[PR #64524][2] — DeepGEMM commit bump for Muse Spark** (closed/merged). Kernel dependency update required by the above.
- **[PR #64527][3] — Mistral-Large-4-1T-A52B recipe.** Deployment recipe for a 1T-parameter / 52B-active MoE.
- **[PR #64031][4] — Kimi-K3 DSpark Markov sampling on ROCm.** Shards sampling over the vocabulary across TP ranks with a single launch per draft position.
- **[PR #51406][5] — ROCm: fused QK-norm + RoPE + gate Triton kernel for Qwen3-Next/Qwen3.5**, previously CUDA-gated only.
- **[PR #60102][6] — ROCm gfx942: route sparse MLA prefill (≥512 query rows) to AITER `pa_prefill_sparse`** for DeepSeek-v4.1.
- **[PR #57924][7] — Nemotron VL: move image/video normalization to device**, eliminating CPU preprocessing on the critical path.
- **[PR #64362][8] — OpenVLA chat-template fallback**, fixing HTTP 400s when serving `openvla/openvla-7b` via `/v1/chat/completions`.
- **[Issue #31085][9] / [Issue #30135][10] — SM120 (RTX PRO 6000 Blackwell) NVFP4/MXFP4 gaps.** SM120 is still not recognized in MXFP4 backend selection, forcing fallback to Marlin; open feature requests, no fix PR linked.
- **[Issue #38175][11] — RFC tracker: full CUDA Graph for ViT encoders** (Qwen3-VL, Kimi K2.5, GLM-V), targeting per-layer launch overhead in multimodal serving.
- **[Issue #30679][12] — RFC: replace `torch.cuda` with `torch.accelerator`** to unblock non-CUDA backends; still open, 26 comments.

## Performance & Optimization
- **[PR #64290][13] — Single-pass Triton RMSNorm with PDL for fp32 (Gemma) weights.** Tested end-to-end on Qwen3.8-27B BF16, TP=1, MTP=3 on GB300; draft, not yet ready.
- **[PR #64031][4] — Kimi-K3 on ROCm (TP>1):** rank-local `lm_head` shard of logits (no base-logits all-gather) plus peer exchange of one tagged record per position — removes the all-gather from the sampling hot path.
- **[PR #62560][14] — MRV2 memory growth under load.** The sampler allocates an fp32 copy of `[num_reqs, vocab]` logits every step; after `capture_model()`'s `empty_cache()`, each new largest batch size triggers fresh allocation. Fix warms the sampler post-capture.
- **[PR #60102][6] — gfx942 sparse MLA prefill** offloaded to AITER for long-query prefills; shorter calls stay on the in-tree kernel.
- **[Issue #59575][15] — MI355X / gfx950 optimization tracker for Qwen3.8-Flash-Next** (GatedDeltaNet ×36 layers, QuerySparse Attention ×12, HyperConnection gated residual, MXFP4 MoE); work lands incrementally via PRs.
- **[Issue #57406][16] — GLM-5.3 optimization plan**, following the GLM-5.2 `glm_moe_dsa` work; CUDA items still open.
- **Open regressions worth tracking:** [Issue #34579][17] (GPT-OSS-120B on AMD Strix Halo APU: 7.5 tok/s vs llama.cpp's 50 tok/s) and [Issue #38887][18] (Gemma 4 E4B ~9 tok/s on RTX 4090 under forced `TRITON_ATTN` fallback, v0.19.0). Both stale, no fix PRs.

## Stability & Regressions
Ranked by severity:

1. **[PR #64141][19] — Triton attention overwrites shared KV cache during scale-view creation.** Inline scale-cache fills can clobber already-written cache contents; correctness/corruption risk. Fix PR open, addresses #63944.
2. **[PR #57022][20] — NCCL weight transfer silently no-ops.** `PyNcclCommunicator` sets `disabled=True` when NCCL won't load (e.g., XPU) or `VLLM_DISABLE_PYNCCL=1`, and every collective returns early — a round reports success while moving zero bytes, leaving workers on stale weights. Fix PR open.
3. **[PR #52244][21] — Hybrid GDN prefix-cache misses under MTP spec decoding** (Qwen3.5-122B-A10B). Cached prompts never reach expected depth, and prompts whose length is a multiple of the hash unit get no hit at all (1072-token GDN page). Fix PR open, needs rebase.
4. **[PR #62615][22] / [PR #64444][23] / [PR #61717][24] — KV-offload tiering correctness cluster.** A reported primary HIT can be evicted by a secondary-tier promotion before `prepare_load()`; successful stores aren't published to lookup state; `AsyncLookupManager` pins a resolved "absent" verdict for later requests, hiding blocks written by other instances. All three fixes open.
5. **[Issue #36489][25] — Qwen3.5 397B FP8 unexpected crash on A100** (v0.17.0), repeatable after runtime; open and stale, no linked fix.
6. **[Issue #40124][26] — TurboQuant + hybrid MoE (Qwen3.6-35B-A3B) broken on Ampere SM 80–86**, with 13 community patches; closed, Hopper+ assumptions remain in several paths.
7. **[Issue #30142][27] — `assert request.num_output_placeholders >= 0` failure under async scheduling** (v0.12.0, 8×H200); closed.
8. **[Issue #26195][28] — Missing cached item in `MultiModalReceiverCache`** at large batch size with multi-modal serving; closed.
9. **[PR #64492][29] — Rust frontend rejects assistant turns with null/omitted/empty content**, diverging from the Python frontend's 400-vs-accept behavior; fix open.

## What This Means for Application Developers
- **Tiered KV cache is not yet trustworthy at the edges.** If you run multi-instance or filesystem/object-store KV offload, three open PRs fix missed hits and premature evictions. Pin to a build containing #62615, #64444 and #61717 before relying on cross-instance prefix reuse for cost reduction.
- **Speculative decoding + prefix caching can silently degrade TTFT.** On Qwen3.5-class hybrid GDN models with MTP, prompts aligned to hash-unit boundaries may get zero cache benefit (#52244). Benchmark cache hit rates with MTP enabled, not just with it off.
- **Verify weight updates actually propagate in disaggregated setups.** The NCCL no-op bug (#57022) means a rollout can appear successful while serving stale weights — worth an explicit post-transfer checksum if you depend on dynamic weight refresh.
- **Rust frontend is reaching parity but still lags Python on edge cases** (`return_prompt_text` in #64526, assistant-turn handling in #64492). Gate migrations on your actual message shapes, especially tool-calling transcripts.
- **TurboQuant KV quantization is the most-requested feature** (52 comments, 129 👍 on #38171) but remains unmerged, and the Ampere hybrid-MoE breakage (#40124) signals the code paths are still Hopper-centric. Treat sub-8-bit KV cache on SM 80–86 as unsupported for now.
- **AMD capacity planning:** Kimi-K3 and Qwen3.8-Flash-Next optimizations are landing for gfx950/MI355X, but single-stream throughput on APUs (Strix Halo) still trails llama.cpp by ~6–7× — vLLM remains a batched-throughput play there, not a latency play.

[1]: https://github.com/vllm-project/vllm/pull/64525
[2]: https://github.com/vllm-project/vllm/pull/64524
[3]: https://github.com/vllm-project/vllm/pull/64527
[4]: https://github.com/vllm-project/vllm/pull/64031
[5]: https://github.com/vllm-project/vllm/pull/51406
[6]: https://github.com/vllm-project/vllm/pull/60102
[7]: https://github.com/vllm-project/vllm/pull/57924
[8]: https://github.com/vllm-project/vllm/pull/64362
[9]: https://github.com/vllm-project/vllm/issues/31085
[10]: https://github.com/vllm-project/vllm/issues/30135
[11]: https://github.com/vllm-project/vllm/issues/38175
[12]: https://github.com/vllm-project/vllm/issues/30679
[13]: https://github.com/vllm-project/vllm/pull/64290
[14]: https://github.com/vllm-project/vllm/pull/62560
[15]: https://github.com/vllm-project/vllm/issues/59575
[16]: https://github.com/vllm-project/vllm/issues/57406
[17]: https://github.com/vllm-project/vllm/issues/34579
[18]: https://github.com/vllm-project/vllm/issues/38887
[19]: https://github.com/vllm-project/vllm/pull/64141
[20]: https://github.com/vllm-project/vllm/pull/57022
[21]: https://github.com/vllm-project/vllm/pull/52244
[22]: https://github.com/vllm-project/vllm/pull/62615
[23]: https://github.com/vllm-project/vllm/pull/64444
[24]: https://github.com/vllm-project/vllm/pull/61717
[25]: https://github.com/vllm-project/vllm/issues/36489
[26]: https://github.com/vllm-project/vllm/issues/40124
[27]: https://github.com/vllm-project/vllm/issues/30142
[28]: https://github.com/vllm-project/vllm/issues/26195
[29]: https://github.com/vllm-project/vllm/pull/64492

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-29

## 1. Today's Highlights

CI reliability is the loudest signal in the repo right now: the flaky-test tracker (#42752) has ballooned to 137 comments, and several of today's PRs are blocked behind red `PR Test Base` / `PR Test Extra` runs. On the roadmap side, Q4 prefill-decode disaggregation planning (#43699) and the PD single-protocol-layer unification tracker (#34510) are both active, alongside a concrete push to officially support AMD consumer RDNA3/RDNA4 cards (#30599, #46900, #47254). Two new correctness-class bugs landed in the last 24h worth attention: a DeepSeek-V4.1-Flash scheduler crash on aborted multimodal streams (#47218) and silent weight corruption when updating a DFlash draft model via `update_weights_from_disk` (#46806).

## 2. Releases & Breaking Changes

No releases, API breaks, or migration notes in the last 24 hours. Two dependency-adjacent changes are in flight:

- **[Chore] Bump transformers 5.19.0 → 5.21.0** — adapts chat-template mirroring, rope compat tests, Nemotron-H config defaults, Transformers-backend TP styles, MiniCPM-V 4.6 image sizing ([PR #47204](https://github.com/sgl-project/sglang/pull/47204)).
- **Python 3.13 install requires a Rust toolchain** — hidden and undocumented, traced to `outlines==0.1.11 → outlines_core==0.1.26` exact pin. A packaging fix is needed before 3.13 becomes frictionless ([#47242](https://github.com/sgl-project/sglang/issues/47242)).

## 3. New Model & Hardware Support

- **AMD consumer RDNA3/RDNA4 (gfx1100/1101, gfx1200/1201)** — umbrella tracking issue plus enablement plan to move ROCm beyond datacenter Instinct (`gfx942`/`gfx950`) ([#30599](https://github.com/sgl-project/sglang/issues/30599)); CI + upstreaming plan on `main` ([#46900](https://github.com/sgl-project/sglang/issues/46900)).
- **[Fix][AMD] Default consumer RDNA to Triton attention** — on `main` the ROCm default backend is `aiter`, which has no RDNA kernels, so a default-flag RDNA server never comes up (first decode CUDA-graph capture fails). This PR makes consumer cards bootable out of the box ([PR #47254](https://github.com/sgl-project/sglang/pull/47254)).
- **T-Head PPU / SAIL (Zhenwu 810, 810E, M890P)** — highest-priority roadmap for first-class upstream support ([#37519](https://github.com/sgl-project/sglang/issues/37519)).
- **`sensenova_loop` multimodal** — native text/image/video inference for a Qwen3.5-derived model with shared-weight layer repetition and multi-stream hyper-connections (mHC) ([PR #47281](https://github.com/sgl-project/sglang/pull/47281)).
- **DFlash + Nemotron Bolt E4B GGUF** — speculative decoding for `nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-4GB` with its BF16 GGUF drafter; native config support for plain DFlash checkpoints stored in GGUF ([PR #47279](https://github.com/sgl-project/sglang/pull/47279)).
- **Ascend NPU: DSpark compact mode for DeepSeek-V4** — replaces uniform verify windows / eager post-processing in speculative decoding with SPS-policy-selected windows ([PR #38440](https://github.com/sgl-project/sglang/pull/38440)); **A5 explicit-state bring-up + position-addressed C4 state on PD** ([PR #45723](https://github.com/sgl-project/sglang/pull/45723)).
- **Quantization:** `convrot_int8` validated for Wan2.2 A14B (T2V/I2V) and Wan-Animate-2 14B on GB200/H100 ([PR #46048](https://github.com/sgl-project/sglang/pull/46048)).
- **Transformers-backend models:** an additional new multimodal model is pending but its identity is redacted in the PR body ([PR #47271](https://github.com/sgl-project/sglang/pull/47271)).

## 4. Performance & Optimization

- **DeepSeek-V4 prefill on SM120 may be badly underperforming.** User reports ~2–7K tok/s on 4× RTX PRO 6000 (TP=4, PCIe, no NVLink) vs ~12.5K tok/s with vLLM/marlin on identical hardware; kernel-coverage gaps are suspected but unconfirmed. Worth tracking if you serve DSV4 on Blackwell workstation cards ([#33422](https://github.com/sgl-project/sglang/issues/33422)).
- **[Perf][AMD] gfx950 KDA prefill extend on AITER Gluon `chunk_kda`** — covers the bf16 SSM state-pool path (`--mamba-ssm-dtype bfloat16`, used in the Kimi-K3 MI355X recipe) that the existing HIP gfx950 kernel misses because it only accepts fp32 state pools ([PR #47036](https://github.com/sgl-project/sglang/pull/47036)).
- **[Perf][AMD] Kimi-K3: Quark FP8 KDA in-proj + MLA absorb on FP8 kernels** — attention weights in Kimi-K3 Quark checkpoints are per-channel quantized; this routes the KDA in-projection and MLA absorb through FP8 kernels instead of upcasting ([PR #44362](https://github.com/sgl-project/sglang/pull/44362)).
- **Speculative decoding latency at low concurrency** — opt-in step-level CUDA graph plus fused verify/relay paths for EAGLE v2 chains, targeting host work and small launches between per-stage graphs at 1–4 requests with topk-1 ([PR #46211](https://github.com/sgl-project/sglang/pull/46211)); megakernel work adds DSA/MLA binding views, EAGLE draft capture checks, and early draft embedding sharing ([PR #46210](https://github.com/sgl-project/sglang/pull/46210)).
- **Diffusion:** replay LTX head-gate and RMSNorm launches to recover eager host-dispatch regression from #46109/#46110 without reverting their GPU gains ([PR #47270](https://github.com/sgl-project/sglang/pull/47270)); fixed colored seams in single-GPU Qwen-Image-2.1-Turbo VAE tiling, now closed ([PR #47092](https://github.com/sgl-project/sglang/pull/47092)).
- **Active model-specific perf trackers:** GLM-5.3-Flash on Hopper SM90 ([#45373](https://github.com/sgl-project/sglang/issues/45373)), Qwen3.8-Flash-Next on gfx950/MI355X ([#44304](https://github.com/sgl-project/sglang/issues/44304)).
- **Startup latency:** FlashInfer `cascade` JIT-compiles inside the first prefixed request (~4.5 s) on pip installs — a cold-start tax for serverless/autoscaled deployments ([#46803](https://github.com/sgl-project/sglang/issues/46803)).
- **Router/cache roadmap:** Agentic Cache Scheduling (Q4) proposes supernode-wide L1/L2 KV sharing with cache-location-aware routing across engines ([#43098](https://github.com/sgl-project/sglang/issues/43098)).

## 5. Stability & Regressions

Ranked by severity:

1. **Scheduler crash takes down the whole serving container** — DeepSeek-V4.1-Flash: `items_offset_list` is `None` in the multimodal embedding path under concurrent aborted streams, killing all TP/EP workers. Image `lmsysorg/sglang:dev-dsv41`. No fix PR visible yet ([#47218](https://github.com/sgl-project/sglang/issues/47218)).
2. **Unbounded allocation OOM-crashes the entire TP group** — DeepSeek-V4.1-Flash + DSPARK: `SparsePrefillWorkspace` grows unbounded ([#41076](https://github.com/sgl-project/sglang/issues/41076)).
3. **Silent weight corruption in RL/fine-tuning workflows** — `update_weights_from_disk` loads the *target's* weights into a DFLASH draft because there is no `selector` to disambiguate; the old `disable_draft_model` escape hatch was removed when `selector` was introduced. Same symptom previously reported with EAGLE3 external drafts. No fix PR yet ([#46806](https://github.com/sgl-project/sglang/issues/46806)).
4. **Tools + JSON-schema format cannot be combined** — Kimi-K3 returns HTTP 400; glm47 and DeepSeek silently drop tool calls and hallucinate an answer, breaking `Codex --output-schema` ([#47167](https://github.com/sgl-project/sglang/issues/47167)).
5. **HiCache on hybrid GDN models: host-tier hit rate collapses** — once the evicted working set exceeds ~3 long prefixes, host mamba pool is exhausted while KV stays resident on host, so hits drop to zero ([#45400](https://github.com/sgl-project/sglang/issues/45400)).
6. **GLM-5.3 FP8 inference hangs** after sustained load with full KV cache (TP8, EAGLE, H200) ([#45781](https://github.com/sgl-project/sglang/issues/45781)); broad GLM-5.3 bug tracker still has ~8 unchecked items ([#37524](https://github.com/sgl-project/sglang/issues/37524)).
7. **EAGLE native MTP sampling hang on Ampere** — spins in `tree_speculative_sampling_target_only` at 100% GPU util / low power (2× A2, TP=2, fp8_e5m2 KV) ([#35822](https://github.com/sgl-project/sglang/issues/35822)).
8. **`sgl_kernel` AOT per-op cubin gap on sm_75** — `rmsnorm` fails with `cudaErrorNoKernelImageForDevice` while `silu_and_mul` / `gelu_and_mul` work; no soft fallback to torch/Triton ([#38515](https://github.com/sgl-project/sglang/issues/38515)).
9. **API-surface correctness:** Responses API replays a Codex turn as three assistant blocks, teaching the model to end its turn before the tool call ([#42110](https://github.com/sgl-project/sglang/issues/42110)).
10. **CI/merge-queue health** — flaky tests and NVIDIA `PR Test Base`/`PR Test Extra` infra failures tracked in #42752 (137 comments); complements #17050 ([#42752](https://github.com/sgl-project/sglang/issues/42752)).

**Fixes in flight:** `stop_regex` window bound now counted in UTF-8 bytes instead of code points, so matches split across more tokens than the bound no longer run to `max_new_tokens` ([PR #47280](https://github.com/sgl-project/sglang/pull/47280)); Anthropic endpoint accepts `thinking.type=enabled` without `budget_tokens` (DeepSeek-style clients pass effort in `output_config.effort`), previously a 400 ([PR #45364](https://github.com/sgl-project/sglang/pull/45364)); PD bootstrap server now starts before prefill schedulers, fixing `Decode handshake failed … Aborted by AbortReq` under `RayEngine`/`--use-ray` ([PR #47282](https://github.com/sgl-project/sglang/pull/47282)); Mooncake store RPCs now carry per-request context for end-to-end tracing ([PR #37976](https://github.com/sgl-project/sglang/pull/37976)).

**Caveat on closures:** several issues moved to CLOSED with the `inactive` label today — FP8 KV-cache logprob mismatch at index 96 on H100 ([#25790](https://github.com/sgl-project/sglang/issues/25790)), DCP virtual-loc index-K OOB on GLM-5.3 ([#36886](https://github.com/sgl-project/sglang/issues/36886)), MiMo-V2.5 audio hang under `--enable-dp-attention` ([#37059](https://github.com/sgl-project/sglang/issues/37059)), HiCache chunked-prefill write-through gap ([#33714](https://github.com/sgl-project/sglang/issues/33714)), and PD `KVTransferError` decode-instance-dead ([#37022](https://github.com/sgl-project/sglang/issues/37022)). These are stale-closed, not fixed — do not treat them as resolved.

## 6. What This Means for Application Developers

- **Structured output + tool calling is the sharpest edge right now.** If your agent relies on `tool_choice` together with `response_format`/JSON schema, expect 400s on Kimi-K3 and *silent* tool-call loss on glm47/DeepSeek — the worst failure mode, since the model fabricates an answer instead of erroring. Validate tool-call presence in your client, not just HTTP status (#47167).
- **Don't trust weight-update endpoints blindly in RL/FP pipelines.** Until #46806 lands, a `update_weights_from_disk` call can overwrite your draft model with target weights and produce degraded-but-not-erroring output. Pin a known-good version or verify draft config post-update.
- **PD disaggregation under Ray is currently broken** (`503 Prefill server not fully registered yet` → handshake abort). If you run disaggregated prefill on `--use-ray`, hold off or apply PR #47282.
- **AMD Radeon is becoming viable but not yet default-safe.** Until PR #47254 lands, a consumer RDNA card with default flags won't start because ROCm defaults to `aiter`, which has no RDNA kernels; explicitly select the Triton attention backend.
- **Cold-start budgeting:** FlashInfer `cascade` adds ~4.5 s of JIT to the first request on pip installs (#46803). Pre-warm with a dummy prefixed request if you're on autoscaling or scale-to-zero.
- **Python 3.13 users:** provision a Rust toolchain in your image build, or stay on 3.12 until the `outlines_core` pin is relaxed (#47242).
- **Roadmap signal worth planning around:** supernode-wide KV cache sharing with cache-location-aware routing (#43098) and the PD single-protocol-layer unification (#34510) both point toward SGLang owning more of the cross-engine routing layer — relevant if you're building your own gateway in front of it today.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-29

## 1. Today's Highlights

Backend optimization dominated the last 24 hours: SYCL landed a DPAS flash-attention kernel that runs directly on quantized KV blocks, and Hexagon picked up vectorized UPSCALE, blocked IM2COL, and improved transposed CONCAT bandwidth — all in the b11971–b11982 release window. On the stability side, the most serious open report is a HIP/ROCm fused Gated Delta Net op that leaks recurrent state across requests on reused server slots, which is a correctness hazard for production serving. Grammar/JSON-schema handling also drew attention, with a fix PR already open for the LFM3 `response_format` 500 error.

## 2. Releases & Breaking Changes

No explicit API removals or config-breaking changes in this batch. Notable behavioral fixes and hardening:

- **b11982** — hexagon: UPSCALE op support, with optimized nearest 2x/4x and bilinear 4x paths, non-contiguous rows, and gather-based staging ([#31331](https://github.com/ggml-org/llama.cpp/pull/31331))
- **b11981** — hexagon: improved DDR bandwidth utilization in transposed CONCAT via larger row blocks, double-buffered scratchpads, and padded VTCM strides ([#31277](https://github.com/ggml-org/llama.cpp/pull/31277))
- **b11980** — SYCL: DPAS flash attention kernel for quantized KV cache ([#31125](https://github.com/ggml-org/llama.cpp/pull/31125))
- **b11979** — SYCL: vectorized reordered-layout dequantization stores for Q4_0, K-quants, IQ3, MXFP4 ([#30976](https://github.com/ggml-org/llama.cpp/pull/30976))
- **b11978** — tests: fix `comp_padding` CUDA graph reservation ([#31344](https://github.com/ggml-org/llama.cpp/pull/31344))
- **b11977** — ggml-openvino: correctly slice VIEW inputs of SCALE on the dynamic path ([#31334](https://github.com/ggml-org/llama.cpp/pull/31334))
- **b11976** — Hexagon: block-based HVX-parallel IM2COL with multi-thread output-column blocks ([#31050](https://github.com/ggml-org/llama.cpp/pull/31050))
- **b11972** — common/mtmd: bound control-vector layer indices by model layer count and clamp M-RoPE position loops — **input-validation hardening, recommended update for servers** ([#31336](https://github.com/ggml-org/llama.cpp/pull/31336))
- **b11971** — OpenCL: tiled q4_K GEMM for Adreno, used for tiled q4_K LM head at batch > 1 ([#30412](https://github.com/ggml-org/llama.cpp/pull/30412))

**Pending config-semantics change:** [PR #31371](https://github.com/ggml-org/llama.cpp/pull/31371) stops `n_ctx_slot()` from unconditionally clamping per-slot context to `n_ctx_train`, which currently makes YaRN/`--rope-scaling` context extension ineffective. If you rely on RoPE scaling beyond training context, track this PR.

## 3. New Model & Hardware Support

- **Spark2.5 DSpark draft models** — conversion and inference support, with embedded `draft_model.*` weights exported for speculative decoding ([#31372](https://github.com/ggml-org/llama.cpp/pull/31372))
- **DSA / dspark extended to a new model** — follow-up work on the draft/speculative path ([#31376](https://github.com/ggml-org/llama.cpp/pull/31376))
- **ROCm/HIP: RDNA4_M family** — new `gfx1170`/`gfx1171` targets with 128b WMMA, distinct from existing RDNA4 ([#31373](https://github.com/ggml-org/llama.cpp/pull/31373))
- **TML Inkling architecture** — converter, graph build, and a banded Flash Attention kernel; uses `int64_t` on some ops to avoid MoE index overflow. In progress ([#25731](https://github.com/ggml-org/llama.cpp/pull/25731))
- **DiffusionGemma** — preliminary diffusion-model support with `llama-diffusion-cli` and live visualization. In progress ([#24423](https://github.com/ggml-org/llama.cpp/pull/24423))
- **Backend capability expansion** — Hexagon gains UPSCALE and parallel IM2COL; OpenCL/Adreno gains tiled q4_K GEMM; ggml-openvino fixes SCALE view semantics. These reduce op-fallback pressure tracked in [#14909](https://github.com/ggml-org/llama.cpp/issues/14909) (open, 57 comments).

## 4. Performance & Optimization

- **SYCL quantized-KV flash attention (b11980):** decode and small batches (1–31 query tokens) now run FA directly on q4_0 and q8_0 KV blocks, eliminating scalar dequant/convert work. Direct win for long-context memory-bound serving on Intel GPUs ([#31125](https://github.com/ggml-org/llama.cpp/pull/31125))
- **SYCL dequant stores (b11979):** the reordered-layout Q4_0 kernel previously processed one `qs` byte per work-item with two scalar stores strided 16 elements apart; stores are now widened, with the same treatment applied to K-quants, IQ3, and MXFP4 ([#30976](https://github.com/ggml-org/llama.cpp/pull/30976))
- **Hexagon IM2COL (b11976):** block-based HVX parallelization across threads, with padding fixes and common-case loop unrolling — targets vision/conv workloads on Snapdragon NPUs ([#31050](https://github.com/ggml-org/llama.cpp/pull/31050))
- **Hexagon transposed CONCAT (b11981):** doubled row block size and scratchpad double-buffering to raise DDR bandwidth utilization ([#31277](https://github.com/ggml-org/llama.cpp/pull/31277))
- **OpenCL/Adreno q4_K GEMM (b11971):** tiled dp4a q4_K GEMM split into its own file with a dp4a capability check; benefits batch > 1 (prompt processing) ([#30412](https://github.com/ggml-org/llama.cpp/pull/30412))
- **MoE cache redesign:** ClassLRU hybrid (LRFU/MQ-like, decaying usage history) reports **+6% decode on QFN and +7% on GLM, with 8% fewer expert uploads** versus SLRU on a warm cache ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233)); companion PR adds `--moe-cache-cpu-share auto` with CRF admission for CPU misses ([#31286](https://github.com/ggml-org/llama.cpp/pull/31286))
- **Backend sampling memory:** host probability/candidate buffers are now sized to the max output width of bound sampler chains instead of vocabulary width ([#29576](https://github.com/ggml-org/llama.cpp/pull/29576))
- **CUDA graphs on Hopper:** re-instantiate graphs when a kernel's thread-block cluster dims change; cuBLAS F16 GEMMs pick different cluster shapes by `m` (e.g. 5,1,1 at m=1200 vs 2,2,1 at m=1040) while sharing one kernel ([#31284](https://github.com/ggml-org/llama.cpp/pull/31284))

## 5. Stability & Regressions

Ranked by severity:

1. **Cross-request state contamination (critical, open):** HIP/ROCm fused Gated Delta Net op carries recurrent state across requests on a reused server slot (qwen35/qwen35moe); earlier prompts' text is emitted verbatim in later completions. No fix PR visible — mitigate with per-request slots or non-hybrid models on ROCm ([#29092](https://github.com/ggml-org/llama.cpp/issues/29092))
2. **CUDA graphs hang GPU channel on RTX 5090 Laptop / sm_120 (open):** RC watchdog + Xid 8; `GGML_CUDA_DISABLE_GRAPHS=1` is a complete workaround ([#27330](https://github.com/ggml-org/llama.cpp/issues/27330))
3. **Grammar / JSON-schema correctness cluster:**
   - LFM3: any `response_format` (`json_schema` or `json_object`) returns HTTP 500 "Unexpected empty grammar stack"; **fix PR open** ([#31328](https://github.com/ggml-org/llama.cpp/issues/31328), [#31374](https://github.com/ggml-org/llama.cpp/pull/31374))
   - `json-schema-to-grammar` copies raw control characters (e.g. NUL from `\u0000`) into `pattern` verbatim → "failed to parse grammar" ([#31199](https://github.com/ggml-org/llama.cpp/issues/31199), [#28697](https://github.com/ggml-org/llama.cpp/pull/28697) addresses direct-schema form)
   - Large `enum` in a JSON schema causes superlinear grammar construction, pinning a core for tens of seconds — server-side DoS ([#29457](https://github.com/ggml-org/llama.cpp/issues/29457))
4. **SYCL prompt-processing regression:** `llama-server` ~35% slower in b11804 vs b11046 with CPU-offloaded MoE experts (`-ncmoe`); not reproduced by `llama-bench`, suggesting a server-path issue ([#31262](https://github.com/ggml-org/llama.cpp/issues/31262))
5. **Metal init failure on macOS 14 / M2 Ultra:** constexpr bfloat cast in `unary.metal` ([#31272](https://github.com/ggml-org/llama.cpp/issues/31272))
6. **`--split-mode tensor` crash:** `GGML_ASSERT(bcj.nodes[i])` on the second request with 2x/3x Tesla P100 + CUDA ([#29466](https://github.com/ggml-org/llama.cpp/issues/29466))
7. **Speculative decoding defects:** draft-mtp context sized from `llama_n_ctx()` instead of `llama_n_ctx_seq()`, killing the server at decode entry on large `--ctx-size` ([#28433](https://github.com/ggml-org/llama.cpp/issues/28433)); mtmd image chunks leave positional holes in the draft KV cache → HTTP 500 with `draft-dflash` ([#27408](https://github.com/ggml-org/llama.cpp/issues/27408)); sidecar MTP draft fails with `blk.0.attn_q not found` ([#29345](https://github.com/ggml-org/llama.cpp/issues/29345)); general speculative decode perf drop reported ([#31220](https://github.com/ggml-org/llama.cpp/issues/31220))
8. **SYCL CPY correctness:** copy to q4_1/q5_1/q8_0 does not match CPU reference ([#31140](https://github.com/ggml-org/llama.cpp/issues/31140))
9. **Security:** instruction injection via the `?q=` WebUI query parameter ([#25790](https://github.com/ggml-org/llama.cpp/issues/25790)); GBNF stack overflow via nested repetition (`llama_grammar_advance_stack`) now closed ([#18988](https://github.com/ggml-org/llama.cpp/issues/18988)) — the b11972 bounds hardening is related follow-through
10. **Server lifecycle:** Metal OOM during init still reported "model loaded" and bound the port, then 500'd every request — closed as stale ([#27309](https://github.com/ggml-org/llama.cpp/issues/27309)); Jetson Orin NX hang before serving after the b8638→b9016 server rewrite ([#29499](https://github.com/ggml-org/llama.cpp/issues/29499))

## 6. What This Means for Application Developers

- **Long-context on Intel GPUs got cheaper.** With DPAS FA on q4_0/q8_0 KV, you can keep quantized KV cache for decode-heavy workloads without falling back to dequant-and-convert, directly reducing memory pressure for multi-turn agents. Validate on your SKU: SYCL has a concurrent ~35% prompt-processing regression with `-ncmoe` ([#31262](https://github.com/ggml-org/llama.cpp/issues/31262)).
- **Treat grammar-constrained output as a hardening surface.** Both `response_format` 500s ([#31328](https://github.com/ggml-org/llama.cpp/issues/31328)) and superlinear `enum` compilation ([#29457](https://github.com/ggml-org/llama.cpp/issues/29457)) are attacker-reachable if you accept user-supplied schemas. Cap schema size, pre-compile static grammars, and run the server behind a request timeout.
- **Reasoning-budget + `enable_thinking: false` is a fragile combination.** llama.cpp forces a `</think>` that can collide with a template-inserted tag; the fix is in review ([#31374](https://github.com/ggml-org/llama.cpp/pull/31374)). Until merged, avoid `--reasoning-budget 0` together with `response_format`.
- **Do not co-locate hybrid-model requests on shared ROCm slots** until [#29092](https://github.com/ggml-org/llama.cpp/issues/29092) is resolved — the failure mode is silent cross-tenant prompt leakage, the worst class of serving bug.
- **Blackwell laptop operators:** set `GGML_CUDA_DISABLE_GRAPHS=1` proactively on sm_120 until [#27330](https://github.com/ggml-org/llama.cpp/issues/27330) lands; the failure is a hard GPU channel hang, not a slow path.
- **MoE deployments should evaluate `moe-cache`.** The ClassLRU work ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233), [#31286](https://github.com/ggml-org/llama.cpp/pull/31286)) shows single-digit decode gains and measurably fewer expert uploads on warm caches — meaningful for CPU-offloaded MoE where PCIe traffic is the bottleneck.
- **Upgrade to b11982 or later for input hardening.** The control-vector and M-RoPE bounds checks in [b11972](https://github.com/ggml-org/llama.cpp/pull/31336) close a class of malformed-tensor-name crashes relevant to anyone loading user-supplied GGUFs.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-10-29

## Today's Highlights
Ollama shipped **v0.40.13**, bringing the Liquid AI **LFM3** family — including `lfm3-audio` (TTS) and `lfm3-omni` — to Linux and Windows in addition to Apple silicon. The issue tracker shows an active push to harden **OpenAI-compatibility semantics**, with fixes in flight for dropped assistant reasoning, `null` vs `[]` response fields, and invalid content-type handling. Meanwhile, several high-severity MLX and tool-calling regressions remain open, and a PR adding **Windows/CPU support to the MLX runner** signals broader backend convergence.

---

## Releases & Breaking Changes

- **v0.40.13 — LFM3 on Linux and Windows**  
  `lfm3`, `lfm3-audio`, and `lfm3-omni` now run across Linux, Windows, and Apple silicon. Previously these models failed on non-Apple platforms with “this model requires MLX support.”  
  [Release v0.40.13](https://github.com/ollama/ollama/releases/tag/v0.40.13) · [LFM3 model](https://ollama.com/library/lfm3) · [lfm3-audio](https://ollama.com/library/lfm3-audio) · [lfm3-omni](https://ollama.com/library/lfm3-omni)

- **MLX runtime requirement documentation gap (closed)**  
  Users hit an unclear MLX-runtime error when pulling `lfm3` on Linux; the requirement was undocumented. Closed, with platform support now landing in v0.40.13.  
  [Issue #19369](https://github.com/ollama/ollama/issues/19369) · [Issue #19343](https://github.com/ollama/ollama/issues/19343)

- **CI/Metal workarounds (no API impact)**  
  Buildx pinned to avoid CI deadlocks; temporary patch for a Metal `unary` library compile regression on older macOS.  
  [PR #19373](https://github.com/ollama/ollama/pull/19373) · [PR #19372](https://github.com/ollama/ollama/pull/19372)

---

## New Model & Hardware Support

- **LFM3 family, cross-platform** — text, audio (TTS), and omni models now supported on Linux and Windows, not just Apple silicon.  
  [Release v0.40.13](https://github.com/ollama/ollama/releases/tag/v0.40.13)

- **MLX runner: native Windows + CPU backend (open PR)** — Adds Windows execution for the MLX runner with a CPU backend alongside Metal/CUDA, and enables full TTS inference (e.g., `phonon:en`). Not yet merged/released.  
  [PR #19366](https://github.com/ollama/ollama/pull/19366)

- **Vision capability reporting corrected (closed)** — Models with audio-only projectors (e.g., Voxtral Mini) no longer falsely advertise `vision`. Relevant for capability-based routing.  
  [PR #19328](https://github.com/ollama/ollama/pull/19328)

- **Requested but not landed** — native audio/video input and audio output on Windows ([#19390](https://github.com/ollama/ollama/issues/19390)); ternary quantization for Bonsai 2 ([#19388](https://github.com/ollama/ollama/issues/19388), closed).

---

## Performance & Optimization

No throughput or latency benchmark numbers were published in this window. Relevant infrastructure work:

- **Runner eviction race fixed (closed)** — Prevents a runner marked for eviction from being resurrected by a concurrent request on the loaded-runner fast path; addresses scheduler/session-duration handling.  
  [PR #17416](https://github.com/ollama/ollama/pull/17416)

- **Explicit flash-attention for architectures that default to it (open)** — Targets GPT-OSS Q8 crashes at long context; `auto` flash-attention was disabled under partial offload. Primarily stability, with memory/latency implications for long-context serving.  
  [PR #17477](https://github.com/ollama/ollama/pull/17477)

- **Truncation preserves most recent user message (open)** — Fixes multi-step tool loops where truncation removed the current user query, causing `500: no user query found`.  
  [PR #18697](https://github.com/ollama/ollama/pull/18697)

---

## Stability & Regressions

Ranked by severity:

1. **`gpt-oss:120b` fails to load — GGUF metadata / MXFP4 tensor size overflow** (open)  
   `unsupported tensor "blk.0.ffn_down_exps.weight" size overflows` on v0.40.12; same overflow class previously reported on v0.40.1 with `gpt-oss:latest`. Blocks large MoE MXFP4 models entirely.  
   [Issue #19375](https://github.com/ollama/ollama/issues/19375) · [Issue #18869](https://github.com/ollama/ollama/issues/18869)

2. **MLX cache loss + HTTP 500 retries with Claude Desktop** (open, 0.40.11–0.40.12)  
   ~17K-token initial prefill on M4 Max with 256K configured context; slow first prompt, retries, and cache loss. Follow-up to #18474.  
   [Issue #19359](https://github.com/ollama/ollama/issues/19359)

3. **Qwen3 tool calls dropped by `go_template`** (closed)  
   `/api/chat` with `tools` returns an empty assistant turn (`content: ""`, `tool_calls: null`, `eval_count > 0`); llama.cpp’s template parses the same generation correctly. Serious for agent loops.  
   [Issue #19377](https://github.com/ollama/ollama/issues/19377)

4. **`qwen3.8:27b-mlx` never emits `tool_calls` after free-text content** (open)  
   Tool calling only works when `content` is empty; breaks the common “announce then call” agent pattern.  
   [Issue #19353](https://github.com/ollama/ollama/issues/19353)

5. **`ollama show --modelfile` emits unparseable output** (open)  
   Values containing `"""` plus whitespace produce a Modelfile that `ollama create` rejects — breaks model export/round-trip workflows.  
   [Issue #19342](https://github.com/ollama/ollama/issues/19342)

6. **Claude Code auto-compaction failure with `deepseek-v4.1-flash:1017-cloud`** (open)  
   Long runs fail to compact automatically, loop on “Goal not yet met,” and burn tokens.  
   [Issue #19249](https://github.com/ollama/ollama/issues/19249)

7. **`OLLAMA_NUM_PARALLEL=1` not serializing requests** (open, needs info)  
   Two concurrent sessions still execute; problematic for memory-constrained deployments relying on serialization.  
   [Issue #19262](https://github.com/ollama/ollama/issues/19262)

8. **macOS 27: app window and menu bar icon never appear** (closed)  
   Server and CLI work; Electron/CoreUI bundle lookup failure (`com.electron.ollama`).  
   [Issue #19370](https://github.com/ollama/ollama/issues/19370)

9. **OpenAI-compat correctness issues** (open, fixes in flight)  
   - Assistant `reasoning` dropped and reasoning-only turns deleted when `content` is a content-parts array — [Issue #19380](https://github.com/ollama/ollama/issues/19380), fix [PR #19384](https://github.com/ollama/ollama/pull/19384)  
   - `/v1/models` returns `"data": null` instead of `[]` with zero models — [Issue #19381](https://github.com/ollama/ollama/issues/19381), fix [PR #19387](https://github.com/ollama/ollama/pull/19387) (also covers empty embeddings)  
   - Invalid content types silently accepted alongside `tool_calls` — fix [PR #19389](https://github.com/ollama/ollama/pull/19389)

10. **Native `/api/chat` ignores `audio` field** (open)  
    Models advertising `audio` capability (e.g., `lfm3-omni`) accept audio only via `images`; the `audio` field is silently dropped without error.  
    [Issue #19378](https://github.com/ollama/ollama/issues/19378)

---

## What This Means for Application Developers

- **Audio/omni is now cross-platform, but the API is still awkward.** `lfm3-audio`/`lfm3-omni` run on Linux and Windows from v0.40.13, but native audio input via `/api/chat` is ignored ([#19378](https://github.com/ollama/ollama/issues/19378)). Use the `images` field for now and track the issue before building production audio pipelines.

- **OpenAI-compatible clients need defensive parsing.** Until the pending fixes land, expect `"data": null` from `/v1/models` and `/v1/embeddings` when empty, and reasoning fields to be dropped for content-parts arrays. If you use LangChain, LiteLLM, or typed SDKs, pin Ollama and add null-tolerance on list fields. Fixes: [#19387](https://github.com/ollama/ollama/pull/19387), [#19384](https://github.com/ollama/ollama/pull/19384), [#19389](https://github.com/ollama/ollama/pull/19389).

- **Tool-calling reliability is version- and model-specific.** The `go_template` empty-turn bug is closed, but the MLX Qwen3.8 behavior ([#19353](https://github.com/ollama/ollama/issues/19353)) means agents should tolerate missing `tool_calls` and avoid requiring preamble text before tool invocation on MLX builds. Consider the llama.cpp template path where selectable.

- **Pin model digests; there is no rollback.** Users asking how to revert a model update ([#19385](https://github.com/ollama/ollama/issues/19385)) have no first-class mechanism. Bake digest-pinned manifests into deployments rather than relying on `:latest`.

- **Don’t rely on `OLLAMA_NUM_PARALLEL=1` for memory isolation** until [#19262](https://github.com/ollama/ollama/issues/19262) is resolved; enforce serialization at the gateway/queue layer if you need strict sequencing.

- **MLX on Windows/CPU is coming but unreleased.** [#19366](https://github.com/ollama/ollama/pull/19366) would widen MLX model availability considerably, including TTS; plan capacity testing once merged.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-29

## 1. Today's Highlights

LiteLLM shipped three releases in 24h — an RC on the 1.108 line plus patch releases on 1.106 and 1.105 — all accompanied only by cosign image-signature verification notes, so no changelog detail is available from the release bodies. The dominant theme in the issue tracker is **cost accounting**: five separate reports today describe spend logs recording `$0` or phantom spend across streaming, non-streaming, and budget-reset paths. In parallel, a security-relevant fix is queued for the Vertex AI Live websocket passthrough ([#47716](https://github.com/BerriAI/litellm/pull/47716)), and the Rust gateway migration parent ticket ([#31263](https://github.com/BerriAI/litellm/issues/31263)) remains the most-upvoted open item (21 👍, 32 comments).

## 2. Releases & Breaking Changes

**Released (last 24h)** — all images signed with the cosign key introduced in [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065):

| Version | Notes |
|---|---|
| [v1.108.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.108.0-rc.1) | Release candidate for next minor |
| [v1.106.6](https://github.com/BerriAI/litellm/releases/tag/v1.106.6) | Patch on 1.106 line |
| [v1.105.7](https://github.com/BerriAI/litellm/releases/tag/v1.105.7) | Patch on 1.105 line |

**Breaking changes in flight (open PRs, `!` = breaking — plan migration windows):**

- [#47762](https://github.com/BerriAI/litellm/pull/47762) — `uv.lock` and official Docker image move from OpenAI SDK 2.33.0 → **SDK 3** (`openai<4` already allowed since #44927; langchain-openai 1.1.14 and semantic-router were holding the lock below 3).
- [#47716](https://github.com/BerriAI/litellm/pull/47716) — `/vertex_ai/live` now enforces model-level key access; sockets are closed with `1008` on violation.
- [#49058](https://github.com/BerriAI/litellm/pull/49058) — `litellm_settings.drop_params: false` is now honored for Claude Code and Codex traffic; **requests carrying unsupported params will start returning 400** instead of being silently stripped (regression introduced in v1.103.0).
- [#34889](https://github.com/BerriAI/litellm/pull/34889) — GitHub Copilot OAuth tokens sent directly to Copilot; ghe.com shared sign-in must set `GITHUB_COPILOT_API_BASE` before upgrade, and a caller-supplied `api_base` returns 400 without `drop_params`.
- [#48929](https://github.com/BerriAI/litellm/pull/48929) — `hosted_vllm/` forwards `additionalProperties` and `strict: true` in tool schemas as written; vLLM will now enforce schemas that were previously silently dropped.

## 3. New Model & Hardware Support

- **vLLM model discovery** — [PR #40066](https://github.com/BerriAI/litellm/pull/40066): `hosted_vllm/*` wildcard deployments with `check_provider_endpoint: true` now list the server's models on `/v1/models` even without an API key, so Claude Code et al. populate their model pickers against self-hosted vLLM.
- **Vertex AI batch for Claude** — [PR #45715](https://github.com/BerriAI/litellm/pull/45715): Claude on Vertex now runs through the managed batch route (previously 404'd at create), serializing each line as a native Anthropic Messages row and returning OpenAI-shaped batch rows with usage and cost.
- **DashScope image edits + TTS** — [PR #48998](https://github.com/BerriAI/litellm/pull/48998): `/v1/images/edits` (previously 500) and `/v1/audio/speech` (previously 404) for Qwen image and TTS models.
- **New provider: CLF AI Gateway** — [PR #39324](https://github.com/BerriAI/litellm/pull/39324): OpenAI-compatible gateway serving GLM, Kimi, DeepSeek, and Qwen open-weight models on prepaid credits; nine models ship in the pricing map.
- **New provider: eu/jev decision API** — [PR #46875](https://github.com/BerriAI/litellm/pull/46875): EU-hosted decision API with TypeSafe request format, virtual-key auth, RPM limits, and spend booked from token usage.
- **Azure AI Foundry Agents v2** (Responses API with `agent_reference`) — [Issue #25372](https://github.com/BerriAI/litellm/issues/25372) closed.

## 4. Performance & Optimization

- **Rust gateway migration** — [Issue #31263](https://github.com/BerriAI/litellm/issues/31263): parent ticket for the "fastest and litest AI Gateway" targeting **sub-1ms overhead**. Beta tester signup is open via the linked form; this is the single highest-signal roadmap item in the tracker.
- **OTPM reservation blowup fixed** — [PR #49054](https://github.com/BerriAI/litellm/pull/49054): when a deployment set `otpm` and a request omitted `max_tokens`, the router reserved the model's full max output. A Claude deployment with 128k max output and 50k OTPM returned **429 for every such request even on an empty bucket**. Default reservation is now capped, with token counters reconciled post-call.
- **Worker startup crash-loop** — [PR #45039](https://github.com/BerriAI/litellm/pull/45039): on CPU-limited hosts, multi-worker proxies crash-looped on `"Child process died"` because each worker loaded all of LiteLLM before answering uvicorn's **5s** first health ping. The `litellm` command now defers heavy imports until after the ping is served.
- **Azure SDK client reuse ignored timeouts** — [PR #44204](https://github.com/BerriAI/litellm/pull/44204): shared-credential Azure calls reused one cached SDK client regardless of per-call timeout/retry settings, so a single short-timeout batch poisoned all later batches on that resource; combined SDK + LiteLLM retries could produce **16 upstream attempts** for one failing call.
- **Cached stream fidelity/correctness** — [PR #32096](https://github.com/BerriAI/litellm/pull/32096): streamed cache hits replay in live-stream order, preserving reasoning, citations, and audio instead of dropping them and emitting tool calls before text.
- **Prompt-cache dialect resolution** — [PR #45295](https://github.com/BerriAI/litellm/pull/45295): `azure_ai/` deployments now derive cache format from the deployment's `api_base` rather than the proxy-wide `AZURE_AI_API_BASE`, restoring OpenAI-format caching for Foundry deployments.
- **Web-search-aware routing** — [PR #49005](https://github.com/BerriAI/litellm/pull/49005): router now prefers `supports_web_search: true` deployments for Anthropic/Gemini/dated-OpenAI search tools and `web_search_options`, avoiding wasted provider calls and fallback latency.

## 5. Stability & Regressions

**Severity 1 — spend/budget correctness (systemic cluster):**

- [#48937](https://github.com/BerriAI/litellm/issues/48937) **OPEN** — Streamed `/v1/chat/completions` logged at `$0` when upstream reports `usage.cost: 0` alongside real cost elsewhere. Reported as affecting *all streaming traffic*: `spend = 0` and `cost_breakdown.input_cost = 0.0` in `LiteLLM_SpendLogs`. **No fix PR yet.**
- [#47533](https://github.com/BerriAI/litellm/issues/47533) **CLOSED** — Provider-reported `usage.cost` ignored on *non-streaming* calls for OpenAI-compatible upstreams (`custom_llm_provider: openai`); `response_cost = 0` despite upstream reporting cost.
- [#47675](https://github.com/BerriAI/litellm/issues/47675) **OPEN** — `response_cost` null and spend `$0` when `custom_llm_provider=anthropic` but the priced row lives under a different provider in `model_prices_and_context_window.json`.
- [#35829](https://github.com/BerriAI/litellm/issues/35829) **CLOSED** — Provider `usage.cost` trusted verbatim as USD with no unit validation → astronomical phantom spend from non-USD-denominated values.
- [#34805](https://github.com/BerriAI/litellm/issues/34805) **OPEN** — In-memory spend buffers (`update_spend`, `update_daily_tag_spend`) silently dropped on proxy shutdown; apscheduler jobs don't flush.
- [#37261](https://github.com/BerriAI/litellm/issues/37261) **OPEN** — Without Redis, `provider_budget_config` reports `budget_reset_at` **~57 years in the future**, so monthly budgets never reset.

**Severity 2 — security / access control:**

- [#47716](https://github.com/BerriAI/litellm/pull/47716) **OPEN, fix ready** — Keys scoped to specific models could open *any* Vertex AI Live model on `/vertex_ai/live`, since the model is only named in the first websocket message after auth. Fix validates against caller access and closes with `1008`. Internal copy of [#40078](https://github.com/BerriAI/litellm/pull/40078).
- [#28906](https://github.com/BerriAI/litellm/issues/28906) **CLOSED** — Editing a reusable credential could overwrite real secrets with masked placeholders (`sk-p****cdef`), because `/credentials` returns masked values that pre-populate the edit modal.

**Severity 3 — correctness / protocol translation:**

- [#48773](https://github.com/BerriAI/litellm/issues/48773) **CLOSED** — Responses bridge duplicates tool-call arguments in streamed `/v1/chat/completions` (`azure/responses`): full arguments JSON in the first delta, then re-sent fragment-by-fragment.
- [#19858](https://github.com/BerriAI/litellm/issues/19858) **OPEN since Jan** — Claude tool calling broken on OpenRouter and Vertex Sonnet 4.5 when used via Kilo Code / Roo Code. Long-lived, 6 comments.
- [#41963](https://github.com/BerriAI/litellm/issues/41963) **OPEN** — `/v1/responses` does not normalize a plain string `input` into a list before provider dispatch; strict providers reject it.
- [#46095](https://github.com/BerriAI/litellm/issues/46095) **OPEN** — OpenRouter streaming chunks report the *requested* router name (`openrouter/auto`) rather than the model actually served (`deepseek/deepseek-v4.1-flash`).
- [#45546](https://github.com/BerriAI/litellm/issues/45546) **CLOSED** — Mistral `_strip_output_only_fields` deletes `reasoning_content`/`thinking_blocks` from assistant messages instead of replaying them as `thinking` chunks, contrary to Mistral docs.
- [#47814](https://github.com/BerriAI/litellm/issues/47814) **CLOSED** — TPM reservation serializes Responses input with `str(item)`, counting base64 image payloads as text and roughly **doubling** the token estimate.
- [#48449](https://github.com/BerriAI/litellm/issues/48449) **CLOSED** — Decisions transformations materialize omitted probabilities as `0.0`, conflating "missing" with "reported zero".
- [#42111](https://github.com/BerriAI/litellm/issues/42111) **OPEN** — `/v1/audio/speech` 500s for `openrouter/` TTS slugs: *"Unable to map the custom llm provider=openrouter to a known provider."*
- [#24498](https://github.com/BerriAI/litellm/issues/24498) **CLOSED** — Claude models intermittently return `[System: Empty message content sanitised to satisfy protocol]`.

**Severity 4 — operability:**

- [#33167](https://github.com/BerriAI/litellm/issues/33167) **OPEN, 8 👍, stale** — Since v1.92.0, LiteLLM downloads Prisma binaries at startup and **fails in air-gapped / egress-restricted networks**. High-impact for regulated deployments; no fix PR linked.
- [#30126](https://github.com/BerriAI/litellm/issues/30126) **OPEN, stale** — `/cursor/chat/completions` returns 200 and the provider is billed, but **no SpendLogs or Logs UI entry** is created (v1.88.1).
- [#28994](https://github.com/BerriAI/litellm/issues/28994) **OPEN** — No `os.environ/`-equivalent for reading secrets from files (Docker secrets, K8s-mounted volumes). 3 👍.

## 6. What This Means for Application Developers

- **Do not trust spend logs for streaming traffic right now.** [#48937](https://github.com/BerriAI/litellm/issues/48937) plus the closed [#47533](https://github.com/BerriAI/litellm/issues/47533) and [#35829](https://github.com/BerriAI/litellm/issues/35829) describe a single failure mode: upstream-reported cost being dropped or mis-trusted, with `$0` propagating into key/team/end-user budgets. If you gate usage on `max_budget` or `model_max_budget`, reconcile against provider invoices before trusting enforcement, and prefer non-streaming or client-side token accounting until a fix lands. Note also that per-model budgets for end-users were already reported broken ([#31842](https://github.com/BerriAI/litellm/issues/31842)).
- **Budget resets can silently never fire** without Redis ([#37261](https://github.com/BerriAI/litellm/issues/37261)), and **in-flight spend is lost on restart** ([#34805](https://github.com/BerriAI/litellm/issues/34805)). For anything billing-adjacent, run Redis and drain the proxy gracefully.
- **Pin to a patch release and read the `!` PRs before bumping.** Three releases in 24h means the 1.105/1.106/1.108 lines are all moving. The OpenAI SDK 3 bump ([#47762](https://github.com/BerriAI/litellm/pull/47762)) is the one that will surface in transitive dependency conflicts (langchain-openai, semantic-router).
- **Opt-in provider features keep landing without breaking your config** — DashScope image edits/TTS, Vertex batch for Claude, hosted_vllm model listing, and the CLF/eu-jev providers are all additive. If you run self-hosted vLLM behind the proxy, [#40066](https://github.com/BerriAI/litellm/pull/40066) is worth adopting so coding agents can enumerate your models without a dummy key.
- **Air-gapped deployments should stay below v1.92.0's Prisma behavior or pre-bake binaries.** [#33167](https://github.com/BerriAI/litellm/issues/33167) has been open since July with 8 👍 and no linked fix; startup-time network egress is a hard blocker in restricted environments.
- **Watch the Rust gateway.** [#31263](https://github.com/BerriAI/litellm/issues/31263) targets sub-1ms gateway overhead. If you are architecting for proxy latency budgets today, treat the Python gateway's overhead as a known ceiling and size your beta participation accordingly.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-29

Source: [github.com/unslothai/unsloth](https://github.com/unslothai/unsloth)

---

## 1. Today's Highlights

Unsloth shipped **v0.1.912-beta** ("Trick or Treat + Optimizations"), bundling faster first-launch, faster Linux tool calls, faster model loading, and batched training of compatible fine-tuning jobs alongside the Halloween UI theming. A large share of today's PR traffic is `danielhanchen`'s performance series targeting Studio startup, RAG/compaction scans, and an opt-in whole-step CUDA graph for batch-1 decode. On the stability side, the most serious items are a **silent Alpaca dataset truncation** in "Train on completions" masking ([#15894](https://github.com/unslothai/unsloth/issues/15894) / [#15895](https://github.com/unslothai/unsloth/issues/15895)) and a **post-update model load regression** on macOS 15.0.1 ([#15932](https://github.com/unslothai/unsloth/issues/15932)).

---

## 2. Releases & Breaking Changes

- **[v0.1.912-beta — "Trick or Treat + Optimizations"](https://github.com/unslothai/unsloth/releases)** — October seasonal UI (Halloween theme row, spooky greetings, spiders/bats), plus first-launch speedups, Linux tool-call speedups, and model-loading improvements. Also enables training of compatible fine-tuning jobs together ("multi-L…" — release note truncated in source; appears to be multi-LoRA / multi-job batching). No documented API or config breaking changes.
- **Upgrade caution:** [#15932](https://github.com/unslothai/unsloth/issues/15932) reports a previously working model failing to load *after* updating to 0.1.912-beta on macOS 15.0.1; [#15866](https://github.com/unslothai/unsloth/issues/15866) reports the update itself failing. No fix PR yet — advise pinning 0.1.911-beta for affected macOS/MLX users until triaged.

---

## 3. New Model & Hardware Support

- **[#15588 — Support LFM3-1.7B](https://github.com/unslothai/unsloth/issues/15588)** (OPEN, 10 👍) — request to add the LiquidAI LFM3 family; highest-demand model request in today's set.
- **[#15832 — LFM3 MLX models not listed by `curl v1/models`](https://github.com/unslothai/unsloth/issues/15832)** — partial LFM3 support exists on the MLX path but models are omitted from the OpenAI-compatible model listing, breaking discovery by external clients.
- **[#15954 — [MLX] Neural Engine Prefill toggle](https://github.com/unslothai/unsloth/pull/15954)** — opt-in ANE/GPU split of dense SwiGLU MLP prefill for MLX models, wired alongside the existing Int8 Prefill switch.
- **[#15831 — Python 3.15 / ROCm 10.0](https://github.com/unslothai/unsloth/issues/15831)** — request for ROCm 10.0 wheels now that PyTorch 2.15.0 ships them; no commitment yet.
- **[#14457 — make `unsloth doctor` MLX-aware on Apple Silicon](https://github.com/unslothai/unsloth/pull/14457)** — CLI diagnostics currently probe only CUDA/ROCm and report a false failure on MLX-based Macs.
- Model coverage touched today in bugs/fixes: MiniCPM-V-4.7-1B (Vulkan, [#14987](https://github.com/unslothai/unsloth/issues/14987), closed), Qwen-Image-2.1-Turbo (GGUF/Safetensors, [#15735](https://github.com/unslothai/unsloth/issues/15735), closed), Wan2.1-T2V-1.3B and FLUX.2-klein-9B (low-memory first-load fixes, see §4), MiniMax-H3 (fused QKV, see §4), Dolphin Venice Edition via MLX ([#15858](https://github.com/unslothai/unsloth/issues/15858)).

---

## 4. Performance & Optimization

**Decode / kernels**
- **[#15976 — Whole-step CUDA graph for batch-1 decode](https://github.com/unslothai/unsloth/pull/15976)** — opt-in via `UNSLOTH_DECODE_CUDA_GRAPHS_FULL=1`; captures attention cores inside the graph instead of segmenting around them. Default behavior unchanged.
- **[#15971 — bitsandbytes 4-bit GEMM via unsloth_zoo direct call](https://github.com/unslothai/unsloth/pull/15971)** — bypasses ~50 µs of bitsandbytes Python dispatch per `matmul_4bit` call in `fast_linear_forward`; affects the T4 fp16 batch 5–16 default path and `UNSLOTH_BNB_GEMM_4BIT=1`.

**Studio startup & UI**
- **[#15943 — Load chat page on first use](https://github.com/unslothai/unsloth/pull/15943)** — removes the statically imported `ChatPage` (thread, markdown, reasoning, tool rendering) from the startup bundle for every first screen.
- **[#15947 — Skip OXC validator `npm ci` when tree unchanged](https://github.com/unslothai/unsloth/pull/15947)** — eliminates a redundant install on every setup/update run.

**RAG / indexing**
- **[#15949 — Match linked-folder paths against `.gitignore` runs](https://github.com/unslothai/unsloth/pull/15949)** — replaces per-rule `re.match` with run matching on the 30 s rescan; a 145-line `.gitignore` previously cost ~90 regex calls per file/dir.
- **[#15950 — Literal-led blank-line collapsing regex](https://github.com/unslothai/unsloth/pull/15950)** — `re.sub(r"\n{3,}")` had no literal prefix, forcing a match attempt at every character offset (~10 ns/char) during unstructured seed chunking.
- **[#15972 — Reuse lexical passes across compaction/prune runs](https://github.com/unslothai/unsloth/pull/15972)** — profiling shows query encode + KNN is only 1–4% of compaction recall at 20k archives; the cost is in keyword search and branch filtering.
- **[#15942 — Share one edge index for Recipe Studio connection status](https://github.com/unslothai/unsloth/pull/15942)** — stops per-node subscription to the full edge array and config map.

**Low-memory / OOM fixes**
- **[#15975 — Stage fp32 video encoder on device when dense cast won't fit host RAM](https://github.com/unslothai/unsloth/pull/15975)** — Wan2.1-T2V-1.3B first load OOM-killed on a 12.7 GB T4 host (22.7 GB fp32 UMT5 shards → 13.x GB fp16 cast).
- **[#15974 — Download weights before the small-host placement decision](https://github.com/unslothai/unsloth/pull/15974)** — cold-cache FLUX.2-klein-9B load OOM-killed on 12.7 GB host; warm-cache second load renders in ~60 s.
- **[#15956 — MiniMax-H3 unfused Q/K/V share one ConvRot rotation and act quant](https://github.com/unslothai/unsloth/pull/15956)** (CLOSED) — fusion to a single int8 `to_qkv` only applies when the denoiser is pinned resident; every card under ~57 GB (48 GB Ada, A100-40, L4, 4090/5090) takes streamed placement.

---

## 5. Stability & Regressions

Ranked by severity.

1. **Silent dataset truncation in "Train on completions" (HIGH).** [#15894](https://github.com/unslothai/unsloth/issues/15894) (CLOSED) / [#15895](https://github.com/unslothai/unsloth/issues/15895) (OPEN) — 80%+ of a 272-row Alpaca JSONL is dropped on any instruct model: the Alpaca template is never installed, but masking still searches for its markers. Reproduced on two unrelated model families, Windows 11, RTX 5060 Laptop, v0.1.911-beta and main. Silent data loss during training is the worst failure mode here — audit completion-only datasets.
2. **Model fails to load after 0.1.912-beta update (HIGH).** [#15932](https://github.com/unslothai/unsloth/issues/15932) — macOS 15.0.1, model fine for weeks before the update. No fix PR.
3. **MLX memory estimator wildly overestimates (HIGH).** [#15935](https://github.com/unslothai/unsloth/issues/15935) — reports 69.7 GiB for a model that loads fine on a 24 GB Mac; likely blocks otherwise-valid loads/placement decisions.
4. **Single-column datasets unusable (MEDIUM-HIGH).** [#15892](https://github.com/unslothai/unsloth/issues/15892) — `{"text": "..."}` JSONL reports `Format: unknown`, AI Assist declines, Start silently returns to preview. [#15901](https://github.com/unslothai/unsloth/issues/15901) — AI Assist cannot express "no column mapping needed," so it always fails on single-column data and discards the reason.
5. **Update path failure (MEDIUM).** [#15866](https://github.com/unslothai/unsloth/issues/15866) — Unsloth fails to update from the desktop app.
6. **MLX model rendering / discovery (MEDIUM).** [#15858](https://github.com/unslothai/unsloth/issues/15858) Dolphin Venice Edition renders incorrectly in-UI while working in other apps; [#15832](https://github.com/unslothai/unsloth/issues/15832) LFM3 MLX models missing from `/v1/models`.
7. **UI defects (LOW).** [#15856](https://github.com/unslothai/unsloth/issues/15856) image drag-reorder rejects its own drop on Fedora 44; [#15782](https://github.com/unslothai/unsloth/issues/15782) chat textarea content duplicated across tabs; [#15735](https://github.com/unslothai/unsloth/issues/15735) (CLOSED) Qwen-Image Safetensors missing from "On Device"; [#14987](https://github.com/unslothai/unsloth/issues/14987) (CLOSED) MiniCPM-V-4.7-1B Vulkan generation failure.

---

## 6. What This Means for Application Developers

- **Audit fine-tuning data before trusting completions-only runs.** [#15894](https://github.com/unslothai/unsloth/issues/15894) is a *silent* correctness bug — loss curves will look plausible while 80% of your Alpaca rows are masked out. Validate row counts in the trained artifact, not just the preview pane, and prefer explicit chat templates over inferred Alpaca formatting until this is fixed.
- **Pin your version for MLX/Apple deployments.** 0.1.912-beta has an open load regression on macOS plus a memory estimator that can over-report by ~3x on unified memory. Pin 0.1.911-beta if you depend on Mac inference/training.
- **Decode CUDA graphs are now testable at whole-step granularity.** `UNSLOTH_DECODE_CUDA_GRAPHS_FULL=1` ([#15976](https://github.com/unslothai/unsloth/pull/15976)) is opt-in and default-off — safe to benchmark for single-stream batch-1 serving without changing production behavior.
- **Low-VRAM serving of video/diffusion models is actively improving.** The streamed-placement behavior in [#15956](https://github.com/unslothai/unsloth/pull/15956) and the cold-cache OOM fixes in [#15974](https://github.com/unslothai/unsloth/pull/15974)/[#15975](https://github.com/unslothai/unsloth/pull/15975) mean sub-24 GB cards will increasingly work *only if* the model is already cached — plan for a warm-up load on constrained hosts.
- **Startup latency is dropping.** Lazy-loading the chat page ([#15943](https://github.com/unslothai/unsloth/pull/15943)) and skipping redundant installs ([#15947](https://github.com/unslothai/unsloth/pull/15947)) reduce time-to-first-request for embedded Studio deployments; relevant if you ship Unsloth Studio as a local sidecar.
- **Gateway/discovery gaps remain.** If you front Unsloth with an OpenAI-compatible router, `/v1/models` is not yet reliable for MLX-served models ([#15832](https://github.com/unslothai/unsloth/issues/15832)); maintain an explicit model allowlist rather than relying on runtime discovery (see also upstream [opencode#59557](https://github.com/anomalyco/opencode/issues/59557) tracked in [#15521](https://github.com/unslothai/unsloth/issues/15521)).
- **Worth watching:** an Evals tab driving `lm-evaluation-harness` against GGUFs through Studio's own llama-server ([#12521](https://github.com/unslothai/unsloth/pull/12521)), Flows benchmark/loop nodes that auto-score fine-tunes on held-out rows ([#15941](https://github.com/unslothai/unsloth/pull/15941)), neuron activation capture for LoRA runs ([#5217](https://github.com/unslothai/unsloth/pull/5217)), and a rebuilt per-GPU Live monitor ([#15630](https://github.com/unslothai/unsloth/pull/15630)).

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*