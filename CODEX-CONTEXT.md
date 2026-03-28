# Codex Context

## Repository role

This repository is the parent POM / BOM for Kill Bill Open Source projects.
Its main purpose is to centralize dependency and plugin versions in `pom.xml`,
and those versions are consumed across multiple downstream Kill Bill
repositories.

For dependency updates, the important question is not only whether this
repository builds, but whether downstream repositories still build and test
correctly when they inherit the updated parent.

## CI guardrails

The main CI protection is defined in `.github/workflows/ci.yml`.

Key characteristics:

- It runs on `pull_request`, `push`, and `workflow_dispatch`.
- It builds against Java 11 and Java 17.
- It runs test suites for `h2`, `mysql`, and `postgresql`.
- It checks out and exercises multiple downstream repositories:
  - `killbill-api`
  - `killbill-plugin-api`
  - `killbill-commons`
  - `killbill-plugin-framework-java`
  - `killbill-platform`
  - `killbill-client-java`
  - `killbill`
- It updates downstream repositories to use the current parent version, builds
  them, updates matching version properties back into this parent POM where
  needed, and then runs downstream tests.

Practical implication:

Dependency updates in this repo should be treated as cross-repository
compatibility changes, not isolated version bumps.

## Release and snapshot workflows

Additional workflows exist:

- `.github/workflows/release.yml`
- `.github/workflows/snapshot.yml`
- `.github/workflows/sync.yml`

`release.yml` can update selected internal Kill Bill dependency properties
before performing Maven release steps. `snapshot.yml` and `sync.yml` delegate
to shared workflows in `killbill/gh-actions-shared`.

## Dependabot

Dependabot is configured in `.github/dependabot.yml` for the Maven ecosystem
at the repository root, targeting the `master` branch on a daily schedule.

This configuration already ignores several dependency families or version
ranges known to be unsafe or migration-heavy for this codebase, including:

- Logback `>= 1.3.0`
- Jooby 2.x+
- Netty 5.x+
- Jakarta namespace jumps for several APIs
- Shiro constraints
- Jetty 11.x+
- Ehcache 3.10.x+
- HK2 3.x+
- JAXB runtime 3.x+
- Jersey 3.x+
- JOOQ 3.16.x+
- Derby 10.16.x+
- OSGi log updates with known platform concerns
- Servlet API 5.x+

Practical implication:

If Dependabot is already ignoring a dependency family, there is likely known
compatibility or migration risk. Those upgrades are still in scope for this
project, but they should be treated as deliberate engineering work with
documentation and proper validation, not as routine patch updates.

## Parent POM guardrails

The parent `pom.xml` contains multiple Maven-level checks that help catch
dependency problems early:

- Enforcer rules
- Banned dependencies
- Duplicate dependency version checks
- Upper-bound dependency checks
- Java and Maven version requirements
- Duplicate class/resource detection via duplicate finder
- Dependency version and dependency scope checks

Some checks are intentionally configurable via properties, but the default
posture is to fail on dependency-related issues.

Practical implication:

Even when a version bump looks small, this repository may fail builds because
of dependency graph conflicts, forbidden transitive artifacts, duplicate
classes, or bytecode/version mismatches before downstream tests even start.

## Working assumption for this session

When updating dependencies in this repository:

1. Start from `pom.xml`, because that is the source of truth for managed
   versions.
2. Treat existing Dependabot ignore rules as historical compatibility
   knowledge and migration signals, not as permanent no-upgrade rules.
3. Use the cross-repo CI model as the baseline safety net.
4. Add extra validation for sensitive dependency families when the existing
   safeguards are not sufficient.
5. Treat updates here as potentially ecosystem-wide changes across Kill Bill,
   not local-only maintenance.

## Session integrity guardrails

### 1. Never state something as fact without verifying it first

If you haven't read the actual code, say "I believe" or "let me check."
Do not present assumptions as confirmed truths.

### 2. When citing patterns from other modules, show the code

If you claim "the email module does X," provide file paths and line numbers
before making decisions based on that claim. Use the explore agent or read
the files directly.

### 3. If you don't know, say so

"I'm not sure — let me verify" is always better than a confident wrong answer.
The user values honesty over speed.

### 4. If you made a mistake, own it immediately

Do not minimize, deflect, or quietly fix it. State clearly what was wrong
and what the correct information is.

### 5. Double-check before recommending architectural changes

### 6. Never fabricate code, state, or information

Do not invent variable names, method signatures, or behaviors that don't
exist in the codebase. If you need to propose something new, clearly label
it as a proposal, not as existing code.

## Priorities (in order)

1. **Integrity** — be honest, verify claims, admit mistakes
2. **Correctness** — make changes that actually work
3. **Alignment** — follow the user's architectural preferences
4. **Efficiency** — move fast, but never at the cost of 1-3
