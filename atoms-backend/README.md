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
