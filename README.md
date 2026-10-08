# Numerus

A minimal PHP extension skeleton designed to be used as the starting point for PHP extension projects.

## Requirements

- PHP 8.2 or later
- Autoconf
- A C compiler and build toolchain

## Build

```sh
phpize
./configure --enable-numerus
make
```

## Test

```sh
make test TESTS='tests/*.phpt'
```

The GitHub Actions workflow builds and tests the extension against the supported PHP versions.

## Project structure

- `config.m4` — Autoconf configuration for the extension.
- `php_numerus.h` — Extension declarations and version.
- `numerus.c` — Minimal module implementation.
- `tests/` — PHPT tests.
- `.github/workflows/tests.yml` — Build and test matrix.

## License

MIT
