# AI 基础设施日报 2026-11-01

> 生成时间: 2026-11-01 01:26 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

### **跨项目AI基础设施生态报告 – 2026-11-01**

---

#### **1. 生态概览**  
2026年末的AI基础设施格局呈现出**高性能推理引擎**与**开发者导向部署平台**之间的显著分化，硬件后端和模型类型上的专业化趋势日益明显。vLLM、SGLang 和 llama.cpp 在大语言模型（LLMs）的底层性能优化方面处于领先地位，而 Ollama 与 LiteLLM 则聚焦于易用性及企业级API网关。Unsloth 作为全栈型选手崭露头角，将训练、微调与推理整合为统一工作流——尤其在视觉-语言模型和本地优先开发方面表现突出。生态系统正迅速成熟，分布式服务、推测解码稳定性以及跨平台兼容性成为关注重点。

---

#### **2. 活动对比**

| 项目        | 开放问题（↑） | 合并的PR（↑） | 最新发布 | 备注 |
|-------------|---------------|---------------|----------|------|
| **vLLM**    | 875 (+34)     | 98 (+12)      | `v0.30.0` | 高度关注 SM8x/ROCm 支持；FP8 图像处理存在关键回归 |
| **SGLang**  | 1,122 (+41)   | 87 (+15)      | 无       | 围绕 CUDA 稳定性和 NCCL 集成展开密集开发；351条评论的 core dump 问题 |
| **llama.cpp** | 943 (+28)   | 112 (+18)     | `b12024` | 推出推测解码相关重要修复，SYCL/OpenCL 改进显著；Intel Arc 不稳定问题持续存在 |
| **Ollama**  | 1,367 (+52)   | 76 (+11)      | 无       | 报告问题最多——尤其在 Windows/macOS 平台；内存泄漏占主导 |
| **LiteLLM** | 819 (+22)     | 68 (+10)      | 无       | 聚焦安全（密钥吊销）、路由可靠性与预算控制 |

> ✅ *趋势*：Ollama 因平台特异性缺陷导致问题数量领先；SGLang 与 vLLM 在核心性能改进方面展现出最高工程效率。

---

#### **3. 模型支持竞赛**

| 新模型 / 架构                     | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4-Flash / -0731**    | ✅ (SM8x 路径) | ✅ (MoE 内核优化) | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.8-2.4T-A95B / Flash-Next** | ✅ (ROCm gfx950 计划) | ✅ (fp8_mqa_logits) | ❌ | ❌ | ❌ | ❌ |
| **Gemma 4 (Vision)**             | ⚠️ FP8 崩溃 | ⚠️ 尚未修复 | ✅ (`mtmd`, 无字母框) | ❌ (500 错误) | ❌ | ✅ (CLI 图像输入) |
| **Qwen3.5-MoE / Coder**          | ✅ | ✅ | ✅ (`reasoning_format none`) | ⚠️ 在 `</thinking>` 处终止 | ❌ | ❌ |
| **MIMO v2.5 (1M 上下文)**        | ❌ | ❌ | ❌ | ✅ (请求) | ❌ | ❌ |
| **VirbiusGuard (0.6B 安全卫士)** | ❌ | ❌ | ❌ | ✅ (请求) | ❌ | ❌ |

> 🏆 **胜者**：**Unsloth** 在**视觉-语言模型支持**方面领先，具备 CLI 图像输入与端到端 VLM 工作流能力。  
> 🥈 **vLLM** 在**下一代 MoE 模型生产就绪性**（DeepSeek-V4、Qwen3.8）方面表现最佳。  
> 🥉 **llama.cpp** 在**本地 CPU/GPU 可移植性**与**模型格式灵活性**（GGUF、OpenCL/SYCL）方面优势明显。

---

#### **4. 性能前沿**

| 优化方向              | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|-----------------------|------|--------|-----------|--------|---------|---------|
| **KV Cache 效率**     | ✅ (卸载、边界控制) | ⚠️ (PP 分离) | ⚠️ (共享前缀) | ⚠️ (LFM3-Omni 泄漏) | ⚠️ (支出日志) | ⚠️ (缓存探测) |
| **批处理与吞吐量**    | ✅ (滑动窗口) | ⚠️ (预填充间隙：~2–7K tok/s vs vLLM 的 12.5K) | ⚠️ (MTP 草稿偏差) | ⚠️ (内存压力) | ✅ (Rust Redis 冷却) | ⚠️ (多用户延迟) |
| **量化（FP8, IQ3）**  | ✅ (FP8 Triton 内核) | ✅ (fp8_mqa_logits) | ✅ (IQ3_S/XXS) | ❌ | ❌ | ❌ |
| **分布式服务**        | ✅ (异步卸载、PCP 同步) | ✅ (NCCL EP、M-to-N 传输) | ❌ | ❌ | ✅ (Rust Redis 存储) | ❌ |
| **内核级调优**        | ✅ (移除 int32/int64 复制) | ✅ (MoE 融合、AIter 路由) | ✅ (FWHT、DMA 步长) | ❌ | ❌ | ✅ (日志脱敏、I/O 缓存) |

> 🔍 **关键洞察**：vLLM 与 SGLang 在**分布式推理与内核优化**上持续突破边界；而 **llama.cpp** 在**跨平台量化与轻量级运行时效率**方面占据主导地位。LiteLLM 正通过基于 Rust 的状态管理，在**基础设施韧性**方面实现创新。

---

#### **5. 层级定位**

| 项目        | 主要层级                     | 核心差异化 |
|-------------|------------------------------|------------|
| **vLLM**    | **推理引擎**                 | 高吞吐、GPU 优化，支持最新 MoE 与 Flash 注意力 |
| **SGLang**  | **推理引擎 + 协调层**         | 高级推测解码、多节点分离、专注 NCCL |
| **llama.cpp** | **本地运行时 / 嵌入式推理**   | 以 CPU 为先、原生支持 GGUF，广泛后端兼容（SYCL、OpenCL、WebGPU） |
| **Ollama**  | **开发者网关 / 本地运行时**   | 用户友好的 CLI、代理工具链（tokencut）、Apple Silicon 优化 |
| **LiteLLM** | **API 网关 / 多提供方代理**   | 预算控制、密钥吊销、MCP/多提供方路由 |
| **Unsloth** | **全栈平台（训练 → 服务）**   | 集成工作室、CLI 图像输入、微调+推理流水线 |

> 🧩 **战略洞察**：vLLM 与 SGLang 是**核心推理引擎**；Ollama 与 LiteLLM 是**面向应用的平台**；Unsloth 是唯一将**训练、微调与推理**整合于单一栈的项目。

---

#### **6. 趋势信号**

🔍 **从 2026-11-01 活动中浮现的新兴行业趋势**：

1. **硬件碎片化加速**：  
   - AMD ROCm（gfx950）与 Intel XPU 支持已成为高优先级目标，反映出对非 NVIDIA 生态的需求增长。  
   - 华为 Ascend NPU RFC 提示区域硬件主权需求上升（面向中国部署）。

2. **推测解码稳定性是关键瓶颈**：  
   - 多个项目报告在 MTP/EAGLE 配置下出现崩溃或输出偏差（vLLM、SGLang、llama.cpp）。  
   - 表明**推测解码尚未达到生产可用水平**，在代理系统中使用前需谨慎验证。

3. **内存管理是首要运维风险**：  
   - Ollama（LFM3-Omni 泄漏）、SGLang（CUDA core dump）、Unsloth（NVLink 损坏）均暴露内存相关不稳定性。  
   - 开发者必须监控每模型内存预算，避免共享运行器过载。

4. **安全与治理已成为核心功能**：  
   - LiteLLM 的 `/key/revoke` 接口与 Ollama 的 `tokencut` 功能反映出对**意外泄露防护**和**代理系统上下文控制**的迫切需求。

5. **代理工作流要求全链路集成**：  
   - Unsloth 的 CLI 图像输入、Ollama 的 tokencut、LiteLLM 的 spend logging 均表明：**代理开发者需要端到端工具链**，而非仅依赖推理。

---

### ✅ **对应用开发者的建议**  
- **对于高吞吐大模型服务**：使用 **vLLM**（若使用 Gemma 4 + FP8 图像，请选用 `v0.22.0`）或 **SGLang**（若可接受推测解码更高风险）。  
- **对于本地 CPU 推理**：选择 **llama.cpp**（b12023+）以获得最佳模型兼容性与最小依赖。  
- **对于开发体验与代理工作流**：**Ollama** 搭配 `tokencut`，以及 **LiteLLM** 搭配 `spend_logs_prompt_storage: latest_turn`，提供显著运营优势。  
- **对于视觉-语言流水线**：**Unsloth** 提供从微调到推理最流畅的过渡体验。  
- **始终验证推测解码**——在堆栈中的回归问题修复前，避免使用。

> 💬 *总结*：AI 基础设施栈已不再单一化——**根据你的工作负载、硬件环境与运维成熟度进行选择**。生产环境中，优先考虑稳定性而非新颖性。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

### **vLLM Digest — 2026-11-01**

---

#### **1. 今日亮点**  
vLLM 项目正加速推进对下一代模型与硬件的支持，目前正重点开发 **DeepSeek-V4-Flash** 对 SM8x（Ampere）架构的兼容性，解决在广泛使用的 A100/A800 及 RTX 30xx GPU 上进行生产部署的关键障碍。与此同时，针对 Qwen3.8 系列模型在 gfx950（MI355X）上的 AMD ROCm 性能优化工作也在持续加强，新提交的 PR 集中于降低稀疏索引中的内存开销，并提升分布式系统中 KV offload 的可靠性。

---

#### **2. 发布与破坏性变更**  
*过去 24 小时内未报告新版本发布或破坏性变更。*  
最新稳定版本仍为 `v0.30.0`，当前开发重点聚焦于多后端的功能对齐、性能调优及稳定性修复。

---

#### **3. 新模型与硬件支持**  
- **DeepSeek-V4-Flash / DeepSeek-V4-Flash-0731**：SM8x（Ampere: A100/A800, RTX 30xx）的完整支持正在 [#50576](https://github.com/vllm-project/vllm/issues/50576) 中积极追踪，社区需求强烈（123 条评论）。该功能将解锁对传统企业级 GPU 的完整推理能力。
- **Qwen3.8-2.4T-A95B 与 Qwen3.8-Flash-Next**：针对 **gfx950 / MI355X** 的专用 ROCm 性能优化计划已在 [#57149](https://github.com/vllm-project/vllm/issues/57149) 与 [#59575](https://github.com/vllm-project/vllm/issues/59575) 中启动，目标是优化 MXFP4 MoE 与 GatedDeltaNet 架构。
- **Intel GPU (XPU)**：XPU 支持持续进展，包括修复压缩张量 WNA16 MoE 选择器崩溃问题 [#43750](https://github.com/vllm-project/vllm/issues/43750)，但平台仍处于实验阶段。

---

#### **4. 性能与优化**  
- **ROCm 特定优化**：多项 PR 提升内核效率：
  - [#63910](https://github.com/vllm-project/vllm/pull/63910)：移除 GLM-5.3-Flash kpool 索引器中的 int32/int64 复制 → 减少 CPU-GPU 同步与内存抖动。
  - [#63867](https://github.com/vllm-project/vllm/pull/63867)：跳过滑动窗口预填充中完全掩码的上下文块 → 提升长上下文模型吞吐量。
  - [#62913](https://github.com/vllm-project/vllm/pull/62913)：为 ROCm 预填充注意力启用 FP8 Triton 内核 → 利用 per-tensor FP8 缓存并结合 gfx942 优化内核。
- **内存效率**： 
  - [#55985](https://github.com/vllm-project/vllm/pull/55985)：通过 0.4-fit 计划器限制 `fastsafetensors` 的峰值内存使用，防止模型加载期间出现 OOM。
  - [#59367](https://github.com/vllm-project/vllm/pull/59367)：通过工作区管理器预留 DCP top-k 合并的临时空间 → 防止瞬时内存分配激增。
- **异步与卸载改进**： 
  - [#64236](https://github.com/vllm-project/vllm/pull/64236)：在混合步骤中延迟异步加载 → 避免阻塞同步加载，提升并发性。

---

#### **5. 稳定性与回归问题**  
今日报告的关键稳定性问题如下：

| 严重程度 | 问题 | 描述 | 修复状态 |
|--------|------|-------------|------------|
| 🔴 高 | [#63732](https://github.com/vllm-project/vllm/issues/63732) | 使用 Gemma 4 + FP8 量化时，图像请求因 FP8 转换像素值溢出 patch_embedder 输入而崩溃（`v0.23.0+`） | 尚无修复；已确认为 `v0.22.0` 以来的回归 |
| 🔴 高 | [#63549](https://github.com/vllm-project/vllm/issues/63549) | 异步调度 + 抢占导致 Model Runner V1 崩溃，报错 `KeyError all_token_ids[req_id]` | 尚无修复；影响 P/D 分离的 Ascend 部署 |
| 🟡 中 | [#64489](https://github.com/vllm-project/vllm/issues/64489) | `vllm/vllm-openai:unlimited-ocr` 图像服务无法正确提供 Unlimited-OCR 模型 | 待调查 |
| 🟡 中 | [#64654](https://github.com/vllm-project/vllm/issues/64654) | 请求日志需支持可观测性，同时避免内容泄露 | 功能请求；暂无 PR |

> ✅ *备注*：近期已合并多个修复 PR（如 [#63874](https://github.com/vllm-project/vllm/pull/63874), [#62916](https://github.com/vllm-project/vllm/pull/62916)），但均未涉及上述高严重性回归问题。

---

#### **6. 对应用开发者的影响**  
- **若使用 Gemma 4 并处理 FP8 图像输入，请暂用 `v0.22.0` 版本**，直至 [#63732](https://github.com/vllm-project/vllm/issues/63732) 解决前避免使用 `v0.23.0+`。
- **部署在 AMD ROCm 环境？** 请优先在 MI355X 上测试 Qwen3.8-2.4T-A95B 与 Qwen3.8-Flash-Next —— 未来 PR（如 [#63910](https://github.com/vllm-project/vllm/pull/63910) 与 [#62913](https://github.com/vllm-project/vllm/pull/62913)）预计将带来显著性能提升。
- **混合 Mamba + EAGLE 模型**（如 DeepSeek-V4-Flash）在超过 2 个注意力组时仍可能存在未经验证的前缀命中行为 —— 参见 [#51771](https://github.com/vllm-project/vllm/issues/51771)。
- **谨慎启用 `--reasoning-parser qwen3`** —— 已知在 `<think>` 块后保留前导 `\n\n` 的问题（[#61972](https://github.com/vllm-project/vllm/issues/61972)）可能影响工具调用解析准确性。
- **监控 NixlPushMode 的可靠性** —— 在大规模编排部署中存在已知失败模式；详见 [#48633](https://github.com/vllm-project/vllm/issues/48633)。

> 💡 *技巧提示*：多节点部署时，请确保使用更新的 PR（如 [#62916](https://github.com/vllm-project/vllm/pull/62916)）实现 PCP 各 Rank 间 `engine_id` 的同步，以防止启动失败。

---  
*摘要源自 GitHub 活动：[vllm-project/vllm](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

### **SGLang Digest — 2026-11-01**

#### **1. 今日亮点**  
SGLang 项目持续深化对多样化硬件与工作负载的高吞吐、低延迟推理支持。核心工作聚焦于解决推测解码中的关键稳定性问题（尤其是 EAGLE3/MTP 与 DeepSeek-V4 场景），并推进生产级基础设施建设——最显著的是统一 `api.v1` 后端的迁移进程以及多节点拆分架构的优化。当前重点仍在于稳定基于 NCCL 的通信机制，并提升 CI 可靠性。

#### **2. 发布与破坏性变更**  
*无*。过去 24 小时内未发布新版本或破坏性 API/配置变更。

#### **3. 新模型与硬件支持**  
- **模型支持：**  
  - *DeepSeek-V4.1:* 正在围绕正确提示渲染（`deepseek-recipe`）和通过 `aiter` 路由在 AMD gfx950 上优化 MoE 内核（PR #42499）开展开发。  
  - *MiniMax-M3.1-Flash GB300:* 已加入 SGLang 食谱，用于 PD AgentX 部署（PR #47699）。  
- **硬件/后端支持：**  
  - **NCCL 2.30+ 功能：** 实验性集成 NCCL EP（低延迟解码）、跨组 M-to-N 传输及零 SM 单边通信（跟踪于 #32704，PR #47600）。  
  - **Intel CPU 后端：** 2026 年第四季度持续优化路线图，聚焦 MoE 执行与低成本中小型模型服务（Issue #44675）。  
  - **AMD ROCm：** 持续优化 fp8_mqa_logits 处理逻辑与 MoE 融合（如 PR #42504）。

#### **4. 性能与优化**  
- **发现吞吐量差距：**  
  - 在 SM120（4× RTX PRO 6000，TP=4）上运行 DeepSeek-V4-Flash 预填充时，吞吐仅达 **~2–7K tok/s**，远低于 vLLM/Marlin 报告的 **~12.5K tok/s**（Issue #33422）。正在调查内核覆盖率与调优指导问题。  
- **内核级优化：**  
  - AMD：融合 MoE combine + decode all-reduce（PR #42504）；将 >8 行的路由密集 GEMM 交由 `aiter` 处理以提升吞吐（PR #42499）。  
  - Triton：修复滑动窗口解码仅读取部分窗口的问题（PR #42402）。  
- **内存与调度：**  
  - 在压力下延迟令牌分配器的 free-list 复制操作（PR #42520），以降低 CPU 开销。  
  - 启动代理缓存调度系统路线图（Issue #43098），实现超节点范围内的 KV 缓存共享。

#### **5. 稳定性与回归问题**  
- **严重崩溃与缺陷：**  
  1. **CUDA 核心转储追踪 (#26340):** 351 条评论；来自 PR 测试的自动收集转储表明 CUDA 内核存在系统性不稳定性。高危；暂无修复方案。[GitHub Issue](https://github.com/sgl-project/sglang/issues/26340)  
  2. **PP 拆分解码垃圾输出 (#47613):** 当 `pp_async_batch_depth > pp_size / 2` 时，出现第 0 个 token 的异常运行。可复现且带校验和；影响数据并行扩展能力。[GitHub Issue](https://github.com/sgl-project/sglang/issues/47613)  
  3. **EAGLE 草稿扩展崩溃 (#47490):** 在 DP 注意力 + FlashInfer A2A + 预填充 CUDA graph 场景下出现负维度错误。对推测解码流程至关重要。[GitHub Issue](https://github.com/sgl-project/sglang/issues/47490)  
- **CI/基础设施不稳定：**  
  - `PR Test Base/Extra` 中持续存在不稳定的测试（Issue #42752）；影响 PR 验证流水线可靠性。[GitHub Issue](https://github.com/sgl-project/sglang/issues/42752)

#### **6. 对应用开发者的影响**  
- **预期高吞吐场景下的不稳定性：** 在 #47613 修复前，请避免使用 `pp_async_batch_depth > pp_size / 2`。  
- **监控推测解码行为：** EAGLE3/MTP 与 DeepSeek-V4 模型在特定配置下可能崩溃或产生错误输出——请使用最新 `main` 构建版本进行验证。  
- **尽可能使用统一 API：** 迁移至 `api.v1.Generate`（RFC #45816）将简化客户端代码并提升兼容性——请为未来旧端点弃用做好准备。  
- **避免 S3 与 Modelexpress 冲突：** 显式设置的 `MX_MODEL_URI` 正被 S3 路径覆盖（Issue #47428）——部署脚本中需确保加载器选择一致。  
- **离线基准测试：** 若 HF 连接受限，可使用 `--dataset-name random --offline`（Issue #47420）。  

> ✅ **可操作建议：** 生产部署时，请锁定稳定 `main` 提交版本，并在扩缩容前密切关注 issue #26340 的 CUDA 稳定性预警。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-11-01**

---

### **1. 今日重点**  
最新更新聚焦于优化推测解码的稳定性，提升 SYCL 与 OpenCL 后端的可靠性，并增强对 Gemma 4 与 Qwen3 系列模型的兼容性。关键修复解决了 Intel Arc GPU 上 `--split-mode tensor` 导致的崩溃问题，并确保多任务推理中共享决策前缀的正确处理。

---

### **2. 发布与破坏性变更**  
- **b12024**: 将 LFM SC（长格式记忆）中的 3D 张量操作重构为 2D，以提升内存布局效率。[PR #30831](https://github.com/ggml-org/llama.cpp/pull/30831)  
- **b12023**: 修复服务器响应中共享前缀截断问题，确保子任务间决策嵌入的完整性。修复 #30482。[PR #30710](https://github.com/ggml-org/llama.cpp/pull/30710)  
- **b12022**: 使 `/v1/responses` 接口支持 `extra_params`，提升与下游 LLM 网关的兼容性。[PR #30959](https://github.com/ggml-org/llama.cpp/pull/30959)  
- **b12021**: 在 Gemma 4 模型图像缩放时移除 letterbox 填充，提升视觉输入保真度。[PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)  
- **b12020**: 为 OpenCL 后端添加 FWHT（快速沃尔什-哈达玛变换）支持。[PR #31434](https://github.com/ggml-org/llama.cpp/pull/31434)  

> ⚠️ 未报告任何破坏性 API 变更；所有更新均为新增功能或缺陷修复。

---

### **3. 新模型与硬件支持**  
- **Gemma 4**: 完整支持图像预处理中无 letterboxing（通过 `mtmd`）。已测试 MTP 草稿模型及 `--mmproj`。[PR #30922](https://github.com/ggml-org/llama.cpp/pull/30922)  
- **Qwen3-Coder**: 正确处理 `reasoning_format none` 和 think 块。[PR #31380](https://github.com/ggml-org/llama.cpp/pull/31380)  
- **OpenCL**: 初步支持 IQ3_S 与 IQ3_XXS 量化格式（专为 Adreno 优化）。[PR #31338](https://github.com/ggml-org/llama.cpp/pull/31338)  
- **SYCL**: 通过 PkgConfig 增强 LevelZero 检测（适用于 oneAPI 2026.1+）。[PR #31076](https://github.com/ggml-org/llama.cpp/pull/31076)  
- **WebGPU**: 实现组归一化核函数——对 stable-diffusion.cpp 等下游工具中的扩散模型至关重要。[PR #31490](https://github.com/ggml-org/llama.cpp/pull/31490)

---

### **4. 性能与优化**  
- **SYCL**: 优化 Q4_1/Q5_1/Q8_0 类型的 CPY 四舍五入逻辑，提升跨平台一致性（修复 B70/B580 上的 off-by-one 错误）。[PR #31465](https://github.com/ggml-org/llama.cpp/pull/31465)  
- **CPU**: 将 `mm` 从 3D 重构为 2D 张量重塑，降低 LFM SC 的开销。[PR #30831](https://github.com/ggml-org/llama.cpp/pull/30831)  
- **OpenCL**: 修复 Adreno 830 在执行 `Q6_K` GEMM 时因无效工作组大小导致的崩溃。[PR #31485](https://github.com/ggml-org/llama.cpp/pull/31485)  
- **Hexagon**: DMA 传输现在支持超过 24 位的步长（v75+ 设备），支持更大张量处理。[PR #31442](https://github.com/ggml-org/llama.cpp/pull/31442)  
- **iOS**: 从 `posix_memalign` 切换至 `vm_allocate`，修复模型卸载后内存占用报告不准确的问题。[PR #31437](https://github.com/ggml-org/llama.cpp/pull/31437)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 状态 | 修复 PR | 链接 |
|--------|------|-------|--------|------|
| 高 | `--split-mode tensor` 在双 Intel Arc B70 上因 SYCL 中的 `DEVICE_LOST` 导致崩溃 | 开放 | 待定 | [#27198](https://github.com/ggml-org/llama.cpp/issues/27198) |
| 高 | 推测解码（`draft-dflash/dspark`）在 Qwen3.5-MoE 上 `--temp 0` 时产生非一致输出 | 开放 | 待定 | [#31472](https://github.com/ggml-org/llama.cpp/issues/31472) |
| 高 | CUDA：当 MTP 草稿紧随 KV 缓存回滚时发生非法内存访问（RTX 4060 Ti） | 开放 | 待定 | [#31077](https://github.com/ggml-org/llama.cpp/issues/31077) |
| 中 | Vulkan：近期构建版本性能下降（RX 6600，Qwen3.5-9B-Q5_K_M） | 开放 | 待定 | [#24066](https://github.com/ggml-org/llama.cpp/issues/24066) |
| 中 | SYCL：`--fit` 未能完全考虑内存使用 → 运行时溢出 | 开放 | 待定 | [#27595](https://github.com/ggml-org/llama.cpp/issues/27595) |

> 注意：多个回归问题影响涉及推测解码、MTP 和多 GPU 配置的生产级推理流水线。

---

### **6. 对应用开发者的意义**  
- **推测解码需谨慎使用**：在 #31472 修复前，避免在 MoE 模型（如 `qwen35moe`）上使用 `draft-dflash` / `dspark`。建议改用 `draft-mtp` 作为更安全替代方案。  
- **Intel Arc 用户**：注意 `--split-mode tensor` 模式下可能存在不稳定；请暂勿使用，直到 #27198 修复。可考虑单卡推理或回退至 CUDA。  
- **视觉模型（Gemma 4）**：利用 `mtmd` 改进，实现无失真的更高图像分辨率处理。  
- **构建健壮性**：使用 `--server` 并配合 `extra_params` 支持（b12022+），以更好集成 vLLM、LlamaIndex 等 API 网关。  
- **内存管理**：在 iOS/macOS 平台，仅在应用了 PR #31437 后，`phys_footprint` 才会准确反映真实内存占用。  
- **未来兼容性**：若构建多模态代理或语音合成系统，需关注 WebGPU 组归一化支持（PR #31490）。

👉 **建议**：部署时锁定至 `b12023` 或更高版本以保证稳定性，并在投入生产前监控 #31472 与 #31077 的关键修复进展。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama 摘要 – 2026-11-01**

---

### **1. 今日重点**  
Windows 和 macOS 上关键稳定性问题激增，凸显内存管理与网络容错能力的持续挑战，尤其在大模型和代理工作流场景下。若干关键 PR 修复了长期存在的 OpenAI 兼容 API 行为缺陷（如 `top_p` 被静默覆盖）以及 LFM3-Omni 与 MLX 前缀缓存中的内存泄漏问题。社区仍在推动更优的模型支持（Qwen3.5 0.8B Q4_K_M、MIMO v2.5）与工具链集成（tokencut、BashBrunch）。

---

### **2. 版本发布与破坏性变更**  
*过去 24 小时内未报告新版本或破坏性变更。*  
目前多个高优先级修复正在评审中：
- [PR #19432](https://github.com/ollama/ollama/pull/19432)：修复 `/v1/chat/completions` 接口对模型采样器默认值（`temperature`、`top_p`）的静默覆盖问题。
- [PR #19423](https://github.com/ollama/ollama/pull/19423)：新增 Modelfile 参数与消息角色的大小写不敏感解析 —— 提升与用户自定义配置的兼容性。

---

### **3. 新模型与硬件支持**  
- **新模型请求**：[Issue #19428](https://github.com/ollama/ollama/issues/19428) 请求加入 **VirbiusGuard (0.6B)**，基于 Qwen3Guard-Gen-0.6B 构建的双语代理安全防护模型（Apache-2.0 许可证）。
- **模型请求**：[Issue #15887](https://github.com/ollama/ollama/issues/15887) 呼吁引入 **MIMO v2.5**，小米推出的百万令牌上下文大语言模型（MIT 许可证）。
- **量化请求**：[Issue #19436](https://github.com/ollama/ollama/issues/19436) 希望为 `qwen3.5:0.8b` 添加 `q4_K_M` 量化支持 —— 对 CPU 性能至关重要。
- **硬件/后端**：针对 Apple Silicon 的 MLX 支持（PR #19438、#19422）与 Linux 上 CUDA 库加载（PR #19433）的优化持续推进；暂无新增后端。

---

### **4. 性能与优化**  
- **内存效率**：
  - [PR #19438](https://github.com/ollama/ollama/pull/19438) 修复 `lfm3-omni` 在重复语音请求后持续内存增长的问题（8 次运行累计增加高达 +1.6 GB）。
  - [PR #18131](https://github.com/ollama/ollama/pull/18131) 及后续 [Issue #19435](https://github.com/ollama/ollama/issues/19435) 指出全局缓存预算机制是性能瓶颈；相关提案旨在实现按模型粒度控制内存使用。
- **延迟与吞吐量**：
  - [PR #19408](https://github.com/ollama/ollama/pull/19408) 扩展 MLX 决策模型支持，可在不截断的情况下处理更长输入。
  - [PR #19434](https://github.com/ollama/ollama/pull/19434) 引入 **tokencut** 作为上下文压缩代理 —— 通过结构化折叠 JSON 工具输出来减小输入规模，显著提升长周期代理的吞吐表现。
- **工具链**：[PR #19426](https://github.com/ollama/ollama/pull/19426) 修复因参数名冲突导致的工具调用误路由问题 —— 提升多工具环境下的可靠性。

---

### **5. 稳定性与回归问题**  
*今日报告高严重性问题：*

| 问题 | 严重性 | 概述 | 修复状态 |
|------|----------|--------|------------|
| [#19420](https://github.com/ollama/ollama/issues/19420) | 严重 | `Gemma4`：所有提示均返回 500 内部服务器错误（Windows 11） | ✅ 进行中（尚未修复） |
| [#19427](https://github.com/ollama/ollama/issues/19427) | 高 | `gemma3:4b` 在启用结构化输出且提示较长时返回空 JSON | ⚠️ 尚无修复 PR |
| [#19439](https://github.com/ollama/ollama/issues/19439) | 高 | Ollama 运行器在 RTX 5090 上执行工具调用时断开连接（HTTP 500 /tokenize） | ⚠️ 尚无修复 PR |
| [#19424](https://github.com/ollama/ollama/issues/19424) | 中等 | Qwen 模型在发出 `</thinking>` 后终止对话 | ⚠️ 尚无修复 PR |
| [#19437](https://github.com/ollama/ollama/issues/19437) | 中等 | `lfm3-omni` 在 macOS 上每请求内存增长约 1.6 GB | ✅ 已在 [PR #19438](https://github.com/ollama/ollama/pull/19438) 中修复 |

> 💡 注：多项回归问题指向 MLX 运行器中的内存处理逻辑及 Windows 特有的 TCP/HTTP 行为。

---

### **6. 对应用开发者的启示**  
- **避免在结构化输出场景中使用 `gemma3:4b`**，直至 [#19427](https://github.com/ollama/ollama/issues/19427) 修复完成 —— 可能出现无声失败。
- **在生产流水线中使用 `tokencut`**（[PR #19434](https://github.com/ollama/ollama/pull/19434)）以管理长上下文代理工作流并降低延迟。
- **谨慎使用 Qwen 模型** —— 若需维持连续对话流程，请勿使用 `</thinking>`，该标签会触发提前终止。
- **在 Windows 平台**：注意 TLS 握手超时问题（[#19430](https://github.com/ollama/ollama/issues/19430)）—— 在阻断 X25519MLKEM768 ClientHello 的网络环境下，可能需配置本地代理或调整网络设置。
- **Apple Silicon 用户**：请监控 `lfm3-omni` 的内存使用情况 —— 使用已应用 [PR #19438](https://github.com/ollama/ollama/pull/19438) 的最新构建版本，防止内存失控增长。

> 🔧 技巧提示：未来若支持 `OLLAMA_CACHE_RAM` 配置，或通过限制模型并发数，可缓解多模型共享缓存的竞争问题。

---  
*数据来源：github.com/ollama/ollama | 更新时间：2026-11-01*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM 简报 – 2026-11-01**

---

### **1. 今日重点**  
LiteLLM 项目持续强化企业级代理与网关能力，针对预算控制、路由可靠性及安全性进行了关键修复。主要进展包括新增公开的 `/key/revoke` 接口以应对密钥意外泄露风险，以及在 Anthropic、Amazon Nova 和 Cohere 上改进了 MCP 工具链和流式处理能力。这些更新体现了对运营韧性与多供应商一致性日益增长的关注。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内无新版本发布。*

**破坏性变更（PR #45909）：**  
当 `num_workers > 1` 时，热重载（`--reload`）现在能正确重启所有工作进程，修复了此前仅有一个工作进程重启的问题。此变更影响依赖多工作进程部署配置进行开发的用户。  
👉 [PR #45909](https://github.com/BerriAI/litellm/pull/45909)

**新功能（PR #49545）：**  
新增公开的 `POST /key/revoke` 接口，允许任何人发现已公开的虚拟 API 密钥后立即禁用该密钥——无需管理员权限。密钥将被封禁（而非删除），支持审计与恢复。  
👉 [PR #49545](https://github.com/BerriAI/litellm/pull/49545)

---

### **3. 新模型与硬件支持**  
*今日未新增模型或硬件后端。*

**待增强功能（Issue #34501）：**  
请求通过直接集成 GCP IAM 实现对 **GCP 托管 MCP 服务器** 的原生支持，减少 GKE 集群中对旁路认证代理的需求。  
👉 [Issue #34501](https://github.com/BerriAI/litellm/issues/34501)

---

### **4. 性能与优化**  
*性能优化正在进行中，尚未合并。*

**关键进展（PR #49525）：**  
路由器冷却时间与全局 TPM/RPM 速率限制正被迁移至基于 **Rust 后端的 Redis 存储**，在高吞吐路由场景下提升了并发能力并降低了延迟。该方案复用了 PR #49481 引入的存储接口，实现了 Python 与 Rust 组件间的共享状态。  
👉 [PR #49525](https://github.com/BerriAI/litellm/pull/49525)

**可选存储（PR #49544）：**  
为部署中保留花费日志中的提示内容的场景引入 `spend_logs_prompt_storage: latest_turn` 模式——仅存储无状态请求的当前轮次内容，减少存储膨胀，特别适合代码生成类代理。  
👉 [PR #49544](https://github.com/BerriAI/litellm/pull/49544)

---

### **5. 稳定性与回归问题**  
*今日报告多个稳定性问题，主要集中在流式处理、护栏机制和预算控制的边缘情况。*

- **严重（Issue #48809）：**  
  团队成员预算耗尽时错误地阻断非消耗型路由（如 `GET /v1/models`），破坏 OpenAI 兼容客户端发现流程。  
  👉 [Issue #48809](https://github.com/BerriAI/litellm/issues/48809) *(修复待定)*

- **高严重性（Issue #48364）：**  
  管理员级别 API 密钥在预算耗尽后无法强制执行 `max_budget` 或 `tpm_limit`，导致无限使用。  
  👉 [Issue #48364](https://github.com/BerriAI/litellm/issues/48364) *(修复待定)*

- **流式失败（Issue #35589）：**  
  Amazon Nova 流式处理因 `httpx` zstd 解码器重复使用已结束的 `decompressobj` 导致 100% 失败。  
  👉 [Issue #35589](https://github.com/BerriAI/litellm/issues/35589) *(修复 PR: #49529？尚未合并)*

- **护栏损坏（Issue #49449）：**  
  Anthropic 流式护栏错误地将传输块解析为完整 SSE 帧，导致静默文本扫描失败并产生警告泛滥。  
  👉 [Issue #49449](https://github.com/BerriAI/litellm/issues/49449) *(修复 PR: #49529？尚无修复)*

---

### **6. 对应用开发者的影响**  
使用 LiteLLM 进行生产部署的开发者应：
- **更新路由逻辑**，避免在预算耗尽后依赖 `GET /models` —— 可考虑预缓存模型列表。
- **在 CI/CD 流程或代理系统中使用新的 `/key/revoke` 接口**，以缓解密钥意外泄露风险。
- **启用 `spend_logs_prompt_storage: latest_turn`**，若构建代码生成或长上下文代理，以降低日志体积与成本。
- **在预发环境验证管理员密钥的预算控制有效性**——当前行为可能存在绕过漏洞。
- **监控 Amazon Nova 与 Anthropic 护栏的流式行为**；预计在相关 PR 合并前仍可能出现中断。

这些更新标志着 LiteLLM 正逐步成长为一个稳健的推理基础设施层，尤其适用于管理多供应商、多代理及受监管工作负载的团队。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth 简报 – 2026-11-01**

---

### **1. 今日亮点**  
Unsloth 持续快速演进为全栈 AI 开发平台，Studio 在 UI/UX 上迎来重大优化，同时在视觉模型的 CLI 工具方面实现关键改进。核心 PR 实现了 `unsloth inference` 与 `chat` 命令对图像输入的支持，修复了非 NVLink GPU 上模型加载的长期问题，并通过更智能的日志脱敏和文件系统处理显著优化了后端性能。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
无新版本发布，亦无破坏性 API/配置变更。项目仍在积极开发中，通过拉取请求持续推进功能与稳定性改进。

---

### **3. 新模型与硬件支持**  
- ✅ **视觉语言模型（VLMs）**：CLI 现已通过 `--image` 标志支持在 `inference` 与 `chat` 命令中传入图像（PR #16347），使 Qwen2.5-VL-3B-Instruct 等 VLM 的从训练到推理的全流程自动化成为可能。  
- 🚧 **昇腾 NPU（华为）后端**：RFC #10772 提议原生支持华为昇腾 NPU —— 对面向中国的 LLM 部署与微调至关重要。这反映了国内 AI 硬件兼容性的日益增长需求。  
- 🌐 **ModelScope 集成**：问题 #16263 与 #16294 指出，当模型被锁定到特定修订版或缺乏默认分支回退机制时，从 ModelScope 加载模型存在困难，表明对部分第三方模型源的支持尚不完整。

> 🔗 [RFC: 昇腾 NPU 支持](https://github.com/unslothai/unsloth/issues/10772)  
> 🔗 [CLI 图像输入修复](https://github.com/unslothai/unsloth/pull/16347)

---

### **4. 性能与优化**  
Studio 后端逻辑实现显著性能提升：
- **日志脱敏加速**：PR #16356 早期跳过无法匹配的正则规则，将无关行的每行处理成本从约 3–13ms 降至接近零。  
- **文件系统效率提升**：多个 PR（#16355、#16354、#16353、#16352）通过缓存目录列表并仅在会话中解析一次元数据，减少了冗余 I/O 操作。对于大型数据集，可使模型/库加载的启动时间缩短高达 80%。  
- **调度器唤醒优化**：PR #16350 用即时唤醒替代队列轮询，在停止时立即唤醒，显著降低并发请求切换的延迟（对多用户推理服务器至关重要）。

> 🔗 [优化日志脱敏](https://github.com/unslothai/unsloth/pull/16356)  
> 🔗 [高效画廊与语音加载](https://github.com/unslothai/unsloth/pull/16355)  
> 🔗 [更快的数据集文件夹检查](https://github.com/unslothai/unsloth/pull/16353)

---

### **5. 稳定性与回归问题**  
今日报告关键稳定性问题：
- ❌ **非 NVLink GPU 数据损坏**：问题 #10613 确认 `GGML_CUDA_P2P=1` 在 RTX 6000 Ada、PRO 6000、L40/L40S 及 L4 显卡上被错误启用，导致多 GPU 推理过程中出现无声数据损坏。*修复 PR 待提交。*  
- ⚠️ **Windows 上模型加载卡死**：问题 #16294 报告，当使用 Auto attention 且 torch < 2.13 缺少 Flash Attention 支持时，触发“无可用内核”崩溃，尤其影响 MiniMax-H3 模型。  
- ⚠️ **Studio 启动卡死**：问题 #16267 揭露每个缓存的 GGUF 模型都会触发对 Hugging Face 的重新探测，导致启动过程出现 20 分钟超时。严重影响本地优先工作流的可用性。  
- ⚠️ **设置持久化丢失**：问题 #16309 指出，关键设置（外观、安全、浏览器）仅存储于 `localStorage`，导致跨浏览器来源或会话时发生静默重置。

> 🔗 [NVLink 配置错误漏洞](https://github.com/unslothai/unsloth/issues/10613)  
> 🔗 [Windows Flash Attention 崩溃](https://github.com/unslothai/unsloth/issues/16294)  
> 🔗 [Studio 启动超时](https://github.com/unslothai/unsloth/issues/16267)

---

### **6. 对应用开发者的影响**  
- **构建可靠的 VLM 工作流**：随着 CLI 图像支持合并（PR #16347），开发者可完全自动化从微调到推理的视觉模型流水线，无需切换工具。  
- **规避 NVLink 风险**：若部署在非 NVLink GPU（如 RTX 6000 Ada），请手动禁用 `GGML_CUDA_P2P=1`，直至修复上线。可通过环境变量或在 `unsloth_cli` 配置中覆盖。  
- **优化大规模数据集场景**：利用 `GET /api/library` 与数据集检查的新优化，显著降低管理数千个资产的应用的启动开销。  
- **规划跨浏览器持久化策略**：避免依赖客户端设置存储；预计未来更新将把设置移至服务端（参见 #16309）。  
- **关注硬件多样性**：随着昇腾 NPU 支持进入 RFC 阶段，需为面向中国市场的部署准备双后端工作流。

> 💡 小贴士：在 CLI 中显式使用 `trust_remote_code=True`（通过 `--trust-remote-code`），以避免自定义模型仓库加载失败（问题 #16331）。

*简报基于 GitHub 活动整理（2026-11-01 UTC）。*

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*