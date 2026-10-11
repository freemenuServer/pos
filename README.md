# FreeMenu POS (browser edition)

## Included
- `index.html` — responsive POS interface (menu, cart, tables, KOT, order history)
- `config.js` — existing Supabase project URL + public publishable key from the supplied FreeMenu source
- `setup.sql` — additive POS tables and owner-based RLS policies
- `assets/freemenu-logo.png` — FreeMenu logo extracted from the supplied ZIP

## Important: do not treat this as production-ready until tested
The supplied website source identifies the Supabase project and public publishable key. A publishable/anon key is expected to be present in browser code; **never put a `service_role` or secret key in this folder**.
The POS schema is additive and does not alter existing tables. Before running SQL, inspect your existing `restaurants` table. The included policies assume `restaurants.owner_id` references `auth.users.id`. If your project uses a different owner/admin mapping, adapt those policies before enabling real POS use. Never disable RLS or make POS order tables public.
The UI assumes existing `menu_items` has `restaurant_id`, `name`, `category`, `price` (or price_full/price_half/price_quarter), `is_available`, `is_veg`, `item_type`, `image_url`, `description`, and optional `sort_order`. Missing optional fields should be fine; if `sort_order` does not exist, remove `.order("sort_order", ...)` in `index.html`.
The initial app selects a restaurant visible under the signed-in user's existing RLS policies. If no restaurant is visible, fix the existing read policy/owner assignment rather than exposing all restaurant data publicly.
Order insert uses `pos_orders`, `pos_order_items`, and `pos_kot_tickets`. A complete bill here records an order only; it does not collect or process a payment.

## GitHub Pages deployment
1. Upload `index.html`, `config.js`, `setup.sql`, and `assets/` to the root of your separate `freemenuServer/pos` repository (same `main` branch selected in Pages).
2. In Supabase SQL Editor, review `setup.sql` and the owner field assumption. Run it only after confirming it matches your schema. It creates only `pos_*` tables and policies.
3. GitHub repository → Settings → Pages → Custom domain should be `pos.freemenu.in`.
4. Cloudflare DNS record: `CNAME`, name `pos`, target `freemenuserver.github.io`. Keep the existing `freemenu.in` and `www` records unchanged.
5. Wait for GitHub Pages to issue the TLS certificate, then enable Enforce HTTPS.
6. Open `https://pos.freemenu.in`, sign in with an existing Supabase Auth account, test menu read, KOT, order creation and order history using a test restaurant/order first.

## Brand
Logo file is from the user-provided FreeMenu project archive. The POS title/brand reads “FreeMenu POS”.
