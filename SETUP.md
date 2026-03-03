# 🪖 HOSTILE TAKEOVER — Setup Guide

## What you get
A war-room style live dashboard deployed on Vercel that reads your Google Sheet every 10 seconds. No backend, no database, no API keys.

---

## Step 1 — Set up your Google Sheet

Create a new Google Sheet with **two tabs** named exactly as shown:

### Tab 1: `Teams`

| TeamName | Capital | Tanks | NavySeals | AirForce | TanksDestroyed | NavyDestroyed | AirDestroyed |
|----------|---------|-------|-----------|----------|----------------|---------------|--------------|
| Alpha Squad | 50 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bravo Unit  | 50 | 0 | 0 | 0 | 0 | 0 | 0 |
| Charlie Co  | 50 | 0 | 0 | 0 | 0 | 0 | 0 |

- **Capital** — current capital points for the team (start at 50)
- **Tanks / NavySeals / AirForce** — total vehicles *purchased* by the team
- **TanksDestroyed / NavyDestroyed / AirDestroyed** — number destroyed so far

The dashboard shows **Alive = Owned − Destroyed** automatically.

---

### Tab 2: `Stocks`

| Department | Price | Change |
|------------|-------|--------|
| TANK       | 100   | 0      |
| NAVY       | 100   | 0      |
| AIR        | 100   | 0      |

- **Department** must be exactly: `TANK`, `NAVY`, `AIR`
- **Price** — current stock price
- **Change** — delta from last round (positive or negative)

---

## Step 2 — Make the Sheet public

1. Click **Share** (top right)
2. Change access to **"Anyone with the link"** → **Viewer**
3. Copy the Sheet URL — you need the **Sheet ID** from it:
   ```
   https://docs.google.com/spreadsheets/d/  >>>SHEET_ID<<<  /edit
   ```

---

## Step 3 — Deploy to Vercel

### Option A — GitHub (recommended)
1. Push this folder to a GitHub repo
2. Go to [vercel.com](https://vercel.com) → **New Project** → Import your repo
3. Framework: **Other** | Root Directory: `/` | No build command needed
4. Click **Deploy** — done in 30 seconds

### Option B — Vercel CLI
```bash
npm i -g vercel
cd hostile-takeover
vercel --prod
```

---

## Step 4 — Connect in the browser

1. Open your Vercel URL
2. Paste your **Sheet ID** into the input field
3. Click **▶ CONNECT SHEET**
4. The dashboard starts live — refreshes every 10 seconds automatically

---

## How facilitators update the game

Facilitators just **edit the Google Sheet directly** — no special tools needed:

| Action | What to change in sheet |
|--------|------------------------|
| Team buys a Tank | Increase `Tanks` column for that team |
| Tank gets destroyed | Increase `TanksDestroyed` column |
| Capital spent | Decrease `Capital` column |
| Stock price changes | Edit `Price` and `Change` in Stocks tab |
| New team joins | Add a new row to Teams tab |

Multiple facilitators can edit simultaneously using Google Sheets' real-time collaboration — the dashboard will pick up all changes within 10 seconds.

---

## Tips

- Sort teams in the sheet however you like — the dashboard re-sorts by Capital automatically
- The leader gets a gold crown 👑 and glowing border
- A scrolling ticker at the top shows live stock prices + team standings
- The sheet ID is remembered in the browser — viewers only need to paste it once
