# Product Substitution Logic - PR FAQ

## Press Release

### HotWax adds product substitution logic for rescuing sold-out orders before they become cancellations

HotWax Commerce today announced product substitution logic for HotWax OMS, giving retailers a structured way to replace sold-out products with approved alternatives during order routing and customer-service resolution.

Retailers often have products that can safely replace one another: the same item in updated packaging, a newer SKU replacing an old SKU, a near-identical shade or size, a private-label equivalent, or a customer-approved fallback. In most OMS workflows, those relationships are either trapped in spreadsheets, handled by customer-service judgment, or ignored entirely. When the original product is sold out, the order becomes unfillable even if the retailer has a valid substitute sitting in inventory.

HotWax now turns those product relationships into operational logic.

Merchants and operations teams can link substitute products using product associations. The routing engine can detect when a sales order item is out of stock but a configured substitute has inventory at a candidate facility. Order Manager then gives customer-service and operations users a guided resolution experience: review the unavailable original item, inspect the approved substitute, choose a custom replacement if needed, cancel only the impacted item, park the order, or release the updated order after the swap.

The result is a more flexible fulfillment workflow. Retailers can save orders that would otherwise be cancelled, protect revenue, reduce customer-service improvisation, and create an auditable trail of how the original order item was resolved.

### The Problem

Traditional order routing treats each order item too literally. If SKU A is sold out, the item is unfillable even when SKU B is a valid replacement.

That creates several operational problems:

- Orders are cancelled even though a customer-acceptable substitute is available.
- Customer-service teams rely on notes, tribal knowledge, or Slack messages to decide what can replace what.
- Warehouses see an order as unfillable even when another product at the same facility could satisfy the customer need.
- Product teams cannot govern substitutions centrally.
- Refunds, cancellations, replacement items, and customer promises are handled as separate manual steps.
- The OMS loses the audit trail connecting the original item to the replacement item.

This is especially painful for retailers with seasonal products, limited inventory, packaging changes, bundles, beauty shades, apparel variants, replenishment delays, and SKU migrations. The product catalog may know that two items are interchangeable, but the fulfillment process cannot act on that knowledge.

### The Solution

HotWax uses product associations as the substitution model.

In the Products app, users can add substitute products to a product's inventory policy. Those links are stored as active `PRODUCT_SUBSTITUTE` associations. The association can be expired or reactivated, so teams can govern which substitutes are currently valid instead of relying on permanent hard-coded rules.

The routing engine uses those associations during inventory sourcing. When the original product does not have enough available stock, routing checks active substitute associations and looks for substitute inventory at the same facility. If a substitute has available inventory and brokering is allowed, the facility can still be considered a viable option instead of immediately pushing the order into a dead-end unfillable path.

Order Manager turns the detected substitution opportunity into a task-based resolution workflow. Swap tasks appear in the Swap queue and in the order detail Holds section. Users can filter the queue by swappable orders, date range, channel, and search term. Each task shows the routing context, unavailable ordered items, suggested replacement items, original total, new total, suggested refund, customer contact information, and available actions.

When an approved substitute is available, Order Manager highlights it as an approved swap. If no replacement is available, the UI shows that the item has no replacement in stock and prepares the user to cancel the item or pick a custom substitute. If the configured substitute is not right for the customer, the user can open custom swap, choose from approved substitutes, or search inventory at the assigned facility for another in-stock product.

When the user releases the updated order, HotWax applies the resolution: substitute items are swapped, cancelled items are cancelled, the task is completed, and the updated order can continue moving through fulfillment.

### Why This Matters

Substitution turns unfillable orders into resolvable orders.

Without substitution logic, inventory shortage produces a binary outcome: the original item can be fulfilled or it cannot. With substitution logic, the OMS can ask a better question: is there an approved replacement that can fulfill the customer promise?

That matters because the best operational answer depends on the scenario:

- If the substitute is already approved and in stock, the order can be updated and released.
- If the substitute is available but needs customer-service judgment, a user can review and approve it.
- If the approved substitute is out of stock, the user can search facility inventory for another custom swap.
- If no acceptable replacement exists, only the impacted item can be cancelled.
- If the order needs warehouse intervention, it can be parked at a selected facility.
- If the entire order should be cancelled, the user can cancel it from the same task.

This gives retailers a practical middle ground between rigid routing and fully manual exception handling.

It also creates a foundation for future auto-swap policies. Retailers should be able to decide which substitute relationships are safe enough to apply automatically and which require manual approval. A same-SKU packaging replacement may be auto-approved, while a shade or style replacement may require customer-service review. HotWax's product association model gives the data foundation for both.

### Customer Experience

A customer orders a product that later sells out at every eligible facility. During routing, HotWax checks whether the product has active substitute associations. The original product is unavailable, but an approved substitute is available at one facility.

Instead of treating the order as a simple cancellation, HotWax routes the order into a substitution resolution workflow.

An operations user opens the Swap queue in Order Manager and the task card shows the order, customer contact information, assigned facility, routing path, routing justification, ordered items, and suggested items. The sold-out product is marked unavailable. The approved substitute is shown in the suggested items list with available stock and a new order total.

If the substitute is acceptable, the user clicks Release updated order. HotWax creates the replacement order item, cancels the original item, links the old and new items with an item-swap association, completes the task, and releases the order back into fulfillment.

If the substitute is not acceptable, the user opens the action menu. They can cancel the item, open custom swap, or view inventory. Custom swap shows configured substitute products first, then lets the user search for another in-stock product at the facility. If the user selects a custom substitute, the suggested-items list updates before release.

If the order should not be resolved immediately, the user can park it at a facility. If the whole order should be cancelled, they can cancel the order from the same task.

Customer-service users can also open the order detail page and resolve the same swap task from the Holds section, so they do not need to leave the customer conversation to find a separate queue.

### Launch Scope

The substitution workflow includes:

- Substitute product setup through product associations
- Products app support for viewing, adding, expiring, and reactivating substitute links
- Active `PRODUCT_SUBSTITUTE` relationships as the substitution data model
- Routing support for checking substitute inventory when the original product is unavailable
- Facility-level substitute inventory checks using product facility ATP and brokering eligibility
- Swap task queue in Order Manager
- Swappable filter for focusing on orders with substitution opportunities
- Queue filters for search, date range, and sales channel
- Order detail Holds section support for order-scoped swap tasks
- Swap task cards with customer contact, routing detail, ordered items, suggested items, original total, new total, and suggested refund
- Approved swap labeling for configured substitute products
- No-replacement state when the original item is unavailable and no in-stock substitute is available
- Custom swap modal with approved substitutes and facility inventory search
- Inventory view action for the impacted product
- Item cancellation when no replacement is selected
- Order cancellation from the swap task
- Parking an order at a selected facility
- Release updated order action
- Backend swap service that adds the replacement item, cancels the original item, and records an `ITEM_SWAP` relationship between old and new order items

Auto-swap policy should be treated as the next productization step: the data model and resolution workflow are in place, but retailers still need explicit configuration for which substitute relationships can be applied automatically versus which must remain manual-review tasks.

## FAQ

### What is product substitution?

Product substitution lets HotWax replace a sold-out order item with an approved alternative product.

The approved relationship is stored as a product association. When the original product is unavailable, HotWax can detect the substitute, check whether it has inventory, and present a guided swap workflow in Order Manager.

### How are substitutes configured?

Substitutes are configured as product associations.

In the Products app, users can open a product detail page, go to inventory policy, and add substitute products. Those links are saved as `PRODUCT_SUBSTITUTE` associations from the original product to the replacement product. Users can also expire or reactivate the association when a substitute should no longer be valid or becomes valid again.

### Why use ProductAssoc instead of a custom substitution table?

Product associations already model relationships between products. They support effective dates, association types, sequence, quantity, and product-to-product direction.

Using `PRODUCT_SUBSTITUTE` means substitution is part of the product data model instead of hidden routing configuration. Product teams can govern the relationship, routing can consume it, Order Manager can display it, and future integrations can import or export it.

### How does routing detect a swappable order?

During inventory sourcing, routing evaluates the original order item inventory at candidate facilities. If the original product is short, routing checks active `PRODUCT_SUBSTITUTE` associations for that product and looks for substitute inventory at the same facility.

If a substitute has available stock and brokering is allowed, routing can treat that facility as a possible resolution path instead of rejecting it solely because the original SKU is unavailable.

### Does routing immediately change the order item?

The first rollout supports detection and manual resolution. The routing layer detects that substitute inventory can make an otherwise unfillable order resolvable, and Order Manager presents the swap task for review.

Automatic substitution should be governed by an explicit policy layer. Some substitutes are safe enough to auto-apply, while others require customer-service approval.

### What is the difference between manual swap and auto-swap?

Manual swap means the OMS detects the opportunity and creates a task for a user to resolve. The user reviews the original item, suggested substitute, inventory, pricing, refund impact, and customer context before releasing the updated order.

Auto-swap means the OMS applies the replacement without a manual task when a retailer has configured that substitute relationship as safe for automatic approval. This should be limited to trusted scenarios such as packaging changes, SKU migrations, or equivalent replacement products.

### What does Order Manager show in the Swap queue?

The Swap queue shows tasks for orders that need substitution or negative-resolution review.

Users can search the queue, filter by date range, filter by sales channel, and toggle Swappable to focus on orders with replacement opportunities. Each card shows the order context, customer contact details, routing movement, routing justification, ordered items, suggested items, original total, new total, suggested refund, and action buttons.

### What does the ordered-items section show?

Ordered items show the products originally purchased by the customer. Items that are short are marked unavailable.

For unavailable items, the user can open an action menu to cancel the item, choose a custom swap, or view inventory.

### What does the suggested-items section show?

Suggested items show what the order will look like after resolution.

If an unavailable item has an in-stock approved substitute, the substitute appears as an approved swap. If no replacement is available, the item appears with a no-replacement state and the resolution defaults toward cancellation unless the user chooses a custom replacement.

The card also shows the original total, new total, and suggested refund so the user understands the financial impact before releasing the updated order.

### What is custom swap?

Custom swap lets the user override the suggested replacement.

The modal has two paths. The substitute-products view shows configured substitutes for the item and disables substitutes that do not have stock at the assigned facility. The product-search view lets the user search inventory at the assigned facility and choose another in-stock product when the configured substitute is not right for the situation.

### Can users inspect inventory before deciding?

Yes. The suggested-product action menu includes View inventory. That opens product inventory context for the impacted product so the user can make a better resolution decision before swapping or cancelling.

### What happens when the user releases the updated order?

Order Manager builds two lists: items to swap and items to cancel.

For swapped items, the backend swap service adds a new order item for the replacement product, cancels the original order item, and creates an `ITEM_SWAP` association connecting the original item to the replacement item. For cancelled items, Order Manager calls the item cancellation flow. After the updates succeed, Order Manager completes the swap task and refreshes the queue.

### Why record an ITEM_SWAP relationship?

The order needs an audit trail.

Cancelling the old line and adding a new line is not enough by itself. The `ITEM_SWAP` relationship explains that the new item is the replacement for the original item. That gives operations, customer service, reporting, and downstream integrations a way to understand why the order changed.

### What happens if there is no substitute in stock?

Order Manager shows the item as unavailable with no replacement in stock.

The user can cancel the item, search for a custom in-stock replacement, park the order for later handling, or cancel the order. The workflow keeps the decision explicit instead of silently losing the order.

### What happens if the customer must approve the replacement?

The task can remain open while customer service contacts the customer. The card exposes phone and email information, shows the suggested replacement, and lets the user park or defer the order instead of forcing an immediate cancellation.

The roadmap should add stronger customer approval tracking, including notes, communication history, and explicit approved-by-customer status before release.

### Can this resolve only one item on a multi-item order?

Yes. The swap task works at the item level inside a ship group. The user can swap one unavailable item, cancel another item, and leave available items unchanged.

That matters because the right answer is often not whole-order cancellation. A retailer may keep the rest of the order moving while resolving only the impacted line.

### Can the whole order be cancelled?

Yes. The swap task includes a Cancel order action. When the entire order should not continue, the user can cancel all items from the task rather than resolving each item separately.

### Can the order be parked?

Yes. The user can park the order and choose a facility. Parking is useful when the order needs warehouse review, facility-specific handling, or a temporary hold while the team waits for customer approval or more inventory.

### Where can a customer-service user resolve this from?

There are two entry points.

Operations users can work from the Swap queue. Customer-service users can open the order detail page and use the Holds section, where the same swap task card appears alongside other order tasks such as address, hold, and fraud review tasks.

This matters because customer service often starts from the customer order, while operations often starts from a work queue.

### How does this interact with the Products app?

The Products app is where substitution policy starts. Product teams can link substitutes on the product detail page and manage active or expired substitute relationships.

Order routing and Order Manager then consume that product data. The product team controls what is eligible, while operations controls how a specific order is resolved.

### How does this interact with routing?

Routing uses substitute inventory to avoid prematurely treating an order as impossible to fulfill.

The routing logic checks original product stock and configured substitute stock at candidate facilities. That lets the engine identify facilities where the original item cannot be fulfilled as-is, but the order may still be recoverable through substitution.

### How does this interact with order tasks?

Swap resolution is task-based. The task type is part of the same order-task model used for other order exceptions.

That means substitution does not become a hidden routing side effect. It becomes a visible work item with status, assignment potential, queue filters, order detail visibility, and a completion action.

### What are the main scenarios?

Scenario 1: approved substitute in stock. Routing detects substitute inventory, Order Manager suggests the approved swap, the user releases the updated order, and fulfillment continues.

Scenario 2: approved substitute exists but is out of stock. Order Manager shows no replacement in stock. The user can search for a custom replacement, cancel the item, park the order, or cancel the order.

Scenario 3: no configured substitute exists. The item remains unresolved, but the user can still open custom swap and search facility inventory if a one-off replacement is acceptable.

Scenario 4: customer approval is required. The user reviews the proposed replacement, contacts the customer, parks or leaves the task open, and releases the updated order only after approval.

Scenario 5: automatic substitution is allowed. A future policy layer can auto-apply trusted substitute relationships and leave manual tasks only for exceptions.

### What should come next?

The next phase should productize substitution policy and automation:

- Auto-approve flags for substitute relationships
- Product-store or channel-specific substitution rules
- Customer approval tracking
- Reason codes for manual and automatic swaps
- Refund handling in the swap service
- Better ranking when multiple substitutes are configured
- Substitute inventory reservation before user release
- Audit history in order detail showing old item, new item, user, time, reason, and customer approval
- Integration events so Shopify, ERP, WMS, and customer-service tools understand the replacement
- Reporting on saved orders, cancelled items, substitute usage, refund impact, and unavailable products

The long-term goal is to let retailers define which products are interchangeable, let routing detect substitution opportunities automatically, and give Order Manager the right level of human control for each scenario.
