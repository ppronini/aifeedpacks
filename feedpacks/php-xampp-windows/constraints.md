# Constraints

## Hard Rules

### Filesystem
- ❌ Never use `\` in code paths — always `/`
- ❌ Never hardcode `E:\xampp\htdocs\` in code
- ❌ Never use Windows-specific functions (`exec('dir')`, etc.)
- ✅ Use `__DIR__` for relative paths
- ✅ Use `DIRECTORY_SEPARATOR` only for OS-level file operations

### Database
- ❌ Never use deprecated `mysql_*` functions
- ❌ Never interpolate variables directly into SQL
- ✅ Always use prepared statements or query builder
- ✅ Always set `utf8mb4` charset
- ✅ Always escape/validate user input

### Security
- ❌ Never store plaintext passwords
- ❌ Never expose credentials in code
- ❌ Never use `eval()`, `extract()`, `compact()` with user input
- ✅ Use `password_hash()` / `password_verify()`
- ✅ Use CSRF tokens on all forms
- ✅ Always validate file uploads (type, size, path)

### Output
- ❌ Never echo user input directly
- ✅ Always use `htmlspecialchars()` (via `val()` helper)
- ✅ Use `header('Content-Type: text/html; charset=utf-8')`

### Cross-Platform
- ❌ Never assume case-insensitive filesystem
- ❌ Never assume CRLF line endings
- ❌ Never assume Windows-specific commands exist
- ✅ Test file includes with exact case
- ✅ Set `core.autocrlf` in Git or use `.gitattributes`

### XAMPP-Specific
- ❌ Don't rely on XAMPP defaults in production code
- ✅ Use `config.php` with environment-aware settings
- ✅ Separate dev config from prod config
