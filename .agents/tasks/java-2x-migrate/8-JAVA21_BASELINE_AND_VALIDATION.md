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

## Focus

- `project.build.targetJdk`
- enforcer bytecode limits
- surefire and test JVM flags
- CI matrix updates
- cross-repo validation on the Java 21 path

## Deliverables

1. Raise the parent baseline intentionally.
2. Update CI to validate Java 21.
3. Re-check downstream build and test behavior under the new baseline.
4. Decide whether Java 25 should remain a later follow-on target.

## Safe point

Do not raise the Java baseline before the major framework tracks are ready.
This task should be the integration gate, not the first move.

## Exit criteria

- The parent and downstream validation path work on Java 21.
- CI has an explicit Java 21 safety net.
- Java 25 is either deferred explicitly or planned from a stable Java 21 base.
