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

Pełna dokumentacja: https://doc.rust-lang.org/reference/comments.html
