# AI 基础设施日报 2026-11-02

> 生成时间: 2026-11-02 01:47 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目 AI 基础设施生态报告 – 2026-11-02**

---

### **1. 生态概览**  
AI 推理与服务生态正进入硬件专业化的关键阶段，NVIDIA Blackwell（SM120）与 AMD MI350X 成为新的性能前沿。各项目正通过针对性的内核优化、解耦推理支持以及跨平台运行时扩展，快速适配下一代架构。一个清晰的分化正在形成：*高性能引擎*（vLLM、SGLang）、*轻量级本地运行时*（llama.cpp）与*集成应用网关*（Ollama、LiteLLM）各自服务于不同的部署需求。与此同时，基于 Rust 构建的基础设施组件——尤其在 vLLM 与 LiteLLM 中——正成为低延迟、可扩展系统的基础层，势头迅猛。

---

### **2. 活动对比**

| 项目       | 近24小时开启的问题 | 近24小时合并的PR | 发布版本 | 状态 |
|---------------|----------------------------|--------------------------|----------|--------|
| **vLLM**      | 8                          | 12                       | 无     | 稳定，聚焦 SM120/ROCm 调优 |
| **SGLang**    | 7                          | 6                        | 无     | 持续修复漏洞；高负载下稳定性验证 |
| **llama.cpp** | 9                          | 7                        | 6        | 频繁小版本发布；后端主导 |
| **Ollama**    | 5                          | 4                        | 无     | 功能导向；安全与解析更新 |
| **LiteLLM**   | 6                          | 3                        | 无     | 核心路由存在高严重性稳定性问题 |
| **Unsloth**   | 6                          | 6                        | 无     | 用户体验/稳定性修复；后端扩展 |

> ✅ *注：所有项目今日均未报告破坏性变更或新发布，表明早期采用后的稳定期已开启。*

---

### **3. 模型支持竞赛**

| 新模型 / 架构         | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **DeepSeek-V4.1-Flash**          | ✅ (SM120 调优) | ⚠️ (dSV4 预填充延迟) | ❌ | ❌ | ❌ | ❌ |
| **MiniMax-M3.1-Flash**           | ✅ (ROCm 优化) | ✅ (支持 MI350X) | ❌ | ❌ | ❌ | ❌ |
| **GLM-5.3-Flash**                | ✅ (ROCm kpool 优化) | ⚠️ (SM120 捕获崩溃) | ❌ | ❌ | ❌ | ❌ |
| **Qwen3.5 / Qwen3.8 GGUF**       | ❌ | ✅ (预填充融合) | ❌ | ⚠️ (加载失败) | ❌ | ❌ |
| **Apple Silicon (MLX)**          | ❌ | ⚠️ (RFC 进行中) | ❌ | ❌ | ❌ | ✅ (MLX 草稿支持) |
| **Intel XPU (MoE + SSD offload)**| ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (RFC 提出) |
| **AMD MI350X / MI355X**          | ❌ | ✅ (完整路径启用) | ❌ | ❌ | ❌ | ❌ |

> 🏆 **领先者**：**SGLang** 在 *硬件多样性* 上领先，全面支持 MI350X 并实验性集成 Apple Silicon。  
> 🥈 **亚军**：**Unsloth** 在 *多后端可访问性* 上表现突出，尤其在 Apple Silicon 与 Vulkan 上。  
> 🥉 **前沿焦点**：**vLLM** 在 *Blackwell 特定优化* 上占据主导，尤其针对 Flash 模型。

---

### **4. 性能前沿**

| 优化领域               | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **KV 缓存卸载**          | ✅ (P/D, MoRI-IO) | ⚠️ (负载下内存溢出) | ❌ | ❌ | ❌ | ❌ |
| **批处理与吞吐量**        | ✅ (融合 all-reduce 配置) | ⚠️ (预填充间隙落后于 vLLM) | ✅ (dflash) | ❌ | ✅ (Rust 路由器) | ❌ |
| **量化 (MXFP8, FP8)**    | ✅ (cuBLASLt, NVFP4 偏差) | ⚠️ (FA4 回退问题) | ✅ (iq3_s, 分块 GEMM) | ❌ | ❌ | ✅ (FP8 LoRA) |
| **分布式服务 (P/D, Wide-EP)** | ✅ (CI 已启用) | ⚠️ (dLLM 路线图) | ❌ | ❌ | ❌ | ❌ |
| **内核级调优 (SM120)**  | ✅ (高优先级) | ⚠️ (CUDA 核心转储风险) | ⚠️ (Blackwell 上挂起) | ❌ | ❌ | ❌ |

> 🔥 **重点方向**：  
> - **vLLM** 在 **Blackwell 时代 GPU** 的内核级调优上占据主导。  
> - **SGLang** 优先关注 **长时间运行稳定性** 与 **分布式可扩展性**。  
> - **llama.cpp** 在 **低端设备/NPU 可移植性** 方面领先，依托 OpenCL/Vulkan。  
> - **LiteLLM** 通过采用 Rust 核心，推动 **网关性能** 边界。

---

### **5. 层级定位**

| 项目       | 主要层级             | 关键差异化 |
|---------------|----------------------------|--------------------|
| **vLLM**      | **推理引擎**       | SM120 上吞吐最高；深度内核定制；专注 P/D 与 KV 卸载 |
| **SGLang**    | **推理引擎 + 网关** | 强大的分布式服务（dLLM），多节点 HTTP 路由，支持 MoE 的调度器 |
| **llama.cpp** | **本地运行时 / 边缘**   | 跨平台可移植性（Vulkan、SYCL、MLX）；轻量级，适用于嵌入式场景 |
| **Ollama**    | **应用网关 / CLI** | 开发者优先体验；结构化输出；工具调用；提示词缓存 |
| **LiteLLM**   | **API 网关 / 代理**    | 多提供商路由；成本追踪；企业级可观测性 |
| **Unsloth**   | **Studio + 微调 UI** | 统一的 CLI/Studio；支持丰富媒体输入；MoE/CPU-GPU 分层；训练工具 |

> 🧩 **战略集群**：  
> - **引擎层**：vLLM、SGLang  
> - **运行时层**：llama.cpp  
> - **网关/代理层**：Ollama、LiteLLM  
> - **训练/用户体验层**：Unsloth

---

### **6. 趋势信号**

#### 🔍 **关键行业趋势提取**：
1. **硬件专业化已成为必然要求**  
   Blackwell（SM120）与 MI350X 不再是未来展望——它们已是现实战场。vLLM 与 SGLang 在架构特定内核调优上的领先，表明“通用推理”时代已终结。

2. **Rust 正成为基础层**  
   vLLM 的 Rust 前端与 LiteLLM 的 Rust 网关核心，标志着向 **无 GIL、高吞吐量路由** 的大规模演进——这对智能体流水线与实时应用至关重要。

3. **解耦与分布式服务日趋成熟**  
   vLLM 的 P/D + MoRI-IO 工作与 SGLang 的 dLLM 路线图显示，*分布式推理* 正从研究走向生产就绪——尤其适用于长上下文智能体。

4. **安全与资源隔离优先级上升**  
   Ollama 的按模型提示词缓存配额与 LiteLLM 的 `pass_through_endpoints` id 清除机制，反映出对 API 网关中 *多租户风险* 与 *请求完整性* 的日益重视。

5. **边缘与异构后端持续扩展**  
   llama.cpp 对 OpenCL/iq3_s 的支持，以及 Unsloth 对 MLX/Vulkan 的增强，显示出对 **设备端推理** 的强劲需求，尤其是在 Apple Silicon 与移动 NPU 上。

#### 📌 **应用开发者应重点关注**：
- **在 #56892 修复前，避免在 SM120 GPU 上使用 `--enforce-eager`**（vLLM）。
- **若运行 DSV4.1 或 GLM-5.3-Flash，建议在 Blackwell 上使用 Triton 而非 FA4**（SGLang）。
- **在 Ollama 中启用按模型提示词缓存**，以隔离资源使用。
- **监控 LiteLLM 的 `cost` 报告**——提供方数值可能存在严重偏差。
- **尽早规划基于 Rust 的网关**（LiteLLM、vLLM）——规模下延迟与可扩展性将至关重要。
- **测试跨平台模型加载行为**（Unsloth、llama.cpp），因 GPU/NPU 边缘场景仍存在持续崩溃问题。

---

> ✅ **最终结论**：生态已不再关注“哪个引擎最快”——而是聚焦于 **分层韧性、硬件对齐与运维成熟度**。顶级开发者将基于 **vLLM/SGLang 实现性能突破**，借助 **LiteLLM/Ollama 优化开发体验**，并利用 **Unsloth/llama.cpp 实现边缘灵活性**，同时以 Rust 作为关键基础设施组件的核心支撑。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-02

---

### **1. 今日亮点**  
vLLM 项目持续加速对 **Blackwell 架构硬件（SM120）** 的支持，针对 DeepSeek-V4.1-Flash 与 MiniMax-M3.1 模型在 NVIDIA 与 ROCm 平台上的性能优化和关键修复不断推进。重点聚焦于稳定 **解耦推理（P/D）** 与 **KV 缓存卸载**，尤其通过 NixlPushMode 与 MooncakeConnector 的集成实现突破。与此同时，Rust 前端已达到功能对齐里程碑，标志着其作为替代 API 接口的成熟度不断提升。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
无新版本发布，亦无破坏性 API/配置变更。`VLLM_USE_RUST_FRONTEND=1` 实验性标志仍处于激活状态，但尚未达到生产可用水平。

> 🔗 [GitHub Issues: 无新发布](https://github.com/vllm-project/vllm/releases)

---

### **3. 新模型与硬件支持**  
- **DeepSeek-V4.1-Flash** 已针对 SM120（RTX PRO 6000 Blackwell）完成性能调优与现场验证，相关工作见 #56700 与 #56892。  
- **MiniMax-M3.1-Flash** 获得 ROCm 特定的解码优化（#65032），包括并行 top-k 选择及 MXFP8 GEMM 调优（#62509）。  
- **GLM-5.3-Flash** 在 ROCm 上实现内核改进，通过减少主机-设备同步（#63072）以及 kpool 索引优化（#63910）。  
- **ROCm 支持**扩展至分布式推理（P/D + Wide-EP）CI 启用（#48117），同时正在推进 MoRI-IO PD 解耦的稳定性优化（#59116）。

> 🔗 [PR #65032: ROCm 并行解码 top-k](https://github.com/vllm-project/vllm/pull/65032)  
> 🔗 [PR #63910: GLM-5.3-Flash ROCm KPool 优化](https://github.com/vllm-project/vllm/pull/63910)

---

### **4. 性能与优化**  
- **DeepSeek-V4.1**：将 all-reduce 与 mHC token cap 合并为可配置项（`--kernel-config '{"mhc_overlap_max_tokens": N}'`），显著提升高并发场景下的吞吐量（#63837）。  
- **MXFP8 GEMMs** 针对 DeepSeek-V4.1 现已路由至 `cuBLASLt` 处理大输入（>8192 行），相较 FlashInfer CuTe-DSL 实现可观提速（#64815）。  
- **NVFP4 CUTLASS epilogue** 现已内联偏置计算，移除独立内核，降低大前缀填充场景下的延迟（#61821）。  
- **KV 卸载**：优化状态发布逻辑，确保成功存储能反映在查找状态中，避免陈旧覆盖问题（#64444）。  
- **调度器指标**：新增每步拷贝-写（CoW）KV 块计数器，便于深入分析内存抖动情况（#65038）。

> 🔗 [PR #64815: 大规模 MXFP8 GEMMs 使用 cuBLASLt](https://github.com/vllm-project/vllm/pull/64815)  
> 🔗 [PR #61821: NVFP4 偏置内联在 epilogue](https://github.com/vllm-project/vllm/pull/61821)

---

### **5. 稳定性与回归问题**  
- **严重崩溃**：当 `exclude` 条目（如 `mtp.fc.weight`）因权重名称解析错误导致不匹配时，`Quark` 伪解码会静默失败（#65034）。*暂无修复 PR。*  
- **CUDA Graph 捕获失败**：在 H200-NVL TP=8 且使用 FlashInfer 时，融合 allreduce 在捕获阶段崩溃；当前绕行方案性能下降约 34 倍（#48071）。  
- **SM120 内核回归**：在 8x RTX PRO 6000 Blackwell 上启用 `--enforce-eager` 时，解码吞吐量极低，CUDA Graph 完全不可用（#56892）。*需优先修复。*  
- **内存损坏**：`DiffusionGemma-26B-A4B` 在并发负载下因 `post_norm` 层张量形状不匹配（`257 != 256`）而崩溃（#58035）。  
- **MoE 层 Bug**：在流水线并行下，`GLM-5.x` 在阶段开始时重用了陈旧的 top-k 索引，导致输出错误（#63170）。

> 🔗 [Issue #65034: Quark Exclude 不匹配](https://github.com/vllm-project/vllm/issues/65034)  
> 🔗 [Issue #56892: SM120 解码吞吐崩溃](https://github.com/vllm-project/vllm/issues/56892)

---

### **6. 对应用开发者的影响**  
- **在 SM120 GPU 上慎用 `--enforce-eager`**——预期性能下降；建议禁用 CUDA Graph，直至 #56892 修复完成。  
- **对于代理系统与长周期 RLHF 流程**，请监控 `free_block_queue` 的分配顺序，并谨慎使用 `reset_prefix_cache()`——可参考 #40244 提出的 API 扩展方案。  
- **集成外部路由器**时，请利用实时队列信号（如 `NUM_REQUESTS_RUNNING/WAITING`）——请求追踪功能（#49538）目前正积极征集需求。  
- **多模态应用**：避免依赖重叠音频分块——当前实现采用非重叠分块（#49414）；请相应更新预处理逻辑。  
- **Rust 前端用户**：功能对齐进展迅速（#44280）；预计很快可在低延迟、嵌入式部署中稳定采用。

> 🔗 [RFC #44280: Rust 前端对齐路线图](https://github.com/vllm-project/vllm/issues/44280)  
> 🔗 [功能请求 #49538: 实时队列信号](https://github.com/vllm-project/vllm/issues/49538)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

**SGLang 简报 – 2026-11-02**

---

### **1. 今日重点**  
SGLang 生态系统持续成熟，重点聚焦下一代硬件（尤其是 NVIDIA Blackwell (SM120) 和 AMD MI350X）的稳定性与性能。针对 CUDA 核心转储问题 (#26340)、DeepSeek-V4.1 规划解码卡死问题 (#35822)，以及高负载下 GPU 内存耗尽问题 (#43486) 的关键缺陷正在积极修复。与此同时，dLLM Serving 路线图 (#39499) 和 HiCache IPC 设计 (#37372) 取得显著进展，表明底层基础设施投入持续加深。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未报告任何内容。*  
未观察到新版本发布或破坏性 API/配置变更。最新稳定镜像（`lmsysorg/sglang:v0.5.20`）仍在使用，尽管 `dev-dsv41` 构建版本正在积极测试。

---

### **3. 新模型与硬件支持**  
- ✅ **AMD MI350X / MI355X (gfx950)**：通过 PR #46693 添加对 MiniMax-M3.1-Flash 的支持，实现从注意力机制、MoE/GEMM 到快速 all-reduce 内核的完整推理路径。  
- ✅ **Apple Silicon（MLX/Torch 互操作性）**：RFC #32321 提出重构方案，利用由 Torch 主导的 SRT 路径及导出的 MLX 区域，为原生 Apple Silicon 服务铺平道路。  
- ✅ **Intel XPU**：RFC #46520 提议采用带前瞻专家调度（LES）的分页专家机制，并支持将单 GPU MoE 工作负载卸载至 SSD 层——旨在提升未来可扩展性。  
- ✅ **Ascend NPU**：文档优化工作正在进行中（PR #36090），但部署清晰度仍存在差距（问题 #47382）。

---

### **4. 性能与优化**  
- 🚀 **Qwen3.5 预填充融合**：PR #47785 将预填充阶段的 QK 归一化与 RoPE 融合为连续的 Q 张量，减少 NVIDIA GPU 上的内核开销。  
- ⚙️ **RMSNorm JIT 调优**：PR #47503 引入一个针对 SM100 优化的单一模板，适用于普通、融合残差加法及 FP8 每张量量化变体，提升跨后端的一致性。  
- 🔍 **Hunyuan3D 导出优化**：PR #46956 在生成 GLB 时跳过冗余 OBJ 导出，降低形状+绘画工作流中的中间 I/O 量。  
- 📉 **DeepSeek-V4 dsv4 预填充吞吐量**：问题 #33422 报告在 4× RTX PRO 6000（SM120）上，其吞吐量约为 2–7K tok/s，远低于 vLLM/Marlin 的 ~12.5K。后端调优与内核覆盖情况正在调查中。

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 摘要 | 修复状态 |
|--------|------|--------|-----------|
| 🔥 高 | [#26340](https://github.com/sgl-project/sglang/issues/26340) | CI 自动收集到的 CUDA 核心转储；测试期间反复崩溃。 | *正在进行调查；暂无修复方案* |
| 🔥 高 | [#43486](https://github.com/sgl-project/sglang/issues/43486) | DSV4.1 Flash + DSPARK：在持续解码下，CUDA 缓存分配器耗尽 GPU 内存（每 15–25 分钟发生一次 OOM），尽管仅使用了 29% KV 池。 | *严重漏洞；尚未提出修复方案* |
| 🔥 高 | [#35822](https://github.com/sgl-project/sglang/issues/35822) | EAGLE 规划解码在 Ampere（A2）上使用 `tree_speculative_sampling_target_only` 时卡死。 | *已复现；修复待定* |
| 🟡 中 | [#42012](https://github.com/sgl-project/sglang/issues/42012) | GLM-5.3-Flash 在 SM120 上使用 FA4 后端进行 CUDA 图捕获失败；仅 Triton 可用。 | *已有临时方案；需修复 FA4* |
| 🟡 中 | [#47218](https://github.com/sgl-project/sglang/issues/47218) | DeepSeek-V4 调度器在并发被中断流中多模态偏移变为 `None` 时崩溃。 | *确认为 Bug；修复中* |

---

### **6. 对应用开发者的启示**  
- **在 Blackwell（SM120）上使用 FA4 后端时预期不稳定**：对于 GLM-5.3-Flash 与 DSV4.1，建议以 Triton 作为回退方案，直至相关 PR 落地。请关注 #26340 与 #42012 获取更新。  
- **高并发长上下文工作负载需谨慎优化**：CUDA 缓存分配器存在内存泄漏（问题 #43486），即使 KV 池使用率较低也可能引发 OOM —— 除非已打补丁，否则避免最大批次解码。  
- **充分利用即将推出的 dLLM Serving 功能**：路线图 (#39499) 将提供更好的块扩散支持与全序列模型的基础兼容性 —— 非常适合智能体流水线场景。  
- **尽早规划多节点部署**：RFC #47657 引入每节点独立的 HTTP 前端与全局 `X-Data-Parallel-Rank`，使外部路由器如 `llm-d` 能高效跨节点路由。  
- **关注 Apple Silicon 准备就绪状态**：若目标为 M 系列 Mac，可跟进 #32321 以获取未来原生 MLX 集成信息。

> 💡 *实用提示*：目前建议使用 `v0.5.20` 或 `dev-dsv41` 镜像以保证稳定性；在 Blackwell 或复杂 MoE 部署场景运行生产负载时，请避免使用 `main`。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-11-02**

---

### **1. 今日重点**  
最新更新聚焦于 Vulkan、SYCL 与 OpenCL 后端的关键稳定性与性能优化，尤其针对 Intel Arc B70/B60 与 Qualcomm Adreno 等高端 GPU。主要进展包括：OpenCL 中初步支持 `iq3_s` 量化，修复 Hexagon 上的 DMA 传输问题，以及引入新的 `dflash` 优化，通过设备内存拷贝显著提升预填充（prefill）速度。值得注意的是，多 GPU SYCL 系统在 MoE 推理中出现严重性能退化（降速 100–1000 倍），目前正进行排查。

---

### **2. 发布与破坏性变更**  
- **b12032**：在 SYCL 后端中为 LevelZero 的头文件与库添加 PkgConfig 支持 ([#31076](https://github.com/ggml-org/llama.cpp/pull/31076))  
- **b12031**：在 OpenCL 中实现 `iq3_s` 量化支持（AOS/SOA 与分块 GEMM 变体），包含 Adreno 特定优化 ([#31338](https://github.com/ggml-org/llama.cpp/pull/31338))  
- **b12030**：修复 Hexagon v75+ 设备上 2D DMA 步长处理问题，此前步长 >24 位会导致数据损坏 ([#31442](https://github.com/ggml-org/llama.cpp/pull/31442))  
- **b12029**：修复 MTMD 后端中 InternVL 模型的 CLS token 顺序问题 ([#30886](https://github.com/ggml-org/llama.cpp/pull/30886))  
- **b12028**：在 `test-llama-archs` 中启用 Gemma 4 的 GQA 测试 ([#31401](https://github.com/ggml-org/llama.cpp/pull/31401))  
- **b12027**：为 LFM3 模型启用张量拆分以提升内存效率 ([#31431](https://github.com/ggml-org/llama.cpp/pull/31431))

> ✅ *未检测到破坏性 API 变更；均为功能增强或缺陷修复版本。*

---

### **3. 新模型与硬件支持**  
- **OpenCL**：新增实验性 `iq3_s` 量化支持（AOS/SOA、分块矩阵乘法、Adreno 优化）。  
- **Hexagon**：v75+ DSP 完全支持大步长 DMA 传输（对 NPU 卸载至关重要）。  
- **MTMD**：新增对 NVIDIA Nemotron 3.5 Transcribe SALM 模型的支持 ([#31471](https://github.com/ggml-org/llama.cpp/pull/31471))。  
- **CUDA**：正在进行 `TQ1_0` 与 `TQ2_0` 类型支持（Maple Preview），相关内核实现已合并 ([#31512](https://github.com/ggml-org/llama.cpp/pull/31512))。  
- **Gemma 4**：已完全集成至测试套件，并启用 GQA ([#31401](https://github.com/ggml-org/llama.cpp/pull/31401))。

---

### **4. 性能与优化**  
- **dflash 预填充加速**：新 `dflash` 实现采用设备内存拷贝，显著提升预填充吞吐量——早期基准显示预填充提速约 30%，解码性能提升约 5% ([#31506](https://github.com/ggml-org/llama.cpp/pull/31506))。  
- **MoE 缓存优化**：混合 LRFU/MQ 风格缓存（`ClassLRU`）在 QFN 上带来 +6% 解码增益，在 GLM 上达 +7%，且专家上传次数减少 8% ([#31233](https://github.com/ggml-org/llama.cpp/pull/31233))。  
- **Flash Attention**：`flash_attn_ext_rows` 扩展通过优化行过滤机制，降低多序列推理中的统一 KV 缓存开销 ([#29510](https://github.com/ggml-org/llama.cpp/pull/29510))。  
- **CUDA**：新增 `SWIGLU_CLAMP` 内核，支持 float16/fp32 张量的步进/连续布局 ([#31517](https://github.com/ggml-org/llama.cpp/pull/31517))。

---

### **5. 稳定性与回归问题**  
- **严重回归（SYCL）**：在 Intel Arc B60/B70 上使用多 GPU `--cpu-moe` + `--split-mode layer` 时，预填充速度下降 100–1000 倍（例如 5 个 token 提示耗时 165 秒），原因系 CPU 主导的专家调度 ([#31510](https://github.com/ggml-org/llama.cpp/issues/31510))。暂无修复 PR。  
- **Vulkan 性能下降**：近期构建版本在 RX 6600 上对 Qwen3.5-9B-Q5_K_M GGUF 模型的评估性能出现明显退化 ([#24066](https://github.com/ggml-org/llama.cpp/issues/24066)) —— 49 条评论，仍开放。  
- **AMD RADV 崩溃**：`draft-mtp` 模式在提示处理阶段因 `llama_decode()` 调用过频导致 `DeviceLost` 错误 ([#27306](https://github.com/ggml-org/llama.cpp/issues/27306))。  
- **CUDA Blackwell 假死**：RTX 5090D 在解码 `Qwen3.8-27B-NVFP4` 模型时卡死 ([#27329](https://github.com/ggml-org/llama.cpp/issues/27329))。  
- **Metal OOM on M5 Max**：Gemma 4 31B 在 `n_ctx` 较大时崩溃，提示“内存不足”，尽管系统拥有 128GB 统一内存 ([#29521](https://github.com/ggml-org/llama.cpp/issues/29521))。

---

### **6. 对应用开发者的启示**  
- **谨慎使用 dflash**：虽可提升预填充速度，但请确保后端支持 `device_copy` 路径——若需高预填充吞吐量，避免在 HIP 后端使用 `draft-mtp` ([#31495](https://github.com/ggml-org/llama.cpp/issues/31495))。  
- **在 [#31510](https://github.com/ggml-org/llama.cpp/issues/31510) 修复前，避免在多 GPU SYCL 环境中使用 `--cpu-moe`** —— 预期将出现严重性能下降。  
- **升级至 b12031+** 以获得移动端/NPU 平台（Adreno、Hexagon）更好的 OpenCL 支持；考虑使用 `iq3_s` 以降低显存占用。  
- **密切关注 Metal/MacOS 构建** —— 大上下文模型仍存在已知内存溢出问题；可通过减小 `n_ctx` 或使用 `--no-mmap` 作为临时方案。  
- **仅在 MoE 模型上启用 `--moe-cache-mib`** —— 若请求了 MoE 缓存而模型为密集权重（非 MoE），将导致失败 ([#31497](https://github.com/ggml-org/llama.cpp/pull/31497))。

> 🔗 **推荐操作**：  
> - 按后端（Vulkan/HIP/CUDA）测试 `dflash` 与 `draft-mtp` 行为。  
> - 使用 `gguf-dump` 时需谨慎——最近审计发现存在逃逸序列注入风险 ([#28969](https://github.com/ggml-org/llama.cpp/issues/28969))。  
> - 密切关注问题 #31510，尤其是涉及 SYCL 多 GPU 工作负载的场景。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-02**

---

### **1. 今日亮点**  
Ollama 生态系统持续成熟，模型解析、提示缓存和工具处理方面取得关键改进——尤其针对采用 GPT-OSS 和谐格式的新兴模型如 Qwen3.8 与 LLM-jp-4.1。Gemma3:4b 结构化输出失败问题及 Qwen 模型在思考阶段的幻觉现象正在修复中，同时新提交的 PR 支持按模型设置提示缓存预算，并提供更精细的令牌级指标日志记录。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本。*  
但多项重要变更正在进行中：
- **PR #19450**：通过 API 请求和 Modelfile 配置引入按模型的 `prompt_cache` 预算（例如 `PROMPT_CACHE=512MiB`）。此举解决了所有模型共享全局缓存预算的长期限制问题（[#19435](https://github.com/ollama/ollama/issues/19435)）。
- **PR #19447**：修复 `/api/embed` 接口对 `tokenizer.ggml.add_bos_token` 与 `add_eos_token` 的忽略问题，防止禁用这些标记的模型因上下文长度错误而报错（[#19446](https://github.com/ollama/ollama/issues/19446)）。
- **PR #19443**：从默认 CORS 来源中移除 `file://*`，防止本地 HTML 文件访问未认证的 API —— 通过要求显式启用 `OLLAMA_ORIGINS` 提升安全性。

---

### **3. 新模型与硬件支持**  
- **Qwen3.8-27B Hybrid-Attention GGUF 版本**：加载失败，原因在于第 64 层缺失 `attn_qkv/attn_gate` 投影（[#19459](https://github.com/ollama/ollama/issues/19459)）。表明存在架构不匹配或 GGUF 导出不完整的问题，可能影响使用 Huihui-AI 抽象变体的用户。
- **LLM-jp-4.1（8B/32B/33B GGUF）**：已识别为支持和谐格式，但需更新解析器以正确处理特殊标记后跟空格的行为（[#18728](https://github.com/ollama/ollama/issues/18728)）。
- **Qwen3.5 0.8B Q4_K_M 量化版本**：请求上线（[#19436](https://github.com/ollama/ollama/issues/19436)）——对 CPU 推理性能至关重要。目前仅适用于更大尺寸的 Qwen3.5 变体。

---

### **4. 性能与优化**  
- **按模型提示缓存**：PR #19450 实现对各模型内存使用的细粒度控制，使高吞吐量应用可隔离资源密集型模型，避免影响其他模型。
- **令牌重复限制**：PR #19422 在 MLX 后端新增对连续重复令牌（>101 个）的检测机制，防止无限生成循环，提升边缘情况下的可靠性（[#16179](https://github.com/ollama/ollama/issues/16179)）。
- **调度器驱逐优化**：PR #19449 改进驱逐时运行器的选择策略，综合考虑内存占用与空闲时长，提升压力下的负载均衡能力（[#19326](https://github.com/ollama/ollama/issues/19326)）。
- **嵌入上下文准确性**：PR #19447 确保对 BOS/EOS 标记的上下文窗口计算准确，避免嵌入流程中的截断问题。

---

### **5. 稳定性与回归问题**  
| 严重性 | 问题 | 状态 | 修复 PR |
|--------|------|--------|-------|
| 🔴 高 | `gemma3:4b` 在启用结构化输出 + 长提示时返回空 JSON | 已关闭 | [#19427](https://github.com/ollama/ollama/issues/19427) |
| 🔴 高 | Qwen 模型在发出 `</thinking>` 时终止整个对话 | 已关闭 | [#19424](https://github.com/ollama/ollama/issues/19424) |
| 🟡 中 | 长回复导致 `deepseek-r1`、`phi3.5` 等模型性能下降直至重启 | 开放 | [#19457](https://github.com/ollama/ollama/issues/19457) |
| 🟡 中 | `qwen3next:layer64` 缺失 `attn_qkv/attn_gate` 投影 | 开放 | — |
| 🟡 中 | 通用工具解析器将参数字符串误识别为函数名 | 开放 | [#19425](https://github.com/ollama/ollama/issues/19425) |
| 🟡 低 | Windows ARM64 平台 Gemma3 视觉请求在 CPU 图像处理期间超时 | 开放 | [#19444](https://github.com/ollama/ollama/issues/19444) |

> ⚠️ 注意：多个与 **结构化输出**、**工具调用** 和 **上下文管理** 相关的回归问题，表明代理工作流正经历持续的压力测试。

---

### **6. 对应用开发者的意义**  
- **设计为按模型资源隔离**：随着按模型提示缓存预算的实现，开发者应在 Modelfile 或 API 调用中定义 `PROMPT_CACHE`，以避免多租户或基于代理的系统中模型间的资源干扰。
- **谨慎处理工具调用解析**：避免依赖模型生成的函数名与注册工具匹配；应显式使用 `tools:[]`，并预期模型发出未识别标签时可能出现不匹配（[#19425](https://github.com/ollama/ollama/issues/19425)）。
- **安全启用结构化输出**：对于 `gemma3:4b`，除非可验证模式兼容性，否则避免使用长提取提示；建议为返回空 JSON 的情况准备回退逻辑（[#19427](https://github.com/ollama/ollama/issues/19427)）。
- **监控内存与上下文行为**：长时间会话可能导致 `phi3.5` 与 `deepseek-r1` 因缓存键重缩放而性能下降（[#19457](https://github.com/ollama/ollama/issues/19457)）；建议实施定期模型重载或上下文修剪。
- **使用令牌指标进行成本控制**：拟议的可选令牌日志功能（[#19455](https://github.com/ollama/ollama/issues/19455)）即将上线，使运维人员可追踪吞吐量、缓存效率与生成令牌分布——对计费与 SLA 追踪至关重要。

---  
*数据来源：[github.com/ollama/ollama](https://github.com/ollama/ollama)*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

### **LiteLLM 摘要 — 2026-11-02**

#### **1. 今日重点**  
LiteLLM 项目在双核架构方面取得显著进展，新推出的生产就绪 Rust 路由核心（#49513）实现了高性能、低延迟的推理路由，同时保持与现有 Python 工作流的完全兼容。与此同时，多个新提交正在积极稳定 `/v1/messages` 处理中 Rust 与 Python 核心之间的功能一致性（#49610, #49611），并通过改进 Redis 凭证管理与认证强制策略，增强代理的安全性与可观测性。

#### **2. 发布与破坏性变更**  
*过去 24 小时内未发布新版本。*  
但多项关键配置与行为变更正在进行中：  
- `pass_through_endpoints` 现在会静默地从 JSON-RPC 2.0 请求体中移除 `id`，因内部参数冲突所致（问题 #49479）。此行为违反协议规范，需客户端绕过或上游修复。  
- 所有流式请求在 `/cursor/chat/completions` 端点均失败，原因是 `cursor_data_generator()` 缺少 `request` 参数（问题 #35632），该端点目前不可用，直至修复。

#### **3. 新模型与硬件支持**  
*今日未新增模型或硬件后端。*  
值得关注的持续工作包括：  
- 正在扩展对 FAL.ai 完整模型目录的支持，超越当前硬编码列表（问题 #34619）。  
- 改进 Ollama（`ollama_chat`）提供者在工具调用流式处理中的行为，其中 `finish_reason` 错误报告为 `stop` 而非 `tool_calls`（问题 #35663）。

#### **4. 性能与优化**  
通过 **Rust 网关核心** 实现了显著性能提升：  
- 新的 Rust 路由核心（#49513）消除了 Python GIL 争用，降低路由决策延迟。  
- 路由冷却、TPM/RPM 限制及全局使用统计现已移交至基于 Rust 的 Redis 存储（#49525, #49481），提升分布式部署下的可扩展性与一致性。  
- 优化后的 Redis 客户端行为已匹配 `redis-py` 5.3.1 的命令执行、TTL 处理与错误分类（#49189），减少故障转移期间的静默失败。

#### **5. 稳定性与回归问题**  
今日报告的关键稳定性问题包括：  
- **[高严重性]** `pass_through_endpoints` 会移除 JSON-RPC 2.0 请求中的 `id`，破坏协议合规性（问题 #49479）—— *尚未提交修复 PR*。  
- **[高严重性]** 若未经验证即信任提供者报告的 `usage.cost`，成本追踪可能导致账单超支高达 1,000,000 倍（问题 #48398）—— *修复 PR 待审*。  
- **[中等严重性]** 更新预算标签时，因重复的 `budget_id` 参数导致 `/tag/update` 返回 500 错误（问题 #46999）—— *修复 PR 待审*。  
- **[中等严重性]** 精确匹配缓存跨虚拟键/团队共享，导致响应缓存错误（问题 #49313）—— *修复 PR 待审*。  
- **[低严重性]** `model_info` JSON 缺失 `created_at`、`created_by`、`updated_at`，尽管数据库值正确，但前端界面仍显示“未知”（问题 #35623）—— *修复 PR 待审*。

#### **6. 对应用开发者的影响**  
开发者应 **避免在使用 JSON-RPC 2.0 数据时使用 `pass_through_endpoints`**，直到问题 #49479 解决，否则将破坏请求完整性。谨慎对待成本监控：**不要依赖提供者报告的 `cost` 值而不做验证**，尤其在多租户环境中（问题 #48398）。若使用带图像输入的代理工具，请注意默认 `detail:auto` 定价无论图像大小均按 85 个令牌计费（问题 #48422）。对于大规模部署团队，建议升级至新的 Rust 驱动代理（通过 `--enable-rust-gateway`），以获得更高吞吐量与更低延迟——尽管该功能仍为可选且处于稳定化阶段。

> 🔗 [问题 #49479](https://github.com/BerriAI/litellm/issues/49479) | [问题 #48398](https://github.com/BerriAI/litellm/issues/48398) | [PR #49513](https://github.com/BerriAI/litellm/pull/49513)

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-02**

---

### **1. 今日亮点**  
Unsloth 生态系统持续扩展多模型与多后端支持，针对 Windows 与 Linux 平台的模型加载稳定性进行了关键修复，尤其在 GGUF、MoE 及 Vulkan 后端方面。主要改进包括：更优的 MoE 模型中 GPU/CPU 层分布处理、增强的 Gemini API 集成，以及新增 CLI/Studio 对富文本和语音输入的支持。此前在独立 Studio 安装中出现的重大回归问题已解决，恢复了环境隔离的保障。

---

### **2. 发布与破坏性变更**  
过去 24 小时内未报告任何发布或破坏性变更。未发布新版本，也无新的 API/配置破坏性更改。

---

### **3. 新模型与硬件支持**  
- ✅ **Gemini API**：通过 PR [#16433](https://github.com/unslothai/unsloth/pull/16433) 实现完整分页支持，现已可发现所有模型，包括 `gemini-4-argon`、`gemini-3.8-flash` 等此前受 20 模型限制隐藏的型号。
- ✅ **AMD GPU 上的 Vulkan**：PR [#16421](https://github.com/unslothai/unsloth/pull/16421) 在 `llama.cpp` 的 `fit` 步骤崩溃时（如 RX 5700 XT）添加重试逻辑，提升了在 Windows 上加载模型时的容错能力。
- ✅ **MLX 后端**：PR [#16441](https://github.com/unslothai/unsloth/pull/16441) 为基于 MLX 工具引入实验性草稿支持，涵盖视频、语法约束、KV 量化及 LoRA 回复——对 Apple Silicon 用户至关重要。
- ✅ **FP8 LoRA 训练**：PR [#16423](https://github.com/unslothai/unsloth/pull/16423) 在冻结的 16 位基础模型上启用 `float8_training=True`，实现无需全模型量化即可进行混合精度微调。

---

### **4. 性能与优化**  
- ⚙️ **MoE 层用户体验优化**：PR [#16429](https://github.com/unslothai/unsloth/pull/16429) 当 GPU 层设置为 0 时隐藏“MoE 层在 CPU”滑块，减少混淆并降低不必要的配置开销。
- 📈 **内存效率**：PR [#16438](https://github.com/unslothai/unsloth/pull/16438) 修正 macOS 上的内存报告，将 `ru_maxrss` 从字节单位换算为 MiB，解决了 LTX 工作器中误导性的峰值内存读数问题。
- 🔧 **并行化框架**：PR [#16407](https://github.com/unslothai/unsloth/pull/16407) 引入统一环境标志 `UNSLOTH_PARALLEL` 用于 DP/TP/PP/CP，通过单条命令即可灵活配置流水线并行训练。

---

### **5. 稳定性与回归问题**  
- 🔥 **严重回归（事故级）**：两个重复问题（#16186、#16185）指出，`UNSLOTH_STUDIO_HOME=<path> ./install.sh --local` 会静默覆盖现有的基线虚拟环境，导致隔离失效——已在 PR [#16428](https://github.com/unslothai/unsloth/pull/16428) 中通过改进加载器基线追踪机制解决。
- 🛑 **模型加载失败**：  
  - 问题 [#16317](https://github.com/unslothai/unsloth/issues/16317)：使用 Bearer Token + OpenAIAPI + OpenCode 时流式中断——可能由认证头解析错误引起。  
  - 问题 [#16263](https://github.com/unslothai/unsloth/issues/16263)：GGUF 模型因 ModelScope 上固定 HF 版本的 404 错误无法加载；已通过 PR [#16433](https://github.com/unslothai/unsloth/pull/16433) 修复。
- ❌ **GPU 崩溃**：  
  - 问题 [#16294](https://github.com/unslothai/unsloth/issues/16294)：Windows 上使用 MiniMax-H3 且缺失 Flash Attention 时出现“无可用内核”崩溃——已通过 PR [#16421](https://github.com/unslothai/unsloth/pull/16421) 添加降级逻辑解决。
- 🔄 **UI/UX 问题**：  
  - 问题 [#16193](https://github.com/unslothai/unsloth/issues/16193)：桌面画廊中“在文件夹中显示”功能无响应——已通过 PR [#16429](https://github.com/unslothai/unsloth/pull/16429) 修复（尚未合并）。  
  - 问题 [#16303](https://github.com/unslothai/unsloth/issues/16303)：打开聊天后自定义背景重置——仍在调查中。

---

### **6. 对应用开发者的意义**  
- **构建健壮的跨平台智能体**：利用 `UNSLOTH_PARALLEL` 实现跨 TP/DP/PP 轴的可扩展分布式训练。新的能力注册表简化了复杂启动配置。
- **支持多样化的推理后端**：随着 MLX、Vulkan 与 FP8 LoRA 的支持，您的应用可在 Apple Silicon、AMD GPU 及低内存设备上高效运行。
- **妥善处理模型加载边缘情况**：使用 `trust_remote_code=True` 模型时需谨慎（问题 [#16331](https://github.com/unslothai/unsloth/issues/16331)），确保自定义代码路径在 CLI 中显式启用。
- **设计具备弹性的 UI 流程**：避免假设模型状态持久存在——用户在生成中途切换模型时可能遭遇瞬时崩溃（例如 #16436）。应实现重试逻辑与清晰的错误提示。
- **充分利用富媒体功能**：新富文本编辑器（PR [#16396](https://github.com/unslothai/unsloth/pull/16396)）与视频附件修复，使智能体交互更丰富——适用于多模态应用场景。

> ✅ *建议*：在 PR [#16433](https://github.com/unslothai/unsloth/pull/16433) 与 [#16421](https://github.com/unslothai/unsloth/pull/16421) 合并至生产前，锁定至 `main` 或使用 `v0.1.913-beta` 的稳定版本。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*