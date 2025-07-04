wFirst introduced to the world in 2002, intending to replace the [[COM Model]] and create a more flexible and simpler solution.

## CLR: Common Langauge Runtime

CLR, called the *Common Language Runtime*, is the runtime environment of C# which handles loading, locating and managing .NET entities and also low level details such as coordinating threads, memory management, etc.

## CTS: Common Type System

*Common Type System*. The CTS specification details all possible data types and .NET entities.

## CLS: Common Langauge Specification

A subset of the CTS which every .NET aware language can support.
## Managed Code

You can not use C# for non-dotnet related runtimes, officially speaking, code targetting the .NET runtime is called **managed code**. Code not targeting .NET is called **unmanaged code**.
## Assembly

The binary unit that contains managed code is called an *assembly*. .NET binaries have no platform-specific instructions and instead have platform-agnostic instructions called the *Common Intermediate Language (IL)* and aside from that, they also contain type metadata that describes in detail the characteristics of each type within the binary like classes, enums, structs,...

Typically these CIL instructions are not compiled to platform-specific binary until absolutely necessary. 

The entitity that compiles CIL code is called the *jitter*. The .NET runtime uses a JIT compiler for each target CPU with its own specific optimizations in mind. The jitter will also cache the results in memory, so after first compilation, there won't be any need to recompile previously compiled pieces of code or methods.

## Types

Every type in the CTS is inherited by the System.Object class.

Each type has a TryParse method that tries to parse a string and returns a System.Boolean of its success state.

## DateTime and TimeSpan

System namespace contains System.DateTime and System.TimeSpan

```C#
static void UseDatesAndTimes()
{
	DateTime dt = new DateTime(2015, 10, 17);//year, month, day
	TimeSpan ts = new TimeSpan(4, 30, 20);//hours, minutes, seconds
}
```

## Working With String

String has a few methods used for basic String data manipulation.

* Length property contains the length of string
* Compare(): Compare two strings
* Contains(): Determines if string contains substring input
* Equals()
* ToUpper()
* ToLower()
* Trim()
and many more, google when it's necessary...

**Strings are immutable in C#**. Any methods that appear to change it are actually returning you an instance of String and are not modifying the original object.

### Enums

Enums are custom data types with the form of key-value pairs and gain functionality from the System.Enum class type.

**Don't confuse enum and enumerator which are completely different concepts**

By default, C# stores Enum values as Int32 types. You can change this by specifiying the type next to the definition like this:

```C#
enum EmpType : byte 
{
	Manager,
	Grunt,
	Contractor,
}
```

Enums support a ToString method which returns string name of an enum value.

The GetValues() method returns an array contaning all of a particular enum's values.

```C#
Enum e {...};
.
.
.
Array enumData = Enum.GetValues(e.GetType());
foreach(i in enumData)
{
	Console.WriteLine(enumData.GetValue(i));
}
```

## Type Conversions

The runtime will implicitly widen the size of variables however implicit conversion for narrowing is not allowed. It needs to be explicitly stated.

The **checked keyword** can catch System.Overflow exceptions and send it to try-catch block. It's useful for checking for any overflows or underflows. 

Use **unchecked keyword** when data loss is acceptable.
## Control Flow

### If-Else

Similar to C++, except it can only operate on boolean expressions.

## For Loop

Similar to a C++ for loop. However there exists a foreach loop that iterates through arrays and vectors like in python.

### Switch Case

Switch statements can match strings as well.

Pattern Matching is possible in Switch case. In this way, you can compare for types as well.

```C#
object choice;
switch(choice)
{
	case int i:
		Console.WriteLine("Integer");
		break;
	case string g:
		Console.WriteLine("String");
		break;
	case float d:
		Console.WriteLine("Float");
		break;
	default:
		Console.WriteLine("Didn't understand your freaking choice");
		break;
}
```

## Arrays

This is how to define and declare an array

```C#
int[] array = new int[size];

string[] stringArray = new string[] {"one", "two", "three"};

var[] impArray = new[] {1, 2, 3, 4};//an implicitly typed array
```

All arrays have the Length property that return their current size.

**An implicitly typed array does *not* default to System.Object**

However, if you define an array by the System.Object type, you can have any type of data on it.

```C#
object[] objectArray = new object[2];
objectArray[0] = false;
objectArray[1] = 6.5f;
```

After creating an array,  you are free to pass and recieve it in methods.

Basic System.Array functionalities:

* Clear(): static method, sets a range of elements to empty
* CopyTo(): method, Copy array elements from source to destination
* Length: property, returns size of array
* Rank: property, returns the number of dimensions
* Reverse(): static method, reverse contents of a one-dimensional array
* Sort(): sorts intrinsic types that implement the IComparer interface

## Tuples

Lightweight data structures that contain multiple fields. It uses the ValueTuple data type in the CTS. The ValueTuple creates different structs based on the number of properties for a tuple.

```C#
(string , int, string) values = ("a", 9, "c");

public (int Xpos, int Ypos) Deconstruct() => (X, Y);
```

## Methods and Parameter Modifiers

A few modifiers can control how an argument is given to the called method.

* (None) Passing by value
* **out**: define a parameter as output. The method is obliged to give some value to the out parameter. You have to pass a variable as an output parameter while invoking methods.
* **ref**: passing a reference to an initialized existing variable.
* **params**: allows multiple variables to be passed as a single logical argument. It has to be at the end of the method parameters.

You can define methods to take optional arguments. Just give the parameter a default argument at the point of method definition.

The new value given to an optional argument needs to be apparent at compile time.

## Nullable Types

Value types by default can not have the value "null" because null is for representing an empty object reference. However by putting a question mark on a value type we can make it so it can contain a null value as well as its regular domain of values. It's useful for working with databases since Nulls are common there.

```C#
int? nullableNum = 1;
```

# Object Oriented Programming

Objects are user-defined types composed of field data (variables) and members that operate on that data (methods, constructors, events, etc). For more information on Object Oriented Methodology click [here](Object-Oriented-Paradigm)

### Constructors

You can chain constructor calls using the "this" keyword in C#. The this pointer represents the current instance of a class or stuct. The this pointer is a pointer accessible only within the nonstatic methods of a class or struct.

Designate the constructor with the most parameters as the "Master Constructor" and call each relevant constructor for each parameter and pass it to those constructors instead.

```C#
class Motorcycle{ 
	public int driverIntensity; 
	public string driverName; 
	// Constructor chaining. 
	public Motorcycle() {} 
	public Motorcycle(int intensity): this(intensity, "") {} 
	public Motorcycle(string name): this(0, name) {} 
	// This is the 'master' constructor that does all the real work. 
	public Motorcycle(int intensity, string name) 
	{
	if (intensity > 10){
		intensity = 10;
	}
	driverIntensity = intensity;driverName = name;
	}
}
```

When loading a class instance in memory, C# will first visit the master constructor and then goes on to finish executing any remaining instructions.

### Static keyword

Static methods in classes are used for methods that do not require an object instance to be used. Utility classes that provide a utility and does not maintain any state such as the Console class use static methods often.

A few notes:

* Static data of a class is shared by all objects of the class.
* A static member can not reference non-static members.
* Assigning a static member inside a constructor will reset its value each time an object is created.

# Delegates and Events



## LINQ

LINQ is a unified query system designed to retrieve and send data to any sort of database system. 

# Object Lifetime

When variables go out of scope they become candidates for garbage collection. 

Whenever the `new` keyword is encountered, a CIL `newobj` instruction is emitted. C# keeps its heap tidy which is why it is called the *managed heap*. The CIL will maintain a pointer that points to the next place in memory where the next object will be located. The newobj instruction does 3 things:
* Calculates how much memory the object will take
* Checks if the managed heap has enough space
* Advance the next object pointer and then return a reference to the object it created

If the CLR determines that the managed heap does not have space garbage collection will occur.

A storage location that has a reference to an object in heap whether global, static or local is called a *root*.
During the garbage collection process, the CLR will keep track of objects that are still reachable (Those that are rooted). It will build an object graph

The garbage collector gives generational labels on objects. newly created objects are generation 0. The GC first starts sweeping any unrooted generation 0 objects. The surviving objects get promoted to generation 1, If the application requires the additional space, it will start sweeping generation 1 objects after cleaning generation 0s. The surviving ones again are promoted to generation 2 and stay there. The idea here is that objects that stay very long in memory (For example the main window) then they are less likely to become unrooted than more temperory objects. giving specific generations to objects that have been proven to have stayed long are not bothered and GC won't waste its time checking their reachability.

Prior to .NET 4, Dotnet used a Concurrent Garbage Collection system. It would suspend all active threads on every collection cycle to ensure the app doesn't use the managed heap while a sweep was in process. After .NET 4 uses *background garbage collection*.

# Multi-Threading

Each program is a **Process** in Operating Systems. The Operating System isolates processes from each other and handles how much resources they take.

Each process can have the ability to define separate threads where many tasks can be done at the same time. However that will be true paralelism if both the hardware and OS support it.
If not each thread is given a time slice to operate and once the time slice is up, the processor will shift contexts to another thread.

In order for this switching mechanism to work, each thread can write data to a Thread Local Storage. These threads are also provided with separate call stacks.

## Processes

We can use `System.Diagnostics` namespace for getting information about processes.


## Threads

The System.Threading namespace is one approach to building multi-threaded applications in C#/Dotnet. 

The `Thread` class represents a thread of the application.

```C#
Thread currThread = Thread.CurrentThread;
Console.WriteLine(currThread);
```