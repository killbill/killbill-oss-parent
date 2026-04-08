# Task 2.2: Commons Non-Jooby Foundation Changes

## Objective

Implement the minimal Guice and servlet-facing changes in `killbill-commons`
that do not depend on the forked `jooby` module being migration-ready.

## Primary repo scope

- `killbill-commons`
- especially `skeleton`
- especially `metrics`

## Why this is separate

- The Jooby fork has its own execution lane and should not block every commons
  foundation change.
- Some dependency and code cleanup can likely happen before the servlet
  namespace transition in Jooby is finished.

## Deliverables

1. Define the minimal POM and dependency changes that are independent of the
   Jooby migration lane.
2. Implement code changes in commons that reduce future servlet migration
   friction without requiring downstream adoption.
3. Keep the patch series revertable within `killbill-commons`.
4. Document anything intentionally deferred to the Jooby-alignment or Jakarta
   adoption subtasks.


## Worktree constraint

- `killbill-commons/jooby` is under heavy edits in another session.
- Do not modify files under `killbill-commons/jooby` from this task.
- Keep this subtask focused on other commons modules such as `queue`,
  `metrics`, `skeleton`, and similar directories.

## Safe point

Do not merge in changes that require `killbill-platform` or `killbill` to move
at the same time. Avoid mixing in namespace migrations that are blocked on the
forked `jooby` module.

## Exit criteria

- Commons builds with the non-Jooby foundation changes applied.
- The changes are isolated enough that later Jooby and Jakarta work can layer
  on top.
- Remaining blockers are explicitly documented.
