# SAST Discovery Evidence Matrix

This matrix identifies the evidence required to complete the current-state technical discovery for SAST adoption and to prepare implementation of a pilot SAST scan. It is intended to be maintained as a working document during stakeholder interviews and repository analysis.

## How to Use This Matrix

- capture evidence from system owners, platform teams, or repository inspection
- distinguish verified facts from assumptions
- identify blockers that would prevent pilot implementation or a later vendor-evaluation phase
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

## Discovery and Pilot Inputs

| Domain | Data point | Why it is needed | Suggested owner | Source | Confidence | Status | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Portfolio | List of repositories or applications in scope | Defines actual coverage boundary | Engineering leadership | SCM export or portfolio inventory | Low | Open | Minimum input for later scoping |
| Portfolio | Business criticality or application tier per repository | Supports phased adoption and prioritization | Application owners | Service catalog or owner input | Low | Open | Needed before pilot selection |
| Portfolio | Team ownership per repository | Establishes remediation and exception routing | Engineering managers | CODEOWNERS, service catalog, team map | Low | Open | Ownership gaps are a rollout risk |
| Portfolio | Candidate pilot repositories and selection rationale | Defines the initial pilot scope | Engineering leadership and application owners | Stakeholder selection and repository review | Low | Open | Choose repositories that are representative and actively maintained |
| Languages | Primary language per repository | Determines required analyzer coverage | Platform engineering | Repository inspection | Low | Open | Must align with the in-scope language list |
| Languages | Frameworks in use | Determines framework-aware rule needs | Application teams | Repository inspection, manifests | Low | Open | Capture server and client frameworks separately where needed |
| Build | Build tool and version | Determines analysis prerequisites | Platform engineering | Build files and CI config | Low | Open | Include private package feed dependencies |
| Build | Average build duration | Estimates pipeline impact tolerance | Platform engineering | CI metrics | Low | Open | Capture PR and full-build durations separately if possible |
| Build | Acceptable additional scan time budget | Establishes feasible enforcement options | Engineering leadership and platform engineering | Stakeholder interview | Low | Open | Needed before PR scan expectations are set |
| Build | Ability to execute scanners in existing CI runners or containers | Determines whether the pilot can run without new infrastructure | Platform engineering | CI runner inventory and test runs | Low | Open | Include image, container, and network constraints |
| Build | Monorepo or multi-module structure | Affects project detection and configuration strategy | Platform engineering | Repository inspection | Low | Open | Important for JS/TS and Java estates |
| CI/CD | CI/CD platforms in use | Determines portability requirements | DevOps or platform team | Platform inventory | Low | Open | Expect mixed-platform mapping |
| CI/CD | PR validation pattern | Determines where fast feedback can appear | DevOps or platform team | Branch protection and pipeline config | Low | Open | Needed to define future scan trigger options |
| CI/CD | Existing quality gates | Shows current tolerance for build blocking | DevOps or platform team | Pipeline configuration | Low | Open | Include lint, tests, coverage, and quality tools |
| CI/CD | Ability to publish scan results to PRs, artifacts, or code scanning UI | Determines the pilot feedback path | DevOps or platform team | Pipeline configuration and platform capabilities | Low | Open | Must be testable in the pilot workflow |
| Security | Existing SAST, code quality, or security scanning tools | Prevents duplication and reveals current maturity | AppSec | Tool inventory and stakeholder interviews | Low | Open | Include IDE and pipeline tooling |
| Developer experience | Primary IDEs and code-review workflow | Determines where findings should surface for fast remediation | Engineering enablement or application teams | Stakeholder interview | Low | Open | Important for future workflow design |
| Security | Required reporting or audit outputs | Determines result format and governance expectations | AppSec or compliance | Policy documents | Low | Open | SARIF compatibility may matter later |
| Security | Severity model and remediation SLA expectations | Frames future enforcement and triage | AppSec | Security policy | Low | Open | Even informal practices should be recorded |
| Governance | Findings ownership model | Prevents unowned vulnerabilities | Security leadership and engineering leadership | RACI, process interviews | Low | Open | One of the most important non-technical inputs |
| Governance | False-positive and exception process | Determines operational sustainability | AppSec | Process documentation or stakeholder input | Low | Open | Required before any blocking posture |
| Governance | Pilot support owner and escalation path | Ensures pilot issues can be resolved quickly | Platform engineering and AppSec | Stakeholder interview | Low | Open | Define who responds to scan failures or noise issues |
| Hosting | SaaS versus self-hosted constraints | Narrows tool deployment options | Security leadership | Policy or architecture decision | Confirmed | Closed | SaaS is acceptable |
| Security framing | OWASP Top 10 / CWE expectations | Defines baseline coverage objective | Security leadership | Scope decision | Confirmed | Closed | No additional compliance driver confirmed yet |
| Validation readiness | Candidate repositories for future benchmark testing | Enables a representative proof of feasibility | Engineering leadership and AppSec | Stakeholder selection | Low | Open | Include at least one repo per language family where practical |
| Pilot | Pilot success metrics and exit criteria | Defines the go or no-go threshold for scaling | Security leadership and engineering leadership | Stakeholder decision | Low | Open | Include scan time, signal quality, and triage completion |
| Pilot | Fallback posture if the pilot scan fails or is too noisy | Prevents disruption to delivery during the pilot | Platform engineering and AppSec | Stakeholder interview | Low | Open | Non-blocking is the default starting point |

## Stakeholder Interview Guide

Use these prompts when collecting missing evidence.

### Engineering Leadership

- Which applications or repositories should be covered first?
- How should criticality be defined for the initial rollout?
- What level of developer friction is acceptable during early adoption?
- Which repositories and teams should participate in the pilot?
- What outcomes will define pilot success?

### Platform or DevOps Teams

- Which CI/CD platforms, build agents, and pipeline templates are currently in use?
- Which repositories have fragile builds or unusually long validation times?
- How are shared credentials, artifacts, and scan results currently managed?
- Can the current runners or containers support scanner execution without new infrastructure?
- Where will pilot results be published for developer consumption?

### Application Teams

- Which frameworks, private packages, or generated code patterns make builds non-standard?
- Are there known areas that would produce noisy or low-value findings?
- Who should own remediation decisions for each repository?
- Is the team available to participate in triage and feedback during the pilot?
- What fallback is needed if the pilot creates excessive delivery friction?

### AppSec or Security Leadership

- What reporting, exception, and audit requirements must any future SAST model support?
- What severity thresholds would be meaningful for future policy decisions?
- Is the initial goal visibility, risk reduction, or eventual build enforcement?
- What signal quality is acceptable before the pilot is considered successful?
- Who approves suppressions and tuning decisions during the pilot?

## Exit Criteria for Discovery and Pilot Readiness

The evidence collection phase should be considered complete when:

1. all in-scope repositories or application groups are identified and owned
2. language, framework, build, and CI/CD data is confirmed for the initial rollout scope
3. current governance and exception handling practices are documented
4. pilot repositories, pilot owners, and pilot feedback paths are identified
5. pilot success metrics and fallback posture are explicitly defined
6. leadership has enough evidence to decide whether to proceed to pilot execution, vendor evaluation, or broader implementation planning