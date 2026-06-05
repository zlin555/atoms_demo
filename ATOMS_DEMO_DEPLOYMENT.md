# Atoms Agent Builder Demo Deployment

This project is split into two deployable apps:

- `atoms-frontend/`: React/Vite app for Vercel.
- `atoms-backend/`: FastAPI app for Hugging Face Docker Space.

## Backend on Hugging Face

1. Create a new Hugging Face Space.
2. Select `Docker` as the Space SDK.
3. Upload the contents of `atoms-backend/` to the Space root.
4. Set this environment variable after the frontend is deployed:

```bash
FRONTEND_ORIGIN=https://your-vercel-project.vercel.app
```

The backend exposes:

- `GET /health`
- `GET /api/templates`
- `GET /api/projects`
- `POST /api/builds`
- `POST /api/builds/rerun`
- `GET /api/builds/{build_id}`
- `GET /api/builds/{build_id}/export`
- `POST /api/publish`

## Frontend on Vercel

1. Create a Vercel project with `atoms-frontend/` as the project root.
2. Set:

```bash
VITE_API_BASE_URL=https://your-huggingface-space.hf.space
```

3. Build command:

```bash
npm run build
```

4. Output directory:

```bash
dist
```

## Local Development

Backend:

```bash
cd atoms-backend
pip install -r requirements.txt
uvicorn app:app --reload --port 7860
```

Frontend:

```bash
cd atoms-frontend
npm install
npm run dev
```

Create `atoms-frontend/.env.local`:

```bash
VITE_API_BASE_URL=http://localhost:7860
```
