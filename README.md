# Freightysh Shipment Milestone Tracker

A real-time, multi-device shipment milestone tracker for Freightysh — built with plain HTML/JS and powered by Supabase. Hosted free on GitHub Pages.

---

## Stack

| Layer | Technology | Cost |
|-------|------------|------|
| Hosting | GitHub Pages | Free |
| Database | Supabase (PostgreSQL) | Free tier |
| Realtime | Supabase Realtime | Free tier |
| Frontend | Vanilla HTML/CSS/JS | — |

---

## One-time Setup

### Step 1 — Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) and sign up (free)
2. Click **New Project**
3. Choose a name (e.g. `freightysh-tracker`), set a strong database password, pick a region close to India (e.g. `ap-south-1` Mumbai)
4. Wait ~2 minutes for the project to spin up

### Step 2 — Run the Database Schema

1. In your Supabase project, go to **SQL Editor** → **New Query**
2. Open the `schema.sql` file from this repo
3. Paste the entire contents and click **Run**
4. You should see: `Success. No rows returned`

### Step 3 — Get Your API Credentials

1. Go to **Project Settings** (gear icon) → **API**
2. Copy:
   - **Project URL** — looks like `https://xxxxxxxxxxxx.supabase.co`
   - **anon public** key — a long JWT string starting with `eyJ...`

### Step 4 — Host on GitHub Pages

1. Create a new GitHub repository (e.g. `freightysh-tracker`)
   - Set it to **Private** if you want restricted access
2. Upload `index.html` to the repo root
3. Go to **Settings** → **Pages**
4. Under **Source**, select `Deploy from a branch`
5. Choose `main` branch, `/ (root)` folder → **Save**
6. Your app will be live at:
   ```
   https://yourusername.github.io/freightysh-tracker
   ```

### Step 5 — First Launch

1. Open your GitHub Pages URL
2. You'll see the **Connect to Supabase** screen
3. Paste your Project URL and anon key
4. Click **Connect & Launch App**
5. Done — your credentials are saved in the browser. You won't need to enter them again on that device.

---

## Multi-Device Access

To use the tracker on another device (phone, another laptop, share with CHA):

1. Open the GitHub Pages URL on the new device
2. Enter the same Supabase Project URL and anon key
3. All shipments and milestones will load instantly

> **Tip:** Save the URL as a bookmark on all devices you use regularly.

---

## Features

- **15 milestones per consignment** with party tags (CHA / Shipper / Freightysh / Consignee)
- **Real-time sync** — updates appear live across all connected devices
- **Per-milestone notes** — record rejection reasons, remarks, timestamps
- **Date tracking** — log when each milestone was completed
- **Progress dashboard** — visual progress bars, status badges, next-milestone column
- **CSV export** — one-click export of all consignment data with milestone dates
- **Search & filter** — search by ref, route, vessel; filter by status
- **Freightysh branding** — navy + orange colour scheme throughout

---

## Supabase Free Tier Limits

| Resource | Free Limit | Your Expected Usage |
|----------|------------|---------------------|
| Database size | 500 MB | ~50,000 consignments |
| API requests | 50,000/month | Well within range |
| Realtime connections | 200 concurrent | More than enough |
| Bandwidth | 5 GB/month | More than enough |

You will not hit these limits at Freightysh's current scale.

---

## Backup

Use the **Export CSV** button in the app regularly. This gives you a full spreadsheet backup of all consignments and milestone dates that you can archive or share with clients.

---

## Security Note

The `anon` key in Supabase is designed to be public-facing — it's safe to use in a browser app. The Row Level Security (RLS) policy in `schema.sql` controls what operations are allowed. For a small internal team, the current open policy is fine. If you later want login-protected access (only your team can see data), Supabase Auth can be added.

---

## File Structure

```
freightysh-tracker/
├── index.html      ← The entire app (single file)
├── schema.sql      ← Run this once in Supabase SQL Editor
└── README.md       ← This file
```

---

*Built for Freightysh — a subordinate of MICBAC INDIA (OPC) PVT. LTD. and CARBON KING LLP*
