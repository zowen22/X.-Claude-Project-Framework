# Handoff: Roll out Environment Continuity update to existing projects

*Status: `Open`*
*Created: 2026-08-06 — Planner: Sonnet session (X.-Claude-Project-Framework)*
*Priority: `Medium` — Effort: `M`*
*Depends on: `None`*
*Parallel-safe: `Yes` — each target repo is independent; safe to dispatch one executor per repo*

-----

## Goal

Every existing project built from this framework ends up with the same
Environment Continuity protocol the framework repo now has — without
clobbering anything that repo has since customized. The framework repo
(`zowen22/X.-Claude-Project-Framework`, `main` branch) is the source of
truth for the new/changed content; read it live rather than trusting any
copy pasted into this handoff, since it may keep evolving after this
document is written.

## Context

The framework added `1. Project Management/7. Environment Continuity.md` —
a protocol for sessions whose local environment (ephemeral container,
mobile Cowork session, different machine) may not persist state between
runs, so only git-committed work is durable. That triggered several
cascading edits to `CLAUDE.md` and other framework files (see Implementation
Plan). None of that has been synced to any project repo that was cloned
from this template before this change landed.

The planning session that wrote this handoff had GitHub access scoped only
to the framework repo — it could not read or write the target repos, so
none of the specifics below (current CLAUDE.md content, whether file 6
exists, how customized Technical Reference is) have been verified against
the actual target repos. Treat every "if X, do Y" below as genuinely
conditional, not a formality.

Two target repos are confirmed to exist and use this framework:
`zowen22/BetterGolfLeagueTracker` and `zowen22/7.-Golf-Shot-Dispersion-Tool`.
This is very likely not the complete list — see Stop Conditions.

## Findings / Evidence

- `BetterGolfLeagueTracker`'s `CLAUDE.md` was, as of the last time it was
  read (mid-way through this framework's development), essentially what
  became the framework's current `CLAUDE.md` base — it was adopted as
  source-of-truth over a divergent `main` and then extended twice more
  since (Orchestration Handoffs section, dashboard `first 5`→`last 5` fix,
  proactive PM Improvement Suggestions wording, Planner opt-in
  clarification, and now this Environment Continuity change). It is likely
  the closest of the two to current, but is still several commits behind.
- `BetterGolfLeagueTracker` has its own `1. Project Management/Fable Audit/`
  folder (`Handoff Template.md`, `Session Instructions.md`) — an earlier,
  project-local version of what the framework now calls
  `Orchestration/Orchestration Instructions.md` + `Handoff Template.md`.
  The user previously declined to migrate that repo onto the new
  `Orchestration/` structure. **Do not touch it** — see Scope/Out.
- `7.-Golf-Shot-Dispersion-Tool`'s current state is unconfirmed. It was
  created from this repo's GitHub template feature; whether that actually
  populated `1. Project Management/` and `CLAUDE.md` was never verified in
  the planning session (repo access was denied when the check was
  attempted). Read its actual current state before assuming anything.

## Scope

### In

- Add `1. Project Management/7. Environment Continuity.md` to each target
  repo, verbatim from the framework repo's current version, if not already
  present.
- Add `1. Project Management/6. PM Template Improvement Suggestions.md` to
  each target repo (verbatim stub from the framework), if not already
  present — `CLAUDE.md`'s `PM Improvement Suggestions` section references
  this path, and a repo without the file has a dangling reference.
- Sync these specific `CLAUDE.md` changes into each target repo, reconciled
  against whatever that repo's `CLAUDE.md` already has (see Implementation
  Plan step 3 for exactly what changed):
  - `Framework Structure` list gains items 6 and 7
  - `Session Start Routine` gains the git-ground-truth step
  - `Rules` gains the commit-often line
- If the target repo has a `4. Technical Reference.md` with an
  `Environment & Setup` section matching the template's placeholder text,
  append the one-line disambiguation pointer to
  `7. Environment Continuity.md`. If that section has been substantially
  rewritten for the project, append the pointer as an additional sentence
  rather than replacing anything.
- If the target repo has an `Orchestration/Orchestration Instructions.md`
  (i.e., it's already on the new orchestration structure, not the old
  Fable Audit one), apply the same Cutoff Protection Protocol trim/
  cross-reference made in the framework repo. Skip this for any repo still
  on the old Fable Audit structure (see Out).

### Out

- Do not migrate `BetterGolfLeagueTracker`'s `Fable Audit/` folder to the
  `Orchestration/` structure, or touch its contents. That migration was
  explicitly declined by the user in an earlier session and is a separate
  decision.
- Do not touch `Cowork Instructions - Paste.md` in any target repo. That
  file is a paste-target for the Cowork project-instructions field, not
  confirmed to exist as a committed file in any project repo — don't create
  it somewhere it wasn't already.
- Do not silently expand the list of target repos beyond what's confirmed
  in this document or by the user directly — see Stop Conditions.
- Do not resolve a genuine content conflict (a target repo's version says
  something the framework's doesn't, in a way that looks intentional, not
  just "behind") by guessing which wins — that's a Stop Condition.

## Implementation Plan

1. Read `zowen22/X.-Claude-Project-Framework`, `main` branch, current
   versions of: `CLAUDE.md`, `1. Project Management/7. Environment
   Continuity.md`, `1. Project Management/6. PM Template Improvement
   Suggestions.md`, `1. Project Management/4. Technical Reference.md`, and
   `1. Project Management/Orchestration/Orchestration Instructions.md`.
   These are the versions to sync from — not any copy in this handoff.
2. For each target repo: read its current `CLAUDE.md` and
   `1. Project Management/` contents in full before changing anything.
3. Diff the target's `CLAUDE.md` against the framework's. The specific
   deltas this handoff is propagating (confirm they're actually missing
   before adding — the repo may already have some of this if it was synced
   partway at some point):
   - `## Framework Structure` list: add lines 6 (`PM Template Improvement
     Suggestions.md`) and 7 (`Environment Continuity.md`) if missing
   - `## Session Start Routine`: add the step "Establish ground truth per
     `7. Environment Continuity.md` — check `git status`/`git branch`/`git
     log`, don't assume local state matches what the Session Log claims"
     if missing
   - `## Rules`: add "Commit and push after every meaningful increment, not
     in one batch at session end — see `7. Environment Continuity.md`" if
     missing
   Apply only the deltas that are actually absent — don't overwrite parts
   of the target's `CLAUDE.md` that already match or that diverge for a
   reason you can't verify (see Stop Conditions).
4. Add `7. Environment Continuity.md` and (if missing) `6. PM Template
   Improvement Suggestions.md` to the target repo, verbatim from the
   framework.
5. Update the target's `4. Technical Reference.md` Environment & Setup
   section per Scope/In, if applicable.
6. If the target has migrated to the `Orchestration/` structure, update its
   `Orchestration Instructions.md` Cutoff Protection Protocol section to
   match the framework's cross-reference trim. Otherwise skip (see Out).
7. Commit each target repo's changes as one commit (message: reference this
   handoff's filename), push to that repo's default branch per its own
   branch conventions.
8. Fill in the Execution Report below, one subsection per repo touched.

## Stop Conditions

- **Repo list unconfirmed.** Before starting, confirm with the user whether
  `BetterGolfLeagueTracker` and `7.-Golf-Shot-Dispersion-Tool` are the
  complete list of repos using this framework, or whether others exist.
  Don't discover repos by guessing names.
- **No GitHub access to a target repo.** If the executor's session can't
  reach a listed repo (access not granted, repo not found, private with no
  auth), stop for that repo specifically, note it in the Execution Report,
  and continue with the others rather than blocking the whole handoff.
- **`7.-Golf-Shot-Dispersion-Tool` has no `1. Project Management/` folder
  at all.** This would mean the GitHub template copy never actually ran.
  Don't try to silently full-bootstrap the project from scratch as part of
  this handoff — that's a bigger action than "sync one new doc." Stop, note
  it, and ask the user whether to bootstrap it fully (separate handoff) or
  skip.
- **A target repo's relevant section contradicts the framework's**, not
  just lags behind it (e.g., a `Rules` line that says the opposite of the
  framework's, a Session Start Routine with a different step order that
  looks deliberate). Don't force-merge or pick a side — stop and ask the
  user which is source of truth for that repo, the same way the framework
  repo's own CLAUDE.md conflict was resolved earlier (golf-league version
  vs. divergent `main`, user chose explicitly).
- **Any file this handoff expects to exist as a clean insertion point is
  missing or restructured** beyond recognition (e.g., no `## Rules` heading
  at all). Stop for that repo rather than guessing where content should go.

## Definition of Done

- [ ] User has confirmed the full list of target repos (or explicitly
      approved proceeding with just the two known ones)
- [ ] Each in-scope, reachable repo has `7. Environment Continuity.md`
      present and matching the framework's current version
- [ ] Each in-scope, reachable repo has `6. PM Template Improvement
      Suggestions.md` present
- [ ] Each in-scope, reachable repo's `CLAUDE.md` has the three deltas from
      Implementation Plan step 3, or a documented reason each was skipped
- [ ] `BetterGolfLeagueTracker`'s `Fable Audit/` folder is untouched
- [ ] Execution Report below is filled in, one subsection per repo actually
      touched, including any repos skipped and why
- [ ] Status updated to `Done` (or `Blocked` if a Stop Condition was hit
      and not resolvable without the user)

## Critical Files

| File | Why |
|------|-----|
| `zowen22/BetterGolfLeagueTracker` → `CLAUDE.md` | Sync the 3 deltas |
| `zowen22/BetterGolfLeagueTracker` → `1. Project Management/7. Environment Continuity.md` | New file |
| `zowen22/BetterGolfLeagueTracker` → `1. Project Management/6. PM Template Improvement Suggestions.md` | New file, if missing |
| `zowen22/BetterGolfLeagueTracker` → `1. Project Management/4. Technical Reference.md` | Add disambiguation pointer if applicable |
| `zowen22/7.-Golf-Shot-Dispersion-Tool` → same paths as above | Same treatment, state unverified |

-----

## Execution Report

*Filled in by the executor. This closes the loop — the user or a later
planner session reviews the work from here, not from diff archaeology.*

*Executed: [date] — Executor: [model/session]*

### What Was Done

- 

### Deviations from Plan

- 

### Follow-ups Discovered

- 
