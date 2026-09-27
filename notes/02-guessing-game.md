# Ch 2 — The Guessing Game 🎲

Source: https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html
Code: [`../guessing_game`](../guessing_game)

## The main idea
**You build a small game: the computer picks a number from 1 to 100, and you guess until you get it.** The chapter only *introduces* variables, input, crates, `match` and error handling; later chapters explain them properly.

## Step 1: Read a guess ⌨️
```rust
use std::io;

let mut guess = String::new();
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
println!("You guessed: {guess}");
```
- **`let`** makes a variable, which **can't change by default**. **`mut`** makes it changeable.
- **`String::new()`** creates an empty, growable string.
- **`&mut guess`** gives `read_line` a *reference* it can write into (Chapter 4).
- **`.expect(...)`**: `read_line` returns a **`Result`** (`Ok` or `Err`). `expect` crashes with your message on `Err`.
- **`{guess}`** inside `println!` prints the variable.

## Step 2: Add the `rand` crate 📦
```toml
[dependencies]
rand = "0.8.5"
```
- `cargo build` downloads it automatically.
- **`Cargo.lock`** records exact versions, so builds are the same every time.
- **`cargo update`** moves to newer *compatible* versions (0.8.x, never 0.9).
- **`cargo doc --open`** opens the docs for every crate you use.

### ⚠️ Gotcha I hit: use the book's version
I had `rand = "0.10.3"`, and `rand::thread_rng` failed with `cannot find function`. Newer rand renamed the API:

| Book (0.8) | rand 0.10 |
|---|---|
| `use rand::Rng;` | `use rand::RngExt;` |
| `rand::thread_rng()` | `rand::rng()` |
| `.gen_range(1..=100)` | `.random_range(1..=100)` |

**Fix:** set `rand = "0.8.5"` and run `cargo run`. Cargo downloads it and updates `Cargo.lock` itself, so no `cargo update` is needed.
**Lesson:** when a crate's function "can't be found", check the version in `Cargo.toml` first.

## Step 3: Pick the secret number
```rust
use rand::Rng;
let secret_number = rand::thread_rng().gen_range(1..=100);
```
- `use rand::Rng;` is required, or `gen_range` won't be found.
- `1..=100` means 1 to 100 **including** 100.

## Step 4: Compare 🎯
```rust
use std::cmp::Ordering;

match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```
- **`cmp`** returns `Less`, `Greater` or `Equal`.
- **`match`** runs the branch that fits, and won't compile if you miss a case.

Text and numbers can't be compared, so convert the guess first:
```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```
- **Shadowing** lets you reuse the name `guess`.
- **`.trim()`** removes the newline from pressing Enter. **`.parse()`** turns text into a number, and **`: u32`** says which kind.

## Step 5: Keep guessing 🔁
Wrap it all in `loop { ... }`, and add `break;` in the `Equal` branch.

## Step 6: Don't crash on bad input 🛡️
```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```
`Err(_)` means "not a number, and I don't care why", so `continue` asks again.

## ✅ TL;DR
- [ ] **`let` / `let mut`**: fixed vs. changeable variable
- [ ] **`Result`**: `Ok` or `Err`, handled with `expect` or `match`
- [ ] **Crates**: add under `[dependencies]`
- [ ] **`match`**: every case must be covered
- [ ] **Shadowing**: reuse a name, useful after converting types
- [ ] **`loop` / `break` / `continue`**: repeat, stop, skip ahead
