# AI 基础设施日报 2026-11-07

> 生成时间: 2026-11-07 02:20 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI基础设施生态报告 – 2026-11-07**

---

### **1. 生态概览**  
AI推理与服务生态正进入高度专业化阶段，各项目在硬件、模型架构和部署模式上呈现明显分化。vLLM 和 SGLang 正在推动下一代GPU（AMD MI355X、NVIDIA SM120）上的高吞吐、低延迟推理边界，而 llama.cpp 与 Ollama 则强调跨平台可移植性与本地运行效率。LiteLLM 已确立为多提供商编排的事实网关，支持在多样化后端间实现代理工作流。DSA（动态稀疏注意力）、FP8 MLA、MoE分片及解耦预填充/解码模式的出现，标志着生态正从粗放式扩展转向智能化资源调度。

---

### **2. 活动对比**

| 项目       | 近24小时开放问题数 | 近24小时合并PR数 | 发布状态         |
|------------|--------------------|------------------|------------------|
| **vLLM**   | 6                  | 8                | 无               |
| **SGLang** | 9                  | 7                | 无               |
| **llama.cpp** | 5               | 6                | b12180（小版本） |
| **Ollama** | 7                  | 1                | 无               |
| **LiteLLM**| 5                  | 5                | v1.108.2（LTS） ✅ |
| **Unsloth**| 6                  | 5                | v0.1.916-beta（测试版） |

> 🔍 *洞察*：LiteLLM 在稳定性驱动的发布上领先，而 vLLM 与 SGLang 在开发活跃度上占优。尽管存在关键稳定性问题，Ollama 的PR合并量却显著下降——表明可能存在评审瓶颈或贡献者参与度不足。

---

### **3. 模型支持竞赛**

| 新模型 / 架构                 | 支持项目                         | 关键差异化特性 |
|-------------------------------|-----------------------------------|----------------|
| **Kimi-K3.1 / Kimi-K3.1-Pro** | SGLang ✅, llama.cpp ✅, Unsloth ✅  | SGLang：完整MoE分片 + HiSparse；llama.cpp：基于DSA的注意力；Unsloth：视频续接支持 |
| **DeepSeek-V4/V4.1**          | vLLM ✅, SGLang ✅, Ollama ✅       | vLLM：ROCm优化 + MegaMoE局部性；SGLang：PD解耦 |
| **NVIDIA Nemotron 3.5 ASR 流式传输** | vLLM ✅（功能请求）         | 唯一明确支持ASR流式传输的项目 |
| **GLM-5.3-Flash / GLM-5.3**   | SGLang ✅, Ollama ✅               | SGLang：推理错误漏洞；Ollama：MLX后端实验性支持 |
| **Cosmos3-Nano 视频续接**     | Unsloth ✅                        | 独家支持；可在帧边界处扩展视频 |
| **Ascend NPU 扩散模型**       | SGLang（RFC）✅                   | 提议集成MindIE-SD —— 仅SGLang扩散路线图中独有 |

> 🏆 **排行榜**：**SGLang** 在架构创新（MoE分片、HiCache、DFlash）上领先，紧随其后的是 **vLLM** 在硬件特化优化方面的表现。**llama.cpp** 在开源模型可用性与GPU可移植性方面表现卓越。

---

### **4. 性能前沿**

| 优化方向              | 领先项目                     | 关键进展 |
|------------------------|-------------------------------|----------|
| **KV缓存与内存带宽**   | vLLM ✅, SGLang ✅, llama.cpp ✅ | vLLM：FP8→BF16临时转换范围限定（+10.1%）；SGLang：HiCache池控制；llama.cpp：ClassLRU MoE缓存 |
| **推测解码**           | vLLM ✅, SGLang ✅             | vLLM：`load_plan` API抽象；SGLang：侧流上下文投影 + 单核验证 |
| **量化与精度**         | vLLM ✅, llama.cpp ✅, LiteLLM ✅ | vLLM：gfx1250上启用FP8 MLA；llama.cpp：小规模GEMV变体；LiteLLM：每服务器级MCP缓存策略 |
| **分布式服务**         | vLLM ✅, SGLang ✅             | vLLM：权重直接重载加固；SGLang：PD解耦、流水线并行预填充 |
| **内核级优化**         | vLLM ✅, SGLang ✅, llama.cpp ✅ | vLLM：FlashInfer预填充；SGLang：radix-4路由融合；llama.cpp：Vulkan CoopMat支持 |

> 💡 **趋势**：性能前沿已从原始吞吐转向**上下文感知、内存高效执行**——尤其针对长上下文与多模态模型。

---

### **5. 层级定位**

| 项目       | 主要层级               | 角色概述 |
|------------|------------------------|---------|
| **vLLM**   | **服务引擎**           | 高性能、CUDA优化的推理引擎；面向云规模部署 |
| **SGLang** | **服务引擎 + 网关**    | 专用于推测解码与代理工作流；通过统一API支持多种后端 |
| **llama.cpp** | **本地运行时 / 边缘引擎** | 原生CPU/GPU推理，依赖极低；适用于离线/本地部署 |
| **Ollama** | **网关 + 本地运行时**   | 用户友好的命令行/API层；通过轻量级服务桥接模型与应用；注重开发者体验 |
| **LiteLLM** | **API网关 / 编排层**   | 多提供商路由、成本控制、结构化输出处理；作为代理的中间件 |
| **Unsloth** | **代理UI + 运行平台**   | 代理与多模态工作流的全栈工具链；包含工作室、插件与可视化功能 |

> 🧩 **战略洞察**：技术栈已清晰分化：**引擎层**（vLLM/SGLang）、**运行时层**（llama.cpp/Ollama）、**编排层**（LiteLLM）、**代理用户体验层**（Unsloth）。

---

### **6. 趋势信号**

- **硬件特化加速演进**：AMD MI355X 与 Intel Arc B70 支持已成为关键差异点——vLLM 与 SGLang 领先，但 llama.cpp 与 Ollama 正快速追赶。
- **MoE 与解耦成为主流**：vLLM 与 SGLang 在 MoE 分片、TP/DP 分解及预填充/解码分离方面取得显著进展——对大模型扩展至关重要。
- **代理工作流需要集成工具**：Unsloth 的 Claude Agent SDK、LiteLLM 的结构化输出修复、SGLang 的推理解析器改进，均表明**工具调用可靠性**已成为核心要求。
- **生产环境优先考虑稳定性**：尽管功能快速迭代，所有项目均报告严重回归（内存损坏、崩溃、静默数据丢失）。开发者必须优先采用 **LTS 版本**、**可观测性工具** 与 **本地测试**。
- **安全与合规日益重要**：LiteLLM 现已支持提示词脱敏、令牌使用掩码与无会话成本追踪——反映出对审计能力的强烈需求。

> 🚨 **开发者行动建议**：
> - 立即使用 **v1.108.2（LiteLLM LTS）** 保障代理稳定性。
> - 在修复前避免对多模态模型使用 `OLLAMA_NUM_PARALLEL`。
> - 监控 `--hicache-mamba-size-gb` 与 `VLLM_RELOAD_MODE=direct` 以防止静默失败。
> - 在大规模部署新模型前，利用 **基准测试工具**（Unsloth、vLLM SIG Roadmaps）进行验证。

---

✅ **最终结论**：AI基础设施格局已不再关乎“选最快的引擎”——而是选择一个**分层、稳健且可观测的系统**，精准匹配你的部署场景：云规模（vLLM/SGLang）、边缘本地（llama.cpp/Ollama）、代理原生（Unsloth）、或多云环境（LiteLLM）。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM Digest – 2026-11-07**

---

### **1. 今日亮点**  
vLLM 项目持续加速对下一代硬件及推测性解码工作流的支持，关键 PR 已落地：**DeepSeek-V4/V4.1 的 ROCm 优化**、**gfx1250 上 Kimi-K3 FP8 MLA 支持**，以及**直接权重重载的健壮性增强**。针对量化 MoE 模型中的内存安全问题和 FP8 KV 缓存处理的关键修复已到位，同时 Rust 前端与 `load_plan` API 的进展表明引擎抽象正在不断深化。

---

### **2. 发布与破坏性变更**  
无。过去 24 小时内未发布新版本或破坏性变更。

---

### **3. 新模型与硬件支持**  
- ✅ **DeepSeek-V4/V4.1** 现在通过 `stacked context WKV 投影` ([PR #66212](https://github.com/vllm-project/vllm/pull/66212)) 获得 ROCm 特定性能优化，并支持 **MegaMoE 权重的可选局部性放置** ([PR #64835](https://github.com/vllm-project/vllm/pull/64835))。  
- ✅ **Kimi-K3 (FP8 MLA)** 在 **gfx1250 (AMD MI355X)** 上获得完整支持，配备专用预填充/解码内核 ([PR #66208](https://github.com/vllm-project/vllm/pull/66208))。  
- ✅ **NVIDIA Nemotron 3.5 ASR 流式处理 (0.6b)** 作为功能请求被支持 ([Issue #47455](https://github.com/vllm-project/vllm/issues/47455))，实现多语言流式语音识别。  
- ✅ **MiMo-V2 omni** 视频提示标签已修正为反映真实帧时间戳 ([PR #66201](https://github.com/vllm-project/vllm/pull/66201))。

---

### **4. 性能与优化**  
- 🔥 **原生 FlashInfer 预填充** 通过限定范围的 FP8 → BF16 临时转换优化，在 SM120 上将输入 token 吞吐量提升最高达 **10.1%** ([PR #64942](https://github.com/vllm-project/vllm/pull/64942))。  
- 🚀 **MoE 性能提升**：TRT-LLM 单体 MoE 内核现利用 vLLM 的 top-k 路由跳过 DP 填充行，减少不必要的计算 ([PR #65972](https://github.com/vllm-project/vllm/pull/65972))。  
- ⚙️ **编码器 CUDA 图** 经优化，每个批次使用单一重放并采用更细粒度的预算分配，避免资源超分配与延迟突增 ([PR #65227](https://github.com/vllm-project/vllm/pull/65227))。  
- 📈 **ReplaySSM** 仍处于 RFC 阶段 ([Issue #47572](https://github.com/vllm-project/vllm/issues/47572))，目标是降低 Mamba2/GDN 模型中解码内核的内存带宽压力。

---

### **5. 稳定性与回归问题**  
- ❌ **严重内存损坏**：在 sm_86 上使用 PP=3 的 `Qwen3.8-27B-GDN` 出现 **非法内存访问（Xid 31）**，源于自 v0.28.0 起的前缀缓存回归问题 —— 在 v0.31.0 与 v0.32.0 中已确认存在 ([Issue #63505](https://github.com/vllm-project/vllm/issues/63505))。*暂无修复 PR。*  
- ❌ **MoE 输出非确定性**：Marlin MoE 输出依赖专家内部 token 排序，严重影响可复现性，属严重正确性问题 ([Issue #52525](https://github.com/vllm-project/vllm/issues/52525))。*PR 待提交。*  
- ⚠️ **TP 工作线程卡死**：使用 DeepSeek-V4-Pro 与推测解码的 TP=8 配置时，采样阶段因 RPC 超时引发 `EngineDeadError` ([Issue #41530](https://github.com/vllm-project/vllm/issues/41530))。*高危；暂无修复 PR。*  
- 🛠 **今日已合并的修复**：  
  - 修复了在 TP 分片过程中 FP4 缩放组处理错误的问题 ([PR #66213](https://github.com/vllm-project/vllm/pull/66213))。  
  - 修正了激活路径中反量化身份类型不匹配的问题 ([PR #66209](https://github.com/vllm-project/vllm/pull/66209))。  
  - 修复了 NCCL/IPC 路径中打包权重传输错位的问题 ([PR #65487](https://github.com/vllm-project/vllm/pull/65487))。

---

### **6. 对应用开发者的启示**  
- 若你在 AMD GPU 上部署 **Kimi-K3**、**DeepSeek-V4** 或 **GLM-5.3**，请确保使用包含 ROCm 补丁的最新构建版本（[PR #66212](https://github.com/vllm-project/vllm/pull/66212)，[PR #64835](https://github.com/vllm-project/vllm/pull/64835)），以避免性能断崖。  
- 对于大规模推测解码，避免使用会触发 `Qwen3.8-27B` 非法内存访问的 `--max-model-len` 值，直至修复上线。  
- 使用 `VLLM_RELOAD_MODE=direct` 时需谨慎 —— 最近的 PR 强制要求显式启用以保障安全重载（[PR #66002](https://github.com/vllm-project/vllm/pull/66002)），防止无声模型损坏。  
- 可考虑在 Hopper/Blackwell 架构上启用 `enable_batch_invariant_mode`，但请注意当前会不必要地将 Linear 层路由至 Triton（[Issue #65569](https://github.com/vllm-project/vllm/issues/65569)）。  

> 🔗 **专业提示**：关注 [vLLM SIG Roadmaps](https://github.com/vllm-project/vllm/issues/61024) 以了解 2026 年第四季度关于 Rust 前端对齐、load plan 标准化及 BYOG 基准测试的规划。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-11-07

---

### **1. 今日亮点**  
SGLang 项目持续加速硬件与模型的专用化进程，针对 Kimi-K3.1 在 AMD 与 Blackwell GPU 上实现重大性能优化，包括 MoE 层分片和融合内核。关键的 CI 稳定性问题——特别是不稳定的测试用例和 CUDA 核心转储（coredump）——正在积极追踪中，而新提交的 PR 主要聚焦于推测解码、扩散推理以及跨平台支持（XPU、Apple Silicon）的优化。

---

### **2. 发布与破坏性变更**  
过去 24 小时内未报告任何发布或破坏性变更。

---

### **3. 新模型与硬件支持**  
- **Kimi-K3.1**：通过 [PR #48414](https://github.com/sgl-project/sglang/pull/48414) 实现完整模型支持，包含 DFlash 推测解码、PD 分离、HiSparse 以及流水线并行预填充。  
- **AMD (MI355X/MI45x)**：通过 [PR #43126](https://github.com/sgl-project/sglang/pull/43126) 和 [PR #44362](https://github.com/sgl-project/sglang/pull/44362) 提升 Kimi-K3.1 性能，支持 FP8 KDA 内部吸收及 radix-4 路由器融合。  
- **Intel XPU (Battlemage)**：[PR #48787](https://github.com/sgl-project/sglang/pull/48787) 通过 `sgl_kernel.fp8_linear` 实现仅权重的 W8A16 FP8 执行，带来更优性能。  
- **Apple Silicon (MLX)**：[RFC #32321](https://github.com/sgl-project/sglang/issues/32321) 提出重构方案，以支持由 Torch 拥有的 SRT 路径，并导出 MLX 模型区域。  
- **Ascend NPU**：[RFC #48582](https://github.com/sgl-project/sglang/issues/48582) 建议将 MindIE-SD 作为可选加速库集成至 SGLang Diffusion。

---

### **4. 性能与优化**  
- **Kimi-K3.1 (AMD)**：MoE 前/后端分片降低内存带宽压力；[PR #48584](https://github.com/sgl-project/sglang/pull/48584) 显示通过避免跨 TP rank 的冗余数据读取，最高实现 **~1.5倍加速**。  
- **DeepSeek-V4 (SM120)**：在 4× RTX PRO 6000 上吞吐量仍不理想（约 2–7K tok/s，对比 vLLM/Marlin 的 ~12.5K），[Issue #33422](https://github.com/sgl-project/sglang/issues/33422) 强调需对 SM120 内核进行优化。  
- **推测解码**：多个 PR 优化关键路径：  
  - [PR #48948](https://github.com/sgl-project/sglang/pull/48948)：侧流上下文投影 + 单内核验证提交，降低延迟。  
  - [PR #48949](https://github.com/sgl-project/sglang/pull/48949)：词汇分块最终令牌遍历提升小批量效率。  
- **扩散推理**：[PR #48903](https://github.com/sgl-project/sglang/pull/48903) 修复 `kandinsky6-sr` 解码并行测试中的浮点漂移问题，确保可复现性。  
- **HiCache**：[PR #47867](https://github.com/sgl-project/sglang/pull/47867) 引入 `--hicache-mamba-size-gb` 用于固定 Mamba 主机池大小，防止因池过小导致缓存未命中。

---

### **5. 稳定性与回归问题**  
- **CUDA 核心转储追踪 (#26340)**：356 条评论；自动从 `pr-test.yml` 收集。高严重性问题影响 CI 稳定性——尚未修复。  
- **不稳定 CI 测试 (#42752)**：186 条评论；在 NVIDIA `PR Test Base/Extra` 运行中，于看护工作流期间反复失败。  
- **严重缺陷**：  
  - GLM-5.3-Flash 在多工具提示下推理输出退化为重复的 `!` ([#36669](https://github.com/sgl-project/sglang/issues/36669))。  
  - `/v1/chat/completions` 经模型网关传递时静默丢弃 `cache_salt` 字段 ([#48793](https://github.com/sgl-project/sglang/issues/48793))。  
  - 推理解析器错误地在引号包裹的 `</think>` 标签处拆分，截断 CoT 内容 ([#48791](https://github.com/sgl-project/sglang/issues/48791))。  
- **扩散套件回归**：`multimodal-gen-test-2-gpu` 因测试引入后张量不匹配而失败 ([#48892](https://github.com/sgl-project/sglang/issues/48892)) —— 已在 [PR #48903](https://github.com/sgl-project/sglang/pull/48903) 中修复。

---

### **6. 对应用开发者的启示**  
- **近期优化后，Kimi-K3.1 与 DeepSeek-V4 可预期更高吞吐量**，但请留意 SM120 的性能差距。  
- 使用 `--hicache-mamba-size-gb` 避免混合模型中无声的 HiCache 未命中。  
- 若通过模型网关路由，请勿在 `/v1/chat/completions` 中使用 `cache_salt`——该字段将被丢弃。  
- 对于使用复杂工具调用的代理系统，由于已知推理损坏问题，务必谨慎测试 GLM-5.3-Flash。  
- 启用 `--enable-layernorm-sp` 并使用 `--layernorm-sp-min-tokens` 以优化大模型的扩展阶段。  
- 关注 CI 健康状况：预计在 PR 审查中出现不稳定性；建议提交前运行本地测试。

> 🔗 *获取实时更新，请关注：[SGLang GitHub Issues](https://github.com/sgl-project/sglang/issues)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-11-07**

---

### **1. 今日亮点**  
最新更新聚焦于扩展对 Intel Arc GPU（Vulkan/CoopMat）的硬件支持，引入全新 **Kimi-K3.1-Pro** 模型并采用基于 DSA 的注意力机制，同时通过 `llama-server` 的 `--prefill-budget-busy` 参数和预填充/解码分离，进一步推进去中心化推理能力。针对 AMD RDNA4（gfx1201）及 OpenCL/MoE 量化方案的关键性能提升已合并，稳定性修复则解决了 UTF-8 处理与 GPU 内存管理中的关键崩溃问题。

---

### **2. 发布与破坏性变更**  
今日未宣布任何破坏性变更或主版本发布。但 **b12180** 版本引入了新模型（`Kimi-K3.1-Pro`）并包含多项后端优化。`--cache-disk` 功能请求（#20697）仍处于关闭状态，但正在积极讨论中——预计短期内不会有 API 变更。

> 🔗 [GitHub 发布版 b12180](https://github.com/ggml-org/llama.cpp/releases/tag/b12180)

---

### **3. 新模型与硬件支持**  
- ✅ **新模型**：新增 `Kimi-K3.1-Pro`，采用 **DSA（动态稀疏注意力）** 替代原有的全量注意力 MLA 层；支持 top-k 掩码与 32 头 × 128 上下文长度。
  > 🔗 [PR #31779: 增加 Kimi-K3.1-Pro](https://github.com/ggml-org/llama.cpp/pull/31779)
- 🚀 **硬件**：通过 Vulkan/ROCm 增强对 **Intel Arc Pro B70（BMG G31）** 与 **AMD Strix Halo（gfx1151/gfx1201）** 的支持。
  - Vulkan 现已检测 ReBAR 支持，避免因 BAR 分配失败导致的问题（#31658）。
  - ROCm/HIP 构建现在使用固定 32MB BLAS 工作区，而非按调用分配（#31838）。
- 🖥️ **后端**：OpenCL 现在支持 `f16`、`i32`、`i16` 与 `bf16` 类型的 `repeat` 操作（#31782）。WebGPU 新增批处理张量接口（`get_tensor_n` / `set_tensor_n`）（#31473）。

---

### **4. 性能与优化**  
- ⚡ **Flash Attention（FA）**：针对 Vulkan 平台上的预填充优化了 Intel FA 内核（#29357）；未来可通过 `readonly` 缓冲区声明实现进一步提升（#31833）。
- 💾 **MoE 缓存**：引入混合 LRFU/MQ 风格缓存机制（`ClassLRU`）（#31233），显著提升大型 MoE 模型下的专家路由效率。
- 📈 **量化与推测验证**：为 `q8_0`、`q4_k` 与 `q5_1` 添加小规模 GEMV 变体，以提高推测解码精度（#31831）。
- 🧠 **CPU 优化**：CPU Flash Attention 现使用 KleidiAI SME2 内核（#29394）；克罗内克积支持范围扩展至非 2 的幂次维度（#28490）。
- 🛠️ **图优化**：通过层剪枝与提示词标记消除，Gemma 4 E2B/E4B 模型在预填充阶段获得巨大速度提升（#31356）。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 状态 | 修复 PR？ | 说明 |
|------|----------|--------|---------|-------|
| [#31560](https://github.com/ggml-org/llama.cpp/issues/31560) | 严重 | 开放 | ❌ | Intel Arc B70：KHR_coopmat 导致非确定性结果（F16/Q8_0） |
| [#27638](https://github.com/ggml-org/llama.cpp/issues/27638) | 高 | 开放 | ❌ | Vulkan ANV：Flash Attention 回退 → O(N²) PP 性能下降 + 设备丢失 |
| [#31538](https://github.com/ggml-org/llama.cpp/issues/31538) | 中等 | 开放 | ❌ | iGPU：CPU 保留层通过 Vulkan 主机缓冲区占用设备内存 |
| [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) | 高 | 开放 | ❌ | SYCL：`--cpu-moe -sm layer` 在 40,000 MiB MoE 缓存时卡死 |
| [#27725](https://github.com/ggml-org/llama.cpp/issues/27725) | 中等 | 开放 | ❌ | 评估阶段内存泄漏（CUDA） |

> 🔍 *注意：多个回归问题与近期 Flash Attention 及 MoE 优化相关——建议生产部署前仔细测试相关套件。*

---

### **6. 对应用开发者的启示**  
- **去中心化推理已具备可行性**：随着 `server : disaggregated prefill/decode support`（#21266）逐步落地，开发者现可构建可扩展、低延迟的代理系统，使用独立的预填充与解码节点。
- **混合缓存策略需谨慎使用**：`--cache-ram` + `--cache-idle-slots` 当前在并发负载下存在竞争条件（#27148）——建议暂禁用直至修复上线。
- **模型选择至关重要**：Kimi-K3.1-Pro 的 DSA 架构支持更快的长上下文推理，非常适合高吞吐量 RAG 流水线。
- **避免不稳定的后端**：请勿在 Intel Arc B70 上使用 `KHR_coopmat`（问题 #31560）；建议使用 `--no-coopmat` 直至驱动修复。
- **为 MoE 优化做好准备**：谨慎使用 `ClassLRU` 缓存与 `--split-mode layer`；多 GPU 场景下避免 `--cpu-moe`，因其会导致严重性能下降（#31510）。

> 📌 **实用建议**：对于实时应用，启用 `--prefill-budget-busy`（#30992）以防止高负载预填充突发期间出现空闲 GPU 周期。

---  
*简报数据源自 GitHub：[ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# **Ollama Digest – 2026-11-07**

---

### **1. 今日重点**  
今日发现影响使用 AMD 与 Intel GPU 的 Vulkan 后端用户的严重稳定性问题，`llama-server` 在加载模型时因未处理的 C++ 异常（如 `NTSTATUS 0xe06d7363`、`device lost`）崩溃。同时，CUDA 13 支持中出现回归，导致长时间语法约束生成时发生“未指定启动失败”崩溃。此外，已合并一项高优先级修复，防止 `keep_alive` 到期后运行器未能终止引发的 VRAM 泄漏。

---

### **2. 发布与破坏性变更**  
过去 24 小时内无报告。

---

### **3. 新模型与硬件支持**  
- **MLX 后端**：新增对密集型 Nemotron/Luciole Safetensors 模型的实验性支持（`#19566`）——但当前 MLX 运行器尚不识别 `NemotronForCausalLM` 架构。  
- **CUDA 13.5 + SM 103**：ARM64 支持范围扩大，通过 `#19580` 新增 CUDA 13.5 及计算能力 103（SM 103）。  
- **Vulkan 后端**：持续优化 Vulkan 在 AMD Renoir（gfx90c）、Intel Arc 130V（Lunar Lake）及 RX 5500（gfx1010）设备上的稳定性——详见 `#19558`、`#19589`、`#19158`。

---

### **4. 性能与优化**  
- **MLX 并发支持**：PR `#19197` 通过 `OLLAMA_NUM_PARALLEL` 实现对 MLX 模型的并发生成，减少请求间空闲时间，降低首个 token 延迟。  
- **内存效率优化**：PR `#18078` 优化了 MLX 上的 Qwen3.8 Flash Next，关键层保留 BF16 精度，其余部分采用 MXFP8 量化，在质量与内存占用之间取得平衡。  
- **令牌流式输出**：修复 DeepSeek3 解析器中 `</think>` 后空白字符处理问题（`#19580`），改善流式响应输出格式一致性。

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 影响 | 状态 | PR / 链接 |
|--------|------|--------|--------|----------|
| 🔴 严重 | 在 AMD Renoir iGPU（Ryzen 4700U）上，`llama-server` 因 `NTSTATUS 0xe06d7363` 崩溃 | 所有模型加载失败；影响 Vulkan 后端 | 开放 | [Issue #19558](https://github.com/ollama/ollama/issues/19558) |
| 🔴 严重 | `gemma4:12b` 在 Intel Arc 130V（Lunar Lake）iGPU 上因 `ggml_vulkan: device lost` 崩溃 | 模型上传后 GPU 加载失败；自 0.40.x 版本起出现回归 | 开放 | [Issue #19158](https://github.com/ollama/ollama/issues/19158) |
| 🔴 严重 | `cuda_v13`：长时间语法约束生成触发“CUDA 错误：未指定启动失败” → Xid 79（GPU 失联） | 高风险崩溃，持续负载下易发生 | 开放 | [Issue #17026](https://github.com/ollama/ollama/issues/17026) |
| 🟡 高 | `ollama serve` 在 `keep_alive` 到期后无法终止空闲的 `llama-server` 运行器 | 引发 VRAM 泄漏，后续加载时导致内存不足错误 | 已修复 | [PR #19581](https://github.com/ollama/ollama/pull/19581) |
| 🟡 中等 | `OLLAMA_NUM_PARALLEL` 导致多模态模型（如 `llava`、`moondream`）图像处理失败 | 并发场景下请求挂起或冻结 | 开放 | [Issue #4165](https://github.com/ollama/ollama/issues/4165) |
| 🟡 低 | 在丢弃 X25519MLKEM768 ClientHello 的网络中，TLS 握手超时 | 在注重隐私或受限网络中阻塞 `ollama pull` | 开放 | [Issue #19430](https://github.com/ollama/ollama/issues/19430) |

---

### **6. 对应用开发者的启示**  
- 若在 NVIDIA GPU 上执行长文本、语法约束生成任务，请避免使用 CUDA 13.5，改用 `cuda_v12` 作为回退方案，直至 `#17026` 修复。  
- 在 `#4165` 修复前，请勿在多模态模型中启用 `OLLAMA_NUM_PARALLEL` —— 可能导致无声挂起或资源耗尽。  
- 在 AMD/Intel 集成显卡（Renoir、Lunar Lake）上需密切监控 GPU 内存使用情况 —— Vulkan 后端不稳定可能导致意外进程崩溃和持久性 VRAM 锁定。  
- **尽早集成可观测性工具**：新社区集成如 **HomeLab Monitor**（`#19586`）与 **AI Meter**（`#19582`）可实时监控模型加载、VRAM 使用与令牌吞吐量，适合生产环境监控。  
- **预期 macOS 上存在临时 API 问题**：长时间回复后 `/api/chat` 间歇性返回“管道损坏”错误（`#19293`）——建议实现带抖动的重试逻辑。  

> ✅ **可操作建议**：在 `#6987` 提出的 `ollama stop all` 功能正式实现前，可临时使用该命令更可靠地管理模型状态。进展请通过上方 GitHub 问题链接追踪。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM 简报 – 2026-11-07**

---

### **1. 今日亮点**  
最新 LTS 版本 **v1.108.2** 在 Anthropic、Gemini 与 Bedrock 后端上稳定了关键的流式处理与工具调用行为。关键修复包括 `server_side_tool_invocations` 中的数据损坏问题、Claude 模型上 `reasoning_effort` 的异常表现，以及一个高危缺陷：在 Azure 到 Responses 的路由过程中，加密推理内容被意外丢弃。这些改进对代理可靠性与结构化输出准确性至关重要。

---

### **2. 发布与破坏性变更**  
- **v1.108.2 (LTS)**：作为新的稳定基线发布。请使用 `ghcr.io/berriai/litellm:lts` 保持更新。  
  🔗 [发布说明](https://github.com/BerriAI/litellm/releases/tag/v1.108.2)  
- **回滚修复**：#50456 修复了在 Anthropic 背后运行的 `chatgpt/*` 模型中 `/v1/messages` 接口的加密推理往返失败问题，确保端到端的状态化工具调用正常工作。  
  🔗 [PR #50456](https://github.com/BerriAI/litellm/pull/50456)

> ✅ **迁移提示**：若使用 `chatgpt/*` 模型进行工具调用与推理，请升级至 v1.108.2 或更高版本，以避免静默状态丢失。

---

### **3. 新模型与硬件支持**  
- **NVIDIA NIM**：目录刷新现已包含新发布的 NVIDIA NIM 模型（如 `nvidia/llama3-8b-instruct-nim`）。  
  🔗 [PR #50439](https://github.com/BerriAI/litellm/pull/50439)  
- **Vertex AI Claude 合作模型**：现在对 `vertex_ai/claude-*` 支持请求体中先序列化 `system` 与 `tools`，再序列化 `messages`，与上游预期一致。  
  🔗 [PR #49645](https://github.com/BerriAI/litellm/pull/49645)  
- **Cohere Embeddings**：`COHERE_API_BASE` 现在已正确应用于嵌入请求（此前被忽略）。  
  🔗 [Issue #50248](https://github.com/BerriAI/litellm/issues/50248)，[PR #50248](https://github.com/BerriAI/litellm/pull/50248)

---

### **4. 性能与优化**  
- **延迟降低**：通过在 `litellm.acompletion()` 中引入缓存，解决了 SAP GenAI Hub 部署 URL 发现开销（每次调用约 870ms）的问题。  
  🔗 [Issue #47096](https://github.com/BerriAI/litellm/issues/47096)，[PR #47096](https://github.com/BerriAI/litellm/pull/47096)  
- **内存效率**：MCP（模型上下文协议）现支持按服务器可选启用上游工具架构的内存缓存，减少无会话请求中的冗余 API 调用。  
  🔗 [PR #50493](https://github.com/BerriAI/litellm/pull/50493)  
- **流式效率**：Ollama 流式处理现在为每个工具调用分配唯一 `index`，防止 JSON 参数融合。  
  🔗 [PR #49183](https://github.com/BerriAI/litellm/pull/49183)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 状态 | PR / 修复 |
|--------|------|--------|---------|
| 🚨 高 | `stream_chunk_builder` 导致 `Gemini` 的 `server_side_tool_invocations` 被破坏 → 后续轮次出现“工具调用上下文损坏” | 已关闭 | [PR #50492](https://github.com/BerriAI/litellm/pull/50492) |
| 🚨 高 | 在 Azure 到 Responses 的绕行路径（`/azure/responses/...`）中，加密推理被丢弃 | 已关闭 | [PR #50024](https://github.com/BerriAI/litellm/pull/50024) |
| 🟡 中 | `reasoning_effort="none"` 在默认开启的 Claude 模型上未禁用思考，而是直接省略 | 已关闭 | [PR #50492](https://github.com/BerriAI/litellm/pull/50492) |
| 🟡 中 | `Bedrock Converse` 将 `reasoningText` 中的 `signature` 重播至非 Anthropic 模型 → `Bad Request` | 已关闭 | [PR #49969](https://github.com/BerriAI/litellm/pull/49969) |
| 🟡 中 | `langfuse_otel` 无法序列化 `Message` 对象 → 缺失 token 使用量与追踪数据 | 已修复 | [PR #46845](https://github.com/BerriAI/litellm/pull/46845) |

> ⚠️ **重要提示**：今日所有重大稳定性问题均已在 24 小时内通过合并或关闭的 PR 解决。请立即升级至 v1.108.2。

---

### **6. 对应用开发者的影响**  
- **代理与工具链**：确保在 Claude 模型上正确强制执行 `reasoning_effort="none"`；使用 v1.108.2 可避免静默状态保留。  
- **结构化输出可靠性**：使用 `Gemini`、`Anthropic` 或 `Ollama` 进行流式工具调用时，`index` 与 `arguments` 现在正确保留——不再出现合并的 JSON 数据包。  
- **安全与合规**：防护机制现已正确屏蔽缓冲流内容（[PR #50489](https://github.com/BerriAI/litellm/pull/50489)），且可按密钥或团队禁用提示词存储（[PR #50135](https://github.com/BerriAI/litellm/pull/50135)）。  
- **成本控制**：成本映射现已包含 `-latest` 模型别名，提升动态模型路由场景下的预算精确度。

🔧 **行动项**：将您的 LiteLLM 代理与 SDK 升级至 **v1.108.2 (LTS)**，以获得所有稳定性与功能修复。使用 `ghcr.io/berriai/litellm:lts` 确保兼容性。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-07**

---

### **1. 今日亮点**  
v0.1.916-beta 版本在 Unsloth 各方面带来了重大性能提升，包括长对话中更快的输入与搜索、加速的图像/视频加载，以及双 GPU 生成支持。UI 改进现已包含每聊天独立浏览器和浮动窗口控制。关键稳定性修复解决了玻璃主题不一致、长对话延迟，以及多智能体工作流中模型选择漂移等问题。

---

### **2. 发布与破坏性变更**  
- **v0.1.916-beta**：已发布，包含大量性能修复和新 UI 功能。  
  🔗 [GitHub Release](https://github.com/unslothai/unsloth/releases/tag/v0.1.916-beta)  
- **破坏性变更（进行中）**：`transformers` 5.23 将 `gradient_checkpointing` 重命名为 `activation_checkpointing`，影响 Unsloth 训练器逻辑行为。需上游审计。  
  🔗 [Issue #16667](https://github.com/unslothai/unsloth/issues/16667)

---

### **3. 新模型与硬件支持**  
- **Cosmos3-Nano 视频续接**：新增从最后一帧或第一帧扩展视频片段的能力。  
  🔗 [PR #16741](https://github.com/unslothai/unsloth/pull/16741)  
- **Cosmos3 256p + 帧率范围支持**：现已与 NVIDIA 官方模型卡规格对齐；包含按模型设置的帧率选择器。  
  🔗 [PR #16737](https://github.com/unslothai/unsloth/pull/16737)  
- **Claude 智能体集成（实验性）**：通过本地 CLI 登录连接 Anthropic 的 Agent SDK——无需 API 密钥。支持 Claude Pro 与 Max 套餐。  
  🔗 [PR #16963](https://github.com/unslothai/unsloth/pull/16963)  
- **Krea 2 Turbo 图像生成**：现支持 Windows 桌面端（注意：特定条件下报告存在崩溃问题）。  
  🔗 [Issue #16892](https://github.com/unslothai/unsloth/issues/16892)

---

### **4. 性能与优化**  
- **图像加载内存管理**：Studio 在图像加载后会将释放的堆内存返还给操作系统。降低主机内存压力。  
  🔗 [PR #16934](https://github.com/unslothai/unsloth/pull/16934)  
- **扩散模型自动调优缓存**：热重启时，已编译的扩散内核不再重新测试此前调优过的 Triton 简化操作（L4 上节省约 9.5 秒）。  
  🔗 [PR #16981](https://github.com/unslothai/unsloth/pull/16981)  
- **RAG 数据摄入瓶颈**：目前以单文档为单位嵌入（批处理大小为 1），导致每块耗时约 99ms，而批量处理仅需 2.07ms。  
  🔗 [Issue #16715](https://github.com/unslothai/unsloth/issues/16715)  
- **基准测试页面（开发中）**：提议对 GGUF 模型进行表格形式的推测解码、KV 缓存类型及内存卸载设置的扫查。  
  🔗 [PR #11808](https://github.com/unslothai/unsloth/pull/11808)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 状态 | 修复 PR |
|---------|------|--------|--------|
| 严重 | 玻璃主题导致全背景模糊（桌面端/网页端） | 已关闭 | [Issue #16820](https://github.com/unslothai/unsloth/issues/16820) |
| 高 | 长对话引发 UI 延迟（网页端 + 桌面端） | 已关闭 | [Issue #16603](https://github.com/unslothai/unsloth/issues/16603) |
| 高 | `search_conversation` 工具消失（第三次复发） | 开放 | [Issue #16706](https://github.com/unslothai/unsloth/issues/16706) |
| 高 | Unsloth Studio 后端在加载 Krea 2 Turbo 时崩溃（Windows，`0xc0000005`） | 开放 | [Issue #16892](https://github.com/unslothai/unsloth/issues/16892) |
| 中 | DiffusionGemma 多 GPU 提示词 >1792 tokens 时因 `GGML_ASSERT n_ubatch >= n_tokens` 报错中止 | 开放 | [Issue #16925](https://github.com/unslothai/unsloth/issues/16925) |
| 中 | Blender MCP 启动时因 MCP SDK 2.3.0 不兼容导致崩溃 | 已关闭 | [Issue #16878](https://github.com/unslothai/unsloth/issues/16878) |

---

### **6. 对应用开发者的影响**  
- **多模型工作流**：切换智能体时，预期模型选择持久性将有所改善。建议使用按模型扩展参数设置（提案见 [#7589](https://github.com/unslothai/unsloth/issues/7589)）实现更精细控制。  
- **UI/UX 考虑**：避免依赖 `localStorage` 存储账户级偏好——新的同步机制正在逐步推出。应改用服务端存储。  
- **智能体开发**：实验性的 Claude 智能体集成支持本地、免密部署——非常适合对隐私敏感的应用场景。  
- **性能优化**：通过尽早批量处理文档块来优化 RAG 流水线。注意长期上下文处理能力的回归风险。  
- **可扩展性**：随着拟议的插件商店 ([#10801](https://github.com/unslothai/unsloth/issues/10801)) 与 VS Code 扩展 ([#10340](https://github.com/unslothai/unsloth/issues/10340)) 上线，未来集成将更加模块化且原生适配 IDE。

> ✅ **可操作建议**：若构建智能体系统，请测试多模型状态持久性，并考虑利用即将推出的基准测试工具进行模型调优。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*