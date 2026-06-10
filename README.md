# ⚡ Stream URL Extractor

A GitHub Actions + GitHub Pages streaming URL extractor.  
Paste URLs into the Actions workflow → results appear live on your GitHub Pages site.

**Supported hosts:**  
MixDrop · Vidmoly · Voe.sx · StreamWish · StreamTa · StreamRuby · Vids.st · SaveFiles · BigShare · DoodStream · Luluvdoo · FileNoons/EarnVideo · FileLions · VidNest · Vidoza · Upzur · Vinovo · Vidara · VixSrc.to · GogoAnime/MegaPlay · StreamIMDB/Cloudnestra + generic fallback

---

## Setup (one-time, ~3 minutes)

### 1. Enable GitHub Pages

Go to your repo → **Settings → Pages**

- Source: **Deploy from a branch**
- Branch: `main` (or your default branch)
- Folder: `/docs`

Click **Save**. Your viewer will be at:
```
https://<your-username>.github.io/<repo-name>/
```

### 2. Give Actions write permission

Go to **Settings → Actions → General → Workflow permissions**  
Select **Read and write permissions** → Save.

### 3. That's it!

The workflow and viewer are ready. No secrets needed.

---

## Usage

### Via GitHub UI

1. Go to **Actions → 🎬 Stream URL Extractor**
2. Click **Run workflow**
3. Paste your URLs (one per line) into the **URLs** field
4. Optionally add a **label** (e.g. `naruto-ep5`) and toggle **Append** to keep history
5. Click **Run workflow**
6. Wait ~30–90 seconds, then open your GitHub Pages URL

### Via GitHub CLI

```bash
gh workflow run extract.yml \
  -f urls="https://streamwish.to/e/abc123
https://dood.watch/e/xyz789" \
  -f label="batch-01" \
  -f append=false
```

### Via REST API (automation)

```bash
curl -X POST \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/<owner>/<repo>/dispatches \
  -d '{
    "event_type": "extract-urls",
    "client_payload": {
      "urls": "https://streamwish.to/e/abc123\nhttps://dood.watch/e/xyz789",
      "label": "my-batch",
      "append": "true"
    }
  }'
```

---

## Local usage (CLI)

```bash
pip install -r requirements.txt

# Single URL
python extract.py https://streamwish.to/e/abc123

# Multiple URLs, save to JSON
python extract.py --file urls.txt --json-out docs/results.json

# Append to existing results
python extract.py --append --json-out docs/results.json https://dood.watch/e/xyz
```

---

## File structure

```
├── extract.py                    # Core extractor (CLI, no GUI dependencies)
├── requirements.txt
├── .github/
│   └── workflows/
│       └── extract.yml           # GitHub Actions workflow
└── docs/
    ├── index.html                # GitHub Pages viewer (auto-loads results.json)
    └── results.json              # Written by the workflow, read by the viewer
```

---

## Viewer features

- **Live results** — auto-refreshes every 60 seconds
- **Filter** by success / failed / m3u8 / mp4
- **Search** by URL or host name
- **Copy URL** — one click copy
- **MPV command** — copies `mpv "url" --http-header-fields="Referer: ..."` ready to paste
- **VLC command** — copies `vlc "url"`
- **Quality variants** — expands to show all stream resolutions when available
- **Copy All** — bulk copy all successful URLs in current filter
