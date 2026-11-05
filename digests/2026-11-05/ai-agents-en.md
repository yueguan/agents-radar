# OpenClaw Ecosystem Digest 2026-11-05

> Issues: 395 | PRs: 500 | Projects covered: 5 | Generated: 2026-11-05 02:44 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# **OpenClaw Project Digest – 2026-11-05**

---

### **1. Today's Overview**  
The OpenClaw project remains highly active, with **395 open issues** and **500 PRs updated in the last 24 hours**, indicating robust community engagement and rapid development momentum. A new beta release—`v2026.11.5-beta.1`—introduces foundational improvements in knowledge management and data persistence. High-severity bugs (P0/P1) related to memory leaks, crash loops, session instability, and platform-specific failures dominate the issue tracker, signaling ongoing stability challenges. Despite this, feature innovation is accelerating, particularly around agent workflows, security boundaries, and cross-platform reliability.

---

### **2. Releases**  
**✅ `v2026.11.5-beta.1`** — Released on 2026-11-05  
[GitHub Release](https://github.com/openclaw/openclaw/releases/tag/v2026.11.5-beta.1)  

#### **Highlights**
- **Knowledge Scribes**: First-class support for reusable knowledge capture workflows across agents, sessions, and the Control UI. Enables structured, repeatable documentation of agent behavior and insights.  
  - Related: #185583, #185637, #185974, #186270, #186455, #187659 ([@jalehman](https://github.com/jalehman))
- **PostgreSQL Data Plane**: Added PostgreSQL-backed state storage as a new option, improving scalability and persistence for enterprise-grade deployments.  
  - This enables multi-node coordination and high-throughput state management beyond SQLite limitations.

> 🔔 **Migration Note**: Existing SQLite users must manually migrate state using `openclaw doctor --migrate-to-postgres`. No backward compatibility is guaranteed.

---

### **3. Project Progress**  
**✅ Merged / Closed PRs (Today)**  
While no full merge announcements are visible in the top 30 PRs, several critical fixes were closed or marked ready for review:
- **PR #192378** (`fix(storage)`): Forward-porting SQLite admission and lease cleanup fixes from stable to main. Addresses deadlocks during cold DB opens and offline maintenance collisions.  
  [Link](https://github.com/openclaw/openclaw/pull/192378)
- **PR #191830** (`fix(codex)`): Forward-porting thread ownership and cleanup fixes from `2026.10.10` to main. Resolves race conditions in native Codex child turns.  
  [Link](https://github.com/openclaw/openclaw/pull/191830)
- **PR #192606** (`fix(scripts)`): Improved error messaging in `nitSweep` validation to be actionable. Now clearly indicates required schema format.  
  [Link](https://github.com/openclaw/openclaw/pull/192606)

These merges reflect strong focus on **core infrastructure stability**, especially in database handling and native agent execution.

---

### **4. Community Hot Topics**  
Top 5 most commented issues and PRs reveal urgent user pain points:

| Issue/PR | Comments | Severity | Link |
|--------|--------|--------|------|
| **#160959** – Gateway blocks for minutes during plugin capture (regression) | 70 | 🦞 P0 | [Issue #160959](https://github.com/openclaw/openclaw/issues/160959) |
| **#177656** – Native host exec returns shared-state integrity error | 28 | 🦪 P1 | [Issue #177656](https://github.com/openclaw/openclaw/issues/177656) |
| **#170956** – Windows restarts/updates unreliable due to schtasks + plugin shutdown | 28 | 🦪 P1 | [Issue #170956](https://github.com/openclaw/openclaw/issues/170956) |
| **#191944** – macOS `gateway stop` fails with ancestry inspection error | 14 | 🦪 P2 | [Issue #191944](https://github.com/openclaw/openclaw/issues/191944) |
| **PR #193130** – Background replies dropped after reload retires turn registry | 0 (but high-risk) | 🦐 P1 | [PR #193130](https://github.com/openclaw/openclaw/pull/193130) |

> 💡 **Underlying Need**: Users demand **reliable, predictable lifecycle operations**—especially on Windows and macOS—where startup, shutdown, updates, and restarts fail silently or hang indefinitely. The recurring theme is **platform-specific process orchestration flaws** and **incomplete signal handling**.

---

### **5. Bugs & Stability**  
Critical stability issues reported today include:

| Issue | Severity | Impact | Fix PR? | Link |
|------|----------|--------|--------|------|
| **#160959** – Gateway blocks for minutes during large plugin capture | 🦞 P0 | Crash-loop, UX-blocker | ❌ No fix yet | [Issue #160959](https://github.com/openclaw/openclaw/issues/160959) |
| **#154812** – Runaway RSS causes OOM and shutdown timeout | 🦞 P1 | Host-level failure | ❌ Not resolved | [Issue #154812](https://github.com/openclaw/openclaw/issues/154812) |
| **#190307** – Gateway restart leaves agents permanently degraded | 🦞 P1 | Session loss, degraded UX | ❌ No fix | [Issue #190307](https://github.com/openclaw/openclaw/issues/190307) |
| **#185578** – Heap floor ratchets up; strings stay reachable (~25 MiB/h) | 🦐 P1 | Memory leak | ❌ No fix | [Issue #185578](https://github.com/openclaw/openclaw/issues/185578) |
| **#191401** – Signal teardown waits forever if receive never finishes | 🦞 P1 | Hangs on shutdown | ✅ PR #192053 exists | [PR #192053](https://github.com/openclaw/openclaw/pull/192053) |

> ⚠️ **Stability Risk**: Multiple **memory leaks**, **crash loops**, and **hangs on shutdown** persist despite recent fixes. The core runtime appears fragile under sustained load or edge-case scenarios.

---

### **6. Feature Requests & Roadmap Signals**  
Emerging themes in feature requests point toward next-gen capabilities:

| Request | Priority | Key Insight | Potential Inclusion |
|-------|--------|------------|-------------------|
| **#192632** – Claude web search provider (Anthropic server-side) | P3 | Demand for direct Anthropic integration without third-party tools | Likely in v2026.12 |
| **#184803** – Turn off/replace spoken task receipt in Talk calls | P3 | User control over audio feedback | Possible in next minor update |
| **#91676** – Opt-in `skills.workshop.allowSandboxedAgents` | P2 | Sandbox agent empowerment | High priority for security-conscious users |
| **#191513** – Browser pairing via short code approved on host | P2 | Headless Gateway access without secrets | Critical for remote ops |
| **#165685** – Machine-readable reason for yielded collectors | P3 | Diagnostic clarity for automation systems | Foundational for observability |

> 🔮 **Roadmap Signal**: The team is actively building **enterprise-grade observability**, **secure sandboxing**, and **headless access patterns**—indicating a shift toward production deployment readiness.

---

### **7. User Feedback Summary**  
Real-world user pain points are clear and consistent:

- **Windows Users**: "Restarting OpenClaw takes *many* retries. Sometimes it just hangs." — @Rene-F (#170956)  
- **Mac Users**: "I can't stop the gateway—it says 'ancestry could not be inspected'." — @davidchyi-beep (#191944)  
- **Plugin Developers**: "Large plugins make the gateway freeze for minutes at startup." — @obviyus (#160959)  
- **Enterprise Operators**: "We need to know why an agent failed—no diagnostics, just 'UNAVAILABLE'." — @miniteatree (#190307)  
- **Security Teams**: "Sandboxed agents can’t use built-in tools. We need opt-in access." — @rodja (#91676)

> 📌 **Sentiment**: High frustration with **reliability**, **debuggability**, and **platform parity**. However, excitement builds around **knowledge scribing**, **PostgreSQL support**, and **agent autonomy**.

---

### **8. Backlog Watch**  
Critical issues with long-standing status requiring maintainer attention:

| Issue | Age | Status | Reason for Delay |
|------|-----|--------|------------------|
| **#160959** – Plugin capture blocks event loop (70 comments) | 2 months | ❌ Needs-maintainer-review | Regression from 2026.9.6; blocking UX |
| **#177656** – Native host exec integrity error (28 comments) | 2 weeks | ❌ Needs-maintainer-review | Stable version breakage |
| **#150635** – Dreaming deep phase never promotes (24 comments) | 2 months | ❌ Needs-product-decision | Core logic flaw in memory management |
| **#120385** – MCP tools missing in scheduled runs (25 comments) | 3 months | ❌ Needs-maintainer-review | Blocks automation |
| **#161921** – Plugins stuck in circular dependency (17 comments) | 2 months | ❌ Needs-maintainer-review | Blocks upgrades |

> 🛠️ **Urgent Call to Action**: These issues represent **systemic gaps** in **agent lifecycle**, **plugin interoperability**, and **session consistency**. Without dedicated maintainer triage, they risk becoming permanent blockers.

---

### ✅ **Final Assessment**  
OpenClaw is in a **high-growth, high-risk phase**. The addition of **Knowledge Scribes** and **PostgreSQL support** marks significant architectural maturity. However, **stability and reliability remain fragile**, especially on Windows and macOS. The community is vocal, engaged, and demanding better tooling for debugging and operation.  

> **Recommendation**: Prioritize **P0/P1 crash loops**, **shutdown hangs**, and **platform-specific failures** in the next sprint. Invest in **diagnostic visibility** and **automated recovery** to convert user frustration into trust.  

📌 *Project Health Score: 6.8/10 — Innovative but unstable.*

---

## Cross-Ecosystem Comparison

# **Cross-Project Comparison Report: Personal AI Agent Ecosystem – 2026-11-05**

---

### **1. Ecosystem Overview**  
The open-source personal AI assistant and agent ecosystem is entering a phase of rapid maturation, marked by increasing architectural complexity, growing enterprise readiness signals, and heightened focus on stability, security, and observability. Projects are moving beyond basic agent execution to emphasize session persistence, cross-platform reliability, and secure delegation—reflecting real-world deployment needs. While innovation remains strong, especially in knowledge capture and memory systems, persistent stability issues across platforms (particularly Windows and macOS) reveal ongoing technical debt. The landscape is bifurcating: some projects are accelerating feature velocity, while others remain stagnant or focused on foundational hardening.

---

### **2. Activity Comparison**

| Project         | Issues (Open) | PRs Updated (24h) | Release Status       | Health Score (10) |  
|-----------------|---------------|--------------------|----------------------|-------------------|
| **OpenClaw**    | 395           | 500                | `v2026.11.5-beta.1`  | 6.8               |
| **Hermes Agent**| 50            | 50                 | No new release       | 7.5               |
| **IronClaw**    | 1             | 1                  | No new release       | 5.0 (Stable but Dormant) |
| **QwenPaw**     | 10            | 50                 | No new release       | 7.0               |
| **ZeroClaw**    | 21            | 50                 | No new release       | 8.2               |

> ✅ *Note: High PR/issue volume correlates with active development; IronClaw’s minimal activity suggests stagnation.*

---

### **3. OpenClaw's Position**  
OpenClaw stands as the most technically ambitious project in the ecosystem, pioneering **first-class knowledge scribing**, **PostgreSQL-backed state storage**, and **cross-agent workflow reuse**—features that signal a shift toward production-grade agent orchestration. Its **high PR volume (500/24h)** and aggressive beta release cadence indicate a large, engaged community driving rapid iteration. Compared to peers:
- Unlike QwenPaw and ZeroClaw, which focus on platform-specific fixes and security, OpenClaw prioritizes **agent autonomy and system extensibility**.
- It lags behind Hermes Agent and ZeroClaw in session resilience and desktop UX polish.
- Its community size appears larger than average, evidenced by high comment counts (e.g., 70+ on #160959), though this also reflects deep frustration with stability.

> 🔍 **Key Differentiator**: OpenClaw is building an **agent lifecycle framework**, not just a client tool—positioning it for enterprise adoption if stability improves.

---

### **4. Shared Technical Focus Areas**  
Multiple projects converge on critical infrastructure challenges:

| Need                          | Projects Involved                     | Specific Examples |
|-------------------------------|----------------------------------------|-------------------|
| **Session Persistence & Resume** | OpenClaw, Hermes Agent, QwenPaw      | OpenClaw #160959, Hermes #139796, QwenPaw #8565 |
| **Platform-Specific Stability** | OpenClaw, QwenPaw, Hermes Agent      | Windows restart failures (OpenClaw #170956, QwenPaw #8565), macOS shutdown hangs (OpenClaw #191944) |
| **Memory & State Management**   | OpenClaw, QwenPaw, ZeroClaw          | Memory leaks (#185578), stale config after disablement (#8579), shared memory scope breaches (#11198) |
| **Error Visibility & Diagnostics** | OpenClaw, QwenPaw, ZeroClaw          | Poor error messaging (QwenPaw #8580), silent plugin failures (QwenPaw #8567), missing warnings during migration |
| **Secure Delegation & Isolation** | ZeroClaw, OpenClaw, Hermes Agent    | S0-level memory leaks (ZeroClaw #11198), native host exec integrity errors (OpenClaw #177656) |

> 📌 **Pattern**: The ecosystem is collectively addressing **reliability under load**, **platform parity**, and **debuggability**—core barriers to real-world adoption.

---

### **5. Differentiation Analysis**

| Aspect                   | OpenClaw                             | Hermes Agent                         | QwenPaw                              | ZeroClaw                            | IronClaw                      |
|--------------------------|---------------------------------------|--------------------------------------|--------------------------------------|-------------------------------------|-------------------------------|
| **Primary Focus**        | Agent workflows, knowledge management | Session persistence, UX polish       | Plugin reliability, desktop UX       | Security, access control            | Minimal activity              |
| **Target Users**         | Developers, power users, enterprises  | Desktop users, multi-profile teams   | Devs, local agents, plugin creators  | Security-conscious orgs, self-hosters | Niche / inactive              |
| **Architecture**         | Modular, PostgreSQL-ready, agent-first | Profile-centric, checkpoint-driven   | Plugin-heavy, console + desktop      | Identity-aware, RPC-based           | Legacy, low-innovation        |
| **Innovation Signal**    | Knowledge Scribes, Postgres support   | Skill provenance, undo checkpoints   | OpenViking memory, theme tokens      | Append-only event history, Zulip integration | None visible                  |
| **Security Maturity**    | Medium (P0 bugs persist)              | Medium (race conditions)             | Low (silent config failures)         | **High** (S0 fixes in progress)     | Unknown (no updates)          |

> 💡 **Strategic Insight**: ZeroClaw leads in security rigor; OpenClaw leads in functional ambition; QwenPaw excels in plugin extensibility; Hermes Agent focuses on user experience.

---

### **6. Community Momentum & Maturity**  

| Tier                | Projects                                  | Characteristics |
|---------------------|--------------------------------------------|-----------------|
| **Rapid Iteration** | OpenClaw, QwenPaw, ZeroClaw               | >50 PRs/24h, frequent bug reports, feature requests tied to real workflows, active triage |
| **Stabilizing**     | Hermes Agent                               | Focused on core reliability, UI polish, session consistency; fewer features, more fixes |
| **Stagnant**        | IronClaw                                   | Only one PR (dependency bump); no user engagement; risk of abandonment |

> 📈 **Maturity Indicator**: Projects with **high issue-comment density** (e.g., OpenClaw #160959 with 70 comments) show mature usage patterns—users are testing edge cases and demanding transparency.

---

### **7. Trend Signals**  
Based on community feedback and technical direction, key industry trends emerge:

1. **Enterprise Readiness Demand**  
   - Users increasingly require **auditability** (ZeroClaw RFC #10526), **session replay**, and **machine-readable failure reasons** (OpenClaw #165685).  
   - *Value for developers*: Build observability into agent frameworks early—diagnostics are now a competitive differentiator.

2. **Platform Parity is Non-Negotiable**  
   - Repeated failures on **Windows** (OpenClaw #170956, QwenPaw #8565) and **macOS** (OpenClaw #191944) highlight that cross-platform reliability is a baseline expectation—not a nice-to-have.  
   - *Value for developers*: Invest in CI/CD testing across OSes; prioritize process lifecycle handling (start/stop/reload).

3. **Security by Design is Now Expected**  
   - ZeroClaw’s S0 fixes (#11198, #11239) and OpenClaw’s `allowSandboxedAgents` request (#91676) reflect a cultural shift toward **principle-of-least-privilege** and **memory isolation**.  
   - *Value for developers*: Treat access control and data boundaries as first-class concerns from day one.

4. **Agent Autonomy > Manual Control**  
   - Features like **Knowledge Scribes (OpenClaw)**, **skill provenance (Hermes)**, and **per-run sessions (QwenPaw)** indicate a move toward **self-documenting, self-repairing agents**.  
   - *Value for developers*: Design agents to be stateful, introspective, and capable of generating actionable logs.

5. **Headless & Remote Access is Critical**  
   - Demand for browser pairing via short code (Hermes #191513) and headless gateway access (ZeroClaw) shows that **remote agent operation without secrets** is becoming standard.  
   - *Value for developers*: Prioritize secure, zero-trust access patterns over password-based auth.

---

### **Final Recommendation**  
For developers and decision-makers:  
- **Prioritize stability and diagnostics**—especially on Windows/macOS and in session lifecycle handling.  
- **Invest in observability and auditability**—these are now core product requirements, not optional.  
- **Adopt modular, secure architectures**—ZeroClaw’s identity-aware design and OpenClaw’s PostgreSQL support represent future-proof approaches.  
- **Monitor IronClaw closely**—its dormancy may signal broader risks in niche or under-resourced projects.

> 🚀 *The next 6 months will separate agile, resilient ecosystems from those burdened by technical debt.*

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

---

### **Hermes Agent Project Digest — 2026-11-05**

---

#### **1. Today's Overview**  
The Hermes Agent project remains highly active with 50 new issues and 50 updated pull requests in the past 24 hours, reflecting intense development and community engagement. The majority of activity centers on session management, gateway stability, and platform-specific bugs—particularly on Windows and macOS. Despite no new releases, significant progress is being made in core reliability, with several critical fixes targeting session resumption, memory handling, and race conditions. The ecosystem continues to expand through plugin catalog additions and feature enhancements, signaling strong momentum ahead of future milestones.

---

#### **2. Releases**  
*No new releases detected.*  
There are currently **no new versions** published to GitHub or package repositories. The last release (v0.22.1+247.gfb6349f) remains in use, with ongoing improvements focused on stability and compatibility rather than feature delivery. Users should expect a patch release soon to address high-priority bugs like `session.resume` failures and session state corruption.

---

#### **3. Project Progress**  
**Merged/Closed PRs (Today):**  
- ✅ **PR #134601**: *fix(photon): drop chat actions instead of forwarding them as unreadable messages* – Resolves iMessage noise pollution by dropping unsupported events instead of misrouting them as content.  
- 🟡 **PR #148903**: *fix(gateway): end bot-to-bot ping-pong* – Stops infinite loop between bots by disabling runtime notices and reply loops, improving system safety.  
- 🟡 **PR #149206**: *feat(checkpoints): on by default, /undo reverts file edits* – Enhances UX by making checkpoints default-enabled and restoring file changes on undo, resolving long-standing workflow friction.  

These merges reflect a focus on **stability**, **security**, and **user experience consistency**—especially around session integrity and agent autonomy.

---

#### **4. Community Hot Topics**  
Top issues and PRs driving discussion today:

| Issue/PR | Comments | Link | Key Insight |
|--------|---------|------|-----------|
| [#139796](https://github.com/nousresearch/hermes-agent/issues/139796) | 6 | [Bug: Desktop session.resume fails for cold sessions](https://github.com/nousresearch/hermes-agent/issues/139796) | Critical P1 bug affecting desktop users restarting after idle periods; lacks cold-load path for sessions evicted from memory. |
| [#148762](https://github.com/nousresearch/hermes-agent/issues/148762) | 3 | [Race condition: same session ID in two profile stores](https://github.com/nousresearch/hermes-agent/issues/148762) | High-severity race condition causing divergent transcripts across profiles; could lead to data loss or confusion during resume. |
| [#149224](https://github.com/nousresearch/hermes-agent/pull/149224) | N/A | [fix(acp): preserve strict session resume semantics](https://github.com/nousresearch/hermes-agent/pull/149224) | Addresses fundamental inconsistency in session recovery logic—clients can’t distinguish restored vs. recreated sessions. |
| [#149216](https://github.com/nousresearch/hermes-agent/pull/149216) | N/A | [feat(skills): record why a skill exists at create time](https://github.com/nousresearch/hermes-agent/pull/149216) | Direct response to user demand for better auditability and provenance tracking in skill creation. |

> 🔍 **Underlying Need**: Users increasingly rely on **session persistence**, **multi-profile consistency**, and **transparent lineage tracking**—indicating maturity in real-world usage beyond experimentation.

---

#### **5. Bugs & Stability**  
Ranked by severity and impact:

| Severity | Issue | Description | Fix PR? |
|--------|------|-------------|--------|
| 🔴 P1 | [#139796](https://github.com/nousresearch/hermes-agent/issues/139796) | `session.resume` returns `not_found` for cold sessions or those evicted by WS orphan reaper post-restart | ❌ No fix yet; urgent for desktop usability |
| 🔴 P1 | [#148762](https://github.com/nousresearch/hermes-agent/issues/148762) | Same session ID unended in two profile stores → race condition → divergent transcripts | ❌ No fix; high risk of data inconsistency |
| 🟡 P2 | [#148958](https://github.com/nousresearch/hermes-agent/issues/148958) | Kanban worker exits on gateway owner stall → counted as crash → blocks card progression | ✅ Partially addressed via PR #149213 (fencing epoch handoff) |
| 🟡 P2 | [#148742](https://github.com/nousresearch/hermes-agent/issues/148742) | Legacy `state.db` without `INTEGER PRIMARY KEY id` accumulates `NULL` rows → compression fails | ⚠️ Patch pending; requires migration tooling |
| 🟡 P2 | [#148853](https://github.com/nousresearch/hermes-agent/issues/148853) | Liveness watchdog stalls on blocked stderr or cleanup lock | ✅ PR #149214 under review (prevents executor starvation) |

> 💥 **Critical Risk**: Session resilience remains fragile—especially on restart scenarios and multi-profile setups—requiring immediate attention.

---

#### **6. Feature Requests & Roadmap Signals**  
Emerging trends suggest upcoming focus areas:

| Feature Request | Status | Implication |
|----------------|--------|-----------|
| [#149219](https://github.com/nousresearch/hermes-agent/issues/149219) | Open (P2) | User wants **skill provenance tracking** ("why" field); likely to be prioritized in v0.23. |
| [#148739](https://github.com/nousresearch/hermes-agent/issues/148739) | Open (P2) | Plugin-driven profile creation via `mirror_credentials` — signals demand for **self-hosted agent orchestration**. |
| [#149222](https://github.com/nousresearch/hermes-agent/pull/149222) | Merged | Addition of `headroom` plugin (lightweight Hermes keeper for low-RAM devices) — indicates growing interest in **edge deployment**. |
| [#149218](https://github.com/nousresearch/hermes-agent/pull/149218) | Open | Real-time TPS decoding metrics in status bar — shows user desire for **performance transparency**. |

> 📈 **Roadmap Signal**: Expect stronger support for **decentralized, self-managed agents** and **resource-constrained environments** in next cycle.

---

#### **7. User Feedback Summary**  
Real pain points emerging from issue reports:

- **Session Resume Breakage**: Users report losing context after restarts due to missing cold-load paths (#139796). This undermines trust in persistent workflows.
- **Plugin & Config Fragility**: Corrupted `config.yaml` silently re-enables disabled skills (#148812), leading to security and workflow surprises.
- **Platform-Specific Rendering Issues**: On desktop, code blocks lose copy buttons (#141521), dollar signs render escaped (`\$260.02`) (#148859), and image loading fails locally (#148810).
- **UI/UX Inconsistencies**: Retry hints hardcode `Alt+K` instead of platform-aware bindings (#148766), and Git change indicators don’t propagate up folder trees (#148823).

> 👎 **Satisfaction Gap**: While core AI functionality is praised, **desktop reliability and polish** are seen as lagging behind backend robustness.

---

#### **8. Backlog Watch**  
High-impact, long-standing issues needing maintainer attention:

| Issue | Age | Priority | Notes |
|------|-----|----------|-------|
| [#113666](https://github.com/nousresearch/hermes-agent/pull/113666) | 2 months old | P3 | Refactor `useMessageStream` for better testability and maintainability; still open despite clear value. |
| [#148965](https://github.com/nousresearch/hermes-agent/issues/148965) | 1 day old | P3 | Docs conflict: `sessions.md` says compression creates new session, but `configuration.md` says `in_place` keeps ID. Needs clarification. |
| [#148833](https://github.com/nousresearch/hermes-agent/issues/148833) | 1 day old | P3 | New ChatGPT models not appearing in additional profiles — indicates **profile isolation flaws** in provider sync. |
| [#148843](https://github.com/nousresearch/hermes-agent/issues/148843) | 1 day old | P2 | Production patch series showing operational failures in real-world homelab use — suggests need for **hardening docs** and **release testing**. |

> ⚠️ **Maintenance Alert**: Several issues reveal deeper architectural challenges—especially around **session lifecycle**, **cross-profile state**, and **config parsing**—that may require refactoring beyond quick fixes.

---

**Summary**:  
Hermes Agent is in a phase of **intensive stabilization and user-facing polish**, driven by real-world usage in complex, multi-profile environments. While the project is technically robust in its core agent logic, **session persistence, configuration reliability, and desktop UX** remain weak spots. With strong community contributions and clear roadmap signals toward edge deployment and auditability, **v0.23 is poised to deliver major quality-of-life improvements**—if priority bugs are addressed swiftly.

👉 *Track key developments*: [GitHub Repository](https://github.com/nousresearch/hermes-agent)

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

**IronClaw Project Digest – 2026-11-05**

---

### **1. Today's Overview**  
The IronClaw project remains in a state of low activity as of 2026-11-05, with no new issues or releases reported in the past 24 hours. One pull request is open, indicating minimal ongoing development momentum. The absence of closed PRs or resolved issues suggests that core contributions are not currently flowing through the pipeline. The only recent update—a dependency bump via Dependabot—reflects automated maintenance rather than feature development or critical fixes. Overall, the project appears stable but stagnant, with limited community engagement or active development cycles.

---

### **2. Releases**  
*No new releases were published in the last 24 hours.*  
There are currently no release notes or version updates to report. The project maintains its existing release train without incremental improvements or breaking changes.

---

### **3. Project Progress**  
*Only one pull request was updated in the last 24 hours:*  
- **PR #8168**: [chore(deps): bump zip from 8.6.0 to 9.0.5](https://github.com/nearai/ironclaw/pull/8168)  
  - **Status**: Open (merged: 0, closed: 0)  
  - **Summary**: This is an automated dependency update for `zip` (v8.6.0 → v9.0.5), managed by Dependabot. It includes bug fixes related to file mode handling and improved stability in ZIP archive parsing.  
  - **Impact**: Low-risk maintenance; no code changes required from contributors. Expected to improve reliability when handling compressed assets in IronClaw workflows.  
  - **Note**: No associated documentation or migration guide needed—this is a non-breaking patch-level upgrade.

---

### **4. Community Hot Topics**  
*No active issues or high-engagement PRs were observed today.*  
The sole open PR (#8168) has received no comments or reactions (👍: 0), suggesting either lack of visibility or perceived low urgency. Given that it’s a routine dependency update, it may not be drawing attention from users or maintainers. There are no trending discussions or urgent user concerns visible in the issue tracker.

---

### **5. Bugs & Stability**  
*No bugs, crashes, or regressions were reported in the last 24 hours.*  
The current dependency update (zip 9.0.5) addresses known security and stability issues in earlier versions, particularly around file mode handling and malformed ZIP parsing. However, no incidents have been logged in IronClaw’s own error logs or GitHub issue feed, indicating strong system stability at this time. No fix PRs exist for unresolved stability problems.

---

### **6. Feature Requests & Roadmap Signals**  
*No new feature requests or roadmap-related discussions were raised in the last 24 hours.*  
Given the lack of open issues or PRs beyond dependency management, there is no clear signal of emerging user-driven feature demand. However, the presence of a dependency bump for `zip` hints at potential future needs around enhanced file format support (e.g., larger archives, encrypted ZIPs, streaming decompression). These could become focal points in upcoming sprints if users begin reporting performance bottlenecks during asset processing.

---

### **7. User Feedback Summary**  
*No user feedback or pain points were surfaced in the last 24 hours.*  
With zero open issues and no comments on the single active PR, there is no direct evidence of user satisfaction or dissatisfaction. The silence may reflect either high user confidence in the current build or a lack of user interaction with the project. No complaints about performance, usability, or integration were recorded—suggesting either excellent stability or underutilization.

---

### **8. Backlog Watch**  
*No long-standing, high-priority issues are currently flagged.*  
While there are no open issues to review, the fact that all activity is confined to a single automated dependency update raises concern about long-term sustainability. The absence of user-reported bugs, feature requests, or community contributions indicates a possible risk of stagnation. Maintainers should consider proactively reviewing older, unaddressed PRs or issues to prevent erosion of momentum. A health check on the backlog (especially those with >30 days old) is recommended to ensure no critical feedback is being overlooked.

> 🔍 *Suggested Action*: Audit inactive PRs/Issues older than 60 days to identify hidden user needs or technical debt.

---  
**Project Health Score**: 🟡 *Stable but Dormant*  
**Last Updated**: 2026-11-05  
**Source**: [GitHub – nearai/ironclaw](https://github.com/nearai/ironclaw)

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# **QwenPaw Project Digest – 2026-11-05**

---

### **1. Today's Overview**  
The QwenPaw project remains highly active with a strong influx of contributions: **50 pull requests updated in the last 24 hours**, including several high-impact fixes and feature enhancements, and **10 new issues opened or updated**, reflecting ongoing user engagement across diverse deployment environments. The activity is concentrated in core stability (Windows, memory, plugins), UI/UX polish (console, desktop), and platform-specific edge cases (Matrix, AppContainer). While no new releases were published, the rapid PR merge rate indicates imminent updates are likely in the near-term release cycle.

---

### **2. Releases**  
**No new releases** were published as of 2026-11-05. The latest stable version remains `2.3.0b2` (beta), with `2.2.2.post1` still used in some local deployments. No breaking changes have been announced in the current release train, but users on beta channels should expect instability in features like plugin activation, memory indexing, and desktop window behavior.

> 🔗 [GitHub Releases](https://github.com/agentscope-ai/QwenPaw/releases)

---

### **3. Project Progress**  
**Merged/Closed PRs (Today):**
- ✅ **PR #8567** ([fix(plugins): fix platform ambiguous dependency bug](https://github.com/agentscope-ai/QwenPaw/pull/8567)) – Resolves a critical plugin dependency resolution issue that could cause silent failures during startup.
- ✅ **PR #8575** ([fix(desktop): restrict auto-update manifest to stable releases](https://github.com/agentscope-ai/QwenPaw/pull/8575)) – Hardens update safety by preventing pre-release auto-updates in desktop clients, aligning with browser console policy.
- ✅ **PR #8422** ([feat(console): add plugin-facing theme token overrides](https://github.com/agentscope-ai/QwenPaw/pull/8422)) – Enables richer plugin theming via semantic tokens, improving visual consistency and accessibility.
- ✅ **PR #7613** ([feat(memory): add OpenViking memory plugin](https://github.com/agentscope-ai/QwenPaw/pull/7613)) – Finalized integration of an advanced memory backend with automatic recall and search capabilities.

These merges indicate progress in **plugin reliability**, **desktop UX**, and **memory system extensibility**.

---

### **4. Community Hot Topics**  
Top community discussions center on **critical usability regressions** and **platform-specific stability issues**:

- 🚨 **Issue #8581** ([code mode: tool with `name` param fails](https://github.com/agentscope-ai/QwenPaw/issues/8581)) – A **high-severity regression** in `run_code` mode due to argument collision (`name` as both tool name and parameter). Already addressed by **PR #8583** ([fix(code-mode): allow tools with a name parameter](https://github.com/agentscope-ai/QwenPaw/pull/8583)), which is under review. This impacts all code-based agent workflows.
- ⚠️ **Issue #8565** ([Hub Local runtime cannot start on Windows](https://github.com/agentscope-ai/QwenPaw/issues/8565)) – Critical failure during startup on Windows due to missing CPU architecture detection and AppContainer token parsing. Currently blocking local Hub use on Windows; **PR #8577** ([keep AppContainer bridge token as one argv item](https://github.com/agentscope-ai/QwenPaw/pull/8577)) directly addresses the token parsing bug.
- 🔄 **Issue #8538** ([Matrix plugin fails with multiple workspaces](https://github.com/agentscope-ai/QwenPaw/issues/8538)) – Multi-workspace Matrix support is broken due to a shared 10-second startup window race condition. High visibility from users managing multiple accounts.

> 🔗 *Top Issues:* [#8581](https://github.com/agentscope-ai/QwenPaw/issues/8581) | [#8565](https://github.com/agentscope-ai/QwenPaw/issues/8565) | [#8538](https://github.com/agentscope-ai/QwenPaw/issues/8538)  
> 🔗 *Top PRs:* [#8583](https://github.com/agentscope-ai/QwenPaw/pull/8583) | [#8577](https://github.com/agentscope-ai/QwenPaw/pull/8577) | [#8567](https://github.com/agentscope-ai/QwenPaw/pull/8567)

*Underlying Need:* Users demand **reliable multi-platform support**, **predictable plugin behavior**, and **stable code execution environments**—especially for production or complex agent workflows.

---

### **5. Bugs & Stability**  
Critical bugs reported today require urgent attention:

| Severity | Issue | Summary | Fix PR? |
|--------|-------|--------|--------|
| 🔴 **Critical** | [#8565](https://github.com/agentscope-ai/QwenPaw/issues/8565) | Hub Local fails to start on Windows due to AppContainer setup errors | ✅ **PR #8577** (in review) |
| 🔴 **Critical** | [#8581](https://github.com/agentscope-ai/QwenPaw/issues/8581) | Code mode crashes when calling any tool with `name` parameter | ✅ **PR #8583** (in review) |
| 🟡 **High** | [#8580](https://github.com/agentscope-ai/QwenPaw/issues/8580) | Memory tag index returns 503; tags generated but unreadable | ❌ No PR yet |
| 🟡 **High** | [#8579](https://github.com/agentscope-ai/QwenPaw/issues/8579) | Disabling provider leaves stale model config → `ProviderError` | ❌ No PR yet |
| 🟡 **High** | [#8560](https://github.com/agentscope-ai/QwenPaw/issues/8560) | Desktop window is always-on-top, blocking fullscreen apps | ❌ No PR yet |

> 🔗 All issues linked above

These indicate systemic risks in **dependency handling**, **state management**, and **UI lifecycle control**—particularly on Windows and in code-execution contexts.

---

### **6. Feature Requests & Roadmap Signals**  
Emerging roadmap signals show growing demand for:
- **Enhanced memory systems**: PRs like #8532 ([append-only session storage](https://github.com/agentscope-ai/QwenPaw/pull/8532)) and #8578 ([surface stale daily notes](https://github.com/agentscope-ai/QwenPaw/pull/8578)) suggest a shift toward **persistent, structured memory** with better recovery and notification.
- **Improved OS integration**: PR #8460 ([OS-level notifications for approvals](https://github.com/agentscope-ai/QwenPaw/issues/8460)) reflects user desire for proactive alerts—likely to be prioritized in v2.4.
- **Better developer ergonomics**: PR #8557 ([per-run sessions for cron jobs](https://github.com/agentscope-ai/QwenPaw/pull/8557)) aims to prevent history bloat—indicating scaling concerns in automated workflows.

📌 *Predicted in v2.4:* Auto-notification systems, per-job session isolation, enhanced memory indexing, and improved Windows compatibility.

---

### **7. User Feedback Summary**  
Users report **frustration with hidden failures**, **poor error feedback**, and **platform-specific instability**:
- “I can’t send a 4000-character reply to DingTalk — it silently drops formatting.” → [Issue #8278](https://github.com/agentscope-ai/QwenPaw/issues/8278)
- “After disabling a provider, my agent still tries to use it and crashes.” → [Issue #8579](https://github.com/agentscope-ai/QwenPaw/issues/8579)
- “My default agent can’t be edited — even though the API allows it.” → [Issue #8576](https://github.com/agentscope-ai/QwenPaw/issues/8576)
- “I’m stuck in a loop where Matrix doesn’t activate across multiple workspaces.” → [Issue #8538](https://github.com/agentscope-ai/QwenPaw/issues/8538)

💡 *Overall sentiment:* High satisfaction with functionality and extensibility, but **low tolerance for silent failures and poor error messaging**. Users want more **transparency**, **predictability**, and **cross-platform parity**.

---

### **8. Backlog Watch**  
Important long-standing issues requiring maintainer attention:

| Issue | Status | Priority | Notes |
|------|--------|---------|------|
| [#8580](https://github.com/agentscope-ai/QwenPaw/issues/8580) | Open | High | Memory tag index failure blocks agent introspection |
| [#8579](https://github.com/agentscope-ai/QwenPaw/issues/8579) | Open | High | Stale config after provider disablement causes hard crashes |
| [#8560](https://github.com/agentscope-ai/QwenPaw/issues/8560) | Open | High | Desktop window behavior breaks workflow |
| [#8576](https://github.com/agentscope-ai/QwenPaw/issues/8576) | Open | Medium | Console UI inconsistency limits usability |
| [#8538](https://github.com/agentscope-ai/QwenPaw/issues/8538) | Open | High | Multi-workspace Matrix support broken |

> 🔗 [Backlog Dashboard](https://github.com/agentscope-ai/QwenPaw/issues?q=is%3Aopen+label%3A%22Bug%22+sort%3Aupdated-desc)

These issues represent **core UX and stability pain points** that, if unresolved, risk alienating enterprise and power users. Prioritization should focus on **error handling**, **state consistency**, and **platform portability**.

---

**Next Update:** 2026-11-06  
**Maintained by:** AI Agent Ecosystem Analyst  
**Data Source:** GitHub (agentscope-ai/QwenPaw) – 2026-11-05 23:59 UTC

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# **ZeroClaw Project Digest – 2026-11-05**

---

### **1. Today's Overview**  
ZeroClaw (github.com/zeroclaw-labs/zeroclaw) exhibits strong and sustained development momentum as of 2026-11-05, with **50 pull requests** and **21 issues** updated in the last 24 hours—indicating active engineering and community engagement. The project is clearly in a high-intensity phase focused on **security hardening**, **access control refinement**, and **multi-channel integration**, particularly around OIDC, dashboard authentication, and agent isolation. Despite no new releases, multiple critical fixes are being merged ahead of a likely v0.9.1 or v0.9.2 patch release, especially for security vulnerabilities affecting session integrity and admin privilege escalation.

---

### **2. Releases**  
No new releases were published in the last 24 hours.  
> *Note: Several high-severity bugs (e.g., #11198, #11239, #12232) are marked `release:v0.9.0`, suggesting these will be addressed in a forthcoming hotfix release. Maintain backward compatibility expectations but prepare for potential breaking changes in access policies and session scoping.*

---

### **3. Project Progress**  
The following PRs were merged or closed today, advancing core security and stability:

- **PR #12213** ([fix(gateway): enforce the [gateway.pairing_dashboard] pairing limits](https://github.com/zeroclaw-labs/zeroclaw/pull/12213)) – Addresses long-standing security flaw where pairing codes never expired (#9394), now enforcing rate limits and timeouts.
- **PR #11266** ([fix(memory): keep owned-session subagent and pipeline memory private](https://github.com/zeroclaw-labs/zeroclaw/pull/11266)) – Fixes S0-level data leakage risk by ensuring delegated agents don’t inherit owner’s memory scope.
- **PR #11230** ([fix(runtime): withhold session-data tools from non-admin principals](https://github.com/zeroclaw-labs/zeroclaw/pull/11230)) – Reinforces privilege separation; prevents non-admin users from accessing sensitive session metadata.
- **PR #11186** ([feat(rpc): add zeroclaw-rpc-client and an in-process gateway transport](https://github.com/zeroclaw-labs/zeroclaw/pull/11186)) – A foundational architectural improvement enabling faster internal communication and future plugin extensibility.

These represent significant progress in **secure agent delegation**, **identity-aware access control**, and **internal performance optimization**.

---

### **4. Community Hot Topics**  
Top 3 most active items reflect deep user concerns about **security**, **access control**, and **integration flexibility**:

1. **[RFC #10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526)** – *Append-only session event history, deterministic state replay*  
   - **Why it matters**: Users demand auditability and reproducibility of agent behavior across complex workflows. This RFC signals a shift toward immutable execution logs—a key requirement for enterprise compliance and debugging.
   
2. **[Bug #11951](https://github.com/zeroclaw-labs/zeroclaw/issues/11951)** – *gateway WebSocket upgrades fail over built-in TLS listener*  
   - **Why it matters**: Critical for dashboard reliability. High comment count despite low visibility suggests real-world deployment impact, especially for self-hosted setups relying on TLS termination.

3. **[Feature #6437](https://github.com/zeroclaw-labs/zeroclaw/issues/6437)** – *Add Zulip channel (long-poll events MVP)*  
   - **Why it matters**: Reflects growing demand for Slack-like collaboration channels. With Rocket.Chat already merged, Zulip support would expand ZeroClaw’s reach into open-source and privacy-focused teams.

> 🔗 *These topics highlight a maturing ecosystem: users are no longer just asking for features—they’re demanding **auditability**, **reliability**, and **interoperability**.*

---

### **5. Bugs & Stability**  
Critical stability and security issues reported today include:

| Issue | Severity | Status | Fix PR? | Description |
|------|----------|--------|---------|-------------|
| [#12236](https://github.com/zeroclaw-labs/zeroclaw/issues/12236) | S3 | Open | ❌ | Cached HTTP clients shared across test runtimes cause intermittent "dispatch task is gone" errors |
| [#12228](https://github.com/zeroclaw-labs/zeroclaw/issues/12228) | S2 | Open | ❌ | `cron update --agent` reports success but fails to reassign job agent |
| [#12227](https://github.com/zeroclaw-labs/zeroclaw/issues/12227) | S2 | Open | ❌ | Shell approval prompts appear inconsistently due to model-generated “approved” flag |
| [#12225](https://github.com/zeroclaw-labs/zeroclaw/issues/12225) | S2 | Open | ❌ | `glob_search` ignores files outside workspace even when under allowed roots |
| [#12221](https://github.com/zeroclaw-labs/zeroclaw/issues/12221) | S2 | Open | ❌ | Missing warning during `config migrate` for retired `ZEROCLAW_*` env vars |

> ⚠️ **High-risk pattern**: Multiple S2/S3 issues involve **state inconsistency**, **misleading feedback**, or **unexpected behavior** in CLI and tooling—suggesting that testing and error handling need strengthening before v0.9.1.

---

### **6. Feature Requests & Roadmap Signals**  
Emerging roadmap themes from user-driven feature requests:

- **Multi-registry support** ([#12109](https://github.com/zeroclaw-labs/zeroclaw/issues/12109), [#12216](https://github.com/zeroclaw-labs/zeroclaw/pull/12216)): Users want to combine public and private plugin repositories. This is likely to become a **v0.9.1 feature**.
- **Zulip integration** ([#6437](https://github.com/zeroclaw-labs/zeroclaw/issues/6437)): Demand is clear and parallel to existing Rocket.Chat support. Likely to be prioritized next.
- **Long-running shell tasks with async/sync modes** ([#11954](https://github.com/zeroclaw-labs/zeroclaw/issues/11954)): Indicates need for better workflow orchestration—could evolve into a **core runtime enhancement**.
- **Interactive terminal for shell tools** ([#11954](https://github.com/zeroclaw-labs/zeroclaw/issues/11954)): Expected to follow soon after multi-registry support.

> 📌 **Prediction**: The next release (v0.9.1) will likely include **plugin registry multiplexing**, **Zulip channel support**, and **improved cron and shell tooling**, with **security patches** dominating the changelog.

---

### **7. User Feedback Summary**  
Real user pain points emerging from recent activity:

- **Security anxiety**: Multiple S0/S1 bugs involving memory leaks, principal scope loss, and unscoped tool execution indicate users are deeply concerned about **data confidentiality** and **privilege escalation**.
- **CLI UX friction**: Issues like inconsistent approval prompts (`#12227`) and silent failures (`#12228`) suggest CLI feedback loops are not reliable—users feel uncertain about whether actions succeeded.
- **Integration gaps**: Demand for Zulip and XMPP reflects a desire for broader team integration beyond Telegram and web dashboards.
- **Self-hosting challenges**: Issues related to TLS, WebSocket upgrades, and test environment setup point to friction in deploying ZeroClaw in production environments without full infrastructure control.

> ✅ *Users are satisfied with core agent capabilities but express frustration with edge cases, configuration complexity, and lack of observability.*

---

### **8. Backlog Watch**  
Several high-impact, long-standing issues remain unresolved and require maintainer attention:

- **[Bug #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** – *Pairing codes never expire*: Closed but still cited in multiple PRs; **critical security gap** requiring immediate fix validation.
- **[Bug #11198](https://github.com/zeroclaw-labs/zeroclaw/issues/11198)** – *Delegated memory tools lose principal scope*: S0 severity, accepted, yet no fix PR has been merged—**urgent priority**.
- **[Bug #11239](https://github.com/zeroclaw-labs/zeroclaw/issues/11239)** – *Owned sessions reach shared memory plane*: Another S0-level memory isolation failure; fix PR exists but needs review.
- **[RFC #10526](https://github.com/zeroclaw-labs/zeroclaw/issues/10526)** – *Append-only session history*: Accepted, but no implementation PR yet—**architectural cornerstone** for future debugging and compliance.

> 🔍 **Action required**: Maintainers should prioritize reviewing and merging **S0 fixes** (#11198, #11239) and initiating work on **RFC #10526** to enable deterministic replay and audit trails.

---

✅ **Overall Project Health**: **Strong** — Active development, robust security focus, clear roadmap alignment.  
⚠️ **Risks**: Delayed resolution of S0 bugs and lack of release cadence may erode trust in production use.  
🎯 **Next Steps**: Prepare for v0.9.1 release with security patches, multi-registry support, and Zulip channel. Prioritize fixing principal scope and session isolation flaws.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*