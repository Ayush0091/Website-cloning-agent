# How to Run the AI Website Cloning Agent

To start using the AI Website Cloning Agent, you will need to start both the backend API and the frontend dashboard. 

## 1. Setup Your AI Model

You can choose to run this entirely locally with Ollama, or use the cloud-based Google Gemini API.

### Option A: Local Model (Ollama)
If using Ollama, ensure it is installed and running in the background. Then pull the required model (if you haven't already):
```powershell
ollama pull qwen2.5-coder:7b
```

### Option B: Cloud Model (Google Gemini)
If you prefer to use Google Gemini, you need to set your API key as an environment variable in the same terminal where you plan to run the backend:
```powershell
$env:GOOGLE_API_KEY="your_api_key_here"
```

## 2. Start the Backend API (FastAPI)

Open a terminal in the root directory (`e:\agent`) and run the backend server:

```powershell
python -m uvicorn backend.app.main:app --host 127.0.0.1 --port 8000 --reload
```
*The backend should now be running at `http://127.0.0.1:8000`.*

## 3. Start the Frontend Dashboard (React/Vite)

Open a **second, separate terminal**, navigate to the `frontend` folder, and start the development server:

```powershell
cd e:\agent\frontend
npm run dev
```
*The frontend should now be running. Vite usually hosts it at `http://localhost:5174` or `http://localhost:5173`.*

## 4. Open the App
Navigate to the URL provided by the frontend terminal (e.g., `http://localhost:5174`) in your web browser. You can now enter a target website URL and begin cloning!
