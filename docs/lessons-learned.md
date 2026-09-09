# Lessons Learned

## 1. AI memory is not operational state

One of the most important lessons from this project was the difference between conversational memory and operational state.

A support workflow cannot safely assume that an AI model will remember every previous interaction.

Persisting customer requests and their status outside the model creates a much more reliable system.

## 2. Context must be structured

Giving an AI more text is not necessarily the same as giving it better context.

The workflow became more useful when customer messages were represented through structured fields such as:

- message ID;
- sender;
- timestamp;
- message text;
- direction;
- resolution status.

Structured data makes retrieval and decision-making more predictable.

## 3. Tools should have narrow responsibilities

Instead of creating one automation that tries to do everything, I separated responsibilities across scenarios and tools.

Examples include:

- receiving messages;
- storing records;
- searching pending messages;
- sending a response;
- updating request status.

This modular approach made troubleshooting considerably easier.

## 4. AI reasoning and execution should be separated

Drafting a response and sending a response are different operations.

The prototype reinforced the value of keeping consequential external actions behind explicit authorization when appropriate.

This creates a practical human-in-the-loop safeguard.

## 5. Integration work is mostly about edge cases

The conceptual workflow was straightforward.

The difficult part was making the systems communicate reliably.

During implementation I had to troubleshoot:

- WhatsApp Business Cloud connection behavior;
- event payload structure;
- iterators;
- field mappings;
- timestamps and time zones;
- message identifiers;
- Data Store keys;
- tool outputs;
- AI Agent tool calls;
- pending/resolved state changes;
- sending authorization.

This reinforced that automation work requires testing actual system behavior rather than designing only the ideal flow.

## 6. Persistent identifiers matter

Message IDs and record keys are important because they allow the workflow to refer back to the correct customer interaction.

Without reliable identifiers, updating the original request after a response becomes fragile.

## 7. Time-zone normalization matters

Customer-support operations often involve multiple systems with different timestamp formats.

The project required timestamp formatting and timezone handling so stored records would remain understandable and operationally useful.

## 8. AI is more useful when connected to operations

The most valuable part of the project was not simply generating text with AI.

It was giving the AI controlled access to useful operational capabilities:

- retrieving information;
- interpreting state;
- preparing an action;
- executing an authorized action;
- updating the system afterward.

That turns an AI model from a standalone chatbot into part of a workflow.

## What I Would Improve Next

For a production implementation I would add:

- stronger authentication and permissions;
- CRM integration;
- customer identity resolution;
- richer status states;
- SLA rules;
- escalation logic;
- centralized error handling;
- retries and fallback paths;
- structured observability;
- knowledge-base retrieval;
- automated intent and priority classification;
- metrics for response time and resolution time.

## Relevance to Customer Success

Although this prototype focuses on support operations, the same architecture can support Customer Success workflows.

Persistent context, health signals, task routing and AI-assisted next actions can be applied to:

- onboarding;
- adoption monitoring;
- risk detection;
- follow-up;
- renewals;
- expansion opportunities;
- QBR preparation.
