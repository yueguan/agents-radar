# ArXiv AI Research Digest 2026-10-30

> Source: [ArXiv](https://arxiv.org/) (cs.AI, cs.CL, cs.LG) | 38 papers | Generated: 2026-10-30 02:07 UTC

---

# ArXiv AI Research Digest — 2026-10-30

## 1. Today’s Highlights

Today’s batch is dominated by efforts to make expressive LLM components practical: **ParaTTT** parallelizes deep-memory test-time-training layers, **GAAC** compresses KV caches losslessly, and **FSDP/GearUp** target communication and serverless inference overheads. Evaluation and training infrastructure is also maturing, with **Rubricon** automating rubric generation and new benchmarks such as **DecompGym** and **3DCodeEdit** targeting agentic algorithm design and 3D program editing. Agentic coding and theorem proving are increasingly studied through specifications, precedent-vs-intent conflicts, and recursive search, exemplified by **Skeleton Coding** and **EasyProver**. Safety and fairness work moves toward deployment realism: clinical multi-agent deliberation can **launder disparities**, agent memory is vulnerable to **role-grounding attacks**, and multilingual safety audits isolate which prompt components carry language effects.

## 2. Key Papers

Selected 15 papers from 38 submissions.

### 🧠 Large Language Models (architecture, training, alignment, evaluation)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [ParaTTT: Parallelizing Test-Time Training Layers with Deep Memories](http://arxiv.org/abs/2610.38255v1) | Noel Loo, Neehal Tumma, T. Konstantin Rusch et al. | Introduces a parallelization strategy for test-time-training layers whose nonlinear deep memory is trained online by gradient descent. This matters because it could make expressive recurrent/LLM layers practical at scale without sacrificing the benefits of deep fast-weight memory. |
| [Rubricon: Automated Rubric Generation and Refinement for LLM Evaluation and Training](http://arxiv.org/abs/2610.38220v1) | Ulyana Tkachenko, Hui Wen Goh, Guram Gogia et al. | Presents a pipeline that generates and refines discriminative rubrics for LLM evaluation and reward assignment without human preference labels or generator-model training. It lowers the cost of evaluating open-ended tasks where verifiable answers are unavailable. |
| [GAAC: Lossless KV Compression for Bandwidth-Efficient LLM Inference on Personal Devices](http://arxiv.org/abs/2610.38210v1) | Yipin Guo, Arun M George, Siddharth Joshi | Proposes a group-adaptive additive codec that losslessly compresses the KV cache for bandwidth-efficient long-context LLM inference on personal devices. It targets the growing KV-cache memory-bandwidth bottleneck left by weight-only quantization. |
| [Component Translation and Judge-Labelled Refusal: A Multilingual Safety Evaluation](http://arxiv.org/abs/2610.38181v1) | Yongxi Zhou, Junwei Yao, Wenbo Ye et al. | Introduces a paired-reversal multilingual safety evaluation that translates either the full request or the operation component into the same target language. The design isolates which prompt component carries language effects and uses judge-labelled refusal to compare safety behavior. |

### 🤖 Agents & Reasoning (planning, tool use, multi-agent, chain-of-thought)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Precedent or Intent? What Coding Agents Follow When Evolving Existing Code](http://arxiv.org/abs/2610.38244v1) | Haichao Zhu, Geyang Xu, Zhaorui Yang et al. | Studies whether coding agents follow past implementation precedent or new task intent when evolving existing code. The findings inform how specifications and repository context should be designed for reliable agentic software evolution. |
| [Skeleton Coding: Program Structure as a Specification for Agents](http://arxiv.org/abs/2610.38214v1) | Aditya Golatkar, Alessandro Achille, Renos Zabounidis et al. | Argues that program structure itself can serve as a compact specification for coding agents, reducing reliance on exhaustive tests and input-output examples. This could improve controllability and reduce specification burden in agent-assisted programming. |
| [DecompGym: Benchmarking LLM Agents on Designing Decomposition Algorithms](http://arxiv.org/abs/2610.38175v1) | Sikai Cheng, Tinghan Ye, Arnaud Deza et al. | Introduces a benchmark for LLM agents that design decomposition algorithms such as Benders decomposition and column generation. It targets hard, correctness-sensitive optimization workflows where agents must coordinate cuts, columns, and dual values. |
| [Is it my preference? Role Grounding Attack against Agent's Memory via Social Interaction](http://arxiv.org/abs/2610.38209v1) | Haoran Bu, Yiting Shen, Litian Zhang et al. | Identifies Role Grounding Misattribution, a vulnerability in agent memory where attackers exploit role-to-principal attribution through social interaction. It highlights a concrete security risk for persistent LLM agents that learn preferences from social platforms. |
| [Scaling Lean Theorem Proving for Real-World Code Verification](http://arxiv.org/abs/2610.38174v1) | Debargha Ganguly, Divya Raghunathan, Dejan Jovanović et al. | Presents a Lean proving system built from recursive agents with isolated contexts, asynchronous search, parallel tactic probing, and elastic elaboration. It reports 669/672 PutnamBench problems and all six IMO problems in the reported evaluation using an off-the-shelf frontier model. |

### 🔧 Methods & Frameworks (new techniques, benchmarks, efficiency improvements)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [The Endgame of FSDP Communication: Layout-Aware Collectives on Copy Engines, RDMA, and NVSwitch](http://arxiv.org/abs/2610.38228v1) | Zejian Wang, Yifan Zhao, Kaihui Ren et al. | Proposes layout-aware collectives for FSDP2 using copy engines, RDMA, and NVSwitch to avoid copying all-gather outputs and packing gradients into flat buffers. It addresses a remaining communication and memory-movement bottleneck in large-model training. |
| [GearUp: DAG-aware Acceleration for Serverless Inference Serving via Slack-guided Pre-loading](http://arxiv.org/abs/2610.38196v1) | Qi Qi, Yifan Sui, Hanfei Yu et al. | Accelerates serverless ML inference workflows by using DAG-aware, slack-guided pre-loading of libraries, weights, and CUDA kernels along dependency paths. It reduces cumulative startup and model-loading costs in function-level inference serving. |
| [3DCodeEdit: Benchmarking Instruction-Guided 3D Program Editing](http://arxiv.org/abs/2610.38231v1) | Binjie Wang, Xinran Zhao, Shoubin Yu et al. | Introduces a benchmark for instruction-guided editing of 3D programs, where models must understand what code builds and predict how edits reshape the result. It fills a gap between 3D generation and practical code-based editing workflows. |

### 📊 Applications (domain-specific, multimodal, code generation)

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Disparity Laundering: Consensus Without Fairness in Clinical Multi-Agent Systems](http://arxiv.org/abs/2610.38215v1) | Xiaoyang Wang, Tianrui Wang, Christopher C. Yang | Evaluates clinical multi-agent LLM teams across five model families and four aggregation topologies on 120 MIMIC-IV emergency cases. It finds that deliberation can launder disparities rather than average out individual bias, challenging assumptions about consensus-based fairness. |
| [The Indian Financial Automated Advice Bias (IFAAB) Dataset: Auditing Socioeconomic and Demographic Disparities in LLM-Generated Financial Advice in India](http://arxiv.org/abs/2610.38239v1) | Kriti Mahajan | Introduces a counterfactual audit dataset of 31,734 GPT-4o-generated financial advice responses in India to test socioeconomic and demographic disparities. It extends LLM bias auditing to Global South financial guidance, where advice can affect consequential decisions. |
| [MEV: A Multi-Event Video Dataset for Long-Take Generation](http://arxiv.org/abs/2610.38241v1) | Peiyuan Zhu, Shaoan Xie, Yifan Shen et al. | Presents a multi-event video dataset designed for long-take text-to-video generation with temporal dependencies across events. It provides supervision that short single-event clips lack, supporting more temporally consistent multi-event generation. |

## 3. Research Trend Signal

Across today’s papers, the clearest signal is co-design of model capability and systems efficiency. TTT parallelization, lossless KV compression, layout-aware FSDP collectives, and DAG-aware serverless pre-loading all treat inference and training bottlenecks as first-class research problems rather than deployment details. A second trend is the industrialization of evaluation: automated rubric generation, decomposition-algorithm benchmarks, 3D code-editing benchmarks, and Lean proving agents shift evaluation from static QA toward verifiable, multi-step workflows. Third, agent safety is becoming more structural: papers examine memory poisoning via role misattribution, fairness laundering in multi-agent clinical consensus, and component-level multilingual refusal behavior. Finally, domain audits and datasets—clinical triage, Indian financial advice, and long-take video—show growing attention to socioeconomic and temporal realism. Expect more work on agent specifications, security boundaries, and evaluation harnesses that can keep pace with tool-using models.

## 4. Worth Deep Reading

- **[ParaTTT: Parallelizing Test-Time Training Layers with Deep Memories](http://arxiv.org/abs/2610.38255v1)** — A core architecture/systems contribution: if the parallelization works, it addresses a major obstacle to using expressive deep-memory TTT layers at scale. Read for the parallelization design and its implications for recurrent/LLM architectures.

- **[Scaling Lean Theorem Proving for Real-World Code Verification](http://arxiv.org/abs/2610.38174v1)** — Strong empirical theorem-proving results plus a reusable recursive-agent and elastic-elaboration design. Read for how isolated contexts, asynchronous search, and parallel tactic probing combine into a scalable proving system.

- **[Is it my preference? Role Grounding Attack against Agent's Memory via Social Interaction](http://arxiv.org/abs/2610.38209v1)** — Identifies a concrete memory-security failure mode for socially interacting agents. Read for the threat model and implications for persistent agent memory, preference learning, and role-to-principal attribution.

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*