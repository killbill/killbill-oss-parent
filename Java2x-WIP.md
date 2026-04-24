# Java2x WIP

This document is the master task board for the Java 2x migration.

## How To Use This File

- Use this file as the high-level checklist and navigation page.
- Keep detailed task instructions in `.agents/tasks/java-2x-migrate/`.
- When work is active, update the `Current WIP` section below.
- When a task meaningfully changes state, update its checkbox and add a short
  note if needed.

### Current WIP

Tasks 1–3 and 8 are complete for `killbill-oss-parent`. Task 7 is partially
done (major libraries upgraded, minor families remain). Remaining open work
is downstream runtime adoption in `killbill-platform` (Task 4),
`killbill-plugin-framework-java` / Jooby (Task 5), and the `killbill` app
layer (Task 6).

## Task Directory

The detailed task files live under:

[`/.agents/tasks/java-2x-migrate`](./.agents/tasks/java-2x-migrate)

Start here:

- [`0-GETTING_STARTED.md`](./.agents/tasks/java-2x-migrate/0-GETTING_STARTED.md)

## Checklist

- [x] [`1-CI_FOR_JAVA2X_BRANCHES.md`](./.agents/tasks/java-2x-migrate/1-CI_FOR_JAVA2X_BRANCHES.md)
- [x] [`2-GUICE_AND_SERVLET_FOUNDATION.md`](./.agents/tasks/java-2x-migrate/2-GUICE_AND_SERVLET_FOUNDATION.md)
- [x] [`3-JERSEY_JETTY_AND_HK2_ALIGNMENT.md`](./.agents/tasks/java-2x-migrate/3-JERSEY_JETTY_AND_HK2_ALIGNMENT.md) — Jersey 3.1.11, HK2 3.0.6, Jetty 11.0.26, JAX-RS 3.1.0, Swagger 2.2.28. Parent BOM aligned.
- [ ] [`4-PLATFORM_OSGI_AND_LOGGING_RUNTIME.md`](./.agents/tasks/java-2x-migrate/4-PLATFORM_OSGI_AND_LOGGING_RUNTIME.md)
- [ ] [`5-JOOBY_AND_PLUGIN_RUNTIME.md`](./.agents/tasks/java-2x-migrate/5-JOOBY_AND_PLUGIN_RUNTIME.md)
- [ ] [`6-SHIRO_WEB_AND_APPLICATION_ADOPTION.md`](./.agents/tasks/java-2x-migrate/6-SHIRO_WEB_AND_APPLICATION_ADOPTION.md)
- [~] [`7-SUPPORTING_LIBRARY_UPGRADES.md`](./.agents/tasks/java-2x-migrate/7-SUPPORTING_LIBRARY_UPGRADES.md) — Done: Jackson 2.21.2, Logback 1.5.32, Metrics 4.2.38, Ehcache 3.11.1, Shiro 2.0.6, Netty 4.2.7 (pinned — hard-won cross-repo match), Javassist 3.30.2, JAXB 4.0.x. Remaining: jOOQ (blocked #557), Derby, ANTLR, OSGi log.
- [x] [`8-JAVA21_BASELINE_AND_VALIDATION.md`](./.agents/tasks/java-2x-migrate/8-JAVA21_BASELINE_AND_VALIDATION.md) — POM targetJdk=21, CI matrix Java 21, enforcer rules, release workflow all aligned.
- [ ] [`99-CI_RETURN_TO_MASTER.md`](./.agents/tasks/java-2x-migrate/99-CI_RETURN_TO_MASTER.md)
