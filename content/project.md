# Hierarchia

- workspace
  - package
    - crate
      - module

---

# Moduł

- `mod` - słowo kluczowe
- grupuje definicje w semantyczną i syntaktyczną całość
- nie musi odpowiadać plikowi
- może być zagnieżdżony

---

# Crate

- `crate` - słowo kluczowe
- grupuje moduły w semantyczną i syntaktyczną całość
- może być binarny lub biblioteczny
- najmniejsza jednostka kompilacji

---

# Package

- grupuje dowolnie wiele binarnych i co najwyżej jeden biblioteczny crate
- `Cargo.toml` - plik konfiguracyjny
- `Cargo.lock` - plik z zależnościami

---

# Workspace

- grupuje dowolnie wiele pakietów
- `Cargo.toml` - plik konfiguracyjny

---

# Tworzenie nowego projektu

```bash
cargo new example
cargo new example --lib
```

---

# Tworzenie podmodułów

```rust
mod foo {
    mod bar {
        // ...
    }
}
```

---

# Importowanie definicji

```rust
use std::collections::HashMap;
use crate::module::function;
use super::function;
use self::submodule::structure;
```

---

# Importowanie definicji

```rust
use std::collections::*;
```

---

# Zmiana nazwy importowanej definicji

```rust
use std::collections::HashMap as Map;
```

---

# Wprowadzanie traitu w scope

```rust
use std::fmt::Debug as _;
```

---

# Eksportowanie definicji

```rust
pub use submodule::function;
```

---

# Widoczność

```rust
pub fn foo() {}
fn baz() {}
```

---

# Widoczność

```rust
pub fn foo() {}
fn baz() {}
pub(crate) fn bar() {}
pub(super) fn bam() {}
pub(self) fn bim() {}
```

---

# Widoczność

```rust
pub fn foo() {}
fn baz() {}
pub(crate) fn bar() {}
pub(super) fn bam() {}
pub(self) fn bim() {}
pub(in crate::module) fn bum() {}
```

---

# Widoczność

domyślnie wszystko jest prywatne, poza
- metodami i typami z traitu
- wariantami enuma

---

# Wersjonowanie

https://semver.org/

---

# Dodawanie zależności

```bash
cargo add clap
```

```toml
[dependencies]
clap = { version = "=4.4.7" }
```

---

# Rodzaje zależności

```toml
[dependencies]
clap = { version = "=4.4.7" }

[dev-dependencies]
tokio = "1.34.0"

[build-dependencies]
cargo = "0.75.1"
```

---

# Features

```rust
#[cfg(feature = "derive")]
mod derivation_utils {
  // ...
}
```

```toml
[dependencies]
clap = { version = "=4.4.7", features = ["derive"] }
```

---

# Features

```toml
[features]
feature_1 = []
```

---

# Features

```toml
[features]
feature_1 = []
feature_2 = ["feature_1"]
```

---

# Features

```toml
[dependencies]
dependency = { version = "0.1.0", optional = true }

[features]
feature_1 = ["dependency"]
feature_2 = ["feature_1"]
```

---

# Features są addytywne

```rust
#[cfg(all(feature = "std", feature = "no_std"))]
compile_error!(
  "Features `std` and `no_std` are mutually exclusive and cannot be used together"
);
```

---

# Workspaces

```toml
[workspace]
resolver = "2"

members = ["member1", "member2"]

exclude = ["examples/"]

[workspace.package]
edition = "2021"
license = "Apache-2.0"
readme = "README.md"
version = "0.1.0"

[workspace.dependencies]
anyhow = { version = "1.0.71" }
clap = { version = "4.3.4" }
```

---

# Workspaces

```toml
[package]
name = "member1"
edition.workspace = true
license.workspace = true
readme.workspace = true
version.workspace = true
description = "Nice crate"

[dependencies]
clap = { workspace = true }
```

---

# Konfiguracja profilu

```toml
[profile.release]
panic = "unwind"

[profile.production]
inherits = "release"
lto = true
codegen-units = 1
```

---

# Rejestr cratów

- https://crates.io/
- https://docs.rs/
- https://lib.rs/
- https://blessed.rs/crates

---

# Cargo

```bash
cargo build
cargo run
cargo install
cargo publish
cargo fmt
cargo clippy
cargo clean
```
