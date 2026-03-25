# Job Application Tracker

Automated job application tracking system that reads Gmail, parses emails with Claude AI, writes to Excel, publishes a GitHub Pages dashboard, and syncs to Google Drive.

## Prerequisites

- Python 3.10+
- Git (with a GitHub remote configured for dashboard publishing)
- Claude Code CLI installed and configured with Gmail and Google Drive MCP connectors
- An Anthropic API key

## First-Time Setup

1. Clone the repository:

```bash
git clone <your-repo-url>
cd job-tracker
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Configure environment variables:

```bash
cp .env.example .env
```

Edit `.env` and fill in your values:

- `ANTHROPIC_API_KEY` — your Anthropic API key for Claude Haiku email parsing
- `GITHUB_REPO_PATH` — absolute path to this repo on your machine

## Enable GitHub Pages

1. Go to your GitHub repo Settings
2. Navigate to Pages
3. Set source to **Deploy from a branch**
4. Select **main** branch and **/docs** folder
5. Save — your dashboard will be live at `https://<username>.github.io/<repo>/`

## How to Run

```bash
cd src
python tracker.py
```

The tracker will:

1. Fetch job-related emails from Gmail (last 90 days on first run, incremental after)
2. Parse each email with Claude Haiku to extract company, role, status, etc.
3. Write/update `data/applications.xlsx` with color-coded conditional formatting
4. Generate `docs/index.html` dashboard and git push it
5. Upload the Excel file to a "Job Tracker" folder in Google Drive

## Running via Claude Code or Cowork

You can run the tracker directly from Claude Code or Cowork by asking:

> "Run my job tracker" or "Sync my job applications"

Or execute directly:

```bash
claude -p "cd job-tracker/src && python tracker.py"
```

## Data Storage

The `data/` directory is gitignored — the Excel file lives locally only. It is automatically uploaded to Google Drive on every run for online access.

To share the spreadsheet, use the Google Drive link logged after each run.

## Resetting State

To re-process all emails from scratch (last 90 days), delete the state file:

```bash
rm data/state.json
```

The next run will treat all matching emails as new and rebuild the spreadsheet.

## Project Structure

```
job-tracker/
├── src/
│   ├── gmail_reader.py       # Gmail MCP email fetching
│   ├── email_parser.py       # Claude API email parsing
│   ├── excel_writer.py       # openpyxl spreadsheet logic
│   ├── dashboard_generator.py # HTML dashboard + git push
│   ├── drive_uploader.py     # Google Drive MCP upload
│   └── tracker.py            # Main orchestration
├── docs/
│   └── index.html            # Auto-generated GitHub Pages dashboard
├── data/
│   └── applications.xlsx     # Local Excel file (gitignored)
├── .env.example
├── requirements.txt
└── README.md
```
