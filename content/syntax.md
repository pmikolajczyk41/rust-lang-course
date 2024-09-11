# Podstawowa składnia

---

# Hello world

```rust
fn main() {
    println!("Hello, world!");
}
```

---

# Kompilacja z `rustc`

```shell
rustc src/main.rs
./main

> Hello, world!
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

# Control flow

```rust
fn is_even(x: u32) -> bool {
    if x % 2 == 0 {
        true
    } else {
        false
    }
}
```
---

# Pętle

```rust
loop {
    println!("Woohoo!")
}
```
---

# Pętle

```rust
while i < 10 {
    // ..
}
```

---

# Pętle

```rust
for i in some_collection {
    // ..
}
```

---

# Pętle

```rust
for i in some_collection {
    // ..
}
```
 ---
We wszystkich typach pętli można używać instrukcji `continue` i `break`.

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

# Panika

```rust
fn main() {
    panic!("Aaaaaa!");
}
```

---

# Todo

```rust
fn do_something() {
    todo!("To be implemented soon")
}

fn main() {
    do_something()
}
```

---

# Nieosiągalność

```rust
fn increment_small(x: u32) -> u32 {
    if x < 2 {
        x
    } else {
        unreachable!("x should never be greater than 1!")
    }
}
```

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

# Złota zasada Rusta

*Jednocześnie można mieć tylko jedną mutującą referencję **albo** wiele niemutujących.*

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

# Tworzenie nowych typów: krotki

```rust
struct Point(u8, u8);

fn main() {
    let p = Point(1, 2);
    let x = p.0;
    let y = p.1;
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

<img data-src="content/images/no-bugs-bunny.png" height="300">

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

# Typy generyczne

```rust
struct Triple<T> {
    content: [T; 3],
}
```

---

# Typy generyczne

```rust[5]
struct Triple<T> {
    content: [T; 3],
}

impl<T> Triple<T> {
    pub fn new(x: T, y: T, z: T) -> Self {
        todo!()
    }
}
```

---

# `Option`

```rust
struct Point { x: i32, y: i32 }

struct PointBuilder {
    x: Option<i32>,
    y: Option<i32>,
}
```

---

# `Result`

```rust
fn fallible_function() -> Result<u32, String> {
    if !check_conditions() {
        Err("Conditions were not met")
    } else {
        Ok(41)
    }
}
```

---

# Operator `?`

```rust
fn check_conditions() -> Result<bool, String> {
    if .. {
        Ok(true)
    } else {
        Err("Conditions were not met")
    }
}

fn fallible_function() -> Result<u32, String> {
    check_conditions()?;
    Ok(41)
}
```

---

# Traity (cechy)

```rust
trait LegOwner {
    fn has_legs(&self) -> bool {
        true // domyślna implementacja
    }
    
    fn number_of_legs(&self) -> u8; // brak domyślnej implementacji
}
```

---

# Implementacja traitów (implementacja cech)

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

---

# Traity a generyczność

```rust
trait A<T> {
    fn fun(t: T);
}

trait B {
    fn fun<T>(t: T);
}

trait C {
    type T;
    
    fn fun(t: Self::T);
}
```

---

# Domyślna derywacja
```rust
trait Eq {}

#[derive(Eq)]
struct S {
    value: u32
}
```

---

# Scoping traitów (cech)

```rust
trait A {
    fn fun(&self) {}
}
trait B {
    fn fun(&self) {}
}

struct S;

impl A for S {}
impl B for S {}

fn main() {
    let s = S;
    s.fun();
}
```

---

# Trait bounds

```rust
trait A {}
trait B {}

fn fun<T: A + B>(t: T) {}
```

---

# Trait bounds (`where`)

```rust
trait A {}
trait B {}

fn fun<T>(t: T) where T: A + B {}
```

---

# Trait bounds

```rust
trait A {
    type T;
}

fn fun<T: A>(t: T) where <T as A>::T: std::fmt::Debug {}
fn gun<T: A<T=i32>>(t: T) where {}
```

---

# Podstawowe traity

- `Eq`, `PartialEq`
- `Ord`, `PartialOrd`
- `Debug`, `Display`
- `Default`
- `Hash`
- `Iterator`, `IntoIterator`
- `From`, `Into`
- operatory
- `Copy`, `Clone`

---

# `Eq` / `PartialEq`

(częściowa) relacja równoważności


```rust
fn main() {
    assert_eq!(f32::MAX, f32::MAX);
    assert_ne!(f32::NAN, f32::NAN);
}
```

---

# `Ord` / `PartialOrd`

porządek liniowy vs porządek częściowy

---

# `Debug`

```rust
struct Point { x: i32, y: i32 }

impl std::fmt::Debug for Point {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "Point {{ x: {}, y: {} }}", self.x, self.y)
    }
}

fn main() {
    println!("{:?}", Point { x: 1, y: 2 });
}
```

---

# `Debug`

```rust
struct Point { x: i32, y: i32 }

impl std::fmt::Debug for Point {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        f.debug_struct("Point")
            .field("x", &self.x)
            .field("y", &self.y)
            .finish()
    }
}

fn main() {
    println!("{:?}", Point { x: 1, y: 2 });
}
```

---

# `Display`

```rust
#[derive(Debug)]
struct Point { x: i32, y: i32 }

impl std::fmt::Display for Point {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

fn main() {
    let p = Point { x: 1, y: 2 };
    println!("{p}");
    println!("{p:?}");
}
```

---

# `Default`

```rust
struct Point { x: i32, y: i32 }

impl Default for Point {
    fn default() -> Self {
        Point { x: 0, y: 0 }
    }
}
```

---

# `Hash`

```rust
struct Point { x: i32, y: i32, visited: bool }

impl std::hash::Hash for Point {
    fn hash<H: std::hash::Hasher>(&self, state: &mut H) {
        self.x.hash(state);
        self.y.hash(state);
        // ignorujemy `self.visited`
    }
}
```

---

# `Iterator`

```rust
struct PowersOfTwo(u32);

impl PowersOfTwo {
    fn new() -> Self {
        PowersOfTwo(1)
    }
}

impl Iterator for PowersOfTwo {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        let old = self.0;;
        self.0 *= 2;
        Some(old)
    }
}
```

---

# `IntoIterator`

```rust
struct TwoDimGrid {
    width: u32,
    height: u32,
}

struct TwoDimGridIterator {
    grid: TwoDimGrid,
    x: u32,
    y: u32,
}

impl IntoIterator for TwoDimGrid {
    type Item = (u32, u32);
    type IntoIter = TwoDimGridIterator;

    fn into_iter(self) -> Self::IntoIter {
        TwoDimGridIterator {
            grid: self,
            x: 0,
            y: 0,
        }
    }
}

impl Iterator for TwoDimGridIterator {
    type Item = (u32, u32);

    fn next(&mut self) -> Option<Self::Item> {
        todo!()
    }
}
```

---

# Przeciążanie operatorów

```rust
struct Point { x: i32, y: i32 }

impl std::ops::Add for Point {
    type Output = Self;

    fn add(self, other: Self) -> Self {
        Self {x: self.x + other.x, y: self.y + other.y}
    }
}
```

---

# `From` / `Into`

```rust[1-8]
struct LowerCaseString(String);
struct UpperCaseString(String);

impl From<LowerCaseString> for UpperCaseString {
    fn from(s: LowerCaseString) -> Self {
        UpperCaseString(s.0.to_uppercase())
    }
}

fn main() {
    let lower = LowerCaseString("hello".to_string());
    let upper: UpperCaseString = lower.into();
    println!("{}", upper.0);

    let lower = LowerCaseString("world".to_string());
    let upper = UpperCaseString::from(lower);
    println!("{}", upper.0);
}
```

---

# `From` / `Into`

```rust[10-18]
struct LowerCaseString(String);
struct UpperCaseString(String);

impl From<LowerCaseString> for UpperCaseString {
    fn from(s: LowerCaseString) -> Self {
        UpperCaseString(s.0.to_uppercase())
    }
}

fn main() {
    let lower = LowerCaseString("hello".to_string());
    let upper: UpperCaseString = lower.into();
    println!("{}", upper.0);

    let lower = LowerCaseString("world".to_string());
    let upper = UpperCaseString::from(lower);
    println!("{}", upper.0);
}
```

---

# Konwersje dla typów podstawowych

```rust
fn main() {
    let x = 1u8;
    let y = x as u16;
    let z = y as u32;
}
```

 --- 
Więcej przykładów: https://doc.rust-lang.org/rust-by-example/types/cast.html

---

# Move semantics

```rust
fn main() {
    let x = 1;
    let y = x;
    println!("{x} {y}");
}
```

---

# Move semantics

```rust
#[derive(Debug)]
struct S(u8);

fn main() {
    let x = S(1);
    let y = x;
    println!("{x:?} {y:?}");
}
```

---

# Move semantics

```rust
#[derive(Debug, Clone)]
struct S(u8);

fn main() {
    let x = S(1);
    let y = x.clone();
    println!("{x:?} {y:?}");
}
```

---

# Move semantics

```rust
#[derive(Debug, Copy)]
struct S(u8);

fn main() {
    let x = S(1);
    let y = x;
    println!("{x:?} {y:?}");
}
```

---

# Stos a sterta

---

# Closures

```rust

fn main() {
    let add = |x, y| x + y;
    let _four = add(1, 3);
    let add = |x, y| { x + y };
    let _four = add(1, 3);
    let add = |x: u8, y: u8| -> u8 { x + y };
    let _four = add(1, 3);
}
```

---

# `Fn`/`FnOnce`/`FnMut`

- `Fn` - nie mutuje ani nie konsumuje przechwyconych zmiennych
- `FnMut` - mutuje, ale nie konsumuje przechwyconych zmiennych
- `FnOnce` - konsumuje przechwycone zmienne

---

# Przechwytywanie przez wartość

```rust
fn prefix(s: String) -> impl Fn(&str) {
    return move |name| println!("{} {}", s, name)
}

fn main() {
    prefix("Hello".to_string())("there");
}
```

---

# `impl Trait`: argument

```rust
fn f(x: impl std::fmt::Debug) {
    println!("Got {x:?}");
}

fn main() {
    f(1);
    f("Hello");
}
```

---

# `impl Trait`: zwracana wartość

```rust
fn f() -> impl std::fmt::Debug {
    1
}

fn main() {
    println!("{:?}", f());
}
```

---

# const generics

```rust
struct Array<const N: usize> {
    content: [u8; N],
}

fn main() {
    let _a = Array::<3> { content: [1, 2, 3] };
    let _b = Array::<4> { content: [1, 2, 3, 4] };
}
```

---

# `if let`

```rust
fn main() {
    let x = Some(1);
    if let Some(y) = x {
        println!("{y}");
    }
}
```

---

# `let else`

```rust
fn main() {
    let s = "12";
    let Ok(twelve) = s.parse::<u8>() else {
        panic!("Not a number");
    };
    println!("{twelve}");
}
```
