# Task 3: Jersey, Jetty, And HK2 Alignment

## Objective

Align the shared web-stack libraries that sit next to Guice and servlet work,
starting from commons and only then flowing upward.

## Primary repo scope

- `killbill-commons`
- `killbill-platform` only when commons changes require runtime adoption

## Verified anchors

- `killbill-commons/skeleton/pom.xml`
- `killbill-commons/skeleton/src/main/java/org/killbill/commons/skeleton/modules/GuiceServletContainer.java`
- `pom.xml` in this parent for managed Jersey, Jetty, and HK2 versions
- `.github/dependabot.yml` ignore rules for Jersey, Jetty, HK2, servlet, and
  annotation transitions

## Library focus

- `org.glassfish.jersey:*`
- `org.eclipse.jetty:*`
- `org.glassfish.hk2:*`
- related Jakarta annotation and JAX-RS alignment

## Deliverables

1. Verify which Jersey/HK2/Jetty moves are required for the Java 21 path.
2. Separate commons-side API and integration changes from platform runtime
   adoption.
3. Document any hard blockers that still force staged rather than direct
   upgrades.

## Safe point

Try to keep commons-side library alignment and platform-side adoption in
separate commits or sub-steps. If platform changes are required, they should
consume an already-prepared commons shape rather than redefining it.

## Exit criteria

- The shared Jersey/Jetty/HK2 stack has a verified upgrade direction.
- Commons is no longer the blocker for those libraries.
- Platform-specific follow-up, if any, is clearly isolated.
