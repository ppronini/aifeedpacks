# Examples

## Example 1: DB Connection Class (XAMPP → Linux)

```php
<?php

declare(strict_types=1);

/**
 * Singleton DB connection using MySQLi.
 * Works identically on XAMPP (Windows) and Linux production.
 */
class db
{
    private static ?self $instance = null;
    private mysqli $mysqli;

    private function __construct()
    {
        $host = defined('DB_HOST') ? DB_HOST : 'localhost';
        $user = defined('DB_USER') ? DB_USER : 'root';
        $pass = defined('DB_PASS') ? DB_PASS : '';
        $name = defined('DB_NAME') ? DB_NAME : 'myapp';

        mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
        $this->mysqli = new mysqli($host, $user, $pass, $name);
        $this->mysqli->set_charset('utf8mb4');
    }

    public static function get(): self
    {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    public function query(string $sql, ?array $params = null): mysqli_result|bool
    {
        if ($params) {
            $types = str_repeat('s', count($params));
            $stmt = $this->mysqli->prepare($sql);
            $stmt->bind_param($types, ...$params);
            $stmt->execute();
            return $stmt->get_result();
        }
        return $this->mysqli->query($sql);
    }

    public function fetchAll(string $sql, ?array $params = null): array
    {
        $result = $this->query($sql, $params);
        return $result ? $result->fetch_all(MYSQLI_ASSOC) : [];
    }
}
```

## Example 2: Safe Path Handling (Cross-Platform)

```php
<?php

// ✅ GOOD — forward slash works on both Windows and Linux
require __DIR__ . '/app/lib.php';
$path = 'files/uploads/' . $filename;

// ❌ BAD — backslash only works on Windows
require __DIR__ . '\\app\\lib.php';
$path = 'files\\uploads\\' . $filename;

// ✅ GOOD — when OS path is needed
$realPath = __DIR__ . DIRECTORY_SEPARATOR . 'files' . DIRECTORY_SEPARATOR . $filename;

// ✅ GOOD — checking if file exists (PHP handles both)
if (file_exists('files/uploads/' . $filename)) { ... }
```

## Example 3: Safe Output Helper

```php
<?php

/**
 * Safe output — use everywhere in views.
 * Prevents XSS on both platforms.
 */
function val(?string $s): string
{
    return htmlspecialchars($s ?? '', ENT_QUOTES, 'UTF-8');
}

// In view template:
<h1><?= val($pageTitle) ?></h1>
<input value="<?= val($user['name']) ?>">
```

## Example 4: Localization Pattern (Multi-language)

```php
<?php

// langs/ru.php
$lang['lc_save'] = 'Сохранить';
$lang['lc_cancel'] = 'Отмена';
$lang['lc_my_tasks'] = 'Мои задачи';

// langs/en.php
$lang['lc_save'] = 'Save';
$lang['lc_cancel'] = 'Cancel';
$lang['lc_my_tasks'] = 'My Tasks';

// lang() function
function lang(string $key): string
{
    global $_lang;
    return $_lang[$key] ?? $key;
}

// In view:
<button><?= lang('lc_save') ?></button>
```

## Example 5: CSRF Protection

```php
<?php

// Generate token
function generateCsrfToken(): string
{
    if (empty($_SESSION['_csrf'])) {
        $_SESSION['_csrf'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['_csrf'];
}

// Verify token
function verifyCsrfToken(): void
{
    $token = $_POST['_csrf'] ?? '';
    if (!hash_equals($_SESSION['_csrf'] ?? '', $token)) {
        http_response_code(403);
        exit('CSRF validation failed');
    }
}

// In form:
<form method="POST">
    <input type="hidden" name="_csrf" value="<?= generateCsrfToken() ?>">
    ...
</form>
```

## Example 6: XAMPP Error Log Debugging

```php
<?php

// Quick debug log (appends to logs/debug.txt)
function debug_log(mixed $data, string $label = ''): void
{
    $logDir = __DIR__ . '/logs';
    if (!is_dir($logDir)) {
        mkdir($logDir, 0755, true);
    }
    $line = '[' . date('Y-m-d H:i:s') . '] '
          . ($label ? "[$label] " : '')
          . print_r($data, true) . "\n";
    file_put_contents($logDir . '/debug.txt', $line, FILE_APPEND);
}

// Usage:
debug_log($_POST, 'POST data');
debug_log($dbError, 'Database error');
```
