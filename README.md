# 🦀 RUST EXAM - LEVEL 1

<div align="center">

![Rust](https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white)
![Level](https://img.shields.io/badge/Level-1%20Beginner-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

### **Ownership, Borrowing & Lifetime Fundamentals**

</div>

---

<table>
<tr>
<td><b>👤 Student</b></td>
<td>Yasin Akmaz</td>
</tr>
<tr>
<td><b>📅 Date</b></td>
<td>18.01.2026</td>
</tr>
<tr>
<td><b>⏱️ Duration</b></td>
<td>Unlimited</td>
</tr>
<tr>
<td><b>🌐 Internet</b></td>
<td>✅ Allowed</td>
</tr>
<tr>
<td><b>🤖 AI</b></td>
<td>❌ Forbidden</td>
</tr>
</table>

---

## 📋 PART A: MULTIPLE CHOICE

> **4 Questions • 1 Point Each**

---

### Question 1

```rust
let s1 = String::from("hello");
let s2 = s1;
println!("{}", s1);
```

What does this code do?

| Option | Description |
|--------|-------------|
| A | Prints "hello" |
| B | Compiler error (s1 moved) |
| C | Runtime error |
| D | Prints empty string |

<details>
<summary>🔍 Show Answer</summary>

> **Answer: B**
> 
> S1 moved to S2, s1 is no longer in memory!

</details>

---

### Question 2

```rust
let mut x = 5;
let y = &x;
let z = &x;
println!("{} {}", y, z);
```

What does this code do?

| Option | Description |
|--------|-------------|
| A | Compiler error (multiple borrows) |
| B | Prints "5 5" |
| C | Runtime error |
| D | Undefined behavior |

<details>
<summary>🔍 Show Answer</summary>

> **Answer: B**
> 
> Prints 5 5.

</details>

---

### Question 3

```rust
let mut data = vec![1, 2, 3];
let r1 = &mut data;
let r2 = &mut data;
r1.push(4);
```

What does this code do?

| Option | Description |
|--------|-------------|
| A | Works, data = [1, 2, 3, 4] |
| B | Compiler error (two mutable borrows) |
| C | Runtime error |
| D | Works but data doesn't change |

<details>
<summary>🔍 Show Answer</summary>

> **Answer: B**
> 
> Two Mutable Borrows cannot be used at the same time. If R2 was used it might work because it was moved to R2 last!

</details>

---

### Question 4

What is the fundamental difference between `&self` and `self` in Rust?

| Option | Description |
|--------|-------------|
| A | &self is mutable, self is immutable |
| B | &self borrows, self takes ownership (consumes the object) |
| C | They are the same, just syntax difference |
| D | &self is only for structs, self is only for enums |

<details>
<summary>🔍 Show Answer</summary>

> **Answer: B**
> 
> Because &self is borrowing (so it loans), but self uses the object directly and throws it out of memory.

</details>

---

## 📋 PART B: CODE ANALYSIS

> **3 Questions • 1 Point Each**

---

### Question 5

Write the status of the `user` variable after each line of the following code:

*(Valid ✅ / Invalid ❌ / Moved 🚚)*

```rust
let user = String::from("Ahmet");     // user status: ?
let user2 = user;                     // user status: ?
let user3 = user2.clone();            // user2 status: ?
println!("{}", user3);                // user3 status: ?
```

<details>
<summary>🔍 Show Answer</summary>

| Line | Variable | Status | Explanation |
|------|----------|--------|-------------|
| 1 | `user` | ✅ Valid | Created and initialized |
| 2 | `user` | 🚚 Moved → ❌ Invalid | user moved to user2, user invalid |
| 3 | `user2` | ✅ Valid | with clone user3 is now in a separate place in memory |
| 4 | `user3` | ✅ Valid | println! only reads and leaves it, no memory removal situation occurs so it is valid |

</details>

---

### Question 6

Does this code compile? If not, **WHY?**

```rust
fn get_length(s: String) -> usize {
    s.len()
}

fn main() {
    let my_string = String::from("Rust");
    let len1 = get_length(my_string);
    let len2 = get_length(my_string);  // This line
    println!("Lengths: {} {}", len1, len2);
}
```

<details>
<summary>🔍 Show Answer</summary>

> **Does it compile?** ❌ No
> 
> **Explanation:** The get_length function doesn't Borrow the String value, it takes and gives every time and destroys memory, so to get len1 it needs to be cloned and given.

</details>

---

### Question 7

Explain the difference between the two functions below. Which one is "better" and why?

```rust
// Function A
fn print_name(name: String) {
    println!("Name: {}", name);
}

// Function B
fn print_name(name: &str) {
    println!("Name: {}", name);
}
```

<details>
<summary>🔍 Show Answer</summary>

> **Difference:**
> 
> Function A doesn't Borrow and Consumes, we have to say `print_name(value.clone());` every time! Function B borrows and doesn't consume, we can say `print_name(&value);`.
> 
> **Which is better and why:**
> 
> It varies based on usage usage. If it is a single use take-and-throw, then Function A. If we need to use the value without destroying it, Function B is a better solution.

</details>

---

## 📋 PART C: CODING

> **3 Questions • 1 Point Each**

---

### Question 8

Fix the code below using **borrowing**. Only change the signature and call of the `calculate_length` function.

```rust
fn calculate_length(s: String) -> usize {
    s.len()
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(s1);
    println!("'{}' length: {}", s1, len);  // s1 wants to be used!
}
```

<details>
<summary>🔍 Show Answer</summary>

**Fixed Code:**

```rust
fn calculate_length(s: &str) -> usize {
    s.len()
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("'{}' length: {}", s1, len);
}
```

</details>

---

### Question 9

Write a method for this struct: `describe` method should return information about the pizza. Should you use `&self` or `self`? Why?

```rust
struct Pizza {
    name: String,
    price: f64,
}

impl Pizza {
    // WRITE HERE: describe method
    // Example output: "Margarita pizza, price: 75.5 TL"
}
```

<details>
<summary>🔍 Show Answer</summary>

**Fixed Code:**

```rust
struct Pizza {
    name: String,
    price: f64,
}

impl Pizza {
    fn describe(&self) -> &str {
        &self.name
    }
}

fn main() {
    let pizza = Pizza { 
        name: String::from("Margarita"), 
        price: 75.5 
    };
    
    println!("{}", pizza.describe());
    println!("{}", pizza.describe());
}
```

> **Why did you choose that self type?**
> 
> &self was chosen because borrowing is required for it to be called a 2nd time.

</details>

---

### Question 10

Add the correct **lifetime annotation** to the following function:

```rust
fn longer_string(a: &str, b: &str) -> &str {
    if a.len() > b.len() {
        a
    } else {
        b
    }
}
```

<details>
<summary>🔍 Show Answer</summary>

**Fixed Code:**

```rust
fn longer_string<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() {
        a
    } else {
        b
    }
}

fn main()
{
    let my_string_first = String::from("Helloo");
    let my_string_second = String::from("Hello");
    let result = longer_string(&my_string_first, &my_string_second);

    println!("{}", result);
}
```

</details>

---

## 🎯 Scoring

<div align="center">

| Part | Questions | Points |
|------|-----------|--------|
| **Part A** | 1-4 | 4 points |
| **Part B** | 5-7 | 3 points |
| **Part C** | 8-10 | 3 points |
| **Total** | 10 | **10 points** |

### 📊 Passing Grade: **7/10**

</div>

---

<div align="center">

*Built with 🦀 Rust Learning Path*

</div>
