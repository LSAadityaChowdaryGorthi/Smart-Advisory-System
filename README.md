# Smart Crop Advisory System with Enhanced AI Assistance

This project is a lightweight Django application aimed at providing AI-powered crop, fertilizer, pest and weather advisory for Indian farmers. It's designed to run locally with minimal setup.

Quick start

1. Create and activate a Python virtual environment (recommended).

On Windows PowerShell:

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

2. Optionally generate sample AI models (if TensorFlow & scikit-learn installed):

```powershell
python advisory/train_models.py
```

Notes

- The project includes a placeholder Keras model `advisory/ai_models/pest_model.h5`. Run the training script to create a tiny Keras model if you have TensorFlow installed.

```powershell
python manage.py load_sample_data
```

This command copies the SVG sample images in `media/sample_images/` to `media/uploads/` and inserts a few `Contact` rows so the admin interface and demo pages have content.

- OpenWeather API key placeholder is in `advisory/utils/weather.py`.
- LLM: `gpt4all` is optional. If not present, the code falls back to rule-based logic.

Docker / Reproducible run

You can run the app with Docker for a consistent environment. Example:

```powershell
docker build -t smartcrop .
docker run -p 8000:8000 --rm smartcrop
```

Or use `docker-compose`:

```powershell
docker-compose up --build
```

Offline LLM (gpt4all)

The project supports an optional offline LLM (`gpt4all`). To enable it:

- Install a Python wrapper (e.g. `pip install gpt4all`) and download a model binary (e.g. `ggml-gpt4all-j.bin`).
- Set the environment variable `GPT4ALL_MODEL_PATH` to the downloaded model path or `GPT4ALL_MODEL_NAME` to a known model name before running Django. Example:

```powershell
$Env:GPT4ALL_MODEL_PATH = 'C:\\models\\ggml-gpt4all-j.bin'
python manage.py runserver
```

The app will attempt to use `gpt4all` when available and fall back to simple rule-based responses if not.

Pages

- Home, Crop Recommendation, Fertilizer Advisor, Pest Detection, Weather Advisory, Dashboard, About, Contact, AI Explanation

Files of interest

- `advisory/ai_engine/llm_advisor.py` — LLM wrapper with fallbacks
- `advisory/utils/pest_detection.py` — Pest detection loader and fallback
- `advisory/train_models.py` — Script to train small models and produce pickles

CI

- A GitHub Actions workflow is included at `.github/workflows/python-app.yml` to run unit tests on push and pull requests.

Management

- Use `python manage.py train_models` to run the example training script and regenerate demo models.
