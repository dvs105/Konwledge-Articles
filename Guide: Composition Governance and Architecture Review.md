# Composition Governance and Architecture Review Guide

## Purpose

This guide provides governance questions, evidence, and decision records for solutions composing reusable capabilities under [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md).

## 1. Review Scope

Govern proportionately according to:

- Business criticality
- External exposure
- Data sensitivity
- Number of consumers
- Cross-domain complexity
- Financial or entitlement impact
- Operational concentration risk
- Regulatory obligations

## 2. Review Stages

### Concept

Confirm the business outcome, likely domains, reuse opportunities, and major risks.

### Contract and pattern design

Review candidate assessments, contracts, composition pattern, boundaries, and end-to-end qualities.

### Delivery readiness

Confirm tests, telemetry, runbooks, access, catalogue records, and onboarding.

### Production readiness

Confirm service objectives, capacity, recovery, support, rollback, and incident communication.

### Lifecycle review

Review versions, consumer usage, cost, health, exceptions, and retirement.

## 3. Required Evidence

- Business capability map
- Reuse assessment
- Catalogue records
- Context and sequence diagrams
- Contract specifications
- Responsibility model
- Security and privacy assessment
- Failure and recovery design
- Capacity and performance analysis
- Contract tests
- Operational dashboards and runbooks
- Version and migration plan
- Decision and exception records

## 4. Architecture Decision Record Template

### Decision

`<Reuse, extend, wrap, create, or retire>`

### Context

`<Business need, consumers, constraints, criticality>`

### Options

1. `<Option>`
2. `<Option>`
3. `<Option>`

### Evaluation

`<Semantic fit, quality, security, lifecycle, ownership, cost>`

### Consequences

`<Benefits, dependencies, new risks, operational obligations>`

### Decision owner

`<Owner and approval authority>`

### Review trigger

`<Date or material change>`

## 5. Exception Template

| Attribute | Entry |
|---|---|
| Principle requirement | `<Requirement>` |
| Deviation | `<Description>` |
| Rationale | `<Why>` |
| Risk | `<Impact>` |
| Compensating controls | `<Controls>` |
| Owner | `<Owner>` |
| Approver | `<Authority>` |
| Expiry/review | `<Date>` |
| Remediation/exit | `<Plan>` |

## 6. Red Flags

- No enduring owner
- Screen-oriented service boundaries
- Business logic in gateway or BFF
- Shared database access
- Long synchronous chains
- Missing idempotency or reconciliation
- Undocumented consumer dependencies
- Unsupported or deprecated contracts
- No migration path
- Generic `CommonService` growth
- Reuse based only on technical access

## 7. Review Questions

1. Is the capability authoritative?
2. Was the catalogue searched?
3. Why is this candidate semantically correct?
4. What does the composition component own?
5. What must it never own?
6. How does the journey fail safely?
7. How is data minimised and authorised?
8. How are changes tested and communicated?
9. Who funds and supports increased use?
10. How will the dependency be retired?

## Executive Summary

> Governance should make capability choice, composition boundaries, end-to-end quality, ownership, and lifecycle explicit while remaining proportionate to risk and criticality.
