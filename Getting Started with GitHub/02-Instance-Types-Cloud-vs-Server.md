# Instance Types – Cloud vs Server

## Overview

GitHub Enterprise is available in two deployment models. This document helps your OpCo decide which is right based on compliance, infrastructure, and operational requirements.

---

## At a Glance

| | GitHub Enterprise Cloud (GHEC) | GitHub Enterprise Server (GHES) |
|--|-------------------------------|--------------------------------|
| **Hosting** | GitHub-managed SaaS | Self-hosted (on-prem, private cloud, or VM) |
| **Price** | $21/user/mo | $21/user/mo + infrastructure costs |
| **Updates** | Automatic, always latest | Manual upgrades (quarterly releases) |
| **Data Residency** | Multi-region options; EU data residency available | Full control – data stays on your infrastructure |
| **Availability** | 99.9% SLA | Depends on your infrastructure |
| **Scalability** | Managed by GitHub | Managed by your team |
| **Compliance** | SOC 1/2, ISO 27001, FedRAMP (in progress) | Whatever your infra supports |
| **Internet Required** | Yes | Can operate air-gapped |

---

## GitHub Enterprise Cloud (GHEC)

### Best For
- Organizations that want a fully managed platform
- Teams that need the latest features immediately
- Companies comfortable with SaaS data handling

### Key Features
- **Managed Infrastructure** – No servers to maintain, patch, or scale
- **Enterprise Managed Users (EMU)** – Lifecycle-managed accounts via your IdP
- **Data Residency** – Choose where your data is stored (EU available)
- **Audit Log Streaming** – Stream to Splunk, Datadog, Azure, or S3
- **GitHub Connect** – Link Cloud and Server instances for unified search and contributions

### Considerations
- Data is stored on GitHub's infrastructure (encrypted at rest and in transit)
- Internet connectivity required
- Some regulatory environments may require on-prem

---

## GitHub Enterprise Server (GHES)

### Best For
- Highly regulated industries (finance, defense, government)
- Air-gapped or disconnected environments
- Organizations requiring full infrastructure control

### Key Features
- **Self-Hosted** – Deploy on VMware, AWS, Azure, GCP, or bare metal
- **Full Data Control** – All code and metadata stays on your infrastructure
- **Air-Gap Support** – Can operate without internet connectivity
- **Customizable** – Configure networking, storage, and HA clustering
- **GitHub Connect** – Optionally link to github.com for unified features

### Considerations
- Requires dedicated infrastructure team for maintenance
- Updates are manual (typically quarterly)
- Feature releases lag behind Cloud by 1–2 quarters
- Infrastructure costs are additional to the license fee

---

## Hybrid Approach

Many large enterprises use **both** GHEC and GHES:
- GHEC for general development teams
- GHES for highly regulated workloads or sensitive IP
- GitHub Connect bridges both for a unified experience

---

## Decision Matrix

| Requirement | GHEC | GHES |
|-------------|------|------|
| No infrastructure management | ✅ | ❌ |
| Air-gapped operation | ❌ | ✅ |
| Always on latest features | ✅ | ❌ |
| Full data sovereignty | ❌ | ✅ |
| Fastest time to value | ✅ | ❌ |
| Existing on-prem mandate | ❌ | ✅ |

---

## References

- [About GitHub Enterprise Cloud](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/about-github-enterprise-cloud)
- [About GitHub Enterprise Server](https://docs.github.com/en/enterprise-server@latest/admin/overview/about-github-enterprise-server)
- [GitHub Connect](https://docs.github.com/en/enterprise-server@latest/admin/configuration/configuring-github-connect)
