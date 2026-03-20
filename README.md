# API Governance

Govern APIs, MCP servers, and agent workflows across the enterprise using the Naftiko v0.5 capability framework. This project breaks API governance into five capability areas — **Discover**, **Validate**, **Secure**, **Monitor**, and **Measure** — each exposed as HTTP, MCP, and Agent Skill adapters.

See [ROADMAP.md](ROADMAP.md) for planned capability expansions across Automate, Enforce, Compliance, and Developer Experience.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                          DISCOVER (2)                                │
│  API Catalog Discovery   API Standards Alignment                    │
└────────────────────────────────┬────────────────────────────────────┘
                                 ▼
                    Governed API & MCP Landscape
                                 │
              ┌──────────────────┼──────────────────┐
              ▼                  ▼                  ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────────────┐
│  VALIDATE (2)    │ │   SECURE (3)     │ │                          │
│ Spec Validation  │ │ MCP Security     │ │  Reuse across adapters:  │
│ Scorecard        │ │ Security Posture │ │  • HTTP endpoints        │
│                  │ │ Identity Audit   │ │  • MCP tools             │
│                  │ │                  │ │  • Agent Skills          │
└────────┬─────────┘ └────────┬─────────┘ └──────────────────────────┘
         └────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  MONITOR (2)     │ │   MEASURE (3)    │ │                  │
│ Lifecycle        │ │ Cost Attribution │ │  Governance as   │
│ Change Impact    │ │ Reuse Metrics    │ │  golden path,    │
│                  │ │ MCP Registry     │ │  not a gate      │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

---

## Capabilities

### Discover (2 capabilities) — Find & Assess What Exists

| Capability | Source | What It Does |
|---|---|---|
| [api-catalog-discovery](examples/api-catalog-discovery.yml) | Backstage Catalog | Search and discover APIs, capabilities, and MCP servers with governance metadata enrichment — ownership, lifecycle, compliance, reuse |
| [api-standards-alignment](examples/api-standards-alignment.yml) | OpenAPI / AsyncAPI / Naftiko Specs | Check alignment with organizational golden path standards (OpenAPI, AsyncAPI, MCP, A2A, Arazzo) and track standards adoption |

### Validate (2 capabilities) — Assess Governance Quality

| Capability | Source | What It Does |
|---|---|---|
| [api-spec-validation](examples/api-spec-validation.yml) | GitHub / Spectral | Validate specs against organizational Spectral rulesets and JSON Schema — governance as golden path guidance, not a gate |
| [api-governance-scorecard](examples/api-governance-scorecard.yml) | Backstage / Gateway | Generate governance scorecards across five dimensions: documentation, security, lifecycle, operations, adoption |

### Secure (3 capabilities) — Protect APIs & Agents

| Capability | Source | What It Does |
|---|---|---|
| [mcp-server-security-review](examples/mcp-server-security-review.yml) | MCP Registry / Security Scanner | Security review and blessing workflow for third-party MCP servers — tool permissions, data classification, network endpoints |
| [api-security-posture-assessment](examples/api-security-posture-assessment.yml) | Security Scanner / Gateway | Assess security posture against OWASP API Top 10 and organizational policies, including runtime telemetry |
| [agent-identity-audit](examples/agent-identity-audit.yml) | Observability / Identity Service | Audit agent-to-agent identity propagation — OBO token chains, identity escalation detection, permission risk |

### Monitor (2 capabilities) — Track Lifecycle & Changes

| Capability | Source | What It Does |
|---|---|---|
| [api-lifecycle-compliance](examples/api-lifecycle-compliance.yml) | Backstage / Gateway | Monitor and enforce lifecycle policies — deprecation timelines, sunset schedules, spec drift detection |
| [api-change-impact-analysis](examples/api-change-impact-analysis.yml) | GitHub / Spec Diff | Analyze downstream impact of API changes across consuming capabilities — breaking change detection, migration plans |

### Measure (3 capabilities) — Quantify Cost & Reuse

| Capability | Source | What It Does |
|---|---|---|
| [capability-cost-attribution](examples/capability-cost-attribution.yml) | Gateway / LLM Gateway / Cloud Billing | FinOps for API and MCP consumption — cost attribution across gateway, LLM tokens, and compute with budget alerting |
| [api-reuse-metrics](examples/api-reuse-metrics.yml) | Backstage / Gateway | Track, quantify, and report API reuse with leadership-ready ROI reports |
| [mcp-registry-governance](examples/mcp-registry-governance.yml) | MCP Registry / GitHub | Govern the MCP server registry with approval workflows, syncing blessed servers to GitHub's org-level MCP configuration |

---

## Shared Adapters

New adapters created for this project (complement the 31 adapters in the [capabilities repo](../../capabilities/shared/) and 7 in [api-reusability](../api-reusability/api-reusability/shared/)):

| Adapter | Service |
|---------|---------|
| [consumes-backstage.yml](shared/consumes-backstage.yml) | Backstage Software Catalog API — entities, search, relations, scorecards, facets |
| [consumes-spectral.yml](shared/consumes-spectral.yml) | Spectral linting service — spec validation, format detection, drift analysis |
| [consumes-security-scanner.yml](shared/consumes-security-scanner.yml) | Security scanning — OWASP analysis, auth assessment, data classification, MCP tool analysis |
| [consumes-api-gateway.yml](shared/consumes-api-gateway.yml) | API gateway analytics — usage, cost, health metrics, security events |
| [consumes-mcp-registry.yml](shared/consumes-mcp-registry.yml) | MCP server registry — manifests, vendor policy, usage stats, registry sync |
| [consumes-identity-service.yml](shared/consumes-identity-service.yml) | Identity management — OBO verification, escalation detection, permission auditing |
| [consumes-observability.yml](shared/consumes-observability.yml) | Distributed tracing — trace retrieval for identity chain auditing |
| [consumes-spec-diff.yml](shared/consumes-spec-diff.yml) | Spec diff analysis — breaking change detection, consumer impact |
| [consumes-llm-gateway.yml](shared/consumes-llm-gateway.yml) | LLM gateway — token cost tracking and attribution |
| [consumes-cloud-billing.yml](shared/consumes-cloud-billing.yml) | Cloud billing — compute cost attribution by labels |

Existing adapters consumed from the capabilities repo:

- `consumes-github.yml` — GitHub REST API v3 (Validate, Secure, Monitor, Measure)
- `consumes-slack.yml` — Slack Web API (Secure, Measure)

---

## Exposure Modes

Every capability is exposed three ways, aligned with the Naftiko framework:

| Mode | Description |
|------|-------------|
| **HTTP** | REST endpoint (e.g. `POST /governance/scorecard`) for direct integration |
| **MCP** | Model Context Protocol tool under the `api-governance` namespace |
| **Agent Skill** | Agent skill adapter for agentic orchestration workflows |
