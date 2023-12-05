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
