# Workflow Overview

## Main message path

```text
WhatsApp Trigger
      |
    Switch
      |
  Edit Fields
      |
   AI Agent
      |
      IF — contains STAFF_ESCALATION?
      |                         |
     True                      False
      |                         |
     Gmail                WhatsApp Send
      |
 WhatsApp Send
```

The AI Agent is connected to an OpenAI Chat Model, Simple Memory, and Google Sheets tools for product lookup, FAQ lookup, and order saving.

## Routine enquiry

The customer sends a text message. The workflow extracts the message, passes it to the AI Agent, and the agent consults the product catalogue or FAQ sheet where appropriate. The response is sent to the customer over WhatsApp.

## Order handling

The agent gathers the required order details, confirms the order summary with the customer, and uses the Google Sheets append-row tool after the customer explicitly approves the order. The order workbook is used as the demonstrator's record store.

## Human escalation

The agent must escalate immediately if the customer requests a human/staff member or reports a complaint. The response includes the internal marker `STAFF_ESCALATION`, which causes the IF node to route the execution through Gmail before the WhatsApp acknowledgement is sent.

The WhatsApp Send Message node on the escalation route should reference the AI Agent output directly and remove the internal marker:

```javascript
{{ $('AI Agent').first().json.output.replace('STAFF_ESCALATION', '').trim() }}
```

Configure the Gmail recipient as the business's real support inbox before deployment. Confirm that the notification succeeds and the customer receives the acknowledgement during end-to-end testing.

## Credential boundaries

The WhatsApp Trigger's Meta app OAuth credentials are separate from the WhatsApp API access token used by the send-message node. Google Sheets and Gmail also require their own OAuth connections. Credentials are intentionally not included in this repository.
