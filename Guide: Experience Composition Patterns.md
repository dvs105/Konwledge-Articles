# Experience Composition Patterns

## Purpose

This guide supports selection of a composition pattern under [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md) and links forward to [Principle 5](Principle_5_Use_an_Experience_API_or_BFF_Deliberately.md).

## 1. Direct Capability Consumption

```text
Experience -> Capability
```

### Use when

- One capability satisfies the journey need.
- Direct exposure is approved.
- Payload and protocol are suitable.
- Client security and failure handling are acceptable.

### Risks

- Client becomes coupled to domain contracts.
- Public clients may be exposed to unnecessary complexity.
- Multiple calls may migrate into the client over time.

## 2. Client-Side Composition

```text
Experience -> Capability A
           -> Capability B
```

### Use when

- Calls are independent.
- Partial results are acceptable.
- Client capabilities and security model are suitable.

### Avoid when

- The client would hold privileged credentials.
- Cross-service authorization is complex.
- Many network round trips impair the experience.
- Process state must be authoritative.

## 3. Backend-for-Frontend

```text
Web -> Web BFF -> Domain capabilities
Mobile -> Mobile BFF -> Domain capabilities
```

### Responsibilities

- Channel-specific aggregation
- Response shaping
- Protocol translation
- Limited experience caching
- Reduction of client round trips

### Guardrail

A BFF does not own authoritative business rules. See the future Principle 5 document.

## 4. Shared Experience API

```text
Related experiences -> Experience API -> Domain capabilities
```

Useful when several related experiences share a journey model. Avoid turning it into a universal enterprise backend.

## 5. Journey Orchestrator

```text
Experience -> Orchestrator -> A -> B -> C
```

### Use when

- Sequence matters.
- Process state must be durable.
- Compensation and recovery are explicit.
- The journey is long-running or crosses domains.

### Guardrails

- Domain rules remain in domain services.
- Workflow state and business state are distinguished.
- Every failure path has recovery or reconciliation.

## 6. Event-Driven Choreography

```text
A --event--> B --event--> C
```

### Use when

- Eventual consistency is acceptable.
- Participants can react independently.
- Producers need not know all consumers.

### Risks

- Hidden process flow
- Duplicate and out-of-order delivery
- Difficult end-to-end diagnosis
- Schema evolution

Require idempotency, correlation, schema governance, replay design, and reconciliation.

## 7. Pre-Composed Read Projection

```text
Domain events -> Projection builder -> Read store -> Experience
```

### Use when

- The experience is read-heavy.
- Runtime fan-out would be slow or fragile.
- Defined freshness is acceptable.

### Guardrails

- Projection is derived, not authoritative.
- Rebuild and reconciliation are designed.
- Authorization and data classification are preserved.

## 8. API Gateway Aggregation

Use only for simple, bounded technical aggregation supported by the approved platform. Do not place domain workflows or policy decisions in gateway scripts. See [API Gateway vs Core Business Logic](API%20Gateway%20vs%20Core%20Business%20Logic.md).

## 9. Selection Matrix

| Need | Likely pattern |
|---|---|
| One suitable capability | Direct consumption |
| Simple independent calls | Client composition |
| Channel-specific payload | BFF |
| Related journey family | Experience API |
| Durable multi-step process | Orchestrator |
| Asynchronous propagation | Choreography |
| Efficient read-heavy view | Projection |

## 10. Pattern Decision Questions

1. Is an immediate response required?
2. Does sequence matter?
3. Is durable process state required?
4. Can partial results be shown?
5. What happens when each dependency fails?
6. Is the logic channel-specific or cross-channel business behaviour?
7. Can calls run in parallel?
8. Is eventual consistency acceptable?
9. Does the pattern preserve domain ownership?
10. Who operates and supports the composition component?

## Executive Summary

> Select the simplest pattern that meets the journey's semantics, security, reliability, performance, and lifecycle needs while preserving authoritative domain boundaries.
