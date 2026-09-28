# Principle 1: Separate Experience from Business Capability

## Overview

In a headless architecture, the **user experience layer** (the “head”) and the **business capability layer** (the “body”) are deliberately separated.

The experience layer focuses on how users and systems interact with a capability. The business capability layer owns authoritative business behaviour, rules, workflows, integrations, and data access.

## What Each Layer Includes

### Experience Layer

Examples include:

- Websites
- Mobile applications
- Portals
- Chatbots
- AI assistants
- Partner-facing applications

### Business Capability Layer

Examples include:

- Customer management
- Product management
- Pricing
- Orders
- Identity
- Payments
- Workflows
- Systems of record such as CRM, ERP, IAM, and HR systems

Presentation, interaction, and channel-specific concerns **must not contain or own authoritative business logic**. Business rules should instead reside in authoritative backend services and be accessed through governed APIs and events.

## Why This Principle Exists

A headless architecture delivers value when:

- Frontends can change without requiring changes to business services.
- Business services can evolve without unnecessarily breaking channels.
- Business rules are implemented once and reused consistently.
- Frontend and backend teams can work and release more independently.

Without this separation:

- Web and mobile teams may duplicate business logic.
- Different channels may produce inconsistent outcomes.
- Frontend and backend releases become tightly coupled.
- Business rules become scattered across applications.

## Example: Loan Eligibility

Consider a banking capability that determines whether a customer is eligible for a loan.

### Incorrect Approach

The website and mobile application each implement the eligibility rule independently:

```text
Website:
  if age > 18 and salary > 50000
      eligible
```

```text
Mobile application:
  if age > 18 and salary > 50000
      eligible
```

If the policy changes, every channel must be updated. Channels may also produce inconsistent decisions if one implementation is missed or changed incorrectly.

### Correct Approach

The eligibility rule is implemented once in an authoritative business service:

```text
Website / Mobile App / Chatbot / Partner Portal
                         |
                         v
              Loan Eligibility API
                         |
                         v
              Business Rules Service
```

All channels call the same backend capability. When the policy changes, the authoritative service is updated and each channel continues to receive the consistent result through the established contract.

## Alignment with the Reference Architecture

The document’s logical architecture uses the following separation:

```text
Experience Channels
       |
       v
Experience APIs / BFFs
       |
       v
Domain Capabilities
       |
       v
Systems of Record
```

Each layer has a distinct responsibility:

- **Experience channels** present information and manage user interaction.
- **Experience APIs or Backends-for-Frontend (BFFs)** may aggregate calls, shape responses, and handle channel-specific orchestration.
- **Domain capabilities** own authoritative business logic and business behaviour.
- **Systems of record** own authoritative business data and transactions.

## What Is Allowed in the Frontend

This principle does not require the frontend to be “dumb.” Frontends may contain:

- Presentation and formatting logic
- Input assistance and user-friendly validation
- Navigation logic
- Local user-interface state
- Screen rendering
- Channel-specific interaction behaviour
- Personalisation that does not become authoritative business logic

Examples include:

```text
Show a red border when a mandatory field is empty.
Disable the Submit button until required fields are entered.
Display a loading indicator while a request is processed.
Remember whether a page section is expanded.
```

These are experience concerns and appropriately belong in the experience layer.

## What Must Remain in Backend Services

Rules or validation affecting the following areas must be enforced server-side:

- Business correctness
- Security and authorisation
- Financial outcomes
- Entitlements
- Data integrity
- Regulatory or policy decisions

Examples include:

- Pricing calculations
- Credit limits
- User authorisation
- Interest calculations
- Approval routing
- Eligibility decisions
- Account limits

A frontend may repeat some validation to improve usability, but the backend implementation remains authoritative.

## Practical Design Test

A design is conformant when a change to a visual component, navigation flow, or presentation framework does not require a change to the domain service unless the underlying business capability or contract has also changed.

Useful review questions include:

1. Is any authoritative business rule implemented only in a browser, mobile application, or other channel?
2. Do web and mobile channels implement separate versions of the same rule?
3. Does a backend service depend on a particular screen, button, widget, or frontend framework?
4. Can another channel reuse the capability through a governed contract?
5. Is security-sensitive and business-critical validation enforced by the backend?
6. Are channel-specific transformations isolated from the core domain capability?

## Relationship to Other Principles

This principle supports several other headless architecture principles:

- **Principle 3: Keep Core Capabilities Channel-Agnostic**
- **Principle 4: Compose Experiences from Reusable Capabilities**
- **Principle 5: Use an Experience API or Backend-for-Frontend Deliberately**
- **Principle 7: Encapsulate Data Ownership**
- **Principle 12: Build for Independent Delivery**

Principle 1 establishes the architectural boundary that enables reuse, channel neutrality, governed data access, and independent delivery.

## Executive Summary

> **Separate Experience from Business Capability means that websites, mobile applications, portals, chatbots, and AI assistants should focus on presenting and consuming capabilities, while authoritative business rules, workflows, integrations, and data ownership remain in reusable backend domain services accessed through governed APIs and events.**
