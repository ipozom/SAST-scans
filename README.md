# SAST Technical Discovery

This workspace contains the initial documentation set for technical discovery and pilot implementation planning related to Static Application Security Testing (SAST) scans.

The current deliverable covers discovery plus preparation for a pilot SAST scan:

- leadership-facing
- vendor-neutral
- mixed CI/CD aware
- scoped to Java/Kotlin, JavaScript/TypeScript, Python, and C#/.NET
- framed around OWASP Top 10 and CWE coverage expectations
- oriented toward a first pilot implementation on representative repositories

## Contents

- `docs/sast-technical-discovery.md` - primary discovery and assessment document
- `docs/sast-technical-discovery.es.md` - Spanish version of the primary discovery and assessment document
- `docs/sast-discovery-evidence-matrix.md` - evidence collection matrix for validating assumptions and completing the current-state assessment
- `docs/sast-discovery-evidence-matrix.es.md` - Spanish version of the evidence collection matrix

## Scope Boundaries

Included in this phase:

- discovery of technical, operational, and governance factors for SAST adoption
- definition of required evidence, decisions, and constraints
- pilot scope definition, pilot success criteria, and pilot operating guardrails
- assessment criteria for future tool evaluation and pilot implementation

Excluded from this phase:

- product selection or vendor ranking
- full enterprise implementation runbooks or platform-specific CI pipeline examples
- enterprise rollout timeline
- budget or licensing analysis
- remediation playbooks

## Intended Outcome

The goal is to give leadership and implementation stakeholders a shared basis for selecting and executing a pilot SAST scan, and for deciding whether to proceed to broader rollout or formal product evaluation after the pilot.