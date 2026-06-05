---
title: Atoms Agent Demo Backend
emoji: ⚛️
colorFrom: green
colorTo: blue
sdk: docker
app_port: 7860
pinned: false
short_description: FastAPI backend for an Atoms-style agent app builder demo.
---

# Atoms Agent Demo Backend

FastAPI backend for the Atoms-style agent builder demo.

## Local run

```bash
pip install -r requirements.txt
uvicorn app:app --reload --port 7860
```

## Hugging Face deployment

Create a Docker Space, upload this folder, and keep `Dockerfile` at the Space root.

Set `FRONTEND_ORIGIN` to your Vercel URL after deployment, for example:

```bash
FRONTEND_ORIGIN=https://your-app.vercel.app
```

## AI provider

The backend can call any OpenAI-compatible chat completions API.

Set these environment variables in Hugging Face:

```bash
AI_API_BASE_URL=https://api.openai.com/v1
AI_API_KEY=your_api_key
AI_MODEL=gpt-4o-mini
```

For another compatible provider, keep the same `/chat/completions` API shape and change `AI_API_BASE_URL`.

If these variables are not set, the backend still works with deterministic demo output.
