Copy/paste this into docs/MARKETPLACE_SUBMISSION_GUIDELINES.md.
# Marketplace Submission Guidelines

Curator Marketplace exists to distribute **safe**, **high-quality** plugins and themes. Every submission is reviewed with automated checks and manual review.

These guidelines help you get approved fast.

---

## 1) What you can submit

### Plugins
Plugins extend Curator functionality:
- admin pages
- routes/endpoints (prefixed)
- migrations
- assets (CSS/JS)
- hooks into core events

### Themes
Themes control presentation:
- templates/layouts
- styles and scripts
- component partials
- theme manifests

### Bundles
Bundles are curated packs of plugins/themes (managed by marketplace admins in early versions).

---

## 2) Submission tracks (free → paid unlock)

Curator Marketplace uses separate unlock tracks:

### Paid Plugins Unlock
You can submit **free plugins** immediately.

You can submit **paid plugins** after **3 approved free plugins**.

### Paid Themes Unlock
You can submit **free themes** immediately.

You can submit **paid themes** after **3 approved free themes**.

These tracks are independent.

---

## 3) Verified developers

Developers become **Verified** automatically after **5 approved/published releases** (plugins + themes combined), unless an admin override is applied.

---

## 4) Required files

### Plugins must include
- `plugin.json`
- `plugin.php` (entry)
- a clear README (can be included as `README.md` inside the plugin ZIP)

### Themes must include
- `theme.json`
- required templates referenced by the manifest
- optional: `screenshot.png` for listings

Manifests must be valid JSON and match the folder slug.

---

## 5) ZIP packaging rules (very important)

When you upload a ZIP, the ZIP must contain a single top-level folder matching your slug.

✅ Good:
Copy
my-plugin.zip
my-plugin/
plugin.json
plugin.php

❌ Bad:
Copy
my-plugin.zip
plugin.json
plugin.php

The top-level folder is required so Curator can safely install and isolate your code.

---

## 6) Security rules (strict)

Submissions are rejected if they include:
- obfuscated code
- hidden backdoors
- remote code execution
- downloading and executing remote scripts at runtime
- attempts to write files outside allowed storage/plugin paths
- credential harvesting or tracking without disclosure

### Common patterns that trigger review flags
- `eval(` usage
- suspicious `base64_decode` chains
- self-modifying code
- large obfuscated one-line payloads
- hidden executables or binaries

If your plugin needs network access (APIs, webhooks), you must clearly document:
- what it calls
- why
- what data is transmitted

---

## 7) Performance requirements

Plugins/themes must:
- load fast
- avoid heavy work on every request
- cache when reasonable
- avoid blocking HTTP calls on page loads unless necessary

If you need background work, use a job/queue pattern if your Curator build supports it.

---

## 8) Routes and collisions

Plugins must register routes under:
Copy
/p/<plugin-slug>/...

Do not attempt to override core routes or admin routes.

---

## 9) Capability declarations

Plugins must declare requested capabilities in `plugin.json`.

High-risk capabilities require extra review:
- migrations that alter core tables
- filesystem writes
- executing shell commands
- registering many public endpoints

Keep capability scope minimal.

---

## 10) Versioning and releases

- Use semantic versioning where possible: `MAJOR.MINOR.PATCH`
- Every new release should include a changelog entry
- Do not reuse the same version number for different ZIP contents

---

## 11) Listing requirements (metadata)

Your listing should include:
- name + summary
- documentation URL
- support URL or support email
- Curator compatibility (minimum version tested)
- license type (MIT/Commercial/Other)
- screenshots (themes recommended)

---

## 12) Review outcomes

### Approved
- Your release is published and visible in the marketplace.

### Changes requested
- Fix issues and submit an updated release.

### Rejected
- Major policy violation or security issues.
- You may appeal by replying with details and evidence.

---

## 13) Tips to get approved fast

- Start with a small free plugin/theme
- Keep code readable and non-obfuscated
- Provide clear docs and support contact
- Declare only the capabilities you actually use
- Test on a clean Curator install

---

## Submission links

- Developer Dashboard: `/account/developer`
- Submit Listing: `/account/developer/items/new`

---

Next:
- [Plugin Development](./PLUGINS.md)
- [Theme Development](./THEMES.md)
- [Hooks Reference](./HOOKS_REFERENCE.md)
