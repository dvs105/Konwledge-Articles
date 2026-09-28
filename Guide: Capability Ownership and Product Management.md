# Capability Ownership and Product Management

## Purpose

Reusable capabilities create enterprise dependencies. This guide defines the minimum ownership and product-management expectations supporting [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md).

## 1. Ownership Dimensions

| Dimension | Accountability |
|---|---|
| Business semantics | Meaning, policy, business outcomes |
| Product | Roadmap, consumers, prioritisation, funding |
| Technical | Design, implementation, quality, compatibility |
| Security/privacy | Controls, risk, data use, assurance |
| Operations | Monitoring, incidents, support, continuity |
| Contract lifecycle | Versions, deprecation, migration, retirement |

One person may hold several roles, but no responsibility should be unowned.

## 2. Provider Product Obligations

A shared capability provider should maintain:

- Product purpose and scope
- Domain glossary
- Machine-readable contracts
- Consumer onboarding
- Known critical consumers
- Service objectives and limits
- Roadmap and maintenance position
- Security and privacy classification
- Support and escalation
- Compatibility policy
- Deprecation and retirement plan
- Cost and capacity model

## 3. Consumer Obligations

Consumers should:

- Register critical use.
- Use supported versions and behaviour.
- Protect data and credentials.
- Implement documented failure handling.
- Provide demand and capacity information.
- Participate in change impact and tests.
- Migrate within agreed windows.
- Maintain their own dependency owner.

## 4. Lifecycle

```text
Proposed -> Incubating -> Active -> Constrained -> Deprecated -> Retired
```

### Proposed

Business need and ownership are being validated.

### Incubating

Early consumers help stabilise semantics and operations.

### Active

Supported for approved production use.

### Constrained

New onboarding is restricted while risks, capacity, or replacement are addressed.

### Deprecated

Replacement and migration guidance exist; new use is normally prohibited.

### Retired

Production use is disabled and catalogue records are retained for traceability.

## 5. Funding and Capacity

Shared capability funding should account for:

- Baseline operations
- Growth from new consumers
- Security and compliance maintenance
- Contract evolution
- Consumer support
- Resilience and recovery
- Migration and retirement

Unfunded reuse transfers hidden cost and risk to the provider.

## 6. Consumer Council

For critical shared capabilities, a lightweight consumer forum may help coordinate roadmap, changes, incidents, and migration. It must not replace the accountable product owner.

## 7. Product Health Indicators

- Availability and latency against objectives
- Consumer onboarding lead time
- Support volume
- Capacity utilisation
- Contract test coverage
- Deprecated-version usage
- Security and reliability findings
- Cost per transaction or consumer where meaningful

## 8. Ownership Checklist

- [ ] Business owner assigned.
- [ ] Product owner assigned.
- [ ] Technical owner assigned.
- [ ] Operations and support assigned.
- [ ] Security and data accountability assigned.
- [ ] Funding model defined.
- [ ] Consumers and dependencies recorded.
- [ ] Lifecycle and retirement defined.

## Executive Summary

> A capability is not reusable merely because it has an API. Sustainable reuse requires explicit product ownership, funding, support, service quality, compatibility, and retirement accountability.
