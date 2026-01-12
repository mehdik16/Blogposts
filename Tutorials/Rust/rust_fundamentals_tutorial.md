# Rust Fundamentals: A Comprehensive Tutorial for C# Developers

## Introduction

Welcome! This tutorial is specifically designed for senior C# developers learning Rust. You'll notice many similarities between the languages, but Rust introduces unique concepts—particularly around memory safety and ownership—that will challenge your existing paradigms. This is intentional and powerful.

Throughout this tutorial, we'll use 20+ practical coding tasks to build your Rust expertise from fundamentals to advanced concepts. Each task includes a problem description, complete solution, and detailed explanation highlighting how Rust differs from C#.

---

## Part 1: Getting Started with Rust

### Task 1: Hello, Rust! - Your First Program

**Problem**: Create a simple Rust program that prints "Hello, Rust!" to the console.

**Solution**:
```rust
fn main() {
    println!("Hello, Rust!");
}
```

**Explanation**:
- `fn main()` is the entry point of every Rust program (like `static void Main()` in C#)
- `println!` is a macro (note the `!`) that prints text with a newline
- Rust doesn't require semicolons at the end of expressions, but does for statements
- No `using` statements or namespaces needed for basic functionality

**Key Difference from C#**: Rust's simplicity here is deceptive—there's no runtime or garbage collector. This binary is compiled directly to machine code.

---

### Task 2: Variables and Immutability

**Problem**: Create a program that demonstrates variable declaration, mutability, and type inference in Rust.

**Solution**:
```rust
fn main() {
    // Immutable variable (default in Rust)
    let x = 5;
    // x = 6;  // This would cause a compile error!
    
    // Mutable variable (explicit opt-in)
    let mut y = 5;
    y = 6;  // This is allowed
    println!("x = {}, y = {}", x, y);
    
    // Type annotation (optional due to inference)
    let z: i32 = 42;
    println!("z = {}", z);
    
    // Constants (compile-time known, always immutable)
    const MAX_POINTS: u32 = 100_000;
    println!("MAX_POINTS = {}", MAX_POINTS);
}
```

**Explanation**:
- Variables are **immutable by default** in Rust—a major difference from C#
- Use `let mut` to make variables mutable
- Type inference is powerful; Rust infers `i32` for `x`
- Constants use `const` and require explicit type annotations
- The underscore in `100_000` is just for readability (like `100_000` in C# 7+)

**Key Difference from C#**: In C#, variables are mutable by default. Rust flips this—immutability is the default, making code safer by default.

---

### Task 3: Primitive Data Types

**Problem**: Demonstrate all Rust primitive data types and their ranges.

**Solution**:
```rust
fn main() {
    // Signed integers
    let a: i8 = -128;
    let b: i16 = -32_768;
    let c: i32 = -2_147_483_648;
    let d: i64 = -9_223_372_036_854_775_808;
    let e: i128 = -170_141_183_460_469_231_731_687_303_715_884_105_728;
    let f: isize = -1;  // Platform-dependent (32 or 64 bits)
    
    // Unsigned integers
    let g: u8 = 255;
    let h: u16 = 65_535;
    let i: u32 = 4_294_967_295;
    let j: u64 = 18_446_744_073_709_551_615;
    let k: u128 = 340_282_366_920_938_463_463_374_607_431_768_211_455;
    let l: usize = 1;  // Platform-dependent (32 or 64 bits)
    
    // Floating point
    let m: f32 = 3.14;
    let n: f64 = 2.71828;  // Default float type
    
    // Boolean
    let o: bool = true;
    
    // Character (4 bytes, Unicode)
    let p: char = 'A';
    let q: char = '🦀';  // Rust mascot!
    
    println!("All types demonstrated successfully!");
    println!("Crab: {}", q);
}
```

**Explanation**:
- Rust has explicit integer sizes: `i8`, `i16`, `i32`, `i64`, `i128`, and their unsigned counterparts
- `isize` and `usize` are platform-dependent, used for indexing and pointer arithmetic
- `f32` and `f64` for floating-point numbers
- `char` is 4 bytes (Unicode scalar value), not 2 bytes like C#
- Boolean is `bool`, not `bool` (same name, but different implementation)

**Key Difference from C#**: C# has `int`, `long`, `float`, `double`, `decimal`, `char`. Rust is more explicit about sizes and has no `decimal` type (use external crates for arbitrary precision).

---

### Task 4: String Types and String Interpolation

**Problem**: Work with Rust's two main string types and demonstrate string operations.

**Solution**:
```rust
fn main() {
    // String slice (immutable, fixed size, stack or embedded)
    let s1: &str = "Hello, Rust!";
    println!("String slice: {}", s1);
    
    // String (owned, mutable, heap-allocated)
    let mut s2: String = String::from("Hello");
    s2.push_str(", World!");
    println!("Owned string: {}", s2);
    
    // String interpolation (formatting)
    let name = "Alice";
    let age = 30;
    println!("Name: {}, Age: {}", name, age);
    
    // Using format! macro (like string.Format in C#)
    let formatted = format!("{} is {} years old", name, age);
    println!("{}", formatted);
    
    // String methods
    let s3 = "  Rust is awesome  ";
    println!("Original: '{}'", s3);
    println!("Trimmed: '{}'", s3.trim());
    println!("Length: {}", s3.len());
    println!("Uppercase: {}", s3.to_uppercase());
    
    // Iterating over characters
    for ch in "Rust".chars() {
        println!("Character: {}", ch);
    }
}
```

**Explanation**:
- `&str` is a string slice—immutable, borrowed reference to string data
- `String` is an owned, mutable string type (heap-allocated)
- `String::from()` creates a new owned string from a string literal
- `println!` and `format!` use the same formatting syntax
- String methods like `trim()`, `to_uppercase()` return new strings (strings are immutable by default)
- `chars()` iterates over Unicode scalar values

**Key Difference from C#**: C# has a single `string` type. Rust has two: `&str` (borrowed reference) and `String` (owned). This distinction is crucial for memory safety.

---

## Part 2: Control Flow and Functions

### Task 5: If Expressions and Control Flow

**Problem**: Implement a program that uses if/else expressions to categorize numbers.

**Solution**:
```rust
fn main() {
    let number = 6;
    
    // If expression (returns a value)
    let result = if number % 2 == 0 {
        "even"
    } else {
        "odd"
    };
    println!("The number {} is {}", number, result);
    
    // If-else if-else chain
    let age = 25;
    if age < 13 {
        println!("Child");
    } else if age < 18 {
        println!("Teenager");
    } else if age < 65 {
        println!("Adult");
    } else {
        println!("Senior");
    }
    
    // Ternary-like expression (if as expression)
    let status = if age >= 18 { "adult" } else { "minor" };
    println!("Status: {}", status);
}
```

**Explanation**:
- In Rust, `if` is an **expression**, not just a statement—it returns a value
- The last expression in a block (without semicolon) is the return value
- No parentheses required around conditions (though allowed)
- Both branches must return the same type

**Key Difference from C#**: C# has the ternary operator `? :`. Rust uses `if` as an expression, which is more flexible.

---

### Task 6: Loop Constructs

**Problem**: Demonstrate Rust's three loop types: `loop`, `while`, and `for`.

**Solution**:
```rust
fn main() {
    // Infinite loop with break
    let mut count = 0;
    loop {
        println!("Loop iteration: {}", count);
        count += 1;
        if count >= 3 {
            break;
        }
    }
    
    // While loop
    let mut n = 5;
    while n > 0 {
        println!("n = {}", n);
        n -= 1;
    }
    
    // For loop with range
    for i in 0..5 {
        println!("i = {}", i);  // Prints 0, 1, 2, 3, 4
    }
    
    // For loop with inclusive range
    for i in 0..=5 {
        println!("i = {}", i);  // Prints 0, 1, 2, 3, 4, 5
    }
    
    // For loop with collection
    let arr = [10, 20, 30];
    for element in arr {
        println!("Element: {}", element);
    }
    
    // Loop with break and value
    let result = loop {
        count += 1;
        if count == 10 {
            break count * 2;  // Break with a value
        }
    };
    println!("Result: {}", result);  // Prints 20
}
```

**Explanation**:
- `loop` is an infinite loop that must be exited with `break`
- `while` loops while a condition is true
- `for` loops are iterator-based, not index-based (more Pythonic)
- Ranges: `0..5` (exclusive) vs `0..=5` (inclusive)
- `break` can return a value from a loop
- Iterating over collections consumes them (we'll cover borrowing later)

**Key Difference from C#**: C# has traditional `for` loops with index. Rust's `for` loops are iterator-based, which is safer and more expressive.

---

### Task 7: Functions with Parameters and Return Types

**Problem**: Create functions demonstrating parameters, return types, and Rust's expression-based returns.

**Solution**:
```rust
fn main() {
    let result = add(5, 3);
    println!("5 + 3 = {}", result);
    
    let area = calculate_rectangle_area(4.0, 5.0);
    println!("Rectangle area: {}", area);
    
    greet("Alice");
    
    let value = return_five();
    println!("Value: {}", value);
}

// Function with parameters and return type
fn add(a: i32, b: i32) -> i32 {
    a + b  // No semicolon—this is an expression
}

// Function with multiple parameters
fn calculate_rectangle_area(width: f64, height: f64) -> f64 {
    width * height
}

// Function with no return type (returns unit type `()`)
fn greet(name: &str) {
    println!("Hello, {}!", name);
}

// Function that returns a value
fn return_five() -> i32 {
    5
}
```

**Explanation**:
- Function parameters require explicit type annotations
- Return type is specified with `->`
- The last expression (without semicolon) is the return value
- Adding a semicolon turns an expression into a statement, changing the return type to `()`
- `()` is the unit type, equivalent to `void` in C#

**Key Difference from C#**: C# requires explicit `return` statements. Rust uses implicit returns via expressions, which is more concise.

---

### Task 8: Early Returns and Error Handling with Result

**Problem**: Implement a function that validates input and returns a Result type.

**Solution**:
```rust
fn main() {
    match divide(10.0, 2.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    match divide(10.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
}

fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(a / b)
    }
}
```

**Explanation**:
- `Result<T, E>` is an enum that represents either success (`Ok(T)`) or failure (`Err(E)`)
- Early return using `Err()` short-circuits the function
- Pattern matching with `match` handles both cases
- This is Rust's way of handling errors without exceptions

**Key Difference from C#**: C# uses exceptions. Rust uses `Result<T, E>` for recoverable errors, making error handling explicit and type-safe.

---

## Part 3: Ownership and Borrowing (The Heart of Rust)

### Task 9: Understanding Ownership

**Problem**: Demonstrate Rust's ownership system and how values are moved.

**Solution**:
```rust
fn main() {
    // Ownership example with String
    let s1 = String::from("hello");
    let s2 = s1;  // s1 is moved to s2
    
    // println!("{}", s1);  // ERROR! s1 no longer owns the data
    println!("{}", s2);  // This works
    
    // Ownership with functions
    let s3 = String::from("world");
    takes_ownership(s3);  // s3 is moved into the function
    // println!("{}", s3);  // ERROR! s3 no longer owns the data
    
    // Ownership with return values
    let s4 = gives_ownership();
    println!("{}", s4);  // This works
    
    let s5 = String::from("rust");
    let s6 = takes_and_gives_back(s5);
    // println!("{}", s5);  // ERROR!
    println!("{}", s6);  // This works
}

fn takes_ownership(s: String) {
    println!("{}", s);
}  // s goes out of scope and is dropped

fn gives_ownership() -> String {
    let s = String::from("hello");
    s  // s is moved out of the function
}

fn takes_and_gives_back(s: String) -> String {
    s  // s is moved to the caller
}
```

**Explanation**:
- Each value has one owner at a time
- When a value is assigned to another variable, ownership is **moved**
- When a value goes out of scope, it's automatically dropped (freed)
- Functions can take ownership and return it
- Stack types (like `i32`) are copied, not moved (they implement `Copy`)

**Key Difference from C#**: C# has garbage collection. Rust uses ownership and RAII (Resource Acquisition Is Initialization) for automatic memory management without a GC.

---

### Task 10: Borrowing and References

**Problem**: Use borrowing to allow functions to use values without taking ownership.

**Solution**:
```rust
fn main() {
    let s1 = String::from("hello");
    
    // Immutable borrow
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}", s1, len);
    
    // Multiple immutable borrows (allowed)
    let r1 = &s1;
    let r2 = &s1;
    println!("r1: {}, r2: {}", r1, r2);
    
    // Mutable borrow
    let mut s2 = String::from("hello");
    change_string(&mut s2);
    println!("Modified: {}", s2);
    
    // Only one mutable borrow at a time
    let mut s3 = String::from("rust");
    let r3 = &mut s3;
    r3.push_str("!");
    println!("r3: {}", r3);
    // let r4 = &mut s3;  // ERROR! Can't have two mutable borrows
}

fn calculate_length(s: &String) -> usize {
    s.len()
}  // s goes out of scope, but it doesn't own the String, so nothing happens

fn change_string(s: &mut String) {
    s.push_str(", world!");
}
```

**Explanation**:
- `&T` is an immutable reference (borrow)
- `&mut T` is a mutable reference
- Multiple immutable borrows are allowed
- Only one mutable borrow is allowed at a time (prevents data races)
- References don't take ownership; the original owner still owns the data
- Borrowing rules are checked at compile time

**Key Difference from C#**: C# doesn't have explicit borrowing. Rust's borrow checker ensures memory safety at compile time, preventing entire classes of bugs.

---

### Task 11: The Slice Type

**Problem**: Work with slices to reference contiguous sequences of elements.

**Solution**:
```rust
fn main() {
    let s = String::from("hello world");
    
    // String slices
    let hello = &s[0..5];
    let world = &s[6..11];
    println!("hello: {}, world: {}", hello, world);
    
    // Slice from the start
    let slice1 = &s[..5];  // Same as &s[0..5]
    println!("slice1: {}", slice1);
    
    // Slice to the end
    let slice2 = &s[6..];  // Same as &s[6..11]
    println!("slice2: {}", slice2);
    
    // Entire string slice
    let slice3 = &s[..];
    println!("slice3: {}", slice3);
    
    // Array slices
    let arr = [1, 2, 3, 4, 5];
    let arr_slice = &arr[1..4];
    println!("Array slice: {:?}", arr_slice);  // Prints [2, 3, 4]
    
    // Function with slice parameter
    let word = first_word(&s);
    println!("First word: {}", word);
}

fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[..i];
        }
    }
    &s[..]
}
```

**Explanation**:
- Slices are references to contiguous sequences
- `&s[start..end]` creates a slice from index `start` to `end-1`
- Slices can be omitted: `&s[..5]` or `&s[5..]`
- Slices prevent out-of-bounds access at compile time
- `&str` is actually a slice of a string

**Key Difference from C#**: C# has `Substring()` which returns a new string. Rust slices are zero-copy references.

---

## Part 4: Structs and Enums

### Task 12: Defining and Using Structs

**Problem**: Create a struct to represent a person and implement methods.

**Solution**:
```rust
struct Person {
    name: String,
    age: u32,
    email: String,
}

impl Person {
    // Associated function (like a static method in C#)
    fn new(name: String, age: u32, email: String) -> Person {
        Person { name, age, email }
    }
    
    // Method (takes &self)
    fn display_info(&self) {
        println!("Name: {}, Age: {}, Email: {}", self.name, self.age, self.email);
    }
    
    // Method that modifies self (takes &mut self)
    fn have_birthday(&mut self) {
        self.age += 1;
    }
    
    // Method that consumes self (takes self)
    fn into_email(self) -> String {
        self.email
    }
}

fn main() {
    let mut person = Person::new(
        String::from("Alice"),
        30,
        String::from("alice@example.com"),
    );
    
    person.display_info();
    person.have_birthday();
    println!("Age after birthday: {}", person.age);
    
    let email = person.into_email();
    println!("Email: {}", email);
    // person.display_info();  // ERROR! person has been consumed
}
```

**Explanation**:
- Structs group related data together
- `impl` blocks define methods for structs
- `&self` is an immutable borrow (read-only method)
- `&mut self` is a mutable borrow (can modify the struct)
- `self` consumes the struct (takes ownership)
- Associated functions (like `new`) don't take `self` and are called with `::`

**Key Difference from C#**: C# has classes with methods defined in the class body. Rust separates data (struct) from behavior (impl blocks), promoting composition over inheritance.

---

### Task 13: Tuple Structs

**Problem**: Use tuple structs for simple data grouping.

**Solution**:
```rust
struct Color(u8, u8, u8);
struct Point(f64, f64, f64);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0.0, 0.0, 0.0);
    
    println!("Black: ({}, {}, {})", black.0, black.1, black.2);
    println!("Origin: ({}, {}, {})", origin.0, origin.1, origin.2);
}
```

**Explanation**:
- Tuple structs are lightweight structs with unnamed fields
- Access fields by index: `.0`, `.1`, `.2`
- Useful for creating new types without much boilerplate

**Key Difference from C#**: C# would use a class or record. Rust's tuple structs are more lightweight.

---

### Task 14: Enums and Pattern Matching

**Problem**: Define an enum and use pattern matching to handle different variants.

**Solution**:
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

impl Message {
    fn process(&self) {
        match self {
            Message::Quit => println!("Quit"),
            Message::Move { x, y } => println!("Move to ({}, {})", x, y),
            Message::Write(text) => println!("Write: {}", text),
            Message::ChangeColor(r, g, b) => println!("Change color to ({}, {}, {})", r, g, b),
        }
    }
}

fn main() {
    let msg1 = Message::Quit;
    let msg2 = Message::Move { x: 10, y: 20 };
    let msg3 = Message::Write(String::from("Hello"));
    let msg4 = Message::ChangeColor(255, 0, 0);
    
    msg1.process();
    msg2.process();
    msg3.process();
    msg4.process();
}
```

**Explanation**:
- Enums define a type with multiple possible variants
- Each variant can have different associated data
- `match` expressions exhaustively handle all variants
- Pattern matching is type-safe and enforced at compile time

**Key Difference from C#**: C# has enums with a single underlying type. Rust enums are more powerful—each variant can have different data, similar to discriminated unions.

---

### Task 15: Option and Result Enums

**Problem**: Use Rust's built-in `Option` and `Result` enums for safe error handling.

**Solution**:
```rust
fn main() {
    // Option<T> represents Some(T) or None
    let some_number = Some(5);
    let some_string = Some("a string");
    let absent_number: Option<i32> = None;
    
    match some_number {
        Some(n) => println!("Number: {}", n),
        None => println!("No number"),
    }
    
    // Using if let for simpler matching
    if let Some(n) = some_number {
        println!("Number: {}", n);
    }
    
    // Result<T, E> represents Ok(T) or Err(E)
    let result: Result<i32, String> = Ok(42);
    
    match result {
        Ok(value) => println!("Success: {}", value),
        Err(e) => println!("Error: {}", e),
    }
    
    // Using unwrap (panics if None/Err)
    let value = some_number.unwrap();
    println!("Unwrapped: {}", value);
    
    // Using unwrap_or for default value
    let default_value = absent_number.unwrap_or(0);
    println!("Default: {}", default_value);
}
```

**Explanation**:
- `Option<T>` represents an optional value: `Some(T)` or `None`
- `Result<T, E>` represents either success `Ok(T)` or failure `Err(E)`
- `if let` is syntactic sugar for matching a single pattern
- `unwrap()` extracts the value or panics
- `unwrap_or()` provides a default value

**Key Difference from C#**: C# uses `null` and exceptions. Rust uses `Option` and `Result` to make error handling explicit and type-safe.

---

## Part 5: Collections

### Task 16: Vectors

**Problem**: Work with vectors (dynamic arrays) and common operations.

**Solution**:
```rust
fn main() {
    // Create a vector
    let mut v: Vec<i32> = Vec::new();
    
    // Add elements
    v.push(1);
    v.push(2);
    v.push(3);
    
    // Using the vec! macro
    let v2 = vec![1, 2, 3, 4, 5];
    
    // Access elements
    println!("First element: {}", v2[0]);
    
    // Safe access with get
    match v2.get(10) {
        Some(value) => println!("Value: {}", value),
        None => println!("Index out of bounds"),
    }
    
    // Iterate
    for element in &v2 {
        println!("Element: {}", element);
    }
    
    // Mutable iteration
    let mut v3 = vec![1, 2, 3];
    for element in &mut v3 {
        *element += 1;
    }
    println!("Modified: {:?}", v3);
    
    // Vector methods
    println!("Length: {}", v2.len());
    println!("Is empty: {}", v2.is_empty());
    
    // Pop element
    let mut v4 = vec![1, 2, 3];
    if let Some(last) = v4.pop() {
        println!("Popped: {}", last);
    }
}
```

**Explanation**:
- `Vec<T>` is a growable array on the heap
- `Vec::new()` creates an empty vector
- `vec![]` macro creates a vector with initial values
- Indexing with `[]` panics on out-of-bounds; use `get()` for safe access
- Iterating with `&v` borrows immutably; `&mut v` borrows mutably
- `*element` dereferences a mutable reference

**Key Difference from C#**: C# has `List<T>`. Rust's `Vec<T>` is similar but requires explicit borrowing when iterating.

---

### Task 17: HashMaps

**Problem**: Use HashMaps to store key-value pairs.

**Solution**:
```rust
use std::collections::HashMap;

fn main() {
    // Create a HashMap
    let mut scores = HashMap::new();
    
    // Insert values
    scores.insert(String::from("Alice"), 10);
    scores.insert(String::from("Bob"), 20);
    scores.insert(String::from("Charlie"), 15);
    
    // Access values
    if let Some(score) = scores.get("Alice") {
        println!("Alice's score: {}", score);
    }
    
    // Update values
    scores.insert(String::from("Alice"), 25);
    
    // Entry API (more efficient for conditional updates)
    scores.entry(String::from("David")).or_insert(5);
    
    // Iterate
    for (name, score) in &scores {
        println!("{}: {}", name, score);
    }
    
    // Remove
    scores.remove("Bob");
    
    // Check if key exists
    if scores.contains_key("Alice") {
        println!("Alice is in the map");
    }
}
```

**Explanation**:
- `HashMap<K, V>` stores key-value pairs
- Must import from `std::collections`
- `insert()` adds or updates a value
- `get()` returns `Option<&V>`
- `entry()` API is efficient for conditional updates
- Iterating with `&scores` borrows the map

**Key Difference from C#**: C# has `Dictionary<K, V>`. Rust's `HashMap` requires explicit imports and uses the `entry()` API for efficient updates.

---

### Task 18: Iterators and Functional Programming

**Problem**: Use iterators and functional methods like `map`, `filter`, and `collect`.

**Solution**:
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Map: transform each element
    let doubled: Vec<i32> = numbers.iter().map(|x| x * 2).collect();
    println!("Doubled: {:?}", doubled);
    
    // Filter: keep elements matching a condition
    let evens: Vec<&i32> = numbers.iter().filter(|x| *x % 2 == 0).collect();
    println!("Evens: {:?}", evens);
    
    // Chaining operations
    let result: Vec<i32> = numbers
        .iter()
        .filter(|x| *x % 2 == 0)
        .map(|x| x * 2)
        .collect();
    println!("Evens doubled: {:?}", result);
    
    // Fold (reduce)
    let sum = numbers.iter().fold(0, |acc, x| acc + x);
    println!("Sum: {}", sum);
    
    // Any and all
    let has_even = numbers.iter().any(|x| x % 2 == 0);
    let all_positive = numbers.iter().all(|x| x > &0);
    println!("Has even: {}, All positive: {}", has_even, all_positive);
    
    // Find
    if let Some(first_even) = numbers.iter().find(|x| *x % 2 == 0) {
        println!("First even: {}", first_even);
    }
}
```

**Explanation**:
- Iterators are lazy—they don't compute until consumed
- `map()` transforms each element
- `filter()` keeps elements matching a predicate
- `collect()` gathers results into a collection
- Closures `|x| x * 2` are anonymous functions
- `fold()` reduces to a single value
- `any()`, `all()`, `find()` are convenience methods

**Key Difference from C#**: C# uses LINQ with similar methods. Rust's iterators are more explicit about laziness and borrowing.

---

## Part 6: Error Handling and Advanced Concepts

### Task 19: The `?` Operator and Error Propagation

**Problem**: Use the `?` operator to simplify error handling.

**Solution**:
```rust
use std::fs;
use std::io;

fn read_file_content(filename: &str) -> Result<String, io::Error> {
    let content = fs::read_to_string(filename)?;
    Ok(content)
}

fn main() {
    match read_file_content("test.txt") {
        Ok(content) => println!("Content: {}", content),
        Err(e) => println!("Error: {}", e),
    }
}
```

**Explanation**:
- The `?` operator automatically propagates errors
- If the operation returns `Err`, it returns early with that error
- If it returns `Ok`, it unwraps the value and continues
- Much cleaner than nested `match` statements
- Can only be used in functions that return `Result` or `Option`

**Key Difference from C#**: C# uses try-catch. Rust's `?` operator is more explicit and composable.

---

### Task 20: Generics

**Problem**: Write generic functions and structs that work with multiple types.

**Solution**:
```rust
// Generic function
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];
    for &item in list {
        if item > largest {
            largest = item;
        }
    }
    largest
}

// Generic struct
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

// Generic with trait bounds
impl<T: std::fmt::Display> Point<T> {
    fn display(&self) {
        println!("Point: ({}, {})", self.x, self.y);
    }
}

fn main() {
    let numbers = vec![34, 50, 25, 100, 65];
    println!("Largest number: {}", largest(&numbers));
    
    let floats = vec![3.14, 2.71, 1.41];
    println!("Largest float: {}", largest(&floats));
    
    let int_point = Point { x: 5, y: 10 };
    let float_point = Point { x: 1.5, y: 2.5 };
    
    println!("Int point x: {}", int_point.x());
    float_point.display();
}
```

**Explanation**:
- `<T>` declares a generic type parameter
- Trait bounds like `T: PartialOrd + Copy` restrict what types can be used
- Generics are monomorphized at compile time (no runtime overhead)
- Separate `impl` blocks can have different trait bounds

**Key Difference from C#**: C# generics are similar, but Rust's trait bounds are more explicit and powerful.

---

### Task 21: Traits and Polymorphism

**Problem**: Define traits and implement them for different types.

**Solution**:
```rust
trait Animal {
    fn name(&self) -> &str;
    fn sound(&self) -> &str;
    
    // Default implementation
    fn speak(&self) {
        println!("{} says: {}", self.name(), self.sound());
    }
}

struct Dog {
    name: String,
}

struct Cat {
    name: String,
}

impl Animal for Dog {
    fn name(&self) -> &str {
        &self.name
    }
    
    fn sound(&self) -> &str {
        "Woof!"
    }
}

impl Animal for Cat {
    fn name(&self) -> &str {
        &self.name
    }
    
    fn sound(&self) -> &str {
        "Meow!"
    }
}

fn make_sound(animal: &dyn Animal) {
    animal.speak();
}

fn main() {
    let dog = Dog {
        name: String::from("Buddy"),
    };
    let cat = Cat {
        name: String::from("Whiskers"),
    };
    
    make_sound(&dog);
    make_sound(&cat);
}
```

**Explanation**:
- Traits define shared behavior across types
- `impl Trait for Type` implements a trait for a type
- Traits can have default implementations
- `&dyn Trait` is a trait object (dynamic dispatch)
- Trait objects allow polymorphic behavior at runtime

**Key Difference from C#**: C# uses interfaces and inheritance. Rust uses traits, which are more flexible and don't require inheritance hierarchies.

---

### Task 22: Lifetimes

**Problem**: Understand and use lifetimes to ensure references are valid.

**Solution**:
```rust
// Function with explicit lifetimes
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

// Struct with lifetime
struct Excerpt<'a> {
    part: &'a str,
}

impl<'a> Excerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}

fn main() {
    let string1 = String::from("long string is long");
    let string2 = "xyz";
    
    let result = longest(string1.as_str(), string2);
    println!("The longest string is: {}", result);
    
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a period");
    let excerpt = Excerpt {
        part: first_sentence,
    };
    
    println!("{}", excerpt.announce_and_return_part("Listen up!"));
}
```

**Explanation**:
- Lifetimes ensure references don't outlive the data they reference
- `'a` is a lifetime parameter
- `&'a str` means the reference is valid for lifetime `'a`
- Lifetimes are checked at compile time
- Most lifetimes can be elided (inferred) by Rust

**Key Difference from C#**: C# doesn't have explicit lifetimes. Rust's lifetime system prevents dangling references at compile time.

---

### Task 23: Closures

**Problem**: Work with closures and understand their capture behavior.

**Solution**:
```rust
fn main() {
    let x = 4;
    
    // Closure that borrows x
    let equal_to_x = |z| z == x;
    let y = 4;
    assert!(equal_to_x(y));
    
    // Closure that takes ownership
    let s = String::from("hello");
    let takes_ownership = move || println!("{}", s);
    takes_ownership();
    // println!("{}", s);  // ERROR! s was moved
    
    // Mutable closure
    let mut counter = 0;
    let mut increment = || {
        counter += 1;
    };
    increment();
    increment();
    println!("Counter: {}", counter);
    
    // Closures as function parameters
    let numbers = vec![1, 2, 3];
    let add_one = |x| x + 1;
    let results: Vec<_> = numbers.iter().map(add_one).collect();
    println!("Results: {:?}", results);
}
```

**Explanation**:
- Closures capture variables from their environment
- By default, closures borrow variables
- `move` keyword makes closures take ownership
- Closures can be mutable and modify captured variables
- Closures implement `Fn`, `FnMut`, or `FnOnce` traits

**Key Difference from C#**: C# lambdas are similar, but Rust's explicit capture semantics are clearer.

---

### Task 24: Smart Pointers - Box and Rc

**Problem**: Use smart pointers for heap allocation and shared ownership.

**Solution**:
```rust
use std::rc::Rc;

// Box: single ownership
fn use_box() {
    let b = Box::new(5);
    println!("Box value: {}", b);
    
    // Recursive data structure
    enum List {
        Cons(i32, Box<List>),
        Nil,
    }
    
    let list = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
}

// Rc: reference counting (shared ownership)
fn use_rc() {
    let a = Rc::new(5);
    let b = Rc::clone(&a);
    let c = Rc::clone(&a);
    
    println!("Reference count: {}", Rc::strong_count(&a));  // 3
    
    enum List {
        Cons(i32, Rc<List>),
        Nil,
    }
    
    let shared_list = Rc::new(List::Cons(1, Rc::new(List::Cons(2, Rc::new(List::Nil)))));
    let _b = List::Cons(3, Rc::clone(&shared_list));
    let _c = List::Cons(4, Rc::clone(&shared_list));
}

fn main() {
    use_box();
    use_rc();
}
```

**Explanation**:
- `Box<T>` allocates on the heap with single ownership
- `Rc<T>` allows multiple owners via reference counting
- `Rc::clone()` increments the reference count (cheap operation)
- When the last `Rc` is dropped, the data is freed
- `Rc` is single-threaded; use `Arc` for multi-threaded code

**Key Difference from C#**: C# has garbage collection. Rust uses reference counting with `Rc` for shared ownership.

---

### Task 25: Interior Mutability with RefCell

**Problem**: Use RefCell to achieve interior mutability.

**Solution**:
```rust
use std::cell::RefCell;

struct Person {
    name: String,
    age: RefCell<u32>,
}

impl Person {
    fn new(name: String, age: u32) -> Person {
        Person {
            name,
            age: RefCell::new(age),
        }
    }
    
    fn have_birthday(&self) {
        // Mutate through immutable reference
        *self.age.borrow_mut() += 1;
    }
    
    fn display_age(&self) {
        println!("Age: {}", *self.age.borrow());
    }
}

fn main() {
    let person = Person::new(String::from("Alice"), 30);
    person.display_age();
    person.have_birthday();
    person.display_age();
}
```

**Explanation**:
- `RefCell<T>` allows interior mutability
- `borrow()` returns an immutable borrow
- `borrow_mut()` returns a mutable borrow
- Borrows are checked at runtime (panics if violated)
- Useful when compile-time borrow checking is too restrictive

**Key Difference from C#**: C# doesn't have this pattern. Rust's `RefCell` provides runtime borrow checking for cases where compile-time checking is too restrictive.

---

## Part 7: Practical Projects

### Task 26: Building a Command-Line Calculator

**Problem**: Create a simple calculator that reads expressions from the command line.

**Solution**:
```rust
use std::io::{self, Write};

fn main() {
    loop {
        print!("Enter expression (e.g., 5 + 3): ");
        io::stdout().flush().unwrap();
        
        let mut input = String::new();
        io::stdin().read_line(&mut input).expect("Failed to read line");
        
        let input = input.trim();
        
        if input == "quit" {
            break;
        }
        
        match parse_and_calculate(input) {
            Ok(result) => println!("Result: {}", result),
            Err(e) => println!("Error: {}", e),
        }
    }
}

fn parse_and_calculate(input: &str) -> Result<f64, String> {
    let parts: Vec<&str> = input.split_whitespace().collect();
    
    if parts.len() != 3 {
        return Err("Invalid format. Use: number operator number".to_string());
    }
    
    let a: f64 = parts[0].parse().map_err(|_| "Invalid first number")?;
    let operator = parts[1];
    let b: f64 = parts[2].parse().map_err(|_| "Invalid second number")?;
    
    match operator {
        "+" => Ok(a + b),
        "-" => Ok(a - b),
        "*" => Ok(a * b),
        "/" => {
            if b == 0.0 {
                Err("Division by zero".to_string())
            } else {
                Ok(a / b)
            }
        }
        _ => Err(format!("Unknown operator: {}", operator)),
    }
}
```

**Explanation**:
- `io::stdin().read_line()` reads user input
- `parse()` converts strings to numbers
- `?` operator propagates errors
- `match` handles different operators
- Error handling is explicit and type-safe

---

### Task 27: File I/O and Text Processing

**Problem**: Read a file, count word frequencies, and write results to a new file.

**Solution**:
```rust
use std::fs;
use std::collections::HashMap;
use std::io;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let content = fs::read_to_string("input.txt")?;
    
    let mut word_count: HashMap<String, u32> = HashMap::new();
    
    for word in content.split_whitespace() {
        let word = word.to_lowercase();
        *word_count.entry(word).or_insert(0) += 1;
    }
    
    let mut results: Vec<_> = word_count.iter().collect();
    results.sort_by(|a, b| b.1.cmp(a.1));
    
    let mut output = String::new();
    for (word, count) in results {
        output.push_str(&format!("{}: {}\n", word, count));
    }
    
    fs::write("output.txt", output)?;
    println!("Results written to output.txt");
    
    Ok(())
}
```

**Explanation**:
- `fs::read_to_string()` reads entire file into memory
- `fs::write()` writes to a file
- `?` operator simplifies error handling
- `HashMap` stores word frequencies
- `sort_by()` sorts by count in descending order

---

## Conclusion

Congratulations! You've completed 27 comprehensive Rust fundamentals tasks. You now understand:

1. **Basic Syntax**: Variables, types, control flow, and functions
2. **Ownership System**: The core of Rust's memory safety
3. **Borrowing and References**: Safe, zero-cost abstractions
4. **Structs and Enums**: Powerful data modeling tools
5. **Collections**: Vectors, HashMaps, and iterators
6. **Error Handling**: Result and Option types
7. **Traits and Generics**: Polymorphism and code reuse
8. **Lifetimes**: Compile-time memory safety
9. **Smart Pointers**: Advanced memory management
10. **Practical Applications**: Real-world programming patterns

### Next Steps

1. **Practice**: Implement these concepts in your own projects
2. **Read the Book**: [The Rust Programming Language](https://doc.rust-lang.org/book/) is the definitive resource
3. **Explore Crates**: Use [crates.io](https://crates.io/) to discover libraries
4. **Build Projects**: Create real applications to solidify your knowledge
5. **Join Community**: Participate in [Rust forums](https://users.rust-lang.org/) and [Discord](https://discord.gg/rust-lang)

---

## Key Takeaways for C# Developers

| Concept | C# | Rust |
|---------|----|----|
| **Memory Management** | Garbage Collection | Ownership & RAII |
| **Null Handling** | `null` and exceptions | `Option<T>` and `Result<T, E>` |
| **Error Handling** | Exceptions (try-catch) | `Result<T, E>` and `?` operator |
| **String Types** | Single `string` type | `String` and `&str` |
| **Mutability** | Mutable by default | Immutable by default |
| **Inheritance** | Class-based | Trait-based |
| **Generics** | Similar | More explicit with trait bounds |
| **Closures** | Similar | More explicit capture semantics |
| **References** | Implicit | Explicit with `&` and `&mut` |
| **Null Pointers** | NullReferenceException | Compile-time prevention |

Happy coding in Rust! 🦀
