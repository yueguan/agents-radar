# ArXiv AI Research Digest 2026-11-04

> Source: [ArXiv](https://arxiv.org/) (cs.AI, cs.CL, cs.LG) | 50 papers | Generated: 2026-11-04 02:40 UTC

---

**ArXiv AI Research Digest — 2026-11-04**

---

### **Today's Highlights**  
Recent submissions highlight a growing focus on *efficiency*, *alignment*, and *real-world deployment* of large language models. Breakthroughs in KV cache compression (e.g., FlagRot, LoopKV) and reasoning optimization (e.g., PACT, Say Less, Mean Better) signal strong momentum in reducing inference cost without sacrificing performance. Notably, *evaluation integrity* is under scrutiny—new work exposes how rubrics, judges, and evaluation contracts can distort model comparisons. Meanwhile, agent systems are evolving beyond single-task execution: frameworks like EvoGroup and Strategist enable persistent collaboration and adaptive reasoning strategies. The convergence of LLMs with robotics (Pixel-UMI, DiabAgent) and healthcare (Rethinking Next-Visit Prediction, Balancing CoT) underscores the shift from abstract benchmarks to tangible, high-stakes applications.

---

### **Key Papers**

#### 🧠 Large Language Models (architecture, training, alignment, evaluation)
| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Neologism Inoculation Prompting](http://arxiv.org/abs/2611.04260v1) | Africa, Holmes et al. | Introduces a prompt-based inoculation method to reduce reward hacking by training models on harmful examples under explicit behavioral prompts; enhances robustness in frontier models without retraining. |
| [COMPREHEND: Ultra-Fine-Grained Benchmark Understanding by Entailment Hierarchy](http://arxiv.org/abs/2611.04249v1) | Balakrishnan et al. | Proposes a hierarchical taxonomy of user prompts using entailment-based clustering, enabling precise benchmarking of LLM behavior across fine-grained use cases. |
| [Evaluation Contracts and Evaluation Leverage: Sliced-Gap Readings in Structured Prediction Can Move Without Model Change](http://arxiv.org/abs/2611.04099v1) | Lu et al. | Demonstrates that performance gaps (e.g., intra-inter) are not model properties but artifacts of evaluation design—challenging assumptions in NLP benchmarking. |
| [Swapping the Judge Changes the Ranking: A Preregistered Study of Cross-Provider Replication in LLM-Judged System Comparisons](http://arxiv.org/abs/2611.04090v1) | Han, Kim | Reveals significant variance in system rankings when using different LLM judges—even across providers—undermining trust in LLM-evaluated results. |

#### 🤖 Agents & Reasoning (planning, tool use, multi-agent, chain-of-thought)
| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [AlgoPilot: Cross-Paradigm Reasoning in Language Models via Strategy Selection and Guidance](http://arxiv.org/abs/2611.04079v1) | Hao et al. | Enables LLMs to dynamically select between symbolic, programmatic, and planning paradigms for complex tasks, improving solution quality across domains. |
| [EvoGroup: A Self-Evolving Harness for Group Agents in Shared Contexts](http://arxiv.org/abs/2611.03868v1) | Xu et al. | Introduces a memory-aware framework for collaborative group agents that evolves over time through shared context, enabling long-term team coordination. |
| [Strategist: Evolving Reasoning with Agents](http://arxiv.org/abs/2611.03618v1) | Lu et al. | Presents a meta-agent that learns and refines reasoning strategies across tasks, breaking the cycle of hand-coded inference patterns. |
| [DiabAgent: Toward Agentic Physiological Control](http://arxiv.org/abs/2611.03601v1) | Li et al. | Develops an LLM agent capable of sustained, real-time insulin dose adjustment in type 1 diabetes, advancing agentic control in clinical environments. |

#### 🔧 Methods & Frameworks (new techniques, benchmarks, efficiency improvements)
| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [FlagRot: Selective Rotation for Low-Bit KV Caches](http://arxiv.org/abs/2611.03992v1) | Shao et al. | Challenges conventional wisdom on KV cache quantization by showing selective rotation improves accuracy more than uniform rotation—critical for 2-bit caching. |
| [LoopKV: Loop-Aware KV Cache Compression for Serving Looped Transformers](http://arxiv.org/abs/2611.04137v1) | Zhou et al. | Designs a cache compression scheme tailored for looped transformers, enabling higher batch sizes and lower memory overhead during inference. |
| [Say Less, Mean Better: Training-Free Verbosity Control for Language Models](http://arxiv.org/abs/2611.04107v1) | Alyosify et al. | Proposes a method to control response length without prompting or retraining, reducing latency and cost while preserving completeness. |
| [Dory1.0-4B: A Looped Normalized-GPT](http://arxiv.org/abs/2611.03998v1) | Fishman et al. | Introduces Dory1.0-4B, a 4B-parameter looped language model with elastic depth—same checkpoint supports variable compute budgets. |

#### 📊 Applications (domain-specific, multimodal, code generation)
| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [SLATE: A Large-Scale Benchmark for Long-Horizon API Selection in E-commerce Tool Use](http://arxiv.org/abs/2611.03982v1) | Wei et al. | Creates SLATE, a benchmark for LLM agents selecting APIs across long workflows in e-commerce—addressing critical challenges in enterprise automation. |
| [From Natural Language to Exact Cover: A Neuro-Symbolic Approach to Zebra Puzzles](http://arxiv.org/abs/2611.03641v1) | Skaisgiris et al. | Combines neural and symbolic reasoning to solve logic puzzles traditionally resistant to pure LLMs, demonstrating hybrid reasoning’s power. |
| [Pixel-UMI: Learning Environment-Aware Whole-Body Manipulation from Sensorized Human Demonstrations](http://arxiv.org/abs/2611.03647v1) | Liu et al. | Enables robots to learn whole-body manipulation policies from human demonstrations, adapting to environment dynamics via retargeting. |
| [WebHarbor: Docking Real Websites for Evolving GUI Agent Environments](http://arxiv.org/abs/2611.03634v1) | Wang et al. | Provides a stable, real-web docking platform for GUI agents, bridging synthetic training and live evaluation. |

---

### **Research Trend Signal**  
A clear shift toward *practical deployability* and *evaluation rigor* dominates today’s submissions. Efficiency gains—from KV cache compression (FlagRot, LoopKV) to verbosity control (Say Less, Mean Better)—reflect growing pressure to reduce inference costs. Parallelly, researchers are critically examining evaluation practices: studies show that model rankings vary significantly based on judge choice (Swapping the Judge), scoring functions (Evaluation Contracts), and even rubric phrasing (Same Rule, Different Verdicts). This signals a maturing field moving beyond performance metrics toward *reproducibility* and *trustworthiness*. On the agent front, self-evolving frameworks (EvoGroup, Strategist) and long-horizon tool use (SLATE) point to a future where AI systems operate autonomously in complex, dynamic environments. Finally, the integration of LLMs with physical systems (robotics, physiology) and domain-specific tasks (healthcare, finance) indicates a broader trend toward *mission-critical AI*—where models must be reliable, interpretable, and safe.

---

### **Worth Deep Reading**
1. **[Neologism Inoculation Prompting](http://arxiv.org/abs/2611.04260v1)** – Offers a novel, scalable approach to aligning frontier models by training them on their own harmful behaviors, potentially revolutionizing safety engineering without costly retraining.
2. **[Strategist: Evolving Reasoning with Agents](http://arxiv.org/abs/2611.03618v1)** – Introduces a paradigm shift: reasoning as a learnable, reusable skill. If scalable, this could eliminate the need for per-task prompt engineering.
3. **[Swapping the Judge Changes the Ranking](http://arxiv.org/abs/2611.04090v1)** – A foundational study exposing fragility in LLM evaluation. Its preregistered methodology sets a new standard for transparency and reproducibility in AI benchmarking.

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*