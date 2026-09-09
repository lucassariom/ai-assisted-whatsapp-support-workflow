# Workflow Logic

## Purpose

The workflow was designed to answer a practical operational question:

> How can an AI assistant help manage incoming customer messages without losing context or acting blindly?

The resulting logic combines event capture, persistent state, tool-based retrieval, AI reasoning and controlled execution.

## Workflow 1 - Receive an Inbound Message

### Trigger

A customer sends a WhatsApp message.

### Process

1. WhatsApp Business Cloud produces an event.
2. Make captures the event through Watch Events.
3. The message collection is processed through an Iterator.
4. Relevant fields are extracted.
5. The message is stored in the persistent Data Store.
6. The initial request status is recorded.

Example state:

```text
direction: received
status: pending
Workflow 2 - Retrieve Pending Messages

When the assistant needs to determine what still requires attention:

The AI Agent invokes the pending-message tool.
A dedicated Make scenario searches the Data Store.
Records matching the pending condition are retrieved.
Structured results are returned to the AI Agent.
The AI interprets those records and explains what requires action.

This avoids relying on the AI to remember every previous customer interaction.

Workflow 3 - Prepare a Response

Once a pending message has been identified, the AI can:

Review the stored message.
Interpret the customer's intent.
Consider available context.
Draft an appropriate response.
Present the response before execution when authorization is required.

This separates reasoning from external action.

Workflow 4 - Send a Message

The response tool requires operational fields such as:

sender identity;
destination;
message type;
body.

The AI can populate these fields from the available context.

During testing, sending was intentionally placed behind an approval step so that the assistant did not autonomously send consequential messages without confirmation.

Workflow 5 - Resolve the Request

After a successful response:

The original pending record is located.
Its state is updated.
status changes from pending to resolved.
Future pending-message queries no longer treat it as unresolved.
State Model

A simple state model was enough for the prototype:

Inbound request
     ↓
  pending
     ↓
response prepared
     ↓
response sent
     ↓
  resolved

A production version could expand this into states such as:

new
triaged
waiting_for_customer
waiting_for_internal_team
ready_to_send
resolved
escalated
Example

A test inbound record contained:

text: Teste banco 001
direction: received
status: pending

The assistant successfully retrieved the record through its operational tool.

After the workflow processed the request, the corresponding record could be marked as resolved.

Potential Production Extensions

The prototype could be extended with:

intent classification;
urgency classification;
customer identification through CRM data;
SLA tracking;
automatic escalation;
sentiment analysis;
knowledge-base retrieval;
multilingual responses;
Zendesk or HubSpot integration;
human-agent handoff;
CSAT collection;
analytics dashboards;
structured audit logs.
