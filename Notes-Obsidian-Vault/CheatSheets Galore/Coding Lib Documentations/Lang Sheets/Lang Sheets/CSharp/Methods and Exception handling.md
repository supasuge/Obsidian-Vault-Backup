### Creating a method

In C#, a method declaration specifies the method’s name, return type, and parameters within the class definition. Here's an example of a method declaration for a simple method that multiplies two numbers:

```csharp
public int Multiply(int a, int b);
```

The method is declared `public`, which means it can be accessed from other classes. `int` signifies the return type, indicating that the method will return an integer value. `Multiply` is the method's name, and within the parentheses `(int a, int b)`, we define the parameters the method will take.

The definition of a method involves providing the body of the method or what the method does. The code block inside the curly brackets `{}` forms the method’s body. Let's define our `Multiply` method:

```csharp
public int Multiply(int a, int b) 
{
    return a * b;
}
```

The `return` statement specifies the output of the method . In this case, it returns the product of `a` and `b`.

In C#, the terms "declaration" and "definition" of a method aren't typically differentiated, as they are in some languages such as C or C++. This is because C# does not permit separate declaration and definition of methods - when you declare a method, you must also provide its implementation, thus effectively defining it.

### Method Scope

Scope pertains to the visibility or accessibility of variables within the program. In C#, variables declared inside a method, known as local variables, are not accessible outside that method. For instance:

```csharp
public int Multiply(int a, int b) 
{
    int result = a * b;
    return result;
}

public void DisplayResult() 
{
    Console.WriteLine(result); // This will lead to an error
}
```

In the `DisplayResult()` method, accessing the `result` variable local to the Multiply method would result in a compile-time error. This is because the `result` is out of scope in `DisplayResult()`.

However, if a variable is declared in a class but outside any method, it is a global variable and can be accessed from any method within that class.

### Static vs Non-Static Methods

The `static` keyword is used to declare members that `belong to the type` rather than `any instance of the type`. This means that static members are shared across all instances of a type and can be accessed directly using the type name, without creating an instance of the class.

Methods can be declared as `static` or `non-static`, also known as `instance` methods.

Details about `classes` and `instances` will be explored in the `Object-oriented Programming` section, but for now, just note the following.

A `static` method belongs to the class itself rather than any specific class instance. It is declared with the keyword `static`.

```csharp
public class MyClass
{
    // Static method
    public static void MyStaticMethod()
    {
        Console.WriteLine("This is a static method.");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        // Call the static method
        MyClass.MyStaticMethod();  // Outputs: This is a static method.
    }
}
```

To call a static method, you don't need to create an instance of the class. Instead, you use the class name itself.

```csharp
MyClass.MyStaticMethod();
```

Since static methods are tied to the class itself, they can only access the class's other static members (methods, properties, etc.). They cannot access non-static members as those belong to specific instances of the class.

A `non-static` (or `instance`) method belongs to a particular class instance. It is declared without using the `static` keyword.

```csharp
public class MyClass
{
    // Non-static (instance) method
    public void MyInstanceMethod()
    {
        Console.WriteLine("This is an instance method.");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        // Create an instance of MyClass
        MyClass myObject = new MyClass();

        // Call the instance method
        myObject.MyInstanceMethod();  // Outputs: This is an instance method.
    }
}
```

To call a non-static method, you must create an instance of the class.

```csharp
MyClass myObject = new MyClass();
myObject.MyInstanceMethod();
```

Instance methods can access the class's `static` and `non-static` members since they belong to a specific class instance.

Static members can also include `fields`, `properties`, `events`, `operators`, and `constructors`.

## Exceptions

Exception handling in C# is a robust mechanism used to handle runtime errors so that the normal flow of the application can be maintained. C# provides a structured solution to error handling through try-and-catch blocks. Using these blocks, we can isolate code that may throw an exception and enable the program to respond rather than letting the program crash.

### try catch finally

A `try` block is used to encapsulate a region of code. If any statement within the try block throws an exception, that exception will be handled by the associated catch block.

```csharp
try
{
    // Code that could potentially throw an exception.
}
```

The `catch` block is used to catch and handle an exception. It follows a try block or another catch block. Each try block can have multiple catch blocks associated with it, each designed to handle specific or multiple exceptions. A `catch` block without a specified type will catch all exceptions.

```csharp
catch (Exception ex)
{
    // Handle the exception
}
```

A `finally` block lets you execute code after a try block has been completed, regardless of whether an exception has been thrown. It is optional and cleans up resources inside the try block (like database connections, files, or network resources).

```csharp
finally
{
    // Code to be executed after the try block has completed,
    // regardless of whether an exception was thrown.
}
```

Here's an example of try, catch, and finally all used together:

```csharp
try
{
    // Code that could potentially throw an exception.
    int divisor = 0;
    int result = 10 / divisor;
}
catch (DivideByZeroException ex)
{
    // Handle the DivideByZeroException.
    Console.WriteLine("Cannot divide by zero");
}
finally
{
    // Code to be executed after the try block has completed,
    // regardless of whether an exception was thrown.
    Console.WriteLine("This code is always executed.");
}
```

When dealing with `catch blocks`, remember that they can handle multiple exception exceptions. The order in which you specify different catch blocks matters; they're examined top to bottom, so the first one that matches the exception type will be executed. If you have a catch block that handles all exceptions at the top, it will catch all exceptions, and none of the catch blocks below it will execute. This is why the catch block for the most general exception type, `Exception`, is usually last.


```csharp
try
{
    // Code that could throw an exception
    int[] arr = new int[5];
    arr[10] = 30; // This line throws an IndexOutOfRangeException.
}
catch (IndexOutOfRangeException ex)
{
    // Handle specific exception first
    Console.WriteLine("An IndexOutOfRangeException has been caught: " + ex.Message);
}
catch (Exception ex)
{
    // General exception catch block
    Console.WriteLine("An exception has been caught: " + ex.Message);
}
```
