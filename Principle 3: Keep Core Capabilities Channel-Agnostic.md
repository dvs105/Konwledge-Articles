# Principle 3: Keep Core Capabilities Channel-Agnostic

## Overview

**Principle 3** requires core business services to remain independent of the channel, device, and presentation technology consuming them. A domain capability should implement business intent and authoritative policy, not web, mobile, portal, chatbot, or user-interface behaviour.

This principle builds on:

- [Headless Architecture Principles](Headless.md), particularly Principles 1 to 6.
- [API Gateway vs Core Business Logic](API%20Gateway%20vs%20Core%20Business%20Logic.md), which helps distinguish edge responsibilities from domain responsibilities.
- [Principle 1: Separate Experience from Business Capability](Principle_1_Separate_Experience_from_Business_Capability.md).
- [Principle 2: Design Contracts Before Consumers](Principle_2_Design_Contracts_Before_Consumers.md).
- [Design Contract Template](Design_Contract_Template_Principle_2.md).

Additional supporting guides created for this principle:

- [Business Intent and Domain-Oriented API Naming](Business_Intent_and_Domain_Oriented_API_Naming.md)
- [Channel Context and Explicit Policy Inputs](Channel_Context_and_Explicit_Policy_Inputs.md)
- [Channel-Agnostic Capability Examples](Channel_Agnostic_Capability_Examples.md)

---

## Principle Statement

> **Core business services MUST remain independent of the channel, device, and presentation technology consuming them.**

The same capability should be usable by:

- Web applications
- Mobile applications
- Employee and customer portals
- Contact-centre applications
- Partner integrations
- Chatbots and conversational experiences
- AI assistants
- Machine-to-machine consumers
- Future channels that do not yet exist

The business capability should not require redesign merely because a new experience is introduced.

---

## What Is a Core Capability?

A core capability is an authoritative business or platform function that delivers a defined outcome. Examples include:

- Customer management
- Product management
- Pricing
- Order management
- Payment processing
- Identity verification
- Loan eligibility assessment
- Account opening
- Entitlement evaluation
- Workflow processing

These capabilities exist because the business needs them. They should not be created around a page, button, device, or frontend framework.

For example:

```text
Assess Loan Eligibility
```

is a business capability. It remains the same capability when invoked by a website, mobile application, contact centre, partner portal, or AI assistant.

---

## Why This Principle Exists

Channel-neutral capabilities are easier to reuse and less likely to produce inconsistent customer or business outcomes.

Without channel neutrality, an organisation may create separate implementations such as:

```text
Web Pricing Service
Mobile Pricing Service
Portal Pricing Service
Partner Pricing Service
```

These implementations can drift over time through different release schedules, duplicated rules, incomplete policy updates, and inconsistent testing.

Potential consequences include:

- Different prices for equivalent requests
- Different eligibility or entitlement decisions
- Repeated implementation of the same rule
- Channel-specific defects
- Coordinated releases whenever policy changes
- Increased audit and assurance effort
- Difficulty introducing new channels

A channel-agnostic design instead aims for:

```text
Equivalent business context
          +
Same authoritative rule set
          =
Consistent business outcome
```

This does not mean every response must be visually identical. It means the authoritative business semantics and policy enforcement remain consistent.

---

## What Channel-Agnostic Does and Does Not Mean

### It means

- Business services express stable domain intent.
- Authoritative rules are implemented once in the appropriate domain boundary.
- Contracts use business terminology rather than UI terminology.
- Channels may present the same outcome differently without changing its meaning.
- Legitimate contextual differences are represented explicitly as policy inputs.
- Channel-specific composition remains outside the core domain capability.

### It does not mean

- Every channel must expose an identical user journey.
- Every consumer must receive the same payload shape.
- Channel context can never be supplied.
- A BFF or experience API is prohibited.
- Regulatory, risk, accessibility, or operational differences must be ignored.
- All capabilities must be implemented as microservices.

A modular monolith can expose a channel-neutral contract. Channel neutrality concerns responsibility and semantics, not deployment topology.

---

## Business Intent Rather Than Screen Actions

The document requires domain services to express business intent rather than screen actions.

### Good, domain-oriented operations

```text
AssessEligibility
CalculatePremium
CreateOrder
VerifyIdentity
SubmitApplication
ReserveInventory
ApprovePayment
```

These operations describe business outcomes or commands.

### Poor, UI-oriented operations

```text
SubmitButtonClicked
HomePagePricing
MobileCheckoutConfirmation
PortalFormSave
NextScreenValidation
```

These names expose presentation structure and make the contract dependent on a particular experience.

For a deeper naming and design guide, see [Business Intent and Domain-Oriented API Naming](Business_Intent_and_Domain_Oriented_API_Naming.md).

---

## Incorrect Design Example

The following endpoints divide the same eligibility capability by channel:

```http
POST /mobile-eligibility
POST /web-eligibility
POST /portal-eligibility
```

The implementation then branches on channel identity:

```text
if channel is mobile:
    apply mobile rules

if channel is web:
    apply web rules
```

This design is problematic when the rules differ merely because of presentation technology. It creates multiple implicit definitions of the same capability.

---

## Preferred Design Example

A domain-oriented contract exposes the business capability:

```http
POST /eligibility-assessments
```

Example request:

```json
{
  "applicantReference": "APPLICANT-12345",
  "annualGrossIncome": {
    "amount": "95000.00",
    "currency": "AUD"
  },
  "requestedAmount": {
    "amount": "25000.00",
    "currency": "AUD"
  }
}
```

Example response:

```json
{
  "decisionId": "DECISION-789",
  "outcome": "ELIGIBLE",
  "rulesetVersion": "eligibility-policy-1.0"
}
```

The contract expresses eligibility assessment, not web eligibility or mobile eligibility.

A complete illustrative design appears in [Filled Design Contract Example: Loan Eligibility API](Filled_Design_Contract_Example_Loan_Eligibility_API.md).

---

## Separation of Responsibilities

### Experience Channel

The experience channel may own:

- Rendering and layout
- Navigation
- Input assistance
- Local interaction state
- Accessibility of the rendered experience
- Device-specific interaction patterns
- Customer-facing wording approved for that channel
- Presentation-level validation for usability

### Experience API or BFF

A deliberately bounded experience API or backend-for-frontend may own:

- Aggregating calls to multiple capabilities
- Shaping payloads for one channel
- Translating protocols
- Experience-specific caching where safe
- Managing interaction-specific orchestration
- Mapping domain results to channel-ready view models

It must not become the authoritative location for core business rules.

### Core Domain Capability

The core capability owns:

- Authoritative business rules
- Business decisions and calculations
- Entitlement and eligibility evaluation
- Domain validation
- Domain state transitions
- Business invariants
- Auditable policy execution
- Authoritative error and outcome semantics

### API Gateway or Edge

The gateway or edge may own:

- Authentication support
- Routing
- Transport security
- Rate controls
- Payload-size controls
- Threat protection
- Edge telemetry

Core business logic should not be implemented in gateway scripts. See [API Gateway vs Core Business Logic](API%20Gateway%20vs%20Core%20Business%20Logic.md).

---

## When Channel Context Is Legitimate

Channel context may be passed where it has valid business, audit, risk, regulatory, fraud, or operational meaning.

Example:

```json
{
  "channelContext": "ASSISTED_SERVICE"
}
```

This can be legitimate if the context selects an explicit, governed policy, such as:

- A legally required disclosure for a particular interaction type
- A fraud-control policy based on the interaction context
- An audit requirement for staff-assisted processing
- A supported operational handoff
- A risk control that has been approved and documented

The service should not branch by channel merely because the consumer wants a different screen or payload shape.

For decision guidance and examples, see [Channel Context and Explicit Policy Inputs](Channel_Context_and_Explicit_Policy_Inputs.md).

---

## Policy Variation vs Presentation Variation

A central design task is distinguishing legitimate business-policy variation from presentation variation.

| Question | If yes | Likely owner |
|---|---|---|
| Does the difference change business correctness, risk, entitlement, financial outcome, or regulatory treatment? | It may be a policy variation | Domain or policy service |
| Does the difference only change layout, wording, navigation, or payload shape? | It is presentation variation | Experience or BFF |
| Must the difference be audited and governed as a business decision? | Represent it as explicit policy input | Domain/policy service |
| Would the difference disappear if the UI framework changed? | It is probably presentation-specific | Experience layer |

Avoid using `channel` as an ungoverned shortcut for hidden business behaviour. Prefer explicit context such as:

```json
{
  "interactionType": "STAFF_ASSISTED",
  "customerLocation": "AU",
  "riskContext": "REMOTE_ORIGINATION"
}
```

Each field must have a defined business meaning, owner, valid values, and policy effect.

---

## Response Composition

Core services should return business information, not presentation markup.

### Poor response

```json
{
  "html": "<div class='approved'>Eligible</div>",
  "buttonColour": "green",
  "nextScreen": "application-step-2"
}
```

This response assumes a particular rendering technology and journey structure.

### Better response

```json
{
  "outcome": "ELIGIBLE",
  "reasonCodes": [],
  "nextAction": "CONTINUE_TO_APPLICATION"
}
```

The channel decides how to render the result while preserving its business meaning.

Where a channel needs a specialised view model, an experience API or BFF may shape it without redefining the authoritative outcome.

---

## Reference Architecture Alignment

```text
Experience Channels
Web | Mobile | Portal | Partner | Contact Centre | Assistant
                          |
                          v
             Experience APIs / BFFs
             Aggregation | Shaping | Orchestration
                          |
                          v
                 Domain Capabilities
Customer | Product | Order | Pricing | Identity | Payment | Workflow
                          |
                          v
          Systems of Record and Authoritative Data
```

Principle 3 protects the **Domain Capabilities** layer from assumptions about the channels above it.

The domain capability should remain reusable when:

- A web application is redesigned.
- A native mobile application replaces a hybrid application.
- A chatbot or AI assistant is introduced.
- A partner is onboarded.
- An existing portal is retired.
- A new presentation framework is selected.

---

## Anti-Patterns

### Endpoint per screen or button

```text
POST /homepage-offer
POST /click-checkout
POST /save-portal-form
```

### Channel-specific business branches without policy justification

```text
if channel == web: apply rule A
if channel == mobile: apply rule B
```

### Presentation markup from a core service

```json
{
  "html": "<table>...</table>"
}
```

### Duplicate capability per channel

```text
Web Customer Service
Mobile Customer Service
Partner Customer Service
```

### Channel encoded into domain entity names

```text
MobileCustomer
PortalOrder
WebPrice
```

unless those terms represent genuinely distinct business concepts.

### Hidden policy behind generic channel flags

```json
{
  "channel": "MOBILE"
}
```

where consumers cannot understand which policy changes or why.

### BFF as duplicate domain layer

A BFF begins with response shaping but gradually owns pricing, eligibility, entitlements, or approval logic.

---

## Detailed Examples

The companion document [Channel-Agnostic Capability Examples](Channel_Agnostic_Capability_Examples.md) contains worked examples for:

- Loan eligibility
- Pricing
- Payment initiation
- Customer profile
- Product catalogue
- Identity verification
- Regulatory disclosures
- Fraud and risk context

---

## Architecture Review Questions

1. Does the service represent a stable business capability or a page, screen, widget, or channel?
2. Would the API name still make sense if the current user interface were replaced?
3. Can a new channel consume the capability without modifying core business logic?
4. Are equivalent inputs processed through the same authoritative rules?
5. Are channel differences backed by an explicit business, regulatory, audit, risk, or operational rationale?
6. Are policy inputs named by their business meaning rather than a generic channel flag?
7. Does the domain service return business outcomes rather than HTML, navigation, or visual styling?
8. Is channel-specific aggregation or payload shaping outside the domain service?
9. Are authoritative validation and authorization enforced server-side?
10. Are business semantics documented in a governed contract?
11. Does a channel-specific component duplicate any domain rule?
12. Can contract tests prove equivalent business treatment across channels?
13. Are the capability owner, policy owner, consumers, and lifecycle explicit?
14. Are deviations documented through the architecture exception process?

---

## Evidence of Conformance

Evidence may include:

- Domain-oriented API and event names
- Business capability map
- Bounded-context or service responsibility model
- API specifications using business terminology
- Explicit policy catalogue
- Consumer inventory
- BFF scope and responsibility statement
- Tests showing consistent outcomes across channels
- Source-code dependency analysis
- Architecture decision records
- Traceability from policy rules to implementation and tests

---

## Relationship to Other Principles

### Principle 1: Separate Experience from Business Capability

Business rules remain outside the experience layer.

### Principle 2: Design Contracts Before Consumers

A stable contract allows multiple channels to depend on business semantics rather than implementation details.

### Principle 4: Compose Experiences from Reusable Capabilities

Capabilities can be reused only when their semantics are not tied to one channel.

### Principle 5: Use an Experience API or BFF Deliberately

Channel-specific aggregation and response shaping can be isolated without moving authoritative domain logic into the BFF.

### Principle 6: Prefer Stable Domain Boundaries

Services align to durable business capabilities instead of pages, technical tiers, or organisational convenience.

### Principle 11: Treat Compatibility as a Product Obligation

Channel-neutral contracts still require controlled evolution, versioning, migration, and retirement.

### Principle 12: Build for Independent Delivery

Channels can evolve independently when core capabilities are stable and contract-governed.

---

## Practical Conformance Test

Ask:

> **If the organisation introduces a new channel tomorrow, can it reuse the capability without changing the authoritative business logic?**

A strong design usually answers yes. It may require a new experience composition or presentation adapter, but it should not require a duplicate domain capability merely because the channel is new.

Warning signs include:

```text
We need a new service for the mobile channel.
We need another implementation of the eligibility rule.
The core API returns the web page model.
The domain service needs to know which button was clicked.
The gateway contains the approval workflow.
```

---

## Executive Summary

> **Keep Core Capabilities Channel-Agnostic means that business services implement stable business intent and governed policy rather than user-interface behaviour, device-specific logic, or channel-specific workflows. A core capability should deliver consistent authoritative outcomes across web, mobile, portals, contact centres, assistants, partners, and future channels through reusable, governed contracts.**
