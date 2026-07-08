# Reference Walkthrough — DOOM Modernization

## Repository: [samqbush/doomv2](https://github.com/samqbush/doomv2)

## What the Repo Owner Demonstrated

Sam Bush used GitHub Copilot's agentic capabilities to modernize the original 1997 id Software DOOM source code (Linux 1.10 release) — a codebase with a **completely dead toolchain** that cannot compile on any modern system. The demonstration showcases how Copilot can drive an end-to-end legacy modernization from architecture analysis through packaged releases.

---

## The Starting Point

| Attribute | State |
|-----------|-------|
| **Language** | C (K&R-era patterns, pre-C99) |
| **Build system** | GNU Make targeting 1997 Linux |
| **Platform deps** | X11 8-bit PseudoColor, OSS `/dev/dsp`, libc5 headers |
| **Tests** | None |
| **CI/CD** | None |
| **Documentation** | Minimal README from id Software's 1997 release |
| **Can it compile?** | ❌ Fails immediately (`values.h` not found) |
| **Can it run?** | ❌ Even if compiled, X11 PseudoColor and OSS are gone |

---

## What Copilot Produced

### 1. Architecture Analysis (`ARCHITECTURE.md`)

Copilot analyzed the entire legacy codebase and produced a comprehensive architecture reference document covering:

- Complete module map with dependency relationships
- Platform isolation layer identification (all OS deps in `i_*` files only)
- Data contracts (WAD format, framebuffer spec)
- Determinism model (35Hz tics, fixed RNG, `ticcmd_t` streams)
- Developer gotchas and porting surface measurements

**Why this matters:** This replaced weeks of manual reverse engineering. The architecture doc became the foundation for all subsequent planning.

### 2. Modernization Plan (`MODERNIZATION_PLAN.md`)

A 38,000-word phased migration plan with:

- **Feasibility spike** — actual compiler output analysis, not speculation
- **Per-component migration strategies** (freeze-then-lift vs. beachhead rewrite)
- **6 phases** with explicit entry/exit criteria
- **Safety ladder** (L0–L4 rungs per component)
- **Testability milestones** — when each component becomes verifiable
- **Residual risk register** — named and accepted risks
- **Open questions** requiring human decision

### 3. Copilot Instructions (`.github/copilot-instructions.md`)

A detailed project brief that encodes:

- Available commands (with phase-introduction tracking)
- Phase-gating rules (dark vs. lit phases)
- Branching strategy
- Release workflow (VERSION-bump-driven)
- Decisions already made (C11, SDL2, CMake, what's dropped vs. deferred)
- Constraints (no external source ports, self-contained oracle)

**Why this matters:** Every subsequent Copilot session inherits full project context, enabling consistent multi-session modernization without re-explaining the codebase.

### 4. Phased Implementation

| Phase | What Was Done | Key Copilot Contribution |
|-------|---------------|--------------------------|
| **Phase 0** | Archive dead code (`ipx/`, `sersrc/`, `sndserv/`), capture seam snapshots | Identified what to archive vs. port |
| **Phase 1** | CMake build system, 64-bit correctness pass, header shims | Generated modern CMakeLists.txt, fixed pointer/int assumptions |
| **Phase 2** | SDL2 video beachhead — first frame renders | Rewrote `i_video.c` from X11 to SDL2, created demo-parity oracle |
| **Phase 3** | CI/CD pipeline, interactive play verification | Generated GitHub Actions workflows (ci.yml), test harness |
| **Phase 4** | SDL2 audio (in-process, replacing `sndserv`) | Rewrote `i_sound.c`, retired separate sound server process |
| **Phase 5** | POSIX BSD-sockets networking | Ported `i_net.c` from raw UDP to portable POSIX sockets |

### 5. Testing & CI/CD

Built from zero to a full verification suite:

- **Demo-parity test** — replays a v1.10 demo, asserts world-state checksum
- **Frame-hash smoke** — byte-exact framebuffer verification
- **Palette-LUT gate** — validates color conversion correctness
- **Net-loopback test** — 2-process lockstep verification over localhost UDP
- **CI on Linux + macOS** — every push and PR runs the full parity gate

### 6. Release Automation

- **VERSION-file-driven releases** — bump VERSION in PR, merge triggers release
- **Cross-platform packaging** — macOS `.dmg` and Linux `.AppImage`
- **Smoke-boot verification** — packaged app is launched headlessly in CI
- **GPL source archive** — corresponding-source bundle attached automatically

---

## Key Patterns Demonstrated

### Pattern 1: Architecture-First Discovery
Before writing any code, Copilot produced a full architecture analysis. This established the "map" that guided all subsequent work.

### Pattern 2: Phased Migration with Safety Gates
Each phase has explicit exit criteria. Pre-testability ("dark") phases exit on captured snapshots; post-testability ("lit") phases exit on green CI.

### Pattern 3: Self-Contained Oracle Strategy
Rather than depending on external reference implementations, the project builds its own behavioral oracle — the engine's recorded demo becomes its own golden master.

### Pattern 4: Living Project Context
The copilot-instructions.md file evolves with the project. Commands are added as phases introduce them. This keeps every Copilot interaction grounded.

### Pattern 5: Dead Code Triage
Not everything gets modernized. DOS drivers are archived, the separate sound server is collapsed into the main process, and GPU rendering is explicitly deferred.

---

## Applicability to Enterprise Modernization

| DOOM Modernization | Enterprise Equivalent |
|----|-----|
| X11/OSS platform layer rewrite | Legacy API/middleware migration (CORBA, COM, DCOM) |
| Demo-replay oracle | Transaction replay / regression suites |
| Phase-gated migration | Strangler fig pattern with verification gates |
| Copilot-instructions.md | Institutional knowledge capture for AI-assisted dev |
| VERSION-driven releases | GitOps release automation |
| Archive dead code | Decommission legacy integrations |
