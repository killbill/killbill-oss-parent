# Task 4: Platform OSGi And Logging Runtime

## Objective

Update the `killbill-platform` runtime layer after the commons foundation is in
place, with special attention to OSGi and logging behavior.

## Primary repo scope

- `killbill-platform`

## Verified anchors

- `killbill-platform/pom.xml`
- `killbill-platform/server/pom.xml`
- `killbill-platform/osgi/pom.xml`
- `killbill-platform/server/src/main/java/org/killbill/billing/server/listeners/KillbillPlatformGuiceListener.java`
- `killbill-platform/server/src/main/java/org/killbill/billing/server/filters/KillbillGuiceFilter.java`
- `killbill-platform/osgi-api/src/main/java/org/killbill/billing/osgi/api/OSGIKillbillRegistrar.java`

## Library focus

- OSGi artifacts
- `org.osgi.service.log`
- `org.slf4j:*`
- `ch.qos.logback:*`
- runtime logging integration coupled to the servlet/container stack

## Deliverables

1. Reconcile platform runtime behavior with the new commons foundation.
2. Validate OSGi compatibility boundaries explicitly.
3. Resolve logging and servlet-container integration issues without hiding them
   behind stale ignore rules.

## Safe point

Keep runtime/container changes inside `killbill-platform` as a distinct step.
Do not combine application-level `killbill` web descriptor updates into this
task.

## Exit criteria

- Platform compiles and relevant tests pass.
- OSGi and logging behavior is documented for the new baseline.
- Downstream consumers can start adopting platform changes.
