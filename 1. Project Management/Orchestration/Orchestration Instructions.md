# Orchestration Instructions

How to run planner/executor sessions on this project. Roles, not models: the
**Planner** is a high-reasoning session (currently Fable 5) that audits,
designs, and writes handoffs; the **Executor** is a cheaper/faster session
(currently Sonnet) that picks up handoffs and implements them. Update the
model defaults here as they change — the process doesn't.

**When this applies:** The Planner role is opt-in, not automatic. Only act
as Planner when running on a high-reasoning model (Fable/Opus) AND the user
explicitly asks for an audit, plan, or handoff. A Fable/Opus session doing
ordinary work implements directly like any other session. Executor pickup of
Open handoffs, by contrast, is automatic for any session per CLAUDE.md.

-----

## Division of Labor: Planner Plans, Executor Executes

The Planner's job in an audit/review session is **brainstorm → design →
implementation planning → orchestration**. The Planner does not implement —
a separate Executor session picks up the resulting document later, with
**no shared context or memory of the planning session**.

This changes what "done" means for a Planner session: the deliverable is a
document a cold-started Executor can execute correctly and safely on its
own, not code. Use `Orchestration/Handoff Template.md` for every finding,
review outcome, or design meant for an Executor. Save filled copies to
`1. Project Management/Handoffs/<date>-<slug>.md` with `Status: Open` —
that folder is checked at every session start (see CLAUDE.md), so Open
handoffs get picked up automatically.

**The single most important template section is Stop Conditions.** A handoff
is read cold, by a different model, later. Anywhere the Planner would
normally use judgment mid-task must become an explicit, checkable condition
— or the Executor either guesses (bad) or stalls without knowing why (also
bad). Unresolved planning decisions belong in Stop Conditions, not left
implicit.

The Planner may still implement directly when explicitly asked ("audit and
fix" in one session) — the template is for when the roles are separate
sessions.

## Running Executors in Parallel

Multiple Executor sessions can run concurrently on separate handoffs when:

1. Neither handoff lists the other in `Depends on`
2. Their `Critical Files` lists don't overlap (`Parallel-safe: Yes`)

The Planner is responsible for setting these fields accurately — the
dispatch decision is made from the handoff headers alone. When in doubt,
mark `Parallel-safe: No` and run serially.

Each Executor works only its own handoff: execute, fill the Execution
Report, flip Status to `Done` (or `Blocked` on a stop condition), commit.
Do not touch other Open handoffs, even for "quick fixes" — log them as
Follow-ups instead.

Concurrent sessions can also message each other directly at run time — see
`Orchestration/Cross-Session Messaging.md` for the mechanics, addressing
rules, and permission boundaries. Handoffs remain the durable record; a
message is not one.

## Before Starting Any Session

Read these files in order before doing anything else:

1. `4. Technical Reference.md` — architecture, key files, conventions
2. `3. Work Packages.md` — what's done, what's pending
3. `5. Session Log.md` — recent history, decisions, context
4. For Executors: the assigned handoff in `Handoffs/`, fully, before writing any code

These files are ground truth. Don't re-derive what's already documented.
Project-specific key files and verification commands live in Technical
Reference — keep them current there, not here.

## Cutoff Protection Protocol

Usage limits can cut a session off mid-task with no warning — one instance
of the general environment-continuity problem covered in
`7. Environment Continuity.md` (checkpoint discipline, commit/push cadence,
recovery steps). That file's protocol applies to Planner and Executor
sessions exactly as written; the only addition here is Handoff-specific:

- For Executors recovering mid-handoff, the handoff's own Execution Report
  (partially filled or not) shows how far execution got, in addition to the
  Session Log checkpoints and git history.

## Audit Scope Guidance

When running a general audit (not a targeted task), look for:

- **Correctness bugs** — logic errors, edge cases, wrong assumptions
- **Security issues** — injection, CSRF gaps, missing auth checks, exposed data
- **Reuse / simplification** — duplicated logic that should be a shared helper
- **UX consistency** — patterns that differ from the rest of the app without reason
- **Dead code** — routes, templates, or functions no longer reachable

Report findings before fixing. Don't fix and commit speculatively — write
handoffs, or confirm with the user first unless the fix is trivially safe.

## Session End Routine

Both roles, at the end of every session:

1. Append a `COMPLETED` entry to `5. Session Log.md` — what was done, decisions, what's next
2. Update task statuses in `3. Work Packages.md`
3. Executors: ensure the handoff's Execution Report is filled and Status updated
4. Push all commits (follow the project's branch conventions in Technical Reference)
