# Copilot Instructions Best Practices for Modernization

The `.github/copilot-instructions.md` file is automatically loaded by GitHub Copilot in every interaction. For modernization projects, it serves as the living project brief that keeps AI-assisted work grounded in reality.

---

## Why It Matters for Modernization

Legacy codebases have **hidden context** that isn't obvious from the code alone:

- Which commands actually work (and which are aspirational)
- What's been decided and why
- Which phases are complete vs. in-progress
- Safety constraints and non-negotiable invariants

Without copilot-instructions.md, every Copilot session starts from zero. With it, Copilot inherits the full project state.

---

## Recommended Structure

Based on the [samqbush/doomv2](https://github.com/samqbush/doomv2) reference:

### 1. Project Summary (2-3 paragraphs)

```markdown
## Project Context

[Application name] is being modernized from [old stack] to [new stack].
The [specific component] is dead/deprecated because [reason]. The damage
is contained to [isolation boundary]. The safety strategy is [approach].
```

### 2. Commands Table

Document **only commands that actually work today**. Mark future commands clearly:

```markdown
## Commands

| Action | Command |
|--------|---------|
| Build (current) | `cmake -B build && cmake --build build` |
| Run tests | `ctest --test-dir build` |
| Build (legacy, reference only) | `cd legacy/ && make` |
| Lint *(introduced Phase 4)* | `npm run lint` |
```

> **Key principle:** Never invent a command you haven't verified against the current tree.

### 3. Phase Gating Rules

```markdown
## Phase Gating

A phase is not complete until its exit criteria are:
1. Objectively verifiable — runnable commands, not subjective "looks done"
2. Actually executed and recorded
3. Gated — do not advance to phase N+1 until phase N passes
```

### 4. Decisions Log

Record what's been decided so Copilot doesn't re-litigate:

```markdown
## Decisions

- Language stays **TypeScript** (not a rewrite to Rust)
- Backend migrates to **Express → Fastify**
- Database stays **PostgreSQL** (no ORM change)
- Legacy SOAP API is **archived, not ported**
- GraphQL layer is **deferred** to post-modernization
```

### 5. Branching & Release Rules

```markdown
## Branching

Each phase is developed on its own branch. Never commit phase work to main.
Green CI on the PR is the authoritative signal for merge readiness.
```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad |
|--------------|--------------|
| Listing aspirational commands as if they exist | Copilot will try to run them and get confused |
| Omitting "what's dropped" | Copilot may try to modernize dead code |
| Static instructions that never update | Phase 3 Copilot shouldn't think it's still in Phase 1 |
| Vague constraints ("be careful") | Copilot needs specific rules it can follow |
| Including entire architecture docs inline | Keep it focused; reference external docs |

---

## Updating Over Time

The copilot-instructions.md should evolve as the project progresses:

1. **Phase transitions** — Update the commands table when new commands become available
2. **New decisions** — Add decisions as they're made (don't backtrack)
3. **Risk resolution** — Move resolved risks from "open" to "closed"
4. **CI status** — Note when CI becomes a required check

---

## Template

```markdown
# Copilot Instructions — [Project Name] Modernization

> [One-sentence summary of the modernization goal]

## Context

[2-3 paragraphs: what the project is, why it's being modernized, what the
safety strategy is, and where the current phase stands]

## Commands

| Action | Command |
|--------|---------|
| Build | `...` |
| Test | `...` |
| Run | `...` |

> Never invent a command you haven't verified.

## Phase Gating

[Rules for when a phase is complete]

## Decisions

- [Decision 1]
- [Decision 2]

## Branching & Releases

[How branches and releases work]
```
