# Curator Engine Docs Assets

This folder contains documentation assets for the self-hosted Curator Engine product at `curator.local`.

## Screenshot standards

- Desktop screenshots: `1440x900`
- Mobile screenshots: `390x844`
- Format: PNG
- Source: real UI captured from the running engine and logged-in local admin session
- Redaction: sensitive account data must be cropped or covered before commit

## Screenshots

- `01-engine-home-desktop.png`: Curator engine home page on desktop.
- `02-engine-home-mobile.png`: Curator engine home page in a narrow mobile viewport.
- `03-engine-search-results-desktop.png`: Search results view for a live engine query on desktop.
- `04-engine-search-results-mobile.png`: Search results view for the same query on mobile.
- `05-engine-store-products-desktop.png`: Engine store products page.
- `06-engine-login.png`: Login screen.
- `07-engine-register.png`: Registration screen.
- `08-engine-cart.png`: Cart page UI.
- `09-engine-account-downloads.png`: Signed-in account downloads page.
- `10-engine-account-orders.png`: Signed-in account orders page.
- `11-engine-checkout.png`: Checkout page state from the logged-in engine session.
- `12-engine-admin-dashboard.png`: Main admin shell.
- `13-engine-admin-products.png`: Admin products view.
- `14-engine-admin-orders.png`: Admin orders view.
- `15-engine-admin-users.png`: Admin user access view with sensitive email content redacted.
- `16-engine-admin-database-settings.png`: Admin database settings page.
- `17-engine-admin-download-settings.png`: Admin download settings page.
- `18-engine-admin-search-tools.png`: Admin search tools page.
- `19-engine-admin-themes.png`: Admin themes page.
- `20-engine-admin-plugins.png`: Admin plugins page.

## Diagrams

- `01-request-lifecycle.mmd`: Request routing from browser to public or admin response.
- `02-theme-rendering.mmd`: Theme rendering flow from route handler to final HTML.
- `03-plugin-lifecycle-safe-mode.mmd`: Plugin boot flow and safe mode behavior.
- `04-admin-bootstrap-flow.mmd`: First-admin bootstrap flow for `/setup`.
- `05-store-download-flow.mmd`: Download delivery flow from account page to streamed file.

## Reminder

- Do not commit secrets, tokens, passwords, or private operational data.
- Keep this folder limited to documentation assets only.
