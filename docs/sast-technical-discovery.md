# Technical Discovery for SAST Adoption

## Executive Summary

This document captures the initial technical discovery and implementation premise required to implement Static Application Security Testing (SAST) scans across an application estate that includes Java/Kotlin, JavaScript/TypeScript, Python, and C#/.NET. The intended audience is engineering and security leadership.

The current deliverable covers discovery plus preparation for an initial SAST scan that generates a report. It does not recommend a specific vendor or define a full enterprise rollout plan. Its purpose is to establish the technical assessment criteria, operating constraints, reporting requirements, key risks, and leadership decisions required before initial execution.

At this stage, the confirmed direction is:

- the SAST strategy must remain vendor-neutral
- the application estate spans multiple programming languages
- the CI/CD environment is mixed rather than standardized on a single platform
- SaaS-based delivery is acceptable
- OWASP Top 10 and CWE coverage expectations should frame the security discussion
- the first execution milestone should be an initial SAST scan that produces a report on selected repositories

The primary gap is not conceptual. It is evidentiary. The organization still needs a verified inventory of repositories, build systems, CI/CD workflows, ownership boundaries, report consumers, and current secure SDLC controls before a credible initial scan, broader implementation, or tool-selection phase can begin.

## Document Objective

This document is intended to:

- define the technical and operational discovery needed for SAST adoption
- explain the cross-language and cross-platform considerations that affect implementation feasibility
- define the minimum conditions required to implement an initial SAST scan and generate a report
- identify the decisions leadership must make before initial execution
- establish boundaries for what is included in this phase and what is deferred

## Initial SAST Scan Requirement

The first implementation milestone should be an initial SAST scan that generates a report rather than an immediate full rollout. The initial scan is expected to validate scan execution, result quality, and reporting flow before the organization commits to broader adoption.

The initial scan should be designed to:

- run on a limited number of representative repositories rather than the full portfolio
- measure scan duration, signal quality, and developer workflow impact in real CI conditions
- generate a report that can be consumed by leadership, AppSec, and implementation stakeholders
- confirm that findings can be surfaced, triaged, suppressed, and summarized end to end
- produce a clear recommendation on whether to scale, tune, or stop

## Confirmed Scope and Assumptions

### Confirmed Inputs

The following discovery inputs are already confirmed:

| Topic | Current position |
| --- | --- |
| Audience | Engineering leadership and security leadership |
| Deliverable type | Discovery, assessment, and initial scan implementation premise |
| Tool posture | Vendor-neutral |
| Languages in scope | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| CI/CD posture | Multiple or mixed platforms |
| Hosting assumption | SaaS tools are acceptable |
| Security framing | OWASP Top 10 and CWE coverage |
| Initial execution target | Initial SAST scan with report generation |

### Working Assumptions

The following assumptions are used to shape the discovery until estate data is collected:

- the organization operates more than one repository or service, rather than a single homogeneous codebase
- at least part of the estate uses pull-request based change control
- different teams may have different build systems or levels of pipeline maturity
- leadership will need to balance security coverage against developer workflow impact
- the first implementation should stay limited to an initial scan scope before any broader rollout

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
- suitability for inclusion in the initial scan scope

Why this matters:

- scan depth, supported frameworks, and tuning effort vary by language and application type
- adoption planning depends on understanding where high-risk or high-value coverage should start

### Build and Dependency Context

Required evidence:

- build tool per repository, such as Maven, Gradle, npm, pnpm, yarn, pip, Poetry, NuGet, or MSBuild
- package restore requirements and any private registries
- presence of monorepos, generated code, or shared libraries
- average build duration and common causes of build instability
- ability to run the initial scan in existing CI runners or container environments

Why this matters:

- some SAST approaches require build context or full semantic analysis to produce useful results
- repositories with fragile or slow builds create higher rollout friction

### CI/CD Landscape

Required evidence:

- platforms in use, such as GitHub Actions, GitLab CI, Jenkins, or Azure DevOps
- pull-request validation patterns versus branch or scheduled jobs
- current policy gating in pipelines
- artifact retention and results publishing patterns
- available mechanism to publish the initial scan report or raw results artifacts

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
- owner for initial scan support, triage coordination, and report review

Why this matters:

- SAST adoption fails if findings exist without a clear operating model for triage, ownership, and exception handling

## Initial Scan Scope and Reporting Requirements

The documentation must explicitly support implementation of an initial SAST scan and production of a report. That requires more than generic discovery. It requires a clear definition of scan scope, output expectations, success criteria, and operating guardrails.

### Initial Scan Selection Criteria

- the repository is actively maintained and has a clear owner
- the CI pipeline is stable enough to measure scan impact reliably
- the codebase is representative of a major language or framework combination in the estate
- the team is willing to participate in triage and feedback during the initial scan
- the pipeline can publish results or artifacts without major platform-specific workarounds

### Report Requirements

- the report should summarize repositories scanned, execution date, scan scope, and result status
- the report should include counts by severity and confidence where available
- the report should distinguish raw findings, likely true positives, confirmed false positives, and items requiring follow-up
- the report should note execution constraints, scan duration, and any failures or exclusions
- the report should be usable by both technical stakeholders and leadership as a baseline artifact

### Initial Scan Success Criteria

- the scan runs reliably in the selected CI flow
- results can be collected and assembled into a usable report
- findings are surfaced with meaningful severity and confidence categories
- scan duration stays within the agreed initial time budget
- the participating team can review and triage results in its normal workflow
- the initial scan produces a clear recommendation on whether to scale, tune, or stop

### Initial Scan Operating Guardrails

- start non-blocking unless leadership explicitly approves a stricter posture
- limit initial scope to representative repositories instead of portfolio-wide coverage
- define an owner for configuration, results review, and report distribution
- preserve delivery stability by defining a fallback path if the scan fails or produces unacceptable noise
- timebox the initial scan so results are reviewed quickly and decisions are not deferred indefinitely

## Technical Assessment Criteria

The future SAST approach and the initial scan implementation model should be assessed against the following technical criteria.

### Cross-Language Expectations

Minimum expectations across the estate:

- reliable support for the in-scope languages and commonly used frameworks
- ability to identify issues aligned to OWASP Top 10 and relevant CWE classes
- support for data-flow or taint-style analysis where language ecosystems require it
- output that can be published in a portable format such as SARIF
- usable scan modes for both baseline assessment and pull-request feedback
- policy controls that can distinguish severity, confidence, and rule categories
- enough output fidelity to support a useful initial scan report

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

Because the CI/CD environment is mixed, the initial scan design should favor portable patterns rather than pipeline-specific implementation detail.

### Preferred Platform-Agnostic Patterns

- CLI-based scanning that can run in any build agent environment
- containerized scanner execution where environment portability is needed
- standardized results output, preferably SARIF or an equivalent machine-readable format
- API-driven policy evaluation so enforcement logic is not hardcoded differently in each CI system
- separate modes for pull-request scans, full baseline scans, and scheduled rescans
- report generation from standardized outputs or aggregated scan artifacts

### Initial Scan Implementation Guidance

- start with one or a small number of representative repositories rather than attempting cross-portfolio coverage immediately
- prefer non-blocking execution with artifact or dashboard publication in the first iteration
- keep configuration sufficiently consistent across initial scan repositories so scan behavior can be compared
- validate report generation in the workflow the participating team already uses
- define what happens when the scan fails, times out, or produces excessive noise before enabling it in shared pipelines

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

For an initial scan, the organization should expect a phased enforcement posture rather than immediate hard blocking across all repositories. This is an initial implementation premise: the quality of early findings and the maturity of ownership processes usually determine whether blocking can succeed without causing excessive friction.

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
- no initial scan repositories or participating teams have been nominated
- no report audience, format, or distribution path has been defined

These gaps do not block discovery documentation, but they do block a defensible initial scan, broader implementation plan, or vendor evaluation.

## Leadership Decisions Required

Leadership should use this document to provide direction on the following decisions.

### 1. Initial Scan Coverage Scope

Which repositories or application groups should participate in the initial scan, and what makes them representative enough to inform a broader decision?

### 2. Initial Scan Enforcement Posture

Should the initial scan start as informational only, warning-based, or blocking for selected severity levels?

### 3. Ownership Model

Will AppSec, platform engineering, or application teams own operational triage and remediation accountability?

### 4. Report Audience and Format

Who needs to consume the initial scan report, and what level of detail is required for leadership, AppSec, and engineering teams?

### 5. Developer Friction Tolerance

What level of additional build time, review noise, and policy overhead is acceptable in exchange for earlier vulnerability detection?

### 6. Post-Scan Mandate

Should the organization proceed to broader rollout, further tuning, or formal product evaluation after the initial scan report is reviewed?

## Initial Scan Implementation and Validation Activities

Once the minimum discovery evidence is complete, the execution team should validate feasibility by implementing a controlled initial SAST scan rather than moving directly to enterprise rollout.

Recommended initial scan activities:

1. select representative repositories and confirm participating teams and owners
2. implement the initial scan in the chosen CI flow using a non-blocking posture unless otherwise approved
3. test whether the scan produces useful findings aligned to OWASP and CWE expectations
4. measure pull-request and full-scan duration impact against current pipeline baselines
5. assemble and review the generated report with engineering, AppSec, and leadership stakeholders
6. review a sample of high-severity findings with senior engineers to estimate true-positive and false-positive rates
7. verify that triage, suppression, exception handling, and report distribution workflows are operational before considering any blocking policy

## Recommended Next Actions

The immediate next step is to complete the minimum evidence needed to launch an initial SAST scan and generate a report.

Recommended actions:

1. complete the evidence matrix with repository, language, framework, build, and CI/CD data
2. nominate repositories for the initial scan and confirm participating teams and owners
3. confirm ownership, exception workflows, report consumers, and support paths with security and platform stakeholders
4. define report expectations, success metrics, fallback rules, and acceptable pipeline impact before scan execution begins
5. execute the initial scan and use the report to decide on broader rollout, tuning, or formal product evaluation

## Scope Boundaries and Exclusions

This document includes initial scan implementation and report-generation requirements, but excludes:

- vendor comparisons or product recommendations
- full enterprise implementation runbooks or CI examples for every platform
- enterprise rollout sequencing and target dates
- detailed remediation workflows
- licensing or budget analysis

These topics should be handled in later phases once discovery evidence is complete, the initial scan report has been reviewed, and leadership has confirmed the operating direction.

## Appendix A: Evidence Status Summary

| Area | Status | Notes |
| --- | --- | --- |
| Audience and deliverable scope | Confirmed | Leadership-facing, discovery plus initial scan implementation premise |
| Languages in scope | Confirmed | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| CI/CD posture | Confirmed | Mixed environment assumed |
| SaaS acceptability | Confirmed | SaaS tools permitted |
| Security framing | Confirmed | OWASP Top 10 and CWE |
| Repository inventory | Pending | Requires estate data collection |
| Framework inventory | Pending | Requires repository review |
| Build-system inventory | Pending | Requires engineering input |
| CI/CD platform map | Pending | Requires platform or DevOps input |
| Governance model | Pending | Requires AppSec and engineering alignment |
| Initial scan repository selection | Pending | Requires leadership and team alignment |
| Initial scan report definition | Pending | Requires explicit definition of audience, format, and distribution |