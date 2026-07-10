# Job Manager DataDocument Report Builder - PR FAQ

## Press Release

### HotWax turns DataDocuments into a self-service operational report builder

HotWax Commerce today announced a new Data Document report builder in Job Manager, giving implementation and operations teams a low-code way to create reusable OMS reports, preview live results, export CSV files, schedule recurring email exports, and review export history.

The same foundation also points to a larger direction for HotWax OMS: DataDocuments should become the shared definition layer for reports, integration payloads, operational dashboards, and near-real-time data streams.

Most OMS reports are either hard-coded screens, SQL queries owned by engineering, or one-off exports assembled for a single customer request. That slows down operations teams when they need to answer practical questions: which orders are stuck, which imports failed, which products are missing data, which integration messages errored, which fulfillment work is building up, and which records should be sent to a BI or warehouse system.

HotWax DataDocuments solve the problem at the data-shape level. A DataDocument defines a reusable business view over the OMS entity model: the root entity, related entities, selected fields, field aliases, field functions, conditions, and display metadata. Job Manager now gives users a dedicated interface to build, inspect, run, and export those definitions without writing XML or raw SQL.

This makes reporting a product capability instead of a support workflow.

### The Problem

Operational data in an OMS is relational by nature. Orders connect to order items, ship groups, inventory reservations, shipments, payments, customers, facilities, integration messages, product records, and import logs. The questions users ask rarely live in one table.

That creates a recurring gap:

- Operators need cross-record reports but do not want SQL access.
- Implementation teams need customer-specific reports without building custom screens every time.
- Customer support teams need explainable data views for stuck orders and exceptions.
- Integration teams need exportable record sets for debugging and reconciliation.
- Executives need operational data in BI systems, warehouses, and dashboards.
- Engineering teams need reusable data definitions instead of one-off scripts.

Traditional exports are too static for this. A hard-coded report may answer one question, but it does not create a reusable definition that can later be previewed, exported, scheduled, monitored, or streamed.

HotWax needed a way to let trusted users compose OMS business data safely using the platform's entity model, not by exposing raw database access.

### The Solution

Job Manager now treats DataDocuments as a first-class report-building surface.

Users can open a dedicated Data Documents section, browse existing document definitions, create a new document, select a primary entity, add fields from the primary entity or related entities, define aliases, choose field functions such as sum or average (`avg`) where the report needs measures, add structured conditions, validate the shape, preview live results, queue exports, schedule recurring email exports, and review export history.

The graph-first builder makes a DataDocument easier to understand. Instead of treating a report as a flat list of fields, the builder shows the primary entity, related entities, selected fields, conditions, validation issues, preview data, usage, and export history in one detail workspace. Users can move between graph, fields, conditions, preview, usage, issues, and export history without losing the context of the report they are editing.

Preview runs through the OMS DataDocument runtime. Users can select output fields, apply runtime filters, sort results, use distinct mode, page through results, copy data, and export CSV from the preview table. Export queueing uses the same platform pattern as other HotWax export jobs: the request creates a `SystemMessage`, the backend renders the file, and Job Manager shows status, errors, and download actions from export history.

Scheduled email exports extend the same model. A user can choose recipients, frequency, and export parameters. Job Manager creates a service job that calls the DataDocument export service on schedule, producing an auditable export record instead of an untracked manual download.

### Why This Matters

DataDocuments give HotWax OMS a practical semantic layer for operations.

The important shift is not just "users can export a CSV." The important shift is that HotWax can save reusable data definitions that have business meaning. A document can describe stuck order work, product data quality gaps, integration errors, shipment status, inventory reservations, pick profile output, order tasks, import results, or financial settlement candidates.

Once that definition exists, the same shape can support multiple workflows:

- An operator previews the data in Job Manager.
- An implementation user exports a CSV for investigation.
- A scheduled job emails the report every morning.
- A support team uses the report as an exception workbench.
- A BI pipeline loads the exported data into a warehouse.
- A future DataFeed publishes the same business payload as an event stream.

This gives HotWax a compounding reporting model. Each useful DataDocument can become more than a one-time report; it can become a reusable operational asset.

### Customer Experience

An implementation user needs a report that shows orders waiting on downstream fulfillment sync. They open Job Manager, create a DataDocument, choose the relevant order or integration-message entity as the root, add related fields for order name, product store, facility, status, last error, and timestamps, then add conditions for the unresolved statuses they care about.

Before saving the report for the team, they preview live results, add a runtime filter for facility, sort by oldest error first, and confirm the output columns are readable. They queue an export, open export history, and see the generated file and status. Then they schedule the report to email the operations group every morning.

Later, the same organization wants realtime visibility into the same operational backlog. The DataDocument already defines the business payload. The next layer is to attach that definition to a DataFeed and publish committed OMS changes to a queue, warehouse, webhook endpoint, or WebSocket dashboard.

### Launch Scope

The current Job Manager Data Document report builder includes:

- Dedicated Data Documents catalog in Job Manager
- Single graph/detail route for creating and editing DataDocuments
- Graph-first workspace for understanding root entity, relationships, selected fields, conditions, issues, preview, usage, and export history
- Field and condition CRUD for DataDocument definitions
- Direct field and relation-path field support
- Field aliases and selected output fields
- Field functions for measures and aggregations such as count, count distinct, sum, average (`avg`), minimum, and maximum
- Condition authoring and validation feedback
- Searchable field picker and metadata-assisted authoring
- Preview table with runtime filters, selected fields, sort, distinct mode, pagination, copy, and CSV actions
- Export queueing through the DataDocument export service
- Export history backed by `SystemMessage` records
- CSV download for completed exports
- Scheduled email export modal backed by service jobs
- Pause/resume support for scheduled export jobs
- Removal of unbacked query presets so the UI only presents productized capabilities

The strategic expansion areas are filtered-export parity with preview, report sharing and permissions, report templates, stronger relationship metadata, first-class DataFeed authoring, destination configuration, schema/version governance, replay/idempotency controls, and realtime delivery to Pub/Sub, Kafka-style queues, WebSocket dashboards, Snowflake, BigQuery, or other analytics platforms.

## FAQ

### What is a DataDocument?

A DataDocument is a saved business data shape over the OMS entity model. It defines a primary entity, fields, related fields reached through relationship paths, aliases, field functions, conditions, and display metadata.

In practical terms, it is a reusable report or integration payload definition. It is not raw SQL and it is not just a static CSV template.

### What is the Job Manager report builder?

The Job Manager report builder is the user interface for creating, editing, previewing, exporting, and scheduling DataDocuments.

It lets trusted users compose reports from OMS entities using structured fields, relationship paths, filters, aliases, and conditions. The UI hides the underlying XML/entity complexity while preserving the power of the Moqui DataDocument model.

### Why does this belong in Job Manager?

Job Manager already owns operational execution, service jobs, system messages, import/export history, and troubleshooting workflows. DataDocument reports naturally sit in that same operating layer.

Reports are not just analytics artifacts. They often drive scheduled jobs, exception monitoring, integration debugging, export queues, and operational routines. Job Manager is the right place to manage those runnable data workflows.

### Is this a SQL editor?

No. Users are not writing arbitrary SQL.

The builder uses structured DataDocument records and runtime query parameters. Users choose entities, fields, relationship paths, aliases, field functions, operators, conditions, selected fields, sort rules, and pagination. That gives HotWax more control over safety, reuse, validation, permissions, and future UI guidance than a raw SQL console would.

### What did the latest Job Manager work add?

The latest Job Manager work turned DataDocuments from backend/admin configuration into a first-class app workflow.

It added a dedicated catalog, graph/detail workspace, field and condition editing, preview table, export queueing, export history, scheduled email exports, deep-linkable segments, unsaved-change protection, and cleanup of unbacked mock/query-preset behavior.

### Why is the graph builder important?

Most useful OMS reports cross entity relationships. A flat field table makes it hard to understand which data is coming from the root entity and which data is coming from related records.

The graph builder makes the report shape visible. The user can see the root entity, related entities, selected fields, conditions, issues, preview, usage, and export history in one workspace. That makes DataDocuments easier to review, teach, debug, and extend.

### What can users configure?

Users can configure the primary entity, direct fields, relation-path fields, field aliases, field order, field functions, conditions, selected output fields, runtime filters, sort rules, distinct mode, page size, and scheduled export settings.

As the metadata layer matures, the builder should guide users through verified relationships instead of requiring them to know or type relationship paths manually.

### How do field functions work?

DataDocument fields can use functions such as count, count distinct, sum, average (`avg`), minimum, and maximum to turn raw fields into report measures. That matters when a report is not just listing records, but answering questions like total reserved quantity, average processing time, total failed messages, or aggregate demand by facility, channel, or product.

The important product idea is that users should be able to define both dimensions and measures inside the report definition. A user can choose fields that describe the row, such as facility or product store, and fields that calculate values, such as summed quantity or averaged duration.

### How does preview work?

Preview calls the OMS DataDocument runtime endpoint. The user can select fields, apply filters, sort results, use distinct mode, page through records, and inspect the output before exporting or scheduling.

This is important because a report should be validated against live data before it becomes a recurring export or a trusted operational view.

### How does export work?

Exports are queued through the DataDocument export service. The backend creates a `SystemMessage`, renders the DataDocument data to CSV, marks the message status, and makes the generated file available for download.

This is better than treating exports as browser-only downloads because export history, retries, failures, generated files, and scheduled jobs all become auditable.

### Does export support the same filters as preview?

Not fully yet. Preview supports selected fields and runtime filters through the DataDocument view endpoint. The current export service is centered on the saved DataDocument definition plus page, page size, and ordering parameters.

Filtered-export parity should be a priority expansion so the exact report a user previews can be queued, scheduled, emailed, and downloaded without changing semantics.

### How do scheduled email exports work?

Job Manager creates a scheduled service job that calls the DataDocument export service on a cron schedule. The export service produces the CSV, records the work as a `SystemMessage`, and emails the generated file to the configured recipients.

This turns a report into a recurring operating routine without requiring a custom integration for every scheduled report.

### How does export history work?

Export history reads DataDocument export `SystemMessage` records. Users can see export status, timing, errors, and download actions from Job Manager.

This follows the broader HotWax pattern where generated files and outbound work are tracked through System Messages instead of disappearing into local browser downloads.

### How is this related to Data Manager and file history?

Data Manager focuses on importing, mapping, validating, and processing operational files. DataDocument reports focus on shaping and exporting OMS data.

Together, they create a stronger operational data loop. Users can import files, review import logs and file history, inspect the resulting OMS records through DataDocument reports, export reconciliation data, and eventually stream important changes to downstream systems.

### How can this support BI and data warehouses?

DataDocument exports can provide repeatable data extracts for BI and warehouse ingestion. Instead of every warehouse feed starting as a custom SQL query, HotWax can define named business documents for orders, inventory, shipments, exceptions, products, imports, or financial events.

Those documents can be scheduled to land in systems such as cloud storage, BigQuery, Snowflake, Tableau-facing datasets, or other analytics platforms. The short-term path is scheduled export. The long-term path is event-backed DataFeeds for lower-latency pipelines.

### How can this support realtime streaming?

Moqui DataFeeds can attach to DataDocuments and produce document payloads when relevant OMS data changes. A real-time push feed can run after the underlying transaction commits, regenerate affected DataDocument records, and invoke a receive service with the generated document list.

That means the same DataDocument concept used for a report can also define an event payload. For example, HotWax can publish order status, shipment status, inventory adjustment, fulfillment release, order task, pick profile, or integration-error events to a webhook dispatcher, queue, warehouse ingestion endpoint, or realtime dashboard service.

The right product framing is committed-state event publishing, not raw database change-data-capture.

### Does HotWax already use DataFeeds this way?

Yes, HotWax OMS already has a webhook-style DataFeed pattern. A real-time DataFeed can include DataDocuments such as order status, shipment status, and order item payloads, then call a receive service that processes the generated documents and dispatches outbound events.

The Job Manager report builder does not yet turn that into a complete business-user DataFeed authoring product, but it uses the same underlying DataDocument model. That is why this work is a strong foundation for realtime operational data.

### What would first-class realtime feed authoring require?

The next layer should let trusted users or implementation teams define:

- Which DataDocument should publish events
- Which changes should trigger the feed
- Which destination receives the payload
- Which schema version is active
- Which routing attributes are included
- How idempotency keys are generated
- How failures, retries, and replay are handled
- How high-volume feeds are throttled or aggregated
- Which users can view, edit, run, or subscribe to the feed

This should be productized carefully because realtime feeds are operational integrations, not just reports.

### What realtime use cases should HotWax prioritize?

The strongest first use cases are operational streams where latency matters:

- Order status changes
- Fulfillment release and pick profile output
- Shipment status changes
- Inventory adjustments and ATP-impacting changes
- Open and resolved order tasks
- Failed downstream integration messages
- Import completion and failure events
- Order velocity and flash-sale dashboards
- Financial settlement eligibility

These are the events that help teams keep orders moving during high-volume operations.

### How does this compare to static reporting?

Static reporting answers a known question at a point in time. DataDocuments create reusable business data definitions that can be previewed, exported, scheduled, monitored, reused in apps, and eventually streamed.

That makes the reporting layer more durable. The business meaning lives in the DataDocument, not in a one-off spreadsheet, ad hoc SQL query, or hard-coded screen.

### What are the main risks?

The main risks are governance and overreach.

If the builder exposes too much low-level entity complexity, it can become hard to use. If it exposes too little, it becomes another fixed report tool. If realtime feeds are added without schema control, destination management, retry visibility, and permissions, they can become fragile integrations.

The right path is to keep the report builder practical now, then add feed authoring, delivery targets, and realtime monitoring as explicit product layers.
