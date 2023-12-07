Rust is a system's language that is focused on memory safety and efficiency which is comparable in performance to [[C]] and [[C++]] languages.

A great learning resource for Rust is [The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html)

Rust has a packaging software called [[Cargo]] that can easily manage dependencies and software deployment.

You can display strings on console like this:

```Rust
println!("Hello, World!");
```

Display variables like this:

```Rust
println!("This is a variable: {var}");
```
## Variables

```rust
let x = 6;
```

Variables are by default immutable. Meaning they can not change. 

```Rust
let x = 4;
x = 5 //Compiler Error on this line
```

To be able to change a variable, you must add mut to your variable declaration.

```Rust
let mut x = 6;
x = 7; //no compiler errors
```

Constants just like variables are immutable but you can never declare one as "mut" like normal variables and the type must be explicitly given.

```Rust
const GRAVITY: u32 = 9.8;
```

**It is common practice to write const variables in all caps**

## Shadowing

You can **shadow** variables meaning replace them with something else by re-declaring them with the same name. Like this:

```Rust 
let spaces = "    ";
let spaces = spaces.len();
println!("The amount spaces: {spaces}"); //output will be 4
```

Like this, we can perform transformations on a variable without making that variable mutable.

## Data Types






