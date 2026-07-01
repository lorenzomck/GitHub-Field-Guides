# Model Comparison and Selection

## Overview

Different models excel at different tasks. This guide helps you choose the right model based on what you're trying to accomplish, not just by comparing specs.

---

## Recommended Models by Task

| Task | Recommended Models | Why |
|------|-------------------|-----|
| **Fast code completions** | GPT-5 mini, Gemini 3.5 Flash, Claude Haiku 4.5 | Low latency, minimal cost, great for inline suggestions |
| **General coding chat** | Claude Sonnet 5, Claude Sonnet 4.6, GPT-5.4 | Balanced quality and speed for everyday coding questions |
| **Complex debugging** | GPT-5.5, Claude Opus 4.8, Gemini 3.1 Pro | Deep reasoning to trace multi-step issues |
| **Architecture analysis** | GPT-5.5, GPT-5.4, Claude Opus 4.7 | Multi-step problem solving, system-level thinking |
| **Agentic tasks** | GPT-5.3-Codex, GPT-5.4 mini, Claude Fable 5 | Autonomous multi-file changes, tool use, test loops |
| **Code review** | Claude Sonnet 5, Claude Sonnet 4.6 | Strong at understanding context and suggesting improvements |
| **Explaining code** | GPT-5 mini, Claude Haiku 4.5 | Quick, clear explanations without overspending |
| **Large codebase exploration** | GPT-5.4 mini, GPT-5.4 (long context) | Effective with grep-style tools and large context windows |
| **Research/documentation** | Gemini 2.5 Pro, GPT-5.4 | Good at synthesizing information and generating docs |
| **Cost-optimized bulk tasks** | GPT-5.4 nano, Raptor mini, GPT-5 mini | Minimal cost per interaction for high-volume work |

---

## Model Selection by Priority

### Priority: Speed (Low Latency)
1. GPT-5.4 nano
2. GPT-5 mini
3. Gemini 3 Flash
4. Claude Haiku 4.5
5. MAI-Code-1-Flash

### Priority: Quality (Best Results)
1. GPT-5.5
2. Claude Opus 4.8
3. Claude Fable 5
4. Claude Sonnet 5
5. Gemini 3.1 Pro

### Priority: Cost Efficiency
1. GPT-5.4 nano ($0.20 input / $1.25 output per 1M tokens)
2. GPT-5 mini ($0.25 / $2.00)
3. Raptor mini ($0.25 / $2.00)
4. Gemini 3 Flash ($0.50 / $3.00)
5. GPT-5.4 mini ($0.75 / $4.50)

### Priority: Autonomous/Agentic Work
1. GPT-5.3-Codex (purpose-built for agents)
2. Claude Fable 5 (first-attempt correctness, parallel tool batching)
3. GPT-5.4 mini (codebase exploration, tool precision)
4. Gemini 3.1 Pro (edit-then-test loops)
5. Claude Opus 4.8

---

## The "Auto" Option

When you select **Auto** in Copilot, the system automatically chooses the optimal model based on:
- Task complexity
- Available context
- Model availability
- Cost optimization

**Benefits of Auto:**
- Discount on model costs (paid plans)
- No decision fatigue
- Adapts per-interaction

**When to override Auto:**
- You need maximum reasoning power (pick GPT-5.5 or Opus 4.8)
- You're cost-sensitive and want the cheapest model
- You have a known preference for a specific model's style
- You need long context (pick a model with Long context tier)

---

## Provider Comparison

| Aspect | OpenAI (GPT) | Anthropic (Claude) | Google (Gemini) | Microsoft/GitHub |
|--------|-------------|-------------------|----------------|-----------------|
| **Strongest area** | Versatility, reasoning | Code quality, safety | Multimodal, speed | Cost efficiency |
| **Model range** | Nano → 5.5 | Haiku → Fable | Flash → Pro | Single model each |
| **Long context** | GPT-5.4, 5.5 (272K+) | Not tiered | Gemini 3.1 Pro (200K+) | N/A |
| **Agentic focus** | Codex, 5.4 mini | Fable 5, Opus | Gemini 3.1 Pro | N/A |
| **Budget option** | 5.4 nano ($0.20) | Haiku ($1.00) | Flash ($0.50) | MAI-Code ($0.75) |
| **Cache pricing** | Input/Cached | Input/Cached/Write | Input/Cached | Input/Cached |

---

## Decision Flowchart

```
Is this a quick/simple task?
├── YES → GPT-5 mini or Claude Haiku 4.5
└── NO → Does it need autonomous/multi-step execution?
    ├── YES → GPT-5.3-Codex or Claude Fable 5
    └── NO → Does it need deep reasoning?
        ├── YES → GPT-5.5 or Claude Opus 4.8
        └── NO → Claude Sonnet 5 or GPT-5.4 (general purpose)
```

---

## References

- [AI Model Comparison – GitHub Docs](https://docs.github.com/en/copilot/reference/ai-models/model-comparison)
- [About Copilot Auto Model Selection](https://docs.github.com/en/copilot/concepts/auto-model-selection)
