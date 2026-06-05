# Atoms Demo

Atoms-style agent builder demo split into frontend and backend deploy targets.

## Vercel Frontend

If deploying from the repository root, Vercel can use the root `vercel.json`.

Settings:

```bash
Install Command: cd atoms-frontend && npm install
Build Command: npm run build
Output Directory: atoms-frontend/dist
```

Environment variable:

```bash
VITE_API_BASE_URL=https://your-huggingface-space.hf.space
```

You can also set the Vercel project root to `atoms-frontend`. In that case, use the `atoms-frontend/vercel.json` config.

## Hugging Face Backend

Deploy `atoms-backend` as a Docker Space.

Environment variable:

```bash
FRONTEND_ORIGIN=https://your-vercel-project.vercel.app
```
