# System Architecture

## Overview

The project was designed as a modular AI-assisted support workflow rather than a single chatbot prompt.

The architecture separates four responsibilities:

1. Message intake
2. Persistent context
3. AI reasoning
4. Controlled action

This separation makes the workflow easier to troubleshoot, extend and govern.

## High-Level Architecture

```text
Customer
   ↓
WhatsApp Business Cloud
   ↓
Make - Watch Events
   ↓
Message Iterator
   ↓
Normalize message data
   ↓
Persistent Data Store
   ↓
Pending-message retrieval
   ↓
Make AI Agent
   ↓
Reason about context and next action
   ↓
Human authorization when required
   ↓
WhatsApp response
   ↓
Update request status
1. Message Intake

WhatsApp Business Cloud acts as the inbound communication channel.

Make receives WhatsApp events through the Watch Events module. Incoming event payloads may contain multiple structures, so an Iterator is used to expose individual messages and their relevant fields.

Relevant information includes:

Message ID
Sender context
Message text
Timestamp
Message type
Direction
2. Persistent Context Layer

A central design decision was not to make the AI depend only on its current conversation.

Inbound messages are persisted in a Make Data Store.

Each record can contain information such as:

message_id
customer / sender identifier
text
timestamp
direction
resolution status

This allows the system to distinguish between unresolved and resolved requests and retrieve operational context later.

3. Retrieval Layer

A dedicated scenario searches the Data Store for pending records.

Instead of giving the AI unrestricted access to operational data, the assistant interacts with a specific retrieval tool designed to return the information needed for the task.

This makes tool behavior more predictable and easier to troubleshoot.

4. AI Reasoning Layer

The Make AI Agent receives structured information from operational tools.

The agent can use this context to:

identify pending requests;
interpret the customer's message;
summarize the situation;
suggest the next action;
prepare a response;
determine which operational tool is required.

The AI therefore operates on structured system context rather than isolated user prompts.

5. Controlled Action Layer

Sending a message is separated from drafting or reasoning.

During testing, the system was configured so that external communication could require explicit authorization before the send tool was executed.

This creates a human-in-the-loop control point for consequential actions.

6. Resolution Tracking

After a request is handled, its underlying record can be updated from:

pending

to:

resolved

This prevents already-handled requests from repeatedly appearing in pending-message searches.

Design Principles
Persistence over conversational memory

Operational state should live in a persistent system rather than depend exclusively on an AI conversation.

Modular tools

Retrieval, sending and state updates should be separate actions with clear responsibilities.

Human control for consequential actions

AI can prepare and recommend actions while sensitive external actions can remain approval-gated.

Observable state

The workflow stores message status explicitly, making it possible to inspect what is pending, what has been resolved and where failures occurred.

Failure-aware design

The workflow was built iteratively by testing integrations, inspecting module outputs and correcting issues between WhatsApp Business Cloud, Make, the Data Store and the AI Agent.
