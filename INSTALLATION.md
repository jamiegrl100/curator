Doc #2: docs/INSTALLATION.md
Copy/paste this into docs/INSTALLATION.md (create the docs/ folder if needed).
# Installation

This guide installs **Curator Engine** on a Linux server/VPS using Apache, PHP, and MySQL/MariaDB.

## Requirements

- Linux VPS or server
- Apache (recommended) or Nginx
- PHP (8.x recommended) with common extensions
- MySQL or MariaDB
- Ability to create a database + user

> If you already run SnippetSupply/Curator Marketplace on the same server, you can reuse Apache/PHP and create a separate vhost + database.

---

## 1) Download Curator Engine

Download the latest Curator Engine package from:

- `https://curator.snippetsupply.com/download/curator-latest`

Or clone the repository (if you’re installing from source):

```bash
git clone <YOUR_GITHUB_REPO_URL> curator-engine
cd curator-engine
Copy
2) Create a database + user
Create a database and user for Curator.
Example (adjust values to your preference):
Database: curator
User: curator_app
Password: choose a strong password
Using MySQL CLI
CREATE DATABASE curator CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'curator_app'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD_HERE';
GRANT ALL PRIVILEGES ON curator.* TO 'curator_app'@'localhost';
FLUSH PRIVILEGES;
Copy
Using phpMyAdmin
Create the database curator with collation utf8mb4_unicode_ci
Create user curator_app@localhost
Grant privileges on database curator
3) Configure Curator
Curator needs database credentials and a base URL.
Configuration file
Use one of these patterns depending on your build:
.env file (recommended)
config.php (if your codebase uses PHP config)
If your package includes .env.example, copy it:
cp .env.example .env
Copy
Then set values like:
APP_URL=https://your-domain
DB_HOST=localhost
DB_NAME=curator
DB_USER=curator_app
DB_PASS=...
Keep .env out of version control and do not commit secrets.
4) Install the database schema
Curator packages typically include an install SQL file, for example:
install/curator_schema.sql
install/curator_seed.sql (optional)
Option A: Installer script (recommended)
If your package includes an installer script:
php install/install.php
Copy
Option B: Import schema manually
mysql -u curator_app -p curator < install/curator_schema.sql
Copy
If a seed file exists:
mysql -u curator_app -p curator < install/curator_seed.sql
Copy
5) Configure Apache (vhost)
Create a vhost pointing to Curator’s public/ folder.
Example (Apache):
<VirtualHost *:80>
  ServerName your-domain.com

  DocumentRoot /var/www/curator/public

  <Directory /var/www/curator/public>
    AllowOverride All
    Require all granted
  </Directory>

  ErrorLog logs/curator_error.log
  CustomLog logs/curator_access.log combined
</VirtualHost>
Copy
Enable rewrite rules and reload Apache.
Ensure your document root is .../public so storage/config files cannot be served.
6) First login
After install:
Visit your Curator URL in a browser
Create or log in to the admin account (depending on your installer)
Confirm you can access:
Admin dashboard
Products
Downloads
7) Optional hardening checklist
Enable HTTPS (Let’s Encrypt)
Ensure storage folders are not web-accessible
Set correct permissions for writable dirs (uploads/logs)
Back up your database and storage regularly
Troubleshooting
I get a 500 error
Check Apache/Nginx error logs
Check PHP-FPM logs if applicable
Verify DB credentials are correct
Verify schema import completed
Missing tables / schema errors
Re-run install/curator_schema.sql
Confirm you imported into the correct DB
Confirm DB user has privileges
