# Capability Reuse Assessment Framework

## Purpose

Use this framework to decide whether to reuse, extend, wrap, replace, or create a capability. It supports [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md).

## 1. Candidate Summary

| Attribute | Entry |
|---|---|
| Required business outcome | `<Outcome>` |
| Candidate capability | `<Name>` |
| Provider/domain owner | `<Owner>` |
| Intended consumer | `<Consumer>` |
| Criticality | `<Level>` |
| Contract | `<Reference>` |
| Lifecycle state | `<State>` |

## 2. Mandatory Gates

A candidate should not be reused without an approved exception if any mandatory gate fails.

- [ ] The capability has an accountable owner.
- [ ] The business semantics match the requirement.
- [ ] The consumer is approved for the intended use.
- [ ] A supported contract exists.
- [ ] Authorization and data controls are suitable.
- [ ] The lifecycle is compatible with the consumer roadmap.
- [ ] Failure behaviour can be safely integrated.

## 3. Assessment Dimensions

### Semantic fit

Assess business definition, source of truth, rules, state transitions, outcome codes, and error meaning.

### Contract fit

Assess operation coverage, payloads, optionality, compatibility, examples, mocks, and testability.

### Data fit

Assess classification, freshness, quality, minimisation, residency, retention, and permitted purpose.

### Security fit

Assess identity, authorization, workload trust, audit, abuse protection, and secrets.

### Quality fit

Assess availability, latency, throughput, durability, recovery, support hours, and maintenance.

### Lifecycle fit

Assess roadmap, versions, deprecation, vendor dependency, portability, and retirement.

### Ownership fit

Assess business owner, technical owner, support owner, funding, escalation, and consumer management.

### Operational fit

Assess monitoring, alerting, runbooks, incident communication, capacity, and dependency visibility.

### Economic fit

Assess provider enhancement, consumer adaptation, operational uplift, duplicate-build avoidance, and concentration risk.

## 4. Scoring Worksheet

Use the organisation's approved scoring approach. Do not let a numeric total override a failed mandatory gate.

| Dimension | Weight | Score | Evidence | Risk/action |
|---|---:|---:|---|---|
| Semantic fit | `<High>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Contract fit | `<High>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Security/privacy | `<High>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Service quality | `<High>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Ownership | `<High>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Lifecycle | `<Medium>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Operations | `<Medium>` | `<1-5>` | `<Evidence>` | `<Action>` |
| Cost/value | `<Medium>` | `<1-5>` | `<Evidence>` | `<Action>` |

## 5. Decision Outcomes

- **Reuse as-is:** suitable without contract change.
- **Reuse with experience adaptation:** presentation-only transformation is required.
- **Reuse through BFF:** channel aggregation or shaping is required.
- **Extend compatibly:** provider accepts a valid additive enhancement.
- **Publish a purpose-specific contract:** same provider, distinct supported semantics or data minimisation.
- **Create new capability:** no adequate semantic or operational fit exists.
- **Do not proceed:** unresolved risk or unsupported dependency.

## 6. Example

A mobile application needs a customer mailing address. A marketing-contact endpoint exists, but it contains campaign preferences and uses a non-authoritative address projection. The customer-profile capability owns the current address and supports the required classification and availability. The correct decision is to reuse customer profile, not the technically accessible marketing endpoint.

## 7. Decision Record

| Item | Entry |
|---|---|
| Decision | `<Outcome>` |
| Rationale | `<Why>` |
| Options rejected | `<Options>` |
| Risks | `<Risks>` |
| Conditions | `<Conditions>` |
| Owner | `<Owner>` |
| Review date | `<Date>` |

## Executive Summary

> Reuse is appropriate only when business semantics, authority, quality, controls, ownership, lifecycle, and economics align. Accessibility alone is not evidence of suitability.
