# EDM App

An **E**mail-**D**riven **M**anagement assistant that fetches Outlook emails, extracts actionable issues via embedded AI agents, and presents them as structured JSON or styled tables.

## 🚀 Features

- 🔎 **Fetch Outlook Emails** by date range (Microsoft Graph API)
- 🧹 **Clean Email Content**: Strip HTML, normalize line breaks, convert UTC → Asia/Beirut
- 🤖 **Extract Issues** using Llama3-based AI agent (Groq)
- 📄 **Flatten Metadata + Issues** for unified JSON structure
- 📊 **Generate Tables** with a second agent (pipe-delimited or HTML)
- 📁 **Modular Codebase**: Organized into agents, services, models, auth, and utils

## 🧰 Prerequisites

- Python 3.10+
- A virtual environment (recommended)
- Microsoft Graph **refresh token** saved in `refresh_token.txt`
- Groq **API key** set as an environment variable:
  ```bash
  export GROQ_API_KEY="gsk-xxxx"
  ```

## ⚡ Quickstart

### 1. Clone the project
```bash
git clone <your-repo-url> edm_app
cd edm_app
```

### 2. Create and activate a virtual environment
```bash
python -m venv .venv

# Windows PowerShell
.\.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate
```

### 3. Install required packages
```bash
pip install fastapi uvicorn httpx pydantic beautifulsoup4 pytz msal groq
```

### 4. Add your Microsoft refresh token
Save it as a single line in a file named `refresh_token.txt` in the root directory.

### 5. Run the app
```bash
uvicorn edm_app.main:app --reload
```

### 6. Access
- **API available at:** http://127.0.0.1:8000
- **Swagger docs:** http://127.0.0.1:8000/docs

## 🔌 API Endpoints

### POST /emails
Fetch and clean Outlook emails.

**Request:**
```json
{
  "from_date": "2025-06-01",
  "to_date": "2025-06-20"
}
```

**Response:**
```json
[
  {
    "subject": "...",
    "from": "user@domain.com",
    "is_read": false,
    "received_date": "2025-06-19 10:31:53",
    "body_text": "..."
  }
]
```

### POST /smartemails
Fetch emails, extract issues with agent, and return full table or issue list.

**Request:**
```json
{
  "user_query": "between June 1 and June 20"
}
```

**Response:**
List of flattened issue entries or a formatted table string (depending on usage).

## 📁 Project Structure

```
edm_app/
├── refresh_token.txt           # Your Microsoft refresh token
├── edm_app/
│   ├── __init__.py
│   ├── main.py                 # FastAPI app entrypoint
│   ├── config.py               # API keys, timezone, constants
│   ├── auth/
│   │   └── microsoft_auth.py   # Handles MSAL auth + token refresh
│   ├── agents/
│   │   ├── date_range.py       # Agent for parsing date ranges
│   │   ├── formatter.py        # Converts issues to table format
│   │   └── table.py            # Pipe-delimited table generator
│   ├── services/
│   │   ├── email_service.py    # Email fetching, cleaning logic
│   │   └── issue_service.py    # Issue flattening and parsing
│   ├── models/
│   │   ├── email.py            # Pydantic: EmailRequest, EmailOut
│   │   └── issue.py            # Pydantic: Issue, FlatIssue
│   └── utils/
│       └── time_utils.py       # Timezone conversion helpers
```

## 🧠 How to Extend

- **Add new agents** in `agents/`
- **Place new business logic** in `services/`
- **Add routes** in `main.py` or a new `routers/` directory
- **Create new Pydantic models** in `models/`

## 📬 Support

For help or feature requests, contact the maintainer or open a GitHub issue.
