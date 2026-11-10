# AI 基础设施日报 2026-11-10

> 生成时间: 2026-11-10 04:17 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

### **跨项目AI基础设施生态报告 – 2026-11-10**

---

#### **1. 生态概览**  
2026年末的AI基础设施格局呈现出明显的两极分化：一边是**高性能推理引擎**（vLLM、SGLang），另一边是**开发者友好型网关/本地运行时**（Ollama、llama.cpp、LiteLLM），而Unsloth则作为融合本地执行与代理工作流的混合工具层崭露头角。NVIDIA Blackwell SM120 GPU的推出加剧了对硬件特性的深度调优，尤其集中在MoE模型、FlashInfer工作区优化以及CUDA图稳定性方面。与此同时，多模型支持、结构化输出和代理能力正推动整个栈在API兼容性和部署灵活性上的趋同。

---

#### **2. 活动对比**

| 项目       | 开放问题数 | 近24小时合并的PR | 近24小时发布数 | 破坏性变更 |
|---------------|-------------|-------------------------|------------------------|------------------|
| **vLLM**      | 187         | 9                       | 无                   | 无             |
| **SGLang**    | 234         | 5                       | 无                   | 是（v0.5.23）    |
| **llama.cpp** | 321         | 12                      | 10（b12233–b12242）     | 是（API修复）    |
| **Ollama**    | 198         | 4                       | 1（v0.40.16）           | 是（VRAM修复）   |
| **LiteLLM**   | 267         | 6                       | 1（v1.108.4 LTS）       | 是（速率限制器） |
| **Unsloth**   | 172         | 6                       | 无                   | 无             |

> *注：高问题量反映了对下一代硬件（SM120、gfx1151）的积极调试。SGLang与llama.cpp在PR提交速度上表现最强劲。*

---

#### **3. 模型支持竞赛**  
| 新模型 / 架构 | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1-Flash**  | ✅（SM120问题） | ⚠️（严重回归） | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.8-Flash-Next**   | ✅（gfx950/MI355X） | ✅（ROCm修复） | ✅（WebGPU/Metal） | ✅（内存优化） | ✅（成本映射） | ✅（FLUX.3） |
| **NVFP4 MoE（Blackwell）**| ⚠️（关键缺陷） | ⚠️（FA4崩溃） | ❌ | ❌ | ❌ | ✅（Blackwell前回退） |
| **Gemma 4（MTMD音频）** | ❌ | ❌ | ✅（实时流） | ❌ | ❌ | ❌ |
| **Ripple Preview（65B）** | ❌ | ❌ | ❌ | ✅（新模型） | ❌ | ❌ |
| **FLUX.3**               | ❌ | ❌ | ❌ | ❌ | ❌ | ✅（步进注意力） |

> **领先者**：*Ollama* 在新模型可用性上领先（Ripple Preview），而 *llama.cpp* 在边缘设备支持上占优（Metal、WebGPU、SpacemiT）。*Unsloth* 在Blackwell前及ROCm模型韧性方面初显优势。

---

#### **4. 性能前沿**  
优化工作日益集中于五个关键领域：
- **KV缓存与内存管理**：vLLM将FlashInfer工作区减少92%（394 → 32 MiB），提升容量；SGLang修复`UnifiedRadixCache`使用后释放漏洞。
- **量化与内核效率**：vLLM推动Humming后端取代Marlin；llama.cpp在SpacemiT上新增MXFP4支持；SGLang优化DSpark以适配Hopper。
- **批处理与并行**：Ollama启用`OLLAMA_NUM_PARALLEL`用于MLX；LiteLLM通过选择对象缓存改进流式复用。
- **分布式服务**：SGLang与vLLM在多GPU协调（ROCm/gfx950、SM120）方面领先，但均面临稳定性回归问题。
- **内核级调优**：所有项目均聚焦于窄矩阵操作（Vulkan、Intel GPU）、注意力内核（FA4 vs Triton）及推测解码效率。

> **核心焦点**：*vLLM* 与 *SGLang* 在SM120与ROCm的内核级优化上最为先进，但正确性权衡仍存在。

---

#### **5. 层级定位**  

| 项目       | 主要层级                     | 角色摘要 |
|---------------|-----------------------------------|--------------|
| **vLLM**      | **推理引擎**              | 高吞吐、低延迟服务；主导云规模大模型部署。 |
| **SGLang**    | **推理引擎 + 代理运行时** | 结合引擎功能与代理编排；在推测解码与多GPU负载中表现强劲。 |
| **llama.cpp** | **本地运行时 / 边缘推理** | 针对CPU/GPU/边缘设备优化；适用于离线、隐私敏感或异构环境。 |
| **Ollama**    | **开发者网关 / 本地运行时** | 用户优先界面，集成模型库与CLI；连接开发体验与本地推理。 |
| **LiteLLM**   | **LLM网关 / 编排层** | 多提供方路由、成本追踪、速率限制——生产级API栈的关键组件。 |
| **Unsloth**   | **代理工具与用户体验层**       | 聚焦图像生成、模型管理与界面美化；定位为“本地代理工作室”。 |

> **战略差异化**：LiteLLM与Ollama作为抽象层；vLLM/SGLang为性能关键引擎；llama.cpp与Unsloth瞄准边缘/本地自主。

---

#### **6. 趋势信号**  
当前活动揭示出若干关键行业趋势：
- **硬件特异性碎片化**：SM120（Blackwell）与gfx1151（AMD Strix Halo）正成为独立的性能域——无单一项目能在两者上完全稳定。开发者必须尽早验证目标硬件。
- **量化战争**：vLLM从Marlin转向Humming，标志着向统一、现代后端的演进，支持跨架构的MXFP4/FP8。
- **以代理为中心的优化**：Ripple Preview、DeepSeek-V4推测解码、sd.cpp图像生成凸显对端到端代理流水线的需求增长。
- **成本与合规可见性**：LiteLLM的按客户设置的RPM/TPM限额与精准成本追踪，反映出多租户系统中财务治理需求的上升。
- **边缘安全**：SGLang #49361中的使用后释放漏洞与Ollama #19631的VRAM计数错误，表明可靠性已成为合规关注点。

> **面向运维与工程师的可操作建议**：
> - 在vLLM解决#56892前，避免在SM120上使用`--enforce-eager`。
> - 若在Hopper上使用推测解码，请勿升级SGLang至`v0.5.23`。
> - 生产前请在llama.cpp或Ollama上验证音频、视觉与结构化输出流水线。
> - 使用`litellm v1.108.4`需谨慎：升级后务必重新检查所有`model_rpm_limit`设置。
> - 监控Unsloth的`sd.cpp`重试逻辑，确保在资源受限环境下的鲁棒性。

---

*由AI系统分析团队编制 | 2026-11-10*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

### **vLLM Digest — 2026-11-10**

---

#### **1. 今日重点**  
vLLM 项目正集中精力推进 **Blackwell SM120 支持**，报告指出在 RTX PRO 6000 Blackwell GPU 上，DeepSeek-V4.1-Flash 与 NVFP4 MoE 模型存在关键性能与正确性问题。一项重要 RFC (#65560) 提议在所有 NVIDIA 架构上用更现代、架构无关的 **Humming 量化后端** 替代 Marlin。与此同时，ROCm/AMD 方向也取得进展，针对 Qwen3.8-Flash-Next 及 gfx950/MI355X 平台的优化工作正在推进。

---

#### **2. 发布与破坏性变更**  
*无*。过去 24 小时内未发布新版本。未宣布任何破坏性变更或迁移说明。

---

#### **3. 新模型与硬件支持**  
- **SM120（RTX PRO 6000 Blackwell）**：`DeepSeek-V4.1-Flash` 与 `NVFP4 MoE` 模型存在严重支持缺口；启用 `--enforce-eager` 时 CUDA Graph 不可用，解码吞吐量大幅下降。  
- **ROCm/gfx950 / MI355X**：针对 `Qwen3.8-Flash-Next-Quark-MXFP4` 与 `Qwen3.8-2.4T-A95B-Quark-MXFP4` 的优化工作正在进行中 ([Issue #57149](https://github.com/vllm-project/vllm/issues/57149), [#59575](https://github.com/vllm-project/vllm/issues/59575))。  
- **NVIDIA Nemotron 3.5 ASR 流式传输**：已提交功能请求以支持 `nemotron-3.5-asr-streaming-0.6b` ([Issue #47455](https://github.com/vllm-project/vllm/issues/47455))。

---

#### **4. 性能与优化**  
- **SM120 FlashInfer 工作区大小调整**：将工作区从 394 MiB 降低至 32 MiB，在相同内存预算下使 KV 缓存容量提升 **21.27%** ([Issue #63399](https://github.com/vllm-project/vllm/issues/63399))。  
- **推测解码效率提升**：PR #66534 引入在目标 logits 加载阶段提前应用 top-k/p 截断——可能减少验证开销。  
- **量化后端迁移**：RFC #65560 主张用支持新格式（MXFP4、MXFP8、FP8）及所有 NVIDIA 架构（包括 Hopper 与 Blackwell）的 **Humming** 替代旧版 **Marlin**。  
- **MoE 内核统一**：RFC #37753 倡议通过基于类的设计统一 MoE “oracle”，以提升可维护性与可扩展性。

---

#### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|---------|-------|-------------|------------|
| ⚠️ 高 | [Issue #56892](https://github.com/vllm-project/vllm/issues/56892) | 在 SM120 上使用 `--enforce-eager` 时解码吞吐量急剧下降；CUDA Graph 不可用 | ❌ 尚无修复 |
| ⚠️ 高 | [Issue #51525](https://github.com/vllm-project/vllm/issues/51525) | 显式设置 `VLLM_CUTLASS` 选择 NVFP4 MoE 时，在 SM120 上静默返回空结果 | ❌ 尚无修复 |
| ⚠️ 高 | [Issue #66141](https://github.com/vllm-project/vllm/issues/66141) | ROCm `aiter` MLA 内核对 `gqa_ratio=16`、`decode_qlen=3–4` 返回非相关输出 | ✅ PR #66710 中部分修复 |
| ⚠️ 中 | [Issue #53670](https://github.com/vllm-project/vllm/issues/53670) | EAGLE/MTP 前缀缓存最后块丢失导致每次命中重计算约 1,648 个 token（批量吞吐损失约 30–40%） | ❌ 正在处理 |
| ⚠️ 中 | [Issue #66070](https://github.com/vllm-project/vllm/issues/66070) | Qwen3.5 视觉 RoPE 角度精度与 HF 不一致（可复现） | ❌ 尚无修复 |

---

#### **6. 对应用开发者的影响**  
- **在 SM120 GPU 上避免使用 `--enforce-eager`**，直到 #56892 修复完成——否则将面临严重性能下降。仅在必要时使用急进模式，并注意监控静默失败。  
- **尽可能使用 Humming 而非 Marlin**：预期性能更优，硬件覆盖更广，尤其在 Hopper/Blackwell 平台上。请关注 #65560 的发布进度。  
- **使用推测解码或 MoE 模型时，务必严格验证输出**——MTP 与 NVFP4 路径均存在已知正确性问题。  
- **开启指标追踪**（`--enable-mfu-metrics`），以监控权重布局效率，特别是在使用自定义量化配置时 ([PR #65143](https://github.com/vllm-project/vllm/pull/65143))。  
- **对于 AMD 部署**，确保 CI/CD 流水线在 gfx950/MI355X 硬件上进行测试——性能提升正在积极推进，但稳定性仍是重点。

--- 

*数据来源：github.com/vllm-project/vllm | 更新时间：2026-11-10*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

### **SGLang Digest — 2026-11-10**

#### **1. 今日亮点**  
SGLang 项目持续深化对下一代硬件的支持，针对 SM120（Blackwell）和 AMD gfx1151 设备完成了关键稳定性修复，包括注意力后端崩溃及使用已释放内存（use-after-free）漏洞。性能优化方面，通过 DSpark 实现了 Qwen3.8-27B 在 Hopper 架构上的优化，并修复了 DeepSeek-V4 在 SM120 上预填充吞吐量的重大回归问题。社区正积极应对跨平台的不稳定的 CI 基础设施与测试瓶颈。

#### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本。*  
然而，**v0.5.23** 被发现引入了在并发度为 12 的 Hopper（SM90）上使用 `use_deep_select_decode` 时的 **33–45% 解码延迟回归**，影响依赖 DeepSeek-V4.1-Flash 模型进行推测解码的用户——建议升级前等待补丁修复 ([#49336](https://github.com/sgl-project/sglang/issues/49336))。

#### **3. 新模型与硬件支持**  
- **AMD gfx1151（Strix Halo）**：`decode_attention_fwd(..., has_mla=True)` 及确定性推理（`--enable-deterministic-inference`）的关键错误修复正在审查中 ([#48967](https://github.com/sgl-project/sglang/issues/48967), [#48968](https://github.com/sgl-project/sglang/issues/48968))。  
- **Cambricon MLU 后端**：一项路线图问题追踪初始集成工作 ([#45615](https://github.com/sgl-project/sglang/issues/45615))，目前尚未合并任何 PR。  
- **Kimi-K3.1-Pro MI35x**：文档已更新，以对齐 InferenceX 基准测试中的推理方案 ([#49453](https://github.com/sgl-project/sglang/pull/49453))。  
- **SM120（RTX 50 系列 Blackwell）**：多个问题指出在模型加载和 CUDA 图捕获阶段出现运行时失败，尤其在使用 GLM-5.3-Flash 与 DeepSeek-V4 时 ([#42012](https://github.com/sgl-project/sglang/issues/42012), [#49209](https://github.com/sgl-project/sglang/issues/49209))。

#### **4. 性能与优化**  
- **DeepSeek-V4 预填充吞吐量**：报告显示在 4× RTX PRO 6000（SM120）上约为 **~2–7K tok/s**，而 vLLM/Marlin 可达 **~12.5K tok/s**，表明存在显著的调优或内核优化空间 ([#33422](https://github.com/sgl-project/sglang/issues/33422))。  
- **Hopper 上的 Qwen3.8-27B**：v0.5.23 版本在并发度 12、上下文长度 8K–64K 范围内导致每步解码时间增加 **33–45%**，影响推测解码流程 ([#49336](https://github.com/sgl-project/sglang/issues/49336))。  
- **扩散模型优化**：PR 旨在通过将 RoPE 计算卸载至 FlashInfer 内核并启用可中断的 CUDA 图，提升 SANA-Video 2.0 与 Qwen-Image-Edit 性能 ([#41501](https://github.com/sgl-project/sglang/pull/41501), [#49448](https://github.com/sgl-project/sglang/pull/49448))。  
- **AMD GFX950**：针对 MTP 验证行中的融合索引器 logits 与 top-k 选择的优化正在进行中 ([#47276](https://github.com/sgl-project/sglang/pull/47276))。

#### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|--------|------|-------------|------------|
| 🔴 高 | [#49336](https://github.com/sgl-project/sglang/issues/49336) | **v0.5.23 在 Hopper 上使用 DSpark 时解码延迟回归**（+33–45%） | 尚无修复 PR |
| 🔴 高 | [#42012](https://github.com/sgl-project/sglang/issues/42012) | **SM120 上 FA4 注意力后端崩溃**，混合扩展重塑期间；仅 Triton 可用 | 尚无修复 PR |
| 🔴 高 | [#49361](https://github.com/sgl-project/sglang/issues/49361) | **UnifiedRadixCache 中的 use-after-free**，暴露租户 KV 数据——潜在安全风险 | 尚无修复 PR |
| 🟡 中 | [#49133](https://github.com/sgl-project/sglang/issues/49133) | 可中断预填充 CUDA 图生成的 logprob 与急切模式不同 | 尚无修复 PR |
| 🟡 中 | [#48411](https://github.com/sgl-project/sglang/issues/48411) | Xeon SPR 服务器在 KV 分配期间被 OOM 杀死 | 尚无修复 PR |

> *注：不稳定的 CI 基础设施 (#42752) 仍是 PR 验证的阻塞项，影响开发进度。*

#### **6. 对应用开发者的影响**  
- 若在 Hopper 上运行推测解码（DSpark），请**避免使用 v0.5.23**——高并发下解码延迟可能高达 **45% 更高**。请暂时使用 `v0.5.22` 或更早版本，直至修复。  
- **不要在 SM120（Blackwell）上使用 FA4 后端部署如 GLM-5.3-Flash 的模型**——会在混合扩展阶段崩溃。请改用 Triton 直至问题解决。  
- **谨慎使用 `UnifiedRadixCache`**——已确认存在 use-after-free 漏洞，可能导致敏感用户数据泄露。请关注 [#49361](https://github.com/sgl-project/sglang/issues/49361) 获取补丁。  
- 对于**代理类应用**，建议启用可中断 CUDA 图与分页 KV 缓存（通过 PR #49451），以高效扩展长上下文生成能力。  
- **在 AMD 上进行多 GPU 推理时**，需特别注意 `has_mla=True` 与确定性模式设置——这些可能无声失败或产生 NaN 值。请在 gfx1151 上充分验证后再投入生产。

---  
*本摘要基于 GitHub 数据生成：[sgl-project/sglang](https://github.com/sgl-project/sglang)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp Digest – 2026-11-10**

---

### **1. 今日亮点**  
最新更新聚焦于提升 OpenAI 兼容 API 的一致性，特别是在结构化输出和通过 `/v1/audio/speech` 端点生成音频方面。关键修复解决了 `response_format` 中的 JSON Schema 处理问题，确保各模型行为一致。针对 Apple Metal（AMD eGPU）、WebGPU 和 Vulkan 的新内核优化，提升了窄矩阵操作性能，并降低了内存开销。

---

### **2. 发布与破坏性变更**  
- **新版本发布**：过去 24 小时内发布了 `b12242` 至 `b12233` 版本。  
  - [b12242](https://github.com/ggml-org/llama.cpp/releases/tag/b12242)：修复在设置 `response_format` 且使用 tools 时的懒加载语法处理问题 (#31956)。  
  - [b12240](https://github.com/ggml-org/llama.cpp/releases/tag/b12240)：支持将 `text.format.json_schema` 转换为 Responses API 中的补全结果 (#26145)。  
  - [b12238](https://github.com/ggml-org/llama.cpp/releases/tag/b12238)：新增兼容 OpenAI 的 `/v1/audio/speech` 端点，支持 WAV 输出及 PCM 四舍五入对齐 (#31763)。  
  - [b12237](https://github.com/ggml-org/llama.cpp/releases/tag/b12237)：通过 MTMD 实现 Gemma 4 模型的实时音频流处理 (#31965)。  
- **API 稳定性提示**：服务器现在能正确处理 `response_format` 中的顶层 `json_schema`，修复了此前仅支持嵌套结构的预期问题 (#28697)。

---

### **3. 新模型与硬件支持**  
- **模型支持**：  
  - Gemma 4 输入音频流（MTMD）现已支持 (#31965)。  
  - Qwen3-TTS 语音编码器与 PocketTTS 语音合成器现可通过 WebGPU 完全支持（`PAD_REFLECT_1D`、`COL2IM_1D`）(#31991, #31990)。  
- **硬件与后端新增**：  
  - **Apple Metal**：SIMD 宽度感知内核现已支持 AMD eGPU（RX 580、Polaris），可处理 64 宽波前任务 (#31994)。  
  - **Hexagon**：为 Android 与 WoS 设备新增 `SSM_SCAN` 操作支持 (#31921)。  
  - **SpacemiT CPU**：启用 MXFP4 密集重排、IME2 加速、Q4_1 32x256 分派以及通过 RVV 实现的 `GATED_DELTA_NET` 路由 (#31776, #31929, #31897)。  
  - **WebGPU**：扩展操作覆盖范围，新增 `PAD_REFLECT_1D`、`COL2IM_1D`，并改进填充测试 (#31877, #31991)。  
- **量化支持**：现可通过 `--ple` 别名支持外部 PLE 文件加载 (#31185)。

---

### **4. 性能与优化**  
- **内核优化**：  
  - Vulkan：针对 N ≤ 32 的“窄”瓦片配置已调优，显著提升矩阵乘法效率 (#31990)。  
  - Intel GPU：为 Intel 平台新增预填充 FlashAttention 内核，提升预填充吞吐量 (#29357)。  
  - SYCL：通过 `GGML_SYCL_Q4_0_U4_GEMM` 环境变量可绕过 f16 转换开销，实现更快的 Q4_0 权重加载（基准测试显示大矩阵下最高减少 1.2ms）(#31993)。  
- **内存效率**：  
  - `ggml-zdnn`：现支持非 ZDNN 张量，并在每次操作后失效计算张量，降低陈旧状态风险 (#31824)。  
  - `mtmd_helper_resampler`：提升音频重采样精度，降低流式场景下的延迟 (#31964)。

---

### **5. 稳定性与回归问题**  
- **严重问题**：  
  - **KV 缓存截断** (#11970)：高危错误导致 `/v1/chat/completions` 过程中 KV 缓存被错误截断 —— 影响模型正确性；当前开放，已有 50 条评论。  
  - **SYCL MoE Logprobs 非确定性** (#31206)：相同请求在固定种子下产生不同 `logprobs` 值 —— 影响代理类应用的可复现性；已在 iGPU + MoE 上报告。  
- **其他重要缺陷**：  
  - Linux 下因意外图重分配（`ggml_backend_sched_alloc_splits`）导致 GGML 崩溃 (#28753)。  
  - ROCm/GFX1151 在长提示下输出错误 logit (#28211)。  
  - Adreno 830 因 shaderc/NDK SPIR-V 差异导致 Vulkan 管道创建失败（`VK_ERROR_UNKNOWN`）(#28635)。  
- **修复进展**：目前尚无直接解决这些高影响问题的 PR。

---

### **6. 对应用开发者的意义**  
- **结构化输出**：现在可直接在顶层使用 `response_format: json_schema`，无需嵌套包装 —— 适用于代理工具调用。  
- **音频集成**：新 `/v1/audio/speech` 端点支持实时音频流，可构建完整的语音转文本流水线（如语音代理或实时转录）。  
- **跨平台部署**：得益于 Metal（AMD eGPU）、SpacemiT RISC-V 与 WebGPU 改进，可在多种边缘设备上实现低延迟推理。  
- **注意事项**：避免在 Qwen3.5-MoE 模型上使用 `--temp 0` 配合推测解码（`draft-dflash/dspark`）—— 输出可能偏离自回归基线 (#31472)。  
- **配置建议**：若在 SYCL 后端使用 Q4_0 量化模型，建议设置 `GGML_SYCL_Q4_0_U4_GEMM=1`，以避免不必要的 F16 转换开销。

> 🔗 详细信息：[GitHub - llama.cpp](https://github.com/ggml-org/llama.cpp)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-10**

---

### **1. 今日亮点**  
最新发布的 **v0.40.16** 版本引入了 *Ripple Preview* — 由 Ricursive Intelligence 开发的 65B 参数稠密模型，专为智能体编码优化，现已可通过 `ollama run ripple-preview` 使用。关键修复解决了 VRAM 计算和 GPU 分割问题，此前这些问题导致模型被错误逐出或在多 GPU 间错误分配。这些改进显著提升了多 GPU 和内存受限系统上的可靠性。

---

### **2. 发布与破坏性变更**  
- **v0.40.16**:  
  - 引入 [Ripple Preview](https://ollama.com/library/ripple-preview)，一款面向自主编码智能体的 65B 参数稠密模型。  
  - 修复影响模型加载与逐出行为的关键 VRAM 预测及 GPU 分割缺陷 ([PR #19632](https://github.com/ollama/ollama/pull/19632), [Issue #19631](https://github.com/ollama/ollama/issues/19631))。  
  - 在 Apple Silicon（M 系列芯片）上将 `Qwen3.8 Flash Next` 的内存效率提升约 27 GiB。  

> 🔗 [GitHub Release v0.40.16](https://github.com/ollama/ollama/releases/tag/v0.40.16)

---

### **3. 新模型与硬件支持**  
- **新模型**:  
  - [`ripple-preview`](https://ollama.com/library/ripple-preview) – 65B 参数稠密模型，专为智能体工作流中的复杂推理与代码生成设计。  
- **硬件与后端**:  
  - 对 **Apple Silicon (M 系列)** 支持进一步优化，显著降低 `Qwen3.8 Flash Next` 的内存占用。  
  - 持续优化 **ROCm** 与 **Vulkan (AMD RADV)** 性能，但仍有部分回归问题报告（如 #19201）。  
  - 注：仅支持 Vulkan 的系统仍会触发不必要的 CUDA 驱动下载 (#19474)；暂无修复方案。

> 🔗 [Ripple Preview 模型页面](https://ollama.com/library/ripple-preview)

---

### **4. 性能与优化**  
- **内存效率**:  
  - 在 Apple Silicon 上，`Qwen3.8 Flash Next` 的 VRAM 使用量减少 **约 27 GiB**，使 M 系列设备可处理更大上下文。  
- **并行性与吞吐量**:  
  - PR #19197 通过 `OLLAMA_NUM_PARALLEL` 实现 MLX 模型的并发生成，改善并行任务的首个令牌延迟。  
  - PR #17144 在上游 llama.cpp 稳定性修复后，解除对 `qwen35` 与 `qwen35moe` 的并行请求硬性限制。  
- **提示词处理**:  
  - 正在优化 AMD GPU 上的提示词缓存性能；启用缓存后，首次请求后的性能下降约 30 倍（#19201 报告）。

> 🔗 [PR #19197 (MLX 并发)](https://github.com/ollama/ollama/pull/19197) | [PR #17144 (Qwen35 并行性)](https://github.com/ollama/ollama/pull/17144)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 状态 | 修复 PR | 备注 |
|---------|------|--------|--------|-------|
| ⚠️ 高 | VRAM 计算与 GPU 分割错误导致模型被逐出或加载失败 | 已修复 | [#19632](https://github.com/ollama/ollama/pull/19632) | 修复多个关联问题（#19463, #18679 等） |
| ⚠️ 高 | `OLLAMA_KEEP_ALIVE=300` 对长时间用户会话过短 | 开放 | — | 用户报告在反思阶段出现断连（Issue #19624） |
| ⚠️ 高 | `OLLAMA_LOAD_TIMEOUT=300` 对大响应过短 | 开放 | — | 与 #19624 情况类似；影响交互式使用（Issue #19616） |
| ⚠️ 中 | Windows 更新程序在 >512 个进程存在时崩溃 | 已修复 | [#19625](https://github.com/ollama/ollama/pull/19625) | 由于 `EnumProcesses` 字节到 ID 转换错误所致 |
| ⚠️ 中 | `ollama ps` 显示的 VRAM 使用量与实际 `rocm-smi` 不符 | 开放 | — | 差异最高达 23GB（Issue #17251） |
| ⚠️ 低 | `--version` 显示意外警告 | 已关闭 | [#19613](https://github.com/ollama/ollama/pull/19613) | 现仅输出版本号 |

> 🔗 [Issue #19631 (VRAM 修复)](https://github.com/ollama/ollama/issues/19631)

---

### **6. 对应用开发者的意义**  
- **智能体构建者**：Ripple Preview 现已适合用于需要深度代码推理的生产级智能体工作流。建议使用 `ollama run ripple-preview` 进行原型验证。  
- **多模型部署**：VRAM 与 GPU 分割修复稳定了多 GPU 及低内存环境——对边缘推理与容器化智能体至关重要。  
- **配置注意事项**：若应用涉及长对话或批处理工具调用，请将 `OLLAMA_KEEP_ALIVE` 与 `OLLAMA_LOAD_TIMEOUT` 提升至默认 5 分钟以上（参考开放问题 #19624, #19616）。  
- **未来兼容性**：谨慎启用 `OLLAMA_NUM_PARALLEL` —— 尽管支持 MLX 与 Qwen3.5 变体，需确保每个并发请求有充足 GPU 内存。  
- **图像嵌入缺口**：目前尚不支持 `/api/embeddings` 图像功能——建议在 PR #4296 实现前使用外部视觉编码器。

> 🔗 [功能请求：图像嵌入 API](https://github.com/ollama/ollama/issues/4296)  
> 🔗 [功能请求：深色模式](https://github.com/ollama/ollama/issues/1645)

---  
*由人工智能系统分析团队为基础设施工程师准备 | 2026-11-10*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 – 2026-11-10

---

### **1. 今日重点**

最新 LTS 版本 **v1.108.4** 稳定了多提供商部署中关键的成本追踪与速率限制行为。主要修复解决了长期存在的令牌计数问题（例如 `response_cost` 为 null）、TPM/RPM 强制执行（现正确地以配置值的一半应用）以及 Anthropic 请求中 `vector_store_ids` 的正确处理。新增功能支持客户按模型设置 RPM/TPM 限额，提升细粒度预算控制能力。

---

### **2. 发布与破坏性变更**

- **v1.108.4 (LTS)** – 今日发布，带来稳定性与安全性改进。使用 `ghcr.io/berriai/litellm:lts` 可确保处于最新稳定补丁版本。
  - [发布说明](https://github.com/BerriAI/litellm/releases/tag/v1.108.4)
  - [验证签名](https://github.com/BerriAI/litellm/blob/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub)

> ⚠️ **迁移提示**：v3 速率限制器修复 (#34140) 意味着现有的 `model_rpm_limit` 设置现在可能表现为被减半——升级后请审查所有团队级限额。

---

### **3. 新模型与硬件支持**

- ✅ **Glimpse-1.0** 已加入 Together AI 成本映射（`together_ai/elorian-labs/glimpse-1.0`）  
  [PR #50830](https://github.com/BerriAI/litellm/pull/50830)

- ✅ **Vercel AI Gateway** 现在作为 Decisions 提供商受支持  
  [PR #50642](https://github.com/BerriAI/litellm/pull/50642)

- ✅ **AWS Bedrock Converse API** 现在支持 `performanceConfig.latency="optimized"`  
  [PR #50809](https://github.com/BerriAI/litellm/pull/50809)

- ✅ **基于 S3 的媒体支持** 已扩展：s3:// URL 现在可正确路由至 Bedrock Converse 用于图像、文档和视频  
  [PR #50577](https://github.com/BerriAI/litellm/pull/50577)

---

### **4. 性能与优化**

- 🔥 **流式传输效率提升**：在 DeepSeek、Mistral、Groq、Anthropic 和 Vertex AI 等提供方的 HTTP 处理器中重用上游 `choice` 对象，降低每个流式令牌的 CPU 开销。  
  [PR #50786](https://github.com/BerriAI/litellm/pull/50786)

- 📈 **令牌计数准确性提升**：自定义命名部署（如 Azure AI Foundry）现在使用基础模型的分词器进行成本追踪，而非默认回退方案。  
  [PR #50734](https://github.com/BerriAI/litellm/pull/50734)

- 📊 **成本追踪修复**：仅提示调用现在可向提供方 API（如 Gemini、Vertex AI）发送准确的令牌数量，避免低估。  
  [PR #50783](https://github.com/BerriAI/litellm/pull/50783)

---

### **5. 稳定性与回归问题**

| 问题 | 严重性 | 状态 | 修复 PR |
|------|----------|--------|--------|
| `AnthropicException 400`：多余的 `vector_store_ids` 输入 | 高 | 开放 (#23741) | 尚未解决 |
| TPM 限制未随内存缓存滚动更新 | 严重 | 开放 (#47340) | 尚无修复 |
| v3 速率限制器在配置值一半处强制执行 RPM/TPM | 严重 | 已关闭 (#34140) | ✅ 已解决 |
| 当模型在不同提供方定价时成本追踪失败 | 高 | 开放 (#47675) | 待处理 |
| JSON-RPC `id` 被 `pass_through_endpoints` 移除 | 中等 | 开放 (#49479) | 尚无修复 |
| Gemma/TTS `/v1/audio/speech` 因缺少 `voice` 参数失败 | 中等 | 开放 (#20078) | 待处理 |
| RAG 文件摄入返回 LiteLLM 管理的 ID 并使用团队凭证 | 低 | 已关闭 (#50459) | ✅ 已修复 |

> 💡 **注意**：多个高严重性问题仍处于开放状态，尤其集中在跨提供方的成本准确性与速率限制一致性方面。

---

### **6. 对应用开发者的意义**

- **预算与成本可见性**：随着每客户按模型设置 TPM/RPM 限额的功能上线（PR #50797），你可在多租户应用中实施更严格的财务控制。请确保部署配置反映此变更。

- **避免沉默的令牌低估**：若使用自定义名称模型（如 Azure AI Foundry），请确认 `base_model` 设置正确，以防止错误的成本报告。

- **谨慎对待速率限制**：升级至 v1.108.4 后，请重新评估 `model_rpm_limit` 与 `tpm_limit` 值——它们现已正确执行，不再被减半。

- **防护机制与流式传输**：请注意 `Presidio` 防护机制不会恢复 `tool_calls[].function.arguments` 中的敏感信息（问题 #31950）。建议在客户端实现逻辑，或谨慎使用 `output_parse_pii`。

- **安全使用 S3 URL**：通过 Bedrock 进行图像/文档摄入时，确保直接使用 `s3://` 路径——此前会静默拒绝。

> 🛠️ **可操作提示**：在生产环境中，务必对 `lts` 镜像运行 `cosign verify` 以确保完整性。  
> 🔗 [Cosign 验证指南](https://github.com/BerriAI/litellm/blob/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub)

---  
*摘要生成时间：2026-11-10 | 来源：GitHub – BerriAI/litellm*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-10**

---

### **1. 今日亮点**  
Unsloth 持续推进其 AI 基础设施栈的演进，重点聚焦于稳定性、用户体验打磨以及多 GPU/ROCm 性能优化。关键进展包括 `sd.cpp` 中 VRAM 管理的关键修复、对 GGUF 重载的改进处理，以及扩散管道状态追踪的优化——尤其在 ROCm 平台上的表现。与此同时，新提交的 PR 涉及界面一致性、模型管理体验提升，以及跨平台可靠性（Windows/macOS/Linux），预示着 v0.2.0 版本前更深层次的成熟。

---

### **2. 发布与破坏性变更**  
无。过去 24 小时内未发布新版本或破坏性变更。

---

### **3. 新模型与硬件支持**  
- **ROCm (gfx1151)**：通过 PR #17347 与 #17341 实现了多卡间层拆分和注意力核探测的增强支持。  
- **Blackwell 之前的 GPU**：PR #17343 引入了更智能的检查点路由机制——官方 NVFP4 微调模型仅在显式匹配旧硬件时才回退至 INT8，减少不必要的量化操作。  
- **FLUX.3**：基于每张卡的步进 cuDNN 注意力验证支持（#17341）以及联合块量化共享功能（#17337）进一步扩展。  
- **图像生成（sd.cpp）**：在低 VRAM 条件下增强了鲁棒性，当 `sd-server` 拒绝渲染时引入重试逻辑（#17339）。

> 🔗 [PR #17343](https://github.com/unslothai/unsloth/pull/17343) | [PR #17341](https://github.com/unslothai/unsloth/pull/17341)

---

### **4. 性能与优化**  
- **VRAM 效率**：PR #17345 确保 GGUF 重载仅在确认停止后 VRAM 已完全释放时才执行，防止部分 CPU 加载及推测性解码浪费。  
- **内核级优化**：  
  - PR #17347 仅对实际参与层拆分的显卡应用 SDPA 保护，降低冗余 GPU 开销。  
  - PR #17340 在管道卸载后恢复 ROCm 上的 `torch.cuda.tunable` 状态——防止虚假的 GEMM 配置错误。  
- **内存与延迟**：PR #17339 在 VRAM 受限情况下为失败的图像渲染实现重试逻辑，显著提升 8GB RTX 3070 系统的成功率。

> 📈 *预期影响*：受限环境下图像生成失败尝试预计减少 15–30%；长周期推理会话期间稳定性提升。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重程度 | 状态 | 修复 PR | 备注 |
|------|----------|--------|--------|-------|
| Windows 11 上图像生成回退至 CPU（RTX 3060） | 高 | 打开 | 无 | 影响桌面应用用户；可在 Win 上复现，但 M1 Mac 不受影响。 |
| Linux 强制使用 CPU 而非 GPU | 高 | 打开 | 无 | 出现在 Manjaro KDE 系统上；可能与 CUDA 驱动检测或运行时绑定有关。 |
| `重新生成标题` 会复制上一条消息部分内容 | 中 | 打开 | 无 | 影响聊天流程内容完整性的 UX 问题。 |
| 自定义文件夹浏览器对托管账户返回 403 错误 | 中 | 打开 | 无 | Studio 中的安全边界问题，影响企业部署。 |
| 自定义 OpenAI 兼容连接中视频上传被阻塞 | 中 | 打开 | 无 | 阻碍 Studio 工作流中的数据摄入。 |

> ⚠️ **注意**：尽管多个高严重性问题仍处于开放状态，但今日未合并任何修复 PR。团队正在积极处理平台相关回归问题。

---

### **6. 对应用开发者的影响**  
- **对智能体开发者**：在多 GPU 或 ROCm 环境下链式调用图像生成（`sd.cpp`）与 LLM 推理时，可期待更可靠的运行行为。建议使用 `UNSLOTH_REUSE_CACHED_PICKLE_WEIGHTS=0` + `UNSLOTH_PREQUANT_PREFER_SAFETENSORS=1` 强制安全权重加载（#17342）。  
- **对工具集成者**：使用托管账户与自定义 API 端点时需谨慎——权限与文件夹访问在不同环境间仍不一致（#17292, #17295）。  
- **对 UI 构建者**：即将重构的可视化选择器（#17164, #17344）将简化主题定制并改善键盘导航——非常适合无障碍设计。  
- **对部署工程师**：密切监控 VRAM 使用情况——若未完全释放内存，重载仍可能静默失败。建议在客户端层面实现重试逻辑，直至 #17345 完全采纳。

> 💡 技巧提示：在 `sd.cpp` 构建中，若因内存不匹配导致自动拆分失败，可选择性使用 `--auto-fit off`（#17346）。

---  
*摘要源自 GitHub 活动：unslothai/unsloth @ 2026-11-10*

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*