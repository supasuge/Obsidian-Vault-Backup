## Polymorphism

Polymorphism is one of the four fundamental principles of Object-Oriented Programming (OOP), alongside Encapsulation, Inheritance, and Abstraction. The term originates from the Greek words "poly," meaning many, and "morph," meaning forms. Thus, polymorphism is the ability of an entity to take on many forms.

In C#, polymorphism is generally realised through method overloading and overriding.

### Method Overloading

Method overloading, also known as static or compile-time polymorphism, is a technique that allows multiple methods with the same name but different parameters (in terms of number, type, or order) to coexist within a class.

```csharp
public class Mathematics
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }
}
```

In the above class `Mathematics`, the method `Add` is overloaded: one version of the `Add` method accepts two integers, while the other accepts two doubles. The correct version of the method is selected at compile time-based on the arguments supplied.

### Method Overriding

Method overriding, on the other hand, is a form of dynamic or run-time polymorphism. It allows a derived class to provide a different implementation for a method already defined in its base class or one of its base classes. The method in the base class must be marked with the `virtual` keyword, and the method in the derived class must use the `override` keyword.

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("The animal makes a sound");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("The dog barks");
    }
}
```

In the above example, the `Dog` class overrides the `MakeSound` method of the `Animal` class. When `MakeSound` is called on an object of type `Dog`, the overridden version in the `Dog` class is executed.

The concepts of overloading and overriding extend to operators and properties, adding flexibility and expressiveness to C# programming.

### Operator Overloading

Just like methods, C# allows operators to be overloaded. This enables custom types to be manipulated using standard operators, enhancing code readability and intuitiveness. For example, for a `Vector` class representing a mathematical vector, you might overload the '+' operator to perform vector addition:

```csharp
public class Vector
{
    public double X { get; set; }
    public double Y { get; set; }

    public Vector(double x, double y)
    {
        X = x;
        Y = y;
    }

    public static Vector operator +(Vector v1, Vector v2)
    {
        return new Vector(v1.X + v2.X, v1.Y + v2.Y);
    }
}
```

In this example, instances of `Vector` can be added using the `+` operator, just like primitive types:

```csharp
Vector v1 = new Vector(1, 2);
Vector v2 = new Vector(3, 4);
Vector sum = v1 + v2;  // { X = 4, Y = 6 }
```

### Property Overriding

In C#, properties, like methods, can be overridden in derived classes. A base class declares a virtual property, and derived classes can override this property to change its behaviour.

```csharp
public class Animal
{
    public virtual string Name { get; set; }

    public Animal(string name)
    {
        Name = name;
    }
}

public class Dog : Animal
{
    public Dog(string name) : base(name) { }

    public override string Name
    {
        get { return base.Name; }
        set { base.Name = value + " the dog"; }
    }
}
```

In this case, a `Dog` object modifies the behavior of the `Name` property to append " the dog" to any name assigned to it:

```csharp
Dog myDog = new Dog("Rex");
Console.WriteLine(myDog.Name);  // "Rex the dog"
```

These examples underline the power of polymorphism in C# and object-oriented programming. It allows classes to provide tailored implementations of methods, operators, and properties, enabling more natural, expressive, and aligned code with the problem domain.

## Abstraction
In OOP, abstraction is the concept of simplifying complex reality by modeling classes appropriate to the problem and working at the most appropriate level of inheritance for a given aspect of the problem. It is a mechanism that represents the essential features without including the background details.

Abstraction in C# is achieved by using `abstract` classes and `interfaces`. An `abstract` class is a class that cannot be instantiated and is typically used as a base class for other classes. `Abstract` classes can have `abstract` methods which are declared in the `abstract` class and implemented in the derived classes.

```csharp
public abstract class Animal
{
    public abstract void Speak();
}

public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("The dog barks");
    }
}

public class Cat : Animal
{
    public override void Speak()
    {
        Console.WriteLine("The cat meows");
    }
}
```

In this example, `Animal` is an abstract class with an abstract method `Speak`. `Dog` and `Cat` classes are derived from `Animal` and provide their own implementation of `Speak`. When `Speak` is called on an object of type `Animal`, the appropriate version of `Speak` is invoked depending on the actual type of the object.

Abstraction using `Interfaces` is another way to achieve abstraction. An `interface` is like an `abstract` class with no implementation. It only declares the methods and properties but doesn't contain any code. A class that implements an interface must provide an implementation for all the interface methods.

```csharp
public interface IAnimal
{
    void Speak();
}

public class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("The dog barks");
    }
}

public class Cat : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("The cat meows");
    }
}
```



