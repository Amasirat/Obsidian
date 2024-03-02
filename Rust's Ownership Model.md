Ownership is a set of rules that guarantees [[Rust]] is memory safe without a grabage collector. 

There are largely three important rules in Rust regarding variables:

* Every variable has an *owner*.
* There can only be one owner at a time.
* When owner goes out of scope, its value is dropped.

Scope rules are similar to C++.

Any heap allocated memory will be returned automatically when an owner goes out of scope.

When a heap allocated variable goes out of scope, a special function called "drop" is called that acts as that variable's destructor.

## Copying and moving and Which one Rust Does When

Heap allocated variables, when being assigned to a new variable, will technically *move*. That is, A copy of the pointer to data, capacity,... is made to the other variable. Basically a *Shallow Copy*.

However because of the "No duplicate owners" rule of Rust, the variable being assigned will no longer be valid. The variable will be permanently moved to the other variable.

```Rust
let s1 = String::from("Hello");
let s2 = s1;
// s1 is no longer valid
```
![[trpl04-04.svg]]
The same is true when passing these variables to functions. Once moved to the function's parameters, they will no longer be valid.

There is a way to *deep copy* Strings, and that is by using a common Clone method.

```Rust
let s1 = String::from("Hello");
let s2 = s1.Clone();
// both variables can now be used as they are copies
```
![[trpl04-03.svg]]

**Variables with the copy trait are not moved but copied on automatic assignment** because they're inexpensive to copy and are usually allocated on the stack, meaning no special "drop" actions is necessary when they go out of scope. Most scalar types and also tuples *that don't contain variable types with a non-copy trait* have the copy trait that allows Rust to freely copy them however it wishes.

```Rust
let x: u32 = 10;
let y = x;
// x is still valid
```

## Borrowing

We can borrow variables using a reference instead of moving ownership. Therefore we can pass it to functions without losing access to them. 

![[trpl04-05.svg]]

```Rust
let s1 = String::from("hello");
calculate_length(&s1);//We're passing a reference to s1 instead of moving it to the function parameter
fn calculate_length(s: &String) -> usize
{
s.len()
}//s is out of scope however because it does not own data, the data is not dropped.
```

However we can not change the values of something we borrowed like this.

## Mutable References

References that are able to change the value they are borrowing

```Rust
let mut s1 = String::from("Hello");
let r1: &mut String = &mut s1;
```

In order for mutable references be possible, the value they are referencing has to also be mutable.

However, huge Caveat!

You can not make mutable references to one variable more than once.

You can also not make a normal reference to one that has already been borrowed by a mutable reference. This is to prevent data races to occur in Rust, where more than one variable has access to data to write into.

However **Multiple immutable references are allowed**.

**Rust also does not allow dangled references.**

## Slices