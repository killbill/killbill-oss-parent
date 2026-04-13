# Task 2.2: Commons Non-Jooby Foundation Changes

## Objective

Implement the minimal non-Jooby Guice and servlet-facing groundwork in
`killbill-commons` that aligns with the Jooby Phase 2 sequence:
keep source on `javax.*`, prepare for JDK 17, and prepare for the Guice 6
bridge step without starting the Jakarta namespace migration yet.

## Primary repo scope

- `killbill-commons`
- especially `skeleton`
- especially `metrics`

## Why this is separate

- The Jooby fork has its own execution lane and should not block every commons
  foundation change.
- The Jooby plans currently sequence the work as:
  - re-establish a stable pre-Jakarta baseline
  - move to JDK 17
  - move to Guice 6.0 as the bridge version
  - only then start the servlet namespace migration
- This subtask is the commons-local slice of that bridge phase, excluding the
  `killbill-commons/jooby` directory.

## Deliverables

1. Define the minimal commons changes that can land before any
   `javax.servlet` to `jakarta.servlet` source rewrite.
2. Separate pure cleanup work from changes that are specifically needed for:
   - JDK 17 readiness
   - Guice 6.0 / `guice-servlet` 6.0 bridge adoption
3. Implement only the commons-side changes that are outside
   `killbill-commons/jooby` and do not require the Jakarta namespace switch.
4. Keep the patch series revertable within `killbill-commons`.
5. Document anything intentionally deferred to the Jooby-alignment or Jakarta
   adoption subtasks.


## Worktree constraint

- `killbill-commons/jooby` is under heavy edits in another session.
- Do not modify files under `killbill-commons/jooby` from this task.
- Keep this subtask focused on other commons modules such as `queue`,
  `metrics`, `skeleton`, and similar directories.

## Safe point

Do not merge in changes that require `killbill-platform` or `killbill` to move
at the same time. Avoid mixing in namespace migrations that are blocked on the
forked `jooby` module. This task should stop before any bulk `javax.*` to
`jakarta.*` source rewrite in commons.

## Exit criteria

- Commons builds with the non-Jooby foundation changes applied.
- The completed changes are clearly identified as one of:
  - pre-JDK-17 cleanup
  - JDK-17-readiness work
  - Guice-6-bridge-readiness work
- No completed change in this subtask requires a servlet namespace rewrite.
- The changes are isolated enough that later Jooby and Jakarta work can layer
  on top.
- Remaining blockers are explicitly documented.


## Completion status

Completed, with the execution eventually going beyond the original bridge-only
framing.

Actual end state reached in `killbill-commons`:

- Java baseline moved to `21`
- Guice moved to `7.0.0`
- commons-side non-Jooby groundwork was completed
- the repository is now Jakarta-ready for this lane

This means the subtask is complete even though the final commons result went
past the earlier "prepare for Guice 6 bridge" wording.
