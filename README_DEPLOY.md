# Deploying to Render (one-time steps)

This document describes the minimal, one-time setup to enable automated deploys to Render using the GitHub Actions workflow `./.github/workflows/ci-deploy.yml`.

1. Create a Render Web Service

   - In Render, create a new **Web Service** from the GitHub repository `SmartCrop` (connect your GitHub account and select the repo).
   - Set the build command to: `pip install -r requirements.txt` (the workflow also installs deps).
   - Set the start command to: `gunicorn smartcrop.wsgi --bind 0.0.0.0:$PORT --workers 3` (Procfile is included).
   - Choose the Python version 3.11 (or match `runtime.txt`).

2. Add GitHub Secrets

   - Go to your GitHub repository → Settings → Secrets → Actions.
   - Add the following secrets:
     - `RENDER_API_KEY` — your Render API key (you can create one in Render account settings → API Keys).
     - `RENDER_SERVICE_ID` — the Render Service ID for the web service you created (found in the service's settings or the service page URL).

3. Environment variables

   - In Render, add environment variables for production values (Settings → Environment):
     - `DJANGO_SECRET_KEY` — a secure secret key for Django.
     - `DJANGO_DEBUG` — set to `False` in production.
     - `DJANGO_ALLOWED_HOSTS` — comma-separated hostnames your service will serve (e.g. `example.onrender.com`).
     - Any other secrets your app requires (database credentials, third-party API keys).

4. Enable GitHub Actions
   - Once the secrets are set, pushing to `main` (or `master`) will run the workflow which installs dependencies, runs tests, and triggers a Render deploy.

Notes:

- The workflow triggers a Render deploy via the Render API — it does not configure Render for you. You must create the Render service first and set the `RENDER_SERVICE_ID`.
- If your app relies on heavy libraries (TensorFlow), consider using a custom build or a Docker service instead of the default build environment.
