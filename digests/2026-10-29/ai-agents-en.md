# OpenClaw Ecosystem Digest 2026-10-29

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-29 14:08 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-29

## 1. Today's Overview

OpenClaw saw very high triage throughput on 2026-10-29: 500 issues and 500 PRs were updated in the last 24 hours, with 224 PRs merged/closed against 276 still open, and 142 issues closed against 358 open/active. No new release was published, and the most recent stable validation reference is 2026.10.9 (Issue #187490), with extended-stable 2026.9.36 backports still active. Activity is concentrated in release engineering, CI/test stabilization, and a batch of Web UI resource-efficiency fixes, while the issue queue is dominated by gateway startup, crash-recovery, and session-state defects. The headline health concern is severity concentration: at least seven P0 issues appear in the top-50-by-comments sample alone, several tagged `clawsweeper:no-new-fix-pr`, indicating a large stabilization backlog without identified fixes. Overall assessment: **high-velocity maintenance and triage, but elevated stability debt and zero release output today.**

## 2. Releases

No new releases were published in the last 24 hours, and the "Latest Releases" feed is empty.

Context from issue/PR metadata (not a release announcement):
- Stable line appears to be **2026.10.9**, referenced by release-validation flake tracking in [#187490](https://github.com/openclaw/openclaw/issues/187490).
- **2026.10.8** retrospective blockers are being addressed in [#187249](https://github.com/openclaw/openclaw/pull/187249) (release-validation tooling).
- Extended-stable **2026.9.36** is receiving targeted backports: [#187252](https://github.com/openclaw/openclaw/pull/187252) (stateless reasoning replay guard) and [#187701](https://github.com/openclaw/openclaw/pull/187701) (live-test waiver keying).

No breaking changes or migration notes to report today.

## 3. Project Progress

224 PRs were merged/closed in the last 24h. Notable items visible in the top-30 sample:

**Fixes landed (closed PRs):**
- [#187646](https://github.com/openclaw/openclaw/pull/187646) — native Codex helper processes are now settled on Stop, preventing orphaned child commands after session abort (fixes #187598).
- [#187687](https://github.com/openclaw/openclaw/pull/187687) — Web UI stops polling storage maintenance progress in hidden tabs; refreshes once on return.
- [#187701](https://github.com/openclaw/openclaw/pull/187701) — release validation waivers keyed correctly for the extended-stable line.

**Notable in-flight work (open PRs):**
- **MCP capability gap:** [#187699](https://github.com/openclaw/openclaw/pull/187699) makes `tools.invoke` / HTTP `/tools/invoke` work for configured MCP tools (currently returns `not_found` despite appearing in `tools.effective`). This closes a visible feature/implementation mismatch.
- **Web UI efficiency batch (vincentkoc):** [#187691](https://github.com/openclaw/openclaw/pull/187691) (release Markdown workers in hidden tabs), [#187693](https://github.com/openclaw/openclaw/pull/187693) (pause Updates countdown), [#187686](https://github.com/openclaw/openclaw/pull/187686) (pause Logbook polling).
- **Channel fixes:** [#147886](https://github.com/openclaw/openclaw/pull/147886) (Feishu documented `markdown.tables` mode, flagged `merge-risk: compatibility`), [#187555](https://github.com/openclaw/openclaw/pull/187555) (Discord abort-signal propagation on cancellation), [#187199](https://github.com/openclaw/openclaw/pull/187199) (Mattermost Unicode normalization), [#187711](https://github.com/openclaw/openclaw/pull/187711) (Telegram forwarded rich-message debounce).
- **Platform correctness:** [#187703](https://github.com/openclaw/openclaw/pull/187703) preserves cache freshness across wall-clock changes; [#187177](https://github.com/openclaw/openclaw/pull/187177) makes Claude migration fail loudly on malformed JSON instead of silently skipping settings.
- **Memory subsystem:** [#136482](https://github.com/openclaw/openclaw/pull/136482) (XL) proposes moving memory-index publication into a bounded worker to reduce SQLite contention and reindex leaks — open since 2026-09-02 and still `needs proof`.

**Closed issues:** [#178000](https://github.com/openclaw/openclaw/issues/178000) (session-state races misclassified as model fallback), [#136386](https://github.com/openclaw/openclaw/issues/136386) (cron exec allowlist denial), [#187468](https://github.com/openclaw/openclaw/issues/187468) (package-swap update failure).

## 4. Community Hot Topics

Comment volume is the dominant engagement signal today; 👍 counts are near-zero across the board, suggesting discussion-heavy debugging rather than broad user voting.

| Item | Comments | Why it matters |
|---|---|---|
| [#160959](https://github.com/openclaw/openclaw/issues/160959) — P0: Gateway blocks for minutes capturing large external plugins | 64 | Highest-engagement issue in the sample. A 2026.9.6 regression from plugin generation capture (#144252) that blocks the event loop at startup and on runtime republish. Tagged `no-new-fix-pr` + `needs-maintainer-review`. |
| [#164294](https://github.com/openclaw/openclaw/issues/164294) — P2: state DB WAL auto-checkpoint never completes | 38 | Checkpoint permanently blocked by an in-process reader (17,091 log frames, 866 checkpointed). Persistence-layer design issue with unusually high engagement for a P2. |
| [#150635](https://github.com/openclaw/openclaw/issues/150635) — short-term recall eviction blocks dreaming promotion | 21 | Memory architecture defect: 512-entry cap + nightly zero-recall ingestion prevents deep-phase promotion. |
| [#120385](https://github.com/openclaw/openclaw/issues/120385) — Code Mode catalog incomplete for cron/memory-flush turns | 21 | MCP tools missing ("Unknown tool id") surfaced only as a generic exec failure; author corrected the original scope on 2026-10-21. |
| [#166634](https://github.com/openclaw/openclaw/issues/166634) — P0: checkpoint failure leaves agent unavailable after crash recovery | 18 | `SQLite session reclamation commit checkpoint expired` permanently blocks `chat.history`, `models.list`, etc. |
| [#172837](https://github.com/openclaw/openclaw/issues/172837) — P1: Codex `nativeSubagentAssignments` never removed | 17 | Assignment entries re-pin and re-send on every new parent thread after delivery gives up. |
| [#177656](https://github.com/openclaw/openclaw/issues/177656) — P1: host exec integrity-admission error | 17 | ≥28 failures on stable 2026.10.5; child effects unproven. |
| [#44309](https://github.com/openclaw/openclaw/issues/44309) — one-way A2A dispatch mode | 15 | Oldest high-engagement feature ask (2026-03-12); still `needs-product-decision`. |

**Underlying needs:** the top cluster is not about features — it is about **runtime reliability under load, crash, and clock/sleep events**. Users are operating gateways with large plugin trees, multi-GB SQLite state, and long-running sessions; the current failure modes are event-loop starvation, WAL/checkpoint contention, and unrecoverable post-crash states. A second cluster is **memory/recall correctness** (#150635, #157827, #148033), suggesting the dreaming/RAG pipeline needs design review rather than point fixes.

## 5. Bugs & Stability

Ranked by severity, from the top-50 sample. "No fix PR" reflects the `clawsweeper:no-new-fix-pr` tag.

**P0 — Critical**
1. [#160959](https://github.com/openclaw/openclaw/issues/160959) — Gateway event loop blocked for minutes by external/workspace plugin capture (2026.9.6 regression). **No fix PR identified.** Highest comment count in the dataset (64).
2. [#162211](https://github.com/openclaw/openclaw/issues/162211) — Startup blocks event loop 40–200s; health monitor misreads the stall as a disconnect and escalates into a restart loop (2026.9.7, macOS/launchd). Needs live repro.
3. [#166634](https://github.com/openclaw/openclaw/issues/166634) — Post-crash agent permanently `UNAVAILABLE` after reclamation checkpoint expiry. Not reproducible on main.
4. [#158592](https://github.com/openclaw/openclaw/issues/158592) — After host sleep/wake, model runtime publication times out and never recovers; all inbound messages fail until gateway restart.
5. [#160762](https://github.com/openclaw/openclaw/issues/160762) — Telegram + `claude-cli`: mid-turn message dropped entirely; drained prompt carries the *previous* message body (data/message loss). Needs info.
6. [#185819](https://github.com/openclaw/openclaw/issues/185819) — Windows quick-start Codex onboarding fails activation with inactive harness runtime artifact after successful device auth (2026.10.7).
7. [#141413](https://github.com/openclaw/openclaw/issues/141413) — macOS app omits auth token for local Gateway when `gateway.auth.token` is a protected SecretRef; tagged `needs-security-review`. 2 👍.

**P1 — High**
- [#166017](https://github.com/openclaw/openclaw/issues/166017) — `mcp.sessionIdleTtlMs` default not applied when unset; MCP runtimes never evicted; spawn broker hits 256-child cap and all exec/read/MCP starts fail. **Linked fix PR open.**
- [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram opaque `tgcb1:` callback payloads silently dropped for non-plugin callers. **Linked fix PR open.**
- [#177656](https://github.com/openclaw/openclaw/issues/177656) — Native host exec returns shared-state integrity-admission error without child result; ≥28 failures on 2026.10.5. No fix PR.
- [#172837](https://github.com/openclaw/openclaw/issues/172837) — Codex subagent assignments leak and re-send indefinitely. No fix PR.
- [#181459](https://github.com/openclaw/openclaw/issues/181459) — 3.5 GB agent DB makes startup a gamble (18s open, 4–6 min startup, then `commit checkpoint expired`); embeddings stored 3×. Related to #166634.
- [#176342](https://github.com/openclaw/openclaw/issues/176342) — Stale-lease integrity gates serialize agent startup after unclean shutdown, causing minutes-long unavailability.
- [#148789](https://github.com/openclaw/openclaw/issues/148789) — Model fallback chain exhausts all candidates on harness-level session failure and misattributes to last fallback.
- [#155396](https://github.com/openclaw/openclaw/issues/155396) — Codex tool turns fail closed on `provenance_rejected` during final-answer recovery.
- [#167667](https://github.com/openclaw/openclaw/issues/167667) — ChatGPT Responses tool continuation fails with "Rustponses cannot replay."

**P2 but notable**
- [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL checkpoint never completes (38 comments; likely a root cause contributor to #166634/#181459).
- [#163566](https://github.com/openclaw/openclaw/issues/163566) — `session-resource-loader` burns 80–150s CPU per turn after update+repair (2026.9.7).
- [#187490](https://github.com/openclaw/openclaw/issues/187490) — Flaky tests observed during 2026.10.9 stable validation; policy requires parallel fixes on `main`.

**Regression theme:** versions 2026.9.6–2026.10.7 introduced several event-loop/latency regressions (#160959, #162211, #163566). This is the single biggest stability signal in the digest.

## 6. Feature Requests & Roadmap Signals

User-requested features visible today:
- [#44309](https://github.com/openclaw/openclaw/issues/44309) — **One-way dispatch mode for A2A handoffs** without reply-back ping-pong. 15 comments, 7+ months old, awaiting product decision. Strong candidate for a `sessions_send` semantics extension.
- [#140244](https://github.com/openclaw/openclaw/issues/140244) — `--prompt-file <path>` for `openclaw infer model run` (avoids OS argv limits for large automation prompts).
- [#163739](https://github.com/openclaw/openclaw/issues/163739) — Make Control UI `Ctrl+Shift+A` (Archive) configurable; currently collides with browser shortcuts and cannot be remapped.
- [#154524](https://github.com/openclaw/openclaw/issues/154524) — Expose chat messages as headings for screen-reader navigation.
- [#141295](https://github.com/openclaw/openclaw/issues/141295) — TTL/manual clear for interrupted/cancelled pending inputs, which currently inject stale context into every turn after compaction.
- [#141973](https://github.com/openclaw/openclaw/issues/141973) — `tools.exec.notifyOnExit` should support a failure-only/user-requested-only mode and stop relaying SIGTERM-at-drain exits.
- [#150743](https://github.com/openclaw/openclaw/issues/150743) — QQ channel long-term support clarification after the Tencent handoff stalled (ecosystem/sustainability concern, not a bug).

**Prediction:** With #187699 already open, **MCP tool invocation via `tools.invoke`** is the most likely capability to land in the next patch/minor. The hidden-tab polling batch (#187686/#187687/#187691/#187693) appears staged for the next 2026.10.x. One-way A2A dispatch (#44309) and Control UI shortcut configurability (#163739) require product decisions and are unlikely in the immediate next version.

## 7. User Feedback Summary

**Pain points (dissatisfaction drivers):**
- **Startup and recovery reliability** is the dominant complaint. Multiple users describe gateways that take minutes to start, enter restart loops, or leave agents permanently `UNAVAILABLE` after crash/sleep (#160959, #162211, #166634, #158592, #181459, #176342). This is production-blocking, not cosmetic.
- **Latent data loss:** Telegram messages dropped mid-turn and replaced with the previous message body (#160762); message-loss labels also on #126950, #142037, #156070, #149985.
- **Cost/performance of long sessions:** Anthropic cache stuck at ~46k tools+system prefix with ~377–386k `cache_create` per turn (#140129) — a direct cost complaint.
- **Resource leaks:** MCP child processes fill the 256 cap (#166017); Codex subagent assignments accumulate (#172837); pending inputs never expire (#141295).
- **Configuration/onboarding friction:** Windows Codex onboarding (#185819), silent drop of `registerCliBackend` without `modelProvider` (#172969), and migration silently succeeding on malformed JSON (#187177 / underlying issue).

**Positive signals / satisfaction:**
- Users are engaging in high-quality, deeply instrumented reports (many rated 🦞 diamond lobster / 🐚 platinum hermit, with source-level repros and logs), which indicates a technically invested user base willing to help triage.
- Several reporters explicitly verify fixes and correct their own earlier hypotheses (#120385, #143784), showing collaborative maintenance culture.
- Web UI and accessibility polish requests (#163739, #154524, #147732, #177094) suggest users are moving beyond "does it work" toward daily-driver ergonomics — a maturity signal.

## 8. Backlog Watch

Important items with long dwell time or missing maintainer action:

| Item | Age / Status | Concern |
|---|---|---|
| [#44309](https://github.com/openclaw/openclaw/issues/44309) — one-way A2A dispatch | Created 2026-03-12 (~7.5 months), 15 comments | Oldest high-engagement feature request; `needs-product-decision`, no owner. |
| [#120385](https://github.com/openclaw/openclaw/issues/120385) — Code Mode catalog incomplete for cron | Created 2026-08-08, 21 comments, stale | Author corrected scope on 2026-10-21; still no fix PR. |
| [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram opaque callback drop | Created 2026-08-21, 11 comments, P1 | Has a linked open PR but no merge; message-loss impact. |
| [#127195](https://github.com/openclaw/openclaw/issues/127195) — 16 call sites emit identical error with no reason code | Created 2026-08-21, 8 comments | P2 but directly harms production diagnosability; tagged `fix-shape-clear` yet unassigned. |
| [#124306](https://github.com/openclaw/openclaw/issues/124306) — exec approval card doesn't flag mount-point targets | Created 2026-08-15, 6 comments | Safety/UX gap: syntactic risk shown, blast-radius risk hidden. |
| [#136482](https://github.com/openclaw/openclaw/pull/136482) — memory search contention / reindex leaks (XL) | Open since 2026-09-02 | Large architectural PR, `needs proof`, `merge-risk: session-state/availability`. Addresses root causes behind #150635/#164294. |
| [#147886](https://github.com/openclaw/openclaw/pull/147886) — Feishu markdown table mode (XL) | Open since 2026-09-14 | `merge-risk: compatibility`, screenshot proof present; channel users blocked on documented config being rejected. |
| [#150635](https://github.com/openclaw/openclaw/issues/150635) — recall eviction blocks dreaming promotion | Created 2026-09-17, 21 comments | `needs-product-decision`; memory pipeline correctness. |
| [#140129](https://github.com/openclaw/openclaw/issues/140129) — Anthropic cache stuck at 46k prefix | Created 2026-09-06, 15 comments | Cost-impacting; no fix PR. |

**Maintainer attention priorities:** (1) assign owners to the P0 event-loop/crash-recovery cluster (#160959, #162211, #166634, #158592), which share probable root causes in startup capture, WAL checkpointing, and runtime publication; (2) unblock the two open linked-PR P1s (#166017, #126950); (3) resolve the long-stalled product decisions on #44309 and #150635 so triage tags stop accumulating; (4) review the XL PRs #136482 and #147886 before they bit-rot.

---

*Data basis: 500 issues and 500 PRs updated in the last 24h; this digest analyzes the top 50 issues and top 30 PRs by comment count as provided. Severity tiers (P0/P1/P2) and `clawsweeper` tags are taken from issue labels.*

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Agent & Assistant Open-Source Ecosystem
**Data date:** 2026-10-29 · **Projects:** OpenClaw, Hermes Agent, IronClaw, QwenPaw, ZeroClaw

---

## 1. Ecosystem Overview

The open-source personal AI agent landscape on 2026-10-29 is bifurcated between one hyper-scale reference implementation (**OpenClaw**, 500 issues + 500 PRs touched in 24h) and a cluster of mid-scale, release-train-driven projects (**ZeroClaw**, **QwenPaw**, **Hermes**) each stabilizing a specific architectural thesis. No project shipped a release in the window — every active project is in a hardening or beta-preview phase, which suggests the ecosystem is consolidating around reliability rather than feature velocity. Across all five projects, the dominant investment area has shifted from "what the agent can do" to **session durability, multi-tenancy isolation, plugin supply-chain trust, and silent-failure elimination** — the classic signals of a stack moving from demo-grade to production-grade. Notably, activity is extremely uneven: IronClaw is effectively idle while OpenClaw runs ~500× its volume, meaning "ecosystem health" must be read per-project rather than in aggregate.

---

## 2. Activity Comparison

| Metric | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| Issues updated (24h) | 500 (142 closed / 358 open) | 50 (2 closed / 48 open) | 1 (1 open / 0 closed) | 13 (6 closed / 7 open) | 50 (9 closed / 41 open) |
| PRs updated (24h) | 500 (224 merged / 276 open) | 50 (5 merged / 45 open) | 0 | 38 (12 merged / 26 open) | 50 (14 merged / 36 open) |
| **PR close rate** (24h) | **44.8%** | 10.0% | — | **31.6%** | **28.0%** |
| **Issue close rate** (24h) | 28.4% | 4.0% | 0% | **46.2%** | 18.0% |
| Release status | No release; stable ref 2026.10.9 | No release; 0.22.x line | No release | No release; **2.3.0-beta.1** cycle | No release; **v0.9.0** hardening |
| Open P0 / S0 items | **≥7 P0** in top-50 sample | Several critical (data loss) | 0 | 2 high-severity beta blockers | 1 S0/P1 open |
| **Health score** (composite, 1–10) | **6.5** | **5.0** | **3.5** | **7.5** | **8.0** |
| Health rationale | Massive throughput but P0 concentration + zero release output | High engagement but intake-bound; 10% PR close rate | Idle; governance issue unanswered | Fast same-day fixes; beta packaging regressions | Best-managed release train; security backlog pending |

*Health score = weighted composite of 24h closure throughput, release-readiness progress, open critical-severity count, and responsiveness. Directional, not a maintainer grade.*

---

## 3. OpenClaw's Position

**Advantages vs peers**
- **Scale of community:** 500 issues + 500 PRs touched in one day is roughly **10–500×** the throughput of every peer. It has the deepest triage pipeline (labeled severities, `clawsweeper` automation, dedicated release-validation issues) — the only project here with a formalized release-validation and extended-stable backport process (#187490, #187252, #187701).
- **Breadth:** MCP tool invocation, multi-channel (Telegram, Discord, Feishu, Mattermost), Codex/Claude migration, memory/dreaming/RAG subsystems — the widest surface in the set.
- **Reference status:** It is the de facto "core reference," and peers' bug taxonomies echo its subsystems.

**Technical approach differences**
- **Gateway-centric architecture** with a large event loop and in-process plugin capture — the source of both its capability breadth and its biggest P0s (#160959 event-loop blocking, #162211 40–200s startup stalls, #164294 WAL checkpoint starvation).
- **SQLite-backed session/state store** at scale (3.5 GB DBs reported in #181459) — peers are earlier in this curve and have not yet hit the same persistence ceiling.

**Community size comparison**
| | OpenClaw | Next largest (ZeroClaw/Hermes) | Ratio |
|---|---|---|---|
| Daily PR throughput | 500 | 50 | **10×** |
| Daily merged PRs | 224 | 14 | **16×** |
| Engaged long-thread issues (top item comments) | 64 | 34 | ~2× |

**Assessment:** OpenClaw has unmatched top-of-funnel engagement and the most mature process, but its **P0 stability debt is now its defining risk** — event-loop starvation, crash-recovery unavailability, and WAL checkpoint failure are production-blocking at exactly the scale where trust matters most.

---

## 4. Shared Technical Focus Areas

Requirements appearing across two or more projects:

| Theme | Projects (specific needs) |
|---|---|
| **Session lifecycle, resume & persistence** | **OpenClaw** (post-crash `UNAVAILABLE`, checkpoint expiry, state-race misclassification); **Hermes** (unresumable remote sessions, `HERMES_HOME` invalidation, profile-mismatch refusal); **ZeroClaw** (sessions stuck `running` after kill — fixed #11523); **QwenPaw** (compaction/model scoping #8464). *Universal need: portable, recoverable session identity across restart/surface.* |
| **Multi-tenancy & secret isolation** | **Hermes** (`UnscopedSecretError` under `multiplex_profiles` — one root cause, 4+ high-severity reports); **ZeroClaw** (per-sender RBAC #5982, per-agent knowledge graph #9647, S0); **OpenClaw** (`gateway.auth.token` SecretRef handling #141413). |
| **Silent-failure elimination / observability** | **Hermes** (cron deliveries dropped for days, config sections silently deleted #146498); **QwenPaw** (Feishu images silently dropped #8150, Visual Compact no-ops #8488); **ZeroClaw** (misleading `plugin info [loads]` #11336); **OpenClaw** (model fallback misattribution #148789). *Recurring demand: explicit errors/warnings over degraded-but-silent operation.* |
| **Plugin supply-chain & install integrity** | **ZeroClaw** (manifest/component generation mismatch #11562, abandoned staging dirs #11792, lock-file perms #11360); **QwenPaw** (plugin loader crash #8473, `importlib.invalidate_caches` regression); **OpenClaw** (MCP `tools.invoke` not_found mismatch #187699). |
| **Packaging / frozen-build QA** | **QwenPaw** (PyInstaller omits `visual_compression/assets` #8488); **OpenClaw** (Windows Codex onboarding #185819); **ZeroClaw** (CLI-only build coverage #11975/#11976). |
| **Resource footprint / cost governance** | **OpenClaw** (Anthropic cache stuck at 46k prefix #140129, 690MB); **QwenPaw** (whisper import → 690MB RSS #8490); **Hermes** (ACP token double-count #146475); **ZeroClaw** memory-model separation. |
| **Provider registry & integration governance** | **IronClaw** (`providers.json` inclusion policy #8161); **ZeroClaw** (Obscurify.ai #12085); **Hermes** (aimlapi #146493); **QwenPaw** (max_tokens vs max_completion_tokens #8496). |

---

## 5. Differentiation Analysis

| Dimension | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| **Feature focus** | Broadest reference: MCP, multi-channel, memory/dreaming, Codex/Claude migration | Multi-surface session continuity + hybrid local/remote execution (#18715, 43👍) | Curated provider registry, governance-first | Vertical packaged apps (airport-SMS), plugin-based local model mgmt, Piccolo local scoring | Security-first multi-tenant runtime, plugin trust, RBAC |
| **Target users** | Production gateway operators, power users | Multi-host / multi-profile / multi-surface teams | Privacy-conscious, minimal-core adopters | Enterprise/regional deployments, packaged desktop | Shared-agent / multi-tenant deployments |
| **Architecture** | Gateway-centric, large event loop, in-process plugins, SQLite state | Remote `serve` + Desktop/CLI/Telegram/Slack control plane; multiplexed profiles | Minimal core, curated `providers.json` | Plugin Provider architecture, PyInstaller desktop, frozen builds, Rust? (Python) | Rust, composition-based runtime constructors, CLI-only build story |
| **Release posture** | Extended-stable backports, formal validation | 0.22.x incremental | None (idle) | 2.3.0-beta stabilization | v0.9.0 architecture/hardening train |
| **Maturity signal** | Highest engagement, highest P0 debt | High engagement, low close throughput | Low signal, governance-gated | Fast same-day issue→fix cycle, first-time contributors | Best close-throughput discipline, security backlog still open |

**Key takeaway:** The projects are not competing on the same axis. OpenClaw competes on *breadth and community*; ZeroClaw on *security isolation*; QwenPaw on *packaged/vertical deployment*; Hermes on *multi-surface portability*; IronClaw on *minimalism and curation*. This is healthy differentiation, but it also means **each has blind spots the others cover** — e.g., ZeroClaw's RBAC maturity vs OpenClaw's channel breadth.

---

## 6. Community Momentum & Maturity

**Activity tiers**

| Tier | Projects | Characterization |
|---|---|---|
| **T1 — Hyper-scale** | **OpenClaw** | 10× peer throughput; process-mature; stability-debt-carrying; zero release output today |
| **T2 — High-velocity stabilization** | **ZeroClaw**, **QwenPaw** | Active release trains (v0.9.0, 2.3.0-beta); healthy same-period closure; contributors landing fixes |
| **T3 — High-engagement, intake-bound** | **Hermes Agent** | Strong community quality (deep repros, 43👍 on #18715) but **10% PR close rate** — closing throughput, not interest, is the constraint |
| **T4 — Idle / governance-gated** | **IronClaw** | 1 issue, 0 PRs; contribution funnel blocked on policy answer (#8161) |

**Momentum read**
- **Rapidly iterating:** OpenClaw (500/500 but mostly triage + fixes), ZeroClaw (14 merges), QwenPaw (12 merges + 6 closures same day).
- **Stabilizing:** ZeroClaw (v0.9.0 hardening) and QwenPaw (2.3.0 beta) are the clearest "ship a quality release" trajectories.
- **At risk of stalling:** Hermes — 45/50 PRs and 48/50 issues remain open; the multiplexed-secret family (#130663/#146428/#146483/#146484) is a single root-cause fix that would materially improve its ratio.
- **Dormant:** IronClaw — low signal is neither positive nor negative, just absent.

---

## 7. Trend Signals

Industry-level patterns extracted from community feedback, ranked by signal strength:

1. **Session portability is now an expected primitive, not a feature.** Users run Desktop→remote `serve`→Telegram→Slack as one logical session (Hermes #139818/#41220; OpenClaw #158592; ZeroClaw cross-surface rename recovery #10373). *Developer implication: design resume/identity as a first-class, surface-agnostic contract — not per-surface logic.*

2. **Multi-tenancy and per-sender/per-agent isolation are table stakes.** Highest-severity open items cluster here: ZeroClaw #9647 (S0), #5982 (RBAC); Hermes `UnscopedSecretError` family; OpenClaw SecretRef auth. *Implication: secret scoping and ownership attribution must be enforced at the runtime boundary, not the config layer.*

3. **Silent failure is the #1 trust destroyer.** Every project has reports where the system "continues as if healthy" — dropped images, dropped cron deliveries, deleted config sections, misreported plugin loads. *Implication: structured error/reason codes and explicit warnings are a competitive differentiator, not polish.*

4. **Plugin supply-chain integrity is the new attack surface.** Manifest/component generation mismatch (ZeroClaw #11562), loader crashes (QwenPaw #8473), install staging debris (ZeroClaw #11792), MCP tool/impl mismatch (OpenClaw #187699). *Implication: signed manifests, generation pinning, and install transactions are becoming expected.*

5. **Resource footprint and cost are first-class complaints.** 690MB import side-effects (QwenPaw #8490), Anthropic cache prefix bloat (OpenClaw #140129), ACP token double-counting (Hermes #146475), separate cheap memory models (QwenPaw #7719). *Implication: cost observability and lazy-import hygiene belong in the release checklist.*

6. **Provider registries are hardening into governance questions.** IronClaw explicitly curating `providers.json` (#8161, citing Novita closure); ZeroClaw/Hermes/QwenPaw all fielding provider PRs. *Implication: the ecosystem is splitting into "curated core + external provider mechanism," which favors projects with a documented inclusion policy.*

7. **Packaging/frozen-build QA is a release gate, not an afterthought.** QwenPaw's beta shipped without required assets; OpenClaw and ZeroClaw both fielding CLI-only/onboarding build issues. *Implication: CI must build and smoke-test the frozen artifact, not just source.*

8. **Local-first / privacy-preserving inference is a credible demand vector.** Local TTS (Hermes #145492), local Piccolo scoring (QwenPaw #8463), privacy-provider registry pressure (IronClaw), Obscurify integration (ZeroClaw #12085). *Implication: privacy is emerging as a product category, not just a provider option.*

**Bottom line for decision-makers:** OpenClaw is the ecosystem's engine and highest-risk component simultaneously; ZeroClaw and QwenPaw are the healthiest release-readiness stories; Hermes is a strong community bottlenecked on triage; IronClaw is dormant and policy-gated. The cross-cutting technical mandate for 2026-11 is clear — **durable session identity, enforced isolation, loud failures, and trustworthy plugin supply chains.**

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent — Project Digest, 2026-10-29

*Data window: last 24 hours. Based on 50 updated issues (48 open / 2 closed) and 50 updated PRs (45 open / 5 merged or closed); issue/PR detail is drawn from the top-30/top-20 samples by comment count.*

---

## 1. Today's Overview

Activity remains very high: 50 issues and 50 PRs were touched in the last 24 hours, but the workload is heavily weighted toward intake and bug-fixing rather than shipping — no new release was cut, and only 2 issues and 5 PRs were closed against 48 and 45 still open. The dominant theme is **session lifecycle integrity**: resuming, migrating, and restoring sessions across remote backends, multiplexed gateways, profile moves, and surface changes (Desktop/CLI/Telegram), with several reports describing permanently unrecoverable sessions. A second cluster covers **multiplexed-profile secret scoping**, where `UnscopedSecretError` produces 500s, wedged cron runs, and silently dropped deliveries. Community demand is concentrated on hybrid deployment — a remote agent with local tool execution (#18715, 43 👍) — and on making sessions portable across surfaces. Overall the project is highly engaged and issue-report quality is high, but the open:closed ratio and the number of update/migration regressions suggest stability debt accumulating around the recent 0.22.x line.

---

## 2. Releases

None. Zero new releases in the last 24 hours; no changelog, breaking-change, or migration notes to report.

---

## 3. Project Progress

Only a small fraction of updated PRs landed in this window (5 merged/closed out of 50), so progress is best read as *fixes in flight* rather than shipped work.

**Closed / completed**
- **[Issue #140298](https://github.com/NousResearch/hermes-agent/issues/140298) (CLOSED)** — Session-authority admission ignored `user_id` routes, causing every normal DM to be rejected with `profile_mismatch`. Closing this removes a broad gateway admission failure mode.
- **[PR #141018](https://github.com/NousResearch/hermes-agent/pull/141018) (CLOSED)** — Bot Desktop abandoned screens now self-terminate after the idle window (follow-up to #137057); explicitly opened "for discussion," so the design may still be revisited.

**Fix PRs actively advancing (open)**
- Session/compaction correctness: [#146190](https://github.com/NousResearch/hermes-agent/pull/146190) (propagate in-turn compaction count to the live session), [#144601](https://github.com/NousResearch/hermes-agent/pull/144601) (preserve persisted user row during in-place compaction).
- Session creation robustness: [#146373](https://github.com/NousResearch/hermes-agent/pull/146373) (re-ask a lost `session.create` under the same `request_id`).
- Gateway/config boot: [#146130](https://github.com/NousResearch/hermes-agent/pull/146130) (remote config backend no longer deadlocks on first read).
- Kanban correctness: [#146235](https://github.com/NousResearch/hermes-agent/pull/146235) (per-board DB resolution instead of worker pin), [#146491](https://github.com/NousResearch/hermes-agent/pull/146491) (bind session workspace for managed-worker turns), [#146492](https://github.com/NousResearch/hermes-agent/pull/146492) (mark truncated ping text).
- Platform delivery: [#138795](https://github.com/NousResearch/hermes-agent/pull/138795) (Telegram held-inbound replay order), [#143647](https://github.com/NousResearch/hermes-agent/pull/143647) (preserve attachment inlining flags through album batching).
- Provider/auth: [#139881](https://github.com/NousResearch/hermes-agent/pull/139881) (recognize revoked access tokens in HTTP-200 SSE errors).
- Tooling/hygiene: [#146496](https://github.com/NousResearch/hermes-agent/pull/146496) (`scripts/` namespace shadowing), [#146233](https://github.com/NousResearch/hermes-agent/pull/146233) (Python recipe detection from real project evidence), [#146387](https://github.com/NousResearch/hermes-agent/pull/146387) (subagent cancellation metrics).

---

## 4. Community Hot Topics

| Item | Comments / 👍 | Signal |
|---|---|---|
| **[#139818](https://github.com/NousResearch/hermes-agent/issues/139818)** Remote-backend desktop sessions unresumable (`session.resume` → `not_found`) | 34 / 3 | Highest-engagement bug; remote Desktop + `hermes serve` topology is in real production use |
| **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715)** Remote agent with local tool execution | 32 / **43** | Strongest feature demand in the dataset; tagged `needs-decision`, open since 2026-05-02 |
| **[#131238](https://github.com/NousResearch/hermes-agent/issues/131238)** Desktop renders assistant reply twice (streaming bubble never settles) | 9 / 0 | Streaming/UI state defect with visible UX impact |
| **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** Telegram `/resume` should surface Desktop/CLI sessions | 8 / 0 | Session portability across surfaces |
| **[#135522](https://github.com/NousResearch/hermes-agent/issues/135522)** MCP write-capable tool approval silently denied in ACP | 7 / 0 | Consent/trust boundary; silent denial is worse than an error |
| **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** Slack Code channel-scoped sessions | 6 / 2 | Platform parity with Slack's new agent session model |
| **[#141574](https://github.com/NousResearch/hermes-agent/issues/141574)** Desktop saturates CPU (zygote at 600%+) on Omarchy/Arch | 6 / 0 | Performance/reliability on Linux |

**Underlying needs.** The comment concentration shows users running *multi-surface, multi-host, multi-profile* deployments: Desktop clients pointed at a remote `serve`, one gateway host fronting many profiles, and Telegram as a mobile continuation. The recurring unmet need is **portable session identity** — a session created on one surface/host should be resumable and attributable on another. The second need is **explicit consent and accounting** for tool use (MCP approvals, ACP token usage), i.e., users want the agent's actions to be auditable and billable without surprises.

---

## 5. Bugs & Stability

No crashes were newly disclosed as data loss on the agent's core loop, but several reports describe *silent* failures and permanent state corruption, which are effectively more severe. Ranked:

**Critical — data loss / unrecoverable state**
- **[#146478](https://github.com/NousResearch/hermes-agent/issues/146478)** `session.create` mints unrestorable sessions when config has no `model:` — surfaces as `storage_unavailable`, *not recoverable by wiping `state.db`*. No matching fix PR yet (#146373 addresses a different `session.create` failure mode).
- **[#146480](https://github.com/NousResearch/hermes-agent/issues/146480)** Moving `HERMES_HOME` invalidates every local session (`receipt refused: profile_mismatch`) with no rebind/migration path. No fix PR.
- **[#146498](https://github.com/NousResearch/hermes-agent/issues/146498)** Writing a partial config view silently deletes every omitted section, with no cross-process lock and no backup. No fix PR — high blast radius.
- **[#146483](https://github.com/NousResearch/hermes-agent/issues/146483)** Cron `deliver: bot-chat` permanently refused and payload silently dropped when the canonical "Bot Chat" row is closed — reported across 13 profiles for days. No fix PR.
- **[#139818](https://github.com/NousResearch/hermes-agent/issues/139818)** Remote-backend Desktop sessions unresumable after the serving process restarts. No fix PR; highest community engagement.

**High — functional breakage / security-adjacent**
- **[#130663](https://github.com/NousResearch/hermes-agent/issues/130663)**, **[#146428](https://github.com/NousResearch/hermes-agent/issues/146428)**, **[#146484](https://github.com/NousResearch/hermes-agent/issues/146484)** — Multiplexed-profile `UnscopedSecretError` family: dashboard `/api/ops/*` returns 500, cron finalization wedges permanently, and gateway-armed delivery plugins silently stop covering delivery. Same root-cause family; one systemic fix could close all three. No fix PR yet.
- **[#135522](https://github.com/NousResearch/hermes-agent/issues/135522)** Write-capable MCP tools with `trust: untrusted` are silently denied in ACP sessions with no approval prompt. Consent-boundary bug.
- **[#146475](https://github.com/NousResearch/hermes-agent/issues/146475)** ACP `PromptResponse.usage` reports session-cumulative tokens and no cache writes — clients double-count resumed sessions. Billing accuracy.
- **[#145156](https://github.com/NousResearch/hermes-agent/issues/145156)** Windows: Kanban worker hangs forever importing numpy under the `holographic` memory provider; killed only at `max_runtime`.
- **[#141574](https://github.com/NousResearch/hermes-agent/issues/141574)** Desktop CPU saturation (600–650% on one zygote) after GPU process launch failure on Arch/Omarchy.
- **[#142991](https://github.com/NousResearch/hermes-agent/issues/142991)** Regression since v0.22.4: `hermes chat --source desktop` fails with `invalid_params` before model connection — breaks scripts.
- **[#146385](https://github.com/NousResearch/hermes-agent/issues/146385)** Credential pool rotation after a 429 ignores the request model, so another model's cooldown blocks all credentials.

**Medium — UX / observability**
- **[#131238](https://github.com/NousResearch/hermes-agent/issues/131238)** Duplicate assistant bubble (streaming state never settles).
- **[#146430](https://github.com/NousResearch/hermes-agent/issues/146430)** Desktop status bar keeps previous session's cache-hit % and tokens/sec after switching.
- **[#146436](https://github.com/NousResearch/hermes-agent/issues/146436)** Clean OS restart misreported as unclean gateway death (SIGKILL/OOM).
- **[#146499](https://github.com/NousResearch/hermes-agent/issues/146499)** Hosted-room work evidence never persists after standalone→gateway→standalone handoff (`WorkRecordPrefixError`).
- **[#90068](https://github.com/NousResearch/hermes-agent/issues/90068)** Dashboard behind a path-prefix reverse proxy never mounts (`#root` blank).
- **[#110120](https://github.com/NousResearch/hermes-agent/issues/110120)** CLI agent stops executing actions; tagged `needs-repro` since 2026-09-13.

**Stability read.** The bug distribution is not random: it clusters on (a) session persistence/resume under non-default topologies, (b) secret scoping under `multiplex_profiles`, and (c) update/migration boundaries (v0.22.4 `--source` regression, multiplex migration #143150, home-path move #146480). That pattern points to insufficient migration/back-compat testing rather than isolated defects.

---

## 6. Feature Requests & Roadmap Signals

- **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — Remote agent with local tool execution (32 comments, 43 👍, `needs-decision`)**. The clearest roadmap signal in the dataset: split the control plane (remote memory/skills/sessions/models) from the execution plane (local terminal/tools). High reaction count and a six-month lifetime make this the top candidate for a design decision.
- **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** — Telegram `/resume` over cross-source sessions. Cheap to implement relative to value; would directly relieve the session-portability pain seen in #139818.
- **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** — Slack Code channel-scoped sessions, following Slack's Aug 2026 agent-session model.
- **[#145492](https://github.com/NousResearch/hermes-agent/pull/145492)** — Local TTS providers (Phonon, Ollama speech). Aligns with the project's local-first posture; likely merge candidate once reviewed.
- **[#146234](https://github.com/NousResearch/hermes-agent/pull/146234)** — MoA scoped paid references + daily spend alarm; cost-governance signal from a paying-user workflow.
- **[#146493](https://github.com/NousResearch/hermes-agent/pull/146493)** — aimlapi provider integration (vendor-submitted, 400k+ user aggregator). Ecosystem demand for provider breadth.
- **[#146473](https://github.com/NousResearch/hermes-agent/issues/146473)** — Mechanize the dependency DAG (152 edges, 9 cycles today). Internal maintainability signal; relevant to the architectural drift implied by the bug clusters.

**Prediction for the next version (likely a 0.22.6 patch):** session resume/`storage_unavailable` hardening, multiplexed-profile secret scoping, kanban workspace/DB fixes, compaction count/row preservation, and the remote-config boot deadlock. Larger items (#18715, Slack Code, cross-source `/resume`) look like 0.23+ material.

---

## 7. User Feedback Summary

**Pain points**
- *Session continuity is the #1 complaint.* Users lose resumability across process restarts, home-directory moves, multiplex migration, and surface changes — often with no recovery path.
- *Silent failure is worse than loud failure.* Cron deliveries dropped for days (#146483), MCP approvals denied without a prompt (#135522), config sections deleted without warning (#146498). Users repeatedly emphasize "silently" in titles.
- *Regressions on upgrade.* v0.22.4 broke `--source desktop` (#142991); the multiplex migration broke resume (#143150); the 0.22.5 cron delivery rework broke plugin coverage (#146484).
- *Platform gaps.* Windows kanban hangs (#145156), Linux desktop transparency/CPU issues (#145370, #141574), reverse-proxy dashboards (#90068).
- *Accounting transparency.* Double-counted ACP usage (#146475) and cross-model credential cooldowns (#146385) hit users who meter or pay per prompt.

**Use cases observed**
- One gateway host serving many profiles (13 profiles in #146483; multi-profile Docker in #143150).
- Remote `hermes serve` + local Desktop clients with basic auth (#139818).
- Local models (Qwen3.5:9B, GPT-OSS:20B) in CLI workflows (#110120).
- Mobile continuation via Telegram (#41220) and team collaboration via Slack (#120618).
- Vendor-side integration work done proactively (aimlapi, #146493).

**Satisfaction signal.** Reports are unusually detailed and reproduce-oriented, and third parties are building integrations — a healthy sign. Dissatisfaction is concentrated on *trust*: state that disappears, messages that vanish, and config that shrinks without notice.

---

## 8. Backlog Watch

Items with significant age and/or decision-blocking status that need maintainer attention:

- **[#18715](https://github.com/NousResearch/hermes-agent/issues/18715)** — open since 2026-05-02, `needs-decision`, 43 👍. The single most-upvoted request; a decision (accept/defer/decline) would unblock the community.
- **[#41220](https://github.com/NousResearch/hermes-agent/issues/41220)** — open since 2026-06-07, 8 comments, no 👍. Low-cost, high-leverage session-portability fix.
- **[PR #48143](https://github.com/NousResearch/hermes-agent/pull/48143)** — open since 2026-06-18 (`sweeper:blast-contained`), ComfyUI workflow metadata. Over four months without merge; a small, contained cleanup.
- **[#59220](https://github.com/NousResearch/hermes-agent/issues/59220)** — open since 2026-07-05. `skill_manage` tool description conflates `delete` and `remove_file`, causing accidental irreversible skill deletion. Documentation-level fix with real data-loss consequences.
- **[#90068](https://github.com/NousResearch/hermes-agent/issues/90068)** — open since 2026-08-19. Dashboard `base` path missing in `vite.config.ts`; documented reverse-proxy pattern is broken.
- **[#97230](https://github.com/NousResearch/hermes-agent/issues/97230)** — open since 2026-08-28. Cron replays already-delivered chunks after an ambiguous write; duplicate-delivery risk.
- **[#110120](https://github.com/NousResearch/hermes-agent/issues/110120)** — open since 2026-09-13, `needs-repro`. Reported as a severe CLI regression ("agent no longer executes actions") but stalled on reproduction; worth a maintainer-assisted triage.
- **[#120618](https://github.com/NousResearch/hermes-agent/issues/120618)** — open since 2026-09-23; platform-parity request with a documented upstream changelog reference.

**Health note.** With 45 of 50 touched PRs still open and 48 of 50 touched issues still open, closing throughput — not contributor interest — is the binding constraint this week. The most efficient lever is the multiplexed-secret family (#130663 / #146428 / #146483 / #146484), where one root-cause fix likely resolves four high-severity reports.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-10-29

**Repo:** [nearai/ironclaw](https://github.com/nearai/ironclaw)

---

## 1. Today's Overview

IronClaw had a very quiet 24-hour window: 1 issue updated, **0 pull requests** opened, merged, or closed, and **0 releases**. The only activity was a single new open issue ([#8161](https://github.com/nearai/ironclaw/issues/8161)) asking whether a third-party inference provider would be accepted into the built-in `providers.json` registry. No code was merged, so no features advanced and no fixes landed in this period. Overall activity assessment: **minimal / maintenance-quiet** — the project surface is stable but effectively idle for the day, with contributor energy currently focused on governance questions rather than implementation.

| Metric | Value |
|---|---|
| Issues updated (24h) | 1 (1 open, 0 closed) |
| PRs updated (24h) | 0 (0 open, 0 merged/closed) |
| Releases | 0 |
| Net code movement | None observed |
| Maintainer responsiveness signal | 0 comments on the day's only issue |

---

## 2. Releases

No new releases in this period. Nothing to report on changes, breaking changes, or migration notes.

---

## 3. Project Progress

No merged or closed pull requests today, therefore:

- **No features advanced.**
- **No bugs fixed** via merged code.
- **No PR review throughput** to measure.

The only forward-looking signal is procedural: a prospective contributor is seeking pre-approval before investing in a PR ([#8161](https://github.com/nearai/ironclaw/issues/8161)), explicitly citing the prior closure of the Novita provider PR ([#1115](https://github.com/nearai/ironclaw/pull/1115)) as evidence that `providers.json` should not grow unboundedly. That indicates the project's contribution funnel is currently gated on a policy answer, not on engineering capacity.

---

## 4. Community Hot Topics

**1. [#8161 — "Would you accept Obscurify.ai as a named provider?" (OPEN)](https://github.com/nearai/ironclaw/issues/8161)**
- Author: tonydavis629 (self-disclosed operator of the provider)
- Created/updated: 2026-10-28 · Comments: 0 · Reactions: 0

This is the only active thread, and by default the "hottest" item despite zero engagement. Its substance matters more than its metrics:

- The author references the **closed Novita PR ([#1115](https://github.com/nearai/ironclaw/pull/1115))**, where the reasoning given was that the provider list "shouldn't grow to include every provider." That precedent is being treated by contributors as an implicit policy.
- The author argues fit on the basis that the registry **already contains privacy-focused entries** (e.g., Tinfoil), positioning Obscurify.ai in the same category.

**Underlying need:** contributors lack a documented, predictable acceptance bar for `providers.json`. The question being asked is not really "is Obscurify.ai good?" but "what is the inclusion rule?" — a governance/documentation gap rather than a technical one. Every day this goes unanswered, potential contributors face the same wasted-effort risk that Novita reportedly hit.

---

## 5. Bugs & Stability

**No bugs, crashes, or regressions were reported or updated today.** Severity ranking: N/A.

Caveat for interpretation: with zero PR activity and zero issue reports in 24h, this is a *low-signal* stability reading, not positive evidence of hardening. It should be read as "no new negative signal" rather than "verified stable."

---

## 6. Feature Requests & Roadmap Signals

One implicit feature request today:

- **Add Obscurify.ai as a named provider in `providers.json`** ([#8161](https://github.com/nearai/ironclaw/issues/8161)) — a registry/config addition, not a code-path change.

**Prediction for the next version:** unlikely to appear as-is. The referenced precedent (Novita PR closed to avoid list bloat) suggests maintainers lean toward *curating* the built-in list rather than expanding it per-request. More probable near-term outcomes:

1. A written provider-inclusion policy or acceptance criteria (docs change).
2. A shift toward an **external/community provider mechanism** (plugin, user-supplied config, or third-party registry) so the core list stays small.
3. Only then, selective inclusion of privacy-focused providers that satisfy the new criteria.

Watch for a maintainer reply on #8161 as the clearest roadmap tell.

---

## 7. User Feedback Summary

Sample size today is a single user, and that user is a **provider operator with a commercial interest**, so treat as directional only.

- **Pain point:** uncertainty about contribution acceptance criteria → risk of building a PR that gets closed on policy grounds. The author explicitly asked *before* opening a PR, which is a rational response to unclear rules.
- **Use case signaled:** privacy-oriented inference providers as a recognized category in IronClaw's provider registry — i.e., users care about privacy-preserving model access, not just raw provider count.
- **Satisfaction/dissatisfaction:** no explicit satisfaction data; no complaints about bugs or behavior. The mild friction is process-related, not product-related.

---

## 8. Backlog Watch

- **[#8161](https://github.com/nearai/ironclaw/issues/8161)** — open, 0 comments, ~1 day old with no maintainer response. Not yet "long-unanswered," but it is **blocking a volunteer's contribution decision** and is the only item in flight; a short policy answer has outsized leverage today.
- **[#1115](https://github.com/nearai/ironclaw/pull/1115)** (Novita provider PR, closed) — worth a maintainer-authored summary of the closure rationale. Its reasoning is already being cited by new contributors as de facto policy, which means the precedent is circulating without an authoritative, linkable statement.

**Recommended maintainer action:** respond to #8161 with either (a) acceptance criteria for `providers.json` entries, or (b) a pointer to the intended external-provider path — and consider converting that answer into documentation to stop the same question recurring.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-29

## 1. Today's Overview

QwenPaw is in an intensive **2.3.0-beta stabilization cycle**: 38 PRs were touched in the last 24 hours (26 open, 12 merged/closed) against 13 issue updates (7 open, 6 closed), with **no new release published**. Throughput is high, but the open-issue set is dominated by **beta packaging and runtime regressions** — most notably #8473 (all 12 backend plugins fail to load on 2.3.0-beta.1) and #8488 (desktop build ships without visual-compression assets). The maintainers closed two long-running bugs/enhancements today (#8464, #8150) and merged the matching conversation-model fix (#8465), showing responsive triage on high-signal reports. Overall health is **active but release-blocked**: the 2.3.0 beta still has at least two high-severity, unfixed defects, while architectural work (plugin-based local model management, Piccolo scoring, separate memory-write models) continues in parallel.

---

## 2. Releases

No new releases in the last 24 hours, and no release history was provided. *(Section omitted per data availability.)*

---

## 3. Project Progress

### Merged / Closed PRs today (6 visible in the top-20 sample; 12 total)

| PR | Scope | Impact |
|---|---|---|
| [#8465](https://github.com/agentscope-ai/QwenPaw/pull/8465) `fix(models)` XL | Honors conversation-level model settings across all runtime paths, including slash-command dispatch | **Fixes #8464** — `/compact` now uses the session model (M2) instead of the persisted agent default (M1) |
| [#8358](https://github.com/agentscope-ai/QwenPaw/pull/8358) `fix(providers)` M | Aligns Console "thinking" defaults with actual OpenAI-compatible wire parameters (`thinking_enabled`, effort levels) | Removes a silent config/behavior mismatch for custom Chat Completions models |
| [#8492](https://github.com/agentscope-ai/QwenPaw/pull/8492) `fix(security)` M | Prevents false-positive approvals for Code Mode in JEV mode | Security guard correctness — fewer spurious approval prompts without weakening review |
| [#8253](https://github.com/agentscope-ai/QwenPaw/pull/8253) `feat(website)` XXXL | Bilingual JSON-driven evaluation leaderboard at `/evaluation/` with SVG cost/performance chart and Pareto frontier | Community-facing benchmark transparency |
| [#8494](https://github.com/agentscope-ai/QwenPaw/pull/8494) `feat(airport-sms)` S | Scaffolds `packages/airport_sms` with CI | Enables a new vertical SMS-agent package boundary |
| [#8495](https://github.com/agentscope-ai/QwenPaw/pull/8495) `feat(airport-sms)` XXXL | Full SMS Agent stack (PR01–PR15: smoke, governance, console, WeCom) | Large domain-specific feature landing |

### Closed Issues today
- **#8464** (bug, 4 comments) — `/compact` ignored session model switch; resolved by #8465.
- **#8150** (bug, 3 comments) — Feishu inbound rich-text images silently dropped.
- **#4356** (enhancement) — finer-grained file guard / read-only paths.
- **#4129** (enhancement) — `/rewind`-style rollback for files and context.
- **#7740** (enhancement) — Hub-mode admin password reset.
- **#8459** (enhancement) — easier access to unread push messages in Console.

**Takeaway:** the day's merged work clusters around **configuration correctness** (model resolution, thinking parameters) and **security review accuracy**, plus one major docs/site feature. Two multi-month enhancement requests (#4356 from May, #4129 from May) finally closed, suggesting backlog grooming is happening alongside beta work.

---

## 4. Community Hot Topics

1. **[#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — 4 comments (most active issue)** · CLOSED
   Manual `/compact` ignored the session-level model override and always used `agent.json`'s `active_model`; with a strict provider (400 on unsupported params), compaction failed regardless of which model the user selected. The underlying need is **one consistent model-resolution path** for normal turns, slash commands, and background jobs — a class of bug that recurs whenever a new entry point bypasses `AgentBuilder`. Fixed same-day by #8465.

2. **[#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — 3 comments** · CLOSED
   Feishu inbound `post` messages with embedded images delivered text but **silently dropped the images** — no download, no warning. The need here is **channel parity and observable failure**: users expect multimodal inbound content to match outbound capabilities (see the referenced #2792 for the outbound direction). Silent truncation is worse than an explicit error because agents answer with incomplete context.

3. **[#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) — 2 comments** · OPEN
   Windows PyInstaller desktop build of `2.3.0-beta.1` omits `visual_compression/assets`, so every eligible model call raises `FileNotFoundError: atlas-gray-native.ts` and Visual Compact **silently no-ops**. Signals a packaging-manifest gap between source and frozen builds.

4. **[#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — 2 comments** · OPEN
   All 12 backend plugins fail at startup due to unguarded `importlib.invalidate_caches()` in `module_isolation.py:220`. The reporter explicitly notes it is **the same root cause as #8265 at a new call site** — a regression pattern rather than a one-off.

*Note: PR comment counts were reported as `undefined` in the dataset, so PR hotness is inferred from review labels (`Under Review`) and size rather than engagement.*

---

## 5. Bugs & Stability

Ranked by severity, based on user impact and blast radius:

| Severity | Issue | Status | Fix PR? |
|---|---|---|---|
| **Critical** | [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) — 2.3.0-beta.1: **all backend plugins fail to load** (12/12) at server startup; `TypeError` from unguarded `importlib.invalidate_caches()` in `module_isolation.py:220`. Same root cause as #8265 at a new call site. | OPEN | Likely [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497) (tolerate cache-refresh failures) and [#8345](https://github.com/agentscope-ai/QwenPaw/pull/8345) (don't abort plugin install) — both open, overlapping |
| **High** | [#8488](https://github.com/agentscope-ai/QwenPaw/issues/8488) — Desktop/PyInstaller build omits `visual_compression` atlases → `FileNotFoundError` on every eligible model call; Visual Compact silently no-ops. | OPEN | None visible in provided data |
| **High** | [#8490](https://github.com/agentscope-ai/QwenPaw/issues/8490) — `GET /api/workspace/local-whisper-status` does a real `import whisper`, pulling `torch` + CUDA into the server process (~690 MB RSS, never released) merely to render a settings page. | OPEN | ✅ [#8491](https://github.com/agentscope-ai/QwenPaw/pull/8491) (first-time contributor, avoids the import in the probe) |
| **Medium** | [#8464](https://github.com/agentscope-ai/QwenPaw/issues/8464) — `/compact` used the wrong model, causing hard 400 failures with strict providers. | CLOSED | ✅ [#8465](https://github.com/agentscope-ai/QwenPaw/pull/8465) merged/closed |
| **Medium** | [#8150](https://github.com/agentscope-ai/QwenPaw/issues/8150) — Feishu inbound images silently dropped (data loss with no warning). | CLOSED | Not visible in sample |
| **Low** | [#8486](https://github.com/agentscope-ai/QwenPaw/issues/8486) — Console tool-call `<>` icon clipped on the left edge (missing container padding); visible at higher zoom. | OPEN | None |

**Pattern worth flagging:** three of the five open/high bugs (#8473, #8488, #8490) are **specific to the 2.3.0-beta.1 install paths** (frozen desktop build, plugin loader, optional-dependency probing). This points to a QA gap in **packaging and lazy-import hygiene** rather than core agent logic. Also relevant: [#8496](https://github.com/agentscope-ai/QwenPaw/pull/8496) (open) addresses `max_tokens` vs `max_completion_tokens` collisions causing HTTP 400 on scroll-summary requests — another provider-compatibility stability fix in flight.

---

## 6. Feature Requests & Roadmap Signals

Open requests:
- **[#8478](https://github.com/agentscope-ai/QwenPaw/issues/8478)** — Sync bundled DOCX/PPTX/XLSX skills with Anthropic's July 2026 Office skills update while keeping QwenPaw adaptations. Signals that **skill freshness** is now a competitive expectation.
- **[#8481](https://github.com/agentscope-ai/QwenPaw/issues/8481)** — `QWENPAW_LANGUAGE`-style env var to set the default language during initial Docker setup. Small, deployment-oriented, high leverage for international/enterprise rollouts.
- **[#8498](https://github.com/agentscope-ai/QwenPaw/issues/8498)** — "install easily" question (template-only body); low signal, likely docs/onboarding gap.
- Closed today: **#4356** (granular file-guard read-only paths), **#4129** (`/rewind` backup/rollback), **#7740** (Hub admin password reset), **#8459** (unread push-message access). Their closure implies these are either implemented or consciously deferred.

Roadmap signals from open PRs:
- **[#8499](https://github.com/agentscope-ai/QwenPaw/pull/8499)** (WIP, XXXL) — extract local model management into a **Plugin Provider** (downloads, llama.cpp install, service lifecycle). Architectural direction: a thinner host with pluggable providers, preserving existing configs/model files.
- **[#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463)** (XL) — run local **Piccolo** models with bidirectional candidate scoring for JEV tool review and ReMe compaction. Local, privacy-preserving replacements for hosted review models.
- **[#7719](https://github.com/agentscope-ai/QwenPaw/pull/7719)** (XL, Under Review) — separate, cheaper model for **ReMeLight memory writing** (`auto_memory` / `auto_dream` / `daily_paper` / `auto_fin`). Direct cost-control request from heavy users.
- **[#8485](https://github.com/agentscope-ai/QwenPaw/pull/8485)** — "Install app" button for Extension apps in Console.

**Prediction for the next version (2.3.0 final):** plugin-loader hardening (#8497/#8345), desktop asset bundling (#8488), lazy whisper probing (#8491), conversation-scoped model settings (#8465), provider token-limit compatibility (#8496). Larger items (#8499 local-model plugin extraction, #8463 Piccolo, #7719 memory model) look like **2.3.x/2.4** material given their XXXL/XL size and open review state.

---

## 7. User Feedback Summary

**Pain points observed:**
- **Silent failures erode trust.** #8150 (images dropped with no warning), #8488 (Visual Compact no-ops), and #8464 (compaction fails with opaque 400s) all share a theme: the system continues as if healthy while degrading output. Users are explicitly asking for warnings/logging, not just fixes.
- **Resource footprint matters.** #8490's 690 MB permanent RSS increase from merely *opening a settings page* is a serious complaint for self-hosted/desktop users; the fix being a first-time contributor's PR suggests the community is motivated to help.
- **Beta packaging quality.** Two independent reporters on `2.3.0-beta.1` hit install/build-level breakage (plugin loader, PyInstaller assets), which colors perception of the beta more than core logic bugs would.
- **Configuration consistency.** #8464 and #8358 both concern *what the UI shows* vs *what the runtime does* — a recurring satisfaction risk in a multi-provider, multi-model product.
- **Deployment/ops basics.** #7740 (admin password reset in Hub mode) and #8481 (default language env var) come from operators running QwenPaw for teams, not single users — the product is being used in managed deployments and lacks corresponding admin affordances.
- **Power-user workflow gaps.** #4129 (`/rewind`) and #4356 (read-only guarded paths) show users pushing agent autonomy further and needing **reversibility and least-privilege controls** to feel safe.

**Satisfaction signals:** rapid same-day turnaround on #8464 (issue → fix PR → close), responsive triage on #8150 after a 20-day open period, and multiple first-time contributors (#8491, #8345, #8400, #8494) indicate a healthy, approachable contribution pipeline.

---

## 8. Backlog Watch

Items needing maintainer attention, ordered by staleness × importance:

| Item | Age | Why it matters |
|---|---|---|
| [#7719](https://github.com/agentscope-ai/QwenPaw/pull/7719) `feat(memory)` XL, Under Review | ~47 days (since 2026-09-12) | Cost-control feature with clear demand; long review latency risks contributor drop-off |
| [#7835](https://github.com/agentscope-ai/QwenPaw/pull/7835) `fix(memory)` M, Under Review | ~42 days (since 2026-09-17) | Stops auto-memory-recall payload leaking into channel output; overlaps conceptually with [#8457](https://github.com/agentscope-ai/QwenPaw/pull/8457) — **two competing approaches to the same defect**, needs a decision |
| [#8308](https://github.com/agentscope-ai/QwenPaw/pull/8308) `test(api)` XS | ~11 days | Trivial test-alignment PR stuck open; cheap win for CI reliability |
| [#8309](https://github.com/agentscope-ai/QwenPaw/pull/8309) `fix(cli)` M, Under Review | ~11 days | Ollama model discovery before activation — onboarding blocker for local-model users |
| [#8345](https://github.com/agentscope-ai/QwenPaw/pull/8345) `fix(plugins)` S, first-time | ~9 days | Overlaps [#8497](https://github.com/agentscope-ai/QwenPaw/pull/8497); **consolidate** to avoid duplicate/conflicting fixes for #8473 |
| [#8400](https://github.com/agentscope-ai/QwenPaw/pull/8400) `fix(skill)` XS, first-time | ~6 days | The built-in `make-skill` is blocked by QwenPaw's own security scanner — a self-inflicted false positive that undermines trust in the scanner |
| [#8463](https://github.com/agentscope-ai/QwenPaw/pull/8463) `feat(models)` XL | ~1 day | Not stale yet, but large and strategically important (local Piccolo inference); watch for review capacity |
| [#8473](https://github.com/agentscope-ai/QwenPaw/issues/8473) plugin load failure | 1 day | **Same root cause as #8265 at a new call site** — needs a systemic fix (guard all `invalidate_caches()` call sites), not a per-site patch |

**Recommendation for maintainers:** (1) designate one canonical fix for the `invalidate_caches` class of failures and close the duplicate; (2) resolve the #7835 vs #8457 memory-injection design split; (3) bundle #8488 and #8491 into the 2.3.0 release checklist so the beta does not ship with a silently disabled compression feature and a 690 MB import side effect.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-29

## 1. Today's Overview

ZeroClaw remained highly active on 2026-10-29: **50 issues and 50 PRs were updated in the last 24 hours**, including **14 PRs merged/closed** and **9 issues closed**, with **no new release**. The day’s closed work concentrated on v0.9.0 hardening: runtime session recovery, web chat state retention, OIDC credential handling, feature-graph cleanup, CLI-only builds, and plugin/CLI correctness. The active queue is dominated by high-risk security and architecture work around multi-tenancy, plugin trust boundaries, and memory isolation. Overall project health is strong on throughput and release readiness, but several long-running **P1/S0 security issues** remain open and need sustained maintainer attention.

---

## 2. Releases

**No new releases in the last 24 hours.** The latest referenced published build in issue discussion is **v0.8.6**, while multiple PRs and issues are tagged `release:v0.9.0`, indicating active preparation for the next release.

---

## 3. Project Progress

### Runtime and session reliability
- **[PR #11523](https://github.com/zeroclaw-labs/zeroclaw/pull/11523) — CLOSED**  
  Fixes sessions left permanently `running` after a daemon is killed mid-turn. Closes **[Issue #11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432)**.

### Web dashboard and ZeroCode UX
- **[PR #11571](https://github.com/zeroclaw-labs/zeroclaw/pull/11571) — CLOSED**  
  Preserves the in-flight user prompt when chat hydration replaces local state. Closes **[Issue #11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517)**.
- **[PR #11511](https://github.com/zeroclaw-labs/zeroclaw/pull/11511) — CLOSED**  
  Makes ZeroCode Config save/cancel behavior consistent. Closes **[Issue #11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486)**.
- **[PR #12100](https://github.com/zeroclaw-labs/zeroclaw/pull/12100) — OPEN**  
  Follow-up fix to keep `FieldForm` draft state in sync after entity enable toggles.

### Build, CI, and CLI-only support
- **[PR #11269](https://github.com/zeroclaw-labs/zeroclaw/pull/11269) — CLOSED**  
  Stops `metrics` and `embedded-web` from implicitly enabling the full gateway. Closes **[Issue #11969](https://github.com/zeroclaw-labs/zeroclaw/issues/11969)**.
- **[PR #11973](https://github.com/zeroclaw-labs/zeroclaw/pull/11973) — CLOSED**  
  Restores agent owned-state cascade for delete/rename in builds without the gateway. Closes **[Issue #11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970)**.
- **[PR #11974](https://github.com/zeroclaw-labs/zeroclaw/pull/11974) — CLOSED**  
  Runs gateway-dependent tests only when the gateway is built.
- **[PR #11988](https://github.com/zeroclaw-labs/zeroclaw/pull/11988) — CLOSED**  
  Drops `nosemgrep`-suppressed findings before SARIF upload. Closes **[Issue #11987](https://github.com/zeroclaw-labs/zeroclaw/issues/11987)**.
- **[PR #11975](https://github.com/zeroclaw-labs/zeroclaw/pull/11975) — OPEN** and **[PR #11976](https://github.com/zeroclaw-labs/zeroclaw/pull/11976) — OPEN**  
  Add CLI-only build guidance and CI coverage with an artifact-size ratchet.

### Security and channels
- **[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968) — CLOSED**  
  Form-encodes OIDC enrollment Basic credentials per RFC 6749. Closes **[Issue #11957](https://github.com/zeroclaw-labs/zeroclaw/issues/11957)**.
- **[PR #11979](https://github.com/zeroclaw-labs/zeroclaw/pull/11979) — CLOSED**  
  Logs channel reply success only after delivery completes, preventing false success records on rejected sends.

### Providers, docs, and dependencies
- **[PR #11404](https://github.com/zeroclaw-labs/zeroclaw/pull/11404) — CLOSED**  
  Stabilizes fake Grok fixture timing in macOS tests.
- **[PR #11980](https://github.com/zeroclaw-labs/zeroclaw/pull/11980)** and **[PR #11963](https://github.com/zeroclaw-labs/zeroclaw/pull/11963) — CLOSED**  
  Closed runtime-architecture documentation proposals supporting the v0.9.0 composition work.
- **[PR #12018](https://github.com/zeroclaw-labs/zeroclaw/pull/12018) — CLOSED**  
  Dependency bump with 35 Rust crate updates.
- **[PR #12102](https://github.com/zeroclaw-labs/zeroclaw/pull/12102) — OPEN**  
  New Dependabot bump with 34 updates.

---

## 4. Community Hot Topics

PR comment counts were not populated in the snapshot, so ranking below is driven by issue comment counts.

| Item | Activity | Underlying need |
|---|---:|---|
| **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — Per-sender RBAC for multi-tenant deployments | 11 comments | Multi-tenant isolation and least-privilege access. Discussion has narrowed scope to building sender roles on the existing agent/risk-profile model; a draft exists in #11068. |
| **[Issue #10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)** — Prove Discord plugin installation from a release binary | 9 comments | Trustworthy plugin distribution and installation from published artifacts. Real Discord inbound/outbound delivery was verified on v0.8.6, but the issue remains blocked. |
| **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — Schema-validated memory consolidation | 7 comments | Cross-provider memory reliability. Current JSON-in-prompt parsing is fragile across models; users need bounded fallbacks and schema validation. |
| **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — Knowledge graph lacks per-agent attribution | 5 comments | A single shared knowledge graph lets any agent read or mutate another agent’s data. This is an S0 security/data-loss concern. |
| **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack “is thinking…” status missing since v0.8.5 | 5 comments | Channel UX regression in Slack threads; users want visible progress indicators during agent turns. |
| **[Issue #11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)** — Skill review/creation skipped for channel, webhook, gateway turns | 4 comments | Learning-loop parity across all entry points, not just CLI/cron runs. |

**Analysis:** The community is pushing hardest on **security isolation and operational trust** — multi-tenant RBAC, per-agent memory/knowledge ownership, and plugin installation integrity. A second cluster is **UX consistency across channels and surfaces**: Slack progress indicators, web chat state preservation, and skill-learning behavior outside the CLI.

---

## 5. Bugs & Stability

Ranked by severity/risk:

| Severity | Item | Status | Notes / fix PR |
|---|---|---|---|
| **S0 / P1** | **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — Knowledge graph has no per-agent attribution; any agent can read/mutate another’s knowledge | **OPEN** | High-risk data isolation bug. No fix PR shown. |
| **S1** | **[Issue #11957](https://github.com/zeroclaw-labs/zeroclaw/issues/11957)** — OIDC enrollment sends raw client credentials in Basic header | **CLOSED** | Fixed by **[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968)**. |
| **P1** | **[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** — `gateway.pairing_dashboard` accepted but unread; pairing codes never expire | **OPEN** | Security hardening for v0.9.0. No fix PR shown. |
| **S2 / P2** | **[Issue #11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)** — Skill review and creation never run for channel/webhook/gateway turns | **OPEN** | Learning loop degraded outside CLI/cron. |
| **S2 / P2** | **[Issue #11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562)** — Plugin discovery can pair one generation’s manifest with another’s component during update | **OPEN** | Plugin integrity issue in permissive/disabled signature mode. |
| **S2 / P2** | **[Issue #11820](https://github.com/zeroclaw-labs/zeroclaw/issues/11820)** — `config set` for plugin instance settings/egress writes disk only and never reaches running daemon | **OPEN** | Runtime/config divergence. |
| **S2 / P2** | **[Issue #11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336)** — `plugin info` / `plugin list --verify` report `[loads]` for a runtime-refused plugin | **OPEN** | Misleading CLI diagnostics. |
| **S2** | **[Issue #11517](https://github.com/zeroclaw-labs/zeroclaw/issues/11517)** — Web chat reload during a turn drops prompt from screen and localStorage | **CLOSED** | Fixed by **[PR #11571](https://github.com/zeroclaw-labs/zeroclaw/pull/11571)**. |
| **S2** | **[Issue #11432](https://github.com/zeroclaw-labs/zeroclaw/issues/11432)** — Killed daemon leaves session marked `running` forever | **CLOSED** | Fixed by **[PR #11523](https://github.com/zeroclaw-labs/zeroclaw/pull/11523)**. |
| **P2** | **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack thinking status missing since v0.8.5 | **OPEN, in progress** | Channel UX regression. |
| **S3 / P3** | **[Issue #11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360)** — Package lock file readable by other local accounts | **OPEN** | Availability/security hardening. |
| **S3 / P3** | **[Issue #11358](https://github.com/zeroclaw-labs/zeroclaw/issues/11358)** — `plugin migrate` moves packages without package lock | **OPEN** | Concurrency/plugin integrity. |
| **S3** | **[Issue #11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792)** — Interrupted plugin install leaves abandoned staging directory | **OPEN** | Cleanup/operator hygiene. |

**Stability read:** The day closed several meaningful regressions, especially around session state and web chat. However, **plugin state integrity** and **cross-agent memory isolation** remain the largest unresolved stability/security themes.

---

## 6. Feature Requests & Roadmap Signals

### Likely v0.9.0 scope
Items explicitly tagged `release:v0.9.0` and already receiving active PRs:
- Runtime composition and capability-based constructors: **[Issue #10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993)**, **[PR #11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)**.
- CLI-only build and feature-graph correctness: **[PR #11975](https://github.com/zeroclaw-labs/zeroclaw/pull/11975)**, **[PR #11976](https://github.com/zeroclaw-labs/zeroclaw/pull/11976)**.
- Security fixes for OIDC and gateway pairing: **[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)**, **[PR #11968](https://github.com/zeroclaw-labs/zeroclaw/pull/11968)**.
- Agent lifecycle correctness without gateway: **[Issue #11970](https://github.com/zeroclaw-labs/zeroclaw/issues/11970)**, **[PR #11973](https://github.com/zeroclaw-labs/zeroclaw/pull/11973)**.

### Accepted but blocked or parked
- **[Issue #10997](https://github.com/zeroclaw-labs/zeroclaw/issues/10997)** — Install and activate selected plugins during onboarding. **Blocked.**
- **[Issue #10998](https://github.com/zeroclaw-labs/zeroclaw/issues/10998)** — Deliver minimal core tool set and binary-size evidence. **Blocked.**
- **[Issue #10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)** — Publish/prove Discord plugin installation from release binary. **Blocked.**
- **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — Schema-validated memory consolidation. **Parking lot.**

### Newer feature signals
- **[Issue #11890](https://github.com/zeroclaw-labs/zeroclaw/issues/11890)** — Consolidation evidence gate: label memory updates durable/transient and quote user text before writing to Core.
- **[Issue #11561](https://github.com/zeroclaw-labs/zeroclaw/issues/11561)** — Check an update’s added plugin authority inside the plugin host.
- **[Issue #11793](https://github.com/zeroclaw-labs/zeroclaw/issues/11793)** — Owner-only data directory and detection of group/world-writable roots.
- **[PR #12085](https://github.com/zeroclaw-labs/zeroclaw/pull/12085)** — Add Obscurify.ai as a typed OpenAI-compatible provider.
- **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — Per-sender RBAC, accepted but architecturally significant; likely post-v0.9.0 unless narrowed further.

**Prediction:** v0.9.0 will primarily be a **hardening and architecture release** — runtime composition, CLI-only correctness, plugin/security trust boundaries, and gateway feature separation — rather than a broad feature release. RBAC and advanced memory consolidation are more likely to follow after the composition and isolation foundations settle.

---

## 7. User Feedback Summary

**Main pain points reported by users:**

- **Multi-tenant isolation is insufficient.** Both **[#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** and **[#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** point to the same concern: agents can operate with too much shared authority. Users want per-sender roles and per-agent ownership of memory/knowledge.
- **Plugin installation and upgrade behavior is hard to trust.** Users report misleading `plugin info` output (**[#11336](https://github.com/zeroclaw-labs/zeroclaw/issues/11336)**), manifest/component generation mismatches (**[#11562](https://github.com/zeroclaw-labs/zeroclaw/issues/11562)**), lock-file permission issues (**[#11360](https://github.com/zeroclaw-labs/zeroclaw/issues/11360)**), and abandoned staging directories (**[#11792](https://github.com/zeroclaw-labs/zeroclaw/issues/11792)**).
- **Memory consolidation is fragile across providers and models.** **[#6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** describes raw JSON parsing failures, Markdown fences, extra prose, and invalid fields. **[#11890](https://github.com/zeroclaw-labs/zeroclaw/issues/11890)** asks for stronger evidence before promoting transient information into long-term memory.
- **Channel behavior is inconsistent.** Slack users lost the “is thinking…” indicator after v0.8.5 (**[#11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)**), and skill-learning loops do not run for channel/webhook/gateway turns (**[#11332](https://github.com/zeroclaw-labs/zeroclaw/issues/11332)**).
- **CLI and running daemon can diverge.** **[#11820](https://github.com/zeroclaw-labs/zeroclaw/issues/11820)** reports that `config set` updates disk but does not reach a live daemon, which is a significant operator-experience problem.
- **Configuration UX is improving.** The closure of **[#11486](https://github.com/zeroclaw-labs/zeroclaw/issues/11486)** via **[PR #11511](https://github.com/zeroclaw-labs/zeroclaw/pull/11511)** addresses inconsistent ZeroCode save/cancel behavior.

**Satisfaction signal:** The project is responsive — many reported issues received fixes or accepted-status updates within the same period. Dissatisfaction is concentrated in **security isolation, plugin trust, and surface parity**, not in general project responsiveness.

---

## 8. Backlog Watch

Long-running or important items that need maintainer attention:

| Item | Age / Status | Why it matters |
|---|---|---|
| **[Issue #5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — Per-sender RBAC | Created 2026-04-22; open, accepted, 11 comments | Foundational for multi-tenant deployments. Scope has narrowed, but implementation is still pending. |
| **[Issue #6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998)** — Schema-validated memory consolidation | Created 2026-05-29; parking lot | Memory reliability across providers remains a recurring user concern. |
| **[Issue #9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)** — Pairing dashboard unread; pairing codes never expire | Created 2026-07-26; P1, `release:v0.9.0` | Security-sensitive and release-tagged, but still open with no visible fix PR. |
| **[Issue #9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)** — Knowledge graph lacks per-agent attribution | Created 2026-08-01; S0/P1 | Highest-severity open issue in the snapshot; directly affects data isolation. |
| **[Issue #10162](https://github.com/zeroclaw-labs/zeroclaw/issues/10162)** — Plugin install persists package before config seeding | Created 2026-08-20; in progress | Fresh-install path partially handled; already-installed reseed behavior still unresolved. |
| **[Issue #10373](https://github.com/zeroclaw-labs/zeroclaw/issues/10373)** — Share agent-rename recovery across CLI and gateway | Created 2026-08-26; P1 follow-up | Cross-surface consistency for durable state migrations. |
| **[Issue #10993](https://github.com/zeroclaw-labs/zeroclaw/issues/10993)** / **[PR #11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)** — Runtime composition boundary | Since 2026-09-20 / 2026-09-26; large PR open | Key v0.9.0 architecture work; XL PR needs careful review. |
| **[Issue #10997](https://github.com/zeroclaw-labs/zeroclaw/issues/10997)** and **[Issue #10998](https://github.com/zeroclaw-labs/zeroclaw/issues/10998)** — Plugin onboarding and minimal core tool set | Since 2026-09-20; blocked | Both are part of the same v0.9.0 release-train phase and remain blocked. |
| **[Issue #11416](https://github.com/zeroclaw-labs/zeroclaw/issues/11416)** — Slack thinking status regression | Since 2026-10-02; in progress | Visible channel UX regression; appears actively worked. |

**Bottom line:** Project throughput is healthy and v0.9.0 hardening is visibly progressing, but the open backlog still contains **three high-impact security/isolation items** — **[#9647](https://github.com/zeroclaw-labs/zeroclaw/issues/9647)**, **[#9394](https://github.com/zeroclaw-labs/zeroclaw/issues/9394)**, and **[#5982](https://github.com/zeroclaw-labs/zeroclaw/issues/5982)** — that should be prioritized before the project markets itself as safe for multi-tenant or shared-agent deployments.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*