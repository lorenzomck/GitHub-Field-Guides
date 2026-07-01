# Model Catalog

## Overview

GitHub Copilot provides access to AI models from multiple providers — OpenAI, Anthropic, Google, Microsoft, and GitHub's own fine-tuned models. This document catalogs every model currently available.

---

## OpenAI Models

| Model | Category | Release Status | Context | Best For |
|-------|----------|----------------|---------|----------|
| **GPT-5 mini** | Lightweight | GA | Standard | Fast, accurate code completions and explanations |
| **GPT-5.3-Codex** | Powerful | GA | Standard | Agentic software development tasks |
| **GPT-5.4** | Versatile | GA | Standard + Long context | Multi-step problem solving, architecture-level analysis |
| **GPT-5.4 mini** | Lightweight | GA | Standard | Codebase exploration, grep-style tool use |
| **GPT-5.4 nano** | Lightweight | GA | Standard | Ultra-low-cost lightweight tasks |
| **GPT-5.5** | Powerful | GA | Standard + Long context | Deep reasoning, complex debugging, advanced architecture |

### Long Context Support
- **GPT-5.4** – Default tier up to 272K tokens; Long context tier above 272K
- **GPT-5.5** – Default tier up to 272K tokens; Long context tier above 272K

---

## Anthropic Models (Claude)

| Model | Category | Release Status | Best For |
|-------|----------|----------------|----------|
| **Claude Haiku 4.5** | Versatile | GA | Fast, reliable answers to lightweight coding questions |
| **Claude Sonnet 4** | Versatile | GA | General-purpose coding, complex problem-solving |
| **Claude Sonnet 4.5** | Versatile | GA | General-purpose coding and agent tasks |
| **Claude Sonnet 4.6** | Versatile | GA | General-purpose coding and agent tasks |
| **Claude Sonnet 5** | Versatile | GA | General-purpose coding and agent tasks (promotional pricing) |
| **Claude Opus 4.5** | Powerful | GA | Complex problem-solving, sophisticated reasoning |
| **Claude Opus 4.6** | Powerful | GA | Complex problem-solving, sophisticated reasoning |
| **Claude Opus 4.7** | Powerful | GA | Complex problem-solving, sophisticated reasoning |
| **Claude Opus 4.8** | Powerful | GA | Complex problem-solving, sophisticated reasoning |
| **Claude Opus 4.8 (fast mode)** | Powerful | Preview | Fast complex reasoning with higher throughput |
| **Claude Fable 5** | Powerful | GA | Long-horizon autonomous coding, first-attempt correctness |

> **Note:** Claude Fable 5 is currently unavailable per Anthropic's announcement. Check back for updates.

---

## Google Models (Gemini)

| Model | Category | Release Status | Context | Best For |
|-------|----------|----------------|---------|----------|
| **Gemini 2.5 Pro** | Powerful | GA | Standard | Complex code generation, debugging, research |
| **Gemini 3 Flash** | Lightweight | Public Preview | Standard | Fast, reliable lightweight coding |
| **Gemini 3.1 Pro** | Powerful | Public Preview | Standard + Long context | Edit-then-test loops with high tool precision |
| **Gemini 3.5 Flash** | Lightweight | GA | Standard | Fast, reliable lightweight coding |

### Long Context Support
- **Gemini 3.1 Pro** – Default tier up to 200K tokens; Long context tier above 200K

---

## Microsoft Models

| Model | Category | Release Status | Best For |
|-------|----------|----------------|----------|
| **MAI-Code-1-Flash** | Lightweight | GA | Fast, cost-efficient code tasks |

---

## GitHub Fine-Tuned Models

| Model | Category | Release Status | Best For |
|-------|----------|----------------|----------|
| **Raptor mini** | Versatile | GA | Optimized for GitHub-specific coding patterns |

---

## Model Categories Explained

| Category | Description | Typical Use |
|----------|-------------|-------------|
| **Lightweight** | Fast, low-cost models for quick tasks | Code completions, simple questions, fast iterations |
| **Versatile** | Balanced capability and cost | General coding, chat, moderate complexity tasks |
| **Powerful** | Highest capability, deeper reasoning | Architecture decisions, complex debugging, autonomous agents |

---

## References

- [Supported AI Models in GitHub Copilot](https://docs.github.com/en/copilot/reference/ai-models/supported-models)
- [AI Model Comparison](https://docs.github.com/en/copilot/reference/ai-models/model-comparison)
