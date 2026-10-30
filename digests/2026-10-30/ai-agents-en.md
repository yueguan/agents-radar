# OpenClaw Ecosystem Digest 2026-10-30

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-30 02:07 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-30

## 1. Today's Overview

OpenClaw remains a high-velocity project: 500 issues and 500 PRs were updated in the last 24 hours, with 174 issues closed and 192 PRs merged or closed, indicating strong triage and merge throughput. Two releases shipped: a security-focused stable patch, **v2026.10.9**, and a gateway-only **v2026.9.36** extended-stable/LTS release. The active backlog is still large—326 open issues and 308 open PRs among updated items—and is dominated by P0/P1 reliability work around gateway state databases, session lifecycle integrity, MCP runtime management, and the update/recovery path. Overall project health is good on responsiveness and release cadence, but risk is concentrated in state/session durability and several long-running P0/P1 issues. Note: the dataset shows the top 50 issues by comment count and top 30 PRs; PR comment counts were unavailable.

## 2. Releases

### v2026.10.9 — stable security patch
[Release v2026.10.9](https://github.com/openclaw/openclaw/releases/tag/v2026.10.9)

Key changes called out in the release notes:
- **fast-uri 4.2.3**: rejects malformed URI schemes and hosts during MCP schema validation.
- **Zod collection limits**: oversized collections are rejected early in A2UI and the Discord embedded app SDK.
- **IMAP sender parsing**: now uses nodemailer 10.1.4 so spoofed senders are handled correctly.

**Breaking changes / migration notes:** None are stated in the provided release excerpt. This is a dependency-hardening patch; standard upgrade and gateway restart apply. Teams using MCP schema validation, A2UI, the Discord embedded app SDK, or IMAP ingestion should prioritize this release.

**Release validation caveat:** Flaky tests observed during 2026.10.9 validation are tracked in [#187490](https://github.com/openclaw/openclaw/issues/187490). The release classified them as flakes; a parallel fix on `main` is required by policy.

### v2026.9.36 — gateway-only extended-stable (LTS equivalent)
[Release v2026.9.36](https://github.com/openclaw/openclaw/releases/tag/v2026.9.36)

- Gateway-only `extended-stable` release, described as the project’s current LTS equivalent.
- Based on OpenClaw from the end of September 2026, plus critical security updates, reliability/performance fixes, and new model support.
- Best suited to conservative deployments that need a supported, security-maintained baseline rather than the newest stable features.

## 3. Project Progress

The 192 merged/closed PRs and 174 closed issues indicate meaningful cleanup, though the dataset does not separate merged from closed-only PRs.

**Notable closed issues in the active top-50:**
- [#178000](https://github.com/openclaw/openclaw/issues/178000) — Session-state races were misclassified as `reason=unknown`, triggering cross-provider model fallback. Closed after being rated diamond lobster / P1.
- [#186813](https://github.com/openclaw/openclaw/issues/186813) — In-chat managed update (`gateway.update.run`) hung indefinitely in `epoll_wait` on SQLite lock contention. P0, closed.
- [#156635](https://github.com/openclaw/openclaw/issues/156635) — Copilot context budget stayed at synthetic 128k fallback after account discovery. Closed.
- [#187468](https://github.com/openclaw/openclaw/issues/187468) — Update failure report for 2026.9.8. Closed.

**PRs advancing key areas:**
- **Update/recovery hardening:** [#188034](https://github.com/openclaw/openclaw/pull/188034) retains authority through fallback publication; [#188021](https://github.com/openclaw/openclaw/pull/188021) excludes resolved captures from recovery ambiguity; [#188024](https://github.com/openclaw/openclaw/pull/188024) reports failed scheduled registry observations; [#188027](https://github.com/openclaw/openclaw/pull/188027) reads named recovery via primary key; [#188010](https://github.com/openclaw/openclaw/pull/188010) retries installation discovery after returned failures (closed).
- **Gateway responsiveness/performance:** [#188033](https://github.com/openclaw/openclaw/pull/188033) keeps the Gateway responsive during plugin tool discovery; [#188030](https://github.com/openclaw/openclaw/pull/188030) stops doctor re-reading SQLite inspection copies; [#188025](https://github.com/openclaw/openclaw/pull/188025) stops GitHub readiness from copying whole agent databases (P1, security-sensitive).
- **Web/UI correctness:** [#188013](https://github.com/openclaw/openclaw/pull/188013) preserves authored CSP in relative HTML previews (security-boundary risk flagged); [#188017](https://github.com/openclaw/openclaw/pull/188017) fixes duplicate dividers in the session Advanced menu (closed); [#188039](https://github.com/openclaw/openclaw/pull/188039) adds canonical Unicode matching to workspace rail file search.
- **Channels/integrations:** [#188003](https://github.com/openclaw/openclaw/pull/188003) matches canonically equivalent names in Teams live directory search; [#188018](https://github.com/openclaw/openclaw/pull/188018) fixes Matrix image replies in progress mode.
- **Memory:** [#186649](https://github.com/openclaw/openclaw/pull/186649) opts topic files into per-entry chunking with a marker.
- **Developer experience:** [#187962](https://github.com/openclaw/openclaw/pull/187962) adds `OPENCLAW_DEV_RELOAD_BUNDLED_PLUGINS` for bundled plugin code reload in development.

## 4. Community Hot Topics

The most-commented issues reveal sustained user focus on state durability, memory promotion, and MCP tool reliability.

1. **[#164294](https://github.com/openclaw/openclaw/issues/164294) — Gateway state DB WAL auto-checkpoint never completes (38 comments, P2).**  
   The gateway’s own WAL checkpoint on `state/openclaw.sqlite` is permanently blocked by an in-process reader; `consecutiveBlocked` grows and checkpointing never completes. Underlying need: predictable SQLite maintenance and bounded WAL growth under real gateway load.

2. **[#150635](https://github.com/openclaw/openclaw/issues/150635) — Short-term recall retention evicts recalled entries nightly, so dreaming deep phase never promotes (23 comments, P2).**  
   At the 512-entry cap, nightly ingestion stages zero-recall entries and the deep phase never promotes. Underlying need: coherent memory lifecycle semantics between short-term recall capacity, dreaming, and promotion.

3. **[#120385](https://github.com/openclaw/openclaw/issues/120385) — Code Mode tool catalog incomplete for scheduled/cron and memory-flush turns; MCP tools missing (22 comments, P2).**  
   Scheduled jobs fail with “Unknown tool id,” surfaced only as a generic exec failure. Underlying need: tool-catalog parity across interactive, cron, and memory-flush execution paths.

4. **[#164923](https://github.com/openclaw/openclaw/issues/164923) — One workspace promotes 0/512 while sibling workspaces promote 50–90% (19 comments, P2).**  
   `openclaw memory promote` returns no candidates even with thresholds overridden to zero. Underlying need: deterministic, inspectable filtering in multi-workspace memory promotion.

5. **[#172837](https://github.com/openclaw/openclaw/issues/172837) — Codex `nativeSubagentAssignments` never removed after completion delivery gives up (18 comments, P1).**  
   Every new parent thread re-pins and re-sends stale assignments. Underlying need: lifecycle cleanup and retry-exhaustion semantics for native subagents.

**PR-side activity:** Comment counts were unavailable in the dataset, but several PRs are marked “ready for maintainer look,” including [#188003](https://github.com/openclaw/openclaw/pull/188003), [#188021](https://github.com/openclaw/openclaw/pull/188021), [#187986](https://github.com/openclaw/openclaw/pull/187986), and [#184511](https://github.com/openclaw/openclaw/pull/184511). The long-running XL PR [#181813](https://github.com/openclaw/openclaw/pull/181813) (gateway native state owner identity) has been open since 2026-10-23 and is awaiting maintainer review.

## 5. Bugs & Stability

Active issues updated in the last 24 hours, ranked by severity. Fix PRs are noted where identifiable.

### P0 — release-blocking / severe availability
- **[#166634](https://github.com/openclaw/openclaw/issues/166634)** — Late reclamation commit checkpoint failure leaves an agent permanently unavailable after crash recovery; agent refuses `chat.history`, `chat.metadata`, `models.list`, and progress-card requests. 18 comments. No linked fix PR shown.
- **[#162211](https://github.com/openclaw/openclaw/issues/162211)** — Gateway startup blocks the event loop for 40–200s; health monitor misreads the stall as a disconnect and escalates into a restart loop. 14 comments. No linked fix PR shown.
- **[#158592](https://github.com/openclaw/openclaw/issues/158592)** — After macOS sleep/wake, prepared model runtime publication for one agent times out and never recovers; every inbound message fails with “runtime owner was not published” until gateway restart. 11 comments. No linked fix PR shown.
- **[#141413](https://github.com/openclaw/openclaw/issues/141413)** — macOS app omits token for local Gateway using a protected SecretRef; embedded dashboard can authenticate but external connection does not. P0, security review needed, 7 comments, 2 👍.
- **[#186813](https://github.com/openclaw/openclaw/issues/186813)** — Managed update hangs in `epoll_wait` on SQLite transaction lock contention. **Closed**; likely addressed, but no specific merge PR is listed in the provided data.

### P1 — serious functional/crash-loop regressions
- **[#172837](https://github.com/openclaw/openclaw/issues/172837)** — Codex native subagent assignments leak after delivery gives up; re-pinned on every new parent thread. 18 comments. No fix PR shown.
- **[#177656](https://github.com/openclaw/openclaw/issues/177656)** — Native host exec returns “Direct shared-state reader requires current integrity admission” without a child result; at least 28 failures on stable 2026.10.5. 17 comments. No fix PR shown.
- **[#166017](https://github.com/openclaw/openclaw/issues/166017)** — `mcp.sessionIdleTtlMs` default (10 min) is not applied when unset; MCP runtimes are never evicted, spawn broker reaches its 256-child hard cap, and all exec/read/MCP starts fail. 13 comments. Linked fix PR open.
- **[#159094](https://github.com/openclaw/openclaw/issues/159094)** — Gateway owns the state-lifecycle lease but internal workers report another process owns it, closing agent DB execution admission. 14 comments.
- **[#126950](https://github.com/openclaw/openclaw/issues/126950)** — Telegram opaque callback payloads (`tgcb1:`) silently dropped for non-plugin callers. 11 comments, 1 👍. Linked fix PR open.
- **[#163566](https://github.com/openclaw/openclaw/issues/163566)** — Durable context-engine turns stuck as `session-rebound` after update repair; `session-resource-loader` burns ~100s CPU per turn. 10 comments.
- **[#167876](https://github.com/openclaw/openclaw/issues/167876)** — Talk consultations lose caller authority after session setup completes; security review needed. 9 comments, 1 👍.
- **[#158070](https://github.com/openclaw/openclaw/issues/158070)** — Preflight compaction fails with “agent tool caller authority is no longer active” while the run waits in a lane queue, voiding in-flight delivery. 9 comments.
- **[#148789](https://github.com/openclaw/openclaw/issues/148789)** — Model fallback chain exhausts every candidate on a harness-level session failure and misattributes it to the last fallback model. 9 comments.
- **[#178359](https://github.com/openclaw/openclaw/issues/178359)** — `doctor --fix --non-interactive` idles 15+ minutes in plugin-capture stage and never reaches agent DB repair. 8 comments. Related performance PR [#188030](https://github.com/openclaw/openclaw/pull/188030).
- **[#155396](https://github.com/openclaw/openclaw/issues/155396)** — Ordinary Codex tool turns fail closed on `provenance_rejected` during final-answer recovery. 8 comments.
- **[#160762](https://github.com/openclaw/openclaw/issues/160762)** — claude-cli + Telegram: a message sent mid-turn is dropped; drained prompt carries the previous message’s body. 7 comments.

### P2 — notable friction/correctness
- **[#164394](https://github.com/openclaw/openclaw/issues/164394)** — Control UI WebChat transcript jitters continuously when scrolling into history. 13 comments.
- **[#177094](https://github.com/openclaw/openclaw/issues/177094)** — System-notice delivery mirror displaces the turn’s last answer into collapsed tool steps after restart/reload. 9 comments.
- **[#172969](https://github.com/openclaw/openclaw/issues/172969)** — `registerCliBackend` without `modelProvider` is silently dropped; surfaces as “Invalid URL: must be http or https.” 8 comments.
- **[#176342](https://github.com/openclaw/openclaw/issues/176342)** — Stale-lease integrity gates serialize agent startup after unclean shutdown, causing minutes-long unavailability. 7 comments.
- **[#186973](https://github.com/openclaw/openclaw/issues/186973)** — `/clear` state check renders a stale update-history banner weeks after the run, across gateway restart and reboot. 7 comments.
- **[#165433](https://github.com/openclaw/openclaw/issues/165433)** — `chat.send` rejects reconnect envelopes with `__controlUiReconnectResume` when the client is not an Operator UI client. 7 comments.
- **[#164690](https://github.com/openclaw/openclaw/issues/164690)** — claude-cli runs spawn every stdio MCP server twice; each Gateway stdio child carries ~144 MB supervisor wrapper overhead, not configurable. 7 comments.
- **[#167667](https://github.com/openclaw/openclaw/issues/167667)** — ChatGPT Responses tool continuation fails with “Rustponses cannot replay,” surfaced as a generic agent-run error. P1, 6 comments, 1 👍.

**Stability assessment:** The highest-risk cluster is gateway/session state integrity and crash recovery. Several P0/P1 issues have been open for weeks without linked fix PRs, while update-path and doctor-performance fixes are actively in review. The 2026.10.9 security patch is welcome, but it does not address the core state-lifecycle failures.

## 6. Feature Requests & Roadmap Signals

Active user-requested features, ranked by likely relevance:

- **[#6757](https://github.com/openclaw/openclaw/issues/6757) — Agent-triggered context compaction (self-compact tool).** P2, 9 comments, 2 👍, open since February 2026. High strategic fit with the project’s memory/context work; a strong candidate for roadmap attention.
- **[#172547](https://github.com/openclaw/openclaw/issues/172547) — Exclude fenced dreaming ranges from memory-search indexing and recall signal.** 7 comments. Directly connected to the hot memory-promotion issues; likely to be folded into memory-core fixes.
- **[#112638](https://github.com/openclaw/openclaw/issues/112638) — `session.maintenance enforce` mode does not bound the store; thread/channel entries are exempt from reclaim.** 8 comments, open since July. Strong reliability/operability signal.
- **[#163739](https://github.com/openclaw/openclaw/issues/163739) — Make Control UI Ctrl+Shift+A (Archive) configurable; it collides with browser shortcuts.** 7 comments, 1 👍. Small, user-facing UX fix.
- **[#140244](https://github.com/openclaw/openclaw/issues/140244) — Support `--prompt-file` in `openclaw infer model run`.** 7 comments, 1 👍. Automation-oriented CLI improvement.
- **[#154524](https://github.com/openclaw/openclaw/issues/154524) — Expose chat messages as headings for screen-reader navigation.** 6 comments. Accessibility gap.
- **[#147732](https://github.com/openclaw/openclaw/issues/147732) — Paragraph-level RTL direction for mixed Persian/Arabic/Hebrew + English messages.** 6 comments. Internationalization/UI correctness.
- **[#150743](https://github.com/openclaw/openclaw/issues/150743) — QQ channel sustainability: Tencent handoff stalled; clarify long-term support.** 7 comments. Ecosystem/support-model concern, not a code feature.

**Prediction for next version:** The strongest near-term signals are memory/recall correctness (multiple high-comment issues plus PR [#186649](https://github.com/openclaw/openclaw/pull/186649)), update/recovery hardening (already a cluster of PRs), MCP runtime lifecycle defaults, and Control UI accessibility/localization fixes. Agent-triggered compaction is the highest-value larger feature but has been pending since February and may need a product decision.

## 7. User Feedback Summary

**Recurring pain points:**
- **State and session durability:** Users report agents becoming permanently unavailable after crash recovery, WAL checkpoints never completing, stale leases serializing startup, and session-state races triggering cross-provider fallback. These are the most damaging reported experiences because they require gateway restarts or leave agents unusable.
- **Memory/dreaming inconsistency:** Multiple reports describe promotion pipelines that silently produce zero candidates, evict recalled entries nightly, or index dreaming output that can never be promoted. Users are clearly relying on memory as a core workflow, not a peripheral feature.
- **Update and recovery reliability:** In-chat updates hanging on SQLite locks, `doctor --fix` idling, failed registry lookups recorded as successes, and package-swap failures. This erodes trust in the upgrade path, especially for stable/LTS users.
- **MCP/resource management:** MCP runtimes not evicted at default TTL, spawn broker hitting 256 children, duplicate stdio servers under claude-cli, and ~144 MB wrapper overhead per child. Operators on multi-agent or Docker-sandbox setups feel this acutely.
- **UI/UX friction:** WebChat transcript jitter, stale banners, system notices displacing final answers, shortcut collisions, RTL rendering, and screen-reader navigation gaps.
- **Platform-specific issues:** macOS Homebrew Node upgrades breaking bare `openclaw` commands; Android repeatedly querying unknown agent `openclaw`; QQ channel support uncertainty.

**Satisfaction signals:** Strong triage volume, rapid closure of several P0/P1 issues, a clear LTS-equivalent release, and a security patch cadence demonstrate responsive maintenance. The “ready for maintainer look” labels and structured rating system suggest a healthy review process.

**Dissatisfaction signals:** Several P0/P1 issues remain open for weeks without linked fix PRs, and many high-comment issues carry `needs-product-decision`, `needs-maintainer-review`, or `stale` labels. Users are investing significant effort in detailed reproductions; delayed decisions on those reports risk burnout and workarounds.

## 8. Backlog Watch

Important issues/PRs that are old, high-impact, or waiting on maintainer/product action:

- **[#6757](https://github.com/openclaw/openclaw/issues/6757)** — Agent-triggered context compaction. Open since **2026-02-02**, 9 comments, 2 👍, needs product decision. Longest-running high-value feature request in the visible set.
- **[#112638](https://github.com/openclaw/openclaw/issues/112638)** — `session.maintenance enforce` does not bound the store. Open since **2026-07-22**, 8 comments. Operationally important for long-running deployments.
- **[#120385](https://github.com/openclaw/openclaw/issues/120385)** — Code Mode tool catalog incomplete for cron/memory-flush turns. Open since **2026-08-08**, 22 comments. High community engagement; affects scheduled automation reliability.
- **[#126950](https://github.com/openclaw/openclaw/issues/126950)** — Telegram opaque callback payloads dropped. Open since **2026-08-21**, 11 comments, 1 👍, linked fix PR open. Channel integration blocker.
- **[#141413](https://github.com/openclaw/openclaw/issues/141413)** — macOS app omits token for local Gateway using SecretRef. Open since **2026-09-07**, P0, 7 comments, 2 👍, needs security review. High severity with relatively low comment volume—may be under-triaged.
- **[#150635](https://github.com/openclaw/openclaw/issues/150635)** — Dreaming deep phase never promotes due to recall eviction. Open since **2026-09-17**, 23 comments, diamond-lobster rating, needs product decision. High user impact and high engagement.
- **[#158592](https://github.com/openclaw/openclaw/issues/158592)** — Sleep/wake runtime publication timeout. Open since **2026-09-26**, P0, 11 comments, needs live repro. Severe but reproduction-dependent.
- **[#162211](https://github.com/openclaw/openclaw/issues/162211)** — Startup event-loop block → restart loop. Open since **2026-09-30**, P0, 14 comments, needs maintainer review. Release-blocker severity.
- **[#166634](https://github.com/openclaw/openclaw/issues/166634)** — Late reclamation checkpoint failure leaves agent unavailable. Open since **2026-10-07**, P0, 18 comments. One of the highest-severity open issues.
- **[PR #154033](https://github.com/openclaw/openclaw/pull/154033)** — Codex native tool-output images from raw response items. Open since **2026-09-20**, needs proof, security-boundary and message-delivery risk. Long-lived PR with protocol-level implications.
- **[PR #181813](https://github.com/openclaw/openclaw/pull/181813)** — Expose held native state owner identity. Open since **2026-10-23**, XL, P2, ready for maintainer look. Relevant to the state-owner/lease confusion cluster.
- **[PR #184511](https://github.com/openclaw/openclaw/pull/184511)** — Commit invitation membership with involvement. Open since **2026-10-26**, P2, ready for maintainer look, proof sufficient. Authorization/session correctness.
- **[#150743](https://github.com/openclaw/openclaw/issues/150743)** — QQ channel sustainability and Tencent handoff. 7 comments, open since **2026-09-17**. Needs an ecosystem/support decision rather than a code fix.

**Bottom line:** OpenClaw is shipping quickly and maintaining a credible LTS/security cadence, but its most valuable users are hitting state/session durability and memory-promotion failures that remain unresolved for weeks. The next release cycle should be judged on whether the P0 state-lifecycle cluster (#166634, #162211, #158592) and the memory-promotion cluster (#150635, #164923, #172547) receive concrete fixes rather than further triage.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent Open-Source Ecosystem
**Snapshot date: 2026-10-30 | Projects: OpenClaw, Hermes Agent, IronClaw, QwenPaw, ZeroClaw**

---

## 1. Ecosystem Overview

The personal AI assistant / agent OSS landscape is consolidating around a common architectural stack — a gateway process owning session/state, an MCP tool layer, a memory subsystem, and a multi-channel delivery surface — while vendors differentiate on deployment model (single-user vs multi-tenant), language/runtime, and channel reach. Activity is deeply uneven: OpenClaw operates at an order of magnitude above peers (500 issues + 500 PRs touched daily), Hermes/ZeroClaw/QwenPaw cluster at a focused 40–55 items/day, and IronClaw is effectively dormant this window. Across every active project, the dominant engineering theme has shifted from *capability* to *durability and trust*: crash-safe state, honest failure reporting, memory provenance, and identity scoping now outrank feature shipping in community engagement. Release cadence is bifurcating — OpenClaw is the only project maintaining both a fast stable channel and an LTS-equivalent, which is becoming a meaningful competitive moat for conservative adopters.

---

## 2. Activity Comparison (last 24h)

| Project | Issues updated (open/closed) | PRs updated (open/merged-closed) | Release status | Health score* |
|---|---|---|---|---|
| **OpenClaw** | ~500 (174 closed) | ~500 (192 merged/closed) | **2 releases**: v2026.10.9 stable security patch; v2026.9.36 gateway-only LTS | **8.5 / 10** |
| **Hermes Agent** | 50 (48 open / 2 closed) | 50 (10 merged/closed, 40 open) | None | **7.5 / 10** |
| **IronClaw** | 0 | 0 (2 open Dependabot) | None | **4.5 / 10** |
| **QwenPaw** | 15 (10 closed / 5 open) | 40 (13 merged/closed, 27 open) | None (pre-2.3.0 beta stabilization) | **7.5 / 10** |
| **ZeroClaw** | 50 (17 closed / 33 open) | 50 (6 merged/closed, 44 open) | None (between v0.8.7 and v0.9.0) | **7.0 / 10** |

\*Health score is a derived composite of triage/merge throughput, release cadence, open-backlog severity, and unresolved P0/P1 risk. It is directional, not a maintainer metric. OpenClaw scores highest on throughput and release discipline but carries concentrated P0 state-durability risk; QwenPaw/Hermes score similarly from opposite directions (velocity vs. backlog clearing). IronClaw's score reflects low activity, not instability.

**Reading the table:** OpenClaw's raw throughput is ~10× the next-largest project. QwenPaw is the most **PR-heavy relative to issues** (40:15), indicating implementation outpacing intake — a review-bandwidth signature. ZeroClaw shows the widest **intake-to-merge gap** (44 open vs. 6 closed PRs), its principal structural risk. Hermes is the only project showing **backlog-clearing behavior** (months-old PRs merged this window).

---

## 3. OpenClaw's Position

**Advantages vs. peers**
- **Community scale:** ~500 issues + 500 PRs/day vs. 50/50 (Hermes, ZeroClaw) and 15/40 (QwenPaw). The contributor and triage base is an order of magnitude larger, which is why it can sustain 174 issue closures and 192 PR dispositions in a single day.
- **Dual release channel:** The only project shipping both a stable security patch (v2026.10.9) and a gateway-only LTS-equivalent (v2026.9.36 with backported security + reliability fixes). This is a concrete differentiator for conservative/enterprise adopters.
- **Process maturity:** Structured severity ratings (P0–P2, "diamond lobster"), explicit "ready for maintainer look" states, and a release-validation caveat policy (flaky tests tracked to a parallel `main` fix) indicate a governance layer peers lack.
- **Integration surface breadth:** Channels (Telegram, Matrix, Teams, Discord, QQ, IMAP), Web/UI, Code Mode, A2UI, and an MCP runtime — the widest in the set.

**Technical approach differences**
- **Gateway-centric, SQLite-backed state:** Session lifecycle, state-lifecycle leases, WAL checkpoints, and crash recovery are first-class gateway concerns. This is the source of both its strongest guarantee ambitions and its highest-severity open bugs (#166634, #162211, #158592).
- **Memory/dreaming as a subsystem:** Short-term recall caps, nightly "deep phase" promotion, and dreaming-range indexing are unique to OpenClaw among this cohort and drive its top high-comment issues (#150635, #164923).
- **MCP as a runtime with lifecycle management:** Session idle TTLs, spawn-broker child caps, and runtime eviction are treated as operational concerns (#166017) rather than just protocol plumbing.

**Community size comparison:** OpenClaw is the reference implementation and the gravity well of the ecosystem; QwenPaw and ZeroClaw each run at roughly 10% of its daily item volume, and IronClaw is currently inactive. Hermes is the closest analogue in scope but at ~10% of OpenClaw's throughput and without a release channel.

---

## 4. Shared Technical Focus Areas

The same requirement families recur across independent codebases — a strong signal that these are ecosystem-level, not project-local, problems.

| Requirement | Projects voicing it | Specific needs |
|---|---|---|
| **Session/state durability & crash recovery** | OpenClaw (#166634, #162211, #158592), Hermes (#146643, #139210), ZeroClaw (#10526 RFC, #11432) | Crash-safe state machines, bounded WAL growth, restart-safe session adoption, append-only replayable history. ZeroClaw is the only project that has *accepted an RFC* for append-only event history (#10526 → impl #11921). |
| **Memory/context correctness & provenance** | OpenClaw (#150635, #164923, #172547), Hermes (#143379 doc-chain RFC), QwenPaw (#8500, #8457, #7835), ZeroClaw (#9647 knowledge-graph attribution) | Coherent recall→promotion lifecycle, provenance-preserving compaction, no silent payload leakage into channels, per-agent knowledge isolation. |
| **Silent failure / honest status reporting** | QwenPaw (#8500 "success=True on dropped data", #8150, #8488), ZeroClaw (#11332, #11336 false-positive plugin validation), OpenClaw (registry lookups recorded as successes), Hermes (#124874 mislabeled provider failure) | Distinguish "did nothing" from "no data"; fail loudly; never checkpoint a lossy operation as successful. QwenPaw's three independent reports make this the clearest cross-project theme. |
| **Multi-tenant identity, RBAC & attribution** | ZeroClaw (#5982 per-sender RBAC, #9647 S0 cross-agent knowledge), Hermes (#35147 stable `user_id`, profile-scope mismatches), OpenClaw (operator authority, Talk authority loss), QwenPaw (Hub admin password reset #7740) | Per-principal authorization, stable identity mappings, scope isolation, admin ops parity. |
| **MCP / plugin lifecycle integrity** | OpenClaw (#166017 idle-TTL eviction, spawn-broker cap), QwenPaw (#8473 plugin loader regression, #8499 provider decomposition), ZeroClaw (#11562 manifest/component generation mismatch, #11336), Hermes (#142235 read-only MCP tool call) | Deterministic plugin registration, generation-consistency, runtime eviction defaults, read-only/gated access for dashboards. |
| **Cron/scheduled job durability** | Hermes (#135487 SQLite lock, #146428 silent wedge, #145596 paused-job reactivation), OpenClaw (#120385 tool catalog missing for cron/memory-flush), ZeroClaw | Restart-safe occurrence handling, retry semantics, tool-catalog parity across execution paths, visible run history. |
| **Context compaction correctness** | OpenClaw (#6757 self-compact, #8464-adjacent), QwenPaw (#8464 `/compact` ignores session model, #6189 `save_decision()`), Hermes (#139210 compaction duplicates messages) | Session-scoped model routing, decision persistence across compaction, no duplicate-row side effects. |
| **Config schema trust ("accepted but unread")** | ZeroClaw (#9394 pairing dashboard never read, #11589 cost override no-op), OpenClaw (#172969 silent CLI-backend drop), Hermes (#90040 Managed Scope ignored) | Config keys must be wired or rejected; silent no-ops erode schema trust. |
| **Agent loop / cost control** | Hermes (#137170 no circuit breaker, 430 duplicate calls), QwenPaw (#6116 doom loop, closed **wontfix**), ZeroClaw (#11484 repetitive-tool safeguards disabled) | Repeat-success detection (not just repeat-failure), token budgets, call-fingerprint dedupe. QwenPaw's `wontfix` is a notable divergence. |
| **Install/update footprint** | Hermes (#139500 ~22,600 `.pyc` / >303 MiB, #143577 1 GB uv cache), QwenPaw (#8488 desktop packaging regression), OpenClaw (update/recovery hardening cluster) | Prunable caches, crash-safe update tails with TTL self-heal, packaging asset completeness. |

---

## 5. Differentiation Analysis

**Feature focus & target user**
- **OpenClaw** — Broadest personal-assistant platform: memory/dreaming, multi-channel, Web/UI, Code Mode, A2UI. Targets power users and self-hosters who want a full-stack assistant and are willing to operate a gateway. Only project with an explicit conservative-deployment (LTS) tier.
- **Hermes Agent** — Workflow automation: Kanban-driven execution, cron delivery guarantees, multi-profile/team setups, and a **strongly demanded desktop frontend-only install (#38519, 31👍)** for split-machine use. Consumer/productivity and small-team automation.
- **IronClaw** — Infrastructure/latency harness (wasmtime-based, `/harness/latency/runner`). Currently maintenance-only; no user-facing signal this window.
- **QwenPaw** — AgentScope-derived, Chinese-market-first, channel-heavy (Feishu, QQ, WeChat, WeCom, DingTalk, OneBot, Yuanbao). Desktop (Windows, zh-CN) and Hub multi-tenant mode are first-class. In **2.3.0-beta stabilization**, with a plugin/provider decomposition underway.
- **ZeroClaw** — Rust-based, explicitly driving toward **multi-tenant, auditable agent infrastructure**: per-sender RBAC, per-agent knowledge scoping, append-only replayable sessions, ZeroCode TUI. The most security/governance-forward roadmap in the set.

**Technical architecture differences**
- **Language/runtime:** ZeroClaw (Rust, wasmtime), IronClaw (wasmtime harness), QwenPaw (AgentScope/Python, PyInstaller desktop), Hermes (Python gateway), OpenClaw (gateway + SQLite state DB + MCP runtime).
- **State model:** OpenClaw and Hermes are mutable-SQLite-with-lease models (source of their lock-contention and stale-lease bugs). ZeroClaw is the only project **replacing** mutable persistence with an append-only event log (#11921), which structurally removes several of the failure classes OpenClaw is still patching.
- **Tenancy:** OpenClaw/Hermes/QwenPaw started single-user and are retrofitting multi-profile/multi-tenant; ZeroClaw is designing for multi-tenant from the start (RBAC, knowledge scoping, principal withholding).
- **Distribution:** OpenClaw ships dual channels; QwenPaw ships PyInstaller desktop betas; Hermes has no release and a loud install-hygiene complaint; IronClaw ships nothing this window.

---

## 6. Community Momentum & Maturity

**Activity tiers**
- **Tier 1 — Hyperactive:** OpenClaw (~1,000 items touched/day, 2 releases, sustained triage). This is a project operating a release train, not a sprint.
- **Tier 2 — High, focused:** Hermes, ZeroClaw (50/50 each), QwenPaw (55 items, PR-skewed). All three are iterating hard on a defined theme (reliability / security-infra / beta stabilization) without a release this window.
- **Tier 3 — Dormant:** IronClaw (0 issues, 2 Dependabot PRs). Insufficient data to distinguish stability from disengagement.

**Who is rapidly iterating vs. stabilizing**
- **Rapidly iterating:** QwenPaw (2.3.0-beta regressions actively patched: #8473 closed same-day via #8497), OpenClaw (weekly-cadence fixes + security patches), ZeroClaw (v0.9.0 milestone accumulating; 17 issue closures today).
- **Stabilizing / clearing backlog:** Hermes merged **months-old PRs** this window (#73923, #63812, #5490-class cleanup), signaling a maintenance pass rather than new feature work. IronClaw is flat.
- **Review-bandwidth constrained:** QwenPaw's oldest PRs are **40–53 days** old and are `XL/XXL/XXXL` (#7613 at 53 days, #7874 at 41 days); ZeroClaw has **44 open PRs vs. 6 closed** with stacked dependency chains (#11320→#11322→…) and a P1 security PR (#9745) awaiting review since **2026-08-04**; OpenClaw has long-lived XL PRs (#181813 open since 10-23) awaiting maintainer look. The common failure mode is **large, stacked, high-risk PRs starving the review queue**, not lack of contributors.

**Maturity signals ranked:** OpenClaw (highest: dual channel, severity taxonomy, validation policy) > ZeroClaw (accepted architecture RFCs, release-milestone tagging, security review labels) > Hermes/QwenPaw (strong contribution volume, weaker decision throughput — `needs-decision`/`awaiting-reporter`/`wontfix` clusters) > IronClaw (unknown).

---

## 7. Trend Signals (for AI agent developers)

1. **Durable, replayable state is becoming table stakes.** Every active project is fighting session/lease/crash-recovery bugs; ZeroClaw's append-only RFC is the clearest forward answer. **Implication:** design for event-sourced or replayable persistence from day one — retrofitting durability into mutable SQLite is the single largest recurring cost across the cohort.

2. **Silent failure is the new trust killer.** The most damaging reports are not crashes but *success-reported no-ops*: QwenPaw's Auto-Dream checkpointing dropped memory as `success=True` (#8500), Feishu media silently discarded (#8150), Visual Compact silently no-op (#8488), and ZeroClaw's plugins that "load" but never register (#11336). **Implication:** treat "did nothing vs. no data" as a distinct, testable failure class; instrument operations to prove effect.

3. **Multi-tenant identity is the next architectural frontier.** Single-user assistants are being retrofitted for teams: ZeroClaw RBAC/knowledge scoping, Hermes stable `user_id` and profile-scope isolation, QwenPaw Hub admin ops, OpenClaw operator authority. **Implication:** principal/scope should be a first-class dimension in the session and tool-call model, not a config add-on.

4. **Memory correctness is now a reliability requirement, not a feature.** Recall caps, nightly promotion, compaction model routing, and provenance boundaries are hot in OpenClaw, QwenPaw, Hermes, and ZeroClaw simultaneously. Long-context (340k-token sessions in QwenPaw #8464) is mainstream, not edge. **Implication:** memory lifecycle (ingest → recall → promote → compact → archive) needs explicit, inspectable semantics and tests.

5. **Cost and loop control are user-visible pain.** Hermes' 430-duplicate-call report (#137170) and QwenPaw's quantified token waste (#6116) show users metering agent runs. QwenPaw's `wontfix` on the highest-engagement thread is a divergence worth watching — it may push cost-conscious users toward competitors. **Implication:** repeat-success detection, call-fingerprint dedupe, and token budgets belong in the runtime, not just warnings.

6. **MCP/plugin lifecycle is an operations problem.** Idle-TTL eviction failures (OpenClaw #166017), spawn-broker child caps, duplicate stdio servers (~144 MB/child overhead), and generation-mismatch discovery (ZeroClaw #11562) are operational incidents, not protocol bugs. **Implication:** ship eviction defaults, broker caps, and registration verification as first-class features.

7. **Config schema trust is a retention risk.** "Accepted but entirely unread" config (ZeroClaw #9394, #11589; OpenClaw #172969; Hermes #90040) is a recurring class that quietly breaks user assumptions. **Implication:** validate and wire config keys, or fail loudly at load.

8. **Release discipline is differentiating.** Only OpenClaw maintains a stable + LTS pair; Hermes and ZeroClaw ship nothing this window despite heavy activity, and QwenPaw is pre-release beta. For decision-makers, **OpenClaw is currently the only safe default for production-adjacent deployment**, while QwenPaw/ZeroClaw are best treated as high-momentum, pre-GA bets and IronClaw as inactive.

**Bottom line for technical decision-makers:** choose OpenClaw for production-grade breadth and release cadence (accepting active P0 state-durability risk); choose Hermes/QwenPaw for team/channel-specific workflow automation during their stabilization phases; watch ZeroClaw if multi-tenant auditability and append-only state are requirements, contingent on its P1 security review queue clearing (#9647/#9745 stalled ~3 months).

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-10-30

**Data note:** The dataset reports items *updated* in the last 24 hours, not necessarily created that day. PR comment counts were not available. “Merged/closed” PRs are reported together, so merged versus closed-without-merge cannot be fully distinguished from the provided data.

---

## 1. Today's Overview

Hermes Agent remains highly active: 50 issues and 50 PRs were updated in the last 24 hours, with no new release. The issue queue is heavily weighted toward open work — 48 of 50 updated issues remain open/active — while PR throughput is stronger, with 10 of 50 updated PRs merged or closed. Activity is concentrated in P1 reliability problems around install/update state, credential-pool rotation, cron durability, Kanban worker stability, and agent loop control. Community engagement is strong, including a 30-comment architecture RFC and a 31-👍 desktop installation feature request, but several high-severity bugs still lack clearly linked fix PRs. Overall, the project looks healthy and actively maintained, with a sizable backlog and visible reliability debt in gateway, cron, and multi-profile subsystems.

---

## 2. Releases

No new releases were published in the last 24 hours.

---

## 3. Project Progress

Ten PRs were merged or closed in the reporting window. Among the visible closed PRs, progress clustered around delivery observability, resource safety, documentation hygiene, and Kanban correctness:

- [PR #98918](https://github.com/NousResearch/hermes-agent/pull/98918) — Made the local STT fallback configurable, avoiding expensive local Whisper fallback on small VPS instances.
- [PR #98917](https://github.com/NousResearch/hermes-agent/pull/98917) — Added copyable cron job and run IDs to deliveries for easier debugging.
- [PR #98916](https://github.com/NousResearch/hermes-agent/pull/98916) — Preserved the selected Kanban board when deleting tasks; closed as duplicate.
- [PR #98915](https://github.com/NousResearch/hermes-agent/pull/98915) — Kept auto-TTS output under `HERMES_HOME` rather than the system temp directory.
- [PR #73923](https://github.com/NousResearch/hermes-agent/pull/73923) — Persisted `provider_message_id`, `delivery_route`, and `chunk_count` in the delivery ledger for traceability.
- [PR #63812](https://github.com/NousResearch/hermes-agent/pull/63812) — Made the runtime footer use effective OAuth and Fast signals instead of hardcoded provider assumptions.
- [PR #74573](https://github.com/NousResearch/hermes-agent/pull/74573) — Fixed stale zh-Hans skill documentation cross-references.
- [Issue #78980](https://github.com/NousResearch/hermes-agent/issues/78980) — Closed: cron lifecycle guard false-positived on Python scripts containing `~/...` path literals.

**Themes advanced:** delivery traceability, TTS/STT resource safety, documentation accuracy, Kanban deletion correctness, and cron guard false positives. Notably, several closed PRs were months old, suggesting a backlog-clearing pass rather than purely same-day feature work.

---

## 4. Community Hot Topics

### Top Issues by engagement

1. **[Issue #143379](https://github.com/NousResearch/hermes-agent/issues/143379)** — *RFC: THE DOC-CHAIN SUBSTRATE'S NETWORK — the read, the governance, the compaction*  
   **30 comments** · P2 · security, agent, tools, file, memory, compression  
   The most-discussed item. The RFC concerns durable records surviving session compression with provenance intact, and the agent re-reading that record instead of making the user repeat information.  
   **Underlying need:** long-horizon memory correctness, provenance preservation, and governance of compressed context. This is a core architectural concern for personal AI assistants that operate across long sessions.

2. **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — *[Feature]: Hermes Desktop frontend install only*  
   **23 comments · 31 👍** · P2 · needs-decision · Windows  
   Users want to install only the desktop frontend and connect to an agent already running on another machine.  
   **Underlying need:** deployment flexibility and remote-agent UX. The high reaction count signals strong demand, especially for Windows and split-machine setups.

3. **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — *source-completion-pending has no TTL or self-heal*  
   **15 comments** · P1 · install/update  
   A killed update tail can strand `source-completion-pending`, causing every later launch to re-enter the same state.  
   **Underlying need:** crash-safe install/update state machines with TTL and self-healing.

4. **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — *Anthropic per-request credential refresh reverts credential-pool rotation — two pooled subscriptions bill as one*  
   **11 comments · 1 👍** · P1 · auth, billing, security boundary  
   Two pooled subscriptions can be billed as one, and the turn may fall through to another provider.  
   **Underlying need:** correct credential-pool rotation and billing isolation — a high-trust requirement for multi-subscription users.

5. **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — *cron: restart-safe worker dies on `database is locked` in `adopt_claimed_execution()`*  
   **10 comments · 1 👍** · P1  
   A restart-safe cron worker exits after one `SQLite` lock timeout, losing the occurrence.  
   **Underlying need:** durable scheduler behavior under SQLite contention, with retries and safe adoption semantics.

6. **[Issue #145022](https://github.com/NousResearch/hermes-agent/issues/145022)** — *Kanban workers crash with “Kanban execution unknown; retained by owner” when the gateway stalls*  
   **10 comments** · P1 · sessions  
   Concurrent Kanban workers crash when the gateway stalls during saves; work in progress is lost.  
   **Underlying need:** robust gateway/worker coordination and session ownership recovery under load.

### PR engagement note

PR comment counts were unavailable. Notable open fix PRs include [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435) (source-completion tail), [PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792) (credential-pool rotation/429 handling), [PR #146703](https://github.com/NousResearch/hermes-agent/pull/146703) (Telegram general-pool recovery), and [PR #143956](https://github.com/NousResearch/hermes-agent/pull/143956) (Discord typing-state pauses).

---

## 5. Bugs & Stability

### P1 — Critical

1. **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — `source-completion-pending` has no TTL or self-heal; a killed tail strands every later launch.  
   **Fix status:** Likely related fix PR [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435) is open (“charge the owed source-completion tail before it runs”). Issue is marked `awaiting-reporter`.

2. **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — Anthropic per-request credential refresh reverts credential-pool rotation; pooled subscriptions can bill as one.  
   **Fix status:** Related pool-rotation PR [PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792) is open, but it is not confirmed as a complete fix for this billing/rotation regression.

3. **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — Restart-safe cron worker dies on `database is locked`; occurrence is lost.  
   **Fix status:** No direct fix PR identified in the provided data.

4. **[Issue #145022](https://github.com/NousResearch/hermes-agent/issues/145022)** — Kanban workers crash when the gateway stalls during worker saves; in-progress work is lost.  
   **Fix status:** No direct fix PR identified. [PR #146714](https://github.com/NousResearch/hermes-agent/pull/146714) addresses uninitialized sibling Kanban DBs during cleanup, which is adjacent but not a confirmed fix for the stall.

5. **[Issue #137170](https://github.com/NousResearch/hermes-agent/issues/137170)** — Agent loop has no circuit breaker for repeated identical *successful* tool calls; observed 430 duplicate `kanban_comment` calls in one turn.  
   **Fix status:** No direct fix PR identified. This is a notable agent-safety gap: existing loop detection covers repeated failures, not repeated successes.

### P2 — High-impact clusters

**Cron durability and delivery**
- [Issue #146428](https://github.com/NousResearch/hermes-agent/issues/146428) — Cron run wedges silently on `UnscopedSecretError` during home-target resolution in multiplexed profiles.
- [Issue #101623](https://github.com/NousResearch/hermes-agent/issues/101623) — `--continuity` injects the previous output file prompt-first, truncating the prior response and nesting prompts.
- [Issue #145596](https://github.com/NousResearch/hermes-agent/issues/145596) — Paused cron job reactivates after gateway restart.
- [Issue #146668](https://github.com/NousResearch/hermes-agent/issues/146668) — WeCom proactive send hangs; `_dispatch_on_gateway_loop` has no timeout and wedges the delivery queue.
- [Issue #143953](https://github.com/NousResearch/hermes-agent/issues/143953) — Multiplex gateway: satellite Discord session paused `not_found` after cron run; delivery to routed channel fails.

**Gateway performance and session state**
- [Issue #146643](https://github.com/NousResearch/hermes-agent/issues/146643) — Gateway re-reads the whole session history on every change; thousands of sessions cause 1.5–2.5 CPU cores and 30s Desktop timeouts.
- [Issue #120856](https://github.com/NousResearch/hermes-agent/issues/120856) — Gateway restart adopts and later kills other processes’ background jobs via shared `processes.json`.
- [Issue #146507](https://github.com/NousResearch/hermes-agent/issues/146507) — Async-delegation ledger SQLite operations run on the gateway event loop, causing 10–20s stalls and blocked Discord heartbeats.
- [Issue #139210](https://github.com/NousResearch/hermes-agent/issues/139210) — Compaction writes copies of earlier user messages as new rows in `state.db`.
- [Issue #138573](https://github.com/NousResearch/hermes-agent/issues/138573) — Discord voice-note sessions get junk titles (“Empty message greeting”) that lock permanently.

**CLI, install, and configuration**
- [Issue #125537](https://github.com/NousResearch/hermes-agent/issues/125537) — `pm gc` measures the executing snapshot’s install key and silently collects nothing.
- [Issue #139500](https://github.com/NousResearch/hermes-agent/issues/139500) — ~22,600 `.pyc` files bloat even small installs by >303 MiB.
- [Issue #44964](https://github.com/NousResearch/hermes-agent/issues/44964) — `/sessions` CLI command only shows `source=cli` sessions; TUI and other sources are hidden.
- [Issue #90040](https://github.com/NousResearch/hermes-agent/issues/90040) — `skills.external_dirs` ignores Managed Scope and must be duplicated per profile.

**Kanban, provider, and plugin issues**
- [Issue #146237](https://github.com/NousResearch/hermes-agent/issues/146237) — Desktop Kanban card reassignment to another profile fails with `403 profile_scope_mismatch`.
- [Issue #145480](https://github.com/NousResearch/hermes-agent/issues/145480) — `kimi-coding` in-app `/models` discovery fails; static list overwrites cached catalog.
- [Issue #124874](https://github.com/NousResearch/hermes-agent/issues/124874) — Fallback after HTTP-200 refusal/invalid response logs no cause and is mislabeled “provider failure.”

### Open fix PRs worth tracking
- [PR #146046](https://github.com/NousResearch/hermes-agent/pull/146046) — Scratch idle rule for long-running processes.
- [PR #146703](https://github.com/NousResearch/hermes-agent/pull/146703) — Telegram general-pool recovery.
- [PR #143956](https://github.com/NousResearch/hermes-agent/pull/143956) — Discord typing pause across approval/SDK waits.
- [PR #126506](https://github.com/NousResearch/hermes-agent/pull/126506) — Telegram RTL rich-message flag.
- [PR #126889](https://github.com/NousResearch/hermes-agent/pull/126889) — Browser vault card consent gating.
- [PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792) — Credential-pool rotation before waiting on 429.

---

## 6. Feature Requests & Roadmap Signals

- **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — Desktop frontend-only install with remote connection. **23 comments, 31 👍, `needs-decision`.** This is the strongest roadmap signal in the dataset: users want to separate the UI from the agent runtime, especially on Windows.
- **[Issue #35147](https://github.com/NousResearch/hermes-agent/issues/35147)** — Expose stable `user_id` alongside mutable `user_name` in single-user session context, for multi-user team profiles and identity mapping.
- **[Issue #142235](https://github.com/NousResearch/hermes-agent/issues/142235)** — Public read-only MCP tool call for plugin backends, gated by `readOnlyHint`. This would let dashboards and CLI plugins safely read configured MCP servers.
- **[Issue #146699](https://github.com/NousResearch/hermes-agent/issues/146699)** — Cron agents cannot see their jobs’ run history or whether a run was delivered or silenced. Requests better cron observability from the agent’s own tools.
- **[Issue #143577](https://github.com/NousResearch/hermes-agent/issues/143577)** — Documentation/FAQ request for the 1.0 GB `~/.hermes/cache/uv/` directory that sits outside every pruner.
- **[PR #110909](https://github.com/NousResearch/hermes-agent/pull/110909)** — Opt-in cron continuation after a background terminal process completes; relevant to long-running scheduled workflows.

**Prediction:** The next version is likely to prioritize install/update and deployment UX (frontend-only desktop, cache/pruner behavior), cron observability and continuation semantics, and MCP/plugin integration. Multi-profile identity and scope handling also appears repeatedly across bugs and feature requests, suggesting it may become a broader roadmap theme.

---

## 7. User Feedback Summary

**Real pain points**
- **Install footprint and cache management:** Users report ~22,600 `.pyc` files and >303 MiB bloat in small installs ([#139500](https://github.com/NousResearch/hermes-agent/issues/139500)), plus a 1.0 GB `uv` cache outside every pruner ([#143577](https://github.com/NousResearch/hermes-agent/issues/143577)). The tone in #139500 (“Dev Team has refused to fix”) indicates frustration with long-standing install hygiene.
- **Deployment flexibility:** The frontend-only install request ([#38519](https://github.com/NousResearch/hermes-agent/issues/38519)) reflects a real split-machine use case: agent on one host, desktop UI on another.
- **Reliability of background automation:** Cron and Kanban users are hitting lost occurrences, silent wedges, paused-job reactivation, and worker crashes ([#135487](https://github.com/NousResearch/hermes-agent/issues/135487), [#146428](https://github.com/NousResearch/hermes-agent/issues/146428), [#145596](https://github.com/NousResearch/hermes-agent/issues/145596), [#145022](https://github.com/NousResearch/hermes-agent/issues/145022)).
- **Scale and session state:** A few thousand sessions can drive high gateway CPU and Desktop timeouts ([#146643](https://github.com/NousResearch/hermes-agent/issues/146643)); compaction can duplicate user messages in `state.db` ([#139210](https://github.com/NousResearch/hermes-agent/issues/139210)).
- **Multi-profile and identity friction:** Profile-scope mismatches, managed-scope config ignoring, and routed Discord/cron sessions show that multi-profile setups remain rough ([#146237](https://github.com/NousResearch/hermes-agent/issues/146237), [#90040](https://github.com/NousResearch/hermes-agent/issues/90040), [#143953](https://github.com/NousResearch/hermes-agent/issues/143953)).
- **Provider and billing trust:** Credential-pool rotation reverting and pooled subscriptions billing as one ([#118379](https://github.com/NousResearch/hermes-agent/issues/118379)) is a high-trust issue for paying users.

**Use cases visible in the data:** remote desktop-to-agent connections, multi-user team profiles, Kanban-based workflow automation, scheduled cron jobs with delivery guarantees, plugin backends reading MCP servers, and platform integrations including Discord, Telegram, and WeCom.

**Satisfaction/dissatisfaction:** The community is deeply engaged and contributes detailed reports and PRs, including plugin maintainers updating their own catalog entries ([PR #146713](https://github.com/NousResearch/hermes-agent/pull/146713)). However, repeated root-cause families and slow resolution on install bloat and session visibility suggest maintainer attention is stretched. The overall sentiment is constructive but reliability-sensitive.

---

## 8. Backlog Watch

### Long-open issues needing maintainer attention

- **[Issue #38519](https://github.com/NousResearch/hermes-agent/issues/38519)** — Created 2026-06-03 · 23 comments · 31 👍 · `needs-decision` · Windows frontend-only install. High-demand feature with no visible decision.
- **[Issue #35147](https://github.com/NousResearch/hermes-agent/issues/35147)** — Created 2026-05-30 · 3 comments · expose `user_id` in session context. Long-unanswered but important for multi-user setups.
- **[Issue #44964](https://github.com/NousResearch/hermes-agent/issues/44964)** — Created 2026-06-12 · 8 comments · `/sessions` only shows CLI sessions. A visible usability gap in session management.
- **[Issue #90040](https://github.com/NousResearch/hermes-agent/issues/90040)** — Created 2026-08-19 · 4 comments · `skills.external_dirs` ignores Managed Scope. Important for administrators managing shared skill directories.
- **[Issue #120381](https://github.com/NousResearch/hermes-agent/issues/120381)** — Created 2026-09-23 · 5 comments · `/goal` can remain parked after a stalled subagent wait; issue says it is implemented by [PR #107130](https://github.com/NousResearch/hermes-agent/pull/107130), which remains open.
- **[Issue #127284](https://github.com/NousResearch/hermes-agent/issues/127284)** — Created 2026-09-29 · P1 · 15 comments · `awaiting-reporter`. Needs reporter follow-up or maintainer reproduction to unblock the likely fix in [PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435).
- **[Issue #118379](https://github.com/NousResearch/hermes-agent/issues/118379)** — Created 2026-09-21 · P1 · 11 comments · credential-pool billing/rotation. High severity and high trust impact.
- **[Issue #135487](https://github.com/NousResearch/hermes-agent/issues/135487)** — Created 2026-10-09 · P1 · 10 comments · cron worker dies on SQLite lock. No direct fix PR visible.

### Long-open PRs worth reviewing

- **[PR #110909](https://github.com/NousResearch/hermes-agent/pull/110909)** — Created 2026-09-14 · cron continuation after background process completion. A meaningful automation feature awaiting review.
- **[PR #92149](https://github.com/NousResearch/hermes-agent/pull/92149)** — Created 2026-08-22 · preserve memory-context provenance boundary. Directly relevant to the doc-chain/memory RFC discussion.
- **[PR #126889](https://github.com/NousResearch/hermes-agent/pull/126889)** — Created 2026-09-28 · vault card consent gating. Security-boundary fix that has waited over a month.
- **[PR #142792](https://github.com/NousResearch/hermes-agent/pull/142792)** — Created 2026-10-21 · pool rotation before 429 wait. Relevant to the P1 credential-pool issue.
- **[PR #145435](https://github.com/NousResearch/hermes-agent/pull/145435)** — Created 2026-10-27 · source-completion tail fix. A P1-area fix that should be prioritized alongside issue triage.

**Health summary:** The project is active and community-supported, with clear progress on delivery tracing, TTS/STT safety, and documentation. The main risk is a growing reliability backlog in cron, gateway/session state, install/update, and multi-profile behavior — areas where users are running Hermes in production-like, long-running, multi-platform setups.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

## Today’s Overview
On 2026-10-30, IronClaw showed **low, maintenance-only activity**: 0 issues updated in the prior 24 hours, 0 PRs merged or closed, and 0 new releases. The only visible activity was two open Dependabot PRs updating `wasmtime` in `/harness/latency/runner` ([#8162](https://github.com/nearai/ironclaw/pull/8162), [#8163](https://github.com/nearai/ironclaw/pull/8163)). Both PRs show 0 👍 reactions, with comment counts unavailable in the provided data, and neither has been merged. Overall project health for the day is quiet rather than feature- or fix-driven; the main maintainer action is reconciling two overlapping dependency upgrades.

## Project Progress
No PRs were merged or closed today, and no features advanced or bugs were fixed. The two open PRs are dependency-maintenance changes only:
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — bumps `wasmtime` from `47.0.3` to `48.0.7`.
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — bumps `wasmtime` from `47.0.4` to `49.0.5`.

Both target the same path, `/harness/latency/runner`, but propose different target versions. This suggests either duplicate/stale Dependabot branches or separate dependency manifests; maintainers should confirm which upgrade path is correct before merging.

## Community Hot Topics
There were no issues or PRs with recorded comments or reactions, so no genuine “hot topic” emerged. The most visible items were the two open Dependabot PRs:
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — `wasmtime` 47.0.3 → 48.0.7
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — `wasmtime` 47.0.4 → 49.0.5

The underlying need is routine dependency freshness and compatibility validation for the latency harness. Because both PRs touch the same dependency and directory, the community/maintainer need is less about discussion and more about **deduplication and upgrade-path decision-making**.

## Bugs & Stability
No bugs, crashes, or regressions were reported in the 24-hour window. No fix PRs exist. The only potential stability consideration is the `wasmtime` major-version jumps in the two dependency PRs; the provided release-note summaries are truncated, so breaking-change risk cannot be assessed from this dataset alone.

## Feature Requests & Roadmap Signals
No user-requested features or roadmap issues were recorded. The only signals are dependency-maintenance PRs. No releases exist in the dataset, so it is not possible to forecast next-version features; if a release follows, the visible inputs suggest it would be dependency maintenance rather than documented user-facing functionality.

## User Feedback Summary
No direct user feedback was captured: zero issues were updated, and no PR comments were available. As a result, user pain points, use cases, and satisfaction/dissatisfaction cannot be assessed. The absence of issue activity may indicate a quiet/stable period or low engagement, but the data is insufficient to distinguish between those explanations.

## Backlog Watch
No long-unanswered issues are identifiable because the issue list contains 0 items. The two open PRs are recent—created 2026-10-29—so they are not yet backlog, but they need maintainer attention to avoid becoming stale:
- [#8162](https://github.com/nearai/ironclaw/pull/8162) — review whether `wasmtime` 48.0.7 is the intended target.
- [#8163](https://github.com/nearai/ironclaw/pull/8163) — review whether `wasmtime` 49.0.5 supersedes #8162 or belongs to a different manifest.

Recommended action: reconcile the overlapping upgrades, merge the appropriate one after compatibility checks, and close any superseded PR.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-30

---

## 1. Today's Overview

QwenPaw is in a **high-velocity beta stabilization phase**: 55 tracked items were touched in the last 24 hours (15 issues, 40 PRs), with 10 issues closed against 5 still open/active and 13 PRs merged or closed against 27 still open. No release was cut, so all activity is accumulating toward the next `2.3.0` build — and the day's bug reports cluster tightly around `2.3.0-beta.1` regressions (plugin loader, desktop assets, local Whisper import). A notable share of closures are older enhancement requests (May–July filings) closed with only 1–2 comments, which suggests batch triage rather than shipped implementations. Conversely, the open-PR queue is dominated by very large submissions (`size/XL`, `XXL`, `XXXL`), several of which have been open for 4–7 weeks, indicating **review bandwidth — not contributor interest — is the current bottleneck**. Overall health is good on responsiveness but carries elevated regression risk in the new plugin/desktop architecture.

*Data caveats: PR comment counts are `undefined` in the source data and only 20 of 40 PRs are listed, so PR "hotness" below is inferred from labels, size, and age rather than reactions.*

---

## 2. Releases

None (0 new releases; no releases recorded in the dataset). No breaking-change or migration notes apply for this window.

---

## 3. Project Progress

### Closed / merged PRs today (4 of the top-20 shown; 13 total in window)

| PR | Title | Significance |
|---|---|---|
| [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) | `fix(plugins): tolerate import cache refresh failures during plugin loading` | **Directly closes the day's most severe regression** ([#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473)) — all backend plugins failing at startup on `2.3.0-beta.1`. |
| [#8358](https://github.com/agentscope-ai/QwenPaw/pull/8358) | `fix(providers): honor thinking defaults and declared wire controls` | Fixes Console-vs-wire mismatch for OpenAI-compatible models declaring native thinking effort (`low/medium/xhigh`). |
| [#8253](https://github.com/agentscope-ai/QwenPaw/pull/8253) | `feat(website): add JSON-driven evaluation index` | Bilingual `/evaluation/` leaderboard with SVG cost/performance chart and Pareto frontier — project transparency/benchmarking. |
| [#5490](https://github.com/agentscope-ai/QwenPaw/pull/5490) | `feat(console): navigable fullscreen image gallery for chat media` | Open since **2026-06-24** (~4 months); unified `Image.PreviewGroup` replaces isolated per-image previews. |

### Closed issues worth noting

- **[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473)** — plugin load failure at `module_isolation.py:220`, *same root cause as #8265 with a new call site* → second occurrence of an unguarded `importlib.invalidate_caches()`; fixed by #8497.
- **[#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464)** — manual `/compact` ignored session-level model switch, always using `agent.json` `active_model`; blocking for users on strict-validation providers (HTTP 400).
- **[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150)** — Feishu inbound rich-text (`post`) images silently dropped (text delivered, media discarded, no warning). Counterpart to outbound issue #2792.
- **[#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116)** — agent doom loop; closed **`wontfix`** after 9 comments (highest-discussion issue of the day).

### Feature directions advanced by open PRs
- **Memory subsystem overhaul** — [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) (auto-recall as transient `TextBlock` reference context), [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) (stop leaking recall payload to channels), [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291) (ReMe reranker requires non-empty `api_key`), [#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613) (OpenViking backend plugin).
- **Provider/plugin decomposition** — [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499) extracts local model management (downloads, llama.cpp install/lifecycle) into a plugin provider; [#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231) persists market install provenance so "Update All" covers third-party plugins.
- **PawApp control plane** — [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) redesigns SDK and app host contract (task identity, retries, permissions, approvals).

---

## 4. Community Hot Topics

| Rank | Item | Signal | Underlying need |
|---|---|---|---|
| 1 | [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) — Doom loop, repeated identical tool calls | **9 comments**, closed `wontfix` | Users want **cost control and loop-breaking at the runtime layer**, not a warning after ~6 repetitions. `wontfix` on the highest-engagement thread of the day is likely to be read as "token waste is accepted behavior" — worth a maintainer rationale or a documented mitigation (budget limits, call-fingerprint dedupe). |
| 2 | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — `/compact` ignores session model | 5 comments, closed | **Session-scoped configuration consistency.** A 340k-token session with `max_input_length=1M` is not "long tail" usage — context-management correctness is core UX for power users. |
| 3 | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — Feishu `post` images silently dropped | 3 comments, closed | **Channel fidelity + observability.** The silent part is the real complaint: users cannot distinguish "no images" from "images eaten." |
| 4 | [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500) — Auto-Dream drops memory, checkpoints anyway | 2 comments, OPEN | **Durability guarantees and honest status reporting** — a `success=True` on dropped data is a trust failure. |
| 5 | [#6189](https://github.com/agentscope-ai/QwenPaw/issues/6189) — `save_decision()` API | 2 comments, closed; filed 2026-07-16 | **Decision persistence across compaction.** The author correctly identifies the chicken-and-egg problem: the agent has already lost the decision by the time it should archive it. |
| 6 | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — all plugins fail to load | 2 comments, closed, fix PR exists | Beta-testers are running real hosts with 12+ plugins; **regression coverage for the isolation layer is thin.** |
| 7 | [#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613) — OpenViking memory plugin | `size/XXL`, first-time contributor, **open 53 days**, still receiving review follow-ups | Community demand for **pluggable memory backends**; also a retention risk — long review cycles on first-time-contributor XXL PRs. |

**Pattern:** the top three threads all concern *state and cost management across a turn/session* (loop control, compaction model routing, channel media), not new capabilities.

---

## 5. Bugs & Stability

Ranked by user impact (data loss / total breakage > silent degradation > cosmetic).

| Sev | Issue | Status | Impact | Fix PR? |
|---|---|---|---|---|
| **Critical** | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — all 12/12 backend plugins fail at startup, `2.3.0-beta.1` (`module_isolation.py:220`) | CLOSED | Total feature loss for plugin users; **repeat of #8265 root cause at a new call site** | ✅ [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) closed |
| **Critical (silent data loss)** | [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500) — Auto-Dream discards a full day of memory when extract parsing fails, **checkpoints the files anyway**, reports `success=True` | **OPEN** | Unrecoverable memory loss with no user-visible signal; worst class of bug in a personal-assistant product | ❌ None |
| **High** | [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) — desktop/PyInstaller build omits `visual_compression` glyph atlases → `FileNotFoundError: atlas-gray-native.ts`; Visual Compact silently no-ops | **OPEN** | Packaged Windows desktop only; a context-saving feature that silently does nothing means users burn tokens unknowingly | ❌ None |
| **High** | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — manual `/compact` uses `active_model`, not session model | CLOSED | Compaction permanently broken (HTTP 400) for strict providers | Not listed |
| **High** | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — Feishu inbound `post` images silently dropped | CLOSED | Multimodal input loss, no warning; opposite direction of #2792 → **channel media handling lacks symmetric tests** | Not listed |
| **Medium** | [#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490) — `GET /api/workspace/local-whisper-status` does a real `import whisper`; ~690 MB RSS, never released | **OPEN** | Merely *opening a settings page* permanently inflates server memory; optional dependency probe should use `importlib.util.find_spec` | ❌ None |
| **Medium** | [#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502) — Desktop "Open in browser" fails on Chinese paths (file-not-found) and no-ops outside the workspace | **OPEN** (filed 2026-10-30) | Clear i18n/path-encoding defect on default zh-CN Windows; broken core preview flow for the largest user locale | ❌ None |
| **Low** | [#8486](https://github.com/agentscope-ai/QwenPaw/issues/8486) — tool-call `<>` icon clipped (container lacks horizontal padding) | **OPEN** | Cosmetic, worsens at larger font scale/zoom | ❌ None |
| **Low (design)** | [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) — doom loop detected only after ~6 repeats | CLOSED `wontfix` | Wasted tokens/API spend per occurrence | ❌ Declined |

**Stability read:** 3 of the top 4 severity items are **beta-only regressions in the packaging/plugin path** (`2.3.0b1`), i.e., the architecture is changing faster than its distribution tests. Two silent-failure bugs ([#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500), [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)) share one theme: *features report success while doing nothing* — a systemic observability gap worth a dedicated fix, not two patches.

---

## 6. Feature Requests & Roadmap Signals

### Requests closed today (filed May–July; closed with 1–2 comments)
| Issue | Request | Filed | Signal |
|---|---|---|---|
| [#6189](https://github.com/agentscope-ai/QwenPaw/issues/6189) | `save_decision()` structured decision persistence to survive compaction | 2026-07-16 | High-value for long-horizon agents; a framework primitive, hard to work around at agent level |
| [#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356) | Finer file-guard granularity (read-only paths; project-dir allowlist instead of one global blacklist) | 2026-05-14 | **Security/safety posture** — the most enterprise-relevant ask in the batch |
| [#7740](https://github.com/agentscope-ai/QwenPaw/issues/7740) | Hub mode: admin cannot reset user passwords | 2026-09-14 | Basic multi-tenant ops gap; Hub deployments are blocked on manual DB work |
| [#4129](https://github.com/agentscope-ai/QwenPaw/issues/4129) | `/rewind` to roll back files/context | 2026-05-08 | Direct comparison to Claude Code; a top "missing parity" item |
| [#8459](https://github.com/agentscope-ai/QwenPaw/issues/8459) | Easier access to unread push messages (Console) | 2026-10-28 | Async-notification UX; closed after 2 days |
| [#8498](https://github.com/agentscope-ai/QwenPaw/issues/8498) | "install easily" (empty template) | 2026-10-29 | Onboarding friction, but no actionable content |

### Prediction for the next version (`2.3.0` line)
1. **Plugin/provider decomposition** — [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499) (local model management → plugin) and [#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231) (market provenance for "Update All") are both `Under Review`-adjacent and align with the beta's plugin work. **Most likely to land.**
2. **Memory recall correctness** — [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) + [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) + [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291) form a coherent cluster fixing the synthetic-tool-call leakage. **Likely in 2.3.x.**
3. **PawApp SDK/control plane** ([#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874), `XXXL`) — architecturally foundational but has been open 41 days; **likely slips to 2.4.**
4. **Local model scoring** ([#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463), Piccolo bidirectional candidate scoring for Jev decisions/ReMe compaction) — **experimental, uncertain.**

**Signal not to miss:** five long-standing enhancements closed in one day with minimal discussion, while the corresponding code changes are not visible in the PR list. If these were implemented, the digest should surface the PRs; if triaged-as-stale, users of [#4129](https://github.com/agentscope-ai/QwenPaw/issues/4129) and [#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356) will re-file.

---

## 7. User Feedback Summary

**Who is speaking:** predominantly Chinese-language users on Windows desktop and Hub deployments, plus channel-heavy users (Feishu, QQ, WeChat, WeCom, DingTalk, OneBot, Yuanbao).

**Pain points, concretely:**
- **Trust erosion from silent failures.** [#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500) ("run reports `success=True`… nothing user-visible indicates the content was dropped"), [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) ("silently dropped… no warning"), [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) ("Visual Compact silently no-ops"). Three independent reports, same complaint: *the product doesn't tell me when it fails.*
- **Cost anxiety.** [#6116](https://github.com/agentscope-ai/QwenPaw/issues/6116) explicitly quantifies waste ("many API calls and tokens have been wasted"). Users are metering their agent runs.
- **Long-context power usage is mainstream, not edge.** [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) documents a 340k-token session. Compaction correctness is now a first-class reliability requirement.
- **Localization/resource hygiene on desktop.** [#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502) (Chinese path breaks browser-open), [#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490) (690 MB RSS for a settings page). Desktop polish is lagging the backend.
- **Security defaults.** [#8501](https://github.com/agentscope-ai/QwenPaw/pull/8501) — `env list` prints every secret in plaintext and `env set` echoes stored values. Community-initiated fix, strong signal that secure-by-default was not present.
- **Satisfaction indicators:** detailed, reproducible reports with version/commit pins (`2.3.0b1`, `b12b06ed`, `2.2.2.post1`) indicate engaged, technically sophisticated users investing real effort — and first-time contributors ([#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613), [#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400), [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291)) show goodwill. **Dissatisfaction concentrates on responsiveness to beta regressions and on `wontfix` decisions for cost-related issues.**

---

## 8. Backlog Watch

### Long-open PRs needing maintainer decisions
| PR | Size | Age | Risk |
|---|---|---|---|
| [#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613) — OpenViking memory plugin | `XXL`, first-time contributor | **53 days** (since 09-07) | Author is still pushing review follow-ups (concurrency/install-ID fix). Highest churn-risk item; a stalled first-time XXL PR discourages further contribution. |
| [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) — PawApp SDK & app control plane redesign | `XXXL` | 41 days (since 09-19) | Foundational; blocks downstream app work and is too large to review casually — needs an explicit design-review slot or a split. |
| [#8034](https://github.com/agentscope-ai/QwenPaw/pull/8034) — bound inline media **per request**, not per file | `XL` | 31 days (since 09-29) | Real production failure mode: 2 MB/file cap + ~0 token cost ⇒ unbounded multi-MB request bodies, gateway byte-limit rejections. No token-based eviction fires. |
| [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) — stop leaking auto-memory-recall payload to channels | `M` | **43 days** | User-visible leak of internal recall scaffolding into chat channels; overlaps [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457), risking **duplicate/conflicting fixes** — needs a consolidation call. |
| [#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499) — extract local model management into plugin | `XXXL`, WIP | 1 day | Early, but watch for scope creep against [#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463). |
| [#8308](https://github.com/agentscope-ai/QwenPaw/pull/8308) / [#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400) / [#8291](https://github.com/agentscope-ai/QwenPaw/pull/8291) | `XS`–`S` | 12–13 days | Small, low-risk fixes sitting behind big PRs in the queue — good candidates for a fast lane to keep contributor momentum. |

### Issues that need maintainer attention
- **[#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500)** (Auto-Dream silent memory loss) — open with **no linked fix PR**; the only unrecoverable-data-loss bug in today's set. Should be the top of tomorrow's queue.
- **[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)** (missing `visual_compression` assets in desktop build) — open, no fix; likely a one-line `PyInstaller` spec/datas entry, so a cheap, high-value win.
- **[#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490)** (Whisper import = 690 MB permanent RSS) — open, no fix; trivially addressable via `find_spec`.
- **[#4356](https://github.com/agentscope-ai/QwenPaw/issues/4356)** (file-guard read-only/allowlist granularity) — closed after 5+ months and 2 comments with no visible implementing PR. Given it concerns **agent filesystem safety**, recommend publishing the resolution rationale or re-opening.
- **[#8502](https://github.com/agentscope-ai/QwenPaw/issues/8502)** (Chinese-path browser-open failure) — filed today but representative of a class; needs a triage label and an owner before it ages.

---

**Bottom line:** throughput is strong and contributors are engaged, but this window shows **three structural risks** — (a) beta packaging/plugin regressions escaping to users ([#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473), [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488)), (b) silent-failure patterns that undermine trust in memory and context features ([#8500](https://github.com/agentscope-ai/QwenPaw/issues/8500), [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150)), and (c) a large-PR review queue where the oldest items are now 40–53 days old ([#7613](https://github.com/agentscope-ai/QwenPaw/pull/7613), [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874)). None require new engineering capacity so much as triage policy: a small-PR fast lane, a "silent failure" test category, and a decision on the overlapping memory-recall PRs.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-30

---

## 1. Today's Overview

ZeroClaw remains highly active with **50 issue updates** (33 open/active, 17 closed) and **50 PR updates** (44 open, 6 merged/closed) in the last 24 hours, but shipped **no new release**. The most notable signal is a widening intake-to-merge gap: 44 PRs remain open against only 6 closed, and a large share of open PRs are `size:XL`, `needs-author-action`, or `needs-maintainer-review`, suggesting review capacity rather than contributor interest is the constraint. Activity clusters around four themes: **identity and access security** (per-agent attribution, RBAC, principal scoping), **append-only session/event persistence**, **plugin system reliability**, and **ZeroCode TUI polish**. Seventeen issue closures today reflect genuine cleanup momentum — including an accepted architecture RFC and several S2 bug fixes — while the v0.9.0 milestone continues to accumulate security-tagged work.

---

## 2. Releases

No new releases in the last 24 hours. The project is between the recent **v0.8.7** documentation/catalogue release and an accumulating **v0.9.0** milestone (issues #9394, #11970, #12076 and PR #12095 carry `release:v0.9.0`).

---

## 3. Project Progress

**Merged/closed PRs (1 visible of 6 reported):**
- [#12095](https://github.com/zeroclaw-labs/zeroclaw/pull/12095) — `fix(deps): bump pulldown-cmark to 0.13.5 for RUSTSEC-2026-0414`. Low-risk, `size:XS`, but high-impact: the advisory landed 2026-10-28 and was failing the Security job, which blocked `CI Required Gate` on **every** merge. This unblocks the pipeline.

**Closed issues representing real progress:**
- [#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) — **RFC accepted and closed after 14 comments** (append-only session event history, deterministic replay, derived agent streams). Implementation is already live as [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921), making this the single largest architectural advance in flight.
- [#11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562) — plugin discovery could pair one generation's manifest with another's component during update (S2). Closed.
- [#11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432) — daemon killed mid-turn left sessions permanently `state = 'running'`. Closed.
- [#11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517) — web chat reload mid-turn dropped the user's prompt from screen *and* `localStorage`. Closed.
- [#11484](https://github.com/zeroclaw-labs/zeroclaw/issues/11484) — ZeroCode turns disabled repetitive-tool safeguards. Closed.
- [#11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970) — `agents delete/rename` skipped owned-state cascade in builds without the gateway feature. Closed.
- [#11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792), [#11708](https://github.com/zeroclaw-labs/zeroclaw/issues/11708), [#10400](https://github.com/zeroclaw-labs/zeroclaw/issues/10400), [#11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486), [#11987](https://github.com/zeroclaw-labs/zeroclaw/issues/11987), [#12076](https://github.com/zeroclaw-labs/zeroclaw/issues/12076) — plugin orphaned-stage cleanup, tool schema completeness, Telegram unauthorized-sender notice, ZeroCode config UX, Semgrep `nosemgrep` handling, and v0.8.7 translation fixes.

**Features advancing in open PRs:**
- Session persistence: [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921) (append-only event history).
- Plugin webhook RPC, in a dependency stack: [#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) → [#11322](https://github.com/zeroclaw-labs/zeroclaw/pull/11322).
- Security/identity: [#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745) (per-agent knowledge scoping), [#11230](https://github.com/zeroclaw-labs/zeroclaw/pull/11230) (withhold session-data tools from non-admin principals), [#11301](https://github.com/zeroclaw-labs/zeroclaw/pull/11301) (committed-rename recovery), [#11313](https://github.com/zeroclaw-labs/zeroclaw/pull/11313) (publish authorization edits to running daemon), [#11265](https://github.com/zeroclaw-labs/zeroclaw/pull/11265) (roster password lifecycle CLI).
- ZeroCode TUI: [#11727](https://github.com/zeroclaw-labs/zeroclaw/pull/11727), [#11501](https://github.com/zeroclaw-labs/zeroclaw/pull/11501), [#11502](https://github.com/zeroclaw-labs/zeroclaw/pull/11502), [#11505](https://github.com/zeroclaw-labs/zeroclaw/pull/11505), [#11510](https://github.com/zeroclaw-labs/zeroclaw/pull/11510), [#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622), [#11731](https://github.com/zeroclaw-labs/zeroclaw/pull/11731).

---

## 4. Community Hot Topics

| Item | Comments | Why it matters |
|---|---|---|
| [#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) RFC: append-only session events | 14 | Most-discussed item today despite being closed. Community converged on replacing mutable message persistence with replayable event history — a foundational change touching runtime, storage and all channels. |
| [#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982) Per-sender RBAC | 11 | Longest-running high-engagement thread. The scope has been **narrowed** to build sender roles on the existing agent/risk-profile model rather than a new `[rbac.*]` subsystem — a pragmatic de-scoping driven by community feedback. Draft work in #11068. |
| [#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999) Discord plugin install proof | 9 | Currently `status:blocked`, but inbound/outbound Discord delivery was verified on the published v0.8.6 archive. Remaining gap is packaging/installation proof, not runtime behavior. |
| [#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418) ZeroCode "Copy" button does nothing | 8 | S1 workflow-blocked, updated today, `status:in-progress`. High user-visible annoyance in the TUI. |
| [#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416) Slack "is thinking…" status lost since v0.8.5 | 6 | Regression report; reporter explicitly asks "intended?" — a UX signal for Slack channel users. |
| [#10926](https://github.com/zeroclaw-labs/zeroclaw/issues/10926) Matrix `send_via` misroutes to peer identity | 5 | S2; root cause identified in `resolve_target` using the first `external_peers` entry as a room recipient. |
| [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) No per-agent attribution in knowledge graph | 5 | S0 security. Any agent can read/mutate another agent's knowledge. Directly blocks trustworthy multi-agent deployments. |
| [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921) Append-only session events (impl) | n/a | Comment counts unavailable for PRs in this dataset, but this is the largest-scope PR in flight (60+ labels, `risk:high`, `size:XL`). |

**Underlying need:** the community is pushing ZeroClaw from "single-user assistant" toward **multi-tenant, auditable agent infrastructure** — ownership boundaries, replayable state, and per-principal authorization. Almost every hot thread maps to that direction.

---

## 5. Bugs & Stability

| Severity | Issue | Impact | Fix status |
|---|---|---|---|
| **S0 / p1** | [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) Knowledge graph has no per-agent attribution | Any agent reads/mutates another agent's knowledge — data loss / security risk | Fix PR open: [#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745) (`needs-maintainer-review`, open since 2026-08-04) |
| **p1 security** | [#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394) `gateway.pairing_dashboard` accepted but entirely unread; pairing codes never expire | Config silently ignored; stale pairing codes remain valid | **No fix PR linked** |
| **S1** | [#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418) ZeroCode one-click Copy broken | Workflow blocked in TUI | `in-progress`, no PR listed |
| **S2** | [#10926](https://github.com/zeroclaw-labs/zeroclaw/issues/10926) Matrix `send_via` treats peer identities as room destinations | Messages sent to wrong destination | Accepted, no PR |
| **S2** | [#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332) Skill review/creation never run for channel, webhook or gateway turns | Learning loop silently inactive for most non-CLI usage | Accepted, no PR |
| **S2** | [#11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336) `plugin info` / `list --verify` report `[loads]` for plugins the runtime refuses to register | False-positive plugin validation | Accepted, no PR |
| **S2** | [#10294](https://github.com/zeroclaw-labs/zeroclaw/issues/10294) `file_write` transcripts can't distinguish create vs overwrite | Audit/observability gap | Open, follow-up status |
| **S3** | [#11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360) Package lock file world-readable → other local accounts can hold it and break install/remove | Availability only | Accepted, no PR |
| **S3** | [#11358](https://github.com/zeroclaw-labs/zeroclaw/issues/11358) `plugin migrate` ignores package lock, can merge into a concurrently created destination | Race condition | Accepted, no PR |
| **Regression** | [#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416) Slack "is thinking…" status missing since v0.8.5 | UX regression, reporter asks if intentional | Accepted |

**Fixed today:** #11517 (prompt loss on reload), #11432 (stuck `running` sessions), #11562 (manifest/component generation mismatch), #11484 (repetitive-tool safeguards), #11708 (incomplete tool schema), #11970 (missing cascade in gateway-less builds), #11792 (orphaned install stage), #11987 (Semgrep suppression).

**Assessment:** the *reported* bug flow is healthy — 8 stability fixes closed today, and the pulldown-cmark bump removes a repo-wide CI blocker. The concentration of unresolved risk sits in **long-lived security issues** (#9394, #9647) rather than in fresh regressions.

---

## 6. Feature Requests & Roadmap Signals

- **Per-sender RBAC for multi-tenant deployments** — [#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982). Scope narrowed and accepted; a draft exists. Strong candidate for v0.9.0 or v0.10.
- **Unified file intake / resolver / delivery** — [#11013](https://github.com/zeroclaw-labs/zeroclaw/issues/11013) tracker, following accepted RFC #9488. Cross-channel file handling is a recurring user ask.
- **Public runtime composition boundary** — [#10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993). Embeddability of the agent runtime; an ecosystem/extension play rather than an end-user feature.
- **Plugin install reseed semantics** — [#10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162). Follow-up to install durability work (#11098).
- **Operator cost-limit override** — [#11589](https://github.com/zeroclaw-labs/zeroclaw/pull/11589). `cost.allow_override` was documented but read by nothing; PR makes it functional. Small, high-value, likely to land soon.
- **Session-event replay + derived streams** — [#10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526) / [#11921](https://github.com/zeroclaw-labs/zeroclaw/pull/11921). The RFC is closed and implementation is open; this is the headline architectural item.
- **ZeroCode TUI configurability** — per-pane session cap ([#11731](https://github.com/zeroclaw-labs/zeroclaw/pull/11731)), transcript timestamps ([#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622)), multi-select editors ([#11502](https://github.com/zeroclaw-labs/zeroclaw/pull/11502)).

**v0.9.0 prediction:** security/identity work (knowledge-graph scoping, non-admin tool withholding, daemon auth-edit propagation), the pulldown-cmark advisory fix, and Telegram unauth-notice changes. Session event history is large enough that it may slip to v0.9.x or v0.10.

---

## 7. User Feedback Summary

**Pain points expressed by real users:**
- *"This 'Copy' button is not working for me. It does absolutely nothing for my clipboard."* — basic TUI affordance broken (S1), [#11418](https://github.com/zeroclaw-labs/zeroclaw/issues/11418).
- *"I have `skill_improvement` and `skill_creation` both enabled, but the learning loop only runs for CLI/cron style runs."* — silent feature degradation for Matrix/webhook/web-UI users, [#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332).
- *"All of my agent's skills live in a skill bundle... But the skill review fork can't see any of them."* — [#11333](https://github.com/zeroclaw-labs/zeroclaw/issues/11333).
- *"Reloading the dashboard page while an agent turn is still running makes the user's prompt disappear from the screen **and** from `localStorage`."* — data-loss perception during normal use, [#11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517) (now closed).
- *"A session whose turn was in flight when the daemon died stays `state = 'running'` permanently."* — operator trust in session state, [#11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432) (now closed).
- *"Since v0.8.5... the only sign of life is the inbound acknowledgement reaction (👀)."* — Slack users lost progress feedback, [#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416).

**Satisfaction signals:** fast turnaround on well-scoped bugs (multiple S2/S3 issues filed in early October and closed today); maintainers actively narrow scope in threads rather than letting proposals stall; ZeroCode UX work is being delivered as a steady stream of focused PRs from a distinguished contributor.

**Dissatisfaction signals:** regression in Slack feedback, repeated broken-tool-loop reports from local-model users (Ollama `qwen3.5:9b`, MLX-LM), and configuration that is "accepted and entirely unread" (#9394, #11589) — a class of bug that erodes trust in the config schema.

---

## 8. Backlog Watch

| Item | Age | Concern |
|---|---|---|
| [#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394) pairing dashboard ignored + non-expiring pairing codes | Open since **2026-07-26** (~14 weeks) | `priority:p1`, `security:pairing`, tagged `release:v0.9.0`, **no linked fix PR**. Highest unresolved-risk item with zero implementation movement. |
| [#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647) knowledge graph cross-agent access | Open since **2026-08-01** | S0 severity. Fix PR [#9745](https://github.com/zeroclaw-labs/zeroclaw/pull/9745) has sat in `needs-maintainer-review` since **2026-08-04** — nearly three months. Needs review allocation, not more author work. |
| [#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982) per-sender RBAC | Open since **2026-04-22** (~27 weeks) | Accepted and scope-narrowed, 11 comments, but no PR on the narrowed design. Oldest high-engagement feature request in the dataset. |
| [#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999) Discord plugin install proof | Open since 2026-09-20, `status:blocked` | Blocked with 9 comments; runtime verified, packaging proof outstanding. Needs a decision on whether the acceptance bar changes. |
| [#10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162) plugin install seeding retry | Open since 2026-08-20, `in-progress` | Partial fix landed via #11098; the already-installed case still lacks an agreed answer. |
| [#11013](https://github.com/zeroclaw-labs/zeroclaw/issues/11013) unified file intake tracker | Open since 2026-09-20 | Implementation batch tracker with only 3 comments — low visibility for a cross-cutting workstream. |
| [#11301](https://github.com/zeroclaw-labs/zeroclaw/pull/11301) / [#11313](https://github.com/zeroclaw-labs/zeroclaw/pull/11313) | Open since 2026-10-01 | Both `priority:p1`, `risk:high`, `size:XL`, awaiting maintainer review — part of the P1 security queue that is not moving. |
| [#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) / [#11322](https://github.com/zeroclaw-labs/zeroclaw/pull/11322) | Open since 2026-10-01 | Deep dependency stack (`#11319` → `#11339` → `#11165` → …); each layer restacked. Stacked PR chains are a structural review bottleneck. |
| [#11265](https://github.com/zeroclaw-labs/zeroclaw/pull/11265) user password lifecycle CLI | Open since 2026-09-30, `do-not-merge` | Explicitly marked do-not-merge and stacked on two unmerged PRs (#11264, #11313) — will not progress until the base lands. |
| [#10294](https://github.com/zeroclaw-labs/zeroclaw/issues/10294) `file_write` create/overwrite ambiguity | Open since 2026-08-24 | Observability/audit gap for transcript consumers; low priority but blocks reliable file-effect auditing. |

---

### Data caveats
- Only the top 30 of 50 issues and top 20 of 50 PRs by comment count are shown; PR comment counts were returned as `undefined`, so PR engagement is inferred from labels and scope rather than measured discussion volume.
- Of the 6 merged/closed PRs reported, only #12095 is visible in the provided sample.
- Label counts and comment totals are point-in-time snapshots at 2026-10-30.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*