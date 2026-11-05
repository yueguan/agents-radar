# AI 基础设施日报 2026-11-05

> 生成时间: 2026-11-05 02:44 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI基础设施生态报告 – 2026-11-05**

---

### **1. 生态概览**  
2026年末，AI推理基础设施领域呈现出高性能服务引擎、轻量级本地运行时与智能网关之间的快速融合态势——由下一代模型（如DeepSeek-V4、Qwen3.8-Flash-Next）和异构硬件（Blackwell、MI355X、Apple Silicon、RTX 5090）共同驱动。各项目正愈发聚焦于**分布式推理稳定性**、**量化效率**以及**代理原生能力**，并明显转向对模型执行的端到端控制，涵盖从内核级优化到UI集成的文件交互。研究级模型与生产就绪部署之间的差距持续缩小——但代价是工具链与调试复杂度的显著上升。

---

### **2. 活跃度对比**

| 项目       | 开启问题（严重/高优先级） | 最近7天合并的PR | 发布状态       |
|---------------|-----------------------------|------------------------|------------------------|
| **vLLM**      | 12 (4 严重)             | 14                     | 稳定版：v0.32.0 (⚠️ 存在缺陷) |
| **SGLang**    | 14 (4 严重)             | 10                     | 尚无稳定版本      |
| **llama.cpp** | 12 (3 严重)             | 16                     | **v0.9.0 (b12110)** 已发布 |
| **Ollama**    | 10 (3 高)                 | 12                     | 无新版本发布         |
| **LiteLLM**   | 12 (4 高)                 | 15                     | `v1.108.0` 即将发布（RC3） |
| **Unsloth**   | 12 (4 高)                 | 13                     | **v0.1.915-beta** 已发布 |

> ✅ *观察：* **llama.cpp** 在发布速度上领先，推出v0.9.0重大版本；**SGLang** 与 **Unsloth** 尽管尚未发布稳定版，但表现出强劲的测试版活跃度。**vLLM** 在严重问题追踪方面仍最为活跃。

---

### **3. 模型支持竞赛**

| 新模型 / 架构     | vLLM       | SGLang     | llama.cpp       | Ollama        | LiteLLM       | Unsloth         |
|-------------------------------|------------|------------|------------------|---------------|----------------|------------------|
| **DeepSeek-V4-Flash/DSv4.1**  | ✅ 活跃 (SM8x/SM120) | ❌          | ❌               | ❌            | ❌             | ❌               |
| **Qwen3.8-Flash-Next**        | 🚧 ROCm/GPU | ✅ ROCm/MoE | ✅ FP8/OCP       | ✅ MXFP8      | ✅ 迁移中   | ✅ 原生支持 |
| **MiniMax-M3.1**              | 🚧 ROCm     | ❌         | ❌               | ❌            | ❌             | ❌               |
| **Apple Silicon (MLX)**       | ⚠️ 测试     | ✅ RFC + 开发 | ❌               | ✅ YaRN/YaRN  | ❌             | ✅ 完全支持 |
| **Intel Xeon (SPR)**          | ❌          | ✅ CBB模式 | ❌               | ❌            | ❌             | ❌               |
| **AMD MI355X (gfx950)**       | ✅ ROCm性能 | ✅ MoE/QSA | ❌               | ❌            | ❌             | ❌               |
| **NPU (Ascend)**              | ❌          | ✅ 采样掩码 | ❌ | ❌            | ❌             | ❌               |

> 🏆 **排行榜**：  
> - **Unsloth** 在**本地代理就绪性**方面领先（文件I/O、持久上下文、浏览器集成）。  
> - **Qwen3.8-Flash-Next** 在 **llama.cpp**、**Ollama** 和 **SGLang** 中拥有最广泛的原生支持。  
> - **vLLM** 在**企业级分布式推理**方面领先（P/D解耦、多节点稳定性）。

---

### **4. 性能前沿**

| 优化重点           | vLLM                          | SGLang                        | llama.cpp                    | Ollama                     | LiteLLM                      | Unsloth                     |
|-------------------------------|-------------------------------|-------------------------------|------------------------------|----------------------------|------------------------------|-----------------------------|
| **KV缓存效率**       | ✅ 仅后向尾部管理，DFlash/DSpark | ❌ (长prefill崩溃)       | ❌                            | ⚠️ 虚拟显存使用被高估 | ❌                            | ❌                           |
| **批处理与吞吐量**     | ✅ 多序列Flash Attention | ✅ 动态形状CPU图   | ✅ 批处理张量API（WebGPU） | ✅ `batch-size` + 流式输出 | ✅ Fanout.gather（可扩展性） | ✅ RAG批处理嵌入（快约48倍） |
| **量化**              | ✅ FP8动态融合，LoRA压缩 | ✅ MXFP4，QSA                | ✅ OCP FP8 E4M3，GGML批处理 | ✅ MXFP8选择性 | ❌                             | ✅ 默认启用FP8     |
| **分布式服务**       | ✅ P/D解耦（存在关键缺陷） | ❌ 解耦数据损坏 | ❌                             | ❌                           | ✅ 控制平面透传 | ❌                           |
| **内核级调优**       | ✅ Triton融合，MoE回退 | ✅ HIP特定解码，DCP路由 | ✅ OpenCL/Adreno加速 | ❌                           | ✅ Rust路由器核心           | ✅ CUDA图捕获（默认开启） |

> 🔥 **关键洞察：** **vLLM** 在**分布式系统调优**方面占据主导；**Unsloth** 在**通过CUDA图与RAG批处理实现本地性能飞跃**方面领先；**llama.cpp** 在**跨平台内核优化**方面表现卓越。

---

### **5. 层级定位**

| 项目       | 主要层级                  | 次要角色                              | 核心差异化                                  |
|---------------|--------------------------------|----------------------------------------------|-----------------------------------------------------|
| **vLLM**      | **推理引擎**           | 模型服务、分布式服务           | 可扩展高吞吐大语言模型服务的行业标准 |
| **SGLang**    | **模型网关 / 编排器** | 代理工作流、混合执行             | 强调代理逻辑，跨平台运行时支持 |
| **llama.cpp** | **本地运行时 / 边缘推理** | 独立推理、WebGPU/OpenCL支持  | 通用后端支持，依赖极低     |
| **Ollama**    | **开发者网关 / CLI工具** | 本地模型运行器、云同步               | 开发者友好的无缝体验，但控制力较弱       |
| **LiteLLM**   | **控制平面 / API网关** | 成本追踪、防护机制、审计日志     | 合规优先设计，支持SOC 2可观测性 |
| **Unsloth**   | **代理运行时 / 本地UI**   | 微调流程、RAG、文件交互  | 首个支持浏览器文件访问的本地代理平台 |

> 💡 **战略启示：** 技术栈正逐步成熟为**专业化层级**——从**引擎**（vLLM）、**网关**（LiteLLM）、**运行时**（Unsloth）到**编排器**（SGLang）——开发者应根据部署规模、合规需求及代理复杂度进行选型。

---

### **6. 趋势信号**

1. **代理原生基础设施已成为核心**  
   - Unsloth 的文件 I/O、持久上下文与 `search_conversation` 工具表明，**本地代理需要类操作系统级别的能力**，而不仅仅是模型推理。  
   - 开发者应预期未来框架将**文件系统访问、任务调度与内存持久化**作为基础功能。

2. **ROCm 与 AMD 不再是小众选择**  
   - 多个项目（vLLM、SGLang、llama.cpp）正积极针对 **MI355X/gfx950** 优化——表明 AMD 在企业级 AI 集群中的角色日益重要。  
   - 未来将出现更多针对 AMD 架构的**模型专属内核补丁**（例如 #65530）。

3. **合规驱动的网关正在崛起**  
   - LiteLLM 的 WORM 存储、Splunk/Sentinel 集成与审计追踪表明，**治理已不再是事后补充**。  
   - 应用开发者在选择网关时，必须考虑 **SOC 2、数据溯源与防篡改日志**。

4. **量化正迈向平台感知**  
   - Ollama 的选择性 MXFP8、llama.cpp 的 OCP FP8、vLLM 的动态 FP8 融合，表明**量化正从通用格式转向硬件优化、模型感知的策略**。

5. **生产环境中稳定性 > 速度**  
   - 尽管优化力度空前，但**严重回归问题仍主导问题追踪列表**（如 vLLM 的 NixlConnector 竞态条件、SGLang 的长 prefll 内存溢出崩溃）。  
   - **避免在生产环境使用测试版**——优先保障验证、CI 可靠性与可复现性。

---

### ✅ **面向应用开发者的建议**  
- **对于代理场景**：选择 **Unsloth** 或 **SGLang**，以获得丰富的本地工作流与可编程缓存策略。  
- **对于可扩展服务**：使用 **vLLM**（但请避开 v0.32.0，待修复落地后再用）。  
- **对于高合规要求的应用**：采用 **LiteLLM**，搭配签名镜像与审计日志。  
- **对于边缘或本地部署**：**llama.cpp v0.9.0** 提供最佳的可移植性与量化灵活性。  
- **用于快速原型**：**Ollama** 依然理想——但务必在 RTX 5090/WSL2 上仔细测试工具调用。

> 📌 **最终提醒：** “直接部署一个模型”的时代已经结束。如今的成功取决于**分层、可审计、代理感知的基础设施**——选择技术栈时，不仅要考量性能，更要关注**稳定性、合规性与可扩展性**。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-05

---

### **1. 今日亮点**  
vLLM 项目持续加速对下一代模型与硬件的支持，当前重点推进 **DeepSeek-V4-Flash/DSv4.1** 在 SM8x（Ampere）和 SM120（Blackwell）GPU 上的兼容性。核心工作聚焦于 **P/D 分离架构的稳定性**，尤其关注 `NixlConnector` 竞态条件以及多节点部署中的 KV 缓存损坏问题。与此同时，针对 Qwen3.8-Flash-Next 与 MiniMax-M3.1 的 ROCm 性能优化在 gfx950/MI355X 平台上进展迅速。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
今日未发布新版本或引入破坏性 API/配置变更。最新稳定版本仍为 **v0.32.0**，但已报告多个回归缺陷（如 #63343、#65618）。

---

### **3. 新模型与硬件支持**  
- ✅ **DeepSeek-V4-Flash / DeepSeek-V4.1-Flash**: 正在通过 Issue #50576（128 条评论）推进 **SM8x（Ampere: A100/A800, RTX 30xx）** 支持，目前因 DSpark 草稿逻辑中缺少 `packed_modules_mapping` 而受阻（#65618）。  
- 🚧 **Qwen3.8-Flash-Next (gfx950 / MI355X)**: 性能优化路线图正在推进中，相关 Issue #59575；PR #65530 与 #65481 专注于瓦片调优与稀疏解码拆分。  
- 🚧 **MiniMax-M3.1 (ROCm/gfx950)**: 优化工作进行中（PR #65530, #65481）；重点聚焦 NVFP4 稀疏解码与 M=17–256 GEMM 瓦片划分。  
- ⚠️ **Intel GPU (XPU)**: PR #65788 中正在进行注意力核的测试启用，旨在拓展更广泛的硬件覆盖范围。  

> 🔗 [Issue #50576](https://github.com/vllm-project/vllm/issues/50576) | [PR #65788](https://github.com/vllm-project/vllm/pull/65788)

---

### **4. 性能与优化**  
- 🔥 **Triton 内核融合**: PR #64212 增加了 `gelu_tanh_and_mul_per_token_quant` 内核，支持动态 FP8 量化，提升 Gemma3/4 模型中 MLP 融合效率。  
- 💡 **MoE 优化**: PR #65790 在使用 `--moe-backend deep_gemm` 且 M 较小时启用 Triton 回退机制，避免 DeepGEMM 连续布局带来的开销。  
- 📈 **KV 缓存效率**: PR #64109 为 DFlash/DSpark 草稿滑动窗口层引入仅后向尾部管理，降低内存压力。  
- 🛠 **LoRA 性能**: PR #64502 增加多 CTA LoRA 压缩内核（`VLLM_LORA_SHRINK_USE_MULTICTA=1`），消除浮点原子操作与部分归约缓冲区。  
- 🎯 **ROCm 调优**: 多个 PR（如 #65530、#61119）优化 MiniMax-M3.1 与 Qwen3-Next 在 gfx950 上的 GEMM 瓦片划分，并推迟 all-reduce 操作。  

> 🔗 [PR #64212](https://github.com/vllm-project/vllm/pull/64212) | [PR #65790](https://github.com/vllm-project/vllm/pull/65790) | [PR #65530](https://github.com/vllm-project/vllm/pull/65530)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|---------|-------|-------------|------------|
| 🔴 严重 | [#61160](https://github.com/vllm-project/vllm/issues/61160) | P/D 分离架构中自动前缀缓存导致共享前缀的 KV 重用，引发跨请求上下文污染 | 开放 — 在 DeepSeek-V4.1-Flash 上可复现 |
| 🔴 严重 | [#61229](https://github.com/vllm-project/vllm/issues/61229) | NixlConnector：远程 READ 期间释放 KV 块 → 解码阶段消费被覆盖数据 | 开放 — 存在无声损坏风险 |
| 🟡 高 | [#62383](https://github.com/vllm-project/vllm/issues/62383) | Qwen4Exp 单次预填充 >262K token 时在 v0.32.0 下触发 CUDA 非法内存访问崩溃 | 开放 — 已确认为相比 v0.31.1 的回归问题 |
| 🟡 高 | [#65618](https://github.com/vllm-project/vllm/issues/65618) | DSpark 草稿缺少 `packed_modules_mapping` → DeepSeek-V4.1-Flash 接受率降为 0% | 开放 — 阻碍推测解码 |

> ⚠️ 以上回归问题影响使用 P/D 分离架构及大上下文模型的生产级推理流水线。

---

### **6. 对应用开发者的影响**  
- 若使用 DeepSeek-V4.1-Flash 或大上下文 Qwen4Exp 模型，请**避免使用 v0.32.0**，建议临时回退至 v0.31.1，直至 #65618 和 #62383 修复。  
- 若在 P/D 分离部署中遇到跨请求干扰，请启用 `--no-enable-prefix-caching`（参考 #61160）。  
- 在多节点部署中注意监控 NixlConnector 不稳定性 — 除非已验证环境，否则避免在异步 KV 拉取时使用 `--enforce-eager`。  
- **利用新兴的 MoE 与 LoRA 优化**（PR #65790、#64502），在搭载 H200/Hopper 系统的密集或 MoE 模型上获得更高吞吐。  
- **准备应对 ROCm 特定调优** — 未来在 AMD MI355X 上的性能提升需依赖模型特定的内核补丁（如 #65530）。  

> ✅ 实用提示：提交问题时请使用 `collect_env.py` 并提供完整环境信息 —— 许多回归问题源于 vLLM、FlashInfer 与 CUDA/ROCm 工具链之间的版本不匹配。

---  
*摘要生成时间：2026-11-05 | 来源：[vLLM GitHub](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 消息简报 – 2026-11-05

---

### **1. 今日重点**  
SGLang 生态系统持续成熟，跨平台稳定性与性能优化工作持续推进，尤其聚焦于 AMD ROCm 与 Apple Silicon。关键的 CI/CD 可靠性问题（#42752）仍未解决，影响了 PR 验证速度。显著进展包括集成 Kimi K3 的原生 MLA 与实时恢复钩子（#48510），以及修复多模态特性通过 CUDA VMM 传输时导致 GPU 内存耗尽的问题（#48507）。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告新发布或破坏性变更。*  
无新版本发布，亦无破坏性 API/配置变更。项目仍处于积极开发中，暂无稳定版本更新。

---

### **3. 新模型与硬件支持**  
- **AMD ROCm (gfx950 / MI355X)**：针对 `Qwen3.8-Flash-Next` 的 MoE、QSA 与 MXFP4 支持，持续进行性能优化（#44304）。  
- **Apple Silicon (M 系列)**：RFC #32321 提出对 Torch 所有 SRT 路径的重构设计，并引入导出的 MLX 模型区域——迈向原生 Apple Silicon 推理的关键一步。  
- **NPU (Ascend)**：通过 #43517 增加采样掩码支持，使强化学习代理可通过标准 API 访问 logprobs。  
- **Intel Xeon (SPR)**：CPU 核心绑定现已支持下一代 Xeon 的 Compute Building Block (CBB) 模式（#45717）。  
- **扩散模型**：正在开发中，增强对 Wan VAE 解码及模型校验和验证的支持（PR #48225）。

---

### **4. 性能与优化**  
- **AMD DCP 预填充**：PR #48505 引入按线路字节数路由机制，优化 DCP 预填充延迟并减少冗余通信。  
- **ROCm 预测解码**：#48090 在拒绝采样过程中跳过不必要的词汇级排序，提升 HIP 上的效率。  
- **CPU 图计算**：#44394 为 Intel AMX 系统新增动态形状回退图与静态分桶机制，提升预填充/解码吞吐量。  
- **Hopper (SM90)**：跟踪问题 #45373 专注于 GLM-5.3-Flash 的内核优化；@BBuf 正在进行原型验证。  
- **Blackwell (SM120)**：DeepSeek-V4 预填充吞吐量（4× RTX PRO 6000 上约 2–7K tok/s）落后于 vLLM/Marlin（约 12.5K tok/s）；问题 #33422 正寻求调优建议。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 状态 | 修复 PR？ | 链接 |
|------|----------|--------|---------|------|
| 不稳定的 CI / 基础设施故障 | 高 | 开放 | ❌ | [#42752](https://github.com/sgl-project/sglang/issues/42752) |
| 长预填充（>40k token）静默杀死工作进程 | 严重 | 开放 | ❌ | [#36941](https://github.com/sgl-project/sglang/issues/36941) |
| MambaPool.copy_from 在突发准入下引发 CUDA OOM | 高 | 开放 | ❌ | [#47916](https://github.com/sgl-project/sglang/issues/47916) |
| 分离架构：Qwen EAGLE PD 解码在回退后产生损坏输出 | 高 | 开放 | ❌ | [#48356](https://github.com/sgl-project/sglang/issues/48356) |
| Triton 覆盖破坏其他内核 | 中等 | 开放 | ❌ | [#48460](https://github.com/sgl-project/sglang/issues/48460) |

> ⚠️ 多个关键稳定性问题影响高吞吐与长上下文服务场景。目前尚无可用修复。

---

### **6. 对应用开发者的意义**  
- **在长时间运行或高并发场景中预期存在不稳定性**，特别是输入超过 40k token 或使用混合模型（如 GLM-5.3-Flash）时。请避免在生产环境中使用，直至 #36941 和 #47916 修复。  
- **谨慎使用 `--model-checksum`**：虽然大语言模型具备模型完整性校验，但扩散模型缺乏相应保障——建议考虑添加自定义校验（待审 PR #48225）。  
- **利用新功能优化代理工作流**：可编程 KV 缓存策略（#46542）与预测解码禁用语义（#47797）可实现对智能体逻辑的细粒度控制。  
- **监控 CI 健康状况**：频繁的不稳定测试（#42752）可能导致合并延迟——预计依赖项更新与实验功能上线将出现延迟。  
- **规划平台特定调优**：不同硬件间性能差异显著（如 SM120 vs. Hopper），部署前务必在目标 GPU 上对工作负载进行基准测试。

> 💡 *建议*：仅在测试中使用夜间构建；在核心稳定性问题解决前，避免在生产环境使用主分支。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

### **llama.cpp 消息简报 — 2026-11-05**

#### **1. 今日亮点**  
`v0.9.0` 版本引入了量化与推理效率的关键升级，包括 **OCP FP8 E4M3 支持**、用于优化多序列 KV 缓存的新型 `flash_attn_ext_rows` 运算符，以及通用批处理张量 API。OpenCL 与 WebGPU 后端性能显著提升，同时持续修复 Vulkan 设备丢失及 Intel Arc 和 AMD GPU 上的 SYCL 内存溢出问题。

#### **2. 发布与破坏性变更**  
- **`v0.9.0` (b12110)**：正式发布，因更新的量化处理机制和新集成的 `flash_attn_ext_rows` 核心，模型加载行为发生破坏性变更。  
  - [GitHub 发布页面 v0.9.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.9.0)  
  - 迁移提示：依赖旧版 Flash Attention 路径的用户可能需要重新评估 `--flash-attn` 设置；请检查与 `Q4_K_M`、`FP8_E4M3` 及 GQA 模型的兼容性。

#### **3. 新增模型与硬件支持**  
- **新增架构**：Clover、Limite、Metis-1.6、Mistral Large 4  
  - [PR #31706: Meta MobileMoE 支持](https://github.com/ggml-org/llama.cpp/pull/31706)  
  - [PR #31692: Ripple (recursiveAI) 65B 模型支持](https://github.com/ggml-org/llama.cpp/pull/31692)  
- **量化格式**：OCP FP8 E4M3（新增），通过 `GGML_CUDA_FA_QUANTS` 提议实现 Q4_0-Q8_0 批量支持  
- **后端支持**：  
  - **OpenCL**：通过 FA 支持 GQA 2/6 且头数为 256 的场景，显著提升解码性能  
    - [PR #31633](https://github.com/ggml-org/llama.cpp/pull/31633)  
  - **WebGPU**：新增批处理张量 API（`get_tensor_n`, `set_tensor_n`）及 `MEAN` 支持  
    - [PR #31473](https://github.com/ggml-org/llama.cpp/pull/31473)，[PR #31591](https://github.com/ggml-org/llama.cpp/pull/31591)  
  - **SYCL**：继续优化 MTP + Q3 量化性能  
    - [Issue #27373](https://github.com/ggml-org/llama.cpp/issues/27373)  
  - **Vulkan**：Intel 预填充 FA 内核（PR #29357）现已在 Intel Arc 平台上启用  

#### **4. 性能与优化**  
- **Flash Attention**：`flash_attn_ext_rows` 实现多序列推理中高效的统一 KV 缓存，降低流式场景下的开销。  
- **OpenCL**：在高通 Adreno 设备上观察到最高约 **15% 的解码速度提升**，得益于工作组利用率改进与 GLU 内核优化 ([PR #31633](https://github.com/ggml-org/llama.cpp/pull/31633))。  
- **WebGPU**：批处理张量操作在高吞吐场景下将 CPU-GPU 同步点减少约 30–40% ([PR #31473](https://github.com/ggml-org/llama.cpp/pull/31473))。  
- **CUDA**：PR #31679 通过将主机侧架构检测与实际 PTX 编译目标对齐，确保最优的 MMVQ 批大小。  
- **内存效率**：`gguf` 写入器现在跳过主机缓冲区张量的暂存缓冲区 ([PR #31144](https://github.com/ggml-org/llama.cpp/pull/31144))，在模型导出过程中降低峰值内存使用约 10–15%。

#### **5. 稳定性与回归问题**  
- **严重**：在长时间推理（>7小时）或 Flash Attention 回退后，Intel Arc B580/B70 与 AMD RX 7900 XTX 上报告 **Vulkan 设备丢失** ([#29526](https://github.com/ggml-org/llama.cpp/issues/29526), [#31560](https://github.com/ggml-org/llama.cpp/issues/31560))。  
  - 修复补丁待提交；临时方案：禁用 `KHR_coopmat` 或限制会话时长。  
- **高严重性**：**SYCL 双 GPU 在模型加载时卡死** ([#27547](https://github.com/ggml-org/llama.cpp/issues/27547)) 与 **`--fit` 过程中内存溢出** ([#27595](https://github.com/ggml-org/llama.cpp/issues/27595)) —— 均影响生产部署。  
- **中等**：在 Linux 7.x 内核上，`decode()` 因 `vk::Queue::submit: ErrorDeviceLost` 崩溃 ([#30010](https://github.com/ggml-org/llama.cpp/issues/30010))，很可能与 RADV 驱动不稳定有关。  
- **回归**：`#29622` 合并后，Intel Arc B580 出现 5% 解码速度下降 ([#30018](https://github.com/ggml-org/llama.cpp/issues/30018)) —— 经二分定位为 embd+raw token 批次化变更所致。

#### **6. 对应用开发者的启示**  
- **流式处理与智能体**：在多用户、长上下文应用中，使用 `flash_attn_ext_rows` 与 `--batch-size` 配合 `--max-tokens` 提升吞吐量。  
- **模型服务**：在修复落地前，避免在动态提示截断场景下使用 `--cache-reuse` ([PR #30683](https://github.com/ggml-org/llama.cpp/pull/30683))。  
- **硬件选型**：Intel Arc 用户应启用 `--flash-attn` 并测试 `KHR_coopmat`；若出现非确定性结果，请禁用该选项 ([#31560](https://github.com/ggml-org/llama.cpp/issues/31560))。  
- **CI/CD 流水线**：更新 CI 构建以包含 `ccache` 修复 ([PR #31705](https://github.com/ggml-org/llama.cpp/pull/31705))，加速本地开发。  
- **生产环境使用**：注意长会话下 Vulkan 的设备丢失风险；建议每 6–7 小时重启工作进程。

> 🔗 [官网](https://llama.app) | 📦 [下载 v0.9.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.9.0)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-05**

---

### **1. 今日亮点**  
针对在 Windows 系统上使用 NVIDIA RTX 5090 和 WSL2 的用户，出现关键的稳定性与兼容性问题，主要集中在 GPU 内存访问异常以及 Qwen 模型中的工具调用失败。Qwen2.5 库模板中存在一个高危缺陷，由于 Go 结构体格式问题导致生成的 JSON 格式错误，从而破坏原生工具调用。目前新提交的 PR 通过为模板函数实现正确的 `String()` 渲染方式解决了该问题。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内无新版本发布。*  
但持续进行的变更影响了模型行为：  
- **Qwen2.5 库模板修复 (#19534)**：解决 JSON 渲染问题（`{{ .Function }}` 现已输出有效 JSON）。  
  [PR #19534](https://github.com/ollama/ollama/pull/19534)  
- **Modelfile 回环安全**：通过使用引号包裹的 `FROM` 参数修复含空格的模型名称解析问题。  
  [PR #19528](https://github.com/ollama/ollama/pull/19528)

---

### **3. 新模型与硬件支持**  
- **MLX 运行器优化**：  
  - 通过 PR #19231 已启用 Qwen3/Qwen3.5 对 YaRN 上下文扩展的支持。  
    [PR #19231](https://github.com/ollama/ollama/pull/19231)  
  - MLX CUDA 运行器现在在启动阶段拒绝不支持的设备（防止静默失败）。  
    [PR #19521](https://github.com/ollama/ollama/pull/19521)  
- **WSL2 GPU 内存优化**：已提交补丁，以充分利用 RTX Spark N1X 笔记本电脑上的 GPU 内存（128GB 统一内存）。  
  [PR #19520](https://github.com/ollama/ollama/pull/19520)  
- **新 CLI 集成**：通过 `ollama launch kimi` 增加对 Kimi Code CLI 的支持。  
  [PR #19506](https://github.com/ollama/ollama/pull/19506)

---

### **4. 性能与优化**  
- **内存效率**：Qwen3.8 Flash Next 现在选择性使用 MXFP8 量化，关键路径（如非专家 MTP）仍保留 BF16 以提升长文本生成质量。  
  [PR #18078](https://github.com/ollama/ollama/pull/18078)  
- **VRAM 预估准确性**：针对混合模型（如 Qwen3.5/3.8）中 KV 缓存使用量被严重高估（膨胀 4–8 倍）的问题，关键修复待合并。此问题会影响 `keep_alive=-1` 时的模型保留能力。  
  [Issue #19463](https://github.com/ollama/ollama/issues/19463)  
- **流式性能**：现已实现与 OpenAI 兼容的流式响应及时刷新，解决界面卡死问题。  
  [PR #17027](https://github.com/ollama/ollama/pull/17027)

---

### **5. 稳定性与回归问题**  
**高严重性**  
- **Windows 上工具调用崩溃（RTX 5090）**：工具调用期间出现间歇性断连，伴随 HTTP 500 `/tokenize` 错误，影响 Qwen2.5-Coder 与 Muse Glimmer 30B。  
  [Issue #19439](https://github.com/ollama/ollama/issues/19439)  
- **Qwen2.5-Coder 与 Qwen3 工具调用失败**：尽管函数定义合法，原生工具调用仍失败。  
  [Issue #19524](https://github.com/ollama/ollama/issues/19524)  
- **视觉模型崩溃**：当视觉编码器在 GPU 上运行时，`qwen2.5vl:3b` 会报错“prediction aborted, token repeat limit reached”。  
  [Issue #19483](https://github.com/ollama/ollama/issues/19483)  

**中等严重性**  
- **响应无声截断**：即使设置 `truncation=false`，API 仍会无声截断过大的输入（如 Bonsai 27B）。  
  [Issue #19492](https://github.com/ollama/ollama/issues/19492)  
- **云 API 回归**：`deepseek-v4-pro` 拒绝 `max_tokens > 65536`，尽管模型实际容量约 384k。  
  [Issue #19517](https://github.com/ollama/ollama/issues/19517)  
- **用量 API 变更**：`/api/usage` 不再返回会话/每周占比数据（网页 UI 仍显示）。  
  [Issue #19511](https://github.com/ollama/ollama/issues/19511)

---

### **6. 对应用开发者的启示**  
- **在合并 PR #19534 前，请避免使用 Qwen2.5/Coder 的工具调用**——格式错误的 JSON 会导致函数名不匹配并丢弃调用。  
- **在 RTX 5090/WSL2 系统上验证模型兼容性**：预计工具调用和 GPU 内存分配存在不稳定；建议备选方案包括回退至 CPU 或使用低精度量化。  
- **使用 `OLLAMA_DEBUG_LOG_REQUESTS` + PR #17472 调试流式与生成问题**——当前日志缺少响应负载。  
- **不要依赖 `max_tokens > 65536` 用于 Ollama Cloud 上的 `deepseek-v4-pro`**——请采用客户端分块或拆分请求。  
- **更新 Modelfile**：若 `FROM` 模型名称包含空格或特殊字符，请务必用引号包裹以防止解析错误。  

> ✅ *建议*：密切关注 PR #19534 与 #19520，它们将直接影响代理可靠性与 GPU 利用率。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 消息简报 – 2026-11-05

---

### **1. 今日亮点**

最新发布的候选版本 `v1.107.0-rc.3` 以及即将推出的 `v1.108.0` 在控制平面方面带来了重大升级，包括经过身份验证的管理透传、符合 WORM 标准的 S3 对象锁审计日志，以及与 Splunk 和 Microsoft Sentinel 的集成，全面支持 SOC 2 合规。与此同时，关键稳定性修复解决了流式调用数据丢失、预算追踪漏洞以及防护机制异常等问题——尤其针对 Anthropic 的 `/v1/messages` 格式和 MCP 工具处理逻辑。

---

### **2. 发布与破坏性变更**

- **`v1.108.0`（即将发布）**：  
  - 引入通过 cosign 签名的 Docker 镜像（`e82a3c6`）——验证方式如下：
    ```bash
    cosign verify --key https://raw.githubusercontent.com/BerriAI/litellm/e82a3c646908ba0a6ee0beddc4310d7a6cac4065/cosign.pub ghcr.io/berriai/litellm:v1.108.0
    ```
  - 新增 `--control-plane` 标志，可从同一镜像中提供控制平面 API，实现统一部署。
  - 实现完整的审计日志链路，支持 OCSF 标准化、队列投递、哈希链结构，以及对 Splunk HEC 和 Microsoft Sentinel 的日志接收器 ([PR #49662](https://github.com/BerriAI/litellm/pull/49662), [#49660](https://github.com/BerriAI/litellm/pull/49660))。

- **`v1.107.0-rc.3`**：  
  - 修复客户端早期断开时的成本追踪问题 ([Issue #14457](https://github.com/BerriAI/litellm/issues/14457)) 以及流式中间失败错误 ([Issue #49795](https://github.com/BerriAI/litellm/issues/49795))。
  - 修复部分模型更新时 `drop_params` 被静默丢弃的问题 ([Issue #35842](https://github.com/BerriAI/litellm/issues/35842))。

> 🔔 *迁移提示*：若依赖自定义的 `litellm_params`、`guardrails` 或会话预算逻辑，请谨慎升级。当前审计日志已结构化且具备防篡改能力。

---

### **3. 新模型与硬件支持**

- **通义千问云迁移路径**：通过 [功能请求 #36150](https://github.com/BerriAI/litellm/issues/36150) 正式支持，可无缝将 DashScope 迁移至 QwenCloud，使用兼容 OpenAI 的接口。
- **Cloudflare Workers AI 提供商更新**：提议现代化提供商实现以适配 Cloudflare AI API 扩展能力（如文生图、新模型等）——当前在 [问题 #21115](https://github.com/BerriAI/litellm/issues/21115) 中跟踪。
- **Databricks 仅工具响应处理优化**：现在在禁用推理模式时，能正确处理无 `content` 字段的助手消息 ([Issue #48637](https://github.com/BerriAI/litellm/issues/48637))。

---

### **4. 性能与优化**

- **控制平面可扩展性**：  
  - `Fanout.gather` 原语实现跨多个数据平面的有界散集操作，提升全舰队聚合可见性与容错能力 ([PR #49656](https://github.com/BerriAI/litellm/pull/49656))。
  - 引入基于 Rust 的路由器核心，作为性能优化的替代方案，相比原生 Python 路由器具有更低的跨边界开销 ([PR #49513](https://github.com/BerriAI/litellm/pull/49513))。

- **延迟优化**：  
  - 修复 `sse_keepalive` 流中上游错误传播问题，此前该问题会掩盖初始 ping 后的失败情况 ([Issue #49711](https://github.com/BerriAI/litellm/issues/49711))。
  - 优化 `POST /health/test_model` 端点，现对每个模型执行实时三轮健康检查，显著降低部署验证中的误报率 ([PR #49337](https://github.com/BerriAI/litellm/pull/49337))。

---

### **5. 稳定性与回归问题**

| 严重程度 | 问题 | 描述 | 修复 PR |
|--------|------|-------------|--------|
| ⚠️ 严重 | [Issue #14457](https://github.com/BerriAI/litellm/issues/14457) | 流式响应终止导致使用数据完全丢失（计费缺口）。 | 进行中（`v1.107.0-rc.3`） |
| ⚠️ 高 | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) | `/key/update` 请求中途跳过预算窗口内的成本追踪（`LiteLLM_BudgetWindowSpend`）。 | 已在 `v1.107.0-rc.3` 修复 |
| ⚠️ 高 | [Issue #49795](https://github.com/BerriAI/litellm/issues/49795) | 流式中间失败因 `sent_first_chunk` 状态错误导致回退失效。 | 已在 `v1.107.0-rc.3` 修复 |
| ⚠️ 中等 | [Issue #48773](https://github.com/BerriAI/litellm/issues/48773) | Azure 响应 API 在流式传输中重复传递工具调用参数。 | 已在 `v1.107.0-rc.3` 修复 |
| ⚠️ 中等 | [Issue #50065](https://github.com/BerriAI/litellm/issues/50065) | v1.108.0 升级后，旧版客户端调用 `tools/list` 时发生阻塞。 | 正在调查 |

> ✅ *注意*：大多数高严重性问题已在当前 RC 版本中打补丁；请在生产环境上线前充分测试。

---

### **6. 对应用开发者的影响**

- **构建更安全、可审计的系统**：借助增强的审计日志（OCSF 标准、WORM 存储、SIEM 集成），您现在无需外部工具即可满足 SOC 2 等合规要求。
- **避免计费不准确**：建议升级至 `v1.107.0+` 以确保成本追踪准确——尤其当您的应用使用长时运行或可中断的流式请求时至关重要。
- **提升代理可靠性**：防护机制与 MCP 工具处理现已更好地遵循 Anthropic 的 `thinking`/`reasoning_content` 语义，并防止无限循环 ([#48384](https://github.com/BerriAI/litellm/issues/48384), [#36085](https://github.com/BerriAI/litellm/issues/36085))。
- **利用新的可观测性功能**：通过 `/health/test_model` 和弃用提示，主动发现失效的模型路由并规划迁移 ([PR #50022](https://github.com/BerriAI/litellm/pull/50022), [#50080](https://github.com/BerriAI/litellm/pull/50080))。

> 📌 **实用建议**：若您使用自托管模型（如 vLLM）并搭配防护机制，请立即更新，以避免在无效模型上静默执行预调用钩子 ([PR #49295](https://github.com/BerriAI/litellm/pull/49295))。

---  
*本简报数据源自 GitHub：[BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-05**

---

### **1. 今日亮点**  
v0.1.915-beta 版本引入了 **Qwen3.8-Flash-Next**，增强 Glass 主题（现为测试版），并支持模型通过内置浏览器与文件交互——标志着迈向完整本地代理能力的重要一步。关键稳定性修复解决了长对话中的 UI 卡顿、`search_conversation` 工具消失以及 RAG 数据摄入瓶颈问题，同时新提交的 PR 改进了音频模型处理和嵌入模型下载的容错性。

---

### **2. 发布与破坏性变更**  
- **v0.1.915-beta**: [GitHub Release](https://github.com/unslothai/unsloth/releases/tag/v0.1.915-beta)  
  - 新增对 **Qwen3.8-Flash-Next**（优化推理路径）的支持。  
  - 引入 **Glass 主题测试版**，视觉一致性更佳。  
  - 启用 **浏览器内文件交互**（模型可读写内置文件系统）。  
  - 在切换 GGUF 模型时保留聊天上下文；降低空闲内存占用。  
  - 加快 MLX 加载速度，现支持原生 FP8。

> ✅ *API 和配置无破坏性变更；向后兼容性已保持。*

---

### **3. 新模型与硬件支持**  
- **Qwen3.8-Flash-Next**：作为首类模型变体加入，配备优化的 Flash Attention 路径。  
- **Audio.cpp 音乐模型**：扩展支持 `YuE2`、`Stable Audio 3 Medium` 与 `MiDashengLM-Gen`（通过大小估算修复）。  
- **图像模型**：Studio 现在可加载以下单文件 `.safetensors` 变体：  
  - Anima  
  - Chroma1-HD  
  - MOSS-Image  
  - Krea 2 int4/fp8（支持 ComfyUI 兼容的 LoRA/LoKR）  
- **硬件后端**：  
  - 苹果硅设备上强化 Metal（MLX）性能。  
  - 改进 CUDA 图捕获用于解码步骤（默认启用于 `generate()`）。

---

### **4. 性能与优化**  
- **FP8 推理**：在 v0.1.915-beta 中默认开启，减少内存占用且不损失精度。  
- **延迟降低**：  
  - `model.generate()` 默认启用 **CUDA 图** → 显著降低内核启动开销（每步减少超过 675 个内核调用至接近零）。  
  - **MLX 加载提速** 观察到（在 M 系列芯片上大型模型启动速度提升约 2 倍）。  
- **RAG 数据摄入**：  
  - 使用多块批处理（`_EMBED_BATCH`）时，批嵌入处理速度提升 **约 48 倍**（从约 99 毫秒/块降至约 2.07 毫秒/批）。  
  - 修复 #16715：此前即使输入已准备就绪仍逐文档处理。  
- **内存效率**：自 v0.1.915-beta 后，空闲内存使用量显著下降。

---

### **5. 稳定性与回归问题**  
| 问题 | 严重程度 | 状态 | 修复 PR |  
|------|----------|--------|--------|  
| `search_conversation` 工具反复消失 | 高 | 开放 | [#16718](https://github.com/unslothai/unsloth/pull/16718) |  
| 长对话中 UI 卡顿（网页 + 桌面端） | 高 | 开放 | — |  
| Linux 上 AppImage 的 UI 卡顿（Slackware 等） | 高 | 开放 | [#16605](https://github.com/unslothai/unsloth/issues/16605) |  
| RAG 索引在嵌入模型下载期间失败 | 高 | 开放 | [#16722](https://github.com/unslothai/unsloth/pull/16722) |  
| S3 JSONL 清单在 Unicode 分隔符（U+2028/U+2029）下失败 | 中 | 开放 | [#16725](https://github.com/unslothai/unsloth/pull/16725), [#16723](https://github.com/unslothai/unsloth/pull/16723) |  
| Studio 主题首次加载时闪烁（亮色 → 暗色过渡） | 低 | 已关闭 | [#16614](https://github.com/unslothai/unsloth/issues/16614) |

> ⚠️ **紧急**：过去 24 小时内报告多个核心用户体验回归（搜索、UI 卡顿、RAG）；今日已合并多个修复 PR。

---

### **6. 对应用开发者的意义**  
- **代理构建者**：借助 **浏览器文件访问**、**Qwen3.8-Flash-Next** 与 **持久化上下文**，现在可构建轻量级、本地托管的智能体，实现文件读写，并在会话间维持状态。  
- **微调流水线**：使用 `/skills` 命令（`#16717`）与 `keep cancelled jobs` 逻辑（`#16633`）可提升自动化训练流程的健壮性。  
- **嵌入系统**：修复 `EmbeddingModelDownloadRequiredError` 在索引期间的问题（`#16722`），确保即使模型仍在下载也能无缝运行 RAG 流水线。  
- **音视频工作流**：支持 ComfyUI 风格的单文件模型及 Unicode 安全的 JSONL 解析，可构建可靠的音视频微调流水线。  
- **性能调优**：在 `generate()` 调用中默认启用 `CUDA 图`——预计在 GPU 密集型负载下可实现 **30–50% 的延迟降低**。

> 📌 **行动项**：升级至 v0.1.915-beta，审查任何 RAG 或长上下文工作流的回归风险。持续监控 `search_conversation` 行为，直至 #16718 合并。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*