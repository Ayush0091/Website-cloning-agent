# AI Website Cloning Agent (MVP)

A lightweight web engineering MVP that accepts a website URL and automatically generates a styled React + Tailwind CSS application using Playwright, BeautifulSoup, and Ollama or Google Gemini.

This tool is optimized to run on low-resource hardware (e.g. 8 GB RAM / 4 GB GPU laptops) by utilizing quantized local models, while also offering the option to use Google's Gemini API for higher fidelity outputs.

---

## Architecture

```
                                    +----------------------------------+
                                    |        User URL Input            |
                                    +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |        Playwright Crawler        |
                                    | (Loads DOM, dynamic JS, screens) |
                                    +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |     BeautifulSoup HTML Cleaner    |
                                    | (Strips styling/scripts/SVGs,    |
                                    |  compresses DOM hierarchy)       |
                                    +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |   Structured Page Metadata JSON  |
                                    | (Headings, Links, Images, Text)  |
                                    +----------------------------------+
                                                     |
                                                     v
                                     +----------------------------------+
                                     |   LLM Engine (Ollama / Gemini)   |
                                     | (Prompt engineering using        |
                                     |  qwen2.5 or gemini-2.5 models)   |
                                     +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |   React Code Generation Output   |
                                    +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |       Vite Project Builder       |
                                    | (Injects code into base template)|
                                    +----------------------------------+
                                                     |
                                                     v
                                    +----------------------------------+
                                    |    Exportable Project ZIP Archive|
                                    +----------------------------------+
```

---

## Folder Structure

```
e:/agent/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py            # FastAPI entrypoint, endpoints, and job manager
│   │   ├── crawler.py         # Async Playwright scraping runner
│   │   ├── cleaner.py         # BeautifulSoup HTML structural simplifier
│   │   ├── ollama_client.py   # Ollama API adapter & code generation prompts
│   │   ├── generator.py       # Code extractor and ZIP packaging engine
│   │   ├── config.py          # App configurations (directories, models, hosts)
│   │   └── test_pipeline.py   # Unit test runner for crawling, cleaning, & packing
│   └── template/              # Cloned project Vite + React + Tailwind template
│       ├── package.json
│       ├── vite.config.js
│       ├── tailwind.config.js
│       ├── postcss.config.js
│       ├── index.html
│       └── src/
│           ├── main.jsx
│           ├── index.css
│           └── App.jsx        # Target placeholder (overwritten with cloned code)
├── frontend/                  # Dashboard control application
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── index.html
│   └── src/
│       ├── main.jsx
│       ├── index.css
│       └── App.jsx            # Cloner React control UI
├── requirements.txt           # Python dependencies
└── README.md                  # System Documentation
```

---

## Backend API Endpoints

The FastAPI backend exposes the following REST APIs:

1. **`GET /`**
   - Returns API health status.

2. **`GET /api/status-check`**
   - Verifies system connection to Ollama and lists all pulled/available models.

3. **`POST /api/clone`**
   - Accepts a URL, model choice, and optional instructions. Spawns the cloning pipeline as an asynchronous background task.
   - Body:
     ```json
     {
       "url": "https://example.com",
       "model": "qwen2.5-coder:7b",
       "custom_instructions": "Make the primary button deep purple."
     }
     ```
   - Response: `{"job_id": "uuid-string", "message": "..."}`

4. **`GET /api/status/{job_id}`**
   - Polls active job tracking logs, pipeline state (`queued`, `crawling`, `cleaning`, `generating`, `exporting`, `completed`, `failed`), and percentage progress.

5. **`GET /api/export/{job_id}`**
   - Downloads the zipped, ready-to-run React Vite cloned project.

---

## Installation Steps

### Prerequisites
1. **Node.js** (v18 or higher)
2. **Python** (v3.10 or higher)
3. **Ollama** installed and running on your system, **OR** a Google Gemini API Key.

### 1. Set up LLM Models

**Option A: Local Models (Ollama)**
Pull the optimized coding model (4-bit quantized version runs perfectly on 8GB RAM):
```bash
ollama pull qwen2.5-coder:7b
```
Ensure Ollama is running locally in the background (usually runs on `http://localhost:11434`).

**Option B: Google Gemini API**
If you prefer to use Gemini, export your API key as an environment variable and install the optional dependency:
```bash
export GOOGLE_API_KEY="your_api_key_here"
pip install google-generativeai
```

### 2. Install Python Backend Dependencies
From the root of the project:
```bash
pip install -r requirements.txt
playwright install chromium
```

### 3. Install Frontend Dashboard Dependencies
```bash
cd frontend
npm install
cd ..
```

---

## Run Commands

To start the MVP, run the backend and frontend simultaneously.

### 1. Start FastAPI Backend
From the root directory:
```bash
python -m uvicorn backend.app.main:app --host 127.0.0.1 --port 8000 --reload
```

### 2. Start Cloner Dashboard
Open a new terminal session, navigate to the `frontend/` directory and run:
```bash
cd frontend
npm run dev
```
Open your browser and navigate to **`http://localhost:5174`** to use the cloning agent!

---

## Testing Guide

### Automated Tests
Run the automated test suite to verify the HTML parser, DOM cleaning, code extraction, and template packaging:
```bash
python -m unittest backend.app.test_pipeline
```

### Manual Verification
1. Open the dashboard at `http://localhost:5174`.
2. Check that the **Backend** and **Ollama** status indicators are green (Online / Connected).
3. Enter a target website URL (e.g. `https://example.com` or a lightweight portfolo page).
4. Select `qwen2.5-coder:7b` in the dropdown.
5. Click **Clone Target Site**.
6. Monitor the real-time logs terminal as the agent goes through Crawling → HTML Cleaning → Generating → Exporting.
7. Click the **Download React Project ZIP** button upon completion.
8. Unzip the downloaded file, navigate into `cloned-react-project`, run `npm install`, and launch it via `npm run dev` (runs on `http://localhost:5173`) to view the cloned output.

---

## Future Improvements

1. **Multi-Page Crawling**: Extend the crawler to follow same-origin page links and build a multi-route React Router structure.
2. **Local Asset Mirroring**: Download image tags and stylesheets locally into the project directory instead of linking online references.
3. **Refinement Iterations**: Allow users to chat back to the model within the logs page to patch layout defects in the generated React code iteratively.
4. **Tailwind Refinement Parser**: Parse target stylesheet computed CSS rules and convert them dynamically to Tailwind mapping lists.
