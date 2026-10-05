# Travel Guide

Travel Guide is a small web app that generates written and spoken guides for destinations. The frontend is plain HTML, CSS, and JavaScript. A Flask backend uses Google Gemini to write the guide and Murf AI to generate speech.

## Features

- Browse featured destinations, including the Taj Mahal, Red Fort, Gateway of India, Hawa Mahal, Golden Temple, and Mysore Palace.
- Generate a short summary or a detailed guide.
- Choose English, Hindi, Tamil, or Telugu, and a male or female voice.
- Read the generated transcript and play the generated audio.

## Requirements

- Python 3.13 (the backend environment has been verified with this version)
- A Google Gemini API key
- A Murf API key

## Setup

Open PowerShell at the project root and create the backend environment:

```powershell
cd Backend
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Create `Backend/.env` from the example if it does not already exist:

```powershell
Copy-Item .env.example .env
```

Edit `Backend/.env` and add your API keys:

```dotenv
GOOGLE_GENAI_API_KEY=your_google_genai_api_key
MURF_API_KEY=your_murf_api_key
```

Keep `.env` private. It is excluded from Git.

## Run

In PowerShell, from the `Backend` directory, start the API:

```powershell
python app.py
```

The API listens at `http://127.0.0.1:5000`. Keep this terminal open. Open `Frontend/index.html` in a browser to use the app. The frontend sends requests to the local API.

If PowerShell does not activate the virtual environment, run the backend with its interpreter directly:

```powershell
.\.venv\Scripts\python.exe app.py
```

## Deploy on Render

Deploy the backend as a **Web Service** with these settings:

- **Root Directory:** `Backend`
- **Build Command:** `pip install -r requirements.txt`
- **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT`
- **Environment Variables:** set `GOOGLE_GENAI_API_KEY` and `MURF_API_KEY` in the service's Environment settings. Do not commit API keys or put them in `.env.example`.

After the backend deploy succeeds, create a separate **Static Site** for the frontend:

- **Root Directory:** `Frontend`
- **Build Command:** `echo "No build required"`
- **Publish Directory:** `.`

The frontend uses the local API when opened locally and the deployed backend URL when hosted. If the backend service URL changes, update `API_BASE_URL` in `Frontend/index.js` to match.

## API

`POST /generate-audio-guide` accepts JSON with these fields:

```json
{
  "place": "Taj Mahal",
  "answerType": "Summary",
  "language": "English",
  "voiceId": "Matthew",
  "locale": "en-US"
}
```

`answerType` is `Summary` or `Detailed`. Supported languages are English, Hindi, Tamil, and Telugu. The response contains the generated `description` and an `audioBase64` MP3 payload.

## Project Structure

```text
Backend/
  app.py
  requirements.txt
  .env.example
Frontend/
  index.html
  index.js
README.md
```
