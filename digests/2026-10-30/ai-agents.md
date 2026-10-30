# OpenClaw 生态日报 2026-10-30

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-30 02:07 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-30

## 1. 今日概览

OpenClaw 仍是一个高速推进的项目：过去 24 小时内有 500 个 issue 和 500 个 PR 被更新，其中 174 个 issue 已关闭、192 个 PR 已合并或关闭，显示出较强的问题分诊和合并吞吐能力。共发布两个版本：以安全为重点的稳定补丁 **v2026.10.9**，以及仅面向 Gateway 的 **v2026.9.36** 扩展稳定/LTS 版本。活跃积压仍然很大——更新项中有 326 个未关闭 issue 和 308 个未关闭 PR——并且主要集中在围绕 Gateway 状态数据库、会话生命周期完整性、MCP 运行时管理以及更新/恢复路径的 P0/P1 可靠性工作。总体而言，项目在响应速度和发布节奏方面健康状况良好，但风险集中在状态/会话持久性以及若干长期存在的 P0/P1 issue 上。注意：数据集展示的是按评论数排序的前 50 个 issue 和前 30 个 PR；PR 评论数不可用。

## 2. 版本发布

### v2026.10.9 — 稳定版安全补丁
[发布 v2026.10.9](https://github.com/openclaw/openclaw/releases/tag/v2026.10.9)

发行说明中列出的关键变更：
- **fast-uri 4.2.3**：在 MCP schema 校验期间拒绝格式错误的 URI scheme 和 host。
- **Zod 集合限制**：在 A2UI 和 Discord 嵌入式应用 SDK 中会提前拒绝过大的集合。
- **IMAP 发件人解析**：现在使用 nodemailer 10.1.4，因此能够正确处理伪造的发件人。

**破坏性变更 / 迁移说明：** 提供的发行摘录中未说明任何此类内容。这是一个依赖加固补丁；执行标准升级并重启 Gateway 即可。使用 MCP schema 校验、A2UI、Discord 嵌入式应用 SDK 或 IMAP 接入的团队应优先采用此版本。

**发布验证注意事项：** 2026.10.9 验证期间观察到的偶发测试失败记录在 [#187490](https://github.com/openclaw/openclaw/issues/187490)。发布方将其归类为偶发失败（flakes）；按照政策，需要在 `main` 上并行修复。

### v2026.9.36 — 仅 Gateway 的扩展稳定版（等同于 LTS）
[发布 v2026.9.36](https://github.com/openclaw/openclaw/releases/tag/v2026.9.36)

- 仅面向 Gateway 的 `extended-stable` 版本，被描述为项目当前的 LTS 等效版本。
- 基于 2026 年 9 月底的 OpenClaw，并包含关键安全更新、可靠性/性能修复以及新的模型支持。
- 最适合需要受支持且持续维护安全的基线、而非最新稳定特性的保守部署。

## 3. 项目进展

192 个已合并/关闭的 PR 和 174 个已关闭 issue 表明清理工作有实际成效，不过数据集未区分已合并与仅关闭的 PR。

**活跃前 50 个 issue 中值得注意的已关闭项：**
- [#178000](https://github.com/openclaw/openclaw/issues/178000) — 会话状态竞态被误分类为 `reason=unknown`，触发跨提供商的模型回退。被评为 diamond lobster / P1 后关闭。
- [#186813](https://github.com/openclaw/openclaw/issues/186813) — 聊天内托管更新（`gateway.update.run`）因 SQLite 锁竞争而无限期卡在 `epoll_wait`。P0，已关闭。
- [#156635](https://github.com/openclaw/openclaw/issues/156635) — 账户发现后，Copilot 上下文预算仍停留在合成的 128k 回退值。已关闭。
- [#187468](https://github.com/openclaw/openclaw/issues/187468) — 2026.9.8 的更新失败报告。已关闭。

**推进关键领域的 PR：**
- **更新/恢复加固：** [#188034](https://github.com/openclaw/openclaw/pull/188034) 在回退发布过程中保留权限；[#188021](https://github.com/openclaw/openclaw/pull/188021) 从恢复歧义中排除已解决的捕获；[#188024](https://github.com/openclaw/openclaw/pull/188024) 报告失败的定时注册表观测；[#188027](https://github.com/openclaw/openclaw/pull/188027) 通过主键读取命名恢复；[#188010](https://github.com/openclaw/openclaw/pull/188010) 在返回失败后重试安装发现（已关闭）。
- **Gateway 响应性/性能：** [#188033](https://github.com/openclaw/openclaw/pull/188033) 在插件工具发现期间保持 Gateway 响应；[#188030](https://github.com/openclaw/openclaw/pull/188030) 阻止 doctor 重复读取 SQLite 检查副本；[#188025](https://github.com/openclaw/openclaw/pull/188025) 阻止 GitHub 就绪检查复制整个 agent 数据库（P1，安全敏感）。
- **Web/UI 正确性：** [#188013](https://github.com/openclaw/openclaw/pull/188013) 在相对 HTML 预览中保留作者编写的 CSP（已标记安全边界风险）；[#188017](https://github.com/openclaw/openclaw/pull/188017) 修复会话高级菜单中的重复分隔线（已关闭）；[#188039](https://github.com/openclaw/openclaw/pull/188039) 为工作区栏文件搜索增加规范 Unicode 匹配。
- **渠道/集成：** [#188003](https://github.com/openclaw/openclaw/pull/188003) 在 Teams 实时目录搜索中匹配规范等价的名称；[#188018](https://github.com/openclaw/openclaw/pull/188018) 修复进度模式下 Matrix 图片回复。
- **记忆：** [#186649](https://github.com/openclaw/openclaw/pull/186649) 通过标记让主题文件启用按条目分块。
- **开发者体验：** [#187962](https://github.com/openclaw/openclaw/pull/187962) 增加 `OPENCLAW_DEV_RELOAD_BUNDLED_PLUGINS`，用于开发期间重新加载捆绑插件代码。

## 4. 社区热点

评论最多的 issue 显示，用户持续关注状态持久性、记忆提升和 MCP 工具可靠性。

1. **[#164294](https://github.com/openclaw/openclaw/issues/164294) — Gateway 状态数据库 WAL 自动检查点永不完成（38 条评论，P2）。**  
   Gateway 自身对 `state/openclaw.sqlite` 的 WAL 检查点被一个进程内读取器永久阻塞；`consecutiveBlocked` 持续增长，检查点始终无法完成。底层需求：在真实 Gateway 负载下实现可预测的 SQLite 维护和有界的 WAL 增长。

2. **[#150635](https://github.com/openclaw/openclaw/issues/150635) — 短期回忆保留策略每晚驱逐已回忆条目，导致 dreaming 深度阶段永不提升（23 条评论，P2）。**  
   在 512 条目上限下，夜间摄入阶段会暂存零回忆条目，深度阶段永不提升。底层需求：在短期回忆容量、dreaming 和提升之间建立一致的记忆生命周期语义。

3. **[#120385](https://github.com/openclaw/openclaw/issues/120385) — Code Mode 工具目录在定时/cron 和 memory-flush 轮次中不完整；MCP 工具缺失（22 条评论，P2）。**  
   定时任务以“Unknown tool id”失败，仅表现为通用 exec 失败。底层需求：在交互式、cron 和 memory-flush 执行路径之间实现工具目录一致性。

4. **[#164923](https://github.com/openclaw/openclaw/issues/164923) — 一个工作区提升 0/512，而同级工作区提升 50–90%（19 条评论，P2）。**  
   `openclaw memory promote` 即使将阈值覆盖为零也返回无候选。底层需求：多工作区记忆提升中可确定、可检查的过滤机制。

5. **[#172837](https://github.com/openclaw/openclaw/issues/172837) — Codex `nativeSubagentAssignments` 在完成交付放弃后从未移除（18 条评论，P1）。**  
   每个新的父线程都会重新固定并重新发送陈旧分配。底层需求：原生子代理的生命周期清理和重试耗尽语义。

**PR 侧活动：** 数据集中 PR 评论数不可用，但若干 PR 标记为“ready for maintainer look”，包括 [#188003](https://github.com/openclaw/openclaw/pull/188003)、[#188021](https://github.com/openclaw/openclaw/pull/188021)、[#187986](https://github.com/openclaw/openclaw/pull/187986) 和 [#184511](https://github.com/openclaw/openclaw/pull/184511)。长期运行的 XL PR [#181813](https://github.com/openclaw/openclaw/pull/181813)（Gateway 原生状态所有者身份）自 2026-10-23 起保持打开，正在等待维护者审查。

## 5. Bug 与稳定性

过去 24 小时更新的活跃 issue，按严重程度排序。在可识别处注明修复 PR。

### P0 — 发布阻断 / 严重可用性
- **[#166634](https://github.com/openclaw/openclaw/issues/166634)** — 迟到的回收提交检查点失败导致 agent 在崩溃恢复后永久不可用；agent 拒绝 `chat.history`、`chat.metadata`、`models.list` 和进度卡请求。18 条评论。未显示关联修复 PR。
- **[#162211](https://github.com/openclaw/openclaw/issues/162211)** — Gateway 启动阻塞事件循环 40–200 秒；健康监控将此停顿误判为断开连接，并升级为重启循环。14 条评论。未显示关联修复 PR。
- **[#158592](https://github.com/openclaw/openclaw/issues/158592)** — macOS 睡眠/唤醒后，某个 agent 的已准备模型运行时发布超时且永不恢复；在 Gateway 重启之前，所有入站消息都以“runtime owner was not published”失败。11 条评论。未显示关联修复 PR。
- **[#141413](https://github.com/openclaw/openclaw/issues/141413)** — macOS 应用在使用受保护的 SecretRef 为本地 Gateway 时省略 token；嵌入式仪表板可以认证，但外部连接不能。P0，需要安全审查，7 条评论，2 个 👍。
- **[#186813](https://github.com/openclaw/openclaw/issues/186813)** — 托管更新因 SQLite 事务锁竞争卡在 `epoll_wait`。**已关闭**；可能已处理，但提供的数据中未列出具体合并 PR。

### P1 — 严重功能/崩溃循环回归
- **[#172837](https://github.com/openclaw/openclaw/issues/172837)** — Codex 原生子代理分配在交付放弃后泄漏；在每个新的父线程上重新固定。18 条评论。未显示修复 PR。
- **[#177656](https://github.com/openclaw/openclaw/issues/177656)** — 原生 host exec 返回“Direct shared-state reader requires current integrity admission”且没有子结果；在稳定版 2026.10.5 上至少 28 次失败。17 条评论。未显示修复 PR。
- **[#166017](https://github.com/openclaw/openclaw/issues/166017)** — 未设置时 `mcp.sessionIdleTtlMs` 默认值（10 分钟）未生效；MCP 运行时永不驱逐，spawn broker 达到 256 个子进程硬上限，所有 exec/read/MCP 启动均失败。13 条评论。关联修复 PR 已打开。
- **[#159094](https://github.com/openclaw/openclaw/issues/159094)** — Gateway 持有状态生命周期租约，但内部 worker 报告另一个进程持有该租约，从而关闭 agent DB 执行准入。14 条评论。
- **[#126950](https://github.com/openclaw/openclaw/issues/126950)** — 非插件调用方发送的 Telegram 不透明回调 payload（`tgcb1:`）被静默丢弃。11 条评论，1 个 👍。关联修复 PR 已打开。
- **[#163566](https://github.com/openclaw/openclaw/issues/163566)** — 更新修复后，持久化上下文引擎轮次卡在 `session-rebound`；`session-resource-loader` 每轮消耗约 100 秒 CPU。10 条评论。
- **[#167876](https://github.com/openclaw/openclaw/issues/167876)** — 会话设置完成后，Talk 咨询丢失调用方权限；需要安全审查。9 条评论，1 个 👍。
- **[#158070](https://github.com/openclaw/openclaw/issues/158070)** — 当运行在 lane 队列中等待时，预检压缩失败并提示“agent tool caller authority is no longer active”，使进行中的交付失效。9 条评论。
- **[#148789](https://github.com/openclaw/openclaw/issues/148789)** — 模型回退链在 harness 级会话失败时耗尽所有候选，并将其错误归因于最后一个回退模型。9 条评论。
- **[#178359](https://github.com/openclaw/openclaw/issues/178359)** — `doctor --fix --non-interactive` 在插件捕获阶段空闲 15+ 分钟，从未进入 agent DB 修复。8 条评论。相关性能 PR [#188030](https://github.com/openclaw/openclaw/pull/188030)。
- **[#155396](https://github.com/openclaw/openclaw/issues/155396)** — 普通 Codex 工具轮次在最终答案恢复期间因 `provenance_rejected` 而失败关闭（fail closed）。8 条评论。
- **[#160762](https://github.com/openclaw/openclaw/issues/160762)** — claude-cli + Telegram：轮次中途发送的消息被丢弃；排空后的 prompt 携带上一条消息的正文。7 条评论。

### P2 — 值得注意的摩擦/正确性
- **[#164394](https://github.com/openclaw/openclaw/issues/164394)** — 当滚动到历史记录时，Control UI WebChat 转录内容持续抖动。13 条评论。
- **[#177094](https://github.com/openclaw/openclaw/issues/177094)** — 重启/重新加载后，系统通知交付镜像会将轮次的最后答案挤到折叠的工具步骤中。9 条评论。
- **[#172969](https://github.com/openclaw/openclaw/issues/172969)** — 不带 `modelProvider` 的 `registerCliBackend` 被静默丢弃；表现为“Invalid URL: must be http or https.” 8 条评论。
- **[#176342](https://github.com/openclaw/openclaw/issues/176342)** — 非正常关闭后，陈旧租约完整性门禁将 agent 启动串行化，导致数分钟不可用。7 条评论。
- **[#186973](https://github.com/openclaw/openclaw/issues/186973)** — `/clear` 状态检查在运行数周后仍渲染过时的更新历史横幅，且跨 Gateway 重启和系统重启仍然存在。7 条评论。
- **[#165433](https://github.com/openclaw/openclaw/issues/165433)** — 当客户端不是 Operator UI 客户端时，`chat.send` 拒绝带有 `__controlUiReconnectResume` 的重连 envelope。7 条评论。
- **[#164690](https://github.com/openclaw/openclaw/issues/164690)** — claude-cli 运行会为每个 stdio MCP server 启动两次；每个 Gateway stdio 子进程都带有约 144 MB 的 supervisor wrapper 开销，且不可配置。7 条评论。
- **[#167667](https://github.com/openclaw/openclaw/issues/167667)** — ChatGPT Responses 工具续接失败并提示“Rustponses cannot replay”，表现为通用 agent 运行错误。P1，6 条评论，1 个 👍。

**稳定性评估：** 最高风险集群是 Gateway/会话状态完整性和崩溃恢复。若干 P0/P1 issue 已开放数周且没有关联修复 PR，而更新路径和 doctor 性能修复正在积极审查中。2026.10.9 安全补丁值得欢迎，但并未解决核心的状态生命周期故障。

## 6. 功能请求与路线图信号

活跃的用户请求功能，按可能的相关性排序：

- **[#6757](https://github.com/openclaw/openclaw/issues/6757) — Agent 触发的上下文压缩（self-compact 工具）。** P2，9 条评论，2 个 👍，自 2026 年 2 月起开放。与项目记忆/上下文工作高度契合；是路线图关注的有力候选。
- **[#172547](https://github.com/openclaw/openclaw/issues/172547) — 从记忆搜索索引和回忆信号中排除围栏式 dreaming 范围。** 7 条评论。与热门记忆提升 issue 直接相关；可能会并入 memory-core 修复。
- **[#112638](https://github.com/openclaw/openclaw/issues/112638) — `session.maintenance enforce` 模式没有限制存储；线程/渠道条目被豁免回收。** 8 条评论，自 7 月起开放。强烈的可靠性/可运维性信号。
- **[#163739](https://github.com/openclaw/openclaw/issues/163739) — 使 Control UI 的 Ctrl+Shift+A（归档）可配置；它与浏览器快捷键冲突。** 7 条评论，1 个 👍。小型、面向用户的 UX 修复。
- **[#140244](https://github.com/openclaw/openclaw/issues/140244) — 在 `openclaw infer model run` 中支持 `--prompt-file`。** 7 条评论，1 个 👍。面向自动化的 CLI 改进。
- **[#154524](https://github.com/openclaw/openclaw/issues/154524) — 将聊天消息暴露为标题以供屏幕阅读器导航。** 6 条评论。无障碍缺口。
- **[#147732](https://github.com/openclaw/openclaw/issues/147732) — 为波斯语/阿拉伯语/希伯来语与英语混合消息提供段落级 RTL 方向。** 6 条评论。国际化/UI 正确性。
- **[#150743](https://github.com/openclaw/openclaw/issues/150743) — QQ 渠道可持续性：腾讯交接停滞；明确长期支持。** 7 条评论。生态系统/支持模式问题，不是代码功能。

**对下一版本的预测：** 最强的近期信号是记忆/回忆正确性（多个高评论 issue 加上 PR [#186649](https://github.com/openclaw/openclaw/pull/186649)）、更新/恢复加固（已经是 PR 集群）、MCP 运行时生命周期默认值，以及 Control UI 无障碍/本地化修复。Agent 触发的上下文压缩是价值最高的较大功能，但自 2 月以来一直待定，可能需要产品决策。

## 7. 用户反馈总结

**反复出现的痛点：**
- **状态与会话持久性：** 用户报告 agent 在崩溃恢复后永久不可用、WAL 检查点永不完成、陈旧租约使启动串行化，以及会话状态竞态触发跨提供商回退。这些是最具破坏性的反馈体验，因为它们需要重启 Gateway 或导致 agent 无法使用。
- **记忆/dreaming 不一致：** 多份报告描述提升流水线静默产生零候选、每晚驱逐已回忆条目，或索引永远无法提升的 dreaming 输出。用户显然将记忆作为核心工作流，而非外围功能。
- **更新与恢复可靠性：** 聊天内更新因 SQLite 锁挂起、`doctor --fix` 空闲、失败的注册表查找被记录为成功，以及包替换失败。这侵蚀了对升级路径的信任，尤其是稳定版/LTS 用户。
- **MCP/资源管理：** MCP 运行时未在默认 TTL 下驱逐、spawn broker 达到 256 个子进程、claude-cli 下重复的 stdio server，以及每个子进程约 144 MB 的 wrapper 开销。多 agent 或 Docker 沙箱部署的运维人员对此感受尤为强烈。
- **UI/UX 摩擦：** WebChat 转录抖动、过时横幅、系统通知挤占最终答案、快捷键冲突、RTL 渲染，以及屏幕阅读器导航缺口。
- **平台特定问题：** macOS Homebrew Node 升级破坏裸 `openclaw` 命令；Android 反复查询未知 agent `openclaw`；QQ 渠道支持不确定性。

**满意度信号：** 问题分诊量大、若干 P0/P1 issue 快速关闭、清晰的 LTS 等效版本以及安全补丁节奏，均表明维护响应积极。“ready for maintainer look”标签和结构化评级系统表明审查流程健康。

**不满意度信号：** 若干 P0/P1 issue 已开放数周且没有关联修复 PR，许多高评论 issue 带有 `needs-product-decision`、`needs-maintainer-review` 或 `stale` 标签。用户在详细复现上投入了大量精力；对这些报告的决策延迟可能导致倦怠和变通方案。

## 8. 积压关注

重要 issue/PR，它们陈旧、高影响，或等待维护者/产品行动：

- **[#6757](https://github.com/openclaw/openclaw/issues/6757)** — Agent 触发的上下文压缩。自 **2026-02-02** 起开放，9 条评论，2 个 👍，需要产品决策。可见集合中运行时间最长的高价值功能请求。
- **[#112638](https://github.com/openclaw/openclaw/issues/112638)** — `session.maintenance enforce` 没有限制存储。自 **2026-07-22** 起开放，8 条评论。对长期运行部署具有重要运维意义。
- **[#120385](https://github.com/openclaw/openclaw/issues/120385)** — Code Mode 工具目录在 cron/memory-flush 轮次中不完整。自 **2026-08-08** 起开放，22 条评论。社区参与度高；影响定时自动化可靠性。
- **[#126950](https://github.com/openclaw/openclaw/issues/126950)** — Telegram 不透明回调 payload 被丢弃。自 **2026-08-21** 起开放，11 条评论，1 个 👍，关联修复 PR 已打开。渠道集成阻塞项。
- **[#141413](https://github.com/openclaw/openclaw/issues/141413)** — macOS 应用在使用 SecretRef 为本地 Gateway 时省略 token。自 **2026-09-07** 起开放，P0，7 条评论，2 个 👍，需要安全审查。严重性高但评论量相对较低——可能分诊不足。
- **[#150635](https://github.com/openclaw/openclaw/issues/150635)** — Dreaming 深度阶段因回忆驱逐而永不提升。自 **2026-09-17** 起开放，23 条评论，diamond-lobster 评级，需要产品决策。用户影响大且参与度高。
- **[#158592](https://github.com/openclaw/openclaw/issues/158592)** — 睡眠/唤醒运行时发布超时。自 **2026-09-26** 起开放，P0，11 条评论，需要现场复现。严重但依赖复现。
- **[#162211](https://github.com/openclaw/openclaw/issues/162211)** — 启动事件循环阻塞 → 重启循环。自 **2026-09-30** 起开放，P0，14 条评论，需要维护者审查。发布阻断级严重性。
- **[#166634](https://github.com/openclaw/openclaw/issues/166634)** — 迟到回收检查点失败导致 agent 不可用。自 **2026-10-07** 起开放，P0，18 条评论。最高严重性的开放 issue 之一。
- **[PR #154033](https://github.com/openclaw/openclaw/pull/154033)** — 来自原始响应项的 Codex 原生工具输出图像。自 **2026-09-20** 起开放，需要证明，存在安全边界和消息交付风险。长期存活的 PR，具有协议级影响。
- **[PR #181813](https://github.com/openclaw/openclaw/pull/181813)** — 暴露持有的原生状态所有者身份。自 **2026-10-23** 起开放，XL，P2，已准备好供维护者查看。与状态所有者/租约混乱集群相关。
- **[PR #184511](https://github.com/openclaw/openclaw/pull/184511)** — 提交带有参与关系的邀请成员资格。自 **2026-10-26** 起开放，P2，已准备好供维护者查看，证明充分。授权/会话正确性。
- **[#150743](https://github.com/openclaw/openclaw/issues/150743)** — QQ 渠道可持续性与腾讯交接。7 条评论，自 **2026-09-17** 起开放。需要生态系统/支持决策，而非代码修复。

**底线：** OpenClaw 发布迅速，并维持了可信的 LTS/安全节奏，但其最有价值的用户正遭遇状态/会话持久性和记忆提升故障，而这些故障已数周未解决。下一发布周期应以其是否能对 P0 状态生命周期集群（#166634、#162211、#158592）和记忆提升集群（#150635、#164923、#172547）给出具体修复，而非进一步分诊，来评判。

---

## 横向生态对比

# 跨项目对比报告——个人 AI 助理 / Agent 开源生态
**快照日期：2026-10-30 | 项目：OpenClaw、Hermes Agent、IronClaw、QwenPaw、ZeroClaw**

---

## 1. 生态概览

个人 AI 助理 / agent 开源格局正围绕一套共同架构栈收敛——由网关进程持有会话/状态、MCP 工具层、记忆子系统，以及多渠道路由面——厂商则在部署模型（单用户 vs 多租户）、语言/运行时和渠道覆盖上形成差异。活跃度极不均衡：OpenClaw 的活跃度比同侪高一个数量级（每天触及 500 个 issue + 500 个 PR），Hermes/ZeroClaw/QwenPaw 集中在每天 40–55 个条目的聚焦区间，IronClaw 在本窗口实际上处于休眠。在所有活跃项目中，主导工程主题已从 *能力* 转向 *持久性与信任*：崩溃安全状态、诚实故障报告、记忆来源和身份作用域，在社区参与度上现已超过功能发布。发布节奏正在分化——OpenClaw 是唯一同时维护快速稳定通道和等效 LTS 通道的项目，这对保守型采用者而言正成为有意义的竞争护城河。

---

## 2. 活跃度对比（过去 24 小时）

| 项目 | 更新的 Issue（开放/关闭） | 更新的 PR（开放/已合并-关闭） | 发布状态 | 健康度评分* |
|---|---|---|---|---|
| **OpenClaw** | 约 500（174 关闭） | 约 500（192 合并/关闭） | **2 个发布**：v2026.10.9 稳定安全补丁；v2026.9.36 仅网关 LTS | **8.5 / 10** |
| **Hermes Agent** | 50（48 开放 / 2 关闭） | 50（10 合并/关闭，40 开放） | 无 | **7.5 / 10** |
| **IronClaw** | 0 | 0（2 个开放的 Dependabot） | 无 | **4.5 / 10** |
| **QwenPaw** | 15（10 关闭 / 5 开放） | 40（13 合并/关闭，27 开放） | 无（2.3.0 beta 发布前稳定化） | **7.5 / 10** |
| **ZeroClaw** | 50（17 关闭 / 33 开放） | 50（6 合并/关闭，44 开放） | 无（介于 v0.8.7 与 v0.9.0 之间） | **7.0 / 10** |

\*健康度评分是由分流/合并吞吐量、发布节奏、未关闭积压严重度和未解决 P0/P1 风险合成的派生综合指标。它是方向性的，不是维护者指标。OpenClaw 在吞吐量和发布纪律上得分最高，但集中承载 P0 状态持久性风险；QwenPaw/Hermes 从相反方向得到相似评分（速度 vs. 积压清理）。IronClaw 的评分反映低活跃度，而非不稳定。

**解读表格：** OpenClaw 的原始吞吐量约为第二大项目的 10 倍。QwenPaw 相对 issue 最偏 PR 密集（40:15），表明实现速度超过需求接收速度——这是评审带宽受限的典型特征。ZeroClaw 显示出最大的 **录入到合并缺口**（44 个开放 PR vs. 6 个已关闭 PR），这是其主要结构性风险。Hermes 是唯一表现出 **积压清理行为** 的项目（本窗口合并了数月前的 PR）。

---

## 3. OpenClaw 的定位

**相对同行的优势**
- **社区规模：** 每天约 500 个 issue + 500 个 PR，对比 50/50（Hermes、ZeroClaw）和 15/40（QwenPaw）。贡献者和分流基础大一个数量级，因此它能在一天内维持 174 个 issue 关闭和 192 个 PR 处置。
- **双发布通道：** 唯一同时发布稳定安全补丁（v2026.10.9）和仅网关等效 LTS（v2026.9.36，包含回移的安全 + 可靠性修复）的项目。对保守型/企业采用者而言，这是具体的差异化优势。
- **流程成熟度：** 结构化严重度评级（P0–P2、“diamond lobster”）、明确的“ready for maintainer look”状态，以及发布验证注意事项策略（flaky 测试被跟踪到并行 `main` 修复）表明其具备同行缺乏的治理层。
- **集成面广度：** 渠道（Telegram、Matrix、Teams、Discord、QQ、IMAP）、Web/UI、Code Mode、A2UI 和 MCP 运行时——在本组中最广。

**技术路线差异**
- **以网关为中心、SQLite 支撑的状态：** 会话生命周期、状态生命周期租约、WAL 检查点和崩溃恢复都是网关的一等关注点。这既是其最强保证雄心的来源，也是其最高严重度未关闭 bug（#166634、#162211、#158592）的来源。
- **记忆/dreaming 作为子系统：** 短期召回上限、夜间“深度阶段”晋升和 dreaming 范围索引在本组中为 OpenClaw 独有，并推动其评论最多的问题（#150635、#164923）。
- **将 MCP 视为带生命周期管理的运行时：** 会话空闲 TTL、spawn-broker 子进程上限和运行时驱逐被视为运维关注点（#166017），而不仅仅是协议管道。

**社区规模对比：** OpenClaw 是参考实现和生态的引力井；QwenPaw 和 ZeroClaw 的每日条目量各约为其 10%，IronClaw 当前不活跃。Hermes 在范围上最接近，但吞吐量约为 OpenClaw 的 10%，且没有发布通道。

---

## 4. 共同技术重点领域

相同的需求族在独立代码库中反复出现——这是一个强信号，表明它们是生态级而非项目局部问题。

| 需求 | 提出该需求的项目 | 具体需求 |
|---|---|---|
| **会话/状态持久性与崩溃恢复** | OpenClaw (#166634, #162211, #158592), Hermes (#146643, #139210), ZeroClaw (#10526 RFC, #11432) | 崩溃安全状态机、有界 WAL 增长、重启安全会话接管、仅追加可回放历史。ZeroClaw 是唯一一个已 *接受 RFC* 来实现仅追加事件历史的项目（#10526 → 实现 #11921）。 |
| **记忆/上下文正确性与来源** | OpenClaw (#150635, #164923, #172547), Hermes (#143379 doc-chain RFC), QwenPaw (#8500, #8457, #7835), ZeroClaw (#9647 knowledge-graph attribution) | 一致的召回→晋升生命周期、保留来源的压缩、不向渠道静默泄漏 payload、按 agent 隔离知识。 |
| **静默失败 / 诚实状态报告** | QwenPaw (#8500 "success=True on dropped data", #8150, #8488), ZeroClaw (#11332, #11336 false-positive plugin validation), OpenClaw (registry lookups recorded as successes), Hermes (#124874 mislabeled provider failure) | 区分“什么都没做”与“没有数据”；大声失败；绝不将有损操作检查点为成功。QwenPaw 的三份独立报告使这成为最清晰的跨项目主题。 |
| **多租户身份、RBAC 与归属** | ZeroClaw (#5982 per-sender RBAC, #9647 S0 cross-agent knowledge), Hermes (#35147 stable `user_id`, profile-scope mismatches), OpenClaw (operator authority, Talk authority loss), QwenPaw (Hub admin password reset #7740) | 按主体授权、稳定身份映射、作用域隔离、管理操作对等。 |
| **MCP / 插件生命周期完整性** | OpenClaw (#166017 idle-TTL eviction, spawn-broker cap), QwenPaw (#8473 plugin loader regression, #8499 provider decomposition), ZeroClaw (#11562 manifest/component generation mismatch, #11336), Hermes (#142235 read-only MCP tool call) | 确定性插件注册、代际一致性、运行时驱逐默认值、仪表盘的只读/受限访问。 |
| **Cron/定时任务持久性** | Hermes (#135487 SQLite lock, #146428 silent wedge, #145596 paused-job reactivation), OpenClaw (#120385 tool catalog missing for cron/memory-flush), ZeroClaw | 重启安全的触发处理、重试语义、跨执行路径的工具目录对等、可见的运行历史。 |
| **上下文压缩正确性** | OpenClaw (#6757 self-compact, #8464-adjacent), QwenPaw (#8464 `/compact` ignores session model, #6189 `save_decision()`), Hermes (#139210 compaction duplicates messages) | 会话范围的模型路由、压缩期间决策持久化、无重复行副作用。 |
| **配置 schema 信任（“接受但未读取”）** | ZeroClaw (#9394 pairing dashboard never read, #11589 cost override no-op), OpenClaw (#172969 silent CLI-backend drop), Hermes (#90040 Managed Scope ignored) | 配置键必须接通或被拒绝；静默无操作会侵蚀 schema 信任。 |
| **Agent 循环 / 成本控制** | Hermes (#137170 no circuit breaker, 430 duplicate calls), QwenPaw (#6116 doom loop, closed **wontfix**), ZeroClaw (#11484 repetitive-tool safeguards disabled) | 重复成功检测（不只是重复失败）、token 预算、调用指纹去重。QwenPaw 的 `wontfix` 是一处显著差异。 |
| **安装/更新占用** | Hermes (#139500 ~22,600 `.pyc` / >303 MiB, #143577 1 GB uv cache), QwenPaw (#8488 desktop packaging regression), OpenClaw (update/recovery hardening cluster) | 可修剪缓存、带 TTL 自愈的崩溃安全更新尾部、打包资产完整性。 |

---

## 5. 差异化分析

**功能重点与目标用户**
- **OpenClaw**——最广泛的个人助理平台：记忆/dreaming、多渠道、Web/UI、Code Mode、A2UI。面向希望获得全栈助理并愿意运维网关的高级用户和自托管者。唯一具有明确保守部署（LTS）层级的项目。
- **Hermes Agent**——工作流自动化：看板驱动执行、cron 投递保证、多 profile/团队配置，以及针对分机使用的 **强烈需求的桌面纯前端安装（#38519，31👍）**。面向消费者/生产力和小团队自动化。
- **IronClaw**——基础设施/延迟测试装置（基于 wasmtime，`/harness/latency/runner`）。当前仅维护；本窗口无面向用户的信号。
- **QwenPaw**——派生自 AgentScope，面向中国市场优先，渠道密集（飞书、QQ、微信、企业微信、钉钉、OneBot、元宝）。桌面端（Windows、zh-CN）和 Hub 多租户模式是一等公民。正处于 **2.3.0-beta 稳定化**，插件/provider 拆分正在进行中。
- **ZeroClaw**——基于 Rust，明确朝 **多租户、可审计 agent 基础设施** 推进：按发送者 RBAC、按 agent 知识作用域、仅追加可回放会话、ZeroCode TUI。本组中最偏安全/治理的前瞻路线图。

**技术架构差异**
- **语言/运行时：** ZeroClaw（Rust、wasmtime）、IronClaw（wasmtime harness）、QwenPaw（AgentScope/Python、PyInstaller 桌面端）、Hermes（Python 网关）、OpenClaw（网关 + SQLite 状态 DB + MCP 运行时）。
- **状态模型：** OpenClaw 和 Hermes 是可变的、带租约的 SQLite 模型（其锁竞争和过期租约 bug 的来源）。ZeroClaw 是唯一一个 **用仅追加事件日志替换** 可变持久化的项目（#11921），这在结构上消除了 OpenClaw 仍在修补的若干故障类别。
- **租户模型：** OpenClaw/Hermes/QwenPaw 从单用户起步，正在改造以支持多 profile/多租户；ZeroClaw 从一开始就面向多租户设计（RBAC、知识作用域、principal withholding）。
- **分发：** OpenClaw 提供双通道；QwenPaw 提供 PyInstaller 桌面 beta；Hermes 没有发布，且存在强烈的安装卫生抱怨；IronClaw 本窗口未发布任何内容。

---

## 6. 社区动能与成熟度

**活跃度层级**
- **第 1 层——超高频：** OpenClaw（每天触达约 1,000 个条目、2 个发布、持续分流）。这是一个在运行发布列车的项目，而不是短跑。
- **第 2 层——高活跃、聚焦：** Hermes、ZeroClaw（各 50/50）、QwenPaw（55 个条目，偏 PR）。三者都在围绕明确主题（可靠性 / 安全基础设施 / beta 稳定化）高强度迭代，但本窗口无发布。
- **第 3 层——休眠：** IronClaw（0 个 issue，2 个 Dependabot PR）。数据不足以区分稳定与脱离。

**谁在快速迭代 vs. 稳定化**
- **快速迭代：** QwenPaw（2.3.0-beta 回归正在积极修补：#8473 通过 #8497 当日关闭）、OpenClaw（周节奏修复 + 安全补丁）、ZeroClaw（v0.9.0 里程碑持续累积；今日关闭 17 个 issue）。
- **稳定化 / 清理积压：** Hermes 本窗口合并了 **数月前的 PR**（#73923、#63812、#5490 一类清理），表明这是一次维护性清理，而非新功能工作。IronClaw 持平。
- **评审带宽受限：** QwenPaw 最老的 PR 已 **40–53 天**，且为 `XL/XXL/XXXL`（#7613 已 53 天，#7874 已 41 天）；ZeroClaw 有 **44 个开放 PR vs. 6 个已关闭 PR**，存在堆叠依赖链（#11320→#11322→…）和一个自 **2026-08-04** 起等待评审的 P1 安全 PR（#9745）；OpenClaw 有长期存活的 XL PR（#181813 自 10-23 起开放）等待维护者查看。常见故障模式是 **大型、堆叠、高风险 PR 挤占评审队列**，而非贡献者不足。

**成熟度信号排序：** OpenClaw（最高：双通道、严重度分类法、验证策略）> ZeroClaw（已接受架构 RFC、发布里程碑标记、安全评审标签）> Hermes/QwenPaw（贡献量强，决策吞吐较弱——`needs-decision`/`awaiting-reporter`/`wontfix` 集群）> IronClaw（未知）。

---

## 7. 趋势信号（面向 AI agent 开发者）

1. **持久、可回放状态正成为基本门槛。** 每个活跃项目都在与会话/租约/崩溃恢复 bug 斗争；ZeroClaw 的仅追加 RFC 是最清晰的前瞻答案。**启示：** 从第一天起就按事件溯源或可回放持久化来设计——在可变 SQLite 上补持久性，是整个组中最大的单项重复成本。

2. **静默失败是新的信任杀手。** 最具破坏性的报告不是崩溃，而是 *报告成功但实际无操作*：QwenPaw 的 Auto-Dream 检查点将丢弃记忆记录为 `success=True`（#8500）、飞书媒体被静默丢弃（#8150）、Visual Compact 静默无操作（#8488），以及 ZeroClaw 的插件“加载”但从未注册（#11336）。**启示：** 将“什么都没做 vs. 没有数据”视为独立、可测试的故障类别；对操作进行埋点以证明效果。

3. **多租户身份是下一个架构前沿。** 单用户助理正在改造以服务团队：ZeroClaw RBAC/知识作用域、Hermes 稳定 `user_id` 和 profile 作用域隔离、QwenPaw Hub 管理操作、OpenClaw 操作员权限。**启示：** principal/scope 应成为会话和工具调用模型中的一等维度，而不是配置附加项。

4. **记忆正确性如今是可靠性要求，而非功能特性。** 召回上限、夜间晋升、压缩模型路由和来源边界在 OpenClaw、QwenPaw、Hermes 和 ZeroClaw 中同时成为热点。长上下文（QwenPaw #8464 中的 340k token 会话）已是主流，而非边缘场景。**启示：** 记忆生命周期（摄入 → 召回 → 晋升 → 压缩 → 归档）需要明确、可检查的语义和测试。

5. **成本与循环控制是用户可见的痛点。** Hermes 的 430 次重复调用报告（#137170）和 QwenPaw 的量化 token 浪费（#6116）表明用户正在计量 agent 运行。QwenPaw 在参与度最高的线程上标记 `wontfix` 是一处值得关注的差异——这可能将注重成本的用户推向竞品。**启示：** 重复成功检测、调用指纹去重和 token 预算应属于运行时，而不只是告警。

6. **MCP/插件生命周期是运维问题。** 空闲 TTL 驱逐失败（OpenClaw #166017）、spawn-broker 子进程上限、重复 stdio 服务器（约 144 MB/子进程开销）和代际不匹配发现（ZeroClaw #11562）是运维事件，而非协议 bug。**启示：** 将驱逐默认值、broker 上限和注册验证作为一等功能发布。

7. **配置 schema 信任是留存风险。** “接受但完全未读取”的配置（ZeroClaw #9394、#11589；OpenClaw #172969；Hermes #90040）是一类反复出现的问题，会悄悄破坏用户假设。**启示：** 校验并接通配置键，或在加载时大声失败。

8. **发布纪律正在形成差异化。** 只有 OpenClaw 维护稳定 + LTS 组合；Hermes 和 ZeroClaw 尽管活动频繁，本窗口未发布任何内容，QwenPaw 仍是发布前 beta。对决策者而言，**OpenClaw 目前是生产邻近部署唯一安全的默认选择**，而 QwenPaw/ZeroClaw 最好视为高动能、GA 前押注，IronClaw 则视为不活跃。

**技术决策者要点：** 若需要生产级广度和发布节奏，选择 OpenClaw（同时接受活跃的 P0 状态持久性风险）；若需要团队/渠道特定的工作流自动化，在 Hermes/QwenPaw 的稳定化阶段选择它们；若多租户可审计性和仅追加状态是硬性要求，则关注 ZeroClaw，但这取决于其 P1 安全评审队列能否清空（#9647/#9745 已停滞约 3 个月）。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-10-30

**数据说明：** 本数据集报告的是过去 24 小时内*更新*的条目，不一定是当天创建的。PR 评论数不可用。“已合并/已关闭”的 PR 合并报告，因此无法从所提供的数据中完全区分已合并与未合并关闭。

---

## 1. 今日概览

Hermes Agent 依然高度活跃：过去 24 小时内有 50 个 issue 和 50 个 PR 被更新，没有新版本发布。Issue 队列严重偏向未完成的工作——50 个更新 issue 中有 48 个仍处于打开/活跃状态——而 PR 吞吐量更强，50 个更新 PR 中有 10 个已合并或关闭。活动集中在 P1 可靠性问题上，涉及安装/更新状态、凭证池轮换、cron 持久性、Kanban worker 稳定性以及 agent 循环控制。社区参与度很高，包括一个 30 条评论的架构 RFC 和一个获得 31 个 👍 的桌面安装功能请求，但若干高严重性 bug 仍缺乏明确关联的修复 PR。总体而言，项目看起来健康且维护活跃，但存在相当大的积压，以及 gateway、cron 和多 profile 子系统中可见的可靠性债务。

---

## 2. 版本发布

过去 24 小时内没有发布新版本。

---

## 3. 项目进展

报告窗口内有 10 个 PR 被合并或关闭。在可见的已关闭 PR 中，进展集中在交付可观测性、资源安全、文档卫生和 Kanban 正确性方面：

- [PR #98918](https://github.com/NousResearch/hermes-agent/pull/98918) — 使本地 STT 回退可配置，避免在小型 VPS 实例上使用昂贵的本地 Whisper 回退。
- [PR #98917](https://github.com/NousResearch/hermes-agent/pull/98917) — 在交付中添加可复制的 cron 任务和运行 ID，以便于调试。
- [PR #98916](https://github.com/NousResearch/hermes-agent/pull/98916) — 删除任务时保留选定的 Kanban 看板；作为重复项关闭。
- [PR #98915](https://github.com/NousResearch/hermes-agent/pull/98915) — 将自动 TTS 输出保留在 `HERMES_HOME` 下，而不是系统临时目录。
- [PR #73923](https://github.com/NousResearch/hermes-agent/pull/73923) — 在交付账本中持久化 `provider_message_id`、`delivery_route` 和 `chunk_count` 以便追溯。
- [PR #63812](https://github.com/NousResearch/hermes-agent/pull/63812) — 使运行时页脚使用有效的 OAuth 和 Fast 信号，而不是硬编码的提供商假设。
- [PR #74573](https://github.com/NousResearch/hermes-agent/pull/74573) — 修复了过时的简体中文技能文档交叉引用。
- [Issue #78980](https://github.com/NousResearch/hermes-agent/issues/78980) — 已关闭：cron 生命周期守卫在包含 `~/...` 路径字面量的 Python 脚本上误报。

**推进的主题：** 交付可追溯性、TTS/STT 资源安全、文档准确性、Kanban 删除正确性以及 cron 守卫误报。值得注意的是，若干已关闭的 PR 已存在数月，表明这是一次积压清理，而非纯粹的当日功能工作。

---

## 4. 社区热点话题

### 参与度最高的 Issue

1. **[Issue #143379](https://github.com/NousResearch/hermes-agent/issues/143379)** — *RFC：DOC-CHAIN 基质的网络——读取、治理、压缩*  
   **30 条评论** · P2 · security、agent、tools、file、memory、compression  
   讨论最多的条目。该 RFC 涉及持久记录在会话压缩后仍保留来源信息，以及 agent 重新读取该记录而不是让用户重复信息。  
   **底层需求：** 长周期记忆正确性、来源保留以及压缩上下文的治理。这是跨长会话运行的个人 AI 助手的核心架构关切。

2. **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — *[功能]：Hermes Desktop 仅前端安装*  
   **23 条评论 · 31 👍** · P2 · needs-decision · Windows  
   用户希望仅安装桌面前端并连接到已在另一台机器上运行的 agent。  
   **底层需求：** 部署灵活性和远程 agent UX。高反应数表明需求强烈，尤其是对 Windows 和分机设置。

3. **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — *source-completion-pending 没有 TTL 或自愈机制*  
   **15 条评论** · P1 · install/update  
   被终止的更新尾部可能使 `source-completion-pending` 陷入困境，导致后续每次启动都重新进入相同状态。  
   **底层需求：** 具有 TTL 和自愈能力的崩溃安全安装/更新状态机。

4. **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — *Anthropic 每请求凭证刷新会回退凭证池轮换——两个池化订阅被计为一个*  
   **11 条评论 · 1 👍** · P1 · auth、billing、security boundary  
   两个池化订阅可能被计为一个，且该轮次可能落到另一个提供商。  
   **底层需求：** 正确的凭证池轮换和计费隔离——多订阅用户的高信任要求。

5. **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — *cron：重启安全 worker 在 `adopt_claimed_execution()` 中因 `database is locked` 而死亡*  
   **10 条评论 · 1 👍** · P1  
   重启安全的 cron worker 在一次 `SQLite` 锁超时后退出，丢失该次执行。  
   **底层需求：** SQLite 争用下的持久调度器行为，具有重试和安全采纳语义。

6. **[Issue #145022](https://github.com/NousResearch/hermes-agent/issues/145022)** — *gateway 停顿时 Kanban worker 以“Kanban execution unknown; retained by owner”崩溃*  
   **10 条评论** · P1 · sessions  
   当 gateway 在保存期间停顿时，并发 Kanban worker 崩溃；进行中的工作丢失。  
   **底层需求：** 负载下健壮的 gateway/worker 协调和会话所有权恢复。

### PR 参与度说明

PR 评论数不可用。值得注意的开放修复 PR 包括 [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435)（source-completion 尾部）、[PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792)（凭证池轮换/429 处理）、[PR #146703](https://github.com/NousResearch/hermes-agent/pull/146703)（Telegram 通用池恢复）和 [PR #143956](https://github.com/NousResearch/hermes-agent/pull/143956)（Discord 输入状态暂停）。

---

## 5. Bug 与稳定性

### P1 — 严重

1. **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — `source-completion-pending` 没有 TTL 或自愈机制；被终止的尾部使后续每次启动都陷入困境。  
   **修复状态：** 可能相关的修复 PR [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435) 处于开放状态（“在运行前收取欠下的 source-completion 尾部”）。Issue 标记为 `awaiting-reporter`。

2. **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — Anthropic 每请求凭证刷新会回退凭证池轮换；池化订阅可能被计为一个。  
   **修复状态：** 相关的池轮换 PR [PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792) 处于开放状态，但尚未确认是该计费/轮换回归的完整修复。

3. **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — 重启安全 cron worker 因 `database is locked` 死亡；执行丢失。  
   **修复状态：** 所提供数据中未识别到直接修复 PR。

4. **[Issue #145022](https://github.com/NousResearch/hermes-agent/issues/145022)** — worker 保存期间 gateway 停顿时 Kanban worker 崩溃；进行中的工作丢失。  
   **修复状态：** 未识别到直接修复 PR。[PR #146714](https://github.com/NousResearch/hermes-agent/pull/146714) 处理清理期间未初始化的兄弟 Kanban 数据库，这是相邻问题，但不是该停顿的确认修复。

5. **[Issue #137170](https://github.com/NousResearch/hermes-agent/issues/137170)** — Agent 循环对重复的相同*成功*工具调用没有断路器；观察到一轮中出现 430 次重复的 `kanban_comment` 调用。  
   **修复状态：** 未识别到直接修复 PR。这是一个值得注意的 agent 安全缺口：现有的循环检测覆盖重复失败，但不覆盖重复成功。

### P2 — 高影响集群

**Cron 持久性与交付**
- [Issue #146428](https://github.com/NousResearch/hermes-agent/issues/146428) — 在多路复用 profile 中，cron 运行在 home 目标解析期间因 `UnscopedSecretError` 静默卡住。
- [Issue #101623](https://github.com/NousResearch/hermes-agent/issues/101623) — `--continuity` 将前一次输出文件以提示优先方式注入，截断先前响应并嵌套提示。
- [Issue #145596](https://github.com/NousResearch/hermes-agent/issues/145596) — 暂停的 cron 任务在 gateway 重启后重新激活。
- [Issue #146668](https://github.com/NousResearch/hermes-agent/issues/146668) — 企业微信主动发送挂起；`_dispatch_on_gateway_loop` 没有超时并卡住交付队列。
- [Issue #143953](https://github.com/NousResearch/hermes-agent/issues/143953) — 多路复用 gateway：cron 运行后卫星 Discord 会话暂停为 `not_found`；向路由频道的交付失败。

**Gateway 性能与会话状态**
- [Issue #146643](https://github.com/NousResearch/hermes-agent/issues/146643) — Gateway 在每次变更时重新读取整个会话历史；数千个会话导致 1.5–2.5 个 CPU 核心和 30 秒桌面超时。
- [Issue #120856](https://github.com/NousResearch/hermes-agent/issues/120856) — Gateway 重启通过共享的 `processes.json` 采纳并随后杀死其他进程的后台任务。
- [Issue #146507](https://github.com/NousResearch/hermes-agent/issues/146507) — 异步委托账本 SQLite 操作在 gateway 事件循环上运行，导致 10–20 秒停顿并阻塞 Discord 心跳。
- [Issue #139210](https://github.com/NousResearch/hermes-agent/issues/139210) — 压缩将早期用户消息的副本作为新行写入 `state.db`。
- [Issue #138573](https://github.com/NousResearch/hermes-agent/issues/138573) — Discord 语音笔记会话获得垃圾标题（“Empty message greeting”）并永久锁定。

**CLI、安装与配置**
- [Issue #125537](https://github.com/NousResearch/hermes-agent/issues/125537) — `pm gc` 测量执行快照的安装键并静默收集不到任何内容。
- [Issue #139500](https://github.com/NousResearch/hermes-agent/issues/139500) — 约 22,600 个 `.pyc` 文件使即使小型安装也膨胀超过 303 MiB。
- [Issue #44964](https://github.com/NousResearch/hermes-agent/issues/44964) — `/sessions` CLI 命令仅显示 `source=cli` 会话；TUI 和其他来源被隐藏。
- [Issue #90040](https://github.com/NousResearch/hermes-agent/issues/90040) — `skills.external_dirs` 忽略 Managed Scope 且必须按 profile 重复配置。

**Kanban、提供商与插件问题**
- [Issue #146237](https://github.com/NousResearch/hermes-agent/issues/146237) — 桌面 Kanban 卡片重新分配给另一个 profile 时失败，报 `403 profile_scope_mismatch`。
- [Issue #145480](https://github.com/NousResearch/hermes-agent/issues/145480) — `kimi-coding` 应用内 `/models` 发现失败；静态列表覆盖缓存目录。
- [Issue #124874](https://github.com/NousResearch/hermes-agent/issues/124874) — HTTP-200 拒绝/无效响应后的回退未记录原因，并被错误标记为“provider failure”。

### 值得跟踪的开放修复 PR
- [PR #146046](https://github.com/NousResearch/hermes-agent/pull/146046) — 长时间运行进程的临时空闲规则。
- [PR #146703](https://github.com/NousResearch/hermes-agent/pull/146703) — Telegram 通用池恢复。
- [PR #143956](https://github.com/NousResearch/hermes-agent/pull/143956) — 跨审批/SDK 等待的 Discord 输入暂停。
- [PR #126506](https://github.com/NousResearch/hermes-agent/pull/126506) — Telegram RTL 富消息标志。
- [PR #126889](https://github.com/NousResearch/hermes-agent/pull/126889) — 浏览器保险库卡片同意门控。
- [PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792) — 在等待 429 之前进行凭证池轮换。

---

## 6. 功能请求与路线图信号

- **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — 桌面仅前端安装并支持远程连接。**23 条评论，31 👍，`needs-decision`。** 这是数据集中最强的路线图信号：用户希望将 UI 与 agent 运行时分离，尤其是在 Windows 上。
- **[Issue #35147](https://github.com/NousResearch/hermes-agent/issues/35147)** — 在单用户会话上下文中暴露稳定的 `user_id` 以及可变的 `user_name`，用于多用户团队 profile 和身份映射。
- **[Issue #142235](https://github.com/NousResearch/hermes-agent/issues/142235)** — 为插件后端提供公共只读 MCP 工具调用，由 `readOnlyHint` 门控。这将让仪表板和 CLI 插件安全地读取已配置的 MCP 服务器。
- **[Issue #146699](https://github.com/NousResearch/hermes-agent/issues/146699)** — Cron agent 无法查看其任务的运行历史或某次运行是否已交付或被静默。请求从 agent 自身工具中获得更好的 cron 可观测性。
- **[Issue #143577](https://github.com/NousResearch/hermes-agent/issues/143577)** — 关于 1.0 GB `~/.hermes/cache/uv/` 目录（位于所有清理器之外）的文档/FAQ 请求。
- **[PR #110909](https://github.com/NousResearch/hermes-agent/pull/110909)** — 后台终端进程完成后可选加入的 cron 继续执行；与长时间运行的定时工作流相关。

**预测：** 下一个版本可能优先考虑安装/更新和部署 UX（仅前端桌面、缓存/清理器行为）、cron 可观测性和继续执行语义，以及 MCP/插件集成。多 profile 身份和范围处理也在 bug 和功能请求中反复出现，表明它可能成为更广泛的路线图主题。

---

## 7. 用户反馈摘要

**真实痛点**
- **安装占用与缓存管理：** 用户报告小型安装中约 22,600 个 `.pyc` 文件和超过 303 MiB 的膨胀（[#139500](https://github.com/NousResearch/hermes-agent/issues/139500)），以及位于所有清理器之外的 1.0 GB `uv` 缓存（[#143577](https://github.com/NousResearch/hermes-agent/issues/143577)）。#139500 中的语气（“开发团队拒绝修复”）表明对长期存在的安装卫生问题感到沮丧。
- **部署灵活性：** 仅前端安装请求（[#38519](https://github.com/NousResearch/hermes-agent/issues/38519)）反映了一个真实的分机使用场景：agent 在一台主机上，桌面 UI 在另一台上。
- **后台自动化的可靠性：** Cron 和 Kanban 用户遇到丢失执行、静默卡住、暂停任务重新激活和 worker 崩溃（[#135487](https://github.com/NousResearch/hermes-agent/issues/135487)、[#146428](https://github.com/NousResearch/hermes-agent/issues/146428)、[#145596](https://github.com/NousResearch/hermes-agent/issues/145596)、[#145022](https://github.com/NousResearch/hermes-agent/issues/145022)）。
- **规模与会话状态：** 几千个会话即可导致高 gateway CPU 和桌面超时（[#146643](https://github.com/NousResearch/hermes-agent/issues/146643)）；压缩可能在 `state.db` 中重复用户消息（[#139210](https://github.com/NousResearch/hermes-agent/issues/139210)）。
- **多 profile 与身份摩擦：** Profile 范围不匹配、托管范围配置被忽略以及路由的 Discord/cron 会话表明多 profile 设置仍然粗糙（[#146237](https://github.com/NousResearch/hermes-agent/issues/146237)、[#90040](https://github.com/NousResearch/hermes-agent/issues/90040)、[#143953](https://github.com/NousResearch/hermes-agent/issues/143953)）。
- **提供商与计费信任：** 凭证池轮换回退和池化订阅计为一个（[#118379](https://github.com/NousResearch/hermes-agent/issues/118379)）对付费用户来说是一个高信任问题。

**数据中可见的使用场景：** 远程桌面对 agent 连接、多用户团队 profile、基于 Kanban 的工作流自动化、具有交付保证的定时 cron 任务、读取 MCP 服务器的插件后端，以及包括 Discord、Telegram 和企业微信在内的平台集成。

**满意度/不满意度：** 社区深度参与并贡献详细的报告和 PR，包括插件维护者更新自己的目录条目（[PR #146713](https://github.com/NousResearch/hermes-agent/pull/146713)）。然而，反复出现的根因家族以及在安装膨胀和会话可见性方面进展缓慢，表明维护者的注意力被分散。整体情绪是建设性的，但对可靠性敏感。

---

## 8. 积压关注

### 需要维护者关注的长期开放 Issue

- **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — 创建于 2026-06-03 · 23 条评论 · 31 👍 · `needs-decision` · Windows 仅前端安装。高需求功能，但无明显决策。
- **[Issue #35147](https://github.com/NousResearch/hermes-agent/issues/35147)** — 创建于 2026-05-30 · 3 条评论 · 在会话上下文中暴露 `user_id`。长期未答复但对多用户设置很重要。
- **[Issue #44964](https://github.com/NousResearch/hermes-agent/issues/44964)** — 创建于 2026-06-12 · 8 条评论 · `/sessions` 仅显示 CLI 会话。会话管理中一个明显的可用性缺口。
- **[Issue #90040](https://github.com/NousResearch/hermes-agent/issues/90040)** — 创建于 2026-08-19 · 4 条评论 · `skills.external_dirs` 忽略 Managed Scope。对管理共享技能目录的管理员很重要。
- **[Issue #120381](https://github.com/NousResearch/hermes-agent/issues/120381)** — 创建于 2026-09-23 · 5 条评论 · `/goal` 在子 agent 等待停滞后可能保持挂起；issue 称其由 [PR #107130](https://github.com/NousResearch/hermes-agent/pull/107130) 实现，但该 PR 仍处于开放状态。
- **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — 创建于 2026-09-29 · P1 · 15 条评论 · `awaiting-reporter`。需要报告者跟进或维护者复现以解除 [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435) 中可能的修复的阻塞。
- **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — 创建于 2026-09-21 · P1 · 11 条评论 · 凭证池计费/轮换。高严重性和高信任影响。
- **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — 创建于 2026-10-09 · P1 · 10 条评论 · cron worker 因 SQLite 锁死亡。无明显直接修复 PR。

### 值得审查的长期开放 PR

- **[PR #110909](https://github.com/NousResearch/hermes-agent/pull/110909)** — 创建于 2026-09-14 · 后台进程完成后 cron 继续执行。一个有意义的自动化功能，等待审查。
- **[PR #92149](https://github.com/NousResearch/hermes-agent/pull/92149)** — 创建于 2026-08-22 · 保留记忆上下文来源边界。与 doc-chain/memory RFC 讨论直接相关。
- **[PR #126889](https://github.com/NousResearch/hermes-agent/pull/126889)** — 创建于 2026-09-28 · 保险库卡片同意门控。一个已等待超过一个月的安全边界修复。
- **[PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792)** — 创建于 2026-10-21 · 在 429 等待前进行池轮换。与 P1 凭证池 issue 相关。
- **[PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435)** — 创建于 2026-10-27 · source-completion 尾部修复。一个 P1 领域的修复，应与 issue 分类同步优先处理。

**健康摘要：** 项目活跃且受社区支持，在交付追踪、TTS/STT 安全和文档方面有明显进展。主要风险是 cron、gateway/会话状态、安装/更新和多 profile 行为方面不断增长的可靠性积压——这些领域用户正在类生产、长时间运行、多平台的设置中运行 Hermes。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

## 今日概览
2026-10-30，IronClaw 呈现**低强度、仅维护性活动**：过去 24 小时内 0 个 issue 更新，0 个 PR 合并或关闭，0 个新版本发布。唯一可见的活动是两个开放的 Dependabot PR 正在更新 `/harness/latency/runner` 中的 `wasmtime`（[#8162](https://github.com/nearai/ironclaw/pull/8162)、[#8163](https://github.com/nearai/ironclaw/pull/8163)）。两个 PR 均显示 0 个 👍 反应，所提供数据中评论数不可用，且均未合并。当日项目整体健康状态平静，而非由功能或修复驱动；维护者的主要动作是协调处理两个重叠的依赖升级。

## 项目进展
今日没有 PR 被合并或关闭，也没有功能推进或缺陷修复。两个开放 PR 仅为依赖维护变更：
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — 将 `wasmtime` 从 `47.0.3` 升级到 `48.0.7`。
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — 将 `wasmtime` 从 `47.0.4` 升级到 `49.0.5`。

两者都指向同一路径 `/harness/latency/runner`，但提出的目标版本不同。这表明要么存在重复/过期的 Dependabot 分支，要么存在不同的依赖清单；维护者应在合并前确认哪条升级路径正确。

## 社区热点
没有记录到带评论或反应的 issue 或 PR，因此没有出现真正的“热点话题”。最显眼的条目是两个开放的 Dependabot PR：
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — `wasmtime` 47.0.3 → 48.0.7
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — `wasmtime` 47.0.4 → 49.0.5

底层需求是针对延迟测试框架（latency harness）的常规依赖保持最新与兼容性验证。由于两个 PR 涉及同一依赖和同一目录，社区/维护者的需求与其说是讨论，不如说是**去重和升级路径决策**。

## 缺陷与稳定性
24 小时窗口内未报告缺陷、崩溃或回归。不存在修复 PR。唯一潜在稳定性考量是两个依赖 PR 中 `wasmtime` 的主版本跳跃；提供的发布说明摘要被截断，因此仅凭该数据集无法评估破坏性变更风险。

## 功能请求与路线图信号
未记录用户请求的功能或路线图 issue。唯一信号是依赖维护 PR。数据集中不存在版本发布，因此无法预测下一版本功能；若后续有发布，可见输入表明其更可能是依赖维护，而非有文档记录的面向用户功能。

## 用户反馈摘要
未捕获直接用户反馈：更新 issue 数为零，且没有可用的 PR 评论。因此，无法评估用户痛点、用例以及满意度/不满意度。issue 活动缺失可能表明处于平静/稳定期或参与度低，但数据不足以区分这些解释。

## 积压观察
无法识别长期未答复的 issue，因为 issue 列表包含 0 项。两个开放 PR 是近期创建的——创建于 2026-10-29——因此尚不属于积压事项，但需要维护者关注以避免变得陈旧：
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — 审查 `wasmtime` 48.0.7 是否为预期目标版本。
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — 审查 `wasmtime` 49.0.5 是否取代 #8162，或属于不同的清单文件。

建议操作：协调处理重叠的升级，在兼容性检查后合并合适的一项，并关闭任何已被取代的 PR。

---

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 — 2026-10-30

---

## 1. 今日概览

QwenPaw 正处于**高速推进的 beta 稳定阶段**：过去 24 小时触及 55 个跟踪项（15 个 issue、40 个 PR），其中 10 个 issue 已关闭，5 个仍开放/活跃；13 个 PR 已合并或关闭，27 个仍开放。未切出新版本，因此所有活动都在向下一项 `2.3.0` 构建积累——当日 bug 报告紧密集中在 `2.3.0-beta.1` 回归上（插件加载器、桌面资源、本地 Whisper 导入）。相当一部分关闭项是较早的增强请求（5–7 月提交），仅以 1–2 条评论关闭，这更像是批量分诊，而非已交付实现。相反，开放 PR 队列由超大型提交主导（`size/XL`、`XXL`、`XXXL`），其中若干已开放 4–7 周，表明**评审带宽——而非贡献者兴趣——才是当前瓶颈**。总体健康度在响应性方面良好，但在新的插件/桌面架构中带有更高的回归风险。

*数据说明：源数据中 PR 评论数为 `undefined`，且 40 个 PR 中仅列出 20 个，因此下文的 PR“热度”是根据标签、规模和时长推断，而非基于反应数。*

---

## 2. 版本发布

无（0 个新版本；数据集中未记录任何发布）。本窗口不适用任何破坏性变更或迁移说明。

---

## 3. 项目进展

### 今日已关闭/已合并 PR（展示前 20 中的 4 个；窗口内共 13 个）

| PR | 标题 | 意义 |
|---|---|---|
| [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) | `fix(plugins): tolerate import cache refresh failures during plugin loading` | **直接修复当日最严重回归**（[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473)）——所有后端插件在 `2.3.0-beta.1` 上启动失败。 |
| [#8358](https://github.com/agentscope-ai/QwenPaw/pull/8358) | `fix(providers): honor thinking defaults and declared wire controls` | 修复声明原生 thinking effort（`low/medium/xhigh`）的 OpenAI 兼容模型在 Console 与 wire 之间的不匹配。 |
| [#8253](https://github.com/agentscope-ai/QwenPaw/pull/8253) | `feat(website): add JSON-driven evaluation index` | 双语 `/evaluation/` 排行榜，含 SVG 成本/性能图和 Pareto 前沿——提升项目透明度/基准测试能力。 |
| [#5490](https://github.com/agentscope-ai/QwenPaw/pull/5490) | `feat(console): navigable fullscreen image gallery for chat media` | 自 **2026-06-24** 起开放（约 4 个月）；统一的 `Image.PreviewGroup` 取代孤立的逐图预览。 |

### 值得注意的已关闭 issue

- **[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473)** —— 在 `module_isolation.py:220` 处插件加载失败，*与 #8265 根因相同，但出现在新的调用点* → 未加保护的 `importlib.invalidate_caches()` 第二次出现；由 #8497 修复。
- **[#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464)** —— 手动 `/compact` 忽略会话级模型切换，始终使用 `agent.json` 的 `active_model`；对使用严格校验 provider 的用户构成阻塞（HTTP 400）。
- **[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150)** —— 飞书入站富文本（`post`）图片被静默丢弃（文字送达，媒体被丢弃，无警告）。这是出站问题 #2792 的对应问题。
- **[#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116)** —— agent 死循环；在 9 条评论后以 `wontfix` 关闭（当日讨论最多的 issue）。

### 开放 PR 推进的功能方向
- **记忆子系统重构** —— [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457)（将自动召回作为瞬态 `TextBlock` 参考上下文）、[#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835)（停止将召回载荷泄漏到渠道）、[#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291)（ReMe 重排序器要求非空 `api_key`）、[#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613)（OpenViking 后端插件）。
- **Provider/插件拆分** —— [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499) 将本地模型管理（下载、llama.cpp 安装/生命周期）抽取为插件 provider；[#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231) 持久化市场安装来源，使“Update All”覆盖第三方插件。
- **PawApp 控制平面** —— [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) 重新设计 SDK 与应用宿主契约（任务标识、重试、权限、审批）。

---

## 4. 社区热点话题

| 排名 | 条目 | 信号 | 底层需求 |
|---|---|---|---|
| 1 | [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) —— 死循环，重复相同工具调用 | **9 条评论**，以 `wontfix` 关闭 | 用户希望在**运行时层实现成本控制和循环打断**，而不是重复约 6 次后才警告。当日互动度最高的讨论串被标记 `wontfix`，很可能被解读为“token 浪费是可接受行为”——值得维护者给出理由，或记录缓解方案（预算限制、调用指纹去重）。 |
| 2 | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) —— `/compact` 忽略会话模型 | 5 条评论，已关闭 | **会话级配置一致性。** 一个 340k token、`max_input_length=1M` 的会话并非“长尾”用法——上下文管理正确性是重度用户的核心体验。 |
| 3 | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) —— 飞书 `post` 图片被静默丢弃 | 3 条评论，已关闭 | **渠道保真度 + 可观测性。** 真正被抱怨的是“静默”：用户无法区分“没有图片”和“图片被吞了”。 |
| 4 | [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500) —— Auto-Dream 丢弃记忆，却仍做检查点 | 2 条评论，开放 | **持久性保证与诚实的状态报告**——数据已丢失却返回 `success=True` 是信任失败。 |
| 5 | [#6189](https://github.com/agentscope-ai/QwenPaw/issues/6189) —— `save_decision()` API | 2 条评论，已关闭；提交于 2026-07-16 | **跨压缩的决策持久化。** 作者正确指出先有鸡还是先有蛋的问题：等到 agent 应该归档决策时，它已经丢失了该决策。 |
| 6 | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) —— 所有插件加载失败 | 2 条评论，已关闭，已有修复 PR | Beta 测试者在真实主机上运行 12+ 个插件；**隔离层的回归覆盖很薄弱。** |
| 7 | [#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613) —— OpenViking 记忆插件 | `size/XXL`，首次贡献者，**开放 53 天**，仍在接收评审跟进 | 社区对**可插拔记忆后端**的需求；同时也是留存风险——首次贡献者的 XXL PR 评审周期过长。 |

**模式：** 排名前三的讨论串都关乎*单轮/会话中的状态与成本管理*（循环控制、压缩模型路由、渠道媒体），而非新能力。

---

## 5. Bug 与稳定性

按用户影响排序（数据丢失/完全不可用 > 静默降级 > 外观问题）。

| 严重度 | Issue | 状态 | 影响 | 修复 PR？ |
|---|---|---|---|---|
| **严重** | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) —— 全部 12/12 后端插件在启动时失败，`2.3.0-beta.1`（`module_isolation.py:220`） | 已关闭 | 插件用户完全失去功能；**#8265 根因在新调用点复发** | ✅ [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) 已关闭 |
| **严重（静默数据丢失）** | [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500) —— Auto-Dream 在抽取解析失败时丢弃整天的记忆，**仍然对文件做检查点**，并报告 `success=True` | **开放** | 不可恢复的记忆丢失，且无用户可见信号；个人助理产品中最糟糕的一类 bug | ❌ 无 |
| **高** | [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) —— desktop/PyInstaller 构建遗漏 `visual_compression` 字形图集 → `FileNotFoundError: atlas-gray-native.ts`；Visual Compact 静默空操作 | **开放** | 仅影响打包后的 Windows 桌面端；一个节省上下文的功能静默不生效，意味着用户在不知情的情况下消耗 token | ❌ 无 |
| **高** | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) —— 手动 `/compact` 使用 `active_model`，而非会话模型 | 已关闭 | 对严格 provider，压缩永久不可用（HTTP 400） | 未列出 |
| **高** | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) —— 飞书入站 `post` 图片被静默丢弃 | 已关闭 | 多模态输入丢失，无警告；#2792 的反方向 → **渠道媒体处理缺乏对称测试** | 未列出 |
| **中** | [#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490) —— `GET /api/workspace/local-whisper-status` 实际执行 `import whisper`；约 690 MB RSS，且永不释放 | **开放** | 仅*打开设置页*就会永久抬高服务器内存；可选依赖探测应使用 `importlib.util.find_spec` | ❌ 无 |
| **中** | [#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502) —— 桌面端“在浏览器中打开”在中文路径下失败（找不到文件），且在工作区外空操作 | **开放**（提交于 2026-10-30） | 默认 zh-CN Windows 上明确的 i18n/路径编码缺陷；对最大用户语言区域而言，核心预览流程损坏 | ❌ 无 |
| **低** | [#8486](https://github.com/agentscope-ai/QwenPaw/issues/8486) —— 工具调用 `<>` 图标被裁切（容器缺少水平内边距） | **开放** | 外观问题，在更大字体缩放/缩放比例下更严重 | ❌ 无 |
| **低（设计）** | [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) —— 仅在约 6 次重复后才检测到死循环 | 已关闭 `wontfix` | 每次发生都浪费 token/API 开销 | ❌ 已拒绝 |

**稳定性解读：** 严重度前 4 项中有 3 项是**仅存在于 beta 的打包/插件路径回归**（`2.3.0b1`），也就是说架构变化速度快于分发测试。两个静默失败 bug（[#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500)、[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)）有一个共同主题：*功能报告成功却什么也没做*——这是系统性可观测性缺口，值得专门修复，而不是打两个补丁。

---

## 6. 功能请求与路线图信号

### 今日关闭的请求（提交于 5–7 月；以 1–2 条评论关闭）
| Issue | 请求 | 提交日期 | 信号 |
|---|---|---|---|
| [#6189](https://github.com/agentscope-ai/QwenPaw/issues/6189) | `save_decision()` 结构化决策持久化，以在压缩后仍保留 | 2026-07-16 | 对长时程 agent 价值高；属于框架原语，在 agent 层很难绕过 |
| [#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356) | 更细粒度的文件守卫（只读路径；用项目目录允许列表替代单一全局黑名单） | 2026-05-14 | **安全/防护姿态**——本批中最与企业相关的一项诉求 |
| [#7740](https://github.com/agentscope-ai/QwenPaw/issues/7740) | Hub 模式：管理员无法重置用户密码 | 2026-09-14 | 基础多租户运维缺口；Hub 部署因需要手动操作数据库而受阻 |
| [#4129](https://github.com/agentscope-ai/QwenPaw/issues/4129) | `/rewind` 回滚文件/上下文 | 2026-05-08 | 直接对标 Claude Code；最重要的“功能对等缺失”项之一 |
| [#8459](https://github.com/agentscope-ai/QwenPaw/issues/8459) | 更便捷地访问未读推送消息（Console） | 2026-10-28 | 异步通知体验；2 天后关闭 |
| [#8498](https://github.com/agentscope-ai/QwenPaw/issues/8498) | “install easily”（空模板） | 2026-10-29 | 入门摩擦，但无可执行内容 |

### 对下一版本的预测（`2.3.0` 系列）
1. **插件/Provider 拆分** —— [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499)（本地模型管理 → 插件）和 [#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231)（“Update All”的市场来源信息）都接近 `Under Review`，且与 beta 的插件工作方向一致。**最可能落地。**
2. **记忆召回正确性** —— [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) + [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) + [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291) 构成一个连贯集群，修复合成工具调用泄漏。**可能在 2.3.x 中落地。**
3. **PawApp SDK/控制平面**（[#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874)，`XXXL`）——架构上很基础，但已开放 41 天；**可能推迟到 2.4。**
4. **本地模型评分**（[#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463)，用于 Jev 决策/ReMe 压缩的 Piccolo 双向候选评分）——**实验性，尚不确定。**

**不可忽视的信号：** 一天内有五个长期 enhancement 以极少讨论关闭，而相应代码变更并未出现在 PR 列表中。如果这些已实现，摘要应给出对应 PR；如果被判定为过期，则 [#4129](https://github.com/agentscope-ai/QwenPaw/issues/4129) 和 [#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356) 的用户会重新提交。

---

## 7. 用户反馈摘要

**发声者是谁：** 主要是使用 Windows 桌面端和 Hub 部署的中文用户，以及大量使用渠道的用户（飞书、QQ、微信、企业微信、钉钉、OneBot、元宝）。

**具体痛点：**
- **静默失败侵蚀信任。** [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500)（“运行报告 `success=True`……没有任何用户可见信息表明内容被丢弃”）、[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150)（“静默丢弃……无警告”）、[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)（“Visual Compact 静默空操作”）。三份独立报告，同一个抱怨：*产品失败时不告诉我。*
- **成本焦虑。** [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) 明确量化了浪费（“许多 API 调用和 token 已被浪费”）。用户正在计量其 agent 运行成本。
- **长上下文重度使用已是主流，而非边缘场景。** [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) 记录了一个 340k token 的会话。压缩正确性现在是一等可靠性要求。
- **桌面端的本地化/资源卫生。** [#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502)（中文路径破坏浏览器打开）、[#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490)（设置页占用 690 MB RSS）。桌面端打磨落后于后端。
- **安全默认值。** [#8501](https://github.com/agentscope-ai/QwenPaw/pull/8501) —— `env list` 以明文打印所有密钥，`env set` 回显已存储的值。社区发起的修复，强烈表明此前并不存在默认安全。
- **满意度指标：** 带有版本/提交固定的详细、可复现报告（`2.3.0b1`、`b12b06ed`、`2.2.2.post1`）表明用户参与度高、技术成熟并投入了真实精力——首次贡献者（[#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613)、[#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400)、[#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291)）也展现出善意。**不满集中在 beta 回归的响应速度，以及对成本相关 issue 的 `wontfix` 决定上。**

---

## 8. 积压观察

### 需要维护者决策的长期开放 PR
| PR | 规模 | 开放时长 | 风险 |
|---|---|---|---|
| [#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613) —— OpenViking 记忆插件 | `XXL`，首次贡献者 | **53 天**（自 09-07） | 作者仍在推进评审跟进（并发/安装 ID 修复）。流失风险最高的条目；停滞的首次贡献者 XXL PR 会打击后续贡献意愿。 |
| [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) —— PawApp SDK 与应用控制平面重构 | `XXXL` | 41 天（自 09-19） | 基础性工作；阻塞下游应用工作，且规模太大难以随手评审——需要明确的设计评审时段或拆分。 |
| [#8034](https://github.com/agentscope-ai/QwenPaw/pull/8034) —— **按请求**限制内联媒体，而非按文件 | `XL` | 31 天（自 09-29） | 真实生产故障模式：2 MB/文件上限 + 约 0 token 成本 ⇒ 无界的数 MB 请求体，被网关字节上限拒绝。基于 token 的驱逐不会触发。 |
| [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) —— 停止将自动记忆召回载荷泄漏到渠道 | `M` | **43 天** | 内部召回脚手架泄漏到聊天渠道，用户可见；与 [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) 重叠，存在**重复/冲突修复**风险——需要统一决策。 |
| [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499) —— 将本地模型管理抽取为插件 | `XXXL`，WIP | 1 天 | 尚早，但需留意相对 [#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463) 的范围蔓延。 |
| [#8308](https://github.com/agentscope-ai/QwenPaw/pull/8308) / [#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400) / [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291) | `XS`–`S` | 12–13 天 | 小型、低风险修复排在队列中大 PR 之后——适合走快速通道以保持贡献者势头。 |

### 需要维护者关注的 issue
- **[#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500)**（Auto-Dream 静默记忆丢失）——开放且**无关联修复 PR**；今日集合中唯一不可恢复数据丢失 bug。应成为明天队列的首要项。
- **[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)**（桌面构建缺少 `visual_compression` 资源）——开放，无修复；很可能只是 PyInstaller spec/datas 的一行条目，因此是低成本、高价值的收益。
- **[#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490)**（Whisper import = 690 MB 永久 RSS）——开放，无修复；通过 `find_spec` 即可轻松解决。
- **[#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356)**（file-guard 只读/允许列表粒度）——在 5+ 个月、2 条评论后关闭，且无可见的实现 PR。鉴于它涉及 **agent 文件系统安全**，建议公布解决理由或重新打开。
- **[#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502)**（中文路径浏览器打开失败）——今日提交，但具有一类问题的代表性；需要在其变旧之前加上分诊标签并指定负责人。

---

**结论：** 吞吐量强劲，贡献者也在积极参与，但本窗口显示出**三个结构性风险**——（a）beta 打包/插件回归泄漏到用户侧（[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473)、[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)）；（b）静默失败模式侵蚀用户对记忆和上下文功能的信任（[#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500)、[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150)）；（c）大型 PR 评审队列中最老的条目如今已 40–53 天（[#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613)、[#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874)）。这些并不需要新增工程能力，更需要的是一套分诊策略：小型 PR 快速通道、“静默失败”测试类别，以及对存在重叠的记忆召回 PR 做出决策。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-10-30

---

## 1. 今日概览

ZeroClaw 依然高度活跃：过去 24 小时内共有 **50 条 issue 更新**（33 条开放/活跃，17 条已关闭）和 **50 条 PR 更新**（44 条开放，6 条已合并/关闭），但**未发布新版本**。最值得注意的信号是“接收—合并”缺口扩大：44 个 PR 仍处于开放状态，而仅有 6 个关闭；大量开放 PR 带有 `size:XL`、`needs-author-action` 或 `needs-maintainer-review`，说明瓶颈在评审产能，而非贡献者兴趣。活动集中在四个主题：**身份与访问安全**（按 agent 归属、RBAC、principal 作用域）、**仅追加的会话/事件持久化**、**插件系统可靠性**，以及 **ZeroCode TUI 打磨**。今天关闭了 17 个 issue，反映出实打实的清理势头——包括一项已接受架构 RFC 和若干 S2 bug 修复——同时 v0.9.0 里程碑仍在累积带安全标签的工作。

---

## 2. 发布

过去 24 小时没有新发布。项目正处于近期的 **v0.8.7** 文档/目录发布与不断累积的 **v0.9.0** 里程碑之间（issue #9394、#11970、#12076 和 PR #12095 带有 `release:v0.9.0`）。

---

## 3. 项目进展

**已合并/关闭的 PR（报告的 6 个中可见 1 个）：**
- [#12095](https://github.com/zeroclaw-labs/zeroclaw/pull/12095) — `fix(deps): bump pulldown-cmark to 0.13.5 for RUSTSEC-2026-0414`。低风险、`size:XS`，但影响很大：该安全公告于 2026-10-28 落地，导致 Security job 失败，进而阻塞了**每一次**合并的 `CI Required Gate`。这解除了流水线阻塞。

**代表实际进展的已关闭 issue：**
- [#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) — **RFC 在 14 条评论后获接受并关闭**（仅追加的会话事件历史、确定性重放、派生 agent 流）。实现已作为 [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921) 在进行中，使其成为当前推进中的最大单项架构进展。
- [#11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562) — 插件发现流程在更新期间可能将一个代的 manifest 与另一个代的组件配对（S2）。已关闭。
- [#11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432) — 守护进程在回合中途被杀后，会话会永久停留在 `state = 'running'`。已关闭。
- [#11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517) — Web 聊天在回合中途重新加载时，用户提示词会从屏幕*和* `localStorage` 中丢失。已关闭。
- [#11484](https://github.com/zeroclaw-labs/zeroclaw/issues/11484) — ZeroCode 回合禁用了重复工具防护机制。已关闭。
- [#11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970) — `agents delete/rename` 在未启用 gateway feature 的构建中跳过了 owned-state 级联。已关闭。
- [#11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792)、[#11708](https://github.com/zeroclaw-labs/zeroclaw/issues/11708)、[#10400](https://github.com/zeroclaw-labs/zeroclaw/issues/10400)、[#11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486)、[#11987](https://github.com/zeroclaw-labs/zeroclaw/issues/11987)、[#12076](https://github.com/zeroclaw-labs/zeroclaw/issues/12076) — 插件孤立阶段清理、工具 schema 完整性、Telegram 未授权发送者提示、ZeroCode 配置 UX、Semgrep `nosemgrep` 处理，以及 v0.8.7 翻译修复。

**开放 PR 中推进的功能：**
- 会话持久化：[#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921)（仅追加的事件历史）。
- 插件 webhook RPC，处于依赖栈中：[#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) → [#11322](https://github.com/zeroclaw-labs/zeroclaw/pull/11322)。
- 安全/身份：[#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745)（按 agent 的知识作用域）、[#11230](https://github.com/zeroclaw-labs/zeroclaw/pull/11230)（对非管理员 principal 隐藏会话数据工具）、[#11301](https://github.com/zeroclaw-labs/zeroclaw/pull/11301)（已提交重命名恢复）、[#11313](https://github.com/zeroclaw-labs/zeroclaw/pull/11313)（将授权修改发布到运行中的守护进程）、[#11265](https://github.com/zeroclaw-labs/zeroclaw/pull/11265)（roster 密码生命周期 CLI）。
- ZeroCode TUI：[#11727](https://github.com/zeroclaw-labs/zeroclaw/pull/11727)、[#11501](https://github.com/zeroclaw-labs/zeroclaw/pull/11501)、[#11502](https://github.com/zeroclaw-labs/zeroclaw/pull/11502)、[#11505](https://github.com/zeroclaw-labs/zeroclaw/pull/11505)、[#11510](https://github.com/zeroclaw-labs/zeroclaw/pull/11510)、[#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622)、[#11731](https://github.com/zeroclaw-labs/zeroclaw/pull/11731)。

---

## 4. 社区热门话题

| 条目 | 评论数 | 为什么重要 |
|---|---|---|
| [#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) RFC：仅追加的会话事件 | 14 | 尽管已关闭，仍是今天讨论最多的条目。社区已就以下方向达成共识：用可重放的事件历史取代可变的消息持久化——这是一项触及运行时、存储和所有渠道的基础性变更。 |
| [#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982) 按发送者 RBAC | 11 | 运行时间最长、互动度最高的讨论串。范围已**收窄**为在现有 agent/risk-profile 模型上构建发送者角色，而不是新建 `[rbac.*]` 子系统——这是由社区反馈驱动的务实范围缩减。草稿工作见 #11068。 |
| [#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999) Discord 插件安装证明 | 9 | 当前为 `status:blocked`，但已在发布的 v0.8.6 归档包上验证 Discord 入站/出站投递。剩余缺口是打包/安装证明，而非运行时行为。 |
| [#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418) ZeroCode“Copy”按钮无反应 | 8 | S1 工作流阻塞，今天有更新，`status:in-progress`。TUI 中用户可见的严重烦扰。 |
| [#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416) 自 v0.8.5 起 Slack“is thinking…”状态丢失 | 6 | 回归报告；报告者明确询问“是否有意如此？”——这是来自 Slack 渠道用户的 UX 信号。 |
| [#10926](https://github.com/zeroclaw-labs/zeroclaw/issues/10926) Matrix `send_via` 错误路由到对等身份 | 5 | S2；根因已定位：`resolve_target` 将第一个 `external_peers` 条目用作房间接收者。 |
| [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) 知识图谱中没有按 agent 归属 | 5 | S0 安全。任何 agent 都能读取/修改另一个 agent 的知识。直接阻碍可信的多 agent 部署。 |
| [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921) 仅追加的会话事件（实现） | n/a | 此数据集中 PR 的评论数不可用，但这是当前推进中范围最大的 PR（60+ 个标签、`risk:high`、`size:XL`）。 |

**底层需求：** 社区正推动 ZeroClaw 从“单用户助手”走向**多租户、可审计的 agent 基础设施**——所有权边界、可重放状态和按 principal 授权。几乎每个热门讨论串都指向这个方向。

---

## 5. Bug 与稳定性

| 严重程度 | Issue | 影响 | 修复状态 |
|---|---|---|---|
| **S0 / p1** | [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) 知识图谱没有按 agent 归属 | 任何 agent 都能读取/修改另一个 agent 的知识——数据丢失/安全风险 | 修复 PR 已开放：[#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745)（`needs-maintainer-review`，自 2026-08-04 起开放） |
| **p1 安全** | [#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394) `gateway.pairing_dashboard` 被接受但完全未被读取；配对码永不过期 | 配置被静默忽略；过期配对码仍然有效 | **未关联修复 PR** |
| **S1** | [#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418) ZeroCode 一键 Copy 损坏 | TUI 中工作流受阻 | `in-progress`，未列出 PR |
| **S2** | [#10926](https://github.com/zeroclaw-labs/zeroclaw/issues/10926) Matrix `send_via` 将对等身份当作房间目的地 | 消息发送到错误目的地 | 已接受，无 PR |
| **S2** | [#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332) 渠道、webhook 或 gateway 回合从未运行技能审查/创建 | 对大多数非 CLI 用法，学习循环静默失效 | 已接受，无 PR |
| **S2** | [#11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336) `plugin info` / `list --verify` 对运行时拒绝注册的插件报告 `[loads]` | 插件校验误报 | 已接受，无 PR |
| **S2** | [#10294](https://github.com/zeroclaw-labs/zeroclaw/issues/10294) `file_write` 记录无法区分创建与覆盖 | 审计/可观测性缺口 | 开放，后续跟进状态 |
| **S3** | [#11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360) 包锁文件全局可读 → 其他本地账户可持有它并破坏安装/移除 | 仅影响可用性 | 已接受，无 PR |
| **S3** | [#11358](https://github.com/zeroclaw-labs/zeroclaw/issues/11358) `plugin migrate` 忽略包锁，可能合并到并发创建的目的地 | 竞态条件 | 已接受，无 PR |
| **回归** | [#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416) 自 v0.8.5 起 Slack“is thinking…”状态缺失 | UX 回归，报告者询问是否有意为之 | 已接受 |

**今日修复：** #11517（重新加载时提示词丢失）、#11432（卡住的 `running` 会话）、#11562（manifest/组件代际不匹配）、#11484（重复工具防护）、#11708（工具 schema 不完整）、#11970（无 gateway 构建中缺少级联）、#11792（孤立安装阶段）、#11987（Semgrep 抑制）。

**评估：** *报告出来的* bug 流是健康的——今天关闭了 8 个稳定性修复，pulldown-cmark 版本提升还移除了一个全仓库范围的 CI 阻塞。未解决风险的集中点在于**长期存在的安全问题**（#9394、#9647），而非新近回归。

---

## 6. 功能请求与路线图信号

- **面向多租户部署的按发送者 RBAC**——[#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)。范围已收窄并获接受；已有草稿。是 v0.9.0 或 v0.10 的有力候选。
- **统一文件接收/解析/投递**——[#11013](https://github.com/zeroclaw-labs/zeroclaw/issues/11013) 跟踪项，承接已接受的 RFC #9488。跨渠道文件处理是用户反复提出的需求。
- **公开运行时组合边界**——[#10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993)。涉及 agent 运行时的可嵌入性；这是生态/扩展层面的举措，而非终端用户功能。
- **插件安装重新播种语义**——[#10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162)。是安装持久性工作（#11098）的后续。
- **运维方成本上限覆盖**——[#11589](https://github.com/zeroclaw-labs/zeroclaw/pull/11589)。`cost.allow_override` 已有文档但无人读取；该 PR 使其生效。小改动、高价值，可能很快落地。
- **会话事件重放 + 派生流**——[#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) / [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921)。RFC 已关闭，实现已开放；这是头条架构事项。
- **ZeroCode TUI 可配置性**——按窗格会话数上限（[#11731](https://github.com/zeroclaw-labs/zeroclaw/pull/11731)）、transcript 时间戳（[#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622)）、多选编辑器（[#11502](https://github.com/zeroclaw-labs/zeroclaw/pull/11502)）。

**v0.9.0 预测：** 安全/身份工作（知识图谱作用域、非管理员工具隐藏、守护进程授权修改传播）、pulldown-cmark 安全公告修复，以及 Telegram 未授权通知变更。会话事件历史规模足够大，可能推迟到 v0.9.x 或 v0.10。

---

## 7. 用户反馈摘要

**真实用户表达的痛点：**
- *“这个‘Copy’按钮对我没用。它对我的剪贴板完全没有任何作用。”*——基础 TUI 功能损坏（S1），[#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418)。
- *“我同时启用了 `skill_improvement` 和 `skill_creation`，但学习循环只对 CLI/cron 风格的运行生效。”*——对 Matrix/webhook/web-UI 用户而言是功能静默降级，[#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)。
- *“我 agent 的所有技能都在一个 skill bundle 里……但技能审查 fork 看不到其中任何一个。”*——[#11333](https://github.com/zeroclaw-labs/zeroclaw/issues/11333)。
- *“在 agent 回合仍在运行时重新加载仪表盘页面，会让用户提示词从屏幕上**和** `localStorage` 中消失。”*——正常使用中的数据丢失感知，[#11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517)（现已关闭）。
- *“守护进程死亡时正处于进行中回合的会话，会永久停留在 `state = 'running'`。”*——影响运维方对会话状态的信任，[#11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432)（现已关闭）。
- *“自 v0.8.5 起……唯一的生命迹象就是入站确认表情反应（👀）。”*——Slack 用户失去了进度反馈，[#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)。

**满意信号：** 对范围明确的 bug 响应迅速（多个 10 月初提交的 S2/S3 issue 今天关闭）；维护者在讨论串中主动收窄范围，而不是让提案停滞；ZeroCode UX 工作由一位资深贡献者以持续、聚焦的 PR 流交付。

**不满意信号：** Slack 反馈回归；本地模型用户（Ollama `qwen3.5:9b`、MLX-LM）反复报告工具循环损坏；以及“被接受却完全未被读取”的配置（#9394、#11589）——这类 bug 会侵蚀对配置 schema 的信任。

---

## 8. 积压事项观察

| 条目 | 存在时长 | 关注点 |
|---|---|---|
| [#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394) 配对仪表盘被忽略 + 配对码不过期 | 自 **2026-07-26** 起开放（约 14 周） | `priority:p1`、`security:pairing`，标记为 `release:v0.9.0`，**未关联修复 PR**。未解决风险最高且实现零进展的条目。 |
| [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) 知识图谱跨 agent 访问 | 自 **2026-08-01** 起开放 | S0 严重程度。修复 PR [#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745) 自 **2026-08-04** 起一直停留在 `needs-maintainer-review`——将近三个月。需要分配评审资源，而不是更多作者工作。 |
| [#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982) 按发送者 RBAC | 自 **2026-04-22** 起开放（约 27 周） | 已接受且范围收窄，有 11 条评论，但收窄后的设计没有 PR。数据集中最古老的高互动功能请求。 |
| [#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999) Discord 插件安装证明 | 自 2026-09-20 起开放，`status:blocked` | 因 9 条评论而阻塞；运行时已验证，打包证明仍待补齐。需要决定验收标准是否改变。 |
| [#10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162) 插件安装播种重试 | 自 2026-08-20 起开放，`in-progress` | 部分修复已通过 #11098 落地；已安装场景仍缺少一致认可的答案。 |
| [#11013](https://github.com/zeroclaw-labs/zeroclaw/issues/11013) 统一文件接收跟踪项 | 自 2026-09-20 起开放 | 实现批次跟踪项仅有 3 条评论——对一个横切工作流而言可见度偏低。 |
| [#11301](https://github.com/zeroclaw-labs/zeroclaw/pull/11301) / [#11313](https://github.com/zeroclaw-labs/zeroclaw/pull/11313) | 自 2026-10-01 起开放 | 两者均为 `priority:p1`、`risk:high`、`size:XL`，等待维护者评审——属于没有推进的 P1 安全队列。 |
| [#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) / [#11322](https://github.com/zeroclaw-labs/zeroclaw/pull/11322) | 自 2026-10-01 起开放 | 深层依赖栈（`#11319` → `#11339` → `#11165` → …）；每一层都重新堆叠。堆叠 PR 链是结构性评审瓶颈。 |
| [#11265](https://github.com/zeroclaw-labs/zeroclaw/pull/11265) 用户密码生命周期 CLI | 自 2026-09-30 起开放，`do-not-merge` | 明确标记为 do-not-merge，并堆叠在两个未合并 PR（#11264、#11313）之上——在基础 PR 落地前不会推进。 |
| [#10294](https://github.com/zeroclaw-labs/zeroclaw/issues/10294) `file_write` 创建/覆盖歧义 | 自 2026-08-24 起开放 | 对 transcript 消费方而言是 observability/审计缺口；优先级低，但会阻碍可靠的文件效果审计。 |

---

### 数据说明
- 仅展示按评论数排序的 50 个 issue 中的前 30 个，以及 50 个 PR 中的前 20 个；PR 评论数返回为 `undefined`，因此 PR 互动度是根据标签和范围推断，而非实测讨论量。
- 在报告的 6 个已合并/关闭 PR 中，仅 #12095 在提供的样本中可见。
- 标签计数和评论总数为 2026-10-30 的时点快照。

</details>

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*