---
layout: post
title:  "2026-11-28 Moss 26.0.3 Changelog"
date:   2026-11-28
last_modified_at: 2026-11-28
categories: [moss]
---

# Moss 26.0.3 Changelog

26.0.3 is another pre-release on the way to 26.1.0. Its job is groundwork: it
makes arrays good enough to build containers on, so that 26.1.0 can ship a
standard library written in Moss itself.

## Added

- **Array initializer lists.** A declaration can take a C++-style literal, and
  a shorter literal zero-fills the remaining slots:

  ```moss
  array int a[5] = {1, 2, 3};     // {1, 2, 3, 0, 0}
  array int g[2~5] = {7, 8, 9};   // grows to length 3
  array int e[~10] = {};          // stays empty
  ```

  A literal longer than the declared capacity is a compile error, the length
  and capacity must be constant when a literal is present, and every element is
  type-checked.

- **Whole-array assignment.** `a = {9, 8};` replaces the contents of `a`: it
  grows the array when the literal is longer than the current length and
  zero-fills the tail when it is shorter.

- **Prefix assignment.** `a[X] = {...};` rewrites only the first `X` elements
  and leaves the rest of the array alone. The literal may be shorter than `X`
  (the tail is zero-filled), and `X` must be a constant:

  ```moss
  array int a[5] = {1, 2, 3, 4, 5};
  a[3] = {10, 20, 30};   // {10, 20, 30, 4, 5}
  ```

- **`\null`, a hole in a literal.** It skips its slot instead of writing it, so
  a rewrite can leave individual elements alone:

  ```moss
  a[3] = {10, \null, 30};   // slot 1 keeps whatever it held
  ```

- **`__roll`, the carousel write.** For arrays that cannot grow, `__roll`
  writes the values in a ring at index `i % len` instead of failing:

  ```moss
  array int a[8] = {1, 2, 3, 4, 5, 6, 7, 8};
  __roll a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
  // {9, 0, 3, 4, 5, 6, 7, 8}, length still 8
  ```

  Growth is attempted first; wrapping is the fallback. An unbounded array can
  always grow, so `__roll` never wraps there.

- **Constant arrays.** The initializer is required, and writing to the
  constant is rejected in every form, including element writes and
  `grow`/`shrink`:

  ```moss
  const array int digits[5] = {1, 2, 3};
  ```

- **Struct fields may hold arrays and other structs.** An array field starts as
  a fresh empty array, a struct field as a fresh zeroed object:

  ```moss
  struct Buffer {
    array int data;
    Point origin;
    int count;
  }

  Buffer b;
  grow @b.data 3;
  b.data[0] = 10;
  ```

  `mut` parameters now accept a field or an element as well as a plain
  variable, which is what makes that `grow` work.

## Changed

- The `.mbc` container version is now 3, because a struct field carries a full
  layout instead of a single tag byte. Bytecode written by 26.0.2 or earlier
  cannot be read by 26.0.3.
- `grow` and `shrink` declare their array parameter `mut`, so a constant array
  is rejected at compile time rather than at run time.

## Fixed

- A struct that contains itself, directly or through other fields, is rejected
  instead of recursing forever while its default value is built.
- An array declared in a `for` initializer keeps its declared length in the
  native backend; it used to be built empty there while the VM built it with
  the right length.

## Still missing

Arrays of structs and generics are 26.0.4. Until then an array can only hold
`int`, `bool` or `string`, which is enough for `Vec`-style containers but not
for `Vec<Point>`.

---

*Written by DeepSeek Flash.*
