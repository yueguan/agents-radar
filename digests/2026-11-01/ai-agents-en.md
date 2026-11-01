# OpenClaw Ecosystem Digest 2026-11-01

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-11-01 01:26 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# **OpenClaw Project Digest – 2026-11-01**

---

### **1. Today's Overview**  
The OpenClaw project remains highly active with a surge in developer engagement: **500 issues and 500 pull requests updated in the last 24 hours**, indicating intense development momentum. Despite no new releases, the ecosystem is deeply engaged in addressing critical stability, security, and UX issues—particularly around session state integrity, memory management, and plugin behavior. The high volume of open issues (399 active) reflects ongoing challenges in production-grade reliability, especially in self-hosted environments. Overall, the project shows strong community participation but faces significant technical debt in core runtime stability.

---

### **2. Releases**  
❌ **No new releases** were published in the last 24 hours.  
- *Latest stable version:* `2026.10.8` (dc2523e)  
- *Latest beta:* `2026.10.10-beta.3`  
- *Note:* Several critical bugs (e.g., #187900, #184658) are reported on these versions, suggesting potential release blockers pending resolution.

---

### **3. Project Progress**  
✅ **Merged/Closed PRs (today):**  
- [#189106](https://github.com/openclaw/openclaw/pull/189106): *feat: retain repository disk authority through recovery* — Ensures disk ownership persists after restarts, preventing data loss during recovery.  
- [#189889](https://github.com/openclaw/openclaw/pull/189889): *fix(sessions): preserve explicit web search preferences* — Resolves silent preference loss during session patching.  
- [#189843](https://github.com/openclaw/openclaw/pull/189843): *perf(ui): reduce shared agent selector setup and focus work* — Improves UI responsiveness for large agent rosters.  
- [#189395](https://github.com/openclaw/openclaw/pull/189395): *feat(google): use Interactions API for Gemini Flash models* — Enables better performance and compatibility with newer Google models.  

🔹 These fixes address **core UX, stability, and integration improvements**, signaling progress in refining the control plane and model interoperability.

---

### **4. Community Hot Topics**  
🔥 **Top Issues by Comment Count & Severity:**  
1. **[Issue #160959](https://github.com/openclaw/openclaw/issues/160959)**: *Gateway blocks for minutes capturing large external plugins (P0, crash-loop)* — 66 comments; regression from 2026.9.6. Critical for users with complex plugin ecosystems.  
2. **[Issue #153426](https://github.com/openclaw/openclaw/issues/153426)**: *Curated `MEMORY.md` silently excluded forever after provenance failure (P0, diamond lobster)* — 15 comments; impacts user trust in memory persistence.  
3. **[Issue #177656](https://github.com/openclaw/openclaw/issues/177656)**: *Native host exec fails without child result (P1, silver shellfish)* — 22 comments; affects secure execution workflows.  
4. **[Issue #187900](https://github.com/openclaw/openclaw/issues/187900)**: *`doctor` hangs indefinitely at startup on Linux (P1, silver shellfish)* — 9 comments; prevents diagnostics and repair.  

💡 **Underlying Need:** Users demand **predictable, resilient runtime behavior** under load and edge conditions. Failures are often silent or irreversible, eroding confidence in long-term agent operation.

---

### **5. Bugs & Stability**  
🚨 **Critical Bugs Reported (P0/P1, High Impact):**  
- **#160959**: Plugin capture causes **minute-long event loop blockages** at startup and republish — *regression from 2026.9.6*. No fix PR yet.  
- **#153426**: `MEMORY.md` files become **permanently excluded** after untrusted provenance — no diagnostics, no recovery path. *UX release blocker.*  
- **#187900**: `openclaw doctor` **hangs indefinitely** on Linux hosts — renders diagnostics unusable.  
- **#177656**: Native host exec fails with `shared-state reader requires current integrity admission` — *security-critical, no fix.*  
- **#182369**: Gateway RSS grows **~0.1 GiB/h off-heap** — potential memory leak causing instability.  

🔧 **Fix PRs Exist For:**  
- ✅ #189106 (recovery authority retention)  
- ✅ #189889 (web search prefs)  
- ✅ #189843 (agent selector perf)  
- ✅ #189395 (Gemini Flash API)  

🚫 **No PRs linked to P0/P1 crashes or security flaws** — major concern for production users.

---

### **6. Feature Requests & Roadmap Signals**  
📈 **High-Value Feature Requests (User Demand Indicators):**  
- **#159858**: *Let idle worker isolates release memory on small hosts* — 8 comments. Urgent for low-resource deployments.  
- **#138366**: *Let memory slot owner drive Dreams page* — 10 comments. Indicates desire for **user-owned memory governance**.  
- **#150238**: *Add context budget/skip policy for Skill Workshop Review* — 7 comments. Reflects need for **scalable learning experiences**.  
- **#76493**: *Allow `SecretRef` in `mcp.servers[].env`* — 13 comments, 5 👍 — strong demand for **secure secret handling** in config.  

🔮 **Predicted Next Version Additions:**  
- Memory optimization for small hosts  
- Enhanced secret injection via `SecretRef`  
- User-controlled memory lifecycle (Dreams page control)  
- Better context-aware fallback policies  

---

### **7. User Feedback Summary**  
💬 **Real Pain Points from Issue Descriptions:**  
- **Silent failures with no diagnostics** (#153426, #166634): “No CLI, no recovery” — users feel helpless when agents go permanently unavailable.  
- **Unrecoverable state after crashes** (#166634): “UNAVAILABLE” on all key APIs post-recovery.  
- **UI misleading users** (#165421): Stale `maxTokens` display despite correct model context.  
- **Security practices blocked** (#76493): Operators can’t use `--ref-source env` due to schema restrictions.  
- **Long waits with no feedback** (#187900, #178359): Users report “idles for 15+ min with no output.”  

✅ **Satisfaction Signals:**  
- Many users appreciate the **extensive telemetry and diagnostic tools** (e.g., `doctor`, `health`, `status`).  
- The **plugin system and multi-agent support** are seen as powerful, but only if stable.

---

### **8. Backlog Watch**  
⚠️ **Critical Long-Unanswered Issues Needing Maintainer Attention:**  
- **[Issue #153426](https://github.com/openclaw/openclaw/issues/153426)**: *Curated memory roots silently dropped forever* — **P0, diamond lobster, 15 comments, no fix PR** — fundamental trust issue.  
- **[Issue #160959](https://github.com/openclaw/openclaw/issues/160959)**: *Plugin capture blocks event loop for minutes* — **P0, 66 comments, no fix PR** — major usability blocker.  
- **[Issue #177656](https://github.com/openclaw/openclaw/issues/177656)**: *Native host exec fails without child result* — **P1, 22 comments, no fix PR** — security risk.  
- **[PR #129388](https://github.com/openclaw/openclaw/pull/129388)**: *Context-pressure-aware continuation* — **XL feature, needs proof, 11 months old** — flagship AI autonomy signal.  
- **[Issue #112313](https://github.com/openclaw/openclaw/issues/112313)**: *Dead-lettered delivery queue entries permanent* — **P2, 10 comments, no action** — breaks message durability.  

🔍 **Call to Action:** Maintainers must prioritize **P0 stability bugs** and **high-comment features** to prevent further erosion of user trust.

---

**📌 Final Assessment:**  
OpenClaw is **technically ambitious and actively developed**, but **critical stability and UX issues remain unresolved**. With no new releases and high-priority bugs lacking PRs, the project risks slowing adoption despite strong community interest. Immediate triage of top-tier issues is essential to maintain credibility in the personal AI assistant space.

---

## Cross-Ecosystem Comparison

# **Cross-Project Comparison Report: Personal AI Agent Ecosystem – 2026-11-01**

---

### **1. Ecosystem Overview**  
The open-source personal AI assistant and agent ecosystem in Q4 2026 is marked by **rapid iteration, growing production demands, and increasing focus on stability, security, and UX resilience**. Projects are transitioning from early experimentation toward real-world deployment, with strong community engagement across core frameworks like OpenClaw, Hermes Agent, and QwenPaw. While innovation remains high—especially in multi-agent orchestration, mobile access, and context-aware reasoning—**critical technical debt and unresolved P0/P1 issues threaten adoption momentum**, particularly in self-hosted and enterprise environments. The landscape reflects a maturing field where reliability and trust now outweigh novelty.

---

### **2. Activity Comparison**

| Project         | Issues (Last 24h) | PRs (Last 24h) | Releases (Last 24h) | Health Score (1–5) | Notes |
|----------------|-------------------|----------------|----------------------|--------------------|-------|
| **OpenClaw**   | 500               | 500            | ❌ No                | ⚠️ 2.8             | High activity but critical bugs unaddressed |
| **Hermes Agent** | 50              | 50             | ❌ No                | 🟡 3.2             | Focused on stability & security fixes |
| **QwenPaw**    | 9                 | 14             | ❌ No                | 🟡 3.5             | Strong contributor growth; edge-case focus |
| **ZeroClaw**   | 24                | 50             | ❌ No                | ✅ 4.1             | High-quality security & CLI improvements |

> **Health Score Definition**: Based on release cadence, bug severity resolution rate, UX feedback quality, and backlog triage urgency (5 = optimal, 1 = at risk).

---

### **3. OpenClaw's Position**  
OpenClaw stands out as the **most technically ambitious and developer-intensive project**, with the highest volume of contributions and issue activity. Its strength lies in **deep plugin extensibility, multi-agent coordination, and model interoperability** (e.g., Gemini Flash integration). However, this comes at the cost of **significant technical debt**: unresolved P0/P1 crashes, silent state corruption, and lack of diagnostics for recovery. Compared to peers:
- **vs. ZeroClaw**: More mature in agent networking but weaker in security-by-default design.
- **vs. Hermes Agent**: Larger community and broader feature scope, but less focus on mobile or automation polish.
- **vs. QwenPaw**: Far more active in development, but less consistent in release hygiene and UX refinement.

Its community is large and vocal, but **trust erosion due to unrecoverable failures** threatens long-term sustainability if stability isn’t prioritized.

---

### **4. Shared Technical Focus Areas**  
Across all projects, recurring themes indicate **industry-wide maturity signals**:

| Focus Area                     | Projects Involved             | Specific Needs |
|-------------------------------|-------------------------------|----------------|
| **Session Persistence & Recovery** | OpenClaw, Hermes Agent, ZeroClaw | Prevent silent data loss; enable resume via CLI (`/load-session`), avoid `UNAVAILABLE` states |
| **Security Hardening**        | OpenClaw, ZeroClaw, Hermes Agent | Fix credential leaks (e.g., MCP stdio inheriting secrets), prevent approval bypasses, enforce sandbox boundaries |
| **Mobile & Real-Time UX**     | Hermes Agent, QwenPaw          | Fix Android crashes, stuttering, missing notifications; improve voice calling readiness |
| **Legacy Compatibility & Backward Support** | QwenPaw, OpenClaw | Maintain legacy MCP handshakes, support older server configurations |
| **Deployment Flexibility**    | QwenPaw, ZeroClaw              | Support non-root Docker, fnOS sandboxes, proxy setups, restricted environments |

These represent **non-negotiable requirements** for production-grade agent systems.

---

### **5. Differentiation Analysis**

| Dimension               | OpenClaw                            | Hermes Agent                         | QwenPaw                              | ZeroClaw                             |
|-------------------------|-------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|
| **Feature Focus**       | Multi-agent orchestration, plugins, model diversity | Mobile-first, automation, secure execution | Channel-native streaming (Feishu/QQ), Chinese ecosystem | Security, CLI extensibility, RAG |
| **Target Users**        | DevOps, power users, researchers    | Everyday users, remote teams         | Enterprise, Chinese-speaking orgs    | System architects, security-focused devs |
| **Architecture**        | Highly modular, plugin-driven       | Gateway + mobile app hybrid          | Channel-integrated, proxy-aware      | Wasm-based, headless-first, policy-enforced |
| **Key Strength**        | Extensibility & model agnosticism   | UX polish & mobile readiness         | Cross-channel reliability            | Security posture & dependency hygiene |
| **Weakness**            | Unreliable runtime, poor diagnostics | Mobile fragility, session bugs       | UX degradation, outdated UI          | Slow release cycle, RFC bottlenecks |

---

### **6. Community Momentum & Maturity**  

- **Rapid Iteration Tier** (High velocity, high risk):  
  - **OpenClaw** – Highest PR/issue volume; intense development but lagging in release discipline.  
  - **ZeroClaw** – High-quality PRs focused on security and tooling; stable progress despite no releases.

- **Stabilization Phase** (Focus on reliability, user trust):  
  - **Hermes Agent** – Shifting from features to fixing crashes, authentication, and mobile stability.  
  - **QwenPaw** – Active fix pipeline; growing first-time contributors suggest maturing adoption.

- **Inactive/Slowed** (No recent updates):  
  - **IronClaw** – Likely abandoned or in maintenance mode.

> **Maturity Trend**: The ecosystem is moving from “build fast” to “run safely.” Projects that prioritize **security, diagnostics, and backward compatibility** are gaining credibility.

---

### **7. Trend Signals**  
Based on community feedback and PR patterns, key industry trends emerge:

1. **Trust > Innovation**: Users increasingly demand **predictable, recoverable behavior** over new features. Silent failures (e.g., `MEMORY.md` lost forever) are catastrophic for adoption.
2. **Security Is Non-Negotiable**: Credential exposure risks (e.g., agents reading daemon configs) are being flagged as S1/S2 issues—indicating that **zero-trust design is now expected** in agent platforms.
3. **Mobile Access Is a Product Requirement**: Demand for native iOS/Android apps with voice calling (Hermes Agent) signals that **AI assistants must be hands-free and ambient**, not just desktop tools.
4. **UX Polishing Is Critical for Adoption**: Sluggish interfaces, copy-paste lag, and inconsistent responses (QwenPaw) are common pain points—highlighting that **polished UX is a differentiator**, not a luxury.
5. **Enterprise Readiness Requires Contextual Memory**: The RFC for RAG in ZeroClaw and memory governance requests in OpenClaw show that **agents must reason over user-owned documents**, not just chat history.

> 💡 **Value for Developers**: Build with **diagnostics, recovery paths, and secure defaults** from day one. Prioritize stability and user trust—these are now the true gatekeepers of adoption.

---

**📌 Final Insight**:  
The personal AI agent ecosystem is entering its **production phase**. Success will go to projects that balance ambition with reliability—those that ship stable, secure, and usable systems, not just the most feature-rich ones. OpenClaw leads in scale, but ZeroClaw and Hermes Agent may lead in trust. The future belongs to **resilient, observable, and accountable agents**—not just intelligent ones.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# **Hermes Agent Project Digest – 2026-11-01**

---

### **1. Today's Overview**  
The Hermes Agent project remains highly active, with 50 new issues and 50 pull requests updated in the past 24 hours—indicating strong community engagement and ongoing development momentum. While no new releases were issued, the pipeline is saturated with critical fixes, particularly around gateway stability, mobile app reliability, and configuration safety. High-priority bugs (P2/P3) related to session persistence, memory leaks, and authentication misbehavior are dominating discussion, suggesting a focus on robustness ahead of broader feature rollout. The Android beta is at capacity, signaling growing user adoption and demand for native mobile experiences.

---

### **2. Releases**  
❌ **No new releases** detected.  
*Note: The last stable release remains v0.22.5+1855.g642dfd1 (2026.9.24). No migration notes or breaking changes are currently documented.*

---

### **3. Project Progress**  
✅ **13 PRs merged/closed today**, primarily focused on **critical bug fixes and configuration safety**:  
- **PR #147382** (`fix(gateway): record initial delivery attempt atomically`) addresses a race condition that could cause duplicate message delivery after restarts — a known issue affecting message integrity.  
- **PR #147381** (`fix(config): reject empty documents during raw writeback`) prevents accidental overwrites of `config.yaml` when empty or malformed, mitigating data loss risks.  
- **PR #147380** (`fix(prompt): honor configured temporary directories`) ensures temp files respect user-defined `TMPDIR`, improving privacy and volume control.  
- **PR #147075** (`fix(redact): per-line URL gate, recursive searches, argv dumps`) closes three secret-leak vectors found during audit, enhancing security posture.  
- **PR #147072** (`fix(slack): render multi-select clarify as checkboxes + Submit`) improves UX for Slack users by fixing degraded multi-select behavior.  

These merges indicate a strong emphasis on **stability, security, and user trust** rather than new features.

---

### **4. Community Hot Topics**  
🔥 **Top Issues by Engagement**:
| Issue | Comments | Priority | Link |
|------|--------|--------|------|
| [#11911](https://github.com/nousresearch/hermes-agent/issues/11911) | 21 | P3, Feature | [Native Mobile App (iOS & Android) with Voice Calling](https://github.com/nousresearch/hermes-agent/issues/11911) |
| [#147269](https://github.com/nousresearch/hermes-agent/issues/147269) | 5 | P3, Blocker | [Android beta at Firebase tester limit](https://github.com/nousresearch/hermes-agent/issues/147269) |
| [#147373](https://github.com/nousresearch/hermes-agent/issues/147373) | 1 | P2, Critical | [Smart Approval auto-approved destructive PowerShell command](https://github.com/nousresearch/hermes-agent/issues/147373) |

🔍 **Underlying Needs**:  
- **Mobile-first access**: Demand for iOS/Android apps with voice calling reflects a shift toward hands-free, real-time AI interaction.  
- **Scalable testing infrastructure**: The Android beta hitting Firebase’s 100-user limit suggests the need for dedicated distribution channels (e.g., GitHub Sponsors, internal testflight).  
- **Security guardrails**: The **Smart Approval exploit** (#147373) highlights a systemic risk where approval logic doesn’t validate command execution context — a top priority for future guardrails.

---

### **5. Bugs & Stability**  
🚨 **Critical Stability Concerns Reported Today**:

| Bug | Severity | Impact | Fix PR? |
|-----|---------|--------|--------|
| [#147373](https://github.com/nousresearch/hermes-agent/issues/147373) | P2 | Data loss (188k+ files deleted), security breach | ❌ No fix yet |
| [#147345](https://github.com/nousresearch/hermes-agent/issues/147345) | P3 | Missing completion notification in Android beta | ❌ No fix yet |
| [#147321](https://github.com/nousresearch/hermes-agent/issues/147321) | P3 | Stuttering scroll in long replies (Android) | ❌ No fix yet |
| [#147323](https://github.com/nousresearch/hermes-agent/issues/147323) | P3 | Android app crashes with OutOfMemoryError (~1hr use) | ❌ No fix yet |
| [#147379](https://github.com/nousresearch/hermes-agent/issues/147379) | P3 | Duplicate message resend after restart (no dedup marker) | ✅ PR #147382 exists |
| [#145999](https://github.com/nousresearch/hermes-agent/issues/145999) | P2 | Gateway blocks on `get_meta()` → 30s watchdog kills process | ❌ No fix yet |

📌 **Key Risk Areas**:  
- **Mobile app stability** (Android) is fragile under sustained use.  
- **Gateway event loop blocking** due to DB operations poses uptime threats.  
- **Session resilience** is weak when `HERMES_HOME` is moved (see #146480).

---

### **6. Feature Requests & Roadmap Signals**  
🚀 **High-Interest Features from User Base**:
- **Native Mobile App (iOS & Android)** – [#11911](https://github.com/nousresearch/hermes-agent/issues/11911) (21 comments) – Clear signal for productization beyond CLI/desktop.
- **Voice Calling Support** – Implied in mobile request; indicates desire for conversational AI as a "voice assistant" (like Siri/Google Assistant).
- **Cron Job Metadata (tags, description, workflow)** – [#68482](https://github.com/nousresearch/hermes-agent/issues/68482) – Users managing 10+ cron jobs need better organization.
- **Apple Silicon GPU Support (mlx-whisper)** – [#41659](https://github.com/nousresearch/hermes-agent/issues/41659) – Apple users want full hardware acceleration for STT.
- **Configurable JSON Output** – [#147370](https://github.com/nousresearch/hermes-agent/pull/147370) – Indicates increasing automation use (cron, bots, integrations).

🔮 **Predicted Next Version (v0.23.0)**: Likely to include **mobile app support**, **enhanced config safety**, **security hardening**, and **better cron metadata**.

---

### **7. User Feedback Summary**  
💬 **Real User Pain Points**:
- **Mobile Experience**: Android users report stuttering, freezing, and crashes after ~1 hour — a major barrier to daily use.  
- **Session Migration Failure**: Moving `HERMES_HOME` breaks all local sessions with no migration path (#146480). This frustrates power users doing system reorgs.  
- **Authentication Confusion**: The Accounts tab shows “Not connected” even when OAuth is valid (#130480), eroding trust in connection status.  
- **Security Blind Spots**: One user accidentally wiped 188k files via an approved command whose *text* didn’t reflect its actual effect (#147373). This reveals a critical flaw in approval semantics.  
- **Missing Notifications**: Android users get “thinking” alerts but not “done” notifications (#147345), leading to missed replies.

✅ **Satisfaction Signals**:  
- Positive sentiment around new plugins (SoChain, NeuralMind) and improved redaction (#147075).  
- Appreciation for configurable output (#147370) and better SSH handling (#147054).

---

### **8. Backlog Watch**  
⚠️ **Long-Pending, High-Impact Issues Needing Maintainer Attention**:
- [#11911](https://github.com/nousresearch/hermes-agent/issues/11911) – Native mobile app with voice calling: **21 comments, 15 upvotes, 7 months old**. A flagship product vision.  
- [#146480](https://github.com/nousresearch/hermes-agent/issues/146480) – Session invalidation on `HERMES_HOME` move: **3 comments, no resolution**. Core usability issue for advanced users.  
- [#147373](https://github.com/nousresearch/hermes-agent/issues/147373) – Smart Approval bypass: **1 comment, 1 day old, P2 severity**. Immediate security risk requiring triage.  
- [#147377](https://github.com/nousresearch/hermes-agent/issues/147377) – Legacy Bot Chat row can't be adopted: **0 comments, 1 day old**. May indicate deeper state corruption in multiplexed profiles.

📌 **Recommendation**: Prioritize **#147373** and **#11911** for immediate review. These represent both urgent security and strategic product direction.

---

**📊 Summary Status**:  
🟢 **Health**: Active, developer-driven, high engagement  
🔴 **Risks**: Mobile stability, session management, security gaps  
🟡 **Opportunities**: Mobile product launch, automation enablement, enterprise integration

> 📌 *Last Updated: 2026-11-01 | Source: [Hermes Agent GitHub](https://github.com/nousresearch/hermes-agent)*

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# **QwenPaw Project Digest – 2026-11-01**

---

### **1. Today's Overview**  
QwenPaw remains highly active with a surge in developer engagement: **9 open issues** and **14 open pull requests** updated within the last 24 hours, indicating strong momentum in both bug triage and feature development. The project is experiencing significant focus on **infrastructure stability**, **channel reliability (especially Feishu and QQ)**, and **deployment robustness across diverse environments** (Docker, restricted sandboxes, proxy configurations). A notable spike in PRs from first-time contributors suggests growing community adoption and confidence. Despite no new releases, the velocity of fixes and enhancements signals imminent updates.

---

### **2. Releases**  
❌ No new releases published as of 2026-11-01.  
The latest stable version remains **v2.2.2.post1**, with **v2.3.0-beta.1** and **v2.3.0b2** under active testing and reported instability in constrained environments (e.g., fnOS sandbox). Users are advised to avoid beta versions in production until further validation.

---

### **3. Project Progress**  
✅ **Merged/Closed PRs (Today):**  
- **PR #8338** ([fix(channels): carry Telegram quoted reply context into the agent](https://github.com/agentscope-ai/QwenPaw/pull/8338)) – Resolves missing reply context in Telegram, improving conversational coherence.  
- This fix enhances agent understanding of user intent in threaded conversations.

✅ **Key Fixes in Progress:**  
- **PR #8522** ([fix(console): disable nginx buffering on /console/chat SSE](https://github.com/agentscope-ai/QwenPaw/pull/8522)) – Addresses streaming delay behind nginx reverse proxies (linked to Issue #8516).  
- **PR #8519** ([fix(hub): preserve shared Docker runtime data ownership](https://github.com/agentscope-ai/QwenPaw/pull/8519)) – Prevents `EACCES` errors when Hub runs non-root, critical for secure Docker deployments.  
- **PR #8517** ([fix(hub): permission denied on working/config.json](https://github.com/agentscope-ai/QwenPaw/pull/8519)) – Root cause identified; fix pending review.

✅ **Feature Advancement:**  
- **PR #8409** ([feat(channels): add QQ C2C streaming message support](https://github.com/agentscope-ai/QwenPaw/pull/8409)) – Adds real-time streaming replies to QQ channel, mirroring Feishu’s functionality. High-priority for users in Chinese-speaking ecosystems.

---

### **4. Community Hot Topics**  
🔥 **Most Active Issues (by comments/reactions):**  
1. **Issue #8424** – [Server/discover returns HTTP 404 + JSON-RPC -32601 → breaks legacy MCP handshake](https://github.com/agentscope-ai/QwenPaw/issues/8424)  
   - *4 comments*, *no likes* — but critical: prevents legacy server compatibility. Indicates tension between modern protocol enforcement and backward compatibility needs.  
2. **Issue #8515** – [v2.3.0b2 crashes in fnOS sandbox due to incorrect dependency path resolution](https://github.com/agentscope-ai/QwenPaw/issues/8515)  
   - *1 comment*, *no likes* — yet highlights a systemic issue in environment variable handling (`$HOME`, `pwd.getpwuid()` override). Affects niche but growing deployment use cases.  
3. **Issue #8521** – [Frequent blank replies + UI sluggishness + copy-paste lag](https://github.com/agentscope-ai/QwenPaw/issues/8521)  
   - *1 comment*, *no likes* — but reveals **user frustration with core UX**: performance degradation, inconsistent responses, and outdated UI preferences.  

💬 **Underlying Needs:**  
- **Backward compatibility** (legacy MCP servers)  
- **Deployment flexibility** in restricted or custom environments  
- **UX polish** — especially responsiveness, copy-paste reliability, and UI choice

---

### **5. Bugs & Stability**  
🚨 **High Severity Bugs (Critical Impact):**  
| Issue | Description | Fix PR? | Priority |
|------|-------------|--------|---------|
| **#8424** | Legacy MCP servers fail to handshake due to hard-fail on unknown method | ❌ | ⚠️ High |
| **#8517** | Docker Hub fails to start as non-root due to file ownership conflict | ✅ Yes (PR #8519) | ⚠️ High |
| **#8515** | v2.3.0b2 crashes in fnOS sandbox due to incorrect path logic | ❌ | ⚠️ High |

🟡 **Medium Severity:**  
- **#8184** – Doom-loop gate kills legitimate long-running task polling → false termination alerts  
  - *Risk*: Misleading agent behavior, potential workflow failure  
- **#8511** – Intermittent `ProviderError: No active model configured` during concurrent messages  
  - *Impact*: User-facing error in Feishu channel; disrupts chat flow  
- **#8516** – `/api/console/chat` SSE buffered by nginx due to missing `X-Accel-Buffering: no`  
  - *Fix PR*: #8522 (pending review)

🟢 **Low/Moderate:**  
- **#8521** – Copy-paste requires multiple Ctrl+C attempts  
  - *User pain point*, not crash-related but impacts daily usability

---

### **6. Feature Requests & Roadmap Signals**  
📌 **Top User-Requested Features:**  
- **#8518** – [Display per-reply latency in Console chat UI](https://github.com/agentscope-ai/QwenPaw/issues/8518)  
  - *Requested by one user*, but reflects demand for **performance transparency**. Likely to be included in next minor release.  
- **#8521** – Request for **UI mode toggle (old vs new)** and **faster model switching**  
  - *Strong signal*: Users prefer old UI’s efficiency. Suggests roadmap should include **UI customization options** and **session state persistence**.  
- **#8520** – [Reply to `/approval` in agent’s language](https://github.com/agentscope-ai/QwenPaw/pull/8520)  
  - *First-time contributor effort*, indicates growing need for **multilingual control flows**.

🔮 **Predicted Inclusion in Next Version (v2.3.1 or v2.4.0):**  
- Per-reply latency display  
- Language-aware approval command responses  
- Improved UI toggling (if not already in dev)

---

### **7. User Feedback Summary**  
🗣️ **Real User Pain Points (from Issue #8521):**  
- **Inconsistent output**: "QwenPaw shows thinking but no reply content" — raises concerns about model integration or response parsing.  
- **Poor UX in desktop app**:  
  - Copy-paste requires multiple attempts (Ctrl+C)  
  - New UI feels sluggish and inefficient  
  - Model switching is cumbersome due to poor layout  
- **Preference for old UI**: Explicit request for “switch back” button — indicates **design fatigue** with current interface.

💡 **Use Cases Highlighted:**  
- Deployment in **restricted environments** (fnOS, Docker, proxy setups)  
- Use in **enterprise chat channels** (Feishu, Telegram, QQ) requiring reliable, low-latency interactions  
- Need for **multi-language agent control** in global teams

---

### **8. Backlog Watch**  
🔍 **Long-Unanswered Critical Issues Requiring Attention:**  
- **Issue #8424** – Legacy MCP handshake failure  
  - *Created: 2026-10-26*, *updated: 2026-10-31*, *4 comments*, *0 likes*  
  - **No assigned maintainer**, no related PR. High risk for breaking backward compatibility.  
- **Issue #8184** – Doom-loop gate misidentifies long-running tasks as stuck  
  - *Created: 2026-10-12*, *updated: 2026-10-31*, *3 comments*  
  - Currently unaddressed despite being a core agent logic flaw.  
- **Issue #8511** – Intermittent provider error during concurrent messages  
  - *Created: 2026-10-30*, *updated: 2026-10-31*, *2 comments*  
  - Could indicate race condition in message queue or model handler.

🛠️ **Action Needed:**  
Maintainers should prioritize triaging these three issues to prevent user churn and ensure agent reliability in production workflows.

---

### ✅ **Final Assessment**  
QwenPaw is in a **high-growth, high-activity phase** with strong community participation. While **no new releases** have been issued, the volume of PRs and urgent issues signals that **a major patch or minor release (v2.3.1)** is likely imminent. The project demonstrates **strong technical depth** but faces **critical UX and stability challenges** in edge deployments and legacy compatibility. Immediate attention to **backlog items (#8424, #8184, #8511)** will be key to maintaining trust and adoption.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# **ZeroClaw Project Digest – 2026-11-01**

---

### **1. Today's Overview**  
The ZeroClaw project remains highly active with a robust pace of development: **24 new issues** and **50 pull requests** updated in the last 24 hours, indicating strong contributor engagement and rapid iteration. The ecosystem is focused on **security hardening**, **CLI extensibility**, and **agent autonomy refinement**, particularly around session management, memory isolation, and cross-agent communication. While no new releases have been published, several critical security fixes and feature enhancements are nearing merge, signaling preparation for the upcoming `v0.9.0` milestone. The community is actively addressing both functional gaps and deep-seated architectural risks.

---

### **2. Releases**  
❌ **No new releases** were published as of 2026-11-01.  
The project continues to operate from the `master` branch with ongoing stabilization efforts ahead of `v0.9.0`. No release notes or migration guidance are currently available.

---

### **3. Project Progress**  
✅ **Merged/Completed PRs (Today):**  
- **[PR #12071](https://github.com/zeroclaw-labs/zeroclaw/pull/12071)**: Added `/load-session <name|uuid>` command to ZeroCode, enabling direct resume of saved sessions—improving workflow continuity.  
- **[PR #11971](https://github.com/zeroclaw-labs/zeroclaw/issues/11971)**: Enabled CLI-only source build via `cargo generate features --selection cli-only`, supporting minimal server deployments.  
- **[PR #12129](https://github.com/zeroclaw-labs/zeroclaw/pull/12129)**: Upgraded Wasmtime to **48.0.8**, resolving **five RUSTSEC advisories (RUSTSEC-2026-0420 to 0424)** that blocked CI pipelines.  
- **[PR #12111](https://github.com/zeroclaw-labs/zeroclaw/pull/12111)**: Fixed install logic to prevent accidental overwrites of CLI-only builds during updates.  

These merges reflect progress in **tooling reliability**, **deployment flexibility**, and **dependency hygiene**.

---

### **4. Community Hot Topics**  
🔥 **Top Issue**:  
- **[#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)**: *RFC: Knowledge corpus — document retrieval (RAG) for the agent*  
  - **Status**: Open, P2 priority, high risk, needs author action  
  - **Analysis**: This RFC proposes a foundational capability—**agent-accessible knowledge corpora**—to enable contextual reasoning from user-owned documents, standards, and tooling references. It aligns with the growing demand for **context-aware agents** and signals a shift toward **enterprise-grade personal AI assistants**. The discussion around "capability boundary" suggests this may become a core subsystem.

🔥 **Top PR**:  
- **[PR #12170](https://github.com/zeroclaw-labs/zeroclaw/pull/12170)**: *fix(tools): inherit headless SOP step scope on peer-send turns*  
  - **Status**: Open, size:S, recent activity  
  - **Analysis**: Addresses a **critical security flaw** where `send_message_to_peer` bypasses sender autonomy and budget constraints. This is part of a broader effort to enforce **fine-grained policy enforcement** across agent interactions—indicating community focus on **trust boundaries** and **action accountability**.

---

### **5. Bugs & Stability**  
🚨 **Critical Severity (S1 – Workflow Blocked / Security Risk)**:  
- **[#12143](https://github.com/zeroclaw-labs/zeroclaw/issues/12143)**: MCP stdio servers inherit full daemon environment (including secrets), risking credential exposure to third-party code.  
  - **Fix PR**: [PR #12143](https://github.com/zeroclaw-labs/zeroclaw/pull/12143) pending review.  
- **[#12142](https://github.com/zeroclaw-labs/zeroclaw/issues/12142)**: Agent can read daemon config/data files (`config.toml`, `gateway-admin.token`, OTP secrets) under `level = "full"`—**master-key disclosure vulnerability**.  
  - **Fix PR**: [PR #12142](https://github.com/zeroclaw-labs/zeroclaw/pull/12142) in draft; requires maintainer attention.  
- **[#12141](https://github.com/zeroclaw-labs/zeroclaw/issues/12141)**: Admin routes (`/admin/shutdown`, etc.) trust loopback peers unconditionally—enables **approval bypass + DoS attacks**.  
  - **Fix PR**: [PR #12141](https://github.com/zeroclaw-labs/zeroclaw/pull/12141) open, awaiting review.

🟡 **High Severity (S2 – Degraded Behavior)**:  
- **[#12156](https://github.com/zeroclaw-labs/zeroclaw/issues/12156)**: `zeroclaw update` forces glibc build even on musl systems.  
- **[#12145](https://github.com/zeroclaw-labs/zeroclaw/issues/12145)**: Plugin memory limits not enforced due to Wasmtime defaults—potential resource exhaustion.

🟢 **Minor Issues (S3)**:  
- Multiple installer UX bugs (e.g., version display, musl detection, config file persistence) reported in [#12155](https://github.com/zeroclaw-labs/zeroclaw/issues/12155), [#12154](https://github.com/zeroclaw-labs/zeroclaw/issues/12154), [#12153](https://github.com/zeroclaw-labs/zeroclaw/issues/12153).

---

### **6. Feature Requests & Roadmap Signals**  
🚀 **Emerging Themes**:  
- **RAG / Document Retrieval**: [#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235) is the most mature RFC, likely to be prioritized for `v0.9.0` as a **core agent capability**.  
- **Session Resumption**: `/load-session` added via PR #12071—users want **persistent, discoverable session history**.  
- **CLI-Only Builds**: Requested in [#11971](https://github.com/zeroclaw-labs/zeroclaw/issues/11971), now delivered—suggests growing interest in **headless, server-side deployment**.  
- **OIDC Dashboard Sign-In**: [PR #11965](https://github.com/zeroclaw-labs/zeroclaw/pull/11965) enables secure, scalable access—likely to be included in next release.

📌 **Prediction**: `v0.9.0` will emphasize **security-by-default**, **modular deployment**, and **contextual knowledge integration**—positioning ZeroClaw as a production-ready agent platform.

---

### **7. User Feedback Summary**  
🔧 **Pain Points Reported**:  
- **Session Management**: Users struggle with inconsistent session persistence and lack of direct resume commands (resolved by `/load-session`).  
- **Install Reliability**: Misleading version output, incorrect binary selection (musl/glibc), and incomplete cleanup after cancellation frustrate users.  
- **Security Transparency**: Users express concern over silent credential leaks (e.g., `~/.netrc`, `~/.kube/config`) and unclear sandbox boundaries.  
- **Agent Autonomy Gaps**: Users report agents bypassing budget and permission checks when messaging peers—undermining trust in multi-agent workflows.

✅ **Positive Sentiment**:  
- High appreciation for **CLI-first design**, **modular architecture**, and **active security audits**.  
- Developers value **detailed error messages** and **extensive documentation** in PRs like #12132 and #12062.

---

### **8. Backlog Watch**  
⚠️ **High-Priority Items Needing Maintainer Attention**:  
- **[#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)**: *RFC: Knowledge corpus — document retrieval (RAG)*  
  - **Why**: Foundational for advanced agent capabilities. Needs author input to move forward.  
- **[#12142](https://github.com/zeroclaw-labs/zeroclaw/issues/12142)**: *Master-key disclosure via agent reading daemon config*  
  - **Why**: S1 severity. Fix PR exists but requires urgent review.  
- **[#12143](https://github.com/zeroclaw-labs/zeroclaw/issues/12143)**: *MCP stdio servers inherit daemon secrets*  
  - **Why**: Critical credential exposure risk. PR ready for merge.  
- **[#12137](https://github.com/zeroclaw-labs/zeroclaw/issues/12137)**: *Mis-escaped regex in macOS Seatbelt profile*  
  - **Why**: Blocks toolchains under default sandboxing. Fix PR (#12150) already submitted.

> ✅ **Recommendation**: Prioritize **security fixes (S1/S2)** and **RFC triage** before feature delivery. The backlog reflects a mature project with complex security concerns—maintainers should establish clear triage policies for future stability.

---  
*Data collected: 2026-11-01 | Source: GitHub API (zeroclaw-labs/zeroclaw)*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*