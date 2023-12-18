# Lifetimes

```rust
fn main() {
    let i = 3; // Lifetime for `i` starts. ────────────────┐
    //                                                     │
    { //                                                   │
        let borrow1 = &i; // `borrow1` lifetime starts. ──┐│
        //                                                ││
        println!("borrow1: {}", borrow1); //              ││
    } // `borrow1` ends. ─────────────────────────────────┘│
    //                                                     │
    //                                                     │
    { //                                                   │
        let borrow2 = &i; // `borrow2` lifetime starts. ──┐│
        //                                                ││
        println!("borrow2: {}", borrow2); //              ││
    } // `borrow2` ends. ─────────────────────────────────┘│
    //                                                     │
}   // Lifetime ends. ─────────────────────────────────────┘
```

---

# Lifetimes

```rust
fn print_refs<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("x is {} and y is {}", x, y);
}

fn main() {
    let (four, nine) = (4, 9);
    print_refs(&four, &nine);
}
```

---

# Lifetimes

```rust
fn print_refs(x: &i32, y: &i32) {
    println!("x is {} and y is {}", x, y);
}

fn main() {
    let (four, nine) = (4, 9);
    print_refs(&four, &nine);
}
```

---

# Elision

https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision

---

# Wariancja

```rust
fn f<'a>(a: &'a str, b: &'a str) {
    println!("{} {}", a, b);
}

fn main() {
    let s = String::from("Hello");
    let t: &'static str = "World";
    f(&s, t);
}
```

---

# Struktury

```rust
// A type `Borrowed` which houses a reference to an
// `i32`. The reference to `i32` must outlive `Borrowed`.
#[derive(Debug)]
struct Borrowed<'a>(&'a i32);

// Similarly, both references here must outlive this structure.
#[derive(Debug)]
struct NamedBorrowed<'a> {
    x: &'a i32,
    y: &'a i32,
}

// An enum which is either an `i32` or a reference to one.
#[derive(Debug)]
enum Either<'a> {
    Num(i32),
    Ref(&'a i32),
}

fn main() {
    let x = 18;
    let y = 15;

    let single = Borrowed(&x);
    let double = NamedBorrowed { x: &x, y: &y };
    let reference = Either::Ref(&x);
    let number    = Either::Num(y);

    println!("x is borrowed in {:?}", single);
    println!("x and y are borrowed in {:?}", double);
    println!("x is borrowed in {:?}", reference);
    println!("y is *not* borrowed in {:?}", number);
}
```

---

# Bounds

`T: 'a`

 ---
Wszystkie referencje w `T` muszą żyć co najmniej tak długo jak `'a`.

---

# `'static`

```rust
const S: &'static str = "Hello, world!";
```

---

# `'static`

```rust
extern crate rand;
use rand::Fill;

fn random_vec() -> &'static [usize; 100] {
    let mut rng = rand::thread_rng();
    let mut boxed = Box::new([0; 100]);
    boxed.try_fill(&mut rng).unwrap();
    Box::leak(boxed)
}

fn main() {
    let first: &'static [usize; 100] = random_vec();
    let second: &'static [usize; 100] = random_vec();
    assert_ne!(first, second)
}
```

---

# `'static`

```rust
use std::fmt::Debug;

fn print_it( input: impl Debug + 'static ) {
    println!( "'static value passed in is: {:?}", input );
}

fn main() {
    let i = 5;
    print_it(i);
    print_it(&i);
}
```

---

# Trait objects

```rust
pub trait Draw {
    fn draw(&self);
}

pub struct Button;
impl Draw for Button {
    fn draw(&self) {
        println!("Button");
    }
}

pub struct Pane;
impl Draw for Pane {
    fn draw(&self) {
        println!("Pane");
    }
}
```

---

# Trait objects

```rust
pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(Button {}),
            Box::new(Pane {}),
        ],
    };

    for component in screen.components.iter() {
        component.draw();
    }
}
```

---

# Object safe trait

https://doc.rust-lang.org/reference/items/traits.html#object-safety

---

# `no-std`

https://docs.rust-embedded.org/book/intro/no-std.html

---

# `no-std`

`#![no_std]`

---

# Wasm

https://webassembly.org/

---

# Toolchains

https://rust-lang.github.io/rustup/concepts/channels.html

---

# Polecane materiały:
 
- https://github.com/pretzelhammer/rust-blog/blob/master/posts/common-rust-lifetime-misconceptions.md
- https://nnethercote.github.io/perf-book/benchmarking.html
- https://os.phil-opp.com/async-await/
