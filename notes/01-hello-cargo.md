# Ch 1.3 — Hello, Cargo! 🦀📦

Source: https://doc.rust-lang.org/book/ch01-03-hello-cargo.html

## The main idea
**Cargo is Rust's build tool and package manager.** It compiles your code, downloads the libraries you use, and keeps your project tidy. Real Rust projects use it instead of running `rustc` by hand.

## 1. Check you have it
```bash
cargo --version
```

## 2. Make a project
```bash
cargo new hello_cargo
cd hello_cargo
```
```
hello_cargo/
├── Cargo.toml     ← project settings
├── .gitignore     ← git is set up for you
└── src/
    └── main.rs    ← your code
```
💡 **Rule:** code goes in `src/`. The top folder is for config, README and license files.

## 3. `Cargo.toml` = the project's ID card
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```
- `[package]` holds your project's name, version and Rust edition.
- `[dependencies]` lists the libraries you use (Rust calls them **crates**).

## 4. The three commands that matter 🎯

| Command | What it does | When to use it |
|---|---|---|
| `cargo build` | Compiles the program to `target/debug/hello_cargo` | When you want the program file |
| `cargo run` | Builds **and** runs it in one step | **Most of the time** |
| `cargo check` | Checks the code compiles but doesn't build a program | While coding, because it's **much faster** |

💡 If nothing changed, `cargo run` skips the rebuild.

## 5. Side notes
- **`Cargo.lock`** appears after your first build. It records exact library versions. **Never edit it.**
- **`target/`** holds the build output. Git already ignores it.

## 6. Release builds 🚀
```bash
cargo build --release
```
- Output goes to `target/release/`.
- It compiles **slower** but the program **runs faster**. Use it for final versions and benchmarks.

## 7. Why this matters
Every Rust project works the same way:
```bash
git clone <some-repo> && cd <some-repo> && cargo build
```

## ✅ TL;DR
- [ ] `cargo new name`: start a project
- [ ] Code goes in `src/main.rs`
- [ ] `cargo check` while writing
- [ ] `cargo run` to try it
- [ ] `cargo build --release` for the final version
