# OpenClaw 生态日报 2026-10-29

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-29 14:08 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-29

## 1. 今日概览

OpenClaw 在 2026-10-29 的分诊吞吐量非常高：过去 24 小时内有 500 个 issue 和 500 个 PR 更新，其中 224 个 PR 已合并/关闭，276 个仍开启；142 个 issue 已关闭，358 个仍开启/活跃。未发布新版本，最近的稳定版验证参考是 2026.10.9（Issue #187490），扩展稳定版 2026.9.36 的回合并仍活跃。活动集中在发布工程、CI/测试稳定化，以及一批 Web UI 资源效率修复；而 issue 队列主要由网关启动、崩溃恢复和会话状态缺陷占据。最突出的健康担忧是严重性集中：仅在按评论数排序的前 50 个样本中就至少有七个 P0 issue，其中若干标记为 `clawsweeper:no-new-fix-pr`，表明存在大量缺乏明确修复方案的稳定化积压。总体评估：**维护和分诊速度快，但稳定性债务偏高，且今日零发布产出。**

## 2. 发布

过去 24 小时内未发布新版本，“Latest Releases”信息流为空。

来自 issue/PR 元数据的上下文（并非发布公告）：
- 稳定版线似乎是 **2026.10.9**，由 [#187490](https://github.com/openclaw/openclaw/issues/187490) 中的发布验证不稳定测试跟踪引用。
- **2026.10.8** 的复盘阻塞项正在 [#187249](https://github.com/openclaw/openclaw/pull/187249) 中处理（发布验证工具）。
- 扩展稳定版 **2026.9.36** 正在接收定向回合并：[#187252](https://github.com/openclaw/openclaw/pull/187252)（无状态推理回放保护）和 [#187701](https://github.com/openclaw/openclaw/pull/187701)（实时测试豁免键控）。

今日无破坏性变更或迁移说明可报告。

## 3. 项目进展

过去 24 小时有 224 个 PR 已合并/关闭。前 30 个样本中可见的值得关注项：

**已落地修复（已关闭 PR）：**
- [#187646](https://github.com/openclaw/openclaw/pull/187646) — 原生 Codex 辅助进程现在会在 Stop 时被清理，防止会话中止后产生孤儿子命令（修复 #187598）。
- [#187687](https://github.com/openclaw/openclaw/pull/187687) — Web UI 在隐藏标签页中停止轮询存储维护进度；返回时刷新一次。
- [#187701](https://github.com/openclaw/openclaw/pull/187701) — 发布验证豁免已正确按扩展稳定版线设置键值。

**值得关注的在途工作（已开启 PR）：**
- **MCP 能力缺口：** [#187699](https://github.com/openclaw/openclaw/pull/187699) 使配置的 MCP 工具可通过 `tools.invoke` / HTTP `/tools/invoke` 工作（当前尽管出现在 `tools.effective` 中，却返回 `not_found`）。这消除了一处明显的功能与实现不一致。
- **Web UI 效率批次（vincentkoc）：** [#187691](https://github.com/openclaw/openclaw/pull/187691)（在隐藏标签页释放 Markdown worker）、[#187693](https://github.com/openclaw/openclaw/pull/187693)（暂停 Updates 倒计时）、[#187686](https://github.com/openclaw/openclaw/pull/187686)（暂停 Logbook 轮询）。
- **渠道修复：** [#147886](https://github.com/openclaw/openclaw/pull/147886)（Feishu 已记录的 `markdown.tables` 模式，标记为 `merge-risk: compatibility`）、[#187555](https://github.com/openclaw/openclaw/pull/187555)（Discord 取消时中止信号传播）、[#187199](https://github.com/openclaw/openclaw/pull/187199)（Mattermost Unicode 规范化）、[#187711](https://github.com/openclaw/openclaw/pull/187711)（Telegram 转发富消息防抖）。
- **平台正确性：** [#187703](https://github.com/openclaw/openclaw/pull/187703) 在系统时钟变化时保持缓存新鲜度；[#187177](https://github.com/openclaw/openclaw/pull/187177) 让 Claude 迁移在 JSON 格式错误时明确失败，而不是静默跳过设置。
- **记忆子系统：** [#136482](https://github.com/openclaw/openclaw/pull/136482)（XL）提议将记忆索引发布移入有界 worker，以减少 SQLite 争用和重建索引泄漏——自 2026-09-02 开启，仍为 `needs proof`。

**已关闭 issue：** [#178000](https://github.com/openclaw/openclaw/issues/178000)（会话状态竞态被误分类为模型回退）、[#136386](https://github.com/openclaw/openclaw/issues/136386)（cron exec 允许列表拒绝）、[#187468](https://github.com/openclaw/openclaw/issues/187468)（package-swap 更新失败）。

## 4. 社区热点话题

评论量是今天主要的互动信号；👍 数量普遍接近零，表明互动以讨论和调试为主，而非广泛的用户投票。

| 条目 | 评论数 | 重要性 |
|---|---|---|
| [#160959](https://github.com/openclaw/openclaw/issues/160959) — P0：网关在捕获大型外部插件时阻塞数分钟 | 64 | 样本中互动最高的问题。这是 2026.9.6 因插件生成捕获（#144252）引入的回归，会在启动时和运行时重新发布时阻塞事件循环。标记为 `no-new-fix-pr` + `needs-maintainer-review`。 |
| [#164294](https://github.com/openclaw/openclaw/issues/164294) — P2：状态 DB WAL 自动检查点永不完成 | 38 | 检查点被进程内读取器永久阻塞（17,091 个日志帧，866 个已检查点）。这是持久化层设计问题，对 P2 而言互动异常高。 |
| [#150635](https://github.com/openclaw/openclaw/issues/150635) — 短期召回驱逐阻碍 dreaming 提升 | 21 | 记忆架构缺陷：512 条目上限 + 每夜零召回摄取阻止深度阶段提升。 |
| [#120385](https://github.com/openclaw/openclaw/issues/120385) — 用于 cron/记忆刷写轮次的 Code Mode 目录不完整 | 21 | MCP 工具缺失（“Unknown tool id”）仅表现为通用 exec 失败；作者于 2026-10-21 更正了原始范围。 |
| [#166634](https://github.com/openclaw/openclaw/issues/166634) — P0：检查点失败导致 agent 在崩溃恢复后不可用 | 18 | `SQLite session reclamation commit checkpoint expired` 会永久阻塞 `chat.history`、`models.list` 等。 |
| [#172837](https://github.com/openclaw/openclaw/issues/172837) — P1：Codex `nativeSubagentAssignments` 永不移除 | 17 | 在投递放弃后，分配条目会在每个新的父线程上重新固定并重新发送。 |
| [#177656](https://github.com/openclaw/openclaw/issues/177656) — P1：主机 exec 完整性准入错误 | 17 | 在稳定版 2026.10.5 上至少 28 次失败；子效果未证实。 |
| [#44309](https://github.com/openclaw/openclaw/issues/44309) — 单向 A2A 派发模式 | 15 | 最古老的高互动功能请求（2026-03-12）；仍为 `needs-product-decision`。 |

**底层需求：**最集中的问题不是功能，而是**负载、崩溃以及时钟/睡眠事件下的运行时可靠性**。用户运行着带大型插件树、数 GB SQLite 状态和长时间运行会话的网关；当前故障模式是事件循环饥饿、WAL/检查点争用以及崩溃后不可恢复状态。第二组是**记忆/召回正确性**（#150635、#157827、#148033），表明 dreaming/RAG 流水线需要设计评审，而非点状修复。

## 5. Bug 与稳定性

按严重性排序，来自前 50 样本。“无修复 PR”反映 `clawsweeper:no-new-fix-pr` 标签。

**P0 — 严重**
1. [#160959](https://github.com/openclaw/openclaw/issues/160959) — 网关事件循环被外部/工作区插件捕获阻塞数分钟（2026.9.6 回归）。**未识别到修复 PR。** 数据集中评论数最高（64）。
2. [#162211](https://github.com/openclaw/openclaw/issues/162211) — 启动阻塞事件循环 40–200 秒；健康监控将卡顿误读为断开，并升级为重启循环（2026.9.7，macOS/launchd）。需要线上复现。
3. [#166634](https://github.com/openclaw/openclaw/issues/166634) — 崩溃后 agent 在回收检查点过期后永久 `UNAVAILABLE`。在 main 上无法复现。
4. [#158592](https://github.com/openclaw/openclaw/issues/158592) — 主机睡眠/唤醒后，模型运行时发布超时且永不恢复；在网关重启前所有入站消息都失败。
5. [#160762](https://github.com/openclaw/openclaw/issues/160762) — Telegram + `claude-cli`：轮次中途消息被完全丢弃；排空后的 prompt 携带*上一条*消息正文（数据/消息丢失）。需要更多信息。
6. [#185819](https://github.com/openclaw/openclaw/issues/185819) — Windows 快速开始 Codex 入门在设备认证成功后因未激活的 harness 运行时工件而激活失败（2026.10.7）。
7. [#141413](https://github.com/openclaw/openclaw/issues/141413) — macOS 应用在 `gateway.auth.token` 为受保护 SecretRef 时，对本地网关省略认证令牌；标记为 `needs-security-review`。2 👍。

**P1 — 高**
- [#166017](https://github.com/openclaw/openclaw/issues/166017) — `mcp.sessionIdleTtlMs` 未设置时默认值未应用；MCP 运行时永不驱逐；spawn broker 触及 256 子进程上限，所有 exec/read/MCP 启动失败。**关联修复 PR 已开启。**
- [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram 不透明 `tgcb1:` 回调 payload 对非插件调用方被静默丢弃。**关联修复 PR 已开启。**
- [#177656](https://github.com/openclaw/openclaw/issues/177656) — 原生主机 exec 返回共享状态完整性准入错误且无子结果；在 2026.10.5 上至少 28 次失败。无修复 PR。
- [#172837](https://github.com/openclaw/openclaw/issues/172837) — Codex 子 agent 分配泄漏并无限重发。无修复 PR。
- [#181459](https://github.com/openclaw/openclaw/issues/181459) — 3.5 GB 的 agent DB 使启动变为赌博（打开 18 秒，启动 4–6 分钟，然后 `commit checkpoint expired`）；嵌入存储了 3 次。与 #166634 相关。
- [#176342](https://github.com/openclaw/openclaw/issues/176342) — 非正常关闭后，陈旧租约完整性门控串行化 agent 启动，导致数分钟不可用。
- [#148789](https://github.com/openclaw/openclaw/issues/148789) — 模型回退链在 harness 级会话失败时耗尽所有候选，并错误归因于最后一个回退。
- [#155396](https://github.com/openclaw/openclaw/issues/155396) — Codex 工具轮次在最终答案恢复期间因 `provenance_rejected` 而故障关闭。
- [#167667](https://github.com/openclaw/openclaw/issues/167667) — ChatGPT Responses 工具续接失败，报错“Rustponses cannot replay。”

**P2 但值得注意**
- [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL 检查点永不完成（38 条评论；很可能是 #166634/#181459 的根因之一）。
- [#163566](https://github.com/openclaw/openclaw/issues/163566) — 更新 + 修复后，`session-resource-loader` 每轮消耗 80–150 秒 CPU（2026.9.7）。
- [#187490](https://github.com/openclaw/openclaw/issues/187490) — 在 2026.10.9 稳定版验证期间观察到不稳定测试；策略要求在 `main` 上并行修复。

**回归主题：**版本 2026.9.6–2026.10.7 引入了多个事件循环/延迟回归（#160959、#162211、#163566）。这是本摘要中最大的单一稳定性信号。

## 6. 功能请求与路线图信号

今日可见的用户请求功能：
- [#44309](https://github.com/openclaw/openclaw/issues/44309) — **A2A 交接的单向派发模式**，无需回复往返。15 条评论，已存在 7 个多月，等待产品决策。是 `sessions_send` 语义扩展的有力候选。
- [#140244](https://github.com/openclaw/openclaw/issues/140244) — 为 `openclaw infer model run` 增加 `--prompt-file <path>`（避免大型自动化 prompt 触发 OS argv 限制）。
- [#163739](https://github.com/openclaw/openclaw/issues/163739) — 使 Control UI 的 `Ctrl+Shift+A`（归档）可配置；当前与浏览器快捷键冲突且无法重新映射。
- [#154524](https://github.com/openclaw/openclaw/issues/154524) — 将聊天消息暴露为标题，供屏幕阅读器导航。
- [#141295](https://github.com/openclaw/openclaw/issues/141295) — 为中断/取消的待处理输入提供 TTL/手动清除；这些输入当前会在压缩后向每一轮注入陈旧上下文。
- [#141973](https://github.com/openclaw/openclaw/issues/141973) — `tools.exec.notifyOnExit` 应支持仅失败/仅用户请求模式，并停止转发排空时 SIGTERM 退出。
- [#150743](https://github.com/openclaw/openclaw/issues/150743) — Tencent 交接停滞后的 QQ 渠道长期支持澄清（生态/可持续性担忧，不是 bug）。

**预测：**由于 #187699 已开启，**通过 `tools.invoke` 调用 MCP 工具**最可能落地到下一个补丁/次版本。隐藏标签页轮询批次（#187686/#187687/#187691/#187693）似乎已为下一个 2026.10.x 做好准备。单向 A2A 派发（#44309）和 Control UI 快捷键可配置性（#163739）需要产品决策，不太可能在紧接的下一个版本中出现。

## 7. 用户反馈摘要

**痛点（不满驱动因素）：**
- **启动与恢复可靠性**是主要抱怨。多名用户描述网关启动需数分钟、进入重启循环，或在崩溃/睡眠后使 agent 永久 `UNAVAILABLE`（#160959、#162211、#166634、#158592、#181459、#176342）。这是生产阻塞问题，而非表面问题。
- **潜在数据丢失：**Telegram 消息在轮次中途被丢弃并替换为上一条消息正文（#160762）；#126950、#142037、#156070、#149985 也带有消息丢失标签。
- **长会话的成本/性能：**Anthropic 缓存卡在约 46k 工具+系统前缀，每轮约 377–386k `cache_create`（#140129）——直接的成本抱怨。
- **资源泄漏：**MCP 子进程填满 256 上限（#166017）；Codex 子 agent 分配累积（#172837）；待处理输入永不过期（#141295）。
- **配置/入门摩擦：**Windows Codex 入门（#185819）、缺少 `modelProvider` 的 `registerCliBackend` 被静默丢弃（#172969），以及迁移在 JSON 格式错误时静默成功（#187177 / 底层问题）。

**积极信号 / 满意度：**
- 用户正在提交高质量、深度埋点的报告（许多被评为 🦞 diamond lobster / 🐚 platinum hermit，附源码级复现和日志），这表明技术投入度高的用户群愿意帮助分诊。
- 多名报告者明确验证修复并更正自己早先的假设（#120385、#143784），显示出协作式维护文化。
- Web UI 和可访问性打磨请求（#163739、#154524、#147732、#177094）表明用户正从“它能不能用”转向日常主力工具的易用性——一个成熟度信号。

## 8. 积压观察

存续时间长或缺少维护者行动的重要条目：

| 条目 | 存续时间 / 状态 | 关注点 |
|---|---|---|
| [#44309](https://github.com/openclaw/openclaw/issues/44309) — 单向 A2A 派发 | 创建于 2026-03-12（约 7.5 个月），15 条评论 | 最古老的高互动功能请求；`needs-product-decision`，无负责人。 |
| [#120385](https://github.com/openclaw/openclaw/issues/120385) — 用于 cron 的 Code Mode 目录不完整 | 创建于 2026-08-08，21 条评论，已停滞 | 作者于 2026-10-21 更正范围；仍无修复 PR。 |
| [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram 不透明回调丢弃 | 创建于 2026-08-21，11 条评论，P1 | 有关联的已开启 PR 但未合并；影响为消息丢失。 |
| [#127195](https://github.com/openclaw/openclaw/issues/127195) — 16 个调用点发出相同错误且无原因码 | 创建于 2026-08-21，8 条评论 | 虽为 P2 但直接损害生产可诊断性；标记为 `fix-shape-clear` 却未分配。 |
| [#124306](https://github.com/openclaw/openclaw/issues/124306) — exec 审批卡不标记挂载点目标 | 创建于 2026-08-15，6 条评论 | 安全/UX 缺口：显示语法风险，隐藏波及范围风险。 |
| [#136482](https://github.com/openclaw/openclaw/pull/136482) — 记忆搜索争用 / 重建索引泄漏（XL） | 自 2026-09-02 开启 | 大型架构 PR，`needs proof`，`merge-risk: session-state/availability`。解决 #150635/#164294 背后的根因。 |
| [#147886](https://github.com/openclaw/openclaw/pull/147886) — Feishu markdown 表格模式（XL） | 自 2026-09-14 开启 | `merge-risk: compatibility`，已有截图证明；渠道用户因文档化配置被拒绝而受阻。 |
| [#150635](https://github.com/openclaw/openclaw/issues/150635) — 召回驱逐阻碍 dreaming 提升 | 创建于 2026-09-17，21 条评论 | `needs-product-decision`；记忆流水线正确性。 |
| [#140129](https://github.com/openclaw/openclaw/issues/140129) — Anthropic 缓存卡在 46k 前缀 | 创建于 2026-09-06，15 条评论 | 影响成本；无修复 PR。 |

**维护者关注优先级：**（1）为 P0 事件循环/崩溃恢复集群（#160959、#162211、#166634、#158592）指派负责人，它们很可能在启动捕获、WAL 检查点和运行时发布方面共享根因；（2）解除两个已开启关联 PR 的 P1（#166017、#126950）的阻塞；（3）解决 #44309 和 #150635 长期停滞的产品决策，使分诊标签不再累积；（4）在 #136482 和 #147886 腐化前审查这两个 XL PR。

---

*数据基础：过去 24 小时内有 500 个 issue 和 500 个 PR 更新；本摘要分析提供的按评论数排序的前 50 个 issue 和前 30 个 PR。严重性层级（P0/P1/P2）和 `clawsweeper` 标签取自 issue 标签。*

---

---

## 横向生态对比

# 跨项目对比报告 — 个人 AI Agent 与助手开源生态
**数据日期：** 2026-10-29 · **项目：** OpenClaw、Hermes Agent、IronClaw、QwenPaw、ZeroClaw

---

## 1. 生态概览

2026-10-29 的开源个人 AI Agent 格局分化为一个超大规模参考实现（**OpenClaw**，24h 内触及 500 个 Issue + 500 个 PR）和一簇发布列车驱动、中等规模的项目（**ZeroClaw**、**QwenPaw**、**Hermes**），后者各自正在稳定一个特定的架构命题。窗口内没有任何项目发布版本——每个活跃项目都处于加固或 beta 预览阶段，这表明生态正围绕可靠性而非功能速度进行整合。五大项目中，主导投资领域已从“agent 能做什么”转向**会话持久性、多租户隔离、插件供应链信任和静默故障消除**——这是技术栈从演示级走向生产级的经典信号。值得注意的是，活跃度极不均衡：IronClaw 实际上闲置，而 OpenClaw 的运行量约为其 500×，这意味着“生态健康度”必须按项目解读，而非按总量解读。

---

## 2. 活动对比

| 指标 | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| Issue 更新数（24h） | 500（142 已关闭 / 358 开放） | 50（2 已关闭 / 48 开放） | 1（1 开放 / 0 已关闭） | 13（6 已关闭 / 7 开放） | 50（9 已关闭 / 41 开放） |
| PR 更新数（24h） | 500（224 已合并 / 276 开放） | 50（5 已合并 / 45 开放） | 0 | 38（12 已合并 / 26 开放） | 50（14 已合并 / 36 开放） |
| **PR 关闭率**（24h） | **44.8%** | 10.0% | — | **31.6%** | **28.0%** |
| **Issue 关闭率**（24h） | 28.4% | 4.0% | 0% | **46.2%** | 18.0% |
| 发布状态 | 无发布；稳定参考 2026.10.9 | 无发布；0.22.x 系列 | 无发布 | 无发布；**2.3.0-beta.1** 周期 | 无发布；**v0.9.0** 加固中 |
| 开放 P0 / S0 项 | 前 50 样本中 **≥7 P0** | 若干严重（数据丢失） | 0 | 2 个高严重度 beta 阻塞项 | 1 个开放 S0/P1 |
| **健康评分**（综合，1–10） | **6.5** | **5.0** | **3.5** | **7.5** | **8.0** |
| 健康评分理由 | 吞吐量巨大，但 P0 集中 + 零发布产出 | 参与度高但受接收/分诊限制；PR 关闭率 10% | 闲置；治理 Issue 未答复 | 快速当日修复；beta 打包回归 | 发布列车管理最佳；安全积压待处理 |

*健康评分 = 24h 关闭吞吐、发布就绪进展、开放严重度计数和响应速度的加权综合。方向性参考，非维护者评级。*

---

## 3. OpenClaw 的定位

**相较同行的优势**
- **社区规模：** 一天内触及 500 个 Issue + 500 个 PR，约为每个同行吞吐量的 **10–500×**。它拥有最深的分诊流水线（带标签的严重度、`clawsweeper` 自动化、专门的发布验证 Issue）——此处唯一具备正式化发布验证与 extended-stable 回移流程的项目（#187490、#187252、#187701）。
- **广度：** MCP 工具调用、多渠道（Telegram、Discord、Feishu、Mattermost）、Codex/Claude 迁移、memory/dreaming/RAG 子系统——本集合中最广的表面。
- **参考地位：** 它是事实上的“核心参考”，同行的 bug 分类也呼应其子系统。

**技术路线差异**
- **以 Gateway 为中心的架构**，配有大事件循环和进程内插件捕获——这既是其能力广度的来源，也是其最大 P0 的来源（#160959 事件循环阻塞、#162211 40–200s 启动卡顿、#164294 WAL checkpoint 饥饿）。
- **大规模下基于 SQLite 的会话/状态存储**（#181459 报告 3.5 GB 数据库）——同行还处在这条曲线的更早阶段，尚未触及同样的持久化上限。

**社区规模对比**
| | OpenClaw | 次大规模（ZeroClaw/Hermes） | 比例 |
|---|---|---|---|
| 每日 PR 吞吐量 | 500 | 50 | **10×** |
| 每日合并 PR 数 | 224 | 14 | **16×** |
| 参与度高的长线程 Issue（最高项评论数） | 64 | 34 | ~2× |

**评估：** OpenClaw 拥有无可匹敌的漏斗顶端参与度和最成熟的流程，但其 **P0 稳定性债务现在已是其决定性风险**——事件循环饥饿、崩溃恢复不可用、WAL checkpoint 失败，恰好在信任最重要的规模上构成生产阻塞。

---

## 4. 共同技术重点领域

两个或更多项目共同出现的需求：

| 主题 | 项目（具体需求） |
|---|---|
| **会话生命周期、恢复与持久化** | **OpenClaw**（崩溃后 `UNAVAILABLE`、检查点过期、状态竞争误分类）；**Hermes**（无法恢复的远程会话、`HERMES_HOME` 失效、profile 不匹配拒绝）；**ZeroClaw**（kill 后会话卡在 `running`——已修复 #11523）；**QwenPaw**（压缩/模型作用域 #8464）。*通用需求：跨重启/跨交互面可移植、可恢复的会话身份。* |
| **多租户与密钥隔离** | **Hermes**（`multiplex_profiles` 下的 `UnscopedSecretError`——一个根因，4+ 高严重度报告）；**ZeroClaw**（按发送方 RBAC #5982、按 agent 知识图谱 #9647、S0）；**OpenClaw**（`gateway.auth.token` SecretRef 处理 #141413）。 |
| **静默故障消除 / 可观测性** | **Hermes**（cron 投递连续多日丢失、配置段被静默删除 #146498）；**QwenPaw**（Feishu 图片被静默丢弃 #8150、Visual Compact 空操作 #8488）；**ZeroClaw**（误导性的 `plugin info [loads]` #11336）；**OpenClaw**（模型回退错误归因 #148789）。*反复出现的需求：显式错误/警告，而非降级但静默运行。* |
| **插件供应链与安装完整性** | **ZeroClaw**（manifest/组件生成不匹配 #11562、废弃的 staging 目录 #11792、锁文件权限 #11360）；**QwenPaw**（插件加载器崩溃 #8473、`importlib.invalidate_caches` 回归）；**OpenClaw**（MCP `tools.invoke` not_found 不匹配 #187699）。 |
| **打包 / 冻结构建 QA** | **QwenPaw**（PyInstaller 遗漏 `visual_compression/assets` #8488）；**OpenClaw**（Windows Codex 入门引导 #185819）；**ZeroClaw**（仅 CLI 构建覆盖 #11975/#11976）。 |
| **资源占用 / 成本治理** | **OpenClaw**（Anthropic 缓存卡在 46k 前缀 #140129，690MB）；**QwenPaw**（whisper 导入 → 690MB RSS #8490）；**Hermes**（ACP token 重复计数 #146475）；**ZeroClaw** 内存模型分离。 |
| **Provider 注册表与集成治理** | **IronClaw**（`providers.json` 纳入策略 #8161）；**ZeroClaw**（Obscurify.ai #12085）；**Hermes**（aimlapi #146493）；**QwenPaw**（max_tokens vs max_completion_tokens #8496）。 |

---

## 5. 差异化分析

| 维度 | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| **功能重点** | 最广的参考实现：MCP、多渠道、memory/dreaming、Codex/Claude 迁移 | 多交互面会话连续性 + 本地/远程混合执行（#18715，43👍） | 精选 Provider 注册表，治理优先 | 垂直打包应用（airport-SMS）、基于插件的本地模型管理、Piccolo 本地评分 | 安全优先的多租户运行时、插件信任、RBAC |
| **目标用户** | 生产 Gateway 运维者、高级用户 | 多主机 / 多 profile / 多交互面团队 | 注重隐私、最小核心采纳者 | 企业/区域部署、打包桌面 | 共享 agent / 多租户部署 |
| **架构** | 以 Gateway 为中心、大事件循环、进程内插件、SQLite 状态 | 远程 `serve` + Desktop/CLI/Telegram/Slack 控制平面；多路复用 profile | 最小核心、精选 `providers.json` | Plugin Provider 架构、PyInstaller 桌面、冻结构建、Rust？（Python） | Rust、基于组合的运行时构造器、仅 CLI 构建方案 |
| **发布姿态** | extended-stable 回移、正式验证 | 0.22.x 增量 | 无（闲置） | 2.3.0-beta 稳定化 | v0.9.0 架构/加固列车 |
| **成熟度信号** | 参与度最高、P0 债务最高 | 参与度高、关闭吞吐低 | 信号低、受治理门控 | 快速当日 issue→fix 循环、首次贡献者 | 关闭吞吐纪律最佳、安全积压仍开放 |

**关键结论：** 这些项目并非在同一轴线上竞争。OpenClaw 在*广度和社区*上竞争；ZeroClaw 在*安全隔离*；QwenPaw 在*打包/垂直部署*；Hermes 在*多交互面可移植性*；IronClaw 在*极简主义和精选*。这是健康的差异化，但也意味着**每个项目都有其他项目覆盖的盲点**——例如 ZeroClaw 的 RBAC 成熟度 vs OpenClaw 的渠道广度。

---

## 6. 社区势头与成熟度

**活跃度分层**

| 层级 | 项目 | 特征 |
|---|---|---|
| **T1 — 超大规模** | **OpenClaw** | 10× 同行吞吐；流程成熟；承载稳定性债务；今日零发布产出 |
| **T2 — 高速稳定化** | **ZeroClaw**、**QwenPaw** | 活跃发布列车（v0.9.0、2.3.0-beta）；同期关闭健康；贡献者落地修复 |
| **T3 — 高参与、受接收限制** | **Hermes Agent** | 社区质量强（深度复现，在 #18715 上 43👍）但 **PR 关闭率 10%**——约束是关闭吞吐，不是兴趣 |
| **T4 — 闲置 / 受治理门控** | **IronClaw** | 1 个 Issue，0 个 PR；贡献漏斗因策略答复受阻（#8161） |

**势头解读**
- **快速迭代：** OpenClaw（500/500，但主要是分诊 + 修复）、ZeroClaw（14 次合并）、QwenPaw（12 次合并 + 同日 6 次关闭）。
- **稳定化：** ZeroClaw（v0.9.0 加固）和 QwenPaw（2.3.0 beta）是最清晰的“发布高质量版本”轨迹。
- **有停滞风险：** Hermes——45/50 个 PR 和 48/50 个 Issue 仍开放；multiplexed-secret 系列（#130663/#146428/#146483/#146484）是单一根因修复，会实质性改善其比例。
- **休眠：** IronClaw——低信号既非正面也非负面，只是缺席。

---

## 7. 趋势信号

从社区反馈中提取的行业级模式，按信号强度排序：

1. **会话可移植性现在已是预期原语，而非功能。** 用户将 Desktop→远程 `serve`→Telegram→Slack 当作一个逻辑会话运行（Hermes #139818/#41220；OpenClaw #158592；ZeroClaw 跨交互面重命名恢复 #10373）。*开发者启示：将恢复/身份设计为一等、与交互面无关的契约——而非按交互面分别写逻辑。*

2. **多租户与按发送方/按 agent 隔离是基本门槛。** 最高严重度的开放项集中于此：ZeroClaw #9647（S0）、#5982（RBAC）；Hermes `UnscopedSecretError` 系列；OpenClaw SecretRef 认证。*启示：密钥作用域和所有权归属必须在运行时边界强制执行，而非配置层。*

3. **静默故障是头号信任摧毁者。** 每个项目都有系统“继续运行如同健康”的报告——图片丢失、cron 投递丢失、配置段被删除、插件加载误报。*启示：结构化错误/原因码和显式警告是竞争差异化因素，而非润色。*

4. **插件供应链完整性是新的攻击面。** Manifest/组件生成不匹配（ZeroClaw #11562）、加载器崩溃（QwenPaw #8473）、安装 staging 残留（ZeroClaw #11792）、MCP 工具/实现不匹配（OpenClaw #187699）。*启示：签名 manifest、生成版本固定和安装事务正成为预期。*

5. **资源占用和成本是一等投诉。** 690MB 导入副作用（QwenPaw #8490）、Anthropic 缓存前缀膨胀（OpenClaw #140129）、ACP token 重复计数（Hermes #146475）、分离的廉价记忆模型（QwenPaw #7719）。*启示：成本可观测性和惰性导入卫生应进入发布检查清单。*

6. **Provider 注册表正在硬化为治理问题。** IronClaw 明确精选 `providers.json`（#8161，引用 Novita 关闭）；ZeroClaw/Hermes/QwenPaw 都在处理 Provider PR。*启示：生态正分裂为“精选核心 + 外部 Provider 机制”，这有利于拥有成文纳入策略的项目。*

7. **打包/冻结构建 QA 是发布门禁，而非事后补丁。** QwenPaw 的 beta 发布时缺少必需资源；OpenClaw 和 ZeroClaw 都在处理仅 CLI/入门引导构建问题。*启示：CI 必须构建并对冻结产物做冒烟测试，而不仅是源代码。*

8. **本地优先 / 隐私保护推理是可信需求向量。** 本地 TTS（Hermes #145492）、本地 Piccolo 评分（QwenPaw #8463）、隐私 Provider 注册表压力（IronClaw）、Obscurify 集成（ZeroClaw #12085）。*启示：隐私正成为产品品类，而不仅是 Provider 选项。*

**给决策者的结论：** OpenClaw 同时是生态的引擎和最高风险组件；ZeroClaw 和 QwenPaw 是发布就绪度最健康的故事；Hermes 是强社区但受分诊瓶颈限制；IronClaw 休眠且受策略门控。2026-11 的跨领域技术任务明确——**持久会话身份、强制隔离、显式失败、可信插件供应链。**

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent — 项目摘要，2026-10-29

*数据窗口：过去 24 小时。基于 50 个更新 issue（48 个开放 / 2 个关闭）和 50 个更新 PR（45 个开放 / 5 个已合并或关闭）；issue/PR 详情取自按评论数排序的前 30/前 20 个样本。*

---

## 1. 今日概览

活动量仍然很高：过去 24 小时内有 50 个 issue 和 50 个 PR 被触及，但工作量严重偏向接收新问题和修 bug，而不是发布——没有切出新版本，只有 2 个 issue 和 5 个 PR 被关闭，而仍有 48 个和 45 个开放。主导主题是**会话生命周期完整性**：在远程后端、多路复用网关、配置档迁移和入口变化（Desktop/CLI/Telegram）之间恢复、迁移和还原会话；多份报告描述了永久无法恢复的会话。第二个问题簇涉及**多路复用配置档的密钥作用域**：`UnscopedSecretError` 会产生 500、卡死 cron 运行，并导致投递被静默丢弃。社区需求集中在混合部署——远程 agent 搭配本地工具执行（#18715，43 👍）——以及让会话可跨入口移植。总体而言，项目参与度很高，issue 报告质量也很高，但开放:关闭比例以及更新/迁移回归的数量表明，近期 0.22.x 系列周围正在累积稳定性债务。

---

## 2. 发布

无。过去 24 小时没有新版本发布；没有需要报告的变更日志、破坏性变更或迁移说明。

---

## 3. 项目进度

本窗口内只有一小部分更新 PR 落地（50 个中 5 个已合并/关闭），因此进度最好理解为*正在推进的修复*，而非已交付工作。

**已关闭 / 已完成**
- **[Issue #140298](https://github.com/NousResearch/hermes-agent/issues/140298)（已关闭）** — 会话权限准入忽略了 `user_id` 路由，导致所有普通私信都被以 `profile_mismatch` 拒绝。关闭此问题消除了一大类网关准入失败模式。
- **[PR #141018](https://github.com/NousResearch/hermes-agent/pull/141018)（已关闭）** — Bot Desktop 被遗弃的界面现在会在空闲窗口过后自行终止（#137057 的后续）；明确以“供讨论”方式开启，因此设计仍可能被重新审视。

**正在积极推进的修复 PR（开放）**
- 会话/压缩正确性：[#146190](https://github.com/NousResearch/hermes-agent/pull/146190)（将轮内压缩计数传播到活动会话）、[#144601](https://github.com/NousResearch/hermes-agent/pull/144601)（原地压缩期间保留已持久化的用户行）。
- 会话创建健壮性：[#146373](https://github.com/NousResearch/hermes-agent/pull/146373)（在相同 `request_id` 下重新询问丢失的 `session.create`）。
- 网关/配置启动：[#146130](https://github.com/NousResearch/hermes-agent/pull/146130)（远程配置后端首次读取时不再死锁）。
- Kanban 正确性：[#146235](https://github.com/NousResearch/hermes-agent/pull/146235)（按看板解析 DB，而非固定到 worker）、[#146491](https://github.com/NousResearch/hermes-agent/pull/146491)（为受管 worker 轮次绑定会话工作区）、[#146492](https://github.com/NousResearch/hermes-agent/pull/146492)（标记被截断的 ping 文本）。
- 平台投递：[#138795](https://github.com/NousResearch/hermes-agent/pull/138795)（Telegram 暂存入站消息的重放顺序）、[#143647](https://github.com/NousResearch/hermes-agent/pull/143647)（在相册批处理过程中保留附件内联标志）。
- 提供商/认证：[#139881](https://github.com/NousResearch/hermes-agent/pull/139881)（在 HTTP-200 SSE 错误中识别已撤销的访问令牌）。
- 工具/卫生：[#146496](https://github.com/NousResearch/hermes-agent/pull/146496)（`scripts/` 命名空间遮蔽）、[#146233](https://github.com/NousResearch/hermes-agent/pull/146233)（基于真实项目证据检测 Python recipe）、[#146387](https://github.com/NousResearch/hermes-agent/pull/146387)（子代理取消指标）。

---

## 4. 社区热点

| 条目 | 评论 / 👍 | 信号 |
|---|---|---|
| **[#139818](https://github.com/NousResearch/hermes-agent/issues/139818)** 远程后端 Desktop 会话无法恢复（`session.resume` → `not_found`） | 34 / 3 | 互动量最高的 bug；远程 Desktop + `hermes serve` 拓扑已在真实生产中使用 |
| **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715)** 远程 agent 搭配本地工具执行 | 32 / **43** | 数据集中最强的功能需求；标记为 `needs-decision`，自 2026-05-02 起开放 |
| **[#131238](https://github.com/NousResearch/hermes-agent/issues/131238)** Desktop 将助手回复渲染两次（流式气泡始终无法稳定下来） | 9 / 0 | 流式/UI 状态缺陷，具有可见的 UX 影响 |
| **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** Telegram `/resume` 应能呈现 Desktop/CLI 会话 | 8 / 0 | 跨入口的会话可移植性 |
| **[#135522](https://github.com/NousResearch/hermes-agent/issues/135522)** 具备写入能力的 MCP 工具审批在 ACP 中被静默拒绝 | 7 / 0 | 同意/信任边界；静默拒绝比报错更糟 |
| **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** Slack Code 按频道作用域的会话 | 6 / 2 | 与 Slack 新 agent 会话模型实现平台对齐 |
| **[#141574](https://github.com/NousResearch/hermes-agent/issues/141574)** Desktop 在 Omarchy/Arch 上打满 CPU（zygote 达 600%+） | 6 / 0 | Linux 上的性能/可靠性 |

**底层需求。** 评论集中度表明，用户正在运行*多入口、多主机、多配置档*部署：Desktop 客户端指向远程 `serve`，一台网关主机面向多个配置档，Telegram 作为移动端延续。反复出现且未被满足的需求是**可移植的会话身份**——在一个入口/主机上创建的会话，应能在另一个入口/主机上恢复并归属。第二个需求是工具使用的**显式同意与计量**（MCP 审批、ACP token 用量），也就是说，用户希望 agent 的行为可审计、可计费，且没有意外。

---

## 5. Bug 与稳定性

没有新披露的崩溃被归为 agent 核心循环上的数据丢失，但多份报告描述了*静默*失败和永久状态损坏，这些问题实际上更严重。排序如下：

**严重 — 数据丢失 / 不可恢复状态**
- **[#146478](https://github.com/NousResearch/hermes-agent/issues/146478)** `session.create` 在配置缺少 `model:` 时会生成无法恢复的会话——表现为 `storage_unavailable`，*无法通过清空 `state.db` 恢复*。尚无匹配的修复 PR（#146373 处理的是另一种 `session.create` 失败模式）。
- **[#146480](https://github.com/NousResearch/hermes-agent/issues/146480)** 移动 `HERMES_HOME` 会使所有本地会话失效（`receipt refused: profile_mismatch`），且没有重新绑定/迁移路径。无修复 PR。
- **[#146498](https://github.com/NousResearch/hermes-agent/issues/146498)** 写入部分配置视图会静默删除所有未包含的配置段，既没有跨进程锁，也没有备份。无修复 PR——影响面大。
- **[#146483](https://github.com/NousResearch/hermes-agent/issues/146483)** 当规范“Bot Chat”行被关闭时，Cron `deliver: bot-chat` 会被永久拒绝，且 payload 被静默丢弃——已跨 13 个配置档持续数天报告。无修复 PR。
- **[#139818](https://github.com/NousResearch/hermes-agent/issues/139818)** 远程后端 Desktop 会话在 serving 进程重启后无法恢复。无修复 PR；社区互动量最高。

**高 — 功能损坏 / 安全相关**
- **[#130663](https://github.com/NousResearch/hermes-agent/issues/130663)**、**[#146428](https://github.com/NousResearch/hermes-agent/issues/146428)**、**[#146484](https://github.com/NousResearch/hermes-agent/issues/146484)** — 多路复用配置档的 `UnscopedSecretError` 系列：dashboard `/api/ops/*` 返回 500，cron 收尾永久卡死，网关启用的投递插件静默停止覆盖投递。同一根因家族；一个系统性修复可能关闭全部三个问题。尚无修复 PR。
- **[#135522](https://github.com/NousResearch/hermes-agent/issues/135522)** 带有 `trust: untrusted` 的写入型 MCP 工具在 ACP 会话中被静默拒绝，且没有审批提示。同意边界 bug。
- **[#146475](https://github.com/NousResearch/hermes-agent/issues/146475)** ACP `PromptResponse.usage` 报告的是会话累计 token，且不包含缓存写入——客户端会对恢复的会话重复计数。计费准确性问题。
- **[#145156](https://github.com/NousResearch/hermes-agent/issues/145156)** Windows：在 `holographic` 记忆提供方下，Kanban worker 导入 numpy 时永久卡住；只会在 `max_runtime` 时被杀死。
- **[#141574](https://github.com/NousResearch/hermes-agent/issues/141574)** 在 Arch/Omarchy 上 GPU 进程启动失败后，Desktop CPU 饱和（单个 zygote 达 600–650%）。
- **[#142991](https://github.com/NousResearch/hermes-agent/issues/142991)** 自 v0.22.4 起的回归：`hermes chat --source desktop` 在连接模型前因 `invalid_params` 失败——会破坏脚本。
- **[#146385](https://github.com/NousResearch/hermes-agent/issues/146385)** 429 后的凭据池轮换忽略请求模型，因此另一个模型的冷却会阻塞所有凭据。

**中 — UX / 可观测性**
- **[#131238](https://github.com/NousResearch/hermes-agent/issues/131238)** 助手气泡重复（流式状态始终无法稳定）。
- **[#146430](https://github.com/NousResearch/hermes-agent/issues/146430)** Desktop 状态栏在切换后仍保留上一个会话的缓存命中百分比和 tokens/sec。
- **[#146436](https://github.com/NousResearch/hermes-agent/issues/146436)** 正常 OS 重启被误报为网关非正常死亡（SIGKILL/OOM）。
- **[#146499](https://github.com/NousResearch/hermes-agent/issues/146499)** 托管房间的工作证据在 standalone→gateway→standalone 交接后从不持久化（`WorkRecordPrefixError`）。
- **[#90068](https://github.com/NousResearch/hermes-agent/issues/90068)** 位于路径前缀反向代理后的 dashboard 始终无法挂载（`#root` 为空）。
- **[#110120](https://github.com/NousResearch/hermes-agent/issues/110120)** CLI agent 停止执行动作；自 2026-09-13 起标记为 `needs-repro`。

**稳定性解读。** bug 分布并非随机：它集中在 (a) 非默认拓扑下的会话持久化/恢复，(b) `multiplex_profiles` 下的密钥作用域，以及 (c) 更新/迁移边界（v0.22.4 `--source` 回归、多路复用迁移 #143150、home 路径移动 #146480）。这一模式指向迁移/向后兼容测试不足，而非孤立缺陷。

---

## 6. 功能请求与路线图信号

- **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — 远程 agent 搭配本地工具执行（32 条评论，43 👍，`needs-decision`）**。这是数据集中最清晰的路线图信号：将控制平面（远程 memory/skills/sessions/models）与执行平面（本地 terminal/tools）拆分。高反应数和长达六个月的存续时间，使其成为设计决策的首要候选。
- **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** — 通过 Telegram `/resume` 访问跨来源会话。相对价值而言实现成本低；将直接缓解 #139818 中看到的会话可移植性痛点。
- **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** — Slack Code 按频道作用域的会话，遵循 Slack 2026 年 8 月的 agent 会话模型。
- **[#145492](https://github.com/NousResearch/hermes-agent/pull/145492)** — 本地 TTS 提供商（Phonon、Ollama speech）。符合项目的本地优先定位；审查后很可能是合并候选。
- **[#146234](https://github.com/NousResearch/hermes-agent/pull/146234)** — MoA 作用域内的付费引用 + 每日支出告警；来自付费用户工作流的成本治理信号。
- **[#146493](https://github.com/NousResearch/hermes-agent/pull/146493)** — aimlapi 提供商集成（厂商提交，40 万+ 用户聚合器）。生态对提供商广度的需求。
- **[#146473](https://github.com/NousResearch/hermes-agent/issues/146473)** — 自动化依赖 DAG（当前 152 条边、9 个环）。内部可维护性信号；与 bug 聚类所暗示的架构漂移相关。

**对下一版本的预测（很可能是 0.22.6 补丁）：** 会话恢复/`storage_unavailable` 加固、多路复用配置档密钥作用域、kanban 工作区/DB 修复、压缩计数/行保留，以及远程配置启动死锁。更大的事项（#18715、Slack Code、跨来源 `/resume`）看起来属于 0.23+ 的材料。

---

## 7. 用户反馈摘要

**痛点**
- *会话连续性是第一大抱怨点。* 用户在进程重启、home 目录移动、多路复用迁移和入口变化后失去可恢复性——而且往往没有恢复路径。
- *静默失败比显式失败更糟。* Cron 投递连续数天丢失（#146483），MCP 审批在没有提示的情况下被拒绝（#135522），配置段在无警告下被删除（#146498）。用户在标题中反复强调“静默”。
- *升级回归。* v0.22.4 破坏了 `--source desktop`（#142991）；多路复用迁移破坏了恢复（#143150）；0.22.5 的 cron 投递重构破坏了插件覆盖（#146484）。
- *平台缺口。* Windows kanban 卡死（#145156），Linux desktop 透明度/CPU 问题（#145370、#141574），反向代理 dashboard（#90068）。
- *计量透明度。* ACP 用量重复计数（#146475）和跨模型凭据冷却（#146385）会影响按 prompt 计量或付费的用户。

**观察到的用例**
- 一台网关主机服务多个配置档（#146483 中有 13 个配置档；#143150 中有多配置档 Docker）。
- 远程 `hermes serve` + 带 basic auth 的本地 Desktop 客户端（#139818）。
- CLI 工作流中的本地模型（Qwen3.5:9B、GPT-OSS:20B）（#110120）。
- 通过 Telegram 进行移动端延续（#41220），通过 Slack 进行团队协作（#120618）。
- 厂商主动完成的集成工作（aimlapi，#146493）。

**满意度信号。** 报告异常详细且以复现为导向，第三方正在构建集成——这是一个健康信号。不满主要集中在*信任*上：状态消失、消息消失、配置在无通知的情况下缩减。

---

## 8. 积压关注

存在时间较长和/或阻塞决策、需要维护者关注的事项：

- **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715)** — 自 2026-05-02 起开放，`needs-decision`，43 👍。得票最高的单一请求；做出决定（接受/推迟/拒绝）将解锁社区。
- **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** — 自 2026-06-07 起开放，8 条评论，无 👍。低成本、高杠杆的会话可移植性修复。
- **[PR #48143](https://github.com/NousResearch/hermes-agent/pull/48143)** — 自 2026-06-18 起开放（`sweeper:blast-contained`），ComfyUI 工作流元数据。超过四个月未合并；一个小范围、受控的清理。
- **[#59220](https://github.com/NousResearch/hermes-agent/issues/59220)** — 自 2026-07-05 起开放。`skill_manage` 工具描述混淆了 `delete` 和 `remove_file`，导致意外且不可逆的技能删除。文档级修复，却会造成真实数据丢失后果。
- **[#90068](https://github.com/NousResearch/hermes-agent/issues/90068)** — 自 2026-08-19 起开放。`vite.config.ts` 中缺少 dashboard `base` 路径；文档中记录的反向代理模式已损坏。
- **[#97230](https://github.com/NousResearch/hermes-agent/issues/97230)** — 自 2026-08-28 起开放。Cron 在一次不明确的写入后重放已投递的分块；存在重复投递风险。
- **[#110120](https://github.com/NousResearch/hermes-agent/issues/110120)** — 自 2026-09-13 起开放，`needs-repro`。被报告为严重的 CLI 回归（“agent 不再执行动作”），但卡在复现上；值得维护者协助分诊。
- **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** — 自 2026-09-23 起开放；平台对齐请求，附有上游变更日志参考文档。

**健康度说明。** 50 个被触及的 PR 中仍有 45 个开放，50 个被触及的 issue 中仍有 48 个开放，因此本周的硬约束是关闭吞吐量——而不是贡献者兴趣。最高效的杠杆是多路复用密钥系列（#130663 / #146428 / #146483 / #146484），其中一个根因修复很可能解决四个高严重度报告。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目摘要 — 2026-10-29

**仓库：** [nearai/ironclaw](https://github.com/nearai/ironclaw)

---

## 1. 今日概览

IronClaw 度过了非常平静的 24 小时：1 个 issue 有更新，**0 个 pull request** 被创建、合并或关闭，**0 个 release**。唯一的活动是一个新开的 issue（[#8161](https://github.com/nearai/ironclaw/issues/8161)），询问是否会接受某个第三方推理提供商进入内置的 `providers.json` 注册表。本周期内没有代码被合并，因此没有功能推进，也没有修复落地。整体活跃度评估：**极低 / 维护性静默**——项目表面稳定但当天实际处于闲置状态，贡献者的精力目前集中在治理问题上，而非实现层面。

| 指标 | 数值 |
|---|---|
| Issue 更新数（24 小时） | 1（1 个开放，0 个关闭） |
| PR 更新数（24 小时） | 0（0 个开放，0 个合并/关闭） |
| Release 数 | 0 |
| 净代码变动 | 未观察到 |
| 维护者响应信号 | 当天唯一 issue 上 0 条评论 |

---

## 2. Releases

本周期内没有新 release。关于变更、破坏性变更或迁移说明均无可报告的内容。

---

## 3. 项目进展

今天没有合并或关闭的 pull request，因此：

- **没有功能推进。**
- **没有通过合并代码修复的 bug。**
- **没有 PR 审查吞吐量**可供衡量。

唯一的前瞻性信号是程序性的：一位潜在贡献者希望在投入 PR 之前获得预批准（[#8161](https://github.com/nearai/ironclaw/issues/8161)），并明确引用此前 Novita provider PR（[#1115](https://github.com/nearai/ironclaw/pull/1115)）被关闭一事，作为 `providers.json` 不应无限扩张的证据。这表明项目当前的贡献通道受阻于一个策略层面的答复，而非工程能力。

---

## 4. 社区热点话题

**1. [#8161 — “你们会接受 Obscurify.ai 作为命名提供商吗？”（开放）](https://github.com/nearai/ironclaw/issues/8161)**
- 作者：tonydavis629（自述为该提供商的运营者）
- 创建/更新时间：2026-10-28 · 评论：0 · 反应：0

这是唯一活跃的讨论串，尽管零互动，却默认成为“最热”条目。其实质内容比其指标更重要：

- 作者引用了**已关闭的 Novita PR（[#1115](https://github.com/nearai/ironclaw/pull/1115)）**，当时给出的理由是提供商列表“不应扩张到包含每一个提供商”。贡献者正将这一先例视为一项隐式策略。
- 作者以注册表**已包含注重隐私的条目**（例如 Tinfoil）为由，论证将 Obscurify.ai 归入同一类别是合理的。

**深层需求：** 贡献者缺乏针对 `providers.json` 的成文、可预期的准入标准。所问的问题实际上不是“Obscurify.ai 好不好？”，而是“纳入规则是什么？”——这是治理/文档层面的缺口，而非技术问题。这个问题每多一天未获答复，潜在贡献者就面临着据称 Novita 所遭遇的同样的白白投入风险。

---

## 5. Bug 与稳定性

**今天没有报告或更新任何 bug、崩溃或回归。** 严重性排序：不适用。

解读时的注意事项：24 小时内零 PR 活动、零 issue 报告，这是一个*低信号*的稳定性读数，而非加固的正面证据。应将其理解为“没有新的负面信号”，而非“已验证稳定”。

---

## 6. 功能请求与路线图信号

今天有一个隐式功能请求：

- **将 Obscurify.ai 作为命名提供商添加到 `providers.json`**（[#8161](https://github.com/nearai/ironclaw/issues/8161)）——这是一项注册表/配置新增，而非代码路径变更。

**对下一版本的预测：** 不太可能原样出现。所引用的先例（Novita PR 因避免列表膨胀而被关闭）表明维护者倾向于*精心筛选*内置列表，而非按请求逐一扩张。更可能的近期结果是：

1. 一份成文的提供商纳入策略或准入标准（文档变更）。
2. 转向**外部/社区提供商机制**（插件、用户自备配置或第三方注册表），从而让核心列表保持精简。
3. 只有在此之后，才会选择性纳入满足新标准的注重隐私的提供商。

请关注 #8161 上维护者的回复，这是最清晰的路线图信号。

---

## 7. 用户反馈摘要

今天的样本量仅有一位用户，且该用户是**带有商业利益的提供商运营者**，因此仅供参考方向。

- **痛点：** 对贡献准入标准的不确定性 → 担心做出的 PR 因策略原因被关闭。作者在开 PR *之前*就明确提问，这是对规则不清晰的理性反应。
- **所暗示的用例：** 将注重隐私的推理提供商视为 IronClaw 提供商注册表中一个被认可的类别——也就是说，用户关心的是保护隐私的模型访问，而不仅仅是提供商的绝对数量。
- **满意度/不满意度：** 没有明确的满意度数据；没有关于 bug 或行为的抱怨。轻微的摩擦属于流程层面，而非产品层面。

---

## 8. 积压事项关注

- **[#8161](https://github.com/nearai/ironclaw/issues/8161)** —— 开放，0 条评论，约 1 天未获维护者回复。尚不构成“长期未答复”，但它**阻碍了一位志愿者的贡献决策**，且是唯一在途事项；今天一个简短的政策答复就能产生超常的杠杆效应。
- **[#1115](https://github.com/nearai/ironclaw/pull/1115)**（Novita provider PR，已关闭）—— 值得由维护者撰写一份关闭理由的总结。其理由已被新贡献者当作事实上的策略引用，这意味着该先例正在缺乏权威、可链接的声明的情况下流传。

**建议维护者采取的行动：** 回复 #8161，要么 (a) 给出 `providers.json` 条目的准入标准，要么 (b) 指向预期的外部提供商路径——并考虑将该答复转化为文档，以避免同一问题反复出现。

---

规则：
- 仅输出翻译内容。不要前言，不要解释，不要在整体输出外用 Markdown 围栏包裹。
- 完全保留 Markdown 结构：标题、表格（包括列对齐行）、列表、引用块、粗体/斜体、水平分割线、emoji。
- URL、链接目标、代码片段、代码块、数字和日期保持原样。
- 项目名称、仓库 slug、用户名、版本标签、文件路径和 API/配置标识符保持原样——不要翻译它们。
- 类似 #12345 的 Issue/PR 引用及其链接文本保持原样。
- 使用自然的技术中文，采用中文开发者简报的行文语域——而非逐字直译。

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 — 2026-10-29

## 1. 今日概览

QwenPaw 正处于紧张的 **2.3.0-beta 稳定化周期**：过去 24 小时内共触及 38 个 PR（26 个开启、12 个已合并/关闭），对应 13 条 issue 更新（7 个开启、6 个关闭），且**未发布新版本**。吞吐量很高，但开启的 issue 主要集中在 **beta 打包与运行时回归**上——最突出的是 #8473（在 2.3.0-beta.1 上全部 12 个后端插件均加载失败）和 #8488（桌面版构建未包含视觉压缩资源）。维护者今天关闭了两个长期存在的缺陷/增强项（#8464、#8150），并合并了对应的对话模型修复（#8465），显示出对高价值报告的响应式分诊。整体健康度为**活跃但发布受阻**：2.3.0 beta 仍至少存在两个高严重度、未修复的缺陷，与此同时架构性工作（基于插件的本地模型管理、Piccolo 评分、独立记忆写入模型）仍在并行推进。

---

## 2. 发布

过去 24 小时内无新发布，也未提供发布历史。*（因数据可得性，本节省略。）*

---

## 3. 项目进展

### 今日合并 / 关闭的 PR（前 20 样本中可见 6 个；共 12 个）

| PR | 范围 | 影响 |
|---|---|---|
| [#8465](https://github.com/agentscope-ai/QwenPaw/pull/8465) `fix(models)` XL | 在所有运行时路径上遵循会话级模型设置，包括斜杠命令分发 | **修复 #8464**——`/compact` 现在使用会话模型（M2），而非持久化的 agent 默认模型（M1） |
| [#8358](https://github.com/agentscope-ai/QwenPaw/pull/8358) `fix(providers)` M | 使 Console 的“thinking”默认值与实际的 OpenAI 兼容线级参数（`thinking_enabled`、effort 等级）对齐 | 消除了自定义 Chat Completions 模型在配置/行为上的静默不一致 |
| [#8492](https://github.com/agentscope-ai/QwenPaw/pull/8492) `fix(security)` M | 防止 JEV 模式下 Code Mode 的误报审批 | 安全防护正确性——在不削弱审查的前提下减少虚假审批提示 |
| [#8253](https://github.com/agentscope-ai/QwenPaw/pull/8253) `feat(website)` XXXL | `/evaluation/` 上的双语 JSON 驱动评测排行榜，含 SVG 成本/性能图表与帕累托前沿 | 面向社区的基准透明度 |
| [#8494](https://github.com/agentscope-ai/QwenPaw/pull/8494) `feat(airport-sms)` S | 搭建 `packages/airport_sms` 并接入 CI | 启用新的垂直 SMS-agent 包边界 |
| [#8495](https://github.com/agentscope-ai/QwenPaw/pull/8495) `feat(airport-sms)` XXXL | 完整 SMS Agent 技术栈（PR01–PR15：smoke、治理、console、WeCom） | 大型领域特定功能落地 |

### 今日关闭的 Issue
- **#8464**（bug，4 条评论）——`/compact` 忽略了会话模型切换；由 #8465 解决。
- **#8150**（bug，3 条评论）——飞书入站富文本图片被静默丢弃。
- **#4356**（enhancement）——更细粒度的文件防护 / 只读路径。
- **#4129**（enhancement）——面向文件与上下文的 `/rewind` 式回滚。
- **#7740**（enhancement）——Hub 模式管理员密码重置。
- **#8459**（enhancement）——在 Console 中更便捷地访问未读推送消息。

**要点：** 今日合并的工作集中在**配置正确性**（模型解析、thinking 参数）和**安全审查准确性**上，外加一项重要的文档/站点功能。两个历时数月的增强请求（#4356 始于 5 月、#4129 始于 5 月）终于关闭，表明在 beta 工作的同时也在进行积压项梳理。

---

## 4. 社区热点

1. **[#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — 4 条评论（最活跃 issue）** · 已关闭
   手动 `/compact` 忽略了会话级模型覆盖，始终使用 `agent.json` 中的 `active_model`；在严格 provider（对不支持的参数返回 400）下，无论用户选择哪个模型，压缩都会失败。底层需求是为普通轮次、斜杠命令和后台任务提供**统一的模型解析路径**——每当新的入口绕过 `AgentBuilder` 时，这类 bug 就会复发。由 #8465 当日修复。

2. **[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — 3 条评论** · 已关闭
   带内嵌图片的飞书入站 `post` 消息只投递了文本，**静默丢弃了图片**——既无下载，也无警告。这里的需求是**渠道对等性与可观测的失败**：用户期望多模态入站内容与出站能力相匹配（参见所引用的 #2792 了解出站方向）。静默截断比显式报错更糟，因为 agent 会在不完整的上下文中作答。

3. **[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) — 2 条评论** · 开启
   `2.3.0-beta.1` 的 Windows PyInstaller 桌面构建遗漏了 `visual_compression/assets`，导致每次符合条件的模型调用都抛出 `FileNotFoundError: atlas-gray-native.ts`，Visual Compact **静默失效**。这表明源码构建与冻结构建之间存在打包清单缺口。

4. **[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — 2 条评论** · 开启
   全部 12 个后端插件在启动时失败，原因是 `module_isolation.py:220` 中未加防护的 `importlib.invalidate_caches()`。报告者明确指出这**与 #8265 是同一根因、只是出现在新的调用点**——属于回归模式，而非一次性问题。

*注：数据集中 PR 评论数报告为 `undefined`，因此 PR 热度依据评审标签（`Under Review`）与规模推断，而非互动量。*

---

## 5. 缺陷与稳定性

按严重度排序，依据用户影响与影响范围：

| 严重度 | Issue | 状态 | 是否有修复 PR？ |
|---|---|---|---|
| **严重** | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — 2.3.0-beta.1：服务器启动时**全部后端插件加载失败**（12/12）；`module_isolation.py:220` 中未加防护的 `importlib.invalidate_caches()` 抛出 `TypeError`。与 #8265 同一根因，出现在新调用点。 | 开启 | 可能是 [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497)（容忍缓存刷新失败）与 [#8345](https://github.com/agentscope-ai/QwenPaw/pull/8345)（不中止插件安装）——两者均开启且重叠 |
| **高** | [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) — 桌面/PyInstaller 构建遗漏 `visual_compression` 图集 → 每次符合条件的模型调用都 `FileNotFoundError`；Visual Compact 静默失效。 | 开启 | 所提供数据中未见 |
| **高** | [#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490) — `GET /api/workspace/local-whisper-status` 会真正执行 `import whisper`，仅仅为了渲染设置页就把 `torch` + CUDA 拉入服务器进程（约 690 MB RSS，且永不释放）。 | 开启 | ✅ [#8491](https://github.com/agentscope-ai/QwenPaw/pull/8491)（首次贡献者，在探测中避免该导入） |
| **中** | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — `/compact` 使用了错误的模型，在严格 provider 下导致硬 400 失败。 | 已关闭 | ✅ [#8465](https://github.com/agentscope-ai/QwenPaw/pull/8465) 已合并/关闭 |
| **中** | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — 飞书入站图片被静默丢弃（数据丢失且无警告）。 | 已关闭 | 样本中未见 |
| **低** | [#8486](https://github.com/agentscope-ai/QwenPaw/issues/8486) — Console 工具调用 `<>` 图标左边缘被裁剪（缺少容器内边距）；在较高缩放下可见。 | 开启 | 无 |

**值得标记的模式：** 五个开启/高严重度 bug 中有三个（#8473、#8488、#8490）**特定于 2.3.0-beta.1 的安装路径**（冻结桌面构建、插件加载器、可选依赖探测）。这表明 QA 缺口在于**打包与惰性导入卫生**，而非核心 agent 逻辑。同样相关的是：[#8496](https://github.com/agentscope-ai/QwenPaw/pull/8496)（开启）解决了 `max_tokens` 与 `max_completion_tokens` 冲突导致滚动摘要请求 HTTP 400 的问题——另一项 provider 兼容性稳定性修复正在推进中。

---

## 6. 功能请求与路线图信号

开启的请求：
- **[#8478](https://github.com/agentscope-ai/QwenPaw/issues/8478)** — 将内置 DOCX/PPTX/XLSX 技能同步至 Anthropic 2026 年 7 月的 Office 技能更新，同时保留 QwenPaw 的适配。这表明**技能时效性**已成为一项竞争性预期。
- **[#8481](https://github.com/agentscope-ai/QwenPaw/issues/8481)** — 提供 `QWENPAW_LANGUAGE` 风格的环境变量，在 Docker 初始设置时设定默认语言。改动小、面向部署、对国际化/企业推广杠杆作用大。
- **[#8498](https://github.com/agentscope-ai/QwenPaw/issues/8498)** — “install easily”提问（正文仅为模板）；信号弱，很可能是文档/上手引导缺口。
- 今日关闭：**#4356**（细粒度文件防护只读路径）、**#4129**（`/rewind` 备份/回滚）、**#7740**（Hub 管理员密码重置）、**#8459**（未读推送消息访问）。它们的关闭意味着这些要么已实现，要么被有意识地推迟。

开启 PR 中的路线图信号：
- **[#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499)**（WIP，XXXL）——将本地模型管理抽取为 **Plugin Provider**（下载、llama.cpp 安装、服务生命周期）。架构方向：更薄的主机 + 可插拔 provider，保留现有配置/模型文件。
- **[#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463)**（XL）——运行本地 **Piccolo** 模型，为 JEV 工具审查与 ReMe 压缩提供双向候选评分。以本地、隐私保护的方式替代托管审查模型。
- **[#7719](https://github.com/agentscope-ai/QwenPaw/pull/7719)**（XL，Under Review）——为 **ReMeLight 记忆写入**（`auto_memory` / `auto_dream` / `daily_paper` / `auto_fin`）提供独立且更便宜的模型。这是重度用户的直接成本控制诉求。
- **[#8485](https://github.com/agentscope-ai/QwenPaw/pull/8485)** — Console 中 Extension 应用的“Install app”按钮。

**对下一版本（2.3.0 正式版）的预测：** 插件加载器加固（#8497/#8345）、桌面资源打包（#8488）、惰性 whisper 探测（#8491）、会话级模型设置（#8465）、provider token 上限兼容性（#8496）。更大的条目（#8499 本地模型插件抽取、#8463 Piccolo、#7719 记忆模型）鉴于其 XXXL/XL 规模与开启的评审状态，看起来属于 **2.3.x/2.4** 范畴。

---

## 7. 用户反馈总结

**观察到的痛点：**
- **静默失败侵蚀信任。** #8150（图片被丢弃且无警告）、#8488（Visual Compact 静默失效）和 #8464（压缩因不透明的 400 失败）都有一个共同主题：系统在降低输出质量的同时仍表现得一切正常。用户明确要求提供警告/日志，而不仅仅是修复。
- **资源占用很重要。** #8490 中仅仅是*打开一个设置页*就导致 690 MB 的永久 RSS 增长，这对自托管/桌面用户是严重抱怨；而修复来自一位首次贡献者的 PR，说明社区有动力提供帮助。
- **Beta 打包质量。** 两位独立报告者在 `2.3.0-beta.1` 上遇到安装/构建层面的损坏（插件加载器、PyInstaller 资源），这比核心逻辑 bug 更能影响对 beta 的观感。
- **配置一致性。** #8464 与 #8358 都涉及*UI 显示的内容*与*运行时实际行为*之间的差异——这是多 provider、多模型产品中反复出现的满意度风险。
- **部署/运维基础。** #7740（Hub 模式管理员密码重置）与 #8481（默认语言环境变量）来自为团队运行 QwenPaw 的运维人员，而非单用户——产品正被用于托管部署，却缺乏相应的管理员能力。
- **高级用户工作流缺口。** #4129（`/rewind`）与 #4356（只读防护路径）表明用户在进一步推进 agent 自主性，需要**可逆性与最小权限控制**才能感到安全。

**满意度信号：** #8464 当日快速周转（issue → 修复 PR → 关闭）、#8150 在开启 20 天后得到响应式分诊，以及多位首次贡献者（#8491、#8345、#8400、#8494），表明贡献管道健康且友好。

---

## 8. 积压项观察

需要维护者关注的条目，按陈旧度 × 重要性排序：

| 条目 | 时长 | 重要性 |
|---|---|---|
| [#7719](https://github.com/agentscope-ai/QwenPaw/pull/7719) `feat(memory)` XL，Under Review | 约 47 天（自 2026-09-12） | 需求明确的成本控制功能；评审延迟过长有贡献者流失风险 |
| [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) `fix(memory)` M，Under Review | 约 42 天（自 2026-09-17） | 阻止自动记忆召回载荷泄漏到渠道输出；概念上与 [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) 重叠——**同一缺陷的两种竞争方案**，需要决策 |
| [#8308](https://github.com/agentscope-ai/QwenPaw/pull/8308) `test(api)` XS | 约 11 天 | 一个简单的测试对齐 PR 卡在开启状态；对 CI 可靠性而言是廉价收益 |
| [#8309](https://github.com/agentscope-ai/QwenPaw/pull/8309) `fix(cli)` M，Under Review | 约 11 天 | 激活前的 Ollama 模型发现——本地模型用户的上手阻塞项 |
| [#8345](https://github.com/agentscope-ai/QwenPaw/pull/8345) `fix(plugins)` S，首次贡献 | 约 9 天 | 与 [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) 重叠；**合并**以避免针对 #8473 的重复/冲突修复 |
| [#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400) `fix(skill)` XS，首次贡献 | 约 6 天 | 内置 `make-skill` 被 QwenPaw 自身的安全扫描器拦截——一个自伤的误报，削弱了对扫描器的信任 |
| [#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463) `feat(models)` XL | 约 1 天 | 尚不陈旧，但规模大且具战略重要性（本地 Piccolo 推理）；留意评审容量 |
| [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) 插件加载失败 | 1 天 | **与 #8265 同一根因，出现在新调用点**——需要系统性修复（防护所有 `invalidate_caches()` 调用点），而非逐点打补丁 |

**给维护者的建议：**（1）为 `invalidate_caches` 这一类失败指定唯一权威修复并关闭重复项；（2）解决 #7835 与 #8457 的记忆注入设计分歧；（3）将 #8488 与 #8491 纳入 2.3.0 发布检查清单，以免 beta 带着静默禁用的压缩功能和 690 MB 的导入副作用发布。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-10-29

## 1. 今日概览

ZeroClaw 在 2026-10-29 仍保持高度活跃：过去 24 小时内 **50 个 issue 和 50 个 PR 有更新**，其中 **14 个 PR 已合并/关闭**、**9 个 issue 已关闭**，**没有新版本发布**。当日关闭的工作集中在 v0.9.0 加固：运行时会话恢复、Web 聊天状态保留、OIDC 凭据处理、feature-graph 清理、仅 CLI 构建，以及插件/CLI 正确性。活跃队列主要由围绕多租户、插件信任边界和内存隔离的高风险安全与架构工作占据。整体项目健康度在吞吐量和发布就绪度方面表现强劲，但仍有若干长期存在的 **P1/S0 安全问题**处于开启状态，需要维护者持续关注。

---

## 2. 发布

**过去 24 小时内没有新版本发布。** issue 讨论中最新引用的已发布构建是 **v0.8.6**，而多个 PR 和 issue 打上了 `release:v0.9.0` 标签，表明正在积极准备下一个版本。

---

## 3. 项目进展

### 运行时与会话可靠性
- **[PR #11523](https://github.com/zeroclaw-labs/zeroclaw/pull/11523) — 已关闭**  
  修复守护进程在一轮对话中途被杀后，会话永久停留在 `running` 状态的问题。关闭 **[Issue #11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432)**。

### Web 仪表盘与 ZeroCode 用户体验
- **[PR #11571](https://github.com/zeroclaw-labs/zeroclaw/pull/11571) — 已关闭**  
  在聊天 hydration 替换本地状态时，保留正在发送中的用户提示词。关闭 **[Issue #11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517)**。
- **[PR #11511](https://github.com/zeroclaw-labs/zeroclaw/pull/11511) — 已关闭**  
  使 ZeroCode Config 的保存/取消行为保持一致。关闭 **[Issue #11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486)**。
- **[PR #12100](https://github.com/zeroclaw-labs/zeroclaw/pull/12100) — 未关闭**  
  后续修复：在实体启用开关切换后，保持 `FieldForm` 草稿状态同步。

### 构建、CI 与仅 CLI 支持
- **[PR #11269](https://github.com/zeroclaw-labs/zeroclaw/pull/11269) — 已关闭**  
  阻止 `metrics` 和 `embedded-web` 隐式启用完整网关。关闭 **[Issue #11969](https://github.com/zeroclaw-labs/zeroclaw/issues/11969)**。
- **[PR #11973](https://github.com/zeroclaw-labs/zeroclaw/pull/11973) — 已关闭**  
  在不带网关的构建中，恢复删除/重命名时的 agent 所属状态级联。关闭 **[Issue #11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970)**。
- **[PR #11974](https://github.com/zeroclaw-labs/zeroclaw/pull/11974) — 已关闭**  
  仅在构建网关时运行依赖网关的测试。
- **[PR #11988](https://github.com/zeroclaw-labs/zeroclaw/pull/11988) — 已关闭**  
  在 SARIF 上传前丢弃被 `nosemgrep` 抑制的发现项。关闭 **[Issue #11987](https://github.com/zeroclaw-labs/zeroclaw/issues/11987)**。
- **[PR #11975](https://github.com/zeroclaw-labs/zeroclaw/pull/11975) — 未关闭** 和 **[PR #11976](https://github.com/zeroclaw-labs/zeroclaw/pull/11976) — 未关闭**  
  增加仅 CLI 构建指南，并通过产物大小棘轮补充 CI 覆盖。

### 安全与渠道
- **[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968) — 已关闭**  
  按 RFC 6749 对 OIDC 注册的 Basic 凭据进行表单编码。关闭 **[Issue #11957](https://github.com/zeroclaw-labs/zeroclaw/issues/11957)**。
- **[PR #11979](https://github.com/zeroclaw-labs/zeroclaw/pull/11979) — 已关闭**  
  仅在投递完成后记录渠道回复成功，防止被拒绝的发送产生虚假成功记录。

### 提供商、文档与依赖
- **[PR #11404](https://github.com/zeroclaw-labs/zeroclaw/pull/11404) — 已关闭**  
  稳定 macOS 测试中 fake Grok fixture 的时序。
- **[PR #11980](https://github.com/zeroclaw-labs/zeroclaw/pull/11980)** 和 **[PR #11963](https://github.com/zeroclaw-labs/zeroclaw/pull/11963) — 已关闭**  
  关闭了支持 v0.9.0 组合工作的运行时架构文档提案。
- **[PR #12018](https://github.com/zeroclaw-labs/zeroclaw/pull/12018) — 已关闭**  
  依赖升级，包含 35 个 Rust crate 更新。
- **[PR #12102](https://github.com/zeroclaw-labs/zeroclaw/pull/12102) — 未关闭**  
  新的 Dependabot 升级，包含 34 项更新。

---

## 4. 社区热点

快照中未填充 PR 评论数，因此以下排名由 issue 评论数驱动。

| 条目 | 活跃度 | 潜在需求 |
|---|---:|---|
| **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — 多租户部署的按发送方 RBAC | 11 条评论 | 多租户隔离与最小权限访问。讨论已将范围收窄为在现有 agent/risk-profile 模型上构建发送方角色；#11068 中已有草稿。 |
| **[Issue #10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)** — 证明可从发布二进制安装 Discord 插件 | 9 条评论 | 可信的插件分发以及从已发布制品安装。已在 v0.8.6 上验证真实的 Discord 入站/出站投递，但该 issue 仍受阻。 |
| **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — 经 schema 校验的记忆整合 | 7 条评论 | 跨提供商记忆可靠性。当前提示内 JSON 解析在不同模型上很脆弱；用户需要有界回退和 schema 校验。 |
| **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — 知识图谱缺少按 agent 归属 | 5 条评论 | 单一共享知识图谱使任何 agent 都能读取或修改另一个 agent 的数据。这是一个 S0 安全/数据丢失问题。 |
| **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack 的“is thinking…”状态自 v0.8.5 起缺失 | 5 条评论 | Slack 线程中的渠道 UX 回归；用户希望在 agent 回合期间看到可见的进度指示。 |
| **[Issue #11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)** — 渠道、webhook、gateway 回合跳过技能审查/创建 | 4 条评论 | 学习循环在所有入口点保持一致，而不仅是 CLI/cron 运行。 |

**分析：** 社区最强烈推动的是 **安全隔离与运营信任**——多租户 RBAC、按 agent 的内存/知识所有权，以及插件安装完整性。第二个集群是 **跨渠道与跨界面的 UX 一致性**：Slack 进度指示、Web 聊天状态保留，以及 CLI 之外的技能学习行为。

---

## 5. Bug 与稳定性

按严重性/风险排序：

| 严重性 | 条目 | 状态 | 备注/修复 PR |
|---|---|---|---|
| **S0 / P1** | **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — 知识图谱没有按 agent 归属；任何 agent 都能读取/修改另一个 agent 的知识 | **未关闭** | 高风险数据隔离缺陷。未显示修复 PR。 |
| **S1** | **[Issue #11957](https://github.com/zeroclaw-labs/zeroclaw/issues/11957)** — OIDC 注册在 Basic 头中发送原始客户端凭据 | **已关闭** | 由 **[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968)** 修复。 |
| **P1** | **[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** — `gateway.pairing_dashboard` 被接受但未读取；配对码永不过期 | **未关闭** | v0.9.0 的安全加固。未显示修复 PR。 |
| **S2 / P2** | **[Issue #11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)** — 渠道/webhook/gateway 回合从不运行技能审查和创建 | **未关闭** | CLI/cron 之外的学习循环退化。 |
| **S2 / P2** | **[Issue #11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562)** — 插件发现可能在更新期间将某一代的 manifest 与另一代的组件配对 | **未关闭** | 在宽松/禁用签名模式下的插件完整性问题。 |
| **S2 / P2** | **[Issue #11820](https://github.com/zeroclaw-labs/zeroclaw/issues/11820)** — 插件实例设置/出站的 `config set` 仅写入磁盘，永远不会到达运行中的守护进程 | **未关闭** | 运行时与配置不一致。 |
| **S2 / P2** | **[Issue #11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336)** — `plugin info` / `plugin list --verify` 对运行时拒绝的插件报告 `[loads]` | **未关闭** | 误导性的 CLI 诊断信息。 |
| **S2** | **[Issue #11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517)** — 回合期间重新加载 Web 聊天会从屏幕和 localStorage 中丢弃提示词 | **已关闭** | 由 **[PR #11571](https://github.com/zeroclaw-labs/zeroclaw/pull/11571)** 修复。 |
| **S2** | **[Issue #11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432)** — 被杀死的守护进程使会话永远标记为 `running` | **已关闭** | 由 **[PR #11523](https://github.com/zeroclaw-labs/zeroclaw/pull/11523)** 修复。 |
| **P2** | **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack 思考状态自 v0.8.5 起缺失 | **未关闭，进行中** | 渠道 UX 回归。 |
| **S3 / P3** | **[Issue #11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360)** — 包锁文件可被其他本地账户读取 | **未关闭** | 可用性/安全加固。 |
| **S3 / P3** | **[Issue #11358](https://github.com/zeroclaw-labs/zeroclaw/issues/11358)** — `plugin migrate` 在没有包锁的情况下移动包 | **未关闭** | 并发/插件完整性。 |
| **S3** | **[Issue #11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792)** — 中断的插件安装会留下废弃的暂存目录 | **未关闭** | 清理/运维卫生。 |

**稳定性解读：** 当日关闭了若干有意义的回归问题，尤其是围绕会话状态和 Web 聊天的。然而，**插件状态完整性**和**跨 agent 内存隔离**仍是最大的未解决稳定性/安全主题。

---

## 6. 功能请求与路线图信号

### 可能的 v0.9.0 范围
显式标记为 `release:v0.9.0` 且已经收到活跃 PR 的条目：
- 运行时组合与基于能力的构造函数：**[Issue #10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993)**、**[PR #11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)**。
- 仅 CLI 构建与 feature-graph 正确性：**[PR #11975](https://github.com/zeroclaw-labs/zeroclaw/pull/11975)**、**[PR #11976](https://github.com/zeroclaw-labs/zeroclaw/pull/11976)**。
- OIDC 与网关配对的安全修复：**[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)**、**[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968)**。
- 无网关时的 agent 生命周期正确性：**[Issue #11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970)**、**[PR #11973](https://github.com/zeroclaw-labs/zeroclaw/pull/11973)**。

### 已接受但受阻或搁置
- **[Issue #10997](https://github.com/zeroclaw-labs/zeroclaw/issues/10997)** — 在引导期间安装并激活所选插件。**受阻。**
- **[Issue #10998](https://github.com/zeroclaw-labs/zeroclaw/issues/10998)** — 交付最小核心工具集和二进制大小证据。**受阻。**
- **[Issue #10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)** — 发布/证明可从发布二进制安装 Discord 插件。**受阻。**
- **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — 经 schema 校验的记忆整合。**搁置。**

### 较新的功能信号
- **[Issue #11890](https://github.com/zeroclaw-labs/zeroclaw/issues/11890)** — 整合证据门禁：在写入 Core 前，将记忆更新标记为持久/瞬态并引用用户文本。
- **[Issue #11561](https://github.com/zeroclaw-labs/zeroclaw/issues/11561)** — 在插件宿主内检查更新新增的插件权限。
- **[Issue #11793](https://github.com/zeroclaw-labs/zeroclaw/issues/11793)** — 仅所有者可访问的数据目录，以及检测组/所有用户可写的根目录。
- **[PR #12085](https://github.com/zeroclaw-labs/zeroclaw/pull/12085)** — 将 Obscurify.ai 添加为类型化的 OpenAI 兼容提供商。
- **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — 按发送方 RBAC，已接受但架构意义重大；除非进一步收窄，否则可能在 v0.9.0 之后。

**预测：** v0.9.0 将主要是一个 **加固与架构版本**——运行时组合、仅 CLI 正确性、插件/安全信任边界，以及网关功能分离——而不是一个广泛的功能版本。RBAC 和高级记忆整合更可能在组合与隔离基础稳定之后跟进。

---

## 7. 用户反馈总结

**用户报告的主要痛点：**

- **多租户隔离不足。** **[#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** 和 **[#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** 都指向同一担忧：agent 可以以过多共享权限运行。用户希望按发送方设置角色，并按 agent 拥有内存/知识。
- **插件安装和升级行为难以信任。** 用户报告了误导性的 `plugin info` 输出（**[#11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336)**）、manifest/组件代际不匹配（**[#11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562)**）、锁文件权限问题（**[#11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360)**），以及废弃的暂存目录（**[#11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792)**）。
- **跨提供商和模型的记忆整合很脆弱。** **[#6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** 描述了原始 JSON 解析失败、Markdown 代码围栏、额外文本和无效字段。**[#11890](https://github.com/zeroclaw-labs/zeroclaw/issues/11890)** 要求在将瞬态信息提升为长期记忆前提供更强证据。
- **渠道行为不一致。** Slack 用户在 v0.8.5 之后失去了“is thinking…”指示器（**[#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)**），而且技能学习循环不会为渠道/webhook/gateway 回合运行（**[#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)**）。
- **CLI 与运行中的守护进程可能不一致。** **[#11820](https://github.com/zeroclaw-labs/zeroclaw/issues/11820)** 报告 `config set` 更新了磁盘但未到达实时守护进程，这是一个重要的运维体验问题。
- **配置用户体验正在改善。** 通过 **[PR #11511](https://github.com/zeroclaw-labs/zeroclaw/pull/11511)** 关闭 **[#11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486)**，解决了 ZeroCode 保存/取消行为不一致的问题。

**满意度信号：** 项目响应及时——许多报告的问题在同一时期获得修复或接受状态更新。不满集中在 **安全隔离、插件信任和界面一致性**，而不是项目整体响应速度。

---

## 8. 待办积压观察

需要维护者关注的长期或重要事项：

| 条目 | 年龄/状态 | 为何重要 |
|---|---|---|
| **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — 按发送方 RBAC | 创建于 2026-04-22；未关闭、已接受，11 条评论 | 多租户部署的基础。范围已收窄，但实现仍待完成。 |
| **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — 经 schema 校验的记忆整合 | 创建于 2026-05-29；搁置 | 跨提供商的记忆可靠性仍是用户反复关注的问题。 |
| **[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** — 配对仪表盘未读取；配对码永不过期 | 创建于 2026-07-26；P1，`release:v0.9.0` | 安全敏感且带发布标签，但仍未关闭，且没有可见的修复 PR。 |
| **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — 知识图谱缺少按 agent 归属 | 创建于 2026-08-01；S0/P1 | 快照中最高严重性的未关闭 issue；直接影响数据隔离。 |
| **[Issue #10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162)** — 插件安装在配置初始化前持久化包 | 创建于 2026-08-20；进行中 | 全新安装路径已部分处理；已安装后的重新初始化行为仍未解决。 |
| **[Issue #10373](https://github.com/zeroclaw-labs/zeroclaw/issues/10373)** — 在 CLI 和网关之间共享 agent 重命名恢复 | 创建于 2026-08-26；P1 后续 | 持久状态迁移的跨界面一致性。 |
| **[Issue #10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993)** / **[PR #11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)** — 运行时组合边界 | 自 2026-09-20 / 2026-09-26；大型 PR 未关闭 | 关键的 v0.9.0 架构工作；XL PR 需要仔细审查。 |
| **[Issue #10997](https://github.com/zeroclaw-labs/zeroclaw/issues/10997)** 和 **[Issue #10998](https://github.com/zeroclaw-labs/zeroclaw/issues/10998)** — 插件引导和最小核心工具集 | 自 2026-09-20；受阻 | 两者都属于同一 v0.9.0 发布列车阶段，且仍受阻。 |
| **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack 思考状态回归 | 自 2026-10-02；进行中 | 可见的渠道 UX 回归；似乎正在积极处理。 |

**底线：** 项目吞吐量健康，v0.9.0 加固正在明显推进，但开放积压仍包含 **三个高影响安全/隔离事项**——**[#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)**、**[#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** 和 **[#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)**——在项目将其宣传为可安全用于多租户或共享 agent 部署之前，应优先处理它们。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*