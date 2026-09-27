# Ch 3.2 — Data Types 🔢

Source: https://doc.rust-lang.org/book/ch03-02-data-types.html

## The main idea
**Every value in Rust has a type, and the compiler must know every type when it compiles.** It usually works types out by itself. When it can't, you add one with `: type`:
```rust
let guess: u32 = "42".parse().expect("Not a number!");
// without `: u32` → error: type annotations needed
```

There are two groups:
- **Scalar** = one value: integers, floats, booleans, chars
- **Compound** = several values grouped together: tuples, arrays

---

## 1. Integers (whole numbers)

| Size | Signed (can be negative) | Unsigned (0 and up) |
|---|---|---|
| 8-bit | `i8` | `u8` |
| 16-bit | `i16` | `u16` |
| 32-bit | **`i32`** ← default | `u32` |
| 64-bit | `i64` | `u64` |
| 128-bit | `i128` | `u128` |
| your CPU's size | `isize` | **`usize`** ← used for indexes |

- `i` = signed, `u` = unsigned, and the number is the size in bits.
- `i8` holds −128 to 127, and `u8` holds 0 to 255.
- **Not sure? Use `i32`.**

### Ways to write them
| Style | Example |
|---|---|
| Decimal | `98_222` (the `_` is just for readability) |
| Hex | `0xff` |
| Octal | `0o77` |
| Binary | `0b1111_0000` |
| Byte (`u8` only) | `b'A'` |

You can also put the type on the end: `57u8`.

### ⚠️ Overflow: putting 256 into a `u8`
- In a **debug build** (`cargo run`), the program **panics** (crashes with an error).
- In a **release build** (`--release`), it **wraps around**: 256 becomes 0 and 257 becomes 1. There is no error.
- To choose the behaviour yourself, use these methods:
  - `wrapping_add`: always wraps around
  - `checked_add`: returns `None` if it overflows
  - `overflowing_add`: returns the value plus `true`/`false` for "did it overflow?"
  - `saturating_add`: stops at the max or min value

---

## 2. Floats (decimals)
```rust
let x = 2.0;      // f64 ← default
let y: f32 = 3.0; // f32
```
Use `f64`: it's about as fast as `f32` on modern CPUs, and more precise.

## 3. Math
```rust
let sum = 5 + 10;
let difference = 95.5 - 4.3;
let product = 4 * 30;
let quotient = 56.7 / 32.2;
let truncated = -5 / 3; // -1  ← integer division drops the decimal part
let remainder = 43 % 5; // 3
```

## 4. Booleans
```rust
let t = true;
let f: bool = false;
```
Used for `if` conditions (Chapter 3.5).

## 5. Characters
```rust
let c = 'z';
let heart_eyed_cat = '😻';
```
- A `char` uses **single quotes**, and a string uses "double quotes".
- A `char` is 4 bytes and can hold any Unicode character, including emoji and letters from any language.

---

## 6. Tuples: mixed types, fixed length 📦
```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);

let (x, y, z) = tup;    // destructuring: unpack into 3 variables
println!("{y}");        // 6.4

let five_hundred = tup.0;   // access by position with a dot
let six_point_four = tup.1;
```
- A tuple **can't grow or shrink**.
- The empty tuple `()` is called **unit**. It means "no value", and functions that return nothing return it.

## 7. Arrays: same type, fixed length 📏
```rust
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5]; // [type; length]
let a = [3; 5];                    // [3, 3, 3, 3, 3]

let first = a[0];  // indexes start at 0
```
- Arrays are good when the size never changes, like the 12 months of the year.
- Need a list that grows? Use a **vector** (`Vec`), which comes in Chapter 8.

### ⚠️ Out-of-bounds index = panic
```rust
let a = [1, 2, 3, 4, 5];
let element = a[10]; // index out of bounds: the len is 5 but the index is 10
```
If the index comes from user input, the compiler can't catch this, so Rust **checks at runtime and stops the program**. It never reads memory it shouldn't, which is part of what keeps Rust safe.

---

## ✅ TL;DR
- [ ] The compiler must know every type; add `: type` when it can't guess
- [ ] Integers: `i32` by default, `usize` for indexes
- [ ] Overflow panics in debug and wraps in release; use `checked_*` and friends to choose
- [ ] Floats: `f64` by default
- [ ] `-5 / 3 == -1`: integer division drops the decimal
- [ ] `char` uses single quotes, is 4 bytes, and emoji are fine
- [ ] Tuple: mixed types, `tup.0`, `let (a, b) = tup;`
- [ ] Array: same type, `[i32; 5]`, fixed size, bad index = panic

**Try it now:** make `let x: u8 = 255;`, then `let y = x + 1;`, and run it. The compiler catches this one. Next, use `255u8.checked_add(1)` and print it with `{:?}`. It prints `None`.
