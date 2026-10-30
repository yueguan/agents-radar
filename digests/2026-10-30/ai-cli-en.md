# AI CLI Tools Community Digest 2026-10-30

> Generated: 2026-10-30 02:07 UTC | Tools covered: 7

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

# Cross-Tool AI CLI Comparison Report — 2026-10-30

## 1. Ecosystem Overview

The AI CLI ecosystem is in a **reliability-and-cost-hardening phase**. Releases are frequent — stable, nightly, SDK, desktop, and daemon tracks — but community attention has shifted from raw model capability to **agent correctness, quota accounting, session storage growth, permission semantics, and non-interactive/ACP parity**. Windows remains a recurring weak platform across several tools, while MCP/plugin authentication and lifecycle behavior are becoming a shared ecosystem bottleneck. PR throughput varies sharply: Codex and Qwen show high-velocity merge/release engineering, Pi and OpenCode are actively landing fixes, while Claude Code and Copilot CLI show high issue volume with comparatively low external PR throughput.

---

## 2. Activity Comparison

Counts below are as reported in the supplied digests. Where the digest did not provide a full tracker count, the number of **surfaced items** is shown with `†`. `N/A` means no discussion data was provided in this dataset; it does not imply the channel is inactive.

| Tool | Issues activity | PR activity | Discussions | Release status today |
|---|---:|---:|---:|---|
| **Claude Code** | 50 updated | 2 updated | N/A | **v2.1.314** — out-of-tree write blocking; IdP-group gateway routing |
| **OpenAI Codex** | ≥15 surfaced† | 17 listed | 10 | **rust-v0.166.0** — transcript selection/search, markdown consistency |
| **Gemini CLI** | 10 surfaced† | 10 listed | N/A | **v0.67.0-nightly** — README scope, inline history resize fix |
| **GitHub Copilot CLI** | 38 updated | 0 updated | N/A | **No release** |
| **OpenCode** | 13 surfaced† | 14 listed | N/A | **v2.2.2 / v2.2.1** + process-daemon releases |
| **Pi** | 50 updated | 23 updated | 2 | **No release** |
| **Qwen Code** | ≥15 surfaced† | 20 listed | N/A | **v0.26.4** CLI, SDK, Desktop + nightly |

**Notable signals:** Claude Code’s issue-to-PR ratio is highly asymmetric (50 issues vs. 2 PRs; one PR is README-only). Copilot CLI had heavy issue traffic but zero PR/release movement. Codex’s PR activity is largely a bot-authored merge train. Pi and Qwen show the strongest maintenance throughput in this window.

---

## 3. Shared Feature Directions

### Cost, quota, and prompt-cache determinism
Across Claude Code, Codex, OpenCode, Pi, Copilot CLI, and Qwen Code, users want **predictable, auditable spend**. Specific needs include:
- Per-entrypoint accounting parity — Claude Code’s headless `claude -p` quota penalty (#97074).
- Subagent and fan-out cost inclusion — OpenCode (#45417), Claude Code spinner/token overhead (#86879).
- Provider-reported cost and correct long-context/cache pricing — Pi (#6881), OpenCode (#59803).
- Stable prompt caches via deterministic tool ordering — Pi (#11529).
- Predictable quota resets rather than ad-hoc promotions — Codex (#57967), OpenCode free-tier mega-thread (#59387), Copilot PRU exhaustion (#4802).

### Session storage, compaction, and long-session resource control
This is the highest-severity recurring theme:
- Codex reports 16–165 GiB session growth from image/base64 re-persistence during compaction and subagent forks.
- OpenCode reports 13 GB `event` tables and O(n²) history re-reads.
- Copilot CLI reports 37 GiB RSS growth in long parallel-agent sessions and 300s compaction timeouts.
- Claude Code reports idle CPU burn from git polling and Windows VM leaks.
- Pi reports V8 OOM in multi-day sessions and transcript-rendering scaling issues.

Shared need: **user-facing session inspection/pruning, bounded compaction, image deduplication, and resource caps**.

### Permission and security controls with recovery paths
Communities want **explicit, composable policy** — not just restrictions:
- Claude Code: out-of-tree write blocking, trusted-device overrides, symlinked config-dir behavior.
- Codex: managed network policy, Windows sandbox denials, MCP approval metadata.
- Copilot CLI: default-deny tool allowlists and non-interactive sandboxing.
- OpenCode: `webfetch` permissions applied inside Code Mode, per-server TLS trust.
- Gemini CLI: sandbox gaps such as Podman sockets and path traversal in checkpoint fallback.
- Pi: session-file permissions and correct project-trust semantics.

A common failure pattern is **opaque denial without remediation** — the policy block is less damaging than the absence of a review/approval path.

### Remote, mobile, ACP, and non-interactive parity
Users increasingly run these tools in CI, daemons, remote sessions, and mobile clients:
- Claude Code Remote Control diverges across CLI, desktop, VS Code, and mobile.
- Codex has Android queue bugs and app-server/TMUX environment misattribution.
- Copilot CLI’s ACP surface lacks `contextTier`, `ask_user`, and non-blocking `session/prompt` semantics.
- OpenCode and Qwen Code both show daemon/Web Shell/SDK protocol-fidelity issues.
- Gemini CLI subagents need better observability and `/bug` context.

Shared ask: **same session identity, permissions, tools, and cancellation semantics across every surface**.

### MCP/plugin lifecycle and authentication robustness
MCP is becoming a first-class integration layer, but auth and lifecycle remain brittle:
- Copilot CLI: OAuth re-prompts, Entra token-cache failures, provider token-exchange failures.
- Claude Code: elicitation capability declared but not rendered; `.mcpb` install prompting; marketplace git polling.
- Codex: MCP tool approval metadata and credential leakage prevention.
- OpenCode: duplicate MCP processes and private-network TLS trust.
- Pi: extension lifecycle races around `session_start`, compaction, and shutdown.

### Subagent/agent-loop observability and control
Across Gemini CLI, OpenCode, Claude Code, Codex, Pi, and Qwen Code:
- Subagents report success after `MAX_TURNS` exhaustion (Gemini #22323).
- Agents hang indefinitely or terminate autonomous work prematurely.
- `/goal`, `/loop`, and stop controls do not always cancel pending work.
- Users want activity sidebars, trajectories in bug reports, and programmatic stop hooks.

---

## 4. Differentiation Analysis

| Tool | Primary focus | Target users | Technical approach |
|---|---|---|---|
| **Claude Code** | Enterprise permissioning, Remote Control, Cowork/desktop, gateway routing | Teams and enterprises using Anthropic models | Policy-rich CLI/desktop/VS Code integration; strong permission model but low external PR throughput |
| **OpenAI Codex** | Rust CLI/desktop/VS Code, managed app-server, MCP approvals, release automation | OpenAI ecosystem, enterprise network deployments | High-velocity automated merge/release engineering; strong transcript UX and platform hardening |
| **Gemini CLI** | Subagent runtime, sandboxing, AST-aware tooling, Google/Gemini ecosystem | Gemini and Google Cloud developers | Nightly-driven agent-runtime experimentation; safety and token-efficiency research |
| **GitHub Copilot CLI** | GitHub/org integration, ACP automation, MCP/Entra auth | GitHub Enterprise and CI automation users | Non-interactive/ACP-first surface; strong policy demand but slow fix throughput in this window |
| **OpenCode** | Provider-agnostic open tooling, Desktop/TUI, plugins, Zen/free tier | Power users, self-hosters, multi-provider teams | Fast client releases and broad provider support; regression-prone desktop cadence |
| **Pi** | Minimal headless core, TUI performance, provider abstraction, extensions | Terminal power users and extension developers | Correctness/perf-focused maintenance; clean separation of headless core from TUI |
| **Qwen Code** | Qwen/Alibaba ecosystem, self-hosted models, daemon/Web Shell/Desktop/SDK | Local/self-hosted and enterprise Qwen users | Multi-surface monorepo; heavy CI/release-infrastructure investment |

**Key contrast:** Claude Code and Copilot CLI are strongest on **enterprise policy and organizational integration**, but currently show weaker community-fix throughput. Codex, Pi, and Qwen Code are stronger on **release engineering and maintenance velocity**. OpenCode is the most **provider-agnostic and fast-moving**, but carries visible regression and quota-management risk. Gemini CLI is the most **agent-runtime/research-oriented**, with subagent reliability as its central weakness.

---

## 5. Community Momentum & Maturity

**Highest issue engagement:** Claude Code (#97074 at 115 👍), OpenCode (#59387 at 113 comments), Codex session-storage reports, and Copilot CLI’s long-running 403/model-listing thread. These are mature communities that now stress-test cost, policy, and scale rather than basic functionality.

**Highest PR/release velocity in this window:** Codex (bot merge train, rust-v0.166.0), OpenCode (two client releases plus daemon packages), Pi (50 issues and 23 PRs updated, large fix wave), and Qwen Code (v0.26.4 across CLI/SDK/Desktop plus nightly). These tools are iterating quickly, though OpenCode shows release-to-release desktop regressions and Qwen shows chronic CI attribution problems.

**Maturity vs. responsiveness:**
- **Claude Code** is enterprise-mature but has a visible volume-to-fix mismatch: 50 issues vs. 2 PRs, with one substantive community fix open since March.
- **Copilot CLI** has strong organizational reach but no PR/release movement today and persistent MCP/ACP authentication churn.
- **Gemini CLI** is nightly-active and improving security/agent-runtime correctness, but subagent reliability remains a P1-level trust issue.
- **Pi** looks operationally mature in this window: many privacy, path-handling, and performance fixes closed same-day.
- **Qwen Code** is investing heavily in CI/release infrastructure, but those failures are now a first-class contributor pain point.

---

## 6. Trend Signals

1. **Agent reliability is now more important than model novelty.** False subagent success, indefinite hangs, premature termination, and non-cancellable loops are recurring across Gemini CLI, OpenCode, Claude Code, Codex, Pi, and Qwen Code. Developers need **observable, cancellable, bounded agent loops**.

2. **Cost and storage are first-class product surfaces.** Token/quota accounting, prompt-cache stability, compaction behavior, and session pruning are no longer backend details. Tools without transparent accounting will struggle in CI and team billing scenarios.

3. **Permissions are shifting from “allow/deny” to “policy with recovery.”** The market wants explicit sandboxing, out-of-tree write protection, network policy, per-server TLS trust, and default-deny automation — but also actionable denial messages and override paths.

4. **Non-interactive and ACP usage is outpacing reliability.** Headless quota penalties, ACP session semantics, remote queues, and daemon protocol fidelity are becoming adoption blockers for automation-heavy teams.

5. **MCP/plugin authentication is the new integration tax.** OAuth persistence, Entra/SSO caches, token exchange, elicitation rendering, lifecycle timeouts, and credential leakage are shared ecosystem problems.

6. **Windows and terminal/platform fidelity remain underinvested.** VS Code startup failures, path casing, clipboard/env inheritance, terminal escape sequences, and VM leaks appear across Codex, Claude Code, Copilot CLI, Pi, and OpenCode.

7. **Release engineering itself is a competitive differentiator.** Bot merge trains, nightly channels, CI sharding, runner capacity, and release-integrity checks are visible in Codex, Qwen Code, Gemini CLI, and OpenCode. Communities now judge tools partly by **how reliably fixes ship**.

**Reference value for developers:** pin versions in CI, monitor per-entrypoint quota behavior, cap session/compaction storage, validate subagent success states independently, prefer explicit permission allowlists, test Windows and remote/ACP paths, and treat MCP auth/token lifecycle as a production dependency rather than a setup detail.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report
**Source:** github.com/anthropics/skills · **Data as of:** 2026-10-30

> **Data caveat:** all 50 PRs report `Comments: undefined` and `👍: 0` in the export. Ranking below therefore follows the source ordering (the list is pre-sorted by comment count) and is cross-validated with update recency, cross-referenced issues, and repeat-contributor signals. Issue comment counts are available and used directly.

---

## 1. Top Skills Ranking (most-discussed PRs)

**#1 — [PR #1742](https://github.com/anthropics/skills/pull/1742) — `mcp-builder`: MCP v2 compatibility**
`fix(mcp-builder): support mcp>=2 streamable_http_client import and custom headers` · @Kuldeeep18 · Sep 8 → Oct 8
- **Function:** Repairs `skills/mcp-builder/scripts/connections.py` after the `mcp>=2.0.0` breaking change — `streamablehttp_client` → `streamable_http_client`, and custom headers now require `create_mcp_http_client`/`http_client` instead of a direct kwarg.
- **Highlights:** Closes #1668; a pure upstream-SDK-breakage fix, so it blocks anyone building MCP servers on the current `mcp` release.
- **Status:** OPEN (draft-quality fix, awaiting maintainer review).

**#2 — [PR #1298](https://github.com/anthropics/skills/pull/1298) — `skill-creator`: trigger-eval correctness**
`fix(skill-creator): isolate trigger evals and handle Windows and runtime failures` · @MartinCajiao · Jun 10 → Sep 16
- **Function:** Fixes false misses/invalid scores in trigger evaluation: per-worker command probes competing, `select()` on subprocess pipes failing on Windows, unrelated tools aborting the scan, and runtime failures being mis-scored as non-triggers (which silently passes negative examples).
- **Highlights:** The longest-running and most structurally complex fix in the set; directly attacks eval *validity*, not just crashes. Four months open indicates non-trivial review burden.
- **Status:** OPEN.

**#3 — [PR #1245](https://github.com/anthropics/skills/pull/1245) — Net-new skill bundle**
`Add notion-spec-to-implementation and quantitative-resume-auditor skills` · @mrdesouzaphd-cmyk · Jun 2 → Oct 15
- **Function:** (a) `notion-spec-to-implementation` — decomposes Notion spec pages into implementable tasks with acceptance criteria and progress tracking; (b) `quantitative-resume-auditor` — a second, unrelated skill bundled into the same PR.
- **Highlights:** Classic community-submission pattern (multi-skill bundling), which is also the main friction point — reviewers generally prefer one skill per PR.
- **Status:** OPEN.

**#4 — [PR #1771](https://github.com/anthropics/skills/pull/1771) — `proofcore-contract-auditor` (Web3)**
`feat(skills): add proofcore-contract-auditor for smart contract notarization` · @ProofCore-Protocol · Sep 15 → Sep 16
- **Function:** Static analysis of Solidity/Rust contracts, with cryptographic audit proofs anchored to TON Blockchain via a zero-storage Merkle protocol.
- **Highlights:** Vendor-branded skill (company namespace + proprietary backend), which raises the same trust-boundary questions as Issue #492.
- **Status:** OPEN, no visible update since creation.

**#5 — [PR #1734](https://github.com/anthropics/skills/pull/1734) — `docx`: orphaned comment detection**
`Detect orphaned docx comments` · @rohitjain25 · Sep 6 → Sep 25
- **Function:** Identifies Word comments whose anchor text has been deleted — a real corruption class in generated/accepted-changes documents.
- **Highlights:** Empty summary in the PR body, which is likely why it stalls despite a well-scoped problem.
- **Status:** OPEN.

**#6 — [PR #1703](https://github.com/anthropics/skills/pull/1703) — `md2video-audio`**
`Add md2video-audio skill` · @70v-Yoyo · Sep 1 → Sep 15
- **Function:** Compiles Markdown → Marp slides → narrated MP4 with human-like TTS, at zero API cost.
- **Highlights:** Representative of the "Markdown-in, media-out" pipeline demand; "zero-cost" framing suggests local/offline tooling dependencies that need review.
- **Status:** OPEN.

**#7 — [PR #1980](https://github.com/anthropics/skills/pull/1980) — `webapp-testing`: command injection (CWE-78)**
`webapp-testing: avoid shell=True in with_server.py` · @Pcmhacker-piro · Oct 6 → Oct 25
- **Function:** Replaces `subprocess.Popen(..., shell=True)` (used to support `cd <dir> && <command>`) with a non-shell spawn that still supports directory chaining.
- **Highlights:** One of the few **security-class** PRs in the top set, and the most recently updated; high merge likelihood given the narrow blast radius.
- **Status:** OPEN.

**#8 — [PR #1792](https://github.com/anthropics/skills/pull/1792) — `docx`: silent LibreOffice timeout**
`fix(docx): report LibreOffice timeout as an error and verify the output` · @TINGyu123644 · Sep 19 → Sep 25
- **Function:** `accept_changes.py` no longer reports success on `soffice` timeout; success is claimed only after verifying `w:ins`/`w:del`/`w:moveFrom`/`w:moveTo` are absent from `word/*.xml`.
- **Highlights:** Fixes a *false-success* failure mode — the highest-severity category for agent-driven tooling, since it corrupts downstream trust silently.
- **Status:** OPEN.

> **Notable structural signal:** `skill-creator` supplies **3 of the top-20 PRs** ([#1298](https://github.com/anthropics/skills/pull/1298), [#1681](https://github.com/anthropics/skills/pull/1681), [#1997](https://github.com/anthropics/skills/pull/1997)) — it is the single most-contested surface in the repo.

---

## 2. Community Demand Trends (from Issues)

**① Trust boundaries and namespace governance — the dominant issue**
[Issue #492](https://github.com/anthropics/skills/issues/492) (46 comments, the highest-engagement item in the entire dataset) argues community skills are distributed under the `anthropic/` namespace, letting third-party skills impersonate official ones and inherit elevated permissions. Adjacent: [Issue #189](https://github.com/anthropics/skills/issues/189) (9 👍) on `document-skills` and `example-skills` shipping **identical content**, creating duplicate skills in the context window. *Demand: provenance/signing, namespace separation, and packaging hygiene.*

**② Distribution and org-level sharing**
[Issue #228](https://github.com/anthropics/skills/issues/228) (17 comments, 8 👍) — skills must be manually exported as `.skill` files and re-uploaded by each colleague; users want a shared org skill library or direct share link. *Demand: skill registry/sharing primitives, not more skills.*

**③ Evaluation and trigger-reliability tooling**
[Issue #556](https://github.com/anthropics/skills/issues/556) (13 comments, 7 👍) — `run_eval.py` produces a **0% trigger rate across all queries**; [Issue #1390](https://github.com/anthropics/skills/issues/1390) — `mcp-builder`'s `evaluation.py` scores 0/N against every real MCP server by swallowing `TextContent` serialization errors into fabricated tool errors; [Issue #1394](https://github.com/anthropics/skills/issues/1394) — `eval-viewer` display-path XSS via non-attribute-safe `escapeHtml`. *Demand: a trustworthy measurement harness before any skill-quality claim can be made.*

**④ Long-horizon agent state / memory**
[Issue #1329](https://github.com/anthropics/skills/issues/1329) (10 comments) — `compact-memory`, a symbolic-notation skill for compressing an agent's own notes and persistent memory. Complements the token-bloat complaint in [Issue #1487](https://github.com/anthropics/skills/issues/1487), where the `claude-api` skill eagerly injects **~156k tokens in a single tool call**. *Demand: context economy as a first-class skill concern.*

**⑤ Output quality gates**
[Issue #1385](https://github.com/anthropics/skills/issues/1385) (6 comments) — a three-gate pipeline (pre-task calibration → adversarial review → delivery verification); [Issue #412](https://github.com/anthropics/skills/issues/412) (CLOSED, 6 comments) — `agent-governance` for policy enforcement, threat detection, trust scoring, audit trails. *Demand: verification/review skills, adjacent to code review and test generation.*

**⑥ Skill description quality (triggerability) and document fidelity**
[Issue #2064](https://github.com/anthropics/skills/issues/2064) — `theme-factory`'s description states *what* it does but not *when to use it*, so it under-triggers; contrast [Issue #202](https://github.com/anthropics/skills/issues/202) (CLOSED), which argued `skill-creator` reads like human documentation rather than operational instruction. Concrete correctness bugs round this out: [Issue #2018](https://github.com/anthropics/skills/issues/2018) — `docx/merge_runs.py` writes XML with unbound namespace prefixes while reporting success; [Issue #1868](https://github.com/anthropics/skills/issues/1868) — off-by-one in `create_gradient_background` never emits its own `bottom_color`. *Demand: precise, machine-actionable skill metadata + verified office-format output.*

---

## 3. High-Potential Pending Skills (active, not yet merged)

| PR | Skill / Scope | Last activity | Why it may land soon |
|---|---|---|---|
| [#1975](https://github.com/anthropics/skills/pull/1975) | `pptx`: reject `--cols < 1` up front in `thumbnail.py` | 2026-10-27 | Narrow fix, closes #1809, converts a post-render `ValueError` into an argparse error — trivially reviewable |
| [#1980](https://github.com/anthropics/skills/pull/1980) | `webapp-testing`: remove `shell=True` (CWE-78) | 2026-10-25 | Security fix with a contained diff; security-class PRs typically fast-track |
| [#1681](https://github.com/anthropics/skills/pull/1681) | `skill-creator`: direct execution of `package_skill.py` + path docs | 2026-10-24 | Fixes a hard `ModuleNotFoundError`; from the same author as #1742, i.e. an engaged contributor |
| [#525](https://github.com/anthropics/skills/pull/525) | New `pyxel` skill — retro game dev (implementation, headless input runs, frame inspection) | 2026-10-23 | Long-lived (since Mar 5) but still receiving updates; mature reference set; zero sponsor conflict |
| [#1997](https://github.com/anthropics/skills/pull/1997) | `skill-creator`: correct YAML frontmatter scalar parsing | 2026-10-19 | Fixes description/quote/escape corruption feeding the eval + optimization commands — complements #1298 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | `algorithmic-art`: `wrapAround()` now actually wraps | 2026-10-18 | One-function modulo fix, closes #1897 |
| [#1976](https://github.com/anthropics/skills/pull/1976) | `webapp-testing`: report `textarea`/`select` correctly in `element_discovery.py` | 2026-10-18 | One-line fallback narrowing, closes #1891 |
| [#1245](https://github.com/anthropics/skills/pull/1245) | `notion-spec-to-implementation` + `quantitative-resume-auditor` | 2026-10-15 | Highest-ambition net-new bundle still active; needs splitting into two PRs to progress |
| [#1742](https://github.com/anthropics/skills/pull/1742) | `mcp-builder`: MCP v2 import/header compatibility | 2026-10-08 | Blocks all MCP-server skill work on current `mcp`; strong merge candidate |

**Pattern worth flagging:** @gerardrecinto contributes a cluster of three issue-linked micro-fixes ([#1975](https://github.com/anthropics/skills/pull/1975), [#1976](https://github.com/anthropics/skills/pull/1976), [#1977](https://github.com/anthropics/skills/pull/1977)) — each closes a numbered issue with a one-function diff. This is the highest-probability merge cohort in the backlog and the cleanest contribution template for new community members.

---

## 4. Skills Ecosystem Insight

**The community's demand has shifted from *adding* skills to *hardening the skill supply chain* — provenance and namespace trust, package/duplicate hygiene, and a trigger-evaluation harness that actually reports the truth — because every net-new skill inherits the credibility of a toolchain that currently mis-reports its own results.**

---

# Claude Code Community Digest — 2026-10-30

## 1. Today's Highlights

A fresh release (**v2.1.314**) tightens the permission model with `permissions.blockWritesOutsideWorkingDirectories`, making Write/Edit/NotebookEdit refuse out-of-tree paths in *every* permission mode, and adds IdP-group targeting for Claude apps gateway upstreams. The issue tracker remains dominated by cost-accounting anomalies — the headless `claude -p` token-burn report (**#97074**) has now crossed 115 👍 and 60 comments — plus a steady drumbeat of Remote Control, permission-mode, and plugin/MCP reliability bugs across desktop, VS Code, and Windows.

---

## 2. Releases

**v2.1.314**
- **`permissions.blockWritesOutsideWorkingDirectories`** — Write, Edit, and NotebookEdit now refuse paths outside the working directories regardless of permission mode. This closes a long-standing gap where non-default permission modes could still mutate files outside the project root (relevant to the cluster of permission/auto-mode complaints like #101018 and #98044).
- **`groups` for Claude apps gateway upstreams** — upstreams can now declare an optional list of IdP groups; only developers in those groups are routed to the upstream. Useful for staged rollouts and team-scoped gateway routing.
- ⚠️ Release notes were truncated at the second bullet — treat this summary as partial.

---

## 3. Hot Issues

1. **[#97074](https://github.com/anthropics/claude-code/issues/97074)** — *Headless `claude -p` consumes ~1.8× more of the 5-hour window per token than interactive CLI* (60 comments, 115 👍). The single highest-engagement item today. A measurable 80% quota penalty for identical workloads in `sdk-cli` versus `cli` undermines any CI/automation budget planning; the 👍 count suggests broad reproducibility across teams.
2. **[#56089](https://github.com/anthropics/claude-code/issues/56089)** — *Allow configuring Cowork VM (vhdx) bundle storage location* (14 comments, 59 👍). Windows users are forced onto `C:` for multi-GB VM images; a straightforward configurability gap with strong sustained demand since May.
3. **[#88731](https://github.com/anthropics/claude-code/issues/88731)** — *Artifact tool missing in sessions spawned by `claude remote-control` (server mode)* (10 comments). Notable asymmetry: the same machine/account works via `claude --remote-control` but not the server-mode path — a strong repro signal for a tool-injection bug.
4. **[#86879](https://github.com/anthropics/claude-code/issues/86879)** — *Supported way to disable the `agent_summary` spinner* (8 comments, 70 👍). Measured at **22.7% of requests and ~18% of weighted tokens** in subagent-heavy sessions. High ratio of 👍 to comments = quiet, widespread agreement that decorative spinner text should not cost quota.
5. **[#82501](https://github.com/anthropics/claude-code/issues/82501)** — *`send_message` from unattended sessions worked 2026-07-21, hard-rejected 2026-07-30* (8 comments). An undocumented behavior reversal with no override path — a reliability problem for scheduled automation.
6. **[#88901](https://github.com/anthropics/claude-code/issues/88901)** — *Embedded Claude Code declares elicitation capability but never renders server-initiated elicitations* (7 comments). MCP servers hang until tool timeout; capability negotiation lying to servers is a protocol-integrity issue, not just a UI bug.
7. **[#100114](https://github.com/anthropics/claude-code/issues/100114)** — *Windows desktop: Remote Control not restored after relaunch; sessions archived on mobile* (7 comments). Silent auto-update → lost session state → cross-device archive churn. A trust-eroding failure mode for the Remote Control beta.
8. **[#37569](https://github.com/anthropics/claude-code/issues/37569)** — *Option to disable streamed response output in TUI* (6 comments, 27 👍). Long-running accessibility/focus request; still open after seven months.
9. **[#103864](https://github.com/anthropics/claude-code/issues/103864)** — *"Require trusted devices" silently blocks agent-to-session actions, no approve-on-device override* (4 comments). `create_session`, `list_events`, `send_message` all refused with no recovery path — a security feature that becomes an availability outage.
10. **[#98044](https://github.com/anthropics/claude-code/issues/98044)** — *Auto-memory writes prompt every time since 2.1.280 when `CLAUDE_CONFIG_DIR`'s `projects/` is symlinked* (2 comments, has repro). Allow rules, hooks, and auto mode all fail to approve — a regression that punishes multi-profile setups.

*Also worth watching:* **[#105894](https://github.com/anthropics/claude-code/issues/105894)** — a user message that was never recorded appeared in model context (potential context-integrity issue), and **[#106085](https://github.com/anthropics/claude-code/issues/106085)** — idle sessions burning 1.3–1.8 CPU cores each on `git ls-remote --tags` against local directory marketplaces.

---

## 4. Key PR Progress

⚠️ **Only 2 pull requests were updated in the last 24h** — the repo's PR throughput is very low relative to issue volume (2 PRs vs. 50 updated issues), which is itself a signal worth flagging.

1. **[#32751](https://github.com/anthropics/claude-code/pull/32751)** — `fix(hookify): map stop/prompt events to correct field names` (OPEN, updated 2026-10-29). Fixes #32153: Hookify rules using the simple `pattern` syntax with `event: stop` or `event: prompt` never fire, because `config_loader.py` maps them to `field='content'`, which `rule_engine._extract_field()` cannot resolve for Stop/UserPromptSubmit payloads. This is a silent-failure bug — hooks simply don't run, with no error surfaced.
2. **[#57333](https://github.com/anthropics/claude-code/pull/57333)** — `Update README.md` (CLOSED, updated 2026-10-29). Documentation-only; no functional impact.

*Takeaway:* the community is filing bugs far faster than external PRs are landing. Hookify (#32751) is the only substantive community fix in flight, and it has been open since March.

---

## 5. Hot Discussions

Omitted — no Discussions data was provided in this dataset.

---

## 6. Feature Request Trends

Distilled from the 30 surfaced issues:

| Direction | Representative issues | Signal |
|---|---|---|
| **Cost & quota transparency/control** | #97074, #86879 | Highest engagement in the dataset. Users want per-entrypoint accounting parity and the ability to switch off token-consuming "decoration" (spinners, summaries). |
| **Granular permission & security controls** | #103864, #101018, #98044, v2.1.314 | Demand for explicit overrides, not just restrictions: approve-on-device, override auto-mode denials from mobile/web, and correct behavior under symlinked config dirs. |
| **Desktop/Cowork configurability & stability** | #56089, #100114, #91265, #94536 | Storage location control, Remote Control state persistence across restarts, VM resource leaks, and input-flow regressions in the Code tab. |
| **TUI & IDE ergonomics** | #37569, #16078, #92108, #97535 | Disable streaming, show active skill in status line, `/diff` covering `--add-dir` paths, and a VS Code "add file to chat" command. Small asks, long-standing. |
| **Plugin / MCP lifecycle robustness** | #95683, #106087, #106085 | `.mcpb` `user_config` prompting for marketplace installs, retry-after-startup-failure, and stopping marketplace git polling from spinning idle sessions. |
| **Remote Control parity across surfaces** | #88731, #96352, #100114, #101018 | The beta consistently diverges between CLI, desktop, VS Code Remote-SSH, and mobile — duplicate `--resume` instances, missing tools, archived sessions. |

---

## 7. Developer Pain Points

1. **Silent, unmeasured cost multipliers.** The two most-upvoted issues (#97074, #86879) both describe token/quota overhead invisible to the user — an 1.8× headless penalty and a spinner worth ~18% of weighted tokens. Developers cannot budget CI or subagent workloads when overhead is undocumented and varies by entrypoint.
2. **Permission modes that fail unpredictably — in both directions.** Users report auto mode *denying* explicitly-approved low-stakes edits (#101018), *blocking* agents entirely under "Require trusted devices" (#103864), and *prompting every time* when config dirs are symlinked (#98044). The mental model of `defaultMode` / allow rules / hooks is not composable.
3. **Remote Control is the least reliable surface.** Missing Artifact tool in server mode (#88731), duplicate `--resume` processes over VS Code Remote-SSH (#96352), and state loss after Windows desktop relaunch (#100114) all point to session-identity handling as the root weakness.
4. **Idle resource burn.** A 1.3–1.8-core git polling loop per idle session (#106085), a persistent Windows kernel pool leak from the Cowork VM (#91265), and orphaned processes / inhibited sleep on Linux (#89110). Background session pools amplify all three linearly.
5. **Failure modes with no recovery path.** MCP servers cannot self-recover after startup failure — the 15-minute failure cache then blocks all new sessions (#106087). Elicitation requests hang to tool timeout (#88901). `send_message` from scheduled tasks was silently hard-rejected (§#82501). In each case the developer has no lever.
6. **Undocumented behavior reversals between releases.** The 2026-07-21 → 07-30 `send_message` change and the 2.1.280 symlink regression arrived without migration notes, forcing teams to pin versions defensively.
7. **Volume-to-fix mismatch.** 50 issues updated in 24h against 2 PRs, one of which is a README edit. The Hookify event-mapping fix (#32751) has sat open since March — hook authors are shipping rules that silently never fire.

---
*Generated from github.com/anthropics/claude-code data for 2026-10-30. Release notes and PR summaries were partially truncated in the source data.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-30

## 1. Today's Highlights

Codex shipped **rust-v0.166.0**, headlined by keyboard-driven transcript selection with integrated search and configurable shortcuts — a long-standing UX gap finally closed. Meanwhile, the issue tracker is dominated by **session-storage blowups from repeated image/base64 re-persistence during compaction**, with multiple reports in the 16–165 GiB range; one major multi-agent V2 variant (#34268) was closed. Windows remains the most unstable platform, with the top-engagement issue being a VS Code extension UI startup failure (42 comments, 39 👍).

## 2. Releases

**rust-v0.166.0** ([release](https://github.com/openai/codex/releases))
- **Transcript navigation**: keyboard-driven selection with word, line, and page motions, integrated search, and configurable shortcuts (#57221–#57224). This directly addresses the long-running "can't search the chat" complaint (see Discussion #8582).
- **Markdown consistency**: uniform rendering across async questions, choices, answers, and transcript replies (#57259, #57292).
- **Model update**: release notes reference an added "GPT…" model, but the provided changelog is truncated — treat the model announcement as unconfirmed until the full notes are available.

## 3. Hot Issues

1. **[#56691](https://github.com/openai/codex/issues/56691) — Codex 26.1021.22027 fails to start UI in VS Code 1.143.1 on Windows** (42 comments, 39 👍). The highest-engagement issue of the day: the extension loads but renders no UI. A near-duplicate report (#58150) on VS Code 1.144.0 suggests a broader Windows/Electron startup regression rather than a single bad install.
2. **[#45403](https://github.com/openai/codex/issues/45403) — Windows "Full access" cleanup denied with opaque "blocked by policy" and no review path** (28 comments, 6 👍). Documents a trust-breaking mismatch: the permission UI promises unrestricted file access, yet sandbox policy silently vetoes an explicitly authorized `Remove-Item`. No remediation affordance is offered.
3. **[#26633](https://github.com/openai/codex/issues/26633) — Desktop automations ignore timezone for RRULE scheduling** (26 comments, 8 👍). Weekly schedules execute as if `BYHOUR` were UTC; explicit `DTSTART;TZID=Europe/Paris` has no effect. High-impact for anyone relying on scheduled agent runs.
4. **[#48500](https://github.com/openai/codex/issues/48500) — Managed app-server hooks run with the first client's TMUX_PANE (0.157 regression)** (21 comments, 25 👍). Since 0.157, lifecycle hooks execute inside a shared daemon that inherited the spawning TUI's environment, so hook events are attributed to the wrong terminal pane. Strong upvote ratio signals broad multi-pane/multi-session usage.
5. **[#34268](https://github.com/openai/codex/issues/34268) — [CLOSED] Multi-agent V2 full-history forks duplicate compaction snapshots and images, >100 GiB session growth** (17 comments, 7 👍). The canonical write-up of the storage-bloat family; closure is a positive signal that the multiplicative-fork path has been addressed.
6. **[#35458](https://github.com/openai/codex/issues/35458) — Screenshots re-persisted on every compaction and inherited by subagent forks, ~165 GiB (95% base64 images)** (16 comments). The most extreme reported case; illustrates that images, not text, are the dominant storage cost.
7. **[#36596](https://github.com/openai/codex/issues/36596) — Codex terminates active autonomous work despite explicit "continue until user input required"** (16 comments, 6 👍). A model-behavior/agentic-control complaint: long-running tasks stop early, undermining unattended workflows.
8. **[#41708](https://github.com/openai/codex/issues/41708) — Android Remote message queue can stay stuck after host turn completes** (13 comments, 3 👍). Messages enter the visible queue but never reach the Windows host; the mobile client shows a stale Stop control. Mobile continuity is only as good as its recovery path.
9. **[#35485](https://github.com/openai/codex/issues/35485) — Windows: bundled `node_repl` MCP processes leak one-per-thread until app-server exit** (10 comments). A slow resource leak that compounds with thread count — relevant to long-lived desktop sessions.
10. **[#58115](https://github.com/openai/codex/issues/58115) — Sandboxing/rmcp-client: Seatbelt glob subtree suffix omitted; `[::1]` OAuth callback binds `0.0.0.0`** (1 comment). Low discussion volume but high severity: three distinct security-boundary edge cases, including an OAuth callback listening on all interfaces. Worth immediate triage.

*Also notable:* #55461 (repeated send failures, timeouts, UI going dark), #55659 (paused iframe Document responses after CUA execution), #50309 (`auto_review.extra_policy` silently dropped), #56860 (Linux leaks ~24.6 GB of marketplace staging dirs, ~10 GB/hour), #43618 (Linux completed-turn collapse hides replies, with root cause and patch).

## 4. Key PR Progress

Nearly all of today's PR activity is a high-velocity, bot-authored (`copyberry[bot]`) merge train — worth noting as a signal of automated release engineering rather than individual contributions.

1. **[#58202](https://github.com/openai/codex/pull/58202) — Keep chats accessible across model-provider changes.** Prevents history from disappearing from resume lookup or failing to open when the server rejects resumption due to provider mismatch, missing config, or policy conflict.
2. **[#58196](https://github.com/openai/codex/pull/58196) — Require Bazel release artifacts and publish DotSlash manifests.** Removes Bazel `continue-on-error`, making Bazel builds/signing/packaging blocking for releases — a release-integrity hardening step for desktop consumers.
3. **[#58182](https://github.com/openai/codex/pull/58182) — Support managed network requirements under `permissions.network`.** Extends `requirements.toml` from filesystem to network constraints, applied through config loading, network approvals, and Windows sandbox policy validation.
4. **[#58165](https://github.com/openai/codex/pull/58165) — Keep remote plugin bundle URLs out of diagnostics.** Prevents signature tokens in download-URL query parameters from leaking into errors and debug output. Small change, real credential-hygiene impact.
5. **[#58161](https://github.com/openai/codex/pull/58161) — Include tool names and annotation hints in MCP approval metadata.** Adds `tool_name` plus `readOnlyHint`/`destructiveHint`/`idempotentHint`/`openWorldHint` to approval elicitations, enabling more informed consent UX for MCP tools.
6. **[#58159](https://github.com/openai/codex/pull/58159) — Feature flag for code-mode host output truncation.** Disabled-by-default `code_mode_host_output_truncation` requests server-side truncation over stdio and gRPC, with injected providers unaffected.
7. **[#58157](https://github.com/openai/codex/pull/58157) — Expose system proxy support as an experimental setting.** Adds "System proxy" to `/experimental` for macOS/Windows, keeps `respect_system_proxy` off by default, and updates doctor guidance — a practical unblock for corporate-network users.
8. **[#58147](https://github.com/openai/codex/pull/58147) — Return standalone web search failures to the model.** Converts fatal turn-aborting errors into unsuccessful tool outputs the model can reason about and recover from.
9. **[#58140](https://github.com/openai/codex/pull/58140) — Route standalone web search through the thread's workspace context.** Fixes searches bypassing workspace routing by hitting the bootstrap backend instead of the discovered workspace backend.
10. **[#58168](https://github.com/openai/codex/pull/58168) — Create missing parent directories for MXC deny paths on Windows.** MXC requires deny-path parents to exist even when the path itself doesn't; this prevents policy-expansion failures.

*Also merged:* #58198 (centralized `ConfigToml` TOML deserialization), #58166 (explicit rollout recording failures/completion), #58172 (trace completion + payload UTF-8 validation on replay), #58195 (remote read compatibility with older executors), #58190 (custom providers can opt into Cyber access programs), #58144 (additional-context count/size/truncation metrics), #58130 (pasted text in skills search).

## 5. Hot Discussions

**Ideas**
- **[#57967](https://github.com/openai/codex/discussions/57967) — 1–2 free Codex usage resets per month for ChatGPT Plus.** Asks to convert occasional promotional resets into a predictable monthly allowance. Predictability, not raw quota, is the stated ask.

**Q&A**
- **[#8582](https://github.com/openai/codex/discussions/8582) — Search/Find in the active chat history** (2 comments, 4 👍). `Ctrl-F` always targets the editor, and no context-menu search exists. Notably, v0.166.0's transcript search is a direct answer to this thread.
- **[#5210](https://github.com/openai/codex/discussions/5210) — Any way to clear the text area?** A long-lived (Oct 2025) small UX gap: no shortcut to clear a accidentally pasted large prompt.

**Show and tell**
- **[#53803](https://github.com/openai/codex/discussions/53803) — swe-mux: run several Codex sessions with live status, and the same sessions on your phone.** Runs the real `codex` CLI in a real terminal with no proxying; surfaces working/done/waiting state and mirrors sessions to mobile.
- **[#58093](https://github.com/openai/codex/discussions/58093) — OpenFusion: orchestrator/router for subtask handling.** Routes subtasks to the cheapest capable model, with a judge escalating on weak attempts; ships benchmarks.
- **[#58104](https://github.com/openai/codex/discussions/58104) — Kilobat: networking for AI agents and humans.** Mac menu-bar app for network monitoring/diagnostics plus a free OSS CLI + MCP so agents can inspect traffic; tracks Codex, Claude Code, and Gemini usage.
- **[#57746](https://github.com/openai/codex/discussions/57746) — Zozo: inspectable issue-to-PR example and review checklist.** Coordinates coding runtimes against a backlog; shares a public issue/diff/CI-log example plus review checks.
- **[#58031](https://github.com/openai/codex/discussions/58031) — Using Codex with current Alibaba Cloud docs and official citations.** Practitioner note on verifying cloud-specific facts (endpoints, regions, model names) rather than trusting generated output.
- **[#53552](https://github.com/openai/codex/discussions/53552) — NOS: paste an HTML page, fix wording on screen, share as a link.** In-rendered-page editing with round-tripped source.
- **[#51338](https://github.com/openai/codex/discussions/51338) — Brok: what should an AI assistant actually do to be useful in a dev workflow?** Design discussion pushing beyond the chat-interface model.

## 6. Feature Request Trends

- **In-transcript search and navigation.** Discussion #8582 explicitly asked for chat-history search; v0.166.0's keyboard selection + integrated search is the direct response. Expect follow-ups around discoverability and default keybindings.
- **Predictable usage economics.** #57967 requests scheduled monthly resets rather than ad-hoc promos — a shift from "more quota" to "stable, plannable quota."
- **Session lifecycle and storage management.** The largest cluster of issues implies a feature gap: no user-facing tooling to inspect, prune, or reconcile session data. #55439 asks for a safe, non-destructive reconciliation command for duplicate rollouts / missing DB rows — effectively `codex doctor --fix`.
- **Multi-session orchestration and external observability.** Show-and-tell projects (#53803, #58093, #58104) all orbit running many agents concurrently and knowing which one needs attention — a capability the core product does not yet expose natively.
- **Enterprise/network configurability.** #58182 (managed network policy) and #58157 (system proxy opt-in) reflect demand for centrally administered, policy-compliant deployments.
- **Mobile/remote continuity.** #41708 and #53803 both target "same sessions on your phone," suggesting remote access is being used in practice faster than its reliability is being hardened.

## 7. Developer Pain Points

- **Session storage explosion (highest-severity recurring theme).** Seven overlapping reports — #34268 (110 GiB), #35458 (~165 GiB, 95% base64 images), #39013 (16.9 GB), #33735 (24 GB), #40323 (16 GiB), #23257, #28916 — describe the same mechanism: compaction checkpoints re-embed full images, then multi-agent forks inherit them multiplicatively. Consequences extend beyond disk: #28916 reports input lag and constant spinners on image-heavy histories. The closure of #34268 is progress, but the number of open duplicates suggests users aren't confident it's fully fixed.
- **Windows as the weak platform.** UI startup failure in VS Code (#56691, #58150), opaque sandbox denials on "Full access" (#45403), MCP process leaks (#35485), workspace settings failing to load (#54168), and stale auto-review scope (#58080). Windows users encounter both correctness and diagnosability problems.
- **Opaque policy decisions with no recourse.** #45403 and #58080 share a pattern: the agent is blocked by policy, the message is non-actionable, and no review/approval path exists. This erodes trust in the permission model more than the block itself.
- **Environment misattribution in shared daemons.** #48500 (TMUX_PANE inherited from first client) is a class of bug that will recur as the managed app-server becomes the default multiplexing layer.
- **Premature termination of autonomous work.** #36596: explicit "continue until user input is required" instructions are not honored, limiting value for long unattended runs.
- **Connectivity and queue reliability.** #55461 (send failures, timeouts, dark UI) and #41708 (stuck Android queue with stale Stop control) indicate recovery paths are weaker than the happy path.
- **Small but persistent UX gaps.** No clear-input shortcut (#5210), no in-chat search until v0.166.0 (#8582), collapsed turns hiding replies on Linux (#43618), image lightbox not fitting the window under zoom (#26851), and the macOS Pet overlay swallowing no input and being unmovable (#45397).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-30

## 1. Today's Highlights

A new nightly (`v0.67.0-nightly.20261030.gb633c0703`) landed with a README scope clarification and a CLI fix for inline history re-rendering after terminal resizes. Activity remains concentrated on agent/subagent reliability: the top issue, `#22323`, shows subagents reporting `status: "success"` / `Termination Reason: "GOAL"` after actually exhausting `MAX_TURNS`. Security and correctness PRs also moved forward, including a path-traversal fix in checkpoint fallback and an MCP discovery timeout bound.

## 2. Releases

**v0.67.0-nightly.20261030.gb633c0703** ([release](https://github.com/google-gemini/gemini-cli/releases))
- `docs(readme)`: clarifies Gemini CLI scope, limitations, and tool landscape ([#29995](https://github.com/google-gemini/gemini-cli/pull/29995)).
- `fix(cli)`: bounds inline history re-render after terminal width changes (PR ref truncated in feed).
- Version bump PR: [#30045](https://github.com/google-gemini/gemini-cli/pull/30045) (automated nightly bump).

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent MAX_TURNS recovery misreported as GOAL success** (P1, 13 comments, 👍2). `codebase_investigator` returns `success`/`GOAL` despite hitting the turn limit before doing any analysis. Why it matters: silently masks agent failure and corrupts downstream trust in subagent results — the highest-engagement thread of the day.
2. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs indefinitely** (P1, 8 comments, 👍8). Simple tasks like folder creation stall for an hour; instructing the model not to delegate works around it. Strong community upvotes signal broad impact.
3. **[#28004](https://github.com/google-gemini/gemini-cli/issues/28004) — Duplicate tool results for completed shell calls** (P2, 9 comments). Reproducible with a Gemini-compatible provider; duplicate `functionResponse` submissions can corrupt conversation state.
4. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Zero-dependency OS sandboxing + post-execution intent routing** (P2, 9 comments). Proposes leaning into Gemini 3's native bash affinity (`grep`/`sed`/`awk`) without sacrificing sandboxing — a design-level direction for the agent runtime.
5. **[#28416](https://github.com/google-gemini/gemini-cli/issues/28416) — GlobTool returns raw symlink paths but resolves internally** (P2, 7 comments). Inconsistency causes downstream `read_file`/edit failures on macOS and symlinked workspaces.
6. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini rarely invokes skills or subagents unprompted** (P2, 7 comments). Anecdotal but recurring: explicit instructions work, implicit routing does not.
7. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — Assess AST-aware file reads, search, and codebase mapping** (P2, 7 comments). Epic exploring fewer misaligned reads, smaller token footprints, and better navigation.
8. **[#28999](https://github.com/google-gemini/gemini-cli/issues/28999) — macOS Seatbelt profile misses Podman API sockets** (security, 1 comment). Gap in the container-runtime deny list (`podman.sock`, `~/.podman`) leaves a sandbox-escape vector via privileged containers.
9. **[#29036](https://github.com/google-gemini/gemini-cli/issues/29036) — Concurrent extension installs corrupt each other** (2 comments). `installOrUpdateExtension()` checks existence then awaits before writing; parallel installs or `update --all` can race and clobber destinations.
10. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — Agent should discourage destructive behavior** (P2, 3 comments, 👍1). Model reaches for `git reset` / `--force` when safer paths exist — relevant to trust and safety in automated workflows.

## 4. Key PR Progress

1. **[#29876](https://github.com/google-gemini/gemini-cli/pull/29876) — Prevent path traversal in legacy checkpoint fallback** (P1, security, open). Fixes traversal in `Logger._getCheckpointPath()` / `deleteCheckpoint()` when using the unencoded `checkpoint-${tag}.json` name.
2. **[#29767](https://github.com/google-gemini/gemini-cli/pull/29767) — Bound MCP `tools/list` discovery with a 60s default timeout** (P1, closed). Discovery previously inherited the 10-minute tool-call timeout, stalling silently for unresponsive servers.
3. **[#29765](https://github.com/google-gemini/gemini-cli/pull/29765) — Make `FileKeychain` writes atomic and serialized** (security, closed). Prevents silent credential loss from concurrent writes and lockout from interrupted writes.
4. **[#29875](https://github.com/google-gemini/gemini-cli/pull/29875) — Add `gemini-4-argon` model support** (open). Registers the model in `VALID_GEMINI_MODELS` / `defaultModelConfigs` and adds `isGemini4Model` capability checks.
5. **[#29879](https://github.com/google-gemini/gemini-cli/pull/29879) — Discard buffered tool calls on mid-stream error or retry** (open). Prevents orphaned `ToolCallRequest` events across `Retry`, `UserCancelled`, `LoopDetected`, and `ContextWindowWillOverflow`.
6. **[#29881](https://github.com/google-gemini/gemini-cli/pull/29881) — ACP retry progress + retry-delay cap + model fallback** (open). Fixes silent 429 retries in `--acp` mode, including multi-hour quota-reset messages misclassified as transient capacity errors.
7. **[#30044](https://github.com/google-gemini/gemini-cli/pull/30044) — Windows libuv `dev: 0` stat handling in `readSecureFileBuffer`** (open). Affects Node 22.12.0–22.16.x bundling libuv 1.49.x–1.50.0, where path-based `stat` reports `dev: 0`.
8. **[#30043](https://github.com/google-gemini/gemini-cli/pull/30043) — Expose tool call IDs to `BeforeTool` / `AfterTool` hooks** (open). Enables integrations to pair pre/post hook events; field kept optional.
9. **[#29878](https://github.com/google-gemini/gemini-cli/pull/29878) — Report blocked path segment instead of misleading “path outside workspace”** (open). Improves diagnosability of `Config.validatePathAccess` failures.
10. **[#29882](https://github.com/google-gemini/gemini-cli/pull/29882) — Preserve file search results across query changes** (open). Result caching could hide valid matches (e.g. `Src` then `Src*`); caching now applies only to identical queries.

## 5. Hot Discussions

No Discussions data was provided for this period, so this section is omitted.

## 6. Feature Request Trends

- **Subagent observability and debuggability**: expose subagent trajectories via `/chat share` ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)), include subagent context in `/bug` reports ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)), and sprint-tracked subagent work ([#20195](https://github.com/google-gemini/gemini-cli/issues/20195)).
- **AST-aware / token-frugal tooling**: AST reads and codebase mapping ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) and “tactful extraction” to cut ~36.6k-token/turn baselines ([#19561](https://github.com/google-gemini/gemini-cli/issues/19561)).
- **Persistent task tracking**: replace in-context `WriteToDo` with file-based CRUD to avoid context rot ([#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000)).
- **Sandboxing and safety**: zero-dependency OS sandboxes ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)) and discouraging destructive git operations ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)).
- **Browser agent resilience**: honor `settings.json` overrides ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)), session takeover / lock recovery ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232)), and Wayland support ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)).
- **Agent self-awareness**: accurate CLI flags, hotkeys, and self-execution guidance ([#21432](https://github.com/google-gemini/gemini-cli/issues/21432)).

## 7. Developer Pain Points

- **Subagent reliability is the dominant frustration**: indefinite hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), false success on turn-limit exhaustion ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)), ignored configuration ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)), and low spontaneous skill/subagent use ([#21968](https://github.com/google-gemini/gemini-cli/issues/21968)).
- **Path and filesystem edge cases**: symlinked agent definitions not discovered ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)), glob/realpath divergence breaking edits ([#28416](https://github.com/google-gemini/gemini-cli/issues/28416)), and confusing “outside workspace” errors ([#29878](https://github.com/google-gemini/gemini-cli/pull/29878)).
- **Tool orchestration fragility**: duplicate tool results ([#28004](https://github.com/google-gemini/gemini-cli/issues/28004)), 400 errors past the tool-count ceiling ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)), and stalls on interactive prompts like `vite` ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)).
- **CLI lifecycle crashes**: hook-driven crashes at output time ([#22186](https://github.com/google-gemini/gemini-cli/issues/22186)) and high memory / GC exhaustion ([#28829](https://github.com/google-gemini/gemini-cli/issues/28829)).
- **Workspace hygiene**: model scatters temporary edit scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)), complicating clean commits.
- **Onboarding friction**: individual-account auth migration failures break sign-in flows ([#28745](https://github.com/google-gemini/gemini-cli/issues/28745)).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — Community Digest
**Date:** 2026-10-30 · **Source:** [github/copilot-cli](https://github.com/github/copilot-cli)

## Today's Highlights

No new releases or pull requests landed in the last 24 hours, but issue traffic stayed heavy (38 issues updated) with the community concentrated on **agent session stability** (37 GiB memory growth in long parallel-agent runs, 300s compaction timeouts) and **ACP/MCP authentication regressions** (OAuth pages re-opening every session, Entra token caches not persisting). A cluster of ACP-related reports — `session/prompt` blocking on async bash, missing `contextTier` config, unconditional session aborts — suggests the non-interactive/ACP surface is now the leading source of friction.

## Releases

No releases published in the last 24 hours.

## Hot Issues

1. **[#552 — Failed to list models: 403 unauthorized](https://github.com/github/copilot-cli/issues/552)** · OPEN · 31 comments · 👍4
   The longest-running open thread in the dataset (open since 2025-11-13, still updated today). Users in orgs that have "enabled Copilot CLI" still get `403 unauthorized` when listing models on 0.0.355+. High comment count signals a persistent entitlement/policy mapping problem rather than an isolated config error.

2. **[#4802 — PRU quota wiped out, likely tied to Assisted Permissions](https://github.com/github/copilot-cli/issues/4802)** · OPEN · 8 comments
   A Copilot Pro (legacy request-based PRU) subscriber reports quota exhaustion correlated with enabling assisted permissions. Billing-adjacent bugs draw outsized attention — the community needs clarity on whether agent-driven tool calls consume PRUs differently.

3. **[#5254 — 1.0.103: main process grows to 37 GiB during long parallel-agent session](https://github.com/github/copilot-cli/issues/5254)** · OPEN · 2 comments
   37.3 GiB physical footprint after ~6h19m with 133M malloc allocations and 10.7 GiB fragmentation on a 32 GiB Mac. The clearest signature yet of a memory-management defect in long-lived, multi-sub-agent sessions — a hard blocker for unattended/CI-style usage.

4. **[#4994 — Regression (1.0.88 → 1.0.89): MCP OAuth page opens on every ACP session](https://github.com/github/copilot-cli/issues/4994)** · OPEN · 5 comments · 👍8
   A valid, unexpired token in `~/.copilot/mcp-oauth-config` is ignored on each new `--acp` session, forcing repeated browser SSO. Highest-reacted open bug in today's set; directly breaks headless ACP automation. Companion closed issue [#5229](https://github.com/github/copilot-cli/issues/5229) covers the same root cause on token *expiry* (no refresh_token grant attempted).

5. **[#5292 — Automatic compaction repeatedly times out at 300s until hitting context limit](https://github.com/github/copilot-cli/issues/5292)** · OPEN (triage) · new
   Once the summarizer times out, the CLI has no recovery path before hard context-limit rejection. This turns a transient latency blip into a dead session — a reliability gap rather than a tuning issue.

6. **[#5293 — ACP: `session/prompt` response held until async bash shell exits](https://github.com/github/copilot-cli/issues/5293)** · OPEN (triage) · new
   Foreground work completes, but the response is withheld while an `async` bash tool (dev server, watcher) keeps running, so clients appear hung. Classic protocol-semantics mismatch that will bite every ACP client integrating long-running processes.

7. **[#5289 — `copilot --cloud` fails immediately during provisioning (internal server error)](https://github.com/github/copilot-cli/issues/5289)** · OPEN (triage) · 1.0.105
   Remote cloud sessions fail at environment provisioning with an opaque 500 before the agent host connects. Opaque server-side failures on a flagship workflow are the hardest class of bug for users to self-diagnose.

8. **[#4275 — ACP: expose `contextTier` as a session config option](https://github.com/github/copilot-cli/issues/4275)** · OPEN · 7 comments · 👍4
   The interactive `/model` picker can change context tier mid-session; ACP clients can only set it at spawn. A precise, well-scoped parity request from a third-party client vendor (GitKraken) — representative of ACP feature-parity demand.

9. **[#5275 — Support default-deny tool permissions with explicit allowlist in non-interactive mode](https://github.com/github/copilot-cli/issues/5275)** · OPEN · 1 comment
   `--deny-tool` always overriding `--allow-tool`, combined with default-permitted tools, makes strict sandboxing impossible in CI. Security-relevant for anyone running agentic CLI workloads unattended.

10. **[#4613 — CLOSED: MCP schemas eagerly injected, +354K startup tokens](https://github.com/github/copilot-cli/issues/4613)** · CLOSED · 3 comments
    A high-severity 1.0.80-line regression where the full ambient MCP catalog is injected into the first model request even for trivial prompts. Closed today — good news for token cost and latency on MCP-heavy setups, and worth verifying in your own configs.

## Key PR Progress

No pull requests were updated in the last 24 hours (0 items). Nothing to report on in-flight fixes, so all items above remain tracked through issues only.

## Hot Discussions

No discussion data was provided for this period.

## Feature Request Trends

- **ACP / non-interactive parity is the dominant theme.** Requests consistently ask for ACP to match interactive TUI capability: a `contextTier` session option ([#4275](https://github.com/github/copilot-cli/issues/4275)), an `ask_user` / `ask_question` extension method ([#2109](https://github.com/github/copilot-cli/issues/2109)), and non-aborting `session/prompt` semantics for background sub-agents ([#4555](https://github.com/github/copilot-cli/issues/4555)).
- **Policy enforcement for automation.** Strict default-deny tool allowlists ([#5275](https://github.com/github/copilot-cli/issues/5275)) — developers want provable sandboxes, not layered overrides.
- **Developer-environment fidelity.** Inheriting PowerShell environment variables ([#5281](https://github.com/github/copilot-cli/issues/5281)) and avoiding absolute `cache_path` in plugin config for Docker/multi-`$HOME` setups ([#3864](https://github.com/github/copilot-cli/issues/3864)).
- **UI/UX configurability and readability.** Remappable `shift+enter` submit/newline behavior ([#5291](https://github.com/github/copilot-cli/issues/5291)), Markdown output from `/copy` instead of HTML ([#5288](https://github.com/github/copilot-cli/issues/5288)), and restoring the September color theme's contrast ([#5056](https://github.com/github/copilot-cli/issues/5056)).
- **Documented-but-missing flags.** `--fusion-tier` is described in the model-selection docs but absent from the CLI ([#5290](https://github.com/github/copilot-cli/issues/5290)).

## Developer Pain Points

- **MCP authentication is the single biggest source of churn.** OAuth tokens are re-prompted or ignored ([#4994](https://github.com/github/copilot-cli/issues/4994), [#5229](https://github.com/github/copilot-cli/issues/5229)), Windows Entra broker caches stay 0 bytes between processes ([#5236](https://github.com/github/copilot-cli/issues/5236)), standard Entra `api://` scopes are rejected ([#5061](https://github.com/github/copilot-cli/issues/5061)), and token exchange fails outright for providers like Datadog ([#5058](https://github.com/github/copilot-cli/issues/5058)). Net effect: remote MCP servers are unreliable in headless and multi-process workflows.
- **Silent failures over loud errors.** Dropped MCP image results under BYOK ([#4600](https://github.com/github/copilot-cli/issues/4600)), plugins that silently never fire their `sessionStart` hooks ([#3864](https://github.com/github/copilot-cli/issues/3864)), and zero plugin agents activated in interactive mode while `--prompt` activates four ([#4592](https://github.com/github/copilot-cli/issues/4592)).
- **Resource leaks undermine long sessions.** 37 GiB RSS growth in parallel-agent runs ([#5254](https://github.com/github/copilot-cli/issues/5254)) and an idle `copilot-file-search` thread pinning a CPU core with unbounded diagnostic logging ([#4710](https://github.com/github/copilot-cli/issues/4710), now closed).
- **Regressions arrive per point release.** The 1.0.8x/1.0.9x line has produced repeated step-change regressions (OAuth behavior, eager MCP schema injection, `/copy` format), leaving users unsure whether to pin versions or upgrade.
- **Platform-specific breakage.** Windows clipboard writes fail while the CLI runs ([#3981](https://github.com/github/copilot-cli/issues/3981)), Windows shell runner ignores PowerShell env vars ([#5281](https://github.com/github/copilot-cli/issues/5281)), and macOS keychain ACL/partition mismatches cause prompts on every launch ([#4273](https://github.com/github/copilot-cli/issues/4273)).
- **Slow, aging threads.** [#552](https://github.com/github/copilot-cli/issues/552) (403 model listing) has been open for nearly a year with 31 comments — organizational entitlement problems remain under-explained in docs.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-10-30

---

## 1. Today's Highlights

OpenCode shipped two client releases in 24 hours: **v2.2.2** makes image generation available by default whenever a connected provider exposes image models, while **v2.2.1** introduced the long-running `/goal <objective>` session mode with pause/resume/clear controls. The community's attention remains dominated by the **free-tier quota mega-thread (#59387, 113 comments)**, where reset timers still appear broken across Desktop and Zen/Go. Alongside that, several high-value closed issues landed around storage growth, git snapshot behavior, and cost accounting — all long-standing scale complaints.

---

## 2. Releases

| Release | Notes |
|---|---|
| **v2.2.2** | **Image generation by default** — agents can call `experimental_image_generate` without extra configuration when a provider supports image models; preferred model is set via `experimental.image_generate.model` ([#60258](https://github.com/anomalyco/opencode/pull/60258)). |
| **v2.2.1** | **Goal-directed sessions** — `/goal <objective>` in TUI or Desktop keeps the session running until a reviewer confirms completion; `/goal pause`, `/goal resume`, `/goal clear` manage it ([#59437](https://github.com/anomalyco/opencode/pull/59437)). |
| **process-daemon v0.4.0 / v0.3.3** | `opencode-pty` 0.4.0 and 0.3.3 (protocol 7) published to npm as `@opencode/pty`. |

⚠️ Early regression report against v2.2.2: [Desktop side-panel tabs cannot be closed and New-tab launchers ignore clicks (#60381)](https://github.com/anomalyco/opencode/issues/60381). A fix for a related unparseable browser-pane chunk is already open in [#60388](https://github.com/anomalyco/opencode/pull/60388).

---

## 3. Hot Issues

1. **[#59387 — [zen] Free tier issues: read this first](https://github.com/anomalyco/opencode/issues/59387)** `OPEN` · 113 comments · 👍9
   The canonical triage thread for free-tier failures ("Free usage exceeded", reset timers that never fire, Desktop version gating). Volume alone makes it the community's central pressure point; it now acts as a de-facto bug tracker for quota logic.

2. **[#33356 — Unbounded growth of the `event` table (13 GB+ `opencode.db`)](https://github.com/anomalyco/opencode/issues/33356)** `CLOSED` · 47 comments · 👍12
   Long-lived instances grow without bound because `message.updated.*` snapshots are never pruned. Real operators reported 13 GB DBs filling 22 GB volumes to 99%. Closed now, but the retention/compaction design remains the biggest scale concern for self-hosted users.

3. **[#2999 — Provide means to disable Ctrl-C](https://github.com/anomalyco/opencode/issues/2999)** `CLOSED` · 42 comments · 👍30
   The highest-upvoted item in this window: Ctrl-C instantly kills the process on Windows Terminal/WezTerm instead of copying. Configurable keybindings/interrupt handling is a recurring accessibility complaint.

4. **[#3176 — Why is OpenCode massively abusing git?](https://github.com/anomalyco/opencode/issues/3176)** `CLOSED` · 23 comments · 👍12
   Session snapshots running `git add .` on a 45 GB / 54K-file tree. Highlights the cost of snapshot-based undo on large monorepos; strong agreement that snapshotting needs ignore rules or scoping.

5. **[#26602 — Desktop hits 5-minute Headers Timeout with slow local providers](https://github.com/anomalyco/opencode/issues/26602)** `CLOSED` · 24 comments · 👍2
   Requests abort at exactly 5 minutes even with `"timeout": false`, breaking local llama.cpp/Ollama-style setups. A key local-first usability gap.

6. **[#45417 — Session cost excludes subagent cost](https://github.com/anomalyco/opencode/issues/45417)** `CLOSED` · 10 comments · 👍17
   TUI sidebar, `opencode stats`, and `/export` under-report spend for fan-out workflows. High upvote ratio signals teams rely on these numbers for billing.

7. **[#51497 — perf(v2): every LLM call re-reads and re-decodes full session history](https://github.com/anomalyco/opencode/issues/51497)** `OPEN` · 5 comments · 👍1
   O(n) per step, O(n²) per session, with full-table SELECTs and per-row schema decoding. Accompanied by a deliberately non-mergeable demonstration PR ([#51499](https://github.com/anomalyco/opencode/pull/51499)) proposing an incremental cache + opt-in step GC.

8. **[#60381 — Desktop side-panel tabs can't be closed (2.2.2 regression)](https://github.com/anomalyco/opencode/issues/60381)** `OPEN` · 4 comments
   Fresh regression, persists after window reload; launcher rows except Terminal/Open file are dead. Timely because it directly follows the v2.2.2 upgrade path.

9. **[#60410 — Project `experimental` block hides every global `experimental` option](https://github.com/anomalyco/opencode/issues/60410)** `OPEN` · `reproduced` · 2 comments
   `Config.latest` returns whole blocks rather than merging fields, so any project-level experimental key silently nullifies the user's global settings. Config-semantics bugs are quietly destructive for power users.

10. **[#16626 — Add `session.stopping` plugin hook to re-enter the agent loop](https://github.com/anomalyco/opencode/issues/16626)** `CLOSED` · 15 comments · 👍12
    Plugins cannot prevent the loop from terminating. Closely related to the new `/goal` feature and a prerequisite for real stop-condition automation.

*Also notable:* [#18654 — no way to change/remove email in OpenCode Zen](https://github.com/anomalyco/opencode/issues/18654) (👍20), [#31554 — MCP servers spawn 2–4 duplicate processes](https://github.com/anomalyco/opencode/issues/31554), [#34877 — cost shows $0.00 for GLM/DeepSeek/Qwen via `@ai-sdk/openai-compatible`](https://github.com/anomalyco/opencode/issues/34877).

---

## 4. Key PR Progress

1. **[#60414 — feat(app): voice in the composer with first-call onboarding](https://github.com/anomalyco/opencode/pull/60414)** `OPEN`
   Promotes the experimental Desktop voice call from behind a Knobs toggle to the composer, with onboarding on first use. Follow-up to #58249/#58587/#58591.

2. **[#60388 — fix(desktop): keep the browser pane chunk parseable](https://github.com/anomalyco/opencode/pull/60388)** `OPEN`
   Fixes an unparseable browser-pane chunk produced by the desktop main build in 2.2.2 (closes #60369, #60386). Direct remediation for the current release.

3. **[#60039 — feat(mcp): per-server TLS trust for remote MCP servers](https://github.com/anomalyco/opencode/pull/60039)** `OPEN`
   Adds an optional `tls` block so a self-signed or private-CA MCP server can be trusted individually instead of globally disabling verification.

4. **[#60041 — feat(mcp): trust private-network server certificates on first use](https://github.com/anomalyco/opencode/pull/60041)** `OPEN`
   Stacked on #60039: a first-use trust flow, mirroring SSH-style TOFU instead of a manual fingerprint copy.

5. **[#59954 — fix(core): apply webfetch permissions to Code Mode `fetch`](https://github.com/anomalyco/opencode/pull/59954)** `OPEN`
   Security-relevant: a URL denied for `webfetch` could previously still be fetched from inside a Code Mode program, and "ask" rules never prompted.

6. **[#60406 — fix(client): store session forms created without a location](https://github.com/anomalyco/opencode/pull/60406)** `OPEN`
   Closes #59759, where the question tool published `form.created` without `location`, leaving a pending question on the server that the TUI never rendered.

7. **[#59803 — fix(core): price Copilot long-context tiers and cache writes](https://github.com/anomalyco/opencode/pull/59803)** `OPEN`
   Corrects severe undercounting on long GitHub Copilot sessions (closes #52089, #59671) — relevant for anyone using cost data for budgeting.

8. **[#58066 — fix(server): allow `credential.update` to rotate the stored value](https://github.com/anomalyco/opencode/pull/58066)** `OPEN`
   `PATCH /api/credential/:id` only accepted `label`, so rotating an expired API key silently kept the old secret. Important for automated key rotation.

9. **[#60416 — fix(tui): keep HTML error pages out of the retry notice](https://github.com/anomalyco/opencode/pull/60416)** `OPEN`
   Truncates/summarizes provider HTML error bodies (e.g. nginx 503) instead of dumping whole documents into multi-line retry banners.

10. **[#60044 — feat(ai): type OpenRouter options on the Responses route](https://github.com/anomalyco/opencode/pull/60044)** `OPEN`
    Surfaces OpenRouter's `provider` routing, `models` fallbacks, and `plugins` in the typed Responses options instead of letting them pass through untyped/unschematized.

*Also in flight:* [#60109](https://github.com/anomalyco/opencode/pull/60109) (surface permission feedback inside Code Mode), [#60407](https://github.com/anomalyco/opencode/pull/60407) and [#60390](https://github.com/anomalyco/opencode/pull/60390) (unifying `tool()` across Promise/Effect clients and fixing typed-tool variance), [#60380](https://github.com/anomalyco/opencode/pull/60380) (run dynamic provider npm fallback after external SDK hooks).

---

## 5. Hot Discussions

No discussion data was provided for this window; this section is omitted.

---

## 6. Feature Request Trends

- **Agent loop control & long-horizon autonomy.** `/goal` shipped in v2.2.1, but requests for a `session.stopping` hook and stop-condition plugins (#16626) show the community wants programmatic control, not just UI verbs.
- **Observability for parallel agents.** A dedicated agent activity sidebar (#27995) for monitoring `explore` / `librarian` / `oracle` background agents, instead of calling `background_output()` manually.
- **Plugin/extensibility surface area.** A generic cross-client "UI intent" channel (#6330) plus richer hook coverage — plugins currently can't influence UX across TUI/Desktop consistently.
- **Configurable input & accessibility.** Disabling/remapping Ctrl-C (#2999), RTL/Bidi rendering for Persian and Arabic (#16875).
- **Account & identity management.** Changing or removing email in OpenCode Zen (#18654, 👍20) and cleaner handling of duplicate identities after GitHub email changes.
- **Enterprise/private-network connectivity.** Per-server TLS trust for remote MCP servers (#60039, #60041, tracked under #40111), driven by self-signed certs in corporate environments.
- **Cost transparency.** Subagent costs in session totals (#45417), long-context/cache pricing for Copilot (#59803), and non-zero cost reporting for OpenAI-compatible Chinese providers (#34877).

---

## 7. Developer Pain Points

1. **Free-tier quota logic is the #1 source of friction.** Reset timers that add 24 hours instead of firing (#60062), limits that never reset across days (#59178, #60297), and repeated "one message and blocked again" reports (#60064) have converged into a 113-comment mega-thread (#59387). The absence of a documented window/counter amplifies frustration.
2. **Scale and storage on long-lived instances.** Unbounded `event`-table growth to 13 GB (#33356) and O(n²) history re-reads per session (#51497) suggest the persistence layer hasn't kept pace with multi-hour, subagent-heavy workflows.
3. **Release-to-release Desktop regressions.** v2.2.2 broke side-panel interaction (#60381) and produced an unparseable browser-pane chunk (#60388). Users on fast upgrade cadences feel this immediately.
4. **Cost accounting is not trustworthy enough for teams.** Missing subagent spend (#45417), $0.00 for openai-compatible Chinese providers (#34877), and Copilot undercounts (#59803) undermine `/export` and `opencode stats` as billing evidence.
5. **Timeouts and networking assumptions break local-first use.** A hard 5-minute headers timeout that ignores `"timeout": false` (#26602), plus IP-sensitive `ECONNRESET` against the built-in Zen provider (#60422).
6. **Resource leaks in the process layer.** MCP servers spawning 2–4 duplicate processes each, exhausting `TasksMax` and producing `EAGAIN` (#31554).
7. **Config semantics surprise power users.** A project-level `experimental` block silently shadowing all global experimental options (#60410) — a class of merge bugs that is hard to diagnose from the TUI.
8. **Permission boundaries leak across execution modes.** Code Mode `fetch` bypassing `webfetch` permission rules (#59954) and rejection feedback being swallowed (#60109) weaken the trust model users expect from explicit allow/ask/deny config.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-30

## 1. Today's Highlights

No new release landed in the last 24 hours, but the repository saw a heavy maintenance wave: 50 issues and 23 PRs were updated, with a large batch of fixes closed around Windows path handling, terminal input sequences, session-file privacy, and project-trust semantics. Performance and determinism were the other dominant themes — transcript virtualization for the TUI (#11598), prompt-cache stability via stable tool ordering (#11529), and per-request Bedrock client creation (#11527) all drew attention. Provider-compatibility gaps in `pi-ai` remain the most structurally important open problem (#9508), while the newly filed RPC shutdown bug (#11599) was triaged and closed the same day.

---

## 2. Releases

None in the last 24 hours. Omit.

---

## 3. Hot Issues

1. **[#9656](https://github.com/earendil-works/pi/issues/9656) — Mouse wheel scrolls prompt history instead of transcript in fullscreen on Windows + Zellij** `OPEN`
   The most-discussed issue in the window (12 comments, 5 👍). Input routing breaks only in the Windows → Alacritty → Zellij → Pi chain, while direct terminals and Linux+tmux work. It signals that the fullscreen input model still lacks a correct multiplexer-aware path, and it has stayed open since mid-September despite steady engagement.

2. **[#9508](https://github.com/earendil-works/pi/issues/9508) — `pi-ai` sends unsupported OpenAI-specific request fields/roles/auth to compatible providers** `OPEN`
   9 comments and still unresolved. Providers behind the "OpenAI-compatible" label reject OpenAI-only fields, roles, and auth headers with 400/422, so otherwise-valid `models.json` configurations fail. This is the clearest ecosystem-level blocker for third-party model support.

3. **[#11157](https://github.com/earendil-works/pi/issues/11157) — Extensions that `sendMessage` at `session_start` turn every session into a legacy transcript** `OPEN` `prio:p0` `inprogress`
   Because `getInitialSystemMessage` reads `messages[0]`, extensions like pi-memory and pi-session-search push custom messages ahead of the system prompt, causing Anthropic requests to lose the `system` field. A high-severity correctness issue affecting the extension ecosystem, now marked in progress.

4. **[#10982](https://github.com/earendil-works/pi/issues/10982) — `pi.sendMessage(triggerTurn)` starts a run while manual compaction is in progress** `OPEN`
   A concurrency bug: the triggered run executes against uncompacted context and interleaves with compaction output. Low comment volume but high impact for anyone relying on programmatic turns during `/compact`.

5. **[#11529](https://github.com/earendil-works/pi/issues/11529) — Tool order depends on registration order, so the same tools can bust the prompt cache** `OPEN`
   `getCurrentTools()` returns insertion order, so a tool removed and re-added (MCP reconnect, `tool_search` loads, extension reload) shifts to the end of the Map. Since tools sit in the cached prefix, this silently invalidates prompt caches and inflates cost.

6. **[#11527](https://github.com/earendil-works/pi/issues/11527) — Bedrock: a new `BedrockRuntimeClient` is created for every request** `OPEN`
   `stream()` instantiates a fresh client per call, discarding the credential provider and HTTP/2 session. With SSO profiles this forces a `GetRoleCredentials` round trip on every model call — a straightforward but meaningful latency and throughput regression.

7. **[#10770](https://github.com/earendil-works/pi/issues/10770) — Default permissions of `.pi/agent/sessions` should be `0700`** `CLOSED`
   Session directories were created under the process umask, leaving transcripts world-readable on typical `0022` systems. 6 comments and a prompt fix — a privacy-hardening win with clear user demand.

8. **[#10947](https://github.com/earendil-works/pi/issues/10947) — `isProjectTrusted()` ignores saved Do-not-trust decisions** `CLOSED`
   A saved `false` in `trust.json` was ignored for projects without trust-requiring resources, because the auto-trust check ran before the trust store was read. Security-relevant semantics bug, now resolved.

9. **[#11570](https://github.com/earendil-works/pi/issues/11570) — V8 OOM (SIGABRT) in long sessions with giant `ConsString` flattening** `CLOSED` `no-action`
   Five production crash reports from 2+ day sessions (~8MB session files) sharing stack signatures in a `flatMap` string allocation. Closed without action, which leaves long-session memory behavior as an open operational risk.

10. **[#10187](https://github.com/earendil-works/pi/issues/10187) — Move `deviceId` out of the global settings** `CLOSED` `no-action`
    3 👍 and 5 comments from users managing agent settings as shared dotfiles. A device-specific identifier in a committed config file is a real workflow conflict and a recurring class of request.

---

## 4. Key PR Progress

1. **[#11598](https://github.com/earendil-works/pi/pull/11598) — perf(tui): virtualize the transcript and resize the fullscreen layout lazily** `CLOSED`
   Directly addresses the O(transcript) per-frame cost: every message component previously stayed alive for the whole session, making scrolling, typing, and the working indicator scale with history size. The most consequential performance change in this batch.

2. **[#6881](https://github.com/earendil-works/pi/pull/6881) — feat(ai): use provider-reported cost when responses include it** `CLOSED`
   Uses billed cost for `usage.cost.total` when available (including `cost_details.upstream_inference_cost` on BYOK), falling back to `calculateCost`. Improves cost accuracy for gateway and upstream-billed setups.

3. **[#11594](https://github.com/earendil-works/pi/pull/11594) — fix(ai): handle reasoning models on Alibaba endpoints** `CLOSED`
   Avoids 400 errors caused by the `developer` role on Alibaba reasoning endpoints — another concrete instance of the provider-heterogeneity problem tracked in #9508.

4. **[#11591](https://github.com/earendil-works/pi/pull/11591) — fix(tui): drop space after styled word at wrap boundary, reset strikethrough at line end** `CLOSED`
   Fixes #10925. ANSI close codes attached to the following space caused an extra leading space on wrapped lines; the tokenizer and `wrapSingleLine` are corrected.

5. **[#11588](https://github.com/earendil-works/pi/pull/11588) — fix(coding-agent): compare paths case-insensitively on Windows** `CLOSED`
   Fixes #10488. `realpathSync` preserves input casing, so `c:\Users` vs `C:\Users` produced false skill collisions; path comparisons now normalize casing on Windows.

6. **[#11587](https://github.com/earendil-works/pi/pull/11587) — fix(coding-agent): create session directories as `0700` and session files as `0600`** `CLOSED`
   The implementation for #10770. Closes the transcript-readability gap that existed under a standard `0022` umask.

7. **[#11590](https://github.com/earendil-works/pi/pull/11590) — fix(coding-agent): refresh unpinned temporary npm extensions** `CLOSED`
   Fixes #11375. `pi -e npm:<pkg>` previously reused the first download forever; unversioned specs and dist-tags now re-resolve when a newer version exists.

8. **[#11586](https://github.com/earendil-works/pi/pull/11586) / [#11585](https://github.com/earendil-works/pi/pull/11585) — fix(tui): treat `ESC`+`Return` with native Shift held as `Shift+Enter`** `CLOSED`
   Apple Terminal sends `\x1b\r` for Shift+Enter; without the kitty protocol this parsed as `alt+enter` and queued a follow-up instead of inserting a newline. Both the native-Shift fallback and the Apple Terminal path are covered.

9. **[#11577](https://github.com/earendil-works/pi/pull/11577) — fix(coding-agent): pick the smaller of PNG and JPEG when resizing images** `CLOSED`
   The candidate list always tried PNG first, so photo-like JPEGs were sent as PNGs many times larger even when a smaller JPEG was available under `maxBytes`.

10. **[#11580](https://github.com/earendil-works/pi/pull/11580) — Add Arabic RTL rendering to the TUI (bidirectional text, Windows Terminal)** `CLOSED`
    Implements UAX #9 bidi reordering for terminals that don't apply it (notably Windows Terminal), fixing mirrored Arabic text. Notable as an internationalization step for the TUI layer.

Also worth tracking: **[#11562](https://github.com/earendil-works/pi/pull/11562)** (OPEN) starts new sessions in the cwd's default directory instead of inheriting a foreign project's folder, and **[#11572](https://github.com/earendil-works/pi/pull/11572)** stops retrying Zhipu/Z.ai usage-limit errors (code 1308, HTTP 429).

---

## 5. Hot Discussions

**Show and tell**
- **[#11503](https://github.com/earendil-works/pi/discussions/11503) — pi-rs: a Rust port of the headless core, behavioral parity verified atom by atom** (1 comment, 1 👍)
  A community Rust reimplementation of pi 0.74.2's headless core — agent loop, 7 built-in tools, session JSONL, CLI surface, and system prompt — with byte-level parity checks against the original. Includes a DeepSeek API compatibility finding. Valuable both as an independent parity harness and as validation that the headless core is cleanly separable from the TUI.

**Q&A**
- **[#11574](https://github.com/earendil-works/pi/discussions/11574) — How to upload an npm package to the Pi package source?** (0 comments)
  A package author has built an extension but can't find documented guidance on publishing it. Unanswered, and it points at a gap in the extension distribution story rather than a one-off question.

---

## 6. Feature Request Trends

- **Provider abstraction correctness.** The largest cluster by far: OpenAI-specific fields leaking to compatible providers (#9508), Alibaba `developer` role handling (#11594), Google thought-signature merging/dropping (#11538), Z.ai forced-thinking behavior (#8706), Zhipu retry classification (#11572), and Bedrock client reuse (#11527). Users are running Pi against a long tail of providers and want the request layer to be conservative and provider-aware.
- **Prompt-cache and cost determinism.** Stable tool ordering (#11529), provider-reported cost (#6881), and output-budget defaults that trigger 402s on OpenRouter (#11517) all point to a demand for predictable spend.
- **Security and privacy defaults.** `0700`/`0600` session artifacts (#10770, #11587), correct trust-store semantics (#10947), and separating device-local state from shareable config (#10187).
- **Long-session performance and memory.** Transcript virtualization (#11598), `getSessionName()` scanning every entry per render (#11567), and V8 OOM in multi-day sessions (#11570).
- **Terminal and platform compatibility.** Windows drive-letter casing (#10488), Zellij mouse routing (#9656), Apple Terminal Shift+Enter (#11559/#11586), split mouse-report tails (#11172), Orca Kitty-image detection (#9329), and RTL text (#11580).
- **MCP and extension ergonomics.** Namespace-scoped `tool_search` queries (#11558), recoverable unsupported image formats in tool results (#11552), passing model-call origin to `HarnessOptions.models` (#11596), and lifecycle-safe extension calls (#11157, #10982, #11599).

---

## 7. Developer Pain Points

- **Windows remains the roughest platform.** Case-sensitive path comparisons, drive-letter inconsistency between cwd and home, and terminal-specific escape sequences generate a steady stream of bugs (#10488, #9656, #11586). Several were fixed this window, but the pattern is recurring rather than one-off.
- **"OpenAI-compatible" is not a contract.** Developers repeatedly hit 400/422 errors because Pi emits OpenAI-only fields, roles, or auth headers — or because model metadata (context, output capacity, thinking format) is misapplied. #9508 has been open since September with 9 comments and no resolution.
- **Sessions degrade as they grow.** Typing latency at ~400k entries, O(transcript) rendering, and multi-day sessions aborting with V8 OOM are three symptoms of the same scaling problem. Two of the three were closed without action, leaving operators to work around it.
- **Silent correctness failures are the worst class of bug.** Empty `stdout` with exit 0 in `pi -p` (#11568), an idle RPC process that never exits after `ctx.shutdown()` (#11599), and extensions silently converting every session to a legacy transcript (#11157) all fail quietly rather than loudly.
- **Configuration portability.** Users who version-control their agent settings cannot safely do so while `deviceId` lives in global settings (#10187), and trust decisions stored on disk were previously not honored (#10947).
- **Extension lifecycle races.** `session_start` message injection, `triggerTurn` during compaction, and async shutdown are all cases where extensions and core have unclear ordering guarantees — a gap that will grow as the extension ecosystem does.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-30

*Source: [github.com/QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)*

---

## 1. Today's Highlights

Qwen Code shipped **v0.26.4** across all three tracks — CLI/nightly, TypeScript SDK (v0.1.26) and Desktop (v0.26.4) — with a small, low-risk change list focused on generated-UI theming and Managed Agent SQL history bounds. The bigger story is behind the scenes: CI and release infrastructure dominate the issue tracker, with chronic main-branch failures (`#14724`, `#12714`) and a wave of runner-capacity/partitioning work landing in PRs. Meanwhile, core reliability fixes (SDK stream-json message identity, `/loop` cancellation, config persistence) are accumulating as the community stress-tests the daemon, SDK and Web Shell surfaces.

---

## 2. Releases

**v0.26.4** — stable CLI release, with no documented breaking changes.

- **feat(generated-ui): follow the host theme in generated views** — [PR #15798](https://github.com/QwenLM/qwen-code/pull/15798). Generated UI now inherits the host application's theme instead of forcing its own.
- **feat(managed): add bounded SQL history queries** (nightly channel, PR [#15839](https://github.com/QwenLM/qwen-code/pull/15839) by @doudouOUC) — caps history query cost in the Managed Agent service.
- **feat(generated-ui): keep chatting with a ca…** — incremental continuation work in the same generated-UI area.

**sdk-typescript-v0.1.26** — bundles CLI **0.26.4** (built from the same source branch/ref as the SDK). Note: the generated release body still lists both `0.26.4` and `0.26.3` in duplicated "Bundled CLI Version" blocks, and the bundled-CLI section appears malformed in the published notes — worth a docs cleanup.

**desktop-v0.26.4** — includes `fix(config): report controls that were silently dropped` ([#15794](https://github.com/QwenLM/qwen-code/pull/15794) by @yiliang114) and docs for the shared-session flow.

**v0.26.4-nightly.20261029.70be140e14** — nightly release cut from `release/v0.26.4-nightly.20261029.70be140e14`.

---

## 3. Hot Issues

1. **[#14724](https://github.com/QwenLM/qwen-code/issues/14724) — Main CI failed: Managed Agent gate, no test identified** *(OPEN, 95 comments, updated daily since 2026-10-19)*
   A bot-filed, per-commit CI failure tracker that has become the single noisiest thread in the repo. It signals a real structural problem: the Managed Agent gate can fail without attributing a test, so nothing gets actioned. With 95 comments and `autofix/skip`, it is effectively a standing alert nobody can close.

2. **[#15052](https://github.com/QwenLM/qwen-code/issues/15052) — CI: preserve independent main E2E validation when pending runs are replaced** *(OPEN, P2, 14 comments, ready-for-human)*
   Ref-scoped concurrency replaces older pending main E2E runs, so intermediate SHAs never get an independent validation result. Maintainer @yiliang114 argues for per-SHA validation and failure attribution — this is the root cause behind many "provisional" CI reports.

3. **[#14215](https://github.com/QwenLM/qwen-code/issues/14215) — Context immutability for self-hosted models** *(OPEN, P2, 7 comments, 1 👍, need-discussion)*
   The most substantive community feature thread: users running local models with slow prompt processing want memories, dreams, sessions and suggestions to stop mutating context, since each rewrite costs minutes of prefill. Tagged `roadmap/context-performance` and `scope/caching` — a genuine architectural tension between feature richness and local inference economics.

4. **[#15461](https://github.com/QwenLM/qwen-code/issues/15461) — Release: reserve ECS runner slots and activate the build/publish split** *(OPEN, P2, 8 comments, ready-for-human)*
   `publish` is the last release job still competing for GitHub-hosted capacity; the ECS release host was never provisioned. This is release-engineering debt directly affecting how fast fixes reach users.

5. **[#15861](https://github.com/QwenLM/qwen-code/issues/15861) — ci(smoke): web-shell host-directory-picker smoke times out, PR-independent** *(OPEN, **P1**, 3 comments)*
   A deterministic smoke failure on unrelated PRs, meaning every contribution pays a re-attribution round. P1 on a test-infra issue reflects how much CI trust has eroded.

6. **[#15855](https://github.com/QwenLM/qwen-code/issues/15855) — `contextWindowSize` gets automatically removed** *(OPEN, **P1**, need-information, 3 comments)*
   A user's explicit 128k limit in an OpenAI-compatible provider block is silently stripped. High severity because it breaks local-model workflows with no error — silent config rewriting is a recurring trust breaker (see also #15794 and #15829).

7. **[#15894](https://github.com/QwenLM/qwen-code/issues/15894) — `/loop` stop sent as a user message doesn't cancel pending `loop_wakeup`** *(OPEN, P2, daemon, background-automation)*
   An 80-minute autonomous `/loop` tick ignored an explicit stop and resumed. This is a control/safety issue for autonomous agents — users expect stop to mean stop.

8. **[#15875](https://github.com/QwenLM/qwen-code/issues/15875) — SDK stream-json: text snapshot changes `message.id` after thinking** *(OPEN, P2, scope/sdk, scope/non-interactive)*
   With `--include-partial-messages`, the final text snapshot's `message.id` differs from the originating `message_start`. Breaks any SDK consumer that correlates streaming fragments by ID — a core protocol-fidelity bug for integrations.

9. **[#15327](https://github.com/QwenLM/qwen-code/issues/15327) — tracking(core): run every surface on one agent loop behind `experimental.unifiedAgentLoop`** *(OPEN, P1, ready-for-human)*
   The architectural north star: one agent loop in `packages/core` shared by CLI, daemon, Web Shell and Desktop. Today each surface has its own loop, which explains the long tail of surface-specific bugs.

10. **[#13649](https://github.com/QwenLM/qwen-code/issues/13649) — feat(a2a): contextId-less A2A messages each create an indistinguishable, unbounded chat session** *(OPEN, P2, need-discussion)*
    Since #13583, the default A2A request shape (no `contextId`) creates one session per message, flooding the owner's Web Shell session list with no way to distinguish or bound them. A multi-agent design gap with real UX fallout.

*Also notable:* [#12714](https://github.com/QwenLM/qwen-code/issues/12714) (13 comments, CI `src/llm.test.tsx` routing failures), [#15739](https://github.com/QwenLM/qwen-code/issues/15739) (closed after 9 comments), [#1206](https://github.com/QwenLM/qwen-code/issues/1206) (2 👍, dynamic multi-model support for OpenAI-compatible APIs — long-running and still open since 2025-12), [#15877](https://github.com/QwenLM/qwen-code/issues/15877) (remote daemon listener/origin setup unclear), [#15869](https://github.com/QwenLM/qwen-code/issues/15869) (PRs #14348/#15450 stalled 3 days — community escalation on review latency).

---

## 4. Key PR Progress

1. **[#15871](https://github.com/QwenLM/qwen-code/pull/15871) — refactor(managed-agent): split server into Maven modules** (@doudouOUC)
   Splits the Managed Agent Server into six production Maven modules plus one unpublished verification module, with framework-neutral contracts at the boundaries. The largest structural change in flight for the managed surface.

2. **[#11854](https://github.com/QwenLM/qwen-code/pull/11854) — feat: add hybrid code mode** (@DragonnZhang, `autofix/takeover`)
   Adds a Codex-aligned `tools.mode` enum (`direct` / `code_mode` / `code_mode_only`) exposing an isolated `exec` JavaScript tool while keeping ordinary tools callable. This is the most significant capability addition in the queue for tool-using agents.

3. **[#15466](https://github.com/QwenLM/qwen-code/pull/15466) — feat(serve): add user-scope Skill activation routes** (@callmeYe)
   New daemon routes `PUT /skills/:name/activation` and batch `PUT /skills/activation` (up to 100 names) for the user settings scope. Unblocks Skill management from the Web Shell without a workspace selector.

4. **[#15863](https://github.com/QwenLM/qwen-code/pull/15863) — fix(cli): separate instruction files from auto memory in context usage** (@callmeYe)
   Project/user/extension/language instructions now report under *Instruction files*, while auto-memory policy and catalog snapshots report under *Auto memory*. Directly improves context-budget transparency — relevant to the #14215 debate.

5. **[#15854](https://github.com/QwenLM/qwen-code/pull/15854) — perf(release): reuse verified native audio prebuilds across releases** (@yiliang114)
   Reuses audio prebuilds from a successful main release when build-input fingerprints match; validated cache hits skip the five-target build matrix, and any lookup/download/validation failure falls back to a full rebuild. Clear release-latency win with a safe fallback path.

6. **[#15888](https://github.com/QwenLM/qwen-code/pull/15888) — fix(ci): partition the complete macOS test suite into bounded jobs** (@yiliang114)
   Splits macOS CI into seven jobs (three CLI shards, two core shards, remaining workspace suites, standalone/runtime scripts) with the original check retained as an aggregate. Follow-up findings tracked in [#15895](https://github.com/QwenLM/qwen-code/issues/15895).

7. **[#15885](https://github.com/QwenLM/qwen-code/pull/15885) — fix(ci): route the E2E build job through the pool-first runner expression** (@yiliang114)
   Moves the E2E shared build job onto the same self-hosted `ecs-qwen` pool-first routing every other job uses. Together with #15854 and #15461, this is the runner-capacity story of the week.

8. **[#12799](https://github.com/QwenLM/qwen-code/pull/12799) — fix(edit): keep the line endings an edit did not touch** (@feiiiiii5)
   Preserves original bytes outside local edit spans and selects inserted line endings per span, while whole-file replacements keep prior CRLF behavior. A long-running correctness fix with outsized impact on Windows/dirty-diff complaints.

9. **[#15829](https://github.com/QwenLM/qwen-code/pull/15829) — fix(vscode): keep sibling model settings when writing provider config** (@2SinceJuly)
   Configuring a provider from the VS Code companion previously rewrote the entire `settings.model` object with only `name`, deleting sibling keys. Now writes `name`/`baseUrl` key-by-key — same class of silent-config-loss bug as #15855 and #15794.

10. **[#15851](https://github.com/QwenLM/qwen-code/pull/15851) — fix(cli): report pre-send refusals instead of replaying answers** (@2SinceJuly)
    When a turn is refused before sending (session token limit or a `UserPromptSubmit` hook block), the shared result builder fell back to the *previous* turn's assistant message. Fixes a serious correctness bug for persistent stream-json sessions.

*Also worth tracking:* [#15883](https://github.com/QwenLM/qwen-code/pull/15883) and [#15882](https://github.com/QwenLM/qwen-code/pull/15882) (recovery-fixture transcript coverage; Managed JDBC selection in Hosted fault fixtures), [#15803-series](https://github.com/QwenLM/qwen-code/pull/15286) Java cache hygiene fixes ([#15286](https://github.com/QwenLM/qwen-code/pull/15286), [#15274](https://github.com/QwenLM/qwen-code/pull/15274)), [#15060](https://github.com/QwenLM/qwen-code/pull/15060) (shared session view reset state), [#13026](https://github.com/QwenLM/qwen-code/pull/13026) (report speculation copy failures), [#13035](https://github.com/QwenLM/qwen-code/pull/13035) (tolerant managed-session metadata parsing), [#13067](https://github.com/QwenLM/qwen-code/pull/13067) (Ctrl+named-key bug), [#13080](https://github.com/QwenLM/qwen-code/pull/13080) (`/stats` scrolling on short terminals), [#14653](https://github.com/QwenLM/qwen-code/pull/14653) (`/mcp` selection by identity).

---

## 5. Hot Discussions

*No Discussion data was provided in this dataset — section omitted.*

---

## 6. Feature Request Trends

- **Context & memory economics for self-hosted models** (`roadmap/context-performance`, `scope/memory`, `scope/caching`) — the strongest single direction. #14215 argues that memories/dreams/suggestions/sessions should become opt-in or immutable when prompt processing is slow, and #15863's instruction-vs-auto-memory split shows the team moving toward better context transparency.
- **One agent loop / unified session management** (`roadmap/session-management`) — #15327 proposes a single `packages/core` agent loop behind `experimental.unifiedAgentLoop`, explicitly to eliminate surface-specific divergence across CLI, daemon, Web Shell and Desktop.
- **Multi-agent and A2A hardening** (`roadmap/multi-agent`) — #13649 (unbounded session creation), #13645 (shared mention grammar, typed executing-status set) and #13583 follow-ups show demand for predictable multi-agent semantics rather than more features.
- **Dynamic multi-model support for OpenAI-compatible endpoints** — #1206 (open since 2025-12, 2 👍) still asks for fetching and switching models dynamically instead of hardcoding one.
- **Web Shell / Desktop UX parity** — keyboard shortcuts for Session Overview and Split View (#13175, closed), Russian/arbitrary-locale i18n (#13391, closed), no-workspace session visibility (#11878, closed), sidebar category consistency (#15867), macOS daemon `PATH` extension (#12690). The closure rate suggests active triage here.
- **CI/release infrastructure as a product surface** — ECS runner reservation (#15461), overnight macOS validation policy (#15783), pool-first runner routing. These are now first-class feature requests, not chores.
- **Remote/daemon access ergonomics** — #15877 (listener + origin setup for remote `qwen serve`) and #12059 (remote-webview failure modes: forwarded-port Host gate, IPv6 CSP, Remote-SSH/WSL) point at growing remote-development usage.

---

## 7. Developer Pain Points

- **Chronic, unattributable CI failures.** #14724 (95 comments), #12714 (13), #14796, #15712, #15281 and #15739 all trace back to the same pattern: a run fails before any test result is reported, so the tracker can only file per-commit. #15052 diagnoses the concurrency root cause. Contributors are losing time to re-attribution on *unrelated* PRs (#15861 is explicitly "PR-independent").
- **macOS CI cost and duration.** #15783 requests moving routine macOS/Darwin validation to overnight Asia/Shanghai runs; #15888 partitions the suite into seven bounded jobs. Both exist because macOS lanes are the slowest, flakiest gate.
- **Silent configuration loss.** Three independent reports in one window: `contextWindowSize` stripped (#15855), VS Code provider writes clobbering sibling `settings.model` keys (#15829), and Desktop dropping controls without notice (#15794, now fixed). The pattern — mutate a shared config object instead of patching keys — is a systemic hazard for user trust.
- **Streaming/SDK protocol fidelity.** #15875 (message ID changes mid-stream) and #15851 (replaying a previous turn's answer after a pre-send refusal) both break consumers that treat the stream as authoritative. Machine-readable output is now a contract, and it's leaking.
- **Autonomy controls that don't take effect.** #15894: an explicit stop does not cancel a pending `loop_wakeup`, so a self-paced `/loop` re-arms itself. For users running autonomous managers, this is the difference between a tool and an unbounded process.
- **Edit correctness on real filesystems.** Long-lived PRs #12799 (line endings), #13026 (speculation copy failures), #13035 (torn transcript lines) and #13067 (Ctrl+named-key input) show a steady undercurrent of byte-level and input-level correctness bugs that take weeks to land because they sit behind a noisy CI gate.
- **Local/self-hosted model ergonomics.** #14215 (slow prompt processing), #15855 (context window ignored), #15877 (remote daemon setup) — the self-hosted segment consistently hits friction where cloud users don't, and the issues carry the highest per-user impact.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*