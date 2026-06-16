# Routing Simulation - PR FAQ

## Press Release

### HotWax lets retailers simulate ship-from-store routing changes before they touch live fulfillment

HotWax Commerce today announced Routing Simulation, a new order routing simulation capability for HotWax OMS that lets retailers test ship-from-store and warehouse routing strategies against production-shaped OMS data before releasing changes to live fulfillment.

With Routing Simulation, operations and implementation teams can ask practical what-if questions before they change routing policy. What happens if we lower brokering safety stock for a promotion? What if we add stores to a fulfillment group, cap store order volume, change rule sequence, widen a distance rule, or treat a warehouse as already at capacity? HotWax can run the proposed change in an isolated simulation context, compare it to the current baseline, and show the expected impact without mutating production orders, inventory, reservations, or facility order counts.

This gives retailers a safer way to expand ship-from-store. Instead of guessing, waiting for engineering analysis, or learning from live-order fallout, teams can compare routing variants, inspect facility workload, explain order-level outcomes, and decide which policy is ready to publish.

### The Problem

Ship-from-store turns the store network into a fulfillment network. That creates more capacity and faster delivery options, but it also makes routing policy harder to change safely.

A small configuration change can produce a large operating shift. More stores may become eligible, but the added work may overload one region. Lower safety stock may rescue online orders, but it may also increase store stockout risk. A rule sequence change may make one facility group absorb work that another group used to handle. A distance or capacity change may improve fill rate while increasing delivery cost or store labor pressure.

Most retailers test these changes in weak ways:

- They reason from intuition and change production.
- They test in staging with stale or artificial data.
- They inspect individual orders but cannot see network-level impact.
- They ask engineering for custom SQL, then still miss procedural routing behavior such as rule fall-through, inventory consumption, and facility order limits.

For high-volume retailers, the cost of being wrong is immediate: unfillable orders, late shipments, store confusion, excess split shipments, unnecessary customer-service work, and loss of trust in ship-from-store.

### The Solution

Routing Simulation gives HotWax a controlled what-if engine for brokering.

The new `sim-routing` Moqui component is extracted from the order-routing work and depends on `maarg-util`, `oms`, and `order-routing`. It provides a simulation REST surface, simulation screens mounted under Order Routing, isolated H2-backed simulation data, saved scenarios, editable routing variations, persisted simulation results, exports, and Docker deployment support for a dedicated simulation instance.

For simple analysis, Routing Simulation supports product-store scoped what-if runs. A user can test parameter and data overrides such as safety stock, distance, facility group membership, inventory counts, allow-brokering flags, facility order limits, at-limit facilities, assignment mode, inventory sorting, and inventory consumption modeling.

For deeper analysis, Routing Simulation supports routing-group scoped group runs. The group-run engine replays the full routing group pipeline, evaluates active routing records, applies filters, runs active rules, follows run-next-rule behavior, calls the routing action service, models simulated inventory and capacity consumption, and returns a trace for each order item ship group.

The same platform also supports editable variations. A user can clone a live routing group into the H2 `SIMVAR` schema, edit the simulated routing configuration, save the whole variation config or individual rule changes, and run a full group simulation against that variation. Production routing configuration is not mutated by the simulation.

### Why This Matters

Routing is no longer just configuration. For ship-from-store retailers, routing policy controls customer promises, store labor, warehouse workload, inventory exposure, and transportation cost.

Sim Routing adds the missing step between configuration and production. Teams can compare options before orders are already moving. They can see whether a policy routes more orders, whether the improvement comes from a healthy spread of work or from a few overloaded facilities, whether safety-stock changes are worth the inventory risk, and whether a rule sequence change improves throughput or simply moves the backlog.

This is especially important for retailers expanding ship-from-store. The first rollout is usually conservative. As teams gain confidence, they want to test more aggressive strategies: wider fulfillment regions, more stores, lower inventory buffers, promotion-specific rules, store-capacity limits, fulfillment lanes by geography or delivery promise, and different policies for expedited orders. Sim Routing lets them explore those strategies with real operating context instead of guesses.

### Customer Experience

An operations manager is preparing for a weekend promotion. The current routing group first tries the warehouse, then routes eligible orders to a regional store group. The team wants to know whether more stores should participate and whether safety stock can be lowered for promotion SKUs.

The manager opens the simulation workflow for the routing group and creates several variants:

- Baseline: current routing policy.
- Variant A: lower brokering safety stock for promotion SKUs.
- Variant B: add 20 more stores to the fulfillment group and cap each store at a controlled order limit.
- Variant C: widen distance eligibility for expedited orders while leaving standard orders unchanged.

HotWax runs the baseline and variants against the simulation data. The result shows how many order items were attempted, brokered, queued, or left unfillable. It shows which facilities gained work, which final reasons changed, and which rules were responsible for each result.

The team sees that Variant A routes more orders but concentrates too much work at five stores. Variant B routes nearly as many additional orders while keeping stores below their capacity threshold. Variant C helps expedited orders but increases average fulfillment distance more than the team is willing to accept.

Instead of making three live changes and watching the fallout, the team publishes the safer policy, saves the scenario, and keeps the rejected variants for later review.

### Launch Scope

The current `hotwax/sim-routing` implementation covers:

- Standalone `sim-routing` component version 1.0.0, extracted from order-routing and depending on `maarg-util`, `oms`, and `order-routing`.
- Product-store scoped what-if simulation with sync and async REST workflows.
- Routing-group scoped full pipeline simulation with sync and async REST workflows.
- Moqui `ServiceJobRun` backed async submit and poll for long-running simulations.
- Saved simulation scenarios with create, find, get, update, deactivate, reactivate, variant add, update, remove, and reorder services.
- Editable SIMVAR variations, including clone, list, get, whole-config replace, routing status edits, filter edits, rule status and sequence edits, inventory condition edits, action edits, and run-against-variation simulation.
- Persisted simulation result reads for past simulations, including header list, simulation detail, variant items, and server-side aggregates.
- CSV and queryable H2 export for completed simulation runs, including result CSVs, input CSVs, `query.mv.db`, manifest, and optional zip output.
- H2 `LIVE` mirror for simulation execution, with startup schema provisioning and column drift checking.
- Read-only `prod-source` MySQL replica as the source for syncing production-shaped data into H2.
- Initial-load console services to generate load batches, analyze source-query cost, recursively chunk large reads, run approved batches, and abort running work.
- Docker deployment assets for a dedicated simulation instance with embedded H2 transactional storage, H2 simulation storage, and read-only MySQL replica access.
- Simulation screens mounted under the existing Order Routing screen tree.
- Read-only context APIs for brokering facility groups, brokering settings, brokering queue distribution, facility order limits, facility change summaries, and routing-run summaries.

The frontend `hotwax/order-routing` app has also merged the Available to Promise and inventory sourcing surfaces into the routing app. That app consolidation is related because users need routing rules, inventory channels, facility groups, safety stock, threshold, shipping, store pickup, and brokering context in one operational workspace. Sim Routing is the next layer: it lets those users test policy changes before publishing them.

### Not In Launch Scope Yet

- Automatic application of a simulated variation to live routing configuration.
- Agent-driven write actions that change routing policy.
- Marketing claims around split-order or shipping-cost metrics until those metrics are verified in the current implementation.
- Permission and authentication hardening for every externally reachable simulation path.

## FAQ

### Why make it a separate component?

Simulation has different operating requirements from live routing.

Live routing is the production decision engine. Sim Routing needs isolated H2 execution, a read-only production source, local result persistence, initial-load and sync controls, variation config stored outside production, and deployment patterns for a dedicated simulation instance. Keeping this in a separate component lets HotWax depend on the real order-routing services while isolating simulation-specific data, screens, jobs, and deployment concerns.

### How is this different from Test Drive?

Test Drive and Sim Routing answer different questions.

Test Drive is the existing OMS workflow for validating how routing behaves for a specific order or ship group. It helps a user inspect whether an order is eligible for a route, which rule matched, which facility was selected, and why the current configuration produced a specific outcome. It is useful for implementation proof, customer demos, and debugging a known order.

Routing Simulation is broader. It forecasts the network-level impact of a proposed routing policy change before that change is published. Instead of testing one order path, it compares baseline and variant outcomes across a bounded order scope, facility network, inventory picture, routing group, and capacity model. It answers questions such as how many more orders route, which facilities gain workload, which final reasons change, and whether a proposed policy creates operational risk.

The other important difference is isolation. Test Drive is tied to live OMS routing behavior and the current test-drive flow warns users that testing an order can allocate it to inventory unless the tested order is reset. Routing Simulation runs in an isolated simulation environment backed by the H2 `LIVE` mirror and `SIMVAR` variation config, so scenario analysis does not mutate production orders, inventory, reservations, or live routing configuration.

They should work together. Test Drive is still the right tool for explaining one order. Routing Simulation is the right tool for deciding whether a routing change is safe for the broader ship-from-store network.

### Does Routing Simulation mutate production data?

No. The intended architecture avoids production mutation.

The Docker deployment model uses three datasource roles:

- `transactional`: embedded H2 for framework data, saved scenarios, and `BrokeringSimulation*` results.
- `simulation`: H2 file storage for the simulation engine and `LIVE` mirror.
- `prod-source`: read-only MySQL production replica used as source data and only selected from.

Production-shaped data is synced into the H2 `LIVE` schema. Variations are cloned into the H2 `SIMVAR` schema. Simulation runs read and write inside the simulation environment, not the production OMS database.

### What is the `LIVE` mirror?

`LIVE` is the H2 schema that mirrors the source tables needed by simulation.

The component provisions the schema at startup from entity metadata, compares source and H2 columns for drift, and keeps data current through sync jobs and initial-load tooling. The goal is to give the simulation engine production-shaped data without running what-if writes against production.

### What is `SIMVAR`?

`SIMVAR` is the H2 schema used for editable routing variations.

A user can clone a live routing group into SIMVAR, change routing status, filters, rule sequence, rule assignment, inventory conditions, and actions, then run a group simulation against that variation. The edited config stays in simulation storage until a separate future publish workflow explicitly applies an approved configuration to live routing.

### What is a saved scenario?

A saved scenario is a reusable set of simulation variants for a routing group.

For example, a retailer can save a "holiday ship-from-store expansion" scenario with variants for lower safety stock, additional stores, different facility order limits, and different rule sequences. Users can update the scenario, reorder variants, deactivate it, reactivate it, and run it again later.

### What is the difference between product-store what-if simulation and routing-group simulation?

Product-store what-if simulation is the simpler impact workflow. It takes flat parameter and data overrides for a product store, runs the current baseline against a proposed change, and returns impact data.

Routing-group simulation is the deeper workflow. It replays a full order routing group end to end, including routing records, filters, rule sequence, rule fall-through, routing actions, and per-order traces. It is the right model for debugging a routing pipeline and comparing larger policy variants.

### What routing parameters can be simulated?

The current input model supports distance, brokering safety stock, week-of-supply threshold, facility group, facility order limit behavior, split order item group behavior, assignment mode, inventory sort order, and inventory consumption modeling.

It also supports data overrides such as minimum stock, inventory counts, allow-brokering flags, maximum order limits, at-limit facilities, and facility group membership changes.

### What routing configuration changes can be simulated?

The SIMVAR variation path supports routing configuration edits without changing production. Users can replace the full variation config tree or make targeted edits to routing status, filters, rule status, rule sequence, rule assignment mode, inventory conditions, and actions.

This is important because many real routing decisions are not simple parameter changes. Teams often need to test a different rule order, an added queue action, or a changed inventory condition.

### What does the result show?

The group-run result shows the routing group, product store, attempted item count, brokered item count, queued item count, duration, run options, and per-variant outcomes.

At the order level, the result can show the order ID, ship group, order item, final reason, final assignments, and rule attempts. Final reasons include outcomes such as fully brokered, partially brokered, queued, unfillable, and error.

The persisted reader services also expose aggregate summaries by final reason, facility workload, routed quantity, and distance so the frontend does not need to pull every item to render a comparison view.

### How are past simulations reviewed?

Simulation runs can be persisted into `BrokeringSimulation*` entities. The read services support:

- Listing past simulations with filters.
- Opening one simulation with its variants.
- Paging per-item outcomes for a variant.
- Filtering variant items by facility, final reason, or order.
- Fetching server-side aggregates for comparison views.

This turns simulation from a one-time API response into a reviewable operating record.

### What gets exported?

Completed group-run simulations can be exported as a self-contained folder. The export includes result CSVs, input CSVs, a queryable H2 database file named `query.mv.db`, a manifest, and optionally a zip file.

This gives implementation, customer success, and analytics teams a way to inspect a run outside the application, reproduce analysis, or share data with stakeholders without granting direct access to the simulation instance.

### How does the initial-load console work?

The initial-load tooling helps populate the H2 `LIVE` mirror safely from the read-only source.

The flow is designed around operator approval:

- Generate load batches.
- Analyze source-query cost with EXPLAIN-style analysis.
- Recursively chunk large reads based on cost.
- Approve and run safe leaf batches.
- Abort running work when needed.

The design avoids blind full-table pulls against a production replica. It treats analysis and execution separately and only writes into H2 `LIVE`, not the MySQL source.

### What is the default sample cap?

The current config uses `simulation.sampleCap` with a default of 500. The current variant max count is 5.

Sample caps are necessary because a full production queue can be large. The UI and any agent answer should disclose when a cap was used so users understand whether the run represents the full queue or a bounded sample.

### How does inventory consumption modeling work?

The simulation can model inventory consumption between order iterations.

When enabled, each simulated assignment reduces simulated availability before later orders are evaluated. That makes the run closer to real brokering behavior: earlier orders consume inventory and capacity that later orders cannot use. An opt-out remains useful for parity and diagnostic cases where a user wants per-order-independent behavior.

### How does this help store operations?

Store operations teams can understand store workload before a routing change is live.

They can see which stores gain work, whether stores are nearing configured order limits, whether a policy spreads demand across the network, and whether a change creates concentrated load in one region. That helps them expand ship-from-store without surprising stores.

### How does this help implementation and customer success?

Implementation and CS teams can use Sim Routing during rollout, tuning, and customer reviews.

For a new ship-from-store launch, they can show conservative, moderate, and aggressive policies side by side. For an existing customer, they can investigate complaints about store workload, explain why orders are landing in specific queues, and propose safer routing changes backed by simulation output.

### How does this help executives or business owners?

Simulation turns routing policy into a measurable business decision.

Instead of saying "this change should route more orders," teams can show expected changes in routed count, final reasons, facility workload, queue outcomes, capacity warnings, and data freshness. That makes ship-from-store expansion easier to approve and easier to govern.

### How does this relate to the routing app and ATP app merge?

The routing app now includes ATP and inventory sourcing rule surfaces that used to be separate. That creates the right product workspace for simulation.

The intended user experience is one operational place where users can review routing rules, inventory channels, facility groups, safety stock, threshold rules, shipping rules, store pickup rules, brokering context, and simulation results. Sim Routing should sit on top of that workspace so users can model, compare, approve, and eventually publish routing policy from the same context.
