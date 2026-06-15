# Order Tasks

## Press Release

### Heading

HotWax turns stuck orders into assigned, actionable work instead of one-status-at-a-time holds.

**Subheading:** Order Tasks link orders to WorkEfforts so multiple hold reasons can be owned, worked, resolved, and audited while the order still brokers, reserves inventory, and stays out of the 3PL until it is safe to release.

## Summary

HotWax Order Tasks give retailers a more powerful way to manage order exceptions. Instead of treating a hold as a single status on the order, HotWax links the order to one or more WorkEfforts. Each WorkEffort can carry a reason, purpose, description, status, source reference, ship group, assignee, reporter, customer context, and resolution notes.

That changes the operating model. A bad-address issue can belong to customer service, a fraud review can belong to risk operations, a substitution review can belong to fulfillment, and a shipment exception can belong to a supervisor. All of those can exist on the same order at the same time without collapsing into a vague "on hold" state or forcing the last status update to overwrite the earlier reason.

The most important operational difference is that task-linked orders can still broker and reserve inventory while being held back from 3PL sync. Retailers keep visibility into warehouse workload and protect inventory for the customer on time, without letting an unresolved address, fraud, substitution, or service task reach the warehouse prematurely.

For customer service reps, this means a clearer answer when a customer asks why an order is stuck. For operations teams, it means the OMS becomes a task-based workflow engine for keeping orders moving. Teams can work from task boards and queue pages, filter by assignee, open the full order only when needed, take the required action, and resolve the task with a comment.

Shopify fulfillment hold sync is one strong use case for the same architecture. Shopify holds can become HotWax WorkEfforts, and resolving the work in either system can keep the other system in sync.

## Problem

Most OMS hold models flatten exception work into broad statuses such as "On Hold", "Fraud Hold", or "Address Hold". That is useful for preventing a release, but it does not answer the questions operations teams actually ask all day:

- Why is this order stuck?
- Who owns the next action?
- Is the issue address validation, fraud, substitution, shipment exception, payment, or something else?
- What if the order has more than one blocker at the same time?
- Which issue was added by risk, which was added by fulfillment, and which was added by customer service?
- Can customer service answer the customer without asking a warehouse or developer?
- Can multiple teams attach different reasons to the same order?
- Can one issue be resolved while another remains open?
- Can the order reserve inventory and show warehouse demand while still being prevented from syncing to the 3PL?
- Can supervisors see which unresolved tasks are assigned to which reps?

Market research shows that other platforms commonly support holds, risk automation, fulfillment capacity holds, or customer-service visibility. Shopify supports multiple fulfillment holds and Flow-based fraud automation, including hold reason and reason-notes fields. ShipHero supports fraud holds that keep orders out of the picking queue. Manhattan positions customer-service visibility as part of a unified OMS. HotWax can build on these patterns by treating Shopify holds and OMS exceptions as first-class OMS tasks: multiple WorkEffort records on the same order, each with ownership, workflow context, assignment, status lifecycle, and resolution history.

## Solution

HotWax models order exceptions as WorkEffort-backed Order Tasks.

The backend stores the task as a WorkEffort and links it to the order through `OrderHeaderWorkEffort`. A task can also be scoped to a ship group when the work is specific to a shipment path, address, facility, or item group. OMS exposes APIs to create order tasks, fetch task detail, list order task queues, change task status, and attach resolution comments through communication events.

This gives HotWax three advantages over a single hold-status field:

- Multiple active reasons can exist on the same order without overwriting each other.
- The order can keep moving through internal OMS steps such as brokering and inventory reservation while the unresolved task prevents unsafe 3PL sync.
- Users can work from a WorkEffort-driven task board instead of opening orders one by one to discover why each order is stuck.

The frontend turns that data model into operational workflows:

- Hold queue for general order holds.
- Bad Address queue for invalid address tasks.
- Swap queue for negative-reservation or substitution review.
- Fraud queue for risk review tasks.
- Order detail Holds segment for seeing active tasks on a single order.
- Add Task modal for creating a new task with type, purpose, name, and description.
- Assignee modal for finding staff and assigning ownership.
- Task cards that show customer contact context, order context, reason, next action, and resolution controls.

This makes a hold actionable. The hold is no longer just a status. It is a register of work that explains what is wrong, who needs to fix it, what they should do next, what inventory and warehouse demand are already reserved, and what happened when it was resolved.

## Getting started

Create an order task from OMS or Order Manager with order id, task type, task purpose, name, and description. Link the task to a ship group when the blocker is shipment-specific, such as bad address or substitution review. Assign the task to a user or team when ownership is known.

Users can then work the task from the correct queue: Hold, Bad Address, Swap, Fraud, or order detail. Where the workflow allows it, the order can continue to broker and reserve inventory while 3PL sync remains blocked until the task is resolved.

When the work is complete, resolve the task with a status update and resolution comment. Other active tasks stay open if the order still has additional blockers.

## Testimony

**Internal quote:** A hold tells you an order is stuck. An Order Task tells you why it is stuck, who owns it, what inventory is already protected, and what has to happen before the order is sent to the 3PL.

**Customer quote:** When a shopper asks why an order has not shipped, our reps need a real answer. They need to see the address issue, fraud review, or shipment exception without chasing three systems.

## Call to action

Use Order Tasks as the standard pattern for order exception work. Every new hold reason should create an actionable task with a type, purpose, owner, status, resolution path, and downstream-release policy instead of adding another flat order status.

## FAQs

**Question 1: What is an Order Task?**

Answer: An Order Task is a WorkEffort linked to an order through `OrderHeaderWorkEffort`. It represents work that must happen for the order to continue moving. Examples include bad address review, fraud review, substitution review, manual hold review, shipment exception follow-up, and customer-service escalation.

**Question 2: How is this different from a normal hold status?**

Answer: A hold status says the order is blocked. An Order Task explains the blocker and gives someone a unit of work to resolve. Because tasks are WorkEfforts, they can carry task type, purpose, name, description, status, created date, created by user, source reference, assignee, reporter, comments, and resolution history. Multiple tasks can exist on one order at the same time.

**Question 3: Why use WorkEffort?**

Answer: WorkEffort already models actionable work, assignment, status, and communication in the OMS data model. Reusing it avoids inventing a narrow hold table that only works for one exception type. The same pattern can support customer-service tasks, fulfillment tasks, fraud review, shipment exceptions, and future workflow actions.

**Question 4: Why is this better than one hold reason field?**

Answer: A single hold reason field forces the system to pick one explanation. WorkEfforts let each actor create their own task without destroying someone else's reason. Risk can create a fraud review, customer service can create an address task, fulfillment can create a substitution task, and a supervisor can create an escalation task. The order becomes a workflow hub, not a single mutable status.

**Question 5: Can task-linked orders still broker and reserve inventory?**

Answer: Yes. In many hold models, a held order is simply removed from fulfillment work. That protects the warehouse, but it also hides demand and can delay reservation until the issue is resolved. HotWax can treat the task as a downstream-release control instead: the order can continue through brokering and reserve inventory, while the open task prevents the order from syncing to the 3PL until the issue is fixed.

**Question 6: Why does the 3PL sync boundary matter?**

Answer: The 3PL is the point where operational mistakes become expensive. If a bad-address, fraud, or substitution issue reaches the warehouse too early, teams can waste pick labor, ship the wrong package, or create avoidable cancellation work. Holding the 3PL sync while still allowing OMS brokering and reservation gives retailers both controls: upstream planning visibility and downstream execution safety.

**Question 7: How should HotWax work with Shopify fulfillment holds?**

Answer: Shopify fulfillment holds should be treated as an integration source, not as a competing feature. Shopify is good at placing fulfillment holds from commerce events, apps, and Shopify Flow. HotWax is good at operational ownership, OMS brokering, inventory reservation, task queues, customer-service context, and 3PL release control. The best product story is to connect them: Shopify holds become HotWax WorkEfforts, and WorkEffort resolution updates the corresponding Shopify hold.

**Question 8: Is this different from Shopify fulfillment hold reasons?**

Answer: Yes, but the point is complementary. Shopify fulfillment holds can carry a reason and reason notes, and Shopify Flow can automate those holds. That gives HotWax useful upstream context. The HotWax difference is that the hold reason becomes an assigned WorkEffort with its own type, purpose, assignee, reporter, status, comments, resolution path, and relationship back to the order or ship group.

**Question 9: What happens if a hold is resolved in Shopify first?**

Answer: The integration should update the matching HotWax task. If Shopify releases a fulfillment hold, HotWax should mark the linked WorkEffort as completed or externally resolved, capture the release source, and re-evaluate whether the order can sync to the 3PL. This prevents split-brain operations where Shopify says the hold is gone but the OMS task board still shows work as open.

**Question 10: What happens if a task is resolved in HotWax first?**

Answer: The integration should release the matching Shopify fulfillment hold using the stored hold handle or source reference, then mark the WorkEffort complete with the CSR's resolution comment. This prevents the opposite split-brain issue: customer service resolves the task in OMS, but Shopify still blocks fulfillment because the commerce-side hold was never released.

**Question 11: What backend APIs support this?**

Answer: OMS exposes task APIs under `oms/orders/tasks` to list order tasks, list ship-group tasks, create an order task, fetch task detail by `workEffortId`, change WorkEffort task status, and attach communication content when resolving a task. Order detail can also fetch work efforts linked to a specific order through `oms/orders/{orderId}/workEfforts`.

**Question 12: What data does task detail return?**

Answer: Task detail includes order id, order name, external id, product store, order type, order date, grand total, ship group, shipment method, facility, carrier, WorkEffort type, purpose, name, description, created date, creator, customer, billing contact, shipping contact, and assigned parties.

**Question 13: How do assignment and ownership work?**

Answer: Tasks can have WorkEffort party assignments. Order Manager can search enabled employee parties, assign a task to the current user or another staff member, and display assignee/reporter information on task cards. A stuck order should not sit in a shared queue with no accountable person when the next action is known.

**Question 14: Can multiple teams add tasks to the same order?**

Answer: Yes. An order can have a fraud review task, a bad-address task, and a shipment exception task at the same time. Each task can have its own reason, owner, and resolution status. Resolving one task does not have to erase the history or ownership of another task.

**Question 15: Are tasks only order-level?**

Answer: No. Tasks can be order-level or ship-group scoped. Order-level tasks are useful for fraud, payment, customer-service escalation, and general holds. Ship-group tasks are useful for address, facility, substitution, carrier, and shipment-path issues.

**Question 16: Which task queues exist today?**

Answer: Order Manager currently has dedicated queue patterns for Hold tasks, Bad Address tasks, Swap/substitution tasks, and Fraud/risk tasks. These queues filter open task work by task type, purpose, product store, assignee, date, order, and channel where supported.

**Question 17: Why is a task board better than opening orders one by one?**

Answer: Opening orders one by one is useful for deep investigation, but it is a poor way to run exception operations. Teams need to see their work grouped by reason, assignee, age, channel, and priority. A WorkEffort-driven task board lets CSRs, fulfillment users, risk teams, and supervisors start from the work queue instead of the order list.

**Question 18: How do resolution comments work?**

Answer: When a user changes a task status, Order Manager can include resolution content. OMS writes that content as a communication event linked to the WorkEffort. This gives teams an audit trail for what was done, not just that the task moved from created to completed.

**Question 19: How does this compare to other OMS and fulfillment systems?**

Answer: Other systems commonly support holds, fraud holds, workflow automation, or customer-service visibility. Shopify supports fulfillment holds and Flow automations for risk workflows, including high-risk order cancellation and payment-capture decisions. Shopify holds can include reason and reason notes, and multiple holds can exist on a fulfillment order. ShipHero supports fraud holds that keep orders out of the picking queue. Manhattan markets unified order and customer-service visibility.

The differentiator for HotWax is not simply that it can hold an order or store a hold reason. The differentiator is that HotWax can integrate Shopify's fulfillment-hold model with OMS WorkEfforts: represent exception work as multiple assigned tasks connected to the order, let the order continue internal planning steps such as brokering and reservation where appropriate, and block the downstream 3PL handoff until the right task is resolved.

## Internal FAQs

**Question 1: What should be configurable next?**

Answer: Retailers should be able to configure task types and task purposes by product store, which systems can create which task types, default assignee or team by task purpose, SLA or due-time rules by reason, which tasks block fulfillment release, which tasks allow brokering/reservation but block 3PL sync, and which task resolutions release Shopify fulfillment holds or notify customer-service and warehouse integrations.

**Question 2: What is not complete yet?**

Answer: The WorkEffort-backed foundation exists, but the product story still needs hardening and documentation: user manual guidance for each task queue, a standard reason taxonomy, role and permission guidance, SLA/due-date handling, supervisor reporting, release-policy documentation, and bidirectional Shopify fulfillment hold sync design.

**Question 3: What is the demo slice?**

Answer: Use one order with multiple active issues: a bad-address task assigned to a CSR, a fraud-review task assigned to risk operations, and a substitution task assigned to fulfillment. Show that each task has its own owner, reason, and action path. Then show that the order can still reserve inventory and contribute to warehouse workload planning while the unresolved task keeps the 3PL sync blocked. Finally, release one task and show the matching downstream hold or release behavior.
