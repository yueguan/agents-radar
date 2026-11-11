# AI 基础设施日报 2026-11-11

> 生成时间: 2026-11-11 02:13 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目 AI 基础设施生态报告 – 2026-11-11**

---

### **1. 生态概览**  
截至2026年末，AI 推理基础设施领域呈现出高度专业化、跨平台加速以及安全意识显著提升的特征。各项目正聚焦于异构硬件（尤其是 AMD ROCm (gfx950/MI355X)、NVIDIA Blackwell (SM120) 和 Intel XPU）上的大模型高性能推理服务，同时持续深化与智能体工作流、多模态输入及推测解码的集成。在 LiteLLM v1.107.0 安全事件之后，以“安全设计”为核心的部署实践明显增强，标志着 DevOps 标准日趋成熟。与此同时，llama.cpp 与 Ollama 等本地运行时持续突破轻量级嵌入式推理边界，推动边缘与移动场景的应用落地。

---

### **2. 活动对比**

| 项目       | 开放问题数 | 已合并的 PR | 发布状态             |
|------------|-------------|------------|----------------------|
| **vLLM**   | 48          | 14         | 稳定版：`v0.33.0rc1.dev992` |
| **SGLang** | 62          | 12         | 补丁发布 (`v0.5.23`)，存在回归问题 |
| **llama.cpp** | 51        | 10         | 无新版本发布；内部变更 |
| **Ollama** | 38          | 10         | RC 版：`v0.40.17-rc0` — 稳定更新 |
| **LiteLLM** | 58         | 9          | 安全补丁：`v1.110.0-rc.1`，LTS 版 `v1.108.5` |
| **Unsloth** | 47         | 8          | 测试版：`0.1.916-beta` — 无破坏性变更 |

> ✅ **洞察**：SGLang 在问题数量上领先（反映活跃开发与调试），而 LiteLLM 与 Ollama 展现出强劲的发布节奏，聚焦稳定性和安全性。vLLM 在 API 连续性方面仍保持最高稳定性。

---

### **3. 模型支持竞赛**

| 新模型 / 架构                   | 支持项目                     | 备注 |
|--------------------------------|------------------------------|------|
| **Qwen3.8-Flash-Next (MXFP4)** | vLLM, SGLang, Unsloth        | vLLM 与 SGLang 通过 AITER/FlyDSL 实现专用内核调优 |
| **DeepSeek-V4.1-Flash**       | vLLM, SGLang, Ollama (通过 MLX) | SGLang 默认启用 DSA KV 缓存；vLLM 优化解码批处理 |
| **Kimi-K3/K3.1 Vision**        | vLLM, Ollama, Unsloth        | Ollama 通过 MLX 支持视频输入；vLLM 支持仅元数据图像 |
| **MiniMax-H3-Image**           | Unsloth (新增)               | 首个重大图像生成功能加入 |
| **Mistral-Large-4**            | SGLang (完整支持)            | 包含 MoE、MLA、YaRN、Pixtral 多模态特性 |
| **Nemotron 3.5 Bolt 74B-A9B**  | Ollama (MLX 后端)            | 支持 3bit 量化与专家配置 |
| **TTS 模型 (zonos3-mini)**     | llama.cpp                    | 新增基于 Jinja 的文本归一化处理 |
| **AMD gfx950 / MI355X**        | vLLM, SGLang, llama.cpp      | vLLM 在 GEMM/内核调优方面领先；SGLang 默认启用 FP8 KV 缓存 |

> 🏆 **排行榜**：  
> - **vLLM** 在 **ROCm 优化深度** 与 **模型专属内核调优** 上胜出。  
> - **SGLang** 在 **多模型覆盖** 方面领先，尤其在 Mistral-Large-4 与 Qwen4-Exp 场景。  
> - **Ollama** 在 **就绪智能体的多模态支持**（视频 + 结构化推理）上占据主导。  
> - **Unsloth** 在 **UI/工具链集成** 与 **本地模型可移植性** 上表现卓越。

---

### **4. 性能前沿**

| 优化方向               | 领先项目                     | 关键进展 |
|------------------------|------------------------------|----------|
| **KV 缓存效率**        | vLLM, SGLang, llama.cpp      | SGLang 默认启用 FP8_e4m3；llama.cpp 实现融合反量化；vLLM 采用 split-K BF16 路由器 |
| **批处理与吞吐量**     | vLLM, SGLang                 | vLLM 采用 split-K GEMM；SGLang 引入 BF16x3 混合 mHC |
| **量化与融合内核**     | vLLM, llama.cpp, SGLang      | llama.cpp 实现融合 Q8_0/Q4_0 KV 反量化；vLLM/SGLang 支持 MXFP4 内核 |
| **分布式推理服务**     | vLLM, SGLang                 | vLLM 修复 NIXL/P/D 连接器；Mooncake 缓存失效机制优化 |
| **推测解码**           | Ollama, SGLang, vLLM         | Ollama 提供指标上报；Unsloth 出现草案接受率下降；SGLang 存在延迟回归 |
| **内核级调优**         | vLLM (AITER), SGLang (FlyDSL)| GFX950 特定 GEMM；warp split-K FlashInfer |

> 🔥 **前沿热点**：  
> - **ROCm (gfx950)** 已成为内核级优化的核心目标（vLLM/SGLang）。  
> - **FP8 与 MXFP4** 正逐步成为内存敏感部署的标准。  
> - **推测解码** 在各平台仍不稳定——SGLang 的性能回归、llama.cpp 的发散错误揭示了实现成熟度不足的问题。

---

### **5. 层级定位**

| 项目       | 主要层级                  | 角色摘要 |
|------------|----------------------------|----------|
| **vLLM**   | **推理引擎**               | 高吞吐、低延迟推理引擎，具备先进批处理与内核调优能力，面向数据中心规模部署。 |
| **SGLang** | **推理引擎 + 网关**       | 混合运行时，内置编排、智能体支持与多后端路由，专注分布式系统与推测解码。 |
| **llama.cpp** | **本地运行时 / 嵌入式**   | 轻量级、支持 CPU/GPU 加速的推理引擎，适用于边缘设备、移动端与离线环境，强调二进制效率与底层控制。 |
| **Ollama** | **网关 + 本地运行时**      | 开发者友好的 CLI/UI 层，提供模型管理、工具调用与智能体原型构建，实现本地执行与类云易用性的无缝衔接。 |
| **LiteLLM** | **API 网关 / 编排层**     | 多提供商抽象层，支持成本追踪、速率限制与可观测性，是企业级 LLM 流水线的关键组件。 |
| **Unsloth** | **智能体工作室 / UX 平台** | 全流程智能体开发环境，包含 UI、反向代理支持与模型集成工具，聚焦开发者体验与部署安全。 |

> 📊 **战略差异化**：  
> - **工程师** 在 vLLM 与 SGLang 之间选择时，应权衡吞吐量与灵活性。  
> - **产品团队** 构建智能体时，推荐优先使用 Ollama 或 Unsloth 以实现快速原型验证。  
> - **企业用户** 需借助 LiteLLM 实现成本可视性与供应商无关性。

---

### **6. 趋势信号**

#### **新兴行业趋势（源自 2026-11-11 活动）：**
1. **硬件多样化加速**：AMD ROCm (gfx950/MI355X) 已不再是小众选择，vLLM 与 SGLang 已提供原厂内核优化。
2. **FP8 与 MXFP4 成为标准**：所有主要项目均支持或默认启用 FP8 KV 缓存，标志着大规模内存高效推理时代的到来。
3. **安全不可妥协**：LiteLLM v1.107.0 事件凸显供应链卫生已成为基础设施信任的核心要素，签名容器与审计日志已成为基本要求。
4. **推测解码仍脆弱**：尽管广泛应用，但 SGLang 的性能回归、llama.cpp 的发散错误与 Unsloth 的准确率下降表明实现尚不成熟。
5. **多模态工作流进入主流**：视频输入（Ollama）、视觉模型（Unsloth）、图像搜索（LiteLLM）已成生产就绪功能，不再属于实验范畴。
6. **开发者体验成为新差异点**：Ollama 与 Unsloth 的成功并非源于原始性能，而在于无缝的工具链、一致的 UI 与可靠的模型加载体验。

#### **对应用开发者的可操作建议：**
- ✅ **优先考虑安全**：使用 `ghcr.io/berriai/litellm:lts` 并通过 cosign 验证镜像签名。
- ✅ **验证推测解码输出**，特别是量化模型——预期存在不一致性。
- ✅ **在可用时利用 FP8 与 MXFP4**（vLLM/SGLang），实现内存节省而不牺牲吞吐。
- ✅ **使用 Ollama 的 `OLLAMA_NUM_PARALLEL`** 降低智能体工作负载中的首个令牌延迟。
- ✅ **在 AMD/Intel Arc GPU 上测试**——已知 ROCm 与 Vulkan 驱动存在不稳定性。
- ✅ **避免在 Hopper 架构上使用 SGLang v0.5.23**——报告存在显著解码延迟回归。

---

> **最终提示**：当前的 AI 推理栈已不仅关乎速度——更关乎 **可靠性、安全性与可用性**。今日的领先者不仅是最快，更是最稳健、集成度最高、最安全可规模化部署的。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM 摘要 – 2026-11-11**

---

### **1. 今日亮点**  
vLLM 项目持续推进对 AMD ROCm 硬件的激进优化，多个高影响力性能 PR 针对 gfx950（MI355X）和 MI350X 平台——尤其聚焦于 Qwen3.8-Flash-Next 与 DeepSeek-V4.1 模型。关键稳定性修复已合并至 NIXL P/D 解耦架构及 MooncakeConnector 缓存失效问题，提升了分布式推理场景下的可靠性。Intel XPU 引擎的新 CI 支持进一步拓展了跨平台兼容性。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
未观察到新版本发布或破坏性 API/配置变更。项目当前稳定在 v0.33.0rc1.dev992+g1648fefab 版本，重点仍放在内部优化而非面向用户的更新。

---

### **3. 新模型与硬件支持**  
- **ROCm (gfx950 / MI355X)**：新增对 `amd/Qwen3.8-2.4T-A95B-Quark-MXFP4` 与 `Qwen3.8-Flash-Next-Quark-MXFP4` 的支持，通过 AITER 的 FlyDSL 实现专用 GEMM 与内核调优。
- **Intel XPU**：已在 Intel CI 中启用 `v1/engine` 测试（#66926），标志着向完整 XPU 集成迈出关键一步。
- **Kimi-K3/K3.1**：EPD 中新增仅元数据图像输入支持（#66925），并更新了 B300 AgentX 预设配置（#66433）。
- **多模态**：PyNvVideoCodec 现已支持关键帧感知采样，提升视频预处理效率（#66073）。

> 🔗 [PR #66926](https://github.com/vllm-project/vllm/pull/66926) | [PR #66925](https://github.com/vllm-project/vllm/pull/66925) | [PR #66073](https://github.com/vllm-project/vllm/pull/66073)

---

### **4. 性能与优化**  
- **ROCm/GFX950**：为 DeepSeek-V4.1 解码批次（22 ≤ M ≤ 256）引入 Split-K BF16 路由 GEMM，替代 hipBLASLt，显著提升并发专家执行下的吞吐量（#65749）。
- **GLM-5.3**：在 SM107 上添加 FlashInfer warp split-K 与 `ll_bf16` GEMM，降低小令牌数情况下的延迟（#65706）。
- **MiniMax-M2.7**：针对 v0.29–0.32 版本中观察到的 TP4+EP 吞吐下降问题，正在进行修复（#63705）。
- **批量无关模式**：通过改用 Humming 路由而非原始 Triton，修复了线性层重复计算效率低下的问题（#66838）。
- **DSpark/Muse Spark**：在 NVIDIA SM100 上融合草稿归一化与马尔可夫采样，实现 TPOT 延迟降低 -4.4%（#66764）。
- **CI/构建**：通过构建优化，将 PR 到信号时间从约 73 分钟缩短至目标 ~30 分钟（#66411）。

> 🔗 [PR #65749](https://github.com/vllm-project/vllm/pull/65749) | [PR #65706](https://github.com/vllm-project/vllm/pull/65706) | [PR #66764](https://github.com/vllm-project/vllm/pull/66764) | [PR #66838](https://github.com/vllm-project/vllm/pull/66838)

---

### **5. 稳定性与回归问题**  
- **严重**：MiMo-V2.6-Flash-MOPD 在 TP > 1 时 DFlash 接受失败（#66387）。此问题影响使用 DFlash 草稿器的多节点推理流水线。
- **高严重性**：MooncakeConnector 在编码器重置后未清除缓存，导致后续请求中出现过期嵌入（#66846）。修复已合并。
- **中等**：NIXL P/D 连接器因丢失对等连接而卡死；通过重新握手逻辑修复（#66795）。
- **回归**：自提交 `0fae39a` 后，Qwen3.5-122B-A10B-FP8 准确率下降（#65906）。调查正在进行中。
- **轻微**：新 `(max-num-batched-tokens, max-num-seqs)` 形状首次启动时分配的 KV 缓存比后续启动小约 5%（#54383）。

> 🔗 [Issue #66387](https://github.com/vllm-project/vllm/issues/66387) | [PR #66846](https://github.com/vllm-project/vllm/pull/66846) | [PR #66795](https://github.com/vllm-project/vllm/pull/66795) | [Issue #65906](https://github.com/vllm-project/vllm/issues/65906)

---

### **6. 对应用开发者的启示**  
- **对于部署在 AMD GPU 上的生产环境**：预计在 MI355X/gfx950 上，Qwen3.8 系列与 DeepSeek-V4.1 模型的吞吐量与稳定性将得到提升。建议配合 AITER 的稀疏 MLA 内核使用 `--kv-cache-dtype fp8_e4m3` 以节省内存（#66036）。
- **对于使用结构化输出的智能体**：请注意自 `#45424` 以来解码吞吐量约下降 2 倍；若使用基于语法的解析，请评估其影响。
- **对于分布式系统**：请确保使用最新 vLLM 版本，避免重启或预填充重置期间 Mooncake/NIXL 连接器的已知问题（#66846, #66795）。
- **对于模型开发者**：若部署混合 GDN/Mamba 模型且采用异构 TP，需验证 `MooncakeConnector` 兼容性（#53078 仍开放但已跟踪）。
- **对于 CI/CD 流水线**：构建时间正在改善——预计反馈周期更快（目标信号时间 ~30 分钟）。

> ✅ **实用建议**：仅在必要时使用 `VLLM_BATCH_INVARIANT=1`；近期修复（#66838）已避免量化 GPTQ 层中昂贵的重复展开操作。

---  
*数据来源：[vllm-project/vllm GitHub](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-11**

---

### **1. 今日亮点**  
SGLang 项目持续强化多 GPU 与跨架构支持，AMD ROCm（gfx950/gfx1201）和 Blackwell（SM120）后端获得关键性能提升。重点进展包括优化 ROCm 上的 DSA KV 缓存默认配置、在 HiCache 中启用 Mamba 主机池的 FP8 支持，并修复影响 PR 验证的不稳定的 CI 基础设施。在 v0.5.22 之后，Hopper（SM90）上解码延迟出现严重回归，凸显持续调优的挑战。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未检测到新发布版本。*  
然而，观察到一个显著的**性能回归**：从 `v0.5.22` 升级至 `v0.5.23` 在 Hopper（SM90）上并发度为 12 时导致**每步解码时间增加 33–45%**，尤其在 DSPARK 指令推测解码场景下更为明显。开发者在升级前应关注此问题 ([#49336](https://github.com/sgl-project/sglang/issues/49336))。

---

### **3. 新模型与硬件支持**  
- **AMD ROCm (gfx950/gfx1201)**：DeepSeek-V4.1-Flash 与 Qwen4-Exp 已建立专用优化追踪 ([#49327](https://github.com/sgl-project/sglang/issues/49327))。  
- **Mistral-Large-4**：已完整支持，包含 MoE、MLA、DSA 稀疏注意力、YaRN 及通过 Pixtral 实现的多模态视觉功能 ([#47271](https://github.com/sgl-project/sglang/pull/47271))。  
- **Qwen4-Exp**：已集成至 HiCache，支持统一的 Engram 存储 ([#46248](https://github.com/sgl-project/sglang/pull/46248))。  
- **NVFP4 KV**：在 SM120/B200 上持续推进；初始合并请求已提交 ([#29913](https://github.com/sgl-project/sglang/issues/29913))。

---

### **4. 性能与优化**  
- **AMD ROCm**：  
  - gfx950 上默认 DSA KV 缓存现已设为 `fp8_e4m3` ([#48996](https://github.com/sgl-project/sglang/pull/48996))。  
  - 在 MI355X 上通过填充 Q 并采用分组步进启动方式，实现更快的 MQA logits 解码 ([#48649](https://github.com/sgl-project/sglang/pull/48649))。  
  - 12 头 MLA 解码与 8 token 验证现在使用 fp8-Q ASM 运行 ([#45320](https://github.com/sgl-project/sglang/pull/45320))。  
- **NVIDIA (SM120/Blackwell)**：  
  - DeepSeek-V4.1 解码批处理中采用 mHC 混合模式 BF16x3，提升吞吐量 ([#48456](https://github.com/sgl-project/sglang/pull/48456))。  
- **HiCache 与 Mooncake**：  
  - 在排队阶段将驻留于 SSD 的 KV 数据预加载至 DRAM，降低延迟 ([#49696](https://github.com/sgl-project/sglang/pull/49696))。  
  - Mamba 主机池大小现可通过 `--hicache-mamba-size-gb` 配置 ([#47867](https://github.com/sgl-project/sglang/pull/47867))。

---

### **5. 稳定性与回归问题**  
- **严重回归**：`DeepSeek-V4.1-Flash` 在 SM90 上从 v0.5.22 升级至 v0.5.23 后解码延迟上升 **33–45%** ([#49336](https://github.com/sgl-project/sglang/issues/49336))。  
- **FA4 后端崩溃**：GLM-5.3-Flash 在 SM120 上进行 CUDA graph 捕获时因混合扩展重塑而崩溃——仅 Triton 后端可正常工作 ([#42012](https://github.com/sgl-project/sglang/issues/42012))。  
- **缓存命中率下降**：在 HiCache + Mooncake 场景下，当 L2/L3 压力增大时，前缀缓存命中率可复现地下降 ([#47481](https://github.com/sgl-project/sglang/issues/47481))。  
- **缺失 BOS Token**：Rust 服务端在分词器期望 `add_bos_token` 时返回空的起始序列标记 ([#49519](https://github.com/sgl-project/sglang/issues/49519))。  
- **CI 不稳定**：[不稳定测试跟踪器](https://github.com/sgl-project/sglang/issues/42752) 中评论数量高，表明系统性 CI 不稳定性影响了 PR 验证。

---

### **6. 对应用开发者的启示**  
- **在 Hopper GPU 上谨慎使用 v0.5.23+**：除非使用非推测解码，否则预期解码速度显著下降。  
- **若部署于 MI355X 或 gfx950**：请启用 `fp8_e4m3` KV 缓存，并对 MXFP4 模型使用 `--fp4-gemm-backend aiter|triton`。  
- **优化代理工作流**：利用 HiCache + Mooncake：排队阶段已启用预加载，提升 L3 离线效率。  
- **避免在 SM120 上使用 FA4 后端运行 GLM-5.3-Flash**：存在已知崩溃问题，建议坚持使用 Triton 后端。  
- **监控 CI 稳定性**：频繁的不稳定测试可能延缓集成流程；建议锁定至稳定提交，直至 [#42752](https://github.com/sgl-project/sglang/issues/42752) 修复完成。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-11-11**

---

### **1. 今日重点**  
最新更新聚焦于关键的 CUDA 稳定性修复，包括减少 VMM 内存池分配以防止高显存 GPU 上的 OOM 错误，并改进了图重用的处理逻辑。后端鲁棒性持续提升，新增 SYCL 优化与 Vulkan 错误缓解措施，同时模型支持范围扩展，新增 kimi K3.1 视觉模型和 TTS 模型。

---

### **2. 发布与破坏性变更**  
过去 24 小时内未发布新的标签版本。但内部有以下重要变更：  
- **CUDA**：针对具备统一内存的 GPU，将 VMM 内存池预留量减半（从 16GB → 8GB），以避免 `cuMemAddressReserve` 失败 ([#30542](https://github.com/ggml-org/llama.cpp/pull/30542))。  
- **CUDA**：将图与并发流状态从 `common.cuh` 中移出，以降低重建开销 ([#32001](https://github.com/ggml-org/llama.cpp/pull/32001))。  
- **SYCL**：在 `set_main_device` 中引入缓存设备 ID，避免重复加锁 ([#32013](https://github.com/ggml-org/llama.cpp/pull/32013))。

> ⚠️ *使用大显存内存池或频繁图重用的开发者应验证其 CUDA 配置是否符合这些变更。*

---

### **3. 新模型与硬件支持**  
- ✅ **视觉模型**：通过 `mtmd` 后端新增对 *kimi K3.1 vision* 的支持 ([#31980](https://github.com/ggml-org/llama.cpp/pull/31980))。  
- ✅ **TTS 模型**：新增 `zonos3-mini` TTS 模型，支持通过 Jinja 实现自定义文本归一化 ([#32020](https://github.com/ggml-org/llama.cpp/pull/32020))。  
- ✅ **后端**：  
  - **CANN**：通过正确传递注意力掩码修复 Flash Attention 中的 NaN 问题 ([#31590](https://github.com/ggml-org/llama.cpp/pull/31590))。  
  - **SYCL**：提出改进 FP16 构建支持的方案 ([#31678](https://github.com/ggml-org/llama.cpp/issues/31678))。  
  - **Vulkan**：继续推进 Adreno 驱动兼容性工作 ([#28635](https://github.com/ggml-org/llama.cpp/issues/28635), [#30649](https://github.com/ggml-org/llama.cpp/issues/30649))。  
- ✅ **量化支持**：正在进行对 Ternary-Bonsai-2 使用的 `PQ2_0`（类型 142）和 `PTQ1_0`（类型 143）的支持开发 ([#29058](https://github.com/ggml-org/llama.cpp/issues/29058))。

---

### **4. 性能与优化**  
- **CUDA**：将 Q8_0/Q4_0 KV 反量化操作融合进 MMA Flash Attention 内核，消除中间 f16 数据生成，降低临时内存占用 ([#31802](https://github.com/ggml-org/llama.cpp/pull/31802))。  
- **HIP/ROCm**：通过固定分配 32MB BLAS 工作区替代按调用分配，修复工作区分配问题，提升稳定性和性能 ([#31838](https://github.com/ggml-org/llama.cpp/pull/31838))。  
- **Vulkan**：提出将令牌验证与 GQA 头处理合并为单行瓦片的优化方案，实现更优的内存聚合 ([#30190](https://github.com/ggml-org/llama.cpp/pull/30190))。  
- **CPU**：跨子上下文共享计算缓冲区，提升多线程推理效率 ([#31945](https://github.com/ggml-org/llama.cpp/pull/31945))。

> 💡 *使用融合 KV 反量化或优化后端时，预期 MoE 和量化模型的延迟可明显降低。*

---

### **5. 稳定性与回归问题**  
今日报告的关键问题：  
1. **推测解码结果偏差**（漏洞）：当目标模型为量化模型（如 Q4_K_M）时，贪婪采样在推测模式与普通模式下输出不一致；bf16 模式下正常运行 ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)) — *40 条评论，3 个赞*。  
2. **推测解码段错误**（严重）：在设置 Llama-3.1-405B-Instruct + Llama-3.1-8B 草稿模型时发生崩溃 ([#9949](https://github.com/ggml-org/llama.cpp/issues/9949)) — *17 条评论，0 个赞*。  
3. **SYCL 在双 Arc Pro B70 上触发 TDR 重置**：因加载 DFlash2 草稿模型导致 GPU 驱动重启 ([#28778](https://github.com/ggml-org/llama.cpp/issues/28778)) — *12 条评论*。  
4. **Vulkan 在 Adreno 840 上驱动崩溃**：在全量卸载 27B TQ1_0 模型时，`vkGetDeviceFaultInfoEXT` 导致崩溃 ([#30649](https://github.com/ggml-org/llama.cpp/issues/30649)) — *4 条评论*。  

> 🔍 *目前尚无 PR 直接修复这些问题；使用推测解码或移动 Vulkan 后端的开发者应谨慎操作。*

---

### **6. 对应用开发者的启示**  
- 使用 `GGML_CUDA_POOL_DEBUG=1` 调试 CUDA 内存问题——对诊断 OOM 场景非常有帮助 ([#31919](https://github.com/ggml-org/llama.cpp/pull/31919))。  
- 若在 CPU 上使用 `--cpu-moe` 运行 MoE 模型，请注意双 GPU SYCL 环境下可能出现性能下降 ([#31510](https://github.com/ggml-org/llama.cpp/issues/31510))。  
- 对于使用推测解码的生产系统，务必仔细验证输出——尤其是量化目标模型，因存在已知的偏差漏洞 ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618))。  
- 若使用 HIP 构建，建议升级至 ROCm 6.3，以确保同步行为和掩码宽度处理正确 ([#31617](https://github.com/ggml-org/llama.cpp/pull/31617))。  
- UI 现已从 `llama-brand` 拉取品牌资产，确保部署间视觉标识一致性 ([#32039](https://github.com/ggml-org/llama.cpp/pull/32039))。

> 📌 *请对特定后端的边缘情况保持警惕——特别是 Intel Arc、Adreno 与 CANN 平台，并在真实负载下测试关键路径。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-11**

---

### **1. 今日亮点**  
最新发布的 **v0.40.17-rc0** 在基于 Vulkan 的 GPU 上显著提升了模型运行器的稳定性，并增强了 CLI 在重启过程中的容错能力。关键进展包括：MLX 后端支持视频输入，以及推测性解码的指标报告——标志着与高性能推理工作流的深度集成。此外，UI 现在可通过 `/api/show` 动态识别模型级别的思考能力，确保客户端间行为一致。

---

### **2. 发布与破坏性变更**  
- **v0.40.17-rc0**（最新版）：  
  - ✅ 改进 ChatGPT CLI 的恢复逻辑与优雅重启机制 ([#19583](https://github.com/ollama/ollama/pull/19583))  
  - ✅ 通过 `cmd/launch` 中的 `thinking` 元数据增强 OpenCode 设置 ([#19409](https://github.com/ollama/ollama/pull/19409))  
  - ✅ 针对集成 Vulkan 的 GPU 优化内存映射（`mmap`）([#19409](https://github.com/ollama/ollama/pull/19409))  
  > *注：未报告破坏性 API 变更；向后兼容性已保留。*

---

### **3. 新模型与硬件支持**  
- **MLX 后端**：  
  - 通过 `images` 字段新增对 **视频输入**（MP4/MOV）的支持，适用于 LFM3-VL、LFM3-Omni 以及 Qwen3.5/3.6 模型 ([#19348](https://github.com/ollama/ollama/pull/19348))  
  - 新增对 **Nemotron 3.5 Bolt 74B-A9B** 的支持，包含每层专家配置、混合精度堆叠及 3 位量化 ([#19229](https://github.com/ollama/ollama/pull/19229))  
- **Vulkan 后端**：  
  - 优化集成 GPU 上的 mmap 使用，提升内存效率  
- **新功能**：  
  - MLX 模型现在在 `/api/show` 响应中报告 `"video"` 能力  
  - 决策类模型（如 Liquid d1、Kev、H2O Lightning）已支持通过 `/v1/systemone` 提案 ([#19639](https://github.com/ollama/ollama/issues/19639))

---

### **4. 性能与优化**  
- **推测性解码指标**：  
  - 每个请求的草稿接受率、草稿与实际接受的 token 数量现已暴露于 API 响应及 `--verbose` 输出中 ([#19368](https://github.com/ollama/ollama/pull/19368))  
- **并行推理**：  
  - MLX 现在支持通过 `OLLAMA_NUM_PARALLEL` 实现并发生成（默认值 = 1），降低多请求负载下的首个 token 延迟 ([#19197](https://github.com/ollama/ollama/pull/19197))  
- **模型加载**：  
  - 新增 `ollama load MODEL` 命令，可在不启动会话的情况下预热模型 ([#16207](https://github.com/ollama/ollama/pull/16207))  
- **内存管理**：  
  - 修复 MLX KV-cache 泄漏问题：内存不再随请求累积，调用间已正确释放 ([#17875](https://github.com/ollama/ollama/issues/17875) — 已关闭)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 状态 | 修复/拉取请求 | 链接 |
|--------|------|--------|--------|------|
| ⚠️ 高 | Windows RTX 5090：工具调用期间运行器断开 → HTTP 500 /tokenize | 开放 | 无 PR | [#19439](https://github.com/ollama/ollama/issues/19439) |
| ⚠️ 高 | AMD Radeon 780M Vulkan 回归：大模型上出现 `ErrorDeviceLost` | 已关闭 | 0.32.10 版本修复 | [#17748](https://github.com/ollama/ollama/issues/17748) |
| ⚠️ 中 | Intel Iris Xe iGPU：0.33.3+ 版本中 qwen2.5:14b 出现 `ErrorOutOfDeviceMemory` | 已关闭 | 降级回退方案 | [#18531](https://github.com/ollama/ollama/issues/18531) |
| ⚠️ 中 | OLLAMA_KEEP_ALIVE=300s 对长交互周期过短 | 开放 | 无 PR | [#19624](https://github.com/ollama/ollama/issues/19624) |
| 🟡 低 | `-v`/`--version` 显示误导性“警告”信息 | 已关闭 | 已修补 | [#19613](https://github.com/ollama/ollama/issues/19613) |

> **注意**：**Windows + RTX 5090** 与 **AMD 780M Vulkan** 上仍存在关键回归问题，表明近期运行器在特定 GPU 上存在压力点。

---

### **6. 对应用开发者的意义**  
- **构建更智能的代理**：使用 `OLLAMA_NUM_PARALLEL` 降低多用户或批处理场景下的延迟（仅限 MLX）。  
- **利用结构化推理**：通过 `/api/show` 动态开启思考模式，实现跨模型界面一致性——无需再硬编码名称判断。  
- **集成多模态数据**：MLX 后端现已支持视频输入——非常适合需要视觉上下文的代理系统。  
- **监控推测性解码性能**：利用新指标调整草稿深度，评估真实场景下的加速效果。  
- **规避常见陷阱**：在 Cloud `/api/chat` 中注意大负载（>32 MiB）的未公开限制；使用 `load` 命令提前预热模型。  

> 🔧 **实用提示**：为确保在 Intel/AMD 集成 GPU 上稳定推理，建议暂定版本低于 0.33.3，直至 Vulkan 问题彻底解决。  

---  
*数据来源 [github.com/ollama/ollama](https://github.com/ollama/ollama) — 更新于 2026-11-11。*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

### **1. 今日亮点**  
LiteLLM 最新动态聚焦于对高调漏洞版本 `1.107.0` 的关键安全修复，该版本因嵌入了窃取凭证的木马程序，已被从 PyPI 和 Docker 注册表中移除。与此同时，项目持续推进架构现代化，统一了容器镜像、Helm Charts 及 Terraform 工作负载，并通过 cosign 签名强化安全性。核心关注点仍在于提升 Anthropic、Vertex AI、Bedrock 等多提供商的成本追踪准确性和流式传输可靠性。

---

### **2. 发布与破坏性变更**  
- **v1.110.0-rc.1**, **v1.109.0**, **v1.108.5 (LTS)**：发布新版，安全态势增强，且所有镜像均使用 [commit `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 中的密钥进行 cosign 签名。  
  - **需采取行动**：部署前必须使用 `cosign verify` 验证镜像完整性。  
  - **安全提示**：受感染的 `v1.107.0` 已从所有公开注册表中下架；请勿引用该版本。  
  - **LTS 更新**：`ghcr.io/berriai/litellm:lts` 现指向 `v1.108.5`——推荐用于生产环境以确保稳定性。

> 🔐 [验证 Docker 镜像签名](https://docs.sigstore.dev/cosign/overview/)  
> 📦 [发布 v1.110.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.110.0-rc.1)  
> 💡 [LTS 标签指引](https://github.com/BerriAI/litellm#lts)

---

### **3. 新模型与硬件支持**  
本周期未新增模型或硬件后端。但有显著改进：
- **Bedrock 知识库搜索增强** ([PR #51063](https://github.com/BerriAI/litellm/pull/51063))：支持基于图像的查询，并在使用多模态嵌入模型时返回图像匹配结果。
- **Mistral 原生 Token 计数** ([PR #50653](https://github.com/BerriAI/litellm/pull/50653))：集成 Mistral 官方 `/responses/input_tokens` 接口，实现精确 token 计费与上下文窗口控制。

---

### **4. 性能与优化**  
- **流式延迟降低**：PR [#51033](https://github.com/BerriAI/litellm/pull/51033) 确保流在终端事件触发时立即终止，而非等待上游连接关闭——在长时响应（如 Claude Code）中可将感知延迟降低高达约 30 秒。
- **TPM 速率限制修复**：PR [#49675](https://github.com/BerriAI/litellm/pull/49675) 解决了上游返回 429 时提前耗尽 TPM 的问题，恢复了高并发场景下的正确限流行为。
- **SSE Keepalive 处理**：PR [#49711](https://github.com/BerriAI/litellm/pull/49711) 通过确保上游错误能在流中传播，防止 keepalive ping 后出现静默失败。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 状态 | 修复 PR | 说明 |
|------|----------|--------|--------|-------|
| [#47826](https://github.com/BerriAI/litellm/issues/47826) | ⚠️ 关键 | 已关闭 | N/A | 恶意 `v1.107.0` 包已从 PyPI/Docker 移除。**所有用户必须立即升级。** |
| [#47340](https://github.com/BerriAI/litellm/issues/47340) | 🛑 高 | 开放 | N/A | `tpm_limit` 在内存中永不重置；需手动调用 `/key/update`。 |
| [#51029](https://github.com/BerriAI/litellm/issues/51029) | 🟡 中等 | 开放 | N/A | OTel `gen_ai.response.model` 显示别名而非实际使用的模型。 |
| [#49675](https://github.com/BerriAI/litellm/issues/49675) | 🛑 高 | 开放 | N/A | 429 场景下 TPM 计数器超期存活——导致误限流。 |
| [#48937](https://github.com/BerriAI/litellm/issues/48937) | 🟡 中等 | 已关闭 | PR [#50957](https://github.com/BerriAI/litellm/pull/50957) | 流式成本记录为 $0，尽管实际已产生消耗。已在 sidecar 支出收集器中修复。 |

> ✅ **已修复**：成本日志记录（`#48937`）和格式错误请求（`#50937`）均已合并相关修复 PR。

---

### **6. 对应用开发者的意义**  
- **安全优先**：立即审计部署中是否存在 `v1.107.0` 或更早版本。生产环境推荐使用 `ghcr.io/berriai/litellm:lts` 以确保稳定与安全。
- **成本准确性**：若依赖自定义计价、`base_model` 或跨平台成本追踪，请确认配置与近期修复一致——尤其注意 `response_cost` 与 `spend` 日志（如 `#47675`, `#47908`）。
- **流式可靠性**：使用流式 API（尤其是 Anthropic/Claude）的代理应用将受益于 `#51033` 等修复，获得更可预测的终止行为。
- **未来兼容性**：建议采用新的统一 Docker/Helm/Terraform 工作流（[PRs #43332, #43330, #43358](https://github.com/BerriAI/litellm/pulls?utf8=%E2%9C%93&q=43332)），简化 CI/CD 与基础设施即代码流程。
- **防护机制与工具**：请注意 `custom_code` 正则标志与 `MCP tool` 防护可见性仍有限制——可参考 [PR #51097](https://github.com/BerriAI/litellm/pull/51097) 获取即将支持的功能。

🔧 **建议操作**：  
- 升级至 `v1.108.5-lts` 或更高版本。  
- 审查虚拟密钥与支出配置。  
- 启用 OpenTelemetry 并验证跟踪中 `gen_ai.response.model` 的一致性。  

---  
*摘要生成时间：2026-11-11 | 来源：[GitHub – BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-11**

---

### **1. 今日亮点**  
Unsloth 生态系统持续成熟，Studio 在可用性和安全性方面聚焦改进，尤其体现在反向代理支持、UI 一致性以及模型集成上。关键 PR 修复了关键渲染错误（如玻璃主题错乱），提升了 TTS/音频模型的可见性，并通过 Cloudflare Tunnel 实现安全的自定义域名托管——这对企业级部署至关重要。一项重大修复确保在调用 vLLM/SGLang 工具时正确处理 `max_tokens`，解决了与上下文长度相关的 400 错误。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内无报告。*  
未发布新版本或破坏性配置变更。最新桌面版仍为 **0.1.916-beta**，未发布迁移说明。

---

### **3. 新模型与硬件支持**  
- ✅ **MiniMax-H3-Image** 已加入 Studio 的图像生成流水线（`MiniMaxAI/MiniMax-H3-Image`），支持文本到图像及多参考编辑（最多 16 张图像）。[PR #17453](https://github.com/unslothai/unsloth/pull/17453)  
- ✅ **Yandex** 作为主搜索引擎的备用层级集成，提升对 DDGS 反机器人限流的抗性。[PR #17447](https://github.com/unslothai/unsloth/pull/17447)  
- ✅ 针对 Linux 发行版（Fedora/RHEL/CentOS）请求 RPM 包，表明对原生打包的需求日益增长。[Issue #17445](https://github.com/unslothai/unsloth/issues/17445)  

> *注意：今日未引入新的 GPU 后端支持（如新 ROCm/NVIDIA 硬件）。*

---

### **4. 性能与优化**  
- 🚀 **上下文处理优化**：Studio 现在在向 vLLM 和 SGLang 发起工具调用时，正确遵守 `max_tokens` 限制，即使提示词 + max_tokens 超过窗口大小，也能防止 `context_length_exceeded` 错误。[PR #17426](https://github.com/unslothai/unsloth/pull/17426)  
- ⏱️ **模型加载效率**：`push_to_hub_gguf()` 现在可正确嵌入用户定义的标签至 README 卡片中，提升自定义量化模型的可发现性。[PR #17438](https://github.com/unslothai/unsloth/pull/17438)  
- 💡 **动态 GGUF 校准**：社区活跃提案，扩展校准数据集中的乌克兰语覆盖范围，以提升动态量化精度。[Issue #17371](https://github.com/unslothai/unsloth/issues/17371)

---

### **5. 稳定性与回归问题**  
- 🔴 **严重 UI 问题**：选择模型时玻璃主题导致严重界面渲染错乱。已在 [PR #17417](https://github.com/unslothai/unsloth/pull/17417) 中修复（已关闭）。  
- 🔴 **安全漏洞**：若回环请求携带 `X-Forwarded-*` 头部，代理页面会暴露种子管理员密码。已在 [PR #17454](https://github.com/unslothai/unsloth/pull/17454) 中修补。  
- 🔴 **AMD GPU 崩溃**：PyTorch 在 RX 6700 XT（gfx1031）上因 `unsloth-rocm` Docker 镜像中错误识别 RDNA 2 架构而发生段错误。[Issue #17147](https://github.com/unslothai/unsloth/issues/17147) — *尚未修复*。  
- 🔴 **本地模型加载失败**：Intel Arc 140V 上图像生成模型无法加载。[Issue #17137](https://github.com/unslothai/unsloth/issues/17137) — *待调查*。  
- 🟡 **工具使用回归**：在真实负载下，Qwen3.8-Flash-Next UD 3.1 GGUF 模型的草稿接受率从约 74% 下降至 56–66%，因 MTP 头部不匹配所致。[Issue #17402](https://github.com/unslothai/unsloth/issues/17402) — *计划进行头部对齐，待处理*。

---

### **6. 对应用开发者的影响**  
- **安全部署**：使用 [PR #17435](https://github.com/unslothai/unsloth/pull/17435) 可在允许列表内的反向代理（如 Cloudflare Tunnel）后部署 Unsloth Desktop，无需暴露凭证。  
- **可靠工具链**：确保应用安全使用 `max_tokens`，在集成 vLLM/SGLang 后端时验证上下文长度边界。  
- **模型可移植性**：利用 `push_to_hub_gguf(tags=...)` 正确标记并记录自定义 GGUF 模型；避免因缺少 README 元数据导致静默失败。  
- **UI/UX 注意事项**：在生产版本中启用玻璃主题前，请等待 [PR #17417](https://github.com/unslothai/unsloth/pull/17417) 推出。  
- **硬件感知设计**：在 AMD 与 Intel Arc GPU 上进行测试——已知 ROCm 与 Intel 驱动栈存在不稳定性。建议为这些平台准备降级方案。

> 对于智能体与微调应用：通过 API 加载的决策模型即使被排除推理，仍可能出现在聊天选择器中。[PR #17451](https://github.com/unslothai/unsloth/pull/17451) 正在解决此问题。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*