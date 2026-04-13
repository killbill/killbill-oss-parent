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

## Temporary Analysis

This section is temporary analysis only. It is intended to capture the best
current classification from local repository inspection while
`killbill-commons` is still under heavy active change.

### Classification meanings

- `TOTALLY_SAFE`
  - Reasonable to evaluate and potentially upgrade in an isolated slice
    without first stabilizing the current Jooby / servlet migration lane.
- `MAYBE_SAFE`
  - Potentially workable in isolation, but still needs targeted downstream
    validation because usage is real and compatibility risk is non-trivial.
- `WAITING_FOR_JOOBY`
  - Coupled enough to the current Jooby / servlet / Jersey / Jetty migration
    track that moving it before that lane stabilizes would create churn or
    false failures.
- `UNKNOWN`
  - Not enough verified evidence yet to call it safe or blocked; requires
    deeper repo-specific inspection before implementation.

### Research basis

This analysis is based on the checked-in state of:

- this parent BOM `pom.xml`
- `.github/dependabot.yml`
- downstream POM and source usage in:
  - `killbill-commons`
  - `killbill-platform`
  - `killbill-plugin-framework-java`
  - `killbill`
  - lighter repos when relevant

Key verified signals from local inspection:

- `killbill`, `killbill-platform`, `killbill-plugin-framework-java`, and
  `killbill-commons` still contain mixed `jakarta.*` dependencies with
  remaining `javax.servlet` and `javax.ws.rs` code.
- Jooby usage is concentrated in `killbill-commons/jooby`,
  `killbill-plugin-framework-java`, and several platform OSGi bundles.
- Jetty, Jersey, HK2, servlet, and related Jakarta namespace work still sit on
  the critical migration path, not the supporting-library lane.
- Shiro usage is concentrated in `killbill` and is directly tied to
  application adoption work.
- `org.jooq` usage appears narrow in the current workspace, primarily in
  `killbill-plugin-framework-java`.
- Derby usage appears narrow in the current workspace, primarily in
  `killbill-commons/jdbi`.
- JAXB API/runtime usage is present in `killbill-api`, `killbill-commons`, and
  `killbill`.
- `org.osgi.service.log` usage is concentrated in `killbill-platform`.

### Family classification

| Dependency family | Classification | Rationale |
| --- | --- | --- |
| `ch.qos.logback:*` | `MAYBE_SAFE` | Not a Jooby blocker by itself, but runtime logging is used in `killbill-platform`, `killbill`, and OSGi-related modules. The ignore rule already records prior compatibility concerns. Could be done in an isolated slice, but only with platform and app validation. |
| `io.jooby:*` / `org.jooby:*` | `WAITING_FOR_JOOBY` | Directly part of the user-approved Jooby exception lane. Current usage is in `killbill-commons/jooby`, `killbill-plugin-framework-java`, and platform bundles. This is not a supporting-library-first move. |
| `io.netty:* >= 5.0.0` | `UNKNOWN` | The parent BOM manages Netty, but this analysis did not yet verify the full downstream runtime surface and upgrade pressure. Netty 5 is a major line change and should not be assumed safe from the current evidence. |
| `jakarta.activation:jakarta.activation-api >= 2.0.0` | `UNKNOWN` | This analysis did not verify enough direct usage to call it independently safe, but it also does not appear to be the primary blocker today. Requires focused JAXB / XML consumer inspection. |
| `jakarta.ws.rs:jakarta.ws.rs-api >= 3.0.0` | `WAITING_FOR_JOOBY` | `killbill` still has substantial `javax.ws.rs` source usage while dependencies have already moved toward `jakarta.ws.rs-api`. This belongs with the servlet / Jersey adoption path, not as an isolated supporting-family bump. |
| `jakarta.xml.bind:jakarta.xml.bind-api >= 3.0.0` | `MAYBE_SAFE` | JAXB usage is real across `killbill-api`, `killbill-commons`, and `killbill`, but it is more contained than the servlet stack. Possible as a dedicated slice if validated carefully. |
| `org.antlr:stringtemplate >= 4.0.0` | `TOTALLY_SAFE` | No meaningful verified migration coupling surfaced in the current workspace review. If still present transitively, it should be one of the safer families to inspect and upgrade independently. |
| `org.apache.shiro:* < 2.0.0` | `UNKNOWN` | The current ignore rule is unusual because it blocks versions below 2.0.0, while the parent currently manages Shiro `1.12.0`. Regardless, Shiro is deeply embedded in `killbill` and tied to Task 6. More inspection is needed before treating it as a support-only upgrade. |
| `org.eclipse.jetty:* >= 11.0.0` | `WAITING_FOR_JOOBY` | Jetty is part of the primary web-stack breakpoint. Usage exists in `killbill-commons`, `killbill-platform`, `killbill`, and `killbill-commons/jooby`. Advancing Jetty without the servlet/Jooby lane being stable is likely to create churn. |
| `org.ehcache:* >= 3.10.0` | `UNKNOWN` | The ignore rule documents prior known platform concerns, but this pass did not verify enough current usage to classify it as independently safe or blocked. |
| `org.glassfish.hk2:* >= 3.0.0` | `WAITING_FOR_JOOBY` | HK2 is directly coupled to Jersey and the servlet bridge in `killbill-commons/skeleton`. It remains part of the main migration spine, not a support-only bump. |
| `org.glassfish.jaxb:jaxb-runtime >= 3.0.0` | `MAYBE_SAFE` | Runtime JAXB upgrades are adjacent to the JAXB API move and appear less entangled than servlet/Jooby, but they still need focused validation across XML-loading consumers. |
| `org.glassfish.jersey.*:* >= 3.0.0` | `WAITING_FOR_JOOBY` | Jersey is explicitly part of Task 3 and sits on the core web-stack path. It is coupled to HK2, servlet namespace alignment, and commons/platform adoption. |
| `org.jooq:* >= 3.16.0` | `MAYBE_SAFE` | The current workspace shows comparatively narrow direct usage, mainly in `killbill-plugin-framework-java`, and the BOM already documents this as a delayed but targeted family. This looks workable as a dedicated slice once that repo is ready for focused validation. |
| `org.apache.derby:* >= 10.16.0.0` | `TOTALLY_SAFE` | The constraint is already documented as Java-17-related, and current usage appears narrow in `killbill-commons/jdbi`. For a Java 21 migration, Derby looks like a low-coupling candidate for a dedicated validation slice. |
| `jakarta.annotation:* >= 2.0.0` | `WAITING_FOR_JOOBY` | The ignore comment already ties this to the broader Jersey / servlet namespace migration. This should move with the main Jakarta web-stack transition, not ahead of it. |
| `org.osgi:org.osgi.service.log >= 1.4.0` | `MAYBE_SAFE` | Strongly localized to `killbill-platform` OSGi modules. Not blocked on Jooby itself, but still runtime-sensitive enough that platform-focused validation is mandatory. |
| `jakarta.servlet:jakarta.servlet-api >= 5.0.0` | `WAITING_FOR_JOOBY` | This is core migration-path work, not a supporting-family upgrade. The workspace still shows mixed dependency-level Jakarta adoption with remaining `javax.servlet` imports across multiple repos. |

### Provisional execution order for Task 7

If work starts before the main commons migration stabilizes, the least risky
investigation order appears to be:

1. `org.apache.derby:*`
2. `org.antlr:stringtemplate`
3. JAXB pair:
   `jakarta.xml.bind:jakarta.xml.bind-api` and
   `org.glassfish.jaxb:jaxb-runtime`
4. `org.jooq:*`
5. `org.osgi:org.osgi.service.log`
6. `ch.qos.logback:*`

Families that should stay out of Task 7 implementation until the main lane is
stable:

- Jooby
- Jetty
- Jersey
- HK2
- `jakarta.servlet`
- `jakarta.ws.rs`
- `jakarta.annotation`

### Open gaps

The following families still need deeper repo-specific confirmation before any
implementation work:

- `io.netty:* >= 5.0.0`
- `jakarta.activation:jakarta.activation-api >= 2.0.0`
- `org.ehcache:* >= 3.10.0`
- the exact intended Shiro target and dependabot ignore semantics
