# SupportFlow AI - Customer Complaint Intake (n8n)

An n8n workflow that turns customer complaints into tracked support tickets. It collects complaints through a web form, stores them in Airtable, uses AI to classify them and draft a reply, and requires **human approval** before anything is sent to the customer. Staff are alerted on Telegram when attention is needed.

> Educational demonstration project. It is not an official business support channel.

## What it does

1. **Intake:** a customer submits a complaint through an n8n form (name, email, optional order number, subject, message).
2. **Ticket creation:** a ticket is created in Airtable with an auto-generated ID (`TKT-YYYYMMDD-<execution id>`), and the customer gets an acknowledgment email.
3. **AI classification:** an AI model extracts the category, priority, sentiment, a short summary, and the reason for the priority. Groq is the primary model and Gemini is the backup. If both fail, the ticket is marked **Needs Review** and staff are alerted.
4. **Urgent alerts:** high-priority tickets trigger an instant Telegram alert.
5. **AI draft reply:** the AI writes a polite draft response (80 to 130 words) that does not promise refunds or invent policies. The draft is saved to Airtable.
6. **Human approval:** an approval email (Approve / Reject buttons) goes to a reviewer.
   - **Approved:** the ticket is rechecked and the reply is emailed to the customer, then the ticket is marked **Response Sent**.
   - **Rejected:** the ticket is marked **Needs Review** and staff are alerted. A human edits the draft in Airtable and sets the ticket to **Ready to Send**. The workflow checks every minute (up to 5 times), then sends the edited reply.
7. **Resolution emails:** every minute, tickets marked **Resolved** that have a resolution message and no email sent yet are checked against Gmail (to avoid duplicates), then the customer receives the resolution email.
8. **Daily report:** every day at 18:00, a Telegram summary of unresolved tickets is sent (counts by status, high-priority ticket IDs).
9. **Failure alerts:** Telegram alerts for AI failures, email sending failures, and Airtable update failures, with instructions to avoid resending emails blindly.

## Tools used

| Tool | Purpose |
|---|---|
| n8n | Workflow automation |
| Airtable | Ticket database |
| Groq (`openai/gpt-oss-20b`) | Primary AI model |
| Google Gemini | Backup AI model |
| Gmail | Acknowledgment, approval, reply, and resolution emails |
| Telegram | Staff alerts and daily report |

## Requirements

- An [n8n](https://n8n.io) instance (cloud or self-hosted)
- An Airtable account with OAuth connected to n8n
- A Groq API key and a Google Gemini API key
- A Gmail account connected via OAuth
- A Telegram bot token and your Telegram chat ID

## Airtable setup

Create a base with a table named `Support Tickets` with these fields:

| Field | Type |
|---|---|
| Ticket ID, Customer Name, Customer Email, Order Number, Subject, Message | Single line / long text |
| AI Summary, Priority Reason, AI Draft Reply, Ticket Resolution Notification | Long text |
| Source | Single select: Support Form, Email |
| Category | Single select: Delivery, Product Quality, Sizing & Exchange, Billing & Payment, Refund & Return, Technical, General |
| Priority | Single select: High, Medium, Low |
| Sentiment | Single select: Negative, Neutral, Positive |
| Approval Status | Single select: Not Requested, Pending, Approved, Rejected |
| Status | Single select: New, Awaiting Approval, Needs Review, Ready to Send, Sending, Response Sent, Resolved |
| Created At, Response Sent At, Resolution Email Sent At | Date with time |

## Setup

1. In n8n, create a new workflow and choose **Import from File**, then select `workflow.json`.
2. Reconnect your credentials: Airtable, Groq, Google Gemini, Gmail, and Telegram.
3. In every Airtable node, select your own base and the `Support Tickets` table.
4. Replace `YOUR_TELEGRAM_CHAT_ID` in the Telegram nodes with your chat ID.
5. In **Request Human Approval**, replace `your-approver@example.com` with the email of the person who approves replies.
6. Test the form with a sample complaint, then activate the workflow.

## Notes

- Credentials and personal IDs are not included in this export.
- AI replies are never sent without human approval.
- Do not commit API keys or personal data to this repository.

## Files

- `workflow.json`: the exportable n8n workflow
- `README.md`: this file
