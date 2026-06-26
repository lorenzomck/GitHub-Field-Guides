# AICOE Governance Checklist

Use this checklist when evaluating, approving, or reviewing AI use cases and tools.

---

## 1. Responsible AI

- [ ] Use case has a clear problem statement and intended benefit
- [ ] Potential for bias, fairness issues, or harmful outputs has been assessed
- [ ] Human oversight mechanism is defined (human-in-the-loop or human-on-the-loop)
- [ ] Transparency: users know when they are interacting with AI-generated content
- [ ] Limitations and failure modes are documented
- [ ] Aligns with [Microsoft Responsible AI Principles](https://www.microsoft.com/en-us/ai/responsible-ai-resources)

## 2. Privacy & Data Protection

- [ ] No personal data is sent to external AI services without approval
- [ ] Data classification of inputs/outputs is documented (public, internal, confidential, restricted)
- [ ] Data residency requirements are met
- [ ] Retention and deletion policies are defined
- [ ] Privacy impact assessment completed (if required)
- [ ] Compliant with relevant regulations (GDPR, CCPA, etc.)

## 3. Security

- [ ] Tool or service has been reviewed by security team
- [ ] Authentication and access controls are in place
- [ ] No secrets, credentials, or sensitive code are exposed to AI services
- [ ] Network and firewall configuration reviewed (see [Copilot firewall docs](https://docs.github.com/en/enterprise-cloud@latest/copilot/responsible-use/agents))
- [ ] Supply chain risks assessed (third-party models, plugins, MCP servers)
- [ ] Logging and audit trail enabled

## 4. Approval & Ownership

- [ ] Use-case owner identified and accountable
- [ ] Appropriate approval level obtained:
  - Low risk: AICOE Lead approval
  - Medium risk: Governance Board approval
  - High risk: Executive Sponsor + Legal/Compliance sign-off
- [ ] Approval documented with date and rationale
- [ ] Review date set (recommend every 6 months or on material change)

## 5. Usage Review & Monitoring

- [ ] Usage metrics are being collected (adoption, volume, errors)
- [ ] Output quality is being monitored (spot checks, user feedback)
- [ ] Incident response process is defined for AI-related issues
- [ ] Escalation path is clear
- [ ] Regular review scheduled (monthly for active use cases)
- [ ] Decommissioning criteria defined (when to stop or replace)

---

## Quick Reference: Risk Levels

| Level | Examples | Approval |
|-------|----------|----------|
| **Low** | Code completion, documentation drafting, test generation | AICOE Lead |
| **Medium** | Customer-facing content generation, data analysis with internal data | Governance Board |
| **High** | Decisions affecting people, use of restricted data, autonomous actions | Executive + Legal |

---

## Related Resources

- [GitHub Copilot Trust Center](https://copilot.github.trust.page/)
- [GitHub Copilot Agents – Responsible Use](https://docs.github.com/en/enterprise-cloud@latest/copilot/responsible-use/agents)
- [Microsoft Responsible AI Resources](https://www.microsoft.com/en-us/ai/responsible-ai-resources)

---

*Review and update this checklist quarterly. Adapt risk levels to your organization's risk appetite.*
