# PHP Extension Template

A minimal starting point for building PHP extensions in C. The repository provides a small build system, PHPT tests, Unix and Windows build metadata, and GitHub Actions CI.

This is a template, not a complete application architecture. Rename the example extension (`numerus`) and adapt the configuration, source files, tests, and CI before using it for a real extension.

## Requirements

- PHP 8.2 or later, including the development tools for the PHP version you are building against
- `phpize` and Autoconf
- A C compiler and build toolchain
- `make` on Unix-like systems

The CI workflow currently builds against PHP 8.2, 8.3, 8.4, and 8.5 on Linux. This does not by itself guarantee compatibility with every operating system or PHP build configuration.

## Build

On Unix-like systems, from the repository root:

```sh
phpize
./configure --enable-numerus
make
```

Check that the extension loads:

```sh
php -d extension="$(pwd)/modules/numerus.so" -r 'var_dump(extension_loaded("numerus"));'
```

The expected output is `bool(true)`.

On Windows, use the standard PHP SDK/build tools and the repository's `config.w32` configuration. The Unix `phpize` / `configure` commands do not apply to the Windows build process.

## Testing

### Extension tests (PHPT)

PHPT is the primary test mechanism in this repository. It exercises the extension through PHP and is appropriate for verifying extension loading, exposed functions, return values, warnings, and other PHP-visible behavior.

After building:

```sh
make test TESTS='tests/*.phpt'
```

See the [PHP documentation on testing PHP extensions](https://www.php.net/manual/en/install.pecl.php-test.php) for details about PHPT tests and their format.

### Userland unit tests (PHPUnit)

If you add unit tests for PHP userland code—for example, helper classes, wrappers, or application code—read the [PHPUnit 11 documentation](https://docs.phpunit.de/en/11.5/) before writing the test suite. PHPUnit 11 is the relevant documentation line for projects that still support PHP 8.2; check PHPUnit's requirements before selecting a different major version.

PHPUnit does not replace PHPT for testing the extension's native build and PHP runtime integration. Use each test approach for the layer it is designed to cover.

## CI

The GitHub Actions workflow currently checks:

- Linux builds with PHP 8.2, 8.3, 8.4, and 8.5
- Compilation and PHPT test execution for each matrix version
- Whether the built extension loads in the matrix job
- A separate debug-oriented build and PHPT test run on PHP 8.5

The workflow is a starting point. Add operating systems, PHP configurations, or runtime checks when the extension's supported platform matrix requires them.

## Project structure

- `config.m4` — Unix Autoconf configuration.
- `config.w32` — Windows build configuration.
- `php_numerus.h` — module declarations and example version.
- `numerus.c` — minimal module implementation.
- `tests/` — PHPT tests.
- `.github/workflows/tests.yml` — build and test workflow.
- `.gitignore` — generated build artefacts.
- `.editorconfig` — basic repository conventions.

## Using this as a template

When creating a new extension from this repository:

1. Rename the repository and extension identifiers from `numerus` to the new extension name.
2. Update `config.m4` and `config.w32`.
3. Update the module declaration, version, and any matching source/header filenames.
4. Replace or extend the example PHPT tests to cover the extension's public behavior.
5. Add PHPUnit only if the project includes PHP userland code that benefits from unit tests; consult the PHPUnit documentation above.
6. Update the CI workflow to reflect the PHP versions and platforms you actually support.

Keep the initial structure small and introduce additional architecture only when the extension's requirements justify it.

## License

MIT
