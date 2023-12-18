# Widoki

- `Vec<T>` -> `&[T]`
- `String` -> `&str`
- `PathBuf` -> `&Path`

---

# `str`, `[T]`, `Path`

- nieznany rozmiar w czasie kompilacji (DST)
- może być używany tylko przez jakiś typ wskaźnika (np. `&str`, `Box<str>`)

---

# `str`

```rust
fn main() {
    let s: &str = "Hello, world!";

    let string = String::from("Hello, world!");
    let s: &str = &string;
    let s: &str = &string[..];
    let s: &str = &string[0..2];
}
```

---

# `[T]`

```rust
fn main() {
    let array: [i32; 3] = [1, 2, 3];
    let slice: &[i32] = &array;
    let slice: &[i32] = &array[..];
    let slice: &[i32] = &array[0..2];
}
```

---

# `Path`

```rust
use std::path::Path;
fn main() {
    let path: &Path = Path::new("foo/bar.txt");
}
```

---

# Deref coercion

```rust
fn f(s: &str) {
    println!("{s}");
}

fn main() {
    let string = String::from("Hello, world!");
    f(&string);
}
```

---

# Deref coercion

```rust
fn f(s: &str) {
    println!("{s}");
}

fn main() {
    let string = String::from("Hello, world!");
    let reference: &String = &string;
    f(reference);
}
```

---

# `Deref`

```rust
pub trait Deref {
    /// The resulting type after dereferencing.
    type Target: ?Sized;

    /// Dereferences the value.
    fn deref(&self) -> &Self::Target;
}
```

---

# `Deref`

```rust
impl ops::Deref for String {
    type Target = str;

    fn deref(&self) -> &str {
        unsafe { str::from_utf8_unchecked(&self.vec) }
    }
}
```

---

# Smart pointers

---

# `Box<T>`

własność obiektu na stercie

---

# `Box<T>`

```rust
fn main() {
    let x = 5;
    let y = Box::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```

---

# Typy rekurencyjne

```rust
enum List {
    Cons(i32, List),
    Nil,
}
```

---

# Typy rekurencyjne

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

fn main() {
    let list = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
}
```

---

# `Rc<T>`

reference-counting pointer - współdzielona własność obiektu na stercie

---

# `Rc<T>`

```rust
use std::rc::Rc;

fn main() {
    let x = 5;
    let y = Rc::new(x);
    let z = Rc::clone(&y);

    assert_eq!(5, x);
    assert_eq!(5, *y);
    assert_eq!(5, *z);
}
```

---

# `Rc<T>`

```rust
enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};
use std::rc::Rc;

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

---

# `Rc<T>`

```rust
fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a));
    let b = Cons(3, Rc::clone(&a));
    println!("count after creating b = {}", Rc::strong_count(&a));
    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

---

# Cells

https://doc.rust-lang.org/std/cell/

---

# `RefCell<T>`

mutowalna pamięć z regułami pożyczania sprawdzanymi w czasie wykonania

---

# `RefCell<T>`

```rust
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(5);
    let y = x.borrow();
    let z = x.borrow_mut();
}
```

---

# `Rc<RefCell<T>>`

```rust
#[derive(Debug)]
enum List {
    Cons(Rc<RefCell<i32>>, Rc<List>),
    Nil,
}

fn main() {
    let value = Rc::new(RefCell::new(5));

    let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));

    let b = Cons(Rc::new(RefCell::new(3)), Rc::clone(&a));
    let c = Cons(Rc::new(RefCell::new(4)), Rc::clone(&a));

    *value.borrow_mut() += 10;

    println!("a after = {:?}", a);
    println!("b after = {:?}", b);
    println!("c after = {:?}", c);
}
```

---

# Cykl

```rust
#[derive(Debug)]
struct Hehe {
    a: Option<Rc<RefCell<Hehe>>>,
}

fn main() {
    let s1 = Rc::new(RefCell::new(Hehe { a: None }));
    let s2 = Rc::new(RefCell::new(Hehe { a: None }));

    s2.borrow_mut().a = Some(s1.clone());
    s1.borrow_mut().a = Some(s2.clone());

    println!("{:?}", s1);
}
```

---

# `Weak<T>`

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

#[derive(Debug)]
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
```

---

# Drop

```rust
pub trait Drop {
    /// Executes the destructor for this type.
    ///
    /// This method is called implicitly when the value goes out of scope,
    /// and cannot be called explicitly (this is compiler error [E0040]).
    /// However, the [`mem::drop`] function in the prelude can be
    /// used to call the argument's `Drop` implementation.
    ///
    /// ...
    fn drop(&mut self);
}
```
