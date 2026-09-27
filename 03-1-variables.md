# Ch 3.1 — Variables & Mutability 🔒

Source: https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html

## The main idea
**Variables can't change unless you say so.** This stops bugs where some other code changes a value you thought was fixed.

Note: **keywords** (`let`, `fn`, `match`, …) can't be used as variable names.

## 1. Immutable by default
```rust
let x = 5;
x = 6; // ❌ error: cannot assign twice to immutable variable
```

## 2. `mut` = allowed to change
```rust
let mut x = 5;
x = 6; // ✅
```

## 3. Constants 📌
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
- They can **never** be `mut`, and the **type is required**.
- Names are **ALL_CAPS_WITH_UNDERSCORES**.
- The value must be known **when the code compiles** (not user input).
- They can live **anywhere**, even outside functions.

## 4. Shadowing 👥
```rust
let x = 5;
let x = x + 1;          // 6
{
    let x = x * 2;      // 12, only inside the braces
    println!("{x}");    // 12
}
println!("{x}");        // 6
```

## 5. Shadowing vs `mut` ⚖️

| | Shadowing | `mut` |
|---|---|---|
| Can change the value | ✅ | ✅ |
| Can change the **type** | ✅ | ❌ |
| Stays immutable afterwards | ✅ | ❌ |

```rust
let spaces = "   ";
let spaces = spaces.len();   // ✅ text → number

let mut spaces = "   ";
spaces = spaces.len();       // ❌ mismatched types
```

## ✅ TL;DR
- [ ] `let x = 5;`: can't change
- [ ] `let mut x = 5;`: can change
- [ ] `const MAX: u32 = 100;`: fixed forever, type required, ALL_CAPS
- [ ] Shadowing makes a new variable and can change the type
- [ ] Shadowing inside `{ }` ends when the braces end
