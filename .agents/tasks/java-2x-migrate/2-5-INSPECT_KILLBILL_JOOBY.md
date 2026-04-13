# Task 2.5: Inspect Kill Bill Jooby

## Objective

Perform a read-only inspection of `killbill-commons/jooby` to understand the
dependency, import, and servlet-surface impact of the forked Jooby lane on the
Java 2x migration.

## Primary repo scope

- `killbill-commons/jooby`
- read-only inspection only

## Why this exists

- `killbill-commons/jooby` is under heavy edits in another session and should
  not be modified from this task lane.
- The rest of the commons migration still needs verified information about the
  Jooby fork dependencies and API surface.
- This task provides a safe workaround: inspect and document, but do not edit.

## Deliverables

1. Inventory Jooby dependencies from POM files and any relevant module wiring.
2. Inventory important import surfaces, especially:
   - `javax.servlet`
   - `jakarta.servlet`
   - Guice and Guice servlet
   - Jersey / Jetty / HK2 touch points where relevant
3. Identify which commons modules outside `jooby` depend on Jooby-owned types
   or behavior.
4. Document blockers, compatibility assumptions, and expected handoff points
   for later subtasks.

## Safe point

This task is inspection and documentation only. Do not modify files under
`killbill-commons/jooby`.

## Exit criteria

- A read-only dependency and import map exists for the Jooby fork lane.
- Downstream commons tasks have enough information to avoid guessing about the
  Jooby boundary.
- Any required future Jooby-side work is documented explicitly for the session
  that owns that directory.


## Completion status

Completed as a planning / boundary task.

The Jooby lane was sufficiently understood and aligned for the commons
migration to finish successfully. Further Jooby work, if any, now belongs to
post-commons downstream adoption rather than the original Task 2 discovery
phase.
