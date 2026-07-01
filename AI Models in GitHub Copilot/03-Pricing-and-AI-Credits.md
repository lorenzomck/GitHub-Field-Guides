# Pricing and AI Credits

## Overview

GitHub Copilot uses an **AI Credits** billing system. Every interaction consumes tokens, which are converted into AI credits at model-specific rates. This document explains how pricing works and how to manage costs.

---

## How AI Credits Work

- **1 AI Credit = $0.01 USD**
- Each interaction consumes **input tokens** (your prompt) + **output tokens** (the model's response)
- Some models support **cached tokens** (reused context) at discounted rates
- Anthropic models additionally charge for **cache writes**
- Total cost = tokens consumed × per-token rate for that model

---

## What's Included (Free with Your Plan)

| Plan | Included AI Credits | Code Completions |
|------|-------------------|------------------|
| Copilot Free | Limited allowance | 2,000/month |
| Copilot Pro ($10/mo) | ~$15 value/month | Unlimited |
| Copilot Pro+ ($39/mo) | ~$70 value/month | Unlimited |
| Copilot Max ($100/mo) | ~$200 value/month | Unlimited |
| Copilot Business ($19/user/mo) | $19/user pooled | Unlimited |
| Copilot Enterprise ($39/user/mo) | $39/user pooled | Unlimited |

> **Important:** Code completions and next-edit suggestions are **not billed** in AI credits. They remain unlimited on all paid plans.

---

## Pricing Tables (Per 1 Million Tokens)

### OpenAI

| Model | Input | Cached Input | Output | Category |
|-------|------:|-------------:|-------:|----------|
| GPT-5.4 nano | $0.20 | $0.02 | $1.25 | Lightweight |
| GPT-5 mini | $0.25 | $0.025 | $2.00 | Lightweight |
| GPT-5.4 mini | $0.75 | $0.075 | $4.50 | Lightweight |
| GPT-5.3-Codex | $1.75 | $0.175 | $14.00 | Powerful |
| GPT-5.4 | $2.50 | $0.25 | $15.00 | Versatile |
| GPT-5.4 (long context) | $5.00 | $0.50 | $22.50 | Versatile |
| GPT-5.5 | $5.00 | $0.50 | $30.00 | Powerful |
| GPT-5.5 (long context) | $10.00 | $1.00 | $45.00 | Powerful |

### Anthropic

| Model | Input | Cached Input | Cache Write | Output | Category |
|-------|------:|-------------:|------------:|-------:|----------|
| Claude Haiku 4.5 | $1.00 | $0.10 | $1.25 | $5.00 | Versatile |
| Claude Sonnet 5 ⭐ | $2.00 | $0.20 | $2.50 | $10.00 | Versatile |
| Claude Sonnet 4/4.5/4.6 | $3.00 | $0.30 | $3.75 | $15.00 | Versatile |
| Claude Opus 4.5–4.8 | $5.00 | $0.50 | $6.25 | $25.00 | Powerful |
| Claude Opus 4.8 (fast) | $10.00 | $1.00 | $12.50 | $50.00 | Powerful |
| Claude Fable 5 | $10.00 | $1.00 | $12.50 | $50.00 | Powerful |

> ⭐ Claude Sonnet 5 promotional pricing through August 31, 2026.

### Google

| Model | Input | Cached Input | Output | Category |
|-------|------:|-------------:|-------:|----------|
| Gemini 3 Flash | $0.50 | $0.05 | $3.00 | Lightweight |
| Gemini 2.5 Pro | $1.25 | $0.125 | $10.00 | Powerful |
| Gemini 3.5 Flash | $1.50 | $0.15 | $9.00 | Lightweight |
| Gemini 3.1 Pro | $2.00 | $0.20 | $12.00 | Powerful |
| Gemini 3.1 Pro (long context) | $4.00 | $0.40 | $18.00 | Powerful |

### Microsoft & GitHub

| Model | Input | Cached Input | Output | Category |
|-------|------:|-------------:|-------:|----------|
| Raptor mini (GitHub) | $0.25 | $0.025 | $2.00 | Versatile |
| MAI-Code-1-Flash | $0.75 | $0.075 | $4.50 | Lightweight |

---

## Cost Examples

### Typical Interactions (Approximate)

| Interaction Type | Typical Tokens | With GPT-5 mini | With Claude Sonnet 5 | With GPT-5.5 |
|------------------|---------------|-----------------|---------------------|--------------|
| Quick question (~500 in / 200 out) | 700 total | ~$0.0005 | ~$0.003 | ~$0.009 |
| Code explanation (~2K in / 1K out) | 3K total | ~$0.003 | ~$0.014 | ~$0.035 |
| Agent task (~10K in / 5K out) | 15K total | ~$0.013 | ~$0.070 | ~$0.175 |
| Large refactor (~50K in / 20K out) | 70K total | ~$0.053 | ~$0.300 | ~$0.850 |

### Monthly Budget Estimates (Per Developer)

| Usage Level | Cheap Models | Mid-Tier | Premium Models |
|-------------|-------------|----------|----------------|
| Light (20 interactions/day) | ~$2–5/mo | ~$10–20/mo | ~$30–50/mo |
| Moderate (50 interactions/day) | ~$5–12/mo | ~$25–50/mo | ~$75–125/mo |
| Heavy/Agentic (100+ interactions/day) | ~$12–25/mo | ~$50–100/mo | ~$150–300/mo |

---

## Cost Management Strategies

### For Admins
1. **Use Auto model selection** – Gets a discount and optimizes model choice per task
2. **Set spending limits** – Configure at the enterprise/org level
3. **Monitor usage dashboards** – Track credits consumed by team/user
4. **Restrict expensive models** – Limit access to Powerful-tier models if needed
5. **Pool credits (Business/Enterprise)** – Credits are pooled org-wide, smoothing individual spikes

### For Developers
1. **Let Auto choose** – Usually the most cost-effective option
2. **Use lightweight models for simple tasks** – Don't use GPT-5.5 for a quick question
3. **Be concise in prompts** – Fewer input tokens = lower cost
4. **Leverage caching** – Repeated context is charged at 10% of input rate
5. **Use code completions freely** – They're unlimited and don't consume credits

---

## Business/Enterprise Pooled Credits

For Copilot Business ($19/user/mo) and Enterprise ($39/user/mo):
- Credits are **pooled** at the billing entity (org or enterprise) level
- Individual users don't have hard caps — the pool absorbs spikes
- Admins can monitor pool consumption and set alerts
- Overage beyond the pool is billed at standard per-token rates

---

## References

- [Models and Pricing – GitHub Docs](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- [Usage-Based Billing for Organizations](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Usage-Based Billing for Individuals](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-individuals)
