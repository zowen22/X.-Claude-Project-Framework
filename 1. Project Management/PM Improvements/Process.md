# PM Improvements Check — Process

*What to do when the user asks for a "PM improvements check" (or equivalent —
"see what's come out of the improvement suggestions," "check for template
updates," etc). This file lives only in the template repo
(`X.-Claude-Project-Framework`) — it is not part of what gets generated into
individual project instances, and CLAUDE.md doesn't need to reference it.*

## Why this exists

Every project instance logs PM friction points into its own
`1. Project Management/6. PM Template Improvement Suggestions.md` (per the
"PM Improvement Suggestions" rule in CLAUDE.md). Those files are **not
wiped** when a suggestion gets incorporated into the template — some get a
manual "Resolution note" appended in place (see BetterGolfLeagueTracker's
file for the convention), but nothing guarantees that, and it's not this
process's job to enforce it. Re-scanning every project's raw file on every
check would otherwise mean re-surfacing the same already-decided suggestion
every single time. `Processed Log.md` in this folder is what breaks that
loop — it's the durable record of "already looked at this," independent of
whatever state the source repo's own file happens to be in.

The template also changes for reasons that have nothing to do with a
suggestion file — a session working directly in this repo can add or edit
CLAUDE.md sections on its own initiative (this happened 2026-08-05/06:
Environment Continuity doc, dashboard doc sync, Framework Structure fix —
none of those came from a `6. PM Template Improvement Suggestions.md`
entry). Those changes can still independently satisfy — fully or partly —
an open suggestion filed elsewhere. Don't assume "no one has processed this
suggestion yet" means "nothing has changed that's relevant to it" — always
diff a suggestion's *ask* against the template's *current* CLAUDE.md, not
against what you remember the template saying last time.

## Steps

1. **Read the template's current CLAUDE.md first**, before reading any
   source suggestion files. You need the actual current state to judge
   whether a suggestion is already (fully or partly) satisfied, independent
   of what `Processed Log.md` says — the log records past decisions, not a
   live diff.

2. **Enumerate source files.** For every repo under `~/projects/` except
   `X.-Claude-Project-Framework` itself, look for
   `1. Project Management/6. PM Template Improvement Suggestions.md`. Not
   every repo has one yet (as of 2026-08-07, `1.-Autonomous-UAVs` and
   `6.-Curriculum-Tool` don't) — skip silently, that's expected for newer or
   less-active projects, not an error.

3. **Read every entry**, including ones the source file already marks
   `[Resolved ...]` or with a resolution note — those notes are informal and
   per-project, not guaranteed to match what's actually in the template, so
   don't trust them as the source of truth. The dedup check is step 4, not
   this.

4. **Dedup against `Processed Log.md`.** For each entry, form its key
   (`[Source Repo] YYYY-MM-DD — Short Title`, taken from the entry's own
   heading) and check whether that key already appears in `Processed Log.md`
   under "Promoted to template" or "Resolved outside the template." If it
   does **and** step 1's read of current CLAUDE.md confirms the ask is fully
   satisfied, skip it. If the log entry says "partially addressed" (or your
   own read of current CLAUDE.md shows only part of the ask landed), don't
   skip — carry the residual into `Consolidated Suggestions.md` as its own
   entry, explicitly framed as a residual of the logged entry (see items #1
   and #7 there as of 2026-08-07 for the pattern), not as a fresh unrelated
   suggestion.

5. **Add genuinely new (or residual) entries to `Consolidated Suggestions.md`**
   under "Pending Review," one subsection per suggestion (or merged, if two
   sources describe the same underlying gap — e.g. item #1 in that file
   merges a Magic-Band and a BGLT suggestion that were really the same issue
   from two angles). Include: source repo + date, the observation, the
   suggested change, and a **Recommendation** line — your judgment call on
   whether this is safe to adopt as-is, needs generalizing away from
   project-specific phrasing, needs a decision from the user, or depends on
   a fix elsewhere first (e.g. item #2, which needs a dashboard repo code
   fix before its CLAUDE.md wording can be corrected).

6. **Surface it to the user** — don't silently auto-adopt suggestions into
   the template CLAUDE.md, even ones marked "ready to adopt" in your
   recommendation. Cross-session memory (feedback/reference-type entries)
   can make a suggestion obviously correct, but the template feeds every
   future project, so changes to it warrant an explicit go-ahead, not an
   assumption.

7. **On approval, apply the change:**
   - Edit `X.-Claude-Project-Framework/CLAUDE.md` directly (this is the
     canonical source — project instances get regenerated from it, not the
     other way around).
   - Move the suggestion out of "Pending Review" in
     `Consolidated Suggestions.md`.
   - Add or update the corresponding entry in `Processed Log.md` under
     "Promoted to template" (or "Resolved outside the template" if the
     decision was "don't put this in CLAUDE.md, handle it another way" —
     e.g. via memory, like the MCP-connector suggestion). If it's a full
     resolution of a previously "partially addressed" entry, update that
     entry's Outcome rather than adding a duplicate.
   - **Isolate before editing.** This repo gets concurrent pushes from other
     sessions (mobile/Cowork, other background jobs) — confirmed to happen
     in practice, not theoretical. `git fetch` and check
     `origin/main` before assuming local state is current, and do the edit
     in a worktree/branch rather than directly against a checkout that might
     be behind. If a push is rejected, fetch, re-read what changed (it may
     have already addressed part of what you're about to commit — see the
     "Why this exists" note above), reconcile, and re-push. Don't force-push.
   - Commit and push the template repo (per standing preference: push
     immediately, don't wait for separate confirmation once the user has
     approved the underlying change).

8. **Existing project instances are not auto-updated.** A template CLAUDE.md
   change does not retroactively propagate to `11.-Resume`,
   `8.-Magic-Band`, etc. — their CLAUDE.md files are independent copies.
   Propagating an update to a live project is a separate, explicit action;
   flag it as available but don't do it as part of this process unless
   asked.

## What "done" looks like for one check

`Consolidated Suggestions.md` contains only suggestions still awaiting a
decision (including residuals of partially-addressed ones); everything
fully decided (adopted or not) has moved into `Processed Log.md` with an
accurate Outcome; the template repo is committed and pushed if anything
changed.
