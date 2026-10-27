# 📄 ATS Resume Checker

Upload a resume (PDF, DOCX or TXT) and get:

- An **ATS score out of 100** with a breakdown (keywords, impact, formatting, structure, clarity)
- Strengths, issues, and found vs. missing keywords
- Prioritized improvements and example bullet rewrites
- A downloadable report

Optionally paste a job description for a more accurate keyword score.

Built with [Streamlit](https://streamlit.io) and Google's Gemini Flash models.

## Files

| File | Purpose |
|------|---------|
| `app.py` | The Streamlit app |
| `requirements.txt` | Python dependencies |
| `README.md` | This file |

## Run locally

1. Install Python 3.10+.
2. Get a free API key at <https://aistudio.google.com/apikey>.
3. In a terminal inside the project folder:

```bash
pip install -r requirements.txt
streamlit run app.py
```

4. Paste your API key in the sidebar (or set it as a secret, see below).

## Configuration

The app reads these values from Streamlit secrets or environment variables:

| Name | Required | Description |
|------|----------|-------------|
| `GEMINI_API_KEY` | Yes | Your Gemini API key |
| `GEMINI_MODEL` | No | Model name. Defaults to `gemini-3.7-flash` |

Google renames Flash models regularly. If you see "model was not found", look up
the current name at <https://ai.google.dev/gemini-api/docs/models> and change it
in the sidebar or the `GEMINI_MODEL` secret.

For local use, create `.streamlit/secrets.toml` (never commit this file):

```toml
GEMINI_API_KEY = "your-key-here"
GEMINI_MODEL = "gemini-3.7-flash"
```

## Deploy on Streamlit Community Cloud

1. Push this project to a **GitHub** repository (see below).
2. Go to <https://share.streamlit.io> and sign in with GitHub.
3. Click **Create app** → **Deploy a public app from GitHub**.
4. Choose your repository, branch `main`, and main file path `app.py`.
5. Open **Advanced settings → Secrets** and paste:

   ```toml
   GEMINI_API_KEY = "your-key-here"
   ```

6. Click **Deploy**. The first build takes a few minutes.

## How the score works

Gemini rates five categories from 0 to 100. The overall score is a weighted
average computed in code (not by the AI):

| Category | Weight |
|----------|--------|
| Keywords & Relevance | 30% |
| Impact & Achievements | 25% |
| ATS-Friendly Formatting | 20% |
| Structure & Completeness | 15% |
| Clarity & Grammar | 10% |

This is an AI-based estimate, not the output of a real ATS. Treat it as guidance.

## Privacy

Resume text is sent to the Gemini API for analysis and is not stored by this app.
Do not upload documents you are not comfortable sharing.

## Limitations

- Scanned or image-only PDFs can't be read (an ATS can't read them either).
- Very long resumes are truncated to about 20,000 characters.
- Scores can vary slightly between runs.
