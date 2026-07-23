---
name: Manage subscription assignments
description: List, assign, and revoke user seats on an AppDirect subscription.
api: openapi/appdirect-companies-openapi-original.yml
operations:
- resource_Subscription_readSubscriptionAssignments_GET
- resource_Subscription_createSubscriptionAssignment_POST
- resource_Subscription_deleteSubscriptionAssignment_DELETE
---

# Manage subscription assignments

Assign and revoke seats on a subscription so the right users get access to a purchased product.

## Auth
OAuth 2.0 bearer with a grant that carries `ROLE_SYS_ADMIN` (company admin) or
`ROLE_PARTNER` (system). Base URL `https://{marketplaceURL}/api`.

## Steps
1. **List current assignments** — `resource_Subscription_readSubscriptionAssignments_GET`
   (`GET /account/v2/subscriptions/{subscriptionUuid}/assignments`).
2. **Assign a user** — `resource_Subscription_createSubscriptionAssignment_POST`
   (`POST /account/v2/subscriptions/{subscriptionUuid}/assignments/{userUuid}`).
3. **Revoke a user** — `resource_Subscription_deleteSubscriptionAssignment_DELETE`
   (`DELETE /account/v2/subscriptions/{subscriptionUuid}/assignments/{userUuid}`).

## Rules
- `subscriptionUuid` and `userUuid` come from the subscription and membership APIs.
- A `403` means the token's scope/role cannot manage that subscription.
- Re-list after each change to confirm the seat count; retry `429` with backoff.
