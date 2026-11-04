# AI 基础设施日报 2026-11-04

> 生成时间: 2026-11-04 02:40 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI基础设施生态报告 – 2026-11-04**

---

### **1. 生态概览**

截至2026年末，AI推理与服务领域呈现出高度专业化、硬件加速迅猛以及高性能分布式引擎与轻量级开发者友好网关之间日益分化的趋势。各项目在关键能力上趋于收敛——批量不变推理、推测解码、多GPU拆分、FP8/量化模型支持——但在目标应用场景上则不断分化：从边缘移动推理（llama.cpp）到大规模智能体编排（SGLang、LiteLLM）。异构后端（ROCm、MLX、Blackwell SM120）的兴起加剧了竞争，性能提升如今已与底层内核优化和内存管理紧密绑定。与此同时，多模态视频输入与实时TTS流的出现，预示着向丰富、交互式AI应用的转变。

---

### **2. 活跃度对比**

| 项目       | 近24小时开放问题数 | 近24小时合并PR数 | 发布状态        |
|------------|-------------------|-------------------|----------------|
| **vLLM**   | 5                 | 7                 | 无              |
| **SGLang** | 9                 | 5                 | 无              |
| **llama.cpp** | 6             | 5                 | `b12094`, `b12091` |
| **Ollama** | 3                 | 4                 | `v0.40.15-rc0`  |
| **LiteLLM** | 4               | 4                 | `v1.109.0-rc.1` |
| **Unsloth** | 7               | 3                 | `v0.1.914-beta` |

> ✅ *观察：* SGLang 和 vLLM 在活跃问题数量上领先，表明其在稳定性与可扩展性方面投入了深度工程精力。Ollama 与 LiteLLM 推动发布速度，反映出强劲的产品化势头。Unsloth 的测试版发布展示了激进的功能交付，尽管存在界面稳定性问题。

---

### **3. 模型支持竞赛**

| 新模型 / 架构              | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1 (ROCm)**   | ✅   | ✅     | ❌        | ❌     | ❌      | ✅      |
| **Nemotron 3.5 ASR 流式传输** | 🚧   | ❌     | ❌        | ❌     | ❌      | ❌      |
| **Nemotron 3.5 转录**      | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **Metis-1.6**              | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **xing4_0**                | ❌   | ❌     | ✅        | ❌     | ❌      | ❌      |
| **MiMo V2.6 (MLX/GGUF)**   | ❌   | ❌     | ❌        | ✅     | ❌      | ❌      |
| **Qwen3-VL / LFM3-VL (视频)** | ❌   | ❌     | ❌        | ✅ (macOS) | ❌    | ✅      |

> 🏆 **胜出者：** *Ollama* 在面向用户的多模态模型支持上领先，尤其在Apple Silicon视频输入方面表现突出。  
> 🔥 **创新领导者：** *Unsloth* 将Qwen3-VL、Llama 4等前沿模型集成至其FastModel流水线，并通过CUDA图加速实现极致性能。  
> ⚠️ **差距：** 目前尚无项目支持 **Nemotron 3.5 ASR 流式传输**——这为网关或微调框架提供了潜在机会。

---

### **4. 性能前沿**

| 优化重点                  | vLLM                     | SGLang                  | llama.cpp               | Ollama                | LiteLLM               | Unsloth               |
|----------------------------|--------------------------|-------------------------|-------------------------|-----------------------|-----------------------|------------------------|
| **KV缓存与内存**           | ✅ 缓存感知准入，TPOT回退 | ✅ HiCache分片         | ✅ `--moe-cache-mib`（有缺陷） | ✅ MLX前缀缓存修复 | ✅ Redis电路断路器 | ✅ LoRA折叠（I/O） |
| **批处理与推理**           | ✅ 批量不变模式，稀疏索引 | ✅ PD拆分，DCP大小修复 | ✅ `--sm tensor`（CUDA） | ✅ 并发生成         | ✅ 流式TTS          | ✅ CUDA图（3–7倍提速） |
| **量化与内核**             | ✅ NVFP4，BF16融合，ROCm | ✅ DeepGEMM（SM120），Gluon内核 | ✅ NVFP4 GGUF，Q2_0，Hexagon | ✅ GGUF验证         | ✅ Cosign签名图像     | ✅ FP8 LoRA |
| **分布式服务**             | ✅ 分片预填充（ROCm）   | ✅ 跨节点PD + HiCache   | ❌                      | ❌                    | ✅ 代理级路由        | ❌ |
| **推测解码**               | ✅ Draft-MTP（有注意事项） | ✅ DFLASH（易崩溃）     | ✅ Draft-MTP（正确性缺陷） | ❌                   | ✅ 回退逻辑          | ✅ FastModel CUDA图 |

> 🔍 **趋势：** 高端性能正日益由**内核级优化**（如DeepGEMM、Gluon、CUDA图）和**分布式内存协调**（HiCache、TPOT回退）定义。Unsloth凭借**CUDA图实现3–7倍解码提速**脱颖而出；而SGLang与vLLM则在**PD拆分**与**可扩展KV管理**方面持续突破边界。

---

### **5. 层级定位**

| 项目       | 主要层级              | 核心差异化                                  | 目标用户                          |
|------------|------------------------|--------------------------------------------|----------------------------------|
| **vLLM**   | 高性能引擎            | 批量不变推理，ROCm扩展                     | 企业推理团队                     |
| **SGLang** | 多后端推理引擎        | PD拆分，Blackwell/ROCm优化                 | 大规模智能体平台                 |
| **llama.cpp** | 本地运行时（CPU/GPU） | 跨平台，轻量，移动端支持                   | 开发者，边缘，嵌入式系统         |
| **Ollama** | 开发者网关 + CLI      | 简洁用户体验，本地优先，多模态视频输入     | 单人开发者，原型设计，智能体     |
| **LiteLLM** | API代理与编排         | 多供应商路由，软预算控制，安全机制         | 企业，内部工具                   |
| **Unsloth** | 智能体导向运行时      | FastModel，CUDA图，Flows编辑器            | AI智能体，实时工作流             |

> 🎯 **战略洞察：** vLLM/SGLang 主导**基础设施层**；Ollama/LiteLLM 控制**开发者访问层**；Unsloth 以性能优先设计聚焦于**智能体执行层**。

---

### **6. 趋势信号**

#### **提取的关键趋势：**
1. **异构硬件加速成为主流**：ROCm、AMD MI350X/P、Intel XPU、Apple MLX、NVIDIA Blackwell 已不再是小众选择，而是性能提升的核心驱动力。vLLM 与 SGLang 在 ROCm 集成方面处于领先地位。
2. **拆分架构已进入生产环境**：SGLang 已证实 **预填充-解码拆分（PD）** 在 TP/PP/CP/DCP 场景下稳定可用，非常适合大规模低延迟、高吞吐推理。
3. **网关的安全与治理意识增强**：LiteLLM 的 cosign签名镜像与 `soft_budget` 警报，标志着对 **可观测性、合规性与成本控制** 的成熟关注。
4. **以智能体为中心的优化**：Unsloth 的 CUDA 图、Ollama 的视频输入、SGLang 的 HiCache 表明，**实时智能体工作流** 需要端到端优化，而不仅是模型质量。
5. **多模态优先**：视频输入（Ollama）、音频流（LiteLLM）、视觉模型（SGLang/vLLM）已成为标配，而非实验性功能。

#### **开发者应重点关注：**
- **在拆分部署环境中避免使用 `--enable-prefix-caching`**（vLLM Issue #61160）。
- **在 #25618 修复前，勿对量化模型使用 `draft-mtp`**（llama.cpp）。
- **监控 Redis 故障转移行为**（LiteLLM PR #49884）——对生产代理至关重要。
- **准备应对API统一**（SGLang RFC #45816，LiteLLM `callback_settings`）。
- **优先保障安全实践**：使用签名Docker镜像（LiteLLM），并在流式客户端中验证 `AuthorizationError`。

> ✅ **最终结论：** 基础设施栈正从“仅追求更快推理”转向**安全、可扩展、面向智能体的平台**。请根据你的层级选择：**引擎（vLLM/SGLang）、网关（Ollama/LiteLLM）或智能体运行时（Unsloth）**，并使你的技术栈与新兴标准对齐——**拆分架构、安全性、多模态就绪性**。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

### **vLLM Digest — 2026-11-04**

#### **1. 今日亮点**  
vLLM 项目持续深化对 **批处理无关推理（batch-invariant inference）** 的支持，议题 #27433（104 条评论）推动优化工作，致力于消除大语言模型服务中的非确定性问题——这对实现可复现、生产级推理至关重要。与此同时，在 **ROCm 与多模态性能** 方面也取得显著进展，包括关键的 PR #65555，该功能在 AMD GPU 上将 DeepSeek-V4.1 的稀疏索引预填充行跨张量并行（TP）层级分片，显著提升扩展能力。

#### **2. 发布与破坏性变更**  
过去 24 小时内未报告任何发布或破坏性变更。未观察到新版本发布或接口/配置的破坏性更改。

#### **3. 新模型与硬件支持**  
- ✅ **DeepSeek-V4.1**：现已完整支持 ROCm，通过 PR #65555 实现稀疏索引预填充行的分片处理，可在 MI350X/P 系统上获得更好的扩展性。  
- ✅ **NVIDIA Nemotron 3.5 ASR 流式**（nemotron-3.5-asr-streaming-0.6b）：作为功能请求（议题 #47455）加入，旨在支持多语言流式语音转文本场景。  
- ✅ **GLM-5.3-Flash**：通过 PR #63070 实现 ROCm 特定优化，默认启用可拆分的 CUDA 图以提升编译成功率。  
- 🚧 **PaliGemma**：因存在错误的多模态输出行为（因果注意力与所需双向前缀语言模型不匹配），提议弃用（PR #65409）。  

> 🔗 [PR #65555](https://github.com/vllm-project/vllm/pull/65555) | [PR #63070](https://github.com/vllm-project/vllm/pull/63070) | [PR #65409](https://github.com/vllm-project/vllm/pull/65409)

#### **4. 性能与优化**  
- 在 Qwen3.8-Flash-Next-NVFP4 上实现 **+13.8% 解码吞吐量**，采用上下文感知的稀疏语言模型头（PR #59740），结合全 NVFP4 量化后最高可达 **+23.3% 提升**。  
- 通过 PR #65344，为 SM89（RTX 4090）的预填充内核新增 **BF16 投影融合** 功能（可选开启），专用于 Qwen3.5-4B MoE MLPs 场景。  
- **缓存感知的请求准入排序**（PR #54625）通过优先处理带有缓存前缀的请求，提升效率——在高吞吐场景下有效降低冷启动延迟。  
- **KV 缓存池饱和缓解**：PR #64460 引入自动准入退避机制，防止高并发下解码 TPOT 膨胀（约 2.3 倍）问题。  

> 🔗 [PR #59740](https://github.com/vllm-project/vllm/pull/59740) | [PR #65344](https://github.com/vllm-project/vllm/pull/65344) | [PR #54625](https://github.com/vllm-project/vllm/pull/54625) | [PR #64460](https://github.com/vllm-project/vllm/pull/64460)

#### **5. 稳定性与回归问题**  
- **严重**：`enable_adaptive_verification` 在 SM120（Blackwell）GPU 上被拒绝（议题 #63343），导致高级验证功能无法使用。  
- **严重**：在 PD 分离架构中，因共享前缀 KV 复用导致跨请求上下文污染（议题 #61160），造成用户间内容泄露——已通过禁用前缀缓存（`--no-enable-prefix-caching`）修复。  
- **高严重性**：在可信计算模式（TDX 客户端）下出现静默垃圾输出，原因为陈旧的 UVA 视图（议题 #57224）；可通过设置 `VLLM_USE_V2_MODEL_RUNNER=0` 临时规避。  
- **ROCm**：在高解码并发下，`rocm_fp8_paged_mqa_logits` 中触发 `Workspace is locked` 断言（议题 #64874），影响 GLM-5.3-Flash。  
- **其他**：由于 AMD 嵌入层缺失 `weight_scale`，Qwen3.8-Flash-Next-FP8 在 ROCm 上无法加载（议题 #58688）。  

> 🔗 [Issue #63343](https://github.com/vllm-project/vllm/issues/63343) | [Issue #61160](https://github.com/vllm-project/vllm/issues/61160) | [Issue #57224](https://github.com/vllm-project/vllm/issues/57224) | [Issue #64874](https://github.com/vllm-project/vllm/issues/64874) | [Issue #58688](https://github.com/vllm-project/vllm/issues/58688)

#### **6. 对应用开发者的意义**  
- **谨慎使用批处理无关模式**：确保您的负载符合确定性推理预期——详见议题 #27433，相关优化仍在进行中。  
- **在分离式部署中避免前缀缓存**：若服务敏感或混合内容流量，请使用 `--no-enable-prefix-caching`，直至议题 #61160 修复。  
- **充分利用 ROCm 优势**：若使用 AMD MI350X/P 硬件，启用基于分片的稀疏索引（PR #65555）和可拆分的 CUDA 图（PR #63070），以获得更佳的 DeepSeek-V4.1 性能。  
- **关注内存问题**：高并发解码工作负载可能遭遇工作区锁定——建议实现退避逻辑或调整队列调度策略。  
- **准备迁移 PaliGemma**：如在多模态流水线中使用，请尽早迁移，目前无明确修复时间表。  

> 🔗 [RFC #27433](https://github.com/vllm-project/vllm/issues/27433) | [Issue #61160](https://github.com/vllm-project/vllm/issues/61160) | [PR #65555](https://github.com/vllm-project/vllm/pull/65555)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-04**

---

### **1. 今日亮点**  
SGLang 生态系统持续巩固其作为高性能、多后端推理引擎的地位，尤其在 **Prefill-Decode 分离（PD）** 稳定性和 **Blackwell（SM120）** 内核优化方面取得显著进展。关键更新包括 `SGLANG_SIMULATE_ACC_LEN` 种子一致性修复、Qwen3.8-Flash-Next 在 AMD ROCm 上的改进支持，以及 **Mooncake HiCache** 分布式存储跨节点集成的积极开发。关键的 CI 可靠性问题仍在调查中，不稳定测试的追踪工作持续推进。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本或破坏性变更。*  
然而，通过 **[RFC: 完全迁移至共享 api.v1 Generate](https://github.com/sgl-project/sglang/issues/45816)** 统一后端 API 的努力正在推进——这将统一 `sgl-processor`、`sgl-router` 与 Rust 服务端的生成逻辑。开发者应为 `api.v1` 中即将到来的模式变更做好准备。

---

### **3. 新模型与硬件支持**  
- ✅ **AMD ROCm (gfx950 / MI355X):** 针对 **Qwen3.8-Flash-Next** 的活跃性能优化，包括 GLM-5 解码链的 Gluon 内核融合 ([PR #46486](https://github.com/sgl-project/sglang/pull/46486))，以及修复 MI300X 上 `--dcp-size > 1` 导致的崩溃问题 ([Issue #38709](https://github.com/sgl-project/sglang/issues/38709))。  
- ✅ **Intel XPU:** RFC 提议支持 **分页专家 + 前瞻专家调度（LES）**，并为 iGPU MoE 降载提供 SSD 层支持 ([Issue #46520](https://github.com/sgl-project/sglang/issues/46520))。  
- ✅ **NVIDIA Blackwell (SM120):** DeepSeek-V4.1 FP8 模型已原生启用 block32 DeepGEMM（`--fp8-gemm-backend deep_gemm`），并优化了 DSV4 预填充吞吐量 ([PR #47210](https://github.com/sgl-project/sglang/pull/47210))。  
- ✅ **新模型：** 原生支持 **Nemotron-3-Embed-1B-BF16** ([PR #45885](https://github.com/sgl-project/sglang/pull/45885))，并扩展 Mooncake Engram 存储以支持 **Qwen4-Exp** ([PR #47673](https://github.com/sgl-project/sglang/pull/47673))。

---

### **4. 性能与优化**  
- 🔥 **SM120 上的 DeepSeek-V4.1：** 通过 `deep_gemm` 后端实现预填充优化的重大进展；预计吞吐量将从 ~2–7K tok/s 提升至接近 **12.5K tok/s**（媲美 vLLM/Marlin），待进一步调优 ([Issue #33422](https://github.com/sgl-project/sglang/issues/33422))。  
- 🚀 **AMD 上的 Qwen3.8-Flash-Next：** GLM-5 解码 MLA 链现已在 gfx950 上以 Gluon 内核运行，降低内核启动开销 ([PR #46486](https://github.com/sgl-project/sglang/pull/46486))。  
- ⚡ **内存效率：** 多项 PR 聚焦于减少 EAGLE/MTP 启动期间冗余的 MoE 数据拷贝 ([PR #42510](https://github.com/sgl-project/sglang/pull/42510))，以及优化 `vectorized_gather_kernel` 的越界检查 ([Issue #37936](https://github.com/sgl-project/sglang/issues/37936))。  
- 💾 **存储层：** Mooncake HiCache 现已支持 **节点分片的 Engram 存储**，可在计算节点间实现可扩展的分布式 L2/L3 缓存 ([PR #47673](https://github.com/sgl-project/sglang/pull/47673))。

---

### **5. 稳定性与回归问题**  
⚠️ **严重崩溃与缺陷：**  
- **CUDA 核心转储追踪 (#26340)**：354 条评论 —— `pr-test.yml` 工作流中反复出现的核心转储，表明底层 GPU 驱动/运行时存在不稳定性，可能与推测解码或 PD 分离有关。*尚未修复。*  
- **DFLASH + PD 分离崩溃 (#36140)**：当解码在 PD 模式下运行时，DFLASH 推测解码失败，因缺少分离连接配置。*修复待定。*  
- **ROCm `--dcp-size > 1` 崩溃 (#38709)**：`forward_absorb_rocm_core` 中从单值返回解包出两个值。*已在 MI300X 上复现。*  
- **HiCache 备份失败 (#48145)**：Qwen3.8-Flash-Next-FP8 模型的 Mooncake HiCache 备份失败。*高优先级。*  
- **VAE 解码时间尖峰 (#48196)**：在迁移到 `5090-sm*` 运行器后，1-GPU-5090 运行器上 Wan VAE 解码时间飙升 2–10 倍，影响多模态流水线。

🟢 **不稳定的 CI 基础设施**：问题 #42752 跟踪 NVIDIA `PR Test Base/Extra` 运行中的持续不稳定性，阻碍了 PR 验证。*正在处理中。*

---

### **6. 对应用开发者的影响**  
- **放心使用 PD 分离**：Q4 路线图确认，PD 已在 TP/PP/CP/DCP/DP-attention 上稳定运行，并支持 HiCache 与推测解码 —— 适用于大规模、低延迟推理场景。  
- **针对 Blackwell (SM120) 优化**：对 DeepSeek-V4.1 及其他模型启用 `--fp8-gemm-backend deep_gemm`，以释放近峰值吞吐量。密切监控预填充基准表现。  
- **避免在 PD 模式下使用 DFLASH**：在 #36140 修复前，避免在 `--disaggregation-mode decode` 下使用 DFLASH 推测解码。建议改用 DSPARK 或 EAGLE。  
- **为 API 统一做准备**：开始将工具链与正在演进的 `api.v1.Generate` 模式对齐（见 #45816）。预计旧版端点将被弃用。  
- **处理多模态边缘情况**：谨慎对待提示词中的原始 `<|image|>` 占位符 —— 若未正确配对图像数据，可能导致 500 错误 ([Issue #37579](https://github.com/sgl-project/sglang/issues/37579))。建议使用 `oss-harmony` 进行离线服务 ([PR #48267](https://github.com/sgl-project/sglang/pull/48267))。  

👉 **可操作建议**：若在 AMD 平台运行，请验证模型与 ROCm 7.2 的兼容性，并在 #38709 修复前监控 `--dcp-size` 限制。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp 消息简报 – 2026-11-04**

---

### **1. 今日亮点**  
最新开发周期在 Hexagon 与 CUDA 后端上显著提升了推测解码性能，通过优化 `--sm tensor` 通信，提示词评估吞吐量提升 19%。新增对 **Metis-1.6**、**Nemotron 3.5 Transcribe** 以及 **xing4_0** 模型的支持，拓展了多模态与专用推理工作负载的生态。关键稳定性修复解决了 MTP 推测解码崩溃及 GPU 内存管理问题。

---

### **2. 发布与破坏性变更**  
- **b12094**：Hexagon 后端优化：通过跳过无关 tile 及使用 block scan（`#31279`）实现更快的 `TOP_K` 采样。  
- **b12091**：推测解码现在可正确保存/恢复 MTP 隐状态；简化检查点机制（`#30141`）。  
- **b12087**：新增对 **Nemotron 3.5 Transcribe** 模型架构的支持（`#31471`）。  
- **b12089**：通过新架构注册与转换脚本，全面支持 **Metis-1.6** 模型（`#31134`）。  
- **b12088**：启用 **NVFP4 LFM3 GGUF** 格式支持（`#31608`）。  

> 📌 *未检测到破坏性 API 变更。所有更新均为新增功能或性能优化。*

---

### **3. 新模型与硬件支持**  
| 模型 / 架构 | 后端 | 状态 | PR/问题 |
|----------------------|--------|--------|---------|
| **Metis-1.6** | CPU/GPU | ✅ 已支持 | [#31134](https://github.com/ggml-org/llama.cpp/pull/31134) |
| **Nemotron 3.5 Transcribe** | CPU/GPU | ✅ 已支持 | [#31471](https://github.com/ggml-org/llama.cpp/pull/31471) |
| **xing4_0** | CPU/GPU | ✅ 已支持（新架构） | [#31589](https://github.com/ggml-org/llama.cpp/pull/31589) |
| **Hexagon (Snapdragon 7 Gen 4)** | HMX | ⚠️ 部分支持 | 问题 [#29473](https://github.com/ggml-org/llama.cpp/issues/29473) |
| **AMD gfx1151 (ROCm 10.0+)** | HIP | ✅ CI 测试已启用 | PR [#31567](https://github.com/ggml-org/llama.cpp/pull/31567) |

> 🔧 *新增量化格式：NVFP4（Q4_K_M/Q5_K_M）、Q2_0（Adreno OpenCL），以及 LIGHTNING_INDEXER（Hexagon）的 F16/F32 支持。*

---

### **4. 性能与优化**  
- **Hexagon**：通过跳过无关 tile 并以 `top_k_scan()` 替代 bitonic sort，加速 `TOP_K` 采样 —— 降低每 token 延迟。  
- **CUDA**：`--sm tensor` 提示词评估性能相比原生版本平均提升 **+19% t/s**，且无 PPL 下降（`#30956`）。  
- **SYCL**：`fast_ggml_sycl_comm_exchange()` 优化设备间通信 —— 缩小 `--sm layer` 与 `--sm tensor` 之间的差距。  
- **OpenCL**：针对 Adreno GPU 优化 `q2_0 GEMV` 内核（每线程输出 4–8 行）—— 提升低比特量化性能。  
- **WebGPU**：新增批处理张量接口（`get_tensor_n`、`set_tensor_n`），提升批处理效率（`#31473`）。  

> 💡 *这些优化对高吞吐推测解码与大上下文推理至关重要。*

---

### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 受影响后端 | 状态 | 修复 PR |
|------|----------|-------------------|--------|--------|
| [Bug #25618](https://github.com/ggml-org/llama.cpp/issues/25618)：量化目标上的 Draft-MTP 贪心输出发散 | 高 | CUDA/SYCL | 待处理 | 尚无修复 |
| [Bug #31586](https://github.com/ggml-org/llama.cpp/issues/31586)：CUDA 加载 DFlash 时使用 `--sm tensor` 崩溃 | 高 | CUDA | 待处理 | 正在处理中 |
| [Bug #31064](https://github.com/ggml-org/llama.cpp/issues/31064)：oneAPI 2026.2.0 上 SYCL 段错误 | 中 | SYCL | 待处理 | PR [#31605](https://github.com/ggml-org/llama.cpp/pull/31605)（Docker 修复） |
| [Bug #30869](https://github.com/ggml-org/llama.cpp/issues/30869)：AMD CDNA2 (gfx90a) 编译失败 | 中 | HIP | 待处理 | 尚无 PR |
| [Bug #29893](https://github.com/ggml-org/llama.cpp/issues/29893)：Vulkan 上使用 `--moe-cache-mib` 导致显存飙升 | 中 | Vulkan | 待处理 | 尚无修复 |

> ⚠️ **紧急**：MTP 推测正确性问题（#25618）影响量化模型在贪心采样下的行为 —— 可能在生产级代理中导致输出不一致。

---

### **6. 对应用开发者的意义**  
- **在 #25618 修复前，仅可在 BF16 或非量化目标上安全使用 `--spec-type draft-mtp`** —— 避免在 Q4_K_M、Q5_K_M 等量化格式下使用。  
- **在多 GPU 环境中启用 `--sm tensor` 以获得更高吞吐量** —— 当前性能比之前快最多 19%。  
- **利用新模型支持**：通过标准 CLI 标志部署 **Metis-1.6**、**Nemotron 3.5 Transcribe** 与 **xing4_0** 模型。  
- **通过 Hexagon + LIGHTNING_INDEXER 支持优化移动端推理**（请检查设备兼容性）。  
- **避免在 Vulkan 上使用 `--moe-cache-mib`** 因存在内存回归问题。建议改用 `--cache-ram`。  
- **如使用 Intel SYCL，升级 Docker 镜像** —— PR [#31605](https://github.com/ggml-org/llama.cpp/pull/31605) 修复了运行时崩溃。  

> 🛠️ *对于生产系统：监控 `llama-server` 日志中 `GGML_ASSERT` 崩溃情况，特别是在 MTP/Draft 加载阶段。应用来自 `b12094+` 的补丁，并针对目标量化方案进行测试。*  

🔗 **官方网站**：[https://llama.app](https://llama.app)  
🔗 **最新发布版**：[b12094](https://github.com/ggml-org/llama.cpp/releases/tag/b12094)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama 摘要 – 2026-11-04**

---

### **1. 今日亮点**  
最新发布的 **v0.40.15-rc0** 版本在 Apple Silicon（MLX）性能方面实现关键优化，并新增对全新 **MiMo V2.6** 模型的支持。主要改进包括 MLX 上的视频输入能力、增强的分词 API，以及流式响应中授权处理的修复。这些更新显著提升了多模态推理的可靠性与开发者工具体验。

---

### **2. 发布与重大变更**  
- **v0.40.15-rc0**：发布包含关键修复与功能增强。  
  - *MLX 前缀缓存内存管理* 已优化，避免在 32GB Apple Silicon 系统上出现过度交换使用的问题 ([#18131](https://github.com/ollama/ollama/issues/18131))。  
  - 通过 `./ollama pull dhiltgen/mimo-v2.6` 新增支持 *MiMo V2.6* 模型 ([#19400](https://github.com/ollama/ollama/pull/19400))。  
- **API 改进**：  
  - 流式响应中的 `401 Unauthorized` 错误现在即使响应体格式异常也能正确传递 `AuthorizationError` ([#19489](https://github.com/ollama/ollama/issues/19489)，已在 [#19491](https://github.com/ollama/ollama/pull/19491) 中修复)。  
  - 新增 `/api/tokenize` 与 `/api/detokenize` 接口，可在不加载模型的情况下进行分词计数 ([#19498](https://github.com/ollama/ollama/pull/19498))。

---

### **3. 新模型与硬件支持**  
- **新模型**：  
  - **MiMo V2.6**（MLX/GGUF）—— 支持多模态推理，指令遵循与上下文处理能力得到提升。  
  - **AutoTrust**（MLX）—— 实验性决策模型，适用于具备信任感知的代理工作流 ([#19399](https://github.com/ollama/ollama/pull/19399))。  
- **硬件与后端增强**：  
  - **MLX 视频输入支持**已加入 macOS：Qwen3-VL、LFM3-VL 与 LFM3-Omni 模型现可通过 `images` 字段接收 MP4/MOV 视频片段 ([#19348](https://github.com/ollama/ollama/pull/19348))。  
  - **CUDA/NVIDIA GPU** 对视觉模型（Qwen2.5vl:3b）的支持已确认——此前因旧显卡内核不匹配而失败 ([#19482](https://github.com/ollama/ollama/issues/19482))。  
  - **Linux/Windows 视频输入**仍在开发中；当前仅在 macOS MLX 上可用 ([#19503](https://github.com/ollama/ollama/issues/19503))。

---

### **4. 性能与优化**  
- **模型下载速度**：  
  - GGUF blob 下载时的校验现采用并行 HTTP 范围增量验证，消除下载后冗余哈希计算 ([#19505](https://github.com/ollama/ollama/pull/19505))。  
  - 大模型（如 70B+）延迟降低约 30–50%。  
- **内存效率**：  
  - MLX 前缀缓存不再在 Apple Silicon（32GB 内存）上过度分配，有效防止代理负载下大量交换行为 ([#18131](https://github.com/ollama/ollama/issues/18131))。  
- **并发支持**：  
  - MLX 模型现已支持通过 `OLLAMA_NUM_PARALLEL` 设置实现并发生成 ([#19197](https://github.com/ollama/ollama/pull/19197))，显著降低批量请求的首令牌延迟。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 状态 | 修复 PR |
|------|----------|--------|--------|
| **模型驱逐 + 并发加载时调度器死锁** | 严重 | 开放 | 尚无修复 ([#17408](https://github.com/ollama/ollama/issues/17408)) |
| **工具调用截断返回 HTTP 500** | 高 | 开放 | 已在 [#19494](https://github.com/ollama/ollama/pull/19494) 中修复 |
| **大上下文窗口的显存估算错误** | 中等 | 开放 | 尚无修复 ([#10128](https://github.com/ollama/ollama/issues/10128)) |
| **v0.40.13 中 NVIDIA Pascal GPU CUDA 错误** | 高 | 开放 | 尚无修复 ([#19485](https://github.com/ollama/ollama/issues/19485)) |
| **HumanNumber 四舍五入越过单位边界** | 低 | 开放 | 已在 [#19500](https://github.com/ollama/ollama/pull/19500)、[#19499](https://github.com/ollama/ollama/pull/19499) 中修复 |

> ⚠️ **重要提示**：调度器死锁问题仍未解决，在高并发场景下可能导致服务完全冻结。

---

### **6. 对应用开发者的意义**  
- **代理与工具**：使用 `OLLAMA_NUM_PARALLEL` 减少多请求流程中的延迟；确保 `num_predict` 设置高于预期工具调用长度，以避免返回 HTTP 500 ([#19494](https://github.com/ollama/ollama/pull/19494))。  
- **多模态应用**：在 macOS MLX 上利用 Qwen3-VL 与 LFM3-VL 模型的视频输入功能——适用于 RAG 或视觉代理场景。  
- **CI/CD 与自动化**：在 `list` 与 `ps` 命令中使用 `--json` 标志，实现脚本化的模型管理 ([#17349](https://github.com/ollama/ollama/pull/17349))。  
- **安全与可靠性**：流式客户端中始终验证 `AuthorizationError`；使用 `/api/tokenize` 预先计算提示长度，避免无声截断 ([#19498](https://github.com/ollama/ollama/pull/19498))。  
- **规避陷阱**：不要假设 `truncation=false` 即可防止输入截断——该参数目前仅在通过 `openai` 兼容层显式传入时才生效 ([#19501](https://github.com/ollama/ollama/pull/19501))。

---  
*数据来源：[github.com/ollama/ollama](https://github.com/ollama/ollama)*  
*摘要生成时间：2026-11-04*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM 摘要 – 2026-11-04**

---

### **1. 今日亮点**  
最新发布的候选版本 `v1.109.0-rc.1` 引入了通过 cosign 签名的 Docker 镜像以增强安全性，并在代理层配置方面取得进展，包括支持按密钥设置回调（callback）和内部用户软预算预警。关键稳定性修复解决了流式处理中的核心问题（如 `flex_unavailable` 错误映射）以及 Redis 故障转移期间的状态管理问题，同时正在进行的工作也在提升 Admin UI 的国际化准备度。

---

### **2. 发布与破坏性变更**  
- **`v1.109.0-rc.1`**：已发布，cosign 签名验证关联提交 [`e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065)。所有 Docker 镜像现已签名——请使用 [cosign](https://docs.sigstore.dev/cosign/overview/) 进行验证。  
- **破坏性变更（代理）**：`callback_settings` 现可通过虚拟密钥（`config.yaml`）按名称引用，实现无需硬编码密钥即可动态注入凭证。详见 PR [#49995](https://github.com/BerriAI/litellm/pull/49995)。  
- **新功能**：内部用户现支持 `soft_budget`，在支出触及硬限制前于阈值触发告警（通过 Slack/邮件/ webhook）。PR [#49975](https://github.com/BerriAI/litellm/pull/49975)。

---

### **3. 新模型与硬件支持**  
- **Cloudflare Workers AI**：通过 `cloudflare` 提供商新增对音频和图像生成的支持（PRs [#35055](https://github.com/BerriAI/litellm/pull/35055), [#35056](https://github.com/BerriAI/litellm/pull/35056)）。  
- **Bedrock Converse Claude**：改进对 `output_config.effort` 的处理及结构化输出路由（PR [#49990](https://github.com/BerriAI/litellm/pull/49990)）。  
- **Google GenAI**：`generate_content` 接口现在保留客户端请求头的转发（PR [#49972](https://github.com/BerriAI/litellm/pull/49972)）。

---

### **4. 性能与优化**  
- **流式 TTS**：文本转语音现可通过 `/v1/audio/speech` 实时流式传输音频（PR [#46222](https://github.com/BerriAI/litellm/pull/46222)），对长片段（如 ElevenLabs）初始播放延迟降低高达约 65%。  
- **令牌预算控制**：v3 速率限制器现在更准确地跟踪上游 429 响应下的 TPM 使用情况；修复待合并于 PR [#49675](https://github.com/BerriAI/litellm/pull/49675)。  
- **Admin UI 构建**：Docker 构建中现使用预构建的 UI 资产，使 CI 时间减少约 40%（PR [#29092](https://github.com/BerriAI/litellm/pull/29092)）。

---

### **5. 稳定性与回归问题**  
- **严重**：主节点故障转移后，由于非连接错误（如 `READONLY`），Redis 电路断路器会无限期保持 `HALF_OPEN` 状态。修复 PR 正在进行中：[#49884](https://github.com/BerriAI/litellm/pull/49884)。  
- **高严重性**：OpenAI 的 `flex_unavailable` 错误返回 500 而非 429，导致回退逻辑失效。已在 PR [#49736](https://github.com/BerriAI/litellm/pull/49736) 中修复。  
- **中等严重性**：若未安装 `tzdata-legacy`，在使用非标准时区（如 `TZ=Asia/Calcutta`）时，代理启动将崩溃（Ubuntu 24.04+）。修复 PR：[#49813](https://github.com/BerriAI/litellm/pull/49813)。  
- **轻微**：HuggingFace embeddings 忽略 `timeout` 参数（PR [#47299](https://github.com/BerriAI/litellm/pull/47299)）；已在 `main` 分支修复。

---

### **6. 对应用开发者的意义**  
- **增强可观测性与控制力**：利用 `soft_budget` 和按密钥配置的 `callback_settings`，可在不重新部署的情况下强制执行成本治理并追踪日志。  
- **更优的流式用户体验**：借助实时 TTS 流式传输及改进的错误处理（如 429 与 500 区分），构建更具弹性的智能体工作流。  
- **规避陷阱**：在生产环境中使用非标准时区时，请确保已安装 `tzdata-legacy`。  
- **面向未来的准备**：为即将支持国际化（i18n）的 UI 组件做准备（PRs [#49991](https://github.com/BerriAI/litellm/pull/49991), [#49994](https://github.com/BerriAI/litellm/pull/49994) 等）——文本将很快可翻译。  

> **可操作建议**：升级至 `v1.109.0-rc.1`，并在正式发布前针对新的 `callback_settings` 与 `soft_budget` 模式验证你的代理配置。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

### **Unsloth Digest — 2026-11-04**

---

#### **1. 今日亮点**  
v0.1.914-beta 版本引入了 *Glass 主题*，增强了个性化功能（背景、欢迎语），并提升了多 GPU 与 CPU 训练支持下的生成速度。关键性能优化包括：FastModel 变体的 CUDA 图解码（T4 上提速 3–7 倍）以及 FP8 LoRA 集成。与此同时，UI 稳定性仍是重点——聊天持久化、模型加载和 AppImage 启动中的严重回归问题正在积极排查。

---

#### **2. 发布与破坏性变更**  
- **v0.1.914-beta** ([发布说明](https://github.com/unslothai/unsloth/releases/tag/v0.1.914-beta))  
  - 新增 Glass 主题及可自定义的聊天背景/欢迎语。  
  - 引入 CPU 训练，并扩展了多 GPU 训练选项。  
  - 生成延迟与模型加载性能显著提升。  
  - *注意：* macOS 与 Linux AppImage 用户报告更新失败；建议强制重启或清除缓存。

---

#### **3. 新模型与硬件支持**  
- **新后端支持：**  
  - ROCm 支持现已通过 `PR #16609` ([#16609](https://github.com/unslothai/unsloth/pull/16609)) 明确测试并验证，确保在 AMD gfx1151（Strix Halo）及 Windows ROCm 系统上的稳定运行。  
  - Apple MLX 支持已在 macOS 26.6.2 上确认 ([Issue #16561](https://github.com/unslothai/unsloth/issues/16561))。  
- **新模型支持：**  
  - Kimi-3.1-pro 虽然在其他地方可用，但在 Model Hub Connect 中缺失 ([Issue #16586](https://github.com/unslothai/unsloth/issues/16586))；提示后端同步问题。  
  - Qwen3-VL、Llama 4、Mistral 3、Granite、Phi-4、OLMo 2、Cohere 2、GLM-4 以及 Qwen3-MoE 现已受益于 CUDA 图解码 ([PR #16587](https://github.com/unslothai/unsloth/pull/16587))。

---

#### **4. 性能与优化**  
- 在 T4 GPU 上，通过为 FastModel 模型（Gemma 3、Qwen3-VL、Llama 4 等）启用 CUDA 图编译，实现 3–7 倍解码加速——比特级等效于急切模式 ([PR #16587](https://github.com/unslothai/unsloth/pull/16587))。  
- **FP8 LoRA** 实现对冻结的 16 位基础层进行 fp8 前向计算与输入梯度计算，同时保存/合并时仍保持精确精度 ([PR #16423](https://github.com/unslothai/unsloth/pull/16423))。  
- **LoRA 折叠**：16 位 + LoRA 模型在长序列生成期间临时将 LoRA 权重折叠进基础权重，实现 **1.3–1.4 倍** 的解码加速 ([PR #16576](https://github.com/unslothai/unsloth/pull/16576))。  
- **内存优化**：`adapter_config.json` 现在仅在每次推理工作线程加载时读取一次 ([PR #16604](https://github.com/unslothai/unsloth/pull/16604))，降低 I/O 开销。

---

#### **5. 稳定性与回归问题**  
| 问题 | 严重程度 | 状态 | 链接 |  
|------|----------|--------|------|  
| Slackware64 上因 GLib/AppIndicator 符号查找错误导致 AppImage 无法启动 | 严重 | 开放 | [#16605](https://github.com/unslothai/unsloth/issues/16605) |  
| 长对话中出现 UI 卡顿（网页版 + 桌面端） | 高 | 开放 | [#16603](https://github.com/unslothai/unsloth/issues/16603) |  
| AppImage 版 Unsloth Studio UI 极其卡顿 | 中 | 开放 | [#15999](https://github.com/unslothai/unsloth/issues/15999) |  
| 空闲自动卸载功能失效 | 中 | 已关闭 | [#16484](https://github.com/unslothai/unsloth/issues/16484) |  
| 加载模型时静默取消正在进行的聊天 | 高 | 已关闭 | [#16479](https://github.com/unslothai/unsloth/issues/16479) |  
| `search_conversation` 工具再次消失（第二次） | 中 | 开放 | [#16602](https://github.com/unslothai/unsloth/issues/16602) |  

> ✅ *修复进行中*：针对模型加载冲突（`#16581`, `#16479`）和背景图像持久化（`#16574`）的 PR 正在审核中。

---

#### **6. 对应用开发者的意义**  
- **构建更高性能的智能体**：利用 CUDA 图与 FP8 LoRA，实现 3–7 倍更快推理，且不牺牲模型保真度——非常适合实时智能体工作流。  
- **实现跨平台部署**：借助改进的 ROCm 与 MLX 支持，您的应用可在 AMD 与 Apple Silicon 硬件上高效运行。  
- **避免静默状态丢失**：加载/卸载模型时需谨慎——当前行为会静默取消正在进行的生成；请显式添加 `force_cancel_active` 检查。  
- **为长时间会话设计**：长对话触发 UI 卡顿——优化状态管理，或尽早使用上下文压缩策略。  
- **扩展 Flows 流水线**：新推出的 Flows 节点编辑器（如 PR `#14730`, `#15604`）支持声明式 AI 流水线、基准测试与智能体编排——为应用逻辑提供未来兼容性保障。  

---  
*摘要生成时间：2026-11-04 | 来源：[unslothai/unsloth GitHub](https://github.com/unslothai/unsloth)*

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*