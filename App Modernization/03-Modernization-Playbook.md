# Copilot-Driven Modernization Playbook

A step-by-step guide for using GitHub Copilot to modernize legacy applications, based on patterns demonstrated in [samqbush/doomv2](https://github.com/samqbush/doomv2).

---

## Phase 0: Discovery & Architecture Analysis

### Goal
Produce a complete architecture understanding before writing any code.

### Steps

1. **Point Copilot at the codebase** — Use Copilot Chat or agentic mode to analyze the repository structure, dependencies, and module boundaries.

2. **Generate an ARCHITECTURE.md** — Ask Copilot to produce a comprehensive architecture reference covering:
   - Module map and dependency graph
   - Platform isolation (what touches the OS vs. what's portable)
   - Data contracts and external interfaces
   - Build system and commands inventory
   - Developer gotchas and known dead-ends

3. **Run the feasibility spike** — Attempt to build with the current toolchain. Capture:
   - First failure point (what breaks and why)
   - Porting surface measurement (how many files are affected)
   - Isolation assessment (is the damage contained or systemic?)

4. **Triage components** — Categorize each component:
   - **Port** — Core logic that's mostly sound, needs targeted fixes
   - **Rewrite** — Platform code where the underlying tech is gone
   - **Archive** — Dead code with no modern equivalent
   - **Defer** — Future enhancements beyond modernization scope

### Exit Criteria
- [ ] Architecture document reviewed and accurate
- [ ] Feasibility spike results captured (actual compiler output)
- [ ] Component triage complete with rationale

---

## Phase 1: Build System & Compilation

### Goal
Get the codebase compiling on a modern toolchain without changing behavior.

### Steps

1. **Introduce a modern build system** — Use Copilot to generate CMake, Meson, or equivalent from the legacy Makefile.

2. **Fix header/API removals** — Replace removed headers with modern equivalents (e.g., `values.h` → `limits.h` + `float.h`).

3. **Address type-width issues** — Fix `int`/pointer size assumptions for 64-bit.

4. **Stub the dead platform layer** — Create minimal stubs for platform functions so the core compiles even if it can't run.

### Exit Criteria
- [ ] `cmake -B build && cmake --build build` succeeds (or equivalent)
- [ ] No UB from pointer/int size mismatches
- [ ] Core logic compiles cleanly with `-Wall -Wextra`

---

## Phase 2: First Runnable State (Beachhead)

### Goal
Get the application running — even minimally — to establish a behavioral baseline.

### Steps

1. **Implement the minimum platform layer** — Replace one dead platform file at a time (video first, then audio, then networking).

2. **Establish the oracle** — Capture a behavioral baseline:
   - Transaction replay, deterministic output, or integration test
   - Record golden-master checksums/hashes

3. **Create the first automated tests** — Even one passing test is a milestone. Behavioral parity tests are most valuable.

### Exit Criteria
- [ ] Application runs (even in limited form)
- [ ] Behavioral oracle captured (replay test, checksum, etc.)
- [ ] At least one automated test passes

---

## Phase 3: CI/CD & Verification

### Goal
Establish automated verification so regressions are caught immediately.

### Steps

1. **Generate CI workflow** — Use Copilot to create GitHub Actions (or equivalent) that builds and tests on every push/PR.

2. **Expand the test suite** — Add tests for each component as it crosses its "testability milestone."

3. **Gate on green CI** — From this point forward, all changes must pass CI before merge.

### Exit Criteria
- [ ] CI runs on every PR
- [ ] Full test suite passes
- [ ] Branch protection enforces green CI

---

## Phase 4: Incremental Completion

### Goal
Complete the remaining platform rewrites and hardening.

### Steps

1. **Work one subsystem at a time** — Audio, networking, or other platform components.

2. **Each subsystem follows the same pattern:**
   - Copilot generates the modern replacement
   - Add subsystem-specific tests
   - Verify parity with the oracle
   - CI stays green

3. **Update copilot-instructions.md** — Add new commands and update phase status as you go.

### Exit Criteria
- [ ] All platform components modernized
- [ ] Full parity gate green
- [ ] Interactive/manual smoke testing passes

---

## Phase 5: Packaging & Release

### Goal
Ship the modernized application with automated release infrastructure.

### Steps

1. **Create packaging scripts** — Platform-specific installers (DMG, AppImage, MSI, Docker, etc.)

2. **Automate releases** — VERSION-file-driven or tag-driven release workflow.

3. **Smoke-boot in CI** — Verify packaged artifacts actually launch.

4. **Document the release process** — So future releases are one-click.

### Exit Criteria
- [ ] Packaging produces installable artifacts
- [ ] Release workflow automates the full publish pipeline
- [ ] Packaged app verified in CI

---

## Tips for Success

| Tip | Rationale |
|-----|-----------|
| **Architecture first, code second** | Understanding the system prevents wasted effort |
| **Phase-gate strictly** | Don't advance until exit criteria are met |
| **Name your risks** | Accepted risks are fine; unknown risks are dangerous |
| **Keep copilot-instructions.md current** | Stale context leads to bad Copilot suggestions |
| **Archive, don't delete** | Dead code moved to `legacy/` preserves history |
| **One branch per phase** | Clean PRs with captured evidence of exit criteria |
