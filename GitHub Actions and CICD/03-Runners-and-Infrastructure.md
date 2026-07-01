# 03 - Runners and Infrastructure

Runner strategy is one of the most important architectural choices in GitHub Actions. It determines performance, isolation, network reach, compliance posture, and cost.

> Official docs: <https://docs.github.com/actions/using-github-hosted-runners>

## Runner Options Overview

| Runner type | Managed by | Best for | Key trade-off |
|---|---|---|---|
| **GitHub-hosted runners** | GitHub | Standard CI/CD, fast onboarding, low ops burden | Less control over network and customization |
| **Larger runners** | GitHub | High-performance or specialized workloads | Higher cost for premium capacity |
| **Self-hosted runners** | Customer | Private network access, custom tooling, compliance constraints | You own patching, scaling, and security |
| **Runner scale sets (ARC)** | Customer + Kubernetes | Elastic self-hosted fleets | Higher platform complexity |

## GitHub-Hosted Runners

GitHub-hosted runners are ephemeral machines provisioned by GitHub for each job. They are the default choice for most teams because they minimize operational overhead.

### Advantages

- No runner patching or lifecycle management
- Fast setup for Linux, Windows, and macOS
- Clean environment for each job
- Strong fit for public internet-facing build and test tasks

### Typical use cases

- Unit and integration testing
- Static analysis and security scanning
- Package builds
- Documentation generation
- Deployment orchestration to cloud services using OIDC

## Larger Runners

Larger runners provide more CPU, memory, disk, or specialized capabilities than standard GitHub-hosted runners.

### Use them when

- Builds are CPU or memory bound
- Integration tests need larger disks or longer execution windows
- GPU or special hardware-backed scenarios are required
- Queue times or parallelism constraints justify premium capacity

### Planning considerations

- Benchmark representative workloads before broad adoption
- Restrict access using runner groups
- Track cost per pipeline family
- Pair with concurrency controls to avoid runaway spend

## Self-Hosted Runners

Self-hosted runners execute jobs on machines you manage: VMs, bare metal, containers, or Kubernetes nodes.

### When self-hosted makes sense

- Workloads require access to internal systems, private networks, or on-prem dependencies
- You must use custom hardened images or licensed tools
- Data residency or compliance requirements restrict shared infrastructure
- Specialized hardware or persistent caches are necessary

### Risks and responsibilities

- OS patching and package updates
- Runner image maintenance
- Credential hygiene and network segmentation
- Secure teardown or reimaging between jobs
- Capacity planning and autoscaling

## Runner Groups

Runner groups help control which repositories can use which runners.

### Recommended patterns

| Pattern | Why it helps |
|---|---|
| Separate prod deployment runners from CI runners | Limits blast radius |
| Isolate regulated workloads | Supports compliance boundaries |
| Group by business unit or environment tier | Simplifies access reviews |
| Restrict admin-only automation | Protects sensitive operational jobs |

## Runner Scale Sets and Actions Runner Controller (ARC)

Actions Runner Controller (ARC) is a Kubernetes-based solution for managing elastic fleets of self-hosted runners. Runner scale sets simplify provisioning and lifecycle management for ephemeral self-hosted runners.

### Best fit for ARC

- Enterprise platform teams already operating Kubernetes reliably
- Large-scale multi-repository workloads
- Need for ephemeral runners with dynamic autoscaling
- Standardized hardened images with policy-controlled execution

### Operational benefits

- Ephemeral runners reduce job-to-job persistence risk
- Autoscaling can reduce idle infrastructure cost
- Centralized image management supports compliance and repeatability

## Cost Comparison

| Dimension | GitHub-hosted | Larger runners | Self-hosted |
|---|---|---|---|
| Direct infrastructure management | None | None | High |
| Predictable unit pricing | High | Medium | Low to medium |
| Hidden operational cost | Low | Low | High |
| Burst capacity | Strong | Strong | Depends on your platform |
| Network/private system access | Limited | Limited | Strong |

### Practical interpretation

- Choose **GitHub-hosted** when engineering speed and low overhead matter most.
- Choose **larger runners** when specific workloads justify premium compute.
- Choose **self-hosted** only when a clear technical, compliance, or network requirement exists.
- Choose **ARC** when self-hosted demand is large enough to justify platform automation.

## Security Considerations for Self-Hosted Runners

### High-priority controls

- Use **ephemeral** runners whenever possible.
- Avoid long-lived mutable hosts for untrusted code.
- Segregate runners for pull request workloads vs. deployment workloads.
- Restrict repository access via runner groups.
- Minimize outbound and inbound network access.
- Rotate credentials and prefer OIDC over static cloud keys.
- Collect logs and telemetry outside the runner host.

### Pull request risk warning

Never allow untrusted pull request code from forks to run on privileged self-hosted runners with network access to production systems or sensitive internal services.

## Decision Framework

| Requirement | Best fit |
|---|---|
| Minimal ops overhead | GitHub-hosted |
| Need more CPU / RAM / disk | Larger runners |
| Access to private internal resources | Self-hosted |
| Elastic enterprise self-hosted fleet | ARC / runner scale sets |
| Regulated deployment execution | Dedicated self-hosted or isolated larger runners |

## Infrastructure Checklist

- [ ] Classify workloads by trust level and environment access.
- [ ] Decide where GitHub-hosted is the default.
- [ ] Document justified exceptions for self-hosted usage.
- [ ] Define runner group boundaries and repository entitlements.
- [ ] Standardize base images and update cadence.
- [ ] Enable ephemeral execution where possible.
- [ ] Monitor queue time, job duration, failure rate, and cost.

## Reference Links

- [About GitHub-hosted runners](https://docs.github.com/actions/using-github-hosted-runners/about-github-hosted-runners)
- [About larger runners](https://docs.github.com/actions/using-github-hosted-runners/about-larger-runners)
- [About self-hosted runners](https://docs.github.com/actions/hosting-your-own-runners/about-self-hosted-runners)
- [About Actions Runner Controller](https://docs.github.com/actions/hosting-your-own-runners/about-actions-runner-controller)
- [Using runner groups](https://docs.github.com/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups)
