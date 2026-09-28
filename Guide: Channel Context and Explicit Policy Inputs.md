# Channel Context and Explicit Policy Inputs

## Purpose

This guide explains when channel context is legitimate and how to avoid turning a generic `channel` field into hidden business logic. It supports [Principle 3: Keep Core Capabilities Channel-Agnostic](Principle_3_Keep_Core_Capabilities_Channel_Agnostic.md).

Also see:

- [Headless Architecture Principles](Headless.md)
- [Design Contract Template](Design_Contract_Template_Principle_2.md)
- [Filled Design Contract Example: Loan Eligibility API](Filled_Design_Contract_Example_Loan_Eligibility_API.md)

---

## Core Rule

> Pass context only when it has explicit business, audit, regulatory, risk, security, or operational meaning. Do not use channel identity as a shortcut for presentation-specific behaviour.

---

## Legitimate Uses

Channel or interaction context may affect a capability when it selects a governed policy, for example:

- Staff-assisted versus customer self-service processing
- Remote versus in-person identity verification
- A disclosure required for a particular regulated interaction
- Fraud controls that depend on interaction conditions
- Audit evidence required for an assisted transaction
- Operational routing that has defined business meaning

Example:

```json
{
  "interactionContext": {
    "interactionType": "STAFF_ASSISTED",
    "originationMode": "REMOTE",
    "customerRegion": "AU"
  }
}
```

Each value must have defined semantics and a known policy effect.

---

## Weak Channel Flag

```json
{
  "channel": "MOBILE"
}
```

This is risky when the service contains hidden branches such as:

```text
if MOBILE, reduce validation
if WEB, show another price
if PORTAL, skip approval
```

Consumers cannot see the actual business rationale, and policy becomes coupled to channel labels.

---

## Prefer Explicit Business Context

Instead of:

```json
{
  "channel": "MOBILE"
}
```

consider explicit fields where justified:

```json
{
  "interactionType": "CUSTOMER_SELF_SERVICE",
  "presentmentMode": "REMOTE_DIGITAL",
  "authenticationStrength": "PHISHING_RESISTANT",
  "customerJurisdiction": "AU"
}
```

These fields explain the business context directly. They should only be introduced when required and approved.

---

## Decision Matrix

| Difference | Domain policy input? | Experience concern? |
|---|---:|---:|
| Different page layout | No | Yes |
| Native mobile gesture | No | Yes |
| Different payload shape for screen efficiency | No | Yes, often BFF |
| Regulatory disclosure determined by jurisdiction and interaction type | Yes | Rendering also remains with experience |
| Fraud treatment based on verified risk context | Yes | No, except presentation of outcome |
| Different colour or wording | No | Yes |
| Staff-assisted audit requirement | Yes | Experience captures required evidence |
| Pricing difference with no approved policy basis | No | It is an anti-pattern |

---

## Policy Input Definition Template

For each contextual field, document:

| Attribute | Example |
|---|---|
| Field name | `interactionType` |
| Business definition | How the customer or representative is interacting |
| Allowed values | `CUSTOMER_SELF_SERVICE`, `STAFF_ASSISTED`, `PARTNER_ASSISTED` |
| Authoritative source | Approved experience or identity context |
| Validation owner | Domain service |
| Policy effect | Selects documented control or disclosure policy |
| Audit requirement | Value and policy version recorded with decision |
| Security consideration | Consumer cannot assert privileged context without authorization |
| Compatibility rule | Additions assessed for exhaustive consumers |

---

## Security Considerations

A service must not blindly trust a caller-supplied channel or context value. It should determine whether:

- The calling workload is authorized to assert that context.
- The value is consistent with authenticated identity claims.
- A gateway or BFF can alter it.
- It changes authorization, entitlements, or financial outcomes.
- It must be signed, derived, or independently verified.
- It must be recorded in the audit trail.

For example, a public client must not be able to claim `STAFF_ASSISTED` merely by modifying JSON.

---

## Review Questions

1. What exact business or control requirement needs this field?
2. Would the requirement still exist if the frontend technology changed?
3. Is the field's meaning defined independently of a product or device?
4. Who owns the policy activated by the value?
5. Can the caller be trusted to assert it?
6. Does the provider validate it against identity or system context?
7. Is the policy outcome auditable?
8. Could a more precise business-context field replace a generic channel flag?
9. Is the difference actually presentation shaping that belongs in a BFF?
10. Have compatibility implications for new values been assessed?

## Executive Summary

> A core capability may accept contextual information when that information has explicit, governed business meaning. Channel identity must not become an opaque switch for hidden or duplicated business behaviour.
