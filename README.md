# AI-Assisted WhatsApp Customer Support Workflow

A practical AI and no-code automation project designed to receive inbound WhatsApp messages, preserve customer context, identify pending requests, route actions and support AI-assisted responses.

## Project Overview

I designed and tested this workflow to explore how generative AI could operate within a real customer-support process rather than treating each message as an isolated prompt.

The system connected WhatsApp Business Cloud with Make and a persistent data layer. Incoming messages were captured, normalized and stored with contextual information so an AI assistant could later retrieve pending requests, reason about the appropriate next action and, when explicitly authorized, send a response and update the status of the request.

## Core Problems Addressed

- Capture inbound customer messages automatically
- Preserve message and customer context
- Track unresolved versus resolved requests
- Retrieve pending messages on demand
- Give an AI agent access to operational tools
- Draft or send responses under defined rules
- Update the underlying record after resolution
- Troubleshoot failures across integrations

## Technologies

- WhatsApp Business Cloud
- Make
- Make AI Agent
- Make Data Store
- Generative AI / GPT
- Telegram as an assistant interface during testing
- No-code workflow automation

## Documentation

- [System Architecture](docs/architecture.md)
- [Workflow Logic](docs/workflow-logic.md)
- [Lessons Learned](docs/lessons-learned.md)
