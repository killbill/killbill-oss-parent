# Task 7: Supporting Library Upgrades

## Objective

Handle supporting dependency families deliberately rather than mixing them into
the first framework breakpoints.

## Primary repo scope

- parent BOM in this repo
- impacted downstream repos only when a specific family requires it

## Verified anchors

- `pom.xml`
- `.github/dependabot.yml`

## Library focus

- `org.jooq:*`
- JAXB and related Jakarta XML binding artifacts
- Logback family follow-up
- Derby and other baseline-sensitive libraries
- other ignored families that are not the primary Guice/servlet/runtime path

## Deliverables

1. Classify each ignored family as:
   before Java 21 enabler, during Java 21 migration work, or after-Java-21
   cleanup.
2. Upgrade only one family or tightly related family set at a time.
3. Document rationale and validation for each family.

## Safe point

Each dependency family should be upgraded in its own revertable slice whenever
possible. Avoid “mega bump” changes.

## Exit criteria

- Each supporting family has an explicit placement in the migration timeline.
- Any implemented upgrade is documented and cross-repo validated.
- No ignored family is left in a vague “we’ll figure it out later” state.
