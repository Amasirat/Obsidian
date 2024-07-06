First introduced to the world in 2002, intending to replace the [[COM Model]] and create a more flexible and simpler solution.
## CLR: Common Langauge Runtime

CLR, called the *Common Language Runtime*, is the runtime environment of C# which handles loading, locating and managing .NET entities and also low level details such as coordinating threads, memory management, etc.

## CTS: Common Type System

*Common Type System*. The CTS specification details all possible data types and .NET entities.

## CLS: Common Langauge Specification

A subset of the CTS which every .NET aware language can support.
## Managed Code

Officially speaking, code targetting the .NET runtime is called **managed code**. Code not targeting .NET is called **unmanaged code**.
## Assembly

The binary unit that contains managed code is called an *assembly*. .NET binaries have no platform-specific instructions and instead have platform-agnostic instructions called the *Common Intermediate Language (IL)* and aside from that, they also contain type metadata that describes in detail the characteristics of each type within the binary like classes, enums, structs,...

Typically these CIL instructions are not compiled to platform-specific binary until absolutely necessary. 

The entitity that compiles CIL code is called the *jitter*. The .NET runtime uses a JIT compiler for each target CPU with its own specific optimizations in mind. The jitter will also cache the results in memory, so after first compilation, there won't be any need to recompile previously compiled pieces of code or methods.

## Types

Every type in the CTS is inherited by the System.Object class which is represented by *object* inside C#.



Each type has a TryParse static method that tries to parse a string and returns a System.Boolean of its success state.

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
* Assigning an static member inside a constructor will reset its value each time an object is created.
* a static constructor is only called once and handles the assignment of value to static data fields of a class, especially when the data is not known at compile-time.
* a static class can be created. It will only allow static members and will not be able to be created using the new keyword. Utility classes are usually static classes.

## Access Modifiers

* public: No access restrictions
* private: Only accessed by class
* protected: Only accessed by class and its children gained through inheritence
* internal: Only accessed by the current assembly.
* protected internal: Only accessed within the class and its children of the current assemby

**Default access modifier is private and classes are internal**

Non-nested types (for example a class) can not be assigned as private.

You can encapsulate object state using .NET properties. Properties are basically a simplified form and way of writing set and get methods for state.

```C#
public string Name
{
	get { return name; }
	set
	{
		if (value.Length > 15) //value is the value that a user outside the class gives the property
			Console.WriteLine("Error! Name length exceeds 15 characters!");
		else
			empName = value;
	}
}
```

**value** is a *contextual keyword* only in the set scope. 

You can then treat these properties as variables outside of the class and in user space.

A few notes:
* Omiting each of the get or set blocks will make the property either "write-only" or "read-only".
* You can use *automatic property syntax* to create properties that do not have particular bussiness logic required.
```C#
public string PetName { get; set; }
public int Speed { get; set; }
public string Color { get; set; }
```
These properties don't need private backing fields as they create their own automatically at compile time.

From C#6 onwards, you can create read-only properties **however no write-only properties**.

## Constant Field Data

Constant fields are implicitly static and class-level.

a const field data has to be initialized at the point of definition, and at compile-time.

## Read-only Fields

A read-only field can not be changed, just like a constant however you can give it data at runtime and can be legally asigned within the scope of a constructor but no where else.

```C#
public readonly double PI;
```

readonly fields are **not static implicitly**. 

In order to define a static readonly variable, you have to mark it explicitly as static. If you want to give it value at runtime, you'll need to use a static constructor.

## Partial Classes

You can seperate class definitions throughout your codebase given that it is in the same .NET namespace and its name is identical each time it is referenced again.

```C#
class Car
{
//constructors

//fields

//properties

//methods
}

//above turns into

partial class Car
{
//constructors

//fields
}

partial class Car
{
//properties

//methods
}
```

At assembly level, not much is made and it will still combine into a full class, however it is mostly used to seperate boilerplate, and actual useful sections of the class that a developer has to interact with during development.

## Inheritance

There are two flavors of Inheritance: 

* **Classic Inheritance**: The "is-a" relationship
* **Containment/Delegation Model**: The "has-a" relationship
## Classical Inheritance

You can derive classes from a base class in C#. 

```C#
class MiniVan : Car
{
}
```

By doing this, the MiniVan access has every public field that the Car class has.

**Constructors, only belong to the current class so a derived class can not use a public constructor of a base class, however it can chainload using it in its own constructors**

Derived classes do not have access to any private members of the base class.

C# demands a class have only **one** base class. **Multiple Inheritance is not allowed**.

if you mark a class as "sealed", that class can not be derived from any further.

```C#
//MiniVan will be derived from Car, however no other class can be derived from MiniVan.
sealed class MiniVan : Car
{
}
```

*C# Structs are always implicitly sealed.*

You can chainload constructors of the base class to fill in the properties that are base class related.

```C#
class Manager : Employee
{
	public Manager(string fullName, int age, int empID, float currPay, string ssn, int numbOfOpts) : base(fullName, age, empID, currPay, ssn)
	{
	StockOptions = numbOfOpts;
	}
}
```

You will be able to use an appropriate constructor from the base class inside a constructor of a derived class if that specific constructor exists.

using "base()" is not restricted to just constructors.



## Containment/Delegation Model

You can create data fields of another type of class to show that a class "has" another class within itself

```C#
class Car
{
private Engine eng1;
}
```

## Nested Types

You can create class level classes, Enums, and structs which are relevant. For example a soldier in a game can only have a few types of weapons which can be enforced by creating a class-level enum to contain the types of weapons they are allowed to have.

# Polymorphism

*Polymorphism* provides a way for sub-classes to perform a particular method in different ways using *method overloading*.

A base class has to indicate that a method it contains can be overriden by a sub-class, and that indicator is the *virtual* keyword. These methods are called *Virtual Methods*.

```C#
partial class Employee
{
	public virtual void GiveBonus(float amount)
	{
		Pay += amount;
	}
}

partial class Manager : Employee
{
	public override void GiveBonus(float amount)
	{
		...
		base.GiveBonus(amount * ...);//you can use the base method, if an implemantation exists for this method
	}
}
```

You can also seal virtual methods from being further overridden by another derived class.

```C#
public override sealed void GiveBonus()
{
....
}
```

The further derived classes in the chain will not be able to override this method anymore.

You can use the *abstract* keyword to create abstract classes that can not be instanced.

You can also use the same keyword to force derived classes to override a method from an abstract base class.

```C#
public abstract void Draw();
```

Abstract methods can only be defined in abstract classes.

You can hold any derived classes from the base class inside a variable of the base class type.

```C#
Shape[] shapes = {Circle, Hexagon, ...}//you can have arrays of type shape which is allowed to take any derived class from shape
```

You can shadow methods of the base class with the *new* keyword. Essentially using your own definition of a function or property instead of the one from base class.

```C#
public new void Draw();

public new string PetName{set; get;};
```

## Class Casting rules

We have two rules regarding how casting derived classes work.

* Rule 1: Any derived class can be implicitly upcasted to one of their base classes but not vise-versa.
* Rule 2: You can explicitly castdown a class to one of its derived classes.

Casting is evaluated at runtime, so any invalid castings will not be caught by the compiler. Instead you will get an InvalidCastException error.

You can use the *as* keyword to determine if a cast is valid at runtime.

```C#
object[] things = new object[4];
things[0] = new Circle();
things[1] = false;
things[2] = 3;
things[3] = "three";

foreach(object item in things)
{
	Circle c = item as Circle;
	if (c == null)
		Console.WriteLine("item is not a circle");
	else
		c.Draw();
}
```

There is also the *is* keyword that returns false if the casting failed.

*Each class you define will implicitly inherit from System.Object if another base class is not defined*
These are the members of the object class:

* **virtual bool Equals(object)**: it returns true if an object refers to the same memory. You can override it to instead check for internal state of two objects. if this method is overriden , GetHashCode needs to be overriden as well.
* **virtual void Finalize()**: garbage collection related function
* **virtual int GetHashCode():** returns an int that identifies object instance.
* **virtual string ToString()**: returns a string representation of the current type
* **Type GetType()**: returns a Type object that describes fully the currently referenced type.
* **object MemberwiseClone()**: a method that returns a member-by-member copy of the current object. It has a *protected* access level.
# Interfaces

Interfaces are a set of abstract methods. Their difference to abstract classes is that they are *just* a set of abstract members and there are no constructors or the sort. 

The problem with using abstract classes to define properties for classes is that only derived classes can access them and also each derived class *has* to define every abstract member even though it might not make sense to do so. With interfaces, any class, even ones seemingly unconnected to each other in their chain of hiearchy can define properties from an interface.

Interfaces do not specify base classes and are not even derived from System.Object, and the members of interfaces are always public and abstract so there is no need for specification on that front.

After defining an interface, when defining class types, interfaces have to come after a derived class(if one exists) and are seperated by commas.

**Any type given properties of an interface, has the responsibility to define every abstract member of the interface for itself**

In order to tell which Interfaces are implemented for a given type, you can use an explicit cast to that interface type. If it fails it will throw an InvalidCastException. You can also use the *as* and *is* keywords.

```C#
interface IPointy 
{
	byte GetPoints();
}

void Main()
{
	Hexagon hex = new Hexagon();
	if(hex is IPointy _)
	{
		Console.WriteLine(hex.GetPoints());
	}
}

```

**If you have multiple interfaces that contain the same method and which a class implements, you can use the dot operator to signify which interface's method you are going to implement.**

The two wide uses of interfaces are:

* There are characteristics that only a subset of classes in the inheritance chain have.
* There are characteristics that a bunch of unrelated classes should have.

## Some Common Interfaces in .NET ecosystem.

### IEnumerable

In order for a type to be used by the C# foreach construct, it has to implement the GetEnumerator() method which is formalized by IEnumerable. 

GetEnumerator() itself returns a reference to another interface called IEnumerator
### IEnumerator

It is an interface that provides the formalization of members dictating the infrastructure of Enumerating through a class.

If you are creating an Enumerable class, you can either define all of the abstract methods of these interfaces yourself, or you can simply call the definition of the System.Array.
```C#
using System.Collections;

class Garage : IEnumerable
{
	private Car[] carArray = new Car[4];
	public Garage()
	{
		carArray[0] = ...
		...
		..
		.
	}
	public IEnumerator GetEnumerator()
	{
		return carArray.GetEnumerator();
	}
}
```

Or you can use *yield* as well

```C#
class Garage : IEnumerable
{

	public IEnumerator GetEnumerator()
	{
		foreach (Car c in carArray)
		{
			yield return c;
		}
	}
}
```

*yield* keyword determines the value that needs to be returned to the caller's foreach.

### ICloneable

The ICloneable interface contains the object Clone() method. You can implement this interface and define this method in order to be able to create a copy of the class state into another variable.

**If your type contains only valuetypes, use MemberwiseClone(). If not you have to create a new object to take into account any reference type members**
### IComparable

Provides a key in order to be able to compare two class states or be able to sort.

```C#
class Car : IComparable
{

int IComparable.CompareTo(object obj)
{
	Car temp = obj as Car;
	if(temp != null)
	{
		if(this.CarID > temp.CarID)
			return 1;//every number more than zero means greater than
		if(this.CarID < temp.CarID)
			return -1;//any number less than zero means smaller than
		else
			return 0;//zero means equal
	}
	else
	{
		throw new ArgumentException("Parameter is not a car!");
	}
}
```
# Structued Error Handling

Exception handling in .NET is streamlined.

The System.Exception class is the main exception class that encapsulates exception message and call stack trace.

System.SystemException is a class derived from Exception that only .NET runtime throws.

System.ApplicationException is another class derived from the main Exception class meant to be used to derive your own specific Exception class tailored to your own codebase.

Most of these different exception classes do not change much from the original Exception class and are used to know what sector of the application rasied an error with the help of the *is* keyword.

System.Exception is also derived from System.Object.
# Collections and Generics

The most primitive container is an Array, however arrays are fixed sizes, so there are different types of containers in the System.Collections namespace that contain different data structures for varied usecases. 

Collection classes are built to dynamically resize themselves. There are two types of collection classes:

* Nongeneric: Those that can not be given a type and can take in types they are designed for
* Generic: Which have explicit static types

## Boxing

**Boxing refers to the act of assigning a value type to a System.Object.
```C#
int myInt = 5;
object myIntBox = myInt;
```

When you box a value, the CLR creates a new object on the heap and copies the value inside and returns a reference to it.

**Unboxing is the process of copying the value of a boxed object to a variable of appropriate type on the stack**
```C#
object myIntBox = myInt;
int unboxed = (int)myInt
```

If this fails, an *InvalidCastException* will be thrown.
## System.Collections

System.Collections contains nongeneric collection classes. When data is added to these class instances, they will usually be boxed into an object type.

Some collections include:
* ArrayList
* BitArray: Stores 0s and 1s
* Hashtable
* Queue
* Stack
* SortedList
## System.Collections.Generics

However because the process of boxing and unboxing takes resources and some time, it is not an efficient solution, not to mention there is a possibility that a cast will fail therefore it is not a type safe solution either. 
Generics exist to solve these problems that collections have.

Generics are a way of defining variables of any type T. They work kind of similarly to C++'s templates.

**Anything can be of a generic type except for enums**

Interfaces can also be generics. Forexample, here's IComparable, both of generic and nongeneric types.

```C#
//non-generic
public class Car: IComparable
{
	int IComparable.CompareTo(object obj)
	{
		...
	}
}
//generic
public class Car : IComparable<Car>
{
	int IComparable<Car>.CompareTo(Car obj)
	{
		...
	}
}
```

Most non-generic classes in System.Collections and other interfaces have generic equivelents.

## Collection Initialization

You can use *Collection Initialization Syntax* with classes that implement the ICollection interface or its generic equivelent.

```C#
//primative array
int[] myArray = {0,2,3,8,4,9,7};
//generic list
List<int> myGenericList = new List<int> {1,8,7,9,2,5,4};
//ArrayList
ArrayList myList = new ArrayList{"d", "i","c","k"};
```


# Delegates

Delegates are objects that point to a method or a list of methods and can invoke them. Similar to function pointers of C however Delegates in C# give asynchronous capability to the programmer.

When you define a delegate:

```C#
public delegate int BinaryOp(int x, int y);
```

The compiler creates a sealed class which contains the following methods

```C#
public sealed class BinaryOp : System.MuticastDelegate
{
	public int Invoke(int x, int y);

	public IAsyncResult BeginInvoke(int x, int y, 
	AsyncCallback cb, object state);

	public int EndInvoke(IAsyncResult result);
}
```

Invoke(), invokes the method the delegate is pointing to synchronously, while the other two methods begin invoking the method asynchronously.

## System.MulticastDelegate

The hidden class that is created when you use the delegate keyword is derived from System.MulticastDelegate class and that class is also derived from System.Delegate.

The MulticastDelegate has a few methods that it has inherited or implemented that you can use along with your delegate. 

* Delegate[] GetInvocationList(): List of methods that the delegate is pointing to. *(MulticastDelegate)*
* static Delegate Combine(params Delegate[] delegates): Add method to the list *(Delegate)*
* static Delegate Remove\/RemoveAll(Delegate source, Delegate value): remove method from invocation list *(Delegate)*
* Target: returns class name of the method the delegate points to if that method is *not* static *(Delegate)*
* Method: returns details of a method maintained by delegate *(Delegate)*


**When using delegates, it is advised to check if they are null first**

Giving a method to a delegate is like this:
```C#
public delegate int temp(int x, int y);

temp delegateVar = new temp(wh.Add);//adding a method through its ctor

class wh
{
	int Add(int x, int y)
	{
	...
	}
}
```
You can use the Combine() indirectly through this syntax.

```C#
public deligate int Notifier(int x, int y);

Notifier deligateVariable = new Notifier();
Notifier method = new Notifier(wh.Add);

deligateVariable += method;
```

You can remove a specific method like this.

```C#
deligateVariable -= method;
```

*Be careful that, if the deligate is empty, this will throw an error. Only do this if you know a deligate does have at least one other method.*

## Method Group Conversion

Instead of creating a seperate delegate to add a method to another delegate, *Method Group Conversion* allows you to pass in the method directly without going the extra step.

Passing in a method to an object deriving form the Delegate class will cast that method as a delegate.

```C#
public deligate int deligateVar(int x, int y);

deligateVar = new deligateVar();

deligateVar = wh.Add;
```

## Generic Delegates

```C#
public delegate void MyGenericDelegate<T>(T arg);
```

## Action<> and Func<> delegates

There are two delegates that C# has by default.

* Action<>: Which can take in any method with up to 16 parameters and **void** return type.
```C#
Action<int,int> AddDeligate = wh.Add;
```
* Func<>: Which can take in methods with any return type and 16 parameters.
```C#
string someFunction(int, double)
{
...
}
Func<int, double, string> funcDelegate = someFunction;
```

## Events

Instead of creating custom encapsulation methods in order to interact with a delegate of our class. We can instead use the event keyword to create an event that a class can fire. Basically abstracting away the process of registering and unregistering with our delegates.

```C#
class Car
{
public delegate void CarEngineHandler(string msg);

public event CarEngineHandler Explode;
public event CarEngineHandler AboutToExplode;

}
```

You can then use these events the same way as a delegate class.

```C#
if(Explode != null)
	Explode("Car Exploded");
```

Under the hood, when the event is called it will call the Remove(), Add() or Invoke() methods by itself.

You can listen for events in caller space like this:

```C#
Car.CarEngineHandler d = new Car.CarEngineHandler(functionToCall);//delegate
myCar.Exploded += d;//add delegate to event list
myCar.Exploded -= d;//remove delegate from event list

//or we can use method group conversion
Car c1 = new c1();

c1.AboutToExplode += CarIsAlmostDoomed;
void CarIsAlmostDoomed(string msg)
{
...
}
```

Instead of checking against null everytime you invoke a delegate, you can use *Null-Conditional Operator* to check it automatically. in C#6+ only

```C#
if(CarIsDead)
{
	Exploded?.Invoke("Sorry, Car's dead fam!");
}
```


# Anonymous Methods

Most of the time we register methods to events that we don't use anywhere else, therefore it is a bother to create a seperate method for that strict purpose, therefore there is a functionality called *Anonymous Methods* that lets us define an unnamed method that is only used when an event is called.

```C#
Sometype t = new Sometype();

t.SomeEvent += delegate(someArgs...) 
{ 
// Statements 
};
```

Anonymous methods have access to the local variables of the method they are used in. The variables are formerly known as *outer variables*. Some rules regarding this are:

* They can't use ref or out parameters
* it cannot have a local variable of the same name of its own, however it *can* have local variables with the same name as some in the class
* it can also access instance or static variables of the class of the method

# Lambda Expressions

Using Lambda expressions you can abbreviate anonymous methods even further.

```C#
Delegate t = new Delegate((T1 variable, T2 variable, ...) => {
	//Statements
});
```

The compiler can implicitly understand what type a variable is based on the definition of your delegate. If your lambda method is just one line, the brackets are not necessary. 

# Advanced Features






