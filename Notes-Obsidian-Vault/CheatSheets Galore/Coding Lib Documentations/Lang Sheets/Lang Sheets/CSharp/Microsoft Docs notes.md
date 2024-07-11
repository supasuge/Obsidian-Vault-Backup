## Table of Contents

- [C# Type System Overview](#c#\type\system\overview)
  - [Value Types](#Value\Types)
  - [Reference Types](#Reference\Types)
  - [Generics](#Generics)
  - [Arrays](#Arrays)
  - [Nullable Types](#Nullable\Types)
  - [Unified Type System](#Unified\Type\System)
  - [Variables](#Variables)
    - [Variable Types](#Variable\Types)
  - [Examples](#Examples)
    - [Boxing and Unboxing](#Boxing\and\Unboxing)
    - [Nullable Type](#Nullable\Type)
    - [Array Declaration](#Array\Declaration)
    - [Enum Declaration](#Enum\Declaration)
    - [Struct Declaration](#Struct\Declaration)
    - [Class Declaration](#Class\Declaration)
    - [Interface Implementation](#Interface\Implementation)
    - [Delegate Usage](#Delegate\Usage)

`type` Defines the structure and behavior of any data in C#. The declaration of a type may include it's *members*, *base type*, *interfaces it implements*, and *operations permitted for that type*.

	Value Type
- directly cotnain their data.

	`Reference Types`
- store references to their data, the latter being known as object. 
- With reference types, its possible for two variables to reference the same object and possible for operations on one variable to affect the object referenced by the othe variable. With value types, the variabels each have their own copy of the data, and it isn't possible for operations on one to affect the other 

# C# Type System Overview

## Value Types
- **Simple Types**
  - Signed Integral: `sbyte`, `short`, `int`, `long`
  - Unsigned Integral: `byte`, `ushort`, `uint`, `ulong`
  - Unicode Characters: `char` (UTF-16 code unit)
  - IEEE Binary Floating-Point: `float`, `double`
  - High-Precision Decimal Floating-Point: `decimal`
  - Boolean: `bool` (true or false)

- **Enum Types**
  - Syntax: `enum E { ... }`
  - Distinct type with named constants
  - Underlying type: one of the eight integral types

- **Struct Types**
  - Syntax: `struct S { ... }`
  - Value type, no heap allocation
  - Implicit inheritance from `object`

- **Nullable Value Types**
  - Extension of value types with `null` value
  - Syntax: `T?` for non-nullable type `T`

- **Tuple Value Types**
  - User-defined: `(T1, T2, ...)`

## Reference Types
- **Class Types**
  - Base class: `object`
  - Unicode Strings: `string` (UTF-16 sequence)
  - User-defined: `class C { ... }`
  - Supports single inheritance and polymorphism

- **Interface Types**
  - Syntax: `interface I { ... }`
  - Contract with public members
  - Multiple inheritance

- **Array Types**
  - Single-dimensional: `int[]`
  - Multi-dimensional: `int[,]`
  - Jagged: `int[][]`

- **Delegate Types**
  - Syntax: `delegate int D(...)`
  - References to methods
  - Object-oriented, type-safe

## Generics
- Supported by class, struct, interface, delegate types
- Parameterized with other types

## Arrays
- Constructed with square brackets
- Example: `int[]`, `int[,]`, `int[][]`

## Nullable Types
- Syntax: `T?` for non-nullable type `T`
- Can hold `null` or a value of type `T`

## Unified Type System
- Every type derives from `object`
- Boxing and unboxing for value types

## Variables
- Represent storage locations
- Types determine allowable values

### Variable Types
- Non-nullable Value Type: Exact type value
- Nullable Value Type: `null` or exact type value
- Object: `null`, any reference type, or boxed value type
- Class Type: `null`, instance of the class, or derived class instance
- Interface Type: `null`, instance of implementing class, or boxed value type
- Array Type: `null`, instance of the array, or compatible array type
- Delegate Type: `null` or compatible delegate instance

## Examples

### Boxing and Unboxing
```csharp
int i = 123;
object o = i;    // Boxing
int j = (int)o;  // Unboxing
```

### Nullable Type
```csharp
int? nullableInt = null;
nullableInt = 5;
```

### Array Declaration
```csharp
int[] singleDimension = new int[5];
int[,] multiDimension = new int[3,2];
int[][] jaggedArray = new int[3][];
```

### Enum Declaration
```csharp
enum Days { Sun, Mon, Tue, Wed, Thu, Fri, Sat };
```

### Struct Declaration
```csharp
struct Point
{
    public int X, Y;
}
```

### Class Declaration
```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

### Interface Implementation
```csharp
interface IAnimal
{
    void Speak();
}

class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Bark");
    }
}
```

### Delegate Usage
```csharp
delegate int Operation(int x, int y);
Operation add = (x, y) => x + y;
int result = add(5, 3);
```

---

These notes provide a concise yet comprehensive overview of the C# type system, including examples for better understanding.
