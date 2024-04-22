First introduced to the world in 2002, intending to replace the [[COM Model]] and create a more flexible and simpler solution.

## CLR: Common Langauge Runtime

Called the *Common Language Runtime*, is the runtime environment of C# which handles loading, locating and managing .NET entities and also low level details such as coordinating threads, memory management, etc.

## CTS: Common Type System

*Common Type System*. The CTS specification details all possible data types and .NET entities.

## CLS: Common Langauge Specification

A subset of the CTS which every .NET aware language can support.


## Managed Code

You can not use C# for non-dotnet related runtimes, officially speaking, code targetting the .NET runtime is called **managed code**. 

## Assembly

The binary unit that contains that managed code is called an *assembly*. .NET binaries have no platform-specific instructions and instead have platform-agnostic instructions called the *Common Intermediate Language (IL)* and aside from that, they also contain type metadata that describes in detail the characteristics of each type within the binary like classes, enums, structs,...

Typically these CIL instructions are not compiled to platform-specific binary until absolutely necessary. 

The entitity that compiles CIL code is called the *jitter*. The .NET runtime uses a JIT compiler for each target CPU with its own specific optimizations in mind. The jitter will also cache the results in memory, so after first compilation, there won't be any need to recompile previously compiled pieces of code or methods.

## Types

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
* Contains(): Determins if string contains substring input
* Equals()
* ToUpper()
* ToLower()
* Trim()
and many more, google when it's necessary...

**Strings are immutable in C#**. Any methods that appear to change it are actually returning you an instance of String and are not modifying the original object.

## Type Conversions

The runtime will implicitly widen the size of variables however implicit conversion for narrowing is not allowed. It needs to be explicitly stated.

Thec **checked keyword** can catch System.Overflow exceptions and send it to try-catch block. It's useful for checking for any overflows or underflows. 

Use **unchecked keyword** when data loss is acceptable.


## Control Flow

### If-Else

Similar to C++, except it can only operate on boolean expressions.

## For Loop

Similar to a C++ for loop. However there exists a foreach loop that iterates through arrays and vectors.

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

