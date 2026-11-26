---
layout: post
title:  "Moss 26.0.1 Changelog"
date:   2026-11-26
last_modified_at: 2026-11-26
categories: [moss]
---
# Moss 26.0.1 Changelog

## Versioning

Versions are `[year].[major].[pre-release]`:

- `26.0.1`, `26.0.2`, ... are pre-releases published in 2026 on the way to
  the stable `26.1.0`.
- `26.1.0` is the stable release of that line; the last number is `0` for
  stable releases.
- Pre-releases may change behaviour or syntax without notice; stable
  releases keep compatibility within their major version.

## [26.0.2] — unreleased

### Added

- Continuous integration: the compiler is built and the full test suite runs
  on Linux and Windows for every push and pull request.
- Some array function.
- const.
- struct.
- "for" changes.
- A secret.

  Wait for it! =)

### Changed

- secret

### Fixed

- secret

## [26.0.1] — 2026-11-26

First pre-release towards `26.1.0`; supersedes the initial `26.0.0` upload.

### Added

- Language basics: `int`, `bool`, `string`, `void`, mutable variables with
  default values, functions, `if`/`else`, `while`, C-style `for`, and
  `__go (a ~ b) when (cond)` filtered iteration with the implicit `ex`.
- Capabilities: `pick head { module: @cap as alias; }`, `cap` definitions in
  `.msh` modules, overloading, `mut` parameters, and the built-in native
  `core` module.
- `@` as the mention marker, including mention lists
  (`ask @a @"," @b`) and aliases (`@output as show`).
- `ask` / `input` / `show` I/O, string concatenation and comparison.
- Structs with reference semantics and field access.
- Static arrays with bounds checks, lengths from constants or variables,
  and array parameters.
- Two backends: a bytecode VM with a `.mbc` container, and a native C
  backend that drives gcc.
- `moss build` (native executable), `moss pack` (executable with the VM
  embedded), `moss -c` / `moss run` (bytecode).
- `mossbuild.sh` / `mossbuild.bat` one-click wrappers, which fall back to
  `moss pack` when no C compiler is available.
- Documentation: `README.md`, `BUILDING.md`.

### Known limitations

- No collections beyond arrays, no generics, no first-class functions,
  no enums or pattern matching, no methods, no error handling.
- Generated C leaks strings; programs are meant to be short-lived.
