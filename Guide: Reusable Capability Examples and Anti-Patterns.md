# Reusable Capability Examples and Anti-Patterns

## Purpose

Worked examples supporting [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md).

## 1. Account Opening

### Capabilities

- Identity verification
- Customer profile
- Product catalogue
- Eligibility
- Disclosure
- Application
- Workflow
- Notification

### Good composition

The journey orchestrator coordinates progress while domain services retain authoritative decisions.

### Anti-pattern

The portal implements identity, eligibility, and disclosure selection in frontend code.

## 2. Customer Dashboard

### Capabilities

- Customer profile
- Account summary
- Product holdings
- Notifications
- Content

### Pattern

A BFF aggregates read results and shapes a dashboard view. Non-critical sections may degrade independently.

### Anti-pattern

The dashboard directly queries several operational databases.

## 3. Product Pricing

### Good reuse

All channels call the same pricing capability with explicit product, customer, agreement, currency, and effective-time context.

### Anti-pattern

`WebPrice`, `MobilePrice`, and `PartnerPrice` are separately implemented without governed policy differences.

## 4. Payments

### Good reuse

A payment capability owns validation, authorization, state, idempotency, and outcome. The experience owns data capture and presentation.

### Anti-pattern

A BFF executes payment correctness rules and retries non-idempotent calls without a key.

## 5. Disclosures

### Good reuse

A policy capability selects the disclosure identifier and version. A content capability supplies governed structured content. Each experience renders it accessibly and records required acknowledgement.

### Anti-pattern

Every channel maintains copied disclosure text and independent effective dates.

## 6. Search

### Good reuse

A search projection is built from authorised sources with explicit freshness, access filtering, rebuild, and reconciliation.

### Anti-pattern

A generic search index exposes restricted content and relies on the UI to hide results.

## 7. Notification

### Good reuse

A notification platform handles delivery channels, templates, provider integration, retries, and delivery status. The domain capability determines when a business notification is required and supplies approved semantic data.

### Anti-pattern

The notification service begins deciding loan eligibility or account status.

## 8. Identity

### Good reuse

The platform authenticates users and workloads. Domain services enforce resource authorization and business entitlements.

### Anti-pattern

Every application implements its own identity store or assumes gateway authentication is sufficient authorization.

## 9. Common Utility Service

### Failure mode

A `CommonService` gradually accumulates dates, pricing, customer lookup, file conversion, notifications, and entitlements.

### Correction

Separate coherent platform utilities from domain capabilities. Assign owners and explicit contracts. Retire accidental shared logic through planned migration.

## 10. Copy-and-Paste Reuse

### Failure mode

A team copies a pricing library into a mobile backend.

### Consequence

Policy updates must be deployed in several places, producing drift.

### Correction

Use one authoritative capability or, where local execution is necessary, establish a governed rules product with versioning, distribution, testing, and audit.

## 11. Semantic Mismatch

### Failure mode

A marketing customer list is reused as the authoritative customer profile because it is easy to access.

### Consequence

Data may be stale, incomplete, or used outside its permitted purpose.

### Correction

Use the authoritative customer capability or publish an approved purpose-specific projection.

## 12. Synchronous Fan-Out

### Failure mode

A homepage waits for ten mandatory services.

### Consequence

Latency and failure probability compound.

### Correction options

- Mark non-critical sections optional.
- Run independent calls in parallel.
- Use bounded caching.
- Use a pre-composed projection.
- Load sections progressively.
- Redesign the journey.

## 13. Capability Concentration Risk

### Scenario

A shared identity or payment capability serves many critical journeys.

### Required controls

- Capacity model
- Isolation and quotas
- Resilience and recovery
- Consumer inventory
- Change governance
- Incident communication
- Continuity testing

Reuse reduces duplication but increases the importance of the shared dependency.

## Executive Summary

> Good composition reuses authoritative capabilities while preserving clear boundaries. Most failures arise from duplicated logic, semantic mismatch, missing ownership, direct data access, or unbounded runtime dependency chains.
