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

```rust [1,3,5,8-10]
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

# Argumenty funkcji i zwracane wartości

```rust [1-3|]
fn increment(x: u8) -> u8 {
    return x + 1;
}

fn greet(times: u8) {
    println!("Be greeted {times} times!")
}

fn main() {
    greet(increment(0));
}
```

---

# Zwracanie wartości

```rust
fn increment(x: u8) -> u8 {
    return x + 1;
}

fn increment(x: u8) -> u8 {
    x + 1
}
```
 ---
Ostatnia linia w bloku definiuje zwracaną wartość.

---

# 'Brak' zwracanej wartości

```rust
fn greet() {
    // pomiędzy ostatnim średnikiem a nawiasem klamrowym nie ma żadnej wartości,
    // więc implicite funkcja zwraca `()`
    println!("Hello, world!");
}

fn greet() {
    println!("Hello, world!") // średnik nie jest potrzebny, `println!` zwraca `()`
}

fn greet() -> () { // explicite zwracany typ `()`
    println!("Hello, world!");
}

fn greet() -> () { // explicite zwracany typ `()`
    return println!("Hello, world!"); // explicite instrukcja `return`
}
```

---

# Makra

```rust
fn main() {
    println!("Hello, world!");
    assert!(true);
    assert_eq!(1, 2 - 1);
}
```

---

# Wypisywanie tekstu na stdout

```rust
fn printing(x: u8) {
    println!("{}", x);
    println!("{x}");
    println!("{}", x + 1);
    // println!("{x + 1}"); // błąd kompilacji
}
```

 ---
Pełna dokumentacja: https://doc.rust-lang.org/std/fmt/

---

# Zmienne

```rust
fn main() {
    let x = 1;
    let y: u8 = 2;
    let y = -3i128;
}
```

---

# Zmienne

```rust
fn main() {
    let x = 1;
    x += 1; // błąd kompilacji
}
```

---

# Zmienne mutowalne

```rust
fn main() {
    let mut x = 1;
    x += 1;
}
```

---

# Deklaracja i inicjalizacja zmiennej

```rust
fn main() {
    let x: u8; // dopóki nie zostanie zainicjalizowana, nie można jej użyć
    x = 1; // mimo, że `x` nie jest mutowalna, można ją raz zainicjalizować
}
```

---

# Stałe

```rust
const X: u8 = 1;
const Y: u8 = 2;

fn main() {
    const Z: u8 = X + Y;
}
```

---

# Stałe statyczne

```rust
static X: u8 = 1;
static Y: u8 = 2;

fn main() {
    static Z: u8 = X + Y;
}
```

 ---
Różnica między `const` a `static`: [link](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/const-and-static.html)

---

# Referencje

```rust
fn main() {
    let x = 1;
    let y = &x;
    let z = &y;
    let w = &&x;
}
```

---

# Dereferencja

```rust
fn main() {
    let x = 1;
    let y = &x;
    let z = *y;
    println!("{} {} {}", *y, y, z);
}
```

---

# Zmiana wartości poprzez referencję

```rust
fn main() {
    let x = 1;
    let y = &x;
    *y = 2; // błąd kompilacji
}
```

---

# Zmiana wartości poprzez referencję

```rust
fn main() {
    let x = 1;
    let y = &mut x; // błąd kompilacji
    *y = 2;
}
```

---

# Zmiana wartości poprzez referencję

```rust
fn main() {
    let mut x = 1;
    let y = &mut x;
    *y = 2;
}
```

---

# Tworzenie nowych typów: `type`

```rust
type Byte = u8; // nowa nazwa dla `u8`, nie jest nowym typem

fn main() {
    let x: Byte = 1;
    let x: Byte = 1u8;
}
```

---

# Tworzenie nowych typów: `struct`

```rust
struct Point {
    x: u8,
    y: u8,
}

fn main() {
    let p = Point { x: 1, y: 2 };
}
```

---

# Tworzenie nowych typów: `enum`

```rust
enum Animal {
    Capybara,
    Spider,
    Snek,
}

fn main() {
    let roman = Animal::Capybara;
}
```

---

# Warianty z danymi

```rust
enum Point {
    Cartesian { x: u8, y: u8 },
    Polar { r: u8, theta: u8 },
}
```

---

# Dziedziczenie

<img data-src="content/images/no.png" height="300">

 ---
Ale polimorfizm nadal jest możliwy. <!-- .element: class="fragment" data-fragment-index="1" -->

---

# Metody i słowo kluczowe `self`

```rust [5-15]
struct S {
    x: u8,
}

impl S {
    fn new(x: u8) -> Self {
        S {
            x: x,
        }
    }
    
    fn x(&self) -> u8 {
        self.x
    }
}
```

---

# Bloki `impl`

```rust [5-17]
struct S {
    x: u8,
}

impl S {
    fn new(x: u8) -> Self {
        S {
            x: x,
        }
    }
}

impl S {
    fn x(&self) -> u8 {
        self.x
    }
}
```

---

# Konstrukcja obiektów

```rust [9-10]
struct S {
    x: u8,
    y: u8,
}

impl S {
    fn new(x: u8, y_minus_one: u8) -> Self {
        S {
            x,
            y: y_minus_one + 1,
        }
    }
}
```

---

# Traits

```rust
trait LegOwner {
    fn has_legs(&self) -> bool {
        true // domyślna implementacja
    }
    
    fn number_of_legs(&self) -> u8; // brak domyślnej implementacji
}
```

---

# Implementacja traitów

```rust [9-17]
trait LegOwner { /**/ }

enum Animal {
    Capybara,
    Spider,
    Snek,
}

impl LegOwner for Animal {
    fn number_of_legs(&self) -> u8 {
        match *self {
            Animal::Capybara => 4,
            Animal::Spider => 8,
            Animal::Snek => 0,
        }
    }
}
```