# UIA-benchmarks
Simple harness to test A-reflex behavior and resilience loops (A4→B3→C1).

Prototype app for **UIA_Inference_Certification** aligned to your requested workflow.

## What this version does

This version now supports both:
- **Manual workflow** in 4 steps (manifold, AX classification, envelope, verification), and
- **Autopilot workflow**: one prompt in the app triggers the full pipeline through a backend endpoint.

### Manual workflow (UI)
1. Get the agent manifold by testing it.
2. Detect which AX families the prompt likely triggers.
3. Build and view **Inference Envelope (Pre-generation)** data.
4. Verify and view **Envelope Verification (Post-generation)** validation data against envelope targets (PASS/FAIL).

### Autopilot workflow (new)
Use **Autopilot mode** in the UI or call the API directly:

`POST /analyze-and-run`

Request body:
```json
{
  "prompt": "your prompt here"
}
```

Response includes:
- manifold summary,
- predicted AX families,
- generated envelope,
- observed metrics,
- verification checks,
- optional model run metadata.

If `OPENAI_API_KEY` is configured, the backend will also attempt a real OpenAI call with the generated envelope in system instructions.

## Files

- `index.html` – UI with manual workflow and Autopilot panel.
- `app.js` – frontend logic for local analysis + backend autopilot trigger.
- `server.js` – local backend serving static files and `/analyze-and-run` orchestration.
- `styles.css` – dark dashboard styling.
- `package.json` – `npm start` script.

## Run locally

```bash
npm start
```

Then open `http://127.0.0.1:4173`.

If Autopilot fails, use the **Connection diagnostics** card in the UI and click **Check backend**. It validates `/health` and warns if you are on a static preview URL instead of the Node server.

## If your GitHub `main` only shows `README.md`

Your repository history may still be at the initial commit. To put the full app files back into `UIA-benchmarks/main`, merge the PR for the app commit, or run these commands in your local clone/Codespace and then push:

```bash
git checkout main
git fetch --all
git cherry-pick f9b140c
git push origin main
```

After that, `main` should contain `app.js`, `index.html`, `server.js`, `styles.css`, and `package.json`.


## Deploy (public URL)

### Option A: Render (recommended for beginners)

This repo now includes `render.yaml`, so Render can auto-configure service settings.

1. Push this repo to GitHub with all app files (`package.json`, `server.js`, `index.html`, `app.js`, `styles.css`, `render.yaml`).
2. Go to Render → **New** → **Blueprint**.
3. Connect your GitHub account and select `UIA-benchmarks`.
4. Render reads `render.yaml` and creates a web service automatically.
5. Open the generated URL (for example: `https://uia-benchmarks.onrender.com`).

If you want real model execution, add environment variable `OPENAI_API_KEY` in Render service settings and redeploy.

### Option B: Docker (any host)

A `Dockerfile` is included. Build and run:

```bash
docker build -t uia-benchmarks .
docker run -p 4173:4173 uia-benchmarks
```

Then open `http://127.0.0.1:4173`.

If Autopilot fails, use the **Connection diagnostics** card in the UI and click **Check backend**. It validates `/health` and warns if you are on a static preview URL instead of the Node server.
