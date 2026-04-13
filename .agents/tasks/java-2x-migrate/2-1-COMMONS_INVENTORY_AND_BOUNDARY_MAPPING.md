# Task 2.1: Commons Inventory And Boundary Mapping

## Objective

Create a verified inventory of Guice, Guice servlet, and servlet namespace
usage in `killbill-commons`, with special attention to `skeleton` and
`metrics`.

## Primary repo scope

- `killbill-commons`
- especially `skeleton`
- especially `metrics`

## Why this is first

- The migration needs an exact map of `javax.servlet`, `jakarta.servlet`, and
  Guice servlet touch points before code changes start spreading across
  modules.
- This is the lowest-risk executable slice in Task 2 because it produces a
  blocker map without forcing adoption changes yet.

## Deliverables

1. Inventory all `javax.servlet` imports and usages in `killbill-commons`.
2. Inventory all `jakarta.servlet` imports and usages in `killbill-commons`.
3. Inventory all Guice servlet usage, including listeners, filters, servlet
   modules, and request-scope related bindings.
4. Identify which usages are inside the Jooby fork lane versus outside it.
5. Document the namespace and dependency boundaries that later subtasks must
   preserve or replace.


## Worktree constraint

- `killbill-commons/jooby` is under heavy edits in another session.
- Do not modify files under `killbill-commons/jooby` from this task.
- You may inspect it only when needed for read-only boundary mapping.
- You may freely work in other `killbill-commons` directories such as
  `queue`, `metrics`, `skeleton`, and similar modules.

## Safe point

This task should be documentation and analysis only. Do not mix in dependency
upgrades or runtime behavior changes.

## Exit criteria

- A verified file-by-file inventory exists for servlet and Guice servlet usage
  in commons.
- Jooby-coupled versus non-Jooby-coupled usages are called out explicitly.
- The next subtasks can proceed using a real blocker map instead of
  assumptions.


## Verified findings

- The servlet and Guice servlet surface outside `killbill-commons/jooby` is
  concentrated in `skeleton` and `metrics`.
- No non-Jooby source files outside `skeleton` and `metrics` were found using
  `javax.servlet`, `jakarta.servlet`, `ServletModule`,
  `GuiceServletContextListener`, or `GuiceFilter`.
- `skeleton/pom.xml` and `metrics/pom.xml` already declare
  `jakarta.servlet:jakarta.servlet-api`, while the implementation code in those
  modules still imports `javax.servlet.*`.
- `skeleton` and `metrics` both still depend on Guice servlet via
  `com.google.inject.extensions:guice-servlet`.

## Verified inventory

### Dependency declarations

- `skeleton/pom.xml`
  - `com.google.inject.extensions:guice-servlet`
  - `jakarta.servlet:jakarta.servlet-api`
- `metrics/pom.xml`
  - `com.google.inject.extensions:guice-servlet`
  - `jakarta.servlet:jakarta.servlet-api`

### `javax.servlet` usage in `skeleton`

- `skeleton/src/main/java/org/killbill/commons/skeleton/listeners/GuiceServletContextListener.java`
  - `ServletContextEvent`
  - extends `com.google.inject.servlet.GuiceServletContextListener`
- `skeleton/src/main/java/org/killbill/commons/skeleton/listeners/JULServletContextListener.java`
  - `ServletContextEvent`
  - `ServletContextListener`
- `skeleton/src/main/java/org/killbill/commons/skeleton/modules/BaseServerModule.java`
  - `Filter`
  - `HttpServlet`
  - extends `ServletModule`
- `skeleton/src/main/java/org/killbill/commons/skeleton/modules/BaseServerModuleBuilder.java`
  - `Filter`
  - `HttpServlet`
- `skeleton/src/main/java/org/killbill/commons/skeleton/modules/JerseyBaseServerModule.java`
  - `Filter`
  - `HttpServlet`
- `skeleton/src/main/java/org/killbill/commons/skeleton/modules/GuiceServletContainer.java`
  - `ServletException`
- `skeleton/src/main/java/org/killbill/commons/skeleton/servlets/LogInvalidResourcesServlet.java`
  - `HttpServlet`
  - `HttpServletRequest`
  - `HttpServletResponse`
  - `ServletException`
- `skeleton/src/test/java/org/killbill/commons/skeleton/modules/AbstractBaseServerModuleTest.java`
  - `ServletContextEvent`
  - `GuiceFilter`
  - `GuiceServletContextListener`

### Guice servlet usage in `skeleton`

- `skeleton/src/main/java/org/killbill/commons/skeleton/listeners/GuiceServletContextListener.java`
- `skeleton/src/main/java/org/killbill/commons/skeleton/modules/BaseServerModule.java`
- `skeleton/src/test/java/org/killbill/commons/skeleton/modules/AbstractBaseServerModuleTest.java`
- `skeleton/src/test/java/org/killbill/commons/skeleton/modules/TestBaseServerModule.java`
- `skeleton/spotbugs-exclude.xml`
  - references `GuiceServletContextListener`

### `javax.servlet` usage in `metrics`

- `metrics/src/main/java/org/killbill/commons/metrics/servlets/HealthCheckServlet.java`
  - `ServletConfig`
  - `ServletContext`
  - `ServletException`
  - `ServletRequest`
  - `HttpServlet`
  - `HttpServletRequest`
  - `HttpServletResponse`
- `metrics/src/main/java/org/killbill/commons/metrics/servlets/MetricsServlet.java`
  - `ServletConfig`
  - `ServletContext`
  - `ServletException`
  - `ServletRequest`
  - `HttpServlet`
  - `HttpServletRequest`
  - `HttpServletResponse`
- `metrics/src/main/java/org/killbill/commons/metrics/servlets/ThreadDumpServlet.java`
  - `HttpServlet`
  - `HttpServletRequest`
  - `HttpServletResponse`
- `metrics/src/main/java/org/killbill/commons/metrics/servlets/InstrumentedFilter.java`
  - `AsyncEvent`
  - `AsyncListener`
  - `Filter`
  - `FilterChain`
  - `FilterConfig`
  - `ServletException`
  - `ServletRequest`
  - `ServletResponse`
  - `HttpServletResponse`
  - `HttpServletResponseWrapper`

### Guice servlet usage in `metrics`

- `metrics/src/main/java/org/killbill/commons/metrics/modules/AdminServletModule.java`
  - extends `ServletModule`
- `metrics/src/main/java/org/killbill/commons/metrics/modules/StatsModule.java`
  - installs `AdminServletModule`

## Boundary summary

- The verified non-Jooby migration boundary is concentrated in two commons
  modules only: `skeleton` and `metrics`.
- `skeleton` owns the main listener and server wiring boundary:
  `GuiceServletContextListener`, `JULServletContextListener`,
  `BaseServerModule`, `JerseyBaseServerModule`, and `GuiceServletContainer`.
- `metrics` owns servlet endpoints and one servlet `Filter`, but not the main
  server bootstrap.
- There are currently no verified `jakarta.servlet` imports in non-Jooby source
  code; the Jakarta presence is dependency-level only in the two POM files.
- The most important mismatch is therefore:
  - dependency declarations are already on `jakarta.servlet-api`
  - source and tests still compile against `javax.servlet`
  - Guice servlet still anchors the servlet boundary in both modules

## Explicit blockers for later subtasks

- Any real namespace transition in `skeleton` or `metrics` must account for
  the current Guice servlet boundary, not only the servlet API imports.
- `GuiceServletContainer` also bridges into Jersey/HK2, so servlet migration in
  `skeleton` is coupled to later Jersey/HK2 alignment work even though this
  inventory task stays local to commons.
- `killbill-commons/jooby` was intentionally excluded from this scan and still
  needs separate read-only analysis under Task 2.5 when that boundary becomes
  relevant.


## Completion status

Completed.

The inventory was consumed by the completed commons migration. The important
verified conclusion held through execution: outside `killbill-commons/jooby`,
the non-Jooby servlet/Guice surface was concentrated in `skeleton` and
`metrics`, and those modules were migrated successfully.
