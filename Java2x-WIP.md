# Java2x WIP

This document is the master task board for the Java 2x migration.

## How To Use This File

- Use this file as the high-level checklist and navigation page.
- Keep detailed task instructions in `.agents/tasks/java-2x-migrate/`.
- When work is active, update the `Current WIP` section below.
- When a task meaningfully changes state, update its checkbox and add a short
  note if needed.

### Current WIP

Work on `.agents/tasks/java-2x-migrate/2-GUICE_AND_SERVLET_FOUNDATION.md`.
Task 1 CI setup is complete: `java2x` branches are ready upstream for the
critical-path repositories, including `killbill-plugin-api`, `killbill-oss-parent`
and `killbill` have the needed repo-local CI updates, and the shared
`gh-actions-shared` `ci` and `codeql-analysis` workflows were verified as
branch-agnostic. Task 2 is scoped only to `killbill-commons`; before continuing
there, I need you to refresh me again with the current `killbill-commons`
branch/worktree state from your other session.

Example:

Work on `.agents/tasks/java-2x-migrate/2-GUICE_AND_SERVLET_FOUNDATION.md`.
Code migration for guice is completed.
Library up-to-date.
Still fixing unit tests.

## Task Directory

The detailed task files live under:

[`/.agents/tasks/java-2x-migrate`](./.agents/tasks/java-2x-migrate)

Start here:

- [`0-GETTING_STARTED.md`](./.agents/tasks/java-2x-migrate/0-GETTING_STARTED.md)

## Checklist

- [x] [`1-CI_FOR_JAVA2X_BRANCHES.md`](./.agents/tasks/java-2x-migrate/1-CI_FOR_JAVA2X_BRANCHES.md)
- [ ] [`2-GUICE_AND_SERVLET_FOUNDATION.md`](./.agents/tasks/java-2x-migrate/2-GUICE_AND_SERVLET_FOUNDATION.md)
- [ ] [`3-JERSEY_JETTY_AND_HK2_ALIGNMENT.md`](./.agents/tasks/java-2x-migrate/3-JERSEY_JETTY_AND_HK2_ALIGNMENT.md)
- [ ] [`4-PLATFORM_OSGI_AND_LOGGING_RUNTIME.md`](./.agents/tasks/java-2x-migrate/4-PLATFORM_OSGI_AND_LOGGING_RUNTIME.md)
- [ ] [`5-JOOBY_AND_PLUGIN_RUNTIME.md`](./.agents/tasks/java-2x-migrate/5-JOOBY_AND_PLUGIN_RUNTIME.md)
- [ ] [`6-SHIRO_WEB_AND_APPLICATION_ADOPTION.md`](./.agents/tasks/java-2x-migrate/6-SHIRO_WEB_AND_APPLICATION_ADOPTION.md)
- [ ] [`7-SUPPORTING_LIBRARY_UPGRADES.md`](./.agents/tasks/java-2x-migrate/7-SUPPORTING_LIBRARY_UPGRADES.md)
- [ ] [`8-JAVA21_BASELINE_AND_VALIDATION.md`](./.agents/tasks/java-2x-migrate/8-JAVA21_BASELINE_AND_VALIDATION.md)
- [ ] [`99-CI_RETURN_TO_MASTER.md`](./.agents/tasks/java-2x-migrate/99-CI_RETURN_TO_MASTER.md)
