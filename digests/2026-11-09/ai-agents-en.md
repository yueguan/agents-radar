# OpenClaw Ecosystem Digest 2026-11-09

> Issues: 486 | PRs: 500 | Projects covered: 5 | Generated: 2026-11-09 01:56 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# **OpenClaw Project Digest – 2026-11-09**

---

### **1. Today's Overview**  
OpenClaw remains highly active with a surge in community engagement: **486 issues** and **500 pull requests** updated in the last 24 hours, reflecting intense development and user-driven feedback. The project is in a critical phase of stability refinement, with a strong focus on memory management, privacy enforcement, and session reliability—particularly for CLI and macOS users. A new release, **v2026.11.2**, introduces significant trust boundary improvements, signaling a shift toward secure, production-ready configurations. Despite high activity, several P0/P1 bugs affecting core functionality (e.g., agent unavailability, crash loops) remain unresolved, indicating ongoing challenges in system resilience.

---

### **2. Releases**  
🔹 **v2026.11.2** – *Released today*  
[GitHub Release](https://github.com/openclaw/openclaw/releases/tag/v2026.11.2)  

#### **Highlights**  
- **Privacy & Trust Boundaries**:  
  - Configuration secrets, conversation content, tool errors, and channel payloads are now excluded from diagnostic logs.  
  - Enforces selected provider and navigation trust boundaries.  
  - MCP grants are released after warm CLI turns to prevent lingering access.  
  *(PRs: #191318, #174998)*

> ✅ **Migration Note**: No breaking changes reported. Users should upgrade to benefit from enhanced data isolation and reduced log exposure.

---

### **3. Project Progress**  
✅ **Merged/Closed PRs (Today)**: 151  
Several high-impact fixes were merged, including:

- **UI/UX Polish**:  
  - Fixed IME composition Enter behavior in login form ([#195444](https://github.com/openclaw/openclaw/pull/195444)).  
  - Prevented backslashes from being stripped in Windows paths ([#195504](https://github.com/openclaw/openclaw/pull/195504)).  
  - Made Details motion animations frame-cadence independent ([#195506](https://github.com/openclaw/openclaw/pull/195506)).

- **Memory & Stability**:  
  - Fixed embedding probe cache expiration on wall-clock adjustments ([#195503](https://github.com/openclaw/openclaw/pull/195503)).  
  - Fixed Active Memory circuit breaker timing issues ([#195472](https://github.com/openclaw/openclaw/pull/195472)).  
  - Improved GitHub detail reader cache expiry logic ([#195450](https://github.com/openclaw/openclaw/pull/195450)).

- **CLI & Workflow**:  
  - Added `openclaw update status` to list all recorded update runs ([#188200](https://github.com/openclaw/openclaw/pull/188200)).  
  - Fixed `agents list` deprecation warnings falsely blaming Matrix plugin ([#195489](https://github.com/openclaw/openclaw/pull/195489)).  
  - Ensured SQLite source revision workers stay independent of Gateway imports ([#195499](https://github.com/openclaw/openclaw/pull/195499)).

---

### **4. Community Hot Topics**  
🔥 **Top Issues by Comment Count** (most active in past 24h):  

| Issue | Summary | Link |
|------|--------|------|
| [#166634](https://github.com/openclaw/openclaw/issues/166634) | Agent unavailable after crash recovery due to SQLite checkpoint failure | [Issue #166634](https://github.com/openclaw/openclaw/issues/166634) |
| [#191944](https://github.com/openclaw/openclaw/issues/191944) | `gateway stop` fails on macOS with "Process ancestry could not be fully inspected" | [Issue #191944](https://github.com/openclaw/openclaw/issues/191944) |
| [#38327](https://github.com/openclaw/openclaw/issues/38327) | Regression: "Cannot convert undefined or null to object" with Google Vertex/Gemini model | [Issue #38327](https://github.com/openclaw/openclaw/issues/38327) |
| [#193962](https://github.com/openclaw/openclaw/issues/193962) | Fix for SQLite header-probe timeout unreachable via `openclaw update` | [Issue #193962](https://github.com/openclaw/openclaw/issues/193962) |

> 📌 **Underlying Needs**:  
> - **Reliability under stress** (crash recovery, long-running sessions).  
> - **Cross-platform stability**, especially macOS and Windows.  
> - **Model compatibility** post-upgrade, particularly with Google’s Gemini models.  
> - **Update resilience**: users can’t reach fixes via standard update flow — a systemic UX risk.

---

### **5. Bugs & Stability**  
⚠️ **Critical Bugs (P0/P1)** Reported Today:  

| Issue | Severity | Summary | Fix PR? |
|------|----------|--------|--------|
| [#193962](https://github.com/openclaw/openclaw/issues/193962) | P0 | Can't reach fix for SQLite header-probe timeout via `update` | ❌ No |
| [#191944](https://github.com/openclaw/openclaw/issues/191944) | P2 | `gateway stop` fails on macOS | ❌ No |
| [#191847](https://github.com/openclaw/openclaw/issues/191847) | P1 | Large session kills 512 MB SQLite worker → "admission is closed" | ❌ No |
| [#181432](https://github.com/openclaw/openclaw/issues/181432) | P1 | Excessive RSS (~1.9 GB) after 2026.10.5 update | ❌ No |
| [#158592](https://github.com/openclaw/openclaw/issues/158592) | P0 | After host sleep, agent runtime never recovers | ❌ No |

> ⚠️ **Pattern**: Persistent **memory leaks**, **SQLite contention**, and **crash-loop triggers** dominate. Many are regression-related (post-2026.10.x), suggesting recent changes introduced instability. Several are **release blockers** but lack immediate fixes.

---

### **6. Feature Requests & Roadmap Signals**  
💡 **High-Potential Features for Next Version**:  

| Request | Status | Why It’s Likely | Link |
|-------|--------|------------------|------|
| Per-agent dreaming configuration | Open, P2, 16 comments | Addresses OOM risks during cron jobs | [#67413](https://github.com/openclaw/openclaw/issues/67413) |
| Opt-in personal identity on iOS/macOS | Open, P2, 11 comments | Critical for user ownership in shared environments | [#162164](https://github.com/openclaw/openclaw/issues/162164) |
| Rendered Markdown preview in file review | Open, P2, 10 comments | Direct UX friction; popular request | [#128090](https://github.com/openclaw/openclaw/issues/128090) |
| Reduce tool schema token overhead (~3,500 tokens/session) | Open, P2, 12 comments | High impact on context efficiency | [#14785](https://github.com/openclaw/openclaw/issues/14785) |

> 📈 **Trend**: Users are pushing for **fine-grained control**, **performance optimization**, and **richer UI/UX**—especially around memory usage and visual clarity.

---

### **7. User Feedback Summary**  
🗣️ **Real Pain Points Observed**:  
- **“My agent crashes after sleep”** – Frequent reports of agents becoming permanently unavailable after macOS wake cycles (#158592).  
- **“Update breaks everything”** – Users upgrading from 2026.9.7 to 2026.10.10 face silent failures (e.g., SQLite worker death, model validation errors).  
- **“I can’t stop the gateway”** – macOS users stuck unable to terminate the service via CLI.  
- **“Tool parameters vanish after long chats”** – Silent loss of input data in extended conversations (#53408).  
- **“My secret is leaked in logs”** – Strong demand for stricter privacy controls (now addressed in v2026.11.2).

> 💬 **Satisfaction**: Positive feedback on **privacy improvements** and **UI polish** (IME, path handling). Dissatisfaction centers on **stability**, **upgrade safety**, and **debugging difficulty**.

---

### **8. Backlog Watch**  
⏳ **Long-Unanswered, High-Impact Items Needing Attention**:  

| Issue | Age | Priority | Status | Link |
|------|-----|----------|--------|------|
| [#166634](https://github.com/openclaw/openclaw/issues/166634) | 2026-10-07 | P1 | Open, 26 comments, no fix PR | [Issue #166634](https://github.com/openclaw/openclaw/issues/166634) |
| [#191944](https://github.com/openclaw/openclaw/issues/191944) | 2026-11-03 | P2 | Open, 25 comments, no fix | [Issue #191944](https://github.com/openclaw/openclaw/issues/191944) |
| [#38327](https://github.com/openclaw/openclaw/issues/38327) | 2026-03-06 | P1 | Open, 18 comments, regression | [Issue #38327](https://github.com/openclaw/openclaw/issues/38327) |
| [#193962](https://github.com/openclaw/openclaw/issues/193962) | 2026-11-06 | P0 | Open, 15 comments, no fix | [Issue #193962](https://github.com/openclaw/openclaw/issues/193962) |
| [#177909](https://github.com/openclaw/openclaw/issues/177909) | 2026-10-20 | P2 | Open, 10 comments, misclassified as model failure | [Issue #177909](https://github.com/openclaw/openclaw/issues/177909) |

> 🔔 **Urgency**: These represent **critical user workflows** that are broken or poorly diagnosed. Maintainers must prioritize triage and fix PRs to avoid further erosion of trust.

---

📌 **Final Assessment**:  
OpenClaw is **highly active but under pressure**. While v2026.11.2 delivers important privacy wins, underlying stability issues—especially around memory, SQLite, and cross-platform reliability—remain acute. The backlog reflects deep user frustration with upgrades, crashes, and debugging. Immediate attention to **P0/P1 stability bugs** and **long-standing regressions** is essential to maintain momentum and credibility.

---

## Cross-Ecosystem Comparison

# **Cross-Project Comparison Report: Personal AI Agent Ecosystem – 2026-11-09**

---

### **1. Ecosystem Overview**  
The open-source personal AI agent ecosystem in late 2026 is characterized by rapid evolution, strong technical specialization, and growing maturity in security, privacy, and cross-platform resilience. Projects are increasingly focused on production-grade reliability—moving beyond prototyping toward secure, auditable, and user-controlled execution environments. While foundational capabilities like model integration and session management remain common, the frontier has shifted to **trust boundaries**, **memory isolation**, **agent durability**, and **user autonomy** across platforms. The landscape reflects a clear bifurcation: some projects prioritize **developer-first extensibility**, others emphasize **end-user safety and operational stability**.

---

### **2. Activity Comparison**

| Project | Issues (Last 24h) | PRs (Last 24h) | Release Status | Health Score* |
|--------|-------------------|----------------|----------------|---------------|
| **OpenClaw** | 486 | 500 | ✅ v2026.11.2 released | ⭐⭐⭐⭐☆ (4.5/5) |
| **Hermes Agent** | 50 | 50 | ❌ None | ⭐⭐⭐⭐☆ (4.3/5) |
| **IronClaw** | 0 | 6 | ❌ None | ⭐⭐⭐☆☆ (3.2/5) |
| **QwenPaw** | 5 | 15 | ❌ None | ⭐⭐⭐⭐☆ (4.4/5) |
| **ZeroClaw** | 13 | 50 | ❌ None | ⭐⭐⭐⭐☆ (4.6/5) |

> *Health Score: Based on activity density, fix velocity, bug severity, release cadence, and community engagement (0–5 scale).*

> 🔍 **Insight**: OpenClaw leads in raw volume, ZeroClaw in focused momentum, QwenPaw in UX-driven refinement, Hermes Agent in architectural depth, and IronClaw in quiet but critical dependency hygiene.

---

### **3. OpenClaw's Position**  
OpenClaw stands as the **most active and visible project** in the ecosystem, with unmatched contributor and issue volume. Its technical approach centers on **privacy-by-design**, **session resilience**, and **cross-platform stability**, particularly for CLI and macOS users. Unlike peers that focus on niche use cases or infrastructure, OpenClaw aggressively targets **production readiness** through granular trust boundaries, diagnostic log sanitization (v2026.11.2), and memory circuit breakers.

Compared to peers:
- **vs. ZeroClaw**: More mature in UI/UX and diagnostics, but less advanced in agent lifecycle tracking.
- **vs. QwenPaw**: Stronger in system-level stability and error visibility, though QwenPaw shows sharper focus on session clarity.
- **vs. Hermes Agent**: Broader scope in tooling and provider support; Hermes excels in multi-tenant state modeling.
- **vs. IronClaw**: Far more user-facing; IronClaw operates at lower abstraction with minimal community interaction.

Its **community size is likely largest**, evidenced by comment volumes and issue churn—making it a de facto reference point for agent developers.

---

### **4. Shared Technical Focus Areas**  
Across all five projects, recurring technical needs reflect convergence toward **secure, reliable, and observable AI agents**:

| Focus Area | Projects Affected | Key Needs |
|----------|------------------|---------|
| **Memory & Stability** | OpenClaw, Hermes Agent, QwenPaw, ZeroClaw | Prevent OOM crashes, avoid silent leaks, manage SQLite contention, improve worker isolation |
| **Session Persistence & Recovery** | OpenClaw, QwenPaw, ZeroClaw | Fix post-crash/restart failures, prevent config reset (e.g., QQ `enabled` flag), ensure auditability |
| **Security & Trust Boundaries** | OpenClaw, ZeroClaw, Hermes Agent | Enforce sandboxed delegation, prevent filesystem scope leakage, isolate agent identity |
| **Model Observability & Fallbacks** | QwenPaw, OpenClaw, Hermes Agent | Detect silent model switches, expose fallback reasons, improve debugging transparency |
| **Cross-Platform Reliability** | OpenClaw, Hermes Agent, QwenPaw, ZeroClaw | Fix Windows subprocess deadlocks, macOS process inspection issues, long-path handling |

> 📌 **Pattern**: All projects now treat **crash recovery**, **config integrity**, and **debuggability** as non-negotiable requirements—not just features.

---

### **5. Differentiation Analysis**

| Dimension | OpenClaw | Hermes Agent | IronClaw | QwenPaw | ZeroClaw |
|--------|----------|--------------|----------|---------|----------|
| **Target User** | Devs + power users | Enterprise teams, multi-agent workflows | Infrastructure engineers | Multi-channel chat users | Security-conscious operators |
| **Core Focus** | Privacy, stability, CLI | State fencing, multi-tenancy, OAuth | Dependency hygiene, WASM runtime | UX polish, observability | Agent durability, access control |
| **Architecture** | Monolithic agent + gateway | Plugin-driven, scope-fenced | Rust-based, WASM-enabled | Python-centric, web-native | Policy-enforced, append-only logs |
| **Key Innovation** | Diagnostic log sanitization | Memory consolidation opt-in | Automated dependency updates | Dynamic tab titles, fallback metadata | Immutable session events, bounded delegation |
| **Deployment Model** | Local-first, CLI-heavy | Cloud-hosted, TUI/web | Edge-computing, lightweight | Web app + mobile | Self-hosted, relay-secured |

> 🔑 **Differentiator Summary**:  
> - **OpenClaw** = **Privacy-first stability**  
> - **Hermes Agent** = **Multi-tenant autonomy**  
> - **IronClaw** = **Infrastructure resilience**  
> - **QwenPaw** = **User experience clarity**  
> - **ZeroClaw** = **Trust-boundary enforcement**

---

### **6. Community Momentum & Maturity**

| Tier | Projects | Characteristics |
|------|--------|----------------|
| **Rapid Iteration (High Velocity)** | OpenClaw, ZeroClaw, Hermes Agent | >50 PRs/issue updates daily; P0 bugs actively triaged; roadmap aligned with user feedback |
| **Feature Refinement (Stable Growth)** | QwenPaw | Focused on polishing beta; high-quality PRs under review; user-centric fixes |
| **Maintenance Mode (Quiet Health)** | IronClaw | No user issues; automated dependency updates only; low engagement but stable foundation |

> 📈 **Maturity Signal**: OpenClaw and ZeroClaw represent **mature, productized ecosystems**—with defined roadmaps, security priorities, and developer trust. QwenPaw is transitioning from alpha to beta. IronClaw remains a **hidden gem**—stable but not widely adopted.

---

### **7. Trend Signals**  
Based on community feedback and PR trends, the following industry-wide signals emerge:

1. **Privacy as Default**  
   → OpenClaw’s v2026.11.2 logging changes signal a shift: **no sensitive data in logs** is becoming standard, not optional.

2. **Observability Over Opacity**  
   → Users demand **visible model fallbacks** (#8103, #8628), **error context**, and **runtime state**—a move away from "black box" agents.

3. **Agent Durability & Auditability**  
   → ZeroClaw’s append-only session logs and OpenClaw’s crash recovery fixes show rising demand for **immutable, traceable agent histories**.

4. **Cross-Platform Parity Is Non-Negotiable**  
   → Repeated macOS/Windows issues (#191944, #135977) indicate that **platform parity is now a release blocker**, not a nice-to-have.

5. **Security by Design, Not Add-On**  
   → Features like ZeroClaw’s bounded delegation, Hermes Agent’s OAuth pool isolation, and OpenClaw’s MCP grant timing reflect a **paradigm shift** from “add security later” to **embed trust boundaries early**.

> 💡 **Value for Developers**: These trends suggest that successful future agents will be judged not on reasoning quality alone, but on **reliability, debuggability, and trustworthiness**—making these traits essential for adoption in enterprise and production settings.

---

### ✅ **Final Takeaway**  
The personal AI agent ecosystem is maturing rapidly. **OpenClaw leads in breadth and urgency**, while **ZeroClaw and Hermes Agent define the next generation of secure, multi-agent systems**. **QwenPaw** demonstrates how UX excellence drives user retention, and **IronClaw** proves that even quiet maintenance can underpin critical infrastructure. For developers, the message is clear: **build for resilience, observe for trust, and design for privacy from day one**.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# **Hermes Agent Project Digest – 2026-11-09**

---

### **1. Today's Overview**  
The Hermes Agent project remains highly active with a robust pipeline of updates: **50 issues and 50 pull requests updated in the last 24 hours**, indicating sustained momentum across development, testing, and community engagement. The majority of activity centers on platform-specific stability (especially Windows and Telegram), session handling, memory isolation, and security hardening. While no new releases have been published, the volume of PRs—particularly those focused on state management, scope fencing, and multi-tenant architecture—suggests significant architectural refinements are underway ahead of an upcoming release cycle.

---

### **2. Releases**  
*No new releases were published as of 2026-11-09.*  
The project continues to operate on `main` with incremental improvements. Users should expect a patch or minor version update soon, likely incorporating recent fixes related to session persistence, memory safety, and cross-platform stability.

---

### **3. Project Progress**  
**Merged/Closed PRs (Today):**  
- ✅ **PR #151598** (`fix(memory): strip auto-loaded skill bodies before provider sync`) — Resolves issue #92036 by ensuring auto-loaded skill content is cleaned before syncing, preventing data leakage in memory providers.
- ✅ **PR #151597** (`fix(catalog): repoint hermes-crux to tony-simons-aiowa`) — Corrects repository ownership for the `hermes-crux` plugin catalog, resolving a misdirected dependency.
- ✅ **PR #151577** (`fix(build): keep the previous node_modules until the pnpm install succeeds`) — Prevents build failures due to transient registry outages by preserving existing `node_modules`.

These fixes improve build resilience, plugin integrity, and memory hygiene—critical for production stability.

---

### **4. Community Hot Topics**  
Top 5 most commented/engaged items reflect core user pain points:

| Issue | Comments | Link | Key Insight |
|------|----------|------|-----------|
| [#110374](https://github.com/NousResearch/hermes-agent/issues/110374) | 14 | [Slack status indicator broken](https://github.com/NousResearch/hermes-agent/issues/110374) | A regression in `slack-sdk >= 3.44` silently breaks real-time status indicators; users lose visibility into agent thinking state. High priority due to impact on UX in collaborative environments. |
| [#141919](https://github.com/NousResearch/hermes-agent/issues/141919) | 9 | [ACP duplicates streamed responses](https://github.com/NousResearch/hermes-agent/issues/141919) | Plugin hooks on `transform_llm_output` cause double-rendering of replies—critical for streaming clients like TUI and web dashboards. Indicates flawed stream lifecycle management. |
| [#135977](https://github.com/NousResearch/hermes-agent/issues/135977) | 7 | [Windows `session/new` hangs forever](https://github.com/NousResearch/hermes-agent/issues/135977) | A deep Windows subprocess deadlock involving Git pipe inheritance; blocks all new sessions. High-severity blocker for Windows users. |
| [#147613](https://github.com/NousResearch/hermes-agent/issues/147613) | 3 | [Kanban worker stalls on Windows at seq 15](https://github.com/NousResearch/hermes-agent/issues/147613) | Deterministic hang post-update; suggests race condition or timing bug in background task scheduling. Reproducible across profiles. |
| [#148365](https://github.com/NousResearch/hermes-agent/issues/148365) | 2 | [iOS WebSocket drops every 10–60s](https://github.com/NousResearch/hermes-agent/issues/148365) | Plugins hub rebuilds every 7–46 seconds under load, causing iOS client disconnections. Points to inefficient plugin discovery logic. |

> 💡 **Underlying Need:** Users demand *predictable, stable, low-latency interactions*, especially across platforms (Windows, mobile, TUI). Real-time feedback (status, streaming) and session continuity are top priorities.

---

### **5. Bugs & Stability**  
Critical bugs reported today, ranked by severity:

| Bug | Severity | Platform | Status | Fix PR? |
|-----|----------|----------|--------|---------|
| [#135977](https://github.com/NousResearch/hermes-agent/issues/135977) | P2 (High) | Windows | Open | ❌ No fix yet |
| [#147613](https://github.com/NousResearch/hermes-agent/issues/147613) | P2 (High) | Windows | Open | ❌ No fix yet |
| [#138337](https://github.com/NousResearch/hermes-agent/issues/138337) | P1 (Critical) | Anthropic OAuth | Open | ❌ No fix yet |
| [#151542](https://github.com/NousResearch/hermes-agent/issues/151542) | P2 (High) | CLI / Terminal | Open | ❌ No fix yet |
| [#151401](https://github.com/NousResearch/hermes-agent/issues/151401) | P3 (Medium) | Streaming / Tool Calls | Open | ❌ No fix yet |

> 🔴 **P1 Critical**: `delegate_task` children share an OAuth pool and revoke each other’s tokens on 401, leading to cascading failures. This is a serious security boundary violation affecting hosted agents using Anthropic.

---

### **6. Feature Requests & Roadmap Signals**  
Emerging themes suggest strong interest in **multi-tenancy, privacy, and autonomy**:

| Feature | Requested By | Link | Predicted In Next Version? |
|-------|--------------|------|----------------------------|
| 🚀 Multi-User Preference Memory (#17254) | Carloschancc | [Issue #17254](https://github.com/NousResearch/hermes-agent/issues/17254) | ✅ Yes – Already being addressed via PR stack |
| 🛠️ Explicit Opt-In for Memory Consolidation (#106919) | 0xble | [Issue #106919](https://github.com/NousResearch/hermes-agent/issues/106919) | ✅ Likely – Aligns with privacy-first trends |
| 🔐 Bens Magical Ticket of Multi-tenancy (#146503) | benbarclay | [Issue #146503](https://github.com/NousResearch/hermes-agent/issues/146503) | ✅ High probability – Supported by stacked PRs |
| 🧩 Plugin Goal Seam (ctx.goals) (#138280) | 100yenadmin | [Issue #138280](https://github.com/NousResearch/hermes-agent/issues/138280) | ✅ Plausible – Needed for advanced workflow plugins |
| 🌍 UI Localization (pt-BR, etc.) (#114195) | devbetortiz | [Issue #114195](https://github.com/NousResearch/hermes-agent/issues/114195) | ⚠️ Possible – Lower priority but growing demand |

> 📌 **Roadmap Signal**: The team is actively building toward **multi-tenant, sandboxed, secure execution environments**, with focus on scope isolation, state fencing, and autonomous memory management.

---

### **7. User Feedback Summary**  
Real-world pain points from users include:

- **Loss of context**: Immediate chat switches drop prompts (Issue #151269).
- **Unreliable approvals**: Code execution approval times out even after re-consent (Issue #149865).
- **Missing feedback**: Voice messages dropped silently on Telegram when emoji-heavy (Issue #124406).
- **Inconsistent behavior**: Relative `rm` commands not flagged as dangerous despite risk (Issue #151542).
- **Mobile instability**: iOS app disconnects repeatedly due to slow plugin rebuilds (Issue #148365).

Users report high satisfaction with AI reasoning and tool integration but express frustration with **UX consistency, reliability under load, and platform parity**—especially on Windows and mobile.

---

### **8. Backlog Watch**  
Important long-standing issues requiring maintainer attention:

| Issue | Age | Priority | Status | Notes |
|------|-----|----------|--------|-------|
| [#17254](https://github.com/NousResearch/hermes-agent/issues/17254) | 2026-04-29 (~6 months) | P3 | Open | Request for user-ID-isolated memory – critical for group use cases |
| [#28226](https://github.com/NousResearch/hermes-agent/issues/28226) | 2026-05-18 (~5 months) | P3 | Open | Proposal for multi-tenant memory OS with diagnostics and isolation tests |
| [#146503](https://github.com/NousResearch/hermes-agent/issues/146503) | 2026-10-29 (~1 month) | P3 | Open | Foundational for enterprise hosting; currently blocked on implementation clarity |
| [#89385](https://github.com/NousResearch/hermes-agent/issues/89385) | 2026-08-18 (~3 months) | P3 | Closed | Already has a working consumer (Memory Lane); needs official support |

> ⏳ **Note**: Several high-value features are stalled due to "needs-decision" or "needs-repro" labels. Maintainers should prioritize triaging these to unlock community contributions.

---

### ✅ **Final Assessment**  
Hermes Agent is in a phase of **deep architectural refinement**, with strong momentum in security, state management, and multi-tenancy. Despite a lack of new releases, the project shows excellent health through consistent PR velocity, mature issue tracking, and alignment with user-driven roadmaps. Immediate focus should be on **resolving Windows regressions, fixing OAuth token conflicts, and stabilizing streaming/UI responsiveness** to maintain trust across platforms.

> 🔗 **Project Dashboard**: [https://github.com/NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)  
> 📊 **Activity Metrics**: 50 issues, 50 PRs updated in 24h — **high signal-to-noise ratio**.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

**IronClaw Project Digest – 2026-11-09**

---

### **1. Today's Overview**  
The IronClaw project shows minimal activity on November 9, 2026, with no new issues or releases reported in the past 24 hours. However, six pull requests were updated—five remain open and one was merged—indicating ongoing maintenance of dependencies. All recent PRs are automated updates from `dependabot[bot]`, focusing primarily on Rust ecosystem libraries, CI/CD tooling, and WebAssembly components. The absence of user-driven issues suggests a stable but low-engagement period, likely due to scheduled dependency upkeep rather than feature development.

---

### **2. Releases**  
No new releases have been published as of 2026-11-09. The project remains on its current version state, with all updates being internal dependency bumps through PRs rather than formal release cycles.

---

### **3. Project Progress**  
Only one PR was merged today:  
- **PR #8160** ([closed](https://github.com/nearai/ironclaw/pull/8160)) — *chore(deps): bump the everything-else group across 1 directory with 43 updates* (merged on Nov 8). This update included critical security and stability patches for core crates like `thiserror` (v2.0.20 → v2.0.21), `uuid` (v1.24.0 → v1.28.0), and `base64`. While not introducing functional changes, this merge improves dependency hygiene and mitigates potential vulnerabilities.

---

### **4. Community Hot Topics**  
The most active discussion is centered around automated dependency updates, with **six PRs opened by dependabot[bot]** in the last 30 days. The top three high-impact PRs are:  
- **PR #8169** ([open](https://github.com/nearai/ironclaw/pull/8169)): Bumps 44 packages in the `everything-else` group, including `uuid` to v1.29.0 and `base64` to latest.  
- **PR #8141** ([open](https://github.com/nearai/ironclaw/pull/8141)): Updates Tokio ecosystem crates (`tokio`, `tokio-test`, `tokio-util`) to newer versions (e.g., `tokio` v1.53.1 → v1.53.3), crucial for async runtime stability.  
- **PR #8159** ([open](https://github.com/nearai/ironclaw/pull/8159)): Upgrades GitHub Actions workflows, notably `anthropics/claude-code-action` to v1.0.259, reflecting integration with AI code assistants.

These PRs reflect a strong focus on maintaining up-to-date, secure infrastructure—particularly in CI/CD and async systems—suggesting community demand for robust, modern tooling.

---

### **5. Bugs & Stability**  
No bugs, crashes, or regressions were reported in the last 24 hours. All recent PRs are non-breaking dependency upgrades, and none indicate known breaking changes. The lack of issue reports supports a stable runtime environment, though long-term stability depends on timely review of the open dependency PRs.

---

### **6. Feature Requests & Roadmap Signals**  
No explicit feature requests are visible in the issue tracker. However, the consistent focus on **WASM tooling** (via PR #7834) and **Tokio ecosystem upgrades** signals interest in:  
- Enhanced WebAssembly support (e.g., Wasmtime, wit-parser), possibly for edge computing or sandboxed execution.  
- Improved async performance and reliability, indicating future roadmap emphasis on scalable, concurrent AI agent execution.  
These trends suggest that next version may prioritize **runtime efficiency**, **modular WASM-based agents**, and **AI-native concurrency patterns**.

---

### **7. User Feedback Summary**  
No direct user feedback is available in issues or PR comments. However, the sheer volume of automated dependency updates—especially in critical areas like `serde_json`, `tokio`, and `uuid`—implies indirect user satisfaction with proactive security and compatibility maintenance. Users likely appreciate the project’s commitment to dependency hygiene without requiring manual intervention.

---

### **8. Backlog Watch**  
Several high-priority dependency PRs remain open and unreviewed:  
- **PR #8169** ([open](https://github.com/nearai/ironclaw/pull/8169)): 44 package updates; requires attention to avoid drift.  
- **PR #8141** ([open](https://github.com/nearai/ironclaw/pull/8141)): Tokio updates affecting async behavior—critical for performance.  
- **PR #7834** ([open](https://github.com/nearai/ironclaw/pull/7834)): WASM toolchain upgrades; essential for agent portability and deployment flexibility.  

These PRs represent technical debt accumulation risk if not reviewed promptly. Maintainers should prioritize these to prevent future breakage during larger refactorings.

---

**Summary**: IronClaw is in a steady-state maintenance phase with strong focus on dependency health. No urgent risks exist, but proactive review of open automation PRs is advised to sustain long-term stability and readiness for future innovation.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# **QwenPaw Project Digest – 2026-11-09**

---

### **1. Today's Overview**  
The QwenPaw project remains highly active with a strong momentum in both issue triage and pull request development. Over the past 24 hours, 5 new issues were opened (all unresolved), and 15 PRs were updated—indicating robust contributor engagement and ongoing feature refinement. Notably, no releases were published, suggesting that the team is prioritizing internal stability and feature polishing ahead of a formal update cycle. The mix of bug fixes, UX enhancements, and model observability improvements reflects a matured focus on reliability and user experience.

---

### **2. Releases**  
❌ **No new releases** were published as of 2026-11-09.  
The latest version remains **2.2.2.post1**, with no changelog or migration notes available for upcoming updates. The absence of a release suggests that the current beta (2.3.0-beta.2) may be undergoing final validation before public rollout.

---

### **3. Project Progress**  
✅ **Key merged/closed PRs (today):** None  
However, several high-impact PRs are actively under review or recently submitted:

- **PR #8631** ([fix(console): follow active chat name in browser tab title](https://github.com/agentscope-ai/QwenPaw/pull/8631)) – Implements dynamic tab titles showing the current conversation name, directly addressing usability in multi-tab workflows.
- **PR #8630** ([fix(channels): preserve enabled state on partial channel updates](https://github.com/agentscope-ai/QwenPaw/pull/8630)) – Fixes a critical regression where QQ channel `enabled` flag was silently reset after config reloads.
- **PR #8628** ([feat(providers): add model fallback metadata to chat response](https://github.com/agentscope-ai/QwenPaw/pull/8628)) – Adds visibility into silent model fallbacks via `model_used`, `model_req`, and `fallback_reason` fields in responses — directly tackling a major observability gap.

These PRs indicate strong progress in **UX polish**, **config integrity**, and **system transparency**.

---

### **4. Community Hot Topics**  

#### 🔥 **Most Active Issue: #8475 – [Bug] QQ channel `enabled` flag silently reset after restart**
- **URL:** https://github.com/agentscope-ai/QwenPaw/issues/8475  
- **Activity:** 2 comments, last updated Nov 8, 2026  
- **Impact:** High — breaks message reception entirely without warning, affecting core functionality for users relying on QQ integration.  
- **Underlying Need:** Reliable persistence of configuration states across restarts; users demand clear feedback when critical features fail silently.

#### 🌟 **Most Strategic PR: #8628 – Add model fallback metadata to chat response**
- **URL:** https://github.com/agentscope-ai/QwenPaw/pull/8628  
- **Author:** AdityaxDeore  
- **Significance:** Directly resolves **Issue #8103** (silent fallbacks with no user notification). This PR enables users to detect and debug model switching behavior — essential for production use cases involving API reliability.

#### 💬 **User-Centric Enhancement: #8605 – Browser tab title follows current session name**
- **URL:** https://github.com/agentscope-ai/QwenPaw/issues/8605  
- **Comment:** 1 comment, created Nov 6, 2026  
- **Need:** Users managing multiple conversations simultaneously struggle to distinguish tabs due to identical titles (`"Chat · QwenPaw"`). This is a quality-of-life fix with high user satisfaction potential.

> ✅ *Trend Insight:* Community is increasingly focused on **observability**, **session clarity**, and **resilience** — signaling maturity beyond basic functionality.

---

### **5. Bugs & Stability**  

| Severity | Issue | Summary | Fix PR? |
|--------|-------|---------|--------|
| ⚠️ Critical | [#8475](https://github.com/agentscope-ai/QwenPaw/issues/8475) | QQ channel `enabled` flag resets to `false` post-restart, halting all messaging with no logs | ✅ Yes – **PR #8630** addresses this |
| ⚠️ High | [#8629](https://github.com/agentscope-ai/QwenPaw/issues/8629) | Oversized historical images re-sent on every call; misleading "Image loaded" log persists | ❌ No fix yet – impacts performance and UX |
| ⚠️ Medium | [#8627](https://github.com/agentscope-ai/QwenPaw/issues/8627) | Checkpoint service fails on long Git paths; WeChat channel stuck at `DEPENDENCY_ENVIRONMENT_AMBIGUOUS` on Windows | ❌ No fix yet – affects Windows users with long file paths |
| ⚠️ Low | [#8624](https://github.com/agentscope-ai/QwenPaw/issues/8624) | SQLite errors hidden behind timeout messages after deadline | ✅ Partial fix in **PR #8624** (now preserves original error) |

> 🔍 **Stability Note:** Several regressions involve **configuration persistence**, **error visibility**, and **platform-specific edge cases** (especially Windows/Git). The project is stable but sensitive to edge-case environments.

---

### **6. Feature Requests & Roadmap Signals**  

| Feature Request | Status | Predicted In Version |
|------------------|--------|------------------------|
| [#8605](https://github.com/agentscope-ai/QwenPaw/issues/8605) – Tab title follows active session | Open | **2.3.0** (likely) |
| [#8626](https://github.com/agentscope-ai/QwenPaw/issues/8626) – Per-job `thinking_level` override | Open | **2.3.0** (high priority) |
| [#8625](https://github.com/agentscope-ai/QwenPaw/issues/8625) – Row-only search in `recall_history` | Open | **2.3.0** (performance-focused) |
| [#8103](https://github.com/agentscope-ai/QwenPaw/issues/8103) – Notify user on model fallback | Open → PR #8628 merged | **2.3.0** (confirmed) |

> 📈 **Roadmap Signal:** The next major release (**2.3.0**) will likely emphasize:
> - **Observability & debugging** (fallback tracking, error visibility)
> - **Performance optimization** (search efficiency, token usage)
> - **Multi-session UX** (tab titles, per-job config overrides)

---

### **7. User Feedback Summary**  

- **Pain Points:**
  - Silent failures (e.g., disabled channels, model fallbacks) cause frustration with no diagnostics.
  - Poor visibility into system state during runtime (no indication when models switch).
  - Confusing UI in multi-tab scenarios — users cannot tell which conversation they're in.
  - Long-path issues on Windows disrupt checkpointing and dependency resolution.

- **Satisfaction Indicators:**
  - Positive sentiment around **dynamic tab titles** and **model fallback visibility** — users appreciate being informed about what’s happening.
  - First-time contributors are actively engaging (e.g., PR #8628, #8524), indicating growing community trust.

> 💬 *Quote from Issue #8475:* _“After a restart, my bot stopped receiving messages completely — I had no idea why until I checked the config manually.”_  
> → Highlights need for **proactive warnings** and **config audit trails**.

---

### **8. Backlog Watch**  

| Issue | Priority | Why It Needs Attention |
|------|----------|------------------------|
| [#8475](https://github.com/agentscope-ai/QwenPaw/issues/8475) – QQ channel `enabled` reset | 🔴 Critical | Breaks core functionality; already has a fix PR (#8630) – should be fast-tracked for merge. |
| [#8103](https://github.com/agentscope-ai/QwenPaw/issues/8103) – Silent model fallbacks | 🔴 High | Affects trust in AI output; PR #8628 exists but needs review — could block adoption in production. |
| [#8627](https://github.com/agentscope-ai/QwenPaw/issues/8627) – Long path + WeChat dependency failure | 🟡 Medium | Blocks Windows users with default installations; affects early adopters. |
| [#8629](https://github.com/agentscope-ai/QwenPaw/issues/8629) – Image re-sending & misleading logs | 🟡 Medium | Impacts performance and user confidence; requires deeper investigation into media handling pipeline. |

> ⏳ **Action Item:** Maintainers should prioritize reviewing **PR #8630**, **PR #8628**, and **PR #8626** to accelerate delivery of key stability and UX improvements ahead of 2.3.0.

---

### ✅ **Final Assessment**  
QwenPaw is in a **strong, healthy phase of development** with a vibrant community and well-targeted improvements. The project demonstrates excellent responsiveness to user-reported pain points, especially in **config durability**, **debugging visibility**, and **multi-session usability**. With a solid foundation of PRs ready for merge, the next release (2.3.0) is poised to significantly enhance reliability and user control — setting a strong precedent for future open-source agent platforms.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# **ZeroClaw Project Digest**  
**Date:** 2026-11-09  
**Repository:** [github.com/zeroclaw-labs/zeroclaw](https://github.com/zeroclaw-labs/zeroclaw)

---

### **1. Today's Overview**

The ZeroClaw project remains highly active with a surge in development momentum: **50 pull requests** and **13 new issues** updated within the last 24 hours, indicating strong contributor engagement and ongoing architectural refinement. The focus is clearly on **security hardening**, **cross-platform compatibility**, and **agent lifecycle management**, particularly around identity, access control, and runtime integrity. Despite no new releases, the pipeline shows advanced work toward **v0.9.0**, with critical path features like dashboard chat for non-admins, bounded delegate targeting, and persistent agent relays progressing rapidly. The community is actively shaping the next major release through detailed RFCs and infrastructure-level improvements.

---

### **2. Releases**

❌ **No new releases** were published today or in the past 7 days.  
- The project continues to target **v0.9.0**, as evidenced by multiple `release:v0.9.0` tags across open issues and PRs.
- No breaking changes or migration notes are currently documented due to lack of release activity.

> 🔗 *No release artifacts available at this time.*

---

### **3. Project Progress (Merged/Closed PRs)**

✅ **8 PRs merged or closed** in the last 24 hours — primarily focused on security policy enforcement, config validation, and CLI usability:

- **PR #11921** – [`feat(session): implement append-only session event history`](https://github.com/zeroclaw-labs/zeroclaw/pull/11921)  
  → Centralized, immutable session logging now captures all agent interactions, approvals, tool calls, and state transitions — significantly improving auditability and debugging.

- **PR #11406** – [`fix(config)!: refuse macOS broad roots in filesystem channel`](https://github.com/zeroclaw-labs/zeroclaw/pull/11406)  
  → Security fix preventing dangerous root-level filesystem monitoring on macOS; stacked on prior Windows fix (#11394), now complete.

- **PR #11268** – [`docs(auth): align gateway policy publication guidance`](https://github.com/zeroclaw-labs/zeroclaw/pull/11268)  
  → Clarified documentation on when authorization edits take effect, reducing confusion around reload boundaries.

- **PR #11226** – [`fix(config): cascade agent renames into permission-profile selectors`](https://github.com/zeroclaw-labs/zeroclaw/pull/11226)  
  → Ensures permission profiles automatically reflect renamed agents, fixing potential access drift.

- **PR #11313** – [`fix(cli): publish authorization edits from config set and config patch`](https://github.com/zeroclaw-labs/zeroclaw/pull/11313)  
  → CLI commands now immediately apply policy updates to running daemons, eliminating stale configuration states.

- **PR #11301** – [`fix(agents): share durable committed-rename recovery`](https://github.com/zeroclaw-labs/zeroclaw/pull/11301)  
  → Fixes race condition where failed agent renames left state inconsistent across CLI, gateway, and RPC.

- **PR #11868** – [`fix(agent): stop repeated calls before dispatch`](https://github.com/zeroclaw-labs/zeroclaw/pull/11868)  
  → Prevents redundant tool execution and retains final results for clean closeout — improves reliability.

- **PR #11317** – [`docs(runtime): propose bounded plugin webhook RPC placement exception`](https://github.com/zeroclaw-labs/zeroclaw/pull/11317)  
  → Adds formal exception handling for plugin webhooks, supporting future secure RPC design.

---

### **4. Community Hot Topics**

🔥 **Top Issues & PRs by Engagement (Last 24h)**

| Issue/PR | Title | Activity | Link |
|--------|------|---------|------|
| **#7432** | `[Tracker]: Runtime, dashboard sign-in and gateway delivery` | 16 comments, high risk, v0.9.0 milestone | [Issue #7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) |
| **#9872** | `[Bug]: Bounded delegate target resolves filesystem to delegator's workspace` | 5 comments, P1 severity, high-risk | [Issue #9872](https://github.com/zeroclaw-labs/zeroclaw/issues/9872) |
| **#9597** | `[Feature]: make peer-agent turns durable and attributable` | 5 comments, in-progress, critical for agent loop stability | [Issue #9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597) |
| **#12270** | `feat(relay): serve a daemon's web dashboard through the relay` | 1 comment, XL size, v0.9.0 feature | [PR #12270](https://github.com/zeroclaw-labs/zeroclaw/pull/12270) |

🔍 **Analysis of Underlying Needs:**
- **Security context isolation** is a recurring theme: #9872 reveals a critical flaw where bounded delegation leaks file system scope — suggesting deeper sandboxing gaps.
- **Agent durability and traceability** (#9597, #12287) point to a growing need for reliable, auditable agent workflows — especially for multi-hop chains.
- **Dashboard accessibility** (#11984, #12096) reflects user demand for self-service capabilities without admin rights, pushing the project toward granular permission models.
- **Cross-platform CI** (#7461) highlights developer friction, signaling that support for Windows/macOS is becoming a priority for wider adoption.

---

### **5. Bugs & Stability**

⚠️ **Critical Bugs Reported (Severity S2 or Higher)**

| Issue | Description | Severity | Status | Fix PR? |
|------|-------------|----------|--------|--------|
| **#9872** | Bounded delegate targets write to delegator’s workspace instead of own | S2 – degraded behavior | Open | ❌ No fix yet |
| **#12289** | Disabled agents still run turns via operator chat and RPC | S2 – degraded behavior | Open | ❌ No fix yet |
| **#12288** | "Discard all" retains form drafts, leading to data inconsistency | S2 – degraded behavior | Open | ❌ No fix yet |
| **#10501** | MCP tool-result images fail on OpenAI-compatible providers due to invalid role placement | S2 – degraded behavior | Closed | ✅ Fixed in PR #10501 |

🛠️ **Stability Note:** While several S2 bugs remain open, the team has demonstrated responsiveness — e.g., #10501 was resolved quickly after being reported. However, two high-severity issues (#9872, #12289) affecting core security and behavior have not been addressed despite being open since August and November.

---

### **6. Feature Requests & Roadmap Signals**

🚀 **Emerging Priorities for v0.9.0 and Beyond**

| Feature | Description | Status | Likely Inclusion |
|-------|-------------|--------|----------------|
| **Scoped dashboard chat for non-admins** (`#11984`) | Let users chat with agents they’re authorized to use | Open | ✅ High probability |
| **Bounded peer-relay chains** (`#12287`) | Limit recursive agent chains (A→B→A) to prevent infinite loops | Open | ✅ Strong signal |
| **Persistent, attributable peer-agent turns** (`#9597`) | Replace detached `process_message` with tracked, durable tasks | In-progress | ✅ Core v0.9.0 |
| **Zsh grammar-aware command policy** (`#12296`) | Enforce precise POSIX rules in zsh shell contexts | Open | ✅ Likely post-v0.9.0 |
| **Daemon dashboard served via relay** (`#12270`) | Enable browser access to local daemon dashboards | Open | ✅ v0.9.0 candidate |

📌 **Prediction:** The upcoming **v0.9.0 release** will likely center on **enhanced security**, **agent durability**, **cross-platform support**, and **user empowerment** through non-admin dashboard access — with strict cost controls and chain limits for agent interactions.

---

### **7. User Feedback Summary**

🗣️ **Real User Pain Points Observed (from Issues & PR Discussions)**

- **Delegation misbehavior** (#9872): Users expect bounded delegates to operate in isolated workspaces — current behavior violates trust and security assumptions.
- **Inconsistent state after agent rename** (#11301, #11226): Admins report confusion when permission profiles don’t update automatically — undermines trust in config systems.
- **Dashboard limitations**: Non-admin users feel locked out despite having agent access — desire for autonomy and direct interaction.
- **Form persistence bugs** (#12288): UI frustration when “discard all” doesn’t clear draft state — impacts usability and perceived reliability.
- **Windows/macOS CI gap** (#7461): Developers using non-Linux environments face build/test inconsistencies — hinders contribution and testing diversity.

💡 **Satisfaction Indicator:** Positive sentiment in PR reviews and issue acceptance rates suggests users appreciate the project’s direction and technical rigor, even if some UX and edge-case issues persist.

---

### **8. Backlog Watch**

⏳ **High-Impact, Long-Unanswered Items Needing Attention**

| Issue | Summary | Age | Priority | Action Needed |
|------|--------|-----|--------|--------------|
| **#7432** | Runtime, dashboard sign-in, and gateway delivery tracker | 5 months | P2, High Risk | Review roadmap alignment, assign owners |
| **#9597** | Make peer-agent turns durable and attributable | 3 months | P2, High Risk | Finalize design, push to implementation |
| **#12287** | Bound peer-relay chains across recipient turns | 1 day | P2, High Risk | Maintain momentum; review dependency on #9597 |
| **#12296** | Check zsh grammar for command parsing | 1 day | P2, High Risk | Pair with #11994 for consistency |
| **#11313** | Publish CLI config edits to running daemon | 1 month | P1, High Risk | Monitor merge status; ensure testing |

📌 **Recommendation:** Maintainers should prioritize **#7432** and **#9597** to unblock v0.9.0 roadmap items. These are foundational for agent reliability and security. Additionally, **#12296** and **#12287** should be reviewed promptly to avoid delay in chain-limiting and shell safety features.

---

**🔚 Final Assessment:**  
ZeroClaw is in a **strong, active phase of development**, with a clear vision for v0.9.0. The project demonstrates excellent engineering discipline, but faces challenges in **balancing rapid feature rollout with critical bug resolution** and **maintaining consistent user experience across platforms and roles**. With continued focus on security, traceability, and inclusivity, ZeroClaw is poised to become a leading framework for secure, agent-driven AI systems.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*