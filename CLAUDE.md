# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Curity Identity Server plugin template** repository. It provides boilerplate Gradle build configuration, CI/CD workflows, and dependency setup for developing plugins against the Curity SDK. Actual plugin source code is added by developers who create repositories from this template.

- **Language:** Java 21 (source), Groovy (tests)
- **Build:** Gradle 8.5 with the `io.curity.gradle.curity-plugin-dev` plugin (v0.3.0)
- **Testing:** Spock Framework (BDD, Groovy-based) on JUnit 5 Platform
- **SDK:** Curity Identity Server SDK 11.0.0 (compile-only)

## Build & Development Commands

```bash
./gradlew build              # Build and run all tests
./gradlew test               # Run unit tests only
./gradlew integrationTest    # Run integration tests (requires .env with LICENSE_KEY)
./gradlew deployToLocal      # Deploy plugin to local Curity server (requires .env with IDSVR_HOME)
./gradlew createReleaseDir   # Compile plugin + dependencies into build/release/
```

**Running a single test:**
```bash
./gradlew test --tests "com.example.MySpec"
./gradlew test --tests "*MySpec.some feature method"
```

Integration tests are named `*IntegrationSpec` and require a running Curity Identity Server (via Testcontainers) with a valid license key.

## Environment Setup

1. **GitHub Packages auth** (required for dependencies): Add to `~/.gradle/gradle.properties`:
   ```properties
   gpr.user=YOUR_GITHUB_USERNAME
   gpr.token=YOUR_GITHUB_TOKEN  # needs read:packages scope
   ```

2. **Local .env file** (for deployToLocal and integrationTest): Copy `.env-example` to `.env` and set:
   - `IDSVR_HOME` — path to local Curity Identity Server installation
   - `LICENSE_KEY` — Curity Identity Server license JWT

## Architecture

- **`build.gradle`** — Central configuration: dependencies, SDK versions, test setup, JAR manifest, deployment tasks. The `curity-plugin-dev` Gradle plugin provides `deployToLocal` and integration test infrastructure.
- **`settings.gradle`** — Plugin management repositories and `rootProject.name` (must be customized per plugin).
- **`src/main/java/`** — Plugin implementation (Java). Implements interfaces from `se.curity.identityserver:identityserver.sdk`.
- **`src/test/groovy/`** — Tests using Spock Framework. Unit tests are `*Spec`, integration tests are `*IntegrationSpec`.
- **`.github/workflows/release.yml`** — Release workflow (manual dispatch), delegates to the reusable `curity-ps/ps-release-workflow`.

## Key Dependencies

- `se.curity.identityserver:identityserver.sdk:11.0.0` — Curity SDK (compile-only; provided at runtime by the server)
- `io.curity:curity-ps-sdk-commons:0.3.1` — Shared utilities for Curity plugins; also provides test utilities via the `tests` classifier
- `org.spockframework:spock-core:2.4-M4-groovy-4.0` — BDD test framework

## Release Process

Releases are triggered manually via GitHub Actions (Actions tab → "Build & Release" → "Run workflow"). The workflow delegates to `curity-ps/ps-release-workflow` and requires `PACKAGE_TOKEN` and `LICENSE_KEY` secrets.
