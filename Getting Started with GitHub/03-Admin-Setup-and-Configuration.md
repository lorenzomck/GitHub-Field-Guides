# Admin Setup and Configuration

## Overview

This guide walks enterprise and organization administrators through the key steps to configure GitHub for your OpCo. Follow this checklist to ensure a secure, well-governed setup from day one.

---

## Phase 1: Enterprise Account Setup

### 1.1 Create Your Enterprise
- Navigate to [github.com/enterprise](https://github.com/enterprise) or use your trial/paid license
- Choose a clear enterprise name (e.g., `your-company-name`)
- Designate at least **two enterprise owners** for redundancy

### 1.2 Configure Identity & Access
- **SAML SSO** – Integrate with your Identity Provider (Azure AD, Okta, OneLogin, PingIdentity)
- **SCIM Provisioning** – Enable automatic user provisioning and de-provisioning
- **Enterprise Managed Users (EMU)** – Consider EMU if you want full lifecycle control from your IdP

### 1.3 Set Enterprise Policies
- Repository creation permissions (who can create repos)
- Repository visibility defaults (private by default recommended)
- Fork permissions (restrict or allow)
- Actions permissions (which actions can run, allowed repositories)
- Copilot settings (enable/disable, which users get access)

---

## Phase 2: Organization Structure

### 2.1 Create Organizations
- Create one or more organizations under your enterprise (e.g., by business unit, product, or team)
- Each org has its own repositories, teams, and settings

### 2.2 Set Up Teams
- Create teams that mirror your organizational structure
- Use nested teams for hierarchical permissions
- Map teams to repositories with appropriate access levels:
  - **Read** – View and clone
  - **Triage** – Manage issues/PRs without write access
  - **Write** – Push to non-protected branches
  - **Maintain** – Manage repo without admin access
  - **Admin** – Full repository control

### 2.3 Configure Repository Defaults
- Set default branch name (e.g., `main`)
- Create repository templates for standardized project setup
- Configure `.github` repository for org-wide issue/PR templates and workflows

---

## Phase 3: Security Configuration

### 3.1 Authentication Security
- [ ] Enforce SAML SSO for all organization members
- [ ] Require 2FA for any users not covered by SSO
- [ ] Configure IP allow lists (restrict access to corporate networks/VPN)
- [ ] Set session timeout policies

### 3.2 Repository Security
- [ ] Enable branch protection rules on `main`/`release` branches
  - Require pull request reviews (minimum 1–2 reviewers)
  - Require status checks to pass before merging
  - Require signed commits (optional but recommended)
  - Prevent force pushes and deletions
- [ ] Enable secret scanning and push protection
- [ ] Enable Dependabot alerts and security updates
- [ ] Enable code scanning with CodeQL (if Advanced Security licensed)

### 3.3 Audit and Monitoring
- [ ] Configure audit log streaming (Splunk, Datadog, S3, Azure Blob)
- [ ] Set up webhook notifications for critical events
- [ ] Review audit log regularly for anomalies

---

## Phase 4: CI/CD and Automation

### 4.1 GitHub Actions
- Configure organization-level runner groups (self-hosted or GitHub-hosted)
- Set allowed actions policies (GitHub-authored only, verified marketplace, or specific actions)
- Create reusable workflows for common CI/CD patterns
- Set spending limits for Actions minutes

### 4.2 GitHub Apps and Integrations
- Review and approve third-party GitHub Apps at the org level
- Use GitHub Apps over OAuth Apps for better security and granularity
- Document approved integrations for your teams

---

## Phase 5: Onboarding Users

### 5.1 Automated Onboarding
- SSO/SCIM handles account creation automatically
- Team sync maps IdP groups to GitHub teams
- New users are auto-assigned to appropriate teams and repos

### 5.2 Developer Documentation
- Create an internal onboarding guide with:
  - How to access GitHub (SSO login URL)
  - Team and repository naming conventions
  - Branching strategy and PR workflow
  - Required tools (Git, CLI, IDE extensions)
  - Where to get help

### 5.3 Offboarding
- SCIM automatically removes access when users are deactivated in your IdP
- Conduct periodic access reviews (quarterly recommended)
- Transfer repository ownership before removing admin users

---

## Admin Checklist Summary

| Category | Task | Status |
|----------|------|--------|
| Identity | Configure SAML SSO | ⬜ |
| Identity | Enable SCIM provisioning | ⬜ |
| Security | Enforce 2FA or SSO | ⬜ |
| Security | Set IP allow lists | ⬜ |
| Security | Enable branch protections | ⬜ |
| Security | Enable secret scanning | ⬜ |
| Security | Configure audit log streaming | ⬜ |
| Governance | Set repository visibility defaults | ⬜ |
| Governance | Define Actions policies | ⬜ |
| Governance | Designate 2+ enterprise owners | ⬜ |
| Onboarding | Create developer onboarding docs | ⬜ |
| Onboarding | Configure team sync | ⬜ |

---

## References

- [Enterprise Admin Documentation](https://docs.github.com/en/enterprise-cloud@latest/admin)
- [Managing SAML SSO](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management)
- [Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-a-branch-protection-rule)
- [Audit Log](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise)
