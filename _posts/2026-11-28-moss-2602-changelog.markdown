---
layout: post
title:  "2026-11-28 Moss 26.0.2 Changelog"
date:   2026-11-28
last_modified_at: 2026-11-28
categories: [moss]
---

# Moss 26.0.2 Changelog

26.0.2 is the second pre-release on the way to 26.1.0. This one is mostly
about arrays: they grow, they shrink, and their capacity is part of the
declaration.

## Added

- **Dynamic arrays with capacity limits.** A declaration carries its length,
  or a range:

  ```moss
  array int a[10];     // fixed length 10
  array int b[2~5];    // starts at 2, grows up to 5
  array int c[~];      // empty, unbounded
  array int d[1~];     // starts at 1, unbounded
  array int e[~10];    // empty, at most 10
  ```

- **`len`, `grow` and `shrink`** as `core` capabilities, with one overload set
  per element type (`int`, `bool`, `string`).

- **`const` bindings.** `const int n = 1;` must be initialized and cannot be
  assigned; `mut` parameters also reject constants.

- **Methods declared inside `struct` bodies.** Fields are in scope directly and
  the receiver is the implicit `self` parameter:

  ```moss
  struct Point {
    int x;
    int y;
    fn int sum() { return x + y; }
  }
  ```

- **`for x in a` over arrays** and **`for i in a~b`** over integer ranges.

- **Continuous integration.** The compiler is built and the full test suite
  runs on Linux and Windows for every push and pull request.

## Changed

- `build.sh` honours `MOSS_STATIC=0`, for platforms without a static
  libstdc++.

## Fixed

- `moss pack` now marks the packaged program executable on POSIX systems;
  before, the produced file could not be run on Linux.

---

*Written by DeepSeek Flash.*
