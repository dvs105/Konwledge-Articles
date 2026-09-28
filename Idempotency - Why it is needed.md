# 6.5 Idempotency

## Overview

Based on the surrounding content of the **Design Contract Example** and **Principle 2: Design Contracts Before Consumers**, idempotency is not a business rule. It is a contract behaviour that makes API operations safe and predictable when requests are repeated.

## What Is Idempotency?

An operation is **idempotent** when the same request can be submitted multiple times and still produce the same result, without creating additional unintended side effects.

In simple terms:

```text
1 request             -> 1 outcome
2 identical requests  -> the same outcome
10 identical requests -> the same outcome
```

The authoritative business operation occurs once, even when the consumer has to submit the request more than once.

## Why Idempotency Exists

In a distributed system, a consumer may not know whether a request succeeded. For example:

```text
Consumer sends request
        |
        v
Server completes processing
        |
        X
Response is lost in the network
```

The consumer cannot immediately determine whether:

- The request failed before processing.
- The request succeeded but the response was lost.
- The request is still being processed.
- A network or dependency timeout occurred.

Without idempotency, retrying the request may create a duplicate business outcome.

For example, repeatedly submitting:

```text
Create loan application
```

could incorrectly create:

```text
Loan application 123
Loan application 124
Loan application 125
```

even though the customer initiated only one logical request.

Idempotency prevents this duplication.

## How the Contract Implements Idempotency

The example contract requires an HTTP request header such as:

```http
Idempotency-Key: 471a16a2-fb98-4cba-9206-c3b72e1018e2
```

The idempotency key uniquely identifies one **logical business request**.

```text
Consumer
   |
   | POST request
   | Idempotency-Key: ABC123
   v
Eligibility API
```

After completing the request, the provider maintains an association between the key and the authoritative outcome:

```text
ABC123 -> Decision 789
```

The consumer must reuse that same key when retrying the same logical request.

## First Request

The consumer submits an eligibility assessment:

```http
POST /v1/eligibility-assessments
Idempotency-Key: ABC123
Content-Type: application/json
```

The provider processes the request and returns:

```json
{
  "decisionId": "789",
  "outcome": "ELIGIBLE"
}
```

The provider retains the relationship:

```text
ABC123 -> Decision 789
```

## Retry Scenario

Suppose the service completes processing but the response is lost:

```text
Consumer
    |
    v
Request is processed successfully
    |
    X
Response is lost
```

The consumer retries using the same idempotency key:

```http
POST /v1/eligibility-assessments
Idempotency-Key: ABC123
Content-Type: application/json
```

The provider recognises the key and returns the original outcome:

```json
{
  "decisionId": "789",
  "outcome": "ELIGIBLE"
}
```

It does not create a new decision such as `Decision 790`. The logical business operation has occurred only once.

## Why the Same Payload Matters

The contract states that repeating a request with the same idempotency key and the same material payload returns the original decision.

### Original Request

```http
Idempotency-Key: ABC123
```

```json
{
  "requestedAmount": {
    "amount": "25000.00",
    "currency": "AUD"
  }
}
```

### Replayed Request

```http
Idempotency-Key: ABC123
```

```json
{
  "requestedAmount": {
    "amount": "25000.00",
    "currency": "AUD"
  }
}
```

The result is:

```text
The original decision is returned.
No duplicate business processing is performed.
```

## What Happens If the Payload Changes?

An idempotency key must not be reused for a different logical request.

### First Request

```http
Idempotency-Key: ABC123
```

```json
{
  "requestedAmount": {
    "amount": "25000.00",
    "currency": "AUD"
  }
}
```

### Second Request with Different Content

```http
Idempotency-Key: ABC123
```

```json
{
  "requestedAmount": {
    "amount": "50000.00",
    "currency": "AUD"
  }
}
```

The same key now refers to two materially different requests. The provider cannot safely treat them as the same operation.

The contract therefore returns:

```http
409 Conflict
```

```json
{
  "code": "IDEMPOTENCY_KEY_REUSED"
}
```

This avoids ambiguity and prevents the key from being used to overwrite, corrupt, or accidentally duplicate an existing operation.

## Consumer Responsibilities

The consumer should:

1. Generate one unique idempotency key for each logical business request.
2. Retain that key until the operation has reached a known outcome.
3. Reuse the original key when retrying the same request.
4. Submit the same material payload with that key.
5. Avoid generating a new key merely because a timeout or connection failure occurred.
6. Treat `409 IDEMPOTENCY_KEY_REUSED` as a consumer-side request-management problem.
7. Follow the contract's limits for key format, scope, and retention.

## Provider Responsibilities

The provider should:

1. Validate the idempotency key format.
2. Associate the key with the authenticated consumer and logical operation.
3. Record enough information to recognise a replay of the same material request.
4. Return the original authoritative outcome for a valid replay.
5. Reject reuse of the key with materially different content.
6. Prevent concurrent requests using the same key from creating duplicate outcomes.
7. Define how long idempotency records are retained.
8. Protect idempotency records according to the classification of the associated transaction.
9. Emit appropriate metrics, logs, and audit evidence without exposing sensitive payloads.

## Concurrent Duplicate Requests

Two identical requests may arrive almost simultaneously:

```text
Request A with key ABC123 -----> Provider
Request B with key ABC123 -----> Provider
```

The provider must coordinate processing so both requests do not independently create business outcomes.

A safe result is:

```text
Request A creates Decision 789.
Request B receives Decision 789 or an explicitly documented in-progress response.
```

The exact coordination mechanism is an implementation concern, but the externally visible behaviour must conform to the contract.

## Idempotency Is Not the Same as Deduplication

The concepts are related but not identical:

- **Idempotency** defines the externally visible guarantee that repeating a logical request does not create additional unintended effects.
- **Deduplication** is one possible internal mechanism for detecting and suppressing repeated processing.

Consumers depend on the idempotency guarantee, not on the provider's internal storage or deduplication technology.

## Idempotency Is Not the Same as Caching

Caching reuses a response primarily to improve performance. Idempotency protects the correctness of an operation when a request is repeated.

An idempotent response may be retrieved from a retained outcome, but the purpose is to prevent duplicate effects rather than simply to make the response faster.

## Relationship to HTTP Methods

Some HTTP operations are naturally intended to be idempotent:

- `GET` retrieves a representation and should not create a business side effect.
- `PUT` normally replaces a resource at a known identifier and should produce the same intended state when repeated.
- `DELETE` should leave the resource deleted when repeated, although later responses may differ.

`POST` is not inherently idempotent. However, an API contract can make a POST-based business operation safely repeatable by requiring an idempotency key and defining replay behaviour.

The loan eligibility example uses this approach because the consumer asks the provider to create an assessment and assign a decision identifier.

## Why Idempotency Is Important in Headless Architecture

A headless architecture allows multiple independently developed consumers to invoke the same business capability:

```text
Web application
Mobile application
Assisted-service portal
Partner channel
        |
        v
Loan Eligibility API
```

Each channel may experience timeouts, connection failures, user resubmission, or automated retry behaviour.

By defining idempotency in the contract, every consumer receives the same assurance:

```text
If the same assessment is retried with the same key,
a duplicate eligibility decision will not be created.
```

This supports:

- Independent consumer delivery
- Safe and bounded retries
- Network fault tolerance
- Consistent behaviour across channels
- Predictable recovery following ambiguous failures
- Reduced risk of duplicate business transactions

Consumers do not need to know how the provider internally implements this guarantee.

## Illustrative Business Examples

### Loan Applications

```text
Submit application
```

A retry must not create multiple loan applications.

### Payments

```text
Transfer AUD 10,000
```

A retry must not transfer the amount twice.

### Card Blocking

```text
Block card
```

Repeating the request should leave the card blocked without creating duplicate operational actions.

### Eligibility Assessments

```text
Assess loan eligibility
```

A retry using the original key should return the original eligibility decision rather than creating another decision.

## Relationship to Principle 2

Idempotency is an example of **contract-first design** because the interface explicitly defines:

- The key the consumer must provide
- The scope of one logical request
- How identical retries behave
- How concurrent duplicates behave
- Whether the original outcome is returned
- How materially different payloads are detected
- Which error is returned for invalid key reuse
- How long the guarantee applies

This allows consumer and provider teams to design, build, mock, and test their implementations against an agreed behaviour before the full backend implementation exists.

## Contract Review Questions

Use the following questions when reviewing idempotency requirements:

1. Which operations can cause a business side effect?
2. Which operations require an idempotency key?
3. What constitutes the same logical request?
4. Which request fields are considered material when comparing retries?
5. Is the key scoped by consumer, user, operation, tenant, or another boundary?
6. What key format is permitted?
7. How long is the key and outcome retained?
8. What happens when the first request is still processing?
9. What happens when the first request completed but its response was lost?
10. What happens when the same key is used with different content?
11. Are transient failures retried using the original key?
12. How are concurrent duplicate requests controlled?
13. What telemetry proves that duplicates were prevented?
14. Does the design avoid placing sensitive business data inside the key?

## Executive Summary

> **Idempotency ensures that a consumer can safely retry the same business request without causing duplicate processing. By requiring an idempotency key and defining replay behaviour in the contract, the API guarantees that one logical request produces one authoritative outcome, even when responses are lost, networks fail, or retries occur.**
