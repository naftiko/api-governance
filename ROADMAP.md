# API Governance Capability Roadmap

This roadmap tracks planned capability expansions for the API governance use case beyond the current **Discover → Validate → Secure → Monitor → Measure** architecture. The full lifecycle target adds: **Automate**, **Enforce**, and **Developer Experience**.

Each proposed capability follows the same Naftiko v0.5 schema, directory structure, and triple-exposure pattern (HTTP, MCP, Agent Skill) as the existing capabilities.

---

## Current State (12 Capabilities)

| Area | Count | Capabilities |
|---|---|---|
| **Discover** | 2 | API Catalog Discovery, API Standards Alignment |
| **Validate** | 2 | API Spec Validation, API Governance Scorecard |
| **Secure** | 3 | MCP Server Security Review, API Security Posture Assessment, Agent Identity Audit |
| **Monitor** | 2 | API Lifecycle Compliance, API Change Impact Analysis |
| **Measure** | 3 | Capability Cost Attribution, API Reuse Metrics, MCP Registry Governance |

---

## 1. Automated Remediation (New Area)

Governance today detects issues but relies on manual intervention to fix them. Automated remediation capabilities close the loop — auto-generating fixes, opening PRs, and applying policy corrections without human bottlenecks.

| Capability | What It Does |
|---|---|
| `remediate-spec-violations` | Auto-generate fixes for Spectral violations and open PRs against spec repos — resolve common linting issues without manual intervention |
| `remediate-deprecation-migration` | Auto-generate consumer migration code when an API enters deprecation — stub implementations pointing to successor APIs |
| `remediate-mcp-permission-scoping` | Auto-scope MCP server permissions to least-privilege based on actual usage patterns — reduce over-permissioned agent access |

**MRD Problems Addressed:**

- Problem 30: Need governance framed as a "golden path," not a gate
- Problem 31: Need to avoid "rules in the pipeline = governance" oversimplification
- Problem 15: Need governance rules available in coding assistants via MCP

**Design Partner Sources:** Anna Daugherty (Arnica — pipeline-less security, rules at code generation time), Mark Avery (Cvent — automated discovery, no manual YAML)

---

## 2. Policy Enforcement (New Area)

Current capabilities assess compliance but don't enforce it at runtime. Policy enforcement capabilities embed governance decisions into the runtime path — blocking non-compliant deployments, enforcing budget guardrails, and gating MCP server access.

| Capability | What It Does |
|---|---|
| `enforce-deployment-gate` | Block capability deployments that fail governance scorecard thresholds — integrate with CI/CD as a quality gate |
| `enforce-budget-guardrails` | Enforce API/AI spending limits at runtime — throttle or block capability calls that would exceed team budgets |
| `enforce-mcp-access-policy` | Runtime enforcement of MCP server access policies — team-level permissions, data classification gates, approved-server-only mode |
| `enforce-identity-policy` | Block agent workflows that violate identity propagation policies — prevent OBO chain breaks at the gateway level |

**MRD Problems Addressed:**

- Problem 3: Need governed approach to 3rd-party services via MCP server
- Problem 6: Need MCP to integrate with existing API governance tooling
- Problem 7: Need AI FinOps — cost visibility and control across AI services
- Problem 8: Need to govern MCP servers not one-to-one with APIs

**Design Partner Sources:** Ulrich (EY — budget, capacity, data confidentiality guardrails), John Musser (Ford — MCP governance as #1 priority), Norbert Anthony (P&G — governance at portfolio scale)

---

## 3. Developer Experience (New Area)

Governance must meet developers where they work. These capabilities surface governance insights directly in the IDE and PR workflow — where developers make integration decisions — rather than requiring them to visit a separate portal or wait for pipeline results.

| Capability | What It Does |
|---|---|
| `surface-governance-ide` | Expose governance scorecards, security findings, and compliance status as MCP tools available in IDE copilots — developers see governance context at decision time |
| `surface-governance-pr-check` | PR-level governance check that flags security violations, breaking changes, and non-compliant specs before merge — inline with existing review workflows |
| `surface-governance-slack-digest` | Daily/weekly governance digest to team Slack channels — new violations, upcoming sunsets, budget warnings, MCP registry changes |

**MRD Problems Addressed:**

- Problem 2: Need to incentivize developers to reuse APIs when developing — in IDE and copilot
- Problem 19: Need discoverability, not just governance — the real barrier to reuse
- Problem 30: Need governance framed as a "golden path," not a gate

**Design Partner Sources:** Anna Daugherty (Arnica — developer workflow focus, PR-level detection), Cvent (copilot-embedded discovery), Mark Avery (Backstage integration)

---

## 4. Compliance Reporting (New Area)

Enterprises need governance tied to regulatory frameworks. These capabilities map API governance posture to compliance standards and generate audit-ready reports.

| Capability | What It Does |
|---|---|
| `report-compliance-mapping` | Map API governance scorecard dimensions to compliance frameworks (NIST SP 800-53, SOC 2, PCI DSS, GDPR) — generate compliance evidence from governance data |
| `report-audit-trail` | Generate audit trails for governance decisions — MCP approvals, security reviews, policy exceptions, identity chain verifications |

**MRD Problems Addressed:**

- Problem 3: Need governed approach to 3rd-party services via MCP server
- Problem 6: Need MCP to integrate with existing API governance tooling

**Design Partner Sources:** Ulrich (EY — compliance at scale, multi-region requirements), PRD Enterprise Edition (continuous compliance approach similar to CCF)

---

## Capability Summary

| Area | Built | Proposed | Total |
|---|---|---|---|
| **Discover** | 2 | 0 | 2 |
| **Validate** | 2 | 0 | 2 |
| **Secure** | 3 | 0 | 3 |
| **Monitor** | 2 | 0 | 2 |
| **Measure** | 3 | 0 | 3 |
| **Automate** | 0 | 3 | 3 |
| **Enforce** | 0 | 4 | 4 |
| **Developer Experience** | 0 | 3 | 3 |
| **Compliance** | 0 | 2 | 2 |
| **Total** | **12** | **12** | **24** |

---

## Full Lifecycle Architecture

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
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  VALIDATE (2)    │ │   SECURE (3)     │ │                  │
│ Spec Validation  │ │ MCP Security     │ │  Reuse across:   │
│ Scorecard        │ │ Security Posture │ │  • HTTP endpoints│
│                  │ │ Identity Audit   │ │  • MCP tools     │
│                  │ │                  │ │  • Agent Skills  │
└────────┬─────────┘ └────────┬─────────┘ └──────────────────┘
         └────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  MONITOR (2)     │ │   MEASURE (3)    │ │  ENFORCE (4)     │
│ Lifecycle        │ │ Cost Attribution │ │ Deployment Gate  │
│ Change Impact    │ │ Reuse Metrics    │ │ Budget Guardrails│
│                  │ │ MCP Registry     │ │ MCP Access       │
│                  │ │                  │ │ Identity Policy  │
└──────────────────┘ └──────────────────┘ └──────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         ▼                    ▼                    ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  AUTOMATE (3)    │ │ COMPLIANCE (2)   │ │     DX (3)       │
│ Spec Remediation │ │ Compliance Map   │ │ IDE Surface      │
│ Migration Gen    │ │ Audit Trail      │ │ PR Check         │
│ Permission Scope │ │                  │ │ Slack Digest     │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

---

## Shared Adapters Required

New shared adapters still needed:

| Adapter | Service | Area |
|---|---|---|
| `consumes-opa.yml` | Open Policy Agent | Enforce |
| `consumes-kyverno.yml` | Kyverno Policy Engine | Enforce |

Already built in this project (`shared/`):

- `consumes-backstage.yml`, `consumes-spectral.yml`, `consumes-security-scanner.yml` (new)
- `consumes-api-gateway.yml`, `consumes-mcp-registry.yml`, `consumes-identity-service.yml` (new)
- `consumes-observability.yml`, `consumes-spec-diff.yml` (new)
- `consumes-llm-gateway.yml`, `consumes-cloud-billing.yml` (new)

Existing adapters consumed from the capabilities repo:

- `consumes-github.yml` (Validate, Secure, Monitor, Measure)
- `consumes-slack.yml` (Secure, Measure)

---

## MRD Problem Coverage

| Problem | Description | Capabilities |
|---|---|---|
| 2 | Quantify and incentivize API reuse | Measure (reuse-metrics) **BUILT**, DX (IDE surface) |
| 3 | Governed approach to 3rd-party MCP | Secure (mcp-security-review) **BUILT**, Measure (mcp-registry) **BUILT**, Enforce (mcp-access) |
| 6 | MCP integration with API governance | Secure (all) **BUILT**, Enforce (all), Compliance (all) |
| 7 | AI FinOps — cost visibility and control | Measure (cost-attribution) **BUILT**, Enforce (budget-guardrails) |
| 8 | MCP servers not 1:1 with APIs | Secure (mcp-security-review) **BUILT**, Enforce (mcp-access) |
| 12 | Agent-to-agent identity propagation | Secure (identity-audit) **BUILT**, Enforce (identity-policy) |
| 15 | Governance rules in coding assistants via MCP | Validate (spec-validation) **BUILT**, DX (IDE surface), Automate (spec-remediation) |
| 17 | Governance review tracking and reporting | Validate (scorecard) **BUILT**, Compliance (audit-trail) |
| 19 | Discoverability as the real barrier | Discover (all) **BUILT**, DX (IDE surface) |
| 20 | Reuse metrics that connect to business outcomes | Measure (reuse-metrics, cost-attribution) **BUILT** |
| 22 | Map APIs to products and business capabilities | Discover (catalog-discovery) **BUILT** |
| 24 | Manage spend across 3rd-party APIs | Measure (cost-attribution) **BUILT**, Enforce (budget-guardrails) |
| 28 | Know who is calling APIs after handoff | Secure (identity-audit) **BUILT** |
| 30 | Governance as golden path, not gate | Validate (spec-validation, scorecard) **BUILT**, DX (all), Automate (all) |
| 31 | Avoid "rules in pipeline = governance" | Validate (all) **BUILT**, Automate (all), DX (all) |
