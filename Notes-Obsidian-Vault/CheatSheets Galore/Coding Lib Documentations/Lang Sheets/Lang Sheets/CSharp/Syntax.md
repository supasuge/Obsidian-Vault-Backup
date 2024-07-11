## Main Method
In C#, any application begins it's execution from a special method known as `Main`. This is the standard entry point of any C# application. When the app starts, the `Main` method is the invoked method. 

The `Main` method can be located inside any class in the C# project; however, placing it within a class named Program is standard practice.

```csharp
class Program
{
    public static void Main() { }
    public static int Main() { }
    public static int Main(string[] args) { }
    public static async Task Main() { }
    public static async Task<int> Main() { }
    public static async Task Main(string[] args) { }
    public static async Task<int> Main(string[] args) { }
    public static void Main(string[] args)
    {
        // Program execution starts here
    }
}
```

## Case Sensitivity

In C#, case sensitivity is a fundamental aspect of the language syntax. This means the language differentiates between uppercase and lowercase characters, treating them as distinct. As a result, identifiers named with different cases are considered separate entities by the C# compiler.

For instance, consider the following variable declarations:

```csharp
int myVariable = 10;
int MyVariable = 20;
```

Even though `myVariable` and `MyVariable` may appear similar, they are treated as two completely different variables due to the difference in case. The first has a lowercase ‘m’, while the second begins with an uppercase 'M'. If you were to print both variables, they would output different values.


## Identifiers

In C#, an identifier is a name assigned to a variable, method, function, or any user-defined item. It is essentially a way to refer to a code component for use in operations, functions, and algorithms.

There are a few rules and conventions to bear in mind when creating identifiers in C#:

1. An identifier must start with a letter or an underscore character (`_`). For example, `variable`, `_variable` are valid identifiers but `1variable` is not.
2. After the first character, it can have a series of alphanumeric characters (letters and digits) and an underscore (`_`).
3. Reserved words in C# cannot be used as identifiers. For example, you cannot use `int` or `while` as an identifier because they have predefined uses in C#.
4. There's no strict limit on the length of an identifier in C#, but it's advisable to keep them reasonably short to maintain readability.

```csharp
int score;
string playerName;
float _temperature;
```

## Keywords

C# includes a set of reserved words known as keywords, which have predefined meanings in the language's syntax. Examples of keywords include `public`, `class`, `void`, `if`, `else`, `while`, etc. Keywords cannot be used as identifiers.

```csharp
class MyClass { ... }
if (condition) { ... }
```


## The ;

The semicolon (`;`) is known as a `statement terminator` in C#. It's employed to signify the end of a specific statement or command in the code. Using the semicolon as a statement terminator is common in many programming languages, including `C++`, `Java`, and `JavaScript`.

```csharp
int x = 10;  // The semicolon terminates the variable declaration statement.

Console.WriteLine(x);  // The semicolon terminates the method call statement.

x++;  // The semicolon terminates the increment operation.
```

By correctly terminating each statement, the compiler can discern where one command ends and the next begins. Neglecting to include a semicolon at the end of a statement often results in compile-time errors.

## Statements & Expressions

A statement in C# represents a complete command to perform a specific action.

An expression, on the other hand, is a combination of operands (variables, literals, method calls, etc.) and operators (`+, -, *, /, %, etc.`) that can be evaluated to a single value.

```csharp
int sum = 10 + 20; // This line is a statement
10 + 20; // This is an expression
```

## Blocks of Code

Blocks in C# are sections of code enclosed in braces (`{ }`). They are typically used to group multiple statements together to form a single executable unit, such as the body of a method, loop, or conditional statement.

```csharp
if (number > 5)
{
    Console.WriteLine("The number is greater than 5");
    number--;
}
```


## Comments

Comments in C# are used to add explanatory notes in the source code. Single-line comments begin with two forward slashes (`//`), and multi-line comments are enclosed between `/* and */`. The C# compiler ignores comments.
```csharp
// This is a single-line comment

/* This is a 
   multi-line 
   comment */
```

## Read Compiler Errors

The C# compiler provides incredible verbosity with its error messages. Don't just discard the output as "Oh no, it's an error"; it will contain valuable information, generally indicating the root problem.

A typical C# compiler error message has three parts:

```bash
/tmp/X/Program.cs(5,21): error CS0029: Cannot implicitly convert type 'string' to 'int'
```

1. `Error Location`: This tells you where in your code the error occurred, typically indicated by a line number and a character position.
2. `Error Code`: This alphanumeric code uniquely identifies the error type. It is helpful when looking up additional information about the error.
3. `Error Description`: This part of the message describes the error in plain English. It provides insights into what rule was violated in the code.


