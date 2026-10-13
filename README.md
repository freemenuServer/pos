# FreeMenu POS — POS-only package

This package contains only POS-related files. The public FreeMenu website pages, marketing pages, online menu pages, owner website dashboard, CNAME and website assets are intentionally excluded.

## Files
- `index.html` — POS application
- `superadmin.html` — Super Admin interface
- `config.js` — browser-safe Supabase URL and publishable key configuration
- `setup.sql` — POS order/KOT database additions and policies; review before running
- `assets/freemenu-logo.png` — logo used by POS

## Run
Serve this folder from a local web server or your intended POS hosting environment. `index.html` and `superadmin.html` load `config.js` from the same folder.

## Supabase safety
- `config.js` must contain only a publishable/anon key. Never put a `service_role` key in browser files.
- Review `setup.sql` against the current database and RLS policies before executing it. Back up the database first.
- A successful local page load does not prove live database, billing, reports, admin permissions, or printer integration has been end-to-end tested.
