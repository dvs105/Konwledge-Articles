# API Gateway vs Core Business Logic

This statement appears in the **Common Misconceptions** section of the Headless Architecture document and specifically corrects the misconception:

> **The API gateway is the business layer.**

The document's response is:

> **Incorrect. Gateways enforce cross-cutting policies and route traffic. Core business logic belongs in owned domain capabilities.**

## What It Means in the Context of the Document

The architecture distinguishes between the responsibilities of the API gateway and those of domain capabilities.

### API Gateway Responsibilities

The gateway sits in the **API and Access Management** layer of the reference architecture. It is responsible for common platform concerns such as:

- Authentication support
- Routing
- Rate limiting
- Traffic management
- Telemetry and analytics
- Transport security
- Policy enforcement

These are **cross-cutting concerns** because they apply consistently across multiple APIs and services.

The document explicitly requires that:

> **Edge controls MUST NOT contain core business logic.**

It also identifies the following as an anti-pattern:

> **Implementing business workflows in gateway scripts.**

### Domain Capability Responsibilities

The business services in the **Domain Capabilities** layer own business decisions and rules. Examples in the reference architecture include:

- Customer
- Product
- Order
- Pricing
- Identity
- Payment
- Workflow

These services are where the organisation's authoritative business behaviour should reside.

Examples of logic that belongs in domain capabilities include:

- Pricing calculations
- Customer eligibility decisions
- Approval rules
- Payment validation
- Entitlement checks
- Order-processing rules

## Practical Example

### Correct Design

```text
Client
  |
API Gateway
  |
Order Service
```

The gateway:

- Validates the OAuth token.
- Applies rate limits.
- Routes the request to the Order Service.

The Order Service:

- Checks stock.
- Calculates pricing.
- Applies business rules.
- Creates the order.

This follows the principle that business logic belongs in the domain service that owns the capability.

### Incorrect Design

```text
Client
  |
API Gateway
  |
Order Service
```

The gateway:

- Calculates discounts.
- Applies approval rules.
- Determines customer eligibility.
- Decides whether an order may proceed.

The Order Service merely stores data.

In this design, the gateway has become the business layer, which the architecture principles explicitly discourage.

## Why This Separation Matters

Placing business logic in gateways can cause:

- Business rules to become scattered across infrastructure components.
- Unclear domain ownership.
- More difficult testing and change management.
- Duplication of logic across channels or gateways.
- Gateway configuration changes to become business-function changes.
- Reduced reuse because business behaviours are not encapsulated within domain capabilities.

Keeping business rules in **owned domain capabilities** allows web, mobile, partner, assistant, and machine consumers to use the same authoritative business behaviour consistently.

## Summary

An API gateway is a traffic-control and cross-cutting policy-enforcement point, not a business-processing layer. Business decisions and authoritative rules must remain in the domain services that own the relevant business capabilities.
# API Gateway vs Core Business Logic

This statement appears in the **Common Misconceptions** section of the Headless Architecture document and specifically corrects the misconception:

> **The API gateway is the business layer.**

The document's response is:

> **Incorrect. Gateways enforce cross-cutting policies and route traffic. Core business logic belongs in owned domain capabilities.**

## What It Means in the Context of the Document

The architecture distinguishes between the responsibilities of the API gateway and those of domain capabilities.

### API Gateway Responsibilities

The gateway sits in the **API and Access Management** layer of the reference architecture. It is responsible for common platform concerns such as:

- Authentication support
- Routing
- Rate limiting
- Traffic management
- Telemetry and analytics
- Transport security
- Policy enforcement

These are **cross-cutting concerns** because they apply consistently across multiple APIs and services.

The document explicitly requires that:

> **Edge controls MUST NOT contain core business logic.**

It also identifies the following as an anti-pattern:

> **Implementing business workflows in gateway scripts.**

### Domain Capability Responsibilities

The business services in the **Domain Capabilities** layer own business decisions and rules. Examples in the reference architecture include:

- Customer
- Product
- Order
- Pricing
- Identity
- Payment
- Workflow

These services are where the organisation's authoritative business behaviour should reside.

Examples of logic that belongs in domain capabilities include:

- Pricing calculations
- Customer eligibility decisions
- Approval rules
- Payment validation
- Entitlement checks
- Order-processing rules

## Practical Example

### Correct Design

```text
Client
  |
API Gateway
  |
Order Service
```

The gateway:

- Validates the OAuth token.
- Applies rate limits.
- Routes the request to the Order Service.

The Order Service:

- Checks stock.
- Calculates pricing.
- Applies business rules.
- Creates the order.

This follows the principle that business logic belongs in the domain service that owns the capability.

### Incorrect Design

```text
Client
  |
API Gateway
  |
Order Service
```

The gateway:

- Calculates discounts.
- Applies approval rules.
- Determines customer eligibility.
- Decides whether an order may proceed.

The Order Service merely stores data.

In this design, the gateway has become the business layer, which the architecture principles explicitly discourage.

## Why This Separation Matters

Placing business logic in gateways can cause:

- Business rules to become scattered across infrastructure components.
- Unclear domain ownership.
- More difficult testing and change management.
- Duplication of logic across channels or gateways.
- Gateway configuration changes to become business-function changes.
- Reduced reuse because business behaviours are not encapsulated within domain capabilities.

Keeping business rules in **owned domain capabilities** allows web, mobile, partner, assistant, and machine consumers to use the same authoritative business behaviour consistently.

## Summary

An API gateway is a traffic-control and cross-cutting policy-enforcement point, not a business-processing layer. Business decisions and authoritative rules must remain in the domain services that own the relevant business capabilities.
