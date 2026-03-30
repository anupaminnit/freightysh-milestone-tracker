# Freightysh Shipment Tracker — v2

Role-based milestone tracker with authentication, file attachments, and real-time sync.  
Stack: **GitHub Pages** (hosting) + **Supabase** (Postgres DB + Auth + Storage).

**Repository:** [github.com/anupaminnit/freightysh-milestone-tracker](https://github.com/anupaminnit/freightysh-milestone-tracker)  
**Live App:** [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)

---

## What's New in v2

| Feature | v1 | v2 |
|---------|----|----|
| Login system | ❌ | ✅ Email + password auth |
| Role-based access | ❌ | ✅ Freightysh / CHA / Shipper |
| CHA sees only their consignments | ❌ | ✅ Enforced via RLS |
| File attachments per milestone | ❌ | ✅ Supabase Storage |
| Party-specific upload permissions | ❌ | ✅ Upload only on your milestones |
| Data persistence | Browser only | ✅ Supabase Postgres |
| Real-time sync | ✅ | ✅ |

---

## Role Permissions

| Action | Freightysh | CHA | Shipper |
|--------|-----------|-----|---------|
| Create consignments | ✅ | ❌ | ❌ |
| View all consignments | ✅ | ❌ (own only) | ❌ (own only) |
| Delete consignments | ✅ | ❌ | ❌ |
| Toggle CHA milestones (M2,4,5,6,7,8,10,11,12) | ✅ | ✅ | ❌ |
| Toggle Shipper milestones (M1, M3) | ✅ | ❌ | ✅ |
| Toggle Freightysh milestones (M9,13,14,15) | ✅ | ❌ | ❌ |
| Upload attachments | Anywhere | CHA milestones | Shipper milestones |
| Add notes | Any milestone | Any milestone | Any milestone |
| Reset a milestone | ✅ | ❌ | ❌ |

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

> **Note:** The schema drops and recreates all tables. If migrating from v1, your old data will be removed.

---

### Step 3 — Create the Storage Bucket

1. In Supabase: go to **Storage**
2. Click **New bucket**
3. Name: `attachments`
4. Toggle **Public bucket: ON**
5. Click **Create bucket**

Then, in **SQL Editor**, run these three storage policies from the bottom of `schema.sql`:

```sql
create policy "storage_authenticated_upload" ...
create policy "storage_authenticated_read" ...
create policy "storage_authenticated_delete" ...
```

---

### Step 4 — Disable Email Confirmation (for internal use)

1. Go to **Authentication → Providers → Email**
2. Toggle **Confirm email: OFF**
3. Click **Save**

> This lets your team sign in immediately after registering. For a public-facing app, leave this on.

---

### Step 5 — Get Your API Credentials

1. Go to **Project Settings → API**
2. Copy:
   - **Project URL** — `https://xxxxxxxxxxxx.supabase.co`
   - **anon public** key — long `eyJ...` string

---

### Step 6 — Deploy to GitHub Pages

1. Push `index.html` to the root of [github.com/anupaminnit/freightysh-milestone-tracker](https://github.com/anupaminnit/freightysh-milestone-tracker)
2. Go to **Settings → Pages → Deploy from main branch / root**
3. Your app will be live at: [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)

---

### Step 7 — First Launch

1. Open [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker)
2. **Setup screen**: paste your Supabase Project URL + anon key → **Save & Continue**
3. **Auth screen**: click **Register** and create your Freightysh account (select role: *Freightysh — Internal Team*)
4. You're in. Create your first consignment.

---

## Onboarding CHA and Shippers

### How to give CHA access to a consignment

1. When creating a consignment, enter the CHA's email in the **CHA Email** field
2. Share [anupaminnit.github.io/freightysh-milestone-tracker](https://anupaminnit.github.io/freightysh-milestone-tracker) with your CHA
3. CHA goes to the URL → **Register** → enters their email (must match exactly) → selects role **CHA**
4. On login, they will see only consignments where their email was entered as CHA Email
5. They can toggle CHA milestones and upload documents to CHA-tagged milestones

### How to give Shipper access

Same flow — enter their email in **Shipper Email** during consignment creation. Shipper registers with that email and sees only their consignment.

> **Important:** The email must match exactly — tell CHAs and Shippers to register with the same email you entered in the consignment form.

---

## File Attachments

- Accepted formats: PDF, JPG, PNG, XLSX, XLS, DOC, DOCX, CSV
- Uploaded files appear as clickable chips under each milestone
- Files open in a new browser tab
- CHA can only upload to CHA milestones; Shippers to Shipper milestones; Freightysh anywhere
- Files are stored in your Supabase project's Storage bucket

---

## Supabase Free Tier Limits

| Resource | Free Limit | Notes |
|----------|------------|-------|
| Database | 500 MB | Thousands of consignments |
| Storage | 1 GB | Plenty for shipping documents |
| API requests | 50,000/month | Well within range |
| Auth users | Unlimited | |
| Realtime | 200 concurrent connections | |

---

## File Structure

```
freightysh-milestone-tracker/
├── index.html    ← Entire app (single file, upload this to GitHub)
├── schema.sql    ← Run once in Supabase SQL Editor
└── README.md     ← This file
```

---

## Security Notes

- **Anon key**: Safe to expose — it's designed for client-side use. Supabase RLS policies enforce all access rules server-side.
- **Row Level Security**: All data access is enforced at the database level, not just the UI. Even if someone inspects the code, they cannot query data outside their role.
- **Freightysh role**: Currently self-selected on signup. For stricter control, you can add a DB trigger to reject `freightysh` role signups from unknown emails, or manually verify via Supabase dashboard.
- **Storage**: Files are in a public bucket (readable by URL). For sensitive documents, switch to a private bucket and use signed URLs (requires code change).

---

*Freightysh — a subordinate of MICBAC INDIA (OPC) PVT. LTD. and CARBON KING LLP*
