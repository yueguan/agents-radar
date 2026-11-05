# AI 开源趋势日报 2026-11-05

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-11-05 02:44 UTC

---

# **AI 开源趋势报告**  
*日期：2026-11-05*

---

## **1. 今日亮点**

AI 开源生态正迎来爆发式增长，特别是在**原生代理基础设施**领域，包括持久化内存、上下文压缩以及跨多工具与环境的代理工作流。NVIDIA 的 *auto-ontology* 正在成为语义推理层的关键参与者，支持确定性、可解释的 LLM 交互。与此同时，*claude-mem* 与 *mem0* 正在为长期代理记忆树立新标准，其中 *claude-mem* 今日新增超过 1,170 颗星——凸显了对会话感知型 AI 代理的需求。*MoneyPrinterTurbo* 与 *Scrapling* 的迅速走红，反映出市场对**由 AI 驱动的内容自动化**的强烈兴趣，尤其是视频生成和用于代理场景的网页抓取。

---

## **2. 各类别顶级项目**

### 🔧 AI 基础设施

| 项目 | 语言 | 星标数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [NVIDIA/auto-ontology](https://github.com/NVIDIA/auto-ontology) | Python | 0 (+2,050) | 一个基于图结构的语义推理层构建框架，可在数据上实现确定性查询生成与更优的上下文检索，适用于 LLM 与代理。正逐步成为可解释 AI 工作流的基础工具。 |
| [garrytan/gstack](https://github.com/garrytan/gstack) | TypeScript | 0 (+290) | 一套精选的命令行工具集合，包含 CEO、设计师、工程经理等角色配置，专为减少配置开销而设计，以驱动 AI 代理工作流。正迅速成为“开箱即用”的代理开发环境。 |
| [vercel-labs/agent-desktop](https://github.com/vercel-labs/agent-desktop) | Rust | 0 (+2,470) | 专为 AI 代理设计的终端级计算机操作命令行工具，支持通过命令行完全控制主机系统。其星标快速增长，表明终端原生代理控制需求持续上升。 |

### 🤖 AI 代理 / 工作流

| 项目 | 语言 | 星标数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 112,935 [topic:rag] (+1,172) | 利用 AI 压缩技术，在多个代理会话间实现持久上下文存储。兼容 Claude Code、Copilot、Gemini 等平台，现已成为最活跃的 RAG + 代理集成工具之一。 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | Shell | 0 (+1,379) | 来自工程师个人 `.agents` 目录的真实世界代理技能集合。反映了社区在共享可复用、生产就绪的代理能力方面的日益成熟实践。 |
| [scenario-labs/skills](https://github.com/scenario-labs/skills) | Python | 0 (+224) | 通过 Scenario MCP 实现图像、视频、音频及 3D 资产生成的生产级 AI 技能。聚焦一致性、模型选择与成本感知执行，是创意代理流水线的理想选择。 |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 297,292 [topic:llm] | 优化性能、内存、安全性和研究导向开发的代理调度系统，支持 Claude Code、Codex、Opencode 与 Cursor。庞大的社区采纳率标志着代理编排成熟度的提升。 |

### 📦 AI 应用

| 项目 | 语言 | 星标数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 139,089 [topic:llm] (+617) | 一键式 AI 视频生成工具，支持从主题或关键词自动触发工作流。因其结合 LLM 与多模态模型的能力，迅速成为创作者的首选工具。 |
| [D4Vinci/Scrapling](https://github.com/D4Vinci/Scrapling) | Python | 0 (+2,005) | “最佳”网页抓取框架，涵盖单请求到大规模爬取。专为 AI 代理设计，集成 Discord 与每日提示功能——正成为代理数据摄入的事实标准。 |
| [Thysrael/Horizon](https://github.com/Thysrael/Horizon) | Python | 0 (+121) | AI 驱动的新闻雷达，每日生成中英文简报。展示了本地化、个性化 AI 内容分发系统的兴起。 |

### 🧠 LLM / 训练

| 项目 | 语言 | 星标数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [FareedKhan-dev/kimi-k3-in-c](https://github.com/FareedKhan-dev/kimi-k3-in-c) | C | 0 (+351) | 一个拥有 2.78 万亿参数的 Kimi K3 模型，仅使用 8.24 GB RAM 在单个 CPU 上运行推理。无需 BLAS、GPU 或框架——彻底革新轻量级 LLM 部署方式。 |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 5,554 [topic:llm-model] | 在 Apple Silicon 上学习 LLM 推理。构建极小体积的 vLLM + Qwen 堆栈——非常适合系统工程师探索边缘 LLM 部署。 |

### 🔍 RAG / 知识库

| 项目 | 语言 | 星标数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 142,495 [topic:rag] | 将代码库、文档、SQL 模式、配置文件与 PDF 转换为可查询的知识图谱。采用本地 AST 解析并提供确定性解释——无需向量存储。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 93,515 [topic:rag] | 原生代理数据库演进而来，发展为上下文基础设施。支持 RAG 流水线全生命周期管理，具备生产级可靠性。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 68,421 [topic:rag] | AI 代理的即插即用记忆层。支持持久化上下文，专为生产环境设计——是长时运行、自我演化的代理的关键使能者。 |
| [volcengine/OpenViking](https://github.com/volcengine/OpenViking) | Python | 41,554 [topic:vector-db] | 自我演化的上下文数据库，统一代理记忆、RAG 与技能。专为具备动态知识更新能力的自主代理系统设计。 |

---

## **3. 趋势信号分析**

今日的趋势清晰地指向**原生代理、持久智能**的转变。*claude-mem*、*mem0* 与 *OpenViking* 等工具占据热门榜单前列，表明开发者正优先考虑**长期上下文保留**与**有状态推理**，而非一次性调用 LLM。这与近期具备更深层次自我反思能力的 LLM（如 o1 类推理）进展相吻合，其中记忆与连续性至关重要。

一种新堆栈正在形成：**RAG + 代理记忆 + 技能编排**，以 *Graphify*、*ECC* 与 *scenario-labs/skills* 为代表。这些项目构成连贯的流水线——知识图谱为记忆系统提供输入，进而触发技能执行，构建闭环智能工作流。这标志着从简单的提示工程迈向**自主、自适应的代理生态系统**。

值得注意的是，**轻量级推理**正获得关注——*kimi-k3-in-c* 证明即使超大规模模型也能在无框架、无 GPU 情况下于 CPU 上运行，为边缘部署打开新可能。与此同时，**网页抓取与数据摄入**（如 *Scrapling*、*firecrawl*）正成为代理训练与运行的基础，表明实时、多样化数据访问已成核心能力，而非附加选项。

这些进展恰逢 Kimi K3 与 DeepSeek-V3 等新一代模型的发布，进一步推动对开源工具的需求，以实现**本地化、私密化、可扩展的代理部署**——这是对日益增长的 API 成本与数据隐私担忧的直接回应。

---

## **4. 社区热点**

- **[thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)** – 发展最快的代理记忆解决方案；任何构建持久化 AI 工作流的开发者都不可或缺。
- **[Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)** – 知识表示领域的变革者；适合打造确定性、可解释代理的开发者。
- **[FareedKhan-dev/kimi-k3-in-c](https://github.com/FareedKhan-dev/kimi-k3-in-c)** – 证明万亿参数模型可在本地运行——对注重隐私、低成本的 AI 应用至关重要。
- **[harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)** – 代表了 AI 内容创作的民主化；非常适合独立创作者与初创企业。
- **[affaan-m/ECC](https://github.com/affaan-m/ECC)** – 性能优化的代理调度系统标杆；高效扩展代理工作负载的必备工具。

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*