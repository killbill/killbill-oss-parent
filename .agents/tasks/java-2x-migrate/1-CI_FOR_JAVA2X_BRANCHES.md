# Task 1: CI For Java2x Branches

## Objective

Adjust CI in the critical-path repositories so PRs targeting `java2x` receive
full and reliable coverage during the migration.

## Primary repo scope

- `killbill-oss-parent`
- `killbill-commons`
- `killbill-plugin-framework-java`
- `killbill-platform`
- `killbill-api`
- `killbill-client-java`
- `killbill`

## Why this is early

- The migration is planned around cross-repository `java2x` branches.
- Without branch-aware CI, migration PRs can land into `java2x` without the
  same confidence level currently expected on `master`.
- This is infrastructure work that should exist before heavy implementation
  begins.

## Deliverables

1. Identify the CI workflows in the critical-path repositories that need branch
   targeting updates.
2. Update them so PRs against `java2x` are fully covered.
3. Confirm cross-repo references, matrix behavior, and parent-version flows are
   consistent with the `java2x` branch model.
4. Document any temporary CI exceptions explicitly.

## Safe point

Keep CI branch-targeting work isolated from library and code migration changes.
This task should be revertable without rolling back application code changes.

## Exit criteria

- PRs targeting `java2x` are covered in the critical-path repositories.
- CI behavior for `java2x` is documented and reviewable.
- The implementation tasks can rely on branch-aware CI rather than ad hoc
  manual validation.

## Verified findings

- `killbill-oss-parent` required a real change: [`.github/workflows/ci.yml`](./../../../.github/workflows/ci.yml)
  hard-coded all downstream checkout refs to `refs/heads/master`, which would
  validate migration PRs against the wrong branch set.
- `killbill-commons`, `killbill-platform`, and
  `killbill-plugin-framework-java` currently use reusable workflows with an
  unfiltered `pull_request` trigger. Based on the checked-in workflow files,
  PRs targeting `java2x` are already picked up there without extra branch
  targeting changes.
- `killbill` also has an unfiltered `pull_request` trigger for its main CI
  workflow, but its local e2e job still checks out
  `killbill-integration-tests` from `refs/heads/master`. That is a separate
  repository-level follow-up if the migration lane also needs branch-coupled
  integration tests there.

## Implementation notes

- `killbill-oss-parent` CI now resolves a single downstream branch ref from the
  workflow context:
  - PRs targeting `java2x` use `refs/heads/java2x`
  - pushes to `java2x` use `refs/heads/java2x`
  - all other cases keep using `refs/heads/master`
- The downstream checkout set remains aligned across:
  `killbill-api`, `killbill-plugin-api`, `killbill-commons`,
  `killbill-plugin-framework-java`, `killbill-platform`,
  `killbill-client-java`, and `killbill`.
- No library or application migration logic was mixed into this task.

## Temporary exceptions

- `killbill` e2e coverage is not fully branch-coupled yet because
  `killbill-integration-tests` is still pinned to `refs/heads/master` in that
  repository's workflow.
- The shared-workflow repositories rely on `killbill/gh-actions-shared@main`.
  Their `java2x` PR coverage is currently inherited from the repository-level
  `pull_request` trigger, not from any migration-specific branch logic.
