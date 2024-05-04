
SOLID principle is a set of five abstract principles that dictate how a maintainable and flexible software should be designed.

## Single Responsibilty

A class should have only **one** responsibility or only one reason to change.

```C#
class Employee
{
...
//extra responsibility
public void Save()//this is not the responsibility of the Employee class
{
...
}
}
```

This principle facilitates simpler testing flows as by giving a class just one responsiblity, managing its behavior will become easier however if a class has multiple responsibilities, changing code related to one responsibility may impact the behavior of another one of its responsibilities. Making maintaining, testing, and debuging your classes that much harder.
## Open-Closed

Classes should be opened for **Extension** and not **Modification**. Meaning if you want to extend the functionalities of a class, derive a new class for example to be able to extend its functionalities, or create a common interface, to extend needed functionality to other classes.

This way testing will become simpler, as changing a single class everytime will force you to test the behavior of the *entire* class everytime. However this way the original class will not change, so there would be no need to test and the possibility that the behavior of the original class will change no longer exists.

## Liskov Substitution

It states that an object of a child class must be able to **replace** an object of the parent class without **breaking** the application.

Basically a child class should be able to do anything the **parent class** can do **or more**. There shouldn't be a method or property of a parent class that a child class could not implement or use.
## Interface Segregation

A class should not be forced to implement interfaces that it does not use.

```C#
public interface IVehicle
{
	void Drive();
	void Fly();
}

public class FlyingCar : IVehicle
{
	public void Drive();
	public void Fly();
}

public class Car : IVehicle
{
	public void Drive();
	public void Fly();//can not be used by car
}
```

One method of avoiding this issue is to seperate and in other words segregate interface functionalities into different and seperate interfaces

```C#
interface IDrive
{
	void Drive();
}
interface IFly
{
	void Fly();
}
```

It's usually recommended to have multiple smaller interfaces than one large interface.

## Dependency Inversion

A **high-level** class should not depend upon a **lower-level** class.