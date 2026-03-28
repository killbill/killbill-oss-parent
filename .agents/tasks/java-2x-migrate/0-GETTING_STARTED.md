# Java 2x Migration: Getting Started

## Goal

The current working goal is a Java 21-first migration path for Kill Bill, with
Java 25 treated as a possible follow-on target after Java 21 is stable.

## User-specific rules for this migration

1. Guice is expected to be a major migration area and must be handled
   carefully.
2. `org.kill-bill.commons:killbill-jooby` is a user-approved exception for this
   session: assume that fork will use the correct Guice and Servlet versions.
   This assumption applies only to that module and should not be generalized.
3. If there is doubt, stop and ask before locking in a direction.
4. Library freshness is a real project goal, including for families currently
   ignored in `.github/dependabot.yml`.
5. Those upgrades must be documented and properly tested.

## Repository context

- This repo is the parent POM/BOM for Kill Bill Open Source projects.
- Dependency updates here are cross-repository compatibility changes, not local
  version bumps.
- The CI contract in `.github/workflows/ci.yml` rebuilds and tests:
  `killbill-api`, `killbill-plugin-api`, `killbill-commons`,
  `killbill-plugin-framework-java`, `killbill-platform`,
  `killbill-client-java`, and `killbill`.
- Local copies of those repositories are available in the workspace and should
  be used for verification when relevant.

## Current verified baseline

- The parent POM still builds to Java 11.
- CI currently validates Java 11 and 17, not 21 or 25.
- The highest-risk migration areas already verified are:
  Guice, servlet/Jakarta alignment, Jersey, Jetty, Jooby, HK2, Shiro, OSGi,
  and logging.

## Verified repo sequence

1. `killbill-commons`
2. `killbill-platform`
3. `killbill-plugin-framework-java`
4. `killbill`
5. lighter repos and cleanup

This order is based on verified dependency direction in the local POMs.

## How to treat ignored library families

Do not treat Dependabot ignore rules as permanent no-upgrade rules. Treat them
as migration signals.

Classify each ignored family into one of these buckets:

1. Before Java 21: enabling upgrades that reduce migration friction without
   forcing the main framework breakpoints.
2. During Java 21: upgrades that are part of the migration itself.
3. After Java 21 stabilization: cleanup and catch-up upgrades.

## CI branch model

- For this migration, the preferred integration model is a `java2x` branch in
  the critical-path repositories.
- PRs for migration work should target `java2x`, not `master`, while the
  migration lane is active.
- CI needs dedicated work both to cover PRs against `java2x` and later to
  restore normal targeting back to `master`.

## Task layout

The numbered task files in this directory are ordered execution tracks. Each
task tries to be a safe point:

- narrow dependency family scope
- minimal repo overlap where possible
- explicit exit criteria
- revertable in isolation as much as practical

## Primary references

- [`CODEX-CONTEXT.md`](./../../../CODEX-CONTEXT.md)
- [`pom.xml`](./../../../pom.xml)
- [`.github/workflows/ci.yml`](./../../../.github/workflows/ci.yml)
- [`.github/dependabot.yml`](./../../../.github/dependabot.yml)
