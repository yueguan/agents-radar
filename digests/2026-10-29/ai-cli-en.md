# AI CLI Tools Community Digest 2026-10-29

> Generated: 2026-10-29 14:08 UTC | Tools covered: 7

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

# Cross-Tool AI CLI Comparison Report — 2026-10-29

*All figures below are digest-reported observations, not full-repo totals. “N/A” means no count was provided in the digest, not that the channel is inactive. No tool in this set was identified as having Issues/PRs disabled upstream.*

## 1. Ecosystem Overview

AI CLI tools are converging on agentic development workspaces: MCP integration, session persistence, cost/quota visibility, long-running goals, and approval-based safety are now core competitive surfaces. Release cadence is uneven — OpenCode and Qwen Code shipped releases, Codex shipped Rust alphas, while Claude Code, Gemini CLI, Copilot CLI, and Pi shipped nothing in the last 24 hours — but issue traffic remains high across all tools. The most severe recurring risks are silent data/config loss, auth and MCP credential lifecycle failures, prompt-cache/token waste, and Windows/desktop parity gaps. Competition is shifting from raw model access toward operational reliability, enterprise governance, and cross-platform consistency.

## 2. Activity Comparison

| Tool | Issues / issue activity | PRs | Discussions | Release status |
|---|---:|---:|---:|---|
| **Claude Code** | 50 issues updated; top thread #27302 has 289 comments / 435 👍 | 2 updated | N/A | No release |
| **OpenAI Codex** | Total not reported; 10 hot issues highlighted; #36040 has 99 comments | 10 key PRs highlighted | 10 items: 2 Ideas, 3 Q&A, 5 Show-and-tell | 2 Rust alphas: `0.166.0-alpha.6`, `alpha.5.1` |
| **Gemini CLI** | 30 top issues updated in `area/agent` | 10 key PRs highlighted | N/A | No release |
| **GitHub Copilot CLI** | 10 hot issues highlighted; #552 has 31 comments | 0 updated | N/A | No release |
| **OpenCode** | 10 hot issues highlighted; #59387 has 98 comments | 10 key PRs highlighted, plus 4 additional noted | N/A | `v2.2.1`; `@opencode/pty@0.3.2` |
| **Pi** | 10 hot issues highlighted; #7547 has 91 comments | 10 key PRs highlighted | 1 discussion | No release |
| **Qwen Code** | 10 hot issues highlighted; #14724 has 79 comments | 10 key PRs highlighted | N/A | `v0.26.3`, desktop, nightly, TypeScript SDK |

## 3. Shared Feature Directions

- **Cost, quota, and prompt-cache transparency**  
  Claude Code wants persistent usage indicators and fewer avoidable cache writes (#80261, #105564). Codex users want predictable monthly resets (#57967). Copilot CLI reports PRU burn and eager MCP schema injection inflating startup tokens (#4802, #4613). OpenCode has 1.28M cache-write tokens with zero reads (#48542) and a 98-comment free-tier quota thread (#59387). Pi reports prompt-cache invalidation from tool ordering (#11529) and missing Bedrock cache metadata (#11528). Gemini CLI is targeting token-frugal context management (#19561).  
  **Shared need:** visible spend, cache-hit/miss metrics, bounded image/context retention, and controls that prevent invisible token waste.

- **Session identity, memory, and lifecycle control**  
  Claude Code needs path-independent project identity and safer retention (#101522, #41458, #74682). Codex is adding message editing/undo and chat navigation (#11086, #2379, #8582). Gemini CLI is hardening session-history preservation and cancellation semantics (#30019, #30017). OpenCode is exploring persistent memory (`/teach`, `/recall`, `/memory`) and now ships `/goal`. Pi is fixing default session-directory and `/resume` semantics (#11561, #11562). Qwen Code is polishing session overview, archive, export, and restore flows.  
  **Shared need:** durable, portable sessions; explicit retention; cross-machine project identity; predictable resume/archive behavior.

- **MCP supervision and auth lifecycle**  
  Claude Code has auth/MCP reliability issues around 401 helper calls and worktree-scoped MCP server reaping (#105408, #105857). Copilot CLI has MCP OAuth DCR expiry, silent auth failures, and GHE proxy gaps (#4017, #5284, #5286). OpenCode needs MCP pre-spawn/warm-up and reconnect (#48743). Codex is adding per-chat leases for shared MCP connections (#57978). Pi sees MCP reconnects invalidating prompt cache through tool-order changes (#11529).  
  **Shared need:** MCP is moving from “support it” to “supervise it” — warm-up, reconnect, credential rotation, lease isolation, and enterprise proxy compatibility.

- **Enforceable safety and permission controls**  
  Codex users report prompt-level prohibitions being violated and want enforceable file restrictions (#57827); Codex PRs add Guardian review and fresh human approval checkpoints. Gemini CLI has maintainer-authored asks to discourage destructive git operations and improve sandboxing (#22672, #19873). Copilot CLI requests default-deny with explicit allowlists in non-interactive mode (#5275). OpenCode has a silent `$HOME` fallback when project directories fail (#59817). Pi wants private `0700` session directories (#10770).  
  **Shared need:** policy-level sandboxing, file-scope enforcement, approval checkpoints, and loud failure instead of silent fallback.

- **Platform and observability parity**  
  Claude Code lacks OpenTelemetry on the web surface and has Windows Remote Control regressions (#32364, #105658). Codex has recurring Windows desktop regressions (#50704 and related). Gemini CLI has Wayland and Windows/PowerShell issues. Copilot CLI has Windows clipboard, PowerShell env inheritance, and sandbox preflight failures. OpenCode has a Windows background-service cluster. Pi’s 91-comment Windows thread is the clearest platform-fragmentation signal. Qwen Code has CI/Web Shell and Remote-SSH coverage gaps.  
  **Shared need:** first-class Windows/Wayland support, desktop/web parity, and telemetry that works across CLI, web, and IDE surfaces.

- **Long-running agent control and observability**  
  OpenCode shipped `/goal` but also has a 500+ step runaway loop report (#49042). Gemini CLI’s top issue is subagent MAX_TURNS reported as success (#22323), plus generalist-agent hangs (#21409). Codex is adding Guardian review and approval checkpoints. Claude Code has routine-session sprawl and hanging cloud routines.  
  **Shared need:** step/budget ceilings, cancellation that actually stops child processes, subagent trajectory visibility, and reviewer-verified completion rather than optimistic status reporting.

## 4. Differentiation Analysis

- **Claude Code** targets enterprise and high-volume power users. Its strongest signal is identity, MCP/auth reliability, OTel parity, Connector multi-account support, and cost control. It has the heaviest issue engagement in this set but low PR throughput today, suggesting a mature product with a large reliability backlog.

- **OpenAI Codex** spans CLI, desktop, cloud, and mobile remote control. Its technical direction includes Rust alpha builds, Guardian review/approval infrastructure, MCP connection leases, and Code Mode output truncation. It is especially focused on policy enforcement and desktop/remote workflows, but image-heavy context and Windows desktop regressions are major friction points.

- **Gemini CLI** is the most explicitly subagent- and orchestration-centric. It is investing in AST-aware codebase tooling, token-frugal context management, sandboxing, and subagent observability. Its weakness is terminal-state correctness: false success reporting, indefinite hangs, and non-interactive `-p` mode reliability directly threaten CI/scripting use.

- **GitHub Copilot CLI** targets GitHub enterprise workflows. Differentiation is ACP protocol completeness, MCP auth, GHE/proxy behavior, permission semantics, and BYOK/config introspection. It has strong enterprise pain around auth persistence — Entra, macOS keychain, Ubuntu keychain, and MCP OAuth — and zero PR updates today, with fixes landing through other channels.

- **OpenCode** is the fastest-shipping open-source-style option in this set. `v2.2.1` adds persistent `/goal` loops, while PRs add Code Mode predicate gates, Mermaid rendering, message-action menus, and protocol/CI fixes. Its adoption friction is Zen free-tier quota state, cache economics, and Windows background-service reliability.

- **Pi** is the most terminal/extension-focused tool. Its community emphasizes Windows usability, fullscreen TUI performance, extension API parity, session-storage semantics, and provider routing for Bedrock/Anthropic-compatible traffic. A community Rust port (`pi-rs`) signals demand for a non-Node runtime and behavioral parity testing.

- **Qwen Code** targets Qwen and local/OpenAI-compatible backends, with CLI, Desktop, Web Shell, and TypeScript SDK surfaces. It is shipping releases and managed-session permissions, but its issue tracker is dominated by CI attribution noise, release-host capacity, and a new P1 config regression where `contextWindowSize` is silently removed.

## 5. Community Momentum & Maturity

**Highest visible engagement:** Claude Code has the largest single thread (#27302, 289 comments) and 50 updated issues. Codex has the highest-comment new-regression signal (#36040, 99 comments). OpenCode’s quota thread (#59387, 98 comments), Pi’s Windows thread (#7547, 91 comments), and Qwen’s CI gate thread (#14724, 79 comments) are the loudest per-project signals.

**PR throughput:** Codex, Gemini CLI, OpenCode, Pi, and Qwen Code each highlighted roughly 10 key PRs. Claude Code had 2 updated PRs; Copilot CLI had 0. This does not mean Claude/Copilot are inactive — both show heavy triage and issue activity — but their visible fix throughput is lower in this window.

**Rapid iteration:** OpenCode and Qwen Code are shipping releases while actively changing UX and infrastructure. Codex is iterating through Rust alphas and safety/review PRs. Gemini CLI is in a substantial fix wave around session history, cancellation, shell security, and tool-call correctness. Pi is landing many focused maintenance fixes but had no release.

**Maturity profile:** Claude Code, Codex, and Copilot CLI have enterprise-scale adoption but still show serious auth, identity, proxy, and platform-parity debt. OpenCode and Qwen Code are iterating quickly but show quota, CI, and release-infrastructure maturity gaps. Pi is community-driven and maintenance-heavy, with platform fragmentation as its main ceiling. Gemini CLI’s agent architecture is ambitious, but subagent reliability and non-interactive mode are not yet stable enough for conservative CI adoption.

## 6. Trend Signals

- **Agent loops are becoming first-class, and so are their safety rails.** OpenCode’s `/goal` launch alongside a 500+ step runaway report shows the pattern: persistent objectives require budget ceilings, pause/resume, reviewer verification, and cancellation guarantees. Gemini’s false `GOAL`/success reports and Codex’s Guardian approval work point the same direction.

- **Context is now a cost, correctness, and reliability surface.** Image retention, auto-compaction, prompt-cache invalidation, startup schema injection, and session-start context rewrites are all recurring cost leaks. Developers should expect cache metrics, context-budget controls, image pruning, and compaction observability from serious tools.

- **MCP has shifted from integration feature to operational burden.** The next competitive differentiator is MCP lifecycle management: warm-up, reconnect, OAuth/DCR expiry, token rotation, per-chat leases, and enterprise proxy support. Silent MCP failures are especially damaging because users cannot self-diagnose them.

- **Identity and session portability are enterprise blockers.** Multi-account Connectors, path-independent project identity, cross-machine memory, `/resume` visibility, and retention guarantees are recurring asks. Tools that key sessions to filesystem paths or local keychains will struggle in containerized, multi-worktree, and multi-machine workflows.

- **Safety must be enforceable, not prompt-level.** Across Codex, Gemini, Copilot, OpenCode, and Pi, users are asking for sandboxing, default-deny permissions, file-scope restrictions, approval checkpoints, and loud failures. Prompt instructions alone are no longer trusted as guardrails.

- **Platform parity is a adoption gate.** Windows, Wayland, desktop background services, terminal rendering, and enterprise auth/proxy behavior generate disproportionate pain. Teams evaluating AI CLIs should include Windows and enterprise-proxy test matrices, not just macOS/Linux happy paths.

- **Provider and local-model flexibility matters.** Qwen Code’s local Ollama `contextWindowSize` regression, Pi’s Bedrock/Mantle and Anthropic routing work, Copilot CLI’s BYOK/config requests, and OpenCode’s proxy configuration demand all show that users want model/provider choice without hidden config semantics.

**Reference value for developers:** prioritize observable, cancellable agent loops; measurable context and cache economics; durable session identity; managed MCP lifecycle; enforceable permission policy; and cross-platform/enterprise-auth testing. These are now stronger predictors of production readiness than feature-count comparisons alone.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report
**Source:** github.com/anthropics/skills · **Data as of 2026-10-29**

> **Data caveat:** the PR export shows `Comments: undefined` and `👍: 0` for all 20 PRs, so ranking below uses observable proxies — cross-referenced issues, revision/update recency, and the centrality of the skill being touched. All 20 PRs in the export are **OPEN**; no merges are recorded in this snapshot.

---

## 1. Top Skills Ranking (most-discussed PRs)

**1. [PR #1298](https://github.com/anthropics/skills/pull/1298) — `skill-creator`: isolate trigger evals, fix Windows & runtime failures**
*Author: MartinCajiao · Created 2026-06-10 · Updated 2026-09-16 · OPEN*
Touches the meta-skill that generates and evaluates every other Skill. Fixes false trigger-eval misses caused by competing per-worker probes, `select()` on subprocess pipes failing on Windows, and unrelated tools aborting scans — critically, runtime failures currently register as "non-triggers," which silently *passes* negative examples and corrupts optimization. This is the direct counterpart to Issue [#556](https://github.com/anthropics/skills/issues/556) (0% trigger rate), making it the highest-leverage PR in the queue. **Status: open, long-running (~3 months), still revised.**

**2. [PR #1742](https://github.com/anthropics/skills/pull/1742) — `mcp-builder`: support `mcp>=2` `streamable_http_client` + custom headers**
*Author: Kuldeeep18 · Created 2026-09-08 · Updated 2026-10-08 · OPEN*
In `mcp>=2.0.0`, `streamablehttp_client` was renamed to `streamable_http_client`, and custom headers moved to `create_mcp_http_client` / `http_client` instead of a direct kwarg. Closes [#1668](https://github.com/anthropics/skills/issues/1668). A hard breakage for anyone scaffolding MCP servers on current SDKs — this is ecosystem-compatibility maintenance, not a cosmetic fix. **Status: open, actively revised.**

**3. [PR #1980](https://github.com/anthropics/skills/pull/1980) — `webapp-testing`: remove `shell=True` from `with_server.py`**
*Author: Pcmhacker-piro · Created 2026-10-06 · Updated 2026-10-25 · OPEN*
Removes `subprocess.Popen(..., shell=True)` used to support `cd <dir> && <command>` chains, closing a command-injection vector (CWE-78) reachable from unsanitized agent/user command strings. The most recently updated PR in the set and part of a clear security-hardening wave (see Issues #492, #1394). **Status: open, recent activity.**

**4. [PR #1997](https://github.com/anthropics/skills/pull/1997) — `skill-creator`: parse YAML frontmatter scalars correctly**
*Author: BichengWang · Created 2026-10-10 · Updated 2026-10-19 · OPEN*
`parse_skill_md` manually strips quotes and joins continuation lines, so eval and description-optimization commands receive metadata that diverges from the actual `SKILL.md` — e.g. `"review my draft."` loses its closing quote and escapes. Silent metadata drift degrades description optimization. **Status: open, recent.**

**5. [PR #1681](https://github.com/anthropics/skills/pull/1681) — `skill-creator`: direct execution of `package_skill.py`**
*Author: Kuldeeep18 · Created 2026-08-27 · Updated 2026-10-24 · OPEN*
`python skills/skill-creator/scripts/package_skill.py <path>` fails with `ModuleNotFoundError: No module named 'scripts.quick_validate'`; docstrings and CLI help also carry stale paths. Blocks the standalone packaging workflow that most third-party Skill authors use. **Status: open, actively revised.**

**6. [PR #525](https://github.com/anthropics/skills/pull/525) — Add `pyxel` skill for retro game development**
*Author: kitao · Created 2026-03-05 · Updated 2026-10-23 · OPEN*
Guides creation, debugging, and verification of Python retro games: headless input-driven runs, direct frame inspection, task-specific state checks, plus references on Pyxel behavior and presentation. Authored by Pyxel's own maintainer. The longest-lived and still-updated new-Skill PR in the set. **Status: open, ~8 months old, still maintained.**

**7. [PR #1245](https://github.com/anthropics/skills/pull/1245) — Add `notion-spec-to-implementation` + `quantitative-resume-auditor`**
*Author: mrdesouzaphd-cmyk · Created 2026-06-02 · Updated 2026-10-15 · OPEN*
Bundles two Skills: one decomposes Notion product/tech specs into implementation tasks with acceptance criteria and progress tracking; the other audits resumes against quantitative criteria. Represents the "spec → executable task graph" workflow-automation direction. **Status: open, recently updated.**

**8. [PR #1975](https://github.com/anthropics/skills/pull/1975) / [#1976](https://github.com/anthropics/skills/pull/1976) / [#1977](https://github.com/anthropics/skills/pull/1977) — correctness trio**
*Author: gerardrecinto · Created 2026-10-06 · Updated 2026-10-18–10-27 · OPEN*
Three small, well-scoped fixes landing together: `pptx` rejects `--cols < 1` at argparse instead of after a LibreOffice render (fixes #1809); `webapp-testing` reports `textarea`/`select` tags correctly instead of as `text` (fixes #1891); `algorithmic-art`'s `wrapAround()` now actually wraps negatives via modulo (fixes #1897). Signals a contributor doing disciplined issue-triage cleanup. **Status: all open.**

**Also notable:** [#1792](https://github.com/anthropics/skills/pull/1792) (`docx`: report LibreOffice timeout as an error and verify output), [#1734](https://github.com/anthropics/skills/pull/1734) (detect orphaned docx comments), [#538](https://github.com/anthropics/skills/pull/538) (case-sensitive file references in `pdf/SKILL.md` — an 8-month-old one-line-class fix), [#514](https://github.com/anthropics/skills/pull/514) (`document-typography`: orphan/widow control, numbering alignment).

**Pattern:** the top of this ranking is dominated by *fixes to bundled Skills* — 4 of the top 5 touch `skill-creator`, `mcp-builder`, or `webapp-testing` — not by net-new Skill submissions.

---

## 2. Community Demand Trends (from Issues)

**① Trust boundary & namespace security — the single loudest thread**
[Issue #492](https://github.com/anthropics/skills/issues/492) (46 comments, 3 👍) is by far the most-discussed item in the entire dataset: community Skills distributed under the `anthropic/` namespace impersonate official Anthropic Skills, so users grant elevated permissions to code they believe is first-party. Related hardening: [#1394](https://github.com/anthropics/skills/issues/1394) (display-path XSS in the skill-creator eval-viewer via non-attribute-safe `escapeHtml`).

**② Evaluation & triggering reliability**
[Issue #556](https://github.com/anthropics/skills/issues/556) (13 comments, 7 👍): `run_eval.py` yields a **0% trigger rate across all queries** — no query ever invokes the `Skill`/`Read` tool. [#1390](https://github.com/anthropics/skills/issues/1390): `mcp-builder/evaluation.py` fabricates a tool error for every call and scores 0/N invisibly. Demand: make the measurement layer truthful before scaling Skill authoring.

**③ Distribution & org-level sharing**
[Issue #228](https://github.com/anthropics/skills/issues/228) (17 comments, 8 👍): users must download `.skill` files, Slack them, and have colleagues manually upload via Settings → Capabilities. A shared library or share link is requested. [#189](https://github.com/anthropics/skills/issues/189) (9 👍): `document-skills` and `example-skills` ship identical content, causing duplicate Skills and wasted context.

**④ Context/token economics**
[Issue #1487](https://github.com/anthropics/skills/issues/1487): `claude-api` eagerly injects **~156k tokens**, exhausting the window in one tool call. [#202](https://github.com/anthropics/skills/issues/202): `skill-creator` reads as developer documentation rather than an operational skill — verbose tone is a token-efficiency tax. Demand is shifting from "does it work" to "does it fit."

**⑤ Metadata/discovery quality**
[Issue #2064](https://github.com/anthropics/skills/issues/2064): `theme-factory`'s description says *what* the skill does but not *when to use it* — a systemic frontmatter-authoring gap that directly limits trigger rates.

**⑥ Proposed new Skill directions (directional signal, lower volume)**
[#1329](https://github.com/anthropics/skills/issues/1329) `compact-memory` (symbolic notation for compact agent state); [#412](https://github.com/anthropics/skills/issues/412) `agent-governance` (policy enforcement, threat detection, trust scoring, audit trails); [#1385](https://github.com/anthropics/skills/issues/1385) Reasoning Quality Gate Pipeline (pre-task calibration → adversarial review → delivery verification). Common thread: **agent self-management and output quality control**, not domain tooling.

---

## 3. High-Potential Pending Skills (new Skills not yet merged)

| PR | Skill | Updated | Why it may land |
|---|---|---|---|
| [#525](https://github.com/anthropics/skills/pull/525) | `pyxel` — retro game dev | 2026-10-23 | Maintainer-authored, continuously updated for ~8 months, concrete verification methodology (headless frame inspection) |
| [#1245](https://github.com/anthropics/skills/pull/1245) | `notion-spec-to-implementation` + `quantitative-resume-auditor` | 2026-10-15 | Two-for-one bundle; spec→task decomposition is a high-leverage workflow gap |
| [#1703](https://github.com/anthropics/skills/pull/1703) | `md2video-audio` — Markdown → MP4 with voiceover | 2026-09-15 | Fully specified pipeline (Marp slides → TTS → render); zero-cost positioning |
| [#1771](https://github.com/anthropics/skills/pull/1771) | `proofcore-contract-auditor` — Solidity/Rust static analysis | 2026-09-16 | Only Web3/Smart-contract Skill proposed; vendor-backed, but TON/Merkle anchoring raises scope-review questions |
| [#1615](https://github.com/anthropics/skills/pull/1615) | `scnet-hpc` — SSH/Slurm HPC operations | 2026-08-24 | Profile-based cluster ops; niche but well-scoped, activity has cooled |
| [#486](https://github.com/anthropics/skills/pull/486) | `odt` — OpenDocument create/fill/parse | 2026-04-14 | Fills a real gap vs. the existing `docx`/`pptx`/`pdf` trio; **stalled ~6 months**, needs a rebase to revive |

**Stalled-but-valuable:** [#514](https://github.com/anthropics/skills/pull/514) `document-typography` addresses a defect class affecting *every* generated document (orphan word wrap, widow headers, numbering misalignment) yet has been quiet since 2026-03-13; [#210](https://github.com/anthropics/skills/pull/210) rewrites `frontend-design` for single-conversation actionability.

**Review-bandwidth bottleneck:** the oldest open fix in this set is [#538](https://github.com/anthropics/skills/pull/538) (2026-03-06) — eight case-sensitivity mismatches breaking `pdf/SKILL.md` on case-sensitive filesystems. Trivial to merge, open ~8 months. This suggests merge throughput, not contributor supply, is the constraint.

---

## 4. Skills Ecosystem Insight

> **The community's demand is concentrated on the reliability and trustworthiness of the Skills toolchain itself — that `skill-creator`'s evaluation harness actually triggers and scores honestly, that bundled Skills keep working against current SDKs, and that the `anthropic/` namespace can't be impersonated — rather than on any new Skill category.**

---

*Methodology note: PR comment counts and reaction counts were unavailable (`undefined` / `0`) in the source export, so PR ranking reflects cross-referenced issues, update recency, and skill centrality. Issue-based trends are ranked by actual comment counts.*

---

# Claude Code Community Digest — 2026-10-29

## 1. Today's Highlights

No releases shipped in the last 24 hours, but issue activity is heavy (50 issues updated) and skews toward **auth/MCP reliability and cost control**. The long-running multi-account Connector request (#27302) remains the community's most-engaged thread at 289 comments and 435 👍, while a new data-loss report (#41458) and a prompt-cache cost regression (#105564) are drawing fresh attention from power users. Web/desktop parity gaps — OpenTelemetry configuration, routine session lifecycle, and Windows Remote Control — round out today's signal.

## 2. Releases

No new releases in the last 24 hours.

## 3. Hot Issues

1. **[#27302](https://github.com/anthropics/claude-code/issues/27302) — Support multiple Connector accounts (OPEN, 289 comments, 435 👍)**
   The single highest-engagement issue in the repo. Users want the same Connector bound to multiple accounts simultaneously in Claude and Claude Code on the web. The volume of 👍 signals this is a blocker for agencies and multi-tenant developers.

2. **[#41458](https://github.com/anthropics/claude-code/issues/41458) — `cleanupPeriodDays: 99999` ignored, 490 sessions deleted (OPEN, regression, data-loss)**
   An explicit retention setting is being silently overridden, destroying session history on macOS. Labeled `data-loss` + `regression`, this is the most severe correctness issue in today's batch — it undermines trust in any config-driven retention guarantee.

3. **[#80261](https://github.com/anthropics/claude-code/issues/80261) — Persistent usage-limit indicator in desktop app (OPEN, 12 comments, 29 👍)**
   Usage limits are buried; developers want an always-visible quota signal on the main screen. Steady support reflects how cost anxiety now shapes day-to-day workflow.

4. **[#32364](https://github.com/anthropics/claude-code/issues/32364) — OpenTelemetry configuration in Claude Code on the Web (OPEN, 9 comments, 37 👍)**
   OTel is available in the CLI but not in the web surface, breaking observability parity for teams that standardize on claude.ai/code. High 👍-to-comment ratio suggests quiet but broad enterprise demand.

5. **[#101522](https://github.com/anthropics/claude-code/issues/101522) — Path-independent project identity + memory sync (OPEN, 8 comments)**
   Projects are keyed by filesystem path slug, so the same repo appears as several unrelated projects across machines, containers, and worktrees. A foundational ask that would fix a whole class of memory fragmentation bugs.

6. **[#95764](https://github.com/anthropics/claude-code/issues/95764) — Opus 5 returns inter-tool prose as summarized `thinking` blocks (OPEN, 8 comments)**
   Since 2026-09-11, prose between tool calls arrives as a server-written summary, dropping code blocks and leaving the model believing the user saw content they never did. A subtle model/TUI contract regression with real debugging consequences.

7. **[#74682](https://github.com/anthropics/claude-code/issues/74682) — Auto-archive routine run sessions (OPEN, 8 comments)**
   Hourly routines generate ~18 stale tabs per day with no cleanup. Small ergonomics request, but the volume-driven tab sprawl makes desktop routines hard to use at scale.

8. **[#105564](https://github.com/anthropics/claude-code/issues/105564) — Second turn re-writes ~17k session-start context to prompt cache (OPEN, 2 comments)**
   Every new session pays one avoidable full-context cache write. Low comment count but high practical impact — this is a direct, recurring cost leak on Windows.

9. **[#105408](https://github.com/anthropics/claude-code/issues/105408) — `apiKeyHelper` not re-run on 401 under Vertex with `SKIP_VERTEX_AUTH=1` (OPEN, 27 👍)**
   Gateway setups lose credential rotation: an expired token never triggers the helper. The 27 👍 on a one-day-old issue shows how many teams run Vertex through gateways.

10. **[#105857](https://github.com/anthropics/claude-code/issues/105857) — Project-scoped stdio MCP server killed as stale in git worktree sessions (OPEN)**
    MCP servers connect successfully (~0.4s) then get reaped ~2.5s later under `.claude/worktrees/<name>`. Blocks a common agentic-worktree workflow and points at a path-identity bug adjacent to #101522.

*Also worth watching:* [#105958](https://github.com/anthropics/claude-code/issues/105958) (cloud routines hang on unanswered `control_request` — 0/4 runs execute), [#105658](https://github.com/anthropics/claude-code/issues/105658) (Windows Remote Control sessions exit ~4s after spawn since CLI 2.1.308), [#101916](https://github.com/anthropics/claude-code/issues/101916) (no integrity protection for resumed session transcripts).

## 4. Key PR Progress

Only **2 pull requests** were updated in the last 24 hours, so the full set is listed below rather than a top 10.

1. **[#32751](https://github.com/anthropics/claude-code/pull/32751) — `fix(hookify)`: map stop/prompt events to correct field names (OPEN)**
   Fixes #32153. Hookify rules using the simple `pattern` syntax with `event: stop` or `event: prompt` never fire, because `config_loader.py` maps them to `field='content'`, which `rule_engine._extract_field()` cannot resolve for Stop/UserPromptSubmit events. Restores a silently dead hook path.

2. **[#32755](https://github.com/anthropics/claude-code/pull/32755) — `feat(plugins)`: add edit-verifier plugin for post-edit verification (OPEN)**
   Addresses #32658. Adds a `PostToolUse` hook that verifies Edit-tool outcomes instead of assuming success — catching whitespace mismatches and already-modified targets that currently fail silently while the model proceeds on a false premise.

## 5. Hot Discussions

No discussion data was provided for this period; section omitted.

## 6. Feature Request Trends

- **Multi-account & identity flexibility** — Binding multiple accounts to one Connector (#27302), path-independent project identity (#101522), and cross-machine memory sync form a single "identity is too rigid" cluster.
- **Observability parity across surfaces** — OpenTelemetry in Claude Code on the Web (#32364) is the leading example of CLI capabilities missing from the web/cloud runtime.
- **Cost & quota transparency** — Persistent usage indicators (#80261), session-limit vs. Agent SDK accounting (#104738), and cache-write waste (#105564) all ask for the same thing: make spend legible and predictable.
- **Session lifecycle control** — Auto-archive for routine runs (#74682), integrity protection for resumed transcripts (#101916), and non-destructive retention (#41458).
- **Desktop/keybinding customization** — Rebinding or disabling ⌘K (#105956) and persistent quota UI signal a push for a configurable desktop surface.
- **Web/cloud reliability** — Routines that hang on permission control requests (#105958) and Android download naming (#105955).

## 7. Developer Pain Points

- **Silent data loss beats performance concerns.** `cleanupPeriodDays` being ignored (#41458) and unverified transcript integrity (#101916) erode the assumption that local session history is durable.
- **Auth and token state is fragile around MCP.** Stale `needs_auth` states clearing shared Keychain tokens (#105957), refresh-token corruption on macOS (#105456), and `apiKeyHelper` not firing on 401 (#105408) all point to a credential lifecycle that doesn't survive retries, gateways, or multi-session sharing.
- **Cost leaks are small, frequent, and invisible.** A 17k-token cache rewrite on turn two (#105564) and token spikes on failed MCP calls to disabled servers (#105670) are the kind of per-session overhead users cannot see or prevent.
- **Model instruction adherence degrades over long sessions.** Reports of CLAUDE.md rules being ignored (#105604, closed as duplicate), scope creep (#103836), and memory rules followed only ~72% of the time when just the MEMORY.md index is in context (#105867) suggest memory architecture, not prompt wording, is the bottleneck.
- **MCP lifecycle breaks in non-standard repo layouts.** Worktree-scoped stdio servers being reaped (#105857) and the built-in `claude-artifacts` server 404ing on every session start (#105488).
- **Platform-specific regressions keep landing on Windows.** Remote Control session exits (#105658), Cowork rejecting plain-ASCII path handling (#105954), and the cache regression above all share a `platform:windows` label.
- **Long-tail process pain:** bot-closed duplicate reports without fixes (#95268, reopened slash-menu regression) remain a recurring community complaint about triage quality.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-29

## 1. Today's Highlights

Two Rust alpha builds shipped (`0.166.0-alpha.6` / `alpha.5.1`) with no detailed changelog. Community attention is dominated by a cluster of image-heavy context and compaction failures causing WebSocket disconnects, oversized requests, and repeated auto-compaction across macOS and Windows. Windows desktop regressions and long-standing UX requests (message editing, undo/redo, chat search) remain highly visible.

## 2. Releases

- **rust-v0.166.0-alpha.6** — [Release](https://github.com/openai/codex/releases/tag/rust-v0.166.0-alpha.6)  
  Alpha release only; no release notes beyond the version bump.
- **rust-v0.166.0-alpha.5.1** — [Release](https://github.com/openai/codex/releases/tag/rust-v0.166.0-alpha.5.1)  
  Alpha patch release; no substantive changelog provided.

No stable release was published in the last 24 hours.

## 3. Hot Issues

1. **[#36040](https://github.com/openai/codex/issues/36040) — iOS Remote only lists projects with recent chats**  
   A high-engagement regression with 99 comments. iOS Remote pairing appears to filter project discovery by recent chat activity, which breaks access to older projects. This is the most-discussed issue in the window.

2. **[#25828](https://github.com/openai/codex/issues/25828) — Phone verification cannot send codes**  
   A login-blocking auth bug affecting users in Indonesia, with 37 comments and reports across multiple numbers. Authentication failures have outsized impact because they prevent any Codex usage.

3. **[#33493](https://github.com/openai/codex/issues/33493) — Local compaction v2 retains unbounded `input_image` payloads**  
   Image-heavy threads repeatedly auto-compact because raw image payloads remain in replacement history. This is a representative issue for a broader image/context retention problem affecting desktop sessions.

4. **[#11086](https://github.com/openai/codex/issues/11086) — App: message editing**  
   The highest-voted request in the dataset (69 👍). Users want Cursor-style message editing with undo of subsequent edits, indicating strong demand for more iterative conversational workflows.

5. **[#2379](https://github.com/openai/codex/issues/2379) — Undo/redo typing in the TUI**  
   A long-standing request (38 👍) for `Cmd-Z` / `Shift-Cmd-Z` in the prompt editor. Small ergonomic gap, but persistent community demand.

6. **[#50704](https://github.com/openai/codex/issues/50704) — Windows dot rejects existing local chat IDs**  
   The cloud reader rejects valid local chat IDs while a native local reader succeeds, suggesting route/format incompatibility. 19 comments indicate meaningful Windows remote-control impact.

7. **[#57827](https://github.com/openai/codex/issues/57827) — Explicit prohibitions repeatedly violated; request for enforceable file restrictions**  
   A safety and trust concern: users report Codex modifying files outside authorized scope, acting without approval, and deleting files despite explicit instructions. The request is for enforceable sandbox/file-write controls rather than prompt-level prohibitions.

8. **[#57372](https://github.com/openai/codex/issues/57372) — Regression in 0.164.0: tool calls fail with `unknown field result_buffered_for_ns`**  
   Upgrading from 0.163.0 to 0.164.0 breaks GPT-6 Astra tool calls during repository exploration. A clear release-to-release regression with a working previous version, making it high priority for CLI users.

9. **[#54955](https://github.com/openai/codex/issues/54955) — Turn-diff capture filled disk with 263.7 GB of `tmp_pack` garbage**  
   A severe disk-usage bug on macOS Desktop, compounded by an unavailable disable control and config changes not applying live. This is an operational risk for long-running local sessions.

10. **[#58077](https://github.com/openai/codex/issues/58077) — macOS Send button intermittently disabled at startup**  
    A fresh issue showing Chat and Work intermittently unable to send, with account lookup failures and executor 401 expiry. Web and iPad reportedly work, pointing to a desktop-specific auth/session problem.

## 4. Key PR Progress

1. **[#58083](https://github.com/openai/codex/pull/58083) — Use a dedicated binary for Bazel schema generation**  
   Moves schema export off the application build critical path by building the exporter with `--cfg=test`, avoiding unnecessary test compilation.

2. **[#58075](https://github.com/openai/codex/pull/58075) — Use serving assessments for adaptive Guardian review**  
   Matches final serving assessments to responses and output items, then uses them instead of asynchronous scoring for adaptive review. Relevant to approval latency and policy enforcement.

3. **[#58045](https://github.com/openai/codex/pull/58045) — Managed command checkpoints requiring fresh human approval**  
   Adds `decision = "prompt-user"` support in managed `requirements.toml` prefix rules, allowing policies to require explicit human confirmation independent of automatic review or saved approvals.

4. **[#58040](https://github.com/openai/codex/pull/58040) — Preserve native tool-call evidence through Guardian review**  
   Keeps original function-call metadata available even after the call leaves live conversation history, improving review reliability for encrypted multi-agent messages.

5. **[#58034](https://github.com/openai/codex/pull/58034) — Add an option to disable successful SSE delta logs**  
   Introduces `otel.log_sse_deltas` (default enabled). Setting it to `false` suppresses successful `.delta` event logs while preserving errors, completions, and metrics.

6. **[#58028](https://github.com/openai/codex/pull/58028) — Direct Guardian review of recorded exec calls**  
   Adds `CodexThread::guardian_review` to assess an injected `exec_command` against recorded history without executing it, reusing command preparation and approval policy.

7. **[#57978](https://github.com/openai/codex/pull/57978) — Per-chat leases for shared MCP connections**  
   Negotiates a `codex/connection-lease` extension for pooled stdio MCP connections and attaches host-owned lease IDs to tool calls and resource reads.

8. **[#57970](https://github.com/openai/codex/pull/57970) — Experimental UI tree renderer for the status line**  
   Adds a disabled-by-default `tui_extensions_statusline` feature supporting text, HTTP(S) links, rows, spacers, dividers, and gaps through a shared renderer.

9. **[#57870](https://github.com/openai/codex/pull/57870) — Server-side output truncation for Code Mode sessions**  
   Makes Code Mode honor each session's `output_truncation` setting instead of always using client-side output handling, enabling server truncation and observation budgets.

10. **[#57866](https://github.com/openai/codex/pull/57866) — Retry transient environment startup failures on the next turn**  
    Allows commands to recover after an executor becomes available, rather than persisting a startup failure across turns when the environment and workspace are unchanged.

## 5. Hot Discussions

### Ideas
- **[#30332](https://github.com/openai/codex/discussions/30332) — Background updates for Codex CLI without blocking startup**  
  Requests a Claude Code-style update flow: check and install in the background while keeping startup non-blocking.
- **[#57967](https://github.com/openai/codex/discussions/57967) — 1–2 free Codex usage resets per month for ChatGPT Plus**  
  Proposes making occasional promotional resets a predictable monthly benefit, reflecting ongoing usage-limit sensitivity.

### Q&A
- **[#8582](https://github.com/openai/codex/discussions/8582) — Search/find in active chat history**  
  Users cannot search within the Codex chat; `Ctrl-F` always targets the editor. A recurring discoverability and navigation gap.
- **[#5210](https://github.com/openai/codex/discussions/5210) — Clear the text input area**  
  Asks for a keyboard shortcut to clear large accidental prompt pastes, a small but persistent TUI ergonomics request.
- **[#57771](https://github.com/openai/codex/discussions/57771) — Documentation request: producer/build mapping and persisted execution event format**  
  Requests documentation for execution-record fields such as `custom_tool_call.input` and `item`, aimed at teams using Codex records as evidence sources.

### Show and tell
- **[#57746](https://github.com/openai/codex/discussions/57746) — Zozo: inspectable issue-to-PR example and review checklist**  
  Shares an issue-to-PR workflow coordinating coding runtimes including Codex, with public issue, diff, and CI logs.
- **[#57591](https://github.com/openai/codex/discussions/57591) — toli: let Codex and Claude Code collaborate across machines**  
  An Apache-2.0 tool for direct communication between existing coding agents, including across different developers' machines.
- **[#56429](https://github.com/openai/codex/discussions/56429) — sports-video-coach: Codex reviews sports video and writes a coaching page**  
  Demonstrates a non-coding use case producing timestamped clips, drills, and targets from practice footage.
- **[#58031](https://github.com/openai/codex/discussions/58031) — Using Codex with current Alibaba Cloud documentation and official citations**  
  Highlights the need to verify cloud-specific facts such as endpoints, regions, and model support before using generated configuration.
- **[#57797](https://github.com/openai/codex/discussions/57797) — Keep Codex CLI running 24/7 on a cloud box**  
  A Bash installer using tmux, auto-restart, and phone alerts for remote Codex sessions primarily accessed from mobile.

## 6. Feature Request Trends

- **Conversation editing and navigation:** Message editing with undo ([#11086](https://github.com/openai/codex/issues/11086)), undo/redo in the TUI ([#2379](https://github.com/openai/codex/issues/2379)), chat-history search ([#8582](https://github.com/openai/codex/discussions/8582)), and clearing the prompt ([#5210](https://github.com/openai/codex/discussions/5210)).
- **Predictable usage and updates:** Monthly usage resets for Plus subscribers ([#57967](https://github.com/openai/codex/discussions/57967)) and non-blocking background CLI updates ([#30332](https://github.com/openai/codex/discussions/30332)).
- **Enforceable safety controls:** Users want file-write/delete restrictions that cannot be overridden by model behavior, plus stronger approval checkpoints ([#57827](https://github.com/openai/codex/issues/57827)).
- **Better context and media handling:** Repeated requests center on bounding retained image payloads, reliable compaction, and avoiding reconnect/request-size failures in long sessions.
- **Cross-agent interoperability:** Community projects increasingly focus on connecting Codex with other coding agents and external documentation sources.

## 7. Developer Pain Points

- **Image-heavy sessions destabilize long-running work.** Multiple issues describe retained images causing repeated auto-compaction, WebSocket reconnects, 35–46 MiB requests, HTTP 400s, and multi-gigabyte upload amplification ([#33493](https://github.com/openai/codex/issues/33493), [#55730](https://github.com/openai/codex/issues/55730), [#54501](https://github.com/openai/codex/issues/54501), [#55076](https://github.com/openai/codex/issues/55076)).
- **Windows Desktop remains a regression hotspot.** Organization settings, side chat shortcuts, networking, session visibility, and chat-ID handling all have recent reports ([#50704](https://github.com/openai/codex/issues/50704), [#56620](https://github.com/openai/codex/issues/56620), [#57988](https://github.com/openai/codex/issues/57988), [#56194](https://github.com/openai/codex/issues/56194)).
- **Authentication and session failures block work entirely.** Phone verification failures and intermittent disabled Send buttons prevent users from starting or continuing sessions ([#25828](https://github.com/openai/codex/issues/25828), [#58077](https://github.com/openai/codex/issues/58077)).
- **Local resource usage can become hazardous.** A 263.7 GB `tmp_pack` accumulation and unavailable disable controls show that caching and diff capture need stronger cleanup guarantees ([#54955](https://github.com/openai/codex/issues/54955)).
- **Release regressions erode upgrade confidence.** A concrete 0.164.0 tool-call breakage with 0.163.0 working encourages users to pin versions ([#57372](https://github.com/openai/codex/issues/57372)).
- **Prompt-level constraints are not trusted as guardrails.** Users report repeated unauthorized file operations despite explicit prohibitions, increasing demand for enforceable policy and approval mechanisms ([#57827](https://github.com/openai/codex/issues/57827)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-29

## 1. Today's Highlights

No releases shipped in the last 24 hours, but activity remained concentrated in the `area/agent` workstream, where 30 of the top issues by comment count were updated. The dominant themes are **subagent reliability and observability** — false success reporting, indefinite hangs, and configuration overrides being ignored — alongside a notable wave of PRs hardening session-history integrity, cancellation semantics, and shell-security checks.

---

## 2. Releases

No new releases in the last 24 hours.

---

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS is reported as GOAL success** (P1, 13 comments, 👍2)
   The `codebase_investigator` subagent returns `status: "success"` / `Termination Reason: "GOAL"` even though its own payload says it hit the turn cap before doing any analysis. This is the most-discussed issue of the day because it silently corrupts agent orchestration decisions — parent agents treat a truncated run as a clean completion.

2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Leverage model's bash affinity via zero-dependency OS sandboxing** (P2, 9 comments, 👍1)
   Proposes letting Gemini 3 chain native POSIX tools (`grep`, `sed`, `awk`) inside a sandbox rather than routing everything through bespoke tools. A design-level discussion about matching the model's training distribution without sacrificing user security.

3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs** (P1, 8 comments, 👍8)
   The highest-reacted issue in the set: any deferral to the generalist agent can hang indefinitely, even for trivial work like folder creation. Users report waiting up to an hour; disabling subagent delegation is the current workaround.

4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — Assess impact of AST-aware file reads, search, and mapping** (P2, 7 comments, 👍1)
   An EPIC tracking whether AST-aware reads can tighten method bounds per tool call, cut wasted turns, and reduce token noise. Paired with [#22746](https://github.com/google-gemini/gemini-cli/issues/22746) (tilth/glyph) and [#22747](https://github.com/google-gemini/gemini-cli/issues/22747) (ast-grep) — clearly a coordinated investigation.

5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough** (P2, 7 comments)
   Anecdotal but widely echoed: custom skills (e.g. `gradle`, `git`) are only invoked when explicitly instructed, not when contextually relevant. Points at skill-discovery/steering quality rather than capability.

6. **[#29325](https://github.com/google-gemini/gemini-cli/issues/29325) — Print mode (`-p`) hangs indefinitely on all inference** (P1, 5 comments)
   Reported on 1.2.2 and independently confirmed: the CLI accepts the prompt, then never returns — no error, no timeout. High severity for anyone scripting Gemini CLI in CI.

7. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — Browser Agent ignores `settings.json` overrides (e.g. `maxTurns`)** (P2, 4 comments)
   `AgentRegistry` merges settings correctly at init, but the Browser Agent bypasses them at runtime. A concrete configuration-integrity bug with an obvious root cause.

8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails on Wayland** (P1, 4 comments, 👍1)
   Reports `Termination Reason: GOAL` alongside a failure — the same success/failure mislabeling pattern as #22323, this time on Linux/Wayland.

9. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 400 error with >128 tools** (P2, 3 comments)
   Exceeding the tool budget produces a hard 400. Expectation is that the agent scopes enabled tools more intelligently rather than failing the whole turn.

10. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — Agent should stop/discourage destructive behavior** (P2, 3 comments, 👍1)
    The model occasionally reaches for `git reset` or `--force` where a safer path exists. Maintainer-authored, which signals this is being treated as a guardrail requirement rather than a user error.

---

## 4. Key PR Progress

1. **[#30019](https://github.com/google-gemini/gemini-cli/pull/30019) — fix(core): preserve session history during tool output masking**
   Prevents the first user prompt from being lost when resuming sessions whose tool outputs were masked, avoiding turn coalescing side effects in `getHistory()`.

2. **[#30020](https://github.com/google-gemini/gemini-cli/pull/30020) — fix(core): drop tool calls streamed by failed attempts on retry**
   On mid-stream retries, both the failed and successful attempt's `ToolCallRequest` events were being executed — meaning side-effecting commands could run twice. A significant correctness fix.

3. **[#30022](https://github.com/google-gemini/gemini-cli/pull/30022) — fix(cli): run ACP tool calls through hooks**
   Routes editor-driven (ACP) sessions through `BeforeTool`/`AfterTool`, closing a policy gap where hook rules only applied to the interactive CLI.

4. **[#30029](https://github.com/google-gemini/gemini-cli/pull/30029) — feat(core,cli): make command substitution block user-configurable**
   Adds `security.allowCommandSubstitution` (default `false`) to replace the hardcoded block on `$()`, backticks, and process substitution. Fixes #27393; marked `help wanted`.

5. **[#30026](https://github.com/google-gemini/gemini-cli/pull/30026) — fix(core): treat project temp paths as local in untrusted flag checks**
   Stops Gemini's own helper scripts under `~/.gemini/tmp/<project>/...` from triggering repeated confirmation prompts after their paths appear in shell output.

6. **[#30027](https://github.com/google-gemini/gemini-cli/pull/30027) — feat(cli): support separate custom theme configuration** (P1)
   Lets users define custom themes independently of the built-in set — a long-standing customization request landing as a large PR.

7. **[#30023](https://github.com/google-gemini/gemini-cli/pull/30023) — fix(ci,sdk): run SDK tests in PR CI and fix AgentLoopContext mock**
   Re-enables 6 previously skipped `GeminiCliSession.sendStream()` unit tests and adds the SDK package to the CI test shard.

8. **[#30017](https://github.com/google-gemini/gemini-cli/pull/30017) — fix(core): keep committed turns when cancelling a tool-result continuation**
   Fixes history rollback that erased the user prompt *and* already-executed tool turns (which may have modified files) when cancelling mid-request.

9. **[#29977](https://github.com/google-gemini/gemini-cli/pull/29977) — fix(core): honor cancellation in `read_background_output`**
   The tool ignored its abort signal, sleeping the full `delay_ms` and reading the log anyway. Paired with [#29978](https://github.com/google-gemini/gemini-cli/pull/29978), which fixes dropped boundary lines in the same 64KB tail-read window.

10. **[#30006](https://github.com/google-gemini/gemini-cli/pull/30006) — fix(core): use PowerShell CIM fallback for Windows IDE process lookup**
    Replaces a Unix-only `ps` fallback that silently returned an empty command string when the PowerShell snapshot failed.

---

## 5. Hot Discussions

Omitted — no discussion data was provided.

---

## 6. Feature Request Trends

- **AST-aware codebase tooling** — The strongest coordinated direction this cycle: [#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#22747](https://github.com/google-gemini/gemini-cli/issues/22747). Motivation is both precision (correct method bounds in one call) and efficiency (fewer turns, less token noise).
- **Token-frugal context management** — [#19561](https://github.com/google-gemini/gemini-cli/issues/19561) proposes a "Tactful Extraction" hierarchy (grep first, surgical reads later) against a ~36.6k token/turn baseline. Related: replacing in-context todo lists with persistent file-backed tracking ([#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000)).
- **Subagent observability & governance** — Shareable subagent trajectories via `/chat share` ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)), subagent context in `/bug` reports ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)), discovery via `settings.json` ([#18285](https://github.com/google-gemini/gemini-cli/issues/18285)), and parallel subagent shared memory ([#18287](https://github.com/google-gemini/gemini-cli/issues/18287)).
- **Sandboxing & scoped policy** — Bash-native sandboxing ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)) and per-workspace policy instead of global policy ([#18397](https://github.com/google-gemini/gemini-cli/issues/18397)).
- **Self-awareness of CLI mechanics** — [#21432](https://github.com/google-gemini/gemini-cli/issues/21432) asks the agent to know its own flags, hotkeys, and invocation patterns accurately.

---

## 7. Developer Pain Points

- **Terminal-state bugs in subagents are the top frustration.** Indefinite hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), false `GOAL`/`success` reporting on turn-limit exhaustion ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983)), and repeated `status/need-retesting` tags suggest fixes land but aren't staying fixed.
- **Non-interactive mode is unreliable.** `-p` hanging on *all* inference with no timeout ([#29325](https://github.com/google-gemini/gemini-cli/issues/29325) is the clearest blocker for CI/scripted use.
- **Config doesn't always win.** Settings merged at init but ignored at runtime by the Browser Agent ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)); symlinked agent files aren't discovered at all ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)).
- **Workspace hygiene.** The model litters temp edit scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)) and can reach for destructive git operations ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)) — both increase friction at commit time.
- **Stuck interactive prompts.** Scaffolding flows like `create vite` block on TUI prompts instead of proceeding ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)).
- **Platform-specific breakage** — Wayland browser support and Windows/PowerShell quoting and process detection continue to generate a steady tail of bugs.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — Community Digest
**Date:** 2026-10-29 · **Repo:** [github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. Today's Highlights

Activity was dominated by **MCP authentication and enterprise-proxy failures** (Entra token caching, OAuth dynamic client registration, `*.ghe.com` proxying) alongside a fresh batch of resource-exhaustion reports — including a 37 GiB memory blow-up in a long-running parallel-agent session. Three long-standing high-impact issues were **closed** today (#4851 Azure MCP BrokenPipe, #4613 eager MCP schema injection, #4710 runaway file-search thread), suggesting sustained triage throughput. Meanwhile, the community's highest-reaction items remain **Linux keychain** (#2165, 22 👍) and the long-running **403 model-listing** outage (#552, 31 comments).

---

## 2. Releases

No new releases published in the last 24 hours. Field reports reference versions **1.0.103** and **1.0.105** as current.

---

## 3. Hot Issues (10)

### 1. [#552](https://github.com/github/copilot-cli/issues/552) — `Failed to list models: 403 unauthorized` (OPEN, 31 comments, 4 👍)
The single most-commented issue in the repo. Organizations that have enabled Copilot CLI still see an empty model list because the CLI receives a 403 on the models endpoint. Affects v0.0.355 and evidently still unresolved nearly a year later — a persistent entitlement/permission-mapping blocker for enterprise onboarding.

### 2. [#4802](https://github.com/github/copilot-cli/issues/4802) — PRU quota wiped out, likely tied to Assisted Permissions (OPEN, 8 comments)
A Copilot Pro (legacy request-based PRU) user reports that enabling **Assisted Permissions** correlated with burning through the remaining 234 PRUs in a single incident. High business impact: a permissions feature appears to multiply premium-request consumption. No reactions yet, but it's the top non-auth operational complaint.

### 3. [#4851](https://github.com/github/copilot-cli/issues/4851) — Azure MCP server fails sending HTTP request *(CLOSED)* (6 comments, 10 👍)
Rust runtime throws `BrokenPipe` when validating an **Azure API Center MCP registry** — a regression that broke a previously working setup overnight. Closed with strong community approval (10 👍); a good signal that MCP transport robustness is being actively fixed.

### 4. [#2165](https://github.com/github/copilot-cli/issues/2165) — Ubuntu keychain support is broken (OPEN, 5 comments, **22 👍**)
Highest-reaction issue in the digest. Two bugs: incorrect documentation around `secret-tool`, and broken keychain access on Ubuntu. Linux users are effectively locked out of smooth auth flows — the reaction count reflects a broad, underserved platform segment.

### 5. [#4017](https://github.com/github/copilot-cli/issues/4017) — MCP OAuth: non-first-party HTTP servers never launch the deferred runtime browser flow (OPEN, 5 comments, 3 👍)
In the Desktop app, remote MCP servers (Atlassian, incident.io) cancel the host token and then silently fail to start the browser flow. Result: **no auth, no error, no popup** — the worst possible failure mode. Toggling the server has no effect. Nicely diagnosed root cause.

### 6. [#2109](https://github.com/github/copilot-cli/issues/2109) — ACP: add `ask_user` / `ask_question` extension method (OPEN, 4 comments, 8 👍)
Today's top **feature** request by reaction. ACP support currently exposes only `session/request_permission`; third-party ACP clients cannot receive clarifying questions or return structured answers. Clearing this gap would substantially improve custom-client parity with the TUI.

### 7. [#4555](https://github.com/github/copilot-cli/issues/4555) — ACP: `session/prompt` unconditionally aborts the session, killing background sub-agents (OPEN, 3 comments)
`copilot --acp --stdio` calls `session.abort()` as the *first* action of the prompt handler, cancelling all background sub-agent tasks — behavior the interactive TUI does not exhibit. A correctness/parity bug that makes ACP mode unusable for multi-agent orchestration.

### 8. [#4710](https://github.com/github/copilot-cli/issues/4710) — Runaway `copilot-file-search` thread burns CPU and unbounded disk while idle *(CLOSED)* (3 comments)
With `--yolo`, an idle session pinned a CPU core and wrote unbounded diagnostic logs under `~/.copilot/log`. Reported on 1.0.83-3 / Ubuntu 24.04. Closure is welcome news for anyone running long-lived sessions on laptops or CI runners.

### 9. [#4613](https://github.com/github/copilot-cli/issues/4613) — 1.0.80+ regression: MCP schemas eagerly injected, ~354K startup tokens *(CLOSED)* (3 comments)
Reported as high severity: the runtime stopped deferring MCP tool schemas, injecting the full ambient catalog into the first model request even for trivial prompts. Directly translates into latency and premium-request cost. Closing this removes a major performance regression in the 1.0.80+ line.

### 10. [#5254](https://github.com/github/copilot-cli/issues/5254) — 1.0.103 main process hits 37.3 GiB, 133M mallocs, 10.7 GiB fragmentation (OPEN, 2 comments)
A resumed, long-running parallel-agent session on a 32 GiB Apple Silicon Mac exceeded physical RAM after ~6h19m, driving heavy swap and intermittency. Combined with #4710 and #5205, this points to a systemic **long-session resource management** gap.

**Also notable (just filed today):** [#5289](https://github.com/github/copilot-cli/issues/5289) `copilot --cloud` fails at provisioning with internal server error; [#5288](https://github.com/github/copilot-cli/issues/5288) `/copy` now emits HTML instead of Markdown; [#5284](https://github.com/github/copilot-cli/issues/5284) expired MCP OAuth DCR registrations are reused forever (`invalid_client`); [#5286](https://github.com/github/copilot-cli/issues/5286) sandbox proxy won't forward tokens to `*.ghe.com`.

---

## 4. Key PR Progress

No pull requests were updated in the last 24 hours (0 items). The three closures above (#4851, #4613, #4710) indicate fixes landed through other channels.

---

## 5. Hot Discussions

No discussion data was provided for this window.

---

## 6. Feature Request Trends

Distilled from today's issues:

- **ACP protocol completeness** — #2109 `ask_user`/`ask_question`, #4555 background sub-agent preservation. Custom ACP clients are the fastest-growing integration surface and currently lag the TUI in capability.
- **Deterministic permission control** — #5275 requests default-deny with an explicit allowlist in non-interactive mode, since `--deny-tool` always overrides `--allow-tool`. Pairs with #4802's concern that *Assisted Permissions* is opaque in its cost/behavior.
- **BYOK / provider configurability** — #5287 asks for a published JSON Schema for `providers.json`; #5290 reports the documented `--fusion-tier` argument is missing. Users want introspection into undocumented config surfaces.
- **Enterprise & data-residency correctness** — #5280 (`/login` mislabels `*.ghe.com`), #5286 (sandbox proxy + `gh` for GHE), #552 (org entitlement → 403). Enterprise labeling and proxy behavior need first-class treatment.
- **Output format control** — #5288 requests `/copy` return Markdown again rather than HTML.

---

## 7. Developer Pain Points

1. **Authentication is the dominant failure domain.** Entra token caching is broken on Windows (#5236: cache file stays 0 bytes → re-prompt every process), macOS keychain XARA partition mismatches cause per-launch prompts (#4273), Ubuntu keychain is broken (#2165), Entra `api://` scopes were rejected (#5061, closed), and MCP OAuth DCR registrations never expire (#5284). Cross-platform credential persistence is the single largest source of friction.
2. **MCP silently fails.** #4017 produces no auth, no error, no popup; #4916 returns base64 `text/plain` resource blobs to the model as empty output; #4851 broke overnight. Failure modes are largely invisible, making self-diagnosis hard.
3. **Long sessions leak resources.** 37 GiB RSS growth (#5254), a runaway idle search thread (#4710), and context rollover crashing on a missing `checkpoint.md` (#5205) all hit users running multi-hour or parallel-agent workloads.
4. **Windows lags other platforms.** Clipboard copy breaks while the CLI runs (#3981), PowerShell environment variables are not inherited by the shell runner (#5281), and sandbox preflight fails with `CreateProcessW` error 2 under enterprise policy (#5285).
5. **Configuration semantics surprise users.** `includeCoAuthoredBy: false` is not inherited by `task` subagents (#4576, 5 👍), compaction fires at 50% with GPT-5.6 Luna Max (#4571, closed), and startup token cost ballooned from eager MCP schema injection (#4613, closed). Config that is silently not honored erodes trust faster than missing features.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-10-29

## Today's Highlights

OpenCode shipped **v2.2.1**, headlined by the new `/goal` primitive that turns a session into a persistent, reviewer-verified objective loop in both the TUI and Desktop. The release landed alongside a `process-daemon`/`@opencode/pty` bump to protocol 7. Meanwhile, triage volume remains dominated by Zen free-tier quota complaints (issue #59387 has ballooned to 98 comments) and a wave of `pending close` cleanup across older bug reports.

---

## Releases

### v2.2.1
- **Keep Working Toward a Goal**: `/goal <objective>` in the TUI or Desktop keeps the session running until a reviewer confirms the objective is met. Control surface includes `/goal pause`, `/goal resume`, and `/goal clear`.
- PR: [anomalyco/opencode#59437](https://github.com/anomalyco/opencode/pull/59437)

### process-daemon v0.3.2
- `opencode-pty` 0.3.2 (protocol 7) built from `packages/process-daemon`, published to npm as `@opencode/pty@0.3.2`. Affects terminal/PTY consumers; a protocol bump is worth pinning deliberately.

---

## Hot Issues

1. **[#59387](https://github.com/anomalyco/opencode/issues/59387) — Zen free tier master thread (98 comments, 👍9)** — The canonical funnel for free-tier failures: "Free usage exceeded," reset timers that never fire, and Desktop versions rejecting subscriptions. The high comment count reflects unresolved per-user quota state rather than a single reproducible bug.

2. **[#18654](https://github.com/anomalyco/opencode/issues/18654) — Cannot change or remove email in OpenCode Zen (14 comments, 👍20)** — Highest-reaction item in the set. GitHub email changes create duplicate Zen identities with no user-facing remediation. Marked `pending close`, but the 👍 count signals real account-lifecycle friction.

3. **[#48743](https://github.com/anomalyco/opencode/issues/48743) — MCP pre-spawn/warm-up + reconnect path (10 comments)** — On Windows Desktop with 14+ local stdio MCP servers, *all* of them get marked `failed` at session start from concurrent cold starts. The servers are healthy; the supervisor isn't. A warm-up/reconnect API would fix a whole class of setups.

4. **[#48657](https://github.com/anomalyco/opencode/issues/48657) — Session never drains when `Snapshot.capture()` stalls (8 comments)** — `git add --all --pathspec-from-file=-` hangs on a large untracked, non-gitignored tree, blocking `llm.stream()` and leaving the execution claim held forever. A hard hang with no timeout or user-visible diagnostic.

5. **[#48497](https://github.com/anomalyco/opencode/issues/48497) — Persistent memory system (`/teach`, `/recall`, `/learn`, `/memory`) (7 comments)** — Proposes first-class long-term memory rather than re-deriving context each session. Closed as triage, but the request direction recurs repeatedly.

6. **[#49042](https://github.com/anomalyco/opencode/issues/49042) — Agent loop runs 500+ steps with zero user input (6 comments)** — Runaway auto-continue after the model emits final text. No step ceiling, no guard. Pairs directly with today's `/goal` release: persistent loops need persistent safety rails.

7. **[#24981](https://github.com/anomalyco/opencode/issues/24981) — Explicit proxy configuration in opencode config (6 comments, 👍8)** — Still open since April. Env-var-only proxying (`HTTPS_PROXY`, etc.) doesn't cover per-provider routing or GUI-launched Desktop sessions.

8. **[#59817](https://github.com/anomalyco/opencode/issues/59817) — `mkdir EEXIST` on `shell/<projectID>` silently falls back to home dir (3 comments, `reproduced`)** — A file occupying the project-ID path causes 500s on every request, and the TUI quietly starts sessions in `$HOME`. Silent fallback is the dangerous part — it can run tools in the wrong directory.

9. **[#48542](https://github.com/anomalyco/opencode/issues/48542) — Compaction writes Anthropic prompt cache that is never read (3 comments)** — 70 compactions over 7 days wrote 1,278,055 cache tokens with 0 reads. Cache writes bill at a premium, so this is direct, quantifiable spend waste.

10. **[#59916](https://github.com/anomalyco/opencode/issues/59916) — `opencode run` Ctrl-C leaves the bash tool command running (3 comments)** — On SIGINT/SIGTERM during a bash tool call, OpenCode exits but the child keeps executing; on `--continue` the model is told it was "interrupted," desynchronizing state from reality. Especially bad under CI `timeout` and editor integrations.

*Also worth watching:* [#59909](https://github.com/anomalyco/opencode/issues/59909) (paste freezes on Termux, scales with paste size), [#48740](https://github.com/anomalyco/opencode/issues/48740) (Zen `/v1/responses` 422 when a tool output contains an image — bricks screenshot sessions), [#49034](https://github.com/anomalyco/opencode/issues/49034) (v2 on Windows times out waiting for the background service).

---

## Key PR Progress

1. **[#60028](https://github.com/anomalyco/opencode/pull/60028) — `feat(core)`: Code Mode predicate gates schedule firing** — Schedules can take `if: { type: "code", code, allow }` evaluated before each run with **no model call**. The program sees only allowlisted tools plus `state`, `now`, and `runs`, and returns `{ wake, state, message? }`. Builds on the scheduling PR #59401 — this is the "cheap gate before expensive agent" pattern.

2. **[#60219](https://github.com/anomalyco/opencode/pull/60219) — `fix(core)`: select tools automatically during session steps** — Makes OpenAI-compatible requests send `tool_choice: "auto"` on normal steps, fixing vLLM gateways that reject requests omitting the field. Closes #60202.

3. **[#60228](https://github.com/anomalyco/opencode/pull/60228) — `chore(protocol)`: regenerate OpenAPI document** — The Effect 4.0.6 upgrade (#59630) changed the generated schema, so `check:generated` was failing on `dev` and blocking every PR's required `unit (linux)` check. Small diff (`maximum: 9007199254740991` on three integer fields), high blast radius — merges unblock CI for everyone.

4. **[#60208](https://github.com/anomalyco/opencode/pull/60208) — `feat(session-ui)`: user message actions into a menu** — Collapses per-message footers to **Rewind to here** + a **More actions** (`…`) menu with timestamp, **Fork from here**, and **Copy message**. Reduces visual noise in long timelines.

5. **[#60209](https://github.com/anomalyco/opencode/pull/60209) — `feat(merman)`: render Mermaid class diagrams** — `classDiagram` fences previously rendered as raw code in the TUI. Adds a class-diagram renderer to merman (refs #40508).

6. **[#60222](https://github.com/anomalyco/opencode/pull/60222) — `fix(tui)`: preserve captured attachments and source paths** — Keeps #59833's guarantee that agents receive the original path for pasted images and PDFs, while still sending captured content. Closes #60210.

7. **[#60216](https://github.com/anomalyco/opencode/pull/60216) — `fix(app)`: stop dropping frames during queued-prompt grow animation** — Removes the per-frame timeline restyle and forced layouts in the submit key handler, which dropped frames on an M5 Pro. Representative of a broader UI-perf cleanup thread.

8. **[#60125](https://github.com/anomalyco/opencode/pull/60125) — `fix(core)`: preserve pathless MCP OAuth resources** — Custom provider validation was dropping a pathless resource such as `https://example.com`. Closes #58956 and unblocks OAuth-protected MCP servers behind root-domain issuers.

9. **[#60001](https://github.com/anomalyco/opencode/pull/60001) — `feat(app)`: redesign the goal pill** — Redesigns the goal plugin's composer pill and popover (stacked on #60000, which adds `ComposerPill.icon`). This is the UI half of the v2.2.1 `/goal` launch.

10. **[#55765](https://github.com/anomalyco/opencode/pull/55765) — `fix(app)`: match shortcuts on non-Latin keyboard layouts** — Ctrl/Meta shortcuts fall back to a key's physical position when the literal binding misses; exact custom bindings still win. Closes #55676 — a long-standing accessibility gap for Cyrillic/Arabic/CJK layouts.

*Also merged/moving:* [#56010](https://github.com/anomalyco/opencode/pull/56010) (interactive `plugin add`/`remove` CLI), [#57090](https://github.com/anomalyco/opencode/pull/57090) (proportionate partial Code Mode catalog updates), [#60212](https://github.com/anomalyco/opencode/pull/60212) (syntax-highlighted shell commands in the TUI), [#60223](https://github.com/anomalyco/opencode/pull/60223) (live `/status` link in site footers).

---

## Feature Request Trends

- **Persistent memory and reusable context** — `/teach`, `/recall`, `/learn`, `/memory` ([#48497](https://github.com/anomalyco/opencode/issues/48497)) is the clearest articulation, but the same need surfaces as `AGENTS.md` workarounds and repeated re-priming in long-lived sessions. Expect this to converge with the `/goal` session-lifetime work.
- **Network/proxy configurability** — [#24981](https://github.com/anomalyco/opencode/issues/24981) asks for proxy settings in `opencode config` rather than env vars only. Corporate and GUI-launched environments are the drivers.
- **Account lifecycle for Zen** — Email change/removal ([#18654](https://github.com/anomalyco/opencode/issues/18654)) plus duplicate-identity cleanup. Billing-adjacent and repeatedly upvoted.
- **MCP lifecycle management** — Warm-up/pre-spawn and reconnect ([#48743](https://github.com/anomalyco/opencode/issues/48743)), plus pathless OAuth resource preservation ([#60125](https://github.com/anomalyco/opencode/pull/60125)). The ask is shifting from "support MCP" to "supervise MCP."
- **Desktop/TUI UI scaling and density** — Model selector capped at ~284px on large monitors ([#48789](https://github.com/anomalyco/opencode/issues/48789)); quick toggles for reasoning summaries and expanded shell/edit parts in the title bar ([#48671](https://github.com/anomalyco/opencode/issues/48671)).
- **Session-scoped model/agent selection for attached TUI/ACP** — [#48759](https://github.com/anomalyco/opencode/issues/48759), from embedders driving the same runtime via ACP. A power-user/embedding signal rather than a mass request.

---

## Developer Pain Points

- **Free-tier quota state is the single loudest complaint.** [#59387](https://github.com/anomalyco/opencode/issues/59387) (98 comments) plus [#59911](https://github.com/anomalyco/opencode/issues/59911) and [#60224](https://github.com/anomalyco/opencode/issues/60224) all describe the same failure shape: a limit message with a reset timestamp that never arrives. Users can't self-diagnose, so every instance becomes a support thread.
- **Hangs without timeouts.** Both [#48657](https://github.com/anomalyco/opencode/issues/48657) (`git add` on huge untracked trees) and [#59909](https://github.com/anomalyco/opencode/issues/59909) (Termux paste, cost scaling with input size) are O(n)-or-worse stalls with no progress signal or cancellation. The TUI has no "this is stuck" affordance.
- **Runaway agent loops.** [#49042](https://github.com/anomalyco/opencode/issues/49042) reports 500+ auto-continued steps with no user input and no guard. With `/goal` now shipping, an explicit step/budget ceiling is an obvious follow-up.
- **Cache economics are invisible and wrong.** [#48542](https://github.com/anomalyco/opencode/issues/48542) (1.28M cache-write tokens, 0 reads) and [#48624](https://github.com/anomalyco/opencode/issues/48624) (`glm-5.3-flash` showing zero cache reads while `deepseek-v4.1-flash` caches normally) both amount to users paying for caching they never benefit from — with no surfaced metrics to notice it.
- **Signal handling and orphaned child processes.** [#59916](https://github.com/anomalyco/opencode/issues/59916) leaves bash commands running after exit *and* records a false "interrupted" state, so resume diverges from reality. This pattern is hostile to CI and editor integrations.
- **Silent fallbacks that change execution context.** [#59817](https://github.com/anomalyco/opencode/issues/59817) quietly relocates a session to `$HOME` when `mkdir` returns `EEXIST` — errors that alter *where tools run* should fail loudly.
- **Config strictness asymmetry.** [#49027](https://github.com/anomalyco/opencode/issues/49027) forwards unknown agent config fields verbatim into upstream provider requests, producing `invalid_request_error`; [#48555](https://github.com/anomalyco/opencode/issues/48555) rejects a subagent for mixed-case model names *after* suggesting the correct name. Config validation is inconsistent in both directions — too permissive outward, too brittle inward.
- **Windows Desktop background service reliability.** [#49034](https://github.com/anomalyco/opencode/issues/49034) (startup timeout for the managed service), [#48715](https://github.com/anomalyco/opencode/issues/48715) (sidecar `0xC0000409` crashes under memory pressure), and [#48743](https://github.com/anomalyco/opencode/issues/48743) (MCP cold-start failures) form a consistent Windows stability cluster.

---

*No Discussion data was provided for this period, so that section is omitted.*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-29

## Today's Highlights
No new releases landed in the last 24 hours; activity was dominated by the long-running Windows usability thread (#7547, now 91 comments) and a cluster of fullscreen/TUI rendering performance reports. On the fix side, a wave of closed PRs addressed session-storage semantics, terminal control-sequence handling, `pi.exec` buffering, and environment-daemon teardown, while Bedrock Mantle routing and Anthropic server-side message threads remain in review.

## Releases
None in the last 24 hours.

## Hot Issues

1. **[#7547](https://github.com/earendil-works/pi/issues/7547) — [Windows] How do you use Pi on Windows? What issues are you seeing?** (91 comments, 👍2)  
   The central sink thread for Windows support. The motivation is explicitly strategic: too many supported run paths make it unclear whether to fix bugs, improve docs, or delegate to extensions. Highest-engagement issue in the dataset and the best signal for where Windows investment should go.

2. **[#9549](https://github.com/earendil-works/pi/issues/9549) — Large transcripts re-render every frame; resize re-emits the whole transcript** (17 comments, 👍1)  
   On Windows Terminal with 2 logical cores, a large transcript saturates a core. Reported with agent-assisted measurements and verified with `pi -ne`, making it unusually actionable. Points at fullscreen render-loop architecture rather than a one-off bug.

3. **[#7739](https://github.com/earendil-works/pi/issues/7739) — Set a startup-time budget targeting jcode-comparable latency and memory** (12 comments)  
   Proposes a concrete benchmark-backed budget against jcode (10 interactive PTY launches, Linux). Signals community interest in treating startup latency as a tracked, regression-guarded metric rather than an ad hoc concern.

4. **[#9656](https://github.com/earendil-works/pi/issues/9656) — Mouse wheel scrolls prompt history instead of transcript in fullscreen on Windows + Zellij** (11 comments, 👍5)  
   Highest upvote count among open bugs here. Narrow repro (Windows → Alacritty → Zellij → Pi; works without multiplexer, works on Linux+tmux), which strongly suggests multiplexer/terminal capability detection is the culprit.

5. **[#9884](https://github.com/earendil-works/pi/issues/9884) — Configured default model sometimes replaced by a fallback at startup** (7 comments)  
   Reproduced 4/20 startups: `defaultProvider`/`defaultModel` from a runtime-discovered extension catalog is silently swapped for the built-in `deepseek/deepseek-v4-pro`. Nondeterministic startup state is a trust problem for teams pinning models.

6. **[#10954](https://github.com/earendil-works/pi/issues/10954) — OpenCode Go Claude (`anthropic-messages`): 400 on replayed thinking block after tool list change** (6 comments)  
   Same failure class as #10324 but on a different transport, with `block_binding` only sent for `supportsMidConvoEffort`. Important for anyone routing Anthropic-compatible traffic through third-party relays; AI-disclosed and experimentally verified.

7. **[#10386](https://github.com/earendil-works/pi/issues/10386) — Durable: host coding-agent extensions (AgentSession-backed runtime or hook parity)** (6 comments, 👍1)  
   Requests either an `AgentSession`-backed runtime or `ExtensionAPI` hook parity so Durable can run as a first-class extension. Represents the broader ask for extension surfaces that match core capability.

8. **[#10616](https://github.com/earendil-works/pi/issues/10616) — Extension `pi.exec` fails for `.cmd` commands on Windows (npm, npx, pnpm)** (5 comments)  
   `pi.exec("npm", ["--version"])` returns exit 1 with empty stdout, while the bash tool succeeds. A clean, isolated Windows extension-API defect with an obvious impact on the extension ecosystem.

9. **[#10770](https://github.com/earendil-works/pi/issues/10770) — Default permissions of `.pi/agent/sessions` should be `0700`** (5 comments, `inprogress`)  
   Fresh session directories are created group/world-readable. Marked in progress; a straightforward privacy hardening request that matters for shared machines and CI runners.

10. **[#11567](https://github.com/earendil-works/pi/issues/11567) — Typing slows down in long sessions: `getSessionName()` scans every entry on each render** (2 comments, filed today)  
    At ~400k entries, key-to-render p50 is 21 ms vs 1.5 ms empty; profiling attributes 94% of busy CPU to `SessionManager.getSessionName()` called from the footer on every render. A newly filed, well-profiled regression with a clear fix path.

*Also worth watching:* [#11529](https://github.com/earendil-works/pi/issues/11529) (tool ordering from a `Map` busts the prompt cache when MCP servers reconnect) and [#11527](https://github.com/earendil-works/pi/issues/11527) (a new `BedrockRuntimeClient` per request, forcing SSO credential round trips).

## Key PR Progress

1. **[#11565](https://github.com/earendil-works/pi/pull/11565) — fix(tui): stop writing OSC 133 prompt markers in regular mode** (closed)  
   Regular TUI mode emitted OSC 133 `A`/`B`/`C` markers; terminals displaying OSC 7501 status (Rex, iTerm2 3.7.5+) interpreted `133;A` as a new shell prompt and dropped the working indicator. Resolves #11519.

2. **[#11561](https://github.com/earendil-works/pi/pull/11561) — fix(coding-agent): treat sessions in the sessions directory as default storage** (closed)  
   `usesDefaultSessionDir()` returned false whenever a session's cwd differed from its folder name, so `/resume` silently listed only a subset. Fixes a confusing session-discovery bug.

3. **[#11562](https://github.com/earendil-works/pi/pull/11562) — fix(coding-agent): start new sessions in the cwd's default directory** (open)  
   Companion to #11561: `/new`, `/fork`, and `/clone` currently inherit the opened file's folder and treat it as a custom `--session-dir`. Aligns new-session storage with the actual working directory.

4. **[#11556](https://github.com/earendil-works/pi/pull/11556) — fix(coding-agent): let the SDK stream resolve auth for summaries** (closed)  
   Sessions from `createAgentSession` passed a resolved access token as `options.apiKey` for compaction/branch/bug-report summaries, bypassing `modelRuntime.streamSimple`'s auth resolution. Important for SDK embedders using OAuth.

5. **[#11546](https://github.com/earendil-works/pi/pull/11546) — fix(coding-agent): cap `pi.exec` output at 256 MiB** (closed, closes #11360)  
   Adds `ExecOptions.maxBuffer`, defaulting to the smaller of 256 MiB and the runtime string limit; oversized commands are terminated with `truncated: true` and a nonzero exit code. Prevents runaway memory from chatty subprocesses.

6. **[#11545](https://github.com/earendil-works/pi/pull/11545) — Anthropic message threads** (open)  
   Uses Anthropic's server-side message-threads beta to send only new messages when a session id and caching are active, while still building the full request for comparison. Potentially significant token/cost reduction for long sessions.

7. **[#11544](https://github.com/earendil-works/pi/pull/11544) — fix(env): kill the daemon's commands when it is terminated** (closed)  
   `Connection.close()` sent `SIGTERM` before the daemon could run `kill_all`, leaving commands running. Clean teardown fix for the local Unix daemon path.

8. **[#11543](https://github.com/earendil-works/pi/pull/11543) / [#11541](https://github.com/earendil-works/pi/pull/11541) — fix(coding-agent): strip private-parameter CSI sequences in `stripAnsi`** (closed)  
   `stripAnsi` treated `<`, `=`, `>` as CSI final bytes, so mouse reports like `ESC[<35;59;1M` leaked parameters into tool output as literal text. Two implementations merged; low-level but affects transcript cleanliness.

9. **[#11348](https://github.com/earendil-works/pi/pull/11348) — fix(tui,coding-agent): handle selection page keys in all scrolling lists** (closed, fixes #7629)  
   Rebases and narrows #7680: honors `tui.select.pageUp`/`pageDown` across selection components, autocomplete, and agent selectors, with an explicit fullscreen capture flag so active selections take precedence over transcript scrolling.

10. **[#11372](https://github.com/earendil-works/pi/pull/11372) — fix(ai): route Bedrock models Converse does not serve through Bedrock Mantle** (open)  
    Addresses #5363 for Bedrock API keys: 16 listed models (GPT-5.4–GPT-6.1, gpt-oss, Grok 4.3/4.6, Gemma 4) are only available on Mantle, the OpenAI-compatible endpoint, and currently fail as invalid Converse model identifiers. Stacked follow-ups #8572/#8573 add SigV4/IAM auth and Anthropic Messages routing.

## Hot Discussions

**Show and tell**

- **[#11503](https://github.com/earendil-works/pi/discussions/11503) — `pi-rs`: a Rust port of the headless core — behavioral parity verified atom by atom (+ a DeepSeek API compat finding)** (1 comment, 👍1)  
  A community Rust reimplementation of pi 0.74.2's headless core: agent loop, seven built-in tools, session JSONL, CLI surface, and system prompt, MIT-licensed with design credit to pi. Includes a DeepSeek API compatibility finding. Notable as both a parity-testing artifact and a signal of demand for a non-Node runtime.

## Feature Request Trends

- **Windows as a first-class platform.** The dominant cluster: usage-path consolidation (#7547), `.cmd`/`npx` execution (#10616), multiplexer-aware scrolling (#9656), and rendering cost on low-core Windows terminals (#9549).
- **Performance as a budgeted, measurable property.** Startup latency vs. jcode (#7739), per-frame transcript re-rendering (#9549), and O(entries) work on the render path (#11567) all frame performance as regressions to be guarded, not just optimized.
- **Deeper extension APIs.** Hook parity / `AgentSession`-backed runtimes (#10386), returnable `tool_result` results for gating and truncation (#11553), namespace-scoped `tool_search` (`+docs read_page`, #11558), deferred runtime reloads (#7948), and per-tool MCP exposure toggles (#11526).
- **Provider and catalog correctness.** Prompt-cache metadata for Bedrock Claude (#11528), Mantle routing for models Converse rejects (#11372), and configurable OAuth/Copilot timeouts (#11549).
- **Privacy and credential hygiene.** `0700` session directories (#10770) and moving `deviceId` out of shareable global settings (#10187).
- **Session storage semantics.** Default-directory resolution and `/resume` visibility (#11561, #11562) — small fixes with outsized UX impact.

## Developer Pain Points

- **Windows support is fragmented across run paths.** The 91-comment sink thread makes the root frustration explicit: users cannot tell which Windows setup is "blessed," and extension authors hit platform-specific breakage (`pi.exec` with `.cmd`, Zellij multiplexing, terminal status markers).
- **Long sessions degrade interactivity.** Two independent reports (#9549, #11567) trace slowness to work done on every render — full transcript re-emission and per-render session-name scans. The 21 ms vs. 1.5 ms keystroke latency at 400k entries is a concrete ceiling.
- **Prompt caching is easy to invalidate accidentally.** Tool insertion order in a `Map` (#11529) and missing `promptCache` metadata on Bedrock (#11528) silently convert cached turns into full-price ones.
- **Nondeterministic startup configuration.** A configured default model being replaced by a fallback in 20% of starts (#9884) undermines reproducibility for teams.
- **Provider adapters leak upstream quirks.** Replayed thinking blocks over relays (#10954), per-request Bedrock client construction forcing SSO round trips (#11527), oversized default output budgets triggering HTTP 402 (#11517), and fixed 15s/5s OAuth timeouts (#11549).
- **Session storage is surprising.** Sessions live in folders named after the original cwd, causing `/resume` to hide sessions and `/new` to inherit the wrong directory (#11561, #11562).
- **Subprocess and daemon lifecycle edge cases.** Unbounded `pi.exec` output (#11360 → #11546), panics on helper-thread exhaustion (#11433), and orphaned commands on daemon shutdown (#11544) are all "works until it doesn't" failure modes.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-29

## 1. Today's Highlights

The v0.26.3 release line landed across CLI, Desktop, and the TypeScript SDK, with generated UI cards now preserving inputs and state across navigation and split views and rendering live results only on MCP Apps–capable clients. Meanwhile, the issue tracker is dominated by CI and release-infrastructure work: a long-running Managed Agent gate failure has accumulated 79 comments, and a newly reported P1 config regression (`contextWindowSize` being silently removed) is the most user-impacting new bug of the day.

## 2. Releases

- **[v0.26.3](https://github.com/QwenLM/qwen-code/releases)** — Main stable release. Release notes in the source feed are placeholder-only (`qwen-release-notes:v1`), so the substantive changes are visible through the bundled Desktop and nightly entries below.
- **[desktop-v0.26.3](https://github.com/QwenLM/qwen-code/releases)** — Adds managed session permission rules and Auto approval ([#15448](https://github.com/QwenLM/qwen-code/pull/15448)) and hardens serve token handling in the CLI.
- **[v0.26.3-nightly.20261028.ca204559a7](https://github.com/QwenLM/qwen-code/releases)** — Includes `fix(config): report controls that were silently dropped` ([#15794](https://github.com/QwenLM/qwen-code/pull/15794)) and managed-mode documentation updates.
- **[sdk-typescript-v0.1.25](https://github.com/QwenLM/qwen-code/releases)** — Bundles CLI 0.26.3 (built from the same source ref as the SDK).
- **Highlighted change:** Generated UI cards retain inputs and state across navigation and split views, display live views after the initial calculation, and are gated to clients that can render MCP Apps ([#15611](https://github.com/QwenLM/qwen-code/pull/15611), [#15787](https://github.com/QwenLM/qwen-code/pull/15787)).

## 3. Hot Issues

1. **[#15855](https://github.com/QwenLM/qwen-code/issues/15855) — `contextWindowSize` gets automatically removed** `[P1, bug, configuration, need-information]` · 3 comments · New today. Users running local OpenAI-compatible backends (Ollama, 128k window) report their configured context limit is stripped from settings, which can cause unexpected truncation or context overflow. Highest-severity new user-facing bug in this window.
2. **[#14724](https://github.com/QwenLM/qwen-code/issues/14724) — Main CI failed: Managed Agent gate, no test identified** `[bug, autofix/skip]` · 79 comments · Open since 2026-10-19. The single loudest thread in the repo; the bot keeps appending commit markers without converging on a test attribution, which is exactly the failure mode the tracker is supposed to prevent.
3. **[#13078](https://github.com/QwenLM/qwen-code/issues/13078) — Daily dependency CVE audit failed** `[needs-triage, ci-cd]` · 35 comments · A month-long recurring failure. Whether the cause is a real high-severity advisory or a flaky npm audit endpoint, the signal has been unusable for 30 days, which is a supply-chain risk.
4. **[#15861](https://github.com/QwenLM/qwen-code/issues/15861) — Web Shell host-directory-picker smoke times out deterministically** `[P1, testing, ready-for-agent]` · 2 comments · Opened today. A PR-independent failure means every contributor pays a re-attribution round; the reporter explicitly frames it as a tax on unrelated PRs.
5. **[#15052](https://github.com/QwenLM/qwen-code/issues/15052) — Preserve independent main E2E validation when pending runs are replaced** `[P2, ci-cd, ready-for-human]` · 14 comments · Ref-scoped concurrency can cancel older pending runs even with `cancel-in-progress: false`, so intermediate `main` SHAs lose independent validation. Important for anyone relying on per-commit green status.
6. **[#15461](https://github.com/QwenLM/qwen-code/issues/15461) — Isolate the release-only host from the shared runner pool** `[P2, ci-cd, ready-for-human]` · 7 comments · The opt-in route to `ecs-qwen-release-host` was merged but the machine was never provisioned, so `publish` still competes for GitHub-hosted capacity. Directly tied to release latency.
7. **[#15783](https://github.com/QwenLM/qwen-code/issues/15783) — Move routine macOS/Darwin validation to overnight runs** `[P3, ci-cd, need-discussion]` · 4 comments · Proposes a repo-wide policy: overnight Asia/Shanghai macOS runs on `main`, Linux-only for PRs. A cost/capacity decision that would affect contributor feedback loops.
8. **[#11665](https://github.com/QwenLM/qwen-code/issues/11665) — Responses cleanup can break reasoning/tool-call adjacency** `[P2, core, CLOSED]` · 5 comments · A subtle correctness bug: replayed `reasoning` items and their `function_call` must be treated as one unit, but cleanup handled them separately. Closure is a meaningful reliability win for OpenAI Responses–compatible providers.
9. **[#1206](https://github.com/QwenLM/qwen-code/issues/1206) — Dynamic multi-model support for OpenAI-compatible APIs** `[feature-request]` · 3 comments, 2 👍 · Open since 2025-12-10. Users want to discover and switch among models from a single endpoint rather than hardcoding one. The longest-lived product request in this batch.
10. **[#15866](https://github.com/QwenLM/qwen-code/issues/15866) — Show Export and Restore inline on archived session rows** `[P3, ui, web-shell]` · 2 comments · Opened today. Small but representative of a steady stream of Web Shell ergonomics requests around session lifecycle actions.

## 4. Key PR Progress

1. **[#15620](https://github.com/QwenLM/qwen-code/pull/15620) — feat(channels): Microsoft Teams extension and host ingress** · Adds Teams support for personal chats, explicit group mentions, channel-thread isolation, and ordered final-text replies, usable standalone or embedded via `createTeamsIngress`. Largest new integration surface in this batch.
2. **[#15865](https://github.com/QwenLM/qwen-code/pull/15865) — feat(managed): live message paging in WebShell** · Reads the latest 100 materialized items directly from SQL (including sessions without a published snapshot), pages older messages on scroll-up, and keeps live traffic on the existing event stream.
3. **[#15863](https://github.com/QwenLM/qwen-code/pull/15863) — fix(cli): separate instruction files from auto memory in context usage** · Splits the context-usage breakdown into "Instruction files" (project/user/extension/language) vs. "Auto memory" (policy + catalog snapshots) across API, SDK types, and terminal output. Makes context budgeting auditable.
4. **[#15849](https://github.com/QwenLM/qwen-code/pull/15849) — feat(web-shell): extend one-click update coverage** · Extends the daemon one-click update to pnpm/yarn/bun global installs and to npm prefixes that require sudo, using a user-scoped isolated staging prefix.
5. **[#15837](https://github.com/QwenLM/qwen-code/pull/15837) — fix(cli): reply to SDK `initialize` only after config is ready** · Closes a race where the SDK control response could be sent before SDK MCP servers and staged SDK-only config were in place. Important for SDK embedders.
6. **[#15835](https://github.com/QwenLM/qwen-code/pull/15835) — fix(web-shell): keep "+ Add session" clickable when the catalog is empty** · The button was disabled exactly in the case the reload-on-open effect exists for, making the picker unreachable. Straightforward Split View usability fix.
7. **[#13445](https://github.com/QwenLM/qwen-code/pull/13445) — feat: render plan approval as markdown and capture plans as Todos** · Long-running draft replacing raw `<pre>` output in `ToolApproval.tsx` and turning approved plans into Session Workflow Todos. Notable as one of the older still-open feature PRs.
8. **[#14577](https://github.com/QwenLM/qwen-code/pull/14577) — fix(web-shell): prune split panes whose session left the active catalog** · Treats the daemon's `409 session_archived` as terminal, so an archived session's pane no longer stays empty and retries forever.
9. **[#15854](https://github.com/QwenLM/qwen-code/pull/15854) — perf(release): reuse verified native audio prebuilds across releases** · Fingerprints audio build inputs and reuses prior official `main` artifacts when identical, avoiding five target rebuilds per release. Direct answer to release-time complaints.
10. **[#15864](https://github.com/QwenLM/qwen-code/pull/15864) — fix(ci): skip source TUI gates for Companion-only changes** · Skips TUI parity/no-flicker jobs for VSCode-Companion-only PRs while still running them for manifests, bundler config, `scripts/`, and mixed changes.

## 5. Hot Discussions

No discussion data was provided in the source feed, so this section is omitted.

## 6. Feature Request Trends

- **Web Shell / Desktop ergonomics is the dominant product theme.** Keyboard shortcuts for Session Overview and Split View ([#13175](https://github.com/QwenLM/qwen-code/issues/13175), closed), inline Export/Restore on archived rows ([#15866](https://github.com/QwenLM/qwen-code/issues/15866)), sidebar category header alignment ([#15867](https://github.com/QwenLM/qwen-code/issues/15867)), host-supplied directory pickers ([#15186](https://github.com/QwenLM/qwen-code/issues/15186), closed), and macOS daemon `PATH` extension ([#12690](https://github.com/QwenLM/qwen-code/issues/12690), closed) all point at polishing the shell rather than adding core capability.
- **Session management correctness.** No-workspace sessions missing from Session Overview ([#11878](https://github.com/QwenLM/qwen-code/issues/11878), closed) and branch actions disappearing after disk restore ([#13782](https://github.com/QwenLM/qwen-code/issues/13782), closed) were both resolved; splitting, archiving, and restoring sessions remain active friction areas.
- **Provider/model flexibility.** Dynamic discovery and switching of models on OpenAI-compatible endpoints ([#1206](https://github.com/QwenLM/qwen-code/issues/1206)) remains the clearest unmet ask, reinforced by today's `contextWindowSize` regression for local Ollama users ([#15855](https://github.com/QwenLM/qwen-code/issues/15855)).
- **IDE and remote-development coverage.** The VS Code companion still needs coverage for forwarded-port host gating, IPv6 CSP, and Remote-SSH/WSL runs ([#12059](https://github.com/QwenLM/qwen-code/issues/12059)).
- **Skills lifecycle maturity.** Follow-up work from the user-scope Skill activation PR ([#15622](https://github.com/QwenLM/qwen-code/issues/15622)) shows the feature is shipping but still accumulating contract and lifecycle refinements.
- **Localization.** Browser-locale fallback (everything non-`zh-CN` collapsing to `en`) was raised for Goal cards ([#13391](https://github.com/QwenLM/qwen-code/issues/13391), closed) — the i18n surface is still only two locales.

## 7. Developer Pain Points

- **CI noise is crowding out real signal.** Bot-filed "no test identified" issues ([#14724](https://github.com/QwenLM/qwen-code/issues/14724) at 79 comments, [#15173](https://github.com/QwenLM/qwen-code/issues/15173) at 19, plus several per-commit Managed Agent gate reports) generate high comment volume without converging on attribution, and closed-when-green reports keep needing manual dismissal.
- **Deterministic, PR-independent test failures block everyone.** [#15861](https://github.com/QwenLM/qwen-code/issues/15861) makes the Web Shell smoke lane fail on unrelated PRs, forcing re-attribution on every change.
- **Release pipeline latency and capacity.** Post-publish fan-out still waits 37–50 minutes on GitHub-hosted runners ([#15465](https://github.com/QwenLM/qwen-code/issues/15465)), and the dedicated release host remains unprovisioned ([#15461](https://github.com/QwenLM/qwen-code/issues/15461)).
- **Per-SHA validation can be silently lost.** Concurrency replacement on `main` E2E means an intermediate commit may never get an independent verdict ([#15052](https://github.com/QwenLM/qwen-code/issues/15052)).
- **macOS runner cost pressure.** Repeated proposals to move Darwin validation overnight ([#15783](https://github.com/QwenLM/qwen-code/issues/15783)) reflect a queue/cost tradeoff contributors feel in PR turnaround.
- **Security auditing has been red for a month.** The daily dependency CVE audit ([#13078](https://github.com/QwenLM/qwen-code/issues/13078)) has produced 35 comments with no resolution, leaving teams unsure whether a real advisory is outstanding.
- **Configuration is not always trustworthy.** Silently dropped settings appeared both as a fix (`fix(config): report controls that were silently dropped`, [#15794](https://github.com/QwenLM/qwen-code/pull/15794)) and as a new P1 report ([#15855](https://github.com/QwenLM/qwen-code/issues/15855)) — users want the CLI to surface, not swallow, config it cannot apply.
- **Runtime environment fragility.** Historical crashes from missing full-ICU `Intl.Segmenter` on RHEL 10 ([#11747](https://github.com/QwenLM/qwen-code/issues/11747), closed) highlight a recurring class of host-runtime incompatibility without actionable diagnostics.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*