# Examples

## Good — typed properties & constructor promotion

```php
<?php

declare(strict_types=1);

namespace App\Service;

final readonly class UserService
{
    public function __construct(
        private UserRepository $repository,
        private LoggerInterface $logger,
    ) {}
}
```

## Bad — mixed types, no strict types

```php
<?php

class user_service {
    var $repo;
    function save($data) {
        // ...
    }
}
```
