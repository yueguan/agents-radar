# AI CLI 工具社区动态日报 2026-10-30

> 生成时间: 2026-10-30 02:07 UTC | 覆盖工具: 7 个

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

# 跨工具 AI CLI 对比报告 — 2026-10-30

## 1. 生态概览

AI CLI 生态正处于**可靠性与成本加固阶段**。发布频繁——stable、nightly、SDK、desktop 和 daemon 等多条发布轨道并行——但社区注意力已从原始模型能力转向**agent 正确性、配额核算、会话存储增长、权限语义以及非交互/ACP 一致性**。Windows 在多个工具中仍是反复出现的薄弱平台，而 MCP/插件认证与生命周期行为正成为整个生态的共同瓶颈。PR 吞吐量差异显著：Codex 和 Qwen 展现出高速的合并/发布工程能力，Pi 和 OpenCode 正在积极合入修复，而 Claude Code 和 Copilot CLI 的 issue 量很大，外部 PR 吞吐量却相对较低。

---

## 2. 活动对比

下表中的计数均来自所提供摘要。若摘要未提供完整的 tracker 计数，则用 † 标出可见条目的数量。N/A 表示该数据集中未提供讨论数据；这并不表示该渠道不活跃。

| 工具 | Issue 活动 | PR 活动 | 讨论 | 今日发布状态 |
|---|---:|---:|---:|---|
| **Claude Code** | 50 条更新 | 2 条更新 | N/A | **v2.1.314** — 仓库外写入拦截；IdP 组网关路由 |
| **OpenAI Codex** | ≥15 条可见† | 列出 17 条 | 10 | **rust-v0.166.0** — transcript 选择/搜索、Markdown 一致性 |
| **Gemini CLI** | 10 条可见† | 列出 10 条 | N/A | **v0.67.0-nightly** — README 适用范围、内联历史调整大小修复 |
| **GitHub Copilot CLI** | 38 条更新 | 0 条更新 | N/A | **无发布** |
| **OpenCode** | 13 条可见† | 列出 14 条 | N/A | **v2.2.2 / v2.2.1** + process-daemon 发布 |
| **Pi** | 50 条更新 | 23 条更新 | 2 | **无发布** |
| **Qwen Code** | ≥15 条可见† | 列出 20 条 | N/A | **v0.26.4** CLI、SDK、Desktop + nightly |

**显著信号：** Claude Code 的 issue 与 PR 比例高度不对称（50 个 issue 对 2 个 PR；其中一个 PR 仅修改 README）。Copilot CLI 的 issue 流量很大，但 PR/发布零进展。Codex 的 PR 活动主要是一条由 bot 编写的合并列车。Pi 和 Qwen 在本窗口内展现出最强的维护吞吐量。

---

## 3. 共同功能方向

### 成本、配额与提示缓存确定性
在 Claude Code、Codex、OpenCode、Pi、Copilot CLI 和 Qwen Code 中，用户都希望获得**可预测、可审计的支出**。具体需求包括：
- 各入口核算一致性 —— Claude Code 无头模式 `claude -p` 的配额惩罚（#97074）。
- 纳入子代理与扇出成本 —— OpenCode（#45417）、Claude Code 的 spinner/token 开销（#86879）。
- 供应商报告的成本与正确的长上下文/缓存定价 —— Pi（#6881）、OpenCode（#59803）。
- 通过确定性的工具排序实现稳定的提示缓存 —— Pi（#11529）。
- 可预测的配额重置，而非临时促销 —— Codex（#57967）、OpenCode 免费层大型讨论帖（#59387）、Copilot PRU 耗尽（#4802）。

### 会话存储、压缩与长会话资源控制
这是严重程度最高的反复出现主题：
- Codex 报告称，在压缩和子代理分叉期间，因 image/base64 重复持久化导致会话增长 16–165 GiB。
- OpenCode 报告出现 13 GB 的 `event` 表以及 O(n²) 历史记录重复读取。
- Copilot CLI 报告长时间并行代理会话中 RSS 增长 37 GiB，以及 300 秒压缩超时。
- Claude Code 报告 git 轮询导致空闲 CPU 消耗，以及 Windows VM 泄漏。
- Pi 报告多日会话中出现 V8 OOM，以及 transcript 渲染扩展性问题。

共同需求：**面向用户的会话检查/清理、有界压缩、图像去重和资源上限**。

### 带恢复路径的权限与安全控制
社区希望获得**显式、可组合的策略**——而不只是限制：
- Claude Code：仓库外写入拦截、可信设备覆盖、符号链接配置目录行为。
- Codex：受管网络策略、Windows 沙箱拒绝、MCP 审批元数据。
- Copilot CLI：默认拒绝的工具允许列表和非交互沙箱。
- OpenCode：在 Code Mode 内部应用 `webfetch` 权限、按服务器配置 TLS 信任。
- Gemini CLI：沙箱缺口，例如 Podman socket 以及 checkpoint fallback 中的路径遍历。
- Pi：会话文件权限和正确的项目信任语义。

一个常见故障模式是**不透明拒绝且无补救措施**——策略阻断的破坏性反而小于缺少审查/审批路径。

### 远程、移动端、ACP 与非交互一致性
用户越来越多地在 CI、daemon、远程会话和移动客户端中运行这些工具：
- Claude Code Remote Control 在 CLI、desktop、VS Code 和移动端之间表现不一致。
- Codex 存在 Android 队列 bug 以及 app-server/TMUX 环境归因错误。
- Copilot CLI 的 ACP 接口缺少 `contextTier`、`ask_user` 以及非阻塞 `session/prompt` 语义。
- OpenCode 和 Qwen Code 都出现 daemon/Web Shell/SDK 协议保真度问题。
- Gemini CLI 子代理需要更好的可观测性和 `/bug` 上下文。

共同诉求：**在每个界面上保持相同的会话身份、权限、工具和取消语义**。

### MCP/插件生命周期与认证健壮性
MCP 正在成为一等集成层，但认证和生命周期仍然脆弱：
- Copilot CLI：OAuth 重复提示、Entra token 缓存失败、供应商 token 交换失败。
- Claude Code：声明了 elicitation 能力但未渲染；`.mcpb` 安装提示；marketplace git 轮询。
- Codex：MCP 工具审批元数据和凭据泄漏防护。
- OpenCode：重复的 MCP 进程和私有网络 TLS 信任。
- Pi：围绕 `session_start`、压缩和关闭的扩展生命周期竞态。

### 子代理/代理循环可观测性与控制
在 Gemini CLI、OpenCode、Claude Code、Codex、Pi 和 Qwen Code 中：
- 子代理在 `MAX_TURNS` 耗尽后仍报告成功（Gemini #22323）。
- 代理会无限期挂起，或过早终止自主工作。
- `/goal`、`/loop` 和停止控件并不总能取消待处理工作。
- 用户希望有活动侧边栏、bug 报告中的轨迹，以及程序化停止钩子。

---

## 4. 差异化分析

| 工具 | 主要关注点 | 目标用户 | 技术路径 |
|---|---|---|---|
| **Claude Code** | 企业权限管理、Remote Control、Cowork/desktop、网关路由 | 使用 Anthropic 模型的团队和企业 | 策略丰富的 CLI/desktop/VS Code 集成；权限模型强，但外部 PR 吞吐量低 |
| **OpenAI Codex** | Rust CLI/desktop/VS Code、受管 app-server、MCP 审批、发布自动化 | OpenAI 生态、企业网络部署 | 高速自动化合并/发布工程；强 transcript UX 和平台加固 |
| **Gemini CLI** | 子代理运行时、沙箱、AST 感知工具、Google/Gemini 生态 | Gemini 和 Google Cloud 开发者 | 由 nightly 驱动的代理运行时实验；安全与 token 效率研究 |
| **GitHub Copilot CLI** | GitHub/组织集成、ACP 自动化、MCP/Entra 认证 | GitHub Enterprise 和 CI 自动化用户 | 非交互/ACP 优先界面；强策略需求，但本窗口修复吞吐慢 |
| **OpenCode** | 供应商无关的开放工具、Desktop/TUI、插件、Zen/免费层 | 高级用户、自托管者、多供应商团队 | 客户端发布快、供应商支持广；desktop 发布节奏易出现回归 |
| **Pi** | 极简无头核心、TUI 性能、供应商抽象、扩展 | 终端高级用户和扩展开发者 | 聚焦正确性/性能的维护；无头核心与 TUI 干净分离 |
| **Qwen Code** | Qwen/阿里巴巴生态、自托管模型、daemon/Web Shell/Desktop/SDK | 本地/自托管和企业 Qwen 用户 | 多界面 monorepo；重度投入 CI/发布基础设施 |

**关键对比：** Claude Code 和 Copilot CLI 在**企业策略和组织集成**方面最强，但目前社区修复吞吐较弱。Codex、Pi 和 Qwen Code 在**发布工程和维护速度**方面更强。OpenCode 最具**供应商无关性和快速演进**特点，但存在明显的回归和配额管理风险。Gemini CLI 最偏向**代理运行时/研究导向**，其核心弱点是子代理可靠性。

---

## 5. 社区动能与成熟度

**issue 参与度最高：** Claude Code（#97074，115 👍）、OpenCode（#59387，113 条评论）、Codex 的会话存储报告，以及 Copilot CLI 长期存在的 403/模型列表讨论帖。这些是成熟社区，如今压力测试的是成本、策略和规模，而非基础功能。

**本窗口 PR/发布速度最高：** Codex（bot 合并列车，rust-v0.166.0）、OpenCode（两次客户端发布外加 daemon 包）、Pi（50 个 issue 和 23 个 PR 更新，大规模修复潮）以及 Qwen Code（v0.26.4 覆盖 CLI/SDK/Desktop，外加 nightly）。这些工具迭代很快，不过 OpenCode 出现逐个版本间的 desktop 回归，Qwen 则存在长期性的 CI 归因问题。

**成熟度 vs. 响应性：**
- **Claude Code** 企业级成熟，但存在明显的问题量与修复量错配：50 个 issue 对 2 个 PR，且其中一个实质性社区修复自 3 月起一直开放。
- **Copilot CLI** 组织覆盖强，但今天没有 PR/发布进展，且 MCP/ACP 认证问题持续反复。
- **Gemini CLI** 保持 nightly 活跃，并持续改进安全/代理运行时正确性，但子代理可靠性仍是 P1 级信任问题。
- **Pi** 在本窗口内看起来运营上较成熟：许多隐私、路径处理和性能修复在当天关闭。
- **Qwen Code** 正大力投入 CI/发布基础设施，但这些失败如今已成为贡献者的头等痛点。

---

## 6. 趋势信号

1. **代理可靠性如今比模型新颖性更重要。** 子代理虚假成功、无限期挂起、过早终止和不可取消循环在 Gemini CLI、OpenCode、Claude Code、Codex、Pi 和 Qwen Code 中反复出现。开发者需要**可观测、可取消、有界的代理循环**。

2. **成本与存储已是一等产品界面。** token/配额核算、提示缓存稳定性、压缩行为和会话清理不再是后端细节。缺乏透明核算的工具将在 CI 和团队计费场景中举步维艰。

3. **权限正从“允许/拒绝”转向“带恢复的策略”。** 市场需要显式沙箱、仓库外写入保护、网络策略、按服务器配置的 TLS 信任和默认拒绝自动化——但也需要可操作的拒绝消息和覆盖路径。

4. **非交互与 ACP 使用正超前于可靠性。** 无头配额惩罚、ACP 会话语义、远程队列和 daemon 协议保真度正成为重度自动化团队采用的阻碍。

5. **MCP/插件认证是新的集成税。** OAuth 持久化、Entra/SSO 缓存、token 交换、elicitation 渲染、生命周期超时和凭据泄漏是整个生态的共同问题。

6. **Windows 和终端/平台保真度仍然投入不足。** VS Code 启动失败、路径大小写、剪贴板/环境变量继承、终端转义序列和 VM 泄漏在 Codex、Claude Code、Copilot CLI、Pi 和 OpenCode 中均有出现。

7. **发布工程本身已成为竞争差异化因素。** bot 合并列车、nightly 渠道、CI 分片、runner 容量和发布完整性检查在 Codex、Qwen Code、Gemini CLI 和 OpenCode 中清晰可见。社区如今部分依据**修复交付的可靠性**来评判工具。

**对开发者的参考价值：** 在 CI 中固定版本，监控各入口的配额行为，限制会话/压缩存储，独立验证子代理成功状态，优先使用显式权限允许列表，测试 Windows 和远程/ACP 路径，并将 MCP 认证/token 生命周期视为生产依赖，而非安装细节。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills —— 社区亮点报告
**来源：** github.com/anthropics/skills · **数据截至：** 2026-10-30

> **数据说明：** 全部 50 个 PR 在导出中均报告 `Comments: undefined` 和 `👍: 0`。因此下方排名遵循源排序（该列表已按评论数预排序），并结合更新时间、交叉引用的 issue 以及重复贡献者信号进行交叉验证。Issue 评论数可用，并直接使用。

---

## 1. 热门 Skills 排名（讨论最多的 PR）

**#1 — [PR #1742](https://github.com/anthropics/skills/pull/1742) — `mcp-builder`：MCP v2 兼容性**
`fix(mcp-builder): support mcp>=2 streamable_http_client import and custom headers` · @Kuldeeep18 · 9 月 8 日 → 10 月 8 日
- **功能：** 在 `mcp>=2.0.0` 破坏性变更后修复 `skills/mcp-builder/scripts/connections.py`——`streamablehttp_client` → `streamable_http_client`，且自定义 header 现在需要使用 `create_mcp_http_client`/`http_client`，而非直接传 kwarg。
- **亮点：** 关闭 #1668；纯粹的 upstream-SDK 破坏性变更修复，因此会阻塞所有基于当前 `mcp` 版本构建 MCP server 的用户。
- **状态：** OPEN（草稿质量级别的修复，等待维护者审查）。

**#2 — [PR #1298](https://github.com/anthropics/skills/pull/1298) — `skill-creator`：触发评估正确性**
`fix(skill-creator): isolate trigger evals and handle Windows and runtime failures` · @MartinCajiao · 6 月 10 日 → 9 月 16 日
- **功能：** 修复触发评估中的误判漏检与无效评分：多个 worker 的命令探测相互竞争、Windows 上子进程管道的 `select()` 失败、无关工具中断扫描，以及运行时失败被误判为"未触发"（从而静默通过负样本）。
- **亮点：** 本批中历时最久、结构最复杂的修复；直接针对评估的*有效性*，而不仅仅是崩溃问题。开放四个月表明审查负担不轻。
- **状态：** OPEN。

**#3 — [PR #1245](https://github.com/anthropics/skills/pull/1245) — 全新 skill 包**
`Add notion-spec-to-implementation and quantitative-resume-auditor skills` · @mrdesouzaphd-cmyk · 6 月 2 日 → 10 月 15 日
- **功能：** （a）`notion-spec-to-implementation`——将 Notion 规格页面拆解为可实施的任务，附带验收标准和进度跟踪；（b）`quantitative-resume-auditor`——第二个不相关的 skill 被打包进同一个 PR。
- **亮点：** 典型的社区提交模式（多 skill 打包），这也是主要的摩擦点——审查者通常偏好一个 PR 只包含一个 skill。
- **状态：** OPEN。

**#4 — [PR #1771](https://github.com/anthropics/skills/pull/1771) — `proofcore-contract-auditor`（Web3）**
`feat(skills): add proofcore-contract-auditor for smart contract notarization` · @ProofCore-Protocol · 9 月 15 日 → 9 月 16 日
- **功能：** 对 Solidity/Rust 合约进行静态分析，并通过零存储 Merkle 协议将密码学审计证明锚定到 TON 区块链。
- **亮点：** 厂商品牌化的 skill（公司命名空间 + 专有后端），引发了与 Issue #492 相同的信任边界问题。
- **状态：** OPEN，自创建以来无可见更新。

**#5 — [PR #1734](https://github.com/anthropics/skills/pull/1734) — `docx`：孤立批注检测**
`Detect orphaned docx comments` · @rohitjain25 · 9 月 6 日 → 9 月 25 日
- **功能：** 识别锚点文本已被删除的 Word 批注——这是生成/接受修订文档中的一类真实损坏。
- **亮点：** PR 正文中的摘要为空，这很可能是尽管问题范围明确却仍停滞的原因。
- **状态：** OPEN。

**#6 — [PR #1703](https://github.com/anthropics/skills/pull/1703) — `md2video-audio`**
`Add md2video-audio skill` · @70v-Yoyo · 9 月 1 日 → 9 月 15 日
- **功能：** 将 Markdown → Marp 幻灯片 → 带旁白的 MP4 依次编译，使用拟人化 TTS，零 API 成本。
- **亮点：** "Markdown 输入、媒体输出"流水线需求的典型代表；"零成本"的说法暗示存在需要审查的本地/离线工具依赖。
- **状态：** OPEN。

**#7 — [PR #1980](https://github.com/anthropics/skills/pull/1980) — `webapp-testing`：命令注入（CWE-78）**
`webapp-testing: avoid shell=True in with_server.py` · @Pcmhacker-piro · 10 月 6 日 → 10 月 25 日
- **功能：** 将 `subprocess.Popen(..., shell=True)`（用于支持 `cd <dir> && <command>`）替换为非 shell 启动方式，同时仍支持目录链式操作。
- **亮点：** 是榜单中少数**安全类** PR 之一，也是最近更新的；鉴于影响范围狭窄，合并可能性很高。
- **状态：** OPEN。

**#8 — [PR #1792](https://github.com/anthropics/skills/pull/1792) — `docx`：静默的 LibreOffice 超时**
`fix(docx): report LibreOffice timeout as an error and verify the output` · @TINGyu123644 · 9 月 19 日 → 9 月 25 日
- **功能：** `accept_changes.py` 在 `soffice` 超时时不再报告成功；仅在校验 `word/*.xml` 中不存在 `w:ins`/`w:del`/`w:moveFrom`/`w:moveTo` 后才声称成功。
- **亮点：** 修复了一种*虚假成功*的故障模式——这对 agent 驱动的工具而言是严重程度最高的类别，因为它会静默地破坏下游信任。
- **状态：** OPEN。

> **值得注意的结构性信号：** `skill-creator` 贡献了**前 20 个 PR 中的 3 个**（[#1298](https://github.com/anthropics/skills/pull/1298)、[#1681](https://github.com/anthropics/skills/pull/1681)、[#1997](https://github.com/anthropics/skills/pull/1997)）——它是仓库中争议最集中的单一界面。

---

## 2. 社区需求趋势（来自 Issues）

**① 信任边界与命名空间治理——最主要的 issue**
[Issue #492](https://github.com/anthropics/skills/issues/492)（46 条评论，整个数据集中互动量最高的条目）认为社区 skills 以 `anthropic/` 命名空间分发，使第三方 skills 可以冒充官方 skills 并继承提升后的权限。相关：[Issue #189](https://github.com/anthropics/skills/issues/189)（9 👍）指出 `document-skills` 和 `example-skills` 发布**完全相同的内容**，在上下文窗口中产生重复 skills。*需求：来源追溯/签名、命名空间隔离以及打包规范。*

**② 分发与组织级共享**
[Issue #228](https://github.com/anthropics/skills/issues/228)（17 条评论，8 👍）——skills 必须手动导出为 `.skill` 文件，并由每位同事重新上传；用户希望有共享的组织 skill 库或直接分享链接。*需求：skill 注册/共享原语，而非更多 skills。*

**③ 评估与触发性可靠性工具**
[Issue #556](https://github.com/anthropics/skills/issues/556)（13 条评论，7 👍）——`run_eval.py` 在所有查询上产生 **0% 的触发率**；[Issue #1390](https://github.com/anthropics/skills/issues/1390)——`mcp-builder` 的 `evaluation.py` 对每个真实 MCP server 的评分都是 0/N，因为它将 `TextContent` 序列化错误吞掉，变成伪造的工具错误；[Issue #1394](https://github.com/anthropics/skills/issues/1394)——`eval-viewer` 显示路径存在 XSS，原因是 `escapeHtml` 未做属性安全处理。*需求：在任何 skill 质量声称之前，先有一个可信赖的测量框架。*

**④ 长周期 agent 状态/记忆**
[Issue #1329](https://github.com/anthropics/skills/issues/1329)（10 条评论）——`compact-memory`，一个符号记法 skill，用于压缩 agent 自身的笔记和持久记忆。它与 [Issue #1487](https://github.com/anthropics/skills/issues/1487) 中的 token 膨胀抱怨互补，该 issue 指出 `claude-api` skill 在单次工具调用中急切注入**约 156k token**。*需求：将上下文经济性作为一等 skill 关注点。*

**⑤ 输出质量门禁**
[Issue #1385](https://github.com/anthropics/skills/issues/1385)（6 条评论）——三闸流水线（任务前校准 → 对抗性审查 → 交付验证）；[Issue #412](https://github.com/anthropics/skills/issues/412)（CLOSED，6 条评论）——`agent-governance`，用于策略执行、威胁检测、信任评分、审计追踪。*需求：验证/审查类 skills，与代码审查和测试生成邻近。*

**⑥ Skill 描述质量（可触发性）与文档保真度**
[Issue #2064](https://github.com/anthropics/skills/issues/2064)——`theme-factory` 的描述说明了它*做什么*，但没说*何时使用*，因此触发不足；对比 [Issue #202](https://github.com/anthropics/skills/issues/202)（CLOSED），该 issue 认为 `skill-creator` 读起来像人类文档，而非操作指令。具体正确性 bug 进一步印证了这一点：[Issue #2018](https://github.com/anthropics/skills/issues/2018)——`docx/merge_runs.py` 写出的 XML 带有未绑定的命名空间前缀，却报告成功；[Issue #1868](https://github.com/anthropics/skills/issues/1868)——`create_gradient_background` 中的差一错误导致它永远不输出自己的 `bottom_color`。*需求：精确的、机器可执行的 skill 元数据 + 经过验证的 office 格式输出。*

---

## 3. 高潜力待合并 Skills（活跃，尚未合并）

| PR | Skill / 范围 | 最近活动 | 为何可能很快合入 |
|---|---|---|---|
| [#1975](https://github.com/anthropics/skills/pull/1975) | `pptx`：在 `thumbnail.py` 中提前拒绝 `--cols < 1` | 2026-10-27 | 窄范围修复，关闭 #1809，将渲染后的 `ValueError` 转为 argparse 错误——审查极其简单 |
| [#1980](https://github.com/anthropics/skills/pull/1980) | `webapp-testing`：移除 `shell=True`（CWE-78） | 2026-10-25 | 安全修复，diff 范围可控；安全类 PR 通常会被快速通道处理 |
| [#1681](https://github.com/anthropics/skills/pull/1681) | `skill-creator`：直接执行 `package_skill.py` + 路径文档 | 2026-10-24 | 修复硬性 `ModuleNotFoundError`；作者与 #1742 相同，即一位积极参与的贡献者 |
| [#525](https://github.com/anthropics/skills/pull/525) | 新增 `pyxel` skill——复古游戏开发（实现、无头输入运行、帧检查） | 2026-10-23 | 长期存在（自 3 月 5 日起）但仍在更新；参考集成熟；无赞助冲突 |
| [#1997](https://github.com/anthropics/skills/pull/1997) | `skill-creator`：修正 YAML frontmatter 标量解析 | 2026-10-19 | 修复描述/引号/转义损坏（这些损坏会喂给 eval + 优化命令）——与 #1298 互补 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | `algorithmic-art`：`wrapAround()` 现在真正环绕 | 2026-10-18 | 单函数取模修复，关闭 #1897 |
| [#1976](https://github.com/anthropics/skills/pull/1976) | `webapp-testing`：在 `element_discovery.py` 中正确报告 `textarea`/`select` | 2026-10-18 | 一行回退收窄，关闭 #1891 |
| [#1245](https://github.com/anthropics/skills/pull/1245) | `notion-spec-to-implementation` + `quantitative-resume-auditor` | 2026-10-15 | 雄心最大的全新包仍在活跃；需要拆分为两个 PR 才能推进 |
| [#1742](https://github.com/anthropics/skills/pull/1742) | `mcp-builder`：MCP v2 导入/header 兼容性 | 2026-10-08 | 阻塞当前 `mcp` 上所有 MCP-server skill 工作；强有力的合并候选 |

**值得标记的模式：** @gerardrecinto 贡献了一组三个与 issue 关联的微型修复（[#1975](https://github.com/anthropics/skills/pull/1975)、[#1976](https://github.com/anthropics/skills/pull/1976)、[#1977](https://github.com/anthropics/skills/pull/1977)）——每个都以单函数 diff 关闭一个编号 issue。这是待办队列中合并概率最高的队列，也是新社区成员最干净的贡献模板。

---

## 4. Skills 生态洞察

**社区的需求已从*新增* skills 转向*加固 skill 供应链*——来源与命名空间信任、包/重复清理，以及一个真正报告真相的触发评估框架——因为每个全新 skill 都继承了一条当前会误报自身结果的工具链的可信度。**

---

# Claude Code 社区摘要 — 2026-10-30

## 1. 今日亮点

新版本 **v2.1.314** 收紧了权限模型，引入 `permissions.blockWritesOutsideWorkingDirectories`，使 Write/Edit/NotebookEdit 在*所有*权限模式下均拒绝工作目录之外的路径，同时为 Claude apps 网关上游新增了 IdP 组定向功能。问题追踪器仍以成本核算异常为主导——无头模式 `claude -p` 的 token 消耗报告（**#97074**）已突破 115 👍 和 60 条评论——此外还有桌面端、VS Code 和 Windows 上 Remote Control、权限模式以及插件/MCP 可靠性问题持续不断。

---

## 2. 版本发布

**v2.1.314**
- **`permissions.blockWritesOutsideWorkingDirectories`** — Write、Edit 和 NotebookEdit 现在无论权限模式如何，都会拒绝工作目录之外的路径。这填补了一个长期存在的缺口：非默认权限模式仍可修改项目根目录之外的文件（与 #101018 和 #98044 等一批权限/自动模式投诉相关）。
- **Claude apps 网关上游的 `groups`** — 上游现在可以声明一个可选的 IdP 组列表；只有属于这些组的开发者才会被路由到该上游。适用于分阶段发布和团队范围的网关路由。
- ⚠️ 发布说明在第二条后被截断——请将此摘要视为不完整。

---

## 3. 热门问题

1. **[#97074](https://github.com/anthropics/claude-code/issues/97074)** — *无头模式 `claude -p` 每 token 消耗的 5 小时窗口配额约为交互式 CLI 的 1.8 倍*（60 条评论，115 👍）。今日互动量最高的条目。相同工作负载在 `sdk-cli` 与 `cli` 之间存在可量化的 80% 配额惩罚，这破坏了任何 CI/自动化预算规划；👍 数量表明该问题在各团队中广泛可复现。
2. **[#56089](https://github.com/anthropics/claude-code/issues/56089)** — *允许配置 Cowork VM（vhdx）磁盘包存储位置*（14 条评论，59 👍）。Windows 用户被迫将数 GB 的 VM 镜像放在 `C:` 盘；这是一个直截了当的可配置性缺口，自五月以来需求持续强劲。
3. **[#88731](https://github.com/anthropics/claude-code/issues/88731)** — *由 `claude remote-control`（服务器模式）生成的会话中缺少 Artifact 工具*（10 条评论）。值得注意的不对称性：同一台机器/同一账号通过 `claude --remote-control` 正常工作，但服务器模式路径不行——这是工具注入 bug 的强复现信号。
4. **[#86879](https://github.com/anthropics/claude-code/issues/86879)** — *支持禁用 `agent_summary` 加载动画的方式*（8 条评论，70 👍）。在子代理密集的会话中测得占**请求量的 22.7% 和加权 token 的约 18%**。👍 与评论的高比例 = 沉默而广泛的共识：装饰性加载动画文本不应消耗配额。
5. **[#82501](https://github.com/anthropics/claude-code/issues/82501)** — *无人值守会话的 `send_message` 在 2026-07-21 可用，2026-07-30 被硬性拒绝*（8 条评论）。一次无文档记录的行为反转，且无覆盖路径——对定时自动化而言是可靠性问题。
6. **[#88901](https://github.com/anthropics/claude-code/issues/88901)** — *嵌入式 Claude Code 声明了 elicitation 能力但从不渲染服务器发起的 elicitation*（7 条评论）。MCP 服务器挂起直到工具超时；能力协商对服务器撒谎是协议完整性问题，而不仅仅是 UI bug。
7. **[#100114](https://github.com/anthropics/claude-code/issues/100114)** — *Windows 桌面端：重启后 Remote Control 未恢复；会话在移动端被归档*（7 条评论）。静默自动更新 → 丢失会话状态 → 跨设备归档混乱。这是 Remote Control 测试版中一个侵蚀信任的故障模式。
8. **[#37569](https://github.com/anthropics/claude-code/issues/37569)** — *支持在 TUI 中禁用流式响应输出*（6 条评论，27 👍）。长期存在的可访问性/专注度请求；七个月后仍未解决。
9. **[#103864](https://github.com/anthropics/claude-code/issues/103864)** — *"Require trusted devices" 静默阻止代理到会话的操作，无设备端批准覆盖*（4 条评论）。`create_session`、`list_events`、`send_message` 全部被拒绝且无恢复路径——一个安全功能变成了可用性故障。
10. **[#98044](https://github.com/anthropics/claude-code/issues/98044)** — *自 2.1.280 起，当 `CLAUDE_CONFIG_DIR` 的 `projects/` 为符号链接时，自动记忆写入每次都弹出提示*（2 条评论，有复现步骤）。允许规则、钩子和自动模式均无法批准——这是一个惩罚多配置方案的回归。

*同样值得关注：* **[#105894](https://github.com/anthropics/claude-code/issues/105894)** — 一条从未被记录的用户消息出现在了模型上下文中（潜在的上下文完整性问题），以及 **[#106085](https://github.com/anthropics/claude-code/issues/106085)** — 空闲会话各自对本地目录市场执行 `git ls-remote --tags`，每个会话消耗 1.3–1.8 个 CPU 核心。

---

## 4. 关键 PR 进展

⚠️ **过去 24 小时内仅有 2 个拉取请求被更新**——相对于问题数量，仓库的 PR 吞吐量非常低（2 个 PR 对 50 个更新的问题），这本身就是一个值得关注的信号。

1. **[#32751](https://github.com/anthropics/claude-code/pull/32751)** — `fix(hookify): map stop/prompt events to correct field names`（OPEN，更新于 2026-10-29）。修复 #32153：使用简单 `pattern` 语法且 `event: stop` 或 `event: prompt` 的 Hookify 规则从不触发，因为 `config_loader.py` 将它们映射到 `field='content'`，而 `rule_engine._extract_field()` 无法为 Stop/UserPromptSubmit 载荷解析该字段。这是一个静默失败 bug——钩子根本不运行，且不产生任何错误提示。
2. **[#57333](https://github.com/anthropics/claude-code/pull/57333)** — `Update README.md`（CLOSED，更新于 2026-10-29）。仅文档变更；无功能影响。

*要点：* 社区提交 bug 的速度远超外部 PR 的落地速度。Hookify（#32751）是唯一在途的实质性社区修复，且自三月起一直处于打开状态。

---

## 5. 热门讨论

略——本数据集中未提供 Discussions 数据。

---

## 6. 功能请求趋势

从 30 个已呈现的问题中提炼：

| 方向 | 代表性问题 | 信号 |
|---|---|---|
| **成本与配额透明度/控制** | #97074, #86879 | 数据集中互动量最高。用户希望各入口点的核算一致，并能够关闭消耗 token 的"装饰"（加载动画、摘要）。 |
| **细粒度权限与安全控制** | #103864, #101018, #98044, v2.1.314 | 需求是显式覆盖而非仅仅限制：设备端批准、从移动端/网页端覆盖自动模式拒绝、以及符号链接配置目录下的正确行为。 |
| **桌面端/Cowork 可配置性与稳定性** | #56089, #100114, #91265, #94536 | 存储位置控制、Remote Control 状态跨重启持久化、VM 资源泄漏、以及 Code 标签页的输入流回归。 |
| **TUI 与 IDE 人体工学** | #37569, #16078, #92108, #97535 | 禁用流式输出、在状态栏显示活跃技能、`/diff` 覆盖 `--add-dir` 路径、以及 VS Code 的"添加文件到聊天"命令。小需求，长期存在。 |
| **插件 / MCP 生命周期健壮性** | #95683, #106087, #106085 | `.mcpb` 的 `user_config` 在市场安装时弹出提示、启动失败后重试、以及停止市场 git 轮询导致空闲会话空转。 |
| **Remote Control 跨界面一致性** | #88731, #96352, #100114, #101018 | 测试版在 CLI、桌面端、VS Code Remote-SSH 和移动端之间持续存在差异——重复的 `--resume` 实例、缺失的工具、被归档的会话。 |

---

## 7. 开发者痛点

1. **静默的、未测量的成本乘数。** 两个最高赞问题（#97074、#86879）都描述了用户不可见的 token/配额开销——1.8 倍的无头模式惩罚和约占加权 token 18% 的加载动画。当开销无文档记录且因入口点而异时，开发者无法为 CI 或子代理工作负载做预算。
2. **权限模式在两个方向上都会不可预测地失败。** 用户报告自动模式*拒绝*明确批准的低风险编辑（#101018）、在 "Require trusted devices" 下*完全阻止*代理（#103864）、以及当配置目录为符号链接时*每次都弹出提示*（#98044）。`defaultMode` / 允许规则 / 钩子的心智模型不可组合。
3. **Remote Control 是最不可靠的界面。** 服务器模式缺少 Artifact 工具（#88731）、VS Code Remote-SSH 上重复的 `--resume` 进程（#96352）、以及 Windows 桌面端重启后状态丢失（#100114）——都指向会话身份处理是根本弱点。
4. **空闲资源消耗。** 每个空闲会话 1.3–1.8 核心的 git 轮询循环（#106085）、Cowork VM 导致的 Windows 内核池持续泄漏（#91265）、以及 Linux 上的孤儿进程/休眠抑制（#89110）。后台会话池会线性放大这三个问题。
5. **无恢复路径的故障模式。** MCP 服务器在启动失败后无法自行恢复——15 分钟的失败缓存随后阻止所有新会话（#106087）。Elicitation 请求挂起直到工具超时（#88901）。来自定时任务的 `send_message` 被静默硬性拒绝（§#82501）。在每种情况下，开发者都无计可施。
6. **版本间无文档记录的行为反转。** 2026-07-21 → 07-30 的 `send_message` 变更和 2.1.280 的符号链接回归均未附带迁移说明，迫使团队防御性地锁定版本。
7. **问题量与修复量的错配。** 24 小时内更新了 50 个问题，而 PR 仅有 2 个，其中一个是 README 编辑。Hookify 事件映射修复（#32751）自三月起一直开放——钩子作者正在发布永远不会触发的规则。

---
*基于 github.com/anthropics/claude-code 2026-10-30 的数据生成。发布说明和 PR 摘要在源数据中被部分截断。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-10-30

## 1. 今日亮点

Codex 发布了 **rust-v0.166.0**，主打键盘驱动的对话记录选择功能，并集成搜索与可配置快捷键——一个长期存在的 UX 缺口终于被补上。与此同时，Issue 跟踪器被**压缩期间反复重新持久化图片/base64 所导致的会话存储爆炸**所占据，多份报告达到 16–165 GiB 区间；其中一个重要的多智能体 V2 变体（#34268）已关闭。Windows 仍是最不稳定的平台，互动量最高的 Issue 是 VS Code 扩展 UI 启动失败（42 条评论，39 👍）。

## 2. 版本发布

**rust-v0.166.0** ([发布](https://github.com/openai/codex/releases))
- **对话记录导航**：支持键盘驱动的选择，包括按词、按行、按页移动，集成搜索，以及可配置快捷键（#57221–#57224）。这直接回应了长期存在的“无法搜索聊天内容”抱怨（见 Discussion #8582）。
- **Markdown 一致性**：在异步提问、选项、回答和对话记录回复中统一渲染（#57259、#57292）。
- **模型更新**：发布说明提到新增了一个“GPT…”模型，但所提供的变更日志被截断——在完整说明可用之前，应将模型公告视为未经确认。

## 3. 热门 Issue

1. **[#56691](https://github.com/openai/codex/issues/56691) — Codex 26.1021.22027 在 Windows 上的 VS Code 1.143.1 中无法启动 UI**（42 条评论，39 👍）。当日互动量最高的 Issue：扩展能加载，但不渲染任何 UI。VS Code 1.144.0 上一份几乎重复的报告（#58150）表明，这更像是更广泛的 Windows/Electron 启动回归，而不是单次安装损坏。
2. **[#45403](https://github.com/openai/codex/issues/45403) — Windows “完全访问”清理被以含糊的“被策略阻止”拒绝，且没有复核路径**（28 条评论，6 👍）。记录了一种破坏信任的不一致：权限 UI 承诺不受限制的文件访问，但沙箱策略却静默否决一个已明确授权的 `Remove-Item`。且没有提供补救入口。
3. **[#26633](https://github.com/openai/codex/issues/26633) — 桌面自动化在 RRULE 调度中忽略时区**（26 条评论，8 👍）。每周调度执行时把 `BYHOUR` 当作 UTC；显式指定 `DTSTART;TZID=Europe/Paris` 也没有效果。对于依赖定时智能体运行的用户影响很大。
4. **[#48500](https://github.com/openai/codex/issues/48500) — 托管的 app-server 钩子使用首个客户端的 TMUX_PANE 运行（0.157 回归）**（21 条评论，25 👍）。自 0.157 起，生命周期钩子在共享守护进程内执行，而该进程继承了启动它的 TUI 环境，因此钩子事件被归因到错误的终端窗格。较高的点赞比例表明多窗格/多会话使用广泛。
5. **[#34268](https://github.com/openai/codex/issues/34268) — [已关闭] 多智能体 V2 全历史分叉会复制压缩快照和图片，导致会话增长超过 100 GiB**（17 条评论，7 👍）。这是存储膨胀问题族的典型记录；关闭是一个积极信号，表明乘法式分叉路径已经得到处理。
6. **[#35458](https://github.com/openai/codex/issues/35458) — 截图在每次压缩时被重新持久化，并被 subagent 分叉继承，约 165 GiB（95% 为 base64 图片）**（16 条评论）。这是报告中最极端的案例；说明主要存储开销来自图片，而不是文本。
7. **[#36596](https://github.com/openai/codex/issues/36596) — 尽管明确要求“持续运行直到需要用户输入”，Codex 仍会终止活跃的自主工作**（16 条评论，6 👍）。这是关于模型行为/智能体控制的抱怨：长时间运行任务过早停止，削弱了无人值守工作流。
8. **[#41708](https://github.com/openai/codex/issues/41708) — Android Remote 消息队列可能在宿主机回合完成后仍保持卡住**（13 条评论，3 👍）。消息进入可见队列，但从未到达 Windows 宿主机；移动客户端显示过期的停止控件。移动端连续性的好坏取决于其恢复路径。
9. **[#35485](https://github.com/openai/codex/issues/35485) — Windows：捆绑的 `node_repl` MCP 进程按线程泄漏，直到 app-server 退出**（10 条评论）。一种缓慢的资源泄漏，会随线程数累积——与长期运行的桌面会话相关。
10. **[#58115](https://github.com/openai/codex/issues/58115) — 沙箱/rmcp-client：Seatbelt glob 子树后缀被遗漏；`[::1]` OAuth 回调绑定到 `0.0.0.0`**（1 条评论）。讨论量低但严重性高：三个不同的安全边界边缘案例，包括一个监听所有接口的 OAuth 回调。值得立即分诊。

*另需关注：* #55461（反复发送失败、超时、UI 变黑）、#55659（CUA 执行后暂停 iframe Document 响应）、#50309（`auto_review.extra_policy` 被静默丢弃）、#56860（Linux 泄漏约 24.6 GB 的 marketplace 暂存目录，约 10 GB/小时）、#43618（Linux 上已完成回合折叠隐藏回复，附根因与补丁）。

## 4. 关键 PR 进展

今天几乎所有 PR 活动都来自一条高速、由机器人（`copyberry[bot]`）创作的合并列车——值得将其视为自动化发布工程的信号，而非个人贡献。

1. **[#58202](https://github.com/openai/codex/pull/58202) — 在模型提供商变更时保持聊天可访问。** 防止历史记录从恢复查找中消失，或当服务器因提供商不匹配、配置缺失或策略冲突而拒绝恢复时无法打开。
2. **[#58196](https://github.com/openai/codex/pull/58196) — 要求 Bazel 发布产物并发布 DotSlash 清单。** 移除 Bazel `continue-on-error`，使 Bazel 构建/签名/打包成为发布的阻塞项——面向桌面消费者强化发布完整性的一步。
3. **[#58182](https://github.com/openai/codex/pull/58182) — 在 `permissions.network` 下支持托管网络要求。** 将 `requirements.toml` 从文件系统约束扩展到网络约束，并通过配置加载、网络审批和 Windows 沙箱策略校验来应用。
4. **[#58165](https://github.com/openai/codex/pull/58165) — 避免远程插件包 URL 进入诊断信息。** 防止下载 URL 查询参数中的签名令牌泄漏到错误和调试输出中。改动虽小，但对凭证卫生有实际影响。
5. **[#58161](https://github.com/openai/codex/pull/58161) — 在 MCP 审批元数据中包含工具名称和注解提示。** 在审批请求中加入 `tool_name` 以及 `readOnlyHint`/`destructiveHint`/`idempotentHint`/`openWorldHint`，使 MCP 工具的同意 UX 更具信息依据。
6. **[#58159](https://github.com/openai/codex/pull/58159) — 为 code-mode 宿主输出截断添加功能开关。** 默认关闭的 `code_mode_host_output_truncation` 会通过 stdio 和 gRPC 请求服务端截断，注入的 provider 不受影响。
7. **[#58157](https://github.com/openai/codex/pull/58157) — 将系统代理支持作为实验性设置公开。** 在 macOS/Windows 的 `/experimental` 中加入“System proxy”，保持 `respect_system_proxy` 默认关闭，并更新 doctor 指引——为企业网络用户提供实际可行的解除阻塞方案。
8. **[#58147](https://github.com/openai/codex/pull/58147) — 将独立 Web 搜索失败返回给模型。** 将导致回合中止的致命错误转换为不成功的工具输出，使模型可以推理并从中恢复。
9. **[#58140](https://github.com/openai/codex/pull/58140) — 让独立 Web 搜索经由线程的工作区上下文路由。** 修复搜索绕过工作区路由、命中 bootstrap 后端而非已发现工作区后端的问题。
10. **[#58168](https://github.com/openai/codex/pull/58168) — 在 Windows 上为 MXC 拒绝路径创建缺失的父目录。** MXC 要求拒绝路径的父目录存在，即使路径本身不存在；这可防止策略展开失败。

*另已合并：* #58198（集中式 `ConfigToml` TOML 反序列化）、#58166（显式记录 rollout 失败/完成）、#58172（跟踪完成 + 重放时负载 UTF-8 校验）、#58195（远程读取与旧版执行器的兼容性）、#58190（自定义 provider 可选择加入 Cyber 访问计划）、#58144（附加上下文计数/大小/截断指标）、#58130（skills 搜索中的粘贴文本）。

## 5. 热门讨论

**想法**
- **[#57967](https://github.com/openai/codex/discussions/57967) — 每月为 ChatGPT Plus 提供 1–2 次免费 Codex 用量重置。** 请求将偶发的促销重置转变为可预测的月度配额。所要求的重点是可预测性，而非原始配额。

**问答**
- **[#8582](https://github.com/openai/codex/discussions/8582) — 在当前聊天历史中搜索/查找**（2 条评论，4 👍）。`Ctrl-F` 总是作用于编辑器，且不存在上下文菜单搜索。值得注意的是，v0.166.0 的对话记录搜索正是对该帖的直接回应。
- **[#5210](https://github.com/openai/codex/discussions/5210) — 有办法清空文本区域吗？** 一个长期存在（Oct 2025）的小型 UX 缺口：没有快捷键可以清空误粘贴的大段提示词。

**展示与分享**
- **[#53803](https://github.com/openai/codex/discussions/53803) — swe-mux：运行多个 Codex 会话并查看实时状态，还能在手机上访问相同会话。** 在真实终端中运行真正的 `codex` CLI，不做代理；展示工作/完成/等待状态，并将会话镜像到移动端。
- **[#58093](https://github.com/openai/codex/discussions/58093) — OpenFusion：用于子任务处理的编排器/路由器。** 将子任务路由到成本最低且能力足够的模型，并由评审器在尝试效果不佳时升级；附带基准测试。
- **[#58104](https://github.com/openai/codex/discussions/58104) — Kilobat：面向 AI 智能体与人类的网络工具。** 一款用于网络监控/诊断的 Mac 菜单栏应用，另提供免费 OSS CLI + MCP，使智能体可以检查流量；可跟踪 Codex、Claude Code 和 Gemini 的使用情况。
- **[#57746](https://github.com/openai/codex/discussions/57746) — Zozo：可检查的从 Issue 到 PR 示例与审查清单。** 针对待办事项协调编码运行时；分享一个公开的 Issue/diff/CI 日志示例以及审查检查项。
- **[#58031](https://github.com/openai/codex/discussions/58031) — 结合当前阿里云文档与官方引用使用 Codex。** 一篇从业者笔记，讲述如何核验云相关事实（端点、地域、模型名称），而不是直接信任生成输出。
- **[#53552](https://github.com/openai/codex/discussions/53552) — NOS：粘贴 HTML 页面，在屏幕上修改措辞，并以链接分享。** 在渲染后的页面中直接编辑，并往返同步源码。
- **[#51338](https://github.com/openai/codex/discussions/51338) — Brok：AI 助手到底应该做什么才能在开发工作流中有用？** 一场超越聊天界面模型的设计讨论。

## 6. 功能请求趋势

- **对话记录内搜索与导航。** Discussion #8582 明确要求聊天历史搜索；v0.166.0 的键盘选择 + 集成搜索就是直接回应。预计后续会围绕可发现性和默认键位展开。
- **可预测的用量经济性。** #57967 请求按计划每月重置，而不是临时促销——这是从“更多配额”向“稳定、可规划配额”的转变。
- **会话生命周期与存储管理。** 最大的一类 Issue 暗示了功能缺口：没有面向用户的工具来检查、清理或核对会话数据。#55439 请求提供一个安全、非破坏性的核对命令，用于处理重复 rollout / 缺失的数据库行——实际上相当于 `codex doctor --fix`。
- **多会话编排与外部可观测性。** 展示与分享类项目（#53803、#58093、#58104）都围绕并发运行多个智能体并了解哪一个需要关注——核心产品尚未原生提供这一能力。
- **企业/网络可配置性。** #58182（托管网络策略）和 #58157（系统代理选择加入）反映了对集中管理、符合策略的部署的需求。
- **移动端/远程连续性。** #41708 和 #53803 都瞄准“在手机上访问相同会话”，这表明远程访问在实际使用中的普及速度快于其可靠性加固速度。

## 7. 开发者痛点

- **会话存储爆炸（最高严重性的反复主题）。** 七份重叠报告——#34268（110 GiB）、#35458（~165 GiB，95% 为 base64 图片）、#39013（16.9 GB）、#33735（24 GB）、#40323（16 GiB）、#23257、#28916——描述了同一机制：压缩检查点重新嵌入完整图片，然后多智能体分叉以乘法方式继承它们。后果不止于磁盘：#28916 报告在图片密集的历史记录中出现输入延迟和持续转圈。关闭 #34268 是进展，但仍有大量开放重复项，说明用户并不确信它已完全修复。
- **Windows 是薄弱平台。** VS Code 中 UI 启动失败（#56691、#58150）、“完全访问”下含糊的沙箱拒绝（#45403）、MCP 进程泄漏（#35485）、工作区设置加载失败（#54168），以及过期的自动审查范围（#58080）。Windows 用户同时遇到正确性和可诊断性问题。
- **不透明且无救济途径的策略决策。** #45403 和 #58080 有共同模式：智能体被策略阻止，消息不可操作，且不存在复核/审批路径。相比阻止本身，这更会侵蚀对权限模型的信任。
- **共享守护进程中的环境错误归因。** #48500（从首个客户端继承 TMUX_PANE）是一类会反复出现的 bug，随着托管 app-server 成为默认多路复用层，这类问题还会再现。
- **自主工作过早终止。** #36596：明确的“持续运行直到需要用户输入”指令未被遵守，限制了长时间无人值守运行的价值。
- **连接性与队列可靠性。** #55461（发送失败、超时、UI 变黑）和 #41708（Android 队列卡住且停止控件过期）表明恢复路径比正常路径更脆弱。
- **小而持续的 UX 缺口。** 没有清空输入的快捷键（#5210）；直到 v0.166.0 才有聊天内搜索（#8582）；Linux 上折叠回合隐藏回复（#43618）；缩放时图片灯箱不适配窗口（#26851）；以及 macOS Pet 浮层不吞输入且无法移动（#45397）。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区摘要 — 2026-10-30

## 1. 今日亮点

新的 nightly 构建（`v0.67.0-nightly.20261030.gb633c0703`）已发布，包含 README 范围澄清，以及一项修复终端尺寸变化后内联历史重新渲染的 CLI 修复。活动仍集中在 agent/subagent 可靠性上：排名第一的 Issue #22323 显示，子代理在实际耗尽 `MAX_TURNS` 后仍报告 `status: "success"` / `Termination Reason: "GOAL"`。安全与正确性相关 PR 也有所推进，包括 checkpoint fallback 中的路径遍历修复，以及 MCP 发现超时限制。

## 2. 发布

**v0.67.0-nightly.20261030.gb633c0703** ([发布](https://github.com/google-gemini/gemini-cli/releases))
- `docs(readme)`：澄清 Gemini CLI 的范围、限制与工具生态（[#29995](https://github.com/google-gemini/gemini-cli/pull/29995)）。
- `fix(cli)`：限制终端宽度变化后的内联历史重新渲染（feed 中的 PR 引用被截断）。
- 版本号提升 PR：[#30045](https://github.com/google-gemini/gemini-cli/pull/30045)（自动化 nightly 版本提升）。

## 3. 热门 Issue

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — 子代理 MAX_TURNS 恢复被误报为 GOAL 成功**（P1，13 条评论，👍2）。`codebase_investigator` 在尚未进行任何分析前就触及轮次上限，却仍返回 `success`/`GOAL`。为何重要：这会静默掩盖代理失败，并破坏下游对子代理结果的信任——这是当天互动量最高的讨论串。
2. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — 通用代理无限期挂起**（P1，8 条评论，👍8）。创建文件夹这类简单任务会卡住一小时；指示模型不要委派即可绕过该问题。社区大量点赞表明影响广泛。
3. **[#28004](https://github.com/google-gemini/gemini-cli/issues/28004) — 已完成的 shell 调用产生重复工具结果**（P2，9 条评论）。可在兼容 Gemini 的提供商环境中复现；重复提交 `functionResponse` 可能破坏对话状态。
4. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — 零依赖 OS 沙箱 + 执行后意图路由**（P2，9 条评论）。提议借助 Gemini 3 对 bash 的原生亲和性（`grep`/`sed`/`awk`），同时不牺牲沙箱隔离——这是代理运行时在设计层面的一个方向。
5. **[#28416](https://github.com/google-gemini/gemini-cli/issues/28416) — GlobTool 返回原始符号链接路径，但内部会解析**（P2，7 条评论）。这种不一致会导致 macOS 及符号链接工作区中的下游 `read_file`/编辑操作失败。
6. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini 很少在无提示时调用技能或子代理**（P2，7 条评论）。虽有轶事色彩但反复出现：显式指令有效，隐式路由无效。
7. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — 评估 AST 感知的文件读取、搜索与代码库映射**（P2，7 条评论）。该 Epic 探索减少错位读取、缩小 token 占用以及改进导航。
8. **[#28999](https://github.com/google-gemini/gemini-cli/issues/28999) — macOS Seatbelt 配置遗漏 Podman API 套接字**（安全，1 条评论）。容器运行时拒绝列表（`podman.sock`、`~/.podman`）中的缺口，留下了通过特权容器逃逸沙箱的途径。
9. **[#29036](https://github.com/google-gemini/gemini-cli/issues/29036) — 并发扩展安装会相互破坏**（2 条评论）。`installOrUpdateExtension()` 先检查是否存在，然后在写入前等待；并行安装或 `update --all` 可能发生竞态并覆盖目标位置。
10. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — 代理应劝阻破坏性行为**（P2，3 条评论，👍1）。在存在更安全路径时，模型仍会动用 `git reset` / `--force`——这与自动化工作流中的信任和安全相关。

## 4. 关键 PR 进展

1. **[#29876](https://github.com/google-gemini/gemini-cli/pull/29876) — 防止旧版 checkpoint 回退中的路径遍历**（P1，安全，开放中）。修复在使用未编码的 `checkpoint-${tag}.json` 名称时，`Logger._getCheckpointPath()` / `deleteCheckpoint()` 中的遍历问题。
2. **[#29767](https://github.com/google-gemini/gemini-cli/pull/29767) — 为 MCP `tools/list` 发现设置 60 秒默认超时上限**（P1，已关闭）。此前发现过程会继承 10 分钟的工具调用超时，导致无响应服务器静默卡住。
3. **[#29765](https://github.com/google-gemini/gemini-cli/pull/29765) — 使 `FileKeychain` 写入具备原子性并串行化**（安全，已关闭）。防止并发写入导致凭据静默丢失，以及写入中断导致锁定。
4. **[#29875](https://github.com/google-gemini/gemini-cli/pull/29875) — 添加 `gemini-4-argon` 模型支持**（开放中）。在 `VALID_GEMINI_MODELS` / `defaultModelConfigs` 中注册该模型，并添加 `isGemini4Model` 能力检查。
5. **[#29879](https://github.com/google-gemini/gemini-cli/pull/29879) — 在流中途出错或重试时丢弃缓冲的工具调用**（开放中）。防止在 `Retry`、`UserCancelled`、`LoopDetected` 和 `ContextWindowWillOverflow` 过程中产生孤立的 `ToolCallRequest` 事件。
6. **[#29881](https://github.com/google-gemini/gemini-cli/pull/29881) — ACP 重试进度 + 重试延迟上限 + 模型回退**（开放中）。修复 `--acp` 模式下静默的 429 重试，包括将多小时的配额重置消息误判为瞬态容量错误的问题。
7. **[#30044](https://github.com/google-gemini/gemini-cli/pull/30044) — `readSecureFileBuffer` 中 Windows libuv `dev: 0` stat 处理**（开放中）。影响捆绑 libuv 1.49.x–1.50.0 的 Node 22.12.0–22.16.x，在这些版本中基于路径的 stat 会报告 dev: 0。
8. **[#30043](https://github.com/google-gemini/gemini-cli/pull/30043) — 向 `BeforeTool` / `AfterTool` 钩子暴露工具调用 ID**（开放中）。使集成能够配对前后钩子事件；该字段保持可选。
9. **[#29878](https://github.com/google-gemini/gemini-cli/pull/29878) — 报告被阻止的路径段，而不是误导性的“路径在工作区之外”**（开放中）。提升 `Config.validatePathAccess` 失败的可诊断性。
10. **[#29882](https://github.com/google-gemini/gemini-cli/pull/29882) — 在查询变更时保留文件搜索结果**（开放中）。结果缓存可能会隐藏有效匹配（例如先 `Src` 后 `Src*`）；现在缓存仅适用于完全相同的查询。

## 5. 热门讨论

本期未提供 Discussions 数据，因此省略本节。

## 6. 功能请求趋势

- **子代理可观测性与可调试性**：通过 `/chat share` 暴露子代理轨迹（[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)），在 `/bug` 报告中包含子代理上下文（[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)），以及以 sprint 跟踪子代理工作（[#20195](https://github.com/google-gemini/gemini-cli/issues/20195)）。
- **AST 感知 / token 节约型工具**：AST 读取与代码库映射（[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)、[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)）以及“有策略的提取”以减少约 36.6k token/轮的基线（[#19561](https://github.com/google-gemini/gemini-cli/issues/19561)）。
- **持久化任务跟踪**：用基于文件的 CRUD 替换上下文内的 `WriteToDo`，以避免上下文腐化（[#18836](https://github.com/google-gemini/gemini-cli/issues/18836)、[#21000](https://github.com/google-gemini/gemini-cli/issues/21000)）。
- **沙箱与安全**：零依赖 OS 沙箱（[#19873](https://github.com/google-gemini/gemini-cli/issues/19873)）以及劝阻破坏性 git 操作（[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)）。
- **浏览器代理韧性**：遵循 `settings.json` 覆盖（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)）、会话接管 / 锁恢复（[#22232](https://github.com/google-gemini/gemini-cli/issues/22232)）以及 Wayland 支持（[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)）。
- **代理自我认知**：准确的 CLI 标志、快捷键与自执行指导（[#21432](https://github.com/google-gemini/gemini-cli/issues/21432)）。

## 7. 开发者痛点

- **子代理可靠性是主要挫败来源**：无限期挂起（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）、轮次上限耗尽时误报成功（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)）、配置被忽略（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)），以及技能/子代理自发使用率低（[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)）。
- **路径与文件系统边界情况**：符号链接的代理定义未被发现（[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)）、glob/realpath 分歧破坏编辑（[#28416](https://github.com/google-gemini/gemini-cli/issues/28416)），以及令人困惑的“工作区之外”错误（[#29878](https://github.com/google-gemini/gemini-cli/pull/29878)）。
- **工具编排脆弱性**：重复工具结果（[#28004](https://github.com/google-gemini/gemini-cli/issues/28004)）、超过工具数量上限后的 400 错误（[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)），以及卡在 `vite` 等交互式提示上（[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)）。
- **CLI 生命周期崩溃**：输出时由钩子驱动的崩溃（[#22186](https://github.com/google-gemini/gemini-cli/issues/22186)）以及高内存 / GC 耗尽（[#28829](https://github.com/google-gemini/gemini-cli/issues/28829)）。
- **工作区整洁性**：模型将临时编辑脚本散落到各个目录（[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)），使干净提交复杂化。
- **上手摩擦**：个人账户认证迁移失败会破坏登录流程（[#28745](https://github.com/google-gemini/gemini-cli/issues/28745)）。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — 社区摘要
**日期：** 2026-10-30 · **来源：** [github/copilot-cli](https://github.com/github/copilot-cli)

## 今日亮点

过去 24 小时内没有新发布或 Pull Request 落地，但 Issue 流量依然很高（38 个 Issue 有更新），社区集中在 **代理会话稳定性**（长时间并行代理运行中 37 GiB 内存增长、300s 压缩超时）和 **ACP/MCP 认证回归**（每次会话都重新打开 OAuth 页面、Entra token 缓存不持久）。一组与 ACP 相关的报告——`session/prompt` 在异步 bash 上阻塞、缺少 `contextTier` 配置、无条件中止会话——表明非交互/ACP 接口面现在是主要摩擦来源。

## 发布

过去 24 小时内未发布任何版本。

## 热点 Issue

1. **[#552 — Failed to list models: 403 unauthorized](https://github.com/github/copilot-cli/issues/552)** · OPEN · 31 条评论 · 👍4
   数据集中开放时间最长的讨论帖（自 2025-11-13 开放，今天仍有更新）。已“启用 Copilot CLI”的组织中的用户，在 0.0.355+ 上列出模型时仍会收到 `403 unauthorized`。高评论数表明这是持续的权益/策略映射问题，而非孤立的配置错误。

2. **[#4802 — PRU quota wiped out, likely tied to Assisted Permissions](https://github.com/github/copilot-cli/issues/4802)** · OPEN · 8 条评论
   一位 Copilot Pro（旧版基于请求的 PRU）订阅用户报告，配额耗尽与启用 assisted permissions 相关。计费相关 bug 会受到格外关注——社区需要明确代理驱动的工具调用是否会以不同方式消耗 PRU。

3. **[#5254 — 1.0.103: main process grows to 37 GiB during long parallel-agent session](https://github.com/github/copilot-cli/issues/5254)** · OPEN · 2 条评论
   在 32 GiB Mac 上运行约 6h19m 后，物理占用达 37.3 GiB，有 133M 次 malloc 分配和 10.7 GiB 碎片。这是长生命周期、多子代理会话中内存管理缺陷迄今最清晰的信号——对无人值守/CI 式使用是硬性阻塞。

4. **[#4994 — Regression (1.0.88 → 1.0.89): MCP OAuth page opens on every ACP session](https://github.com/github/copilot-cli/issues/4994)** · OPEN · 5 条评论 · 👍8
   每次新建 `--acp` 会话时，`~/.copilot/mcp-oauth-config` 中有效且未过期的 token 被忽略，迫使重复进行浏览器 SSO。今日集合中反应最高的开放 bug；直接破坏无头 ACP 自动化。配套的已关闭 Issue [#5229](https://github.com/github/copilot-cli/issues/5229) 覆盖 token *过期* 时的相同根因（未尝试 refresh_token grant）。

5. **[#5292 — Automatic compaction repeatedly times out at 300s until hitting context limit](https://github.com/github/copilot-cli/issues/5292)** · OPEN (triage) · new
   一旦摘要器超时，CLI 在硬性上下文限制拒绝之前没有恢复路径。这把短暂的延迟波动变成会话死亡——这是可靠性缺口，而非调优问题。

6. **[#5293 — ACP: `session/prompt` response held until async bash shell exits](https://github.com/github/copilot-cli/issues/5293)** · OPEN (triage) · new
   前台工作已完成，但当 `async` bash 工具（开发服务器、watcher）继续运行时，响应被扣留，因此客户端看起来像卡住。典型的协议语义不匹配，会坑到每个集成长时间运行进程的 ACP 客户端。

7. **[#5289 — `copilot --cloud` fails immediately during provisioning (internal server error)](https://github.com/github/copilot-cli/issues/5289)** · OPEN (triage) · 1.0.105
   远程云会话在环境预配阶段失败，在代理主机连接前返回不透明的 500。旗舰工作流上的不透明服务端故障，是用户最难自行诊断的一类 bug。

8. **[#4275 — ACP: expose `contextTier` as a session config option](https://github.com/github/copilot-cli/issues/4275)** · OPEN · 7 条评论 · 👍4
   交互式 `/model` 选择器可以在会话中途更改上下文层级；ACP 客户端只能在启动时设置。来自第三方客户端厂商 GitKraken 的精确、范围明确的同能请求——代表 ACP 功能对等需求。

9. **[#5275 — Support default-deny tool permissions with explicit allowlist in non-interactive mode](https://github.com/github/copilot-cli/issues/5275)** · OPEN · 1 条评论
   `--deny-tool` 总是覆盖 `--allow-tool`，再加上默认允许的工具，导致 CI 中无法实现严格沙箱。对任何无人值守运行代理式 CLI 工作负载的人都具有安全相关性。

10. **[#4613 — CLOSED: MCP schemas eagerly injected, +354K startup tokens](https://github.com/github/copilot-cli/issues/4613)** · CLOSED · 3 条评论
    一个高严重性的 1.0.80 系列回归：即使对于琐碎提示，完整的环境 MCP 目录也会被注入首次模型请求。今天已关闭——对 MCP 密集型设置的 token 成本和延迟是好消息，值得在你自己的配置中验证。

## 关键 PR 进展

过去 24 小时内没有 Pull Request 更新（0 项）。没有正在进行的修复可报告，因此上述所有条目仍仅通过 Issue 跟踪。

## 热门讨论

本期未提供讨论数据。

## 功能请求趋势

- **ACP / 非交互功能对等是主导主题。** 请求一致要求 ACP 匹配交互式 TUI 能力：`contextTier` 会话选项（[#4275](https://github.com/github/copilot-cli/issues/4275)）、`ask_user` / `ask_question` 扩展方法（[#2109](https://github.com/github/copilot-cli/issues/2109)），以及后台子代理的非中止 `session/prompt` 语义（[#4555](https://github.com/github/copilot-cli/issues/4555)）。
- **面向自动化的策略执行。** 严格的默认拒绝工具允许列表（[#5275](https://github.com/github/copilot-cli/issues/5275)）——开发者想要可证明的沙箱，而不是层层覆盖。
- **开发环境保真度。** 继承 PowerShell 环境变量（[#5281](https://github.com/github/copilot-cli/issues/5281)），以及在 Docker/多 `$HOME` 环境中避免插件配置里使用绝对 `cache_path`（[#3864](https://github.com/github/copilot-cli/issues/3864)）。
- **UI/UX 可配置性与可读性。** 可重新映射的 `shift+enter` 提交/换行行为（[#5291](https://github.com/github/copilot-cli/issues/5291)）、`/copy` 输出 Markdown 而不是 HTML（[#5288](https://github.com/github/copilot-cli/issues/5288)），以及恢复九月配色主题的对比度（[#5056](https://github.com/github/copilot-cli/issues/5056)）。
- **文档中有但实际缺失的 flag。** `--fusion-tier` 在模型选择文档中有描述，但 CLI 中不存在（[#5290](https://github.com/github/copilot-cli/issues/5290)）。

## 开发者痛点

- **MCP 认证是最大的反复问题来源。** OAuth token 被反复提示或忽略（[#4994](https://github.com/github/copilot-cli/issues/4994)、[#5229](https://github.com/github/copilot-cli/issues/5229)），Windows Entra broker 缓存在进程之间保持 0 字节（[#5236](https://github.com/github/copilot-cli/issues/5236)），标准 Entra `api://` scope 被拒绝（[#5061](https://github.com/github/copilot-cli/issues/5061)），并且对 Datadog 等提供商，token 交换直接失败（[#5058](https://github.com/github/copilot-cli/issues/5058)）。净效果：远程 MCP 服务器在无头和多进程工作流中不可靠。
- **静默失败多于显式报错。** BYOK 下 MCP 图像结果被丢弃（[#4600](https://github.com/github/copilot-cli/issues/4600)），插件静默地从不触发其 `sessionStart` 钩子（[#3864](https://github.com/github/copilot-cli/issues/3864)），交互模式下激活 0 个插件代理，而 `--prompt` 会激活 4 个（[#4592](https://github.com/github/copilot-cli/issues/4592)）。
- **资源泄漏损害长时间会话。** 并行代理运行中 RSS 增长 37 GiB（[#5254](https://github.com/github/copilot-cli/issues/5254)），以及空闲的 `copilot-file-search` 线程因无界诊断日志占满一个 CPU 核（[#4710](https://github.com/github/copilot-cli/issues/4710)，现已关闭）。
- **回归按小版本发布出现。** 1.0.8x/1.0.9x 系列反复产生阶跃式回归（OAuth 行为、MCP schema 急切注入、`/copy` 格式），让用户不确定该固定版本还是升级。
- **平台特定故障。** CLI 运行时 Windows 剪贴板写入失败（[#3981](https://github.com/github/copilot-cli/issues/3981)），Windows shell runner 忽略 PowerShell 环境变量（[#5281](https://github.com/github/copilot-cli/issues/5281)），以及 macOS keychain ACL/分区不匹配导致每次启动都提示（[#4273](https://github.com/github/copilot-cli/issues/4273)）。
- **缓慢、陈旧的讨论帖。** [#552](https://github.com/github/copilot-cli/issues/552)（403 模型列表）已开放近一年，有 31 条评论——组织级权益问题在文档中仍解释不足。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区简报 — 2026-10-30

---

## 1. 今日亮点

OpenCode 在 24 小时内发布了两个客户端版本：**v2.2.2** 使图像生成在已连接提供商暴露图像模型时默认可用，而 **v2.2.1** 引入了长时间运行的 `/goal <objective>` 会话模式，支持暂停/恢复/清除控制。社区注意力仍集中在 **免费层配额集中讨论帖（#59387，113 条评论）**，其中重置计时器在 Desktop 和 Zen/Go 上似乎仍处于故障状态。与此同时，若干高价值已关闭 issue 也已落地，涉及存储增长、git 快照行为和成本核算——这些都是长期存在的规模扩展投诉。

---

## 2. 版本发布

| 版本 | 说明 |
|---|---|
| **v2.2.2** | **默认启用图像生成** — 当提供商支持图像模型时，agent 无需额外配置即可调用 `experimental_image_generate`；首选模型通过 `experimental.image_generate.model` 设置（[#60258](https://github.com/anomalyco/opencode/pull/60258)）。 |
| **v2.2.1** | **目标导向会话** — 在 TUI 或 Desktop 中，`/goal <objective>` 会让会话保持运行，直到 reviewer 确认完成；`/goal pause`、`/goal resume`、`/goal clear` 用于管理它（[#59437](https://github.com/anomalyco/opencode/pull/59437)）。 |
| **process-daemon v0.4.0 / v0.3.3** | `opencode-pty` 0.4.0 和 0.3.3（协议 7）已以 `@opencode/pty` 发布到 npm。 |

⚠️ 针对 v2.2.2 的早期回归报告：[Desktop side-panel tabs cannot be closed and New-tab launchers ignore clicks (#60381)](https://github.com/anomalyco/opencode/issues/60381)。针对相关无法解析 browser-pane chunk 的修复已在 [#60388](https://github.com/anomalyco/opencode/pull/60388) 中开启。

---

## 3. 热门 Issue

1. **[#59387 — [zen] Free tier issues: read this first](https://github.com/anomalyco/opencode/issues/59387)** `OPEN` · 113 条评论 · 👍9
   这是免费层故障的权威分诊帖（“Free usage exceeded”、从不触发的重置计时器、Desktop 版本门控）。仅凭讨论量它就已成为社区的核心压力点；如今它事实上充当了配额逻辑的 bug tracker。

2. **[#33356 — Unbounded growth of the `event` table (13 GB+ `opencode.db`)](https://github.com/anomalyco/opencode/issues/33356)** `CLOSED` · 47 条评论 · 👍12
   长期运行的实例会无界增长，因为 `message.updated.*` 快照从未被清理。真实运维者报告过 13 GB 的数据库把 22 GB 卷占满到 99%。现已关闭，但保留/压缩设计仍是自托管用户最大的规模扩展顾虑。

3. **[#2999 — Provide means to disable Ctrl-C](https://github.com/anomalyco/opencode/issues/2999)** `CLOSED` · 42 条评论 · 👍30
   本时间窗口内点赞最高的条目：在 Windows Terminal/WezTerm 中，Ctrl-C 会立即杀死进程，而不是复制。可配置键位绑定/中断处理是一个反复出现的可访问性投诉。

4. **[#3176 — Why is OpenCode massively abusing git?](https://github.com/anomalyco/opencode/issues/3176)** `CLOSED` · 23 条评论 · 👍12
   会话快照会在 45 GB / 54K 文件的树上执行 `git add .`。这凸显了在大型 monorepo 中基于快照的撤销的成本；大家强烈认同快照需要忽略规则或作用域限制。

5. **[#26602 — Desktop hits 5-minute Headers Timeout with slow local providers](https://github.com/anomalyco/opencode/issues/26602)** `CLOSED` · 24 条评论 · 👍2
   即使设置 `"timeout": false`，请求也会在整整 5 分钟时中止，破坏本地 llama.cpp/Ollama 风格的部署。这是一个关键的本地优先可用性缺口。

6. **[#45417 — Session cost excludes subagent cost](https://github.com/anomalyco/opencode/issues/45417)** `CLOSED` · 10 条评论 · 👍17
   TUI 侧边栏、`opencode stats` 和 `/export` 在 fan-out 工作流中少报支出。高点赞比例表明团队依赖这些数字进行计费。

7. **[#51497 — perf(v2): every LLM call re-reads and re-decodes full session history](https://github.com/anomalyco/opencode/issues/51497)** `OPEN` · 5 条评论 · 👍1
   每步 O(n)，每会话 O(n²)，并伴随全表 SELECT 和逐行 schema 解码。配套有一个刻意不可合并的演示 PR（[#51499](https://github.com/anomalyco/opencode/pull/51499)），提出增量缓存 + 可选启用的 step GC。

8. **[#60381 — Desktop side-panel tabs can't be closed (2.2.2 regression)](https://github.com/anomalyco/opencode/issues/60381)** `OPEN` · 4 条评论
   新出现的回归，窗口重载后仍存在；除 Terminal/Open file 外的启动器行都失效。由于它直接跟随 v2.2.2 升级路径，因此非常及时。

9. **[#60410 — Project `experimental` block hides every global `experimental` option](https://github.com/anomalyco/opencode/issues/60410)** `OPEN` · `reproduced` · 2 条评论
   `Config.latest` 返回整块配置而不是合并字段，因此任何项目级 experimental 键都会静默地将用户的全局设置置空。配置语义类 bug 对高级用户具有隐蔽的破坏性。

10. **[#16626 — Add `session.stopping` plugin hook to re-enter the agent loop](https://github.com/anomalyco/opencode/issues/16626)** `CLOSED` · 15 条评论 · 👍12
    插件无法阻止循环终止。这与新的 `/goal` 功能密切相关，也是实现真正的停止条件自动化的前提。

*另需关注：* [#18654 — no way to change/remove email in OpenCode Zen](https://github.com/anomalyco/opencode/issues/18654)（👍20）、[#31554 — MCP servers spawn 2–4 duplicate processes](https://github.com/anomalyco/opencode/issues/31554)、[#34877 — cost shows $0.00 for GLM/DeepSeek/Qwen via `@ai-sdk/openai-compatible`](https://github.com/anomalyco/opencode/issues/34877)。

---

## 4. 关键 PR 进展

1. **[#60414 — feat(app): voice in the composer with first-call onboarding](https://github.com/anomalyco/opencode/pull/60414)** `OPEN`
   将实验性的 Desktop 语音通话从 Knobs 开关后面提升到 composer，并在首次使用时提供 onboarding。这是 #58249/#58587/#58591 的后续。

2. **[#60388 — fix(desktop): keep the browser pane chunk parseable](https://github.com/anomalyco/opencode/pull/60388)** `OPEN`
   修复 2.2.2 中 desktop 主构建产生的无法解析的 browser-pane chunk（关闭 #60369、#60386）。这是对当前版本的直接修复。

3. **[#60039 — feat(mcp): per-server TLS trust for remote MCP servers](https://github.com/anomalyco/opencode/pull/60039)** `OPEN`
   新增可选的 `tls` 配置块，使自签名或私有 CA 的 MCP 服务器可以单独受信任，而无需全局禁用验证。

4. **[#60041 — feat(mcp): trust private-network server certificates on first use](https://github.com/anomalyco/opencode/pull/60041)** `OPEN`
   堆叠在 #60039 之上：提供首次使用信任流程，模仿 SSH 风格的 TOFU，而不是手动复制指纹。

5. **[#59954 — fix(core): apply webfetch permissions to Code Mode `fetch`](https://github.com/anomalyco/opencode/pull/59954)** `OPEN`
   与安全相关：此前，被 `webfetch` 拒绝的 URL 仍可从 Code Mode 程序内部抓取，而且 “ask” 规则从不提示。

6. **[#60406 — fix(client): store session forms created without a location](https://github.com/anomalyco/opencode/pull/60406)** `OPEN`
   关闭 #59759。该问题中，question 工具发布 `form.created` 时没有 `location`，导致服务器上留下一个待处理问题，而 TUI 从未渲染它。

7. **[#59803 — fix(core): price Copilot long-context tiers and cache writes](https://github.com/anomalyco/opencode/pull/59803)** `OPEN`
   修正长时间 GitHub Copilot 会话中的严重少计问题（关闭 #52089、#59671）——这对任何使用成本数据做预算的人都相关。

8. **[#58066 — fix(server): allow `credential.update` to rotate the stored value](https://github.com/anomalyco/opencode/pull/58066)** `OPEN`
   `PATCH /api/credential/:id` 之前只接受 `label`，因此轮换过期的 API key 时会静默保留旧 secret。这对自动化密钥轮换很重要。

9. **[#60416 — fix(tui): keep HTML error pages out of the retry notice](https://github.com/anomalyco/opencode/pull/60416)** `OPEN`
   截断/汇总提供商的 HTML 错误正文（例如 nginx 503），而不是把整份文档倾倒进多行重试提示条中。

10. **[#60044 — feat(ai): type OpenRouter options on the Responses route](https://github.com/anomalyco/opencode/pull/60044)** `OPEN`
    在类型化的 Responses 选项中暴露 OpenRouter 的 `provider` 路由、`models` 回退和 `plugins`，而不是让它们以无类型/无 schema 的方式透传。

*另外仍在进行中：* [#60109](https://github.com/anomalyco/opencode/pull/60109)（在 Code Mode 内呈现权限反馈）、[#60407](https://github.com/anomalyco/opencode/pull/60407) 和 [#60390](https://github.com/anomalyco/opencode/pull/60390)（统一 Promise/Effect 客户端上的 `tool()`，并修复 typed-tool variance）、[#60380](https://github.com/anomalyco/opencode/pull/60380)（在外部 SDK hooks 之后运行动态 provider npm fallback）。

---

## 5. 热门讨论

本时间窗口未提供讨论数据；本节省略。

---

## 6. 功能请求趋势

- **Agent 循环控制与长时程自主性。** `/goal` 已在 v2.2.1 发布，但对 `session.stopping` hook 和停止条件插件的请求（#16626）表明，社区想要的是程序化控制，而不仅仅是 UI 动词。
- **并行 agent 的可观测性。** 一个专用的 agent 活动侧边栏（#27995），用于监控 `explore` / `librarian` / `oracle` 后台 agent，而不是手动调用 `background_output()`。
- **插件/可扩展性覆盖面。** 一个通用的跨客户端“UI intent”通道（#6330），以及更丰富的 hook 覆盖——目前插件无法在 TUI/Desktop 上一致地影响 UX。
- **可配置输入与可访问性。** 禁用/重映射 Ctrl-C（#2999），面向波斯语和阿拉伯语的 RTL/Bidi 渲染（#16875）。
- **账户与身份管理。** 在 OpenCode Zen 中更改或移除邮箱（#18654，👍20），以及更干净地处理 GitHub 邮箱变更后的重复身份。
- **企业/私有网络连接。** 针对远程 MCP 服务器的按服务器 TLS 信任（#60039、#60041，跟踪于 #40111），由企业环境中的自签名证书驱动。
- **成本透明度。** 会话总计中的子 agent 成本（#45417）、Copilot 的长上下文/缓存定价（#59803），以及 OpenAI 兼容中国提供商报告非零成本（#34877）。

---

## 7. 开发者痛点

1. **免费层配额逻辑是头号摩擦来源。** 重置计时器不触发，反而增加 24 小时（#60062）、跨天从不重置的限额（#59178、#60297），以及反复出现的“发一条消息就又被封”报告（#60064），已经汇聚成一个 113 条评论的集中讨论帖（#59387）。缺少有文档说明的窗口/计数器放大了挫败感。
2. **长期运行实例的规模与存储。** `event` 表无界增长到 13 GB（#33356），以及每会话 O(n²) 的历史重读（#51497），表明持久化层尚未跟上多小时的、子 agent 密集的工作流。
3. **Desktop 版本间回归。** v2.2.2 破坏了侧边栏交互（#60381），并产生了无法解析的 browser-pane chunk（#60388）。快速升级节奏的用户会立即感受到。
4. **成本核算对团队来说不够可信。** 缺少子 agent 支出（#45417）、OpenAI 兼容中国提供商显示 $0.00（#34877），以及 Copilot 少计（#59803），削弱了 `/export` 和 `opencode stats` 作为计费证据的可信度。
5. **超时与网络假设破坏本地优先使用。** 硬性的 5 分钟 headers 超时忽略 `"timeout": false`（#26602），再加上针对内置 Zen 提供商的对 IP 敏感的 `ECONNRESET`（#60422）。
6. **进程层的资源泄漏。** MCP 服务器每个生成 2–4 个重复进程，耗尽 `TasksMax` 并产生 `EAGAIN`（#31554）。
7. **配置语义让高级用户措手不及。** 项目级 `experimental` 块静默遮蔽所有全局 experimental 选项（#60410）——这是一类很难从 TUI 诊断的合并 bug。
8. **权限边界在不同执行模式间泄漏。** Code Mode 的 `fetch` 绕过 `webfetch` 权限规则（#59954），拒绝反馈被吞掉（#60109），削弱了用户对显式 allow/ask/deny 配置所期望的信任模型。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区摘要 — 2026-10-30

## 1. 今日要闻

过去 24 小时内没有新版本发布，但仓库迎来了一波密集维护：50 个 issue 和 23 个 PR 被更新，一大批修复围绕 Windows 路径处理、终端输入序列、会话文件隐私和项目信任语义被关闭。性能与确定性是另外两大主题 —— TUI 的转录虚拟化（#11598）、通过稳定工具顺序实现的提示缓存稳定性（#11529），以及每次请求都创建 Bedrock 客户端（#11527），都吸引了关注。`pi-ai` 中的提供商兼容性缺口仍是结构上最重要的未解决问题（#9508），而新提交的 RPC 关闭缺陷（#11599）当天就完成分诊并关闭。

---

## 2. 发布

过去 24 小时内无。略。

---

## 3. 热门 Issue

1. **[#9656](https://github.com/earendil-works/pi/issues/9656) — 在 Windows + Zellij 环境下全屏时，鼠标滚轮滚动的是提示历史而非转录内容** `OPEN`
   本窗口内讨论最多的 issue（12 条评论，5 个 👍）。输入路由仅在 Windows → Alacritty → Zellij → Pi 这条链路中失效，直连终端以及 Linux+tmux 下均正常。这表明全屏输入模型仍缺少一条正确的多路复用器感知路径，且自 9 月中旬起一直保持开启，尽管讨论持续不断。

2. **[#9508](https://github.com/earendil-works/pi/issues/9508) — `pi-ai` 向兼容提供商发送其不支持的 OpenAI 专有请求字段/角色/认证** `OPEN`
   9 条评论，仍未解决。打着“OpenAI 兼容”旗号的提供商会以 400/422 拒绝 OpenAI 专有字段、角色和认证头，导致本应有效的 `models.json` 配置失败。这是第三方模型支持在生态层面最明显的阻塞点。

3. **[#11157](https://github.com/earendil-works/pi/issues/11157) — 在 `session_start` 时调用 `sendMessage` 的扩展会把每个会话都变成旧版转录** `OPEN` `prio:p0` `inprogress`
   由于 `getInitialSystemMessage` 读取 `messages[0]`，pi-memory 和 pi-session-search 之类的扩展会把自定义消息插到系统提示之前，导致 Anthropic 请求丢失 `system` 字段。这是一个影响扩展生态的高严重度正确性问题，目前已标记为进行中。

4. **[#10982](https://github.com/earendil-works/pi/issues/10982) — 手动压缩进行中时，`pi.sendMessage(triggerTurn)` 会启动一次运行** `OPEN`
   一个并发缺陷：被触发的运行针对尚未压缩的上下文执行，并与压缩输出交织在一起。评论不多，但对任何在 `/compact` 期间依赖程序化轮次的用户影响很大。

5. **[#11529](https://github.com/earendil-works/pi/issues/11529) — 工具顺序取决于注册顺序，因此相同的工具集可能击穿提示缓存** `OPEN`
   `getCurrentTools()` 返回的是插入顺序，因此一个工具被移除后重新添加（MCP 重连、`tool_search` 加载、扩展重新加载）会移到 Map 末尾。由于工具位于缓存前缀中，这会静默地使提示缓存失效并抬高成本。

6. **[#11527](https://github.com/earendil-works/pi/issues/11527) — Bedrock：每次请求都会新建一个 `BedrockRuntimeClient`** `OPEN`
   `stream()` 每次调用都会实例化一个新客户端，丢弃凭证提供者和 HTTP/2 会话。在使用 SSO 配置时，这会导致每次模型调用都多一次 `GetRoleCredentials` 往返 —— 一个看似简单但影响显著的延迟与吞吐回归。

7. **[#10770](https://github.com/earendil-works/pi/issues/10770) — `.pi/agent/sessions` 的默认权限应为 `0700`** `CLOSED`
   会话目录此前按进程 umask 创建，在典型的 `0022` 系统上会使转录内容对所有用户可读。6 条评论，并很快得到修复 —— 一次有明确用户需求的隐私加固胜利。

8. **[#10947](https://github.com/earendil-works/pi/issues/10947) — `isProjectTrusted()` 忽略已保存的“不信任”决定** `CLOSED`
   对于没有需要信任的资源的项目，`trust.json` 中保存的 `false` 会被忽略，因为自动信任检查在读取信任存储之前就执行了。这是一个与安全相关的语义缺陷，现已解决。

9. **[#11570](https://github.com/earendil-works/pi/issues/11570) — 长时间会话中巨型 `ConsString` 展平导致 V8 OOM（SIGABRT）** `CLOSED` `no-action`
   五份来自持续 2 天以上会话（约 8MB 会话文件）的生产崩溃报告，堆栈签名都落在同一处 `flatMap` 字符串分配上。已关闭但未处理，这使得长会话的内存行为成为一项悬而未决的运维风险。

10. **[#10187](https://github.com/earendil-works/pi/issues/10187) — 将 `deviceId` 移出全局设置** `CLOSED` `no-action`
    3 个 👍、5 条评论，来自把 agent 设置当作共享 dotfiles 管理的用户。把设备专属标识符放进会被提交的配置文件里，是真实的流程冲突，也是反复出现的请求类型。

---

## 4. 关键 PR 进展

1. **[#11598](https://github.com/earendil-works/pi/pull/11598) — perf(tui)：虚拟化转录内容并延迟调整全屏布局** `CLOSED`
   直接解决了每帧 O(transcript) 的开销：此前每个消息组件在整个会话期间都保持存活，导致滚动、输入和工作指示器的开销随历史长度增长。本批次中影响最大的性能改动。

2. **[#6881](https://github.com/earendil-works/pi/pull/6881) — feat(ai)：当响应包含提供商上报的成本时使用该成本** `CLOSED`
   在可用时（包括 BYOK 场景下的 `cost_details.upstream_inference_cost`）用计费成本填充 `usage.cost.total`，否则回退到 `calculateCost`。提升了网关和上游计费场景下的成本准确性。

3. **[#11594](https://github.com/earendil-works/pi/pull/11594) — fix(ai)：处理阿里云端点上的推理模型** `CLOSED`
   避免阿里云推理端点因 `developer` 角色产生 400 错误 —— 这是 #9508 所跟踪的提供商异构性问题的又一个具体实例。

4. **[#11591](https://github.com/earendil-works/pi/pull/11591) — fix(tui)：在换行边界丢弃带样式单词后的空格，在行尾重置删除线** `CLOSED`
   修复 #10925。附加在后续空格上的 ANSI 关闭码导致换行行首多出一个空格；已修正 tokenizer 和 `wrapSingleLine`。

5. **[#11588](https://github.com/earendil-works/pi/pull/11588) — fix(coding-agent)：在 Windows 上以大小写不敏感方式比较路径** `CLOSED`
   修复 #10488。`realpathSync` 会保留输入的大小写，因此 `c:\Users` 与 `C:\Users` 会产生错误的技能冲突；现在 Windows 上的路径比较会统一大小写。

6. **[#11587](https://github.com/earendil-works/pi/pull/11587) — fix(coding-agent)：以 `0700` 创建会话目录，以 `0600` 创建会话文件** `CLOSED`
   #10770 的实现。填补了标准 `0022` umask 下转录内容可被随意读取的缺口。

7. **[#11590](https://github.com/earendil-works/pi/pull/11590) — fix(coding-agent)：刷新未固定版本的临时 npm 扩展** `CLOSED`
   修复 #11375。此前 `pi -e npm:<pkg>` 会永远复用第一次下载的结果；现在无版本号的 spec 和 dist-tag 在存在更新版本时会重新解析。

8. **[#11586](https://github.com/earendil-works/pi/pull/11586) / [#11585](https://github.com/earendil-works/pi/pull/11585) — fix(tui)：将按住原生 Shift 时的 `ESC`+`Return` 视为 `Shift+Enter`** `CLOSED`
   Apple Terminal 在 Shift+Enter 时发送 `\x1b\r`；在没有 kitty 协议的情况下，这会被解析为 `alt+enter`，结果是排队一条后续消息而不是插入换行。原生 Shift 回退路径和 Apple Terminal 路径均已覆盖。

9. **[#11577](https://github.com/earendil-works/pi/pull/11577) — fix(coding-agent)：调整图片尺寸时选择 PNG 与 JPEG 中较小者** `CLOSED`
   候选列表总是优先尝试 PNG，因此即使存在满足 `maxBytes` 的更小 JPEG，照片类图像也会以体积大出许多的 PNG 发送。

10. **[#11580](https://github.com/earendil-works/pi/pull/11580) — 为 TUI 添加阿拉伯语 RTL 渲染（双向文本，Windows Terminal）** `CLOSED`
    为不自行应用双向重排的终端（尤其是 Windows Terminal）实现 UAX #9 bidi 重排，修复阿拉伯语文本镜像问题。这是 TUI 层国际化的重要一步。

同样值得关注：**[#11562](https://github.com/earendil-works/pi/pull/11562)**（OPEN）让新会话在 cwd 的默认目录中启动，而不是继承外来项目的文件夹；**[#11572](https://github.com/earendil-works/pi/pull/11572)** 停止重试 Zhipu/Z.ai 的用量限制错误（code 1308，HTTP 429）。

---

## 5. 热门讨论

**展示与分享**
- **[#11503](https://github.com/earendil-works/pi/discussions/11503) — pi-rs：无头核心的 Rust 移植，逐原子验证行为一致性**（1 条评论，1 个 👍）
  社区对 pi 0.74.2 无头核心的 Rust 重实现 —— 包括 agent 循环、7 个内置工具、会话 JSONL、CLI 界面和系统提示 —— 并对原版做字节级一致性校验。其中还包含一项 DeepSeek API 兼容性发现。它既是一套独立的一致性验证工具，也证明了无头核心可以与 TUI 干净地分离。

**问答**
- **[#11574](https://github.com/earendil-works/pi/discussions/11574) — 如何将 npm 包上传到 Pi 包源？**（0 条评论）
  一位包作者构建了扩展，却找不到关于发布它的文档指引。该问题无人回答，它指向的是扩展分发环节的缺口，而非一次性的提问。

---

## 6. 功能请求趋势

- **提供商抽象的正确性。** 这是数量最多的一类：OpenAI 专有字段泄漏到兼容提供商（#9508）、阿里云 `developer` 角色处理（#11594）、Google thought-signature 的合并/丢弃（#11538）、Z.ai 强制思考行为（#8706）、Zhipu 重试分类（#11572），以及 Bedrock 客户端复用（#11527）。用户正让 Pi 面对一长串长尾提供商，希望请求层保持保守且具备提供商感知。
- **提示缓存与成本确定性。** 稳定的工具顺序（#11529）、提供商上报成本（#6881），以及在 OpenRouter 上触发 402 的输出预算默认值（#11517），都指向对可预测开销的需求。
- **安全与隐私默认值。** `0700`/`0600` 的会话产物（#10770、#11587）、正确的信任存储语义（#10947），以及将设备本地状态与可共享配置分离（#10187）。
- **长会话性能与内存。** 转录虚拟化（#11598）、`getSessionName()` 每次渲染都扫描所有条目（#11567），以及多日会话中的 V8 OOM（#11570）。
- **终端与平台兼容性。** Windows 盘符大小写（#10488）、Zellij 鼠标路由（#9656）、Apple Terminal 的 Shift+Enter（#11559/#11586）、被拆分的鼠标上报尾部（#11172）、Orca 的 Kitty 图像检测（#9329），以及 RTL 文本（#11580）。
- **MCP 与扩展易用性。** 命名空间作用域的 `tool_search` 查询（#11558）、工具结果中可恢复的不支持图片格式（#11552）、将模型调用来源传给 `HarnessOptions.models`（#11596），以及生命周期安全的扩展调用（#11157、#10982、#11599）。

---

## 7. 开发者痛点

- **Windows 仍是最粗糙的平台。** 大小写敏感的路径比较、cwd 与 home 之间盘符不一致，以及终端特有的转义序列，持续制造着一连串缺陷（#10488、#9656、#11586）。本窗口修复了若干，但这是一再出现的模式，而非一次性的问题。
- **“OpenAI 兼容”并不是一份契约。** 开发者反复撞上 400/422 错误，原因要么是 Pi 发出了 OpenAI 专有的字段、角色或认证头，要么是模型元数据（上下文、输出容量、思考格式）被误用。#9508 自 9 月起一直开启，9 条评论，尚无结论。
- **会话随规模增长而劣化。** 约 40 万条目时的输入延迟、O(transcript) 渲染，以及多日会话因 V8 OOM 中止，是同一个扩展性问题的三种症状。三者中有两个被关闭且未处理，只能由运维自行绕过。
- **静默的正确性失败是最糟糕的一类缺陷。** `pi -p` 中 `stdout` 为空却以退出码 0 结束（#11568）、调用 `ctx.shutdown()` 后 RPC 进程空转且永不退出（#11599），以及扩展静默地把每个会话都转成旧版转录（#11157），都属于安静失败而非大声报错。
- **配置可移植性。** 在 `deviceId` 仍位于全局设置中时，那些对 agent 设置做版本控制的用户无法安全地这么做（#10187）；此外，此前存储在磁盘上的信任决定也未被遵守（#10947）。
- **扩展生命周期竞态。** `session_start` 的消息注入、压缩期间的 `triggerTurn`，以及异步关闭，都是扩展与核心之间顺序保证不明确的场景 —— 随着扩展生态的发展，这个缺口只会越来越大。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区摘要 — 2026-10-30

*来源：[github.com/QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)*

---

## 1. 今日亮点

Qwen Code 在全部三条轨道上发布了 **v0.26.4** —— CLI/nightly、TypeScript SDK（v0.1.26）和 Desktop（v0.26.4）—— 变更列表小而低风险，聚焦于生成式 UI 主题化和 Managed Agent SQL 历史记录边界。更大的故事在幕后：CI 和发布基础设施主导了问题追踪器，主分支长期失败（`#14724`、`#12714`）以及一波 runner 容量/分区工作正在 PR 中落地。与此同时，核心可靠性修复（SDK stream-json 消息标识、`/loop` 取消、配置持久化）正在积累，社区正在对 daemon、SDK 和 Web Shell 界面进行压力测试。

---

## 2. 发布

**v0.26.4** —— 稳定版 CLI 发布，无文档记录的破坏性变更。

- **feat(generated-ui): 在生成视图中跟随宿主主题** —— [PR #15798](https://github.com/QwenLM/qwen-code/pull/15798)。生成式 UI 现在继承宿主应用的主题，而非强制使用自己的主题。
- **feat(managed): 添加有界 SQL 历史查询**（nightly 通道，PR [#15839](https://github.com/QwenLM/qwen-code/pull/15839)，作者 @doudouOUC）—— 限制 Managed Agent 服务中历史查询的成本。
- **feat(generated-ui): 继续与 ca… 聊天** —— 同一生成式 UI 领域的增量续接工作。

**sdk-typescript-v0.1.26** —— 捆绑 CLI **0.26.4**（从与 SDK 相同的源分支/ref 构建）。注意：生成的发布正文仍在重复的“Bundled CLI Version”块中同时列出 `0.26.4` 和 `0.26.3`，且已发布说明中的捆绑 CLI 部分似乎格式异常 —— 值得做一次文档清理。

**desktop-v0.26.4** —— 包含 `fix(config): 报告被静默丢弃的控件`（[#15794](https://github.com/QwenLM/qwen-code/pull/15794)，作者 @yiliang114）以及共享会话流程的文档。

**v0.26.4-nightly.20261029.70be140e14** —— 从 `release/v0.26.4-nightly.20261029.70be140e14` 切出的 nightly 发布。

---

## 3. 热门 Issue

1. **[#14724](https://github.com/QwenLM/qwen-code/issues/14724) — 主分支 CI 失败：Managed Agent 门禁，未识别到测试** *(OPEN，95 条评论，自 2026-10-19 起每日更新)*
   一个由机器人提交的、按提交记录的 CI 失败追踪器，已成为仓库中噪音最大的线程。它标志着一个真实的结构性问题：Managed Agent 门禁可以在不归因任何测试的情况下失败，因此没有任何东西被处理。凭借 95 条评论和 `autofix/skip`，它实际上是一个无人能关闭的常驻警报。

2. **[#15052](https://github.com/QwenLM/qwen-code/issues/15052) — CI：在待处理运行被替换时保留独立的主分支 E2E 验证** *(OPEN，P2，14 条评论，ready-for-human)*
   按 ref 作用域的并发会替换较早的待处理主分支 E2E 运行，因此中间 SHA 永远不会获得独立的验证结果。维护者 @yiliang114 主张按 SHA 验证和失败归因 —— 这是许多“临时性”CI 报告背后的根本原因。

3. **[#14215](https://github.com/QwenLM/qwen-code/issues/14215) — 自托管模型的上下文不可变性** *(OPEN，P2，7 条评论，1 👍，need-discussion)*
   最具实质性的社区功能线程：使用本地模型且提示处理缓慢的用户希望记忆、梦境、会话和建议停止改变上下文，因为每次重写都要花费数分钟的预填充时间。标记为 `roadmap/context-performance` 和 `scope/caching` —— 功能丰富性与本地推理经济性之间真正的架构张力。

4. **[#15461](https://github.com/QwenLM/qwen-code/issues/15461) — 发布：预留 ECS runner 槽位并激活构建/发布拆分** *(OPEN，P2，8 条评论，ready-for-human)*
   `publish` 是最后一个仍在竞争 GitHub 托管容量的发布作业；ECS 发布主机从未被配置。这是直接影响修复到达用户速度的发布工程债务。

5. **[#15861](https://github.com/QwenLM/qwen-code/issues/15861) — ci(smoke)：web-shell 宿主目录选择器冒烟测试超时，与 PR 无关** *(OPEN，**P1**，3 条评论)*
   在不相关 PR 上出现的确定性冒烟测试失败，意味着每个贡献都要付出重新归因的代价。测试基础设施问题被评为 P1，反映了 CI 信任度已严重侵蚀。

6. **[#15855](https://github.com/QwenLM/qwen-code/issues/15855) — `contextWindowSize` 被自动移除** *(OPEN，**P1**，need-information，3 条评论)*
   用户在 OpenAI 兼容提供商块中显式设置的 128k 限制被静默剥离。严重性高，因为它会在无错误的情况下破坏本地模型工作流 —— 静默配置重写是反复出现的信任破坏者（另见 #15794 和 #15829）。

7. **[#15894](https://github.com/QwenLM/qwen-code/issues/15894) — 作为用户消息发送的 `/loop` 停止不会取消待处理的 `loop_wakeup`** *(OPEN，P2，daemon，background-automation)*
   一个 80 分钟的自主 `/loop` tick 忽略了明确的停止并恢复运行。这是自主 agent 的控制/安全问题 —— 用户期望停止就是停止。

8. **[#15875](https://github.com/QwenLM/qwen-code/issues/15875) — SDK stream-json：思考后文本快照改变了 `message.id`** *(OPEN，P2，scope/sdk，scope/non-interactive)*
   使用 `--include-partial-messages` 时，最终文本快照的 `message.id` 与起始的 `message_start` 不同。破坏了任何按 ID 关联流式片段的 SDK 消费者 —— 这是集成的核心协议保真度 bug。

9. **[#15327](https://github.com/QwenLM/qwen-code/issues/15327) — tracking(core)：在 `experimental.unifiedAgentLoop` 后面让每个界面运行在同一个 agent loop 上** *(OPEN，P1，ready-for-human)*
   架构北极星：在 `packages/core` 中有一个 agent loop，由 CLI、daemon、Web Shell 和 Desktop 共享。目前每个界面都有自己的 loop，这解释了界面特定 bug 的长尾。

10. **[#13649](https://github.com/QwenLM/qwen-code/issues/13649) — feat(a2a)：无 contextId 的 A2A 消息各自创建一个无法区分、无界的聊天会话** *(OPEN，P2，need-discussion)*
    自 #13583 以来，默认的 A2A 请求形态（无 `contextId`）为每条消息创建一个会话，淹没了所有者的 Web Shell 会话列表，且无法区分或限制它们。这是一个具有真实 UX 影响的多 agent 设计缺口。

*另需关注：* [#12714](https://github.com/QwenLM/qwen-code/issues/12714)（13 条评论，CI `src/llm.test.tsx` 路由失败）、[#15739](https://github.com/QwenLM/qwen-code/issues/15739)（9 条评论后关闭）、[#1206](https://github.com/QwenLM/qwen-code/issues/1206)（2 👍，OpenAI 兼容 API 的动态多模型支持 —— 自 2025-12 起长期开放且仍未关闭）、[#15877](https://github.com/QwenLM/qwen-code/issues/15877)（远程 daemon 监听器/源站设置不清晰）、[#15869](https://github.com/QwenLM/qwen-code/issues/15869)（PR #14348/#15450 停滞 3 天 —— 社区对审查延迟的升级）。

---

## 4. 关键 PR 进展

1. **[#15871](https://github.com/QwenLM/qwen-code/pull/15871) — refactor(managed-agent)：将服务器拆分为 Maven 模块**（@doudouOUC）
   将 Managed Agent Server 拆分为六个生产 Maven 模块加一个未发布的验证模块，边界处使用框架无关的契约。这是托管界面正在进行中最大的结构性变更。

2. **[#11854](https://github.com/QwenLM/qwen-code/pull/11854) — feat：添加混合代码模式**（@DragonnZhang，`autofix/takeover`）
   添加与 Codex 对齐的 `tools.mode` 枚举（`direct` / `code_mode` / `code_mode_only`），暴露一个隔离的 `exec` JavaScript 工具，同时保持普通工具可调用。这是队列中对使用工具的 agent 最重要的能力新增。

3. **[#15466](https://github.com/QwenLM/qwen-code/pull/15466) — feat(serve)：添加用户作用域 Skill 激活路由**（@callmeYe）
   新的 daemon 路由 `PUT /skills/:name/activation` 和批量 `PUT /skills/activation`（最多 100 个名称），用于用户设置作用域。无需工作区选择器即可从 Web Shell 解除 Skill 管理的阻塞。

4. **[#15863](https://github.com/QwenLM/qwen-code/pull/15863) — fix(cli)：在上下文使用中区分指令文件与自动记忆**（@callmeYe）
   项目/用户/扩展/语言指令现在报告在 *Instruction files* 下，而自动记忆策略和目录快照报告在 *Auto memory* 下。直接改善了上下文预算的透明度 —— 与 #14215 的讨论相关。

5. **[#15854](https://github.com/QwenLM/qwen-code/pull/15854) — perf(release)：跨发布复用经验证的原生音频预构建**（@yiliang114）
   当构建输入指纹匹配时，复用成功主分支发布中的音频预构建；经验证的缓存命中跳过五目标构建矩阵，任何查找/下载/验证失败都回退到完整重建。明确的发布延迟收益，且有安全的回退路径。

6. **[#15888](https://github.com/QwenLM/qwen-code/pull/15888) — fix(ci)：将完整的 macOS 测试套件分区为有界作业**（@yiliang114）
   将 macOS CI 拆分为七个作业（三个 CLI 分片、两个 core 分片、剩余工作区套件、独立/运行时脚本），原始检查保留为聚合。后续发现追踪于 [#15895](https://github.com/QwenLM/qwen-code/issues/15895)。

7. **[#15885](https://github.com/QwenLM/qwen-code/pull/15885) — fix(ci)：将 E2E 构建作业路由到池优先 runner 表达式**（@yiliang114）
   将 E2E 共享构建作业移到与其他所有作业相同的自托管 `ecs-qwen` 池优先路由上。与 #15854 和 #15461 一起，这是本周的 runner 容量故事。

8. **[#12799](https://github.com/QwenLM/qwen-code/pull/12799) — fix(edit)：保留编辑未触及的换行符**（@feiiiiii5）
   在本地编辑跨度之外保留原始字节，并按跨度选择插入的换行符，而全文件替换保持先前的 CRLF 行为。一个长期运行的正确性修复，对 Windows/脏 diff 投诉有巨大影响。

9. **[#15829](https://github.com/QwenLM/qwen-code/pull/15829) — fix(vscode)：写入提供商配置时保留同级模型设置**（@2SinceJuly）
   从 VS Code 伴生应用配置提供商时，之前会用仅含 `name` 的对象重写整个 `settings.model` 对象，删除同级键。现在逐键写入 `name`/`baseUrl` —— 与 #15855 和 #15794 同类别的静默配置丢失 bug。

10. **[#15851](https://github.com/QwenLM/qwen-code/pull/15851) — fix(cli)：报告发送前拒绝而非重放答案**（@2SinceJuly）
    当一轮在发送前被拒绝（会话 token 限制或 `UserPromptSubmit` 钩子阻塞）时，共享结果构建器回退到*上一*轮的助手消息。修复了持久 stream-json 会话的一个严重正确性 bug。

*另值得追踪：* [#15883](https://github.com/QwenLM/qwen-code/pull/15883) 和 [#15882](https://github.com/QwenLM/qwen-code/pull/15882)（恢复夹具转录覆盖；Hosted 故障夹具中的 Managed JDBC 选择）、[#15803 系列](https://github.com/QwenLM/qwen-code/pull/15286) Java 缓存卫生修复（[#15286](https://github.com/QwenLM/qwen-code/pull/15286)、[#15274](https://github.com/QwenLM/qwen-code/pull/15274)）、[#15060](https://github.com/QwenLM/qwen-code/pull/15060)（共享会话视图重置状态）、[#13026](https://github.com/QwenLM/qwen-code/pull/13026)（报告推测复制失败）、[#13035](https://github.com/QwenLM/qwen-code/pull/13035)（容错的托管会话元数据解析）、[#13067](https://github.com/QwenLM/qwen-code/pull/13067)（Ctrl+命名键 bug）、[#13080](https://github.com/QwenLM/qwen-code/pull/13080)（短终端上 `/stats` 滚动）、[#14653](https://github.com/QwenLM/qwen-code/pull/14653)（按身份选择 `/mcp`）。

---

## 5. 热门讨论

*此数据集中未提供 Discussion 数据 —— 本节省略。*

---

## 6. 功能请求趋势

- **自托管模型的上下文与记忆经济学**（`roadmap/context-performance`、`scope/memory`、`scope/caching`）—— 最强的单一方向。#14215 主张当提示处理缓慢时，记忆/梦境/建议/会话应变为可选加入或不可变，而 #15863 的指令与自动记忆拆分显示团队正朝着更好的上下文透明度迈进。
- **一个 agent loop / 统一会话管理**（`roadmap/session-management`）—— #15327 提议在 `experimental.unifiedAgentLoop` 后面有一个单一的 `packages/core` agent loop，明确是为了消除 CLI、daemon、Web Shell 和 Desktop 之间的界面特定分歧。
- **多 agent 和 A2A 加固**（`roadmap/multi-agent`）—— #13649（无界会话创建）、#13645（共享提及语法、类型化执行状态集）和 #13583 的后续表明需要可预测的多 agent 语义，而非更多功能。
- **OpenAI 兼容端点的动态多模型支持** —— #1206（自 2025-12 开放，2 👍）仍要求动态获取和切换模型，而非硬编码一个。
- **Web Shell / Desktop UX 对等** —— Session Overview 和 Split View 的键盘快捷键（#13175，已关闭）、俄语/任意语言 i18n（#13391，已关闭）、无工作区会话可见性（#11878，已关闭）、侧边栏类别一致性（#15867）、macOS daemon `PATH` 扩展（#12690）。关闭率表明此处有积极的分诊。
- **CI/发布基础设施作为产品界面** —— ECS runner 预留（#15461）、隔夜 macOS 验证策略（#15783）、池优先 runner 路由。这些现在是头等功能请求，而非杂务。
- **远程/daemon 访问人体工学** —— #15877（远程 `qwen serve` 的监听器 + 源站设置）和 #12059（远程 webview 故障模式：转发端口 Host 门禁、IPv6 CSP、Remote-SSH/WSL）指向日益增长的远程开发使用。

---

## 7. 开发者痛点

- **长期、无法归因的 CI 失败。** #14724（95 条评论）、#12714（13）、#14796、#15712、#15281 和 #15739 都追溯到同一模式：运行在任何测试结果报告之前就失败，因此追踪器只能按提交记录。#15052 诊断了并发根本原因。贡献者正在因*不相关* PR 上的重新归因而损失时间（#15861 明确“与 PR 无关”）。
- **macOS CI 成本与时长。** #15783 请求将常规 macOS/Darwin 验证移至亚洲/上海隔夜运行；#15888 将套件分区为七个有界作业。两者都存在是因为 macOS 通道是最慢、最不稳定的门禁。
- **静默配置丢失。** 一个窗口内有三份独立报告：`contextWindowSize` 被剥离（#15855）、VS Code 提供商写入覆盖同级 `settings.model` 键（#15829）、Desktop 无通知地丢弃控件（#15794，已修复）。这个模式 —— 修改共享配置对象而非修补键 —— 是用户信任的系统性危险。
- **流式/SDK 协议保真度。** #15875（消息 ID 在流中途改变）和 #15851（发送前拒绝后重放上一轮的答案）都破坏了将流视为权威的消费者。机器可读输出现在是一份契约，而它正在泄漏。
- **不生效的自主控制。** #15894：明确的停止不会取消待处理的 `loop_wakeup`，因此自定步调的 `/loop` 会重新武装自己。对于运行自主管理器的用户，这是工具与无界进程之间的区别。
- **真实文件系统上的编辑正确性。** 长期存在的 PR #12799（换行符）、#13026（推测复制失败）、#13035（撕裂的转录行）和 #13067（Ctrl+命名键输入）显示了字节级和输入级正确性 bug 的稳定暗流，这些 bug 需要数周才能落地，因为它们位于嘈杂的 CI 门禁后面。
- **本地/自托管模型人体工学。** #14215（提示处理缓慢）、#15855（上下文窗口被忽略）、#15877（远程 daemon 设置）—— 自托管细分市场持续遇到云用户不会遇到的摩擦，且这些问题具有最高的每用户影响。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*