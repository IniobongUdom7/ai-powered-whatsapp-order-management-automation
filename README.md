# WhatsApp AI Customer Support & Order Management Automation

An AI-powered WhatsApp customer support and order management system built with **n8n**. It helps a food business respond to routine product enquiries, collect and record customer orders, and route complaints or requests for a staff member to human support.

> **Project case study:** Denco Food Mart  
> **Status:** Core text-based support, order handling, and human-escalation flow tested. Voice-note processing is not currently part of the working feature set.

## Business problem

Food businesses can receive repetitive questions about products and orders through WhatsApp. When enquiries and complaints are handled manually, response times can increase and order details may be harder to track.

This workflow demonstrates how an AI-assisted WhatsApp process can handle routine enquiries while preserving a route to human support for situations that need staff attention.

## What it does

- **WhatsApp customer support:** receives incoming text messages and sends replies.
- **Product enquiries:** uses a Google Sheets product catalogue as a source for product information.
- **FAQs:** retrieves business answers from a Google Sheets FAQ sheet.
- **Order collection:** guides customers through order details and requests confirmation before saving.
- **Order records:** appends confirmed orders to a Google Sheets order-management workbook.
- **Human escalation:** detects complaints and explicit requests to speak with staff, sends an email notification, and returns an acknowledgement to the customer on WhatsApp.
- **Conversation context:** uses n8n Simple Memory to maintain short-term context during a conversation.

## Workflow architecture

1. **WhatsApp Trigger** receives the customer's message.
2. **Switch** routes messages by type. The text route is the working path documented here.
3. **Edit Fields** prepares the incoming customer message.
4. **AI Agent** interprets the request and can use the connected tools:
   - OpenAI Chat Model
   - Simple Memory
   - Google Sheets product lookup
   - Google Sheets FAQ lookup
   - Google Sheets order-saving tool
5. **IF** checks whether the AI response contains the escalation marker.
6. **Escalation route:** Gmail sends a support notification, then WhatsApp sends the customer acknowledgement.
7. **Standard route:** WhatsApp sends the AI response directly to the customer.

See [Workflow overview](docs/workflow-overview.md) for more detail.

## Technology

- n8n — workflow orchestration
- WhatsApp Cloud API — customer messaging
- OpenAI Chat Model through n8n AI Gateway — language understanding and response generation
- Google Sheets — product catalogue, FAQs, and order records
- Gmail — human-escalation notifications

## Google Sheets structure

The project workbook is organised around these tabs:

| Sheet | Purpose |
|---|---|
| Products | Product names, prices, and availability information used by the agent |
| FAQs | Approved answers to common customer questions |
| Orders | Confirmed customer orders and their status |
| Customers | Customer information, where required for order management |

Do not publish real customer data. Use a sanitised demo copy before adding the workbook to a public repository.

## Human escalation behaviour

The agent should immediately escalate when a customer:
- explicitly asks to speak with staff or a human representative; or
- reports a complaint such as damaged goods, missing items, or an incorrect order.

The workflow uses the internal marker `STAFF_ESCALATION` to route the response to the Gmail notification branch. The marker is removed from the WhatsApp message sent to the customer.

**Deployment note:** configure the Gmail recipient as the business's approved support inbox before using this with real customers. The email notification and WhatsApp acknowledgement should be tested together.

## Demo and project assets

- **Loom walkthrough:** link to be added.
- **Workflow screenshot:** to be added under `assets/`.
- **Escalation email screenshot:** to be added under `assets/`.
- **Sanitised Google Sheets workbook:** to be added under `assets/` after review.

## Setup overview

This repository documents the workflow; it does not contain live credentials.

1. Import the exported n8n workflow JSON into your n8n workspace.
2. Reconnect the required credentials in your own environment:
   - WhatsApp Trigger OAuth2 / Meta app credentials
   - WhatsApp API access token and WhatsApp Business Account ID
   - OpenAI / n8n AI Gateway credential
   - Google Sheets OAuth2
   - Gmail OAuth2
3. Connect the Google Sheets nodes to your own workbook and confirm the sheet/tab names and columns.
4. Set the Gmail escalation recipient to the correct business support address.
5. Test routine enquiries, order confirmation and saving, explicit staff requests, and complaints before publishing.
6. Keep API tokens, OAuth secrets, customer phone numbers, and private order data out of Git.

## Security

- Never commit access tokens, app secrets, OAuth credentials, or exported credential data.
- Remove personal customer information from screenshots and sample spreadsheets.
- Use a separate test number and test dataset while validating the workflow.
- Rotate any credential that is accidentally exposed.

## Current scope and limitations

- The documented, tested path is text-based WhatsApp support.
- Voice-note transcription/download nodes are present in the workflow draft but are not confirmed working; voice support is therefore not claimed as a feature.
- Payment verification, inventory synchronisation, delivery tracking, and a dedicated staff dashboard are outside the current implementation.
- This is a demonstrator that requires business-specific configuration and testing before production use.

## Author

**Iniobong Udom**  
GitHub: [@IniobongUdom7](https://github.com/IniobongUdom7)

