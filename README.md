# AI Chatbot n8n Workflow Template

A comprehensive n8n workflow for an AI-powered customer service chatbot. This workflow captures leads, saves them to Google Sheets, and sends automated email confirmations.

## Features
- **AI Agent:** Powered by Google Gemini, capable of natural conversation and lead qualification.
- **Data Collection:** Automatically extracts Name, Email, WhatsApp, Service Interest, and Budget.
- **Google Sheets Integration:** Saves qualified leads to a central spreadsheet.
- **Gmail Notifications:** Sends a confirmation email to the user and an alert to the admin.

## Prerequisites
- An [n8n](https://n8n.io/) instance (Cloud or Self-hosted).
- A Google Cloud Console project with:
  - Gmail API enabled.
  - Google Sheets API enabled.
- A Google Gemini API Key.

## Setup Instructions

### 1. Import the Workflow
1. Open your n8n instance.
2. Click on **Workflows** > **Add Workflow**.
3. Click the menu (three dots) in the top right and select **Import from File**.
4. Upload [n8n_chatbot_workflow.json](./n8n_chatbot_workflow.json).

### 2. Configure Credentials
You will need to set up credentials for the following nodes:

#### Google Gemini Chat Model
- Click on the **Google Gemini Chat Model** node.
- Add or select your **Google Gemini(PaLM) API** credentials.

#### Google Sheets (add data)
- Click on the **add data** node.
- Add or select your **Google Sheets OAuth2 API** credentials.
- Update the `Document ID` and `Sheet Name` with your own Google Sheet details.
- Create a header row with `Name`, `Email`, `Whatsapp`, `Service`, `Budget`, and `Contact Date and Time`. Match those spellings to the column mappings in the node.

#### Gmail (send confirmation msg & inform admin)
- Click on the **send confirmation msg** node.
- Add or select your **Gmail OAuth2** credentials.
- Do the same for the **inform admin** node.

### 3. Customize the AI Agent
- Click on the **AI Agent** node.
- Update the `System Message` to match your company's persona, services, and conversation flow.
- **IMPORTANT:** Do not modify the `JSON Output Format` section at the bottom of the system message, as the workflow relies on this structure to parse data.

### 4. Update Placeholders
- In the **inform admin** node, update the `To Email` field with your admin email address.
- In both Gmail nodes, review and update the email body templates.

## Test the webhook

1. Point the Sheets and Gmail nodes at test resources you control before sending a complete inquiry.
2. Open the **Webhook** node, select **Test URL**, and click **Listen for test event**.
3. Set `WEBHOOK_URL` in your shell to the Test URL, then send a request.

```bash
curl --fail-with-body --request POST "$WEBHOOK_URL" \
  --header 'Content-Type: application/json' \
  --data '{"message":"Hello, I need help with a website redesign.","sessionId":"test-session-123"}'
```

Check the JSON response for `status`, `response`, and `sessionId`. Start a new test listener for each request as needed. Keep the same request `sessionId` for follow-up messages in the same conversation. Use a fresh ID to start a new one.

Once you supply all the requested lead details, check that the workflow adds a row to the test sheet. Use test email addresses you control to check the confirmation and admin messages.

When you are ready to publish, configure authentication on the **Webhook** node for your intended client, publish the workflow, and switch the client to the **Production URL**. In production, use the **Executions** tab to inspect runs. [n8n Webhook documentation](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)
