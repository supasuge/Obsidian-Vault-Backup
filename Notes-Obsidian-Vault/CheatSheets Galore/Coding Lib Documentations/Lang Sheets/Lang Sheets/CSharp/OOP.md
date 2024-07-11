Object-Oriented Programming (OOP) is a programming paradigm that relies on the concept of "objects". Objects are instances of classes, which can contain data in the form of fields, often known as attributes, and code, in the form of methods. In OOP, computer programs are designed by making them out of objects that interact with one another.

There are four main principles of OOP:
1. `Encapsulation` is the practice of keeping fields within a class private and providing access to them via public methods. It's a protective barrier that keeps the data and implementation code bundled together within an object.
2. `Inheritance` is a process by which one class can acquire the properties (methods and fields) of another. With the use of inheritance, information is made manageable in a hierarchical order.
3. `Polymorphism` enables methods to be used as if they were the methods of a class's parent. It's the characteristic of an operation to behave differently based on the types of objects or arguments involved.
4. `Abstraction` represents essential features without including background details or explanations. It provides a simple interface and reduces complexity by hiding unnecessary details.


## Classes & Structs

In C#, a `class` is a blueprint for creating objects, and an object is an instance of a class. Class definitions start with the keyword `class` followed by the name of the class and typically encapsulate data and methods that operate on that data.

Classes are made up of two fundamental elements: `Properties` and `Methods`.

- `Properties` represent data about the class. They are often referred to as attributes or characteristics. For example, in a `Car` class, properties might include `Color`, `Model`, and `Year`.
- `Methods` represent actions or behaviour associated with the class. They are functions defined within a class. For instance, a `Car` class may have methods like `Drive()`, `Park()`, and `Brake()`.

```csharp
class Car
{
    // Properties
    public string Color;
    public int Year;

    // Method
    public void Drive()
    {
        Console.WriteLine($"The {Color} car from {Year} is driving.");
    }
}
```

In the above example, `Car` is a class that contains two properties (`Color` and `Year`) and one method (`Drive`).

To create an object in C#, you use the `new` keyword followed by the class name. This process is often called `instantiation` because you create an "instance" of a class.

```csharp
Car myCar = new Car();
```

In this line, `myCar` is an object of the `Car` class. You can now use the dot operator `.` to access its properties and methods:

```csharp
myCar.Color = "Red";
myCar.Year = 2020;
myCar.Drive();
// output: The Red car from 2020 is driving.
```

Remember that each object has its own copy of properties. Thus, if you create another `Car` object, it will have its own `Color` and `Year`:

```csharp
Car anotherCar = new Car();
anotherCar.Color = "Blue";
anotherCar.Year = 2021;
//output: The Blue car from 2021 is driving.
```

So even though `myCar` and `anotherCar` are both instances of the `Car` class, they have different property values. This allows objects to have unique states while sharing common behavior from their respective classes.

Classes can also have a `constructor`, which is a special method in a class or struct that is automatically called when an object of that class or struct is created. The primary purpose of a constructor is to initialise the object and its data members.

The constructor has the same name as the class or struct, and it doesn't have any return type, not even void. It can take parameters if needed.

```csharp
class Car
{
    // Properties
    public string Color;
    public int Year;
    
    // Constructor
    public Car(string c, int y)
    {
        Color = c;
        Year = y;
    }

    // Method
    public void Drive()
    {
        Console.WriteLine($"The {Color} car from {Year} is driving.");
    }
}
```

You can pass the parameters when the object is instantiated to set the variables.

```csharp
Car myNewCar = new Car("Pink", 2022);
myNewCar.Drive();
// output: The Pink car from 2022 is driving.
```

### Accessors

An `accessor` is a class member function that provides access to the value of private or protected data members. There are two types of accessors - `get` and `set`.

The `get` accessor is used to return the property value. It provides read-only access to the attribute it is assigned to. If only a `get` accessor is specified, the property becomes read-only.

```csharp
class Circle
{
    private double radius;

    public double Radius
    {
        get
        {
            return radius;
        }
    }
}
```

In this example, the `Radius` property has only a `get` accessor, making it read-only. Trying to set its value will result in a compile-time error.

The `set` accessor is used to set the property `value`. It provides write-only access to the attribute it is assigned to. If only a `set` accessor is specified, the property becomes write-only.

```csharp
class Circle
{
    private double radius;

    public double Radius
    {
        set
        {
            if (value > 0)
                radius = value;
            else
                Console.WriteLine("Radius cannot be negative or zero");
        }
    }
}
```

In this example, the `Radius` property has only a `set` accessor. Its value can be set but not directly retrieved. The `value` keyword in C# is a special keyword that is used in the `set` accessor of a property or indexer. It represents the new value the code attempts to assign to the property.

Most commonly, you'll see both `get` and `set` accessors used together. This allows for both reading and writing the property value.

```csharp
class Circle
{
    private double radius;

    public double Radius
    {
        get
        {
            return radius;
        }
        set
        {
            if (value > 0)
                radius = value;
            else
                Console.WriteLine("Radius cannot be negative or zero");
        }
    }
}
```

### Automatic Properties

In C#, an automatic property, also known as auto-implemented property, allows you to define a class property in a concise way without explicitly declaring a backing field. A backing field is a private variable used to store a property’s data.

For example, consider a `full property` with a declared backing field:

```csharp
class Circle
{
    private double radius;

    public double Radius
    {
        get
        {
            return radius;
        }
        set
        {
            radius = value;
        }
    }
}
```

Whereas an automatic property will automatically declare the backing field:

```csharp
class Circle
{
    public double Radius { get; set; }
}
```


In this example, `Radius` is an automatic property. The `{ get; set; }` syntax tells C# to generate a hidden backing field behind the scenes automatically. This field stores the actual data, and the `get` and `set` accessors are used to read from and write to this field.

Functionally both properties are identical.

```csharp
Circle c = new Circle();
c.Radius = 12345.54321;

Console.WriteLine(c.Radius);  // Outputs: 12345.54321
```

Automatic properties provide a shorter and more readable way to create properties, helping keep your code clean and efficient.

### Structs

A `struct`, short for structure, is a value type in C#. This means when a `struct` is created, the variable to which the struct is assigned holds the struct's actual data. This contrasts with reference types, where the variable references the object's data, not the actual data itself.

Structs are useful for small data structures that have value semantics. They can contain fields, methods, and constructors just like classes, but there are some differences:

- Structs do not support inheritance, while classes do. However, both structs and classes can implement interfaces.
- Structs are instantiated without using the `new` keyword, and their constructors are called automatically.
- A struct cannot be `null`, as it's a value type. A class can be `null` because it's a reference type.

```csharp
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

In this example, Point is a struct that represents a point in two-dimensional space. it includes properties (`X` and `Y`) and a constructor that initializes those properties.

## Encapsulation

Encapsulation is one of the four fundamental principles of Object-Oriented Programming (OOP). It is often described as the bundling of data and the methods that operate on that data into a single unit known as a class. It serves as a protective shield that prevents the data from being accessed directly by outside code, hence enforcing data integrity and ensuring security.

In C#, data encapsulation is achieved through access modifiers, which control the visibility and accessibility of classes, methods, and other members. The key access modifiers are `public`, `private`, `protected`, and `internal`.

- A `public` member is accessible from any code in the program.
- A `private` member is only accessible within its own class. This is the most restrictive level of access.
- A `protected` member is accessible within its own class and by derived class instances.
- An `internal` member is accessible only within files in the same assembly.

The convention in C# is to make data members `private` to hide them from other classes (this is known as data hiding). Then, `public` methods known as getters and setters (or, more commonly, properties) are provided to get and set the values of the private fields. These methods serve as the interface to the outside world and protect the data from incorrect or inappropriate manipulation.

```csharp
public class Employee
{
    // Private member data (fields)
    private string name;
    private int age;

    // Public getter and setter methods (properties)
    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age
    {
        get { return age; }
        set 
        { 
            if(value > 0)
                age = value; 
            else 
                Console.WriteLine("Invalid age value");
        }
    }
}
```

In this example above, the `Employee` class encapsulates the `name` and `age` fields. These fields are `private`, so they cannot be access directly from outside the `Employee` class. Instead, access is provided through the public properties `Name` and `Age`, which serve as the interface to the `Employee` class. Notice that the `Age` setter includes validation logic to ensure an invalid age cannot be set. This is an excellent example of encapsulation protecting the data in an object. The data (in thsi case, the `age`) is safeguarded and encapsulated within the `Employee` class.

## Inheritance

Inheritance is a fundamental principle of Object-Oriented Programming (OOP) that allows for the creation of hierarchical classifications of objects. It offers a mechanism where a new class can inherit members (fields, methods, etc.) of an existing class, thereby promoting code reusability and logical classification.

There are two types of inheritance: single inheritance and multilevel inheritance.

### Single Inheritance

In single inheritance, a class (aka a derived or child class) inherits from a single-parent class (also known as a base or superclass). This allows the derived class to reuse (or inherit) the fields and methods of the base class, as well as to introduce new ones.

Consider an example where we have a base class, `Vehicle`, and a derived class, `Car`.

Code: csharp

```csharp
public class Vehicle {
    public string color;
    
    public void Start() {
        Console.WriteLine("Vehicle started");
    }
}

public class Car : Vehicle {
    public string model;
    
    public void Drive() {
        Console.WriteLine("Driving car");
    }
}
```

`Car` is a derived class that inherits from the `Vehicle` base class. It inherits the `color` field and the `Start()` method from `Vehicle` and also defines an additional field `model` and a method `Drive()`.

### Multilevel Inheritance

Multilevel inheritance is a scenario where a derived class inherits from another. This creates a "chain" of inheritance where a class can inherit members from multiple levels up its inheritance hierarchy.

Let's extend the previous example to include a `SportsCar` class inherited from `Car`.

Code: csharp

```csharp
public class SportsCar : Car {
    public int topSpeed;
    
    public void TurboBoost() {
        Console.WriteLine("Turbo boost activated");
    }
}
```

In this case, `SportsCar` is a derived class that inherits from the `Car` class, which in turn inherits from the `Vehicle` class. This means that `SportsCar` has access to the `color` field and `Start()` method from `Vehicle`, the `model` field and `Drive()` method from `Car`, and also defines its own field `topSpeed` and method `TurboBoost()`.

Remember that C# doesn't support multiple inheritance, meaning a class cannot directly inherit from more than one class at the same level. However, as we've seen here, it supports multiple levels of inheritance and allows a class to implement multiple interfaces.

### base

In C#, the `base` keyword is used to access base class members from within a derived class. This can include methods, properties, and fields of the base class. Furthermore, the `base` keyword is most commonly employed within the derived class's constructor to call the base class’s constructor.

To delve deeper, let's examine the use of the `base` keyword in a few examples. Consider a base-class `Vehicle` and a derived-class `Car`.

```csharp
public class Vehicle
{
    public string Color { get; }

    public Vehicle(string color)
    {
        this.Color = color;
    }

    public void DisplayColor()
    {
        Console.WriteLine($"Color: {this.Color}");
    }
}

public class Car : Vehicle
{
    public string Brand { get; }

    public Car(string color, string brand) : base(color)
    {
        this.Brand = brand;
    }

    public void DisplayCarInformation()
    {
        base.DisplayColor();
        Console.WriteLine($"Brand: {this.Brand}");
    }
}
```


