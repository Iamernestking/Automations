# Cacao Co. Customer Support Chatbot (n8n)

An AI customer support chatbot for **Cacao Co.**, a cocoa merchant. The assistant, named **Vision**, answers customer questions, captures leads into Google Sheets, and books virtual meetings on Google Calendar with a Google Meet link.

![Cacao Co. chatbot workflow](Customer%20assisstance%20on%20n8n.png)

## What it does

- **Answers questions** about Cacao Co. products (cocoa beans, nibs, butter, powder), origins, minimum orders, and export regions. If it does not know something, it offers to connect the customer to a person instead of guessing.
- **Captures leads** by collecting the customer's name, phone number, and email at the start of the chat, then saving them to a Google Sheet.
- **Scores leads** automatically as "Serious Lead" or "Unserious Lead" based on signals like price questions, bulk orders, quantities, timelines, export enquiries, or meeting requests.
- **Books meetings** (30 minutes, Google Meet) only after collecting the customer's name, email, and a valid date and time. Meetings are Monday to Friday, 9am to 5pm West Africa Time. Weekends and out-of-hours requests are redirected to the nearest available slot.

## How it works

| Component | Purpose |
|---|---|
| Chat Trigger | Public chat widget that receives customer messages |
| AI Agent | Handles the conversation using the system prompt |
| Groq Chat Model | Language model (`openai/gpt-oss-20b`) |
| Simple Memory | Remembers the last 10 messages of the conversation |
| Append_leads | Google Sheets tool that saves each lead |
| Book_Meetings | Google Calendar tool that creates the Meet event |

## Requirements

- An [n8n](https://n8n.io) instance (cloud or self-hosted)
- A [Groq](https://groq.com) API key
- A Google account connected to n8n via OAuth (Google Sheets and Google Calendar)

## Setup

1. In n8n, create a new workflow and choose **Import from File**, then select `workflow.json`.
2. Reconnect your credentials on these nodes:
   - **Groq Chat Model1**: your Groq API credential
   - **Append_leads**: your Google Sheets OAuth credential
   - **Book_Meetings**: your Google Calendar OAuth credential
3. Create a Google Sheet with a tab named `leads` and these column headers in row 1:
   `Date`, `Name`, `Phone`, `Email`, `Enquiry`, `Category`
4. In the **Append_leads** node, select your own sheet and the `leads` tab.
5. In the **Book_Meetings** node, select the calendar you want meetings created on.
6. Edit the business details in the **AI Agent** system prompt (products, minimum orders, regions, contact email) to match your business.
7. Click **Test workflow** and chat with the bot to confirm it works.
8. When satisfied, toggle the workflow to **Active** to go live.

## Customising

- **Business hours and time zone:** change the scheduling rules and the `Africa/Lagos` time zone in the AI Agent system prompt.
- **Meeting length:** adjust the `minutes: 30` value in the **Book_Meetings** node's End field.
- **Lead rules:** edit the "Capturing Leads" section of the system prompt to change what counts as a serious lead.
- **Welcome message and chat title:** edit them in the **When chat message received** node.

## Files

- `workflow.json`: the exportable n8n workflow
- `README.md`: this file

## Notes

- Credentials are not included in the export. You must connect your own.
- Do not commit API keys or personal data to this repository.
