# Deploying this demo build

This is the **public demo** version of Sakthi's Vehicle & Cyber Forensics Analysis
Tool: the password gate, machine-lock, and vault encryption have all been removed,
and the app preloads two synthetic sample cases on first start so a visitor lands
on a populated dashboard immediately. It is meant to let someone click through the
real tool's workflow and output — not to hold real case data.

## Why not Vercel

Vercel runs Python code as short-lived serverless functions that take one HTTP
request and return one response — it looks for a top-level `app` / `application` /
`handler` object to call. That's the exact error you saw
(`Found app.py, main.py but none export a top-level "app"...`).

Streamlit doesn't fit that shape at all, with or without a password: it's a single
long-running process that holds a persistent WebSocket connection to the browser
for as long as the tab is open, and it reruns the whole script top-to-bottom on
every click. There's no `app` object to export — the "app" *is* the process.
Removing the password doesn't change that; the mismatch is architectural, not a
setting. Vercel simply isn't a host Streamlit apps can run on.

## Where this actually deploys - Streamlit Community Cloud (free)

This folder is already set up for it: `requirements.txt`, `packages.txt`
(installs `tesseract-ocr` for OCR on image evidence), and `.streamlit/config.toml`
(theme + fonts) are all in place, so no extra config is needed.

1. Push this folder to a GitHub repo you own (public or private both work).
2. Go to **share.streamlit.io** and sign in with GitHub.
3. Click **New app**, pick the repo/branch, set the main file path to `app.py`.
4. Click **Deploy**. First boot takes a minute or two (installing dependencies +
   `tesseract-ocr`); after that it's live at a `*.streamlit.app` URL you can share
   with anyone.

Alternative if you'd rather not use GitHub: **Hugging Face Spaces** also runs
Streamlit apps directly (choose the "Streamlit" Space type) and works the same way
using this same `requirements.txt`.

## Resetting the demo

The sidebar has a **Reset demo data** button — it wipes anything a visitor added
and reseeds the two original sample cases. Nothing here is destructive to any real
data, since this build never touches the private tool at all.
