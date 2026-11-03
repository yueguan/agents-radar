# AI 基础设施日报 2026-11-03

> 生成时间: 2026-11-03 03:41 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI推理基础设施生态报告 – 2026-11-03**

---

### **1. 生态概览**  
2026年末，AI推理基础设施领域正迅速向高吞吐、低延迟、多模态服务演进，以支持下一代MoE与混合架构。各项目日益聚焦于硬件特化优化（尤其是SM12x/Blackwell）、推测解码效率以及跨平台可移植性。一个清晰的分野浮现：成熟且生产就绪的引擎（vLLM、Ollama）与快速迭代、创新驱动的平台（SGLang、LiteLLM、Unsloth）之间形成对比。Rust后端的兴起以及GPU驻留的LRU缓存机制，预示着系统正转向面向代理级工作负载、性能敏感且内存安全的架构设计。

---

### **2. 活跃度对比**

| 项目 | 开放问题数 | 近24小时合并的PR数 | 发布状态 |
|--------|--------------|--------------------------|----------------|
| **vLLM** | 587 | 389名贡献者 → 927次提交 | v0.33.0rc3（最终预发布版） |
| **SGLang** | 462 | 来自257名贡献者的1,007个PR | v0.5.23（稳定版本） |
| **llama.cpp** | 315 | 合并了3个关键修复 | 无新标签；持续开发中 |
| **Ollama** | 412 | 22个PR（主要为稳定性改进） | v0.40.14-rc0（RC版） |
| **LiteLLM** | 298 | 12个重大PR（Rust迁移） | 无新发布；处于β阶段 |
| **Unsloth** | 276 | 6个PR（聚焦UI/内存） | 无新发布 |

> ✅ **洞察**：SGLang在社区活跃度和贡献者推进速度上领先；vLLM在最终发布前展现出强劲的工程产出。LiteLLM正转向基础架构重构，全面采用Rust。

---

### **3. 模型支持竞赛**

| 新模型 / 架构 | 支持方 | 备注 |
|--------------------------|--------------|-------|
| **Reflection Beam-1.0 (501B MoE, 23B活跃)** | ✅ vLLM | 首个支持该规模的项目；使用DFlash2草案生成器 |
| **Recursive Flint (40B MoE)** | ✅ vLLM | SGLang与llama.cpp也已支持 |
| **Yandex AliceAI-Foundation-80B-A3B** | ✅ vLLM | 通过#57963添加 |
| **Muse Spark Open / Beam-1.0** | ✅ SGLang | 完整食谱支持；具备视觉-语言能力 |
| **Clover MoE (AR-only)** | ✅ llama.cpp | GGUF原生支持，因果混合注意力 |
| **LFM3-Audio-2B** | ✅ Unsloth | 通过loader.ts实现多模态TTS聊天 |
| **Qwen3.8-Flash-Next** | ✅ 所有项目（vLLM/SGLang/llama.cpp/Ollama） | 但所有项目仍存在FP8/KV缓存问题 |

> 🏆 **胜出者**：**vLLM** 与 **SGLang** 在前沿模型覆盖方面领先——尤其在大型MoE与多模态模型上。**vLLM** 在原始规模与架构就绪性上更具优势。

---

### **4. 性能前沿**

| 优化方向 | 领先项目 | 关键进展 |
|--------------------|------------------|--------------|
| **KV缓存与卸载** | vLLM、SGLang、llama.cpp | 异步查找发布（#57474）、HiSparse卸载（Ascend）、GPU驻留MoE LRU（#29949） |
| **批处理与吞吐** | vLLM、SGLang | 多序列flash attention（`flash_attn_ext_rows`）、SM12x下的live-KV重播 |
| **推测解码** | vLLM、SGLang | 零气泡交接（#64531）、开发中自适应策略 |
| **量化与内存** | llama.cpp、Ollama | Q4_K_M差距除0.8B外已弥合；FP8反量化修复（PR #16156） |
| **分布式服务** | SGLang、LiteLLM | 混合DSA KV池、Bedrock Converse模型路由 |
| **内核级加速** | SGLang、vLLM | 融合A GEMM（K=256），逆RoPE融合至`wo_a`路径 |

> 🔥 **趋势**：优化重心正从通用吞吐转向**面向硬件、上下文感知的调度策略**，尤其体现在**MoE专家管理**、**FP8精度控制**与**异步状态处理**方面。

---

### **5. 层级定位**

| 项目 | 主要层级 | 角色概述 |
|--------|---------------|--------------|
| **vLLM** | **推理引擎** | 高性能、可扩展的CUDA引擎；云/GPU推理的核心 |
| **SGLang** | **多后端推理引擎 + 网关** | 统一NPU/ROCm/CUDA上的执行；类似网关的抽象层 |
| **llama.cpp** | **本地运行时 / 嵌入式引擎** | CPU/GPU混合，低资源占用，适用于边缘与本地部署 |
| **Ollama** | **模型网关 / 本地服务器** | 用户友好的命令行/API层；连接本地模型与云端服务 |
| **LiteLLM** | **API网关 / 代理层** | 统一API外观；支持成本追踪、服务商切换、流式可靠性 |
| **Unsloth** | **代理UI + 运行时编排器** | 面向开发者的前端，包含模型加载、内存管理及多模态用户体验 |

> 📌 **定位洞察**：  
> - **vLLM/SGLang** 在*推理引擎*层级占据主导地位。  
> - **LiteLLM/Ollama** 作为*网关/代理*层，推动API标准化。  
> - **llama.cpp/Unsloth** 专注于*本地/运行时*部署与开发者体验。

---

### **6. 趋势信号**

#### 🔍 **提取的关键行业趋势：**
1. **SM12x（Blackwell）成为新前沿** —— vLLM与SGLang中出现多个回归问题（卡死、吞吐低下），表明Blackwell采纳正在加速，但仍不稳定。生产环境使用仍具风险。
2. **Rust正成为性能基准** —— LiteLLM追求子毫秒级开销目标，以及vLLM实验性Rust前端的推出，标志着向内存安全与延迟降低的战略转型。
3. **MoE规模化不再只是理论** —— Reflection Beam-1.0与Recursive Flint的支持确认，MoE模型已进入生产级推理流水线。
4. **多模态集成日趋成熟** —— Muse Spark Open、Beam-1.0与LFM3-Audio-2B均显示出在视觉/音频支持方面超越文本的持续进展。
5. **内存管理居核心地位** —— GPU驻留的LRU缓存、动态大小调整（Unsloth）、VRAM过预测修复等，凸显内存压力已成为首要约束。

#### 💡 **开发者应关注事项：**
- **避免在生产环境中使用v0.33.0rc3**，直至SM12x卡死与批处理不变性漏洞修复完成（vLLM）。
- **密切关注LiteLLM的Rust迁移** —— 预计近期将带来性能提升，同时可能伴随破坏性变更。
- **利用SGLang的统一后端** 实现多硬件部署（NPU/ROCm/CUDA）。
- **使用Ollama的`--cloud`标志** 以发现模型而无需拉取开销。
- **围绕静默会话取消**（Unsloth、Ollama）与非确定性解码（vLLM）构建代理容错机制。

> ✅ **最终建议**：生产系统应优先考虑**稳定性而非新颖性**。推荐使用**vLLM v0.32.0** 或 **Ollama v0.40.13** 以确保可靠推理。仅在测试环境中使用RC版本。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

**vLLM 消息简报 – 2026-11-03**

---

### **1. 今日亮点**  
vLLM 项目发布了 **v0.33.0rc3**，标志着一个重要里程碑，共包含 927 次提交，来自 389 名贡献者（其中 122 人为新贡献者）。此次发布新增对前沿模型的支持，包括 *Reflection Beam-1.0*（501B MoE，23B 激活）、*Recursive Flint*（40B MoE）以及 *Yandex AliceAI-Foundation-80B-A3B*，进一步拓展了下一代架构高吞吐推理生态。关键稳定性改进正在推进中，尤其集中在批处理不变性与确定性解码方面。

---

### **2. 发布与破坏性变更**  
- **v0.33.0rc3**：v0.33.0 的最终预发布候选版本，现已可通过 PyPI 与 Docker 镜像获取。  
  - [GitHub 发布页](https://github.com/vllm-project/vllm/releases/tag/v0.33.0rc3)  
- 未报告破坏性 API 变更；向后兼容性已保持。  
- 新增 `VLLM_USE_RUST_FRONTEND=1` 标志仍为实验性功能，但功能对齐情况通过 #44280 追踪。

---

### **3. 新模型与硬件支持**  
#### **新增模型**  
- **Reflection Beam-1.0**（501B MoE，23B 激活），支持 DFlash2 draft 功能 (#62074)  
- **Yandex AliceAI-Foundation-80B-A3B** (#57963)  
- **Recursive Flint**（40B MoE，2.5B 激活）(#62938)  
- **Muse Spark**（新增至受支持模型列表）

#### **硬件与后端支持**  
- **SM12x（Blackwell）**：针对 DeepSeek-V4-Flash/DSv4.1 在 RTX PRO 6000 Blackwell（SM120）与 DGX Spark（GB10，SM12.1）上的关键修复正在进行中，当前仍存在卡死与解码吞吐量低的问题 (#56892, #50576)。  
- **ROCm（gfx950 / MI355X）**：针对 Qwen3.8-Flash-Next 的性能优化追踪 (#59575)  
- **aarch64（ARM64）**：在 NVIDIA DGX Spark / Acer GN100 上仍缺少 sm_121 支持 (#36821) —— 尚未解决。

---

### **4. 性能与优化**  
- **批处理不变性确定性**：高优先级工作持续推进；多个 PR（如 #65127、#64493）验证种子范围与分词器行为，确保在 `torch.compile` 与 CUDA 图下可复现 (#27433, #39096)。  
- **KV 缓存与卸载**：  
  - 按请求组异步查找结果发布，提升调度效率 (#57474)。  
  - 入驻预留缓存减少异步 KV 加载的开销 (#65285)。  
- **推测解码**：结构化输出的零空泡草稿令牌交接已优化 (#64531)，提升流水线利用率。  
- **前缀缓存**：截断问题修复（如 #47349）与不一致完成情况（#5543）仍处于开放状态。  
- **Rust 前端**：性能对齐路线图持续进行 (#44280)；初步基准测试显示有潜力，但差距依然存在。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重程度 | 状态 | 修复 PR？ | 链接 |
|------|----------|--------|---------|------|
| `DeepSeek-V4-Flash` 在 `cudagraph_mode=FULL_AND_PIECEWISE + chunked prefill` 下运行约 6 次请求后卡死（SM12.x） | 严重 | 开放 | ❌ | [#40969](https://github.com/vllm-project/vllm/issues/40969) |
| `Qwen3.8-Flash-Next` 在 `indexer_budget` 附近出现非确定性贪心解码 | 高 | 开放 | ❌ | [#54521](https://github.com/vllm-project/vllm/issues/54521) |
| `DeepSeek-V4-Flash` 在 RTX 5080（SM120）上无声卡死 | 严重 | 开放 | ❌ | [#46625](https://github.com/vllm-project/vllm/issues/46625) |
| MTP 推测解码在长序列下崩溃（FP8-Qwen3.6-27B-FP8） | 高 | 开放 | ❌ | [#40756](https://github.com/vllm-project/vllm/issues/40756) |
| `vLLM v1` 前缀缓存导致 `temperature=0` 时首次请求结果不同 | 中等 | 开放 | ❌ | [#40896](https://github.com/vllm-project/vllm/issues/40896) |

> ⚠️ 多个回归问题与 **Blackwell（SM12x）** 和 **批处理不变性** 相关，尚未解决。这些问题影响使用 FP8、MoE 或推测解码的生产部署。

---

### **6. 对应用开发者的启示**  
- **谨慎在生产环境中使用 v0.33.0rc3** —— 虽然 Reflection Beam-1.0 与 Recursive Flint 等新模型释放了大规模 MoE 扩展能力，但 Blackwell（SM12x）支持仍不稳定。在 #56892 与 #40969 修复前，请避免在 GB10/RTX 5080 上使用 `cudagraph_mode=FULL_AND_PIECEWISE` 与 `chunked_prefill`。  
- **仅在可验证确定性的前提下启用 `--enable-prefix-caching`** —— 已知的非确定性输出与截断问题仍存在 (#54521, #47349)。  
- **对于低延迟服务，可尝试使用 Rust 前端（`VLLM_USE_RUST_FRONTEND=1`）**，但需接受功能不完整的情况 (#44280)。  
- **使用 MoE 模型（尤其是 Qwen3.8-Flash-Next）时注意 `temperature=0` 的漂移** —— 可考虑禁用 `persistent_topk`，或使用 `--disable-async-scheduling` 作为临时解决方案。  
- **对于多模型服务或冷启动场景**，请关注 #34303（CUDA 检查点/恢复）—— 它有望在 v1 引擎上实现近乎零冷启动。  

> ✅ **建议**：在 v0.33.0 正式版清除关键的 SM12x 与批处理不变性缺陷前，建议锁定至 `v0.32.0` 以保障稳定推理。使用 CI/CD 流水线对夜间构建版本进行测试，以便及早发现回归问题。

---  
*本简报数据来源于 GitHub：[vLLM 项目](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-11-03**

---

### **1. 今日亮点**  
SGLang 生态系统迎来重大更新，发布 **v0.5.23** 版本，共合并 1,007 个 PR，来自 257 名贡献者——充分体现了社区的活跃参与。核心进展包括新增支持 **Muse Spark Open** 与 **Beam-1.0** 两款 LLM/VLM 模型，同时在 NPU（Ascend）和 AMD ROCm 支持方面取得显著进展。针对不稳定的 CI 基础设施以及推测解码可靠性问题的稳定性优化工作仍在持续推进。

---

### **2. 发布与破坏性变更**  
- **v0.5.23**：今日发布，涵盖推理引擎、模型服务及硬件后端的广泛改进。未报告破坏性 API 变更；迁移指南详见 [发布说明](https://github.com/sgl-project/sglang/releases/tag/v0.5.23)。  
- **RFC：迁移到共享的 `api.v1.Generate` 后端** ([#45816](https://github.com/sgl-project/sglang/issues/45816)) —— 统一生成逻辑至单一模式的工作正在进行中；目前影响内部路由与处理器组件，但尚未涉及用户可见的 API。

---

### **3. 新模型与硬件支持**  
- **新增模型**：  
  - **Muse Spark Open**（LLM/VLM）：已完整支持，配套教程见 [此处](https://docs.sglang.io/cookbook/autoregressive/Meta/MuseSparkOpen)。  
  - **Beam-1.0**（LLM/VLM）：通过 [教程](https://docs.sglang.io/cookbook/autoregressive/Beam/Beam1_0) 支持。  
- **硬件与后端扩展**：  
  - **NPU（Ascend）**：支持 Ascend A5 的 DSV4.1（`#47660`），DeepSeek-V4 的 HiCache L3 支持（`#39784`），以及混合 DSA KV 池集成（`#43533`）。  
  - **AMD ROCm**：针对 gfx950 / MI355X 上的 Qwen3.8-Flash-Next 进行性能优化跟踪（`#44304`）。  
  - **Intel XPU**：提出 RFC 方案，支持分页专家与前瞻专家调度（LES）及 SSD 层卸载（`#46520`）。

---

### **4. 性能与优化**  
- **内核级改进**：  
  - SM12x FA4 解码现在在图重放时使用实时 KV 长度，避免空闲分片（`#47815`）。  
  - 融合 A GEMM 内核放宽 K 对齐要求，从 1024 降至 256，提升与 MLA 模型的兼容性（`#47898`）。  
  - 将逆 RoPE 融入 `wo_a` FP8 量化路径用于 DeepSeek-V4，降低内存带宽消耗（`#43786`）。  
- **内存与卸载优化**：  
  - Ascend DSA 服务中启用 HiSparse KV 卸载，结合 LIM 选择与 CopySFA 注意力机制（`#47383`）。  
  - FLUX.2 Klein 在去噪步骤间缓存参考 K/V（减少重复计算开销）（`#47953`）。  
- **扩散管道增强**：  
  - 新增 DM05 动作推理（`#48056`），Cosmos-Dreams 推送中优化前缀 K/V 缓冲区所有权管理（`#46613`）。

---

### **5. 稳定性与回归问题**  
- **关键不稳定性**：  
  - **[CI] 不稳定测试与基础设施故障** (`#42752`) —— 已有 165 条评论；`PR Test Base/Extra` 运行中反复失败，阻碍合并。对维护者为高优先级。  
- **崩溃与正确性缺陷**：  
  - 当所有状态被锁定时，Mamba 缓存分配失败导致引擎崩溃（`#43204`）；修复待定。  
  - GLM-5.3-Flash NEXTN 在首个请求时崩溃，原因出在越界嵌入收集（`#37548`）；已在补丁中修复。  
  - Qwen3.8-Flash-Next FP8 KV 缓存崩溃，由 QSA 稀疏注意力引起类型不匹配（`#36545`）；已修复。  
  - 使用 DFLASH + FlashInfer 自调优时，贪婪解码结果无法跨重启重现（`#39597`）。  
- **次要但值得关注的问题**：  
  - 混合 Mamba + EAGLE 场景下，当 Mamba 状态填满内存时，内存错误误报 `--mem-fraction-static`（`#47830`）。  
  - 输入中 `<image>` 字面量因令牌/数据不匹配导致请求中断（`#43979`）。

---

### **6. 对应用开发者的意义**  
- **构建健壮的智能体**：可期待在开发中的自适应策略下提升推测解码稳定性（`#23705`），并通过 Mooncake Elastic EP 实现更好的 MoE 扩展性（`#47859`）。  
- **利用多模态模型**：Muse Spark Open 与 Beam-1.0 为视觉语言任务提供新选择——请查阅其教程获取部署指引。  
- **优化成本与延迟**：使用 `fused-a-gemm`、`live-KV-length` 重放及 `K/V 缓存` 功能，可在 Hopper（SM120）与 B200 系统上降低推理开销。  
- **规避潜在陷阱**：注意已知的 FP8/KV 缓存类型不匹配、贪婪解码可复现性问题及图像标记处理问题，直至相关修复上线。  
- **为未来迁移做准备**：`api.v1.Generate` 统一将简化服务器架构——请关注 [#45816](https://github.com/sgl-project/sglang/issues/45816) 获取最新动态。

> 🔗 *完整上下文：[GitHub 仓库](https://github.com/sgl-project/sglang)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp 消息简报 – 2026-11-03**

---

### **1. 今日亮点**  
`llama.cpp` 最新进展聚焦于 **多序列快速注意力优化** 和 **MoE 推理性能提升**，关键合并请求（PR）显著提升了 CUDA、Vulkan 与 SYCL 后端下统一 KV 缓存的效率。核心重点包括 **MoE 专家缓存** 与 **驻留 GPU 的 LRU 策略**，以解决大规模混合专家模型中的可扩展性瓶颈。此外，新增对 **Clover MoE** 以及 **无需语音克隆的 Pocket TTS** 的支持，进一步增强了生态系统的灵活性。

---

### **2. 发布与破坏性变更**  
今日未发布新的标签版本。但已合并若干 **关键修复与行为变更**：  
- `b12063`：引入基于设备的 `dflash` 隐藏特性复制功能 (#31506) —— 提升多设备卸载性能。  
- `b12059`：优化 `flash_attn_ext_rows`，在多序列推理中实现更高效的统一 KV 处理 (#29510)。  
- `b12058`：修复使用 `--nextn` 时 `imatrix` 的崩溃问题 (#31522)。  

> 🔗 [GitHub 提交 b12063](https://github.com/ggml-org/llama.cpp/commit/b12063), [PR #29510](https://github.com/ggml-org/llama.cpp/pull/29510)

---

### **3. 新模型与硬件支持**  
- ✅ **Clover MoE (仅 AR)**：完整支持 GGUF 转换、模型加载及因果混合注意力 (#31306)。  
- ✅ **无需语音克隆的 Pocket TTS**：现可通过骨干 KV 状态支持预设语音状态 (#31429)。  
- ✅ **DFlash (Laguna S 2.1)**：已开启原生 DFlash 支持的功能请求 (#26669) —— 预计即将被优先处理。  
- ✅ **Apple Silicon (arm64)**：近期提交的 macOS/iOS 构建版本已在 GitHub Releases 上更新。  
- ✅ **Intel Arc B70/B60 (SYCL)**：持续优化 SYCL 后端在 Intel GPU 上的表现；详见问题 #31510（报告存在性能下降）。  

> 🔗 [PR #31306](https://github.com/ggml-org/llama.cpp/pull/31306), [Issue #26669](https://github.com/ggml-org/llama.cpp/issues/26669)

---

### **4. 性能与优化**  
- **多序列快速注意力**：`flash_attn_ext_rows` 扩展通过避免逐序列过滤，降低了统一 KV 缓存的开销 (#29510)。基准测试显示，在大批次场景下预填充延迟最高可提升 **约 30%**。  
- **Vulkan + Intel FA 内核**：通过专用着色器优化了 Intel 平台上的预填充性能 (#29357)。  
- **SYCL DPAS 快速注意力**：在 Xe2 平台（B580/B70）上新增对量化 KV 缓存（q4_0/q8_0）的支持，实现更快解码与小批量处理 (#31125)。  
- **MoE 专家缓存（驻留 GPU 的 LRU）**：正在进行的改进 (#29949) 目标是将缓存决策直接运行在 GPU 计算图上——这对可扩展的 MoE 推理至关重要。  
- **CUDA 图保留修复**：防止高并发下 CUDA 图过早被驱逐，降低重编译成本 (#30662)。

> 🔗 [PR #29510](https://github.com/ggml-org/llama.cpp/pull/29510), [PR #31125](https://github.com/ggml-org/llama.cpp/pull/31125), [PR #30662](https://github.com/ggml-org/llama.cpp/pull/30662)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|--------|------|------------|-----------|
| ⚠️ 高 | [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) | 多 GPU (`--split-mode layer`) + `--cpu-moe` 在 Intel Arc B60 上：**预填充速度下降 100–1000 倍（5 个 token 耗时 165 秒）** | 开放 |
| ⚠️ 高 | [#27845](https://github.com/ggml-org/llama.cpp/issues/27845) | 双 Intel B70 SYCL 后端使用 `-sm tensor` 时内存泄漏 | 已关闭（未确认） |
| ⚠️ 中 | [#30697](https://github.com/ggml-org/llama.cpp/issues/30697) | Vulkan 上使用 `--moe-cache-mib` 导致崩溃（RX 6900 XT） | 开放 |
| ⚠️ 中 | [#31538](https://github.com/ggml-org/llama.cpp/issues/31538) | Windows MSVC 构建出现段错误 | 开放 |
| ⚠️ 低 | [#27910](https://github.com/ggml-org/llama.cpp/issues/27910) | RTX 5090 在 Qwen3.8-27B Q6_K 推理期间显示丢失 | 已关闭 |

> 🔗 [Issue #31510](https://github.com/ggml-org/llama.cpp/issues/31510), [Issue #27845](https://github.com/ggml-org/llama.cpp/issues/27845)

---

### **6. 对应用开发者的启示**  
- **大规模部署 MoE 模型？** 关注 **驻留 GPU 的 MoE LRU 缓存** 进展 (#29949) —— 该功能将实现高效专家管理，避免 CPU 过度争用。  
- **使用多 GPU 部署？** 在 #31510 修复前，请避免在 Intel Arc GPU 上同时使用 `--split-mode layer` 与 `--cpu-moe`；可考虑改用 `--split-mode row` 或调整 `--gpu-layers`。  
- **构建智能体系统？** 利用新增的 **无需语音克隆的 Pocket TTS 语音支持** (#31429)，打造轻量级语音合成流水线。  
- **优化推理延迟？** 在支持的平台（CUDA/Vulkan/SYCL）启用 `flash_attn_ext_rows` —— 显著提升批处理与多序列场景下的吞吐量。  
- **处理工具调用？** 升级至最新代码以受益于 **响应 API 中的命名空间工具支持** (#31568) 与 **对 JSON 参数顺序的容错性** (#30193)。

> 🔗 [PR #31568](https://github.com/ggml-org/llama.cpp/pull/31568), [PR #30193](https://github.com/ggml-org/llama.cpp/pull/30193)

---  
*简报生成时间：2026-11-03 | 来源：[github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-03**

---

### **1. 今日亮点**  
Ollama v0.40.14-rc0 修复了关键稳定性问题，包括 CI 中的死锁绕过方案以及改进的 OpenAI 兼容响应处理。新增对 Rockchip NPU 后端的支持，并增强了模型加载诊断功能，反映出硬件支持的日益多样化。多个问题凸显了云访问、基于 GPU 的多模态视觉推理以及量化方面的挑战——尤其是 Qwen3.5 0.8B 模型。

---

### **2. 发布与破坏性变更**  
- **v0.40.14-rc0**:  
  - 通过固定依赖版本解决了 CI 中的 buildx 死锁问题（PR #19373）[github.com/ollama/ollama/pull/19373]  
  - 在 OpenAI 兼容响应中保留可读的 `refusal` 内容（PR #19192）[github.com/ollama/ollama/pull/19192]  
  - 改进 mlx 后端长 top-k 行在分块中的排序（PR #19373）[github.com/ollama/ollama/pull/19373]  

> ⚠️ API 或配置无破坏性变更；本次为聚焦稳定性和正确性的发布候选版。

---

### **3. 新模型与硬件支持**  
- **Rockchip NPU (rknpu)**：已在 Linux 系统上的 RK3588/RK3576 SoC 上原生支持（PR #19480）[github.com/ollama/ollama/pull/19480] — 可将预填充和 MoE 操作卸载至 NPU，同时保持单标记解码在 CPU 执行。  
- **MiMo V2.6**：通过 `dhiltgen/mimo-v2.6` 实验性支持（PR #19400）[github.com/ollama/ollama/pull/19400] — 可通过 `ollama pull` 进行测试。  
- **Qwen3.8-27B hybrid-attention GGUFs**：目前处于调查阶段（Issue #19459）[github.com/ollama/ollama/issues/19459] — 当前因第 64 层缺失 `attn_qkv/attn_gate` 投影而失败。

---

### **4. 性能与优化**  
- **VRAM 高估修复**：PR #19463[github.com/ollama/ollama/issues/19463] 指出 `PredictServerVRAM` 对混合模型（如 Qwen3.5/3.8）的 KV 缓存使用量存在 **4–8 倍**的高估，未考虑 `OLLAMA_KV_CACHE_TYPE`，导致 `keep_alive=-1` 的模型被不必要的驱逐，影响高端系统内存效率。  
- **MLX CUDA 库加载**：PR #19433[github.com/ollama/ollama/pull/19433] 通过将 CUDA 库内联打包解决依赖解析问题，提升了复杂 GPU 环境下 Linux 系统的可靠性。  
- **Unix 域套接字**：功能请求 #19464[github.com/ollama/ollama/issues/19464] 希望在类 Unix 系统上提供进程间通信支持，以降低延迟并增强隔离性。

---

### **5. 稳定性与回归问题**  
| 严重性 | 问题 | 描述 | 修复状态 |
|--------|------|------|----------|
| 🔴 高 | 云访问失败 (#19461)[github.com/ollama/ollama/issues/19461] | 付费用户确认支付后仍无法访问云模型；支持渠道无响应。 | ❌ 未解决 — 关键用户体验/商业化阻塞 |
| 🔴 高 | NVIDIA GPU 上的视觉推理 (#19482, #19483)[github.com/ollama/ollama/issues/19482,19483] | `qwen2.5vl:3b` 在视觉编码器运行于 GPU 时崩溃，提示“令牌重复限制已达到”；在 CPU 上正常工作。 | ❌ 未解决 — 多模态推理回归问题 |
| 🟡 中 | Qwen3.5 0.8B 缺失 Q4_K_M 量化 (#19436)[github.com/ollama/ollama/issues/19436] | 仅 Qwen3.5 0.8B 缺少 `q4_K_M` 量化，限制了 CPU 性能提升。 | ✅ 已请求 — 尚未实现 |
| 🟡 中 | 标记解码错误 (#18785)[github.com/ollama/ollama/issues/18785] | `"python"` 前无空格时解码为空字符串 → 输出中出现无声文本丢失。 | ✅ 已关闭 — 修复已合并 |

---

### **6. 对应用开发者的启示**  
- **云用户**：若您订阅了 Ollama Cloud，遇到支付或访问问题，请直接通过 [hello@ollama.com](mailto:hello@ollama.com) 报告 — 社区支持渠道不可靠。  
- **多模态应用**：在 #19482 修复前，请避免使用 `qwen2.5vl:3b` 搭配基于 GPU 的视觉编码器 — 可回退至纯 CPU 推理或选用替代模型。  
- **性能调优**：对于重度 CPU 工作负载，优先选择 `q4_K_M` 量化（除 Qwen3.5 0.8B 外均可用）；如需，可导入自定义 GGUF 模型。  
- **模型部署**：使用 `ollama ls --cloud`（PR #19153）[github.com/ollama/ollama/pull/19153] 可发现云模型而无需预先拉取 — 提升工作流效率。  
- **本地优先代理**：可考虑集成工具如 **DGC**（PR #19469）[github.com/ollama/ollama/pull/19469]、**Ultimate Assistant**，或 **Herald**（PR #19477）[github.com/ollama/ollama/pull/19477] 以实现本地优先的代理编排。

---  
*数据来源：GitHub — github.com/ollama/ollama — 2026-11-03*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

### **1. 今日亮点**  
LiteLLM 项目正在推进其核心基础设施的升级，重点推动基于 **Rust 的迁移**，目标是实现低于 1ms 的开销，显著提升性能与内存安全性。这一举措详见 [Issue #31263](https://github.com/BerriAI/litellm/issues/31263)，标志着项目长期架构的一次关键转变。与此同时，多项关键修复已落地或处于评审中，包括 `/v1/messages` 接口中流式数据丢失问题的解决、动态密钥更新下的成本追踪优化，以及对百度千帆作为 OpenAI 兼容服务提供商的支持增强。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本。*  

但重大变更正在进行中：  
- **Rust 迁移（进行中）**：一项基础重构，旨在通过 Rust 实现超低延迟推理（开销低于 1ms）。详见 [Issue #31263](https://github.com/BerriAI/litellm/issues/31263) 及相关博客：[LiteLLM Rust 发布](https://docs.litellm.ai/blog/litellm-rust-launch)。早期测试版注册通道 [此处](https://docs.google.com/forms/d/e/1FAIpQLSecWdOjkzjEson2UiZpD...) 开放。  
- **许可证清晰化**：PR ([#34241](https://github.com/BerriAI/litellm/issues/34241)) 旨在明确 MIT 与企业版功能边界，提升合规性与透明度。

---

### **3. 新模型与硬件支持**  
- ✅ **百度千帆（ModelBuilder）**：通过 [PR #49777](https://github.com/BerriAI/litellm/pull/49777) 完全集成为 OpenAI 兼容服务提供商。现已支持自定义部署、正确的 `api_key` 处理及准确的模型路由。  
- 🚧 **LM Studio 上下文窗口同步**：[PR #49597](https://github.com/BerriAI/litellm/pull/49597) 增加对 LM Studio API 实际上下文限制的原生检测，提升本地托管模型（带自定义实例 ID）的准确性。  
- 🔧 **Bedrock Converse 模型**：更新成本映射条目，正确反映 Kimi K2.5、Qwen3 Coder Next、Qwen3 Next 80B 及 Nemotron Nano 3 30B 的真实输入窗口大小（262,144 个 token）([PR #49768](https://github.com/BerriAI/litellm/pull/49768))。

---

### **4. 性能与优化**  
- ⚡ **Rust 迁移（核心举措）**：目标是在代理层实现 **低于 1ms 的开销**。预期收益包括降低延迟、减少内存占用，并在高并发场景下提升可扩展性。  
- 💡 **流式传输效率**：修复确保在 Bedrock 与 Claude Code 模型中，内容块切换（如 `thinking` → `text`）期间不会发生数据丢失([Issue #25214](https://github.com/BerriAI/litellm/issues/25214), [Issue #30765](https://github.com/BerriAI/litellm/issues/30765))。  
- 📈 **成本计算准确性**：如 [#49625](https://github.com/BerriAI/litellm/pull/49625) 等 PR 现已能在提供方不匹配时正确应用自定义部署定价，防止收入流失。

---

### **5. 稳定性与回归问题**  
| 严重性 | 问题 | 描述 | 修复状态 |
|--------|------|-------------|------------|
| 🔴 高 | [Issue #49675](https://github.com/BerriAI/litellm/issues/49675) | v3 速率限制器在上游返回 429 后错误重置 TPM 窗口，导致误判速率限制 | *开放* – 影响限流准确性 |
| 🔴 高 | [Issue #38907](https://github.com/BerriAI/litellm/issues/38907) | 代理在快速序列化路径中剥离了流式响应中的 `delta.reasoning_items` | *已关闭* – 已在 [PR #49402](https://github.com/BerriAI/litellm/pull/49402) 中修复 |
| 🟡 中 | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) | 在请求进行中时执行 `/key/update` 会丢失预算窗口中的成本 | *已关闭* – 修复已合并 |
| 🟡 中 | [Issue #49479](https://github.com/BerriAI/litellm/issues/49479) | `pass_through_endpoints` 会剥离 JSON-RPC 2.0 请求中的 `id`，违反规范 | *开放* – 需要修复 |
| 🟡 中 | [Issue #49617](https://github.com/BerriAI/litellm/issues/49617) | Langfuse OTel 会遗漏 SystemOne 的回答与总 token 数 | *开放* – 影响可观测性 |

---

### **6. 对应用开发者的意义**  
- **预期即将到来的 Rust 代理带来更优的性能与可靠性**——适用于低延迟代理系统与高吞吐量 API。  
- **确保代码正确处理流式传输**——近期修复解决了推理块后首个数据块缺失的问题，避免影响代理状态连续性。  
- **可直接使用新服务商如百度千帆**，通过 OpenAI 兼容端点接入，无需手动绕行。  
- **使用 `litellm config-validate` 与 `token-count` CLI 工具**（正通过 [PR #34772](https://github.com/BerriAI/litellm/pull/34772) 推进）进行预部署验证与离线成本估算。  
- **避免配置陷阱**：注意 `budget_limits` 在请求过程中更新可能不会立即反映在窗口消耗中；尽可能使用稳定的密钥配置。

> ✅ **实用提示**：密切关注 [GitHub Issues](https://github.com/BerriAI/litellm/issues) 与 [PRs](https://github.com/BerriAI/litellm/pulls)——该生态正迅速演进，尤其在流式传输正确性与企业级可观测性方面。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-03**

---

### **1. 今日亮点**  
Unsloth 项目持续快速演进，重点提升了模型服务能力和用户体验，特别是在多模态支持与 UI/UX 精细化方面。值得注意的是，团队已合并多项 PR，实现了通过多模态加载器支持 LFM3-Audio-2B 的聊天功能，并优化了 TTS 音频模型在 GPU 内存受限情况下的尺寸适配。此外，还引入了关键的稳定性修复，包括解决模型加载期间正在运行的对话被静默取消的问题。

---

### **2. 发布与破坏性变更**  
过去 24 小时内未报告任何发布或破坏性变更。未观察到新的版本发布或破坏性的 API/配置更改。

---

### **3. 新模型与硬件支持**  
- ✅ **LFM3-Audio-2B**：通过 `studio/frontend/src/multimodal/loader.ts`（PR #16243）新增文本转语音推理支持。该模型接受音频输入并以文本形式响应，扩展了 Unsloth 的多模态能力。
- 🟡 **AMD RX 5700 XT 上的 Vulkan（Windows 10）**：尽管驱动程序兼容，用户仍报告无法使用 Vulkan 后端（问题 #16395）。暂无修复方案；可能为后端特定内核或驱动绑定问题。
- 🔧 **FP8 量化**：PR #16156 修复了保存 Qwen3 MoE 模型时的 FP8 权重转换不匹配问题，确保去量化后能正确保持 16 位兼容性。

> 🔗 [PR #16243](https://github.com/unslothai/unsloth/pull/16243) | [Issue #16395](https://github.com/unslothai/unsloth/issues/16395)

---

### **4. 性能与优化**  
- ⚙️ **TTS 模型尺寸动态调整**：PR #16514 引入基于实测峰值内存使用量动态调整 audio.cpp TTS 模型大小的功能，使更小的模型即使此前因“单个 GPU 无足够空闲内存”错误而无法运行，现在也能在 GPU 上运行。
- 💡 **内存效率提升**：PR #16476 重构了启动进度测试逻辑，使其独立于主机调度，提升了 CI/CD 流水线中的可靠性（尤其对 Python 3.13）。
- 📈 **KV 缓存增长优化**：PR #16191 改进了急切模式下 KV 缓存的增长逻辑，同时考虑图存储与新生成缓存状态，减少了不必要的过度分配（例如从 1,536 → 3,072 行而非原本的 2,048）。

> 🔗 [PR #16514](https://github.com/unslothai/unsloth/pull/16514) | [PR #16476](https://github.com/unslothai/unsloth/pull/16476) | [PR #16191](https://github.com/unslothai/unsloth/pull/16191)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|---------|------|-------------|------------|
| 🔴 高 | **#16479** | 加载模型时静默取消正在进行的对话；卸载时拒绝操作，除非启用 `force_cancel_active` | ❌ 已开放 |
| 🔴 高 | **#16436** | 生成过程中加载第二个模型失败，提示误导性“内存未释放”错误 | ❌ 已开放 |
| 🟡 中等 | **#16484** | 空闲自动卸载功能失效（未触发） | ❌ 已开放 |
| 🟡 中等 | **#14487** | Windows 平台存在持续内存泄漏（每会话驱动器 C 损失 200–400 MB） | ❌ 已开放 |
| 🟡 中等 | **#15932** | 更新后模型无法加载（如 DeepSeek V4.1 Flash GGUF） | ❌ 已开放 |

> 🔗 [Issue #16479](https://github.com/unslothai/unsloth/issues/16479) | [Issue #16436](https://github.com/unslothai/unsloth/issues/16436) | [Issue #16484](https://github.com/unslothai/unsloth/issues/16484)

---

### **6. 对应用开发者的影响**  
- **构建健壮的模型工作流**：在活跃生成过程中切换模型时需谨慎——当前可能存在静默取消活动会话的情况（问题 #16479），建议在客户端实现轮询或重试逻辑，并配合 `force_cancel_active` 使用。
- **利用新多模态 API**：LFM3-Audio-2B 的集成使得语音输入型 AI 代理成为可能；未来音频增强模型可参考 `multimodal/loader.ts` 的设计模式。
- **优化内存使用**：使用 `idle auto-unload` 设置时需注意——当前行为不一致（问题 #16484）。若可用，请考虑通过 `--auto-unload-timeout` CLI 标志进行控制。
- **关注量化边缘情况**：保存 FP8 模型时，请确保管道正确处理 `.activation_scale` 元数据（PR #16156）；否则下游匹配可能失败。
- **准备 CPU 纯备选方案**：音频/TTS 模型现已更好检测可用的 GPU 内存（PR #16514），但开发者仍应妥善处理 CPU 回退场景。

> 🔗 [PR #16514](https://github.com/unslothai/unsloth/pull/16514) | [Issue #16479](https://github.com/unslothai/unsloth/issues/16479)

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*