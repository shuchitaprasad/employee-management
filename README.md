# Employee Management — Code Quality Baseline

This repository is a small Spring Boot project created to demonstrate and collect baseline static analysis issues from several tools before and after adding AGENTS.MD file.

- Checkstyle
- SpotBugs (with FindSecBugs)
- PMD
- Error Prone

This project  acts as a starting point, same prompt will be given to coding agents GIThub Copilot/ Windsurf for generating a CRUD API code. It will be observed that without defining AGENTS.md file, sample violations number will be very hihg which can be easily act as a metric to measure the code qaulity. This will be followed by addition of AGENTS.md file to the code base and regeneration of the code with AGENTS.d file. We will observe a clear reduction in the static tool violations and improved code coverage. The same tool can be used across coding agents and will live across mutiple chat sessions.

## What this repo shows

- A minimal Spring Boot REST API for `Employee` (entity, repository, service, controller).
- Configured Maven plugins for the code-quality tools and JaCoCo coverage:
  - Checkstyle (custom rules at `config/checkstyle/checkstyle.xml`) — configured to fail the build on violations.
  - SpotBugs (with FindSecBugs) — configured with a filter at `config/spotbugs/spotbugs-exclude.xml`. See `docs/SPOTBUGS_RULES.md` for detailed rule explanations.
  - PMD (ruleset at `config/pmd/ruleset.xml`) — pinned to a PMD-compatible plugin version. See `docs/PMD_RULES.md` for rule documentation.
  - Error Prone (annotation-processor configured in Maven compiler profile). See `docs/ERROR_PRONE_RULES.md` for configured rules.
  - JaCoCo (coverage reports under `target/site/jacoco`).

Detailed documentation for each tool's rules, examples, and best practices can be found in the `docs/` folder:
- `docs/PMD_RULES.md` - PMD rules reference with examples
- `docs/SPOTBUGS_RULES.md` - SpotBugs and FindSecBugs rules guide
- `docs/ERROR_PRONE_RULES.md` - Error Prone compilation checks

## Key report locations (after running the Maven goals below)

- Checkstyle XML report:
  - `target/reports/checkstyle-violations.xml`
- SpotBugs XML report:
  - `target/spotbugs.xml`
- PMD XML report:
  - `target/pmd.xml`
- JaCoCo HTML report (recommended):
  - `target/site/jacoco/index.html`
- JaCoCo XML (machine-readable):
  - `target/site/jacoco/jacoco.xml`


## Quickstart — run analyses and tests (Windows PowerShell)

Note: this project intentionally contains violations and the build is configured to fail if Checkstyle/SpotBugs/PMD find configured failures. If you want to run the checks and see failing output, run the full lifecycle. If you want to run tests / coverage while skipping the checks, see the skip commands below.

Run full verification (may fail due to configured violations):

```powershell
# Runs full verify including checkstyle/spotbugs/pmd (may fail)
.\mvnw.cmd clean verify
```

Generate tests and JaCoCo report (tests only — may still fail in verify if checks are enabled):

```powershell
# Run tests and generate JaCoCo HTML/XML
.\mvnw.cmd test org.jacoco:jacoco-maven-plugin:0.8.10:report -DskipITs=true
```

Run tests & coverage while skipping Checkstyle/SpotBugs/PMD (handy to run tests without build failures):

```powershell
# Skip static-analysis checks so test and report generation complete
.\mvnw.cmd -Dcheckstyle.skip=true -Dspotbugs.skip=true -Dpmd.skip=true clean test org.jacoco:jacoco-maven-plugin:0.8.10:report -DskipITs=true
```

If you only want to run individual plugins, use their goals, for example:

```powershell
# Run only PMD check
.\mvnw.cmd pmd:check

# Run only SpotBugs check
.\mvnw.cmd spotbugs:check

# Run only Checkstyle check
.\mvnw.cmd checkstyle:check
```

## Baseline counts (current state in this repo)

These are the results from recent runs included in the repository session (keep in mind your local run may differ as code changes):

There are 0 violations from the current code.

These numbers are a baseline. The purpose is to collect these before any remediation or automation agent changes are applied.

## Documentation and Rules

### Code Quality Rules
The `docs/` folder contains detailed documentation for each static analysis tool:
- `docs/PMD_RULES.md` - PMD rules with examples and severity levels
- `docs/SPOTBUGS_RULES.md` - SpotBugs rules including security checks
- `docs/ERROR_PRONE_RULES.md` - Error Prone compilation warnings

Each document includes:
- Detailed rule descriptions with code examples
- Severity levels and enforcement policies
- How to suppress specific warnings when needed
- Best practices and common fixes

## Notes and recommended next steps

- This repository serves as a baseline dataset. Do not merge remediation changes directly here — use this baseline to measure improvement.
- The next planned doc is `AGENT.md` which will describe automation/scripting guidance for fixing or triaging issues. That file will be added once the team decides on remediation strategy.
- Recommend adding unit tests that exercise `EmployeeService` and `EmployeeController` (use `@WebMvcTest` or MockMvc) to increase coverage and exercise business logic.
- If you want help creating test skeletons or selectively fixing a subset of Checkstyle/PMD/SpotBugs issues:
  1. Review the rules in the `docs/` folder
  2. Identify which rules you want to address
  3. Tell me which class or rule to prioritize for fixes/tests

## Contact / Maintainers

This repository is a demo baseline. If you need a different baseline (different rulesets or thresholds), update the config files under `config/` and re-run the Maven goals above.

---

(AGENT.md will be created in a future update to describe automated remediation and enforcement workflows.)
