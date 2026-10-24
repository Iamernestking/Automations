# Automations

A collection of my automation projects, mostly built with [n8n](https://n8n.io) and connected to AI models and everyday business tools. Each project lives in its own folder with an importable workflow file and a README explaining what it does and how to set it up.

## Projects

| Project | What it does | Tools |
|---|---|---|
| [Cacao Co. Customer Support Chatbot](./cacao-support-chatbot) | AI chat assistant that answers customer questions, captures and scores leads, and books Google Meet meetings | n8n, Groq, Google Sheets, Google Calendar |
| [RAG Ingestion](./rag-ingestion) | Reads documents from a Google Drive folder, creates embeddings, and stores them in a vector database for AI search | n8n, Google Drive, Google Gemini, Pinecone |
| [SupportFlow AI](./supportflow-ai) | Turns customer complaints into tracked tickets with AI classification, human-approved replies, and Telegram alerts | n8n, Airtable, Groq, Gemini, Gmail, Telegram |

## How to use a project

1. Open the project's folder and read its `README.md`.
2. Download the `workflow.json` file.
3. In n8n, create a new workflow and choose **Import from File**.
4. Reconnect your own credentials (API keys and OAuth accounts) and replace any placeholders such as `YOUR_TELEGRAM_CHAT_ID`.
5. Test the workflow, then activate it.

## Folder structure

```
automations/
├── README.md
├── cacao-support-chatbot/
│   ├── README.md
│   └── workflow.json
├── rag-ingestion/
│   ├── README.md
│   └── workflow.json
└── supportflow-ai/
    ├── README.md
    └── workflow.json
```

## Notes

- Credentials, API keys, and personal IDs are removed from every exported workflow. You need to connect your own accounts.
- Never commit API keys or private data to this repository.
- New projects are added over time.
