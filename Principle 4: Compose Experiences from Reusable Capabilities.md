# Principle 4: Compose Experiences from Reusable Capabilities

## Document Purpose

This document expands **Principle 4** of the enterprise Headless Architecture principles. It explains how digital experiences should be assembled from reusable business, content, data, integration, and platform capabilities without duplicating authoritative domain behaviour.

This document is technology-neutral. It applies whether the implementation uses a modular monolith, microservices, managed platforms, packaged applications, APIs, events, workflows, or a combination of these approaches.

## Related Repository Documents

### Foundation principles

- [Headless Architecture Principles](Headless.md)
- [Principle 1: Separate Experience from Business Capability](Principle_1_Separate_Experience_from_Business_Capability.md)
- [Principle 2: Design Contracts Before Consumers](Principle_2_Design_Contracts_Before_Consumers.md)
- [Principle 3: Keep Core Capabilities Channel-Agnostic](Principle_3_Keep_Core_Capabilities_Channel_Agnostic.md)
- [Principle 5: Use an Experience API or Backend-for-Frontend Deliberately](Principle_5_Use_an_Experience_API_or_BFF_Deliberately.md) *(future document link)*

### Contract and design guidance

- [Design Contract Template](Design_Contract_Template_Principle_2.md)
- [Filled Design Contract Example: Loan Eligibility API](Filled_Design_Contract_Example_Loan_Eligibility_API.md)
- [Business Intent and Domain-Oriented API Naming](Business_Intent_and_Domain_Oriented_API_Naming.md)
- [Channel Context and Explicit Policy Inputs](Channel_Context_and_Explicit_Policy_Inputs.md)
- [API Gateway vs Core Business Logic](API%20Gateway%20vs%20Core%20Business%20Logic.md)

### Principle 4 companion guides

- [Capability Reuse Assessment Framework](Capability_Reuse_Assessment_Framework.md)
- [Experience Composition Patterns](Experience_Composition_Patterns.md)
- [Capability Ownership and Product Management](Capability_Ownership_and_Product_Management.md)
- [Service Catalogue and Capability Discovery Guide](Service_Catalogue_and_Capability_Discovery_Guide.md)
- [Composition Governance and Architecture Review Guide](Composition_Governance_and_Architecture_Review_Guide.md)
- [Reusable Capability Examples and Anti-Patterns](Reusable_Capability_Examples_and_Anti_Patterns.md)

---

## 1. Principle Statement

> **Digital experiences SHOULD be composed from reusable, independently owned business, content, data, integration, and platform capabilities.**

This means that an experience should obtain authoritative business behaviour through supported contracts rather than recreating the behaviour inside the channel.

```text
Customer Experience
        |
        +--> Customer Profile Capability
        +--> Product Catalogue Capability
        +--> Pricing Capability
        +--> Eligibility Capability
        +--> Identity Capability
        +--> Payment Capability
        +--> Content Capability
```

The experience presents a coherent journey. The underlying capabilities remain independently owned, contract-governed, and reusable.

---

## 2. Relationship to Principles 1, 2, 3, and 5

### Principle 1 creates the separation

Experience concerns are separated from authoritative domain logic. Without that separation, there is nothing stable to reuse.

### Principle 2 creates the contract

A reusable capability needs an explicit contract defining inputs, outputs, errors, security, service quality, compatibility, and lifecycle.

### Principle 3 makes reuse possible across channels

A capability coupled to a specific page, device, or framework cannot be reused effectively by other channels.

### Principle 4 applies the capabilities

Experiences assemble governed capabilities into journeys instead of rebuilding customer, pricing, payment, identity, workflow, or content logic.

### Principle 5 bounds channel-specific composition

The future Principle 5 document will explain how an Experience API or BFF may aggregate and shape capabilities for a particular experience while remaining thin and avoiding duplication of domain logic.

```text
Principle 1: Separate
        |
Principle 2: Contract
        |
Principle 3: Make channel-neutral
        |
Principle 4: Reuse and compose
        |
Principle 5: Adapt deliberately for an experience
```

---

## 3. Definitions

### Capability

A business or platform function that delivers a defined outcome and has an accountable owner. Examples include `assess eligibility`, `calculate price`, `verify identity`, `retrieve customer profile`, and `publish approved content`.

### Reusable capability

A capability whose semantics, contract, service quality, security, ownership, and lifecycle are suitable for use by more than one approved consumer.

### Composition

The act of combining independent capabilities to deliver a user or machine journey. Composition may include aggregation, sequencing, protocol translation, response shaping, and state coordination.

### Orchestration

Explicit coordination of multiple interactions by a component or workflow that controls sequencing, decisions, recovery, and progress.

### Choreography

Coordination through independently reacting participants, commonly using events, without one component controlling the entire process.

### Experience API

An API that aggregates or shapes capabilities for the needs of an experience without becoming the authoritative domain layer.

### Backend-for-Frontend

A backend component dedicated to one experience or closely related family of experiences. It may optimise payloads, interaction patterns, and aggregation.

### Service catalogue

A governed inventory describing available capabilities, contracts, owners, consumers, lifecycle states, support arrangements, classifications, and service expectations.

### Semantic fit

The degree to which a capability's business meaning and behaviour match the consumer's actual requirement.

---

## 4. Why This Principle Exists

### 4.1 Faster delivery

A new channel can consume existing customer, identity, product, content, pricing, and payment capabilities rather than implement each function again.

### 4.2 Consistent business outcomes

When equivalent business context reaches the same authoritative capability and policy version, channels are less likely to produce contradictory outcomes.

### 4.3 Reduced duplication

Reuse reduces separately maintained rule sets, integrations, data access paths, security controls, and operational runbooks.

### 4.4 Easier policy change

A policy change is implemented in its authoritative capability and exposed through controlled contract evolution rather than repeated across channels.

### 4.5 Clear accountability

A capability owner can manage quality, risk, funding, compatibility, and retirement across known consumers.

### 4.6 Improved assurance

Security, privacy, resilience, observability, and compliance controls can be assessed around a known capability boundary.

### 4.7 Incremental modernisation

Legacy systems can be encapsulated behind stable contracts, allowing new experiences to use a supported capability while internal implementation evolves.

---

## 5. Composition Model

A composed experience commonly spans the following logical layers:

```text
+--------------------------------------------------------------------+
| Experience Channels                                                |
| Web | Mobile | Portal | Contact Centre | Partner | Assistant        |
+-------------------------------+------------------------------------+
                                |
                                v
+--------------------------------------------------------------------+
| Experience Composition                                             |
| Experience API | BFF | Journey Orchestrator | Presentation Adapter |
+-------------------------------+------------------------------------+
                                |
          +---------------------+---------------------+
          |                     |                     |
          v                     v                     v
+-------------------+  +-------------------+  +-------------------+
| Business          |  | Content and Data  |  | Platform          |
| Capabilities      |  | Capabilities      |  | Capabilities      |
| Customer, Price,  |  | Content, Search,  |  | Identity, Notify, |
| Order, Payment    |  | Media, Data       |  | Audit, Workflow   |
+---------+---------+  +---------+---------+  +---------+---------+
          |                     |                     |
          +---------------------+---------------------+
                                |
                                v
+--------------------------------------------------------------------+
| Systems of Record and Authoritative Sources                        |
+--------------------------------------------------------------------+
```

The experience composition layer should make the journey efficient without absorbing the authority of the capabilities it invokes.

---

## 6. Types of Reusable Capabilities

### 6.1 Business capabilities

Examples:

- Customer profile management
- Product availability
- Pricing and fees
- Eligibility and entitlement
- Payments
- Orders
- Case management
- Approval and workflow

### 6.2 Content capabilities

Examples:

- Structured content delivery
- Media delivery
- Disclosure selection
- Localisation
- Content search
- Publication-state enforcement

### 6.3 Data capabilities

Examples:

- Governed data products
- Reference data
- Search projections
- Reporting datasets
- Event streams

Data reuse must preserve ownership, classification, permitted use, freshness, and reconciliation requirements.

### 6.4 Integration capabilities

Examples:

- Approved adapters
- Protocol translation
- Event publication
- Managed file exchange
- External partner connectivity

Integration capabilities should not become unbounded containers for unrelated business logic.

### 6.5 Platform capabilities

Examples:

- Identity and workload authentication
- Secrets management
- Notification delivery
- Audit services
- Observability platforms
- Workflow engines
- API management

Platform services provide reusable technical capabilities, but the consuming product remains accountable for correct and secure use.

---

## 7. Required Practices

### 7.1 Search before creating

Teams MUST search the approved service, API, event, data, content, and platform catalogues before proposing a new capability.

The search should use business terminology and related domain language, not only the intended product or technology name.

### 7.2 Evaluate semantic fit

Reuse MUST be based on business meaning. An accessible endpoint is not necessarily a valid reusable capability.

Questions include:

- Does it implement the same business concept?
- Is its authoritative source appropriate?
- Are its state transitions and rules suitable?
- Does it provide the required currency, freshness, and consistency?
- Are its error and outcome semantics compatible?

### 7.3 Confirm ownership and lifecycle

A capability intended for reuse MUST have an accountable owner, supported lifecycle, roadmap or maintenance position, support route, and retirement process.

### 7.4 Confirm service quality

The consumer MUST assess availability, latency, throughput, capacity, recovery, support hours, and maintenance expectations against journey requirements.

### 7.5 Confirm security and privacy suitability

The consumer MUST assess exposure classification, authorization model, data classification, minimisation, permitted purpose, retention, residency, and logging restrictions.

### 7.6 Use governed contracts

Consumers MUST depend only on published contract behaviour. Undocumented fields, timing, implementation structures, or internal endpoints are not valid dependencies.

### 7.7 Locate composition deliberately

When multiple domain services are required, composition SHOULD occur in a deliberately selected experience API, BFF, workflow, or orchestration service.

### 7.8 Preserve domain authority

Composition components MUST NOT duplicate or override authoritative pricing, entitlement, eligibility, payment, approval, security, or data-integrity rules.

### 7.9 Register consumers

Critical consumers SHOULD be recorded so providers can assess contract changes, communicate incidents, plan capacity, and manage migrations.

### 7.10 Record the decision

The decision to reuse, extend, wrap, replace, or create a capability SHOULD be recorded with evidence and trade-offs.

---

## 8. Reuse Is Not Automatically Good

Reuse is valuable when it reduces duplication without weakening cohesion or creating inappropriate dependencies.

### 8.1 Reuse by coincidence

Two teams may use similar field names while having different business semantics. A `customer` in marketing, identity, settlement, and risk contexts may represent different bounded concepts.

### 8.2 Reuse with unsuitable quality

An internal, low-criticality service may not satisfy the availability or support requirements of a public critical journey.

### 8.3 Reuse that expands data exposure

A broad customer API may reveal more data than a new consumer requires. A purpose-specific projection or new contract may be safer.

### 8.4 Reuse that creates temporal coupling

A long chain of synchronous calls may technically reuse services while creating poor resilience and latency.

### 8.5 Reuse that transfers ownership risk

A project-owned service with no enduring support arrangement is not a sustainable shared capability.

### 8.6 Reuse that violates domain cohesion

A universal utility service containing unrelated business functions may appear reusable but becomes a distributed monolith and coordination bottleneck.

See [Capability Reuse Assessment Framework](Capability_Reuse_Assessment_Framework.md).

---

## 9. Selecting a Composition Pattern

Use the simplest pattern that satisfies the journey.

### 9.1 Direct consumption

```text
Experience -> One capability
```

Suitable when the capability contract already meets the consumer's needs and direct exposure is approved.

### 9.2 Client-side composition

```text
Experience -> Capability A
           -> Capability B
```

Suitable only when security, performance, consistency, failure behaviour, and client complexity are acceptable.

### 9.3 Experience API or BFF

```text
Experience -> BFF -> Capability A
                  -> Capability B
```

Useful for channel-specific aggregation, payload shaping, batching, protocol adaptation, and latency optimisation.

### 9.4 Journey orchestration

```text
Experience -> Orchestrator -> A -> B -> C
```

Useful when sequence, process state, recovery, compensation, or long-running coordination is meaningful.

### 9.5 Event-driven choreography

```text
Capability A --event--> Capability B --event--> Capability C
```

Useful when participants can react asynchronously and eventual consistency is acceptable.

### 9.6 Pre-composed projection

```text
Events -> Projection Store -> Experience Query
```

Useful for read-heavy experiences that need a stable, efficient view without synchronous fan-out.

Detailed guidance appears in [Experience Composition Patterns](Experience_Composition_Patterns.md).

---

## 10. BFF and Experience API Boundary

A BFF MAY:

- Aggregate several calls
- Shape response payloads
- Convert protocols
- Batch requests
- Apply experience-specific caching
- Translate domain outcomes into view-model structures
- Coordinate short-lived interaction steps

A BFF MUST NOT become the authoritative location for:

- Pricing rules
- Eligibility rules
- Entitlement decisions
- Approval policy
- Payment correctness
- Identity assurance decisions
- Authoritative cross-channel workflow state

If logic must remain correct across channels, survive channel replacement, or be audited as a business decision, it belongs in an authoritative domain or policy capability.

See the future [Principle 5](Principle_5_Use_an_Experience_API_or_BFF_Deliberately.md).

---

## 11. Orchestration vs Domain Logic

Orchestration coordinates capabilities. Domain logic decides business truth within a capability.

### Example

An account-opening journey may:

1. Verify identity.
2. Retrieve customer information.
3. assess product eligibility.
4. capture required disclosures.
5. create an application.
6. initiate approval.

The orchestrator may manage sequence, retries, progress, and compensation. It should not reimplement the identity, eligibility, disclosure, or approval rules.

### Decision test

Ask:

> If a different channel performed the same business process, would this behaviour still be required?

If yes, it may belong in a reusable workflow or domain process rather than a channel-specific BFF.

---

## 12. Capability Ownership Model

Every shared capability requires explicit accountability for:

- Business semantics
- Contract design
- Security and privacy
- Funding
- Capacity
- Reliability
- Operations and support
- Consumer communication
- Versioning and compatibility
- Deprecation and retirement

### Provider obligations

The provider should:

- Publish accurate contracts and examples.
- Maintain service objectives and support arrangements.
- Track critical consumers.
- Communicate material changes and incidents.
- Prefer compatible evolution.
- Provide migration guidance for breaking changes.
- Maintain observability and operational runbooks.

### Consumer obligations

The consumer should:

- Use supported contract behaviour only.
- Respect quotas, classification, and permitted purpose.
- Implement documented failure handling.
- Participate in compatibility testing where required.
- Maintain owner and dependency information.
- Complete migrations within agreed windows.

See [Capability Ownership and Product Management](Capability_Ownership_and_Product_Management.md).

---

## 13. Service Catalogue and Discovery

A useful catalogue entry should include:

- Capability name and business description
- Domain and accountable owner
- Contract locations
- Exposure classification
- Data classification
- Lifecycle state
- Supported versions
- Consumers and onboarding route
- Service objectives
- Quotas and limits
- Support and escalation
- Dependencies
- Deprecation notices
- Cost or funding model where applicable

A catalogue is not merely a list of URLs. It supports discovery, comparison, ownership, change impact, and lifecycle governance.

See [Service Catalogue and Capability Discovery Guide](Service_Catalogue_and_Capability_Discovery_Guide.md).

---

## 14. Reuse Decision Outcomes

A reuse assessment may produce one of several valid outcomes.

### Reuse as-is

The existing contract and quality meet the requirement.

### Reuse with consumer-side adaptation

A harmless presentation transformation is performed by the experience.

### Reuse through a BFF or experience API

Aggregation or payload shaping is required, but authoritative semantics remain unchanged.

### Extend the capability compatibly

The owner agrees to a semantically valid additive change.

### Publish a new capability contract over the same implementation

A distinct supported contract is needed to protect semantics or minimise data exposure.

### Create a new capability

No existing capability has suitable domain meaning, quality, ownership, or control characteristics.

### Do not proceed

Risk, cost, or unsupported dependency makes the proposed composition unsuitable.

---

## 15. Cost and Value Considerations

Reuse is not free. Shared capabilities require investment in:

- Contract quality
- Documentation
- Consumer support
- Capacity
- Compatibility
- Security assurance
- Observability
- Testing
- Migration
- Product management

A reuse decision should compare:

- Avoided duplicate build and maintenance
- Provider enhancement cost
- Consumer adaptation cost
- Increased operational criticality
- Additional capacity and data-transfer cost
- Change-coordination cost
- Migration and retirement cost
- Concentration risk

The objective is not maximum reuse. It is economically and architecturally appropriate reuse.

---

## 16. Reliability and Failure Behaviour

Composition introduces dependency relationships. A journey is only as reliable as its critical path and recovery design.

Required considerations include:

- Explicit timeouts
- Bounded retries
- Idempotency
- Circuit breaking
- Bulkhead isolation
- Back-pressure
- Fallbacks
- Partial results
- Pending states
- Compensation
- Reconciliation
- Dependency service objectives

### Avoid synchronous fan-out without analysis

```text
Experience -> BFF -> A
                  -> B
                  -> C
                  -> D
                  -> E
```

If every dependency is mandatory, aggregate availability and latency may be unsuitable. Consider pre-composed projections, asynchronous processing, optional sections, caching, or journey redesign.

---

## 17. Security and Privacy

Composition does not transfer security accountability away from the experience or provider.

### Key controls

- Authenticate users and workloads using approved mechanisms.
- Enforce authorization at the relevant resource and action boundary.
- Propagate identity and context only where required.
- Prevent confused-deputy behaviour in BFFs and orchestrators.
- Minimise data in requests, responses, caches, logs, and traces.
- Apply classification and purpose restrictions across every hop.
- Do not treat internal network location as trust.
- Protect service credentials and tokens.
- Audit material decisions and privileged actions.

### Aggregation risk

A composed response may combine data that has a higher sensitivity than any individual response. Classification and access control must consider the combined view.

---

## 18. Performance and Capacity

Composition can reduce client complexity but introduce additional calls and transformations.

Assess:

- Number of network hops
- Parallel versus sequential dependencies
- Payload size
- Serialization cost
- Cacheability
- Freshness
- Connection and quota limits
- Downstream rate limits
- Peak traffic multiplication
- Cold-start behaviour
- Third-party latency

Performance should be measured across the journey, not only inside each service.

---

## 19. Observability

A composed journey must remain traceable across boundaries.

Require:

- Correlation context across synchronous and asynchronous calls
- Structured logs
- Metrics by operation and dependency
- Distributed traces where appropriate
- Business outcome metrics
- Distinction between business rejection and technical failure
- Dependency dashboards
- Actionable alerts with owners
- Consumer and contract-version metadata where appropriate

The composition layer should expose which dependency affected an outcome without revealing sensitive implementation details to the end user.

---

## 20. Compatibility and Change Management

A shared capability may support consumers with different release cadences.

Providers should:

- Prefer additive compatible changes.
- Detect breaking changes in delivery pipelines.
- Test provider conformance.
- Support consumer-driven contract tests for critical consumers.
- Maintain a consumer inventory.
- Communicate deprecation.
- Measure runtime usage before retirement.

Consumers should:

- Ignore unknown optional fields where the contract requires it.
- Avoid depending on field order or undocumented behaviour.
- Use supported versions.
- Monitor deprecation notices.
- Migrate within agreed windows.

---

## 21. Common Anti-Patterns

### 21.1 Capability per channel

Separate customer, product, or pricing services are created for web, mobile, and partner channels.

### 21.2 Universal shared service

A `CommonService` accumulates unrelated functions and becomes a coupling hotspot.

### 21.3 Copy-and-paste reuse

Source code is copied rather than the authoritative capability being consumed.

### 21.4 Data-access reuse

Consumers directly query another capability's database instead of using a governed contract.

### 21.5 Semantic mismatch

An API is reused because it contains similar data even though its business meaning and permitted purpose differ.

### 21.6 BFF business layer

The BFF becomes the only place where pricing, eligibility, or entitlement is calculated.

### 21.7 Gateway workflow

Complex business orchestration is implemented in gateway policy scripts.

### 21.8 Synchronous dependency chain

A user request invokes a long serial chain for work that does not require an immediate result.

### 21.9 Shared capability with no owner

Multiple consumers depend on a service whose project has ended.

### 21.10 Permanent version accumulation

Versions are created without migration or retirement.

### 21.11 Reuse by access

A team assumes that because it can call an endpoint, it is approved and supported for its intended use.

### 21.12 Hidden cross-domain transaction

A composition component updates several domains without explicit process state, compensation, or reconciliation.

See [Reusable Capability Examples and Anti-Patterns](Reusable_Capability_Examples_and_Anti_Patterns.md).

---

## 22. Worked Example: Account Opening

### Experience objective

Allow a customer to open an account through web, mobile, or assisted service.

### Reusable capabilities

- Identity verification
- Customer profile
- Product catalogue
- Eligibility assessment
- Disclosure selection
- Application management
- Approval workflow
- Notification

### Composition

```text
Web / Mobile / Assisted Service
              |
              v
       Account Opening BFF
              |
              v
       Journey Orchestrator
       /   /   |   \    \
 Identity Customer Product Disclosure Application
```

### Responsibility boundary

- Experience: rendering, accessibility, interaction, progress display.
- BFF: channel-shaping and short-lived aggregation.
- Orchestrator: process sequence, checkpoints, retries, compensation.
- Domain capabilities: authoritative decisions and state.
- Platform capabilities: identity, audit, notification, telemetry.

### Failure example

If notification fails after the application is durably created, the application must not be rolled back merely because an email failed. The process records the application and retries or reconciles notification separately.

---

## 23. Architecture Decision Process

### Step 1: Define the business need

State the required outcome, consumer, criticality, data, timing, and quality attributes.

### Step 2: Discover candidate capabilities

Search catalogues, patterns, roadmaps, and domain documentation using business terms.

### Step 3: Assess candidates

Evaluate semantics, authority, contract, quality, security, lifecycle, cost, and ownership.

### Step 4: Select composition pattern

Choose direct use, BFF, orchestration, choreography, or projection.

### Step 5: Analyse end-to-end qualities

Model security, privacy, latency, availability, capacity, failure, recovery, observability, and cost.

### Step 6: Agree provider and consumer obligations

Document contracts, tests, support, onboarding, change communication, and migration.

### Step 7: Record the decision

Capture options, rationale, risks, consequences, and exceptions.

### Step 8: Validate through delivery evidence

Use machine-readable specifications, contract tests, traces, dashboards, and operational runbooks.

---

## 24. Architecture Review Questions

### Business and semantic fit

1. What business capability is being reused?
2. Is the provider authoritative for the relevant behaviour or data?
3. Do provider and consumer use the same business meaning?
4. Is any rule duplicated in the experience, BFF, gateway, or orchestrator?

### Boundaries and composition

5. Why was the chosen composition pattern selected?
6. Is composition separate from authoritative domain logic?
7. Are transaction and state boundaries explicit?
8. Is a cross-domain process model required?
9. Could a pre-composed read model avoid runtime fan-out?

### Ownership and lifecycle

10. Who owns the capability and contract?
11. Who funds, supports, secures, and retires it?
12. Are critical consumers known?
13. Is the capability active, deprecated, or strategic?

### Security and data

14. Is each data element required for the stated purpose?
15. Is authorization enforced by the protected capability?
16. Does aggregation increase sensitivity?
17. Are identity and policy context safely propagated?

### Reliability and performance

18. What happens when each dependency fails?
19. Are retries safe and bounded?
20. Is the critical path compatible with journey objectives?
21. Are downstream quotas and capacity modelled?

### Contracts and change

22. Are contracts machine-readable and governed?
23. Are breaking changes detected?
24. Are contract tests implemented?
25. Is deprecation and migration defined?

### Value and cost

26. What duplication is avoided?
27. What new concentration risk is introduced?
28. Is provider enhancement cheaper and safer than duplication?
29. Are platform, telemetry, support, and data-transfer costs included?

---

## 25. Conformance Checklist

- [ ] Existing capability catalogues were searched.
- [ ] Reuse candidates were assessed for semantic fit.
- [ ] The selected capabilities have accountable owners.
- [ ] Contracts, lifecycle states, and support arrangements are published.
- [ ] Security, privacy, and data-use suitability are confirmed.
- [ ] Service quality meets the end-to-end journey need.
- [ ] The composition pattern is justified.
- [ ] Domain authority is not duplicated in the composition layer.
- [ ] Cross-domain state and recovery are explicit.
- [ ] Timeouts, retries, idempotency, and reconciliation are defined.
- [ ] Correlation and observability span the journey.
- [ ] Capacity and downstream constraints are modelled.
- [ ] Provider and critical consumer contract tests exist.
- [ ] Consumer onboarding and support routes are documented.
- [ ] Versioning, deprecation, migration, and retirement are defined.
- [ ] Costs, concentration risk, and operational overhead are considered.
- [ ] Architecture decisions and exceptions are recorded.

---

## 26. Evidence of Conformance

Typical evidence includes:

- Capability map
- Service/API/event/data catalogue entries
- Reuse assessment
- Contract specifications
- Consumer inventory
- Context and sequence diagrams
- Composition responsibility model
- Resilience and failure analysis
- Threat model and privacy assessment
- Contract and integration tests
- Service-level objectives
- Capacity model
- Traces and dashboards
- Support and escalation model
- Version and migration plan
- Architecture decision records
- Exception records
- Cost and value analysis

---

## 27. Measures

Possible measures include:

- Time to onboard an approved consumer
- Supported consumers per capability
- Duplicate capabilities retired or avoided
- Percentage of critical consumers protected by contract tests
- Contract-version age and usage
- Provider availability and latency against objectives
- Consumer migration time
- Reuse-related incidents
- Support effort by consumer
- Cost per transaction or consumer where meaningful
- Lead time for a policy change to reach all channels

Measures should reward appropriate reuse, not merely the number of consumers or APIs.

---

## 28. Executive Summary

> **Compose Experiences from Reusable Capabilities means that channels assemble supported business, content, data, integration, and platform capabilities through governed contracts rather than rebuilding authoritative behaviour. Reuse must be based on semantic fit, ownership, security, service quality, lifecycle, resilience, and cost. Composition may occur through direct consumption, an Experience API, a BFF, orchestration, choreography, or a pre-composed projection, but it must not create a duplicate domain layer.**
