# Output Format

## PHP Code

```php
<?php

declare(strict_types=1);

// Class or procedural code
// PSR-12 compatible formatting
// UTF-8 without BOM
```

## Project Structure Generated

```
project-root/
├── index.php              ← Entry point
├── .htaccess              ← Apache routing
├── .gitignore
├── config.php             ← DB credentials, constants (gitignored)
│
├── app/
│   ├── lib.php            ← Main loader (includes everything)
│   ├── core.php           ← Constants, env setup
│   ├── dbconn.php         ← DB connection class
│   ├── sec.php            ← Auth, sessions
│   └── classes/           ← Business logic classes
│
├── appopen/               ← POST/GET handlers
├── views/                 ← HTML templates
├── langs/                 ← Localization files
├── css/                   ← Stylesheets
├── js/                    ← JavaScript
├── db/
│   ├── schema.sql         ← Full DB schema
│   └── migrations/        ← Incremental SQL changes
├── files/                 ← User uploads (gitignored)
└── logs/                  ← Debug logs (gitignored)
```

## SQL Migrations

```sql
-- db/migrations/001_initial.sql
CREATE TABLE IF NOT EXISTS `migrations` (
    `id` INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    `migration` VARCHAR(255) NOT NULL UNIQUE,
    `descr` VARCHAR(255) DEFAULT '',
    `applied_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Your migration SQL here
INSERT IGNORE INTO migrations (migration) VALUES ('001_initial.sql');
```

## .htaccess (Apache)

```apache
RewriteEngine On

# Force HTTPS (on production)
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]

# Route to entry point
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [QSA,L]
```
