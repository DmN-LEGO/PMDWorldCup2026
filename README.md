# PMDF Brick Cup 2026

Fantasy card-collecting game for PMD Finance — World Cup 2026.

## What's in this repo

```
index.html              — The complete single-file game (deploy to Cloudflare Pages)
brick_cup_scorer.py     — Auto-scorer cron script (runs via GitHub Actions)
.github/workflows/
  brick-cup-cron.yml    — GitHub Actions workflow (runs every 15 mins)
```

## Deploy in 3 steps

### Step 1 — Cloudflare Pages
1. Push this repo to GitHub
2. Go to Cloudflare Pages → Create project → Connect to GitHub → select this repo
3. Build settings: Framework = None, Build command = (empty), Output = /
4. Deploy. Your game URL will be something like `brick-cup.pages.dev`


### Step 3 — Firestore Rules
In Firebase Console → Firestore → Rules, set:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
(Fine for an internal game — tighten later if needed)

## How it works

- The game is a static HTML file — no server needed
- All game state lives in Firebase Firestore (REST API, no SDK for main game logic)
- Google Sign-In uses Firebase Auth compat SDK (loaded from CDN with defer)
- The GitHub Actions cron runs every 15 minutes and handles:
  - Auto-locking windows when matches kick off
  - Scoring finished matches from football-data.org
  - Resolving Safe + Brave predictions and paying bricks
  - Opening next window when all matches are done
  - Paying +20🧱 participation bonus to active players
  - Seeding the Window Store (20 players at 30🧱 each)

## Admin tasks (the only things you need to do)

1. **Adjust bricks** — admin panel → Brick Adjustment
2. **Delete a player** — admin panel → Players → ✕
3. **Resolve Chaos predictions** — admin panel → still there for manual resolution
4. Password: `finance`

## Update frequency

- **Game data** (scores, bricks, standings) — updates every 15 minutes automatically
- **Code** (index.html) — only when you push a new version to GitHub
- Version badge shown on login screen (bottom-right)

## Firebase project
- Project ID: `worldcup2026-4d178`
- Region: Europe West 1
- Auth domain: `worldcup2026-4d178.firebaseapp.com`
