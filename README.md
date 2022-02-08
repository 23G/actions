# Github Actions

### Audit (Composer and NPM)

Requires a `package-lock.json` and `composer.lock`.

```yml
# .github/workflows/audit.yml
name: Audit
on:
  pull_request:
    branches: [ main, develop ]
  push:
    branches: [ main, develop ]
jobs:
  audit:
    uses: 23g/actions/.github/workflows/audit.yml@v2
```

> If your project does not use Composer, you may use the `audit-npm.yml@v2`
> workflow instead.

### Code style

Requires `23g/coding-standard` and
a [PHP_CodeSniffer configuration file](https://github.com/23G/coding-standard/#configuration).

```yml
# .github/workflows/code-style.yml
name: Code style
on:
  pull_request: ~
  push:
    branches: [ main, develop ]
jobs:
  code-style:
    uses: 23g/actions/.github/workflows/code-style.yml@v2
    secrets:
      composer-auth: ${{ secrets.COMPOSER_AUTH }}
#   with:
#     php-version: 8.0
```

### PHPUnit tests

Requires `phpunit/phpunit` and a PHPUnit configuration file.

```yml
# .github/workflows/tests.yml
name: PHPUnit tests
on:
  pull_request: ~
  push:
    branches: [ main, develop ]
jobs:
  tests:
    uses: 23g/actions/.github/workflows/phpunit.yml@v2
    with:
      php-version: 8.0
#     php-extensions: imagick, swoole
#   secrets:
#     composer-auth: ${{ secrets.COMPOSER_AUTH }}
```

> Make sure you enable all the required PHP extensions. If your application
> relies on Laravel Mix's `mix()` helper you should call `TestCase::withoutMix()`
> to avoid having to compile assets.

### PHPStan static analysis

```yml
# .github/workflows/static-analysis.yml
name: Static analysis
on:
  pull_request: ~
  push:
    branches: [ main, develop ]
jobs:
  static-analysis:
    uses: 23g/actions/.github/workflows/phpstan.yml@v2
#   with:
#     php-version: 8.0
#   secrets:
#     composer-auth: ${{ secrets.COMPOSER_AUTH }}
```

Requires `phpstan/phpstan` and a PHPStan configuration file:

```neon
# phpstan.neon.dist
parameters:
    level: 8
    paths:
        - app
        - database
        - tests
```
