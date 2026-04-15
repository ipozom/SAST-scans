# SAST Discovery Evidence Matrix

This matrix identifies the evidence required to complete the current-state technical discovery for SAST adoption. It is intended to be maintained as a working document during stakeholder interviews and repository analysis.

## How to Use This Matrix

- capture evidence from system owners, platform teams, or repository inspection
- distinguish verified facts from assumptions
- identify blockers that would prevent a later implementation or vendor-evaluation phase
- update confidence only when the underlying data source is reliable and current

Confidence scale:

- `Confirmed` - verified by authoritative source or direct inspection
- `High` - strongly supported but not yet fully reconciled
- `Medium` - partially supported or inferred
- `Low` - assumption only

Status scale:

- `Open` - evidence still needed
- `In progress` - being gathered
- `Closed` - collected and validated

## Discovery Inputs

| Domain | Data point | Why it is needed | Suggested owner | Source | Confidence | Status | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Portfolio | List of repositories or applications in scope | Defines actual coverage boundary | Engineering leadership | SCM export or portfolio inventory | Low | Open | Minimum input for later scoping |
| Portfolio | Business criticality or application tier per repository | Supports phased adoption and prioritization | Application owners | Service catalog or owner input | Low | Open | Needed before pilot selection |
| Portfolio | Team ownership per repository | Establishes remediation and exception routing | Engineering managers | CODEOWNERS, service catalog, team map | Low | Open | Ownership gaps are a rollout risk |
| Languages | Primary language per repository | Determines required analyzer coverage | Platform engineering | Repository inspection | Low | Open | Must align with the in-scope language list |
| Languages | Frameworks in use | Determines framework-aware rule needs | Application teams | Repository inspection, manifests | Low | Open | Capture server and client frameworks separately where needed |
| Build | Build tool and version | Determines analysis prerequisites | Platform engineering | Build files and CI config | Low | Open | Include private package feed dependencies |
| Build | Average build duration | Estimates pipeline impact tolerance | Platform engineering | CI metrics | Low | Open | Capture PR and full-build durations separately if possible |
| Build | Acceptable additional scan time budget | Establishes feasible enforcement options | Engineering leadership and platform engineering | Stakeholder interview | Low | Open | Needed before PR scan expectations are set |
| Build | Monorepo or multi-module structure | Affects project detection and configuration strategy | Platform engineering | Repository inspection | Low | Open | Important for JS/TS and Java estates |
| CI/CD | CI/CD platforms in use | Determines portability requirements | DevOps or platform team | Platform inventory | Low | Open | Expect mixed-platform mapping |
| CI/CD | PR validation pattern | Determines where fast feedback can appear | DevOps or platform team | Branch protection and pipeline config | Low | Open | Needed to define future scan trigger options |
| CI/CD | Existing quality gates | Shows current tolerance for build blocking | DevOps or platform team | Pipeline configuration | Low | Open | Include lint, tests, coverage, and quality tools |
| Security | Existing SAST, code quality, or security scanning tools | Prevents duplication and reveals current maturity | AppSec | Tool inventory and stakeholder interviews | Low | Open | Include IDE and pipeline tooling |
| Developer experience | Primary IDEs and code-review workflow | Determines where findings should surface for fast remediation | Engineering enablement or application teams | Stakeholder interview | Low | Open | Important for future workflow design |
| Security | Required reporting or audit outputs | Determines result format and governance expectations | AppSec or compliance | Policy documents | Low | Open | SARIF compatibility may matter later |
| Security | Severity model and remediation SLA expectations | Frames future enforcement and triage | AppSec | Security policy | Low | Open | Even informal practices should be recorded |
| Governance | Findings ownership model | Prevents unowned vulnerabilities | Security leadership and engineering leadership | RACI, process interviews | Low | Open | One of the most important non-technical inputs |
| Governance | False-positive and exception process | Determines operational sustainability | AppSec | Process documentation or stakeholder input | Low | Open | Required before any blocking posture |
| Hosting | SaaS versus self-hosted constraints | Narrows tool deployment options | Security leadership | Policy or architecture decision | Confirmed | Closed | SaaS is acceptable |
| Security framing | OWASP Top 10 / CWE expectations | Defines baseline coverage objective | Security leadership | Scope decision | Confirmed | Closed | No additional compliance driver confirmed yet |
| Validation readiness | Candidate repositories for future benchmark testing | Enables a representative proof of feasibility | Engineering leadership and AppSec | Stakeholder selection | Low | Open | Include at least one repo per language family where practical |

## Stakeholder Interview Guide

Use these prompts when collecting missing evidence.

### Engineering Leadership

- Which applications or repositories should be covered first?
- How should criticality be defined for the initial rollout?
- What level of developer friction is acceptable during early adoption?

### Platform or DevOps Teams

- Which CI/CD platforms, build agents, and pipeline templates are currently in use?
- Which repositories have fragile builds or unusually long validation times?
- How are shared credentials, artifacts, and scan results currently managed?

### Application Teams

- Which frameworks, private packages, or generated code patterns make builds non-standard?
- Are there known areas that would produce noisy or low-value findings?
- Who should own remediation decisions for each repository?

### AppSec or Security Leadership

- What reporting, exception, and audit requirements must any future SAST model support?
- What severity thresholds would be meaningful for future policy decisions?
- Is the initial goal visibility, risk reduction, or eventual build enforcement?

## Exit Criteria for Discovery

The evidence collection phase should be considered complete when:

1. all in-scope repositories or application groups are identified and owned
2. language, framework, build, and CI/CD data is confirmed for the initial rollout scope
3. current governance and exception handling practices are documented
4. leadership has enough evidence to decide whether to proceed to vendor evaluation or implementation planning