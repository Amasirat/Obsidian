C++ is an object oriented language created by Bjourne Stroustroup. It's a compiled language meaning it has to be compiled all at once into a binary program that a machine can run.

**Compilation** is the entire process of **translating C++ code** into code that the computer can understand.
The entire process has multiple stages:
- **Preprocessing**: Handles preprocessor directives (like `#include`) and expands macros.
- **Compilation**: Translates C++ into assembly code.
- **Assembly**: Converts assembly to machine code.
- **Linking**: Combines multiple files and resolves external symbols to create the final executable.

*Headings:*

[[#Basics]]
[[#Control Flow]]
[[#Arrays]]
[[#Functions]]
[[#References]]
[[#Pointers]]
[[#Structs and Enums]]
[[#Exceptions]]
[[#File Handling]]
# Basics

Every C++ program needs to have an entry point called main

```C++
int main() 
{
	return 0; // This is a code that is sent to the operating system,
	// It's just a convention that signifies the program ran successfuly 
}
```

In order to print anything on the console you can use this code:
```C++
std::cout << "Hello, World!\n"; // \n means new line
// It's werid syntax but you'll understand "<<" later...hopefully
```

Variables are places created in memory to save data that the program needs.
Every variable has these caracteristics:
* name (or identifier)
* value (What value it stores)
* type (What type the variable is)
* address (The address of the data in memory)

The computer stores everything in 0s and 1s, Depending on the type of data it is, the computer *has* to interpret those digits differently. For example if it's a character or if its a number, etc.

C++ came with the solution of making the programmer **dictate** what type the variable it creates is.

*Extra note: C++ is called a statically typed language because of this fact. Dynamically typed languages also exist that don't need type declarations. There are detractors and supporters of both approaches but what's important is that in dynamically typed langauges, the language does more to infer the type your variable is and that could make performance impacts. However at the end of the day it's not by that much, so unless you are developing for a really performance-sensitive task or an embedded hardware with limited resources, it should not matter.*

There are a few important data types defined in C++ by default:

* int (4 bytes on most computers, however some old compilers could make it 2 bytes)
* float (4 bytes)
* double (8 bytes)
* long
* short (2 bytes)
* char (1 byte)
* bool
```C++
int integer = 5;
float floatingNumber = 3.54f; // it has less precision than double
double doubleNumber = 3.264562;
char character = 'd';
bool boolean = true;
```

ASCII is an old standard of codifying English Characters into numbers so that they can be represented by binary numbers. a char is simply one byte for that reason.

These are known as the **Fundamental Types** and they are inexpensive to create and use.
## Operators

C++ contians a concept called **operators** which are similar to operators in math, however it covers a lot more operations in C++.
### Arithmetic Operators

* addition: + (a+b)
* subtraction: - (a-b)
* multiply: \* (a\*b)
* division: / (a/b)
* modulator: % (a%b): This operator returns the **remainder** of the division between two numbers.
	* 8 % 2 = 0
	* 9 % 2 = 1
* assignment: = a = 5: 
	* **Important**: In C++ (And most languages) '=' actually just means assignment (This is= that) and **Not mathematical equality**. For example, this is possible even though it does not make any mathematical sense. a = a + 8
* +=: a += x is short form of a = a + x;
* -=: Same but for subtraction
* \*=: Same but for multiplication
* =: Same but for division
* Incrementor: You can use this operator to add 1 to a variable. x++ means x = x + 1
	There are two types of incrementors:
		* post-incrementor: x++
		* pre-incrementor: ++x
**++x has more priority than x++**

```C++
int x = 5;
std::cout << x++; // prints 5 and then increments x by 1
std::cout << ++x; // increments x by 1 and then prints 6
```

Here's how to read input from user:

```C++
int x;
int y;
std::cin >> x;
std::cin >> x >> y; // You can chainload input reading
```
# Control Flow

Programs get fun when you can change the route of your program based on the data you have.

## Conditional Control Flow

There are two methods of controlling the flow of the program based on conditions.

For instance, If this variable is 2, then do this else do something else.

```C++
if(...)
{


}
else if (...)
{

}
else
{

}
```

What goes into the if paranthesis has to be either evaluated to a boolean expression or be type-casted into one either explicitly or implicitly by the compiler.

There's an operator that handles a similar thing in less limited cases known as the ? operator.

```C++
boolean_expression ? "do something" : "else do something else";
```

This is the only operator that takes in *three* operands. It is usually used for simple one line cases of if-else statements.
## Loops

There are four types of loops:

* **while**
* **do-while**
* **for**
* **foreach** (discussed after introducing arrays)

# Arrays

Arrays are groups of variables that are stored sequentially in memory.

Why would you use Arrays?
If you can have variables that are related to each other, it is better to create a sort of list to store them together, both for readability of code, organization of code and optimizability. If you store related variables in arrays, they will be put together in memory, which makes the task of retrieving them relatively trivial for the compiler. (Although nowadays the most important benefit is for our own code readability's sake)

```C++
int array[5];// The number indicates what size the array is
int array[] = {5, 2, 3, 8, 7};// if you initialize array, the size will be inferred
```

The number of an element of an array is called an **index**.

Arrays start from 0, so for the above arrays, the indices are from 0 - 4.

You can access an element within an index like this:
```C++
std::cout << array[2] << '\n'; // prints 3
```

The best way to sequentially access all array elements is with a for loop
```C++
constexpr int n = 5;
int array[n] = {...};
for(int i = 0; i < n; i++)
{
	std::cout << array[i];
}
```

This array is called a static array and it needs to know what length an array will be at compile time, **Therefore you can not give it dynamic size (size that can change during the program's runtime)**

```C++
int y;
std::cin >> y;

int array[y]; // This will not compile
```
# Functions

# References

# Pointers

Pointers are known as some of the most confusing aspects of C/C++. Although the actual idea of a pointer variable is fairly straightforward, it's its execution that can confuse a lot of first time programmers.

Pointers are bascially types of variables that *point* to another variable inside memory. **They contain the address of that variable**, in simple terms.

```C++
int x = 2; // This is a variable

int* ptr = &x; // address of variable is stored inside ptr 
```

*We can't initialize a pointer by giving it a literal address because the compiler will treat that address as an int instead of an int\* type that a pointer needs.*

```C++
int* ptr2 = 0X000125; // Not Allowed. Will not compile!!!!
```

You can dereference a pointer to get the value that its address stores:
```C++
std::cout << *ptr << '\n'; // Prints 2
```

**Difference between a pointer and a reference is this:**

* A pointer, unlike references can change what variable it is pointing to.
* A pointer can be uninitialized unlike references (not recommended to do but it *is* possible...)
* references are not objects but pointers are.

**If you dereference a pointer and assign a new value to it. You will change the value in the address of memory that the pointer holds**

**(&) returns a pointer type instead of an address literal.**

```C++
int x = 2;
std::cout << typeid(&x).name << '\n'; // prints int*
```

A pointer can hold a *null* value as well. Basically meaning that it does not point to anything. This is helpful because we can avoid dereferencing a dangling pointer if we can check for a pointer's null value. Granted, that is supposing the programmer makes sure when a reference is no longer valid, the pointer will change its value to null.

```C++
if (ptr == nullptr) { // You can use the NULL macro as well
	std::cout << "ptr is empty" << '\n';
}
else
{
	std::cout << *ptr << '\n';
}
```

**Do not let a pointer reference to an invalid address, if the pointer does not have a valid address, assign it to null**

**Some Tidbits:**

* A pointer can't point to a const variable

* A pointer to const can point to any type of variable and it means that the pointer can't change the variable it points to.

* A const pointer is different than a pointer to const variable and it means it can't change what **address** it points to
## Dynamic Allocation

We can use this concept to create more interesting things. 

**Dynamic Memory Allocation** is a way for running programs to request memory from the operating system. 

### Stack and Heap

A short detour to the difference between stack and heap is important.
When a program starts running, the operating system will give the program an appropriate place in memory to do its thing.

Programs usually divide the memory they are given into different parts that operate in different ways.

* The text Segment: Holds code instructions
* Static and global data segments: Holds all static or global variables
* Stack and Heap segments

The most important parts for programmers to understand is the difference between a heap and stack memory.

Almost all code that you've written up untill now lived strictly in stack memory. Stack memory is great at fast allocation and deallocation and C++ does all of that for you automatically. The program also has instant access to anything that lives in there.

The heap however is the opposite. It's a portion of the memory that deals with any dynamic allocations. Dealing with the heap is very slow because it needs a lot of setup for even one piece of memory. 

```C++
new int; // allocates memory for a dynamic int and it initializes it to a value
int* d = new int{5};
```

Now you can see the usefulness of pointers, without pointers, you could never have access to the memory you've allocated.

Then if you want to de-allocate use delete for the pointer holding the address.

```C++
delete d;
```

It will not actually *delete* anything, but it will give the operating system that portion of memory back.

After this, the pointer will have a dangling reference, so you have to give it a null value if you wish to use that pointer in the future.

For creating a dynamic array, it will be similar to creating a dynamic variable in the heap. It basically does the same thing, however it creates sequential memory to the amount that you give it.

**You can give it a dynamic size**

```C++
int length = 5;
int* array = new int[length]{};
```

and you can deallocate it the same way, except a minor difference.

```C++
delete[] array;
```

*delete* needs to know that it has to de-allocate a series of memory instead of just one. It keeps track of the length of the array itself so you don't have to worry, just put a \[] at the end like that!
# Structs and Enums

# Exceptions

# File Handling


