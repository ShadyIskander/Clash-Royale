# ⚔ CLASH ROYALE — Setup Guide

## What you get
A war-room style live dashboard deployed on Vercel that reads your Google Sheet every 5 seconds. No backend, no database, no API keys.

---

## Step 1 — Set up your Google Sheet

Create a new Google Sheet with **three tabs** named exactly as shown below.

---

### Tab 1: `Teams`

| TeamName | Capital | Tanks | NavySeals | AirForce | TanksDestroyed | NavyDestroyed | AirDestroyed | BonusPoints | TankStocks | NavyStocks | AirStocks |
|----------|---------|-------|-----------|----------|----------------|---------------|--------------|-------------|------------|------------|-----------|
| Alpha Squad | 50 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bravo Unit  | 50 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Charlie Co  | 50 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

**Column reference:**

| Column | What it means | How it shows on the dashboard |
|--------|--------------|-------------------------------|
| `TeamName` | The team's display name | Shown as the card title |
| `Capital` | Current liquid capital | Shown under ◈ CAPITAL, used in Score, and in the "💧 Liquid Capital" breakdown row |
| `Tanks` | Total tanks purchased | Used to calculate alive tanks (Tanks − TanksDestroyed) |
| `NavySeals` | Total navy seals purchased | Used to calculate alive navy seals |
| `AirForce` | Total air force purchased | Used to calculate alive air force |
| `TanksDestroyed` | Number of tanks destroyed so far | Subtracted from Tanks to show alive count; shown as ☠ X lost on the vehicle tile |
| `NavyDestroyed` | Number of navy seals destroyed | Same as above for navy |
| `AirDestroyed` | Number of air force destroyed | Same as above for air |
| `BonusPoints` | Extra points awarded manually (e.g. battle rewards) | Shown as ⚔ BATTLE BONUS on the team card and added to the team's Score |
| `TankStocks` | Number of Tank stock shares the team holds | Shown as 🪖 Tank Stock in the wealth breakdown; value = TankStocks × TANK Price from Stocks tab |
| `NavyStocks` | Number of Navy stock shares the team holds | Shown as ⚓ Navy Stock in the wealth breakdown; value = NavyStocks × NAVY Price from Stocks tab |
| `AirStocks` | Number of Air stock shares the team holds | Shown as ✈ Air Stock in the wealth breakdown; value = AirStocks × AIR Price from Stocks tab |

**How the team card is built:**
- **SCORE** (top right of card) = `Capital` + alive vehicles × their Points value + `BonusPoints`
- **◈ CAPITAL** section shows the Capital value with a progress bar, a "💧 Liquid Capital" row, and a "🚀 Alive Vehicles" count
- **Stock portfolio** is shown inside the capital section — 🪖 Tank Stock, ⚓ Navy Stock, ✈ Air Stock — each displaying the number of shares held. Their combined value (shares × current stock price) is added to **Total Wealth**
- **⚔ BATTLE BONUS** row appears on the card whenever `BonusPoints` is greater than 0
- **◈ VEHICLES** section shows three tiles — 🪖 TANK, ⚓ NAVY, ✈ AIR — each displaying the alive count and a ☠ lost count if any are destroyed. A fully wiped vehicle type fades out on the card.
- Teams are **auto-sorted by Score**, highest first. The leader gets 👑 #1 and a glowing border.

---

### Tab 2: `Stocks`

| Department | Price | Change |
|------------|-------|--------|
| TANK       | 100   | 0      |
| NAVY       | 100   | 0      |
| AIR        | 100   | 0      |

**Column reference:**

| Column | What it means | How it shows on the dashboard |
|--------|--------------|-------------------------------|
| `Department` | Asset identifier — must be exactly `TANK`, `NAVY`, or `AIR` | Used to match stock data to the correct market card |
| `Price` | Current stock price | Shown on the **MARKET PRICES** panel at the top of the page, and in the scrolling ticker as e.g. `TANK Stock 100` |
| `Change` | Price delta from last round (positive or negative) | Shown with ▲ green for gains, ▼ red for drops, — for no change — both on the market card and in the ticker |

**How it shows on the dashboard:**
- The **scrolling ticker** at the very top of the page cycles through all three stock prices continuously, e.g. `TANK Stock 100 ▲+10 · NAVY Stock 80 — · AIR Stock 100 ▼-5`
- The **MARKET PRICES** panel shows one card per asset with the price large and the change indicator below it

---

### Tab 3: `Prices`

| Vehicle | Price | Points |
|---------|-------|--------|
| Tanks | 60 | 8 |
| NavySeals | 80 | 12 |
| AirForce | 100 | 18 |
| Bazooka | 150 | — |
| BazookaAttack | 20 | Half the points of the destroyed vehicle |

**Column reference:**

| Column | What it means | How it shows on the dashboard |
|--------|--------------|-------------------------------|
| `Vehicle` | Asset name — must match exactly: `Tanks`, `NavySeals`, `AirForce`, `Bazooka`, `BazookaAttack` | Used to look up cost and point values |
| `Price` | Capital cost to purchase or use | Shown on the **MARKET & VEHICLE REFERENCE** panel as "Cost — capital / unit" |
| `Points` | Score points earned per alive unit | Used to calculate each team's Score; shown as "Points — pts / unit" on the reference panel |

**How it shows on the dashboard:**
- The **MARKET & VEHICLE REFERENCE** panel shows one card per vehicle type with its purchase cost and point value
- **Tanks** cost 60 capital, worth 8 pts each while alive
- **NavySeals** cost 80 capital, worth 12 pts each while alive
- **AirForce** cost 100 capital, worth 18 pts each while alive
- **Bazooka** costs 150 capital to build; no point value — it's an attack weapon
- **BazookaAttack** costs 20 capital per launch; awards the attacker half the point value of whatever vehicle they destroy

---

## Step 2 — Make the Sheet public

1. Click **Share** (top right)
2. Change access to **"Anyone with the link"** → **Viewer**
3. Copy the Sheet ID from the URL:
   ```
   https://docs.google.com/spreadsheets/d/  >>>SHEET_ID<<<  /edit
   ```

---

## Step 3 — Deploy to Vercel

### Option A — GitHub (recommended)
1. Push `index.html` to a GitHub repo
2. Go to [vercel.com](https://vercel.com) → **New Project** → Import your repo
3. Framework: **Other** | Root Directory: `/` | No build command needed
4. Click **Deploy** — done in ~30 seconds

### Option B — Vercel CLI
```bash
npm i -g vercel
vercel --prod
```

---

## Step 4 — Open the dashboard

1. Open your Vercel URL
2. The dashboard connects automatically and refreshes every **5 seconds**

---

## How scoring works

**Score = Capital + (Alive Vehicles × Points per vehicle)**

Each vehicle type has a different point value defined in the Prices tab:
- 🪖 Tanks → 8 pts each
- ⚓ NavySeals → 12 pts each
- ✈ AirForce → 18 pts each

Example: a team with 40 capital, 2 alive tanks, and 1 alive navy seal scores **40 + 16 + 12 = 68 pts**

---

## How facilitators update the game

Facilitators just **edit the Google Sheet directly** — no special tools needed:

| Action | What to change in sheet |
|--------|------------------------|
| Team buys a Tank | Increase `Tanks` for that team; decrease `Capital` by 60 |
| Team buys a Navy Seal | Increase `NavySeals`; decrease `Capital` by 80 |
| Team buys Air Force | Increase `AirForce`; decrease `Capital` by 100 |
| Team builds a Bazooka | Decrease `Capital` by 150 |
| Team launches a Bazooka attack | Decrease attacker's `Capital` by 20; increase the matching `*Destroyed` column for the target team |
| Vehicle gets destroyed | Increase the matching `*Destroyed` column for the target team |
| Award a battle bonus | Increase `BonusPoints` for that team |
| Team buys stock shares | Increase `TankStocks`, `NavyStocks`, or `AirStocks` for that team |
| Stock price changes | Edit `Price` and `Change` in the Stocks tab |
| New team joins | Add a new row to the Teams tab |

Multiple facilitators can edit simultaneously — the dashboard picks up all changes within 5 seconds.

---

## Tips

- Sort teams in the sheet however you like — the dashboard re-sorts by Score automatically
- The leader gets a gold crown 👑 and a glowing border
- A scrolling ticker at the top shows live stock prices and team standings
- Destroyed vehicles show a ☠ lost count on the vehicle tile
- A fully wiped vehicle type (all destroyed) fades out automatically on the card
