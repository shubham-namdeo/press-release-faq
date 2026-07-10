# Job Manager V2 - PR FAQ

## Press Release

### HotWax launches Job Manager V2, a Maarg-first control plane for integration operations

HotWax Commerce today announced Job Manager V2, a redesigned operational control plane for the jobs, files, system messages, reports, and search indexes that keep HotWax OMS integrations running.

Job Manager V2 strips away broad framework administration and presents an interface for the scheduled jobs, message flows, file imports, DataDocuments, and health tools that HotWax implementation and operations teams actually use to run retail integrations.

Retailers do not need another technical admin screen that exposes every background service in the platform. They need a focused operating room for the flows that move orders, inventory, fulfillment updates, product data, and integration payloads across the commerce stack.

Job Manager V2 gives them that operating room.

The app now brings together a global integration dashboard, a searchable job catalog, job detail and run history, MDM file history, system message monitoring, message type and remote system configuration, DataDocument reporting, Solr monitoring, and Solr repair into one Maarg-oriented workspace.

### The Problem

Modern OMS operations depend on many background flows. Orders are imported. Inventory updates are processed. Shopify bulk operations run asynchronously. Files are uploaded and validated. System messages are produced, sent, consumed, retried, and sometimes fail. Search indexes need to stay healthy. Scheduled jobs need the right parameters, cadence, and product-store context.

When those flows are hidden inside framework-level admin screens, support becomes slow and expert-dependent.

The old mental model was too technical:

- Find the correct Moqui job screen.
- Know which service name belongs to the business process.
- Know which job is a template versus a product-store-specific clone.
- Know where system messages, remote systems, message types, file logs, and job runs live.
- Know how to interpret status IDs, raw payloads, and linked messages.
- Know when a search issue is really a Solr index issue.

That is not a scalable operating model for retailers, support teams, or implementation teams.

### The Solution

Job Manager V2 makes integration operations visible and actionable from a Maarg-first perspective.

The new dashboard gives users a global picture of integration health: scheduled jobs, paused jobs, jobs without schedules, stuck executions, failed runs, slow jobs, high-priority file ingestion, standard file ingestion, incoming messages, outgoing payloads, failed imports, system message errors, and recent activity. Instead of making users inspect each subsystem first, the dashboard shows where attention is needed and links directly to the relevant job, file log, or system message.

The job catalog turns scheduled jobs into searchable operational cards. Users can search by job name, service, job ID, and product name; filter by category and subcategory; filter by scheduled, paused, or no-schedule state; and open job detail from the same page. The catalog is scoped to HotWax/Maarg jobs by focusing on jobs tied to `instanceOfProductId`, product categories, product-store dependencies, and system-message-remote dependencies. That is the intentional boundary: Job Manager should expose the jobs that operate HotWax integrations, not every framework job that exists in the underlying system.

Job detail gives each job an operating surface. Users can review service metadata, schedule settings, priority, retry behavior, timeout settings, product context, categories, parameters, and run history. They can pause or resume a job, edit its cron schedule and repeat count, edit supported parameters, run the job now, and inspect prior runs. Run history connects back to data logs where a job produced or processed files.

System Messages turn integration payload flow into a navigable journey. The monitor lists messages across all types and remote systems, with filters for status, parent type, message type, remote system, and keyword search. The detail page explains whether a message is inbound or outbound, what remote system it belongs to, what type configuration controls it, where it sits in the status journey, which messages are linked as parents or children, and which sequence steps belong to operations such as Shopify bulk query or import flows. Users can inspect payload content, copy or download files, edit eligible content, and take valid status actions derived from the system message transition model.

Data Manager and file history make imports auditable. Users can review processed files, search by log ID or file name, filter by status, priority, error state, and configuration, see success rate and processing time, cancel pending logs, and open file details for troubleshooting.

DataDocuments add the reporting layer. Job Manager now includes a DataDocument report builder and export history so teams can define reusable cross-record reports, preview data, export CSVs, schedule email exports, and use report definitions as a foundation for future data feeds. That capability has its own dedicated PR FAQ, so Job Manager V2 should treat it as a pillar of the overall control plane without making it the whole story.

Solr monitoring and repair add search health management. Users can see search service health, collection status, ping latency, JVM memory, API coverage, and core metrics. If search data is wrong, they can schedule an enterpriseSearch rebuild, monitor rebuild progress, search for an order or product, and reindex a single record.

### Why This Matters

Job Manager V2 changes integration support from backend spelunking into guided operations.

When a Shopify bulk import fails, a support user can start at the dashboard, see message errors, open the exact SystemMessage, review the payload journey, inspect the message type and remote system configuration, follow linked messages, and take the correct action. When a product import has failed rows, the user can open file history, review the log, inspect record counts, and connect the issue back to the job run. When a search result is stale, the user can check Solr health and reindex the impacted record instead of asking engineering to run a backend command.

That makes Job Manager V2 the right place to grow future integration operations:

- Create jobs from approved service templates.
- Create and manage job categories.
- Promote jobs from templates into product-store or remote-system-specific schedules.
- Manage NiFi/Napita dataflows from the same operating surface.
- Manage ALC-related integration processes.
- Expand Solr integration management beyond monitoring and repair.
- Add stronger queue, retry, throttling, and delivery controls.
- Connect job, message, file, report, and search health into one incident workflow.

### Customer Experience

An operations user opens Job Manager in the morning and starts on the dashboard. They see that most schedules are healthy, but one outgoing payload queue has errors and a high-priority file import finished with failed records.

They open the system message error directly from the dashboard. The detail page shows the message journey, remote system, message type, linked parent and child messages, payload content, and available status actions. The user can see that this payload belongs to a Shopify bulk operation sequence and can jump to the related message that downloaded the file.

Next, they open file history from the dashboard and filter to high-priority failed imports. The file log shows the failed record count, configuration, uploader, size, duration, and status. If the log is still pending, they can cancel it. If the issue traces back to a scheduled job, they open the job detail, review recent runs, edit parameters or schedule, and run the job again.

If customer service reports that an order is missing from search, the user opens Solr Repair, searches for the order, and reindexes that one record. If the issue is broader, they schedule a full enterpriseSearch rebuild and monitor progress.

The user never needs to know which generic OFBiz or Moqui admin screen hides the underlying entity. Job Manager presents the Maarg operating model directly.

### Launch Scope

Job Manager V2 includes:

- Global integration dashboard with health KPIs and actionable alerts
- Job schedule summary for active, paused, no-schedule, stuck, slow, and failed jobs
- High-priority and standard MDM ingestion summaries
- Incoming and outgoing SystemMessage summaries
- Queue Operations Map for file ingestion and message synchronization
- Action center for stuck jobs, failed runs, slow jobs, configuration errors, system message errors, and failed file imports
- Recent activity timeline across system messages and file logs
- Searchable job catalog
- Category and subcategory filters for jobs using product category rollups
- Job status filters for scheduled, paused, and no-schedule jobs
- Maarg-scoped job retrieval using product-backed job metadata and product-store/remote-system dependencies
- Job detail pages with metadata, schedule, parameters, and run history
- Pause/resume, run-now, schedule edit, repeat-count edit, and supported parameter edit actions
- Template/draft job cloning into product-store or remote-system-specific jobs
- MDM file history with KPIs, filters, config lookup, cancellation for pending logs, and detail navigation
- System Message monitor with status, parent type, message type, remote system, search, and pagination
- System Message detail with payload journey, linked messages, operation sequence, payload inspection, content edit, file download, status history, and allowed actions
- Message Type and Remote System catalog/detail/configuration flows
- DataDocument catalog, report builder, export history, and scheduled export foundation
- Solr Monitoring for health, collections, pings, JVM memory, API availability, and metrics
- Solr Repair for enterpriseSearch rebuilds, rebuild progress, order/product search, and single-record reindexing
- Settings shell and instance context

## FAQ

### What is Job Manager V2?

Job Manager V2 is the HotWax operations app for managing integration jobs, system messages, file processing, DataDocument reporting, and search health.

It is designed for implementation, support, and operations users who need to keep integrations running without dropping into framework-level admin screens.

### Why call it a Maarg-only interface?

Because the app intentionally filters and curates what it shows.

The underlying platform can contain many OFBiz, Moqui, and framework-level jobs. Job Manager V2 is not trying to be a universal scheduler console for all of them. It focuses on Maarg/HotWax operational jobs: jobs tied to product metadata, integration categories, product-store dependencies, system-message remotes, file flows, and integration execution.

That is why the job catalog is organized around HotWax business categories and product-backed job records instead of exposing every backend job equally.

### Is Job Manager replacing OFBiz or Moqui admin screens?

For HotWax integration operations, yes. For low-level framework administration, no.

OFBiz and Moqui admin tools may still exist for developers and platform administrators. Job Manager V2 is the productized layer for the operational tasks HotWax teams and customers need every day: search jobs, inspect schedules, run jobs, edit parameters, review runs, troubleshoot messages, inspect files, manage report definitions, and repair search data.

### What does the dashboard show?

The dashboard shows a real-time operating summary across jobs, file ingestion, and system messages.

It includes schedule counts, paused jobs, jobs without schedules, stuck executions, failed runs, slow jobs, high-priority ingestion failures, standard ingestion failures, incoming message errors, outgoing payload errors, queue depth, processing speed, actionable errors, and recent activity.

The purpose is to tell users where to act first.

### How does the job catalog work?

The job catalog lists Maarg operational jobs as cards. Users can search by job name, service name, job ID, and product ID. They can filter by primary category, subcategory, and schedule status.

The category model uses product categories and category rollups, which makes job organization business-friendly. For example, jobs can be grouped by integration domain or operating lane instead of forcing users to know service package names.

### What can users do from job detail?

Users can review job metadata, schedule settings, technical settings, product context, categories, custom parameters, and run history.

They can pause or resume the job, edit schedule fields, edit supported parameters, trigger run-now, and inspect run results. If a job is a draft/template for the current product store or remote system, Job Manager can clone it into a concrete job before scheduling or running it.

### What does "run now" do?

Run-now triggers a job execution without replacing the scheduled job. For draft/template jobs, Job Manager first clones the template into a product-store or remote-system-specific job, fills the dependency parameters, and then runs the cloned job.

That lets HotWax keep reusable job templates while still giving each retailer, store, or remote integration its own runnable schedule.

### What is the System Message flow?

System Messages are the integration payload records that move between HotWax and external systems. They represent inbound and outbound work such as import results, Shopify bulk operations, webhook payloads, exports, and other integration messages.

Job Manager V2 surfaces that flow through a monitor, detail pages, message type configuration, and remote system configuration.

### Why are System Messages so important?

System Messages are where integration truth often lives. A job may run successfully but produce a message that later fails. A webhook may be received but not consumed. A file may be generated but not delivered. A Shopify bulk operation may require multiple linked messages before the whole workflow completes.

By surfacing message status, payloads, remotes, types, linked messages, and sequence flows, Job Manager lets users debug the actual integration journey instead of only checking whether a scheduled job fired.

### What does the System Message detail page show?

It shows whether the message is inbound or outbound, the remote system, the message type, a narrative summary, status journey, status history, linked parent and child messages, operation sequence, payload content, detected file paths, errors, and configuration links.

Users can copy payload content, download linked files when available, edit eligible content, and take valid status actions based on allowed transitions.

### How does Job Manager handle file history?

File History lists Data Manager logs with KPIs and filters. Users can search by log ID or file name, filter by status, priority, error state, and config, review success rate and average processing time, cancel pending logs, and open file detail for troubleshooting.

The dashboard also separates high-priority and standard ingestion, which helps teams distinguish urgent operational files from routine processing.

### How do DataDocuments fit into Job Manager V2?

DataDocuments are the reporting and export pillar. They let teams create reusable cross-record reports, preview results, export CSVs, schedule email exports, and review export history.

This PR FAQ only covers DataDocuments at the Job Manager V2 level because the DataDocument report builder has its own dedicated PR FAQ.

### What does Solr management include today?

Job Manager V2 includes Solr Monitoring and Solr Repair.

Monitoring shows overall search health, Solr and Lucene version, JVM memory, collections, indexed document counts, collection pings, API availability, and key metrics. Repair lets users schedule an enterpriseSearch rebuild, monitor rebuild status, search for orders or products, and reindex a single record.

### Why does Solr belong in Job Manager?

Search health is operational health. If a product, order, or inventory record exists in the OMS but cannot be found in an app, the issue may be an index problem, not a data problem.

Putting Solr monitoring and repair in Job Manager gives support teams a safe operating surface before they escalate to engineering.

### What is the roadmap for job creation?

The roadmap is to let trusted users create jobs from approved service templates. The UI foundation already points in this direction: job creation needs a name, description, category, service, schedule, and custom parameters discovered from the selected service.

The productized version should persist the job, validate service parameters, assign it to the right category, enforce permissions, and make clear whether the job is a reusable template or a concrete product-store/remote-system job.

### What is the roadmap for job categories?

Today, Job Manager uses category and subcategory filters backed by product category rollups. The roadmap is to let administrators create and manage those job categories directly in the app.

That would let teams organize jobs by integration, flow type, business domain, priority, ownership team, or customer-specific operating lane without backend data setup.

### What is the roadmap for NiFi and Napita management?

NiFi/Napita management should become the next layer of integration operations.

Jobs and SystemMessages show scheduled execution and payload state inside HotWax. NiFi/Napita manages dataflow orchestration, transformations, retries, and transport outside the OMS application layer. Job Manager should eventually show NiFi flow health, queue depth, processor status, failed flow files, replay actions, and links between HotWax jobs/messages and NiFi flow execution.

That would give operators one control plane for both OMS-side integration state and integration-tier flow state.

### What is the roadmap for ALC management?

ALC management should be treated as another integration operations domain inside Job Manager.

The exact screens should follow the same principle as jobs, messages, files, and Solr: expose the operational entities, health state, queue state, errors, retry controls, and configuration that support teams need, without forcing users into low-level backend tools.

### What else should be on the roadmap?

The strongest roadmap items are:

- Create jobs from approved templates
- Create and manage job categories
- Manage category membership for jobs
- Promote draft/template jobs into concrete product-store or remote-system jobs
- Add stronger permissions around run-now, schedule edit, parameter edit, and message actions
- Add saved operational views for common error queues
- Add NiFi/Napita flow health and replay controls
- Add ALC management screens
- Expand Solr management to include more index diagnostics and repair history
- Link jobs, job runs, file logs, system messages, DataDocuments, and search repair operations into a single incident trail
- Add alerting and subscriptions for failed jobs, stuck messages, failed imports, and unhealthy search indexes

### What is the business value?

Job Manager V2 reduces the operational cost of integrations.

Instead of asking engineering to inspect backend jobs, raw messages, import logs, or search indexes, support and operations teams get a focused app that explains what is running, what is stuck, what failed, what payload was involved, what file was processed, what search index is unhealthy, and what action is safe to take next.

That is the difference between a framework admin screen and an OMS operations product.

## Open Questions

- Which users should be allowed to run jobs, pause schedules, edit parameters, edit message payloads, or replay messages?
- Which job creation flows should be available to customers versus HotWax implementation teams?
- Should job categories be global, customer-specific, product-store-specific, or integration-specific?
- What is the canonical relationship between a job run, file log, SystemMessage, DataDocument export, and NiFi/Napita flow execution?
- Which NiFi/Napita APIs should Job Manager use for flow status, queue depth, replay, and error inspection?
- What does ALC management need to expose first: health, queues, config, replay, or audit history?
- How should Solr rebuild and reindex actions be permissioned and audited?
- Which dashboard alerts should support subscriptions or external notifications?
