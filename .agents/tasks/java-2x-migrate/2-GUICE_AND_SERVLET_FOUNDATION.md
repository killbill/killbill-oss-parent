# Task 2: Guice And Servlet Foundation

## Objective

Create the first executable migration slice in `killbill-commons` for Guice and
servlet-facing foundation code without dragging in the full application stack.

## Primary repo scope

- `killbill-commons`
- especially `skeleton` and `metrics`

## Verified reasons this is early

- `killbill-commons` is foundational and is consumed by both
  `killbill-platform` and `killbill`.
- `killbill-commons/skeleton` owns Guice servlet listeners, base servlet
  modules, and shared server wiring.

## Verified anchors

- `killbill-commons/skeleton/pom.xml`
- `killbill-commons/skeleton/src/main/java/org/killbill/commons/skeleton/listeners/GuiceServletContextListener.java`
- `killbill-commons/skeleton/src/main/java/org/killbill/commons/skeleton/modules/BaseServerModule.java`
- `killbill-commons/skeleton/src/main/java/org/killbill/commons/skeleton/modules/JerseyBaseServerModule.java`
- `killbill-commons/metrics/pom.xml`

## Library focus

- `com.google.inject:*`
- `com.google.inject.extensions:guice-servlet`
- `jakarta.servlet:jakarta.servlet-api`
- legacy `javax.servlet` imports and types that still survive in commons code

## Deliverables

1. Inventory all Guice servlet and servlet namespace usage in commons.
2. Define the minimal code and dependency changes needed in commons before
   platform adoption.
3. Validate that the changes can build and test in `killbill-commons` without
   immediately changing downstream repos.

## Safe point

This task should be kept revertable at the `killbill-commons` layer. Avoid
mixing `killbill-platform` or `killbill` adoption changes into the same patch
series unless absolutely required.

## Exit criteria

- Commons compiles and its relevant tests pass.
- The Guice/servlet foundation is ready for platform consumption.
- Any remaining blockers are documented explicitly.

## Suggested execution split

- [`2-1-COMMONS_INVENTORY_AND_BOUNDARY_MAPPING.md`](./2-1-COMMONS_INVENTORY_AND_BOUNDARY_MAPPING.md)
- [`2-2-COMMONS_NON_JOOBY_FOUNDATION_CHANGES.md`](./2-2-COMMONS_NON_JOOBY_FOUNDATION_CHANGES.md)
- [`2-3-COMMONS_JOOBY_INTEGRATION_ALIGNMENT.md`](./2-3-COMMONS_JOOBY_INTEGRATION_ALIGNMENT.md)
- [`2-4-COMMONS_JAKARTA_ADOPTION_SLICE.md`](./2-4-COMMONS_JAKARTA_ADOPTION_SLICE.md)


## Completion status

Completed in `killbill-commons`.

Verified end state reported for the completed commons lane:

- `killbill-commons` is Jakarta-namespace ready for the servlet / web-stack
  path.
- Guice moved to `7.0.0`.
- Jetty moved to `11`.
- The commons repository now targets Java `21`.
- Remaining `javax.annotation` usage is treated as non-blocking for this task.
- A working demonstration exists in:
  `/mnt/data/vcs/git/github/xsalefter/kb-jooby-demo`

Implication for the migration plan:

- Task 2 is no longer the blocker.
- The next work should move to parent/BOM alignment and downstream adoption.
