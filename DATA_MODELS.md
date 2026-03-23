# Data Models (Database Overview)

This document explains the core database tables Curator uses and how they fit together.

> Notes:
> - Exact table/column names can vary by build.
> - Treat this as the “mental model” for the platform.
> - If your project ships `install/curator_schema.sql`, that SQL is the definitive schema.

---

## Core tables (engine)

### `settings`
Stores key/value configuration used by the engine and admin.

Common examples:
- store name, currency, support email
- download defaults (expiry days, max downloads)
- SMTP configuration (non-secret fields)
- feature flags (bundles enabled, etc.)

---

## Users & authentication

### `users`
Stores customer and admin accounts.

Typical fields:
- id, email, password_hash
- role/capabilities (admin/reviewer/developer/customer)
- marketing opt-in flags (if enabled)

If you segment users (developer vs customer), it may be stored as a role or flags.

---

## Catalog (products)

### `products`
The primary catalog table.

Typical fields:
- title, slug, short_description
- price_cents, currency
- status (draft/active/archived)
- rich content fields (description_html, highlights/features/faq JSON)

### `categories`
Stores product categories.

### `tags`
Stores tag definitions.

### `product_tags`
Join table linking products ↔ tags.

---

## Media and files

### `product_images`
Stores product screenshots/gallery images:
- product_id, path, alt_text, sort_order

### `product_files`
Stores downloadable files attached to a product:
- label (e.g., AppImage, PDF)
- version (optional)
- file_path, file_name, file_size, sha256

These files are delivered to customers via download tokens, not direct file links.

---

## Cart and orders (commerce)

### `carts`
Tracks the current cart for a session/user.

### `cart_items`
Items in a cart:
- cart_id, product_id, quantity

### `orders`
Represents a completed (or pending) purchase.

Typical fields:
- order_number
- user_id (account required in your current model)
- status (pending/paid/failed/refunded/cancelled)
- subtotal_cents, tax_cents, shipping_cents, total_cents
- gateway_id (stripe/paypal)
- created_at, paid_at

Tax-by-location installs often store a destination snapshot:
- tax_country, tax_state, tax_city, tax_postal (optional)

### `order_items`
Snapshot of what was purchased:
- product_id
- title_snapshot
- unit_price_cents
- quantity
- line_total_cents

---

## Downloads / fulfillment

### `download_tokens`
Tokenized entitlement per file.

Typical fields:
- token (unique)
- user_id
- order_id
- product_file_id
- expires_at (nullable)
- max_downloads
- download_count
- revoked_at (nullable)

Download behavior:
- token must belong to logged-in user
- order must be paid
- token must not be expired/revoked/maxed

### ZIP bundles (optional behavior)
If “Download all as ZIP” is supported, the ZIP is generated on demand and does not need a permanent table by default. Some installs log ZIP download events.

---

## Email system

### `email_queue`
Queued emails (transactional):
- to_email, subject, html_body
- status (queued/sent/failed)
- attempts, last_error
- timestamps

SMTP secrets should be encrypted if stored in DB.

---

## Analytics / metrics (optional but common)

### Page/session tracking tables
Depends on your implementation. Common patterns:
- `sessions` (first-touch referrer/utm)
- `pageviews`
- `events` (clicks, downloads, add_to_cart)
- rollup tables for “popular” sorting

If you support “popular” search sorting:
- daily metrics table
- rollup totals (7/30 days)

---

## Marketplace tables (Curator Marketplace pipeline)

If you run Curator Marketplace, you’ll also have marketplace-specific models.

### `marketplace_items`
A listing for a plugin/theme/bundle:
- type (plugin/theme/bundle)
- slug, name, summary, description_html
- docs_url, support_url
- license_type, billing_type (free/paid later)
- compatibility fields
- status (draft/pending/published/suspended)

### `marketplace_releases`
Versioned artifacts for listings:
- version
- changelog_html
- submitted_zip_path
- published_zip_path
- status (pending/approved/published)

### `marketplace_review_checks`
Automated validation/security checks per release:
- pass/warn/fail + messages

### `marketplace_reviews`
Human reviewer decisions:
- approved/rejected/changes_requested + notes

### `marketplace_messages`
Reviewer ↔ developer communication thread.

### `marketplace_audit_log`
Critical actions log:
- publish, reject, setting changes, etc.

---

## How these tables connect (high level)

- A **product** can have many **product_files** and **product_images**
- A **user** places **orders**
- Each **order** has **order_items**
- A **paid order** generates **download_tokens** for every purchased product file
- The **marketplace** hosts **items** with versioned **releases**

---

## Troubleshooting tips

### “Downloads exist but user can’t download”
Check:
- order status is paid
- token belongs to the user
- token not expired/revoked/maxed

### “Search works but results are slow”
Check indexes:
- FULLTEXT on search fields (if used)
- indexes on status/category/price
- rollups for popularity sorting

---

## Next

- [Plugin Development](./PLUGINS.md)
- [Theme Development](./THEMES.md)
