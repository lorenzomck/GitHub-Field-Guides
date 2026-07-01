# How to Build Autonomous Agent Loops

> A practitioner's guide to hypothesis-driven, self-measuring, self-correcting agent systems.

---

## The Core Idea

You build something. You measure it. You score it against a rubric. You find the weakest point. You fix that one thing. You measure again. The loop runs until a ship gate passes or you hit a wall.

---

## 1. The Loop

```
BUILD → MEASURE → SCORE → DIAGNOSE → FIX → MEASURE AGAIN
  ↑                                              |
  └──────────────────────────────────────────────┘
```

**Phases:**

| Phase | What happens | Gate to proceed |
|---|---|---|
| 0: Pre-flight | Verify env, set hypotheses, confirm rubrics | All paths exist |
| 1: Build | Construct the thing (or skip if it exists) | ≥1 build above rubric threshold |
| 2: Measure | Static analysis, line counts, schema checks | Scores + manifest on disk |
| 3: Diagnose | Compare to baselines, find weakest point | Rework dispatched or threshold met |
| 4: Learn | Update learnings file, memorialize evidence | Learnings + roadmap on disk |
| 5: Test | Functional/quality testing against real targets | ≥1 quality score on disk |
| 6: Ship gate | Multi-layer pass/fail decision | All layers pass → ship |
| 7: Post-run | Diagnostics, handoff, session log parsing | Always runs |

**Key rule: every phase gate checks files on disk.** Not self-reports. Not summaries. `ls` and `wc -c`.

---

## 2. Hypothesis Testing

Every loop iteration should test at least one hypothesis. Untested hypothesis count must not grow between runs.

### How to write a hypothesis

```
ID:        MH-4
Statement: Critic pass finds ≥3 defects
Threshold: ≥3 defects found OR <3 defects found
Status:    untested → confirmed/falsified/partially tested
Evidence:  run ID + specific artifacts
```

### Rules

1. **Always pick the cheapest untested hypothesis first.**
2. **A hypothesis needs a pass/fail threshold before you test it.**
3. **Record the result even if it's negative.** Falsified hypotheses are data, not failures.
4. **Maintain a ledger.** Track status across runs so nothing falls through cracks.

---

## 3. Rubric Scoring

### Why rubrics

Vibes don't compound. Rubrics do.

A rubric converts "this feels better" into "this scores 34/41, up from 31/41, because tier-2 criteria C8 went from FAIL to PASS after the prompt fix."

### The scoring stack (layered, cheapest first)

| Layer | What it measures | Cost |
|---|---|---|
| **Structural rubric** | Does the build have the right files/structure? | Free |
| **Chain test** | Does it execute without crashing? | ~20 min |
| **Heuristic scorer** | Are structural quality markers present? (41 criteria) | ~5 sec |
| **LLM-as-judge** | Is the content semantically correct? (5 dims × 5pt) | ~5 min |
| **Panel of judges** | Multi-perspective semantic scoring (3 judges × 5 dims) | ~15 min |

**Run them in order.** If structural rubric fails, don't waste tokens on LLM judges.

### Heuristic tiers

```
Tier 1 (26 pts): Structural — files exist, sections present, format correct
Tier 2 (10 pts): Content — depth, specificity, actionability
Tier 3 (5 pts):  Semantic — path validity, patch context, no hallucination
```

### Ship gate (multi-layer)

All layers must pass on ≥3 build×target pairs to ship:

| Layer | Threshold |
|---|---|
| Build structure rubric | ≥24/30 |
| Chain test | ≥4/6 PASS |
| Heuristic tier-1 | ≥22/26 |
| Heuristic tier-2 | ≥6/10 |
| Heuristic tier-3 | ≥3/5 |
| Panel median | ≥15/25 |
| No single judge | ≥10/25 |

---

## 4. The Build/Test/Measure/Rework Loop

Run up to 5 iterations:

1. **Identify the weakest scoring dimension.**
2. **Make exactly one targeted change.**
3. **Document what you changed and why.**
4. **Re-measure.** Compare before/after.
5. **Decide:** improved → continue or ship. Regressed → rollback. Flat for 2 cycles → stop.

### Decision table

| Condition | Action |
|---|---|
| All gates pass on ≥3 pairs | **Ship** |
| Heuristic tier-2 < 6/10 | Fix advisor prompts |
| LLM-judge < 15/25 | Fix advisor prompts |
| Rubric < 24/30 | Fix build structure |
| Rubric unchanged 2 cycles | Stop. Ship current best. |
| Rubric regressed | Rollback immediately |

---

## 5. Feed-Forward Learnings

Every run should produce at least one lesson that makes the next run better.

### The LEARNINGS.md pattern

```markdown
| ID | Lesson | Evidence | Fix |
|---|---|---|---|
| L1 | Analysis without action is the default failure mode | 3 runs: 2000+ lines, 0 files modified | 40/60 rule enforcement |
```

### Design patterns that correlate with quality

| Pattern | Why it works |
|---|---|
| File-driven config (not "ask the user") | Agents in non-interactive mode can't ask; file reads always work |
| Explicit stop rules per prompt | Prevents runaway output |
| Named agent files with `model:` field | Without this, fleet sub-agents silently default to cheapest model |
| Multi-pass pipeline with critic | Critic found 3 real defects that single-pass missed |

### Anti-patterns

| Anti-pattern | What goes wrong |
|---|---|
| "Ask the user" in non-interactive context | Agent profiles itself instead of the target |
| Cutting untested features | 0 invocations = 0 tests ≠ 0 value |
| Synthesis that accepts cuts uncritically | Competitive framing collapses into mediocre compromise |
| Trusting self-reports over disk verification | Agents claim "quality testing complete" with 0 files on disk |

---

## 6. Competitive Builds

Run the same task on multiple models/surfaces and compare:

| Build | Model | Surface | Rubric | Heuristic |
|---|---|---|---|---|
| B1 | GPT-5.3-Codex | Codex-app | baseline | — |
| B2 | GPT-5.2-Codex | Fleet | baseline | — |
| B3 | Opus 4.6 | Fleet | 24/30 | 31.3/36 |
| B4 | Opus 4.6 | Autopilot | **30/30** | **34.7/36** |

### Why this works

- Different models emphasize different concerns
- The rubric makes comparison objective
- "Innovation lives in the losers" — lowest-ranked builds often produce the most architecturally novel features
- Competitive planning followed by synthesis produces better plans than single-model planning

---

## 7. The Outer Loop / Inner Loop Architecture

```
OUTER LOOP (orchestrator)
  ├── dispatches inner CLI sessions
  ├── reads their artifacts + session logs
  ├── cross-checks self-reports against ground truth
  ├── scores at every phase gate
  └── decides: re-run, fix, or ship

INNER LOOP (worker agent)
  ├── receives a specific task prompt
  ├── executes with --allow-all --no-ask-user
  ├── produces artifacts + self-diagnostic
  └── stdout captured via tee
```

### The dispatch pattern

```bash
copilot -p "$(cat <prompt>)" \
  --allow-all --no-ask-user \
  2>&1 | tee runs/$RUN_ID/<phase>-stdout.txt
```

### Cross-check protocol

After every inner session:
1. Read the agent's self-diagnostic
2. Parse the session log for actual tool call counts
3. Compare: did it do what it said it did?
4. If discrepancy > threshold, flag and investigate

---

## 8. Key Principles

1. **Execution first.** ≤40% of effort on analysis, ≥60% on building and measuring.
2. **Trust disk, not self-reports.** Verify artifacts exist with `ls` and `wc -c`.
3. **Cheapest test first.** Run free structural checks before burning tokens on LLM judges.
4. **One fix per iteration.** Change one thing, measure the delta.
5. **Rubrics over vibes.** Convert subjective quality assessments into deterministic, numbered criteria.
6. **Feed forward everything.** Every run produces lessons that get read at Phase 0 of the next run.
7. **Falsification > confirmation.** A falsified hypothesis saves work.
8. **Ship gates must be multi-layer.** Structural PASS ≠ quality.
9. **Competitive diversity, not consensus.** Cherry-pick from all builds.
10. **Stop rules prevent runaway.** Cap iterations, output, and time.

---

## 9. Quickstart: Your First Autonomous Loop

```bash
# 1. Set up the run
RUN_ID=$(date -u '+%Y%m%dT%H%M%SZ')
mkdir -p runs/$RUN_ID/{diagnostics,scores,quality}

# 2. Build (or point at existing build)
BUILD_PATH=~/repos/my-agent-project

# 3. Measure — structural rubric
bash scripts/score-rubric.sh $BUILD_PATH > runs/$RUN_ID/scores/rubric.md

# 4. Test — run against a real target
bash scripts/run-advisor-chain-test.sh $BUILD_PATH targets/T4 \
  runs/$RUN_ID/quality/B-T4 T4

# 5. Score — heuristic quality
bash scripts/score-output-quality.sh "$ARTIFACTS" targets/T4 \
  runs/$RUN_ID/quality/B-T4/heuristic-scores.md

# 6. Diagnose — read the scores, find the weakest criterion
cat runs/$RUN_ID/quality/B-T4/heuristic-scores.md

# 7. Fix — edit the specific prompt/agent that governs the weakest criterion

# 8. Repeat from step 3
```

---

## 10. The Full Autonomous Launch

```bash
cd ~/repos/<workflow-repo>
RUN_ID=$(date -u '+%Y%m%dT%H%M%SZ')
mkdir -p runs/$RUN_ID/{diagnostics,scores,data,quality}

copilot --model gpt-5.5 \
  -p "Your run ID is $RUN_ID. $(cat .github/agents/outer-loop-orchestrator.agent.md)" \
  --allow-all --no-ask-user \
  2>&1 | tee runs/$RUN_ID/orchestrator-stdout.txt
```

The orchestrator reads STATUS.md, skips completed phases, runs whatever's next, scores at every gate, and produces a handoff for the next run.

---

*This guide is a living document. Update as the loop evolves and new learnings emerge.*
