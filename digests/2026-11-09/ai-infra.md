# AI 基础设施日报 2026-11-09

> 生成时间: 2026-11-09 01:56 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# **跨项目AI基础设施生态报告 – 2026-11-09**

---

### **1. 生态概览**  
2026年末，AI推理基础设施领域呈现出激进的硬件专用化趋势，推理引擎与底层内核优化日益融合，并且面向代理（agent）原生工具链的成熟度持续提升。NVIDIA的Blackwell（SM120/121）已成为vLLM、SGLang和llama.cpp性能调优的核心焦点，而AMD ROCm与Intel XPU则通过实验性支持及MoE/FP8增强逐步获得关注。整个生态正迅速从单一的推理后端向模块化、可组合式架构演进——网关类项目如LiteLLM负责协调多种引擎，本地运行时（如llama.cpp）则赋能边缘部署。随着对推测解码、长上下文稳定性以及多模态就绪性的重视程度不断提升，生产级可靠性已成为关键差异化因素。

---

### **2. 活动对比**

| 项目       | 开放问题 | 开放PR | 最近发布？ | 状态说明 |
|---------------|-------------|----------|------------------|--------------|
| **vLLM**      | 47          | 32       | 否               | SM120上存在高严重性回归；正在积极修复内核问题 |
| **SGLang**    | 52          | 29       | 否               | 长上下文推理中出现关键性卡死；PD已完全启用 |
| **llama.cpp** | 39          | 28       | 否               | 正在修复推测解码偏差及NaN问题 |
| **Ollama**    | 43          | 31       | 否               | AMD Vulkan回归问题仍存；按模型配置现已合并 |
| **LiteLLM**   | 46          | 38       | ✅ `v1.110.0-dev.2` | 高优先级计费/流式传输缺陷正在修复中 |
| **Unsloth**   | 37          | 26       | 否               | ROCm段错误与Intel Arc加载失败仍为关键问题 |

> *注：所有项目今日均无新稳定版发布。LiteLLM在发布活跃度上领先，提供开发版本。*

---

### **3. 模型支持竞赛**

| 新模型 / 架构        | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|----------------------------------|------|--------|-----------|--------|---------|---------|
| **NVIDIA SM120 (Blackwell)**     | ✅ (活跃调试) | ✅ (完整FP8/MoE) | ✅ (Hopper/Q6_K opt) | ❌ | ❌ | ❌ |
| **AMD ROCm (Octave KV, 3-bit)**  | ✅ (实验性) | ⚠️ (有限支持) | ❌ | ❌ | ❌ | ❌ |
| **Intel XPU (MoE, Qwen3.5)**     | ✅ (持续移植) | ❌ | ❌ | ❌ | ❌ | ✅ (fp64修复) |
| **Qwen3-MoE / Qwen3.8-Flash-Next** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **FLUX.3 / Qwen-Image-2.1**      | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (增强CFG/动态配对) |
| **Rockchip NPU (rknpu)**         | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Ascend 950 NPU / DSA**         | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **TopxAI / Kitani**              | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

> 🔥 **胜出者：SGLang** — 在多硬件模型支持方面领先（包括RTX PRO 5000、Ascend），并已全面集成Longcat2.0、Qwen3-MoE等新兴模型。  
> 🏆 **亚军：Ollama** — 凭借Rockchip NPU支持及MLX/Vulkan的Docker补丁，在边缘计算场景中表现卓越。

---

### **4. 性能前沿**

| 优化重点           | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|-------------------------------|------|--------|-----------|--------|---------|---------|
| **KV缓存效率**       | ✅ Octave KV（3-bit），租约续期 | ✅ LIM/CopySFA卸载 | ❌ | ✅ 按层大小调整 | ❌ | ❌ |
| **MoE内核融合**         | ✅ TRT-LLM跳过填充，RFC统一入口 | ✅ FlashInfer cuTile默认 | ✅ 设备端调度器 | ❌ | ❌ | ❌ |
| **推测解码**      | ⚠️ draft-mtp确定性问题 | ✅ MTP预填充图 | ✅ 修复缓存错位 | ❌ | 🔴 流式丢失 | ❌ |
| **内核级调优**       | ✅ CUDA图稳定性，SM120内核 | ✅ FlashInfer b12x warp-MMA | ✅ TMA/WGMMA Hopper | ❌ | ❌ | ✅ Triton JIT特化 |
| **分布式服务**       | ✅ 前缀缓存，混合注意力 | ✅ 预填充-解码分离（PD） | ❌ | ❌ | ✅ 路由器 + Redis | ❌ |
| **量化灵活性**  | ✅ Humming后端提案 | ✅ 在线W4A4 MoE | ✅ IQ2加速 | ❌ | ❌ | ❌ |

> 💡 **趋势**：vLLM与SGLang正趋同于**针对特定内核的优化**（FP8、MoE、CUDA图），而**llama.cpp**则在**底层运行时鲁棒性**（确定性Top-K、内存泄漏修复）方面表现突出。**LiteLLM**聚焦于**可观测性与成本控制**，而非原始吞吐量。

---

### **5. 层级定位**

| 项目       | 主要层级             | 核心差异点 |
|---------------|----------------------------|--------------------|
| **vLLM**      | 推理引擎（GPU）       | 深度硬件专属内核；MoE + 前缀缓存领导者 |
| **SGLang**    | 推理引擎 + 代理栈 | 预填充-解码分离（PD）；多后端编排能力 |
| **llama.cpp** | 本地运行时（CPU/GPU）    | 跨平台兼容性；推测解码正确性 |
| **Ollama**    | 网关 + CLI运行时      | 按模型配置、边缘/NPU支持、可嵌入Modelfiles |
| **LiteLLM**   | LLM网关 / 代理层    | 多提供商路由、预算追踪、流可靠性 |
| **Unsloth**   | 扩散 + LLM运行时    | 首帧优化；T4上支持INT8；VAE效率 |

> 🔍 **定位洞察**：  
> - **vLLM/SGLang** 在高吞吐GPU推理领域占据主导地位。  
> - **Ollama/LiteLLM** 成为应用与引擎之间的“粘合层”。  
> - **llama.cpp/Unsloth** 面向需要跨平台、确定性或边缘执行的开发者。

---

### **6. 趋势信号**

#### **新兴行业趋势（基于2026-11-09活动）：**
1. **硬件专用化已成为必然要求**：SM120/GB10相关问题占据漏洞报告主流——性能不再可抽象化。开发者必须选择与其硬件栈对齐的引擎。
2. **长上下文稳定性是生产级障碍**：多个项目报告在100万token时出现卡死（SGLang）、提示截断（vLLM）及内存损坏（llama.cpp）等问题。这表明**长上下文推理在实验室外仍极不稳定**。
3. **代理系统要求精确性**：推测解码偏差（llama.cpp）、错误使用计费（LiteLLM）、流式泄漏（SGLang）直接影响代理可靠性——对生产环境而言是红色警报。
4. **多硬件支持正成为竞争壁垒**：具备广泛NPU/ROCm/XPU支持的项目（如Ollama、SGLang）在嵌入式及企业边缘部署中正快速崛起。
5. **量化灵活性 > 单一后端**：vLLM提出的**Humming**方案与llama.cpp的**IQ2模式缓存**，反映出向**可配置、可扩展量化**的转变，而非厂商锁定。

#### **应用开发者应重点关注：**
- ✅ 若使用Blackwell或长上下文，请避免 `v0.33.0`（vLLM）和 `v0.5.23`（SGLang）——两者均存在已知严重回归。
- ✅ 关注 `#50125`（LiteLLM）和 `#25618`（llama.cpp）：这些影响**成本准确性**与**代理输出一致性**。
- ✅ 启用 `num_parallel`（Ollama）和 `--enable-mfu-metrics`（vLLM）以实现更佳资源规划。
- ✅ 对推测解码路径进行严格测试——尤其是量化模型（如Q4_K_M等）及非BF16目标。

> 📌 **结论**：所谓“即插即用”推理的时代已经结束。选择技术栈时，应基于**硬件匹配度、长上下文稳定性、代理级正确性**，而非仅看峰值吞吐量。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest – 2026-11-09

---

### **1. 今日亮点**  
vLLM 项目正在推进针对 NVIDIA Blackwell（SM120）和 AMD ROCm 的深度硬件特定优化，重点包括 CUDA Graph 稳定性、MoE 内核融合以及多模态支持。核心工作涵盖修复在 RTX PRO 6000 Blackwell GPU 上的解码吞吐量下降问题、提升混合注意力模型中前缀缓存的鲁棒性，并为 AMD GPU 原生启用 3 位 KV 缓存（Octave KV）。

---

### **2. 版本发布与破坏性变更**  
过去 24 小时内无报告。未发布新版本，亦无破坏性 API/配置变更。

---

### **3. 新模型与硬件支持**  
- ✅ **NVIDIA SM120 (Blackwell)**：针对 `DeepSeek-V4.1-Flash`、`Qwen3.8-Flash-Next` 和 `Gemma-4` 在 8x RTX PRO 6000 Blackwell 上的调试与优化正在进行（通过 #56892、#65139、#62776）。  
- ✅ **AMD ROCm**：实验性支持 **Octave KV（3 位 KV 缓存）**（通过 #59773）；改进 FP8 MoE 回退逻辑（#66350）。  
- ✅ **多模态**：关于多模态模型中完整 ViT CUDA Graph 支持的 RFC（如 Qwen3-VL、Kimi K2.5）—— #38175。  
- ✅ **流式 ASR**：提议增加对 **NVIDIA Nemotron 3.5 ASR 流式处理（0.6b）** 的支持—— #47455。  
- ✅ **Intel XPU**：正在进行 Qwen3.5 模型及 MoE 解码融合向急切模式（eager mode）的移植工作—— #64687。

---

### **4. 性能与优化**  
- 🔥 **SM120 解码吞吐量修复**：由于损坏的 CUDA Graph 导致 `DeepSeek-V4.1-Flash` 在 SM120 上出现极低解码吞吐量的严重性能下降——已优先处理中（#56892）。  
- 🚀 **MoE 优化**：PR #65972 跳过 TRT-LLM 单体 MoE 内核中不必要的 DP 填充行，使用 vLLM 的 top-k 路由时降低计算开销。  
- ⚙️ **KV 缓存效率**：PR #66443 在 NIXL KV 连接器中引入租约续期与过期处理机制，避免传输停滞；在高负载下提升可靠性。  
- 📈 **MoE 内核统一**：RFC #37753 建议将 MoE “oracle” 统一为基于类的结构，以提升可维护性与可扩展性。  
- 💡 **量化灵活性**：RFC #65560 提议在所有 NVIDIA 架构（Hopper/Blackwell）上将 Marlin 替换为默认量化后端 **Humming**，支持 INT4/INT8 GPTQ/AWQ、FP8、MXFP8/MXFP4 等。

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 描述 | 状态 |
|--------|------|-------------|--------|
| 🔴 高 | #65139 | `v0.33.0` 版本中，在 SM120、NVFP4 MoE、fp8 KV、分块预填充、前缀缓存、MTP 规范解码场景下的 MTP 验证步骤出现 CUDA 非法内存访问 —— 自 `v0.32.0` 起的回归问题 | 开放 |
| 🔴 高 | #54173 | 在 GB10（sm_121）上使用前缀缓存时，GDN 路径出现 `CUBLAS_STATUS_INTERNAL_ERROR` / 非法内存访问；`--no-async-scheduling` 无效 | 开放 |
| 🔴 高 | #62776 | 在 GB10 上，`Qwen3.8-Flash-Next` 中超过 `indexer_budget`（约 2K tokens）的提示内容不可见——模型忽略 2K 之后的输入 | 开放 |
| 🟡 中 | #54308 | 在非 Blackwell ROCm GPU 上使用 `fp32 SSM cache` 时，Qwen3-Next GDN 解码坍缩为重复标记（`!!!!!!...`） | 开放 |
| 🟡 中 | #64874 | 在 GLM-5.3-Flash 上高并发场景下，`rocm_fp8_paged_mqa_logits` 出现 “Workspace is locked” AssertionError | 已关闭 |
| 🟡 中 | #32588 | 长音频转录（Whisper）中，每段的时间戳偏移约 0.5 秒 | 已关闭 |

> *注：多个高严重性问题与 SM120/GB10 硬件及最近的 v0.33.0 发布相关——使用这些平台的开发者应密切关注更新。*

---

### **6. 对应用开发者的启示**  
- 若您在 **NVIDIA Blackwell（SM120）** 上部署 **大型 MoE 模型（如 DeepSeek-V4.1、Qwen3.8-Flash-Next）**，预计性能将下降，直至修复上线——必要时可临时使用 `--enforce-eager`。  
- **混合注意力模型中的前缀缓存**（如 DSpark + EAGLE）在具有 ≥3 个注意力组的复杂模型上仍不稳定——测试时需谨慎，建议在 #51771 修复前禁用。  
- 针对 **多模态推理**，请为即将到来的 ViT CUDA Graph 支持（RFC #38175）做好准备，以提升视觉编码器吞吐量。  
- **AMD 用户** 应关注 Octave KV（3 位 KV 缓存）进展（#59773），在长上下文场景中有望实现 2–3 倍内存节省。  
- 若使用较旧 glibc（< 2.31），请避免依赖 `llguidance`——其已在 `v0.33.0` 中被强制要求（问题 #66345）。  
- 使用 `--enable-mfu-metrics` 可获取自定义量化配置下权重字节的准确估算——现已通过 #65143 支持。

> 🔗 [GitHub Issues](https://github.com/vllm-project/vllm/issues) | [PRs](https://github.com/vllm-project/vllm/pulls)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

### **SGLang Digest — 2026-11-09**

#### **1. 今日亮点**  
SGLang 项目持续深化对下一代硬件的支持，**Blackwell (SM12x) 内核优化**取得重大进展，包括为 RTX PRO 5000 和 GB10 GPU 新增 FP8 与 MoE 分派逻辑。关键稳定性修复解决了长上下文推理（如 GLM-5.3-Flash 在 100 万 token 情况下）中的持久性卡顿问题，以及测试工作流引发的 CUDA 核心转储。一项重要路线图任务——**预填充-解码拆分（PD）**——现已在多种模型族和并行策略中全面运行。

#### **2. 发布与破坏性变更**  
*过去 24 小时内未报告新版本发布或破坏性变更。*  
然而，`#49173` 中正在进行的工作揭示了由于 DSV4.1 中 SM120/SM121 索引错误，导致 GB10 上 DeepSeek-V4 预填充性能下降的回归问题，可能需要在 v0.5.24 发布前打补丁。

#### **3. 新模型与硬件支持**  
- **硬件**：通过调优的 FP8 GEMM 配置（`#45263`）实现对 **RTX PRO 5000 (SM120)** 的完整支持，以及对 **Ascend 950 NPU** 的显式压缩器状态处理（`#49000`）。  
- **模型**：新增对 Ascend arch22 NPU 的 **Longcat2.0** 支持（`#47917`），**Qwen3-MoE** 现已正确处理量化路由状态（`#49185`）。  
- **后端**：为 Ascend DSA 添加原生 **LIM/CopySFA KV 卸载**（`#47383`）；**FlashInfer cuTile NVFP4 MoE** 正被优先设为 SM12x 上的默认选项（`#49123`）。  
- **量化**：扩展 **XPU 上的 FP8 MoE 支持**（`#43994`）及 **Ascend 上的在线 W4A4 MoE**（`#32602`）。

#### **4. 性能与优化**  
- **DeepSeek-V4 预填充**：在 4× RTX PRO 6000（SM120，TP=4）上，吞吐量约为 2–7K tok/s，低于 vLLM/Marlin 的 12.5K，正调查后端调优方案（`#33422`）。  
- **CUDA Graphs**：修复 `HybridLinearAttnBackend` 在完整预填充图捕获期间失败的问题（`#48593`），实现稳定图复用。  
- **内核优化**：  
  - SM12x 上使用 FlashInfer b12x warp-MMA 实现 block-fp8-as-MXFP8，提升密集线性性能（`#45262`）。  
  - H200 上 Triton FA3 内核的前向计算比 flash-attn hopper 构建慢 1.5–3%（`#47734`）。  
  - Intel AMX 注意力内核通过将 SWA 索引转换移入 Triton 得到优化（`#48861`）。

#### **5. 稳定性与回归问题**  
| 问题 | 严重性 | 状态 | 备注 |
|------|----------|--------|-------|
| [`#48964`](https://github.com/sgl-project/sglang/issues/48964) | 严重 | 开放 | GLM-5.3-Flash（v0.5.23）在 100 万 token 请求后引擎状态损坏；v0.5.22 无此问题。 |
| [`#33549`](https://github.com/sgl-project/sglang/issues/33549) | 严重 | 已关闭 | 8×H20 上约 24.5 万上下文解码卡死；监控程序终止服务。 |
| [`#49133`](https://github.com/sgl-project/sglang/issues/49133) | 高 | 开放 | 可破坏的预填充 CUDA Graph 生成的 logprobs 与急切模式不同（Qwen3.8-27B-NVFP4）。 |
| [`#49173`](https://github.com/sgl-project/sglang/issues/49173) | 高 | 开放 | 由于索引器中遗漏 SM120/SM121，DSV4.1 在 GB10 上预填充性能回归——可能引发 8 倍性能下降。 |
| [`#26340`](https://github.com/sgl-project/sglang/issues/26340) | 高 | 开放 | CI 自动收集的 CUDA 核心转储——表明底层运行时存在不稳定性。 |

#### **6. 对应用开发者的影响**  
- **高吞吐推理场景**：优先使用 **SM12x 特定内核**（如 FlashInfer cuTile MoE、调优后的 FP8），并在 `#48593` 修复前避免在混合注意力模型中使用 `--cuda-graph-config {"prefill": "full"}`。  
- **长上下文应用**：若输入超过 10 万 token，避免使用 v0.5.23；请使用 v0.5.22 或打上 `#48964` 补丁。关注 `#49173` 以应对 DeepSeek-V4 预填充性能回归。  
- **智能体系统**：启用 `--hicache-mamba-size-gb`（`#47867`）以控制 Mamba 主机池大小，防止缓存缺失；使用 `#48963` 优化批处理预填充中的共享提示缓存。  
- **多模态应用**：确保图像占位符正确转义，避免 500 错误（`#37579`），并在流式会话中延迟媒体清理操作（`#49207`）。

> 🔗 *完整问题追踪：[sgl-project/sglang Issues](https://github.com/sgl-project/sglang/issues)*  
> 🔗 *进行中的 PR：[sgl-project/sglang PRs](https://github.com/sgl-project/sglang/pulls)*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

**llama.cpp 消息简报 – 2026-11-09**

---

### **1. 今日重点**  
最新更新聚焦于推测解码的稳定性、MoE 优化以及 GPU 后端的健壮性。关键修复解决了 CUDA/Metal 后端中确定性 Top-K 选择问题及循环状态中的 NaN 持续存在现象，同时新提交的 PR 加快了 IQ2 量化速度，并为 SYCL 后端启用了设备端 MoE 调度。这些改动提升了生产推理流水线的可靠性。

---

### **2. 发布与破坏性变更**  
今日未发布新的标记版本。但通过多个 PR 引入了若干高影响力变更：
- **`ggml` 基数 Top-K 现在确保在 CUDA 与 Metal 后端间实现确定性并列选择** ([#31084](https://github.com/ggml-org/llama.cpp/pull/31084)) — 影响 `--spec-type draft-mtp` 的行为。
- **MoE 专家缓存 CPU 等待修复**：防止在访问缓存专家时出现不必要的阻塞 ([#31888](https://github.com/ggml-org/llama.cpp/pull/31888))。
- **推测解码不再对草稿模型应用 MoE 缓存**，降低错位风险 ([#31497](https://github.com/ggml-org/llama.cpp/pull/31497))。

> 🔔 *使用量化模型（如 Q4_K_M）进行推测解码的开发者应验证输出一致性。*

---

### **3. 新模型与硬件支持**  
- **Apple Silicon (arm64)**：持续支持，并针对 SpacemiT IME2 MXFP4 密集重排进行了优化 ([#31776](https://github.com/ggml-org/llama.cpp/pull/31776))。
- **Intel Arc B 系列（Vulkan）**：正在处理长上下文 RAG 提示下持续的输出损坏问题 ([#31828](https://github.com/ggml-org/llama.cpp/issues/31828))；仍推荐使用变通方案 `GGML_VK_DISABLE_COOPMAT`。
- **SYCL 后端**：新增设备端 MoE 调度器 ([#30156](https://github.com/ggml-org/llama.cpp/pull/30156))，支持稀疏 MoE 模型的原生 GPU 路由。
- **量化格式**：
  - `llamafile` 中支持 BF16 与 `q8_0` VXE ([#31866](https://github.com/ggml-org/llama.cpp/pull/31866))。
  - 通过模式缓存加速 IQ2 量化创建 ([#31910](https://github.com/ggml-org/llama.cpp/pull/31910))。

---

### **4. 性能与优化**  
- **Hopper（H100）Q6_K 预填充路径**：使用 TMA 与 WGMMA 优化原生内核，提升密集量化推理速度 ([#31916](https://github.com/ggml-org/llama.cpp/pull/31916))。
- **CUDA MoE 上传流**：可选独立上传流用于 H2D 专家复制，减少竞争；可通过 `GGML_CUDA_UPLOAD_STREAM=1` 进行诊断 ([#31850](https://github.com/ggml-org/llama.cpp/pull/31850))。
- **IQ2 量化创建速度提升**：缓存重复模式，在不改变输出的前提下显著缩短多模型创建时间 ([#31910](https://github.com/ggml-org/llama.cpp/pull/31910))。
- **Vulkan**：为 SSM_SCAN 内核中 `d_state=64` 添加快速路径（对 Zamba2 风格模型至关重要）([#31818](https://github.com/ggml-org/llama.cpp/pull/31818))。

---

### **5. 稳定性与回归问题**  
今日报告的关键问题包括：
- **在贪婪采样（`temperature=0`）下，目标为量化模型（如 Q4_K_M）时推测解码出现分歧**，仅在 BF16 目标上匹配 ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)，39 条评论)。
- **由于乘零重置逻辑导致循环状态中持续出现 NaN** — 可能随时间引发无声退化 ([#31913](https://github.com/ggml-org/llama.cpp/pull/31913))。
- **双 Intel B70 SYCL 后端在生成令牌时存在内存泄漏** ([#27845](https://github.com/ggml-org/llama.cpp/issues/27845)，23 条评论)。
- **即使启用 `GGML_VK_DISABLE_COOPMAT` 变通方案，Intel Arc B 系列的 Vulkan 仍存在输出损坏** ([#31828](https://github.com/ggml-org/llama.cpp/issues/31828))。

> ✅ *NaN 与 Top-K 确定性问题已有修复；推测解码问题仍开放，但正在积极研究。*

---

### **6. 对应用开发者的启示**  
- **在量化模型上谨慎使用 `--spec-type draft-mtp`** — 通过以 `bf16` 为基准测试，确保输出与普通运行一致。
- **在复杂部署中启用 `GGML_CUDA_POOL_DEBUG=1`** 以调试 CUDA 内存碎片。
- **利用设备端 MoE 调度（SYCL）或独立上传流（CUDA）**，以提升大型 MoE 模型的吞吐量。
- **在 Intel Arc B 系列 Vulkan 上避免长上下文 RAG 流程**，直至 #31828 解决；可考虑回退至 `GGML_VK_DISABLE_COOPMAT`。
- **通过类似 [#31612](https://github.com/ggml-org/llama.cpp/pull/31612) 与 [#31915](https://github.com/ggml-org/llama.cpp/pull/31915) 的 PR** 实现自定义工具模式强制校验，确保 Gemma 4/Qwen3 中可靠调用 JSON 工具。

> 📌 *密切监控 #25618 与 #31913 — 它们直接影响依赖推测解码与长时间会话的智能体系统的正确性。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

**Ollama Digest – 2026-11-09**

---

### **1. 今日亮点**  
Ollama 生态系统持续演进，关键改进集中在模型加载效率和后端支持方面，尤其针对 Rockchip NPUs 与 NVIDIA MLX/Vulkan 等新兴硬件。重要缺陷修复包括 `gemma4:12b-it-qat` 元数据中的拼写错误，以及影响 AMD Radeon 780M Vulkan 用户的回归问题。开发者对并发控制与加载超时的细粒度管理需求日益增长，反映出其在生产级 AI 工作流中的采用率不断提升。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内无新版本发布。*  
但针对每模型配置的开发工作已接近完成：
- **PR #9546**（已合并）在 Modelfiles 中新增 `num_parallel` 支持，可覆盖全局 `OLLAMA_NUM_PARALLEL`，实现按模型的精细并发控制。
- **PR #16711** 在 Modelfiles 中引入 `PARAMETER env`，用于为每个模型设置环境变量——这对隔离不同模型的运行行为至关重要。

> 🔗 [PR #9546](https://github.com/ollama/ollama/pull/9546) | [PR #16711](https://github.com/ollama/ollama/pull/16711)

---

### **3. 新模型与硬件支持**  
- ✅ **Rockchip NPU (rknpu)**：PR #19480 为 Linux 系统上的 RK3588/RK3576 SoC 添加原生后端支持，通过 GGUF 模型实现预填充（prefill）与 MoE 操作的卸载。
- ✅ **NVIDIA MLX/Vulkan Docker**：PR #18592 修复了 Docker 容器中缺失的库和配置，恢复了 NVIDIA GPU 上 `vulkan` 与 `mlx` 运行器的功能。
- 🚧 **OSADIA Axiom 架构**：Issue #19611 请求官方支持墨西哥 Next GenAi 公司的专有神经架构，表明对自定义推理后端的兴趣正在上升。

> 🔗 [PR #19480](https://github.com/ollama/ollama/pull/19480) | [PR #18592](https://github.com/ollama/ollama/pull/18592) | [Issue #19611](https://github.com/ollama/ollama/issues/19611)

---

### **4. 性能与优化**  
- **KV 缓存大小计算**：PR #19327 根据各层实际头数动态调整 KV 缓存大小，提升显存估算精度——避免异构模型（如 MoE）中出现过度分配。
- **内存解析优化**：PR #19324 确保单次加载过程中重复的缓冲区行被聚合处理，防止模型初始化阶段丢失关键数据。
- **基准测试可靠性增强**：PR #19600 优化 `cmd/bench` 命令，使其在运行不完整时失败，并截断复用的输出文件——显著提升自动化性能测试的一致性。

> 🔗 [PR #19327](https://github.com/ollama/ollama/pull/19327) | [PR #19324](https://github.com/ollama/ollama/pull/19324) | [PR #19600](https://github.com/ollama/ollama/pull/19600)

---

### **5. 稳定性与回归问题**  
| 严重程度 | 问题 | 摘要 | 修复状态 |
|--------|------|--------|----------|
| ⚠️ 高 | #17748 ([AMD Vulkan 回归](https://github.com/ollama/ollama/issues/17748)) | Radeon 780M 在 v0.32.10 之后出现 `radv/amdgpu: Not enough memory for command submission` | ❌ 开放 |
| ⚠️ 中 | #19596 ([gemma4:12b-it-qat 中的拼写错误](https://github.com/ollama/ollama/issues/19596)) | 元数据中将 `tools` 错写为 `toos` → 导致工具调用失败 | ✅ 已修复（待合并 PR） |
| ⚠️ 中 | #19604 ([代理后 403 Forbidden](https://github.com/ollama/ollama/issues/19604)) | 即使 `curl` 可用，拉取仍失败；源于企业级 Blue Coat CA 证书问题 | ❌ 开放 |
| ⚠️ 低 | #19613 ([`--version` 显示警告](https://github.com/ollama/ollama/issues/19613)) | 版本检查时出现意外的“无法连接”警告 | ❌ 开放 |

> 🔗 [Issue #17748](https://github.com/ollama/ollama/issues/17748) | [Issue #19596](https://github.com/ollama/ollama/issues/19596) | [Issue #19604](https://github.com/ollama/ollama/issues/19604)

---

### **6. 对应用开发者的意义**  
- **并发控制现已按模型粒度生效**：借助 Modelfiles 中的 `num_parallel`，你可将高并发模型（如 LLM）与低延迟嵌入模型并行运行，避免资源争用。此功能适用于多模型服务的吞吐量优化。
- **加载时间过长？请增加超时**：默认 `OLLAMA_LOAD_TIMEOUT=300s` 对复杂模型或慢速存储而言过短（参见 Issue #19616）。若观察到提前加载失败，请在部署配置中适当延长该值。
- **安全与暴露风险**：通过公共代理（如 Cloudflare）暴露 Ollama 而未启用 API 认证，易引发滥用问题（参见 Issue #19595）。务必通过认证机制或 IP 白名单进行访问控制。
- **嵌入维度探测**：使用 `ollama show <model>` 查询模型维度（PR #19612 已更新文档说明）——这对向量数据库集成至关重要。

> 🔗 [PR #19612](https://github.com/ollama/ollama/pull/19612) | [Issue #19616](https://github.com/ollama/ollama/issues/19616) | [Issue #19595](https://github.com/ollama/ollama/issues/19595)

---  
*摘要生成时间：2026-11-09 | 来源：github.com/ollama/ollama*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

**LiteLLM 简报 – 2026-11-09**

---

### **1. 今日亮点**  
LiteLLM 生态系统持续成熟，为生产级 LLM 代理部署带来了可观测性和可靠性的重要提升。主要更新包括增强的预算追踪、对 Bedrock 和 DeepSeek 的流式处理优化，以及新增的供应商集成（Kitani、TopxAI）。关键修复解决了流式数据丢失、使用量统计错误，以及跨源 URL 处理中的安全漏洞问题。

---

### **2. 发布与破坏性变更**  
- **v1.110.0-dev.2**：今日发布，采用 cosign 更新了 Docker 镜像签名。通过 [commit `e82a3c6`](https://github.com/BerriAI/litellm/commit/e82a3c646908ba0a6ee0beddc4310d7a6cac4065) 中的密钥验证。  
  - *注意*：此为开发版本；稳定分支目前尚未预计有破坏性变更。  
  - 部署前请务必验证签名：[cosign 验证指南](https://docs.sigstore.dev/cosign/overview/)。

---

### **3. 新模型与硬件支持**  
- ✅ **TopxAI** 作为官方支持的 OpenAI 兼容供应商加入，通过 [PR #41919](https://github.com/BerriAI/litellm/pull/41919)，支持 11 个文本模型的成本追踪与路由。  
- ✅ **Kitani** 已作为第一类供应商集成 ([PR #50656](https://github.com/BerriAI/litellm/pull/50656))，通过 OpenAI 兼容接口支持开源权重模型。  
- 🔧 **DeepSeek Fetch Tool** 支持已添加 ([PR #50618](https://github.com/BerriAI/litellm/pull/50618))，与现有 Search 工具并行——对依赖外部数据检索的智能体工作流至关重要。

> *今日未引入新的硬件后端（CUDA/ROCm/Metal/CPU）或量化格式。*

---

### **4. 性能与优化**  
- 🚀 **路由器优化**：PR [#50641](https://github.com/BerriAI/litellm/pull/50641) 通过跳过未使用的部署读取和缓存 Anthropic 能力探测，减少热路径开销——直接改善高并发下的延迟表现。  
- ⚡ **Redis 效率**：PR [#31880](https://github.com/BerriAI/litellm/pull/31880) 消除了无速率限制键的冗余 Redis 写入，显著降低大规模场景下的 I/O 负载。  
- 📊 **支出日志性能**：PR [#49556](https://github.com/BerriAI/litellm/pull/49556) 为关键工作器取消和 Redis 重播场景增加了回归测试覆盖，防止故障时出现重复计费。

---

### **5. 稳定性与回归问题**  
| 严重性 | 问题 | 状态 | 修复 PR |
|--------|------|--------|-------|
| 🔴 高 | 尽管非流式调用计数正确，但流式使用量严重低估 (`#36114`) | 开放 | 正在处理——根本原因在流聚合层 |
| 🔴 高 | Bedrock 上 `/v1/messages` 流在客户端断开后仍持续生成 (`#50514`) | 开放 | 已在 [PR #50657](https://github.com/BerriAI/litellm/pull/50657) 中修复 |
| 🔴 高 | `output_config` 导致 Xiaomi MiMo 模型调用失败 (`#24549`) | 已关闭 | 已在 dev 分支打补丁 |
| 🟡 中 | `ModelNotMappedError` 抛出时缺乏上下文信息 (`#47922`) | 开放 | 尚无修复方案——需改进错误报告机制 |
| 🟡 中 | Windows wheel 包含以 CRLF 换行结尾的 tiktoken 缓存 (`#41855`) | 开放 | 导致导入时需网络访问——需临时解决方案 |

> *影响计费准确性和流完整性的重要缺陷正在积极修复中。*

---

### **6. 对应用开发者的影响**  
- **谨慎配置 `max_budget` 与 `rate_limit`**：`#50125` 问题表明预算检查可能允许超出限额的调用——请在生产环境中仔细验证阈值。  
- **使用 Bedrock + 智能体时确保正确的流处理逻辑**：若使用 `ANTHROPIC_MESSAGES_MAX_DETACHED_STREAM_DRAINS=0` 的分离式中断机制，请应用 [PR #50657](https://github.com/BerriAI/litellm/pull/50657) 的修复。  
- **立即利用新供应商**：通过极少配置变更集成 Kitani 与 TopxAI——适用于开源权重模型的成本优化推理。  
- **启用 `turn_off_message_logging` + `redact_message_content`**：使用 [PR #50622](https://github.com/BerriAI/litellm/pull/50622) 保护调试日志中的敏感个人信息，同时保留完整可观测性。  
- **密切监控支出指标**：Prometheus/Grafana 与 LiteLLM UI 之间的差异（`#26611`）表明可能存在数据聚合缺口——建议通过 `SpendLogDelivery` 扫描进行对账。

> *对智能体开发者：在 v1.110.0 发布前，请避免对 Xiaomi MiMo 模型使用 `output_config`。务必彻底测试通过 DeepSeek `Fetch` 工具路径的流式响应。*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

**Unsloth Digest – 2026-11-09**

---

### **1. 今日亮点**  
Unsloth 持续推进扩散模型与大语言模型推理的深度优化，针对 Turing 架构 GPU（T4）及 Intel Arc A 系列显卡修复了 fp8/INT8 性能问题，并显著提升了在多种硬件上的模型加载可靠性。关键 PR 包括通过 `UNSLOTH_DIFFUSION_INT8_GEMM_RACE` 在 T4 上启用 INT8 支持、解决 Intel XPU 上 float64 兼容性问题，以及稳定 Qwen-Image-2.1 与 FLUX.3 的首次渲染编译。

---

### **2. 发布与破坏性变更**  
*过去 24 小时内未检测到新发布或破坏性变更。*  
现有行为保持不变；无需迁移说明。

---

### **3. 新模型与硬件支持**  
- ✅ **Intel Arc A 系列（Arc 140V）**：已通过 `PR #17070` 修复因缺少 `float64` 支持导致的模型加载失败问题。  
- ✅ **Turing GPU（T4, sm75）**：通过设置 `UNSLOTH_DIFFUSION_INT8_GEMM_RACE=0` 实现 INT8 量化支持（`PR #17167`）——此前该功能被直接拒绝。  
- ✅ **Qwen-Image-2.1 与 FLUX.3**：增强对动态 CFG 配对的支持（`PR #17177`），并改进在仅支持 fp16 设备上处理 bf16 权重的能力（`PR #17173`）。  
- ✅ **MiniMax-H3 Sol VAE**：优化设备读取与窗口掩码逻辑（`PR #17085`, `#17100`），有效降低主机与 GPU 间的同步开销。

---

### **4. 性能与优化**  
- 🚀 **INT8 GEMM/GEMV**：修复未列出显卡中的竞争条件问题（`PR #17084`, `#17106`），并优化分组线性层的分块内存使用。  
- ⚙️ **Triton 内核特化**：万能融合块现在根据序列长度生成单一二进制文件（`PR #17101`），减少冗余 JIT 编译。  
- 🔍 **内存效率**：在 MiniMax-H3 VAE 中将设备到主机的读取次数降低 192 倍（`PR #17100`），并在 Qwen-Image-2.1 中消除冗余的 KV 缓存读取（`PR #17118`）。  
- 💡 **FlashInfer 预构建内核**：已将预构建内核安装至 vLLM 引擎环境（`PR #17063`），彻底消除首次启动时的 JIT 编译延迟。

---

### **5. 稳定性与回归问题**  
- ⚠️ **严重**：`unsloth-rocm` 在 gfx1031（RX 6700 XT）上因错误识别 RDNA 2 架构导致崩溃，首次 GPU 操作即触发段错误（`Issue #17147`）。*修复待处理。*  
- ⚠️ **高**：图像生成模型在 Intel Arc 140V 上无法加载（`Issue #17137`）。*根本原因关联 fp64 支持问题（`PR #17070`）。*  
- ⚠️ **中等**：在 torch < 2.12 环境下，FP8 去噪器在首次渲染时卡死（`Issue #17166`）。*已在 `PR #17166` 中修复。*  
- ⚠️ **低**：外观设置标签页存在界面不一致问题（重复设置项、选择器自动关闭）（`Issues #17161`, `#17162`, `#17163`, `#17169`）。*相关 PR 正在开发中。*

---

### **6. 对应用开发者的意义**  
- **通过 `UNSLOTH_DIFFUSION_INT8_GEMM_RACE=0` 在 T4 及旧款 GPU 上启用 INT8** —— 解锁老旧硬件上的高效推理能力。  
- **避免在数据集中使用 `tool_calls=None`** —— 请使用 `PR #17176` 安全处理 Llama 3.x/GPT-OSS 模板中的空 tool calls。  
- **近期修复后，Intel Arc 与 AMD ROCm 平台上的扩散管道将更稳定**；建议使用更新配置测试 `FLUX.3` 与 `Qwen-Image-2.1`。  
- **关注界面稳定性**：多个与外观相关的缺陷可能影响 Studio 用户体验——建议提交报告或贡献修复。

> 🔗 [GitHub Issues](https://github.com/unslothai/unsloth/issues) | [GitHub PRs](https://github.com/unslothai/unsloth/pulls)

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*