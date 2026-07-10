# Shopify Risk Assessment Integration

## Press Release

### Heading

HotWax now uses Shopify risk signals to hold, review, and release suspicious orders before fulfillment.

**Subheading:** Shopify risk assessments flow through the Shopify bridge into OMS approval logic, giving operations teams a controlled Fraud queue instead of letting risky orders reach the 3PL.

## Summary

HotWax now has the integration foundation for Shopify risk and fraud review. The Shopify OMS bridge captures Shopify's order risk summary and assessment details during order sync, forwards that raw risk payload to OMS, and detects later risk changes so OMS can re-evaluate orders as Shopify's recommendation changes.

OMS owns the risk model and the fulfillment decision. It maps Shopify recommendation and assessment values into OMS enums, stores the order-level risk summary, persists provider-level assessments and facts, and evaluates risk during order approval. Pending risk defers approval. Accepted or no-risk orders continue. Investigate and cancel recommendations either create a customer-service review task or auto-cancel, depending on product-store configuration.

Order Manager is the operator surface for that workflow. Fraud task cards let users open the full order from the Fraud queue, review risk context on the order detail page, and continue working even when product cache data is still loading.

## Problem

Shopify can capture orders faster than operations teams can review them, especially during promotions, influencer drops, GWP campaigns, or reseller abuse. If suspicious orders are released to the 3PL too early, the business can ship fraudulent purchases, high-risk reseller orders, bad-address orders, or orders that should have been held for manual review.

Retailers need clean orders to keep moving while suspicious orders are quarantined before warehouse release. They also need fraud and payment rules to be configurable, and they need third-party fraud decisions to become fulfillment controls instead of disconnected notes in another system.

The old pattern for many retailers is manual review across Shopify, fraud tooling, spreadsheets, email, and warehouse exceptions. That creates two bad outcomes: risky orders slip through, and healthy orders get slowed down by the review process.

## Solution

HotWax treats Shopify risk review as an OMS workflow, not a separate back-office checklist.

First, the Shopify bridge captures risk data from Shopify order sync. The order GraphQL query includes Shopify's `risk` object, including recommendation, assessment risk level, provider id/title, fact description, and fact sentiment. On create, the bridge forwards the raw Shopify risk payload to OMS after the OMS order exists. On update, the bridge compares a `riskHash` in `ShopifyOrderHistory` so changes such as `PENDING` to `HIGH`, or new provider assessments, are sent to OMS even when the order already exists.

Second, OMS persists and acts on the risk data. `store#OrderHeaderRisk` maps Shopify risk values into OMS reference data, stores `OrderHeader.riskRecommendationEnumId` and `OrderHeader.riskLevelEnumId`, persists one risk assessment per provider, and stores the provider facts with sentiment. OMS computes the authoritative risk rollup; the bridge does not own that business logic.

Third, order approval uses the risk state. `evaluate#OrderRiskOnApproval` gates `approve#Order`:

- `PENDING` risk defers approval so the order does not release while Shopify is still evaluating it.
- Empty or null recommendation is optimistic and does not block approval.
- `ACCEPT` and `NONE` continue without a review task.
- `INVESTIGATE` creates a risk review task.
- `CANCEL` creates a review task unless product-store setting `AUTO_ACPT_RISK_REC=Y` is enabled, in which case OMS auto-cancels with a risk recommendation reason.

Order Manager then gives the operations team the queue and order-detail surfaces needed to review the task. The Fraud queue can filter by risk recommendation and risk level, the order detail page can load order risk assessments, and task cards provide direct View Order navigation from Fraud queue cards.

## Getting started

Shopify risk assessment handling is built into the Shopify bridge and OMS risk services. Merchants should confirm Shopify order import is enabled, load OMS risk seed data, configure product-store behavior for Shopify cancel recommendations with `AUTO_ACPT_RISK_REC`, and validate the flow with orders that represent accepted, pending, investigate, and cancel recommendations.

Once enabled, operations users can review risky orders in Order Manager's Fraud queue and open the full order for context. Healthy orders continue to brokering and fulfillment while suspicious orders are quarantined.

## Testimony

**Internal quote:** Shopify can tell us what looks risky, but OMS controls whether an order is allowed to reach the warehouse. This integration connects those two decisions.

**Customer quote:** We need suspicious Shopify orders held before they reach the 3PL, but we cannot let one risky order slow down the rest of the batch.

## Call to action

Use the Shopify bridge and OMS risk services as the system of record for Shopify risk review, then use Order Manager's Fraud queue as the daily operations surface for review, release, and cancellation.

## FAQs

**Question 1: Is this just an Order Manager UI change?**

Answer: No. The Order Manager UI changes are only the review surface. The core integration is in the Shopify bridge and OMS. The Shopify bridge captures Shopify risk data, detects risk changes, and forwards the risk payload to OMS. OMS persists the risk model and evaluates order approval. Order Manager exposes the Fraud queue and order-detail context that operators use after OMS creates a review task.

**Question 2: Which Shopify risk data is captured?**

Answer: The bridge captures Shopify's risk recommendation, each assessment's risk level, the assessment provider id/title, and the assessment facts with description and sentiment. This is important because fraud apps that report into Shopify can appear as Shopify risk providers. A third-party fraud platform does not need a custom OMS API on day one if its decision is already represented in Shopify's risk assessment payload.

**Question 3: How does OMS store risk?**

Answer: OMS stores the order-level risk summary on `OrderHeader` with `riskRecommendationEnumId` and `riskLevelEnumId`. Provider-level detail is stored in `OrderHeaderRiskAssessment`, keyed by order and provider. Facts are stored in `OrderHeaderRiskAssessmentFact` with a sentiment value. Because Shopify risk assessments do not expose a stable assessment id, OMS dedupes by provider id. When no provider exists, OMS uses the Shopify sentinel provider.

**Question 4: What happens when Shopify risk is still pending?**

Answer: Pending risk is a hard approval gate. OMS returns a defer action and does not approve the order for fulfillment while the risk level is `PENDING`. When Shopify later updates the risk result, the bridge detects the changed risk hash, forwards the new risk payload, and re-invokes order approval so a previously deferred order can move forward or become a review task.

**Question 5: What happens for Shopify ACCEPT or NONE recommendations?**

Answer: OMS approves silently. These recommendations do not create a review task.

**Question 6: What happens for Shopify INVESTIGATE recommendations?**

Answer: OMS approves the order into an operations review path by creating a `REVIEW_RISK_ORDER` task. Order Manager can show the task in the Fraud queue so a user can review the order before operational release.

**Question 7: What happens for Shopify CANCEL recommendations?**

Answer: It depends on product-store configuration. If `AUTO_ACPT_RISK_REC=Y`, OMS auto-cancels the order with the reason `Auto-cancelled: Shopify risk recommendation = CANCEL`. If that setting is not enabled, OMS creates a risk review task so a user can decide whether to cancel, release, or escalate.

**Question 8: Is HotWax replacing Shopify's fraud analysis?**

Answer: No. Shopify and dedicated fraud platforms can remain the source of risk signals. HotWax acts as the fulfillment-control layer that decides whether a risky order is allowed to proceed to the 3PL.

**Question 9: How does this support Forter or another fraud platform?**

Answer: If the fraud platform writes its decision into Shopify risk assessments, HotWax can ingest it through the Shopify risk payload using provider id/title, recommendation, risk level, and facts. A direct Forter, Riskified, or Signifyd integration can still be added later if a retailer needs fields or decisions that are not available through Shopify's risk payload.

**Question 10: Why does this belong in the OMS?**

Answer: The OMS controls warehouse release. A fraud tool can identify risk, but the OMS is where the operational decision matters: release, hold, cancel, assign, or escalate before fulfillment.

**Question 11: What does the Fraud queue show?**

Answer: The Fraud queue should show the reason the order is risky, the recommendation, the risk level, customer/order context needed for review, and a direct path to the full order detail page.

**Question 12: How do we avoid blocking healthy orders?**

Answer: Risk evaluation is order-scoped. Suspicious orders are deferred or routed to review, while clean orders continue through brokering and fulfillment.

**Question 13: What is configurable?**

Answer: The shipped product-store setting controls whether OMS automatically accepts Shopify's `CANCEL` recommendation and cancels the order. More configuration can build from this foundation: channel-specific behavior, customer segment rules, SKU/quantity thresholds, GWP abuse rules, address mismatch rules, high-value order review, and provider-specific decision handling.

**Question 14: What is the demo slice?**

Answer: Import a Shopify batch with four examples: a clean order with `ACCEPT` or `NONE` risk that releases normally, a `PENDING` order that defers approval until Shopify resolves risk, an `INVESTIGATE` order that creates a Fraud review task, and a `CANCEL` order that either auto-cancels or creates a review task depending on `AUTO_ACPT_RISK_REC`.

## Internal FAQs

**Question 1: What still needs rollout documentation?**

Answer: The integration foundation is implemented across the bridge, OMS, and Order Manager review surface. Remaining work is product hardening and rollout documentation: confirm production Shopify API coverage for each retailer's risk provider, define retailer-specific configuration for auto-cancel versus manual review, expand user-facing actions in Order Manager if users need release/cancel/assign/note/escalation workflows from the Fraud queue, and document the operating procedure in the OMS user manual.

**Question 2: Which implementation sources should be reviewed with this FAQ?**

Answer: Review the Shopify bridge risk ingestion work, OMS risk persistence and approval services, the OMS risk task API, and the Order Manager Fraud queue and order detail risk assessment surfaces.
