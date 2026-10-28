# ai-resume-assistant444
# 📄 ATS Resume Checker

Upload a resume (PDF, DOCX or TXT) and get an ATS-style score out of 100, a
category breakdown, prioritized improvements, keyword gaps and suggested bullet
rewrites. Optionally paste a job description for a tailored keyword match.

Built with **Streamlit** (UI) and a **Google Gemini Flash** model (analysis).

> The score is an AI-based estimate, not the output of a real ATS. Use it as a
> guide, not a guarantee.

## Features

- PDF / DOCX / TXT upload (max 5 MB)
- Overall ATS score (weighted from 5 categories) and job-match score
- Prioritized improvements (High / Medium / Low) with specific fixes
- Found vs. missing keywords
- Before/after bullet rewrites
- Rule-based quick checks (contact info, sections, length, quantified results)
- Downloadable Markdown report

## Project structure

```
.
├── app.py
├── requirements.txt
├── README.md
└── .gitignore        # create this (see step 2 below)
```

## Run locally

1. Install Python 3.10+ and get a free Gemini API key at
   https://aistudio.google.com/apikey
2. Install dependencies:
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. Provide your key (pick one):
   - Paste it into the sidebar when the app opens, **or**
   - Create `.streamlit/secrets.toml`:
     ```toml
     GEMINI_API_KEY = "your-key-here"
     # optional:
     # GEMINI_MODEL = "gemini-3.7-flash"
     ```
   - Or set an environment variable: `export GEMINI_API_KEY=your-key-here`
4. Start the app:
   ```bash
   streamlit run app.py
   ```

## Choosing the Gemini model

The default model is `gemini-3.7-flash`. Google renames and retires models
often, so you can change it in the sidebar or with the `GEMINI_MODEL` secret.
If the chosen name isn't found, the app automatically tries `gemini-3.6-flash`
and then `gemini-2.5-flash`. Current names:
https://ai.google.dev/gemini-api/docs/models

## Push to GitHub

1. Create a `.gitignore` so your key never gets committed:
   ```
   .venv/
   __pycache__/
   .streamlit/secrets.toml
   .env
   ```
2. Create an empty repository on https://github.com/new (e.g. `ats-resume-checker`).
   Don't tick "Add a README" since you already have one.
3. In your project folder run:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: ATS resume checker"
   git branch -M main
   git remote add origin https://github.com/<your-username>/ats-resume-checker.git
   git push -u origin main
   ```
   GitHub requires a Personal Access Token instead of your password when
   prompted (Settings → Developer settings → Personal access tokens), or use
   GitHub Desktop / `gh auth login`.

**Never commit your API key.** If you do by mistake, delete and recreate the key.

## Deploy on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Choose your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings**, pick Python 3.11 or 3.12, and paste in the
   **Secrets** box:
   ```toml
   GEMINI_API_KEY = "your-key-here"
   ```
5. Click **Deploy**. The first build takes a couple of minutes. Later pushes to
   `main` redeploy automatically.

You can edit secrets later from the app's **⋮ → Settings → Secrets**.

> Tip: if your key is in Secrets, anyone using your public app spends *your*
> Gemini quota. To avoid that, leave the secret out so each visitor pastes their
> own key in the sidebar, or make the app private/invite-only.

## Troubleshooting

| Problem | Fix |
|---|---|
| "API key was rejected" | Check the key and that the Gemini API is enabled for it |
| "Models not found" | Set a valid model name in the sidebar |
| "Very little text extracted" | Resume is a scanned image; export a text-based PDF/DOCX |
| Rate limit / quota message | Wait a minute or use a different key |
| Build fails on Streamlit Cloud | Confirm `requirements.txt` is in the repo root |

## Privacy

Resume text is sent to Google's Gemini API for analysis. The app itself does
not store uploaded files or results.
