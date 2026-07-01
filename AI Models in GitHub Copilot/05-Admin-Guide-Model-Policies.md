# Admin Guide – Model Policies

## Overview

Enterprise and organization administrators can control which AI models are available to their users, set spending policies, and monitor usage. This document covers model governance for Copilot Business and Enterprise plans.

---

## Model Access Controls

### What Admins Can Configure

| Setting | Level | Description |
|---------|-------|-------------|
| **Enable/disable models** | Enterprise or Org | Restrict which models users can select |
| **Default model** | Org | Set the default model for new users (Auto recommended) |
| **Spending limits** | Enterprise or Org | Cap total AI credit consumption |
| **Usage alerts** | Enterprise or Org | Notifications when usage thresholds are hit |
| **Model category restrictions** | Enterprise or Org | Allow only Lightweight/Versatile, block Powerful tier |

### How to Configure

1. Navigate to **Enterprise Settings** → **Copilot** → **Policies**
2. Under **Model access**, enable or disable specific models
3. Under **Spending**, set monthly credit limits
4. Under **Defaults**, choose the default model selection

---

## Recommended Policies by Organization Type

### Cost-Conscious Teams
| Policy | Setting |
|--------|---------|
| Default model | Auto |
| Allowed categories | Lightweight + Versatile |
| Powerful models | Disabled or request-only |
| Spending limit | $20–30/user/month |

### Standard Development Teams
| Policy | Setting |
|--------|---------|
| Default model | Auto |
| Allowed categories | All |
| Spending limit | $50–75/user/month |
| Monitoring | Weekly usage reports |

### AI-Heavy / Research Teams
| Policy | Setting |
|--------|---------|
| Default model | Auto |
| Allowed categories | All (including preview) |
| Spending limit | $150–200/user/month or unlimited |
| Monitoring | Daily usage dashboards |

---

## Usage Monitoring

### What You Can Track

| Metric | Where to Find It |
|--------|-----------------|
| Total AI credits consumed | Enterprise billing dashboard |
| Credits by user | User-level usage report |
| Credits by model | Model breakdown view |
| Credits by team/org | Organization-level reporting |
| Trend over time | Usage graphs (daily/weekly/monthly) |
| Top consumers | Ranked user list |

### Setting Up Alerts

Configure alerts to be notified when:
- Organization pool reaches 75% / 90% / 100% of budget
- Individual user exceeds a threshold
- Expensive model usage spikes unexpectedly

---

## Model Governance Best Practices

### 1. Start with Auto as Default
- Reduces cost through built-in discounts
- Prevents "model shopping" behavior
- Still allows override when genuinely needed

### 2. Educate Before Restricting
- Share the [Model Comparison](02-Model-Comparison-and-Selection.md) doc with developers
- Most will self-optimize when they understand the cost/quality tradeoffs
- Restriction without education causes frustration

### 3. Phase Expensive Models
- **Week 1–2:** All models available (learning period)
- **Week 3–4:** Review usage data, identify patterns
- **Month 2+:** Apply policies based on actual usage, not assumptions

### 4. Use Pool Benefits (Business/Enterprise)
- Credits are pooled at the org/enterprise level
- Heavy users are offset by light users
- Avoid per-user hard caps which frustrate power users

### 5. Review Monthly
- Check if spending aligns with productivity gains
- Adjust limits based on team growth and usage patterns
- Report ROI metrics alongside cost metrics

---

## Handling Overage

When the credit pool is exhausted:

| Configuration | What Happens |
|---------------|-------------|
| **Hard limit set** | Users are blocked from premium models until next cycle |
| **Soft limit (alert only)** | Usage continues, admin is notified, overage is billed |
| **No limit** | Usage continues, billed at standard overage rates |

### Overage Billing
- Overage is billed at the same per-token rates shown in [Pricing and AI Credits](03-Pricing-and-AI-Credits.md)
- Billed to the enterprise account
- Appears on the next invoice cycle

---

## Preview / Experimental Models

Some models are in **Public Preview**:
- Gemini 3 Flash
- Gemini 3.1 Pro
- Claude Opus 4.8 (fast mode)

Admin considerations:
- Preview models may change behavior between updates
- Preview models are generally available to all plan types
- You can disable preview models if you need stability guarantees
- Preview model pricing may change at GA

---

## Compliance Considerations

| Concern | Guidance |
|---------|----------|
| **Data residency** | All models process data through GitHub's infrastructure; no data is sent directly to providers |
| **Data retention** | Prompts and responses are not stored beyond the session (Business/Enterprise) |
| **Training exclusion** | Business/Enterprise data is never used to train models |
| **IP indemnity** | Applies to output regardless of which model generates it |
| **Audit logging** | Model selection is captured in audit logs |

---

## Quick Setup Checklist

| Task | Status |
|------|--------|
| Set default model to Auto | ⬜ |
| Review and enable appropriate models | ⬜ |
| Configure spending limits | ⬜ |
| Set up usage alerts (75%, 90%, 100%) | ⬜ |
| Communicate model policy to developers | ⬜ |
| Schedule monthly usage review | ⬜ |
| Document model policy in internal wiki | ⬜ |

---

## References

- [Managing Copilot Policies](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise/managing-policies-and-features-for-copilot-in-your-enterprise)
- [Models and Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- [AI Model Comparison](https://docs.github.com/en/copilot/reference/ai-models/model-comparison)
- [Usage-Based Billing for Organizations](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
