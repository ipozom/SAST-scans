# Technical Discovery for SAST Adoption

## Executive Summary

This document captures the initial technical discovery required to implement Static Application Security Testing (SAST) scans across an application estate that includes Java/Kotlin, JavaScript/TypeScript, Python, and C#/.NET. The intended audience is engineering and security leadership.

The current deliverable is discovery only. It does not recommend a specific vendor or define an implementation plan. Its purpose is to establish the technical assessment criteria, operating constraints, key risks, and leadership decisions required before execution.

At this stage, the confirmed direction is:

- the SAST strategy must remain vendor-neutral
- the application estate spans multiple programming languages
- the CI/CD environment is mixed rather than standardized on a single platform
- SaaS-based delivery is acceptable
- OWASP Top 10 and CWE coverage expectations should frame the security discussion

The primary gap is not conceptual. It is evidentiary. The organization still needs a verified inventory of repositories, build systems, CI/CD workflows, ownership boundaries, and current secure SDLC controls before a credible implementation or tool-selection phase can begin.

## Document Objective

This document is intended to:

- define the technical and operational discovery needed for SAST adoption
- explain the cross-language and cross-platform considerations that affect implementation feasibility
- identify the decisions leadership must make before execution
- establish boundaries for what is included in this phase and what is deferred

## Confirmed Scope and Assumptions

### Confirmed Inputs

The following discovery inputs are already confirmed:

| Topic | Current position |
| --- | --- |
| Audience | Engineering leadership and security leadership |
| Deliverable type | Discovery and assessment only |
| Tool posture | Vendor-neutral |
| Languages in scope | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| CI/CD posture | Multiple or mixed platforms |
| Hosting assumption | SaaS tools are acceptable |
| Security framing | OWASP Top 10 and CWE coverage |

### Working Assumptions

The following assumptions are used to shape the discovery until estate data is collected:

- the organization operates more than one repository or service, rather than a single homogeneous codebase
- at least part of the estate uses pull-request based change control
- different teams may have different build systems or levels of pipeline maturity
- leadership will need to balance security coverage against developer workflow impact

These assumptions should be validated during the evidence-gathering phase.

## Current-State Discovery Requirements

The current-state analysis is incomplete until the following information is collected and verified.

### Repository and Application Inventory

Required evidence:

- list of repositories or applications in scope
- primary language and framework per repository
- repository criticality or tiering
- service type, such as API, web application, batch job, or library
- ownership by team or business domain

Why this matters:

- scan depth, supported frameworks, and tuning effort vary by language and application type
- adoption planning depends on understanding where high-risk or high-value coverage should start

### Build and Dependency Context

Required evidence:

- build tool per repository, such as Maven, Gradle, npm, pnpm, yarn, pip, Poetry, NuGet, or MSBuild
- package restore requirements and any private registries
- presence of monorepos, generated code, or shared libraries
- average build duration and common causes of build instability

Why this matters:

- some SAST approaches require build context or full semantic analysis to produce useful results
- repositories with fragile or slow builds create higher rollout friction

### CI/CD Landscape

Required evidence:

- platforms in use, such as GitHub Actions, GitLab CI, Jenkins, or Azure DevOps
- pull-request validation patterns versus branch or scheduled jobs
- current policy gating in pipelines
- artifact retention and results publishing patterns

Why this matters:

- a mixed CI/CD landscape favors portable scan orchestration patterns rather than platform-specific implementations
- enforcement posture depends on where in the delivery flow scan results are surfaced and acted upon

### Security and Governance Baseline

Required evidence:

- current secure SDLC requirements
- any existing code scanning or quality gate tools
- remediation ownership model
- exception or waiver process
- reporting requirements for leadership or audit stakeholders

Why this matters:

- SAST adoption fails if findings exist without a clear operating model for triage, ownership, and exception handling

## Technical Assessment Criteria

The future SAST approach should be assessed against the following technical criteria.

### Cross-Language Expectations

Minimum expectations across the estate:

- reliable support for the in-scope languages and commonly used frameworks
- ability to identify issues aligned to OWASP Top 10 and relevant CWE classes
- support for data-flow or taint-style analysis where language ecosystems require it
- output that can be published in a portable format such as SARIF
- usable scan modes for both baseline assessment and pull-request feedback
- policy controls that can distinguish severity, confidence, and rule categories

### Language-Specific Considerations

#### Java and Kotlin

Key considerations:

- strong framework awareness is needed for common enterprise stacks such as Spring or Jakarta-based services
- build-coupled analysis may be necessary to achieve useful semantic depth
- large monoliths or multi-module builds can make scan time and environment preparation material factors
- precision matters because high-volume false positives can undermine trust quickly in large codebases

Discovery questions:

- which Java and Kotlin versions are in use
- whether builds depend on private repositories or environment-specific configuration
- whether the estate includes Android code, server-side services, or both

#### JavaScript and TypeScript

Key considerations:

- dynamic language behavior increases the risk of inconsistent analysis quality across tools
- front-end and Node.js services may require different rule depth and threat emphasis
- framework-specific sanitization patterns influence false-positive rates for issues such as injection or cross-site scripting
- monorepo structures and package managers can complicate project detection

Discovery questions:

- which frameworks are in use, such as React, Angular, Next.js, Express, or NestJS
- whether front-end and back-end JavaScript are both in scope
- how monorepos and workspace-based package management are structured

#### Python

Key considerations:

- interpreted environments require careful attention to dependency resolution, runtime assumptions, and framework conventions
- insecure deserialization, command execution, template rendering, and unsafe library use are common areas of interest
- inconsistent typing and dynamic imports can affect result precision
- environment management, such as pip, Poetry, or virtual environments, may complicate reproducible scans

Discovery questions:

- which frameworks are in use, such as Django, Flask, or FastAPI
- whether there are shared internal packages or dynamic plugin patterns
- how Python environments are created in CI pipelines

#### C# and .NET

Key considerations:

- framework and runtime version coverage matters across .NET Framework and modern .NET
- build-aware analysis is often necessary for useful semantic results
- ASP.NET, API, worker, and desktop application types may carry different rule priorities
- solution structure and private package feeds can materially affect scan setup and stability

Discovery questions:

- which runtime generations are in use across the estate
- whether the portfolio includes legacy .NET Framework applications
- how package restore and solution builds are handled in CI

## Integration Architecture Patterns

Because the CI/CD environment is mixed, the discovery should favor portable patterns rather than pipeline-specific implementation detail.

### Preferred Platform-Agnostic Patterns

- CLI-based scanning that can run in any build agent environment
- containerized scanner execution where environment portability is needed
- standardized results output, preferably SARIF or an equivalent machine-readable format
- API-driven policy evaluation so enforcement logic is not hardcoded differently in each CI system
- separate modes for pull-request scans, full baseline scans, and scheduled rescans

### Integration Decision Areas

The future implementation phase will need to choose among these operating patterns:

| Decision area | Options to evaluate | Discovery concern |
| --- | --- | --- |
| Scan trigger | Pull request, branch, scheduled, release | Coverage versus feedback speed |
| Enforcement | Informational, warn, block | Developer friction versus risk reduction |
| Result destination | CI logs, code scanning UI, centralized dashboard | Visibility and accountability |
| Configuration model | Centralized policy, repo-local config, hybrid | Standardization versus team flexibility |
| Legacy findings strategy | Full backlog, baseline suppression, new-code only | Feasibility of early adoption |

## Governance and Operating Model Considerations

SAST is not only a scanning capability. It is an operating model. Discovery must therefore assess how findings will be owned, triaged, and governed.

### Required Governance Topics

- who owns the SAST platform or service
- who owns findings and remediation deadlines
- how false positives are reviewed and suppressed
- how exceptions are approved and audited
- how leadership receives coverage and remediation reporting
- what severity and confidence thresholds are acceptable for enforcement

### Early-Stage Adoption Considerations

For an initial rollout, the organization should expect a phased enforcement posture rather than immediate hard blocking across all repositories. This is not an implementation decision yet, but it is a discovery insight: the quality of early findings and the maturity of ownership processes usually determine whether blocking can succeed without causing excessive friction.

## Risks and Tradeoffs

The following risks are material for a multi-language, mixed-CI environment.

### 1. Uneven Analysis Quality Across Languages

Tools may provide strong semantic analysis for one ecosystem and weaker results for another. A single enterprise posture can therefore mask meaningful differences in coverage and false-positive rates.

### 2. Developer Fatigue From Poor Signal Quality

If early scans generate large numbers of low-value findings, teams will learn to ignore results. This risk is especially high when scan output is introduced without a clear severity model or baseline strategy.

### 3. Pipeline Performance and Reliability Impact

Repositories that require heavyweight builds or environment-specific setup may experience significant increases in validation time. That risk is amplified when multiple CI platforms are in use and agent environments are inconsistent.

### 4. Governance Gaps

If remediation ownership, exception handling, and reporting are undefined, findings accumulate without resolution. In that case, the organization gains scan activity without gaining risk reduction.

### 5. Premature Tool-Centric Decisions

Selecting a tool before understanding the estate can lead to poor language coverage, difficult integration, or an operating model that teams cannot sustain.

## Strategic Gaps Identified

At the time of drafting, the following gaps remain open:

- no verified repository inventory is attached to this document
- no language and framework distribution has been validated by actual repository data
- no build-system inventory has been captured
- no CI/CD platform mapping has been documented by team or portfolio segment
- no current-state remediation ownership model has been confirmed
- no baseline strategy for existing findings has been chosen

These gaps do not block discovery documentation, but they do block a defensible implementation plan or vendor evaluation.

## Leadership Decisions Required

Leadership should use this document to provide direction on the following decisions.

### 1. Coverage Scope

Should SAST be introduced for all repositories, only tier-1 applications, or only new strategic development?

### 2. Enforcement Posture

Should the first implementation phase be informational only, warning-based, or blocking for selected severity levels?

### 3. Ownership Model

Will AppSec, platform engineering, or application teams own operational triage and remediation accountability?

### 4. Developer Friction Tolerance

What level of additional build time, review noise, and policy overhead is acceptable in exchange for earlier vulnerability detection?

### 5. Next-Phase Mandate

Should the organization proceed to a formal product evaluation and implementation-planning phase after the evidence matrix is completed?

## Validation Activities for the Next Phase

Once discovery evidence is complete, the execution team should validate feasibility with a controlled assessment rather than moving directly to enterprise rollout.

Recommended validation activities:

1. select representative repositories from each in-scope language family
2. test whether candidate approaches can produce useful findings aligned to OWASP and CWE expectations
3. measure pull-request and full-scan duration impact against current pipeline baselines
4. review a sample of high-severity findings with senior engineers to estimate true-positive and false-positive rates
5. confirm that results can be surfaced in developer workflows without forcing platform-specific handling in each CI system
6. verify that triage, suppression, and exception workflows are operational before enabling any blocking policy

## Recommended Next Discovery Actions

The immediate next step is not product selection. It is evidence collection.

Recommended actions:

1. complete the evidence matrix with repository, language, framework, build, and CI/CD data
2. confirm current ownership and exception workflows with security and platform stakeholders
3. identify a representative sample of repositories from each language family for future validation
4. define what leadership considers acceptable signal quality and pipeline impact before any tool evaluation begins

## Scope Boundaries and Exclusions

This document excludes:

- vendor comparisons or product recommendations
- implementation steps or CI examples
- rollout sequencing and target dates
- detailed remediation workflows
- licensing or budget analysis

These topics should be handled in later phases once discovery evidence is complete and leadership has confirmed the operating direction.

## Appendix A: Evidence Status Summary

| Area | Status | Notes |
| --- | --- | --- |
| Audience and deliverable scope | Confirmed | Leadership-facing, discovery-only |
| Languages in scope | Confirmed | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| CI/CD posture | Confirmed | Mixed environment assumed |
| SaaS acceptability | Confirmed | SaaS tools permitted |
| Security framing | Confirmed | OWASP Top 10 and CWE |
| Repository inventory | Pending | Requires estate data collection |
| Framework inventory | Pending | Requires repository review |
| Build-system inventory | Pending | Requires engineering input |
| CI/CD platform map | Pending | Requires platform or DevOps input |
| Governance model | Pending | Requires AppSec and engineering alignment |