# LangGraph PDF Chatbot

Ask questions about your PDFs in a Streamlit chat app powered by LangGraph and OpenAI. The assistant can retrieve information from uploaded documents and use tools for web search, calculations, and stock quotes.

## Features

- **Chat with PDFs:** Upload a PDF and ask questions about its contents.
- **Tool-enabled assistant:** Use web search, a calculator, and an Alpha Vantage stock quote tool.
- **Conversation threads:** Keep separate conversations with LangGraph's SQLite checkpointer.
- **Streaming interface:** See assistant responses as they are generated.
- **Optional LangSmith tracing:** Trace model runs when LangSmith is configured.

## Project structure

| File | Description |
| --- | --- |
| `langgraph_rag_backend` | LangGraph backend with PDF ingestion, retrieval, tools, and SQLite checkpoints. |
| `streamlit_rag_frontend.py` | Main interface for chatting with PDFs. |
| `langgraph_backend.py` | Basic chatbot backend without PDF retrieval. |
| `streamlit_frontend_threading.py` | Chat interface with conversation threads and tool status. |
| `streamlit_frontend.py` | Minimal single-thread chat interface. |

## Prerequisites

- Python 3.10 or newer
- An OpenAI API key
- An Alpha Vantage API key for stock quotes
- A LangSmith API key if you want tracing (optional)

## Installation

Clone the repository and move into its directory:

```bash
git clone <your-repository-url>
cd <repository-directory>
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

Install the packages used by the app:

```bash
pip install streamlit langgraph langgraph-checkpoint-sqlite langchain langchain-community langchain-openai langchain-text-splitters langchain-core pypdf faiss-cpu python-dotenv requests
```

## Configure environment variables

Create a `.env` file in the project root. Add your own credentials; never commit this file:

```dotenv
OPENAI_API_KEY=your_openai_api_key
ALPHAVANTAGE_API_KEY=your_alpha_vantage_api_key

# Optional LangSmith tracing
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_api_key
LANGCHAIN_PROJECT=your_project_name
```

The backends load environment variables from `.env` using `python-dotenv`. The stock quote tool expects `ALPHAVANTAGE_API_KEY` to be read from the environment.

## Run the app

Start the PDF chat interface:

```bash
streamlit run streamlit_rag_frontend.py
```

Streamlit will print a local URL to open in your browser. The other included frontends can be started with:

```bash
streamlit run streamlit_frontend_threading.py
```

or:

```bash
streamlit run streamlit_frontend.py
```

## Use the PDF chat

1. Open the app in your browser.
2. Upload a PDF using the sidebar.
3. Ask a question about the uploaded document, or ask a general question that can use the assistant's tools.
4. Choose **New Chat** to start a separate conversation.

PDF text is split into chunks, embedded, and indexed in a FAISS retriever for the active thread. The retrievers are held in memory, so upload the PDF again after restarting the app. Chat checkpoints are stored locally in `chatbot.db`.

## Privacy and security

- Keep `.env` private. It is excluded by `.gitignore` along with the virtual environment, Python cache, and SQLite database files.
- Do not commit API keys, uploaded documents, or private chat data.
- Review the files staged for Git before pushing. If a credential was committed or shared, revoke or rotate it.

## Dependencies

This repository does not currently include a dependency lockfile. For reproducible installs, record and pin the package versions that work in your environment.
