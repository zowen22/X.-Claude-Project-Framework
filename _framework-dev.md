# Framework Dev Notes

Tracks progress and context for building and improving this repo itself.

-----

## Status
`In Progress`

## What This Repo Is
A reusable Claude project template. Clone it for any new project, fill in `1. Project Management/`, paste `Cowork Instructions - Paste.md` into the Claude project instructions field.

## Role
Claude is the manager of this PM system — the framework, its conventions, and how they interact with the Project Dashboard across all projects. That means proactively catching drift (e.g. framework docs falling out of sync with dashboard behavior), not just executing individual asks in isolation.

## Current State
- [x] Initial file structure created
- [x] All 5 project management templates in place
- [x] `CLAUDE.md` with session routines and rules
- [x] `Cowork Instructions - Paste.md` for Claude project instructions field
- [x] CLAUDE.md upgraded from BetterGolfLeagueTracker (Audits, WP conventions, Memory vs PM Files, etc.)
- [x] Branched Plans convention (Plans/ folder)
- [x] Orchestration workflow (Planner/Executor handoffs, Handoffs/ folder)
- [x] `6. PM Template Improvement Suggestions.md` stub
- [x] Project Dashboard compatibility contract in CLAUDE.md
- [x] `7. Environment Continuity.md` + cascading edits
- [x] Deployment default (Vercel) + Access & Credentials convention in Technical Reference template

## Open Handoffs (this repo's own Handoffs/ folder)
- `2026-08-06-rollout-environment-continuity.md` — sync Environment Continuity changes to BetterGolfLeagueTracker and 7.-Golf-Shot-Dispersion-Tool
- `2026-08-06-credential-inventory.md` — inventory where credentials live in those same repos (locations only, never values); run after/sequentially with the handoff above, not in parallel
- Both need an executor session with actual GitHub access to those repos, which this session never had

## Conventions Decided

- Work Packages task items: one clear verb phrase, ≤60 chars — detail goes in Technical Reference or Session Log

## Backlog / Future Improvements

- [ ] Consider adding a `.github/` folder with issue/PR templates
- [ ] Explore making this a proper GitHub template repo (repo settings)
- [ ] Evaluate whether `4. Technical Reference.md` needs more scaffolding
- [ ] Consider a quick-start README explaining how to use the template

## Notes

- The `.txt` extension issue on some files was a Claude Code download quirk — source files are all `.md`
- `CLAUDE.md` serves double duty: Claude Code reads it automatically at session start, and it documents the framework for humans
- Initial session was run from mobile — no local git clone exists; commits exist only on remote
- Keep this file lean — it's a dev scratchpad, not a full project tracker

-----

## Session History

### 2026-06-11 - Initial setup
Created full file structure from uploaded templates. Added `_framework-dev.md` (this file) for meta-tracking.

### 2026-07-04 - Orchestration workflow added (COMPLETED)
Ported "Fable Audit" docs from BetterGolfLeagueTracker as `Orchestration/` (model-agnostic naming: Planner/Executor roles). Improvements over golf league originals: handoff lifecycle (Open→In Progress→Done/Blocked), Execution Report section to close the loop, Depends on/Parallel-safe headers for dispatching concurrent executors, runnable verification in Definition of Done, dedicated `Handoffs/` folder (separate from Audits) checked at session start. Added stub `6. PM Template Improvement Suggestions.md` that CLAUDE.md already referenced.

Decisions: Planner role is opt-in only — requires Fable/Opus AND explicit user request for audit/plan/handoff; executor pickup of Open handoffs is automatic for any session. Earlier same day: golf league CLAUDE.md adopted as source of truth over main's divergent version (force-pushed); Branched Plans section merged in.

Next: golf league repo still uses old `Fable Audit/` structure — user declined migration for now. Handoff Template.md was rebuilt from a condensed fetch, not verbatim — user should skim to confirm nothing was lost.

### 2026-08-03 - Dashboard compatibility contract (COMPLETED)

Added Project Dashboard Compatibility section to CLAUDE.md (user-authored, verbatim): dashboard at X.-Claude-Project-Dashboard polls `2. Project Overview.md` (Status value, Summary prose) and `3. Work Packages.md` (checkbox counts, first 5 unchecked as next steps) from public repos' main branch; new repos must be added to the PROJECTS array in the dashboard's index.html by hand.

Open issue: dashboard requires public repos, but recent project repos were created private — flip to public or add token-based fetch to the dashboard.

### 2026-08-05 - Dashboard sync + PM improvement culture (COMPLETED)

Corrected Project Dashboard Compatibility bullet: dashboard's `parseNextSteps` was changed to pull the **last** 5 unchecked Work Packages lines (was first 5), so active/current tasks surface instead of stale early-phase ones — CLAUDE.md now matches. Broadened PM Improvement Suggestions from reactive to proactive: agents should watch for PM structure gaps as standard practice while executing, not just note them incidentally. Added a **Role** section to this file establishing Claude as manager of the PM system across the framework and its interaction with the dashboard — catch drift proactively, not just execute asks in isolation.

Also fixed the `## Status` placeholder in `2. Project Overview.md` (was three slash-separated values on one line, which the dashboard parses as "Unknown"; now a single `Planning` value with guidance below it).

### 2026-08-06 - Environment Continuity + rollout handoff (COMPLETED)

Added `1. Project Management/7. Environment Continuity.md`: protocol for sessions whose local environment (ephemeral container, mobile Cowork, different machine) may not persist state between runs — only git-committed work is durable. Cascading edits: `CLAUDE.md` (Framework Structure now lists files 6 and 7 — 6 was a pre-existing dangling reference, never actually listed; Session Start Routine checks git ground truth; Rules gets an explicit commit-often line), `4. Technical Reference.md` (disambiguates its existing "Environment & Setup" section from the new doc), `Orchestration Instructions.md` (Cutoff Protection Protocol trimmed to cross-reference the general doc instead of duplicating it), `Cowork Instructions - Paste.md` (structure list synced + condensed rule, since mobile sessions are most exposed to this problem).

Blocked on rollout to existing projects: this session's GitHub access is scoped to the framework repo only, no `list_repos`/`add_repo` tool available to expand it. Wrote `Handoffs/2026-08-06-rollout-environment-continuity.md` — a full context-free handoff for an executor session with access to `BetterGolfLeagueTracker` and `7.-Golf-Shot-Dispersion-Tool` (the two known target repos; list not confirmed complete). User will start a new agent with the right access to pick it up.

Also noticed but did not act on: `Cowork Instructions - Paste.md` had drifted significantly behind `CLAUDE.md` even before today (missing Open Audits, Branched Plans, Orchestration Handoffs, WP Conventions, Session Log Format, Dashboard Compatibility, Memory vs PM Files sections) — only today's specific delta got synced into it. Full resync is a candidate for `6. PM Template Improvement Suggestions.md` or a future session.

### 2026-08-06 (cont'd) - Deployment default + credential location convention (COMPLETED)

Added `## Deployment` (default: Vercel, for quick microsites — user's call, delegated to my judgment) and `## Access & Credentials` (document *where* a credential is managed, never the value) sections to the `4. Technical Reference.md` template. Added a Rules line to CLAUDE.md: never commit actual secret values, document the location instead.

Wrote a second handoff, `Handoffs/2026-08-06-credential-inventory.md`, for the same executor session to scan existing projects for credential/integration references (env files, CI config, deploy config — locations and names only, never values) and populate the new sections per repo. Marked explicitly Parallel-safe: No against the environment-continuity handoff since both touch `4. Technical Reference.md` in the same repos — run sequentially. Heavy Stop Conditions on this one: if a real secret is found already committed anywhere, stop and flag to the user as a likely-needs-rotation issue rather than working around it silently.
