# Task 99: CI Return To Master

## Objective

After the `java2x` migration lane is stable or complete, restore CI targeting
and branch assumptions from `java2x` back to normal `master`-based operation.

## Primary repo scope

- `killbill-oss-parent`
- `killbill-commons`
- `killbill-platform`
- `killbill-plugin-framework-java`
- `killbill`
- any other repository whose CI was adjusted for `java2x`

## Why this is separate

- Branch-integration infrastructure should not become accidental permanent
  drift.
- The work to enable `java2x` coverage and the work to retire it are different
  operational tasks and should be reviewed separately.

## Deliverables

1. Identify every CI change introduced for `java2x`.
2. Restore default targeting and workflow assumptions to `master`.
3. Remove temporary migration-only CI behavior that is no longer needed.
4. Preserve any genuinely useful CI improvements that should remain after the
   migration.

## Safe point

This task should be done only after the migration branch strategy is no longer
required. It should remain revertable independently from the migration code
itself.

## Exit criteria

- CI no longer depends on the `java2x` branch model.
- Temporary migration branch logic has been removed or consciously retained.
- Normal branch policy is restored cleanly.
