# AI Open Source Trends 2026-11-05

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-11-05 02:44 UTC

---

# **AI Open Source Trends Report**  
*Date: 2026-11-05*

---

## **1. Today's Highlights**

The AI open-source ecosystem is witnessing explosive momentum in **agent-native infrastructure**, particularly around persistent memory, context compression, and agent workflows that span multiple tools and environments. NVIDIA’s *auto-ontology* is emerging as a key player in semantic reasoning layers, enabling deterministic, explainable LLM interactions. Meanwhile, *claude-mem* and *mem0* are setting new standards for long-term agent memory, with *claude-mem* gaining over 1,170 stars today—highlighting demand for session-aware AI agents. The surge in *MoneyPrinterTurbo* and *Scrapling* signals strong interest in **AI-driven content automation**, especially video generation and web scraping for agent use cases.

---

## **2. Top Projects by Category**

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [NVIDIA/auto-ontology](https://github.com/NVIDIA/auto-ontology) | Python | 0 (+2,050) | A Python framework for building graph-based semantic reasoning layers over data, enabling deterministic query generation and better context retrieval for LLMs and agents. Emerging as a foundational tool for explainable AI workflows. |
| [garrytan/gstack](https://github.com/garrytan/gstack) | TypeScript | 0 (+290) | A curated CLI setup of 23 opinionated tools acting as CEO, Designer, Eng Manager, etc.—designed to power AI agent workflows with minimal configuration. Gaining traction as a "ready-to-run" agent dev environment. |
| [vercel-labs/agent-desktop](https://github.com/vercel-labs/agent-desktop) | Rust | 0 (+2,470) | A computer-use CLI for AI agents, allowing full system interaction via command line. Its rapid star growth indicates rising demand for terminal-native agent control. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 112,935 [topic:rag] (+1,172) | Persistent context storage across AI agent sessions using AI compression. Works with Claude Code, Copilot, Gemini, and more—now one of the most active RAG + agent integration tools. |
| [mattpocock/skills](https://github.com/mattpocock/skills) | Shell | 0 (+1,379) | A collection of real-world agent skills from an engineer’s personal `.agents` directory. Reflects growing community practice in sharing reusable, production-ready agent capabilities. |
| [scenario-labs/skills](https://github.com/scenario-labs/skills) | Python | 0 (+224) | Production-grade AI skills for generating images, video, audio, and 3D assets via the Scenario MCP. Focus on consistency, model selection, and cost-aware execution—ideal for creative agent pipelines. |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 297,292 [topic:llm] | Agent harness system optimizing performance, memory, security, and research-first development for Claude Code, Codex, Opencode, and Cursor. Massive community adoption signals shift toward agent orchestration maturity. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 139,089 [topic:llm] (+617) | One-click AI video generation from topics or keywords using automated workflows. Rapidly gaining attention as a go-to tool for creators leveraging LLMs and multi-modal models. |
| [D4Vinci/Scrapling](https://github.com/D4Vinci/Scrapling) | Python | 0 (+2,005) | The “best” web scraping framework handling everything from single requests to large-scale crawls. Built for AI agents; integrated with Discord and daily tips—becoming a de facto standard for agent data ingestion. |
| [Thysrael/Horizon](https://github.com/Thysrael/Horizon) | Python | 0 (+121) | AI-powered news radar that generates daily briefings in English and Chinese. Demonstrates rise in localized, personalized AI content delivery systems. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [FareedKhan-dev/kimi-k3-in-c](https://github.com/FareedKhan-dev/kimi-k3-in-c) | C | 0 (+351) | A 2.78-trillion-parameter Kimi K3 running inference on a single CPU with only 8.24 GB RAM. No BLAS, no GPU, no framework—revolutionizing lightweight LLM deployment. |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 5,554 [topic:llm-model] | Learn LLM inference on Apple Silicon. Builds a tiny vLLM + Qwen stack—ideal for systems engineers exploring edge LLM deployment. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 142,495 [topic:rag] | Turns codebases, docs, SQL schemas, configs, and PDFs into queryable knowledge graphs. Uses local AST parsing with deterministic explanations—no vector store required. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 93,515 [topic:rag] | An agent-native database that evolved into context infrastructure. Supports full lifecycle management of RAG pipelines with production-grade reliability. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 68,421 [topic:rag] | Drop-in memory layer for AI agents. Enables persistent context, built for production use—key enabler for long-running, self-evolving agents. |
| [volcengine/OpenViking](https://github.com/volcengine/OpenViking) | Python | 41,554 [topic:vector-db] | Self-evolving context database unifying agent memory, RAG, and skills. Designed for autonomous agent systems with dynamic knowledge updates. |

---

## **3. Trend Signal Analysis**

Today’s trends reveal a clear pivot toward **agent-native, persistent intelligence**. Tools like *claude-mem*, *mem0*, and *OpenViking* dominate the hot list, signaling that developers are prioritizing **long-term context retention** and **stateful reasoning** over one-off LLM calls. This aligns with recent advancements in LLMs capable of deeper self-reflection (e.g., o1-like reasoning), where memory and continuity are critical. 

A new stack is emerging: **RAG + Agent Memory + Skill Orchestration**, exemplified by *Graphify*, *ECC*, and *scenario-labs/skills*. These projects form a cohesive pipeline where knowledge graphs feed into memory systems, which then trigger skill execution—creating closed-loop intelligent workflows. This represents a move beyond simple prompt engineering toward **autonomous, adaptive agent ecosystems**.

Notably, **lightweight inference** is gaining traction—*kimi-k3-in-c* demonstrates that even massive models can run on CPUs without frameworks, opening doors for edge deployment. Meanwhile, **web scraping and data ingestion** (via *Scrapling*, *firecrawl*) are becoming foundational for agent training and operation, suggesting that access to real-time, diverse data is now a core capability—not an afterthought.

These developments coincide with the rollout of next-gen models like Kimi K3 and DeepSeek-V3, fueling demand for open-source tooling that enables **local, private, and scalable agent deployment**—a direct response to growing concerns about API costs and data privacy.

---

## **4. Community Hot Spots**

- **[thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)** – The fastest-growing agent memory solution; essential for any developer building persistent AI workflows.
- **[Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)** – A game-changer for knowledge representation; ideal for developers creating deterministic, explainable agents.
- **[FareedKhan-dev/kimi-k3-in-c](https://github.com/FareedKhan-dev/kimi-k3-in-c)** – Proof that trillion-parameter models can run locally—critical for privacy-focused, low-cost AI applications.
- **[harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)** – Represents the democratization of AI content creation; perfect for indie creators and startups.
- **[affaan-m/ECC](https://github.com/affaan-m/ECC)** – The definitive agent harness for performance optimization—essential for scaling agent workloads efficiently.

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*