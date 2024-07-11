## Table of Contents

- [.NET CLI](#.NET\CLI)
  - [Data Type Conversion](#Data\Type\Conversion)
    - [Logical Operators](#Logical\Operators)
        - [Bitwise Operators](#Bitwise\Operators)
    - [Unary Operators](#Unary\Operators)
    - [Ternary Operator](#Ternary\Operator)
    - [Null Conditional Operators](#Null\Conditional\Operators)
    - [Null-coalescing Operator](#Null-coalescing\Operator)
    - [Implicit Type Conversion (or Widening Conversion)](#Implicit\Type\Conversion\(or\Widening\Conversion))
    - [Explicit Type Conversion (or Narrowing Conversion)](#Explicit\Type\Conversion\(or\Narrowing\Conversion))
    - [is](#is)
    - [as](#as)
  - [Resolving Naming Conflicts with Namespaces](#Resolving\Naming\Conflicts\with\Namespaces)

### .NET CLI

We will interact with `dotnet` via a console more than anything, as Visual Studio Code does not have the same level of 'hands-off tooling' that a full IDE, such as Visual Studio, provides. Below is a breakdown of some of the important commands to know:


- `dotnet new`: Creates a new .NET project. You can specify the type of project (`console`, `classlib`, `webapi`, `mvc`, etc.). For example, `dotnet new console` will create a new console application.

- `dotnet build`: Builds a .NET project and all of its dependencies. The `-c` or `--configuration` option can be used to specify the build configuration (`Debug` or `Release`).
 
- `dotnet run`: Builds and runs the .NET project. It is typically used during the development process to run the application for testing or debugging purposes.
 
- `dotnet test`: Runs unit tests in a .NET project using a test framework such as `MSTest`, `NUnit`, or `xUnit`.

- `dotnet publish`: Packs the application and its dependencies into a folder for deployment to a hosting system. The `-r` or `--runtime` option can be used to specify the target runtime.
 
- `dotnet add package`: Adds a NuGet package reference to the project file. You specify the package by name. For example, `dotnet add package Newtonsoft.Json`.
 
- `dotnet remove package`: Removes a NuGet package reference from the project file. Similar to the `add package` command, you specify the package to remove by name.
 
- `dotnet restore`: Restores the dependencies and tools of a project. This command is implicitly run when you run `dotnet new`, `dotnet build`, `dotnet run`, `dotnet test`, `dotnet publish`, and `dotnet pack`.

- `dotnet clean`: Cleans the output of a project. This command is typically used before you build the project again, as it deletes all the previously compiled files, ensuring that you start from a clean state.

- `dotnet --info`: Displays detailed information about the installed .NET environment, including installed versions and all runtime environments.

This output utilises recent C# features that reduce the amount of code required for a straightforward program. This approach is suitable for small-scale projects operating entirely without a specific structure. However, for our purposes, this project style won't be suitable. Instead, it's recommended to use the provided template for projects.



Code: csharp

```csharp
class Program
{
    public static void Main()
    {
        // ...
    }
}
```


Code: csharp

```csharp
byte aByte = 255; // Range: 0 to 255
sbyte aSbyte = -128; // Range: -128 to 127
short aShort = -32768; // Range: -32,768 to 32,767
ushort aUshort = 65535; // Range: 0 to 65,535
int anInt = -2147483648; // Range: -2,147,483,648 to 2,147,483,647
uint aUint = 4294967295; // Range: 0 to 4,294,967,295
long aLong = -9223372036854775808; // Range: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
ulong aUlong = 18446744073709551615; // Range: 0 to 18,446,744,073,709,551,615
```

Floating Point Types are used to store numbers with decimal points. They include the `float` and `double` types:

Code: csharp

```csharp
float aFloat = 3.1415927f; // Range: ±1.5 x 10^-45 to ±3.4 x 10^38, Precision: 7 digits
double aDouble = 3.141592653589793; // Range: ±5.0 x 10^-324 to ±1.7 x 10^308, Precision: 15-16 digits
```

The Decimal Type is used for large or small decimal numbers and for financial and monetary calculations where precision is critical.

Code: csharp

```csharp
decimal aDecimal = 3.14159265358979323846m; // Range: ±1.0 x 10^-28 to ±7.9 x 10^28, Pr
```


## Data Type Conversion
### Logical Operators

These operators use Boolean (`true` or `false`) values to create logical expressions. C# provides `&&` (logical AND), `||` (logical OR), and `!` (logical NOT).

Code: csharp

```csharp
bool isTrue = true;
bool isFalse = false;

bool andResult = isTrue && isFalse; // Result: false
bool orResult = isTrue || isFalse; // Result: true
bool notResult = !isTrue; // Result: false
```

##### Bitwise Operators

```csharp
& - bitwise AND //compares each bit of the first operand to the corresponding bit of the second operand.

```
### Unary Operators

Unary operators are those that operate on a single operand. They include `++` (increment), `--` (decrement), `+` (unary plus, denotes positive values), and `-` (unary minus, denotes negative values). Unary operators also include the `!` logical negation operator mentioned earlier.

Code: csharp

```csharp
int a = 5;
a++; // Now a is 6
a--; // Now a is 5 again
int b = +a; // b is 5
int c = -a; // c is -5
```

### Ternary Operator

Ternary operators provide a shorthand way of writing an `if-else` condition. The syntax is `condition ? true expression : false expression`. `if-else` and other control flow statements are explained in `Control Statements and Loops`.

Consider the following example:

Code: csharp

```csharp
int a = 10;
int b = 20;

// expanded if-else
if (a > b)
{
    result = "a is greater";
}
else
{
    result = "b is greater";
}

// contracted ternary
string result = a > b ? "a is greater" : "b is greater";
```

In this case, because `a` is not greater than `b`, the result would be `"b is greater"`.

### Null Conditional Operators

These operators, introduced in C# 6.0, are used to access members and elements of an object safely. They return `null` if the object is `null` instead of throwing a `NullReferenceException`.

There are two null conditional operators:

1. `Null Conditional Member Access (?.)`: facilitates the safe access of an object's members, including properties or methods. As depicted in your second example, the Length property of the `authorName` string is being accessed. If `authorName` is `null`, a `NullReferenceException` will not be thrown. Instead, it will simply yield `null`.
2. `Null Conditional Element Access (?[])`: utilised to secure access to array or collection elements. When the array or collection is `null`, using `?[]` to access an element won't result in an exception. Rather, it will return `null`. For instance, in your first example, as `authors` is `null`, `authors?[0]` consequently returns `null`.

Code: csharp

```csharp
string[] authors = null;
var author1 = authors?[0]; // This will not throw an exception even though authors is null. author1 will simply be null.

string authorName = null;
int? length = authorName?.Length; // Again, this will not throw an exception. length will be null.
```

In both cases, if the objects (`authors` and `authorName`) were not null, the respective indices and properties would be accessed normally. However, since they are null, the operators prevent a `NullReferenceException` and return `null`. These operators have significantly enhanced how developers handle null references in C#, providing a more compact and streamlined way to ensure safe access to members of potentially null objects.

### Null-coalescing Operator

In C#, the null-coalescing operator is a binary operator that simplifies checking for null values. It is represented by a double question mark `??`.

The null-coalescing operator returns the value of its left-hand operand if it is not null; otherwise, it evaluates the right-hand operand and returns its result.

Code: csharp

```csharp
int? x = null;
int y = x ?? -1;
```
### Implicit Type Conversion (or Widening Conversion)

The compiler performs These conversions automatically without the programmer's intervention. They happen when converting a smaller type to a larger type (like `int` to `long` or `float`), or a derived class to its base. These conversions are safe and do not lead to data loss.

Code: csharp

```csharp
int numInt = 10;
long numLong = numInt; // Implicit conversion from int to long
```

### Explicit Type Conversion (or Narrowing Conversion)

The programmer performs These conversions manually using predefined functions. Explicit conversions require a cast operator. They happen when converting a larger type to a smaller type (like `long` to `int`) or a base class to its derived. These conversions can lead to data loss or a `System.OverflowException`.

Code: csharp

```csharp
double numDouble = 10.5;
int numInt = (int)numDouble; // Explicit conversion from double to int. numInt will be 10, the fractional part is lost
```

C# also supports built-in methods for converting types, particularly from/to string types, like `ToString()`, `ToInt32()`, `ToDouble()`, etc., which are part of the `Convert` class.

Code: csharp

```csharp
int numInt = 10;
string str = numInt.ToString(); // Converts numInt to a string
int num = Convert.ToInt32(str); // Converts str back to an integer
```

### is

It checks whether an object is compatible with a given type, and the result of the operation is `bool`.

Code: csharp

```csharp
string str = "Hello, World!";
bool result = str is string; // result will be true
```

### as

It is used for casting between compatible reference types or nullable types. If the cast fails, it returns `null` instead of throwing an exception.

Code: csharp

```csharp
object obj = new string("Hello, World!");
string str = obj as string; // str will be "Hello, World!" if the cast is successful; otherwise, it will be null
```


## Resolving Naming Conflicts with Namespaces

A naming conflict occurs when multiple namespaces define code elements with the same name. C# provides ways to resolve such conflicts to ensure unambiguous access to code elements.

To resolve naming conflicts, you can use one of the following approaches:

1. `Fully Qualify the Code Element`: Use the fully qualified name of the code element, including the namespace, to ensure explicit identification.

Code: csharp

```csharp
namespace MyNamespace
{
    class MyClass { }

    class Program
    {
        static void Main()
        {
            // Using fully qualified name to avoid naming conflict
            MyNamespace.MyClass myObject = new MyNamespace.MyClass();
        }
    }
}
```

2. `Alias the Namespace`: When importing, use an alias to differentiate between conflicting namespaces. This provides a shorthand way to refer to the code elements without ambiguity.

Code: csharp

```csharp
using MyAlias = MyNamespace;
using AnotherAlias = AnotherNamespace;

namespace MyNamespace
{
    class MyClass { }
}

namespace AnotherNamespace
{
    class MyClass { }

    class Program
    {
        static void Main()
        {
            // Using aliases to differentiate between conflicting namespaces
            MyAlias.MyClass myObject1 = new MyAlias.MyClass();
            AnotherAlias.MyClass myObject2 = new AnotherAlias.MyClass();
        }
    }
}
```





