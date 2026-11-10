# AI Open Source Trends 2026-11-10

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-11-10 04:17 UTC

---

# **AI Open Source Trends Report – 2026-11-10**

---

## **Step 1: Filter**
Filtered out non-AI projects (e.g., social media hunter, physics simulator, stock analysis without AI focus, general tools). Retained only repositories with clear AI/ML relevance: agent frameworks, LLMs, RAG systems, inference engines, and AI-powered automation.

---

## **Step 2: Categorize**
Projects categorized based on primary function. Multi-category entries are assigned a single primary category for clarity.

---

## **1. Today's Highlights**

Anthropic’s open-sourcing of **`skills`** and **`claude-quickstarts`** signals a strategic push toward developer enablement for AI agents, with `skills` gaining +1,038 stars in just one day. Meanwhile, **`firecrawl`** and **`Agent-Reach`** highlight growing momentum in web-scraping and internet-scale agent capabilities, enabling AI agents to act autonomously across the open web. The explosion of interest in **`affaan-m/ECC`** — an agent harness performance system — reflects rising demand for optimization and reliability in real-world AI workflows. Notably, **`open-webui`**, **`ollama`**, and **`langchain`** continue to dominate as foundational infrastructure, underscoring their role as de facto platforms for local and cloud-based AI application development.

---

## **2. Top Projects by Category**

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [**ollama/ollama**](https://github.com/ollama/ollama) | Go | 186,467 | A lightweight CLI for running local LLMs like Qwen, DeepSeek, and Gemma; widely adopted for rapid prototyping and edge deployment. |
| [**langchain-ai/langchain**](https://github.com/langchain-ai/langchain) | Python | 149,288 | The leading agent engineering platform enabling developers to build modular, tool-integrated AI workflows. |
| [**open-webui/open-webui**](https://github.com/open-webui/open-webui) | Svelte | 158,248 | User-friendly interface supporting Ollama, OpenAI, and more — critical for democratizing access to local LLMs. |
| [**0xPlaygrounds/rig**](https://github.com/0xPlaygrounds/rig) | Rust | 9,103 | Modular LLM application framework in Rust — emerging as a high-performance alternative for systems engineers. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [**NousResearch/hermes-agent**](https://github.com/NousResearch/hermes-agent) | Python | 262,130 | A self-evolving agent that grows with user needs; one of the most starred open-source personal agents. |
| [**Panniantong/Agent-Reach**](https://github.com/Panniantong/Agent-Reach) | Python | 110,669 | Empowers agents to browse Twitter, Reddit, YouTube, GitHub, and Bilibili via CLI — zero API fees, full autonomy. |
| [**career-ops-hq/career-ops**](https://github.com/career-ops-hq/career-ops) | JavaScript | 81,518 | Open-source AI job search agent that tailors resumes, scores jobs, and automates applications — runs locally or via plugins. |
| [**zhayujie/CowAgent**](https://github.com/zhayujie/CowAgent) | Python | 47,855 | Lightweight, multi-model, multi-channel agent with memory and skill extension — ideal for personal AI assistants. |
| [**HKUDS/nanobot**](https://github.com/HKUDS/nanobot) | Python | 49,980 | A minimal, privacy-focused personal agent built for efficiency and extensibility. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [**ZhuLinsen/daily_stock_analysis**](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 69,090 | LLM-driven stock analysis system with real-time news, decision dashboards, and automated alerts — fully self-hosted. |
| [**hugohe3/ppt-master**](https://github.com/hugohe3/ppt-master) | Python | 75,332 | Turns documents into native PowerPoint decks with animations, charts, and narration — ideal for AI content creation. |
| [**harry0703/MoneyPrinterTurbo**](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 141,690 | Automates HD short video generation from keywords using AI workflows — viral for content creators. |
| [**Vibe-Trading**](https://github.com/HKUDS/Vibe-Trading) | Python | 37,979 | Personal trading agent combining sentiment analysis, market data, and autonomous execution logic. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [**open-compass/opencompass**](https://github.com/open-compass/opencompass) | Python | 7,579 | Comprehensive LLM evaluation platform supporting 100+ datasets and models including GPT, Claude, Gemini, and Qwen. |
| [**skyzh/tiny-llm**](https://github.com/skyzh/tiny-llm) | Python | 5,701 | Builds a tiny vLLM + Qwen stack optimized for Apple Silicon — perfect for learning LLM inference at scale. |
| [**rasbt/LLMs-from-scratch**](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 108,122 | Step-by-step implementation of a ChatGPT-like LLM in PyTorch — essential for education and deep understanding. |
| [**genieincodebottle/generative-ai**](https://github.com/genieincodebottle/generative-ai) | Jupyter Notebook | 2,668 | Full roadmap and project library for generative AI — includes prompt engineering, coding prep, and use cases. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [**infiniflow/ragflow**](https://github.com/infiniflow/ragflow) | Go | 93,921 | Agent-native RAG engine evolved into context infrastructure — supports persistent knowledge and memory. |
| [**mem0ai/mem0**](https://github.com/mem0ai/mem0) | Python | 68,720 | Drop-in memory layer for AI agents — enables context persistence across sessions, crucial for long-term reasoning. |
| [**thedotmack/claude-mem**](https://github.com/thedotmack/claude-mem) | TypeScript | 115,464 | Persistent context storage for agents — compresses session history and injects relevant context back. |
| [**headroomlabs-ai/headroom**](https://github.com/headroomlabs-ai/headroom) | Python | 79,261 | Compresses tool outputs and logs before LLM input — cuts tokens by 20–95%, boosting efficiency. |
| [**run-llama/llama_index**](https://github.com/run-llama/llama_index) | Python | 52,862 | Document processing platform for AI — transforms unstructured data into queryable knowledge. |

---

## **3. Trend Signal Analysis**

The most explosive trend today is **agent-centric tooling and workflow automation**, particularly around **internet-scale agent reach and memory persistence**. Projects like `Agent-Reach`, `claude-mem`, and `ECC` reflect a maturing ecosystem where developers are no longer building agents from scratch but instead optimizing *how* they operate — reducing token costs, improving memory retention, and enabling seamless browsing. This shift indicates a move from *prototyping* to *production-grade* agent systems.  

A new tech stack is emerging: **Rust-based agent frontends** (`codewhale-hq/Codewhale`, `0xPlaygrounds/rig`) combined with **modular, low-latency vector databases** (`lancedb`, `qdrant`) and **lightweight memory layers** (`mem0`, `headroom`). These components form a high-performance, self-hosted stack ideal for private or edge deployments.  

This momentum aligns with recent LLM releases (e.g., Claude 3.5, Grok-3) that emphasize **long-context reasoning and agent-like behavior**, pushing the community to build better infrastructure. Additionally, Anthropic’s open-sourcing of `skills` and `quickstarts` suggests a strategic effort to standardize agent capabilities — reinforcing the idea that **tooling and interoperability** are now central to AI adoption.

---

## **4. Community Hot Spots**

- **[affaan-m/ECC](https://github.com/affaan-m/ECC)** — Performance optimization for agent harnesses is becoming a must-have; this repo is setting a new benchmark for agent reliability.
- **[firecrawl/firecrawl](https://github.com/firecrawl/firecrawl)** — Enables AI agents to gather real-time web data without API keys — critical for autonomous agents operating at scale.
- **[infiniflow/ragflow](https://github.com/infiniflow/ragflow)** — Represents the evolution of RAG into *context infrastructure*, not just retrieval — vital for long-haul agent memory.
- **[mem0ai/mem0](https://github.com/mem0ai/mem0)** — Offers a production-ready memory layer; essential for any agent aiming for persistence beyond a single session.
- **[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)** — Demonstrates how LLMs can be deployed for real-time, cost-free financial intelligence — a model for vertical AI apps.

---  
*Report generated: 2026-11-10 | Data sources: GitHub Trending, GitHub Search API (topic: ai-agent, llm-model, rag, llm, vector-db)*

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*