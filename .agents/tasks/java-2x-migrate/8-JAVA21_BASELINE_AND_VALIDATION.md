# Task 8: Java 21 Baseline And Validation

## Objective

Raise the actual Java baseline and CI contract only after the library and
framework layers are ready.

## Primary repo scope

- this parent repo first
- then the CI-covered downstream repositories through validation

## Verified anchors

- `pom.xml`
- `.github/workflows/ci.yml`
- `.github/workflows/release.yml`

## Focus

- `project.build.targetJdk`
- enforcer bytecode limits
- surefire and test JVM flags
- CI matrix updates
- release workflow JDK alignment
- cross-repo validation on the Java 21 path

## Deliverables

1. Raise the parent baseline intentionally.
2. Update `killbill-oss-parent` CI to validate Java 21.
3. Update `killbill-oss-parent` release workflow to run with the effective
   Java 21 baseline.
4. Re-check downstream build and test behavior under the new baseline.
5. Decide whether Java 25 should remain a later follow-on target.

## Safe point

Do not raise the Java baseline before the major framework tracks are ready.
This task should be the integration gate, not the first move.

## Exit criteria

- The parent and downstream validation path work on Java 21.
- CI has an explicit Java 21 safety net.
- Java 25 is either deferred explicitly or planned from a stable Java 21 base.


## Updated status

This is the correct remaining subtask for the outstanding CI work in
`killbill-oss-parent`.

Current verified state:

- normal CI in `killbill-oss-parent` still validates Java `11` and `17`
- release workflow in `killbill-oss-parent` still runs on Java `11`
- `killbill-commons` CI already runs on Java `21`
- `killbill-commons` release workflow has now been updated to Java `21`

Implication:

- the parent repository still needs its Java 21 CI/release alignment work
- this is Task 8 scope, not Task 99
- Task 99 should only be used later to retire `java2x` branch targeting once
  the migration lane is no longer needed
