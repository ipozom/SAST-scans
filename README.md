# SAST Technical Discovery

This workspace contains the initial documentation set for technical discovery and initial implementation planning related to Static Application Security Testing (SAST) scans.

The current deliverable covers discovery plus preparation for an initial SAST scan that generates a report:

- leadership-facing
- vendor-neutral
- mixed CI/CD aware
- scoped to Java/Kotlin, JavaScript/TypeScript, Python, and C#/.NET
- framed around OWASP Top 10 and CWE coverage expectations
- oriented toward executing an initial scan on selected repositories and producing a usable results report

## Contents

- `docs/sast-technical-discovery.md` - primary discovery and assessment document
- `docs/sast-technical-discovery.es.md` - Spanish version of the primary discovery and assessment document
- `docs/sast-discovery-evidence-matrix.md` - evidence collection matrix for validating assumptions and completing the current-state assessment
- `docs/sast-discovery-evidence-matrix.es.md` - Spanish version of the evidence collection matrix

## Scope Boundaries

Included in this phase:

- discovery of technical, operational, and governance factors for SAST adoption
- definition of required evidence, decisions, and constraints
- requirements for implementing an initial SAST scan and generating a report
- assessment criteria for future tool evaluation and broader implementation planning

Excluded from this phase:

- product selection or vendor ranking
- full enterprise implementation runbooks or CI pipeline examples for every platform
- enterprise rollout timeline
- budget or licensing analysis
- remediation playbooks

## Intended Outcome

The goal is to give leadership and implementation stakeholders a shared basis for executing an initial SAST scan, generating a report from it, and deciding what broader rollout or evaluation work should follow.