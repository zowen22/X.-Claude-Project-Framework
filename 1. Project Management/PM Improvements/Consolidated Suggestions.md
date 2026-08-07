# Consolidated PM Template Improvement Suggestions

*Rolls up every open (not-yet-processed) entry from each project's own
`1. Project Management/6. PM Template Improvement Suggestions.md` into one
place for review. Built and maintained per the process in `Process.md` in
this folder — don't edit ad hoc without also updating `Processed Log.md`.*

*Last built: 2026-08-07, covering: `1.-Autonomous-UAVs`, `11.-Resume`,
`12.-Zach-Owen.com`, `6.-Curriculum-Tool`, `8.-Magic-Band`,
`BetterGolfLeagueTracker`. (`1.-Autonomous-UAVs` and `6.-Curriculum-Tool`
have no `6. PM Template Improvement Suggestions.md` file yet — nothing to
pull from them.)*

*Note: while building this, a concurrent session pushed unrelated template
work (commits `77c3b1e`/`59a0d71`/`2e3a168`, 2026-08-05/06 — Environment
Continuity doc, credential inventory) that independently addressed part of
the substance of suggestions #1, #2, and #7 below before this doc was even
built. Their entries reflect the template as of that pull, not the original
raw suggestion text — see `Processed Log.md` for what those commits
actually did.*

-----

## Pending Review

### 1. Session End Routine step 6 still contradicts the new "push every increment" Rule

*Sources: `8.-Magic-Band` 2026-08-07 "Session Start Routine should require a
`git pull` before trusting local state"; `BetterGolfLeagueTracker`
2026-08-05 (suggestion #10) "CLAUDE.md should say pushes happen
automatically, not just commits."*

**Observation:** Both suggestions asked for the same underlying thing —
Claude, not the user, owns pull-before-trusting-state and
push-without-asking. A concurrent session (2026-08-06, commit `77c3b1e`)
independently added most of this: Session Start Routine step 3 now
establishes ground truth via `7. Environment Continuity.md`
(`git status`/`git branch`/`git log`, don't trust local state or the
Session Log blindly), and Rules gained "Commit and push after every
meaningful increment, not in one batch at session end." That resolves the
Magic-Band half and most of the BGLT half.

What's still stale: **Session End Routine step 6 still reads** "Remind user
to commit and push changes to GitHub" — passive, reminder-framed, implying
Claude waits for the user before pushing. That directly contradicts the
Rules line added one section below it in the same file, which says push
happens continuously through the session, not as a session-end ask.

**Suggested change:** Update Session End Routine step 6 to match the Rules
line — something like "Confirm all work from this session is committed and
pushed (should already be true per the Rules' commit-every-increment line —
this step is a final check, not the first push of the session)."

**Recommendation:** Ready to adopt — small, mechanical, closes a real
internal inconsistency in the current live CLAUDE.md, and matches confirmed
user preference (`feedback-always-push`, `feedback-git-pull-push-owned` in
Claude memory).

-----

### 2. Dashboard "next steps" heuristic — doc/code sync landed, deeper phase-aware fix still open

*Source: `8.-Magic-Band` 2026-08-07.*

**Observation:** Magic-Band's suggestion said CLAUDE.md documented "first 5
unchecked lines" while the dashboard's actual code took the last 5, and
asked for three things: (1) fix the slice direction, (2) make it
phase-aware rather than raw file-order, (3) only then update CLAUDE.md's
doc to match the *corrected* behavior. By the time this suggestion was
filed (2026-08-07), part (1) had **already** landed in the template two
days earlier (commit `c184f40`, 2026-08-05) — CLAUDE.md's Project Dashboard
Compatibility section already says "last 5," not "first 5." Magic-Band's
own project CLAUDE.md just hadn't been regenerated from the template since
then, so the session observed stale local doc, not a live drift.

**Still open:** parts (2) and (3) — the dashboard's parser is still raw
file-order (now "last 5" instead of "first 5"), which is exactly the naive
fix Magic-Band warned wasn't sufficient. A project with paused phases
interleaved with active ones can still surface the wrong "next steps."
Fixing this requires code changes in the separate `X.-Claude-Project-Dashboard`
repo, plus a companion CLAUDE.md rule that phase-status labels must be kept
accurate the moment the first item in a phase is checked (see original
suggestion for the exact proposed rule).

**Recommendation:** Not a CLAUDE.md wording task right now — needs a
decision on whether/when to take on the dashboard repo code change first.

-----

### 3. Add a Known Issues / Bug Log PM file

*Source: `BetterGolfLeagueTracker`, suggestion #3.*

**Observation:** Recurring bugs got re-diagnosed in a later session because
there was no issue tracker — the Session Log captures fixes but is
chronological, not queryable by topic.

**Suggested change:** New PM file `6. Known Issues.md` — note this slot is
now taken (`6.` is `PM Template Improvement Suggestions.md` and `7.` is now
`Environment Continuity.md` as of the 2026-08-06 template update), so this
would need to be `8.` or a differently-numbered slot. Lightweight table:
Issue | Status | Fixed In Session | Notes. Closed when fixed, marked
`[Resolved]`, never deleted.

**Recommendation:** Needs a decision — this is a new file added to every
project's Framework Structure, not a wording tweak. Worth confirming
scope/naming with the user before adopting.

-----

### 4. Cross-reference audit findings to Work Package task IDs

*Source: `BetterGolfLeagueTracker`, suggestion #4.*

**Observation:** Audit findings and WP tasks drift out of sync — no link
from an audit finding to the WP task that resolves it, so there's no clear
path to mark a finding resolved when its task closes.

**Suggested change to CLAUDE.md Rules:**
```
- Each audit finding should reference a WP task ID (e.g. "WP5.3-P2")
- Closing the WP task → mark the audit finding [Fixed] in the same session
```

**Recommendation:** Low-risk, mechanical rule addition — reasonable
candidate to adopt as-is.

-----

### 5. Archive old Session Log entries — rule not yet in template

*Source: `BetterGolfLeagueTracker`, suggestion #5 (implemented in BGLT
itself on 2026-08-03; the template-level rule was left pending).*

**Observation:** Session Log grows unbounded and becomes slow to read /
noisy after many sessions. BGLT already split its log at the 5-most-recent
mark into a `5. Session Log Archive.md`, but the CLAUDE.md rule that would
make this standard practice was never adopted into the template.

**Suggested addition to Rules:**
```
- Keep only the last 5 sessions in 5. Session Log.md
- Archive older entries to 5. Session Log Archive.md
- Archive at session end when log exceeds ~10 sessions
```

**Recommendation:** Proven in production on BGLT already — reasonable
candidate to adopt as-is.

-----

### 6. "What's Next" Session Log items must have a corresponding WP task

*Source: `BetterGolfLeagueTracker`, suggestion #6.*

**Observation:** iOS feature-parity tasks were called out in Session Log
"What's Next" notes but never added to Work Packages, so they became
invisible to the WP progress tracker.

**Suggested addition to Rules:**
```
- Any "What's Next" item in the Session Log must have a corresponding WP
  task created before the session is marked COMPLETED
- Cross-platform work (iOS, web, API) should be explicit separate tasks,
  not bundled
```

**Recommendation:** The first line generalizes cleanly to any project. The
second line (iOS/web/API) is BGLT-specific phrasing — would want to
generalize to "components" language consistent with the existing Work
Package Conventions section before adopting.

-----

### 7. Framework Structure section — immediate staleness fixed, generic future-proofing still open

*Source: `BetterGolfLeagueTracker`, suggestion #9 (2026-07-31 PM docs
audit, finding 1.1).*

**Observation:** The original complaint was that CLAUDE.md's "Framework
Structure" section hardcoded exactly 5 numbered files while real projects
(BGLT itself) had grown to 7 files plus extra folders, all undocumented
there. As of the 2026-08-06 template update (commit `77c3b1e`), the
Framework Structure section now explicitly lists items 6 and 7 — the
immediate case is fixed.

**Still open:** the general guidance BGLT proposed — that projects may grow
*further* beyond whatever the template currently lists, and that's expected
rather than drift — was never added. The same staleness will recur the
first time any project needs an 8th file (e.g. suggestion #3 above, if
adopted, would immediately trigger it).

**Suggested addition to Framework Structure** (original wording, still
applicable):
```
Projects may grow additional numbered files beyond this list as real needs
arise (e.g. a template-improvement log, a feature-parity tracker) — see this
project's own numbered files above the base list, if any, and the sections
elsewhere in this document that reference them by name. Supporting folders
alongside `1. Project Management/` (Audits/, Plans/, Handoffs/,
Orchestration/, Artifacts/) are documented in their own CLAUDE.md sections
and/or their own README.md, not repeated here.
```

**Recommendation:** Lower urgency now that the immediate drift is fixed,
but still worth adding — it's the difference between fixing this once more
and fixing it permanently. Consider bundling with #3 if that's adopted,
since a new numbered file would trigger the same gap again immediately.

-----
