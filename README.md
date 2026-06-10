# UCSF CARE Dashboard

An internal link management dashboard for the UCSF CARE team. Organizes Box document links for staff and external partners, with shared data stored in Supabase and hosted via GitHub Pages.

---

## Access

**Live URL:** `https://careregistry.github.io/dashboard`

The dashboard has two sides:

- **Partners** — open access, no login required. Displays partner-facing documents organized by site and category.
- **Staff** — passcode protected. Allows adding, editing, deleting, and managing all links on both sides.

---

## Features

- Add, edit, and delete document links with version numbering
- Pin up to 3 documents per section
- Filter by category, partner site, and "Needs review" status
- Search across all links
- Grid and list view toggle
- CSV bulk import
- Custom categories
- Quick Resources section on the landing page (editable by staff)
- All data shared in real time across team members via Supabase

---

## Tech Stack

| Layer | Service | Cost |
|---|---|---|
| Frontend | GitHub Pages | Free |
| Database | Supabase (PostgreSQL) | Free tier |
| Auth | SHA-256 passcode hash stored in Supabase | — |

---

## Database Tables (Supabase)

| Table | Purpose |
|---|---|
| `links` | All staff and partner document links |
| `custom_cats` | User-created categories |
| `partner_sites` | Pre-seeded list of partner site names |
| `quick_resources` | Landing page quick resource links |
| `settings` | Stores hashed staff passcode |

---

## Updating the Dashboard

To publish changes, upload the updated `index.html` to the GitHub repo:

1. Go to the repo on GitHub
2. Click **Add file → Upload files**
3. Upload the new file (must be named `index.html`)
4. Click **Commit changes**
5. Hard refresh the live URL after ~1 minute (`Cmd+Shift+R` on Mac, `Ctrl+Shift+R` on Windows)

---

## Resetting the Staff Passcode

If the passcode is forgotten, run this in the **Supabase SQL Editor** with the new password:

```sql
update settings
set value = encode(sha256('YourNewPassword'::bytea), 'hex')
where key = 'staff_passcode_hash';
```

---

## Adding a New Partner Site

New sites can be added on the fly from the Add Link form on the Partners side (Staff login required). They are saved to the `partner_sites` table and will appear in the dropdown for all users immediately.

---

## CSV Bulk Import

Staff can import links in bulk via CSV. Download the template from the Import button in the dashboard. Required columns:

| Column | Description |
|---|---|
| `name` | Document name (version number added automatically) |
| `url` | Full URL |
| `side` | `staff` or `partners` |
| `categories` | Comma-separated (e.g. `Flyers,Multilingual`) |
| `version` | Version number (e.g. `1`) |
| `last_updated` | Date in `YYYY-MM-DD` format |
| `needs_review` | `true` or `false` |
| `site` | Partner site name (required for partners side) |

---

## Maintainer

This dashboard is owned and maintained by Emily — UCSF CARE.
