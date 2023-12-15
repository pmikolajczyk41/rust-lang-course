# Multitasking

---

# Wątki

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..5 {
            println!("Count in thread: {i}!");
            thread::sleep(Duration::from_millis(5));
        }
    });

    for i in 1..5 {
        println!("Main thread: {i}");
        thread::sleep(Duration::from_millis(5));
    }
}
```

Notes:

Na podstawie https://google.github.io/comprehensive-rust/concurrency/threads.html

---

# Wątki - czekanie

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..5 {
            println!("Count in thread: {i}!");
            thread::sleep(Duration::from_millis(5));
        }
    });

    for i in 1..5 {
        println!("Main thread: {i}");
        thread::sleep(Duration::from_millis(1));
    }
}
```

---

# Wątki - czekanie

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let fred = thread::spawn(|| {
        for i in 1..5 {
            println!("Count in thread: {i}!");
            thread::sleep(Duration::from_millis(5));
        }
    });

    for i in 1..5 {
        println!("Main thread: {i}");
        thread::sleep(Duration::from_millis(1));
    }

    fred.join().unwrap();
}
```

---

# Kanały

```rust
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();

    tx.send(10).unwrap();
    tx.send(20).unwrap();

    println!("Received: {:?}", rx.recv());
    println!("Received: {:?}", rx.recv());

    let tx2 = tx.clone();
    tx2.send(30).unwrap();
    println!("Received: {:?}", rx.recv());
}
```

---

# Ograniczone kanały

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::sync_channel(3);

    thread::spawn(move || {
        let thread_id = thread::current().id();
        for i in 1..10 {
            tx.send(format!("Message {i}")).unwrap();
            println!("{thread_id:?}: sent Message {i}");
        }
        println!("{thread_id:?}: done");
    });
    thread::sleep(Duration::from_millis(100));

    for msg in rx.iter() {
        println!("Main: got {msg}");
    }
}
```

---

# `Send`

```rust
pub unsafe auto trait Send {}
```

 ---
`T` jest `Send` jeśli można go bezpiecznie wysłać (przenieść _ownership_) do innego wątku.

---

# `auto trait`

```rust
#![feature(negative_impls)]
#![feature(auto_traits)]

auto trait IsCool {}

// Everyone knows that `String`s just aren't cool
impl !IsCool for String {}

struct MyStruct;
struct HasAString(String);

fn check_cool<C: IsCool>(_: C) {}

fn main() {
    check_cool(42);
    check_cool(false);
    check_cool(MyStruct);
    check_cool(String::new());
    check_cool(HasAString(String::new()));
}
```

 ---
https://stackoverflow.com/questions/49710852/what-is-an-auto-trait-in-rust

---

# `Sync`

```rust
pub unsafe auto trait Sync {}
```

 ---
`T` jest `Sync` jeśli można go bezpiecznie współdzielić między wątkami (`&T` jest `Send`).

---

# `Send + Sync`

- `u8`, `char`, `bool` , ...
- `(T, U)`, `[T; N]`, `&[T]`, `struct {x:T}`, ...
- `String`, `Option<T>`, `Vec<T>`, `Box<T>`, ...
- `Arc<T>`, `Mutex<T>`
- `AtomicBool`, `AtomicU8`, ...

jeśli `T` i `U` są `Send + Sync`

---

# `Send + !Sync`

- `mpsc::Sender<T>`, `mpsc::Receiver<T>`
- `Cell<T>`, `RefCell<T>`

(internal mutability)

---

# `!Send + Sync`

- `MutexGuard<T: Sync>`

(systemowe ograniczenia)

---

# `!Send + !Sync`

- `Rc<T>`
- `*const T`, `*mut T`

---

# Współdzielony dostęp

`Arc<Mutex<T>>`

---

# `Arc`

```rust
use std::thread;
use std::sync::Arc;

fn main() {
    let v = Arc::new(vec![10, 20, 30]);
    let mut handles = Vec::new();
    for _ in 1..5 {
        let v = Arc::clone(&v);
        handles.push(thread::spawn(move || {
            let thread_id = thread::current().id();
            println!("{thread_id:?}: {v:?}");
        }));
    }

    handles.into_iter().for_each(|h| h.join().unwrap());
    println!("v: {v:?}");
}
```

---

# `Mutex`

```rust
use std::sync::Mutex;

fn main() {
    let v = Mutex::new(vec![10, 20, 30]);
    println!("v: {:?}", v.lock().unwrap());

    {
        let mut guard = v.lock().unwrap();
        guard.push(40);
    }

    println!("v: {:?}", v.lock().unwrap());
}
```

---

# Multitasking

- kooperatywne
- z wywłaszczaniem

Notes:

Na podstawie https://os.phil-opp.com/async-await/

---

# Wywłaszczanie

<img data-src="content/images/async-preemptive.png" height="500">

---

# Wywłaszczanie

- gwarantujemy, że każdy proces będzie miał swoją porcję czasu
- procesy nie mogą się wzajemnie blokować
- nie musimy ufać procesom, że oddadzą kontrolę
- procesy nie muszą być świadome, że są wywłaszczane

---

# Wywłaszczanie

- context switch
- scheduler
- narzut pamięciowy i czasowy

---

# Asynchroniczność

<img data-src="content/images/async-async-diagram.png" height="500">

---

# Asynchroniczne bingo

https://github.com/pmikolajczyk41/rust-lang-course-async-bingo

---

# `Future`

_Reprezentuje wartość, która może być_ **jeszcze** _niedostępna._

---

# `Future`

_Reprezentuje asynchroniczne obliczenie._

---

# `Future`

```rust
pub trait Future {
    type Output;

    fn poll(
        self: Pin<&mut Self>,
        cx: &mut Context
    ) -> Poll<Self::Output>;
}
```

---

# `Poll`
    
```rust
pub enum Poll<T> {
    /// Represents that a value is immediately ready.
    Ready(T),

    /// Represents that a value is not ready yet.
    ///
    /// When a function returns `Pending`, the function *must* also
    /// ensure that the current task is scheduled to be awoken when
    /// progress can be made.
    Pending,
}
```

---

# Busy waiting

```rust
let future = async_read_file("foo.txt");
let file_content = loop {
    match future.poll(…) {
        Poll::Ready(value) => break value,
        Poll::Pending => {}, // do nothing
    }
}
```

---

# `Waker`, `Executor`

---

# Maszyna stanów

---

# `async trait`

```rust
#[async_trait]
trait AsyncRead {
    async fn read(&mut self, buf: &mut [u8]) -> Result<usize>;
}
```

---

# Biblioteki

- `futures`
- `tokio`
- `async_trait`

 ---

- `rayon`
- `crossbeam`
- `parking_lot`
