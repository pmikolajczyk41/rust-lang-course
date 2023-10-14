# Podstawowa składnia

---

# Hello world

```rust
fn main() {
    println!("Hello, world!");
}
```

---

# Komentarze

```rust[1,3,5,8-10]
//! Dokumentacja węzła zawierającego (inner doc)

/// Dokumentacja węzła poniżej (outer doc)
fn main() {
    // Komentarz jednoliniowy
    println!("Hello, world!");
    
    /*
    Komentarz wieloliniowy
     */
}
```

 ---
Pełna dokumentacja: https://doc.rust-lang.org/reference/comments.html

---

# Funkcje

```rust
fn greet() {
    println!("Hello, world!");
}

fn main() {
    greet();
}
```

---

<img data-src="content/images/fn-keyword.png" height="600">

---

# Podstawowe typy

**Typy skalarne**:
 - `()`
 - `i8`, `i16`, `i32`, `i64`, `i128`, `isize`,
 - `u8`, `u16`, `u32`, `u64`, `u128`, `usize`, 
 - `f32`, `f64`, 
 - `bool`, `char`

**Typy złożone**: 
 - `[u8; 4]`
 - `(i8, bool, bool)`

 ---
Pełna dokumentacja: https://doc.rust-lang.org/rust-by-example/primitives.html

---
