# Freightysh Shipment Tracker

Role-based shipment milestone tracker with authentication, mandatory file attachments, and real-time sync.  
Stack: **GitHub Pages** (hosting) + **Supabase** (Postgres DB + Auth + Storage).

**Repository:** [github.com/anupaminnit/freightysh-milestone-tracker](https://github.com/anupaminnit/freightysh-milestone-tracker)  
**Live App:** [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)  
**Current Version:** `v1.5`

---

## Features

| Feature | Status |
|---------|--------|
| Email + password authentication | ✅ |
| Role-based access (Freightysh / CHA / Shipper) | ✅ |
| CHA sees only consignments assigned to them | ✅ |
| Shipper sees only their own consignment | ✅ |
| 15-step milestone tracking per consignment | ✅ |
| Mandatory attachment before milestone completion | ✅ |
| Party-specific upload permissions | ✅ |
| Per-milestone notes | ✅ |
| Real-time sync across devices | ✅ |
| CSV export | ✅ |
| Supabase Postgres persistence | ✅ |

---

## Role Permissions

| Action | Freightysh | CHA | Shipper |
|--------|-----------|-----|---------|
| Create consignments | ✅ | ❌ | ❌ |
| View all consignments | ✅ | ❌ own only | ❌ own only |
| Delete consignments | ✅ | ❌ | ❌ |
| Reset a milestone | ✅ | ❌ | ❌ |
| Toggle CHA milestones (M2,4,5,6,7,8,10,11,12) | ✅ | ✅ | ❌ |
| Toggle Shipper milestones (M1, M3) | ✅ | ❌ | ✅ |
| Toggle Freightysh milestones (M9,13,14,15) | ✅ | ❌ | ❌ |
| Upload attachments | Any milestone | CHA milestones only | Shipper milestones only |
| Add notes | Any milestone | Any milestone | Any milestone |

> **Attachment rule:** A milestone cannot be marked complete without at least one document uploaded. The checkbox is blocked and a warning is shown until a file is attached.

---

## The 15 Milestones

| # | Milestone | Responsible Party |
|---|-----------|------------------|
| M1 | Goods released from factory via truck | Shipper |
| M2 | Checklist sent by CHA | CHA |
| M3 | Checklist approved by shipping party | Shipper |
| M4 | CHA files for shipping bill | CHA |
| M5 | Goods received at CHA warehouse | CHA |
| M6 | Empty container picked & container survey done | CHA |
| M7 | Loading, stuffing & sealing of container | CHA |
| M8 | Weighing of container (VGM) | CHA |
| M9 | SI submission | Freightysh |
| M10 | Filing of VGM | CHA |
| M11 | LEO copy & export ready report | CHA |
| M12 | Filing of e-Shipping Bill | CHA |
| M13 | Verification of draft BL | Freightysh |
| M14 | Payment for generated invoice | Freightysh |
| M15 | Sharing of Seaway BL | Freightysh |

---

## Setup Guide

### Step 1 — Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) → sign up free
2. Click **New Project**
3. Name: `freightysh-tracker` | Region: `ap-south-1` (Mumbai) | Set a DB password
4. Wait ~2 minutes for the project to initialize

---

### Step 2 — Run the Database Schema

1. In your Supabase project: **SQL Editor → New Query**
2. Paste the entire contents of `schema.sql`
3. Click **Run**
4. You should see: `Success. No rows returned`

> **Note:** The schema drops and recreates all tables on each run. Back up any data before re-running.

---

### Step 3 — Create the Storage Bucket

1. In Supabase: go to **Storage**
2. Click **New bucket**
3. Name: `attachments` | Toggle **Public bucket: ON** | Click **Create bucket**
4. Go to **SQL Editor → New Query** and run the three storage policies from the bottom of `schema.sql` (uncomment them first)

---

### Step 4 — Configure Authentication

1. Go to **Authentication → Providers → Email**
2. Toggle **Enable Email Provider: ON**
3. Toggle **Confirm email: OFF** (for internal team use)
4. Click **Save**
5. Go to **Authentication → Rate Limits**
6. Set **Email signups** to `100` per hour → **Save**

---

### Step 5 — Get Your API Credentials

1. Go to **Project Settings → API**
2. Copy:
   - **Project URL** — `https://xxxxxxxxxxxx.supabase.co`
   - **anon public** key — long `eyJ...` string

---

### Step 6 — Deploy to GitHub Pages

1. Push `index.html` to the root of [github.com/anupaminnit/freightysh-milestone-tracker](https://github.com/anupaminnit/freightysh-milestone-tracker)
2. Go to **Settings → Pages → Source: Deploy from branch → main / root** → **Save**
3. App goes live at: [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)

---

### Step 7 — First Launch

1. Open [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)
2. **Setup screen**: paste your Supabase Project URL + anon key → **Save & Continue**
3. **Register tab**: enter your name, email, password → select role **Freightysh — Internal Team** → **Create Account**
4. You are signed in automatically. Create your first consignment.

---

## Onboarding CHA and Shippers

### How to give CHA access

1. When creating a consignment, enter the CHA's email in the **CHA Email** field
2. Share the app URL with your CHA: [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)
3. CHA opens the URL → enters Supabase credentials (same as yours) → **Register** with their email → selects role **CHA**
4. They will only see consignments where their email is set as CHA Email
5. They can tick CHA milestones and upload documents to CHA-tagged milestones only

### How to give Shipper access

Same flow — enter their email in the **Shipper Email** field during consignment creation. Shipper registers with that exact email and selects role **Shipper**.

> **Important:** The email entered in the consignment form must exactly match the email used to register. Case-sensitive.

---

## File Attachments

- **Mandatory** — a milestone cannot be marked complete without at least one attachment
- Accepted formats: PDF, JPG, PNG, XLSX, XLS, DOC, DOCX, CSV
- Uploaded files appear as clickable chips under each milestone, opening in a new tab
- Files are stored in your Supabase Storage bucket (`attachments`)
- Upload permissions are role-scoped: CHA → CHA milestones, Shipper → Shipper milestones, Freightysh → anywhere

---

## Supabase Free Tier Limits

| Resource | Free Limit | Notes |
|----------|------------|-------|
| Database | 500 MB | Comfortably handles thousands of consignments |
| Storage | 1 GB | Ample for shipping documents |
| API requests | 50,000 / month | Well within range for a small team |
| Auth users | Unlimited | |
| Realtime connections | 200 concurrent | More than enough |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| `Email signups are disabled` | Authentication → Providers → Email → Enable Email Provider: ON |
| `Email not confirmed` | Authentication → Providers → Email → Confirm email: OFF |
| `Email rate limit exceeded` | Authentication → Rate Limits → increase Email signups to 100/hr |
| `relation "profiles" does not exist` | Re-run `schema.sql` — tables were not created in correct order |
| Profile not found after login | Delete the user from Authentication → Users in Supabase, then register fresh |

---

## File Structure

```
freightysh-milestone-tracker/
├── index.html    ← Entire app (upload this to GitHub)
├── schema.sql    ← Run once in Supabase SQL Editor
└── README.md     ← This file
```

---

## Version History

| Version | Changes |
|---------|---------|
| v1.0 | Initial localStorage-only tracker |
| v1.1 | Supabase integration, real-time sync, multi-device |
| v1.2 | Auth system, role-based access, file attachments, RLS |
| v1.3 | Fixed signup flow — trigger-based profile creation, session fix |
| v1.4 | Proper signup architecture — signIn after signUp guarantees session |
| v1.5 | Mandatory attachments before milestone completion, version shown on all screens |

---

## Security Notes

- **Anon key**: Safe to expose in the browser — designed for client-side use. All access rules are enforced server-side via RLS.
- **Row Level Security**: CHA and Shipper cannot query any data outside their assigned consignment, even via the API directly.
- **Profile creation**: Handled entirely by a server-side DB trigger — no client-side writes to the profiles table.
- **Freightysh role**: Self-selected on signup. For stricter control, delete any unauthorized Freightysh registrations via Supabase Dashboard → Authentication → Users.
- **Storage**: Files are in a public bucket (URL-accessible). For sensitive documents, switch to a private bucket with signed URLs.

---

*Freightysh — a subordinate of MICBAC INDIA (OPC) PVT. LTD. and CARBON KING LLP*
