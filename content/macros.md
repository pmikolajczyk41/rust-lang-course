# Makra

1. deklaratywne (_macros by example_)
2. proceduralne

---

# Makra

1. deklaratywne (_macros by example_)
2. proceduralne
    - _function-like_
    - atrybut
    - derywacja

---

# Makra deklaratywne

```rust
fn main() {
    let v = vec![1, 2, 3];
    println!("{v:#?}");
}
```

---

# Makra deklaratywne

```rust
fn main() {
    let v = {
        let mut v = Vec::new();
        v.push(1);
        v.push(2);
        v.push(3);
        v
    };
    println!("{v:#?}");
}
```

---

# Makra deklaratywne

https://doc.rust-lang.org/reference/macros-by-example.html

https://doc.rust-lang.org/rust-by-example/macros.html

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { HashMap::new() };
}

fn main() {
    let map = hash_map!();
    assert_eq!(map.len(), 0);
}
```

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { std::collections::HashMap::new() };
}

fn main() {
    let map = hash_map!();
    assert_eq!(map.len(), 0);
}
```

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { std::collections::HashMap::new() };
}

fn main() {
    let map: HashMap<(), ()> = hash_map!();
    assert_eq!(map.len(), 0);
}
```

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { std::collections::HashMap::new() };
    ($key:expr, $value:expr) => {{
        let mut map = std::collections::HashMap::new();
        map.insert($key, $value);
        map
    }};
}

fn main() {
    let map = hash_map!("a", 1);
    assert_eq!(map.len(), 1);
}
```

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { std::collections::HashMap::new() };
    ($key:expr => $value:expr) => {{
        let mut map = std::collections::HashMap::new();
        map.insert($key, $value);
        map
    }};
}

fn main() {
    let map = hash_map!("a" => 1);
    assert_eq!(map.len(), 1);
}
```

---

# Makra deklaratywne

```rust
macro_rules! hash_map {
    () => { std::collections::HashMap::new() };
    ($key:expr => $value:expr) => {{
        let mut map = std::collections::HashMap::new();
        map.insert($key, $value);
        map
    }};
    ($($key:expr => $value:expr),+ $(,)?) => {{
        let mut map = std::collections::HashMap::new();
        $(map.insert($key, $value);)+
        map
    }};
}

fn main() {
    let map = hash_map!("a" => 1, "b" => 2, "c" => 3);
    assert_eq!(map.len(), 3);
}
```

---

# Makra proceduralne

AST -> AST

---

# Makra proceduralne

```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 1, y: 2 };
    println!("{:#?}", p);
}
```

---

# `cargo expand`

```rust[11-22]
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
struct Point {
    x: i32,
    y: i32,
}
#[automatically_derived]
impl ::core::fmt::Debug for Point {
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        ::core::fmt::Formatter::debug_struct_field2_finish(
            f,
            "Point",
            "x",
            &self.x,
            "y",
            &&self.y,
        )
    }
}
#[allow(dead_code)]
fn main() {
    let p = Point { x: 1, y: 2 };
    {
        ::std::io::_print(format_args!("{0:#?}\n", p));
    };
}
#[rustc_main]
#[no_coverage]
pub fn main() -> () {
    extern crate test;
    test::test_main_static(&[])
}
```

---

# Makra proceduralne

_function-like_
```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
   // ..
}
```

```rust
let sql = sql!(SELECT * FROM users WHERE id = 1);
```

---

# Makra proceduralne

_atrybut_
```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
   // ..
}
```

```rust
#[route(GET, "/")]
fn index() {
    // ..
}
```

---

# Makra proceduralne

_derywacja_
```rust
#[proc_macro_derive(Builder)]
pub fn builder_derive(input: TokenStream) -> TokenStream {
   // ..
}
```

```rust
#[derive(Builder)]
struct Point {
    x: i32,
    y: i32,
}
```

---

# Makra: porównanie

**Proceduralne:**
 - są potężniejsze (siła ekspresji)
 - wymagają większego narzutu kodu i organizacji (dedykowany crate)
 - czystsze (pisane w czystym języku Rust)

**Deklaratywne:**
 - świetne do prostych, lokalnych zadań
 - zwięzłe

---

# Makra proceduralne

wymagają własnego crate

```toml
[lib]
proc-macro = true
```

---

# Makra proceduralne

Przydatne biblioteki:
- `proc-macro`
- `proc-macro2`
- `syn`
- `quote`

---

# Makra proceduralne

<img data-src="content/images/procedural-macro-pipeline.svg" height="500">

---

# Makra proceduralne

```rust
#[proc_macro_attribute]
pub fn my_attr(args: TokenStream, item: TokenStream) -> TokenStream {
    let intermediate_representation = parse(args, item);
    codegen(intermediate_representation);
}
```

---

# Unsafe rust

---

# Unsafe rust

- dereferencja wskaźnika
- wywołanie funkcji `unsafe`
- zmiana wartości mutowalnej statycznej zmiennej
- implementacja traita `unsafe`
- dostęp do pól `union`

---

# Unsafe rust

```rust
fn main() {
   let mut x = 5;
   let p1 = &x as *const i32;
   let p2 = &mut x as *mut i32;
}
```

---

# Unsafe rust

```rust
fn main() {
    let x = 5;
    let y = &x as *const i32;
    let z = unsafe { *y };
    println!("{}", z);
}
```

---

# Unsafe rust

```rust
fn main() {
   let address = 0x012345usize;
   let r = address as *const i32;
   unsafe {
      println!("{:?}", *r);
   }
}
```

---

# Unsafe rust

```rust
unsafe fn hihi() {}

fn main() {
   hihi()
}
```

---

# Unsafe rust

```rust
unsafe fn hihi() {}

fn main() {
   unsafe {
      hihi()
   }
}
```

---

# Unsafe rust

```rust
extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```

---

# Unsafe rust

```rust
static mut COUNTER: u32 = 0;

fn main() {
    unsafe {
        COUNTER += 1;
        println!("COUNTER: {}", COUNTER);
    }
}
```

---

# Unsafe rust

```rust
unsafe trait Foo {
    // ..
}

unsafe impl Foo for i32 {
    // ..
}
```

---

# Unsafe rust

```rust
#[repr(C)]
union MyUnion {
   f1: u32,
   f2: f32,
}

fn main() {
   let mut u = MyUnion { f1: 0 };
   unsafe {
      u.f2 = 1.0;
      println!("{}", u.f2);
      println!("{}", u.f1);
   }
}
```
