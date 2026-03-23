Copy/paste this into docs/PLUGINS.md.
# Plugin Development

Curator plugins extend functionality: add admin pages, register routes, run migrations, enqueue assets, and hook into lifecycle events.

Plugins are installable ZIPs and can be enabled from **Admin → Plugins**.

---

## Plugin structure

A Curator plugin lives at:
Copy
plugins/<plugin-slug>/
plugin.json
plugin.php
migrations/ (optional)
001_init.sql
admin/ (optional)
settings.php
assets/ (optional)
plugin.css
plugin.js
views/ (optional)
*.php

> Keep plugin code scoped to its folder. Curator blocks path traversal and expects `plugin.json` to match the folder slug.

---

## `plugin.json` manifest

Every plugin must include `plugin.json`.

Example:

```json
{
  "slug": "hello-plugin",
  "name": "Hello Plugin",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "Example Curator plugin",
  "license": "MIT",
  "requires_core": ">=1.0.0",
  "entry": "plugin.php",
  "capabilities": [
    "register_routes",
    "register_admin_pages",
    "migrations",
    "enqueue_assets"
  ],
  "migrations": [
    "migrations/001_init.sql"
  ]
}
Copy
Rules
slug must match the folder name.
entry must exist.
Paths must not contain ...
capabilities must be from Curator’s allowlist.
Capabilities
Curator uses capabilities to keep plugins predictable and reviewable.
Common capabilities:
register_routes — register public endpoints
register_admin_pages — add admin pages/menus
migrations — run SQL migrations on enable
enqueue_assets — load CSS/JS
“High-risk” capabilities (require extra review):
anything that executes shell commands
anything that writes outside plugin storage
anything that fetches/executes remote code
Plugin lifecycle
Plugins load through the Plugin Manager:
installed (zip copied to /plugins/<slug>)
enabled (migrations run, plugin entry loaded)
disabled (plugin no longer loaded)
Safe Mode
Curator should support safe mode so a broken plugin cannot brick the site.
Hooks (actions/filters)
Curator supports WordPress-style hooks.
Actions
Register a callback:
add_action('init', function () {
  // initialize plugin (register services, etc.)
});
Copy
Fire an action (core side):
do_action('init');
Copy
Filters
Modify values:
add_filter('product_price_display', function ($value, $product) {
  return $value;
});
Copy
Apply a filter (core side):
$value = apply_filters('product_price_display', $value, $product);
Copy
Important hook points
Common hooks you can rely on:
init
routes_register
admin_menu_register
render_head
render_footer
Commerce hooks (if present in your build):
order_paid
checkout_started
download_served
Marketplace hooks (if present):
submission_received
release_published
If your project has a canonical hooks reference, link it here (recommended).
Registering routes
Curator plugin routes should be prefixed to avoid collisions:
/p/<plugin-slug>/...
Copy
Example:
add_action('routes_register', function ($router) {
  $router->get('/p/hello-plugin/health', function () {
    header('Content-Type: application/json');
    echo json_encode(['ok' => true]);
  });
});
Copy
Adding admin pages
Plugins can add admin pages via the admin menu hook:
add_action('admin_menu_register', function ($adminMenu) {
  $adminMenu->addPage([
    'title' => 'Hello Plugin',
    'slug'  => 'hello-plugin',
    'path'  => __DIR__ . '/admin/settings.php',
    'capability' => 'admin'
  ]);
});
Copy
Your admin renderer should include the given file when the page is visited.
Migrations
If your plugin includes SQL migrations, Curator runs them once when the plugin is enabled.
Example migration file: migrations/001_init.sql
Guidelines:
use CREATE TABLE IF NOT EXISTS
prefer additive changes
never drop tables by default
keep migrations idempotent where possible
Enqueueing assets
Plugins can register assets for:
frontend
admin
Use a registry helper (recommended), e.g.:
add_action('render_head', function () {
  // enqueue CSS
});
Copy
Or use your platform’s asset API if provided.
Packaging a plugin (ZIP)
ZIP the plugin folder so the root contains plugin.json.
Good ZIP layout:
hello-plugin.zip
  hello-plugin/
    plugin.json
    plugin.php
    migrations/
    admin/
Copy
Testing checklist
Before submitting:
install ZIP on a clean Curator install
enable/disable works without errors
migrations run once
routes work under /p/<slug>/...
no fatal errors under safe mode
no secrets included in the ZIP
performance: no heavy work on every request
Submission guidelines (marketplace)
Curator Marketplace reviews plugins for:
correct manifest
security (no obfuscation, no remote code execution)
stability (no breaking core)
clear README and usage instructions
