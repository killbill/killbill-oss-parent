# Task 1: CI For Java2x Branches

## Objective

Adjust CI in the critical-path repositories so PRs targeting `java2x` receive
full and reliable coverage during the migration.

## Primary repo scope

- `killbill-oss-parent`
- `killbill-commons`
- `killbill-plugin-api`
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
- `killbill-commons`, `killbill-plugin-api`, `killbill-platform`,
  `killbill-plugin-framework-java`, `killbill-api`, and
  `killbill-client-java` currently use reusable workflows with an
  unfiltered `pull_request` trigger. Based on the checked-in workflow files,
  PRs targeting `java2x` are already picked up there without extra branch
  targeting changes.
- `killbill` required a real change as well: its push-based CI and CodeQL
  workflows only covered `master`, and its e2e workflow checked out
  `killbill-integration-tests` from `refs/heads/master`.

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
- `killbill` CI now:
  - runs push-based `ci` and `codeql-analysis` for both `master` and `java2x`
  - resolves the `killbill-integration-tests` checkout ref from the branch
    context so `java2x` PRs and pushes use `refs/heads/java2x`
  - keeps `master` as the default fallback outside the migration lane
- `java2x` branches are now in place for:
  `killbill-oss-parent`, `killbill-commons`, `killbill-plugin-api`,
  `killbill-platform`, `killbill-plugin-framework-java`, `killbill-api`,
  `killbill-client-java`, and `killbill`
- No library or application migration logic was mixed into this task.

## Temporary exceptions

- The shared-workflow repositories rely on `killbill/gh-actions-shared@main`.
  Their `java2x` PR coverage is currently inherited from the repository-level
  `pull_request` trigger, not from any migration-specific branch logic.
- `killbill-commons`, `killbill-plugin-api`, `killbill-platform`,
  `killbill-plugin-framework-java`, `killbill-api`, and
  `killbill-client-java` still rely on
  `killbill/gh-actions-shared@main` for the actual reusable CI behavior. That
  shared layer was inspected and is branch-agnostic for the `ci` and
  `codeql-analysis` workflows currently used here.
