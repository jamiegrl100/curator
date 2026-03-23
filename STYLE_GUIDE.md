Copy/paste this into docs/STYLE_GUIDE.md.
# Style Guide (Docs, Listings, Changelogs)

This guide keeps Curator Engine docs and Curator Marketplace listings consistent, readable, and professional.

Use it for:
- documentation pages
- plugin/theme listings
- release notes and changelogs
- support pages

---

## Writing principles

### Be specific
Prefer concrete statements over marketing fluff.

✅ Good:
- “Supports digital downloads with tokens (expiry + max downloads).”
❌ Bad:
- “Enterprise-ready and ultra powerful.”

### Keep it skimmable
Use:
- short paragraphs
- bullet lists
- headings every 200–400 words
- tables for reference data

### One idea per section
Each section should answer a single question:
- “What is it?”
- “How do I install it?”
- “How do I configure it?”
- “What are the limitations?”

---

## Voice and tone

### Default tone
- confident
- practical
- direct

Avoid:
- hype
- insults
- over-claiming future features

### Don’t promise what doesn’t exist
If it’s not shipped, label it as “planned” or omit it.

---

## Documentation structure

Each doc page should start with:

1) **What this page covers** (1–2 sentences)  
2) **Who it’s for** (optional)  
3) **Requirements/assumptions** (if relevant)

Then:
- steps
- examples
- troubleshooting
- next links

### Minimum sections for “how-to” docs
- Prerequisites
- Steps
- Verification (“How do I know it worked?”)
- Common errors

---

## Code blocks

### Use fenced blocks
Prefer:

```bash
command --flags
Copy
Include context
If a command must be run from a specific directory, say so.
Don’t include secrets
Never paste:
API keys
passwords
full .env files
Use placeholders:
YOUR_API_KEY
YOUR_PASSWORD
Plugin/theme listing style (Marketplace)
Listing must include
One-sentence summary (what it does)
Key features (3–7 bullets)
Requirements (Curator version)
Setup steps (short)
Support contact/URL
Changelog (at least for new releases)
Summary format
<What it is> for <Curator> that <primary benefit>.
Example:
“Analytics Pro is a Curator plugin that adds funnel reports and revenue attribution.”
Feature bullets
Use verbs:
“Adds…”
“Supports…”
“Includes…”
Avoid vague:
“Improves…”
“Optimizes…”
Changelog style
Use headings:
Added
Changed
Fixed
Security
Example:
1.2.0
Added
Added CSV export for order reports
Fixed
Fixed token expiry not applying to regenerated tokens
Security
Hardened ZIP extraction against path traversal
Rules:
Never reuse a version number
Never change a published ZIP without a new version
Compatibility & requirements
Always state:
minimum Curator version
tested version (optional)
PHP version requirements (if relevant)
external dependencies (if any)
Example:
Compatible with Curator >= 0.4.0
Tested up to 0.4.2
Screenshots/media
Use 1280×720 or 1440×900 where possible
Show real UI, not mockups (unless clearly labeled)
Add alt text for accessibility
SEO metadata (optional but recommended)
For important pages/listings, provide:
SEO title (50–65 chars)
Meta description (120–160 chars)
Avoid keyword stuffing.
Support and contact
Support page content
Support email
Expected response time
What info users should include (version, logs, steps)
Bug reports
Always ask for:
Curator version
environment (PHP/MySQL/web server)
reproduction steps
Final review checklist
Before publishing docs or listing:
 No dead links
 No placeholder text (“lorem ipsum”, “coming soon” unless intentional)
 No secrets included
 Accurate feature claims
 Clear next steps
