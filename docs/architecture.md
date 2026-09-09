# System Architecture

## Overview

This project was designed as a modular AI-assisted customer-support workflow rather than a single chatbot prompt.

The architecture separates four core responsibilities:

1. **Message intake** — capture and normalize inbound WhatsApp events.
2. **Persistent context** — store customer messages and operational state outside the AI model.
3. **AI reasoning** — retrieve structured context and determine the next appropriate action.
4. **Controlled action** — send responses and update request status under defined rules.

This separation makes the workflow easier to inspect, troubleshoot, extend, and govern.

## High-Level Architecture

```text
Customer
   ↓
WhatsApp Business Cloud
   ↓
Make — Watch Events
   ↓
Message Iterator
   ↓
Normalize message data
   ↓
Persistent Data Store
   ↓
Pending-message retrieval tool
   ↓
Make AI Agent
   ↓
Reason about context and next action
   ↓
Human authorization when required
   ↓
WhatsApp response tool
   ↓
Update request status
```

## 1. Message Intake

WhatsApp Business Cloud acts as the inbound communication channel.

Make receives WhatsApp events through the **Watch Events** module. Incoming payloads may contain multiple nested structures, so an **Iterator** is used to expose individual messages and the fields needed downstream.

Relevant data includes:

- message ID;
- sender/customer context;
- message text;
- timestamp;
- message type;
- direction.

## 2. Persistent Context Layer

A central design decision was not to make the AI depend only on conversational memory.

Inbound messages are persisted in a **Make Data Store**, creating an operational source of truth outside the model. Each record can contain fields such as:

- `message_id`;
- customer/sender identifier;
- `text`;
- `timestamp`;
- `direction`;
- `status`.

This allows the system to distinguish unresolved from resolved requests and retrieve context later without depending on the AI to remember previous interactions.

## 3. Retrieval Layer

A dedicated scenario searches the Data Store for pending records.

Instead of giving the AI unrestricted access to operational data, the agent interacts with a specific retrieval tool that returns only the information required for the task. This keeps tool behavior more predictable and makes failures easier to diagnose.

## 4. AI Reasoning Layer

The **Make AI Agent** receives structured information from operational tools and can use that context to:

- identify pending requests;
- interpret the customer's message;
- summarize the situation;
- suggest the next action;
- prepare a response;
- determine which operational tool is required.

The AI therefore works with explicit system state rather than isolated prompts.

## 5. Controlled Action Layer

Drafting a response and sending a response are intentionally separate operations.

During testing, external communication could require explicit human authorization before the send tool was executed. This creates a practical **human-in-the-loop** control point for consequential actions.

## 6. Resolution Tracking

After a request is handled, the underlying record can be updated from:

```text
pending → resolved
```

This prevents already-handled requests from repeatedly appearing in pending-message searches and provides a simple operational audit trail.

## Design Principles

### Persistence over conversational memory

Operational state should live in a persistent system rather than depend exclusively on an AI conversation.

### Modular tools

Retrieval, sending, and state updates should be separate actions with clear responsibilities.

### Human control for consequential actions

AI can interpret context and prepare actions while sensitive external actions remain approval-gated when appropriate.

### Observable state

The workflow stores message status explicitly, making it possible to inspect what is pending, what has been resolved, and where a failure occurred.

### Failure-aware design

The workflow was built iteratively by testing real integrations, inspecting module outputs, and correcting issues between WhatsApp Business Cloud, Make, the Data Store, and the AI Agent.
