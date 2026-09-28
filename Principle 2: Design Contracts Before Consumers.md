# Principle 2: Design Contracts Before Consumers

## Overview

In a headless architecture, **APIs, events, and schemas are explicit, governed contracts**, not incidental implementation details. They define how consumers and providers interact and provide the stable boundary that allows each side to evolve independently.

This principle can be summarised as:

> **Define the interface before building the application that consumes it.**

The contract becomes the agreed boundary between:

- Frontend and backend teams
- Domain services and consumers
- Internal systems and partners
- Producers and consumers of events
- Human-facing and machine-facing channels

Rather than taking the approach:

> “Build the API and determine the payload later.”

teams should:

> “Agree the contract first, then build both sides against it.”

## Principle Statement

APIs, events, and schemas **MUST** be designed as explicit, governed contracts before or alongside consumer implementation.

This principle is the practical mechanism that supports separation between the experience layer and business capabilities. Consumers depend on a documented contract rather than the provider's internal implementation.

## Why This Principle Exists

Contracts form the stable boundary between independently evolving components.

A headless architecture assumes that:

- Frontends may evolve independently.
- Backend services may evolve independently.
- Channels may be deployed at different times.
- Different teams may own different capabilities.

Without a formal contract:

- Teams make undocumented assumptions.
- Payloads and behaviours become inconsistent.
- Breaking changes occur unexpectedly.
- Integrations become fragile.
- Testing and governance become difficult.

With a well-designed contract:

- Provider and consumer expectations are explicit.
- Teams can work in parallel.
- Changes can be governed and assessed for compatibility.
- Automated validation and contract testing become possible.
- Consumers remain insulated from internal implementation details.

## What Constitutes a Contract?

A contract should define:

- Expected inputs
- Expected outputs
- Error behaviour
- Security requirements
- Service quality expectations
- Ownership
- Semantics
- Compatibility rules
- Versioning and lifecycle expectations

### REST API Contract

For example, an API response may be defined as:

```json
{
  "customerId": "12345",
  "status": "Active"
}
```

The API contract may be documented using an OpenAPI specification.

### Event Contract

An event may be defined as:

```json
{
  "eventType": "CustomerCreated",
  "customerId": "12345",
  "timestamp": "2026-09-28T09:00:00Z"
}
```

The event contract may be documented using an AsyncAPI specification and registered in an approved schema registry.

### GraphQL Contract

A GraphQL interface may define:

```graphql
type Customer {
  id: ID!
  status: String!
}
```

In each case, the interface and its business meaning are agreed before or alongside implementation.

## Analogy: Building Construction

Consider the interface as the dimensions of a doorway in a building.

### Contract-Last Approach

```text
Builder creates doorway
        |
        v
Furniture team arrives later
        |
        v
Doorway is too small
        |
        v
Rework is required
```

### Contract-First Approach

```text
Door dimensions are agreed
        |
        v
Architect approves the design
        |
        v
Builder constructs the doorway
        |
        v
Furniture team designs accordingly
```

The teams can work independently because the boundary was agreed beforehand. An API or event contract serves the same purpose in a headless architecture.

## Practical Example: Product Pricing

Suppose an organisation is exposing a product-pricing capability.

### Poor Approach

The frontend team starts building against:

```text
GET /pricing
```

The backend team later implements:

```text
GET /loan-pricing
```

The frontend expects:

```json
{
  "rate": 4.5
}
```

The backend returns:

```json
{
  "interestRate": 4.5
}
```

The integration fails because the provider and consumer implemented different assumptions.

### Contract-First Approach

Before implementation begins, both teams agree to the contract:

```yaml
GET /pricing/{productId}

Response:
  productId: string
  interestRate: decimal
  effectiveDate: date
```

The frontend and backend teams then build and test against the same specification. The interface becomes predictable, testable, and governable.

## Required Practices

### 1. Synchronous APIs Must Have Machine-Readable Specifications

Synchronous APIs should be described through an appropriate machine-readable specification, such as:

- OpenAPI
- GraphQL schema
- An equivalent approved interface-description format

Machine-readable specifications can support:

- Consistent documentation
- Automated validation
- Mock services
- Contract tests
- Design and governance reviews

### 2. Events Must Have Defined Schemas

An event must have documented:

- Schema
- Ownership
- Business meaning and semantics
- Compatibility rules
- Expected producer and consumer behaviour

For example, an event named:

```text
CustomerCreated
```

must have a clearly defined meaning. Consumers should not have to guess:

- What business occurrence caused it
- When it is emitted
- Which fields are mandatory
- Whether fields can change
- How duplicate or out-of-order events should be handled

### 3. Contracts Must Describe Behaviour, Not Only Data

A useful contract defines more than field names. It should address:

- Valid inputs
- Successful outputs
- Error responses
- Authentication and authorisation expectations
- Relevant performance and availability expectations
- Compatibility and versioning behaviour

### 4. Contract Reviews Should Include Relevant Stakeholders

Contract reviews should involve appropriate representatives, including:

- Intended consumers
- Domain owners
- Security representatives
- Operational representatives

This helps identify ambiguous semantics, missing security controls, unclear support expectations, and operational gaps before implementation becomes difficult to change.

### 5. Consumer and Provider Teams Should Be Able to Work in Parallel

Where frontend and backend delivery proceed concurrently, teams should consider:

- Mock services
- Contract stubs
- Generated client or server models
- Consumer-driven contract testing

A typical parallel-delivery model is:

```text
Frontend Team
     |
     | builds and tests against a mock contract
     |
     v
Agreed API Contract
     ^
     |
     | implements and verifies the provider
     |
Backend Team
```

Both teams can progress without waiting for the full implementation of the other component.

## Connection to Headless Architecture

The document's logical architecture separates the following layers:

```text
Experience Channels
        |
        v
Experience APIs / BFFs
        |
        v
Domain Capabilities
```

The contract forms the boundary between these layers.

For example:

```text
Mobile Application
        |
        | Governed Contract
        v
Customer API
        |
        v
Customer Domain Service
```

The mobile application depends on the **published contract**, not on the service's source code, framework, database design, or other internal implementation details.

This supports independent evolution. The provider may change its internal technology without affecting consumers, provided the published contract and its behaviour remain compatible.

## Common Anti-Patterns

### Reverse-Engineering an Interface

```text
Inspect network calls
        |
        v
Guess the payload format
        |
        v
Build an undocumented integration
```

This creates a dependency on behaviour that may not be supported or stable.

### Exposing Internal Database Structures

For example:

```json
{
  "tbl_cust_id": "123"
}
```

Returning internal table and column structures directly to consumers couples the contract to the provider's implementation and makes future database changes more difficult.

### Undefined Event Schemas

Publishing an event such as:

```text
CustomerChanged
```

without defining what changed, why it was emitted, and which fields are present forces consumers to infer the event's meaning.

### Ambiguous or Overloaded Fields

For example:

```json
{
  "status": "Active"
}
```

The field becomes unsafe if different consumers interpret “Active” differently or if its valid values and business meaning are undocumented.

### Contract Design After Consumer Implementation

Allowing a consumer's code to become the de facto specification often embeds channel-specific assumptions into the provider interface and makes reuse harder.

## Practical Design Test

A contract-first design should be able to answer the following questions:

1. Is the contract documented in an approved machine-readable format?
2. Are inputs, outputs, errors, security, and service expectations defined?
3. Is the business meaning of each important field clear?
4. Can provider and consumer teams build and test independently?
5. Can mock services or contract stubs be generated or maintained?
6. Are breaking changes detectable through automated tests?
7. Are ownership, versioning, compatibility, and retirement expectations defined?
8. Does the contract expose a stable business capability rather than internal database or implementation structures?

## Evidence of Conformance

Typical evidence that this principle has been applied includes:

- OpenAPI specifications
- AsyncAPI specifications
- GraphQL schemas
- Equivalent approved contract specifications
- Schema registry entries
- Mock services or contract stubs
- Contract-testing results
- Consumer-driven contract tests
- API design-review records
- Versioning and compatibility decisions

## Relationship to Other Principles

Principle 2 directly supports:

- **Principle 1: Separate Experience from Business Capability**
- **Principle 3: Keep Core Capabilities Channel-Agnostic**
- **Principle 11: Treat Compatibility as a Product Obligation**
- **Principle 12: Build for Independent Delivery**
- **Principle 23: Make Ownership and Product Management Explicit**

Without well-defined contracts, the experience and business-capability layers cannot reliably evolve independently.

## Executive Summary

> **Design Contracts Before Consumers means that APIs, events, and schemas should be treated as governed products and designed before or alongside implementation. They provide a stable, versioned, testable boundary that allows channels, services, and teams to evolve independently while maintaining compatibility and reducing integration risk.**
