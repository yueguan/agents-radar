# AI CLI 工具社区动态日报 2026-10-29

> 生成时间: 2026-10-29 14:08 UTC | 覆盖工具: 7 个

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## 横向对比

# 跨工具 AI CLI 对比报告 — 2026-10-29

*以下所有数字均为摘要中报告的观察结果，而非完整仓库总量。“N/A”表示摘要中未提供计数，并不表示该渠道不活跃。本组中没有工具被认定在上游禁用了 Issues/PR。*

## 1. 生态概览

AI CLI 工具正逐渐趋同于智能体式开发工作区：MCP 集成、会话持久化、成本/配额可见性、长时间运行目标和基于审批的安全机制，如今都是核心竞争面。发布节奏并不均衡——OpenCode 和 Qwen Code 发布了版本，Codex 发布了 Rust alpha 版本，而 Claude Code、Gemini CLI、Copilot CLI 和 Pi 在过去 24 小时内没有发布任何版本——但所有工具的 Issue 流量仍然很高。最严重的反复出现风险包括静默数据/配置丢失、认证与 MCP 凭据生命周期故障、提示缓存/token 浪费，以及 Windows/桌面端一致性缺口。竞争正从原始模型访问能力转向运行可靠性、企业治理和跨平台一致性。

## 2. 活动对比

| 工具 | Issues / Issue 活动 | PR | 讨论 | 发布状态 |
|---|---:|---:|---:|---|
| **Claude Code** | 50 个 Issue 有更新；最热线程 #27302 有 289 条评论 / 435 👍 | 2 个有更新 | N/A | 无发布 |
| **OpenAI Codex** | 总数未报告；重点列出 10 个热门 Issue；#36040 有 99 条评论 | 重点列出 10 个关键 PR | 10 项：2 个 Ideas、3 个 Q&A、5 个 Show-and-tell | 2 个 Rust alpha：`0.166.0-alpha.6`、`alpha.5.1` |
| **Gemini CLI** | `area/agent` 中 30 个热门 Issue 有更新 | 重点列出 10 个关键 PR | N/A | 无发布 |
| **GitHub Copilot CLI** | 重点列出 10 个热门 Issue；#552 有 31 条评论 | 0 个有更新 | N/A | 无发布 |
| **OpenCode** | 重点列出 10 个热门 Issue；#59387 有 98 条评论 | 重点列出 10 个关键 PR，另注明 4 个 | N/A | `v2.2.1`；`@opencode/pty@0.3.2` |
| **Pi** | 重点列出 10 个热门 Issue；#7547 有 91 条评论 | 重点列出 10 个关键 PR | 1 个讨论 | 无发布 |
| **Qwen Code** | 重点列出 10 个热门 Issue；#14724 有 79 条评论 | 重点列出 10 个关键 PR | N/A | `v0.26.3`、桌面端、nightly、TypeScript SDK |

## 3. 共同功能方向

- **成本、配额与提示缓存透明度**  
  Claude Code 希望提供持久的使用量指示器，并减少可避免的缓存写入（#80261、#105564）。Codex 用户希望月度重置可预测（#57967）。Copilot CLI 报告 PRU 消耗以及过早注入 MCP schema 导致启动 token 膨胀（#4802、#4613）。OpenCode 出现 1.28M 缓存写入 token、零读取的情况（#48542），还有一个 98 条评论的免费层配额讨论串（#59387）。Pi 报告工具顺序导致提示缓存失效（#11529），以及 Bedrock 缓存元数据缺失（#11528）。Gemini CLI 正致力于节省 token 的上下文管理（#19561）。  
  **共同需求：** 可见的支出、缓存命中/未命中指标、有边界的图像/上下文保留，以及防止不可见 token 浪费的控制手段。

- **会话身份、记忆与生命周期控制**  
  Claude Code 需要与路径无关的项目身份，以及更安全的保留策略（#101522、#41458、#74682）。Codex 正在加入消息编辑/撤销和聊天导航（#11086、#2379、#8582）。Gemini CLI 正在强化会话历史保留和取消语义（#30019、#30017）。OpenCode 正在探索持久记忆（`/teach`、`/recall`、`/memory`），并且现已发布 `/goal`。Pi 正在修复默认会话目录和 `/resume` 语义（#11561、#11562）。Qwen Code 正在打磨会话概览、归档、导出和恢复流程。  
  **共同需求：** 持久、可移植的会话；明确的保留策略；跨机器项目身份；可预测的恢复/归档行为。

- **MCP 监管与认证生命周期**  
  Claude Code 存在围绕 401 辅助调用和按 worktree 范围回收 MCP 服务器的认证/MCP 可靠性问题（#105408、#105857）。Copilot CLI 存在 MCP OAuth DCR 过期、静默认证失败和 GHE 代理缺口（#4017、#5284、#5286）。OpenCode 需要 MCP 预启动/预热与重连（#48743）。Codex 正在为共享 MCP 连接增加按聊天租约（#57978）。Pi 发现 MCP 重连会因工具顺序变化而使提示缓存失效（#11529）。  
  **共同需求：** MCP 正从“支持它”转向“监管它”——预热、重连、凭据轮换、租约隔离和企业代理兼容性。

- **可强制执行的安全与权限控制**  
  Codex 用户报告提示词层面的禁令被违反，并希望获得可强制执行的文件限制（#57827）；Codex 的 PR 增加了 Guardian 审查和全新的人工审批检查点。Gemini CLI 有维护者提出的诉求：劝阻破坏性 git 操作并改进沙箱隔离（#22672、#19873）。Copilot CLI 请求在非交互模式下采用默认拒绝并配合显式允许列表（#5275）。OpenCode 在项目目录失败时会静默回退到 `$HOME`（#59817）。Pi 希望使用私有 `0700` 会话目录（#10770）。  
  **共同需求：** 策略级沙箱、文件范围强制执行、审批检查点，以及显式失败而非静默回退。

- **平台与可观测性一致性**  
  Claude Code 在 Web 界面缺少 OpenTelemetry，并且存在 Windows 远程控制回归（#32364、#105658）。Codex 存在反复出现的 Windows 桌面端回归（#50704 及相关问题）。Gemini CLI 存在 Wayland 和 Windows/PowerShell 问题。Copilot CLI 存在 Windows 剪贴板、PowerShell 环境变量继承和沙箱预检失败问题。OpenCode 有一组 Windows 后台服务问题。Pi 那个 91 条评论的 Windows 讨论串是最清晰的平台碎片化信号。Qwen Code 在 CI/Web Shell 和 Remote-SSH 覆盖方面存在缺口。  
  **共同需求：** 一流的 Windows/Wayland 支持、桌面端/Web 一致性，以及可在 CLI、Web 和 IDE 界面间工作的遥测。

- **长时间运行智能体的控制与可观测性**  
  OpenCode 发布了 `/goal`，但也有一份 500+ 步失控循环的报告（#49042）。Gemini CLI 的头号 Issue 是子智能体 MAX_TURNS 被报告为成功（#22323），此外还有通用智能体挂起（#21409）。Codex 正在增加 Guardian 审查和审批检查点。Claude Code 存在 routine 会话泛滥和云端 routine 挂起问题。  
  **共同需求：** 步数/预算上限、真正能停止子进程的取消机制、子智能体轨迹可见性，以及由审查者验证的完成状态，而非乐观的状态报告。

## 4. 差异化分析

- **Claude Code** 面向企业和高频重度用户。其最强信号是身份、MCP/认证可靠性、OTel 一致性、Connector 多账号支持和成本控制。它在本组中 Issue 互动最重，但今日 PR 吞吐量低，表明这是一个成熟产品，同时积压了大量可靠性工作。

- **OpenAI Codex** 覆盖 CLI、桌面端、云端和移动端远程控制。其技术方向包括 Rust alpha 构建、Guardian 审查/审批基础设施、MCP 连接租约和 Code Mode 输出截断。它尤其关注策略执行和桌面端/远程工作流，但图像密集型上下文和 Windows 桌面端回归是主要摩擦点。

- **Gemini CLI** 是最明确以子智能体和编排为中心的工具。它正投入于 AST 感知的代码库工具、节省 token 的上下文管理、沙箱隔离和子智能体可观测性。其弱点是终态正确性：虚假成功报告、无限期挂起，以及非交互式 `-p` 模式的可靠性，直接威胁 CI/脚本化使用。

- **GitHub Copilot CLI** 面向 GitHub 企业工作流。差异化在于 ACP 协议完整性、MCP 认证、GHE/代理行为、权限语义和 BYOK/配置自省。它在认证持久化方面存在强烈的企业痛点——Entra、macOS keychain、Ubuntu keychain 和 MCP OAuth——且今日 PR 更新为零，修复通过其他渠道落地。

- **OpenCode** 是本组中发布最快的开源风格选项。`v2.2.1` 增加了持久化 `/goal` 循环，同时 PR 增加了 Code Mode 谓词门控、Mermaid 渲染、消息操作菜单以及协议/CI 修复。其采用摩擦在于 Zen 免费层配额状态、缓存经济性和 Windows 后台服务可靠性。

- **Pi** 是最聚焦终端/扩展的工具。其社区强调 Windows 可用性、全屏 TUI 性能、扩展 API 一致性、会话存储语义，以及针对 Bedrock/Anthropic 兼容流量的提供商路由。一个社区 Rust 移植版（`pi-rs`）表明市场对非 Node 运行时和行为一致性测试有需求。

- **Qwen Code** 面向 Qwen 以及本地/OpenAI 兼容后端，提供 CLI、Desktop、Web Shell 和 TypeScript SDK 界面。它正在发布版本和托管会话权限，但其 Issue 追踪器被 CI 归因噪声、发布主机容量，以及一个新的 P1 配置回归——其中 `contextWindowSize` 被静默移除——所主导。

## 5. 社区动能与成熟度

**可见参与度最高：** Claude Code 拥有最大的单个讨论串（#27302，289 条评论）和 50 个有更新的 Issue。Codex 拥有评论数最高的新回归信号（#36040，99 条评论）。OpenCode 的配额讨论串（#59387，98 条评论）、Pi 的 Windows 讨论串（#7547，91 条评论）和 Qwen 的 CI 门禁讨论串（#14724，79 条评论）是各项目中最响亮的信号。

**PR 吞吐：** Codex、Gemini CLI、OpenCode、Pi 和 Qwen Code 各自重点列出了约 10 个关键 PR。Claude Code 有 2 个有更新的 PR；Copilot CLI 为 0。这并不意味着 Claude/Copilot 不活跃——两者都表现出大量分诊和 Issue 活动——但其可见的修复吞吐在本时间窗口内较低。

**快速迭代：** OpenCode 和 Qwen Code 正在发布版本，同时积极调整 UX 和基础设施。Codex 正通过 Rust alpha 和安全/审查 PR 迭代。Gemini CLI 正处于围绕会话历史、取消、shell 安全和工具调用正确性的大规模修复浪潮中。Pi 正在落地许多聚焦维护的修复，但没有发布版本。

**成熟度画像：** Claude Code、Codex 和 Copilot CLI 已获得企业级采用，但仍表现出严重的认证、身份、代理和平台一致性债务。OpenCode 和 Qwen Code 迭代迅速，但显示出配额、CI 和发布基础设施方面的成熟度缺口。Pi 由社区驱动且维护负担重，平台碎片化是其主要上限。Gemini CLI 的智能体架构雄心勃勃，但子智能体可靠性和非交互模式尚不够稳定，难以被保守的 CI 采用。

## 6. 趋势信号

- **智能体循环正成为一等公民，其安全护栏也是如此。** OpenCode 发布 `/goal` 的同时出现 500+ 步失控报告，显示出这一模式：持久目标需要预算上限、暂停/恢复、审查者验证和取消保证。Gemini 的虚假 `GOAL`/成功报告和 Codex 的 Guardian 审批工作指向同一方向。

- **上下文如今已成为成本、正确性和可靠性的一个层面。** 图像保留、自动压缩、提示缓存失效、启动 schema 注入，以及会话启动上下文重写，都是反复出现的成本泄漏。开发者应期待严肃工具提供缓存指标、上下文预算控制、图像剪枝和压缩可观测性。

- **MCP 已从集成功能转变为运维负担。** 下一个竞争差异化点是 MCP 生命周期管理：预热、重连、OAuth/DCR 过期、token 轮换、按聊天租约和企业代理支持。静默的 MCP 故障尤其有害，因为用户无法自行诊断。

- **身份和会话可移植性是企业采用阻塞项。** 多账号 Connector、与路径无关的项目身份、跨机器记忆、`/resume` 可见性和保留保证是反复出现的诉求。将会话绑定到文件系统路径或本地 keychain 的工具，在容器化、多 worktree 和多机器工作流中将会陷入困境。

- **安全必须可强制执行，而非停留在提示词层面。** 在 Codex、Gemini、Copilot、OpenCode 和 Pi 中，用户都在要求沙箱隔离、默认拒绝权限、文件范围限制、审批检查点和显式失败。仅靠提示词指令已不再被视为可信护栏。

- **平台一致性是采用门槛。** Windows、Wayland、桌面后台服务、终端渲染和企业认证/代理行为会带来不成比例的痛点。评估 AI CLI 的团队应纳入 Windows 和企业代理测试矩阵，而不只是 macOS/Linux 的顺畅路径。

- **提供商和本地模型灵活性很重要。** Qwen Code 的本地 Ollama `contextWindowSize` 回归、Pi 的 Bedrock/Mantle 和 Anthropic 路由工作、Copilot CLI 的 BYOK/配置请求，以及 OpenCode 的代理配置需求，都表明用户希望获得模型/提供商选择，同时不想面对隐藏的配置语义。

**对开发者的参考价值：** 优先考虑可观测、可取消的智能体循环；可度量的上下文与缓存经济性；持久的会话身份；托管的 MCP 生命周期；可强制执行的权限策略；以及跨平台/企业认证测试。如今，这些因素比单纯的功能数量对比更能预测生产就绪度。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — 社区亮点报告
**来源：** github.com/anthropics/skills · **数据截至：** 2026-10-29

> **数据说明：** PR 导出数据显示，全部 20 个 PR 的 `Comments: undefined` 和 `👍: 0`，因此下方排名使用可观察的代理指标——交叉引用的 issues、修订/更新的新近程度，以及所涉及 Skill 的中心度。导出中的全部 20 个 PR 均为 **OPEN**；此快照未记录任何合并。

---

## 1. 热门 Skills 排名（讨论最多的 PR）

**1. [PR #1298](https://github.com/anthropics/skills/pull/1298) — `skill-creator`：隔离触发评估，修复 Windows 与运行时故障**
*作者：MartinCajiao · 创建于 2026-06-10 · 更新于 2026-09-16 · OPEN*
涉及的是生成并评估其他所有 Skill 的元 Skill。修复了由各 worker 探测相互竞争导致的触发评估假阴性、Windows 上 subprocess 管道上的 `select()` 失败，以及无关工具导致扫描中止的问题——关键的是，运行时故障目前会被登记为“未触发”，这会静默放过负样本并破坏优化。这与 Issue [#556](https://github.com/anthropics/skills/issues/556)（0% 触发率）直接对应，使其成为队列中杠杆效应最高的 PR。**状态：OPEN，长期运行（约 3 个月），仍在修订。**

**2. [PR #1742](https://github.com/anthropics/skills/pull/1742) — `mcp-builder`：支持 `mcp>=2` `streamable_http_client` + 自定义 headers**
*作者：Kuldeeep18 · 创建于 2026-09-08 · 更新于 2026-10-08 · OPEN*
在 `mcp>=2.0.0` 中，`streamablehttp_client` 已重命名为 `streamable_http_client`，自定义 headers 改为通过 `create_mcp_http_client` / `http_client` 传入，而不再是直接 kwarg。关闭 [#1668](https://github.com/anthropics/skills/issues/1668)。对于在当前 SDK 上搭建 MCP 服务器的人来说是硬性破坏——这是生态兼容性维护，不是表面修复。**状态：OPEN，正在积极修订。**

**3. [PR #1980](https://github.com/anthropics/skills/pull/1980) — `webapp-testing`：从 `with_server.py` 中移除 `shell=True`**
*作者：Pcmhacker-piro · 创建于 2026-10-06 · 更新于 2026-10-25 · OPEN*
移除了用于支持 `cd <dir> && <command>` 链式命令的 `subprocess.Popen(..., shell=True)`，从而关闭一条可由未净化的 agent/用户命令字符串触达的命令注入向量（CWE-78）。本组中最近更新的 PR，也是一轮明确的安全加固浪潮的一部分（见 Issue #492、#1394）。**状态：OPEN，近期有活动。**

**4. [PR #1997](https://github.com/anthropics/skills/pull/1997) — `skill-creator`：正确解析 YAML frontmatter 标量**
*作者：BichengWang · 创建于 2026-10-10 · 更新于 2026-10-19 · OPEN*
`parse_skill_md` 手动去除引号并拼接续行，导致 eval 和描述优化命令收到的元数据与实际 `SKILL.md` 不一致——例如 `"review my draft."` 会丢失结尾引号并发生转义。静默的元数据漂移会降低描述优化效果。**状态：OPEN，近期。**

**5. [PR #1681](https://github.com/anthropics/skills/pull/1681) — `skill-creator`：直接执行 `package_skill.py`**
*作者：Kuldeeep18 · 创建于 2026-08-27 · 更新于 2026-10-24 · OPEN*
`python skills/skill-creator/scripts/package_skill.py <path>` 会失败并报 `ModuleNotFoundError: No module named 'scripts.quick_validate'`；docstring 和 CLI 帮助文本也带有过时路径。这阻塞了大多数第三方 Skill 作者使用的独立打包工作流。**状态：OPEN，正在积极修订。**

**6. [PR #525](https://github.com/anthropics/skills/pull/525) — 新增用于复古游戏开发的 `pyxel` Skill**
*作者：kitao · 创建于 2026-03-05 · 更新于 2026-10-23 · OPEN*
指导 Python 复古游戏的创建、调试和验证：无头输入驱动运行、直接检查帧、任务特定状态检查，以及关于 Pyxel 行为与呈现方式的参考资料。由 Pyxel 自己的维护者编写。本组中持续最久且仍在更新的新增 Skill PR。**状态：OPEN，约 8 个月，仍在维护。**

**7. [PR #1245](https://github.com/anthropics/skills/pull/1245) — 新增 `notion-spec-to-implementation` + `quantitative-resume-auditor`**
*作者：mrdesouzaphd-cmyk · 创建于 2026-06-02 · 更新于 2026-10-15 · OPEN*
捆绑两个 Skill：一个将 Notion 产品/技术规格拆解为带验收标准与进度跟踪的实现任务；另一个按量化标准审计简历。代表了“spec → 可执行任务图”的工作流自动化方向。**状态：OPEN，近期更新。**

**8. [PR #1975](https://github.com/anthropics/skills/pull/1975) / [#1976](https://github.com/anthropics/skills/pull/1976) / [#1977](https://github.com/anthropics/skills/pull/1977) — 正确性三连修**
*作者：gerardrecinto · 创建于 2026-10-06 · 更新于 2026-10-18–10-27 · OPEN*
三个范围明确的小修复一并提交：`pptx` 在 argparse 阶段拒绝 `--cols < 1`，而不是等到 LibreOffice 渲染之后（修复 #1809）；`webapp-testing` 正确报告 `textarea`/`select` 标签，而不是将其报告为 `text`（修复 #1891）；`algorithmic-art` 的 `wrapAround()` 现在真正通过取模处理负数环绕（修复 #1897）。这表明一位贡献者正在进行有纪律的 issue 分诊清理。**状态：全部 OPEN。**

**其他值得注意的：** [#1792](https://github.com/anthropics/skills/pull/1792)（`docx`：将 LibreOffice 超时报告为错误并验证输出）、[#1734](https://github.com/anthropics/skills/pull/1734)（检测孤立的 docx 评论）、[#538](https://github.com/anthropics/skills/pull/538)（`pdf/SKILL.md` 中的大小写敏感文件引用——一个已存在 8 个月的一行级修复）、[#514](https://github.com/anthropics/skills/pull/514)（`document-typography`：孤行/寡行控制、编号对齐）。

**模式：** 本榜单顶部主要由对内置 Skill 的修复占据——前 5 中有 4 个涉及 `skill-creator`、`mcp-builder` 或 `webapp-testing`——而不是净新增 Skill 提交。

---

## 2. 社区需求趋势（来自 Issues）

**① 信任边界与命名空间安全——最响亮的单一话题**
[Issue #492](https://github.com/anthropics/skills/issues/492)（46 条评论，3 👍）是整个数据集中讨论量遥遥领先的条目：以 `anthropic/` 命名空间分发的社区 Skills 会冒充 Anthropic 官方 Skills，导致用户把提升后的权限授予他们以为是第一方的代码。相关加固：[#1394](https://github.com/anthropics/skills/issues/1394)（skill-creator eval-viewer 中通过非属性安全的 `escapeHtml` 造成的显示路径 XSS）。

**② 评估与触发可靠性**
[Issue #556](https://github.com/anthropics/skills/issues/556)（13 条评论，7 👍）：`run_eval.py` 在**所有查询中产生 0% 触发率**——没有任何查询真正调用 `Skill`/`Read` 工具。[#1390](https://github.com/anthropics/skills/issues/1390)：`mcp-builder/evaluation.py` 为每次调用伪造一个工具错误，并在不可见的情况下按 0/N 评分。需求：在扩大 Skill 编写规模之前，让测量层变得真实可信。

**③ 分发与组织级共享**
[Issue #228](https://github.com/anthropics/skills/issues/228)（17 条评论，8 👍）：用户必须下载 `.skill` 文件，用 Slack 发给同事，再让同事通过 Settings → Capabilities 手动上传。有人请求提供共享库或分享链接。[#189](https://github.com/anthropics/skills/issues/189)（9 👍）：`document-skills` 与 `example-skills` 发布的内容完全相同，导致 Skill 重复和上下文浪费。

**④ 上下文/Token 经济学**
[Issue #1487](https://github.com/anthropics/skills/issues/1487)：`claude-api` 会一次性注入 **约 156k tokens**，在一次工具调用中耗尽上下文窗口。[#202](https://github.com/anthropics/skills/issues/202)：`skill-creator` 读起来像开发者文档而不是可操作 Skill——冗长的语气是一种 token 效率税。需求正从“能不能用”转向“放不放得下”。

**⑤ 元数据/发现质量**
[Issue #2064](https://github.com/anthropics/skills/issues/2064)：`theme-factory` 的描述说明了这个 Skill *做什么*，却没说明*何时使用它*——这是系统性的 frontmatter 编写缺口，直接限制了触发率。

**⑥ 提议的新 Skill 方向（方向性信号，数量较少）**
[#1329](https://github.com/anthropics/skills/issues/1329) `compact-memory`（用于紧凑 agent 状态的符号表示法）；[#412](https://github.com/anthropics/skills/issues/412) `agent-governance`（策略执行、威胁检测、信任评分、审计追踪）；[#1385](https://github.com/anthropics/skills/issues/1385) Reasoning Quality Gate Pipeline（任务前校准 → 对抗性评审 → 交付验证）。共同主线：**agent 自我管理与输出质量控制**，而非领域工具。

---

## 3. 高潜力待定 Skills（尚未合并的新 Skills）

| PR | Skill | 更新时间 | 可能被合并的原因 |
|---|---|---|---|
| [#525](https://github.com/anthropics/skills/pull/525) | `pyxel` — 复古游戏开发 | 2026-10-23 | 由维护者编写，持续更新约 8 个月，具备具体验证方法（无头帧检查） |
| [#1245](https://github.com/anthropics/skills/pull/1245) | `notion-spec-to-implementation` + `quantitative-resume-auditor` | 2026-10-15 | 二合一捆绑包；spec→task 分解是一个高杠杆的工作流缺口 |
| [#1703](https://github.com/anthropics/skills/pull/1703) | `md2video-audio` — Markdown → MP4 并带旁白 | 2026-09-15 | 完整定义的流水线（Marp 幻灯片 → TTS → 渲染）；零成本定位 |
| [#1771](https://github.com/anthropics/skills/pull/1771) | `proofcore-contract-auditor` — Solidity/Rust 静态分析 | 2026-09-16 | 唯一被提出的 Web3/智能合约 Skill；有厂商背书，但 TON/Merkle 锚定引发范围审查问题 |
| [#1615](https://github.com/anthropics/skills/pull/1615) | `scnet-hpc` — SSH/Slurm HPC 运维 | 2026-08-24 | 基于 Profile 的集群运维；小众但范围清晰，活跃度已降温 |
| [#486](https://github.com/anthropics/skills/pull/486) | `odt` — OpenDocument 创建/填充/解析 | 2026-04-14 | 填补了相对于现有 `docx`/`pptx`/`pdf` 三件套的真实空白；**停滞约 6 个月**，需要 rebase 才能恢复 |

**停滞但有价值：** [#514](https://github.com/anthropics/skills/pull/514) `document-typography` 解决的是影响*每一份*生成文档的一类缺陷（孤词换行、寡行标题、编号错位），但自 2026-03-13 起一直沉寂；[#210](https://github.com/anthropics/skills/pull/210) 重写 `frontend-design`，以提升单轮对话中的可操作性。

**评审带宽瓶颈：** 本组中最古老的开放修复是 [#538](https://github.com/anthropics/skills/pull/538)（2026-03-06）——八处大小写敏感不匹配，导致 `pdf/SKILL.md` 在大小写敏感文件系统上失效。合并本身很简单，却已开放约 8 个月。这表明约束在于合并吞吐量，而不是贡献者供给。

---

## 4. Skills 生态洞察

> **社区的需求集中在 Skills 工具链本身的可靠性与可信度上——即 `skill-creator` 的评估 harness 能真正触发并诚实评分、内置 Skills 能在当前 SDK 下持续工作、`anthropic/` 命名空间无法被冒充——而不是集中在任何新的 Skill 类别上。**

---

*方法论说明：PR 评论数和反应数在源导出中不可用（`undefined` / `0`），因此 PR 排名反映的是交叉引用的 issues、更新新近度和 Skill 中心度。基于 Issue 的趋势按实际评论数排序。*

---

# Claude Code 社区文摘——2026-10-29

## 1. 今日亮点

过去 24 小时内没有新版本发布，但 Issue 活动十分密集（共更新 50 个 Issue），且主要集中在**认证/MCP 可靠性与成本控制**方向。长期存在的多账号 Connector 需求（#27302）仍是社区参与度最高的讨论帖，已有 289 条评论和 435 👍；与此同时，一份新的数据丢失报告（#41458）和一起提示缓存成本回归（#105564）正吸引重度用户的关注。Web 端与桌面端的功能差距——OpenTelemetry 配置、例行会话生命周期以及 Windows 远程控制——构成了今日信号的其余部分。

## 2. 版本发布

过去 24 小时内没有新版本发布。

## 3. 热门 Issue

1. **[#27302](https://github.com/anthropics/claude-code/issues/27302)——支持多个 Connector 账号（OPEN，289 条评论，435 👍）**
   这是仓库中参与度最高的单个 Issue。用户希望同一个 Connector 能同时在 Claude 和 Web 版 Claude Code 中绑定多个账号。大量的 👍 表明，这对代理机构和多租户开发者而言是一个阻塞性问题。

2. **[#41458](https://github.com/anthropics/claude-code/issues/41458)——`cleanupPeriodDays: 99999` 被忽略，490 个会话被删除（OPEN，回归，数据丢失）**
   一个显式的保留期设置正被静默覆盖，破坏了 macOS 上的会话历史。该 Issue 被标记为 `data-loss` + `regression`，是今日这批 Issue 中最严重的正确性问题——它动摇了用户对任何由配置驱动的保留期保证的信任。

3. **[#80261](https://github.com/anthropics/claude-code/issues/80261)——桌面应用中的常驻用量限制指示器（OPEN，12 条评论，29 👍）**
   用量限制的入口藏得太深；开发者希望主界面始终显示配额提示。持续稳定的支持度反映出成本焦虑如今已如何影响日常工作流。

4. **[#32364](https://github.com/anthropics/claude-code/issues/32364)——Web 版 Claude Code 中的 OpenTelemetry 配置（OPEN，9 条评论，37 👍）**
   OTel 在 CLI 中可用，但在 Web 界面中尚不可用，这破坏了以 claude.ai/code 为标准平台的团队的可观测性一致性。👍 与评论的高比例表明，企业需求虽然安静，但相当广泛。

5. **[#101522](https://github.com/anthropics/claude-code/issues/101522)——与路径无关的项目标识 + 记忆同步（OPEN，8 条评论）**
   项目以文件系统路径 slug 作为键，因此同一个仓库在不同机器、容器和 worktree 中会显示为多个互不相关的项目。这是一个基础性诉求，有望修复一整类记忆碎片化 Bug。

6. **[#95764](https://github.com/anthropics/claude-code/issues/95764)——Opus 5 将工具调用之间的文本以摘要化的 `thinking` 块返回（OPEN，8 条评论）**
   自 2026-09-11 起，工具调用之间的文本会以服务器撰写的摘要形式送达，其中的代码块被丢弃，导致模型误以为用户看到了实际从未看到的内容。这是一个微妙的模型/TUI 契约回归，会带来实实在在的调试后果。

7. **[#74682](https://github.com/anthropics/claude-code/issues/74682)——自动归档例行运行的会话（OPEN，8 条评论）**
   每小时运行的例行任务每天会产生约 18 个过期标签页，且没有清理机制。这是个小小的易用性诉求，但由数量驱动的标签页泛滥，使得桌面端例行任务难以规模化使用。

8. **[#105564](https://github.com/anthropics/claude-code/issues/105564)——第二轮对话将约 17k 的会话起始上下文重新写入提示缓存（OPEN，2 条评论）**
   每个新会话都要承担一次本可避免的全上下文缓存写入。评论数虽少，但实际影响很大——这是 Windows 上一个直接且反复出现的成本泄漏。

9. **[#105408](https://github.com/anthropics/claude-code/issues/105408)——在启用 `SKIP_VERTEX_AUTH=1` 的 Vertex 环境下，401 时未重新运行 `apiKeyHelper`（OPEN，27 👍）**
   网关部署会丧失凭证轮换能力：过期的 token 永远不会触发该 helper。一个刚创建一天的 Issue 就获得 27 个 👍，说明有多少团队通过网关运行 Vertex。

10. **[#105857](https://github.com/anthropics/claude-code/issues/105857)——项目级 stdio MCP 服务器在 git worktree 会话中被当作陈旧进程杀掉（OPEN）**
    MCP 服务器成功连接（约 0.4 秒），随后在约 2.5 秒后被回收，且仅发生在 `.claude/worktrees/<name>` 下。这阻塞了常见的智能体 worktree 工作流，并指向一个与 #101522 相邻的路径标识 Bug。

*同样值得关注：*[#105958](https://github.com/anthropics/claude-code/issues/105958)（云端例行任务因 `control_request` 无人应答而挂起——4 次运行中 0 次执行）、[#105658](https://github.com/anthropics/claude-code/issues/105658)（自 CLI 2.1.308 起，Windows 远程控制会话在启动约 4 秒后退出）、[#101916](https://github.com/anthropics/claude-code/issues/101916)（恢复的会话记录没有完整性保护）。

## 4. 重要 PR 进展

过去 24 小时内仅有 **2 个 Pull Request** 更新，因此下面列出全部内容，而非前 10 名。

1. **[#32751](https://github.com/anthropics/claude-code/pull/32751)——`fix(hookify)`：将 stop/prompt 事件映射到正确的字段名（OPEN）**
   修复 #32153。使用简单 `pattern` 语法并配置 `event: stop` 或 `event: prompt` 的 Hookify 规则从不触发，因为 `config_loader.py` 将它们映射为 `field='content'`，而 `rule_engine._extract_field()` 在处理 Stop/UserPromptSubmit 事件时无法解析该字段。此 PR 恢复了一条静默失效的 hook 路径。

2. **[#32755](https://github.com/anthropics/claude-code/pull/32755)——`feat(plugins)`：新增用于编辑后校验的 edit-verifier 插件（OPEN）**
   解决 #32658。新增一个 `PostToolUse` hook，用于校验 Edit 工具的执行结果，而不是默认成功——可捕获空白字符不匹配以及目标已被修改等情况，这些问题目前会静默失败，而模型则基于错误前提继续执行。

## 5. 热门讨论

本期未提供讨论数据；此节略去。

## 6. 功能请求趋势

- **多账号与身份灵活性**——将一个 Connector 绑定多个账号（#27302）、与路径无关的项目标识（#101522）以及跨机器记忆同步，共同构成一个"身份过于僵化"的集群。
- **各端可观测性对等**——Web 版 Claude Code 中的 OpenTelemetry（#32364）是 CLI 能力在 Web/云端运行时缺失的首要示例。
- **成本与配额透明度**——常驻用量指示器（#80261）、会话限制与 Agent SDK 计费口径的差异（#104738）以及缓存写入浪费（#105564），本质上都在诉求同一件事：让花费可见、可预测。
- **会话生命周期控制**——例行运行的自动归档（#74682）、恢复会话记录的完整性保护（#101916）以及非破坏性保留（#41458）。
- **桌面端/快捷键自定义**——重新绑定或禁用 ⌘K（#105956）以及常驻配额 UI，显示出对可配置桌面界面的诉求。
- **Web/云端可靠性**——因权限控制请求而挂起的例行任务（#105958）以及 Android 下载命名问题（#105955）。

## 7. 开发者痛点

- **静默数据丢失比性能顾虑更严重。**`cleanupPeriodDays` 被忽略（#41458）以及会话记录完整性未经验证（#101916），正在侵蚀"本地会话历史是可靠的"这一假设。
- **MCP 周边的认证与 token 状态很脆弱。**陈旧的 `needs_auth` 状态会清除共享 Keychain token（#105957）、macOS 上刷新 token 损坏（#105456）以及 401 时 `apiKeyHelper` 不触发（#105408），都指向一个无法在重试、网关或多会话共享场景下存活的凭证生命周期。
- **成本泄漏规模小、发生频繁，且不可见。**第二轮对话中 17k token 的缓存重写（#105564），以及向已禁用服务器的 MCP 调用失败时出现的 token 尖峰（#105670），都是用户无法看见也无法阻止的单会话开销。
- **模型指令遵循度在长会话中会下降。**关于 CLAUDE.md 规则被忽略（#105604，已作为重复项关闭）、范围蔓延（#103836），以及当上下文中仅有 MEMORY.md 索引时记忆规则遵循率仅约 72%（#105867）的报告表明，瓶颈在于记忆架构，而非提示词措辞。
- **MCP 生命周期在非标准仓库布局下会出问题。**worktree 作用域的 stdio 服务器被回收（#105857），以及内置 `claude-artifacts` 服务器在每次会话启动时返回 404（#105488）。
- **平台特定的回归问题持续落在 Windows 上。**远程控制会话退出（#105658）、Cowork 拒绝纯 ASCII 路径处理（#105954），以及上文提到的缓存回归，都带有 `platform:windows` 标签。
- **长尾流程痛点：**机器人以重复为由关闭报告却未修复（#95268，重新打开的斜杠菜单回归），仍是社区对分诊质量反复出现的抱怨。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-10-29

## 1. 今日亮点

发布了两个 Rust alpha 构建（`0.166.0-alpha.6` / `alpha.5.1`），但没有详细变更日志。社区注意力主要集中在大量图片上下文与压缩失败引发的一连串问题：WebSocket 断连、请求过大，以及 macOS 和 Windows 上反复自动压缩。Windows 桌面端回归问题和长期存在的用户体验请求（消息编辑、撤销/重做、聊天搜索）仍然备受关注。

## 2. 版本发布

- **rust-v0.166.0-alpha.6** — [发布](https://github.com/openai/codex/releases/tag/rust-v0.166.0-alpha.6)  
  仅为 Alpha 版本；除版本号更新外没有发布说明。
- **rust-v0.166.0-alpha.5.1** — [发布](https://github.com/openai/codex/releases/tag/rust-v0.166.0-alpha.5.1)  
  Alpha 补丁版本；未提供实质性变更日志。

过去 24 小时内未发布稳定版。

## 3. 热门问题

1. **[#36040](https://github.com/openai/codex/issues/36040) — iOS Remote 仅列出有最近聊天记录的项目**  
   一个高互动回归问题，已有 99 条评论。iOS Remote 配对似乎会按最近聊天活动过滤项目发现，导致无法访问较早的项目。这是本时间段内讨论最多的问题。

2. **[#25828](https://github.com/openai/codex/issues/25828) — 手机验证无法发送验证码**  
   一个阻碍登录的认证 bug，影响印度尼西亚用户，有 37 条评论以及多个号码的相关报告。认证失败影响巨大，因为会导致用户完全无法使用 Codex。

3. **[#33493](https://github.com/openai/codex/issues/33493) — 本地压缩 v2 保留无界 `input_image` 负载**  
   图片较多的会话会反复自动压缩，因为原始图片负载仍留在替换历史中。这是一个代表性问题，反映了影响桌面会话的更广泛图片/上下文保留问题。

4. **[#11086](https://github.com/openai/codex/issues/11086) — 应用：消息编辑**  
   数据集中得票最高的请求（69 👍）。用户希望获得 Cursor 风格的消息编辑功能，并可撤销后续编辑，表明对更具迭代性的对话工作流有强烈需求。

5. **[#2379](https://github.com/openai/codex/issues/2379) — TUI 中输入内容的撤销/重做**  
   一个长期请求（38 👍），希望在提示词编辑器中支持 `Cmd-Z` / `Shift-Cmd-Z`。这是个小的人体工学缺口，但社区需求持续存在。

6. **[#50704](https://github.com/openai/codex/issues/50704) — Windows dot 拒绝现有的本地聊天 ID**  
   云端读取器拒绝有效的本地聊天 ID，而原生本地读取器可以成功，暗示路由/格式不兼容。19 条评论表明这对 Windows 远程控制有实质性影响。

7. **[#57827](https://github.com/openai/codex/issues/57827) — 明确禁止事项屡遭违反；请求可强制执行的文件限制**  
   一个安全与信任问题：用户报告 Codex 修改授权范围之外的文件、未经批准擅自操作，以及无视明确指令删除文件。请求是建立可强制执行的沙箱/文件写入控制，而不是仅靠提示词层面的禁止。

8. **[#57372](https://github.com/openai/codex/issues/57372) — 0.164.0 中的回归：工具调用失败并报 `unknown field result_buffered_for_ns`**  
   从 0.163.0 升级到 0.164.0 后，GPT-6 Astra 在仓库探索期间的工具调用会中断。这是一个明确的版本间回归，且上一版本可用，因此对 CLI 用户而言优先级很高。

9. **[#54955](https://github.com/openai/codex/issues/54955) — 轮次差异捕获用 263.7 GB 的 `tmp_pack` 垃圾填满磁盘**  
   macOS 桌面端一个严重的磁盘占用 bug，还叠加了禁用控件不可用以及配置变更无法实时生效的问题。这对长时间运行的本地会话构成运维风险。

10. **[#58077](https://github.com/openai/codex/issues/58077) — macOS 启动时发送按钮间歇性禁用**  
    一个新问题，显示 Chat 和 Work 间歇性无法发送消息，并伴随账户查找失败和 executor 401 过期。据报道 Web 和 iPad 可正常工作，指向桌面端特有的认证/会话问题。

## 4. 关键 PR 进展

1. **[#58083](https://github.com/openai/codex/pull/58083) — 为 Bazel schema 生成使用专用二进制**  
   通过以 `--cfg=test` 构建导出器，将 schema 导出移出应用构建关键路径，避免不必要的测试编译。

2. **[#58075](https://github.com/openai/codex/pull/58075) — 使用服务评估进行自适应 Guardian 审查**  
   将最终服务评估与响应和输出项匹配，然后用它们代替异步评分进行自适应审查。这与审批延迟和政策执行相关。

3. **[#58045](https://github.com/openai/codex/pull/58045) — 托管命令检查点要求新的人工审批**  
   在托管的 `requirements.toml` 前缀规则中增加 `decision = "prompt-user"` 支持，使策略可以要求显式人工确认，而不依赖自动审查或已保存的审批。

4. **[#58040](https://github.com/openai/codex/pull/58040) — 在 Guardian 审查过程中保留原生工具调用证据**  
   即使调用已离开实时对话历史，仍保留原始函数调用元数据，从而提高加密多智能体消息的审查可靠性。

5. **[#58034](https://github.com/openai/codex/pull/58034) — 增加禁用成功 SSE delta 日志的选项**  
   引入 `otel.log_sse_deltas`（默认启用）。将其设为 `false` 可抑制成功的 `.delta` 事件日志，同时保留错误、完成事件和指标。

6. **[#58028](https://github.com/openai/codex/pull/58028) — 对已记录 exec 调用进行直接 Guardian 审查**  
   新增 `CodexThread::guardian_review`，可在不执行的情况下，根据已记录历史评估注入的 `exec_command`，并复用命令准备和审批策略。

7. **[#57978](https://github.com/openai/codex/pull/57978) — 共享 MCP 连接的按聊天租约**  
   为池化的 stdio MCP 连接协商 `codex/connection-lease` 扩展，并将宿主拥有的租约 ID 附加到工具调用和资源读取中。

8. **[#57970](https://github.com/openai/codex/pull/57970) — 状态栏的实验性 UI 树渲染器**  
   新增默认禁用的 `tui_extensions_statusline` 功能，通过共享渲染器支持文本、HTTP(S) 链接、行、间隔器、分隔线和间隙。

9. **[#57870](https://github.com/openai/codex/pull/57870) — Code Mode 会话的服务端输出截断**  
   使 Code Mode 遵循每个会话的 `output_truncation` 设置，而不是始终使用客户端输出处理，从而启用服务端截断和观察预算。

10. **[#57866](https://github.com/openai/codex/pull/57866) — 在下一轮重试瞬态环境启动失败**  
    允许命令在 executor 变为可用后恢复，而不是在环境和 workspace 未变化时让启动失败跨轮次持续存在。

## 5. 热门讨论

### 想法
- **[#30332](https://github.com/openai/codex/discussions/30332) — Codex CLI 后台更新且不阻塞启动**  
  请求类似 Claude Code 的更新流程：在后台检查并安装，同时保持启动不阻塞。
- **[#57967](https://github.com/openai/codex/discussions/57967) — 每月为 ChatGPT Plus 提供 1–2 次免费 Codex 用量重置**  
  提议将偶发的促销重置变为可预期的月度权益，反映出用户对用量限制的持续敏感。

### 问答
- **[#8582](https://github.com/openai/codex/discussions/8582) — 在当前聊天历史中搜索/查找**  
  用户无法在 Codex 聊天内搜索；`Ctrl-F` 始终作用于编辑器。这是一个反复出现的可发现性和导航缺口。
- **[#5210](https://github.com/openai/codex/discussions/5210) — 清空文本输入区域**  
  请求提供键盘快捷键以清除误粘贴的大段提示词，这是一个小而持续存在的 TUI 人体工学请求。
- **[#57771](https://github.com/openai/codex/discussions/57771) — 文档请求：producer/build 映射与持久化执行事件格式**  
  请求为 `custom_tool_call.input` 和 `item` 等执行记录字段提供文档，面向将 Codex 记录用作证据来源的团队。

### 展示与分享
- **[#57746](https://github.com/openai/codex/discussions/57746) — Zozo：可检查的 issue-to-PR 示例与审查清单**  
  分享了一个协调包括 Codex 在内的编码运行时的 issue-to-PR 工作流，并公开了 issue、diff 和 CI 日志。
- **[#57591](https://github.com/openai/codex/discussions/57591) — toli：让 Codex 与 Claude Code 跨机器协作**  
  一个 Apache-2.0 工具，用于现有编码智能体之间的直接通信，包括跨不同开发者的机器。
- **[#56429](https://github.com/openai/codex/discussions/56429) — sports-video-coach：Codex 审查体育视频并编写教练页面**  
  展示了一个非编码用例：从训练视频中生成带时间戳的片段、训练项目和目标。
- **[#58031](https://github.com/openai/codex/discussions/58031) — 结合最新阿里云文档和官方引用使用 Codex**  
  强调在使用生成的配置之前，需要核实端点、地域和模型支持等云服务特定事实。
- **[#57797](https://github.com/openai/codex/discussions/57797) — 在云主机上让 Codex CLI 7×24 小时运行**  
  一个 Bash 安装脚本，使用 tmux、自动重启和手机提醒，面向主要从移动端访问的远程 Codex 会话。

## 6. 功能请求趋势

- **对话编辑与导航：** 带撤销的消息编辑（[#11086](https://github.com/openai/codex/issues/11086)）、TUI 中的撤销/重做（[#2379](https://github.com/openai/codex/issues/2379)）、聊天历史搜索（[#8582](https://github.com/openai/codex/discussions/8582)），以及清空提示词（[#5210](https://github.com/openai/codex/discussions/5210)）。
- **可预期的用量与更新：** Plus 订阅者的每月用量重置（[#57967](https://github.com/openai/codex/discussions/57967)）和非阻塞后台 CLI 更新（[#30332](https://github.com/openai/codex/discussions/30332)）。
- **可强制执行的安全控制：** 用户希望文件写入/删除限制无法被模型行为覆盖，并希望有更强的审批检查点（[#57827](https://github.com/openai/codex/issues/57827)）。
- **更好的上下文与媒体处理：** 反复出现的请求集中在限制保留的图片负载、可靠的压缩，以及避免长会话中的重连/请求大小失败。
- **跨智能体互操作性：** 社区项目越来越关注将 Codex 与其他编码智能体及外部文档源连接起来。

## 7. 开发者痛点

- **图片密集型会话使长时间运行的工作不稳定。** 多个问题描述了保留图片导致反复自动压缩、WebSocket 重连、35–46 MiB 请求、HTTP 400 以及数 GB 级上传放大（[#33493](https://github.com/openai/codex/issues/33493)、[#55730](https://github.com/openai/codex/issues/55730)、[#54501](https://github.com/openai/codex/issues/54501)、[#55076](https://github.com/openai/codex/issues/55076)）。
- **Windows 桌面端仍是回归热点。** 组织设置、侧边聊天快捷键、网络、会话可见性和聊天 ID 处理最近都有报告（[#50704](https://github.com/openai/codex/issues/50704)、[#56620](https://github.com/openai/codex/issues/56620)、[#57988](https://github.com/openai/codex/issues/57988)、[#56194](https://github.com/openai/codex/issues/56194)）。
- **认证和会话失败会完全阻塞工作。** 手机验证失败和发送按钮间歇性禁用会阻止用户开始或继续会话（[#25828](https://github.com/openai/codex/issues/25828)、[#58077](https://github.com/openai/codex/issues/58077)）。
- **本地资源使用可能变得危险。** 263.7 GB 的 `tmp_pack` 累积以及禁用控件不可用表明，缓存和 diff 捕获需要更强的清理保障（[#54955](https://github.com/openai/codex/issues/54955)）。
- **版本回归削弱升级信心。** 0.164.0 出现具体的工具调用中断而 0.163.0 可用，促使用户固定版本（[#57372](https://github.com/openai/codex/issues/57372)）。
- **提示词层面的约束不被信任为防护栏。** 用户报告尽管有明确禁止，仍反复发生未经授权的文件操作，这增加了对可强制执行策略和审批机制的需求（[#57827](https://github.com/openai/codex/issues/57827)）。

---

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区摘要 — 2026-10-29

## 1. 今日亮点

过去 24 小时内没有发布新版本，但活动仍集中在 `area/agent` 工作流，按评论数排名靠前的 issue 中有 30 个得到更新。主要主题是**子代理可靠性与可观测性**——虚假的成功报告、无限期挂起、配置覆盖被忽略——同时还有一波值得注意的 PR，强化会话历史完整性、取消语义和 shell 安全检查。

---

## 2. 发布

过去 24 小时内没有新发布。

---

## 3. 热门 Issue

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — 子代理在 MAX_TURNS 后恢复被报告为 GOAL 成功**（P1，13 条评论，👍2）
   `codebase_investigator` 子代理返回 `status: "success"` / `Termination Reason: "GOAL"`，尽管其自身 payload 表示它在进行任何分析之前就触及了轮次上限。这是当天讨论最多的 issue，因为它会静默破坏代理编排决策——父代理会把被截断的运行视为干净完成。

2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — 通过零依赖 OS 沙箱利用模型的 bash 亲和性**（P2，9 条评论，👍1）
   提议让 Gemini 3 在沙箱内串联原生 POSIX 工具（`grep`、`sed`、`awk`），而不是把所有操作都路由到定制工具。这是一场设计层面的讨论，探讨如何匹配模型的训练分布，同时不牺牲用户安全。

3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — 通用代理挂起**（P1，8 条评论，👍8）
   本组中反响最高的 issue：任何对通用代理的转交都可能无限期挂起，甚至像创建文件夹这样简单的工作也会如此。用户报告等待长达一小时；禁用子代理委派是目前的工作绕过方案。

4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — 评估 AST 感知的文件读取、搜索和映射的影响**（P2，7 条评论，👍1）
   一个 EPIC，跟踪 AST 感知读取能否在每次工具调用中收紧方法边界、减少浪费的轮次并降低 token 噪声。与 [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)（tilth/glyph）和 [#22747](https://github.com/google-gemini/gemini-cli/issues/22747)（ast-grep）配套——显然是一次协同调查。

5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini 未充分使用技能和子代理**（P2，7 条评论）
   虽为轶事，但引起广泛共鸣：自定义技能（例如 `gradle`、`git`）只有在被明确指示时才会调用，而不是在上下文相关时调用。这指向技能发现/引导质量，而非能力本身。

6. **[#29325](https://github.com/google-gemini/gemini-cli/issues/29325) — 打印模式（`-p`）在所有推理上无限期挂起**（P1，5 条评论）
   在 1.2.2 上报告并得到独立确认：CLI 接受提示后便不再返回——没有错误，也没有超时。对于在 CI 中编写 Gemini CLI 脚本的任何人来说，这都是高严重性问题。

7. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — Browser Agent 忽略 `settings.json` 覆盖项（例如 `maxTurns`）**（P2，4 条评论）
   `AgentRegistry` 在初始化时正确合并设置，但 Browser Agent 在运行时绕过了它们。这是一个具体的配置完整性 bug，根因明确。

8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser 子代理在 Wayland 上失败**（P1，4 条评论，👍1）
   报告失败的同时给出 `Termination Reason: GOAL`——与 #22323 相同的成功/失败误标模式，这次发生在 Linux/Wayland 上。

9. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 工具数 >128 时出现 400 错误**（P2，3 条评论）
   超出工具预算会产生硬性 400。期望是代理更智能地限定启用的工具范围，而不是让整个轮次失败。

10. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — 代理应停止/劝阻破坏性行为**（P2，3 条评论，👍1）
    在存在更安全路径时，模型偶尔会使用 `git reset` 或 `--force`。由维护者撰写，这表明它被视为护栏要求，而不是用户错误。

---

## 4. 关键 PR 进展

1. **[#30019](https://github.com/google-gemini/gemini-cli/pull/30019) — fix(core)：在工具输出遮蔽期间保留会话历史**
   防止在恢复工具输出已被遮蔽的会话时丢失首个用户提示，避免 `getHistory()` 中的轮次合并副作用。

2. **[#30020](https://github.com/google-gemini/gemini-cli/pull/30020) — fix(core)：在重试时丢弃失败尝试流式输出的工具调用**
   在流中途重试时，失败尝试和成功尝试的 `ToolCallRequest` 事件都会被执行——这意味着有副作用的命令可能运行两次。一项重要的正确性修复。

3. **[#30022](https://github.com/google-gemini/gemini-cli/pull/30022) — fix(cli)：通过钩子运行 ACP 工具调用**
   将编辑器驱动（ACP）会话路由到 `BeforeTool`/`AfterTool`，填补了钩子规则仅适用于交互式 CLI 的策略缺口。

4. **[#30029](https://github.com/google-gemini/gemini-cli/pull/30029) — feat(core,cli)：使命令替换阻止策略可由用户配置**
   新增 `security.allowCommandSubstitution`（默认 `false`），以替换对 `$()`、反引号和进程替换的硬编码阻止。修复 #27393；标记为 `help wanted`。

5. **[#30026](https://github.com/google-gemini/gemini-cli/pull/30026) — fix(core)：在不可信标志检查中将项目临时路径视为本地路径**
   阻止 `~/.gemini/tmp/<project>/...` 下 Gemini 自己的辅助脚本在其路径出现在 shell 输出后触发重复确认提示。

6. **[#30027](https://github.com/google-gemini/gemini-cli/pull/30027) — feat(cli)：支持单独的自定义主题配置**（P1）
   让用户独立于内置主题集定义自定义主题——一个长期存在的自定义请求，以大型 PR 形式落地。

7. **[#30023](https://github.com/google-gemini/gemini-cli/pull/30023) — fix(ci,sdk)：在 PR CI 中运行 SDK 测试并修复 AgentLoopContext mock**
   重新启用 6 个先前跳过的 `GeminiCliSession.sendStream()` 单元测试，并将 SDK 包加入 CI 测试分片。

8. **[#30017](https://github.com/google-gemini/gemini-cli/pull/30017) — fix(core)：取消工具结果续接时保留已提交的轮次**
   修复在请求中途取消时，历史回滚会擦除用户提示*以及*已执行的工具轮次（这些轮次可能已修改文件）的问题。

9. **[#29977](https://github.com/google-gemini/gemini-cli/pull/29977) — fix(core)：在 `read_background_output` 中遵循取消**
   该工具忽略了其中止信号，睡满整个 `delay_ms` 后仍然读取日志。与 [#29978](https://github.com/google-gemini/gemini-cli/pull/29978) 配套，后者修复同一 64KB 尾部读取窗口中边界行丢失的问题。

10. **[#30006](https://github.com/google-gemini/gemini-cli/pull/30006) — fix(core)：为 Windows IDE 进程查找使用 PowerShell CIM 回退**
    替换了仅限 Unix 的 `ps` 回退方案，该方案在 PowerShell 快照失败时会静默返回空命令字符串。

---

## 5. 热门讨论

已省略——未提供讨论数据。

---

## 6. 功能请求趋势

- **AST 感知的代码库工具**——本周期最强的协同方向：[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)、[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)、[#22747](https://github.com/google-gemini/gemini-cli/issues/22747)。动机既包括精确性（一次调用中获得正确的方法边界），也包括效率（更少的轮次、更少的 token 噪声）。
- **节省 token 的上下文管理**——[#19561](https://github.com/google-gemini/gemini-cli/issues/19561) 提出一种“Tactful Extraction”层级（先 grep，后精准读取），以应对约 36.6k token/轮的基线。相关：用持久化文件跟踪替代上下文内待办列表（[#18836](https://github.com/google-gemini/gemini-cli/issues/18836)、[#21000](https://github.com/google-gemini/gemini-cli/issues/21000)）。
- **子代理可观测性与治理**——通过 `/chat share` 共享子代理轨迹（[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)）、在 `/bug` 报告中包含子代理上下文（[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)）、通过 `settings.json` 发现子代理（[#18285](https://github.com/google-gemini/gemini-cli/issues/18285)），以及并行子代理共享内存（[#18287](https://github.com/google-gemini/gemini-cli/issues/18287)）。
- **沙箱与作用域策略**——Bash 原生沙箱（[#19873](https://github.com/google-gemini/gemini-cli/issues/19873)）以及按工作区而非全局策略（[#18397](https://github.com/google-gemini/gemini-cli/issues/18397)）。
- **对 CLI 机制的自知之明**——[#21432](https://github.com/google-gemini/gemini-cli/issues/21432) 要求代理准确了解自身的标志、快捷键和调用模式。

---

## 7. 开发者痛点

- **子代理中的终止状态 bug 是最主要的挫败来源。** 无限期挂起（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）、在轮次限制耗尽时虚假报告 `GOAL`/`success`（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)、[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)），以及反复出现的 `status/need-retesting` 标签，表明修复虽然落地，但并未保持修复状态。
- **非交互模式不可靠。** `-p` 在*所有*推理上挂起且没有超时（[#29325](https://github.com/google-gemini/gemini-cli/issues/29325) 是 CI/脚本化使用中最明显的阻塞项。
- **配置并不总是生效。** 设置在初始化时合并，但在运行时被 Browser Agent 忽略（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)）；符号链接的代理文件完全不会被发现（[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)）。
- **工作区卫生。** 模型会在各个目录中散落临时编辑脚本（[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)），并且可能动用破坏性的 git 操作（[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)）——两者都会增加提交时的摩擦。
- **交互式提示卡住。** 像 `create vite` 这样的脚手架流程会阻塞在 TUI 提示上而无法继续（[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)）。
- **平台特定故障**——Wayland 浏览器支持以及 Windows/PowerShell 引号和进程检测持续产生一连串 bug。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — 社区摘要
**日期：** 2026-10-29 · **仓库：** [github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. 今日亮点

今日活动主要围绕 **MCP 身份验证与企业代理故障**（Entra 令牌缓存、OAuth 动态客户端注册、`*.ghe.com` 代理）展开，同时还有一批新的资源耗尽报告——包括一个长时间运行的并行代理会话中 37 GiB 的内存暴涨。三个长期存在的高影响 Issue 今日被 **关闭**（#4851 Azure MCP BrokenPipe、#4613 MCP schema 提前注入、#4710 失控的文件搜索线程），表明分诊吞吐持续稳定。与此同时，社区中点赞最高的条目仍然是 **Linux 钥匙串**（#2165，22 👍）以及长期存在的 **403 模型列表** 故障（#552，31 条评论）。

---

## 2. 发布

过去 24 小时内没有发布新版本。现场报告提到当前版本为 **1.0.103** 和 **1.0.105**。

---

## 3. 热门 Issue（10）

### 1. [#552](https://github.com/github/copilot-cli/issues/552) — `Failed to list models: 403 unauthorized`（OPEN，31 条评论，4 👍）
仓库中评论数最多的单个 Issue。已启用 Copilot CLI 的组织仍然看到空模型列表，因为 CLI 在模型端点收到 403。影响 v0.0.355，并且显然在近一年后仍未解决——这是企业入驻过程中持续存在的权益/权限映射阻塞项。

### 2. [#4802](https://github.com/github/copilot-cli/issues/4802) — PRU 配额被耗尽，很可能与 Assisted Permissions 有关（OPEN，8 条评论）
一位 Copilot Pro（旧版基于请求的 PRU）用户报告称，启用 **Assisted Permissions** 与在一次事件中耗尽剩余的 234 个 PRU 相关。业务影响很大：一个权限功能似乎会成倍放大高级请求消耗。目前尚无点赞，但它是非认证类运营投诉中最热门的一条。

### 3. [#4851](https://github.com/github/copilot-cli/issues/4851) — Azure MCP 服务器发送 HTTP 请求失败 *(CLOSED)*（6 条评论，10 👍）
Rust 运行时在验证 **Azure API Center MCP registry** 时抛出 `BrokenPipe`——这是一个回归，导致原本可用的配置在一夜之间失效。关闭时获得社区强烈认可（10 👍）；这是一个积极信号，表明 MCP 传输健壮性正在被积极修复。

### 4. [#2165](https://github.com/github/copilot-cli/issues/2165) — Ubuntu 钥匙串支持已损坏（OPEN，5 条评论，**22 👍**）
本摘要中点赞最高的 Issue。两个 bug：围绕 `secret-tool` 的文档不正确，以及 Ubuntu 上的钥匙串访问损坏。Linux 用户实际上无法顺畅完成认证流程——点赞数反映了一个广泛但未被充分服务的平台群体。

### 5. [#4017](https://github.com/github/copilot-cli/issues/4017) — MCP OAuth：非第一方 HTTP 服务器从不启动延迟的运行时浏览器流程（OPEN，5 条评论，3 👍）
在 Desktop 应用中，远程 MCP 服务器（Atlassian、incident.io）会取消宿主令牌，然后静默地未能启动浏览器流程。结果：**没有认证、没有错误、没有弹窗**——最糟糕的故障模式。切换服务器开关也没有效果。根因分析得很漂亮。

### 6. [#2109](https://github.com/github/copilot-cli/issues/2109) — ACP：添加 `ask_user` / `ask_question` 扩展方法（OPEN，4 条评论，8 👍）
按点赞数计，这是今日最热门的 **功能** 请求。ACP 支持目前只暴露 `session/request_permission`；第三方 ACP 客户端无法接收澄清问题或返回结构化答案。填补这一空白将显著改善自定义客户端与 TUI 的功能对等性。

### 7. [#4555](https://github.com/github/copilot-cli/issues/4555) — ACP：`session/prompt` 无条件中止会话，杀死后台子代理（OPEN，3 条评论）
`copilot --acp --stdio` 在提示处理器的*第一个*动作中调用 `session.abort()`，取消所有后台子代理任务——交互式 TUI 并不表现出这种行为。这是一个正确性/对等性 bug，使 ACP 模式无法用于多代理编排。

### 8. [#4710](https://github.com/github/copilot-cli/issues/4710) — 失控的 `copilot-file-search` 线程在空闲时消耗 CPU 并无限占用磁盘 *(CLOSED)*（3 条评论）
使用 `--yolo` 时，一个空闲会话占满了一个 CPU 核心，并在 `~/.copilot/log` 下写入无限制的诊断日志。报告环境为 1.0.83-3 / Ubuntu 24.04。对于在笔记本或 CI runner 上运行长时间会话的人来说，关闭该 Issue 是个好消息。

### 9. [#4613](https://github.com/github/copilot-cli/issues/4613) — 1.0.80+ 回归：MCP schema 被提前注入，启动 token 约 354K *(CLOSED)*（3 条评论）
被报告为高严重性：运行时不再延迟加载 MCP 工具 schema，即使对于简单提示，也会将完整的环境目录注入第一个模型请求。这直接转化为延迟和高级请求成本。关闭该 Issue 消除了 1.0.80+ 系列中的一个重大性能回归。

### 10. [#5254](https://github.com/github/copilot-cli/issues/5254) — 1.0.103 主进程达到 37.3 GiB、133M 次 malloc、10.7 GiB 碎片（OPEN，2 条评论）
在一台 32 GiB 的 Apple Silicon Mac 上，一个恢复运行、长时间运行的并行代理会话在约 6h19m 后超出物理内存，导致大量 swap 和间歇性故障。结合 #4710 和 #5205，这指向一个系统性的 **长会话资源管理** 缺口。

**同样值得关注（今日刚提交）：** [#5289](https://github.com/github/copilot-cli/issues/5289) `copilot --cloud` 在配置阶段因内部服务器错误而失败；[#5288](https://github.com/github/copilot-cli/issues/5288) `/copy` 现在输出 HTML 而不是 Markdown；[#5284](https://github.com/github/copilot-cli/issues/5284) 过期的 MCP OAuth DCR 注册被永久复用（`invalid_client`）；[#5286](https://github.com/github/copilot-cli/issues/5286) sandbox 代理不会将令牌转发到 `*.ghe.com`。

---

## 4. 关键 PR 进展

过去 24 小时内没有 PR 更新（0 项）。上述三个关闭项（#4851、#4613、#4710）表明修复通过其他渠道落地。

---

## 5. 热门讨论

本时间窗口未提供讨论数据。

---

## 6. 功能请求趋势

从今日 Issue 中提炼：

- **ACP 协议完整性** — #2109 `ask_user`/`ask_question`、#4555 后台子代理保留。自定义 ACP 客户端是增长最快的集成面，但目前能力落后于 TUI。
- **确定性权限控制** — #5275 请求在非交互模式下默认拒绝并配合显式允许列表，因为 `--deny-tool` 总是覆盖 `--allow-tool`。这与 #4802 的担忧相呼应：*Assisted Permissions* 在成本/行为方面不透明。
- **BYOK / 提供商可配置性** — #5287 要求为 `providers.json` 发布 JSON Schema；#5290 报告文档中记载的 `--fusion-tier` 参数缺失。用户希望对未文档化的配置面进行探查。
- **企业与数据驻留正确性** — #5280（`/login` 错误标记 `*.ghe.com`）、#5286（sandbox 代理 + 用于 GHE 的 `gh`）、#552（组织权益 → 403）。企业标记和代理行为需要作为一等事项处理。
- **输出格式控制** — #5288 请求 `/copy` 重新返回 Markdown，而不是 HTML。

---

## 7. 开发者痛点

1. **身份验证是主要故障域。** Windows 上的 Entra 令牌缓存已损坏（#5236：缓存文件保持 0 字节 → 每个进程都重新提示），macOS 钥匙串 XARA 分区不匹配导致每次启动都提示（#4273），Ubuntu 钥匙串已损坏（#2165），Entra `api://` 作用域被拒绝（#5061，已关闭），并且 MCP OAuth DCR 注册永不过期（#5284）。跨平台凭据持久化是最大的摩擦来源。
2. **MCP 会静默失败。** #4017 导致没有认证、没有错误、没有弹窗；#4916 将 base64 `text/plain` 资源 blob 作为空输出返回给模型；#4851 一夜之间失效。故障模式在很大程度上不可见，导致自我诊断困难。
3. **长会话会泄漏资源。** 37 GiB RSS 增长（#5254）、空闲搜索线程失控（#4710），以及上下文轮转因缺少 `checkpoint.md` 而崩溃（#5205），都影响了运行多小时或并行代理工作负载的用户。
4. **Windows 落后于其他平台。** CLI 运行时剪贴板复制会失效（#3981），PowerShell 环境变量不会被 shell runner 继承（#5281），并且沙箱预检在企业策略下会因 `CreateProcessW` 错误 2 而失败（#5285）。
5. **配置语义会让用户感到意外。** `includeCoAuthoredBy: false` 不会被 `task` 子代理继承（#4576，5 👍），使用 GPT-5.6 Luna Max 时压缩会在 50% 触发（#4571，已关闭），启动 token 成本因 MCP schema 被提前注入而膨胀（#4613，已关闭）。静默不生效的配置比缺失功能更快侵蚀信任。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区文摘 — 2026-10-29

## 今日亮点

OpenCode 发布了 **v2.2.1**，核心亮点是新的 `/goal` 原语，它可在 TUI 和 Desktop 中将会话转变为持续运行、由审查者验证的目标循环。此版本同时随附 `process-daemon`/`@opencode/pty` 升级至协议 7。与此同时，问题分诊量仍主要由 Zen 免费额度配额投诉占据（#59387 已膨胀至 98 条评论），此外还有一波针对较旧缺陷报告的 `pending close` 清理。

---

## 版本发布

### v2.2.1
- **持续朝着目标推进**：在 TUI 或 Desktop 中使用 `/goal <objective>` 可让会话持续运行，直到审查者确认目标已达成。控制面包括 `/goal pause`、`/goal resume` 和 `/goal clear`。
- PR：[anomalyco/opencode#59437](https://github.com/anomalyco/opencode/pull/59437)

### process-daemon v0.3.2
- `opencode-pty` 0.3.2（协议 7）由 `packages/process-daemon` 构建，并以 `@opencode/pty@0.3.2` 发布至 npm。影响终端/PTY 使用者；协议升级值得有意固定版本。

---

## 热点问题

1. **[#59387](https://github.com/anomalyco/opencode/issues/59387) — Zen 免费额度主帖（98 条评论，👍9）** — 免费额度故障的集中入口：“Free usage exceeded”、永远不会触发的重置计时器，以及 Desktop 版本拒绝订阅。评论数高反映的是每位用户未解决的配额状态，而不是某个可复现的单一缺陷。

2. **[#18654](https://github.com/anomalyco/opencode/issues/18654) — 无法在 OpenCode Zen 中更改或删除邮箱（14 条评论，👍20）** — 本组中反响最高的条目。在 GitHub 更改邮箱会创建重复的 Zen 身份，且没有面向用户的补救措施。虽被标记为 `pending close`，但 👍 数表明账户生命周期确实存在摩擦。

3. **[#48743](https://github.com/anomalyco/opencode/issues/48743) — MCP 预启动/预热 + 重连路径（10 条评论）** — 在装有 14 个以上本地 stdio MCP 服务器的 Windows Desktop 上，会话启动时由于并发冷启动，*所有*服务器都会被标记为 `failed`。服务器本身是健康的；问题出在监管进程。提供预热/重连接口将修复一整类配置场景。

4. **[#48657](https://github.com/anomalyco/opencode/issues/48657) — `Snapshot.capture()` 卡住时会话始终无法排空（8 条评论）** — `git add --all --pathspec-from-file=-` 在大型未跟踪且未被 gitignore 的目录树上挂起，阻塞 `llm.stream()`，并使执行声明被永久持有。这是没有超时、也没有用户可见诊断的硬挂起。

5. **[#48497](https://github.com/anomalyco/opencode/issues/48497) — 持久记忆系统（`/teach`、`/recall`、`/learn`、`/memory`）（7 条评论）** — 提议提供一等公民的长期记忆，而不是每个会话都重新推导上下文。已作为分诊关闭，但这一请求方向反复出现。

6. **[#49042](https://github.com/anomalyco/opencode/issues/49042) — Agent 循环在零用户输入下运行 500+ 步（6 条评论）** — 模型输出最终文本后仍失控地自动继续。没有步数上限，也没有防护。这与今天的 `/goal` 发布直接相关：持久循环需要持久的安全护栏。

7. **[#24981](https://github.com/anomalyco/opencode/issues/24981) — 在 opencode 配置中显式配置代理（6 条评论，👍8）** — 自 4 月以来仍未关闭。仅靠环境变量代理（`HTTPS_PROXY` 等）无法覆盖按提供商路由，也无法覆盖通过 GUI 启动的 Desktop 会话。

8. **[#59817](https://github.com/anomalyco/opencode/issues/59817) — `shell/<projectID>` 上的 `mkdir EEXIST` 静默回退到主目录（3 条评论，`reproduced`）** — 占用 project-ID 路径的文件导致每个请求都返回 500，并且 TUI 会悄悄在 `$HOME` 中启动会话。静默回退才是危险之处——它可能在错误目录中运行工具。

9. **[#48542](https://github.com/anomalyco/opencode/issues/48542) — 压缩写入的 Anthropic 提示缓存从未被读取（3 条评论）** — 7 天内 70 次压缩写入了 1,278,055 个缓存 token，读取次数为 0。缓存写入按高价计费，因此这是直接、可量化的支出浪费。

10. **[#59916](https://github.com/anomalyco/opencode/issues/59916) — `opencode run` 中按 Ctrl-C 后 bash 工具命令仍在运行（3 条评论）** — 在 bash 工具调用期间收到 SIGINT/SIGTERM 时，OpenCode 退出，但子进程继续执行；使用 `--continue` 时，模型会被告知它“被中断”，导致状态与现实不同步。在 CI 的 `timeout` 和编辑器集成下尤其糟糕。

*同样值得关注：* [#59909](https://github.com/anomalyco/opencode/issues/59909)（Termux 上粘贴会冻结，且随粘贴大小加剧）、[#48740](https://github.com/anomalyco/opencode/issues/48740)（Zen `/v1/responses` 在工具输出包含图片时返回 422——会让截图会话瘫痪）、[#49034](https://github.com/anomalyco/opencode/issues/49034)（Windows 上的 v2 等待后台服务超时）。

---

## 重点 PR 进展

1. **[#60028](https://github.com/anomalyco/opencode/pull/60028) — `feat(core)`：Code Mode 谓词门控调度触发** — 调度可接受 `if: { type: "code", code, allow }`，在每次运行前求值，且**不调用模型**。该程序只能看到允许列表中的工具以及 `state`、`now` 和 `runs`，并返回 `{ wake, state, message? }`。基于调度 PR #59401 构建——这是“昂贵 Agent 之前的廉价门控”模式。

2. **[#60219](https://github.com/anomalyco/opencode/pull/60219) — `fix(core)`：在会话步骤中自动选择工具** — 使 OpenAI 兼容请求在普通步骤中发送 `tool_choice: "auto"`，修复了会拒绝省略该字段请求的 vLLM 网关。关闭 #60202。

3. **[#60228](https://github.com/anomalyco/opencode/pull/60228) — `chore(protocol)`：重新生成 OpenAPI 文档** — Effect 4.0.6 升级（#59630）改变了生成的 schema，导致 `check:generated` 在 `dev` 上失败，并阻塞每个 PR 必需的 `unit (linux)` 检查。差异很小（三个整数字段上的 `maximum: 9007199254740991`），但影响范围很大——合并后可为所有人解除 CI 阻塞。

4. **[#60208](https://github.com/anomalyco/opencode/pull/60208) — `feat(session-ui)`：将用户消息操作收进菜单** — 将每条消息的页脚折叠为 **Rewind to here** + 一个 **More actions**（`…`）菜单，菜单内含时间戳、**Fork from here** 和 **Copy message**。减少长时间线中的视觉噪音。

5. **[#60209](https://github.com/anomalyco/opencode/pull/60209) — `feat(merman)`：渲染 Mermaid 类图** — `classDiagram` 围栏此前在 TUI 中渲染为原始代码。为 merman 增加类图渲染器（引用 #40508）。

6. **[#60222](https://github.com/anomalyco/opencode/pull/60222) — `fix(tui)`：保留捕获的附件和源路径** — 保留 #59833 的保证：Agent 会收到粘贴图片和 PDF 的原始路径，同时仍发送捕获的内容。关闭 #60210。

7. **[#60216](https://github.com/anomalyco/opencode/pull/60216) — `fix(app)`：停止在排队提示增长动画期间丢帧** — 移除提交键处理器中每帧时间线重设样式和强制布局，这些操作会在 M5 Pro 上导致丢帧。这是更广泛 UI 性能清理工作线的代表。

8. **[#60125](https://github.com/anomalyco/opencode/pull/60125) — `fix(core)`：保留无路径的 MCP OAuth 资源** — 自定义提供商校验会丢弃诸如 `https://example.com` 这样的无路径资源。关闭 #58956，并解除对根域名签发方背后受 OAuth 保护的 MCP 服务器的阻塞。

9. **[#60001](https://github.com/anomalyco/opencode/pull/60001) — `feat(app)`：重新设计目标胶囊** — 重新设计 goal 插件的输入区胶囊和弹出层（叠加在 #60000 上，该 PR 增加了 `ComposerPill.icon`）。这是 v2.2.1 `/goal` 发布的 UI 部分。

10. **[#55765](https://github.com/anomalyco/opencode/pull/55765) — `fix(app)`：在非拉丁键盘布局上匹配快捷键** — 当字面绑定未命中时，Ctrl/Meta 快捷键会回退到按键的物理位置；精确的自定义绑定仍然优先。关闭 #55676——这是西里尔文/阿拉伯文/CJK 布局长期存在的可访问性缺口。

*同样已合并/推进中：* [#56010](https://github.com/anomalyco/opencode/pull/56010)（交互式 `plugin add`/`remove` CLI）、[#57090](https://github.com/anomalyco/opencode/pull/57090)（按比例更新 Code Mode 目录的部分内容）、[#60212](https://github.com/anomalyco/opencode/pull/60212)（TUI 中语法高亮的 shell 命令）、[#60223](https://github.com/anomalyco/opencode/pull/60223)（站点页脚中的实时 `/status` 链接）。

---

## 功能请求趋势

- **持久记忆与可复用上下文** — `/teach`、`/recall`、`/learn`、`/memory`（[#48497](https://github.com/anomalyco/opencode/issues/48497)）是最清晰的表述，但同样的需求也以 `AGENTS.md` 变通方案和长生命周期会话中反复重新预热的形式出现。预计这会与 `/goal` 的会话生命周期工作汇合。
- **网络/代理可配置性** — [#24981](https://github.com/anomalyco/opencode/issues/24981) 要求在 `opencode config` 中提供代理设置，而不是仅依赖环境变量。企业环境和 GUI 启动环境是主要驱动力。
- **Zen 的账户生命周期** — 邮箱更改/删除（[#18654](https://github.com/anomalyco/opencode/issues/18654)）以及重复身份清理。与计费相邻，且反复获得点赞。
- **MCP 生命周期管理** — 预热/预启动与重连（[#48743](https://github.com/anomalyco/opencode/issues/48743)），以及无路径 OAuth 资源保留（[#60125](https://github.com/anomalyco/opencode/pull/60125)）。诉求正从“支持 MCP”转向“监管 MCP”。
- **Desktop/TUI UI 缩放与密度** — 在大显示器上模型选择器宽度上限约为 284px（[#48789](https://github.com/anomalyco/opencode/issues/48789)）；标题栏中为推理摘要和展开的 shell/编辑部分提供快速切换（[#48671](https://github.com/anomalyco/opencode/issues/48671)）。
- **为附加的 TUI/ACP 提供会话级模型/Agent 选择** — [#48759](https://github.com/anomalyco/opencode/issues/48759)，来自通过 ACP 驱动同一运行时的嵌入方。这是高级用户/嵌入场景的信号，而非大众需求。

---

## 开发者痛点

- **免费额度配额状态是唯一最响亮的投诉。** [#59387](https://github.com/anomalyco/opencode/issues/59387)（98 条评论）以及 [#59911](https://github.com/anomalyco/opencode/issues/59911) 和 [#60224](https://github.com/anomalyco/opencode/issues/60224) 都描述了同一种故障形态：出现一条带有重置时间戳的限额消息，但该时间戳永远不会到来。用户无法自行诊断，因此每个实例都会变成一个支持讨论串。
- **没有超时的挂起。** [#48657](https://github.com/anomalyco/opencode/issues/48657)（在巨大的未跟踪目录树上执行 `git add`）和 [#59909](https://github.com/anomalyco/opencode/issues/59909)（Termux 粘贴，开销随输入大小增长）都是 O(n) 或更差的停顿，没有进度信号，也无法取消。TUI 没有“已经卡住”的提示手段。
- **失控的 Agent 循环。** [#49042](https://github.com/anomalyco/opencode/issues/49042) 报告了 500+ 次自动继续步骤，既无用户输入也无防护。随着 `/goal` 现已发布，显式的步骤/预算上限显然是后续工作。
- **缓存经济性不可见且不正确。** [#48542](https://github.com/anomalyco/opencode/issues/48542)（1.28M 个缓存写入 token，0 次读取）和 [#48624](https://github.com/anomalyco/opencode/issues/48624)（`glm-5.3-flash` 显示零缓存读取，而 `deepseek-v4.1-flash` 正常缓存）都意味着用户为从未受益的缓存付费——却没有暴露的指标来察觉这一点。
- **信号处理与孤儿子进程。** [#59916](https://github.com/anomalyco/opencode/issues/59916) 在退出后仍让 bash 命令继续运行，*并且*记录一个错误的“已中断”状态，导致恢复后与现实偏离。这种模式对 CI 和编辑器集成很不友好。
- **改变执行上下文的静默回退。** [#59817](https://github.com/anomalyco/opencode/issues/59817) 在 `mkdir` 返回 `EEXIST` 时悄悄将会话迁移到 `$HOME`——改变*工具运行位置*的错误应当大声失败。
- **配置严格性不对称。** [#49027](https://github.com/anomalyco/opencode/issues/49027) 将未知的 Agent 配置字段原样转发到上游提供商请求中，产生 `invalid_request_error`；[#48555](https://github.com/anomalyco/opencode/issues/48555) 在建议了正确名称*之后*，仍因模型名称大小写混合而拒绝子 Agent。配置校验在两个方向上都不一致——对外过于宽松，对内过于脆弱。
- **Windows Desktop 后台服务可靠性。** [#49034](https://github.com/anomalyco/opencode/issues/49034)（托管服务启动超时）、[#48715](https://github.com/anomalyco/opencode/issues/48715)（sidecar 在内存压力下以 `0xC0000409` 崩溃）和 [#48743](https://github.com/anomalyco/opencode/issues/48743)（MCP 冷启动失败）构成一个一致的 Windows 稳定性问题簇。

---

*本周期未提供 Discussion 数据，因此省略该部分。*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区文摘 — 2026-10-29

## 今日亮点
过去 24 小时没有新版本发布；活动主要集中在一个长期存在的 Windows 可用性讨论帖（#7547，目前 91 条评论）以及一组全屏/TUI 渲染性能报告上。修复方面，一批已关闭的 PR 处理了会话存储语义、终端控制序列处理、`pi.exec` 缓冲和环境守护进程终止清理，而 Bedrock Mantle 路由和 Anthropic 服务端消息线程仍在审查中。

## 发布
过去 24 小时内无发布。

## 热门 Issue

1. **[#7547](https://github.com/earendil-works/pi/issues/7547) — [Windows] 你如何在 Windows 上使用 Pi？遇到了哪些问题？**（91 条评论，👍2）  
   这是 Windows 支持的集中汇总帖。其动机具有明确的战略性：受支持的运行路径太多，导致不清楚应该修 bug、改进文档，还是交给扩展。它是数据集中互动量最高的 issue，也是判断 Windows 投入方向的最佳信号。

2. **[#9549](https://github.com/earendil-works/pi/issues/9549) — 大型会话记录每帧重新渲染；调整尺寸会重新输出整个会话记录**（17 条评论，👍1）  
   在具有 2 个逻辑核心的 Windows Terminal 上，大型会话记录会占满一个核心。该问题附带 agent 辅助测量结果，并通过 `pi -ne` 验证，因此可操作性异常强。它指向全屏渲染循环架构，而非一次性 bug。

3. **[#7739](https://github.com/earendil-works/pi/issues/7739) — 设定启动时间预算，以对标 jcode 的延迟和内存占用**（12 条评论）  
   提议基于基准测试制定一个具体预算，对标 jcode（10 次交互式 PTY 启动，Linux）。这表明社区希望将启动延迟视为可跟踪、可防回归的指标，而不是临时关注的问题。

4. **[#9656](https://github.com/earendil-works/pi/issues/9656) — 在 Windows + Zellij 的全屏模式下，鼠标滚轮滚动的是提示历史而非会话记录**（11 条评论，👍5）  
   这是此处开放 bug 中点赞数最高的。复现路径很窄（Windows → Alacritty → Zellij → Pi；不使用复用器时正常，在 Linux+tmux 上正常），这强烈表明问题出在复用器/终端能力检测上。

5. **[#9884](https://github.com/earendil-works/pi/issues/9884) — 配置的默认模型有时在启动时被回退模型替换**（7 条评论）  
   在 20 次启动中复现 4 次：来自运行时发现扩展目录的 `defaultProvider`/`defaultModel` 被静默替换为内置的 `deepseek/deepseek-v4-pro`。启动状态不确定对固定模型的团队而言是信任问题。

6. **[#10954](https://github.com/earendil-works/pi/issues/10954) — OpenCode Go Claude（`anthropic-messages`）：工具列表变更后重放 thinking block 时出现 400**（6 条评论）  
   与 #10324 属于同一类故障，但发生在不同的传输通道上，且 `block_binding` 仅在 `supportsMidConvoEffort` 时发送。对于任何通过第三方中继路由 Anthropic 兼容流量的人都很重要；该问题由 AI 披露并经实验验证。

7. **[#10386](https://github.com/earendil-works/pi/issues/10386) — Durable：托管编码代理扩展（AgentSession 支持的运行时或 hook 对等能力）**（6 条评论，👍1）  
   请求提供由 `AgentSession` 支持的运行时，或 `ExtensionAPI` hook 对等能力，以便 Durable 能作为一等扩展运行。这代表了对与核心能力匹配的扩展接口的更广泛诉求。

8. **[#10616](https://github.com/earendil-works/pi/issues/10616) — 扩展 `pi.exec` 在 Windows 上执行 `.cmd` 命令失败（npm、npx、pnpm）**（5 条评论）  
   `pi.exec("npm", ["--version"])` 返回退出码 1 且 stdout 为空，而 bash 工具执行成功。这是一个清晰、独立的 Windows 扩展 API 缺陷，对扩展生态有明显影响。

9. **[#10770](https://github.com/earendil-works/pi/issues/10770) — `.pi/agent/sessions` 的默认权限应为 `0700`**（5 条评论，`inprogress`）  
   新建的会话目录可被同组/所有用户读取。已标记为进行中；这是一个直截了当的隐私加固请求，对共享机器和 CI runner 很重要。

10. **[#11567](https://github.com/earendil-works/pi/issues/11567) — 长会话中打字变慢：`getSessionName()` 在每次渲染时扫描每个条目**（2 条评论，今日提交）  
    在约 40 万个条目时，按键到渲染的 p50 为 21 ms，而空状态下为 1.5 ms；性能分析将 94% 的繁忙 CPU 归因于每次渲染时由页脚调用的 `SessionManager.getSessionName()`。这是一个新提交、剖析充分且修复路径明确的回归问题。

*同样值得关注：* [#11529](https://github.com/earendil-works/pi/issues/11529)（MCP 服务器重连时，来自 `Map` 的工具排序会破坏提示缓存）和 [#11527](https://github.com/earendil-works/pi/issues/11527)（每个请求新建 `BedrockRuntimeClient`，强制进行 SSO 凭据往返）。

## 关键 PR 进展

1. **[#11565](https://github.com/earendil-works/pi/pull/11565) — fix(tui)：在常规模式下停止写入 OSC 133 提示符标记**（已关闭）  
   常规 TUI 模式会发出 OSC 133 `A`/`B`/`C` 标记；显示 OSC 7501 状态的终端（Rex、iTerm2 3.7.5+）将 `133;A` 解释为新的 shell 提示符，并丢弃工作指示器。解决 #11519。

2. **[#11561](https://github.com/earendil-works/pi/pull/11561) — fix(coding-agent)：将会话目录中的会话视为默认存储**（已关闭）  
   当会话的 cwd 与其文件夹名不一致时，`usesDefaultSessionDir()` 会返回 false，因此 `/resume` 会静默地只列出一部分会话。修复了一个令人困惑的会话发现 bug。

3. **[#11562](https://github.com/earendil-works/pi/pull/11562) — fix(coding-agent)：在 cwd 的默认目录中启动新会话**（开放）  
   作为 #11561 的配套改动：`/new`、`/fork` 和 `/clone` 目前会继承已打开文件的文件夹，并将其视为自定义 `--session-dir`。使新会话存储与实际工作目录保持一致。

4. **[#11556](https://github.com/earendil-works/pi/pull/11556) — fix(coding-agent)：让 SDK 流为摘要解析认证**（已关闭）  
   来自 `createAgentSession` 的会话将已解析的访问令牌作为 `options.apiKey` 传给压缩/分支/bug 报告摘要，绕过了 `modelRuntime.streamSimple` 的认证解析。对使用 OAuth 的 SDK 嵌入方很重要。

5. **[#11546](https://github.com/earendil-works/pi/pull/11546) — fix(coding-agent)：将 `pi.exec` 输出上限设为 256 MiB**（已关闭，关闭 #11360）  
   新增 `ExecOptions.maxBuffer`，默认取 256 MiB 与运行时字符串限制中的较小值；超限命令会被终止，并返回 `truncated: true` 和非零退出码。防止输出过多的子进程导致内存失控。

6. **[#11545](https://github.com/earendil-works/pi/pull/11545) — Anthropic 消息线程**（开放）  
   使用 Anthropic 的服务端消息线程 beta，在会话 ID 和缓存启用时只发送新消息，同时仍构建完整请求用于比较。对于长会话，可能显著降低 token/成本。

7. **[#11544](https://github.com/earendil-works/pi/pull/11544) — fix(env)：守护进程被终止时杀掉其命令**（已关闭）  
   `Connection.close()` 在守护进程来得及运行 `kill_all` 之前发送了 `SIGTERM`，导致命令继续运行。这是本地 Unix 守护进程路径的干净清理修复。

8. **[#11543](https://github.com/earendil-works/pi/pull/11543) / [#11541](https://github.com/earendil-works/pi/pull/11541) — fix(coding-agent)：在 `stripAnsi` 中剥离私有参数 CSI 序列**（已关闭）  
   `stripAnsi` 将 `<`、`=`、`>` 视为 CSI 终止字节，因此像 `ESC[<35;59;1M` 这样的鼠标报告会把参数以字面文本形式泄漏到工具输出中。合并了两个实现；属于底层问题，但会影响会话记录的整洁性。

9. **[#11348](https://github.com/earendil-works/pi/pull/11348) — fix(tui,coding-agent)：在所有滚动列表中处理选择翻页键**（已关闭，修复 #7629）  
   对 #7680 进行 rebase 并缩小范围：在选择组件、自动补全和代理选择器中遵循 `tui.select.pageUp`/`pageDown`，并通过显式全屏捕获标志让活动选择优先于会话记录滚动。

10. **[#11372](https://github.com/earendil-works/pi/pull/11372) — fix(ai)：将 Converse 不提供的 Bedrock 模型路由到 Bedrock Mantle**（开放）  
    针对 Bedrock API key 处理 #5363：列出的 16 个模型（GPT-5.4–GPT-6.1、gpt-oss、Grok 4.3/4.6、Gemma 4）仅在 Mantle 上可用，Mantle 是 OpenAI 兼容端点，而目前它们会因无效的 Converse 模型标识符而失败。堆叠的后续 PR #8572/#8573 增加了 SigV4/IAM 认证和 Anthropic Messages 路由。

## 热门讨论

**展示与分享**

- **[#11503](https://github.com/earendil-works/pi/discussions/11503) — `pi-rs`：无头核心的 Rust 移植——逐原子验证行为对等（+ 一项 DeepSeek API 兼容性发现）**（1 条评论，👍1）  
  社区对 pi 0.74.2 无头核心的 Rust 重实现：agent 循环、七个内置工具、会话 JSONL、CLI 界面和系统提示，采用 MIT 许可证，并在设计上致谢 pi。包含一项 DeepSeek API 兼容性发现。它既是对等性测试的产物，也表明对非 Node 运行时的需求。

## 功能请求趋势

- **将 Windows 作为一等平台。** 最集中的一组：使用路径整合（#7547）、`.cmd`/`npx` 执行（#10616）、感知复用器的滚动（#9656），以及低核心数 Windows 终端上的渲染开销（#9549）。
- **将性能作为有预算、可度量的属性。** 启动延迟对标 jcode（#7739）、每帧会话记录重新渲染（#9549），以及渲染路径上的 O(entries) 工作（#11567），都把性能视为需要防范的回归，而不仅仅是要优化的对象。
- **更深入的扩展 API。** hook 对等能力 / 由 `AgentSession` 支持的运行时（#10386）、用于门控和截断的可返回 `tool_result` 结果（#11553）、命名空间作用域的 `tool_search`（`+docs read_page`，#11558）、延迟运行时重载（#7948），以及按工具控制 MCP 暴露的开关（#11526）。
- **提供商与目录正确性。** Bedrock Claude 的提示缓存元数据（#11528）、Converse 拒绝的模型的 Mantle 路由（#11372），以及可配置的 OAuth/Copilot 超时（#11549）。
- **隐私与凭据卫生。** `0700` 会话目录（#10770），以及将 `deviceId` 移出可共享的全局设置（#10187）。
- **会话存储语义。** 默认目录解析和 `/resume` 可见性（#11561、#11562）——小修复，却带来不成比例的 UX 影响。

## 开发者痛点

- **Windows 支持分散在多条运行路径上。** 这个有 91 条评论的汇总帖明确道出了根本挫败感：用户无法判断哪种 Windows 配置是“官方认可”的，扩展作者则会遇到平台特定的损坏（`pi.exec` 执行 `.cmd`、Zellij 复用、终端状态标记）。
- **长会话会降低交互性。** 两份独立报告（#9549、#11567）将变慢归因于每次渲染都执行的工作——完整会话记录重新发送和每次渲染扫描会话名。在 40 万个条目时，21 ms 对 1.5 ms 的按键延迟是一个具体上限。
- **提示缓存很容易被意外失效。** `Map` 中的工具插入顺序（#11529）和 Bedrock 上缺失的 `promptCache` 元数据（#11528）会静默地将缓存轮次变为全价轮次。
- **启动配置不确定。** 配置的默认模型在 20% 的启动中被回退模型替换（#9884），削弱了团队的可复现性。
- **提供商适配器会暴露上游怪癖。** 通过中继重放 thinking block（#10954）、每个请求构建 Bedrock 客户端从而强制 SSO 往返（#11527）、默认输出预算过大触发 HTTP 402（#11517），以及固定的 15s/5s OAuth 超时（#11549）。
- **会话存储令人意外。** 会话存放在以原始 cwd 命名的文件夹中，导致 `/resume` 隐藏部分会话，`/new` 继承错误目录（#11561、#11562）。
- **子进程与守护进程生命周期的边界情况。** 无界的 `pi.exec` 输出（#11360 → #11546）、辅助线程耗尽时 panic（#11433），以及守护进程关闭时命令成为孤儿（#11544），都是“一直能用，直到不能用”的故障模式。

---

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区摘要 — 2026-10-29

## 1. 今日亮点

v0.26.3 发布线已覆盖 CLI、Desktop 和 TypeScript SDK。生成的 UI 卡片现在能在导航和分屏视图之间保留输入与状态，并且仅在支持 MCP Apps 的客户端上渲染实时结果。与此同时，Issue 跟踪器主要被 CI 和发布基础设施相关工作占据：一个长期存在的 Managed Agent 门禁失败已累积 79 条评论，而新报告的 P1 配置回归（`contextWindowSize` 被静默移除）是今日对用户影响最大的新 bug。

## 2. 发布

- **[v0.26.3](https://github.com/QwenLM/qwen-code/releases)** — 主要稳定版。源 feed 中的发布说明仅为占位符（`qwen-release-notes:v1`），因此实质性变更可通过下方随附的 Desktop 和 nightly 条目查看。
- **[desktop-v0.26.3](https://github.com/QwenLM/qwen-code/releases)** — 新增托管会话权限规则和 Auto approval（[#15448](https://github.com/QwenLM/qwen-code/pull/15448)），并加固 CLI 中的 serve token 处理。
- **[v0.26.3-nightly.20261028.ca204559a7](https://github.com/QwenLM/qwen-code/releases)** — 包含 `fix(config): report controls that were silently dropped`（[#15794](https://github.com/QwenLM/qwen-code/pull/15794)）以及托管模式文档更新。
- **[sdk-typescript-v0.1.25](https://github.com/QwenLM/qwen-code/releases)** — 捆绑 CLI 0.26.3（与 SDK 使用同一 source ref 构建）。
- **重点变更：** 生成的 UI 卡片在导航和分屏视图之间保留输入和状态，在初次计算后显示实时视图，并且仅对能够渲染 MCP Apps 的客户端开放（[#15611](https://github.com/QwenLM/qwen-code/pull/15611)、[#15787](https://github.com/QwenLM/qwen-code/pull/15787)）。

## 3. 热门 Issue

1. **[#15855](https://github.com/QwenLM/qwen-code/issues/15855) — `contextWindowSize` 被自动移除** `[P1, bug, configuration, need-information]` · 3 条评论 · 今日新增。使用本地 OpenAI 兼容后端（Ollama，128k 窗口）的用户报告称，其配置的上下文限制被从设置中剥离，这可能导致意外截断或上下文溢出。本时间窗口内最高严重级别的新用户可见 bug。
2. **[#14724](https://github.com/QwenLM/qwen-code/issues/14724) — 主 CI 失败：Managed Agent 门禁，未识别到测试** `[bug, autofix/skip]` · 79 条评论 · 自 2026-10-19 起开放。仓库中讨论最热烈的单一线程；机器人不断追加 commit 标记，却始终无法收敛到具体的测试归因，而这正是跟踪器本应防止的失败模式。
3. **[#13078](https://github.com/QwenLM/qwen-code/issues/13078) — 每日依赖 CVE 审计失败** `[needs-triage, ci-cd]` · 35 条评论 · 一个持续一个月的反复失败。无论原因是真实的高危安全公告，还是 npm audit 端点不稳定，该信号已连续 30 天不可用，构成供应链风险。
4. **[#15861](https://github.com/QwenLM/qwen-code/issues/15861) — Web Shell 宿主目录选择器冒烟测试确定性超时** `[P1, testing, ready-for-agent]` · 2 条评论 · 今日新开。与 PR 无关的失败意味着每位贡献者都要多付出一轮重新归因的成本；报告者明确将其称为对无关 PR 的额外负担。
5. **[#15052](https://github.com/QwenLM/qwen-code/issues/15052) — 当待处理运行被替换时，保留独立的 main E2E 验证** `[P2, ci-cd, ready-for-human]` · 14 条评论 · 即使设置 `cancel-in-progress: false`，按 ref 作用域的并发也可能取消较早的待处理运行，因此中间的 `main` SHA 会失去独立验证。这对任何依赖逐 commit 绿色状态的人都很重要。
6. **[#15461](https://github.com/QwenLM/qwen-code/issues/15461) — 将仅发布用主机从共享 runner 池中隔离** `[P2, ci-cd, ready-for-human]` · 7 条评论 · 通往 `ecs-qwen-release-host` 的可选路径已合并，但机器从未完成置备，因此 `publish` 仍在争夺 GitHub 托管容量。与发布延迟直接相关。
7. **[#15783](https://github.com/QwenLM/qwen-code/issues/15783) — 将常规 macOS/Darwin 验证移至夜间运行** `[P3, ci-cd, need-discussion]` · 4 条评论 · 提议制定全仓库策略：在 `main` 上于 Asia/Shanghai 时区夜间运行 macOS，PR 仅运行 Linux。这是一个会影响贡献者反馈循环的成本/容量决策。
8. **[#11665](https://github.com/QwenLM/qwen-code/issues/11665) — Responses 清理可能破坏 reasoning/tool-call 邻接关系** `[P2, core, CLOSED]` · 5 条评论 · 一个微妙的正确性 bug：重放的 `reasoning` 项及其 `function_call` 必须作为一个整体处理，但清理逻辑将它们分开处理。关闭该问题对兼容 OpenAI Responses 的提供商而言是一次有意义的可靠性提升。
9. **[#1206](https://github.com/QwenLM/qwen-code/issues/1206) — 为 OpenAI 兼容 API 提供动态多模型支持** `[feature-request]` · 3 条评论，2 👍 · 自 2025-12-10 起开放。用户希望从单个端点发现并切换模型，而不是硬编码某一个。本批次中存续时间最长的产品请求。
10. **[#15866](https://github.com/QwenLM/qwen-code/issues/15866) — 在已归档会话行上内联显示 Export 和 Restore** `[P3, ui, web-shell]` · 2 条评论 · 今日新开。虽小，但代表了围绕会话生命周期操作的 Web Shell 易用性请求的稳定流入。

## 4. 关键 PR 进展

1. **[#15620](https://github.com/QwenLM/qwen-code/pull/15620) — feat(channels)：Microsoft Teams 扩展与宿主入口** · 新增对 Teams 个人聊天、显式群组提及、频道线程隔离以及有序最终文本回复的支持，可独立使用，也可通过 `createTeamsIngress` 嵌入。本批次中最大的新集成面。
2. **[#15865](https://github.com/QwenLM/qwen-code/pull/15865) — feat(managed)：WebShell 中的实时消息分页** · 直接从 SQL 读取最新 100 条物化条目（包括没有已发布快照的会话），向上滚动时分页加载更早的消息，并将实时流量保留在现有事件流上。
3. **[#15863](https://github.com/QwenLM/qwen-code/pull/15863) — fix(cli)：在上下文用量中分离指令文件与自动记忆** · 在 API、SDK 类型和终端输出中，将上下文用量明细拆分为 “Instruction files”（项目/用户/扩展/语言）与 “Auto memory”（策略 + 目录快照）。使上下文预算可审计。
4. **[#15849](https://github.com/QwenLM/qwen-code/pull/15849) — feat(web-shell)：扩展一键更新覆盖范围** · 将守护进程一键更新扩展到 pnpm/yarn/bun 全局安装，以及需要 sudo 的 npm prefix，并使用用户作用域的隔离暂存 prefix。
5. **[#15837](https://github.com/QwenLM/qwen-code/pull/15837) — fix(cli)：仅在配置就绪后才回复 SDK `initialize`** · 关闭一个竞态：SDK 控制响应可能在 SDK MCP 服务器和已暂存的 SDK 专用配置就位之前发出。对 SDK 嵌入方很重要。
6. **[#15835](https://github.com/QwenLM/qwen-code/pull/15835) — fix(web-shell)：当目录为空时保持 “+ Add session” 可点击** · 该按钮恰好在“打开时重新加载”效果所要处理的情况下被禁用，导致选择器无法访问。一个直接的 Split View 可用性修复。
7. **[#13445](https://github.com/QwenLM/qwen-code/pull/13445) — feat：将计划审批渲染为 markdown 并将计划捕获为 Todos** · 长期运行的草稿，替换 `ToolApproval.tsx` 中的原始 `<pre>` 输出，并将已批准的计划转为 Session Workflow Todos。值得注意的是，它是最早的仍未关闭的功能 PR 之一。
8. **[#14577](https://github.com/QwenLM/qwen-code/pull/14577) — fix(web-shell)：清理会话已离开活动目录的分屏面板** · 将守护进程的 `409 session_archived` 视为终态，因此已归档会话的面板不再保持空白并无限重试。
9. **[#15854](https://github.com/QwenLM/qwen-code/pull/15854) — perf(release)：跨发布复用已验证的原生音频预构建** · 对音频构建输入做指纹识别，并在完全相同时复用此前官方 `main` 产物，避免每次发布重建五个目标。直接回应发布耗时投诉。
10. **[#15864](https://github.com/QwenLM/qwen-code/pull/15864) — fix(ci)：对仅 Companion 变更跳过源码 TUI 门禁** · 对仅涉及 VSCode-Companion 的 PR 跳过 TUI 一致性/无闪烁任务，但对于 manifest、bundler 配置、`scripts/` 和混合变更仍会运行这些任务。

## 5. 热门讨论

源 feed 未提供讨论数据，因此本节省略。

## 6. 功能请求趋势

- **Web Shell / Desktop 易用性是主导产品主题。** Session Overview 和 Split View 的键盘快捷键（[#13175](https://github.com/QwenLM/qwen-code/issues/13175)，已关闭）、已归档行上的内联 Export/Restore（[#15866](https://github.com/QwenLM/qwen-code/issues/15866)）、侧边栏分类标题对齐（[#15867](https://github.com/QwenLM/qwen-code/issues/15867)）、宿主提供的目录选择器（[#15186](https://github.com/QwenLM/qwen-code/issues/15186)，已关闭），以及 macOS 守护进程 `PATH` 扩展（[#12690](https://github.com/QwenLM/qwen-code/issues/12690)，已关闭），都指向打磨外壳，而非增加核心能力。
- **会话管理正确性。** 无工作区会话在 Session Overview 中缺失（[#11878](https://github.com/QwenLM/qwen-code/issues/11878)，已关闭）以及磁盘恢复后分支操作消失（[#13782](https://github.com/QwenLM/qwen-code/issues/13782)，已关闭）均已解决；拆分、归档和恢复会话仍是活跃的摩擦点。
- **提供商/模型灵活性。** 在 OpenAI 兼容端点上动态发现和切换模型（[#1206](https://github.com/QwenLM/qwen-code/issues/1206)）仍是最明确未满足的诉求，今日本地 Ollama 用户的 `contextWindowSize` 回归（[#15855](https://github.com/QwenLM/qwen-code/issues/15855)）进一步强化了这一点。
- **IDE 与远程开发覆盖。** VS Code companion 仍需覆盖转发端口宿主门禁、IPv6 CSP 以及 Remote-SSH/WSL 运行（[#12059](https://github.com/QwenLM/qwen-code/issues/12059)）。
- **Skills 生命周期成熟度。** 来自用户作用域 Skill 激活 PR（[#15622](https://github.com/QwenLM/qwen-code/issues/15622)）的后续工作表明，该功能正在交付，但仍在积累契约与生命周期改进。
- **本地化。** 浏览器区域设置回退（所有非 `zh-CN` 都折叠为 `en`）已在 Goal 卡片中提出（[#13391](https://github.com/QwenLM/qwen-code/issues/13391)，已关闭）——i18n 范围仍只有两个区域设置。

## 7. 开发者痛点

- **CI 噪声正在挤占真实信号。** 机器人提交的 “no test identified” Issue（[#14724](https://github.com/QwenLM/qwen-code/issues/14724) 有 79 条评论，[#15173](https://github.com/QwenLM/qwen-code/issues/15173) 有 19 条，另有若干逐 commit 的 Managed Agent 门禁报告）产生了大量评论，却无法收敛到归因；而“变绿即关闭”的报告仍需手动驳回。
- **确定性的、与 PR 无关的测试失败会阻塞所有人。** [#15861](https://github.com/QwenLM/qwen-code/issues/15861) 使 Web Shell 冒烟测试通道在无关 PR 上失败，迫使每次变更都要重新归因。
- **发布流水线延迟与容量。** 发布后扇出仍在 GitHub 托管 runner 上等待 37–50 分钟（[#15465](https://github.com/QwenLM/qwen-code/issues/15465)），而专用发布主机仍未置备（[#15461](https://github.com/QwenLM/qwen-code/issues/15461)）。
- **逐 SHA 验证可能被静默丢失。** `main` E2E 上的并发替换意味着中间 commit 可能永远得不到独立结论（[#15052](https://github.com/QwenLM/qwen-code/issues/15052)）。
- **macOS runner 成本压力。** 反复提出的将 Darwin 验证移至夜间的提议（[#15783](https://github.com/QwenLM/qwen-code/issues/15783)）反映了贡献者在 PR 周转中感受到的队列/成本权衡。
- **安全审计已亮红灯一个月。** 每日依赖 CVE 审计（[#13078](https://github.com/QwenLM/qwen-code/issues/13078)）已产生 35 条评论却未解决，使团队无法确定是否仍有真实的安全公告未处理。
- **配置并不总是可信。** 静默丢弃设置既以修复形式出现（`fix(config): report controls that were silently dropped`，[#15794](https://github.com/QwenLM/qwen-code/pull/15794)），也作为新的 P1 报告出现（[#15855](https://github.com/QwenLM/qwen-code/issues/15855)）——用户希望 CLI 暴露而不是吞掉它无法应用的配置。
- **运行时环境脆弱性。** 历史上因 RHEL 10 缺少 full-ICU `Intl.Segmenter` 导致的崩溃（[#11747](https://github.com/QwenLM/qwen-code/issues/11747)，已关闭）凸显了一类反复出现的宿主运行时不兼容问题，且缺乏可操作的诊断信息。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*