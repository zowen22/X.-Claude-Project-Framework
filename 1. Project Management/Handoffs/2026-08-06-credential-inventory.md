# Handoff: Inventory credential/access locations in existing projects

*Status: `Open`*
*Created: 2026-08-06 — Planner: Sonnet session (X.-Claude-Project-Framework)*
*Priority: `Medium` — Effort: `M`*
*Depends on: `None`*
*Parallel-safe: `No` — touches the same `4. Technical Reference.md` file per
repo as `2026-08-06-rollout-environment-continuity.md` (different sections,
but same file). Run sequentially per repo, not concurrently with that
handoff, even though both are individually safe to run across different
repos.*

-----

## Goal

For each existing project, know-how for **where** each credential or
integration it depends on is managed — env var name, which platform's
dashboard, which secret manager — lives in that project's own
`4. Technical Reference.md`. So a new session in a fresh environment (which
starts with none of a prior session's granted access — see
`7. Environment Continuity.md`) can look in one place and know exactly what
needs to be re-provisioned, instead of re-discovering it by trial and error
or by asking the user to explain from scratch.

**This is a documentation task, not a credential-copying task.** The
deliverable is a map of *where things are managed*, never the things
themselves.

## Context

The user wants to be able to ask a fresh agent to spin up something like a
quick microsite and have it "just work" without a round of "what's our
Vercel setup, what are our API keys, where do I find X." The framework now
documents a default (Vercel, for quick deploys — see the template's new
`## Deployment` section in `4. Technical Reference.md`) and a place to
record credential locations (`## Access & Credentials`, same file). Neither
section exists yet in any project repo built before this change — this
handoff populates them for the known existing projects by scanning what's
already there.

The planning session that wrote this handoff has no access to the target
repos and has not seen their actual contents — everything below about what
might be found is a guess at likely patterns, not a confirmed inventory.

## Findings / Evidence

None yet — this handoff exists to produce the findings, not report them.
Known target repos (same list as the companion handoff, and equally
unconfirmed as complete): `zowen22/BetterGolfLeagueTracker`,
`zowen22/7.-Golf-Shot-Dispersion-Tool`.

## Scope

### In

- For each target repo, scan for **references to** credentials/integrations
  — not the values — in likely locations:
  - `.env.example`, `.env.sample`, `.env.template` (committed placeholder
    files that name required env vars without real values)
  - README or any existing docs mentioning setup/API keys/deployment
  - CI config (`.github/workflows/*.yml`) — job env vars, secret
    references (`${{ secrets.X }}`) name what's needed without exposing it
  - Deployment config (`vercel.json`, `netlify.toml`, etc.)
  - The project's own `4. Technical Reference.md` if it already documents
    any of this informally
- For each credential/integration found, add a row to that repo's
  `4. Technical Reference.md` → `## Access & Credentials` table (create the
  section if missing, matching the framework template's current version):
  what it's for, which platform/dashboard manages it, the expected env var
  name, and any notes on how to reprovision it.
- Add the `## Deployment` section to each repo's `4. Technical Reference.md`
  if missing, with the framework's default line, and fill in the table if
  the project's actual deployment setup is discoverable from the same scan
  (e.g. a `vercel.json` or a live Vercel-looking deploy hook confirms it's
  already on Vercel).

### Out

- Do not read, copy, print, log, or write any actual secret value anywhere
  — not into the target repo's files, not into this handoff's Execution
  Report, not into any tool output you'd leave lying around. If a `.env`
  file (not `.env.example`) or similar is present and actually committed to
  the repo (check: is it tracked by git, not just present in the working
  tree), that is itself a finding — see Stop Conditions, do not treat it as
  a normal data source to read from.
- Do not attempt to actually re-provision or test any credential/connector.
  This handoff documents where things are managed; it doesn't grant access.
- Do not guess at credentials/integrations that aren't evidenced by
  anything in the repo — if the user mentioned an integration verbally in a
  past session but it left no trace in the repo, that's not this handoff's
  job to reconstruct. Note it as unknown instead (see Definition of Done).

## Implementation Plan

1. For each target repo, clone/read its full current state.
2. Grep/search for the location patterns listed in Scope/In.
3. For each credential or integration reference found, determine: what it's
   for (from context — a comment, a var name, a CI job name), which
   platform manages it (Vercel, Supabase, GitHub, Shopify, etc. — infer
   from the var name or file it appeared in), and the exact env var name if
   there is one.
4. Write findings into that repo's `4. Technical Reference.md` under
   `## Access & Credentials` (and `## Deployment` if applicable), matching
   the framework template's table format. If the section already exists
   with content, add rows rather than overwriting.
5. Commit and push per that repo's own branch conventions, one commit per
   repo, message referencing this handoff's filename.
6. Fill in the Execution Report, one subsection per repo, listing what was
   found (locations and names only) and what's still unknown.

## Stop Conditions

- **A real secret value is discovered already committed to a repo** (a
  tracked `.env` file, an API key hardcoded in source, a token in a commit
  message, anything that isn't clearly a placeholder). Stop immediately.
  Do not read further into the file than needed to confirm it's a real
  value. Do not quote, copy, or reproduce the value anywhere, including in
  the Execution Report — describe the finding by location only ("`.env` is
  tracked in git at `path/to/.env`, appears to contain live values").
  Flag this to the user as a likely-needs-rotation security issue and mark
  the handoff `Blocked` until they've seen it. This overrides continuing to
  other repos — a leaked credential is higher priority than finishing the
  inventory.
- **Genuinely unsure whether something is a placeholder or a real value.**
  Treat it as real and stop/flag rather than guess wrong in either
  direction.
- **A repo has no discoverable trace of a given integration at all** (e.g.
  you know from user conversation it uses something, but nothing in the
  repo confirms it). Don't fabricate a row for it — list it under "Unknown
  / needs user input" in the Execution Report instead.

## Definition of Done

- [ ] Each in-scope, reachable repo's `4. Technical Reference.md` has an
      `## Access & Credentials` table listing every credential/integration
      found, by location and env var name only — no values
- [ ] Each in-scope, reachable repo's `4. Technical Reference.md` has a
      `## Deployment` section (default noted, actual setup filled in if
      discoverable)
- [ ] No secret values were written anywhere, at any point, including this
      handoff's own Execution Report
- [ ] Any repo where a real committed secret was found is flagged to the
      user and the handoff is `Blocked` pending their review — not silently
      worked around
- [ ] Execution Report filled in, one subsection per repo, including an
      "Unknown / needs user input" list for anything that couldn't be
      confirmed from the repo alone
- [ ] Status updated to `Done` (or `Blocked` per the Stop Condition above)

## Critical Files

| File | Why |
|------|-----|
| `zowen22/BetterGolfLeagueTracker` → `1. Project Management/4. Technical Reference.md` | Add/populate Access & Credentials, Deployment |
| `zowen22/7.-Golf-Shot-Dispersion-Tool` → `1. Project Management/4. Technical Reference.md` | Same, state unverified |
| (both repos) `.env.example`, CI workflow files, deployment config | Read-only scan sources — not modified |

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
