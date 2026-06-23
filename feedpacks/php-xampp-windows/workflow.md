# Workflow — PHP on Windows (XAMPP) Development

## 1. Environment Setup

```
XAMPP Control Panel → Start Apache + MySQL
Project root: E:\xampp\htdocs\your-project\
URL: http://localhost/your-project/
phpMyAdmin: http://localhost/phpmyadmin
```

## 2. New Feature Workflow

1. Understand the requirement
2. Check existing code patterns (classes, views, handlers)
3. Plan: class method → handler → view
4. Write DB migration if needed (`.sql` file in `db/migrations/`)
5. Implement class method in `app/classes/`
6. Create/update handler in `appopen/` (POST/GET processing)
7. Create/update view in `views/`
8. Test locally via `http://localhost/your-project/`
9. Update project documentation (architecture, DB schema docs)
10. Commit with meaningful message

## 3. Database Changes

1. Create migration file: `db/migrations/NNN_description.sql`
2. Test in phpMyAdmin first
3. Update DB schema documentation with new table/column
4. Track migration in a `migrations` table:
   ```sql
   INSERT IGNORE INTO migrations (migration) VALUES ('NNN_description.sql');
   UPDATE migrations SET descr = 'description' WHERE migration = 'NNN_description.sql';
   ```

## 4. Debugging on XAMPP

```
// PHP error display (in dev):
error_reporting(E_ALL);
ini_set('display_errors', 1);

// XAMPP error logs:
//   E:\xampp\php\logs\php_error_log
//   E:\xampp\apache\logs\error.log

// Quick debug helper:
function debug_log($msg) {
    file_put_contents(
        __DIR__ . '/logs/debug.txt',
        '[' . date('Y-m-d H:i:s') . '] ' . print_r($msg, true) . "\n",
        FILE_APPEND
    );
}
```

## 5. Git on Windows

```bash
# .gitattributes for cross-platform line endings:
echo "* text=auto" > .gitattributes

# Or keep CRLF (Windows native):
git config --global core.autocrlf true

# Never commit: .env, logs/, files/uploads
```

## 6. Deployment to Linux

1. Export fresh DB schema + data from phpMyAdmin
2. Push code via Git
3. Pull on server or upload via FTP
4. Import SQL via phpMyAdmin or CLI
5. Update `config.php` with production credentials
6. Set file permissions (Linux):
   ```bash
   find /var/www/project -type f -exec chmod 644 {} \;
   find /var/www/project -type d -exec chmod 755 {} \;
   chmod 640 config.php  # sensitive file
   ```

## 7. Quality Checklist

- [ ] No `\` paths in code (use `/` or `DIRECTORY_SEPARATOR`)
- [ ] Case-sensitive file references match actual filenames
- [ ] No hardcoded Windows paths
- [ ] All SQL uses prepared statements
- [ ] Session-based auth with CSRF protection
- [ ] All output uses `htmlspecialchars()` (or helper `val()`)
- [ ] No `mysql_*` deprecated functions
- [ ] `.htaccess` configured for URL routing
- [ ] Log files are gitignored
- [ ] Credentials are in config, not hardcoded
