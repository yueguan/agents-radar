# AI 基础设施日报 2026-11-08

> 生成时间: 2026-11-08 01:46 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI基础设施生态报告 – 2026-11-08**

---

### **1. 生态概览**  
AI推理与服务生态正进入高性能专业化阶段，由下一代硬件（NVIDIA Blackwell、Apple Silicon、Intel Arc）和复杂模型架构（MoE、Mamba2、扩散模型）共同驱动。各项目发展方向日益分化：vLLM 和 SGLang 在大语言模型的吞吐量与推测性解码方面持续突破；llama.cpp 聚焦于异构后端间的底层可移植性整合；Ollama 和 LiteLLM 作为开发者友好的接入入口；Unsloth 则专注于高级微调与多模态工作流。稳定性仍是关键挑战，尤其在新硬件上，多个项目报告了高严重性回归问题。

---

### **2. 活跃度对比**

| 项目       | 今日开放问题数 | 今日合并的PR数 | 发布状态         |
|------------|----------------|----------------|------------------|
| **vLLM**   | 7              | 5              | v0.33.0 存在回归；生产环境建议使用 v0.32.0 |
| **SGLang** | 9              | 6              | 无新发布；即将进入调优活动前的稳定期 |
| **llama.cpp** | 6           | 4              | b12197–b12196 已发布；小幅更新，无主版本升级 |
| **Ollama** | 8              | 3              | 无新发布；正在审查破坏性变更（如 Gemma 4 渲染问题） |
| **LiteLLM** | 5            | 4              | v1.110.0-dev.1（开发版），v1.108.3 LTS 稳定版；推荐用于生产 |
| **Unsloth** | 5             | 4              | 无发布；当前稳定，无破坏性变更 |

> ✅ *趋势*：vLLM 和 SGLang 展现出最高的工程迭代速度，反映出对前沿推理负载的积极优化。LiteLLM 在稳定性和发布规范方面领先，具备 LTS 支持。

---

### **3. 模型支持竞赛**

| 新模型 / 架构                    | 支持项目                          | 备注 |
|----------------------------------|----------------------------------------|------|
| **NVIDIA Nemotron 3.5 ASR 流式 (0.6b)** | ✅ vLLM (`#47455`) | 首个实现实时语音支持的项目 |
| **Qwen3.5-122B-A10B + MTP 推测解码** | ✅ vLLM (`PR #52244`) | 启用混合 GDN 前缀缓存命中 |
| **Mamba2 ReplaySSM 推测解码 (Triton 后端)** | ✅ vLLM (`#66318`)，SGLang (`#49110`) | 现已支持端到端运行 |
| **MageVL-Cowork (多模态图像+文本)** | ✅ llama.cpp (`#31819`) | 完整集成 `mtmd` |
| **Ripple 模型 (NoPE, 层融合)** | ✅ Ollama (`PR #19592`) | 实验性但集成良好 |
| **Gemma 4 工具调用修复 (内容顺序、标签)** | ✅ Ollama (`#19594`, `#19574`) | 关键代理工作流稳定性提升 |
| **DeepSeek-V4.1-Flash (Blackwell)** | ⚠️ SGLang & vLLM（优化中） | SM120 上高吞吐性能仍不稳定 |
| **Apple Silicon MLX 运行器 (CPU + Windows)** | ✅ Ollama (`#19366`) | 支持非 Metal 部署 —— 关键差异化能力 |

> 🏆 **赢家**：**Ollama** 在面向开发者的模型易用性与跨平台支持方面领先。  
> 🚀 **前沿模型/功能采纳领导者**：**vLLM** 与 **SGLang** 在 Blackwell 平台上对 Mamba2 与 MoE 等新架构的集成速度最快。

---

### **4. 性能前沿**

| 优化方向               | 主要项目                     | 关键进展 |
|------------------------|------------------------------|----------|
| **KV 缓存效率**        | vLLM, SGLang                 | Triton 支持的 Mamba2 推测解码；ROCm 上启用 persistent_topk |
| **批处理与并行化**     | SGLang, vLLM                 | 多节点 PCP/PP；AgentX S19 调优（输出吞吐提升 +35.6%） |
| **量化与内存管理**     | vLLM, llama.cpp              | Humming 内核迁移（vLLM）；`get_weight_byte_size()` 提升 MFU；SYCL 内存安全 |
| **内核级融合**         | SGLang, llama.cpp            | RoPE 降低 75%；融合残差（Anima）；SDPA 融合（Alchemist） |
| **分布式服务**         | vLLM, SGLang                 | 多 rank 引擎同步问题（`#62916`, `#41449`）凸显持续挑战 |
| **长上下文吞吐**       | SGLang, vLLM                 | GLM-5.3-Flash + AgentX：B300 上总吞吐提升 +26.3% |

> 🔥 **热点**：内核融合与注意力优化主导了开发活动——尤其集中在 SGLang 与 vLLM。当前竞争焦点是长上下文提示下的毫秒级每标记延迟。

---

### **5. 层级定位**

| 项目       | 核心层级               | 在栈中的角色                              | 差异化优势 |
|------------|------------------------|-------------------------------------------|------------|
| **vLLM**   | 服务引擎               | NVIDIA 上的高吞吐、低延迟推理              | 针对 SM120/MoE 优化；聚焦 Triton/TensorRT 后端 |
| **SGLang** | 服务引擎 + 网关         | 支持多模态的统一推理编排                   | 扩散模型、长上下文、DFLASH MLA 方面强劲 |
| **llama.cpp** | 本地运行时           | CPU/SYCL/Vulkan/WebGPU 执行；可移植推理    | 跨平台支持业界最佳；轻量级 |
| **Ollama** | 网关 / 开发者体验      | 简化本地部署与 CLI 使用体验                | MLX 运行器扩展、并发生成、工具调用修复 |
| **LiteLLM** | LLM 网关 / 抽象层     | 多提供方路由与可观测性的统一 API           | 安全加固、预算追踪、OpenTelemetry 支持 |
| **Unsloth** | 训练/微调             | 快速训练、RAG、扩散模型流水线              | 仅 `.safetensors` 加载、Ulysses CP、VAE VRAM 优化 |

> 📊 **战略洞察**：生态正呈现分化趋势——**引擎导向型**（vLLM/SGLang） vs. **开发者友好抽象层**（Ollama/LiteLLM） vs. **专用运行时**（llama.cpp） vs. **训练流水线**（Unsloth）。

---

### **6. 趋势信号**

1. **硬件特异性不稳定性已成为新常态**：  
   多个关键问题出现在 **NVIDIA Blackwell (SM120)**（vLLM、SGLang）和 **Intel Arc B系列**（llama.cpp）上，表明驱动程序/编译器/后端成熟度落后于芯片发展。**行动建议**：生产环境避免使用最前沿硬件，直到补丁落地。

2. **推测性解码已成主流**：  
   通过 Triton 后端支持 Mamba2（vLLM/SGLang）、混合 GDN 缓存（vLLM）、DFLASH MLA 草稿服务（SGLang），表明推测性解码不再实验性——而是核心性能杠杆。

3. **安全与可预测性成为优先项**：  
   - **Unsloth** 采用仅 `.safetensors` 加载  
   - **LiteLLM** 强制类型安全与安全 Docker 签名  
   - **llama.cpp** 修复确定性 SYCL 操作  
   → **从“快速”转向“可靠且可复现”**。

4. **代理工作流驱动修复**：  
   工具调用崩溃（Ollama）、消息顺序错误（Ollama）、静默工作进程终止（SGLang）、解析错误（LiteLLM）等问题均源于代理使用场景。**代理已成为基础设施的主要压力测试点**。

5. **开源正迈向生产就绪**：  
   随着 LTS 发布（LiteLLM）、安全扫描（cosign）、详细错误日志（LiteLLM、vLLM），开源推理栈正在迅速成熟——不再是仅限原型验证。

> ✅ **开发者行动指南**：  
> - 在 Blackwell 上使用 **v0.32.0** 版本的 vLLM，直至 `#65139` 修复。  
> - 利用 **Ollama 的 `OLLAMA_NUM_PARALLEL`** 与 **SGLang 的 AgentX 调优** 构建可扩展的代理系统。  
> - 生产环境采用 **`.safetensors`** 与 **`lts` 标签** 以确保可靠性。  
> - 密切监控 **长上下文内存使用** 与 **CUDA graph 行为**——尤其在 GB10 与 Arc GPU 上。

---

*报告编制时间：2026-11-08 | 数据来源：vLLM、SGLang、llama.cpp、Ollama、LiteLLM、Unsloth 的 GitHub 活动*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

### **vLLM Digest — 2026-11-08**

#### **1. 今日亮点**  
vLLM 项目持续加速对 NVIDIA Blackwell（SM120）和 ROCm 的支持，针对高端推理工作负载推出了关键的性能与正确性修复。主要进展包括通过 Triton 后端支持实现 Mamba2 的推测解码兼容性、在所有 NVIDIA 架构上大规模转向用更现代的 Humming 内核替代 Marlin、以及针对 SM120 上 NVFP4 MoE 模型的紧急漏洞修复。

#### **2. 发布与破坏性变更**  
*过去 24 小时内无更新。*  
然而，**v0.33.0** 仍因 `b12x` attention 在 MTP 推测解码期间引发 CUDA 非法内存访问（`#65139`）存在回归问题。在 SM120 上使用 NVFP4 MoE + fp8 KV + chunked prefill 的用户应考虑降级至 v0.32.0，直至修复上线。

#### **3. 新模型与硬件支持**  
- ✅ **NVIDIA Blackwell (SM120)**：已在 RTX PRO 6000 Blackwell Max-Q（8x96GB）上开展活跃开发与测试，报告了多个关于 CUDA graph capture、MoE 内核和 attention 内核的问题。  
- ✅ **ROCm (gfx950, gfx1100)**：持续优化，包括支持 persistent_topk、为 Whisper 启用 decoder CUDA graph、以及为不支持的 AITER FP8 MoE 路径添加回退逻辑。  
- 🚀 **新模型支持**：  
  - 新增对 **NVIDIA Nemotron 3.5 ASR Streaming (0.6b)** 的实验性支持（`#47455`）——适用于多语言实时语音应用。  
  - **Qwen3.5-122B-A10B** 现已支持在 MTP 推测解码下使用 hybrid GDN prefix-cache 命中（PR #52244）。

#### **4. 性能与优化**  
- 🔥 **Mamba2 ReplaySSM 推测解码的 Triton 后端支持**：PR `#66318` 实现了 Mamba2 在 FlashInfer 与 Triton 后端上的推测解码支持，可通过输入缓存实现更快的标准解码与推测解码（`#47572`）。  
- ⚙️ **Humming 内核替换计划**：PR `#65560` 正在追踪从旧版 Marlin 迁移到所有 NVIDIA 架构（Hopper/Blackwell）的 Humming 内核，将统一量化路径并提升新显卡上的性能表现。  
- 💡 **批量无关模式优化**：问题 `#65569` 指出，在 Hopper/Blackwell 上启用 `enable_batch_invariant_mode` 会错误地将 Linear 层路由至 Triton，尽管 cuBLAS 本身已是批量无关——修复后有望带来性能提升。  
- 📊 **量化指标增强**：PR `#65143` 在 `QuantizationConfig` 中新增 `get_weight_byte_size()`，提升了自定义量化模型的 MFU/MBU 估算精度。

#### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 修复状态 |
|---------|-------|-------------|------------|
| 严重 | `#65139` | v0.33.0 在 SM120 上首次 MTP 验证步骤出现 CUDA 非法内存访问（回归问题） | ❌ 未解决；阻碍 Blackwell 上稳定部署 |
| 高 | `#56892` | SM120 上使用 DeepSeek-V4.1-Flash 并开启 `--enforce-eager` 时解码吞吐量极低 | ❌ 紧急；CUDA graphs 不可用 |
| 高 | `#51525` | SM120 上使用 `VLLM_CUTLASS` 的 NVFP4 MoE 生成空输出 | ❌ 静默失败；影响模型输出完整性 |
| 中等 | `#65592` | `"max_tokens": null` 会静默绕过平台限制 | ✅ 已在 PR `#66333` 中修复（待合并） |

> 🔗 [Issue #65139](https://github.com/vllm-project/vllm/issues/65139) | 🔗 [Issue #56892](https://github.com/vllm-project/vllm/issues/56892)

#### **6. 对应用开发者的影响**  
- 若你在 **Blackwell GPU（SM120）** 上部署 **DeepSeek-V4.1-Flash**、**Qwen3.5** 或 **Nemotron-3.5-ASR**，请避免使用 `--enforce-eager`，并暂用 v0.32.0，直到 `#65139` 和 `#56892` 修复。  
- 使用 **Mamba2 的推测解码**？请通过 Triton 启用 `ReplaySSM`（`#66318`）——现已支持端到端运行。  
- 对于 **量化 MoE 模型（NVFP4/MXFP8）**，除非使用 `--moe-backend flashinfer`，否则在 SM120 上预期存在不稳定现象。  
- **自定义量化** 用户应实现 `get_weight_byte_size()` 以确保准确的 MFU 报告（`#65143`）。  
- **多节点部署** 使用 PCP/PP 可能因引擎 ID 同步问题失败（`#62916`）——需留意多秩启动失败情况。  

> ✅ **实用建议**：为保障 Blackwell 上的生产稳定性，建议将 vLLM 版本锁定在 `v0.32.0`，直至 `#65139` 修复。关注 `#65560`，未来通过采用 Humming 内核可获得显著性能提升。

---  
*摘要来自 GitHub 活动：[vLLM Project](https://github.com/vllm-project/vllm)*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang 消息简报 – 2026-11-08**

---

### **1. 今日重点**  
SGLang 项目持续强化多模态与长上下文推理能力，针对 Blackwell GPU 上的扩散模型及 DeepSeek-V4 实现了关键性能提升。关键稳定性修复解决了 CUDA 核心转储问题（#26340）、长预填充（>40k token）期间无声工作进程崩溃，以及在混合请求批次下导致秩死锁的推测解码高危漏洞。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
未推出新版本或破坏性 API/配置变更。项目仍聚焦于稳定性和优化，为即将到来的模型专项调优活动做准备。

---

### **3. 新模型与硬件支持**  
- **Blackwell (SM120/SM121) 支持**：针对 4× RTX PRO 6000 (GB10) 和 DGX Spark (GB10) 上的 DeepSeek-V4 与 GLM-5.3-Flash 进行积极优化。  
- **Apple Silicon (MLX)**：RFC #32321 提出对由 Torch 管理的 SRT 路径进行重构，并导出 MLX 区域——迈向原生 Apple Silicon 服务的关键一步。  
- **多模态扩展**：PRs #49110 与 #49107 实现了 TRTLLM_MLA/CUTEDSL_MLA 后端上的 DFLASH MLA 草稿服务，并修复了 DeepSeek V3.1 的流式工具调用处理问题。  
- **量化**：持续优化 W4A8 MoE 量化（PR #38573）与 ROCm 平台上的 MXFP4（PR #47908）。

> 🔗 [RFC: Apple Silicon 服务重构](https://github.com/sgl-project/sglang/issues/32321)  
> 🔗 [DFLASH MLA 草稿支持](https://github.com/sgl-project/sglang/pull/49110)

---

### **4. 性能与优化**  
- **扩散加速**：  
  - PR #48442 通过比特精确复用，将 HiDream-O1-Image 的 RoPE 内核启动次数减少 75%（从 57.6k → 7.2k）。  
  - PR #48394 在 Anima 中融合门控残差，每 50 步 CFG 请求减少 8,400 个内核。  
  - PR #48122 对每个图像缓存 MiniMax-H3 的参考帧/关键帧，避免重复编码。  
- **长上下文吞吐量**：  
  - PR #49071 报告在 B300 TP4 上，使用 AgentX S19 与 C64 配置时，GLM-5.3-Flash 的**总吞吐量提升 26.3%**，**输出吞吐量提升 35.6%**。  
- **内核级优化**：  
  - PR #45891 将打包预填充 top-k 移至 CUDA 用于 DSA，降低长上下文场景下的开销（GB300）。  
  - PR #48895 融合 DCP8 索引器 top-k 解码，提升 Kimi-K3.1 DCP 的效率。  
  - PR #45807 融合 BF16 预填充索引器得分尾部计算用于 DeepSeek-V4.1，消除不必要的 BF16 中间结果。

> 🔗 [B300 AgentX 调优结果](https://github.com/sgl-project/sglang/pull/49071)  
> 🔗 [HiDream-O1-Image 中的 RoPE 内核减少](https://github.com/sgl-project/sglang/pull/48383)

---

### **5. 稳定性与回归问题**  
- **严重（高危）**：  
  - #26340：`pr-test.yml` 引发的 **CUDA 核心转储**——跨运行自动收集；影响 CI 可靠性与调试。尚未修复。  
  - #41449：在批量处理语法约束请求与其他请求混合时，DSpark + TP 出现 **死锁**，各秩在集体操作中自旋。*修复补丁待提交*。  
  - #36941：长预填充（>40k token）期间发生 **无声工作进程终止**，原因系统一内存耗尽（无 OOM 跟踪日志）。影响 GB10 系统。  
- **中等严重性**：  
  - #48593：混合线性注意力模型的完整预填充 CUDA 图捕获失败（`_replay_metadata` 中出现 IndexError）。  
  - #47734：sglang-kernel FA3 在 H200 上对 hdim=128 非因果场景使用次优的 MmaPV_is_RS=false（比 flash-attn 慢 1.5–3%）。  
  - #33422：DeepSeek-V4 在 SM120 上的预填充吞吐量（约 2–7K tok/s）落后于 vLLM/Marlin（约 12.5K tok/s）——需提供调优指导。

> 🔗 [CUDA 核心转储追踪器](https://github.com/sgl-project/sglang/issues/26340)  
> 🔗 [长预填充中的工作进程崩溃](https://github.com/sgl-project/sglang/issues/36941)

---

### **6. 对应用开发者的启示**  
- **使用长上下文提示（>40k token）时需谨慎**：监控 GB10 系统上的统一内存使用情况；若使用混合注意力后端（如 `HybridLinearAttnBackend`），避免启用完整预填充 CUDA 图。  
- **利用近期扩散优化**：启用图像前缀分片（ERNIE-Image）和缓存复用（MiniMax-H3、Ref2VA），以降低延迟与 GPU 负载。  
- **在 DSpark + TP 环境中，直到 #41449 修复前，避免混合请求批处理并带有语法约束**。  
- **期待配置调优后的吞吐量提升**：在 B300（AgentX）上使用调优后的 GLM-5.3-Flash，以及在 Blackwell 上使用调优后的 DeepSeek-V4——请参阅 PR #49071 获取已基准测试的配置。  
- **关注 CI 不稳定问题**（#42752）：不稳定的测试可能影响合并信心；使用 `/sglang-pr-babysit` 工作流时需谨慎。

> ✅ **可操作建议**：仅在确认不会触发 #48593 的前提下，才使用 `--cuda-graph-config '{"prefill": {"backend": "full"}}'`。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# **llama.cpp 消息简报 – 2026-11-08**

---

### **1. 今日亮点**  
最新更新聚焦于 SYCL、Vulkan 与 Hexagon 后端的稳定性与性能提升，修复了 SYCL 中 `CPY` 操作的确定性行为问题、设备缓冲区分配的内存安全问题，并通过协作矩阵处理增强了对 Intel Arc GPU 的支持。值得注意的是，`mtmd` 模块现已完全遵守图像标记限制，并新增对 MageVL-Cowork 模型的支持，显著提升了多模态推理能力。

---

### **2. 发布与破坏性变更**  
- **b12197**：在 Windows 上自动加载 `libhtp`，不再依赖旧版环境变量（`FASTRPC_MANAGE_DSP_LIBRARY_PATH`、`GGML_HEXAGON_LIBHTP_DIR`）——彻底消除 Hexagon 设备的手动配置需求。  
  🔗 [PR #31781](https://github.com/ggml-org/llama.cpp/pull/31781)  
- **b12196**：修复了 SYCL `CPY` 转换中对 Q4_1/Q5_1/Q8_0 的精确舍入问题，解决了与 CPU 参考实现之间的非确定性输出差异。  
  🔗 [PR #31465](https://github.com/ggml-org/llama.cpp/pull/31465)  
- **b12195**：SYCL 现在拒绝超出可用显存的设备缓冲区分配，防止因过度承诺导致的 GPU 崩溃。  
  🔗 [PR #31285](https://github.com/ggml-org/llama.cpp/pull/31285)  

> ⚠️ **迁移提示**：依赖高 `--ngl` 值的 SYCL 用户如遇到内存不足错误，可能需要调整 `GGML_VK_SUBALLOCATION_BLOCK_SIZE` 或限制 `--split-mode`。

---

### **3. 新模型与硬件支持**  
- **模型支持**：  
  - 在 `mtmd`（多模态文本+图像）中新增对 **MageVL-Cowork** 模型的完整支持。  
    🔗 [Issue #31819](https://github.com/ggml-org/llama.cpp/issues/31819)  
  - 提升与 **Decision 2.0 GGUF** 和 **SystemOne API** 的兼容性（通过功能请求）。  
    🔗 [Issue #30161](https://github.com/ggml-org/llama.cpp/issues/30161)  

- **硬件与后端**：  
  - 增强 **Intel Arc B 系列** 的 Vulkan 支持：修复长上下文提示下的非确定性结果和输出损坏问题。  
    🔗 [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560)  
  - **Hexagon** 现在支持非 ASCII 路径和长目录名；新增绝对 skel 路径解析。  
    🔗 [PR #31781](https://github.com/ggml-org/llama.cpp/pull/31781)  
  - **WebGPU**：实现 `group_norm` 内核，使 WebGPU 平台上的张量操作更高效。  
    🔗 [PR #31490](https://github.com/ggml-org/llama.cpp/pull/31490)

---

### **4. 性能与优化**  
- **SYCL**：  
  - 在 `head_dim=64` 下为 **Alchemist (dg2_g10-g12)** 启用融合 SDPA，解锁 Qwen2/Qwen2.5 系列的优化 Flash Attention。  
    🔗 [PR #31574](https://github.com/ggml-org/llama.cpp/pull/31574)  
  - 为 **Flint/gpt-oss** 添加基于 MKL 的 Flash Attention，提示形状为 (tg128) 时实现 **1.87x 加速**。  
    🔗 [PR #31536](https://github.com/ggml-org/llama.cpp/pull/31536)  
  - 向量化 RMS Norm + 融合 ADD/RMS_NORM/MUL 使 Qwen3.8-27B Q3_K_P 推理吞吐率提升 **+3.4%**（tg128）。  
    🔗 [PR #31753](https://github.com/ggml-org/llama.cpp/pull/31753)  

- **CPU**：  
  - CPU 后端中将 `GGML_TYPE_F16` 的默认 `src1` 类型改为 F32，减少矩阵乘法中的精度损失。  
    🔗 [PR #31854](https://github.com/ggml-org/llama.cpp/pull/31854)  

- **内存**：  
  - `moe-cache` 现支持 `--moe-cache-cpu-share auto`，可动态将 MoE 缺失项卸载至 CPU，平衡负载并降低 GPU 压力。  
    🔗 [PR #31286](https://github.com/ggml-org/llama.cpp/pull/31286)

---

### **5. 稳定性与回归问题**  
- **严重级**：  
  - **Vulkan**：在启用 `KHR_coopmat` 时，**Intel Arc Pro B70 (BMG G31)** 出现非确定性结果——相同输入产生不同输出。  
    🔗 [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560) *(暂无修复 PR)*  
  - **Vulkan**：即使禁用 `GGML_VK_DISABLE_COOPMAT`，**Arc B 系列**仍存在输出损坏问题。  
    🔗 [Issue #31828](https://github.com/ggml-org/llama.cpp/issues/31828) *(临时方案不完整)*  

- **高严重级**：  
  - **Metal**：在 96k 标记会话期间，Draft-MTP 会分配高达 **~15 GB 主机内存**（未启用推测解码时约为 ~5 GB）。  
    🔗 [Issue #30174](https://github.com/ggml-org/llama.cpp/issues/30174) *(修复待定)*  
  - **服务器**：加载一个约 19GB 且分跨两个 RPC 后端的模型时，会无限挂起。  
    🔗 [Issue #31847](https://github.com/ggml-org/llama.cpp/issues/31847) *(多后端设置中的回归问题)*  

- **中等严重级**：  
  - **CPU**：在操作数为 `-128` 时，非 VNNI 系统的 Q8_0 点积运算存在符号错误。  
    🔗 [Issue #29351](https://github.com/ggml-org/llama.cpp/issues/29351) *(修复 PR: #31854)*

---

### **6. 对应用开发者的意义**  
- ✅ **使用 `--moe-cache-cpu-share auto`**，在 GPU 内存受限环境下实现 MoE 模型的可扩展部署。  
- 🛠 **避免使用 `GGML_VK_SUBALLOCATION_BLOCK_SIZE=4 GiB`**，除非明确需要——这是针对子分配碎片化的临时方案（参见 [Issue #27734](https://github.com/ggml-org/llama.cpp/issues/27734)）。  
- ⚠️ **在 Intel Arc B 系列上禁用 `KHRCOOPMAT`**，直至 [Issue #31560](https://github.com/ggml-org/llama.cpp/issues/31560) 修复，以避免输出错误。  
- 📦 **使用 WebGPU 的网页应用**：新实现的 `group_norm` 内核使模型执行流程更完整。  
- 🔄 **服务器部署时**：注意 `cache_prompt=false` 任务——若未正确清理，可能导致内存泄漏（参见 [PR #30188](https://github.com/ggml-org/llama.cpp/pull/30188)）。  
- 💡 **仅在确认 `mtmd` 投影器已更新后**，再启用 `--image-min/max-tokens` ——旧版本构建会忽略这些参数（[PR #31864](https://github.com/ggml-org/llama.cpp/pull/31864)）。

> 📌 **最佳实践**：始终在多种硬件目标上测试模型加载与推理的稳定性，尤其是在使用推测解码或多后端组合场景下。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-08**

---

### **1. 今日亮点**  
Ollama 生态系统持续扩展对高级推理工作流的支持，重点包括 MLX 运行器的增强和模型解析器的修复。值得注意的是，一项新 PR 引入了 MLX 模型的涟漪预览支持与并发生成功能，同时正在解决影响 Gemma 4 和 DeepSeek3 的关键渲染与解析问题。此外，有报告指出在 Windows RTX 5090 上存在一个高危缺陷，影响工具调用功能，凸显出当前 GPU 驱动兼容性方面的持续挑战。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本。*  
但多项破坏性变更正处于积极评审中：  
- **PR #19594**：修复 Gemma 4 助手消息中 `content` 与 `tool_calls` 共存时内容顺序错误的问题 —— 此前的回归可能破坏依赖消息结构的代理逻辑。[链接](https://github.com/ollama/ollama/pull/19594)  
- **PR #19574**：修复 Gemma 4 推理过程中意外泄漏的 `<channel|>` 关闭标签，该问题可能导致结构化输出被破坏。[链接](https://github.com/ollama/ollama/pull/19574)

---

### **3. 新模型与硬件支持**  
- **MLX 运行器扩展（PR #19593、#19590、#19567）**：  
  - 通过 PR #19366 增加 **Windows 支持** 和 **CPU 后端**，使非 Metal 系统也能实现全文本转语音推理（如 `phonon:en`）。[链接](https://github.com/ollama/ollama/pull/19366)  
  - 在 MLX 中引入 **密集 Nemotron 与 Luciole 分词器支持**，可直接加载 Safetensors 检查点。[链接](https://github.com/ollama/ollama/pull/19567)  
- **涟漪模型集成（PR #19592）**：新增实验性 **Ripple 模型**、渲染器与解析器，支持 NoPE（非周期嵌入）及层融合优化。[链接](https://github.com/ollama/ollama/pull/19592)  
- **并发生成（PR #19197）**：通过 `OLLAMA_NUM_PARALLEL` 实现多个并行生成请求，显著降低多用户负载下的延迟。默认值仍为 1；内存开销随请求数量线性增长。[链接](https://github.com/ollama/ollama/pull/19197)

---

### **4. 性能与优化**  
- **显存估算改进（PR #18866）**：通过准确解析 GGUF 元数据中的 KV 缓存使用情况，修正了 `ollama ps` 与 `nvidia-smi` 之间四次计数不一致的问题。对大上下文模型至关重要，避免因过度分配导致上下文窗口截断。[链接](https://github.com/ollama/ollama/pull/18866)  
- **基准测试工具可靠性提升（PR #19600）**：确保 `cmd/bench` 在运行不完整时失败，并通过截断复用输出文件防止文件损坏——对自动化性能测试流水线至关重要。[链接](https://github.com/ollama/ollama/pull/19600)  
- **内存效率提升**：MLX 的融合 MLP 激活、值融合与注意力门控机制减少了冗余计算，提升了每 token 的吞吐量。

---

### **5. 稳定性与回归问题**  
- **严重级别**：  
  - **#19439（开放）**：在 Windows RTX 5090 上进行工具调用时出现崩溃，伴随 HTTP 500 /tokenize 错误。使用 Open WebUI 可复现于 Muse Glimmer 30B 与 Q 模型。对依赖工具调用的生产级代理具有极高风险。[链接](https://github.com/ollama/ollama/issues/19439)  
  - **#17748（开放）**：AMD Radeon 780M Vulkan 后端在 0.32.10 之后版本对大型模型失效，报错 `ErrorDeviceLost`。已在 Linux 上确认为回归问题。[链接](https://github.com/ollama/ollama/issues/17748)  

- **高优先级**：  
  - **#19596（开放）**：`gemma4:12b-it-qat` Modelfile 中存在拼写错误（`toos` 而非 `tools`），导致工具调用解析失败。需立即修复以保证官方模型可用性。[链接](https://github.com/ollama/ollama/issues/19596)  
  - **#17638（开放）**：`gpt-oss:20b` 拒绝自身生成的工具调用，提示“error parsing tool call”——表明内部序列化存在缺陷。[链接](https://github.com/ollama/ollama/issues/17638)  

- **中等优先级**：  
  - **#18825（已关闭）**：`embeddinggemma-2:740m` 在 Linux 上因缺少 MLX 运行时而失败——属于文档缺失问题。[链接](https://github.com/ollama/ollama/issues/18825)  
  - **#19597（开放）**：云服务 `/api/chat` 对大于 32 MiB 的负载返回 400 错误（未公开限制），影响图像密集型聊天应用。[链接](https://github.com/ollama/ollama/issues/19597)

---

### **6. 对应用开发者的意义**  
- **构建稳健的工具调用代理**：在修复 `toos` 拼写错误（#19596）前，请避免使用 `gemma4:12b-it-qat`。可选择替代模型或本地修补 Modelfile。  
- **在异构硬件上部署**：得益于 MLX 的 CPU + Windows 支持（PR #19366），可在桌面与笔记本电脑上广泛部署，涵盖 Apple Silicon 与 Intel GPU。  
- **优化并发处理**：利用 `OLLAMA_NUM_PARALLEL`（PR #19197）降低多用户应用的首 token 延迟，但需密切监控显存使用情况。  
- **谨慎处理大输入**：注意 Ollama Cloud `/api/chat` 存在未公开的 32 MiB 限制——建议对 base64 图像进行分块或预处理。  
- **关注新显卡稳定性**：当前稳定版本下，应避免在 RTX 5090 与 AMD 780M 上使用，直到相关回归问题修复。必要时可启用 `--no-mlx` 或回退至其他后端。  

> *建议*：在 #19439 与 #17748 修复前，生产环境请锁定至 v0.40.13 或更早版本。跟踪 PR #19594 与 #19574 以获取消息渲染的即时修复。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

### **1. 今日亮点**  
LiteLLM 生态系统持续增强对 Anthropic 高级推理与工具工作流的支持，关键修复解决了流式传输可靠性、预算跟踪以及模型特定翻译不一致等问题。在安全性和可观测性方面也展现出显著进展，新增了 UI 本地化 CI 检查、强化的 OpenTelemetry span 刷新机制，以及更完善的流失败日志记录。

---

### **2. 发布与破坏性变更**  
- **v1.110.0-dev.1**：通过 [cosign](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 实现验证过的 Docker 镜像签名。此版本包含预生产环境的安全与稳定性改进，但不建议用于生产环境。  
  🔗 [GitHub Release v1.110.0-dev.1](https://github.com/BerriAI/litellm/releases/tag/v1.110.0-dev.1)  
- **v1.108.3 (LTS)**：现为最新稳定 LTS 版本。用户应拉取 `ghcr.io/berriai/litellm:lts` 以确保长期支持和补丁更新。  
  🔗 [GitHub Release v1.108.3](https://github.com/BerriAI/litellm/releases/tag/v1.108.3)

> ✅ **迁移提示**：若使用 `lts` 标签，无需操作——镜像将自动更新至最新补丁。生产环境请避免使用 `dev` 版本。

---

### **3. 新模型与硬件支持**  
过去 24 小时内未新增模型或硬件后端。但以下工作正在进行中：  
- 通过 `agent_reference` 支持 Azure AI Foundry Agents v2（追踪于 #25372）。  
- Vertex AI TTS 现在在构建端点时尊重 `vertex_location`（修复进行中，见 #50219）。

> 🔧 *注意*：Bedrock Converse 现已支持直接使用 S3 对象 URL（通过 PR #50577），可高效导入图片、文档与视频，无需预先下载。

---

### **4. 性能与优化**  
- **流式传输可靠性**：PR #50597 在流式失败时捕获映射异常类（如 `AnthropicError` → `MidStreamFallbackError`），提升错误日志可读性。  
  🔗 [PR #50597](https://github.com/BerriAI/litellm/pull/50597)  
- **超时控制**：PR #50496 引入 `total_timeout` 作为非流式请求的可选上限，可在不破坏现有行为的前提下实现更精细的端到端延迟控制。  
  🔗 [PR #50496](https://github.com/BerriAI/litellm/pull/50496)  
- **类型安全与构建速度**：PR #50579 消除跨 182 个文件的 1,160 个 `basedpyright Any` 错误，提升类型检查健壮性与开发工具性能。  
  🔗 [PR #50579](https://github.com/BerriAI/litellm/pull/50579)

---

### **5. 稳定性与回归问题**  
今日报告的最高严重性回归问题：

| 问题 | 描述 | 修复状态 | GitHub 链接 |
|------|-------------|------------|-------------|
| [#24498](https://github.com/BerriAI/litellm/issues/24498) | Claude 模型空 system message 清理返回 `[System: Empty message content sanitised...]` | 已关闭 | [Issue #24498](https://github.com/BerriAI/litellm/issues/24498) |
| [#49179](https://github.com/BerriAI/litellm/issues/49179) | 在飞行中请求期间执行 `/key/update` 会从 `BudgetWindowSpend` 中丢失成本，导致支出统计错误 | 已关闭 | [Issue #49179](https://github.com/BerriAI/litellm/issues/49179) |
| [#49969](https://github.com/BerriAI/litellm/issues/49969) | 重放的 `reasoningText.signature` 发送给非 Anthropic Bedrock 模型（如 GLM 5.3）引发 Bad Request | 已关闭 | [Issue #49969](https://github.com/BerriAI/litellm/issues/49969) |
| [#50065](https://github.com/BerriAI/litellm/issues/50065) | legacy 客户端在 v1.108.0 后调用 `notifications/initialized` 后，`tools/list` 延迟 18–50 秒 | 开放 | [Issue #50065](https://github.com/BerriAI/litellm/issues/50065) |
| [#49998](https://github.com/BerriAI/litellm/issues/49998) | `/v1/messages` 上游失败返回 503 “auth DB unreachable” 而非 500 | 已关闭 | [Issue #49998](https://github.com/BerriAI/litellm/issues/49998) |

> ⚠️ **严重**：`budget_limits` 竞态条件（#49179）可能导致键在请求过程中被更新时计费不准确——建议升级至 v1.108.3 或更高版本。

---

### **6. 对应用开发者的影响**  
- **生产环境使用 `lts` 标签**（`ghcr.io/berriai/litellm:lts`），以获得经过验证的稳定补丁。  
- **避免使用 `dev` 构建**，除非正在测试新功能；它们可能包含未经验证的变更。  
- **在 Claude 与非 Anthropic 模型（如 GLM、Mistral）间路由时谨慎处理推理块**——签名不匹配可能导致请求被拒绝（参见 #49969）。  
- **监控 `/v1/messages` 响应一致性**，尤其是在使用工具循环或代理框架时——近期修复解决了误路由与状态损坏问题（如 #48384、#50070）。  
- **在高负载环境中启用 `total_timeout`**，防止请求失控并提升 SLA 遵从度。  
- **验证 `extra_headers` 与 `tool_choice` 的输入类型**——非法输入现在返回 400 而非崩溃（PR #50581）。

> 🛠️ **实用技巧**：使用 PR #50593 强制在仪表盘 UI 中遵循本地化最佳实践——避免在 JSX 中硬编码英文字符串。

---  
*摘要生成时间：2026-11-08 | 来源：[BerriAI/litellm](https://github.com/BerriAI/litellm)*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# **Unsloth Digest – 2026-11-08**

---

### **1. 今日亮点**  
最新动态聚焦于扩散模型、RAG 和上下文并行工作流中的关键性能与稳定性优化。主要的 PR 包括对 Studio LTX 视频生成流水线中 VAE 内存使用的优化，以及修复在 CPU 后端进行 MoE 训练时出现的 GPU 内存偏差问题。一项重要的 UI/UX 升级实现了所有模型类型下仅支持 `.safetensors` 权重加载，提升了安全性和一致性。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
未发布新版本或破坏性 API/配置变更。项目保持稳定，无需迁移。

---

### **3. 新模型与硬件支持**  
- **新增**：通过 [PR #17061](https://github.com/unslothai/unsloth/pull/17061) 实现了图像、视频及文本编码器权重的仅 `.safetensors` 加载支持。彻底移除了对旧式 pickle 格式（`.pt`, `.pth`）的依赖，增强安全性。  
- **硬件**：通过 [PR #15883](https://github.com/unslothai/unsloth/pull/15883) 与 [PR #15882](https://github.com/unslothai/unsloth/pull/15882)，将 Ulysses 上下文并行支持扩展至 **Gemma 4**、**DeepSeek V2/V3 MLA** 以及 **V4 sink attention** 模型。  
- **后端**：为支持 SYCL 的 llama.cpp 构建新增 Intel OneAPI 环境变量加载选项（`setvars.sh`）([PR #17048](https://github.com/unslothai/unsloth/pull/17048))。

---

### **4. 性能与优化**  
- **RAG 数据摄入**：识别出因逐文档嵌入处理导致的 99ms/chunk 延迟开销；批量嵌入现已使吞吐量提升约 **48 倍** ([Issue #16715](https://github.com/unslothai/unsloth/issues/16715))。  
- **扩散 VAE**：在 LTX-2/2.5 瓦片解码中减少 VRAM 开销，每张 GPU 卡仅保留一个活跃瓦片带 ([PR #17060](https://github.com/unslothai/unsloth/pull/17060))。在 SDXL 上最多可节省 **2.3 GiB VRAM**。  
- **MoE 训练**：修复在 CPU 后端且梯度累积 > 1 时路由器辅助损失发散的问题 ([Issue #17054](https://github.com/unslothai/unsloth/issues/17054))，恢复训练稳定性。  
- **上下文并行**：评估损失现在按序列正确归一化，不再乘以 `cp_size` ([PR #17049](https://github.com/unslothai/unsloth/pull/17049))。

---

### **5. 稳定性与回归问题**  
- **严重**：在配备 RTX 3060 的 Windows 11 系统上，尽管存在可用 GPU，图像生成仍回退至 CPU/RAM ([Issue #16803](https://github.com/unslothai/unsloth/issues/16803))。*修复待定。*  
- **高危**：搜索工具从 UI 中缺失（重复问题，第三次出现）——影响代理工作流可靠性 ([Issue #16706](https://github.com/unslothai/unsloth/issues/16706))。  
- **中等**：在尝试绑定局域网地址并覆盖 IP 时，Studio 出现崩溃 ([Issue #17017](https://github.com/unslothai/unsloth/issues/17017))。  
- **UI 问题**：桌面应用中报告 Model Hub 渲染异常（内容重叠、图片错位）([Issue #17004](https://github.com/unslothai/unsloth/issues/17004))；搜索图标模糊 ([Issue #16962](https://github.com/unslothai/unsloth/issues/16962))。  
- **已合并修复**：解决 Windows Git Bash 下创建空文件的问题 ([PR #17010](https://github.com/unslothai/unsloth/pull/17010))；改进 FLUX.3 调制头中的条件流复用 ([PR #17059](https://github.com/unslothai/unsloth/pull/17059))。

---

### **6. 对应用开发者的启示**  
开发者应立即**迁移到仅使用 `.safetensors` 加载权重**——这已是默认路径，可显著提升安全性和行为可预测性。对于**多 GPU 推理流水线**，请确保在启用高梯度累积步数时，不要在 `UNSLOTH_FORCE_CPU=1` 环境下运行 MoE 模型，除非已打补丁。若构建**基于 RAG 的代理系统**，建议在语料库层级对文档摄入进行批处理，避免单块延迟开销。  

针对**视觉类应用**，得益于瓦片化 VAE 优化，视频生成工作流中的 VRAM 压力将明显降低。但需注意**大视频上传（>64MB）**——已有功能请求提高限制 ([Issue #17062](https://github.com/unslothai/unsloth/issues/17062))，因此生产环境需规划分块处理。最后，**暂勿依赖搜索工具**，直到该重复性缺陷修复，推荐使用 CLI/API 替代方案。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*