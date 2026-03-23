
# Theme Development

Curator themes control **presentation**: layouts, templates, and assets (CSS/JS). Business logic (search, checkout, downloads, permissions) stays in core.

Themes are installable ZIPs and can be activated from **Admin → Themes**.

---

## Theme structure

A Curator theme lives at:
Copy
themes/<theme-slug>/
theme.json
screenshot.png (optional)
templates/
layout.php
home.php
search.php
product.php
account.php (optional)
partials/
header.php
footer.php
nav.php
assets/
theme.css
theme.js

> Your exact template keys may vary by your implementation. The important rule: themes provide templates, and core passes data into them.

---

## `theme.json` manifest

Every theme must include a `theme.json` file.

Example:

```json
{
  "slug": "curator",
  "name": "Curator",
  "version": "1.0.0",
  "author": "SnippetSupply",
  "description": "Default Curator theme",
  "license": "MIT",
  "requires_core": ">=1.0.0",
  "templates": {
    "layout": "templates/layout.php",
    "home": "templates/home.php",
    "search": "templates/search.php",
    "product": "templates/product.php"
  },
  "assets": {
    "css": ["assets/theme.css"],
    "js": ["assets/theme.js"]
  }
}
Copy
Rules
slug must match the folder name.
Template paths must exist.
Paths must not contain .. (path traversal is blocked).
Assets are optional, but if declared they must exist.
Template rendering model
Curator renders pages using a theme-aware renderer:
Core selects a template key (e.g., home, search, product)
Core prepares a $data array
Theme templates render using $data
Layout template
templates/layout.php is the wrapper. It should include:
<head> with CSS/JS
shared header/nav/footer
a slot for page content
Example pattern:
<?php
// layout.php
$title = $data['title'] ?? 'Curator';
?>
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
  <?php include __DIR__ . '/partials/header.php'; ?>

  <main>
    <?php include $contentTemplate; ?>
  </main>

  <?php include __DIR__ . '/partials/footer.php'; ?>

  <?php foreach (theme_assets('js') as $js): ?>
    <script src="<?= e($js) ?>"></script>
  <?php endforeach; ?>
</body>
</html>
Copy
Your renderer may implement these helpers differently. The key idea is: keep templates clean and use escape helpers.
Data available in templates
Core should pass structured data into templates. Common keys:
Home
title
featured_items (optional)
Search
query
filters
results
pagination
Product page
product
images
highlights
features
faq
price
add_to_cart_url
Account pages
user
purchases
downloads
Escaping and security
Themes must escape output by default:
Use an e() helper (HTML escape) for text.
Only render trusted HTML where explicitly allowed.
Rendering rich HTML (WYSIWYG)
If your product description is stored as sanitized HTML (recommended), render it as:
<?= $data['product']['description_html'] ?>
Copy
Do not apply escaping to already-sanitized HTML.
Assets
Theme assets should live in assets/ and be referenced through a helper (so paths work regardless of active theme).
Avoid inline scripts unless necessary.
Packaging a theme (ZIP)
When packaging:
ZIP the theme folder so the root contains theme.json.
Good ZIP layout:
my-theme.zip
  my-theme/
    theme.json
    templates/
    assets/
Copy
Testing a theme locally
Install the theme ZIP via Admin → Themes
Activate it
Verify:
home page renders
marketplace/search pages render
product pages render
account pages render
CSS/JS loads cleanly
Check that missing templates fall back to the default theme (if supported)
Theme best practices
Keep templates presentation-only.
Don’t query the database directly from templates.
Keep pages fast (avoid heavy JS by default).
Make CTAs obvious (Download / Buy / Install).
Ensure good mobile spacing and readable typography.
