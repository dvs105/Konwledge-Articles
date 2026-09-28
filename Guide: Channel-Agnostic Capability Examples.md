# Channel-Agnostic Capability Examples

## Purpose

This companion guide provides practical examples for [Principle 3: Keep Core Capabilities Channel-Agnostic](Principle_3_Keep_Core_Capabilities_Channel_Agnostic.md).

Also see:

- [Business Intent and Domain-Oriented API Naming](Business_Intent_and_Domain_Oriented_API_Naming.md)
- [Channel Context and Explicit Policy Inputs](Channel_Context_and_Explicit_Policy_Inputs.md)
- [API Gateway vs Core Business Logic](API%20Gateway%20vs%20Core%20Business%20Logic.md)

All examples are illustrative.

---

## 1. Loan Eligibility

### Channel-coupled

```http
POST /web-loan-eligibility
POST /mobile-loan-eligibility
POST /contact-centre-loan-eligibility
```

Each endpoint contains a separately maintained rule set.

### Channel-agnostic

```http
POST /eligibility-assessments
```

```json
{
  "applicantReference": "APPLICANT-123",
  "requestedAmount": {
    "amount": "25000.00",
    "currency": "AUD"
  }
}
```

```json
{
  "decisionId": "DECISION-789",
  "outcome": "ELIGIBLE",
  "rulesetVersion": "eligibility-policy-1.0"
}
```

The web, mobile, and contact-centre experiences render the same authoritative result appropriately for their users.

---

## 2. Product Pricing

### Channel-coupled

```text
MobilePriceCalculator
WebPriceCalculator
PartnerPriceCalculator
```

### Channel-agnostic

```http
POST /price-calculations
```

```json
{
  "productId": "PRODUCT-42",
  "customerSegment": "STANDARD",
  "effectiveAt": "2026-09-28T03:00:00Z"
}
```

```json
{
  "price": {
    "amount": "39.95",
    "currency": "AUD"
  },
  "pricingPolicyVersion": "pricing-4.2"
}
```

If a legitimate partner discount exists, the input should identify the approved pricing agreement or entitlement, not merely say `channel: PARTNER`.

---

## 3. Payment Initiation

### Channel-coupled

```http
POST /mobile-checkout
POST /web-checkout
```

### Channel-agnostic

```http
POST /payments
```

```json
{
  "paymentReference": "PAYMENT-123",
  "amount": {
    "amount": "125.00",
    "currency": "AUD"
  },
  "payerAccountId": "ACCOUNT-1",
  "payeeId": "PAYEE-9"
}
```

The channel owns the payment form and confirmation presentation. The payment capability owns validation, authorization, state transition, idempotency, and the authoritative outcome.

---

## 4. Customer Profile

### Channel-coupled response

```json
{
  "mobileTitle": "Your details",
  "html": "<div>...</div>",
  "nextScreen": "contact-preferences"
}
```

### Channel-agnostic response

```json
{
  "customerId": "CUSTOMER-123",
  "contactDetails": {
    "email": "masked@example.invalid",
    "mobile": "+61********1"
  },
  "contactPreference": "DIGITAL"
}
```

A BFF may transform this into a channel-specific view model without changing the domain semantics.

---

## 5. Product Catalogue

### Channel-coupled

A service returns “homepage cards” or “mobile tiles” as the authoritative product representation.

### Channel-agnostic

```http
GET /products?status=AVAILABLE&customerSegment=STANDARD
```

The product capability returns structured product facts and eligibility metadata. The experience decides whether to render cards, lists, voice responses, or conversational recommendations.

---

## 6. Identity Verification

### Poor design

```text
if channel == mobile, use strong verification
if channel == web, use weaker verification
```

### Better design

```json
{
  "verificationPurpose": "ACCOUNT_RECOVERY",
  "interactionType": "CUSTOMER_SELF_SERVICE",
  "riskContext": "REMOTE",
  "requiredAssuranceLevel": "HIGH"
}
```

The service evaluates an explicit assurance requirement. The input is business and security context, not merely a device label.

---

## 7. Regulatory Disclosure

A disclosure can have both domain and experience responsibilities.

### Domain or policy service

- Determines which disclosure is required.
- Returns the disclosure identifier, version, effective date, and acknowledgement requirement.

### Experience

- Presents it accessibly.
- Captures the required acknowledgement.
- Preserves required semantic content.

Example response:

```json
{
  "disclosureId": "DISCLOSURE-17",
  "version": "3.0",
  "acknowledgementRequired": true,
  "presentationDeadline": "BEFORE_SUBMISSION"
}
```

The core service should not return channel-specific HTML unless the governed content contract explicitly requires formatted content.

---

## 8. Fraud and Risk Context

A channel may be correlated with risk, but the policy should use meaningful evidence rather than an unexplained label.

### Weak

```json
{
  "channel": "MOBILE"
}
```

### Stronger

```json
{
  "sessionRisk": "ELEVATED",
  "deviceBindingStatus": "VERIFIED",
  "authenticationAssurance": "HIGH",
  "interactionMode": "REMOTE_DIGITAL"
}
```

The provider must confirm which caller is authorised to assert these fields and how they are verified.

---

## Cross-Channel Consistency Test

For each example, execute equivalent business requests through approved consumer contracts and verify:

- The same authoritative ruleset is used.
- Equivalent context produces equivalent business outcomes.
- Presentation differences do not change business semantics.
- Channel-specific components do not duplicate authoritative rules.
- Security and authorization remain enforced in the domain boundary.
- Correlation and policy-version evidence make the decision traceable.

## Executive Summary

> Channel-agnostic design centralises authoritative business behaviour while allowing each experience to optimise presentation, interaction, and composition. Legitimate variation is represented through explicit policy context, not duplicated channel-specific services.
