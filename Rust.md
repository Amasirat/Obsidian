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
const MY_AGE: u32 = 19;
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

Rust is a statically typed language, It has to know the data types of all variables at compile time. Although the compiler is pretty good at infering types based on the variable's value and how it's used.

There are two subsets of data types: **Scalars** and **Compound**.

* **Scalar**

They represent a single value. Rust has 4 primary scalar types. **integers**, **floating-point numbers**, **Booleans**, and **Characters**.

**Integer types:

There are two types of integer types, one unsigned(u) and one signed (i), for which you have specify it's bit size explicitly like this:

```Rust
let x: u32 = 25;
let x: i16 = 12;
let x: u8 = 255;
```

Bit size can either be: 8, 16, 32, 64, or size which defaults to the size of the computer architecture. it will usually be 64 bit on modern computers.

**Floating-Point:**

Just like above but with the f letter.
***f32 is a single-precision float and f64 has double precision.**

**The Boolean Type:**

Just like other programming languages, boolean types can only have true and false values.

```Rust
let f: bool = false;
```

**The Character Type**:

Usage is just like C/C++. However the difference is that it represents a Unicode and not just ASCII Values, and it takes 4 bytes in memory.

* **Compound**

Rust has two primitive compound types: **Tuples** and **[[Arrays]]**.

**Tuple Type:**

General way of grouping together a number of values with different types. It also has fixed size. It can not shrink or grow in size.

```Rust
let tup: (i32, f64, u8) = (500, 6.2, 1); //you can optionally explicitly mention what type each element is or let the compiler infer it all itself.
```

We can destructure a tuple like this:

```Rust
fn main() {
	let tup = (500, 6.2, 120);

	let (x, y, z) = tup;

	println!("Y is: {y}"); //output will be 6.2
}
```

We can access a tuple element directly by using a period and then the index of the value:

```Rust
let tup = (500, 2.0, 321);

let five_hundred = tup.0;
```






