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
4. Upload the `n8n_chatbot_workflow.json` file.

### 2. Configure Credentials
You will need to set up credentials for the following nodes:

#### Google Gemini Chat Model
- Click on the **Google Gemini Chat Model** node.
- Add or select your **Google Gemini(PaLM) API** credentials.

#### Google Sheets (add data)
- Click on the **add data** node.
- Add or select your **Google Sheets OAuth2 API** credentials.
- Update the `Document ID` and `Sheet Name` with your own Google Sheet details.

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

## Testing the Workflow
1. Activate the workflow in n8n.
2. Send a POST request to the Webhook URL with a JSON body:
   ```json
   {
     "message": "Hello",
     "sessionId": "test-session-123"
   }
   ```
3. Verify the AI response and check if data is correctly saved to Google Sheets upon completion.
