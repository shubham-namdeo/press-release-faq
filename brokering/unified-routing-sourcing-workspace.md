# Unified Routing and Sourcing Workspace - PR FAQ

## Press Release

### HotWax brings ATP, routing, facility groups, and product inventory into one sourcing workspace

HotWax Commerce today announced a unified Routing and Sourcing workspace for HotWax OMS, bringing Available to Promise rules, order routing, facility group management, inventory channels, and product inventory management into one operational app.

Retailers use HotWax to decide where an order should be fulfilled. That decision is not made by routing rules alone. It depends on which facilities are allowed to broker, how much safety stock each facility should protect, whether store pickup or shipping is enabled, which inventory channel publishes stock, how facility groups are maintained, and whether a facility is already at its order limit.

The new workspace makes those controls live together. Operations teams can configure sourcing rules, manage routing logic, inspect facility groups, review product inventory, and adjust product-facility settings without jumping between the old Available to Promise app, the Routing app, and legacy HotWax OMS inventory screens.

This is the next step in making ship-from-store easier to operate. Retailers can see and tune the full sourcing model from one place: the rules that make inventory eligible, the groups that define facility networks, the inventory data that explains availability, and the brokering logic that chooses where orders go.

### The Problem

Ship-from-store creates a distributed fulfillment network, but most OMS tools still split the controls across separate administrative surfaces.

One app manages Available to Promise behavior. Another app manages routing rules. Legacy OMS screens show product inventory and product-facility configuration. Facility groups may be created in one place, used in another, and interpreted differently by inventory channels, pickup groups, shipping groups, and brokering groups.

That fragmentation creates real operational risk:

- A routing manager can change a brokering rule without seeing the ATP controls that determine whether a facility is eligible.
- An inventory manager can update allow-brokering, safety stock, or facility limits without understanding which routing rule will consume that configuration.
- Facility groups can be created or associated in one context but missing or hard to use in another.
- Product inventory review still depends on older HotWax OMS pages that are disconnected from the modern routing workflow.
- Support teams need to switch apps to answer one basic question: why was this product available, why was this facility eligible, and why did routing choose this outcome?

For retailers expanding ship-from-store, that fragmentation slows down rollout. Teams need to configure inventory sourcing and routing together because the customer promise depends on both.

### The Solution

HotWax is consolidating sourcing into the Routing app.

The merged workspace keeps the Routing app as the place where orders are brokered, but adds the ATP surfaces that define inventory eligibility. Users can manage threshold rules, safety stock rules, store pickup rules, shipping rules, inventory channels, and order routing from the same side-menu app shell.

The app now treats facility groups as shared sourcing infrastructure. Facility groups are not just routing metadata. They are used by inventory channels, pickup rules, shipping rules, brokering filters, and operational capacity planning. The new facility-group management page gives users a place to create groups, edit group metadata, review group type, see facility counts, preview member facilities, manage facility membership, and archive groups that should no longer be used.

The app also brings inventory-facing controls closer to routing. Facility order limits can be reviewed and edited where sourcing teams already manage ATP behavior. Brokering rules can include or exclude facility groups, evaluate safety stock, check shipment thresholds, apply week-of-supply filters, and optionally turn off the facility order limit check for a specific inventory rule.

The next inventory-management branch extends this consolidation further by adding a modern Inventory page and Inventory Detail page to the same Sourcing section. Users can search products, filter by facility, review ATP and QOH, see minimum stock, allow-pickup, and allow-brokering values, edit product-facility configuration, adjust inventory through variance logging, and review inventory logs from the product detail page.

### Why This Matters

Routing is not a standalone configuration problem. Routing is the final decision that comes after inventory sourcing rules have already decided what is possible.

If a facility is not allowed to broker, routing should not send the order there. If safety stock protects store inventory, routing needs to respect that buffer. If a store has reached its order limit, routing needs to understand that capacity constraint. If a product is only pickup-enabled in certain facility groups, that sourcing rule determines the fulfillment promise before routing gets involved.

By merging ATP and Routing, HotWax gives retailers one operating model:

- Sourcing defines which inventory can be promised.
- Facility groups define which locations participate in each network.
- Inventory channels define how availability is published.
- Product inventory explains the stock and product-facility configuration behind the promise.
- Routing decides which eligible facility should receive the order.

That makes ship-from-store easier to scale. Retailers can add stores, create fulfillment groups, tune inventory buffers, publish availability, and route orders from one workspace instead of managing related behavior across separate apps and legacy screens.

### Customer Experience

A retailer is preparing to expand ship-from-store from 50 stores to 180 stores.

The operations team opens the Routing and Sourcing workspace. They start in Facility Groups and create a new group for the stores that should participate in the rollout. They review the member count, remove stores that are not ready, and archive an old test group that should no longer appear in selectors.

Next, they open Inventory Channels and associate the right facilities with the channel that publishes online inventory. They confirm which configuration facility powers the channel and review the publishing schedule for the connected Shopify shop.

Then they move through the sourcing rules. They update threshold rules so low-stock stores do not expose inventory too aggressively. They review safety stock by facility group. They confirm which stores allow pickup and which groups support shipping. They adjust facility order limits so a newly enabled store does not receive more work than the team can handle.

Before changing routing, the team opens product inventory for a few launch SKUs. They filter by facility, review ATP and QOH, check allow-brokering and allow-pickup settings, and correct a product-facility configuration that would have blocked one store from brokering.

Finally, they open Brokering and update the routing rule to include the new fulfillment group. Because the ATP rules, inventory channel, facility groups, product inventory, and routing rules are now in one workspace, the team can explain the whole sourcing path before orders start flowing.

### Launch Scope

The merged Routing and Sourcing workspace includes:

- Available to Promise pages merged into the Order Routing app.
- Sourcing menu section for Threshold, Safety stock, Store pickup, Shipping, and Inventory channels.
- Routing menu section for Brokering groups, routes, rules, routing tests, and inventory-rule configuration.
- Foundations menu section for Facility groups.
- Top-level routes replacing the old `/tabs/*` routing shell, with redirects for legacy tabs URLs.
- Shared side-menu app shell with sourcing, routing, foundations, settings, OMS instance, timezone, and app version controls.
- ATP product-store state moved into `atpProductStore` so it can coexist with Routing's product-store state.
- Sourcing rule creation and update flows for threshold, safety stock, store pickup, and shipping.
- Facility-group selectors in sourcing rules for included and excluded groups.
- Inventory channel management by product store and facility group.
- Inventory publishing schedule controls for connected Shopify inventory jobs.
- Facility order limit visibility and update controls in the ATP facility list.
- Pickup group toggles for store-pickup facility eligibility.
- Brokering inventory rules that can filter by facility group, exclude facility groups, evaluate brokering safety stock, apply shipment threshold checks, apply week-of-supply filters, sort candidate facilities, and control facility order limit behavior.
- Facility-group management page with create, edit, archive, type display, facility count, member preview, and facility membership management.
- System-wide facility-group lookup for the Facility groups page, so users can see more than product-store-scoped groups.
- Product-store association when a new facility group is created from the sourcing context.
- Locale and shared component consolidation between the ATP and Routing apps.
- Pinia 3 and Vue type migration work needed to support the combined app.

The open inventory-management branch extends the workspace with:

- Inventory list page in the Sourcing menu.
- Product search and facility filter for product inventory.
- Product rows showing ATP, QOH, minimum stock, allow-pickup, and allow-brokering.
- Product inventory detail page with product image, product identifiers, selected facility, configuration summary, inventory summary, and inventory logs.
- Product-facility config editing for allow-brokering, allow-pickup, safety stock, and days-to-ship.
- Bulk product-facility config editing for selected products at a selected facility.
- Inventory variance adjustment for selected products.
- Variance reason selection from inventory reason enums.
- Inventory log review by product and facility.
- Indexed product sync and local cache support for larger inventory lists.

### Not In Launch Scope Yet

- A final decision on when the standalone Available to Promise app is retired for every customer.
- Full retirement of every legacy `hotwax-oms` product inventory link and screen.
- Full product-inventory migration in the merged main branch until the inventory-management PR is completed and merged.
- Guided cross-page diagnostics that automatically explain why a specific order did or did not route.
- Automatic simulation of sourcing changes before publishing. That belongs to the separate Routing Simulation work.
- Role-specific onboarding, permissions cleanup, and audit views for every new inventory and facility-group action.
- Full replacement of all bulk upload workflows for product-facility configuration.

## Testimony

**Internal quote:** "Sourcing is the part of routing that happens before a routing rule ever runs. Bringing ATP rules, facility groups, product inventory, and brokering into one app gives us the operating model retailers actually need for ship-from-store."

**Customer quote:** "When we expand ship-from-store, we do not want to ask three teams to check three different screens. We need to know which stores are eligible, what inventory is protected, what each store can handle, and which routing rule will use that network."

## FAQ

### What is being launched?

HotWax is launching a unified Routing and Sourcing workspace.

The first merged release brings Available to Promise sourcing pages into the Order Routing app. That includes threshold rules, safety stock rules, store pickup rules, shipping rules, inventory channels, and facility group management next to brokering and routing rule configuration.

The inventory-management branch adds the next part of the same product direction: product inventory list and detail pages inside the merged sourcing workspace.

### Why merge Available to Promise and Routing?

Because ATP and Routing are two sides of the same sourcing decision.

ATP rules decide whether inventory should be made available for a promise. Routing decides which eligible facility should fulfill an order. If those controls live in separate apps, teams can easily tune one side without understanding the other.

Merging them gives users one workspace for the whole path from inventory eligibility to fulfillment assignment.

### Is this just a navigation merge?

No. The side-menu consolidation is visible, but the important change is operational.

The merged app resolves store and component conflicts, keeps ATP and Routing product-store state separate where needed, adds sourcing pages to the Routing app shell, carries ATP rule creation and update flows into the Routing app, exposes facility groups as shared infrastructure, and makes inventory rules in routing work with facility groups, safety stock, order limits, and other sourcing conditions.

### What does "sourcing" mean in this context?

Sourcing is the set of rules and data that determine where inventory can be promised from.

In HotWax, that includes product-store context, product-facility configuration, allow-brokering, allow-pickup, safety stock, thresholds, shipping eligibility, store-pickup eligibility, inventory channels, facility groups, facility order limits, and the routing rules that broker orders to facilities.

### Why does ATP belong inside routing management?

Routing cannot make a useful decision until ATP has defined the eligible inventory pool.

For example, a routing rule may include a store group, but the store still should not receive an order if allow-brokering is off, safety stock consumes the available units, pickup-only configuration does not match a shipping order, or the facility has reached its order limit.

Putting ATP in the same app makes those constraints visible before the user changes routing behavior.

### What ATP capabilities are now part of the Routing app?

The merged app includes threshold, safety stock, store pickup, shipping, and inventory channel pages.

Users can create and update sourcing rules, select included and excluded facility groups, manage pickup eligibility, configure shipping/pickup behavior, review inventory channels, and schedule inventory publishing jobs from the same app where brokering rules are maintained.

### How do facility order limits fit into routing?

Facility order limits are capacity constraints.

A store may technically have ATP for a product, but that does not mean it should receive unlimited fulfillment work. Facility order limits help protect store labor by limiting how many orders a facility can take in a period.

Routing inventory rules can evaluate facility order limits, and the merged app also lets users update a facility's fulfillment order limit from the sourcing interface.

### What is the "turn off facility order limit check" option?

It is an inventory-rule control in Brokering that lets a routing configuration bypass the facility order limit check for that rule.

That should be used deliberately. Most routing policies should respect facility capacity, but there are scenarios where a retailer may want a specific rule to ignore the cap, such as an exception lane, a controlled recovery workflow, or a rule where other filters already restrict the candidate facilities.

### Why add facility group management to Routing?

Facility groups are one of the main building blocks for sourcing and routing.

They define store groups, warehouse groups, pickup groups, shipping groups, inventory channels, and brokering groups. If users can select facility groups throughout the Routing app but cannot manage them there, they are forced back into another admin surface to fix the source data.

The Facility groups page makes group creation, editing, membership review, member management, and archiving available where the groups are used.

### What changed in facility group lookup?

The merged app adds a system-wide facility group page instead of relying only on product-store-scoped group lists.

That matters because many useful groups may exist outside the current product-store association. Users need to see the real group universe, understand the type, inspect members, and then associate or use groups in the correct sourcing context.

### How do inventory channels fit into the story?

Inventory channels define which facility group and configuration facility are used to publish availability to a sales channel or shop.

In the merged app, Inventory channels sit in the Sourcing section because they are part of the promise path. A channel can show its configuration facility, member facilities, store/warehouse counts, and publishing jobs for connected Shopify shops.

### Why bring product inventory out of legacy HotWax OMS?

Product inventory is one of the most common troubleshooting surfaces for sourcing.

When an order does not route, users need to inspect the product's ATP, QOH, allow-brokering setting, allow-pickup setting, minimum stock, facility configuration, and inventory history. Legacy `hotwax-oms` inventory pages expose some of this data, but they sit outside the modern routing workflow.

Moving product inventory into the merged app lets users inspect and correct inventory context while they are already working on sourcing and routing.

### What does the new Inventory page show?

The inventory-management branch adds an Inventory page with product search, facility selection, pagination, product images, product identifiers, and per-product inventory/configuration values.

Rows can show ATP, QOH, minimum stock, allow-pickup, and allow-brokering for the selected facility. Users can select products for bulk actions or open a product detail page.

### What does Inventory Detail show?

Inventory Detail shows product context and facility-specific sourcing context together.

The page includes product image, product ID, product name, selected facility, allow-brokering, allow-pickup, safety stock, days-to-ship, QOH, ATP, and inventory logs. Users can edit product-facility configuration or open inventory adjustment from the detail page.

### Can users update inventory from the new app?

The inventory-management branch includes an inventory edit modal for variance logging.

Users select a variance reason, choose add or remove, enter a variance quantity, and submit a variance record for the selected products at the selected facility. The branch posts to the inventory cycle count variance service and records comments indicating the adjustment came from the Order Routing app inventory screen.

### Can users update allow-brokering and safety stock?

Yes, the inventory-management branch adds product-facility configuration editing.

Users can update allow-brokering, allow-pickup, safety stock, and days-to-ship for selected products at a selected facility. This is important because those fields directly affect whether a product can participate in sourcing and routing.

### Does this replace the Products app?

No. Products and Sourcing have different responsibilities.

The Products app is the operational home for product master data, identifiers, product relationships, dimensions, mappings, substitutions, and setup governance.

The Routing and Sourcing workspace is the operational home for inventory availability, product-facility sourcing settings, facility groups, inventory channels, ATP rules, and brokering behavior.

The two apps should connect, but they should not become the same app.

### Does this replace the standalone ATP app?

The product direction is for the merged Routing and Sourcing workspace to become the primary place users manage ATP-related sourcing rules.

The exact retirement timing for the standalone ATP app should be treated as rollout planning. Customers may need a transition period, permission mapping, documentation updates, and validation that every required ATP workflow is covered in the merged app.

### Does this replace legacy product inventory screens?

It should replace the day-to-day operational need for the legacy product inventory screens once the inventory-management branch is complete and deployed.

Legacy links may still need cleanup across `hotwax-oms`, and some advanced or backend-only inventory workflows may remain in older screens until they are migrated or intentionally retired.

### How does this help implementation teams?

Implementation teams can configure a retailer's sourcing network from one place.

They can set facility groups, inventory channels, pickup and shipping eligibility, thresholds, safety stock, capacity limits, and routing rules without losing context. That makes rollout faster and reduces the chance that one app is configured correctly while another still has stale assumptions.

### How does this help support teams?

Support teams can troubleshoot sourcing questions with less app switching.

If an order did not broker, the support user can review the route, inspect the inventory rule, check the facility group, confirm whether the facility is eligible, and inspect product-facility inventory/configuration from the same workspace.

### How does this help store operations?

Store operations teams care about workload, eligibility, and inventory exposure.

The merged app makes it easier to control which stores participate in ship-from-store, which stores publish availability, how much inventory is protected for store demand, which stores can receive pickup or shipping work, and how many orders a store can handle.

### How does this interact with Routing Simulation?

Routing Simulation is a separate capability.

The merged Routing and Sourcing workspace is where users configure the sourcing model. Routing Simulation is where users can test proposed routing and sourcing changes before they affect live fulfillment.

The two capabilities are complementary. A user should eventually be able to change facility groups, safety stock, order limits, or inventory rules in the sourcing workspace and use simulation to understand the impact before publishing a risky change.

### What documentation needs to be updated?

The OMS user manual should stop presenting ATP and Routing as fully separate operating areas.

The documentation should explain the new Sourcing, Routing, and Foundations sections; update workflows for threshold, safety stock, store pickup, shipping, inventory channels, facility groups, brokering rules, and product inventory; and clarify which legacy inventory screens are being replaced.

### What should be measured after launch?

HotWax should measure whether users can complete sourcing workflows with fewer support escalations and less app switching.

Useful metrics include time to create a facility group and use it in a rule, time to diagnose why a facility was excluded, number of legacy product inventory page visits, successful inventory channel publishing updates, errors in product-facility configuration, and support tickets about ATP/routing mismatch.

### What are the main rollout risks?

The main risks are migration completeness and mental-model clarity.

Users need to understand that the Routing app is now broader than brokering. It is the sourcing workspace. Permissions, app names, side-menu labels, docs, and training should reinforce that shift.

The product inventory branch also needs careful review before general rollout because it introduces write actions for product-facility configuration and inventory variance logging. Those actions should have clear permissions, audit visibility, and safe error handling.

### What is the long-term vision?

The long-term vision is a single sourcing control plane for ship-from-store.

A retailer should be able to decide which facilities participate, what inventory is protected, how channels publish ATP, which products can broker, how much store capacity is available, which routing strategy applies, and what operational outcome a change will create.

That is the foundation for safer ship-from-store expansion, better troubleshooting, and future capabilities such as guided diagnostics, routing simulation, approval workflows, and AI-assisted sourcing recommendations.
