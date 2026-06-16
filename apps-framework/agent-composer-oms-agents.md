# Agent Composer Framework and OMS Agents - PR FAQ

## Press Release

### HotWax introduces Agent Composer for building governed OMS agents

**Subheading:** Retail operations teams can now create AI agents that understand OMS workflows, use approved HotWax capabilities as tools, and complete work with human approval where it matters.

## Summary

HotWax Commerce today announced Agent Composer and Workforce for HotWax OMS, a new agent framework that lets operations teams create, test, activate, and run AI agents inside the OMS.

Agent Composer gives business users a guided way to turn operational intent into a working agent. A user can name the agent, write or enhance its instructions, choose the model, set reasoning effort, grant OMS capabilities from a searchable tool catalog, and decide which actions can run automatically versus which require human approval. Before activation, the user can preview the draft agent on real OMS context and see which mutating actions would be held.

Workforce gives teams the operating surface for those agents. Users can start conversations with active agents, monitor whether a conversation is running, pending approval, or in error, inspect the tool calls behind the assistant's answer, and approve or reject sensitive actions without leaving the thread.

The framework is powered by `moqui-ai`, a Moqui-native agent component that treats OMS services as agent tools. This means HotWax agents can move beyond chat: they can read OMS data, call approved business services, preserve run history, request human approval for mutating actions, and leave a traceable record of what happened. The result is an agent system designed for retail operations, where inventory, fulfillment, routing, customer communication, and integrations all require control rather than novelty.

## Problem

Retail operations teams are full of high-value "I wish the system could just..." workflows: summarize orders stuck overnight, investigate fulfillment exceptions, prepare customer-service answers, monitor pre-orders, flag risky inventory situations, or explain why a job failed.

Traditional automation turns each of those ideas into a software project. A business expert explains the workflow to IT, a developer maps the process into code, the team waits for release capacity, and the final result often becomes a narrow rule that is hard to adapt when the business changes.

Generic AI chat tools do not solve this either. They can answer questions, but they usually cannot use real OMS services, respect OMS permissions, distinguish read-only from mutating actions, ask for approval before doing risky work, or provide the audit trail an enterprise team needs.

That gap is especially painful in OMS workflows because the work is both urgent and high stakes. A customer-service representative needs to know why an order is stuck while the customer is waiting. A fulfillment lead needs to know which exception needs action before the shipping window closes. An operations owner needs to understand whether a failed job delayed an integration. These are exactly the moments where teams need software that can investigate, explain, and prepare action without bypassing controls.

Teams need a way to turn those operational ideas into safe, inspectable software without turning every improvement into a custom development project.

## Solution

HotWax Agent Composer closes that gap by making agent creation an OMS-native workflow.

In Composer, a user builds an agent the same way an operations team thinks about work: define the goal, choose the capabilities, decide the guardrails, test the behavior, and activate it for the team. Composer exposes the important agent controls directly: name, instructions, model, reasoning effort, tool selection, read-only versus mutating capability filters, per-tool auto-approval, preview, save, and activation.

In Workforce, active agents become part of the operations floor. Users do not have to guess what an agent is doing. They can see the conversation, the status, the assistant response, the tool calls, the arguments used, and the pending approval request when the agent reaches a sensitive step. That makes the agent experience operationally legible instead of turning AI into another black box.

Under the UI, `moqui-ai` stores agents, tools, tool grants, conversations, runs, run steps, tool calls, approval requests, model choices, cost data, and memory facts as Moqui entities. Agent tools wrap Moqui services, so the agent framework can use the same service layer that already powers OMS workflows instead of inventing a separate automation stack.

This gives HotWax a practical path for agentic OMS workflows. An order agent can summarize active order tasks and recommend the next customer-service action. A job agent can explain failed system messages and point to the likely owner. An inventory agent can inspect ATP, reservations, thresholds, and facility availability. A routing agent can explain why an order was assigned or excluded. A product agent can help review setup, associations, substitutions, and missing data. Each agent can start narrow, use explicitly granted tools, and expand as the retailer curates more capabilities.

The important design choice is that agents are composed from governed tools, not given unrestricted access. Read-only capabilities can be used for investigation and explanation. Mutating capabilities can be held for approval. Preview mode can show what would happen before activation. Run history can show what the agent tried, what it called, where it stopped, and who approved the next step.

## Getting Started

Agent Composer is available from the Agents section in the Company app.

Users start in Composer by creating a draft agent, selecting the model, adding instructions, choosing capabilities from the tool catalog, and previewing the agent. Once the draft behaves correctly, the user activates it and runs it from Workforce.

The best first rollout is a focused agent for a repeatable operational problem, such as order exception review, job failure triage, fulfillment investigation, customer-service order explanation, pre-order risk monitoring, or inventory availability review. Teams should begin with a small tool set, clear approval rules, and one accountable owner before expanding to broader workflows.

## Testimony

**Internal quote:** "The most important thing about Agent Composer is that it does not treat AI as a sidecar chatbot. It turns OMS services into governed tools and gives operations teams a way to compose useful agents from the same workflows HotWax already trusts."

**Customer quote:** "We have dozens of operational checks that are obvious to our team but too small to justify a full development project. Agent Composer lets us turn those ideas into working assistants, test them against real OMS data, and keep humans in control when the action is sensitive."

**Customer quote:** "The approval workflow matters. I want an agent to find the exception and prepare the next step, but I still want a person to approve actions like cancellation, rerouting, or customer-impacting changes."

**Customer quote:** "This is different from asking a chatbot what might be wrong. The agent can inspect the order, show the tools it used, explain the blocker, and then ask us before taking the next step."

## Call to Action

Retailers using HotWax OMS can start by building focused agents for order exceptions, fulfillment investigation, customer-service support, job monitoring, pre-order risk, or inventory review. The first production agents should use a small set of trusted tools, clear approval rules, and well-defined team ownership.

As more OMS capabilities are exposed as tools, retailers can expand from single-purpose assistants to a governed agent workforce that helps teams investigate, decide, and act across the order lifecycle.

## FAQ

### What is Agent Composer?

Agent Composer is the HotWax UI for creating OMS agents.

It lets a user define the agent's purpose, improve the agent instructions, select a model, set reasoning effort, choose approved tools from the capability catalog, configure approval behavior, preview the draft, and activate the agent for use by the team.

### What is Workforce?

Workforce is the operating surface for active agents.

It shows active agent conversations, supports new conversations, displays conversation status, renders assistant messages, shows tool-call details, and gives users inline controls to allow or deny pending tool-call requests.

Composer is where agents are built. Workforce is where they work.

### What is an OMS agent?

An OMS agent is an AI assistant that is grounded in HotWax OMS concepts and can use approved OMS capabilities as tools.

Instead of only answering a question from text, an OMS agent can call services that search, calculate, summarize, inspect, or update operational records. It can work across orders, inventory, fulfillment, routing, jobs, customer-service workflows, and other OMS domains as those capabilities are exposed in the tool catalog.

### How is this different from a chatbot?

A chatbot primarily responds with text.

An OMS agent can take steps. It can inspect data, call approved services, request a decision before a sensitive action, continue after approval, and leave an execution record.

The difference is not the chat interface. The difference is that Agent Composer connects the chat experience to governed OMS capabilities.

### How does Agent Composer know what an agent is allowed to do?

Agents do not receive open-ended system access.

They receive explicit tool grants. A tool is an approved capability in the `AiTool` catalog, backed by a Moqui service. Composer only grants active, exposable tools, and each grant can have its own approval behavior.

This keeps agent design bounded. If an agent needs a capability that does not exist, the framework can record a capability request instead of pretending the tool is available.

### What does the tool catalog contain?

The tool catalog contains business capabilities exposed to agents.

Each tool has a name, description, service name, status, effect, and approval behavior. Tool effects distinguish read-only actions from mutating actions. Composer's tool picker lets users search the catalog and filter by all tools, read-only tools, or mutating tools.

The catalog can include OMS capabilities such as finding orders, summarizing fulfillment exceptions, checking inventory, investigating jobs, reviewing pre-orders, preparing customer-service context, or performing controlled operational updates.

### How are tools implemented technically?

Tools wrap Moqui services.

The `moqui-ai` component stores each tool in `AiTool` and maps it to a service name. When an agent calls a tool, the framework resolves the tool, builds the tool schema from the service definition, and executes the service through Moqui's service layer.

This is important because HotWax does not need a separate automation runtime for agents. Agents can use the same service contracts that already enforce OMS behavior.

### How are permissions handled?

Tool calls execute through Moqui services, so normal Moqui authentication and service authorization remain part of the execution path.

The framework also adds an agent-specific control layer: only active and exposable tools can be granted, tools can require approval, per-agent grants can override approval behavior, and pending tool-call decisions are restricted to the conversation owner or an authorized operator/admin.

Production rollout should continue hardening dedicated agent-administration permissions so agent creation, activation, tool authoring, and cross-user approval queues are governed by explicit operational roles.

### How does HotWax prevent agents from taking unsafe actions?

The framework uses several safety layers:

- Agents only receive explicitly granted tools.
- Tools are classified as read-only or mutating.
- Tool grants can require human approval.
- Preview mode holds mutating calls so the user can see what the agent would do without executing the change.
- Activation checks that granted tools are still active and exposable.
- Agent runs are bounded by iteration, token, and tool-call limits.
- Tool-call requests are recorded and require approval or rejection before execution resumes.
- Tool calls use the Moqui service layer instead of bypassing business logic.

This makes the framework suitable for operational agents, where a wrong update can affect inventory, customer communication, fulfillment, or financial outcomes.

### What happens in preview mode?

Preview mode lets a user test a draft agent before activating it.

Read-only tools can run against real data so the preview is meaningful. Mutating tools are held and displayed as would-be tool calls. The user can see the assistant response and inspect which tools the agent tried to use before deciding whether the instructions, tools, or approval rules need refinement.

### What happens when an active agent requests a sensitive action?

When an agent reaches a tool call that requires approval, the run is suspended and a pending tool-call request is created.

Workforce shows the conversation as pending and displays a permission card with the tool name and decision controls. A user can allow or deny the request. Once all pending requests for that run are decided, the agent resumes with the decision context.

### Why is human-in-the-loop approval important for OMS?

OMS actions often affect real customer promises.

An agent that summarizes exceptions is low risk. An agent that cancels an item, changes a routing decision, releases an order, edits fulfillment data, or triggers a downstream integration needs a different level of control.

Human-in-the-loop approval lets HotWax use agents for high-value operational work without pretending every action should be fully autonomous from day one.

### What kind of agents should retailers build first?

The best first agents should be narrow, high-frequency, and easy to verify.

Examples include:

- Order exception summarizer: reviews stuck orders and explains why they are not moving.
- Fulfillment investigator: checks shipment, facility, and routing context for a specific order.
- Job monitor: summarizes failed jobs and suggests the likely owner or next action.
- Pre-order risk watcher: reviews future inventory and flags orders that may miss their promise date.
- Customer-service order assistant: prepares a support-ready explanation of order status and blockers.
- Inventory review assistant: compares availability, reservation, and fulfillment constraints before an operator takes action.

These agents create value quickly while keeping approval scope manageable.

### How does this fit with Order Manager?

Order Manager is where many agent-driven workflows can become visible to operators.

An order-focused agent can help customer service understand why an order is stuck, explain active tasks, summarize fulfillment risk, identify missing information, prepare a note, or recommend a next step. When an action is required, the agent can hand the operator a decision instead of hiding the work in a background process.

This is especially relevant for task-based order workflows, substitution resolution, fraud review, address correction, routing investigation, and shipment exception handling.

### How does this fit with Job Manager?

Job Manager exposes the message flow and operational health of system jobs.

An agent can help users interpret failures, summarize recent job behavior, identify affected integrations, prepare a troubleshooting brief, or recommend which operational owner should review the issue. As Job Manager V2 adds more management capabilities, agents can become a guided operating layer for job creation, category management, SOLR integration management, NiFi workflows, and ALC management.

### How does this fit with Products, Inventory, and Routing?

Products, Inventory, and Routing provide the data and decision context agents need.

A product agent can explain product setup, associations, kit components, substitutions, or missing data. An inventory agent can inspect ATP, reservations, thresholds, safety stock, and facility availability. A routing agent can explain why an order was brokered a certain way, why a facility was excluded, or what conditions are blocking fulfillment.

The long-term opportunity is to let agents reason across these domains while still using governed OMS tools for the actual work.

### How does this support customer service?

Customer-service teams often need a clear answer faster than they need another screen.

An OMS agent can gather order status, task context, fulfillment state, payment or fraud notes, substitution decisions, and shipment history into a concise explanation. It can draft a response, recommend the next action, and escalate the right pending task without forcing the user to inspect multiple apps manually.

Because tool calls are traceable, the user can see the operational basis for the answer instead of treating it as a black box.

### How does this support operations leaders?

Operations leaders can use agents to reduce the distance between policy and execution.

Instead of waiting for every exception report or workflow assistant to be built as a custom screen, teams can compose focused agents around recurring questions: what failed overnight, what is aging, what is blocked, what needs approval, what changed, and which orders need intervention.

This makes HotWax more adaptable without losing the governance expected from an enterprise OMS.

### How does the framework handle observability?

`moqui-ai` records the execution trail of an agent run.

It stores conversations, messages, runs, run steps, tool calls, tool-call requests, statuses, errors, token usage, and estimated cost. Workforce surfaces the conversation and tool-call context to operators, while the backend keeps the structured run history for audit and future analysis.

Enterprise agent systems need this level of traceability because teams need to understand not only what the agent answered, but what it tried to do and why it stopped.

### Does the framework support model choice?

Yes.

Composer lets users select from available model options and set reasoning effort. The backend supports provider/model configuration for agents, model listing, and model fallback patterns so deployments can tune agents by cost, latency, reliability, and reasoning requirements.

### Does the framework support memory and business vocabulary?

Yes.

`moqui-ai` includes conversation records, conversation facts, context strategies, knowledge topics, glossary terms, synonyms, and domain terminology services. The included OMS domain primer grounds terms such as ATP, online ATP, reserved inventory, safety stock, facilities, brokering, split shipment, ASN, backorder, and pre-order.

This matters because an OMS agent should speak the retailer's operational language, not generic AI language.

### What is the Composer Assistant?

The Composer Assistant is an out-of-the-box agent that helps users build other agents.

It can search the capability catalog, describe tools, list domain terms, propose names, draft an agent, grant capabilities, configure guardrails, preview the agent, activate the agent after approval, and record capability gaps when the desired tool does not exist.

This is the strongest expression of the framework: the agent builder is itself an agent built on the same registry and approval model.

### Why is this important strategically?

Agent Composer can turn HotWax OMS from a set of operational apps into a governed agentic operations platform.

Retailers already rely on HotWax to coordinate orders, inventory, routing, fulfillment, integrations, jobs, and exceptions. Agent Composer adds a new layer where teams can create role-specific assistants for those workflows without asking every idea to become a traditional software project.

The strategic value is not "AI in the product." The value is controlled operational leverage: more of the business can be expressed as governed agents that understand OMS data and act through OMS services.

### What do enterprise teams expect from agents in business systems?

Enterprise teams increasingly expect agents to do more than answer questions. They expect agents to connect to business data, use tools, complete multi-step workflows, ask for approval when needed, explain actions, leave audit trails, and run within permission boundaries.

They also expect observability. Teams need to know which tool was called, with which arguments, under which user, what the outcome was, and what the agent did next.

Agent Composer is designed around those expectations instead of treating agents as unmanaged chat windows.

### What should not be automated immediately?

Retailers should avoid making early agents fully autonomous for customer-impacting or financially sensitive changes.

Examples include order cancellation, refund creation, payment action, customer notification, inventory adjustment, routing override, or downstream release to a fulfillment partner. These are excellent agent-assisted workflows, but they should begin with preview, recommendation, and approval before moving toward selective auto-approval.

### What is in the first scope?

The first scope includes:

- Composer navigation in the Company app.
- Agent name, instruction, model, and reasoning effort setup.
- Instruction enhancement.
- Tool catalog search and filtering.
- Read-only versus mutating tool classification.
- Per-tool auto-approval configuration.
- Draft save and activation.
- Agent preview with held mutating calls.
- Workforce navigation in the Company app.
- Active agent selection.
- Conversation list and conversation detail.
- Conversation status filtering.
- Message sending.
- Tool-call display.
- Pending tool approval and rejection.
- Backend agent, tool, grant, conversation, run, run-step, tool-call, approval, model, memory, knowledge, and glossary entities.
- REST endpoints for agents, tools, models, instruction enhancement, preview, activation, conversations, messages, and tool-call decisions.

### What are the key roadmap items?

The next roadmap should focus on production governance and OMS depth:

- Dedicated agent administration permissions.
- Role-based tool authoring, agent activation, and approval queues.
- Curated OMS tool packs for order management, inventory, routing, products, jobs, integrations, returns, and customer service.
- Agent templates for common retail operations workflows.
- Scheduled and event-triggered agents, not only user-started conversations.
- Evaluation suites for testing agents before production rollout.
- Policy-driven auto-approval for low-risk actions.
- Richer run observability, cost dashboards, and business outcome reporting.
- Recovery patterns for failed tool calls and partial workflows.
- Integration with external task-management and customer-service tools.
- Multi-agent handoff for workflows that cross operational ownership boundaries.

### What is the long-term vision?

The long-term vision is for HotWax OMS to become the trusted execution layer for retail operations agents.

Business users should be able to describe a workflow, compose an agent from approved OMS capabilities, test it safely, deploy it to the right team, and continuously improve it as the business changes. Developers and solution teams should focus on curating reliable tools, policies, templates, and integrations rather than hardcoding every operational assistant from scratch.

In that model, agents do not replace the OMS. They make the OMS easier to operate, easier to extend, and easier to connect to the people responsible for keeping orders moving.
