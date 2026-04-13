# Task 2.3: Commons Jooby Integration Alignment

## Objective

Define and implement the `killbill-commons` changes that must align with the
forked `jooby` module before Jakarta servlet adoption can proceed cleanly.

## Primary repo scope

- `killbill-commons`
- `killbill-commons/jooby`
- commons modules that integrate with Jooby-owned servlet behavior

## Why this is separate

- The project has already decided to fork Jooby 1.6.9 instead of jumping
  directly to Jooby 3.x.
- That fork has its own maintenance and migration lane, and commons should
  only absorb the integration changes that are actually required.

## Deliverables

1. Identify every place where commons depends on Jooby-owned servlet behavior
   or APIs.
2. Define the compatibility contract between commons and the forked Jooby
   module.
3. Document what must be ready in the fork before commons can move its servlet
   foundation further.
4. Implement any commons-side alignment changes that are required before the
   Jakarta namespace transition.


## Worktree constraint

- `killbill-commons/jooby` is under heavy edits in another session.
- Do not edit `killbill-commons/jooby` from this task unless the user
  explicitly reassigns ownership.
- Treat this subtask as a commons-side alignment and documentation task.
- When Jooby details are needed, inspect the module read-only and record the
  required compatibility contract outside the Jooby directory.

## Safe point

Do not reopen the decision to fork Jooby. Treat the fork as an accepted input
to the migration. Keep this task focused on integration boundaries and required
commons-side alignment.

## Exit criteria

- The dependency and code contract between commons and the forked Jooby module
  is explicit.
- Required commons-side alignment changes are complete or clearly blocked on
  the fork lane.
- No hidden Jooby assumptions remain in the commons foundation work.


## Completion status

Completed from the commons side.

The Jooby fork lane and the commons migration were aligned sufficiently for the
repository to reach a working Jakarta-ready state. A working demo is available
at `/mnt/data/vcs/git/github/xsalefter/kb-jooby-demo`.
