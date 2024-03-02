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

There are two types of integer types, one unsigned(u) and one signed (i), for which you have to specify its bit size explicitly like this:

```Rust
let x: u32 = 25;
let x: i16 = 12;
let x: u8 = 255;
```

Bit size can either be: 8, 16, 32, 64, or size which defaults to the size of the computer architecture. it will usually be 64 bit on modern computers.

**Floating-Point:**

Just like above but with the letter **f**.
***f32 is a single-precision float and f64 is double precision.**

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

The Tuple without any values has a special name known as a **unit** which represents an empty value.

```Rust
let unit: () = ();
```

**The Array Type:**

Fixed length array and it is useful when you want to allocate data on the stack.

```Rust
let a: [i32; 5] = [1,2,3,4,5];//i32 is the type of the array and 5 is the size
```

You can also initialize an array like this:

```Rust
let a = [3; 5];
```

Where you'll get:

```Rust
a = [3, 3, 3, 3, 3];
```

accessing array elements is just like c++

## Statements and Expressions

* Statements are instructions that perform action and do not return a value.

```Rust
let a = 6;//this is a statement
```
Because statements do not return values, then you can't do something like this:

```Rust
let a = (let b = 6);
```

* Expressions evaluate to a value.

```Rust
5 + 6//evaluates to 11

println!();//calling macros is an expression

func() { 5 }//if it returns a value, it is  an expression

{
//yes...curly brackets are fucking expressions...yes, this is rust.
}

let a = { //you can fucking do this
	let x = 5;
	x + 1//if you add a semicolon here, this will turn into a statement and it will no longer be an expression for some reason.
}
```

That's how functions work, functions that return a value are expressions, therefore their last line has to be an expression. So no semicolon on the ending line of a function

```Rust
fn five() -> i32 {
	5//if a semicolon was here, a compile error would occur
}
```

## Control Flow

If statements are largely the same as any other language except:

* It only expects expressions that evaluate to bool. Anything else will NOT be converted to bool.
* the "if" in this line in rust terminology is actually an expression.
```Rust
let number if condition { 5 } else {6}; //this is totally valid because if-else evaluate to an expression
```

There's a keyword loop that creates a non-ending loop that can only be cut out with a break statement

```Rust
loop {
	if condition {
		break;
	}
}
```

loops can also bring back values

```Rust
let mut count = 0;
let result = loop {
	count += 1;
	if count == 10 {
		break count //not putting semicolon at the end will make this line an expression and the value of result will become 10
	}
}
```

You can label loops like this:

```Rust
'outer: loop {
...
	loop {
	...
	break 'outer;
	}
}
```

label names **must** begin with a single quote ( ' ).

There's also While and for control flows that work just like in other common languages (They work similarly to python). You can use While to loop if a condition is true and you can use for to loop through collections and arrays like python

```Rust
for item in array {
	println!("{item}");
}
```

You can do this to countdown. The rev() function starts the range from reverse, meaning 4 instead of 1.

```Rust
for i in (1..4).rev() {
	println!("{i} seconds remaining!");
}
```

## String Type

A complex type that allocates memory on the heap in Rust is the String. 

You can create a String from a string literal like this:

```Rust
let x = String::from("Hello");
```

This type of string can be mutated

```Rust
let mut s = String::from("Hello");

s.push_str(", World!");
```



