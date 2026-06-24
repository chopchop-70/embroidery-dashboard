# Mediscrubs Embroidery Dashboard

Live embroidery machine performance dashboard for the Mediscrubs team.

## How it works

```
C:\Embroidery\*.xlsx  →  generate_dashboard_data.py  →  GitHub (data.json)  →  Vercel (dashboard)
      daily at 3:30pm         runs after sync script           auto-deploys
```

## Setup (one-time)

### 1. Create GitHub repository

1. Go to [github.com](https://github.com) and sign up / log in
2. Click **New repository** → name it `embroidery-dashboard` → **Create repository**
3. Upload all files from this folder to the repo

### 2. Get a GitHub Personal Access Token

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click **Generate new token (classic)**
3. Give it a name, set expiry to **No expiration**
4. Tick the **repo** checkbox
5. Click **Generate token** — copy it immediately

### 3. Configure the script

Edit `C:\Scripts\generate_dashboard_data.py`:

```python
GITHUB_TOKEN = "ghp_xxxxxxxxxxxxxxxxxxxx"   # paste your token here
GITHUB_REPO  = "jasondacey/embroidery-dashboard"  # your GitHub username/repo
```

### 4. Deploy to Vercel

1. Go to [vercel.com](https://vercel.com) → Sign up with GitHub
2. Click **Add New Project** → Import your `embroidery-dashboard` repo
3. Leave all settings as default → click **Deploy**
4. Your dashboard is live at `https://embroidery-dashboard.vercel.app`

### 5. Add to Windows Task Scheduler

Add a second task (or extend the existing one) to run after the sync:

```
Program: python
Arguments: C:\Scripts\generate_dashboard_data.py
Trigger: Daily at 3:35 PM (5 min after embroidery_final.py)
```

Or chain it by adding this line to the end of `embroidery_final.py`:
```python
import subprocess
subprocess.run(["python", r"C:\Scripts\generate_dashboard_data.py"])
```

## Files

```
embroidery-dashboard/
├── public/
│   ├── index.html      ← the dashboard
│   └── data.json       ← auto-updated daily by the script
├── scripts/
│   └── generate_dashboard_data.py   ← run on your PC daily
├── vercel.json         ← Vercel config
└── README.md
```

## Updating

The dashboard updates automatically each day after the sync script runs.
To force a refresh, just re-run `generate_dashboard_data.py` manually.
