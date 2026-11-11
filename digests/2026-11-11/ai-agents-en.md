# OpenClaw Ecosystem Digest 2026-11-11

> Issues: 289 | PRs: 500 | Projects covered: 5 | Generated: 2026-11-11 02:13 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# **OpenClaw Project Digest – 2026-11-11**

---

### **1. Today's Overview**  
OpenClaw remains highly active with a robust momentum in development and community engagement. The project saw **500 PRs updated in the last 24 hours** (346 open, 154 merged/closed) and **289 new issues**, indicating strong contributor participation and real-world user feedback. The release of **v2026.11.3** and **v2026.9.37** signals ongoing stabilization and security hardening, particularly for enterprise and gateway-heavy deployments. High-priority bugs related to session state integrity, model fallback behavior, and authentication persistence dominate the issue tracker, reflecting deep operational complexity in multi-agent workflows.

---

### **2. Releases**  
- **`v2026.11.3` (latest)**: Focuses on privacy and reliability improvements.  
  - ✅ **Private task results**: Subagent completions are now kept out of streamed chat replies—enhancing privacy during live interactions. ([#188490](https://github.com/openclaw/openclaw/issues/188490))  
  - ✅ **Reliable child-task handoffs**: Parent agents can now safely reassign work after cancelling a child without leaving orphaned or broken states.  
  - 📌 *No breaking changes reported.*  

- **`v2026.9.37` (gateway-only, extended-stable)**: A retrospective security and stability release from the end of September 2026, incorporating critical fixes and new model support.  
  - 🔒 Includes fixes for SQLite header-probe timeouts and memory exhaustion risks.  
  - 🛠️ Targeted at users on older stable lines needing long-term maintenance.  
  - ⚠️ Not intended for new deployments; use `v2026.11.3` instead.

> 📌 [Release Notes & Downloads](https://github.com/openclaw/openclaw/releases/tag/v2026.11.3)

---

### **3. Project Progress**  
**154 PRs merged or closed today**, showing rapid iteration on core stability and UX polish. Key advancements include:

- ✅ **Session resilience**: Fixes for `sessions_spawn` failures under `claude-cli` runtime ([#154572](https://github.com/openclaw/openclaw/issues/154572)), and improved handling of interrupted inter-session messages ([#144594](https://github.com/openclaw/openclaw/issues/144594)).
- ✅ **UI/UX refinements**: 
  - Fixed double-rendering of assistant turns in CLI backends ([#123792](https://github.com/openclaw/openclaw/issues/123792)).
  - Improved focus frame visibility in read-only chats ([#196478](https://github.com/openclaw/openclaw/pull/196478)).
  - Restored expanded presentation for model-fallback events ([#196942](https://github.com/openclaw/openclaw/pull/196942)).
- ✅ **Security & compliance**: 
  - Refactored delegated configuration approvals to prevent unintended local command execution ([#196760](https://github.com/openclaw/openclaw/pull/196760)).
  - Prevented OAuth headers from leaking OpenClaw version info to xAI/Grok API ([#147728](https://github.com/openclaw/openclaw/issues/147728)).

---

### **4. Community Hot Topics**  
The most active discussions center around **session state integrity**, **model fallback reliability**, and **authentication drift**:

| Issue | Comments | Severity | Link |
|------|--------|---------|------|
| [#150635](https://github.com/openclaw/openclaw/issues/150635) | 27 | 🦞 Diamond Lobster (Critical) | Dreaming deep phase never promotes due to nightly recall eviction |
| [#193962](https://github.com/openclaw/openclaw/issues/193962) | 26 | 🦞 Diamond Lobster (UX Release Blocker) | Update failure due to probe timeout in `package-update` — fix unreachable |
| [#154572](https://github.com/openclaw/openclaw/issues/154572) | 16 | 🦞 Diamond Lobster | `sessions_spawn` fails with `SessionTranscriptWriterClaimReboundError` on `claude-cli` |
| [#170928](https://github.com/openclaw/openclaw/issues/170928) | 10 | 🦪 Silver Shellfish | Windows gateway takes 7–9 min to start post-2026.10.2 update |

> 🔍 **Underlying Need**: Users report persistent instability in long-running sessions, especially across OS boundaries and model providers. There is growing demand for **predictable session lifecycles** and **transparent error recovery**.

---

### **5. Bugs & Stability**  
Top-tier stability concerns involve **crash loops**, **data loss**, and **silent state corruption**:

| Bug | Severity | Status | Fix PR? | Link |
|-----|----------|--------|--------|------|
| [#181459](https://github.com/openclaw/openclaw/issues/181459) | 🦪 Silver Shellfish | Open | ❌ No PR yet | 3.5GB agent DB causes startup crash (`commit checkpoint expired`) |
| [#194403](https://github.com/openclaw/openclaw/issues/194403) | 🦪 Silver Shellfish | Open | ❌ No PR yet | Helper-completion continuations block follow-ups despite "stopped" status |
| [#194664](https://github.com/openclaw/openclaw/issues/194664) | 🦪 Silver Shellfish | Open | ❌ No PR yet | Codex protected continuation blocks all future inputs |
| [#151962](https://github.com/openclaw/openclaw/issues/151962) | 🦪 Silver Shellfish | Open | ❌ No PR yet | Phantom user messages appear in UI but not in transcript |
| [#155271](https://github.com/openclaw/openclaw/issues/155271) | 🦞 Diamond Lobster | Open | ❌ No PR yet | Voice call hangs before farewell audio finishes |

> ⚠️ **Pattern**: Several high-severity issues stem from **asynchronous state race conditions**, **improper cleanup of ephemeral resources**, and **incomplete transaction rollbacks**.

---

### **6. Feature Requests & Roadmap Signals**  
User-driven feature requests reveal emerging priorities for **control**, **transparency**, and **accessibility**:

| Request | Priority | Use Case | Predicted Inclusion |
|-------|---------|--------|------------------|
| [#138366](https://github.com/openclaw/openclaw/issues/138366) | P2 | Let memory plugin own Dreams tab UI | Likely in v2026.12 |
| [#138990](https://github.com/openclaw/openclaw/issues/138990) | P2 | Enforceable checkpoints for long runs | High likelihood in next minor |
| [#155039](https://github.com/openclaw/openclaw/issues/155039) | P2 | Manual drag-and-drop agent ordering in sidebar | Possible in v2026.12 |
| [#154524](https://github.com/openclaw/openclaw/issues/154524) | P2 | Expose chat messages as headings for screen readers | Accessibility push → v2026.12 |
| [#192009](https://github.com/openclaw/openclaw/issues/192009) | P2 | Explicit cross-provider cron fallback authorization | Enterprise-focused → v2026.12 |

> 💡 **Trend**: Users want **greater ownership over session flow**, **auditable state transitions**, and **inclusive design**—signaling maturity beyond basic automation.

---

### **7. User Feedback Summary**  
Real user pain points reflect deep operational use cases:

- **Long-lived sessions (>13 days)** exhibit silent data loss and phantom messages ([#151962](https://github.com/openclaw/openclaw/issues/151962)), indicating stress on memory and state management.
- **Windows gateways** suffer from unexplained startup delays and intermittent unresponsiveness ([#170928](https://github.com/openclaw/openclaw/issues/170928)), affecting enterprise deployments.
- **CLI backend rendering issues** cause duplicated responses ([#123792](https://github.com/openclaw/openclaw/issues/123792)), reducing trust in output fidelity.
- **Model fallback chains** fail silently when rate-limited, blocking entire workflows ([#141115](https://github.com/openclaw/openclaw/issues/141115)), requiring manual intervention.

> ✅ **Satisfaction indicators**: Users appreciate granular control over privacy (e.g., private task results), and the ability to customize tool rosters and memory behavior.

---

### **8. Backlog Watch**  
Critical Issues with **no active fix PRs** and **long-standing unresolved status** require maintainer attention:

| Issue | Age | Comments | Severity | Maintainer Attention Needed? |
|------|-----|--------|----------|----------------------------|
| [#150635](https://github.com/openclaw/openclaw/issues/150635) | 2026-09-17 | 27 | 🦞 Diamond Lobster | ✅ Yes — blocks dreaming promotion |
| [#193962](https://github.com/openclaw/openclaw/issues/193962) | 2026-11-06 | 26 | 🦞 Diamond Lobster | ✅ Yes — prevents update path |
| [#181459](https://github.com/openclaw/openclaw/issues/181459) | 2026-10-23 | 9 | 🦪 Silver Shellfish | ✅ Yes — startup crash risk |
| [#194403](https://github.com/openclaw/openclaw/issues/194403) | 2026-11-07 | 7 | 🦪 Silver Shellfish | ✅ Yes — blocks ongoing work |
| [#151962](https://github.com/openclaw/openclaw/issues/151962) | 2026-09-18 | 15 | 🦪 Silver Shellfish | ✅ Yes — data integrity issue |

> 📌 **Recommendation**: Prioritize triage and assign maintainers to these top 5 high-impact, unresolved issues to prevent further degradation in production environments.

---

**✅ Project Health Assessment**: **High activity, strong community input, but urgent need for stabilizing core session and state integrity.**  
**🔮 Next Steps**: Focus on resolving diamond lobster bugs, enabling graceful fallbacks, and improving long-session resilience ahead of v2026.12.

---

## Cross-Ecosystem Comparison

# **Cross-Project Comparison Report: Personal AI Agent Ecosystem – 2026-11-11**

---

### **1. Ecosystem Overview**  
The open-source personal AI assistant and agent ecosystem in Q4 2026 is characterized by rapid evolution, increasing operational complexity, and a clear shift toward enterprise-grade reliability and security. Projects are moving beyond basic automation to support long-running, multi-agent workflows with cross-provider resilience, auditability, and user control. A growing emphasis on session integrity, data consistency, and platform interoperability reflects maturing use cases across development, DevOps, and knowledge management. The landscape is highly active, with strong community engagement and feature velocity—particularly in plugin ecosystems, UI/UX polish, and cross-platform stability.

---

### **2. Activity Comparison**

| Project        | Issues (24h) | PRs (24h) | Release Status       | Health Score (1–5) |
|----------------|--------------|-----------|------------------------|--------------------|
| **OpenClaw**   | 289          | 500       | v2026.11.3 (latest)    | ⭐⭐⭐⭐⭐ (5.0)       |
| **Hermes Agent** | 50           | 50        | None (pending patch)   | ⭐⭐⭐⭐☆ (4.2)       |
| **QwenPaw**    | 14           | 39        | v2.3.0-beta.4          | ⭐⭐⭐⭐☆ (4.0)       |
| **ZeroClaw**   | 16           | 50        | v0.9.0 (merged, not released) | ⭐⭐⭐☆☆ (3.8)     |
| **IronClaw**   | 0            | 0         | No activity            | ⭐☆☆☆☆ (1.0)       |

> *Health Score: Based on release cadence, issue/PR volume, stability of core features, and backlog triage urgency.*

---

### **3. OpenClaw's Position**  
OpenClaw stands as the most mature and operationally robust project in the ecosystem, with the highest contributor engagement and a clear focus on production-grade stability. Its technical approach centers on **session state integrity**, **privacy-by-design** (e.g., private task results), and **enterprise gateway resilience**, setting it apart from more experimental peers. Compared to Hermes Agent’s autonomy-heavy model or QwenPaw’s desktop-first design, OpenClaw emphasizes predictable, auditable agent lifecycles—ideal for mission-critical workflows. Community size appears largest, evidenced by high comment volumes on critical issues and consistent PR throughput. It leads in both technical depth and real-world deployment readiness.

---

### **4. Shared Technical Focus Areas**  
Across projects, several recurring technical challenges have emerged, indicating shared pain points in the agent ecosystem:

- **Session & State Integrity**:  
  - *OpenClaw* ([#150635](https://github.com/openclaw/openclaw/issues/150635), [#151962](https://github.com/openclaw/openclaw/issues/151962)) and *Hermes Agent* ([#152365](https://github.com/nousresearch/hermes-agent/issues/152365)) report silent data loss and phantom messages in long-running sessions.
  - *ZeroClaw* addresses similar concerns via PRs like [#11911](https://github.com/zeroclaw-labs/zeroclaw/pull/11911) on result persistence.

- **Authentication & Security Gaps**:  
  - *Hermes Agent*’s `smart` approval bypassing file safety checks ([#141346](https://github.com/nousresearch/hermes-agent/issues/141346)) and *ZeroClaw*’s limited account startup crashes ([#12357](https://github.com/zeroclaw-labs/zeroclaw/issues/12357)) highlight risks in privilege handling.

- **Cross-Platform Stability**:  
  - *QwenPaw* (Safari rendering issues, Windows path truncation) and *Hermes Agent* (WeChat voice message hangs) show persistent UI and integration bugs across OS and messaging platforms.

- **Model Fallback & Provider Resilience**:  
  - *OpenClaw* and *Hermes Agent* both face silent failures when rate-limited providers block workflow progression.

> 🔍 **Pattern**: The industry is converging on **stateful, resilient, and secure agent orchestration**—not just smart agents, but reliable ones.

---

### **5. Differentiation Analysis**

| Project        | Feature Focus                         | Target Users                     | Technical Architecture               |
|----------------|---------------------------------------|----------------------------------|--------------------------------------|
| **OpenClaw**   | Session integrity, privacy, enterprise gateways | DevOps, research, compliance teams | Multi-agent hierarchy, stateful pipelines |
| **Hermes Agent** | Plugin-driven autonomy, decision auditability | Creative professionals, hybrid workflows | Decision ledger, proactive governors |
| **QwenPaw**    | Local-first, desktop stability, channel fidelity | Developers, local AI users | Desktop-packaged runtime, plugin dependency manager |
| **ZeroClaw**   | Secure messaging, credential vaults, Rust-native components | Privacy-focused, infrastructure teams | XMPP/Omemo, SSH reuse, TLS relay |
| **IronClaw**   | — (no activity)                       | —                                | —                                    |

> 📌 **Key Differentiator**: OpenClaw prioritizes **predictable, recoverable workflows**; Hermes Agent pushes **autonomous execution with oversight**; QwenPaw focuses on **local execution and desktop UX**; ZeroClaw targets **secure, low-level agent communication**.

---

### **6. Community Momentum & Maturity**

- **High-Momentum Projects (Rapid Iteration)**:
  - **OpenClaw** and **Hermes Agent**: 50+ PRs/issue updates daily, with frequent bug fixes and feature enhancements. Both are actively shaping their roadmaps through community feedback.
  - **QwenPaw**: Strong momentum in beta stabilization, with focused improvements in desktop build reliability and cross-browser compatibility.

- **Stabilizing Projects (Feature Lockdown / Pre-Release Phase)**:
  - **ZeroClaw**: Now entering final phase before v0.9.0 with strong internal coordination; no new releases yet, but high-quality PRs focused on foundational stability.
  - **IronClaw**: Complete stagnation—no activity in last 24 hours. Risk of abandonment or reorganization.

> ✅ **Maturity Signal**: OpenClaw and Hermes Agent are at the **"production-ready" stage**, while QwenPaw and ZeroClaw are in **"stabilization-to-release" mode**.

---

### **7. Trend Signals**  
Based on community feedback and PR patterns, key trends emerging for AI agent developers include:

1. **Shift to Predictable Workflows**: Users demand **auditable, recoverable sessions**—not just “smart” agents. This signals a move from novelty to operational reliability.
2. **Security-First Autonomy**: High concern over automated actions (e.g., file deletion, AppleScript) indicates that **trust must be earned via granular controls**, not assumed.
3. **Local-First + Cross-Platform Consistency**: Desktop stability and Safari/Windows compatibility are now non-negotiable for user adoption.
4. **Plugin Ecosystem Maturity**: Projects are expanding beyond tools to **full-stack integrations** (e.g., `webgraft`, `IDEA CUT`, `deck-diff`)—indicating a need for composability in complex workflows.
5. **Enterprise Readiness Signals**: Features like **enforceable checkpoints**, **scoped secrets**, and **shared auth homes** point to growing demand for team-based, compliant agent deployments.

> 💡 **Strategic Implication**: For developers, success will depend less on model intelligence and more on **systemic reliability, transparency, and security posture**—the next frontier in agent software engineering.

---  
**Prepared for:** Technical decision-makers, open-source maintainers, and AI agent developers  
**Date:** 2026-11-11

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# **Hermes Agent Project Digest – 2026-11-11**

---

### **1. Today's Overview**  
The Hermes Agent project remains highly active with a robust pace of development: **50 issues and 50 pull requests updated in the last 24 hours**, indicating sustained momentum across core components, platforms, and integrations. Activity is concentrated in stability fixes (especially around session state, gateway liveness, and platform-specific crashes), security hardening, and UI/UX refinements for desktop and TUI. While no new releases have been published, several critical PRs are nearing merge, suggesting an imminent update cycle. The community continues to report high-severity risks related to data loss, permission bypasses, and irreversible system actions—highlighting ongoing challenges in safety-critical workflows.

---

### **2. Releases**  
❌ **No new releases** were published today.  
*Note: The project maintains a rapid iteration cadence, but release tracking remains absent as of this date. Maintainers may be preparing a patch release for urgent stability fixes.*

---

### **3. Project Progress**  
✅ **Merged/Closed PRs (Today):**  
- **PR #152766** ([Update Decision Ledger to v0.3.2](https://github.com/nousresearch/hermes-agent/pull/152766)) – Adds a read-only Decision Board to the dashboard for auditability and human review.  
- **PR #152907** ([Add Whiteboard plugin](https://github.com/nousresearch/hermes-agent/pull/152907)) – Introduces an agentic whiteboard tool for visual planning and collaboration.  
- **PR #152416** ([Register proactive-agent governor](https://github.com/nousresearch/hermes-agent/pull/152416)) – Enables turn governance and subagent supervision for long-running conversations.  
- **PR #152705** ([Bump second-key to 0.2.0](https://github.com/nousresearch/hermes-agent/pull/152705)) – Updates a Plugin Hackathon entry to latest version.  

🔧 **Key Advancements:**  
- **Plugin ecosystem expansion**: Three new plugins added to catalog: `deck-diff`, `IDEA CUT`, and `webgraft`, signaling growing focus on content integrity, creative production, and source transparency.  
- **Desktop UX improvements**: PR #152168 introduces a redesigned agent workspace with classic recovery mode, enhancing user control over layout and workflow persistence.

---

### **4. Community Hot Topics**  
🔥 **Top Issues by Comment Count & Severity:**  
1. **[Issue #141346](https://github.com/nousresearch/hermes-agent/issues/141346)** – *Security: Smart approval auto-approved `osascript` commands that closed unsaved PowerPoint decks and deleted exports.*  
   - **39 comments**, zero upvotes — highlights **critical trust failure** in autonomous tool execution. Users demand stricter guardrails before system-level actions.  
   - **Root cause**: Default `smart` approval mode triggered unattended macOS automation without confirming file state or user intent.  
   - **Urgency**: High-risk data loss; requires immediate fix or opt-in configuration change.

2. **[Issue #137838](https://github.com/nousresearch/hermes-agent/issues/137838)** – *macOS gateway exits 75 after missed liveness probes; root cause unclear.*  
   - **11 comments**, 1 👍 – Persistent instability in production deployments.  
   - Indicates **gateway resilience gaps** under load or network jitter; users rely on stable uptime for mission-critical agents.

3. **[PR #153061](https://github.com/nousresearch/hermes-agent/pull/153061)** – *Fix Slack forwarded message delivery.*  
   - **1 comment**, 0 👍 – Though minor, it addresses a common user pain point: forwarded messages vanishing from agent input.  
   - Reflects growing reliance on **Slack integration** and need for full message fidelity.

💡 **Underlying Needs:**  
- **Trust in autonomy**: Users want granular control over dangerous actions (e.g., `osascript`, file deletion).  
- **Platform reliability**: Stability across Telegram, WeChat, Slack, and Matrix is paramount.  
- **Message integrity**: No one wants lost or duplicated messages — especially in enterprise workflows.

---

### **5. Bugs & Stability**  
🚨 **High-Priority Bugs (P1–P2) Reported Today:**  
| Issue | Description | Severity | Fix PR? |
|------|-------------|----------|--------|
| [#152365](https://github.com/nousresearch/hermes-agent/issues/152365) | Weixin voice message (text="", audio/silk) leaves session stuck at "started" → messages silently dropped | P2 | ❌ No fix yet |
| [#152557](https://github.com/nousresearch/hermes-agent/issues/152557) | `write_file` blocked 458s, idle-reaped mid-call — outside GW-60 in-flight guard | P2 | ⚠️ Partial fix in #144597, but not fully resolved |
| [#151727](https://github.com/nousresearch/hermes-agent/issues/151727) | Bots roster times out every 5s due to full table re-sort on each poll | P2 | ❌ No fix yet |
| [#148846](https://github.com/nousresearch/hermes-agent/issues/148846) | Desktop queued prompts lose Edit/Steer controls — only Delete renders | P2 | ❌ No fix yet |

⚠️ **Critical Security Risks:**  
- **[#131564](https://github.com/nousresearch/hermes-agent/issues/131564)** & **[#132192](https://github.com/nousresearch/hermes-agent/issues/132192)** – Security scanner bypasses via malformed UTF-8 and docstring miscount. Both classified as **high severity**; require immediate patching.

---

### **6. Feature Requests & Roadmap Signals**  
📌 **Emerging Trends in User Requests:**  
- **Path-scoped file access** ([#92424](https://github.com/nousresearch/hermes-agent/issues/92424)): Users want to restrict agents to specific folders — key for multi-profile, low-trust use cases.  
- **Shared auth home for profiled workers** ([#29530](https://github.com/nousresearch/hermes-agent/issues/29530)): Avoid split-brain OAuth states across profiles — essential for enterprise scalability.  
- **Persistent session identity post-rename** ([#146522](https://github.com/nousresearch/hermes-agent/issues/146522)): Rename should not break legacy session references — shows need for backward compatibility.  
- **Better TUI interaction** ([#153057](https://github.com/nousresearch/hermes-agent/pull/153057)): Clickable links and API key guidance improve usability in terminal environments.

🔮 **Predicted Next Version Features:**  
- Enhanced **security sandboxing** (file path restrictions, approval gate enhancements).  
- **Profile-aware session management** with persistent identities and shared auth stores.  
- **Improved message delivery fidelity** across all platforms (Telegram, WeChat, Slack, etc.).  
- **Desktop UX overhaul** including redesigned workspace and better queue control.

---

### **7. User Feedback Summary**  
💬 **Real Pain Points Extracted from Issues & PRs:**  
- **“I lost my unsaved PowerPoint deck because the agent auto-ran `osascript` without asking.”** – A clear case of **autonomy vs. safety conflict**.  
- **“My WeChat voice message got stuck forever — no reply, no error.”** – Indicates **silent failures** in messaging pipelines.  
- **“I can’t edit or steer a queued prompt — only delete it.”** – Frustration with **limited interactivity** in desktop interface.  
- **“The `hermes doctor` told me to set `API_KEY` but named the wrong env var.”** – Confusion caused by **poor diagnostic feedback**.  
- **“Why does the desktop show the same avatar for every bot?”** – Visual inconsistency undermines **identity clarity** in group chats.

✅ **Positive Signals:**  
- High engagement in **plugin ecosystem** (Hackathon entries, catalog additions).  
- Active participation in **TUI improvements** and **cross-platform bug reporting**.

---

### **8. Backlog Watch**  
🔍 **Long-Unanswered Critical Issues Needing Attention:**  
- **[Issue #141346](https://github.com/nousresearch/hermes-agent/issues/141346)** – *Smart approval auto-approved destructive AppleScript commands.*  
  - **Age:** 13 days | **Comments:** 39 | **Severity:** P3, type/security  
  - **Status:** Open, no assigned maintainer, no fix PR. **High risk of real-world damage.**  
  - **Action Required:** Immediate triage and mitigation (e.g., opt-in `smart` mode, pre-check for open files).

- **[Issue #125695](https://github.com/nousresearch/hermes-agent/issues/125695)** – *One model_entitlement cooldown disables vision, smart approvals, and titles for a year.*  
  - **Age:** 45 days | **Comments:** 10 | **Severity:** P2  
  - **Impact:** Breaks multiple features across all tools using the same credential.  
  - **Action Required:** Refactor scope logic to isolate entitlements per task, not global.

- **[Issue #132820](https://github.com/nousresearch/hermes-agent/issues/132820)** – *Matrix E2EE build fails during `hermes update` on Linux, cannot be opted out.*  
  - **Age:** 38 days | **Comments:** 8 | **Severity:** P1  
  - **Impact:** Blocks updates for users relying on Matrix integration.  
  - **Action Required:** Add opt-out flag or fallback to pure Python implementation.

---

> ✅ **Final Assessment:**  
Hermes Agent is in a **high-growth, high-risk phase** — rapid innovation in plugins and UX is being offset by unresolved stability and security concerns. The team must prioritize **safety-first fixes** (especially around automated actions and file operations) while continuing to expand the plugin ecosystem. Without addressing top-tier bugs like #141346 and #125695, user trust will erode despite feature richness.

---  
📊 **Data Source**: [Hermes Agent GitHub Repository](https://github.com/nousresearch/hermes-agent)  
📅 **Digest Date**: 2026-11-11

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# **QwenPaw Project Digest – 2026-11-11**

---

### **1. Today's Overview**  
QwenPaw remains highly active with a robust development pace: **39 pull requests** and **14 issues** updated in the past 24 hours, indicating strong community engagement and ongoing refinement. The release of **v2.3.0-beta.4** marks a key milestone in stabilizing core plugin and channel functionality, particularly for desktop builds. Significant progress is being made in fixing startup failures, dependency handling, and cross-browser compatibility—especially in Safari and Windows environments. Overall project health is strong, with a balanced mix of bug fixes, performance improvements, and feature enhancements.

---

### **2. Releases**  
✅ **New Release**: [`v2.3.0-beta.4`](https://github.com/agentscope-ai/QwenPaw/releases/tag/v2.3.0-beta.4)  

#### **What’s Changed**
- **Fix (channels)**: Preserves plugin dependencies across updates and resolves channel startup failures by @Luohh5 ([#8653](https://github.com/agentscope-ai/QwenPaw/pull/8653))  
- **Fix (plugins)**: Recovers from interrupted additive dependency installations, preventing silent startup failures ([#8659](https://github.com/agentscope-ai/QwenPaw/pull/8659))  

> 📌 **Migration Note**: Users on `beta.1`–`beta.3` should upgrade to `beta.4` to resolve persistent plugin activation and dependency corruption issues, especially in packaged desktop builds. No breaking changes reported.

---

### **3. Project Progress**  
**Merged/Closed PRs (Today)**:
- [#8653](https://github.com/agentscope-ai/QwenPaw/pull/8653): Resolved plugin dependency persistence and channel startup crashes — critical for desktop stability.
- [#8659](https://github.com/agentscope-ai/QwenPaw/pull/8659): Fixed recovery logic for partially failed plugin installs — prevents blocking future launches.
- [#8644](https://github.com/agentscope-ai/QwenPaw/issues/8644): Closed — resolved `WORKER_START_FAILED` due to missing `supported_tags` in bundled builds.
- [#8643](https://github.com/agentscope-ai/QwenPaw/issues/8643): Closed — passed installation verification for `beta.3`.
- [#8667](https://github.com/agentscope-ai/QwenPaw/issues/8667): Closed — passed beta release verification for `beta.4`.

🔹 **Key Advancement**: Desktop plugin runtime reliability has seen major improvements, addressing long-standing startup and dependency issues.

---

### **4. Community Hot Topics**  
🔥 **Most Active Issues & PRs (by comments/reactions)**:

| Issue/PR | Title | Activity | Link |
|--------|------|---------|------|
| [#8669](https://github.com/agentscope-ai/QwenPaw/issues/8669) | Feishu channel startup race condition (`CHANNEL_NOT_CONNECTED`) | 2 comments, high impact on desktop users | [Issue #8669](https://github.com/agentscope-ai/QwenPaw/issues/8669) |
| [#8663](https://github.com/agentscope-ai/QwenPaw/issues/8663) | Safari: Message queue action buttons not rendered | 1 comment, UX blocker for Mac users | [Issue #8663](https://github.com/agentscope-ai/QwenPaw/issues/8663) |
| [#8660](https://github.com/agentscope-ai/QwenPaw/pull/8660) | Fix console terminal checks gated on auth | 1 PR, improves security & usability | [PR #8660](https://github.com/agentscope-ai/QwenPaw/pull/8660) |
| [#8670](https://github.com/agentscope-ai/QwenPaw/pull/8670) | Fix Safari message queue icons | 1 PR, direct fix for Safari rendering issue | [PR #8670](https://github.com/agentscope-ai/QwenPaw/pull/8670) |

🔍 **Underlying Needs**:  
- **Cross-browser consistency** (Safari) is emerging as a recurring pain point, especially in the Console UI.  
- **Desktop build stability**, particularly for enterprise-grade channels like Feishu and WeChat, remains a top concern.  
- **Authentication gating** on terminal features suggests growing demand for secure remote access workflows.

---

### **5. Bugs & Stability**  
⚠️ **Critical Bugs Reported Today** (Ranked by Severity):

| Bug | Description | Affected Platform(s) | Fix PR? | Priority |
|-----|-------------|----------------------|--------|----------|
| [#8655](https://github.com/agentscope-ai/QwenPaw/issues/8655) | Attachment path truncated to `C:\` on Windows (since `beta.1`) | Windows, local file uploads | ❌ No PR yet | 🔴 High |
| [#8669](https://github.com/agentscope-ai/QwenPaw/issues/8669) | Feishu channel fails on every startup (`CHANNEL_NOT_CONNECTED`) | Windows desktop, packaged builds | ❌ No fix yet | 🔴 High |
| [#8651](https://github.com/agentscope-ai/QwenPaw/issues/8651) | QQ C2C image-only messages silently dropped | QQ bot gateway, private chat | ❌ No fix yet | 🟡 Medium |
| [#8661](https://github.com/agentscope-ai/QwenPaw/issues/8661) | `/workspace/` links open SPA HTML instead of downloading files | Hub, all platforms | ✅ PR exists ([#8662](https://github.com/agentscope-ai/QwenPaw/pull/8662)) | 🟡 Medium |

> ⚠️ **Regression Alert**: `beta.1` introduced file path truncation bugs affecting Windows users — now confirmed in `beta.3`. Urgent fix needed.

---

### **6. Feature Requests & Roadmap Signals**  
🚀 **Top User-Requested Features**:

| Request | Summary | Status | Likely In Next Version? |
|-------|--------|--------|------------------------|
| [#7990](https://github.com/agentscope-ai/QwenPaw/issues/7990) | Add `thinking_param_style` for Aliyun Token Plan models | Open | ✅ Yes — aligns with v2.3 focus on model catalog completeness |
| [#8656](https://github.com/agentscope-ai/QwenPaw/issues/8656) | Request Mistral provider plugin inclusion | Open | ✅ Likely — supports OpenAI-compatible models; fits ecosystem expansion |
| [#8648](https://github.com/agentscope-ai/QwenPaw/pull/8648) | Add local engine/GPU/auto-update controls | In progress (PR #8648) | ✅ **Definitely in v2.3** — core to desktop user experience |
| [#8178](https://github.com/agentscope-ai/QwenPaw/pull/8178) | Nested skill directories | Under review | ✅ High probability — improves organization at scale |

> 💡 **Roadmap Signal**: QwenPaw is evolving into a **local-first, multi-engine AI agent platform** with growing support for **on-device models (llama.cpp, MLX)**, **enterprise channels (Feishu, DingTalk)**, and **cloud-native provider integration (Mistral, Aliyun)**.

---

### **7. User Feedback Summary**  
🗣️ **Real User Pain Points**:
- **Windows users**: File paths getting corrupted (`C:\`) during uploads — breaks image processing and tool use.
- **Mac users**: Safari renders UI elements incorrectly (message queue actions), reducing usability.
- **Remote deployments**: Lack of password login option hinders Docker/remote access (highlighted in PR #8664).
- **Model configurability**: Users want fine-grained control over "thinking level" for Aliyun models — currently hidden due to missing metadata.

💡 **Satisfaction Indicators**:
- Positive feedback implied in rapid adoption of `beta.4` post-release.
- High engagement in PR reviews and testing — users are actively involved in quality assurance.

---

### **8. Backlog Watch**  
📌 **Long-Unanswered Critical Issues Needing Attention**:

| Issue | Summary | Age | Priority |
|------|--------|-----|----------|
| [#8650](https://github.com/agentscope-ai/QwenPaw/issues/8650) | Visual inconsistency between "thinking" and "tool" blocks causes left-edge flicker | 2 days old | 🔴 High (UX degradation) |
| [#8652](https://github.com/agentscope-ai/QwenPaw/issues/8652) | Mission switches model but retains old one → task failure | 1 day old | 🔴 High (functional regression) |
| [#8669](https://github.com/agentscope-ai/QwenPaw/issues/8669) | Feishu channel startup race condition — fails every time | 1 day old | 🔴 High (blocking production use) |
| [#7990](https://github.com/agentscope-ai/QwenPaw/issues/7990) | Missing `thinking_param_style` for Aliyun models — hides UI controls | 7 weeks old | 🟡 Medium-high (feature gap) |

> ⏳ **Action Needed**: Maintainers should prioritize triaging these high-impact, user-facing issues — especially those affecting **core workflow stability** and **cross-platform consistency**.

---

✅ **Final Assessment**: QwenPaw is in a strong phase of stabilization and feature expansion. The shift toward **desktop-first, locally-run agents with rich plugin ecosystems** is accelerating. With timely fixes to startup and UI regressions, `v2.3.0` could become a major release milestone for both developers and end-users.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# **ZeroClaw Project Digest**  
**Date:** 2026-11-11  
**Repository:** [github.com/zeroclaw-labs/zeroclaw](https://github.com/zeroclaw-labs/zeroclaw)

---

### **1. Today's Overview**

ZeroClaw continues to demonstrate strong momentum with 50 pull requests and 16 issues updated in the last 24 hours, reflecting a high velocity of development and active community engagement. The project is in a critical phase ahead of v0.9.0, with multiple PRs focused on stabilizing core runtime behavior, enhancing security, and refining dashboard functionality. Despite no new releases, significant progress has been made in addressing long-standing bugs related to authentication, configuration handling, and agent communication—particularly around temperature parameter mismanagement and TLS relay buffering. The ecosystem remains healthy, with contributions from both maintainers and external developers.

---

### **2. Releases**

> ❌ **No new releases today or in the past 7 days.**

The last release was v0.8.7 (2026-10-15). A version bump to **v0.9.0** was merged via PR [#12377](https://github.com/zeroclaw-labs/zeroclaw/pull/12377) on 2026-11-10, but it has not yet been published. This indicates that the team is preparing for a coordinated release, likely bundling key fixes and features including:
- Security hardening (e.g., improved credential handling)
- Runtime stability improvements
- Dashboard enhancements (OpenRouter balance display, voice controls)

No breaking changes have been announced publicly; however, users should expect updates to model routing, temperature handling, and OAuth2 flows.

---

### **3. Project Progress**

✅ **Merged/Closed PRs (Today):**  
- [#12377](https://github.com/zeroclaw-labs/zeroclaw/pull/12377): Version bump to v0.9.0 – foundational for upcoming release.
- [#12367](https://github.com/zeroclaw-labs/zeroclaw/pull/12367): Fixes hard-coded `temperature = 0.0` in heartbeat and model-routing probe – resolves critical compatibility issue with models rejecting fixed temperature.
- [#12354](https://github.com/zeroclaw-labs/zeroclaw/pull/12354): Addresses TLS write flushing in `zerorelay`, preventing dashboard asset hangs.
- [#12347](https://github.com/zeroclaw-labs/zeroclaw/pull/12347): Prevents concurrent config file overwrites during `zerocode` startup.
- [#12326](https://github.com/zeroclaw-labs/zeroclaw/pull/12326): Fixes broken links to pruned docs versions by ensuring GitHub Pages has proper 404 handling.

🔧 **Key Features Advanced:**
- PR [#12269](https://github.com/zeroclaw-labs/zeroclaw/pull/12269): Adds paired-device voice settings and browser audio preferences — enables richer user interaction.
- PR [#11847](https://github.com/zeroclaw-labs/zeroclaw/pull/11847): Native Rust XMPP support with optional OMEMO — expands secure messaging capabilities.
- PR [#11869](https://github.com/zeroclaw-labs/zeroclaw/pull/11869): Opt-in SSH transport reuse per task — improves efficiency in long-running workflows.

---

### **4. Community Hot Topics**

🔥 **Most Active Issue:**  
- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) – *Runtime, dashboard sign-in and gateway delivery tracker*  
  - **Status:** Open, accepted, priority P2  
  - **Activity:** 16 comments, ongoing discussion about sequencing and gap closure for v0.9.0  
  - **Analysis:** This is the central roadmap tracker for post-v0.8.7 runtime and gateway work. It signals that the team is finalizing architectural integration points before release, especially around authentication flow and agent lifecycle management.

🔥 **Most Active PR:**  
- [#11911](https://github.com/zeroclaw-labs/zeroclaw/pull/11911) – *Preserve completed tool results and reuse bounded recovery*  
  - **Activity:** High comment volume, review follow-ups, and integration commits  
  - **Analysis:** Indicates deep focus on reliability and state consistency in multi-turn agent workflows. Users are concerned about data loss during cancellations or restarts.

💡 **Emerging User Need:**  
- Multiple issues (#12373, #12372, #12361) point to demand for **better control over agent behavior**, especially in multi-turn conversations and channel output formatting. Users want more granular control over how responses are delivered (e.g., draft mode, reasoning content injection), suggesting growing complexity in real-world use cases.

---

### **5. Bugs & Stability**

| Severity | Issue | Description | Fix PR |
|--------|-------|-------------|--------|
| S1 – Workflow Blocked | [#12354](https://github.com/zeroclaw-labs/zeroclaw/issues/12354) | `zerorelay` doesn’t flush TLS writes → dashboard assets hang until idle timeout | ✅ [#12354](https://github.com/zeroclaw-labs/zeroclaw/pull/12354) |
| S2 – Degraded Behavior | [#12366](https://github.com/zeroclaw-labs/zeroclaw/issues/12366) | Heartbeat and model probe hardcode `temperature=0.0` → fails on models rejecting it | ✅ [#12367](https://github.com/zeroclaw-labs/zeroclaw/pull/12367) |
| S2 – Degraded Behavior | [#12357](https://github.com/zeroclaw-labs/zeroclaw/issues/12357) | Limited accounts exit on startup due to fatal panics in config/logs panes | ✅ Not yet fixed (in backlog) |
| S2 – Degraded Behavior | [#12325](https://github.com/zeroclaw-labs/zeroclaw/issues/12325) | Docs canonical URLs rename stable release dir, breaking older links | ✅ Fixed in PR [#12325](https://github.com/zeroclaw-labs/zeroclaw/pull/12325) |
| S3 – Minor Issue | [#12324](https://github.com/zeroclaw-labs/zeroclaw/issues/12324) | rustdoc API tree and locale 404 pages indexable by bots | ✅ Fixed in PR [#12324](https://github.com/zeroclaw-labs/zeroclaw/pull/12324) |
| S3 – Minor Issue | [#12153](https://github.com/zeroclaw-labs/zeroclaw/issues/12153) | `install.sh` picks musl build on glibc hosts with musl package installed | ✅ Fixed in PR [#12153](https://github.com/zeroclaw-labs/zeroclaw/pull/12153) |

> 🔴 **Critical Note:** While many bugs are resolved, **S2-level issues like #12357 (startup crash for limited users)** remain open despite being reported and confirmed—this may affect usability for non-admin users in production environments.

---

### **6. Feature Requests & Roadmap Signals**

📌 **Top Feature Requests:**
- **[#12373](https://github.com/zeroclaw-labs/zeroclaw/issues/12373): Write-only WebUI credential management and scoped secret references**  
  - Requested by CastelloBrancoTecnologia — calls for operator-only secret vault with metadata listing, controlled references.  
  - **Prediction:** Likely to be included in v0.9.0 as part of enhanced security architecture.

- **[#12372](https://github.com/zeroclaw-labs/zeroclaw/issues/12372): Git channel draft_mode option (placeholder comment vs streaming)**  
  - Urgent need from developers using Git channels for code generation — desire for cleaner, less noisy feedback.  
  - **Prediction:** High chance of inclusion in v0.9.0 given its alignment with UX polish goals.

- **[#12361](https://github.com/zeroclaw-labs/zeroclaw/issues/12361): Multi-turn support for providers requiring `reasoning_content` (e.g., Reflection AI / Beam)**  
  - Highlights a growing gap in provider interoperability.  
  - **Prediction:** Will likely be addressed in v0.9.0 or v0.9.1 as part of broader model compatibility work.

> 📈 **Roadmap Signal:** The project is shifting toward **enterprise-grade security, observability, and workflow control**, with increasing focus on auditability, access restrictions, and cross-provider compatibility.

---

### **7. User Feedback Summary**

💬 **Real User Pain Points:**
- **Limited users cannot fully use the dashboard** — they can sign in but lack chat access ([#11984](https://github.com/zeroclaw-labs/zeroclaw/issues/11984)), creating friction for role-based deployments.
- **Configuration conflicts during concurrent edits** cause silent data loss ([#12347](https://github.com/zeroclaw-labs/zeroclaw/issues/12347)) — a major concern for shared environments.
- **Inconsistent agent behavior across providers** — e.g., temperature hardcoding breaks models like Reflection AI ([#12362](https://github.com/zeroclaw-labs/zeroclaw/issues/12362)), indicating a need for more adaptive inference layer.
- **Lack of control over output formatting in Git channels** — users want cleaner, non-streaming replies ([#12372](https://github.com/zeroclaw-labs/zeroclaw/issues/12372)).

🎯 **Satisfaction Indicators:**
- Positive sentiment around new XMPP support and SSH transport reuse — seen as valuable for advanced automation workflows.
- Appreciation for transparency in documentation improvements (e.g., OIDC device certificate explanation in [#12332](https://github.com/zeroclaw-labs/zeroclaw/issues/12332)).

---

### **8. Backlog Watch**

⚠️ **High-Priority Issues Requiring Maintainer Attention:**

- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432): *Runtime, dashboard sign-in and gateway delivery tracker*  
  - **Why:** Central to v0.9.0 readiness. Contains remaining gaps and acceptance evidence needed for release.  
  - **Status:** Open, accepted, no stale label.  
  - **Action Needed:** Finalize sequencing, assign owners, confirm completion criteria.

- [#12373](https://github.com/zeroclaw-labs/zeroclaw/issues/12373): *Write-only WebUI credential management*  
  - **Why:** Critical for secure, auditable secrets management. Already flagged as "no implementation PR" — needs scoping discussion.  
  - **Status:** Open, no assignee, low activity.  
  - **Action Needed:** Core Team must clarify ownership and scope before development begins.

- [#12357](https://github.com/zeroclaw-labs/zeroclaw/issues/12357): *Limited account exits at startup*  
  - **Why:** S2 severity, affects real-world usage. Still open despite being confirmed.  
  - **Status:** Closed in PR? No.  
  - **Action Needed:** Immediate triage and fix—likely a missing error-handling boundary in TUI pane initialization.

---

> ✅ **Final Assessment:** ZeroClaw is in a strong position heading into v0.9.0. The project shows excellent technical maturity, with consistent PRs, thoughtful issue tracking, and proactive dependency hygiene. However, some **critical stability bugs remain unpatched**, and **key feature decisions await maintainer input**—suggesting a need for tighter coordination ahead of release.  

**Project Health:** ⚠️ **Healthy with Risks** — Strong momentum, but attention required on high-severity bugs and backlog prioritization.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*