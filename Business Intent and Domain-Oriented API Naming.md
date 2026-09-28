# Business Intent and Domain-Oriented API Naming

## Purpose

This guide expands [Principle 3: Keep Core Capabilities Channel-Agnostic](Principle_3_Keep_Core_Capabilities_Channel_Agnostic.md) by showing how names, operations, and resources can express stable business intent instead of user-interface implementation.

Also see:

- [Headless Architecture Principles](Headless.md)
- [Principle 2: Design Contracts Before Consumers](Principle_2_Design_Contracts_Before_Consumers.md)
- [Design Contract Template](Design_Contract_Template_Principle_2.md)

---

## Core Rule

> Name an interface after the business capability, resource, command, query, or event it represents, not after a screen, button, device, or framework.

A name should remain meaningful if the current frontend is replaced.

---

## Naming Tests

Before approving an interface name, ask:

1. Does it describe a business outcome or domain concept?
2. Would it still make sense to a mobile, partner, contact-centre, and machine consumer?
3. Does it avoid page, widget, button, and framework terminology?
4. Is the term defined in the domain glossary?
5. Does the name hide multiple unrelated responsibilities?
6. Does it imply an implementation detail such as a table, stored procedure, or vendor product?

---

## Examples

| UI-oriented name | Domain-oriented alternative | Why it is better |
|---|---|---|
| `submitButtonClicked` | `submitApplication` | Describes a business command |
| `/mobile-checkout` | `/orders/{id}/payment-authorisations` | Expresses domain resources |
| `/homepage-offer` | `/offer-eligibility-assessments` | Separates capability from page location |
| `/save-step-2` | `/applications/{id}` with `PATCH` | Represents state change, not navigation |
| `PortalCustomerService` | `CustomerProfileService` | Reusable by any channel |
| `getScreenData` | `getProductAvailability` | States the requested business information |
| `CustomerChanged` | `CustomerContactDetailsUpdated` | Gives an event precise business meaning |

---

## Commands, Queries, and Events

### Commands

Commands request a business action:

```text
SubmitApplication
ReserveInventory
AuthorisePayment
VerifyIdentity
CancelOrder
```

A command should indicate intent, not implementation steps.

### Queries

Queries request business information:

```text
GetCustomerProfile
SearchAvailableProducts
RetrieveEligibilityDecision
GetOrderStatus
```

A query should not expose internal tables or screen models.

### Events

Events describe something that has occurred:

```text
ApplicationSubmitted
PaymentAuthorised
CustomerContactDetailsUpdated
OrderCancelled
```

Avoid vague events such as `Changed`, `UpdatedData`, or `ProcessComplete` without clear domain meaning.

---

## Resource-Oriented Example

### Weak

```http
POST /clickApplyButton
GET /getSecondPageData
POST /saveMobileForm
```

### Stronger

```http
POST /applications
GET /applications/{applicationId}
PATCH /applications/{applicationId}
POST /applications/{applicationId}/submissions
```

The stronger interface can support many experiences because it represents domain resources and actions.

---

## Avoid Leaking Internal Implementation

Do not expose fields such as:

```json
{
  "tbl_customer_pk": 123,
  "is_mobile_screen": true,
  "stored_proc_status": 0
}
```

Prefer stable business semantics:

```json
{
  "customerId": "CUSTOMER-123",
  "contactPreference": "DIGITAL",
  "status": "ACTIVE"
}
```

---

## Naming Review Checklist

- [ ] Uses vocabulary from the domain glossary.
- [ ] Describes business intent or a stable domain resource.
- [ ] Avoids web, mobile, portal, page, button, widget, and framework names.
- [ ] Avoids database, vendor, or internal implementation terminology.
- [ ] Is precise enough for consumers to understand without reverse-engineering.
- [ ] Has documented request, response, error, security, and compatibility semantics.
- [ ] Has an owner and lifecycle state.
- [ ] Remains meaningful across present and future channels.

## Executive Summary

> A well-named contract describes what the business capability does, not how one user interface invokes it. Domain-oriented naming is a practical control against channel coupling.
