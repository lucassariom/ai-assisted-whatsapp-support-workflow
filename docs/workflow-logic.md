# Workflow Logic

## Purpose

The workflow was designed to answer a practical operational question:

> How can an AI assistant help manage incoming customer messages without losing context or acting blindly?

The resulting logic combines event capture, persistent state, tool-based retrieval, AI reasoning, controlled execution, and resolution tracking.

## Workflow 1 — Receive an Inbound Message

### Trigger

A customer sends a WhatsApp message.

### Process

1. WhatsApp Business Cloud produces an event.
2. Make captures the event through **Watch Events**.
3. The message collection is processed through an **Iterator**.
4. Relevant fields are extracted and normalized.
5. The message is written to the persistent Data Store.
6. The initial request state is recorded.

Example:

```text
direction: received
status: pending
```

## Workflow 2 — Retrieve Pending Messages

When the assistant needs to determine what still requires attention:

1. The AI Agent invokes the pending-message tool.
2. A dedicated Make scenario searches the Data Store.
3. Records matching the `pending` condition are retrieved.
4. Structured results are returned to the AI Agent.
5. The AI interprets those records and identifies what requires action.

This avoids relying on the AI to remember every previous customer interaction.

## Workflow 3 — Prepare a Response

Once a pending message has been identified, the AI can:

1. review the stored message;
2. interpret the customer's intent;
3. consider the available context;
4. draft an appropriate response;
5. present the response before execution when authorization is required.

This separates **reasoning** from **external action**.

## Workflow 4 — Send a Message

The response tool requires operational fields such as:

- sender identity;
- destination;
- message type;
- message body.

The AI can populate these fields from the available structured context.

During testing, sending was intentionally placed behind an approval step so the assistant did not autonomously send consequential messages without confirmation.

## Workflow 5 — Resolve the Request

After a successful response:

1. the original pending record is located;
2. its state is updated;
3. `status` changes from `pending` to `resolved`;
4. future pending-message queries no longer treat it as unresolved.

## State Model

A simple state model was enough for the prototype:

```text
Inbound request
     ↓
  pending
     ↓
response prepared
     ↓
response sent
     ↓
  resolved
```

A production version could expand this into states such as:

```text
new
triaged
waiting_for_customer
waiting_for_internal_team
ready_to_send
resolved
escalated
```

## Example Test

A test inbound record contained:

```text
text: Teste banco 001
direction: received
status: pending
```

The assistant successfully retrieved the record through its operational tool. After the workflow processed the request, the corresponding record could be marked as resolved.

## Potential Production Extensions

The prototype could be extended with:

- intent classification;
- urgency and priority classification;
- customer identification through CRM data;
- SLA tracking;
- automatic escalation;
- sentiment analysis;
- knowledge-base retrieval;
- multilingual responses;
- Zendesk or HubSpot integration;
- human-agent handoff;
- CSAT collection;
- analytics dashboards;
- structured audit logs.
