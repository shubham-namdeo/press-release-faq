# Pick Profiles for Downstream Sync Queuing

## Source Notes

- Pick profile data model: `hotwax-poorti/entity/PickProfileEntities.xml`
- Pick profile execution: `hotwax-poorti/service/co/hotwax/poorti/picking/PickProfileServices.xml`
- Ready-to-pick selection: `hotwax-poorti/sql/ReadyToPickOrders.sql.ftl`
- Fulfillment wave creation: `hotwax-poorti/service/co/hotwax/poorti/FulfillmentServices.xml`
- Sync queue context: Shopify bridge `SystemMessage` jobs, fulfillment feed jobs, sync history, and OMS integration design notes
- Requirements context: configurable warehouse release delays, cancellation grace periods, WMS/3PL backpressure, priority-based fulfillment sync, and business-controlled downstream release sequencing

## Press Release

### HotWax turns pick profiles into business-controlled downstream sync queues

HotWax Commerce today announced an expanded role for Pick Profiles: configurable release queues that let retailers control which orders become downstream fulfillment work, in what order, and in what batch size.

For years, retailers have treated pick lists as a warehouse artifact. A pick list told a picker what to collect after orders had already been released. But in modern omnichannel fulfillment, the more important question happens earlier: which orders should be released to the warehouse or 3PL first, which should wait, and which should stay out of the downstream system entirely until the OMS says they are safe?

Pick Profiles answer that question.

With HotWax, a pick profile is a saved release policy. It can filter eligible ship groups by facility, shipment method, carrier, product store, order date, order priority, sales channel, delivery speed, customer, or other configured fields. It can sort work by business priority. It can run on a schedule. It can limit batch size. It can create fulfillment waves and picklists only for ship groups that are approved, allocated, not already in fulfillment, and not blocked by an open order task.

That makes pick profiles a natural control point for downstream sync queuing. Instead of sending every releasable order to a WMS, 3PL, ERP, or integration job in simple FIFO order, HotWax can use pick profile logic to shape the queue before the downstream system receives work.

### The Problem

Retailers do not release all orders equally.

A same-day order may need to move before a standard order. A TikTok order may need a grace period so cancellation webhooks can arrive before the package reaches the warehouse. A fraud-review order may need to reserve inventory but stay out of the 3PL. A warehouse may only want batches of 200 orders at a time. A downstream 3PL API may be rate-limited. A carrier cutoff may make shipment method more important than order age. A facility may be overloaded while another has capacity.

Most queues are too blunt for that reality. They are often FIFO, code-driven, or integration-specific. When the downstream system is healthy, that may look fine. During flash sales, warehouse backlogs, fraud spikes, cancellation windows, or 3PL outages, FIFO becomes a liability.

Operations teams need a business-configurable way to answer:

- Which orders are eligible to release now?
- Which orders should be held from warehouse sync even if they have inventory reserved?
- Which priority should win when the downstream queue is backed up?
- How large should each release batch be?
- Which facility, channel, shipment method, or product store should a queue serve?
- What happened the last time this release policy ran?

### The Solution

HotWax Pick Profiles provide a configurable release layer between OMS planning and downstream fulfillment execution.

A pick profile group represents a scheduled queue for a facility or operating lane. Within the group, active pick profiles run in sequence. Each profile has conditions, sort rules, and actions. The execution service finds eligible ship groups, applies the configured filters, orders them by the configured priority, and creates fulfillment waves and picklists in controlled batches.

The ready-to-pick query intentionally limits the queue to safe work:

- The order must be an approved sales order.
- The ship group must be assigned to a real fulfillment facility, not a virtual facility.
- The ship group must not already have an active shipment.
- The ship group must contain approved items.
- The ship group must not have an open hold task linked to the order.
- Additional profile filters and sorts are applied dynamically.

Once a profile releases work, HotWax creates sales shipments and picklist records through the fulfillment wave service. That is the moment the order crosses from OMS planning into downstream execution.

For retailers using 3PL or WMS integrations, the same concept can drive downstream sync queues. A pick profile can define the set and order of work that should be transmitted, while SystemMessage, Napita, SQS, SFTP, or API-specific connectors handle transport, retry, throttling, and reconciliation.

### Why This Matters

Pick profiles let business users control release policy without asking engineering to rewrite integration code.

Retailers can define different queues for different operational lanes:

- Same-day and expedited orders before standard orders
- Orders near carrier cutoff before lower-urgency work
- One facility's work independent of another facility's backlog
- TikTok, marketplace, or influencer-campaign orders with channel-specific release rules
- Orders that have completed address, fraud, payment, or customer-service tasks
- 3PL release batches sized to downstream API or warehouse capacity
- Product-store or brand-specific release sequencing

This makes HotWax more resilient under backpressure. When a 3PL slows down, the OMS does not need to stop capturing, approving, brokering, or reserving orders. It can keep internal planning moving while pick profiles control what becomes downstream work and in what order.

### Customer Experience

An operations manager creates a pick profile group for a warehouse. Inside it, they configure one profile for same-day orders, another for expedited orders, and another for standard orders. The same-day profile sorts by carrier delivery days and order age, and limits each run to a controlled batch size. The standard profile runs after higher-priority profiles and only releases work that is not blocked by an open order task.

During a flash sale, orders continue to enter HotWax, reserve inventory, and become eligible for release. If the downstream 3PL slows down, HotWax does not blindly push the whole backlog in FIFO order. The pick profile queue keeps priority work moving first, respects batch sizes, and leaves unresolved orders out of the downstream release path.

Customer service can still see the order in OMS. Inventory is still protected. Warehouse workload is still visible. The 3PL receives only the work HotWax has intentionally released.

### Launch Scope

The current pick profile foundation includes:

- Pick profile groups for organizing scheduled release policies
- Pick profiles with draft, active, and archived statuses
- Configurable filter conditions
- Configurable sort conditions
- Configurable min and max picklist sizes
- Scheduled profile-group execution through Moqui service jobs
- Manual "run now" execution
- Sequential active-profile execution by `sequenceNum`
- Batch and run history with error/result tracking
- Ready-to-pick SQL that excludes already-started fulfillment work and open order tasks
- Fulfillment wave creation that creates sales shipments and picklists

The downstream sync queuing expansion should connect these release decisions directly to integration queue records, sync history, and transport-level throttling so every external system can consume prioritized work safely.

## FAQ

### What is a pick profile?

A pick profile is a configurable rule set for selecting and sequencing fulfillment work. It defines which ship groups are eligible, how they should be sorted, and how large the release batches should be.

The name sounds warehouse-specific, but the capability is broader. It is a release profile for deciding what work crosses from OMS planning into fulfillment execution.

### How is this different from a normal pick list?

A pick list is the output. A pick profile is the policy that decides which orders should become picklist or downstream fulfillment work.

Traditional systems often create pick lists after orders have already been released. HotWax can use pick profiles before that boundary, so operations teams can decide which orders should be released, held, prioritized, or batched.

### Why does this matter for integration queuing?

Downstream integrations need sequencing, not just transport.

An integration queue can send messages, retry failed payloads, and record history. But it still needs to know which business work should be sent first. Pick profiles provide that business layer: they can decide that same-day orders, certain channels, specific facilities, high-priority orders, or older orders should enter the downstream release queue first.

### Which fields can pick profiles use?

The current ready-to-pick query supports fields such as order ID, ship group, facility, shipment method, carrier, product store, order date, order name, external ID, priority, carrier delivery days, sales channel, and bill-to party. The configured enumeration set also includes common operational filters like queue/facility, shipment method type, order priority, sales channel, and batch size.

The model is extensible because pick profile conditions store the field name, operator, value, field type, and sequence.

### How does HotWax avoid releasing unsafe orders?

The ready-to-pick query excludes work that should not be released. It filters to approved sales orders, requires approved items, excludes virtual facilities, excludes ship groups that already have active shipments, and excludes orders with open hold tasks linked to that ship group.

That last point is important. An order can reserve inventory and remain visible as demand, but an open task can still block the warehouse or 3PL release boundary until the issue is resolved.

### How does this work with Order Tasks?

Order Tasks and Pick Profiles fit together naturally.

Order Tasks represent actionable hold reasons: bad address, fraud review, customer-service exception, substitution decision, payment review, or another configured task type. Pick Profiles represent release policy.

The ready-to-pick query already excludes ship groups with open hold tasks. That means a task-linked order can keep moving through internal OMS planning where appropriate, but it does not become downstream fulfillment work until the blocking task is completed or cancelled.

### How does this help during a 3PL outage or rate limit?

If a 3PL slows down, HotWax should not lose orders, re-promise inventory, or send work in a naive FIFO order. Orders can remain captured and reserved in the OMS while the release queue is shaped by pick profiles.

When downstream capacity returns, profiles can release the highest-priority work first and control batch size. Transport tools such as SystemMessage jobs, SQS, SFTP, API connectors, and Napita can then handle delivery, retry, throttling, and reconciliation.

### Does this replace SystemMessage, SQS, SFTP, or Napita?

No. Pick Profiles decide what should be released and in what sequence. SystemMessage, SQS, SFTP, API connectors, and Napita are transport and integration mechanisms.

The strongest architecture combines them: pick profiles provide business prioritization and release eligibility, while integration infrastructure provides delivery guarantees, retry, throttling, and history.

### How does batch size work?

Pick profiles can use configured actions for minimum and maximum picklist size. The run service uses the max size as the working batch size, defaults to a bounded batch when none is configured, and can skip a remaining tail if it is smaller than the configured minimum.

This matters for downstream systems because many WMS and 3PL integrations operate better when work is sent in predictable chunks instead of one huge release or thousands of one-order payloads.

### How are profiles scheduled?

Pick profile groups can be tied to Moqui service jobs. A group can have a cron expression, pause state, next execution time, and run-now action. When the group runs, active profiles in that group execute by sequence number.

That lets retailers create different release schedules by facility, channel, or operational lane.

### What does the run history capture?

Pick profile runs and batches capture start time, end time, profile group, profile ID, errors, result messages, and picklist counts. The entity model also has attempted and released count fields, which should be fully wired into reporting as the queue analytics mature.

This gives operations a way to answer, "Did this release policy run, did it error, and how much work did it release?"

### What should the downstream sync version add?

The next layer should explicitly connect pick profile output to integration queue records. For each target system, HotWax should be able to show:

- Which profile selected the work
- Which run created the release
- Which order, ship group, shipment, or picklist was queued
- Which external system received it
- Current sync status
- Retry count and last error
- Rate-limit or backpressure state
- Whether a higher-priority profile moved ahead of lower-priority work

This turns pick profiles from a fulfillment release mechanism into a full integration-queue control surface.

### What is the business value?

Retailers can change release behavior without changing code. They can prioritize same-day work, protect cancellation grace periods, keep risky or incomplete orders away from the warehouse, throttle 3PL load, and keep inventory reserved while downstream release catches up.

That is a major improvement over FIFO queues, hidden integration logic, and manual warehouse release lists.

### What is the recommended demo?

Create a pick profile group for a warehouse. Add three active profiles:

1. Same-day orders: filter by shipment method, sort by delivery days and order date, small batch size.
2. Priority channel orders: filter by sales channel or order priority, sort by order date.
3. Standard orders: lower sequence, larger batch size.

Then show an order with an open hold task staying out of the ready-to-pick results. Resolve the task, rerun the profile, and show the order moving into a fulfillment wave or picklist. Explain that the same release event can feed the downstream WMS or 3PL sync queue.

## Open Questions

1. Which downstream systems should be controlled by pick-profile release sequencing first: WMS, 3PL, ERP, Shopify fulfillment, or custom exports?
2. Should each pick profile target exactly one external system, or should the fulfillment wave drive multiple target-specific sync queues?
3. How should profile output be represented in sync history: order, ship group, shipment, picklist, or external fulfillment item?
4. Should downstream capacity limits be configured as profile batch sizes, integration remote limits, or both?
5. How should retry and resequencing behave when a high-priority profile fails but lower-priority profiles can still run?
6. Should operations users be able to pause a profile, a profile group, or a specific downstream target independently?
7. What dashboard should show queue health: pending by profile, pending by external system, estimated time to clear, oldest unreleased order, or SLA risk?
8. Which hold task types should block pick profile release by default, and which should allow release?

## User Manual Follow-Up

The user manual should include:

1. Pick profile overview
2. Difference between pick profiles, picklists, fulfillment waves, and downstream sync queues
3. Creating a pick profile group
4. Scheduling a pick profile group
5. Running a group manually
6. Creating pick profiles inside a group
7. Setting profile status and sequence
8. Adding filter conditions
9. Adding sort rules
10. Configuring min and max batch sizes
11. Understanding ready-to-pick eligibility
12. Understanding how open order tasks block release
13. Reviewing pick profile runs and batches
14. Troubleshooting a profile that released no work
15. Recommended profile patterns for same-day, standard, marketplace, fraud-review, grace-period, and 3PL-backpressure queues
