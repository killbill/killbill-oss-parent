# Task 5: Jooby And Plugin Runtime

## Objective

Handle the plugin-runtime track after commons and platform have a stable
direction, using the user-approved `killbill-jooby` exception as an explicit
input.

## Primary repo scope

- `killbill-commons/jooby`
- `killbill-plugin-framework-java`

## Verified anchors

- `killbill-commons/jooby/pom.xml`
- `killbill-commons/pom.xml`
- `killbill-plugin-framework-java/pom.xml`

## Library focus

- Jooby-related code and dependencies
- plugin runtime servlet alignment
- plugin runtime OSGi alignment
- mixed Jakarta and `javax.inject` usage in plugin runtime code

## Deliverables

1. Verify the plugin framework’s dependence on Jooby, OSGi, and servlet APIs.
2. Align plugin runtime code with the already-updated commons/platform layers.
3. Document any residual plugin-specific blockers separately from core server
   blockers.

## Safe point

This task should be revertable without backing out the full commons/platform
migration. Keep plugin-runtime changes isolated from application adoption.

## Exit criteria

- Plugin runtime compiles and relevant tests pass.
- The Jooby exception path has been consumed in a controlled way.
- Remaining plugin-runtime blockers, if any, are explicitly documented.
