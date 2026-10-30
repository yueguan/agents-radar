# AI 基础设施日报 2026-10-30

> 生成时间: 2026-10-30 08:16 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

### **1. 生态系统概览**  
2026年末，AI推理基础设施格局由高吞吐优化、异构硬件支持以及代理驱动工作负载日益复杂化所定义。vLLM和SGLang等项目在分布式服务与推测解码方面持续突破，而llama.cpp和Unsloth则聚焦底层效率与边缘部署。LiteLLM作为关键的统一层，在成本感知与多提供商路由方面发挥重要作用，Ollama则持续弥合开发者易用性与生产级功能之间的差距。生态系统正因后端专业化（NVIDIA CUDA、ROCm/HIP、Apple Silicon及新兴NPU）而愈发碎片化，推动了内核级调优与内存管理领域的激烈竞争。

---

### **2. 活动对比**

| 项目       | 近7天开放问题数 | 近7天合并的PR数 | 近24小时发布数 | 破坏性变更 |
|---------------|------------------------|------------------------|----------------------|------------------|
| **vLLM**      | 38                     | 15                     | 无                   | 无               |
| **SGLang**    | 42                     | 12                     | 无                   | 无               |
| **llama.cpp** | 39                     | 18                     | `b12001`, `b12000`   | 有（`b12000`）   |
| **Ollama**    | 35                     | 10                     | 无                   | 无               |
| **LiteLLM**   | 31                     | 14                     | `v1.109.0-dev.2`     | 有               |
| **Unsloth**   | 33                     | 11                     | 无                   | 有（beta变更）   |

> ✅ *注：所有项目均表现出高度活跃，反映出成熟且快速演进的生态系统。llama.cpp和LiteLLM近期均有发布；LiteLLM因API演进而成为破坏性变更最多的项目。*

---

### **3. 模型支持竞赛**

| 新模型 / 架构 | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------------------------|------|--------|-----------|--------|---------|---------|
| **Qwen3-VL / Qwen3.5**   | ✅ (ViT CUDA图) | 🚧 (进行中) | ❌ | ❌ | ❌ | ❌ |
| **Kimi K3 / Kimi K2.5**  | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ (MLX/LoRA) |
| **DeepSeek-V4.1**        | ✅ (ROCm, TP4调优) | ✅ (HiCache/DSPark) | ✅ (FP8/OCP) | ❌ | ✅ (xAI/grok-4.3) | ❌ |
| **LFM3-Omni / LFM3-1.7B**| ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (开发中) |
| **Metis-1.6**            | ❌ | ❌ | ✅ (混合Mamba-GQA) | ❌ | ❌ | ❌ |
| **Mimo V2.6**            | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Soniox STT (stt-rt-v5)**| ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

> 🏆 **排行榜**：  
> - **llama.cpp** 在*底层模型多样性*（GGUF、Hexagon、OpenCL）方面领先。  
> - **Unsloth** 在*新兴代理模型*（LFM3系列）方面处于领先地位。  
> - **Ollama** 凭借简易的pull命令在*面向用户的模型采用率*上占据主导。  
> - **LiteLLM** 在*新后端多提供商兼容性*（xAI、Azure、Fireworks）方面表现卓越。

---

### **4. 性能前沿**

| 优化重点         | vLLM                  | SGLang               | llama.cpp           | Ollama             | LiteLLM              | Unsloth             |
|----------------------------|------------------------|-----------------------|----------------------|--------------------|-----------------------|----------------------|
| **KV缓存与卸载**     | ✅ 分层，休眠模式   | ✅ HiCache + DCP      | ✅ MoE缓存共享 | ⚠️ KV状态泄漏（ROCm） | ❌ 流式成本损失 | ✅ 自动压缩 |
| **推测解码**   | ✅ 稳定（草稿陈旧度控制） | ✅ HiCache + DCP     | ✅ 增加测试套件 | ❌ 草稿崩溃（问题 #47218） | ❌ 工具调用损坏 | ❌ 不支持 |
| **量化**           | ✅ MXFP4，MoE填充   | ✅ AMD单内核     | ✅ OCP FP8 E4M3      | ❌ 三值请求    | ✅ 令牌计数修复    | ❌ 无量化器API  |
| **分布式服务**    | ✅ PP，TP，FlashInfer   | ✅ 多节点调度器 | ❌ 有限          | ❌ 无扩展        | ✅ 提供商路由   | ❌ 无分布式    |
| **内核级调优**    | ✅ DiffKV，FlashInfer   | ✅ DSpark，AMD内核| ✅ Metal MXFP4       | ❌ GPU初始化错误    | ❌ 无内核控制  | ❌ Vulkan实验性|

> 🔥 **前沿领头羊**：  
> - **vLLM** 在*高吞吐分布式推理*方面领先。  
> - **llama.cpp** 在*跨后端内核创新*（Metal、OpenCL、RDNA4_M）方面领先。  
> - **SGLang** 在*代理性能*（HiCache、DCP、调度器指标）方面领先。

---

### **5. 层级定位**

| 项目       | 主要层级               | 核心差异化 |
|---------------|-----------------------------|--------------------|
| **vLLM**      | **推理引擎**        | 大规模高吞吐服务行业标准；针对NVIDIA GPU和分布式部署进行了优化。 |
| **SGLang**    | **推理引擎 + 代理编排** | 专为代理系统设计；重点关注推测解码、会话感知缓存与调度可观测性。 |
| **llama.cpp** | **本地运行时 / 边缘推理** | 面向CPU/GPU/边缘设备设计；支持离线、轻量级推理，依赖极低。 |
| **Ollama**    | **开发者网关 / 本地CLI** | 本地推理的友好界面；作为具备模型生命周期管理能力的本地LLM服务器。 |
| **LiteLLM**   | **网关 / 成本代理**    | 多提供商路由层，集成统一计费、安全与防护机制——对企业成本管控至关重要。 |
| **Unsloth**   | **代理导向运行时 + UI** | 结合推理引擎与MLX视觉支持、长上下文压缩，以及面向开发者的实时代理工具。 |

> 📌 *这反映出清晰的分层结构：引擎（vLLM/SGLang）、运行时（llama.cpp）、网关（LiteLLM）与应用平台（Ollama/Unsloth）。*

---

### **6. 趋势信号**

#### **从当前活动看新兴趋势**：
1. **以代理为中心的优化**：HiCache、DCP、推测解码与上下文压缩成为讨论焦点——表明*代理工作流*已成为主要负载驱动力。
2. **异构硬件推进**：ROCm、AMD单内核、Apple Silicon及NPU支持已不再是小众需求，而是所有主要项目的开发核心。
3. **成本透明危机**：LiteLLM与Ollama中静默的成本追踪失败揭示出对*多提供商推理中财务问责*的迫切需求。
4. **边缘与移动端扩展**：llama.cpp对Hexagon、OpenCL及RDNA4_M的支持，标志着大模型部署正从数据中心向外延伸。
5. **稳定性优先于功能**：尽管功能快速迭代，但*崩溃修复与回归解决*被优先处理——尤其集中在内存安全、KV缓存与流式传输方面。

#### **应用开发者应关注的重点**：
- **避免静默降级**：警惕CUDA/ROCm崩溃或CPU回退（如Ollama问题 #17380）。
- **验证成本日志**：切勿假设`usage.cost`准确——需审计来自OpenRouter、Gemini和xAI的响应。
- **优先保障代理流程稳定性**：使用`SGLANG_JIT_DEEPGEMM_FAST_WARMUP`，在崩溃修复前避免使用推测解码。
- **规划跨平台可移植性**：利用llama.cpp支持ARM/Adreno/Apple Silicon，使用LiteLLM实现多云成本控制。
- **准备应对破坏性变更**：LiteLLM的`v1.109.0-dev.2`及vLLM即将发布的版本可能需要客户端更新。

> ✅ **最终建议**：构建时选择**vLLM或SGLang**用于可扩展推理，**LiteLLM**用于成本可见性，**llama.cpp**用于边缘部署，**Unsloth**用于原生代理UI——同时将所有输出视为潜在不稳定，直至核心回归问题得到修复。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

### **vLLM Digest — 2026-10-30**

#### **1. 今日亮点**  
vLLM 项目持续深化对下一代多模态与高吞吐量模型的支持，重点推进 ViT CUDA 图优化（#38175）和推测解码稳定性（#62818）。DeepSeek-V4.1 在 ROCm 上的关键性能改进已合并（#63847, #59802），新提交修复了睡眠模式下的内存安全问题（#64630）及基准测试正确性问题（#64703）。

#### **2. 发布与破坏性变更**  
*过去 24 小时内未报告。*  
无新版本发布或破坏性 API/配置变更。

#### **3. 新模型与硬件支持**  
- **模型支持**：  
  - 通过 [RFC #38175](https://github.com/vllm-project/vllm/issues/38175)，多模态模型中的 ViT 编码器（Qwen3-VL、GLM-V、Kimi K2.5、Qwen3.5）已实现完整的 CUDA 图支持。  
  - 为 **DiffusionGemma** 增加流水线并行支持（[PR #52015](https://github.com/vllm-project/vllm/pull/52015)）。  
- **硬件与后端**：  
  - **ROCm (gfx950 / MI355X)**：针对 `amd/Qwen3.8-Flash-Next-Quark-MXFP4` 的性能优化（[Issue #59575](https://github.com/vllm-project/vllm/issues/59575)），包括持久化 topk 内核支持（[PR #63847](https://github.com/vllm-project/vllm/pull/63847)）。  
  - **NVIDIA SM120 (RTX PRO 6000 Blackwell)**：修复 FlashInfer 稀疏 MLA 解码内核缺少 `page_block_size=32` 实例化的问题（[Issue #59203](https://github.com/vllm-project/vllm/issues/59203)）。

#### **4. 性能与优化**  
- **注意力机制与内核**：  
  - 在 SM80 上，使用 `BLOCK_M=128 + KV tile 64` 的 DiffKV 预填充分块可使 200K 上下文块的推理速度提升 **3.0 倍**（[Issue #61095](https://github.com/vllm-project/vllm/issues/61095)）。  
  - 针对 **DeepSeek-V4.1-Flash**，通过专用内核调优，将 TP4 并发度为 1 的解码步骤时间减少（[PR #64364](https://github.com/vllm-project/vllm/pull/64364)）。  
- **内存与卸载**：  
  - 一级睡眠模式现采用精确大小、可复用的固定 CPU 备份，主机内存开销降低最高达 **2 倍**（[PR #64630](https://github.com/vllm-project/vllm/pull/64630)）。  
  - 通过直接生成稠密输出，消除 GLM-5.3-Flash 预填充卷积中的冗余 Q/K/V 复制操作（[PR #61614](https://github.com/vllm-project/vllm/pull/61614)）。  
- **量化**：  
  - 在 SM90 上减少 MXFP4 MoE 的填充开销，提升内存效率（[PR #56697](https://github.com/vllm-project/vllm/pull/56697)）。

#### **5. 稳定性与回归问题**  
- **严重崩溃**：  
  - **Qwen3.8-27B (GDN 混合)** 在 sm_86 上启用前缀缓存时出现非法内存访问（Xid 31）崩溃，自 v0.28.0 版本起引入；*尚未提交修复 PR*（[Issue #63505](https://github.com/vllm-project/vllm/issues/63505)）。  
  - **GLM-5.x (DSA)** 在流水线并行 > 1 时，因阶段开始时层重用导致旧 top-k 索引重复使用（[Issue #63170](https://github.com/vllm-project/vllm/issues/63170)）。*尚未提交修复 PR*。  
- **其他问题**：  
  - **Kimi-K3** 在提示长度恰好为 1536 个 token 时前缀缓存失败（[Issue #50235](https://github.com/vllm-project/vllm/issues/50235)）。  
  - **P2P KV 卸载层级** 在对端故障时未能快速失败（[Issue #63602](https://github.com/vllm-project/vllm/issues/63602)）。  
  - **分层 KV 卸载** 会推动所有等待请求，导致主 DRAM 池被填满（[Issue #49902](https://github.com/vllm-project/vllm/issues/49902)）。

#### **6. 对应用开发者的启示**  
- 使用 **CUDA 图** 加速多模态模型（Qwen3-VL、Kimi）的推理——请确保使用包含 ViT 图支持的近期 nightly 构建版本。  
- 在 **SM120** GPU 上，除非 FlashInfer 内核已修补，否则避免使用 `page_block_size=32`（[#59203](https://github.com/vllm-project/vllm/issues/59203)）。  
- 若使用 **带结构化输出的推测解码**，需警惕草稿过期问题——务必仔细验证语法逻辑。  
- 在 **多 GPU 或去中心化推理** 场景中，请监控各进程的 GPU 内存使用情况（追踪于 [#49816](https://github.com/vllm-project/vllm/issues/49816)），未来版本建议启用每卡指标。  
- **基准测试**：始终确保 `--base-url` 指向服务器根路径，不含 `/v1` 后缀（[PR #64698](https://github.com/vllm-project/vllm/pull/64698)），以避免静默 404 错误。

> 🔗 **关键资源**：  
> - [vLLM GitHub Issues](https://github.com/vllm-project/vllm/issues)  
> - [vLLM PRs](https://github.com/vllm-project/vllm/pulls)  
> - [vLLM Docs – Benchmark CLI](https://github.com/vllm-project/vllm/blob/main/docs/benchmarking/cli.md)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang Digest – 2026-10-30**

---

### **1. 今日亮点**  
SGLang 项目持续强化对高级推理模式的支持，针对 AMD/Ascend 硬件上的 HiCache 与 DSpark 性能问题完成关键修复，并积极推进面向智能体系统的会话感知 KV 缓存编排工作。持续重点关注 CI 稳定性及不稳定的测试问题，其中排名最高的问题 #42752 正追踪 PR 测试流程中反复出现的基础设施故障。

---

### **2. 发布与破坏性变更**  
*无* — 过去 24 小时内未发布新版本或破坏性 API/配置变更。

---

### **3. 新模型与硬件支持**  
- ✅ **AMD ROCm (gfx950 / MI355X)**：通过 Issue #44304 持续优化 `Qwen3.8-Flash-Next`；`--enable-amd-monokernel` 运行时开关现已集成，支持 DeepSeek-V4.1 的关键优化。
- ✅ **平头哥 SAIL (Zhenwu 810/810E/M890P)**：Issue #37519 达成路线图里程碑——上游支持已列为最高优先级功能。
- ✅ **NPU (Ascend A3/DV4)**：DSpark 紧凑模式开发中 (#38440)，动态专家负载均衡 (#47266)，以及 M-RoPE 内核修复 (#44506)。
- ✅ **Apple Silicon (M 系列)**：持续改进 HiCache 与统一基数缓存处理 (#43897, #47422)。

---

### **4. 性能与优化**  
- 🔥 **DeepSeek-V4.1 冷启动加速**：启用 `SGLANG_JIT_DEEPGEMM_FAST_WARMUP` 可实现 **3–5 倍冷启动速度提升**，在分块预填充 16384 场景下无明显运行时性能退化 (#47397)。
- 🚀 **AMD 上的 HiCache + DCP**：PR #45205 实现了在 DSA DCP 中使用 HiCache 支持打包的 MTP 草稿，高并发场景下显著减少冗余预填充（对智能体工作负载至关重要）。
- ⚙️ **调度器指标增强**：PR #46485 新增 `scheduler_iterations_total`，提升调度效率监控的可观测性。
- 📊 **MFU/MBU 带宽修正**：PR #47416 修正了解码读取带宽估算中的过度计数问题，提升了硬件利用率指标的准确性。

---

### **5. 稳定性与回归问题**  
- ⚠️ **严重调度器崩溃（DeepSeek-V4）**：Issue #47218 报告在并发流被中止时，多模态偏移量变为 `None` 导致调度器崩溃——影响全 TP/EP 工作节点并触发容器重启。*尚未提交修复 PR。*
- ⚠️ **HiCache 主机层级命中率下降**：Issue #45400 显示当被驱逐的工作集超过约 3 个长前缀时，主机层级命中率降至零，源于 Mamba 池耗尽。*影响：智能体场景下缓存复用率下降。*
- ⚠️ **CUDA Graph 捕获失败（GLM-5.3-Flash）**：FA4 后端在 SM120（RTX PRO 6000）上混合扩展重塑阶段崩溃；仅 Triton 可用 (#42012)。*对 Flash Attention 用户为高危问题。*
- ⚠️ **CI 不稳定**：Issue #42752 追踪 `PR Test Base/Extra` 运行中持续存在的 CI 基础设施故障——阻碍 PR 验证流程。*暂未识别即时修复方案。*

---

### **6. 对应用开发者的影响**  
构建智能体系统的开发者应优先升级至 `lmsysorg/sglang:dev-dsv41` 或更高版本，以获得 HiCache+DCP 优化及更稳定的调度器支持。生产环境中使用 `SGLANG_JIT_DEEPGEMM_FAST_WARMUP=1` 可有效降低冷启动延迟。对于多模态或高并发应用，请关注流中止时可能引发的崩溃（问题 #47218），若遇到 `None` 偏移错误，可考虑禁用推测解码。在 AMD/Ascend 平台上，启用 `--enable-amd-monokernel` 与 `--enable-eplb` 以获得最佳 MoE 与草稿性能。请密切关注 CI 不稳定性问题——在 #42752 解决前，预计合并 PR 将出现延迟。

👉 [Issue #42752 – CI 不稳定](https://github.com/sgl-project/sglang/issues/42752)  
👉 [PR #47397 – 快速预热](https://github.com/sgl-project/sglang/pull/47397)  
👉 [PR #45205 – AMD 上的 HiCache + DCP](https://github.com/sgl-project/sglang/pull/45205)

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-10-30**

---

### **1. 今日亮点**  
最新版本 `b12001` 为 RDNA4_M（gfx1170/gfx1171）引入了关键的 ROCm/HIP 支持，使底层 GPU 定义与架构的 128b WMMA 能力对齐。与此同时，`b12000` 版本正式落地 OCP FP8 E4M3 量化支持，使 *Qwen3.8-27B-NVFP4* 等模型在推理时实现高效运行——这是推动跨异构后端更广泛采用 FP8 的重要一步。

---

### **2. 发布与破坏性变更**  
- **`b12001`**：为 ROCm/HIP 下的 gfx1170/gfx1171 增加专用的 `RDNA4_M` 家族定义，修正了硬件能力与内核行为之间的错位问题。[PR #31373](https://github.com/ggml-org/llama.cpp/pull/31373)  
- **`b12000`**：启用完整的 OCP FP8 E4M3 支持，包含正确的缩放量生成及 NVFP4→FP8 回退路径的转换修复。[PR #27512](https://github.com/ggml-org/llama.cpp/pull/27512)  
- **迁移提示**：从旧版本升级的用户，若依赖 FP8 E4M3 或 NVFP4 → FP8 路径，应重新使用 `--quantize` 选项转换模型。

---

### **3. 新模型与硬件支持**  
- **模型**：  
  - 通过 GGUF 架构和转换工具链，新增对 **Limite 1B - Violetto** 的支持。[PR #29433](https://github.com/ggml-org/llama.cpp/pull/29433)  
  - 初步支持 **Metis-1.6**（Praxis ~2.94B，Logos ~10.2B），采用混合 Mamba-2 + GQA 注意力机制与固定深度递归。[PR #31134](https://github.com/ggml-org/llama.cpp/pull/31134)  
  - **Mistral Le Chonk** 模型扩展了 DSA 与 sdpark 优化支持。[PR #31376](https://github.com/ggml-org/llama.cpp/pull/31376)  

- **硬件与后端**：  
  - **ROCm/HIP**：全面支持 RDNA4_M（gfx1170/gfx1171）目标平台。[PR #31373](https://github.com/ggml-org/llama.cpp/pull/31373)  
  - **OpenCL**：首次支持 `iq2_s`，包含 AOS/SOA、分块矩阵乘法、DP4A（Adreno）以及窄批次优化。[PR #31267](https://github.com/ggml-org/llama.cpp/pull/31267)  
  - **Hexagon**：基础 `UPSCALE` 操作支持，通过 `vgather` 实现向量化暂存。[PR #31331](https://github.com/ggml-org/llama.cpp/pull/31331)  
  - **Metal**：使用 fp16 位技巧和对齐的 32 位加载优化 MXFP4 矩阵-向量内核；全线程组利用率支持一元/二元/非线性门控单元（GLU）。[PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)  

---

### **4. 性能与优化**  
- **内核级**：Metal MXFP4 矩阵-向量运算已避免查找表，改用对齐字加载，降低延迟。[PR #31075](https://github.com/ggml-org/llama.cpp/pull/31075)  
- **内存**：重用张量暂存缓冲区跳过直接 I/O 乒乓缓冲区，提升 CPU-GPU 数据传输效率。[PR #31321](https://github.com/ggml-org/llama.cpp/pull/31321)  
- **推测解码**：测试套件已添加，用于验证推测解码方法（MTP、DFlash、LiLiCorr）。[PR #31027](https://github.com/ggml-org/llama.cpp/pull/31027)  
- **量化**：OCP FP8 E4M3 在支持硬件上可实现更高吞吐量；早期基准测试显示，在 Qwen3.8-27B 上相比 NVFP4 最高提速达 1.5 倍。  
- **SYCL/XMX-ESIMD**：PR #31195 提出针对 Intel Battlemage GPU 的优化 IQ 量化解码与 XMX 内核——预计可缩小约 35% MoE 提示处理差距。[PR #31195](https://github.com/ggml-org/llama.cpp/pull/31195)

---

### **5. 稳定性与回归问题**  
- **严重**：在 macOS 14 / M2 Ultra 上，Metal 后端因 `unary.metal` 中的 `constexpr bfloat cast` 错误无法初始化。[Issue #31272](https://github.com/ggml-org/llama.cpp/issues/31272)  
- **严重**：所有 `response_format`（JSON schema/object）均导致 LFM3 模型失败，错误信息为“接受片段后意外的空语法栈：6 (30)”。[Issue #31328](https://github.com/ggml-org/llama.cpp/issues/31328)  
- **高优先级**：RTX 5090 笔记本版（sm_120）上存在 CUDA 回归：qwen35 解码性能仅达到理论值的 76%，而 RTX 4090（Linux）可达 86%。[Issue #28196](https://github.com/ggml-org/llama.cpp/issues/28196)  
- **中等**：Arrow Lake iGPU 不支持 SYCL BF16 转换。[Issue #31029](https://github.com/ggml-org/llama.cpp/issues/31029)  
- **修复进行中**：PR #31406 修复重复层的 KV 缓存旋转问题；PR #31368 防止回滚超出 ubatch 快照范围。

---

### **6. 对应用开发者的意义**  
- **部署者**：使用 FP8 E4M3 或 OCP 量化时，请优先选择 `b12000+` 版本——尤其适用于 Qwen3.8 及类似模型。避免使用旧版本，以防 NVFP4 → FP8 转换无声失败。  
- **边缘与移动端**：Hexagon（`UPSCALE`）与 OpenCL（`iq2_s`）支持扩展了在 ARM SoC 及 Adreno 设备上的部署选项。  
- **多 GPU 与云环境**：建议使用 `--moe-cache-cpu-share auto`（PR #31286）动态管理跨 CPU/GPU 层级的 MoE 专家内存——适用于大规模推理场景。  
- **稳定性警告**：在修复合并前，避免在 LFM3 上使用 `response_format`。对于苹果芯片上的 Metal 用户，建议降级至 `b11997` 或手动打补丁。  
- **未来兼容性**：关注 `moe-cache`（SLRU → ClassLRU）与推测解码测试覆盖率相关 PR——这些将影响代理系统中的可扩展性与成本效率。

> 🔗 **官方网址**：[https://llama.app](https://llama.app)  
> 📌 **可信证明**：[GitHub Attestations (61557038)](https://github.com/ggml-org/llama.cpp/attestations/61557038)  
> 🍏 **Apple Silicon 构建**：[macOS Apple Silicon (arm64)](https://github.com/ggml-org/llama.cpp/releases/tag/b12001)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-10-30**

---

### **1. 今日亮点**  
Ollama 生态系统持续扩展其代理与多模态能力，关键贡献集中在工具调用的鲁棒性、OpenAI 兼容性修复，以及对新模型 *Mimo V2.6* 和 *JEV* 的支持。在 ROCm（gfx1151 iGPU）和 CUDA 环境中仍存在严重的稳定性问题，包括内存泄漏和静默回退，而用户对跨平台 GUI 应用的需求依然强烈。

---

### **2. 发布与破坏性变更**  
过去 24 小时内未报告任何发布或破坏性变更。  
- **注意**：当前正在使用版本 `0.40.13`（依据 Issue #19383），但近期未发布正式的发布说明或迁移指南。

---

### **3. 新模型与硬件支持**  
- ✅ **新模型支持**：  
  - [`dhiltgen/mimo-v2.6`](https://github.com/ollama/ollama/pull/19400)：为 MiMo V2.6 增加 MLX 与 GGUF 支持。可通过 `ollama pull dhiltgen/mimo-v2.6` 获取。  
  - [`dhiltgen/jev`](https://github.com/ollama/ollama/pull/19399)：通过 MLX 初步支持 JEV 决策与视觉模型，包含提示编码与 API 路由功能。  
- 🚧 **硬件/后端更新**：  
  - ROCm 后端因在 AMD Strix Halo（gfx1151 iGPU）上出现 GPU 状态泄漏问题而受到关注 —— 参见 [Issue #17847](https://github.com/ollama/ollama/issues/17847)。  
  - 有用户请求为 Bonsai 2 添加三元量化支持（[#19388](https://github.com/ollama/ollama/issues/19388)），表明对高级量化方案的兴趣日益增长。

---

### **4. 性能与优化**  
- 🔧 **模型加载与内存管理**：  
  - PR [#19284](https://github.com/ollama/ollama/pull/19284)：防止在预测 GPU 内存时误将仅 CPU 运行的模型（如嵌入模型）驱逐 —— 提升混合负载系统的效率。  
  - PR [#17615](https://github.com/ollama/ollama/pull/17615)：修复 GraphSize 与 PredictServerVRAM 之间 KV 缓存计数不一致的问题 —— 避免内存预测错误。  
- ⏱️ **延迟与流式处理**：  
  - PR [#19396](https://github.com/ollama/ollama/issues/19396)：提出 LFM3-Omni 模型原生流式音频输出方案 —— 实现无需外部处理的实时语音合成响应。  
  - PR [#19390](https://github.com/ollama/ollama/issues/19390)：请求在 Windows 上原生支持视频/音频输入输出 —— 对多模态代理工作流至关重要。

---

### **5. 稳定性与回归问题**  
**严重**：  
- [Issue #17847](https://github.com/ollama/ollama/issues/17847)：**ROCm（gfx1151 iGPU）在请求间泄露 KV 状态** —— 前次请求的响应内容污染当前请求。严重程度高；影响推理正确性。尚未提交修复 PR。  
- [Issue #17380](https://github.com/ollama/ollama/issues/17380)：预热阶段间歇性出现 `CUDA error: shared object initialization failed`，静默回退至 CPU —— 导致性能下降且难以调试。

**高影响**：  
- [Issue #19385](https://github.com/ollama/ollama/issues/19385)：用户在更新后无法回滚模型（例如 Qwen3:4b）—— 急需实现版本回滚机制。  
- [Issue #19375](https://github.com/ollama/ollama/issues/19375)：大型 MoE 模型（如 gpt-oss:120b）因张量大小溢出（`ffn_down_exps.weight`）导致 GGUF 元数据读取失败 —— 阻止模型加载。

**正在处理稳定性的开放 PR**：  
- [#17566](https://github.com/ollama/ollama/pull/17566)：思考过程的令牌预算控制 —— 防止推理失控。  
- [#17564](https://github.com/ollama/ollama/pull/17564)：防止传递不完整的工具调用 —— 对代理可靠性至关重要。

---

### **6. 对应用开发者的启示**  
- **代理开发**：预计工具调用处理将更加脆弱（尤其是 `qwen3.8:27b-mlx` 与 `qwen3.5:4b`），若助手先生成非空文本，则 `tool_calls` 会被丢弃（[#19353](https://github.com/ollama/ollama/issues/19353)）。在修复落地前，请显式使用 `content: ""` + `tool_calls`。  
- **OpenAI 兼容性**：对 `content: []` 数组保持谨慎 —— 可能导致 `tool_calls` 错误移动（[#19152](https://github.com/ollama/ollama/pull/19152)）。此外，当无模型存在时，`/v1/models` 返回 `null` 而非 `[]` —— 强类型 SDK 中可能引发兼容性问题（[#19381](https://github.com/ollama/ollama/issues/19381)）。  
- **多模态工作流**：原生音视频支持备受期待（[#19390](https://github.com/ollama/ollama/issues/19390)）—— 可考虑基于 LFM3-Omni 的流式 TTS 提案（[#19396](https://github.com/ollama/ollama/issues/19396)）构建替代方案。  
- **部署策略**：在 [issue #17847](https://github.com/ollama/ollama/issues/17847) 解决前，避免在 gfx1151 硬件上使用 ROCm。在 CUDA 环境中，需留意预热阶段的静默 CPU 回退（[#17380](https://github.com/ollama/ollama/issues/17380)）。

> 🔗 **保持关注**：请在 [github.com/ollama/ollama](https://github.com/ollama/ollama) 关注关键问题与 PR。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

### **1. 今日亮点**  
LiteLLM 最新更新聚焦于关键的成本核算准确性、流式传输的可靠性，以及对 Kimi K3 和 Soniox 实时语音识别（STT）等新兴模型的更好支持。关键修复解决了 OpenAI 兼容上游服务中的无声支出损失问题（`#47533`, `#38507`），以及 Gemini 流式传输中工具调用上下文损坏的问题（`#25869`）。代理现在仅在实际运行时保留 `x-litellm-applied-guardrails` 头部（`#48472`），并正确在 Prometheus 指标中报告实时密钥支出（`#49199`）。

---

### **2. 发布与破坏性变更**  
- **v1.109.0-dev.2** 已发布，安全性增强：所有 Docker 镜像现通过 [cosign](https://docs.sigstore.dev/cosign/overview/) 使用 [提交 `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 中的密钥进行签名。  
- **破坏性变更**：`/v1/responses` 流式响应现在发出命名的 SSE 事件（如 `event: content`），而非 `[DONE]` —— 依赖 `[DONE]` 的客户端必须更新逻辑 ([PR #47980](https://github.com/BerriAI/litellm/pull/47980))。  
- **破坏性变更**：仅数据库管理的 `general_settings` 键值现在被忽略，除非由设置 API 管理 ([PR #48967](https://github.com/BerriAI/litellm/pull/48967))。

---

### **3. 新模型与硬件支持**  
- **原生支持请求中**：Kimi K3（Moonshot AI）、Inkling（Thinking Machines Lab）及 Tinker 平台 ([Issue #33921](https://github.com/BerriAI/litellm/issues/33921))。  
- **Soniox 实时流式语音识别（stt-rt-v5）** 已加入代理请求列表 ([Issue #43677](https://github.com/BerriAI/litellm/issues/43677))。  
- **Gemini Live 会话初始化** 现在在首次内容输出时发送，确保 `session.update` 被正确应用 ([PR #39864](https://github.com/BerriAI/litellm/pull/39864))。  
- **Azure AI Foundry grok-4-1**、**xAI grok-4.3** 以及 **Fireworks DeepSeek V4.1** 的工具请求现已按各供应商计费方式独立统计 ([PR #49074](https://github.com/BerriAI/litellm/pull/49074))。

---

### **4. 性能与优化**  
- **延迟与吞吐量**：Vertex AI 透传认证现在每调用一次执行一次（此前为两次），降低延迟并提升预算追踪精度 ([PR #48942](https://github.com/BerriAI/litellm/pull/48942))。  
- **令牌计数准确性**：工具调用的令牌计数现已与 Azure 及 xAI 计费一致（此前最多偏差达 300 个令牌）——对上下文窗口控制至关重要 ([PR #49074](https://github.com/BerriAI/litellm/pull/49074))。  
- **部署缓存**：SAP 提供商的部署现在基于 `(base_url, resource_group)` 进行进程级缓存，实现更快的解析 ([PR #48597](https://github.com/BerriAI/litellm/pull/48597))。

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 影响 | 修复状态 |
|---------|------|--------|------------|
| 🔴 严重 | OpenRouter Responses API (`call_type=aresponses`) 成本追踪静默失败 —— 尽管实际使用，支出仍记录为 $0 ([#38507](https://github.com/BerriAI/litellm/issues/38507)) | 实际成本未反映在预算中；存在财务审计风险 | ❌ 尚未修复 |
| 🔴 严重 | 信任上游返回的 `usage.cost` 值原样不校验单位 → 产生虚增支出 ([#35829](https://github.com/BerriAI/litellm/issues/35829)) | 若上游返回非美元金额，可能导致灾难性超额计费 | ❌ 尚未修复 |
| 🟡 高 | 流式传输在最终块 `choices` 非空时丢失上游 `usage` → 缓存令牌丢失，输入全部计费 ([#36168](https://github.com/BerriAI/litellm/issues/36168)) | 缓存响应成本报告不准确 | ✅ PR 已开放：[#49069](https://github.com/BerriAI/litellm/pull/49069) |
| 🟡 高 | `stream_chunk_builder` 损坏 Gemini `server_side_tool_invocations` → 后续调用出现“工具调用上下文损坏” ([#25869](https://github.com/BerriAI/litellm/issues/25869)) | 使用工具调用 + 网络搜索的智能体工作流中断 | ✅ PR 已开放：[#49061](https://github.com/BerriAI/litellm/pull/49061) |
| 🟡 中等 | 代理关闭时内存中的支出缓冲区丢失 ([#34805](https://github.com/BerriAI/litellm/issues/34805)) | 重启或崩溃时存在数据丢失风险 | ✅ PR 已开放：[#49199](https://github.com/BerriAI/litellm/pull/49199) |

---

### **6. 对应用开发者的启示**  
- **审计你的成本追踪**：若使用 OpenRouter、OmniRoute 或任何 OpenAI 兼容网关，请验证 `response_cost` 是否真实反映使用量 —— 当前版本在非流式模式下可能因未处理 `usage.cost` 导致记录为 $0（[#47533](https://github.com/BerriAI/litellm/issues/47533)）。  
- **避免无声超账单**：永远不要假设 `usage.cost` 是美元单位 —— 在 [#35829](https://github.com/BerriAI/litellm/issues/35829) 修复前，务必在客户端实现校验逻辑。  
- **更新流式客户端**：若依赖 `/v1/responses` 返回的 `[DONE]` 事件，请立即迁移到命名事件处理（`event: done`）([PR #47980](https://github.com/BerriAI/litellm/pull/47980))。  
- **使用更新的工具链**：对于在 Gemini 上使用工具调用 + 网络搜索的智能体，预计在 [#49061](https://github.com/BerriAI/litellm/pull/49061) 合并前会出现功能异常 —— 请避免使用 `stream=True` 直至补丁发布。  
- **安全存储密钥**：通过 [`#28994`](https://github.com/BerriAI/litellm/issues/28994) 使用 `secrets_from_file` 来集成 Docker 秘密或 Kubernetes。

> ✅ **建议**：升级至 `v1.109.0-dev.2` 并验证成本日志，尤其针对高并发或预算敏感型负载。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-10-30**

---

### **1. 今日亮点**  
Unsloth 项目持续扩展对新兴大模型架构的支持，当前正在积极开发 LFM3 系列，并通过 MLX 改进对多模态模型的处理能力。重点 PR 聚焦于内存估算精度（尤其针对 Apple Silicon）、跨标签页与平台的 UI 稳定性，以及长对话场景下的上下文管理优化——这些对于真实世界中的智能体应用至关重要。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内无新发布。*  
然而，`main` 分支中正在进行的变更可能影响从 v0.1.911-beta 升级至 v0.1.912-beta 的用户：  
- 模型加载行为已发生细微变化，特别是针对在 MLX 上的 GGUF 与 safetensors 模型（参见 [PR #15945](https://github.com/unslothai/unsloth/pull/15945)）。  
- `/v1/models` 接口现已正确列出自动映射的 safetensors 检查点（修复后）[PR #15945](https://github.com/unslothai/unsloth/pull/15945)，解决了此前不可见的问题。

---

### **3. 新模型与硬件支持**  
- ✅ **LFM3 系列**：`LFM3-1.7B` 与 `LFM3-Omni` 的支持正在积极开发中 ([Issue #15588](https://github.com/unslothai/unsloth/issues/15588), [PR #16053](https://github.com/unslothai/unsloth/pull/16053))。  
- 📌 **MLX 多模态模型**：在 Apple Silicon 上改进了长上下文多模态聊天的处理能力 ([PR #16079](https://github.com/unslothai/unsloth/pull/16079))，并支持在图像生成中使用 GGUF LoRA ([PR #16053](https://github.com/unslothai/unsloth/pull/16053))。  
- 🔧 **Vulkan 后端**：报告称在 Vulkan（AMD GPU）上实验性支持 MiniCPM-V-4.7-1B，但渲染仍不稳定 ([Issue #14987](https://github.com/unslothai/unsloth/issues/14987))。  
- 💻 **Windows + AMD**：用户报告在加载 GGUF 模型时存在模型加载失败及高内存占用问题 ([Issue #16015](https://github.com/unslothai/unsloth/issues/16015))。

---

### **4. 性能与优化**  
- ⚙️ **内存估算修复**：MLX 内存估算器现在即使在未量化的情况下也考虑完整宽度注意力分数，修正了过度估算问题（例如，报告 69.7 GiB 而实际仅需 24 GB GPU 显存 — [Issue #15935](https://github.com/unslothai/unsloth/issues/15935))。  
- 🚀 **上下文管理**：已修复防止上下文溢出问题，并支持长对话中的自动压缩功能 ([PR #15900](https://github.com/unslothai/unsloth/pull/15900), [Issue #15800](https://github.com/unslothai/unsloth/issues/15800))。  
- 🔄 **流水线并行**：GradScaler 同步修复确保多 GPU 训练中各流水线阶段梯度缩放一致 ([PR #16070](https://github.com/unslothai/unsloth/pull/16070))。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重程度 | 状态 | 相关 PR |
|------|----------|--------|------------|
| 模型卸载时出现核心转储（LLaMA/Server 日志缺失） | 严重 | 已关闭 | [Issue #14411](https://github.com/unslothai/unsloth/issues/14411) |
| AppImage 在 Linux 上界面卡顿 | 高 | 开放 | [Issue #15999](https://github.com/unslothai/unsloth/issues/15999) |
| 聊天标签页间文本重复 | 中等 | 已关闭 | [PR #15795](https://github.com/unslothai/unsloth/pull/15795) |
| Dolphin Venice Edition 在 UI 中渲染异常 | 中等 | 已关闭 | [Issue #15858](https://github.com/unslothai/unsloth/issues/15858) |
| LFM3-Omni 视频输入在 Windows + AMD 上失败 | 中等 | 开放 | [Issue #16065](https://github.com/unslothai/unsloth/issues/16065) |
| LFM3-Omni safetensors 无法加载 | 中等 | 开放 | [Issue #16054](https://github.com/unslothai/unsloth/issues/16054) |

> 注：多个回归问题源于近期对 UI 与后端的重构；相关修复已在 Studio 与 MLX 流水线中优先处理。

---

### **6. 对应用开发者的意义**  
- **用例规划**：对于需要长上下文连续性的生产级智能体（如法律或研究助理），请依赖 `auto-compaction` 功能，并通过更新后的内存追踪工具监控上下文大小 ([Issue #16055](https://github.com/unslothai/unsloth/issues/16055))。  
- **模型选择**：在 Apple Silicon 上使用 MLX 时，优先选用 `LFM3` 与 `Dolphin` 模型——性能预期更优，但需验证其与本地 GGUF/LoRA 工作流的兼容性。  
- **部署注意事项**：在 [PR #15994](https://github.com/unslothai/unsloth/pull/15994) 合并前，避免下载超过 50GB 的 ModelScope 模型；建议改用直接 Hugging Face 链接。  
- **调试提示**：若模型在对话中途崩溃，请检查 `coredumpctl list` 并确认日志已开启——近期核心转储缺乏可追溯性 ([Issue #14411](https://github.com/unslothai/unsloth/issues/14411))。  

> ✅ **建议**：关注 `unslothai/unsloth#main` 以获取即将发布的 v0.1.912 版本候选，多项稳定性与内存优化已排队待合并。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*