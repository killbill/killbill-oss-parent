# Task 2.4: Commons Jakarta Adoption Slice

## Objective

Execute the actual `javax.servlet` to `jakarta.servlet` transition in
`killbill-commons` once the Jooby-alignment prerequisites are satisfied.

## Primary repo scope

- `killbill-commons`
- especially `skeleton`
- especially `metrics`
- any commons code still using legacy `javax.servlet` types

## Why this is last within Task 2

- This is the first irreversible-feeling code change in the commons servlet
  layer and should only happen after the inventory, non-Jooby groundwork, and
  Jooby alignment are understood.
- It is the point where downstream compatibility pressure starts to rise.

## Deliverables

1. Replace the remaining intended `javax.servlet` usage in commons with
   `jakarta.servlet`.
2. Update the required dependency declarations in commons to match the new
   namespace.
3. Verify commons compiles and its relevant tests pass after the namespace
   change.
4. Document any remaining downstream blockers for `killbill-platform` or
   `killbill`.


## Worktree constraint

- `killbill-commons/jooby` is under heavy edits in another session.
- Do not modify files under `killbill-commons/jooby` from this task.
- If the Jakarta transition is blocked on the Jooby fork state, document that
  blocker and stop rather than editing the Jooby directory here.

## Safe point

Keep the transition scoped to commons. Do not fold downstream adoption work
into the same patch series unless a blocker is proven and documented.

## Exit criteria

- Commons compiles against the intended servlet namespace.
- Relevant commons tests pass.
- Remaining downstream migration blockers, if any, are documented explicitly.


## Completion status

Completed.

`killbill-commons` is now reported as Jakarta-ready, with Guice `7.0.0`,
Jetty `11`, and Java `21` as the effective completed state for this task lane.
