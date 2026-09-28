# Service Catalogue and Capability Discovery Guide

## Purpose

This guide defines how teams discover and describe reusable capabilities before creating new ones. It supports [Principle 4](Principle_4_Compose_Experiences_From_Reusable_Capabilities.md).

## 1. Catalogue Scope

The catalogue should cover:

- Business APIs
- Events and schemas
- Data products
- Content APIs
- Platform services
- Integration adapters
- Experience APIs and BFFs
- Workflows and orchestrations

## 2. Minimum Entry

| Attribute | Description |
|---|---|
| Capability name | Stable business or platform name |
| Purpose | Outcome delivered |
| Domain | Owning bounded context |
| Owner | Business, product, technical, operations |
| Contracts | Specifications and examples |
| Exposure | Public, partner, workforce, internal, service-to-service |
| Classification | Data and information handling |
| Lifecycle | Proposed, active, deprecated, retired |
| Service quality | Objectives, limits, maintenance |
| Consumers | Known critical dependants |
| Support | Contact and escalation |
| Change policy | Compatibility and deprecation |
| Onboarding | Access and assurance process |

## 3. Search Process

1. Describe the needed business outcome.
2. Identify domain vocabulary and synonyms.
3. Search capability, API, event, data, content, and platform catalogues.
4. Review related architecture patterns and roadmaps.
5. Contact owners of plausible candidates.
6. Perform a reuse assessment.
7. Record the selected or rejected candidates.

## 4. Search Terms

Search by business nouns and verbs, for example:

- `assess eligibility`
- `customer contact details`
- `authorise payment`
- `publish disclosure`
- `product availability`

Do not search only by project or technology name.

## 5. Catalogue Quality

A high-quality catalogue is:

- Current
- Searchable
- Owned
- Linked to source-controlled contracts
- Connected to runtime and lifecycle evidence
- Clear about support and permitted use

A stale catalogue may be worse than no catalogue because it encourages unsupported dependencies.

## 6. Discovery Anti-Patterns

- Creating a service before searching.
- Searching only for exact technology names.
- Treating an API gateway inventory as a capability catalogue.
- Listing endpoints without business semantics or owners.
- Leaving deprecated contracts discoverable as preferred choices.
- Failing to document rejected reuse options.

## 7. Candidate Comparison

| Candidate | Semantic fit | Owner | Lifecycle | Quality | Security/data | Decision |
|---|---|---|---|---|---|---|
| `<A>` | `<Fit>` | `<Owner>` | `<State>` | `<Fit>` | `<Fit>` | `<Decision>` |

## 8. Governance

- Owners review entries periodically and after material change.
- Publication should be integrated with delivery pipelines where practical.
- Deprecated contracts must show replacement and retirement information.
- Catalogue access must not expose sensitive endpoints or credentials.
- Runtime usage should inform lifecycle decisions where available.

## Executive Summary

> Discovery begins with business intent and ends with an evidence-based reuse decision. A useful catalogue describes semantics, ownership, contracts, lifecycle, quality, controls, and onboarding rather than merely listing endpoints.
