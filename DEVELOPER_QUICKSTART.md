Copy/paste this into docs/DEVELOPER_QUICKSTART.md.
# Developer Quickstart (Plugins & Themes)

This guide gets you from zero to a working **Curator plugin/theme ZIP** you can install locally (and submit to Curator Marketplace).

---

## What you can build

- **Plugins**: add features (admin pages, routes, migrations, hooks, assets)
- **Themes**: change layouts/templates and styling

Start small. Get one clean free release approved. Then iterate.

---

# Part A — Build your first plugin (Hello Plugin)

## 1) Create the plugin folder

Create this structure:
Copy
hello-plugin/
plugin.json
plugin.php

## 2) Create `plugin.json`

```json
{
  "slug": "hello-plugin",
  "name": "Hello Plugin",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "Example Curator plugin that adds a health endpoint.",
  "license": "MIT",
  "requires_core": ">=1.0.0",
  "entry": "plugin.php",
  "capabilities": [
    "register_routes"
  ]
}
Copy
3) Create plugin.php
<?php

add_action('routes_register', function ($router) {
  $router->get('/p/hello-plugin/health', function () {
    header('Content-Type: application/json');
    echo json_encode([
      'ok' => true,
      'plugin' => 'hello-plugin',
      'time' => date('c'),
    ]);
  });
});
Copy
4) Zip it correctly
The ZIP must contain a single top-level folder matching the slug:
✅ Good:
hello-plugin.zip
  hello-plugin/
    plugin.json
    plugin.php
Copy
5) Install it locally
Admin → Plugins → Upload
Upload hello-plugin.zip
Enable it
Visit:
/p/hello-plugin/health
You should see JSON output.
Part B — Build your first theme (Hello Theme)
1) Create the theme folder
hello-theme/
  theme.json
  templates/
    layout.php
    home.php
  assets/
    theme.css
Copy
2) Create theme.json
{
  "slug": "hello-theme",
  "name": "Hello Theme",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "Example Curator theme.",
  "license": "MIT",
  "requires_core": ">=1.0.0",
  "templates": {
    "layout": "templates/layout.php",
    "home": "templates/home.php"
  },
  "assets": {
    "css": ["assets/theme.css"]
  }
}
Copy
3) Add minimal templates
templates/layout.php
<?php $title = $data['title'] ?? 'Curator'; ?>
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title><?= e($title) ?></title>
  <?php foreach (theme_assets('css') as $css): ?>
    <link rel="stylesheet" href="<?= e($css) ?>">
  <?php endforeach; ?>
</head>
<body>
  <header style="padding:16px;border-bottom:1px solid #eee;">
    <strong>Hello Theme</strong>
    <nav style="float:right;">
      <a href="/">Home</a>
      <a href="/marketplace">Marketplace</a>
      <a href="/docs">Docs</a>
    </nav>
  </header>

  <main style="padding:24px;">
    <?php include $contentTemplate; ?>
  </main>
</body>
</html>
Copy
templates/home.php
<h1><?= e($data['title'] ?? 'Welcome') ?></h1>
<p>This is a minimal Curator theme.</p>
Copy
assets/theme.css
body { font-family: system-ui, sans-serif; }
Copy
4) Zip it correctly
✅ Good:
hello-theme.zip
  hello-theme/
    theme.json
    templates/
    assets/
Copy
5) Install it locally
Admin → Themes → Upload
Upload hello-theme.zip
Activate the theme
Refresh your homepage
Part C — Submitting to Curator Marketplace
What to submit first
Start with a free plugin or theme.
Paid unlock rules:
Paid plugins unlock after 3 approved free plugins
Paid themes unlock after 3 approved free themes
Submission checklist
Manifest valid (plugin.json or theme.json)
ZIP structure correct (top-level folder)
No obfuscated code
No remote code execution or hidden downloads
Clear docs/support link
Declare only necessary capabilities
See:
Marketplace Submission Guidelines
