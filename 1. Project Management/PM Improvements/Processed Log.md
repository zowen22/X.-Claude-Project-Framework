# PM Improvement Suggestions — Processed Log

*Every suggestion that has been reviewed at least once, across every project's
`6. PM Template Improvement Suggestions.md` file, keyed so a future scan can
skip it even if the source project's own file was never wiped or annotated.
See `Process.md` in this folder for how this log gets used and updated.*

**Key format:** `[Source Repo] YYYY-MM-DD — Short Title` (date and title
copied from the source suggestion's own heading).

-----

## Promoted to template

### [BetterGolfLeagueTracker] — Clarify Session Start Routine
**Outcome:** Superseded, not adopted verbatim. Original suggestion proposed
specific replacement wording; the live CLAUDE.md Session Start Routine
("Read `3. Work Packages.md`, `5. Session Log.md`, and any open Audits/,
Plans/, or Handoffs/ files...") resolves the same ambiguity ("check if
shared" was the friction) with different wording. Per the source repo's own
2026-07-31 PM docs audit, finding 2.7. Marked `[Resolved 2026-08-03]` in
source.

### [BetterGolfLeagueTracker] — Add Memory vs PM Files Rule
**Outcome:** Promoted verbatim. The `## Memory vs PM Files` section now
exists in the live template CLAUDE.md exactly as proposed. Marked
`[Resolved 2026-08-03]` in source.

### [BetterGolfLeagueTracker] — Add PM Improvement Suggestion Process to CLAUDE.md
**Outcome:** Promoted verbatim. The `## PM Improvement Suggestions` section
now exists in the live template CLAUDE.md exactly as proposed — this is the
rule that tells every project instance where to log new suggestions. Marked
`[Resolved 2026-08-03]` in source. Note: as of 2026-08-05 (commit
`c184f40`) the wording was further broadened from reactive ("if you
observe") to proactive ("don't wait to be asked... actively watch for
gaps") — an independent follow-on change, not itself sourced from a
suggestion file.

### [8.-Magic-Band] 2026-08-07 — Session Start Routine should require a `git pull` before trusting local state
**Outcome:** Independently addressed, not via this exact suggestion.
Commit `77c3b1e` (2026-08-06 — one day *before* this suggestion was filed)
added Session Start Routine step 3 ("Establish ground truth per
`7. Environment Continuity.md`") plus the new `7. Environment Continuity.md`
file itself, which covers the same ground (don't trust local state, check
`git status`/`branch`/`log` before doing anything). Different mechanism
than the literal "add a `git fetch` step" the suggestion proposed, but same
outcome. See `Consolidated Suggestions.md` #1 for the one residual gap this
didn't close (Session End Routine step 6 wording).

### [BetterGolfLeagueTracker] 2026-08-05 — CLAUDE.md should say pushes happen automatically, not just commits (suggestion #10)
**Outcome:** Partially addressed independently. Commit `77c3b1e`
(2026-08-06) added a Rules line — "Commit and push after every meaningful
increment, not in one batch at session end" — which covers the substance of
this suggestion. **Not fully closed**: Session End Routine step 6 itself
still reads "Remind user to commit and push changes to GitHub," the exact
wording this suggestion asked to replace. See `Consolidated Suggestions.md`
#1 — logged there as the residual, don't re-file this as new.

### [BetterGolfLeagueTracker] 2026-07-31 — Framework Structure list stale relative to CLAUDE.md's own sections (suggestion #9)
**Outcome:** Partially addressed independently. Commit `77c3b1e`
(2026-08-06) updated Framework Structure to explicitly list items 6 and 7,
closing the specific staleness this suggestion flagged at the time. **Not
fully closed**: the generic "projects may grow further" guidance proposed
was never added, so the same drift will recur at item 8. See
`Consolidated Suggestions.md` #7 — logged there as the residual, don't
re-file this as new.

-----

## Resolved outside the template (not a CLAUDE.md change)

### [BetterGolfLeagueTracker] 2026-08-03 — Check available MCP connectors before declaring a task blocked
**Outcome:** Not promoted to CLAUDE.md. The source suggestion itself judged
this a general Claude Code tool-discovery habit, not something specific to
PM structure, and saved it as a durable cross-session memory instead
(`feedback-check-connectors-before-blocked` in Claude memory) — more likely
to actually get read than a rule buried in a per-project CLAUDE.md. No
template action needed; do not re-flag if this suggestion resurfaces
elsewhere with the same substance.

-----

## Pending review

*Everything not listed above (or listed above with a "still open" residual)
is still open. See `Consolidated Suggestions.md` in this folder for the
current list — as of 2026-08-07 that's items #1–#7 there, sourced from
`8.-Magic-Band` and `BetterGolfLeagueTracker`. Items #1, #2, and #7 there
are residuals of suggestions logged as "Promoted" above — read both files
together, not `Consolidated Suggestions.md` alone, when doing a check.*

-----
