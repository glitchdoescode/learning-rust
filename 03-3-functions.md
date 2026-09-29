# Ch 3.3 — Functions ⚙️

Source: https://doc.rust-lang.org/book/ch03-03-how-functions-work.html

## The main idea
**Functions start with `fn`, every parameter needs a type, and the last expression in the body is what the function returns. No `return` is needed.** Most of the new ideas in this section come from the difference between *statements* and *expressions*.

---

## 1. Defining and calling
```rust
fn main() {
    println!("Hello, world!");
    another_function();
}

fn another_function() {
    println!("Another function.");
}
```
- Names use **snake_case**: lowercase with underscores.
- **Order doesn't matter.** You can call a function that's defined further down the file.

## 2. Parameters: the type is required
```rust
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}

print_labeled_measurement(5, 'h'); // The measurement is: 5h
```
💡 Rust never guesses parameter types. Because they're always written down, error messages point to the exact problem.

---

## 3. Statements vs. expressions 🧠 (the important part)

| | Statement | Expression |
|---|---|---|
| What it does | Does something | **Produces a value** |
| Examples | `let y = 6;`, a function definition | `5 + 6`, calling a function or macro, a `{ }` block |
| Ends with `;` | Yes | **No** (adding `;` turns it into a statement) |

A `let` doesn't produce a value, so you can't do this:
```rust
let x = (let y = 6); // ❌ error: expected expression, found `let` statement
```
(C and Ruby allow `x = y = 6`. Rust doesn't.)

A **block is an expression**:
```rust
let y = {
    let x = 3;
    x + 1      // ← no semicolon, so the block's value is 4
};
// y == 4
```

---

## 4. Return values
```rust
fn five() -> i32 {
    5
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```
- **`-> i32`** gives the return type.
- The **last expression** (with no `;`) is what gets returned.
- `return value;` also exists. Use it to **return early**, for example inside an `if`.

### ⚠️ The classic mistake: an extra semicolon
```rust
fn plus_one(x: i32) -> i32 {
    x + 1;   // ❌ `;` makes this a statement, so the function returns `()`
}
```
```
error[E0308]: mismatched types
  expected `i32`, found `()`
help: remove this semicolon to return this value
```
The compiler tells you exactly what to fix.

---

## ✅ TL;DR
- [ ] `fn name(param: Type) -> ReturnType { ... }`
- [ ] snake_case names, and definition order doesn't matter
- [ ] Parameter types are **required**
- [ ] A statement does something; an expression produces a value
- [ ] A `{ }` block is an expression, and its value is its last line
- [ ] The last expression with **no `;`** is the return value
- [ ] "expected `i32`, found `()`" usually means you added a `;` you didn't need

### ⚠️ Gotcha I hit: code that runs must be inside `fn main()`
```rust
fn plus_one(x: i32) -> i32 { x + 1 }

plus_one(5)   // ❌ error: missing `fn` or `struct` for function or struct definition
```
The top level of a file holds only **definitions** (`fn`, `struct`, `const`, …). Calls go inside `fn main()`, where the program starts. Ignore the "try `plus_one!(5)`" hint, because it isn't a macro.

**Try it now:**
```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}

fn main() {
    println!("{}", plus_one(5)); // 6
}
```
Run `rustc test.rs && ./test`. Then add a `;` after `x + 1` and read the error.
