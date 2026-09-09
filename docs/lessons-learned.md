# Lessons Learned

## 1. AI memory is not operational state

One of the most important lessons from this project was the difference between conversational memory and operational state.

A support workflow should not assume that an AI model will reliably retain every previous interaction. Persisting customer requests and their status outside the model creates a more dependable system of record.

## 2. Context works better when it is structured

Giving an AI more text is not necessarily the same as giving it better context.

The workflow became more predictable when customer messages were represented through explicit fields such as:

- message ID;
- sender/customer identifier;
- timestamp;
- message text;
- direction;
- resolution status.

Structured context makes retrieval, routing, and decision-making easier to inspect and troubleshoot.

## 3. Tools should have narrow responsibilities

Instead of creating one automation that tries to do everything, I separated responsibilities across scenarios and tools.

Examples include:

- receiving messages;
- storing records;
- retrieving pending messages;
- sending a response;
- updating request status.

This modular approach reduced ambiguity and made failures easier to isolate.

## 4. AI reasoning and execution should be separated

Drafting a response and sending a response are different operations.

The prototype reinforced the value of keeping consequential external actions behind explicit authorization when appropriate. This creates a practical **human-in-the-loop** safeguard while still allowing AI to interpret context and prepare the next action.

## 5. Integration work depends on real payloads and edge cases

The conceptual workflow was straightforward. The difficult part was making the systems communicate reliably.

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

Message IDs and record keys are critical because they allow the workflow to refer back to the correct customer interaction.

Without stable identifiers, retrieving, updating, or resolving the original request becomes fragile.

## 7. Time-zone normalization matters

Customer-support operations often involve systems that represent time differently.

The project required timestamp formatting and timezone handling so stored records would remain understandable and operationally useful across tools.

## 8. AI becomes more useful when connected to operations

The most valuable part of the project was not simply generating text with AI.

It was giving the AI controlled access to operational capabilities:

- retrieving information;
- interpreting state;
- preparing an action;
- executing an authorized action;
- updating the system afterward.

That changes the role of the model from a standalone chatbot into one component of a broader workflow.

## What I Would Improve Next

For a production implementation I would add:

- stronger authentication and permissions;
- CRM integration;
- customer identity resolution;
- richer request states;
- SLA rules;
- escalation logic;
- centralized error handling;
- retries and fallback paths;
- structured observability and alerting;
- knowledge-base retrieval;
- automated intent and priority classification;
- metrics for response time and resolution time;
- privacy and data-retention controls.

## Relevance to Customer Success

Although this prototype focuses on support operations, the same architectural principles can support Customer Success workflows.

Persistent context, structured state, task routing, and AI-assisted next actions can be applied to:

- onboarding and implementation;
- adoption monitoring;
- customer-risk detection;
- structured follow-up;
- renewals;
- expansion opportunities;
- QBR preparation.

The broader lesson is that AI is most useful in Customer Success when it is connected to reliable customer context, clear process ownership, measurable states, and controlled actions.
