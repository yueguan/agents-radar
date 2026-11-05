# ArXiv AI Research Digest 2026-11-05

> Source: [ArXiv](https://arxiv.org/) (cs.AI, cs.CL, cs.LG) | 50 papers | Generated: 2026-11-05 02:44 UTC

---

---

### **Today's Highlights**  
Recent submissions highlight a growing focus on *agent-centric AI*, particularly in autonomous decision-making, multi-agent collaboration, and the integration of human oversight. Key advances include frameworks for proactive memory in LLM agents, strategic clarification in dialogue, and efficient expert pruning in MoE models. There’s also strong momentum in *evaluation rigor*, with new benchmarks like CARTBench and SlackBench addressing real-world complexity in shopping advice and code collaboration. Notably, studies reveal systemic biases—such as the male abstraction bias—and expose vulnerabilities in agent-based judgment systems, underscoring the need for robust, auditable AI.

---

### **Key Papers**

#### 🧠 Large Language Models (architecture, training, alignment, evaluation)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Modality-Aware Expert Pruning for MoE-Based Multimodal Large Language Models](http://arxiv.org/abs/2611.05927v1) | Donggeun Kim et al. | Proposes modality-aware pruning to reduce memory overhead in MoE-MLLMs by selectively deactivating experts not aligned with input modalities. This enables scalable, efficient multimodal reasoning without sacrificing performance. |
| [Suppressing Misaligned Behaviors in Language Models, Durably](http://arxiv.org/abs/2611.05358v1) | Luxi He et al. | Introduces a framework to durably suppress emergent misaligned behaviors in LLMs using controlled model organisms. Demonstrates that post-training interventions can prevent re-emergence, improving long-term reliability. |
| [Aligning Language Models with General Preferences: An Empirical Study of Direct Nash Optimization](http://arxiv.org/abs/2611.05373v1) | Austin Tsai et al. | Presents DNO-Prct, a practical method for aligning LLMs with general preferences via Nash equilibrium approximation. Offers a scalable alternative to reinforcement learning with human feedback. |
| [When Does Better Confidence Improve a Knowledge-Graph Audit?](http://arxiv.org/abs/2611.05877v1) | Zehao Lin et al. | Reveals that higher confidence does not always improve audit outcomes due to label validity and repairability limits. Challenges assumptions about confidence as a proxy for accuracy in knowledge graph validation. |

#### 🤖 Agents & Reasoning (planning, tool use, multi-agent, chain-of-thought)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Autonomous agents for electricity-system planning](http://arxiv.org/abs/2611.05911v1) | Yoon Pyo Lee et al. | Introduces an autonomous framework that iteratively proposes, evaluates, and refines electricity system plans using agent-driven scenario analysis. Enables scalable, adaptive infrastructure planning under complex constraints. |
| [Prospective Memory for Proactive LLM Agents](http://arxiv.org/abs/2611.05731v1) | Yuanchen Bei et al. | Develops a prospective memory mechanism enabling LLM agents to retain future-directed intentions and act upon them when conditions arise. A foundational step toward truly proactive AI assistants. |
| [Learning When Human Decisions Matter: Executor-Relative Consultation Value for LLM Agents](http://arxiv.org/abs/2611.05814v1) | Shuli Jiang et al. | Proposes a metric to quantify when human input is most valuable in LLM workflows, optimizing human-in-the-loop efficiency. Crucial for designing hybrid AI-human systems where human capacity is limited. |
| [TandemBench: Benchmarking Decision Models for Multi-Agent Collaboration](http://arxiv.org/abs/2611.05827v1) | Baohang Li et al. | Introduces TandemBench, a benchmark of 1,200 questions testing coordination in multi-agent cooking scenarios. Provides a rigorous testbed for evaluating decision-making in collaborative settings. |

#### 🔧 Methods & Frameworks (new techniques, benchmarks, efficiency improvements)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Routes of Reasoning: Reading and Reshaping Test-Time Compute in MoE Models](http://arxiv.org/abs/2611.05919v1) | Shengjie Wang et al. | Identifies “overthinking” in MoE models as a waste of compute; proposes a dynamic routing strategy to redirect test-time resources toward high-value reasoning steps. Improves efficiency and performance. |
| [RAGCompass: Generalizable Retriever Routing via Reranking](http://arxiv.org/abs/2611.05596v1) | Valentin Six et al. | Introduces a reranking-based router that selects the best retriever for each query, improving retrieval quality across diverse domains. Addresses the lack of universal retriever performance. |
| [LoopVQ: Sub 1-bit KV Cache Quantization for Looped Transformers](http://arxiv.org/abs/2611.05304v1) | Minsoo Cheong et al. | Achieves sub-1-bit quantization for KV caches in looped transformers, drastically reducing memory usage while preserving performance. Enables deployment of large models on low-resource devices. |
| [You don't need long context](http://arxiv.org/abs/2611.05306v1) | Peter Belcak et al. | Shows frontier LLMs achieve near-optimal accuracy with frequent compaction, challenging the necessity of long context. Suggests cost-effective alternatives for long-horizon tasks. |

#### 📊 Applications (domain-specific, multimodal, code generation)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [CARTBench: Rubric-Based Evaluation of Shopping Advice for Complex and Ambiguous Requests with Timelines](http://arxiv.org/abs/2611.05862v1) | Yuqin Dai et al. | Presents CARTBench, a rubric-based benchmark for evaluating LLMs on complex, timeline-dependent shopping requests. Addresses data scarcity and standardization issues in e-commerce AI evaluation. |
| [SlackBench: Benchmarking Agents on Collaborative Projects Grounded in Real Code Repositories](http://arxiv.org/abs/2611.05512v1) | Mingxuan Li et al. | Introduces SlackBench, a benchmark grounded in real-world code repositories to evaluate AI agents in collaborative software development. Captures real dynamics missed by synthetic or isolated tasks. |
| [MedPhysBench: A Benchmark for Evaluating Language Models on Medical Physics Calculations and Error Detection](http://arxiv.org/abs/2611.05340v1) | Omar Nusrat | Proposes MedPhysBench to assess LLMs in radiation therapy calculations—a high-stakes domain where errors have life-or-death consequences. Fills a critical gap in clinical AI evaluation. |
| [PAYWORTHY: Can LLMs Predict Which Freelance Jobs an AI Agent Will Complete?](http://arxiv.org/abs/2611.05759v1) | Mohammad Reza Rezaei et al. | Tests whether LLMs can predict success before any attempt, using 1,494 real freelance job postings. Opens new avenues for pre-screening and resource allocation in AI labor markets. |

---

### **Research Trend Signal**  
A clear shift toward *practical, deployable AI systems* is evident in today’s submissions. Researchers are moving beyond pure model scaling to focus on *efficiency*, *reliability*, and *real-world integration*. The rise of agent-centric frameworks—like autonomous electricity planners, proactive memory systems, and multi-agent benchmarks—reflects growing maturity in AI’s role as a digital coworker. Concurrently, there is increasing attention to *evaluation integrity*: benchmarks now emphasize real-world data (e.g., SlackBench), adversarial testing (False Reassurance), and audit safety (Information-Matched Auditing). Efficiency innovations—from sub-1-bit quantization to compact retrieval routing—suggest a pragmatic pivot toward deployment-ready models. Underlying all this is a deeper awareness of *bias and risk*: from gendered abstraction patterns to false reassurance in weather alerts, researchers are no longer just building smarter models but more accountable ones.

---

### **Worth Deep Reading**

1. **[Prospective Memory for Proactive LLM Agents](http://arxiv.org/abs/2611.05731v1)**  
   *Why*: This paper redefines what it means for an AI agent to be “proactive.” By embedding future-directed intention tracking into LLMs, it lays the groundwork for agents that anticipate needs rather than react. This is foundational for next-gen personal assistants and autonomous systems.

2. **[SlackBench: Benchmarking Agents on Collaborative Projects Grounded in Real Code Repositories](http://arxiv.org/abs/2611.05512v1)**  
   *Why*: Most agent benchmarks are synthetic or task-isolated. SlackBench uses real codebases and team dynamics, making it one of the most realistic tests yet. It exposes how well agents handle ambiguity, version control, and human-AI collaboration—critical for enterprise adoption.

3. **[False Reassurance in LLM Severe-Weather Guidance: A Replayable Evaluation Against Real NWS Alerts](http://arxiv.org/abs/2611.05290v1)**  
   *Why*: This study reveals a potentially dangerous failure mode: LLMs failing to recognize active warnings. Using replayable real-world cases, it provides a sobering case study in AI safety for high-stakes domains. A must-read for anyone deploying LLMs in public safety applications.

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*