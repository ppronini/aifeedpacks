# Quality Checklist

## Filesystem Safety
- [ ] All code paths use `/` not `\`
- [ ] No hardcoded Windows absolute paths
- [ ] Include paths use `__DIR__` relative
- [ ] File references match actual filename case exactly

## Database
- [ ] All SQL uses prepared statements
- [ ] `utf8mb4` charset set on connection
- [ ] No `mysql_*` deprecated functions
- [ ] Schema exported as `.sql` with no `ENGINE=...` platform-specific options
- [ ] Migrations tracked in `migrations` table

## Security
- [ ] Passwords hashed with `password_hash()`
- [ ] CSRF protection on all forms
- [ ] All output wrapped in `htmlspecialchars()`
- [ ] File uploads validated (type, size, no path traversal)
- [ ] Sessions configured with `httponly`, `secure` (on HTTPS)
- [ ] `.env` or `config.php` in `.gitignore`

## XAMPP Environment
- [ ] Apache `mod_rewrite` enabled
- [ ] `.htaccess` configured for routing
- [ ] PHP error reporting ON in dev, OFF in prod
- [ ] MySQL port matches config (default 3306)

## Cross-Platform
- [ ] `.gitattributes` or `core.autocrlf` configured
- [ ] Case-sensitive includes verified
- [ ] No shell commands that assume Linux
- [ ] File permissions handled (Windows doesn't need chmod)

## Code Quality
- [ ] `declare(strict_types=1)` on all PHP files
- [ ] PSR-12-like formatting
- [ ] No debug output (`var_dump`, `print_r`) in production
- [ ] Consistent naming convention
- [ ] Error handling for DB queries
- [ ] Logs directory is gitignored
