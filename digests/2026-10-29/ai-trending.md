# AI 开源趋势日报 2026-10-29

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-10-29 14:08 UTC

---

# AI 开源趋势报告 — 2026-10-29

筛选范围为 AI/ML 相关项目；非 AI 的通用趋势仓库已排除。Star 数按提供的数据呈现。

### 1. 今日亮点

智能体工具主导了今日的 AI 势头：[pbakaus/impeccable](https://github.com/pbakaus/impeccable) 以 +2,881 星领跑所有 AI 趋势仓库，而 [HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor)（+1,926）、[milind-soni/OpenMausBot](https://github.com/milind-soni/OpenMausBot)（+1,190）、[elayadesign/ai-design-skills](https://github.com/elayadesign/ai-design-skills)（+1,178）、[JetBrains/thinkrail](https://github.com/JetBrains/thinkrail)（+1,116）和 [obra/superpowers](https://github.com/obra/superpowers)（+1,037）进一步印证了对智能体技能、编码 IDE 和个人协作助手的需求。持久上下文正成为独立的基础设施层：[thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) 突破 104k 星，并同时出现在趋势和 RAG 搜索中，此外还有 [mem0ai/mem0](https://github.com/mem0ai/mem0)、[headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom)、[volcengine/OpenViking](https://github.com/volcengine/OpenViking) 和 [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex)。成熟智能体平台依然规模庞大——[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) 达 258k、[affaan-m/ECC](https://github.com/affaan-m/ECC) 达 292k、[Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) 达 187k、[browser-use/browser-use](https://github.com/browser-use/browser-use) 达 119k——但最新的活力集中在 harness/技能和记忆，而不是基础模型。本地优先和自托管 AI 仍通过 [ollama/ollama](https://github.com/ollama/ollama)、[open-webui/open-webui](https://github.com/open-webui/open-webui) 和 [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) 保持强劲。提示词安全内容也通过一个 jailbreaks 列表（+1,610）浮现，不过它更像是一个集合，而非可复用平台。

### 2. 按类别划分的顶级项目

#### 🔧 AI 基础设施

| 项目 | 语言 | Star 数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | Rust | 0 (+2881) | 为编码智能体提供设计词汇和评审层，在上线前发现低质 AI 产物。它以 +2881 星领跑今日 AI 趋势势头。 |
| [JetBrains/thinkrail](https://github.com/JetBrains/thinkrail) | TypeScript | 0 (+1116) | 轻量级 JetBrains AI IDE，面向 vibe coding，可适配开发者工作流。今日 +1116 星表明 IDE 原生编码智能体受到强烈关注。 |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 185,206 | 面向 Kimi、GLM、MiniMax、DeepSeek、gpt-oss、Qwen、Gemma 等模型的本地模型运行器。其广泛的模型支持使其持续处于本地优先 AI 开发的中心。 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 148,684 | 智能体工程平台，也是 LLM 应用最成熟的 SDK 之一。其 148,684 星的基数使其在智能体技术栈整合时成为默认集成层。 |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 78,223 | 在工具输出、日志、文件和 RAG 分块进入 LLM 前进行压缩。面向编码智能体的 token 成本与上下文窗口效率。 |
| [CopilotKit/CopilotKit](https://github.com/CopilotKit/CopilotKit) | TypeScript | 38,363 | 面向智能体和生成式 UI 的前端技术栈，包含 AG-UI Protocol。它正成为智能体后端与面向用户的 React/Angular 应用之间的关键桥梁。 |
| [0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig) | Rust | 9,014 | 用于构建模块化、可扩展 LLM 应用的 Rust 框架。它代表了对性能敏感的 AI 基础设施中不断增长的 Rust 技术栈。 |
| [Picovoice/picollm](https://github.com/Picovoice/picollm) | Python | 317 | 由 X-Bit 量化驱动的端侧 LLM 推理。它凸显了边缘与隐私 AI 作为小而新兴的基础设施细分领域。 |

#### 🤖 AI 智能体 / 工作流

| 项目 | 语言 | Star 数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [obra/superpowers](https://github.com/obra/superpowers) | Shell | 0 (+1037) | 智能体技能框架与软件开发方法论。今日 +1037 星显示对可复用智能体技能和流程规范的需求。 |
| [nightly-labs/openbot](https://github.com/nightly-labs/openbot) | TypeScript | 0 (+705) | 本地优先的桌面工作区，用于运行 Codex、Claude 和 Grok 的持久 AI 队友，支持任务队列和浏览器控制。它反映了向多智能体桌面工作区的转变。 |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 258,384 | 随用户成长并随时间适应的智能体。其 258,384 星使其成为数据集中最受关注的智能体框架之一。 |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 292,317 | 智能体 harness 性能优化，覆盖技能、本能、记忆、安全和研究优先的开发。面向 Claude Code、Codex、Opencode 和 Cursor 工作流。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 187,808 | 长期运行的自主智能体项目，聚焦让所有人可及 AI。它仍是衡量智能体平台雄心和社区规模的基准。 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 119,463 | 用于 AI 浏览器智能体的 Python 库，并提供托管的隐身浏览器和智能体 API。浏览器控制仍是实用智能体的高杠杆能力。 |
| [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach) | Python | 106,049 | 通过一个 CLI 让智能体跨 Twitter、Reddit、YouTube、GitHub 等平台进行读取/搜索。它解决了智能体研究和监控的数据访问瓶颈。 |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | Python | 44,096 | 用于构建弹性、有状态智能体的框架。它是 LangChain 生态中的核心编排层。 |

#### 📦 AI 应用

| 项目 | 语言 | Star 数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor) | Python | 49,244 (+1926) | HKUDS 的终身个性化辅导应用。它在今日 AI 应用中新增 Star 数最高，达 +1926，显示垂直 AI 教育的强劲需求。 |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 157,133 | 用户友好的 AI 界面，支持 Ollama、OpenAI API 等。其 157,133 星的基础使其成为本地和托管模型的默认自托管前端。 |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 136,248 | 使用 AI 工作流从主题或关键词生成高清短视频。它仍是自动化内容生产智能体的高星示例。 |
| [career-ops-hq/career-ops](https://github.com/career-ops-hq/career-ops) | JavaScript | 78,569 | 开源 AI 求职智能体，可扫描招聘板、根据简历给职位打分并定制申请。它显示垂直智能体正进入个人生产力领域。 |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 70,713 | 将文档或主题转换为带图表、旁白和模板的原生 PowerPoint 演示文稿。它把智能体工作流打包成高频办公交付物。 |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 67,697 | 面向文档和模型的本地优先、自有智能体体验。它吸引希望拥有私有 AI 而不租用智能能力的团队。 |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 67,252 | 由 LLM 驱动的多市场股票分析，包含新闻、仪表盘和定时通知。它是智能体金融工具的清晰示例。 |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 53,074 | AI 生产力工作室，包含智能聊天、自主智能体和 300+ 助手。它将前沿 LLM 聚合到单一桌面工作流中。 |

#### 🧠 LLM / 训练

| 项目 | 语言 | Star 数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 201,294 | 面向所有人的开源机器学习框架。尽管出现更新的技术栈，它仍是生产 ML 和训练流水线的基础。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 168,334 | 面向先进文本、视觉、音频和多模态模型的模型定义框架。它是模型架构和微调工作流的事实中心。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 105,169 | 具备强大 GPU 加速的张量和动态神经网络。它仍是现代 AI 的主要训练和研究基础。 |
| [scikit-learn/scikit-learn](https://github.com/scikit-learn/scikit-learn) | Python | 67,988 | 经典的 Python 机器学习库。它仍是表格 ML 和模型评估的实用基线。 |
| [jingyaogong/minimind](https://github.com/jingyaogong/minimind) | Python | 64,601 | 约两小时从零训练一个 64M 参数 LLM。它强烈表明人们对小型、可复现模型训练的兴趣。 |
| [keras-team/keras](https://github.com/keras-team/keras) | Python | 64,398 | 为人设计的深度学习 API，现支持多后端。它为训练和部署提供更简单的入口。 |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,556 | LLM 评估平台，覆盖 OpenAI、Anthropic、Gemini、Qwen、GLM、DeepSeek 和 100+ 数据集。评估正成为模型采用的门控环节。 |
| [datamllab/OpenEBFM](https://github.com/datamllab/OpenEBFM) | Python | 192 | 用于训练和扩展基于能量的基础模型的开源代码。这是自回归 LLM 之外一个细分但值得关注的研究方向。 |

#### 🔍 RAG / 知识

| 项目 | 语言 | Star 数（总计 / 今日） | 摘要 |
| :--- | :--- | ---: | :--- |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 104,300 (+324) | 捕获智能体会话，用 AI 压缩，并将相关上下文注入未来会话。它同时出现在趋势和 RAG 结果中，凸显持久记忆是热门层。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 138,334 | 通过 /graphify 技能将代码库、文档、SQL schema、配置和 PDF 转为可查询知识图谱。其无向量存储、确定性的 AST 方法是对默认 RAG 架构的值得注意的挑战。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 93,083 | 开源 RAG 引擎，将检索与智能体能力融合。它定位为 LLM 和生产级 RAG 的上下文层。 |
| [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | Python | 86,515 | 网页爬虫/抓取器，可将任意网站转换为干净、可供 LLM 使用的 Markdown。它仍是智能体和 RAG 流水线的关键摄取工具。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 68,047 | 面向 AI 智能体和应用的即插即用记忆基础设施。它面向生产级持久上下文。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 46,526 | 云原生向量数据库，用于可扩展的 ANN 搜索。它仍是检索密集型 AI 系统的核心后端。 |
| [volcengine/OpenViking](https://github.com/volcengine/OpenViking) | Python | 41,118 | 自演进的上下文数据库，统一智能体记忆、知识 RAG 和技能。它表明记忆与检索基础设施正在融合。 |
| [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) | Python | 39,887 | 面向无向量、基于推理的 RAG 的文档索引。它代表了从纯向量相似度转向推理优先检索的新兴趋势。 |

### 3. 趋势信号分析

社区最爆炸性的关注正流向智能体运营层，而不是新的基础模型。今日 AI 趋势新增 Star 最高的项目是设计/技能和 harness 项目——[pbakaus/impeccable](https://github.com/pbakaus/impeccable)（+2,881）、[HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor)（+1,926）、[milind-soni/OpenMausBot](https://github.com/milind-soni/OpenMausBot)（+1,190）、[elayadesign/ai-design-skills](https://github.com/elayadesign/ai-design-skills)（+1,178）、[JetBrains/thinkrail](https://github.com/JetBrains/thinkrail)（+1,116）和 [obra/superpowers](https://github.com/obra/superpowers)（+1,037）——外加本地优先的队友工作区，如 [nightly-labs/openbot](https://github.com/nightly-labs/openbot)。这表明团队正在标准化如何引导、审计、赋予工具并让智能体持久化，而不仅仅是调用哪个 LLM。

新技术栈明显偏重 TypeScript，用于桌面/智能体工作区和 React/AG-UI 前端（[CopilotKit/CopilotKit](https://github.com/CopilotKit/CopilotKit)、[nightly-labs/openbot](https://github.com/nightly-labs/openbot)、[CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio)），而 Rust 出现在对性能敏感或与 IDE 相邻的工具中（[pbakaus/impeccable](https://github.com/pbakaus/impeccable)、[codewhale-hq/Codewhale](https://github.com/codewhale-hq/Codewhale)、[0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig)）。基于 Shell 的技能框架表明，一个围绕 Claude Code、Codex、Gemini CLI 及类似编码智能体的打包层正在形成。

记忆和上下文也在分离为专用基础设施：[thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)、[mem0ai/mem0](https://github.com/mem0ai/mem0)、[headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom)、[volcengine/OpenViking](https://github.com/volcengine/OpenViking) 和 [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) 指向压缩、持久会话记忆、上下文数据库以及无向量的基于推理的检索。这是对更长智能体会话、工具输出膨胀以及反复向模型输入上下文成本的直接回应。模型运行器层仍然重要——[ollama/ollama](https://github.com/ollama/ollama) 对 Kimi、GLM、MiniMax、DeepSeek、gpt-oss、Qwen 和 Gemma 的支持使本地优先工作流保持可行——但差异化正向上转移到编排、记忆和垂直工作流打包。企业低代码和垂直智能体（[jeecgboot/JeecgBoot](https://github.com/jeecgboot/JeecgBoot)、[career-ops-hq/career-ops](https://github.com/career-ops-hq/career-ops)、[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)、[hugohe3/ppt-master](https://github.com/hugohe3/ppt-master)）表明智能体模式正从演示走向打包化、可重复的业务任务。

### 4. 社区热点

- **智能体技能与 harness 打包** — [obra/superpowers](https://github.com/obra/superpowers)、[pbakaus/impeccable](https://github.com/pbakaus/impeccable)、[affaan-m/ECC](https://github.com/affaan-m/ECC)。可复用技能、评审/审计循环、记忆和安全 harness 是 Star 增速最高的方向。
- **持久智能体记忆/上下文** — [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)、[mem0ai/mem0](https://github.com/mem0ai/mem0)、[headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom)。会话捕获、压缩和上下文注入正成为 Claude Code/Codex/Gemini 风格智能体的必备层。
- **本地优先智能体工作区** — [nightly-labs/openbot](https://github.com/nightly-labs/openbot)、[CopilotKit/OpenBot](https://github.com/CopilotKit/OpenBot)、[JetBrains/thinkrail](https://github.com/JetBrains/thinkrail)。桌面队友、浏览器控制、任务队列和 IDE 原生编码智能体正在融合。
- **无向量/上下文数据库 RAG** — [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)、[VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex)、[volcengine/OpenViking](https://github.com/volcengine/OpenViking)。知识图谱、推理优先检索以及统一的记忆/RAG/技能正在挑战纯向量技术栈。
- **垂直智能体应用** — [HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor)、[career-ops-hq/career-ops](https://github.com/career-ops-hq/career-ops)、[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)、[hugohe3/ppt-master](https://github.com/hugohe3/ppt-master)。教育、求职、金融和办公自动化显示出强劲的终端用户拉动力。

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*