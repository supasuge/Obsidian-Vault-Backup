### Arithmetic Operators

In C#, these operators enable mathematical operations. They include `+` (addition), `-` (subtraction), `*` (multiplication), `/` (division), and `%` (modulus, which gives the remainder of a division).

```csharp
int a = 10;
int b = 5;

int sum = a + b; // Result: 15
int difference = a - b; // Result: 5
int product = a * b; // Result: 50
int quotient = a / b; // Result: 2
int remainder = a % b; // Result: 0
```


### Relational Operators

These compare two values and determine the relationship between them. C# includes `==` (equal to), `!=` (not equal to), `>` (greater than), `<` (less than), `>=` (greater than or equal to), and `<=` (less than or equal to).

```csharp
int a = 10;
int b = 20;

bool isEqual = (a == b); // Result: false
bool isNotEqual = (a != b); // Result: true
bool isGreaterThan = (a > b); // Result: false
bool isLessThan = (a < b); // Result: true
bool isGreaterThanOrEqualTo = (a >= b); // Result: false
bool isLessThanOrEqualTo = (a <= b); // Result: true
```

### Logical Operators

These operators use Boolean (`true` or `false`) values to create logical expressions. C# provides `&&` (logical AND), `||` (logical OR), and `!` (logical NOT).

```csharp
bool isTrue = true;
bool isFalse = false;

bool andResult = isTrue && isFalse; // Result: false
bool orResult = isTrue || isFalse; // Result: true
bool notResult = !isTrue; // Result: false
```

### Bitwise Operators

Bitwise operators are useful when working with binary data, which is common in fields such as cryptography or image processing where you need to manipulate individual bits within a block of data. Bitwise Operators offer a level of precision that is impossible with conventional arithmetic or logical operators.

They include `&` (bitwise AND), `|` (bitwise OR), `^` (bitwise XOR), `~` (bitwise NOT), `<<` (left shift), and `>>` (right shift).

| **Operator**       | **Description**                                                                                                                                                                                                    |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `&` (bitwise AND)  | Compares each bit of the first operand to the corresponding bit of the second operand. If both bits are 1, the corresponding result bit is set to 1. Otherwise, the result bit is set to 0.                        |
| \| (bitwise OR)    | Compares each bit of the first operand to the corresponding bit of the second operand. If either bit is 1, the corresponding result bit is set to 1. If both bits are 0, the result bit is set to 0.               |
| `^` (bitwise XOR)  | Compares each bit of the first operand to the corresponding bit of the second operand. If the bits are not identical, the corresponding result bit is set to 1. If they are identical, the result bit is set to 0. |
| `~` (bitwise NOT)  | Flips the bits of its operand. If a bit is 0, it becomes 1; if a bit is 1, it becomes 0.                                                                                                                           |
| `<<` (left shift)  | Moves the bits of the operand to the left by the number of places specified by the second operand. Bits that are shifted off the left are discarded, and zeros are shifted in on the right.                        |
| `>>` (right shift) | Moves the bits of the operand to the right by the number of places specified by the second operand. Bits that are shifted off the right are discarded, and zeros are shifted in on the left.                       |
```csharp
int a = 240; // Binary: 1111 0000
int b = 15; // Binary: 0000 1111

int andResult = a & b; // Result would be 0, Binary: 0000 0000
int orResult = a | b; // Result would be 255, Binary: 1111 1111
int xorResult = a ^ b; // Result would be 255, Binary: 1111 1111
int notResult = ~a; // Result would be -241, Binary: 0000 1111
int leftShift = a << 2; // Result would be 960, Binary: 11 1100 0000
int rightShift = a >> 2; // Result would be 60, Binary: 0011 1100
```


### Assignment Operators

C# includes `=` (simple assignment), and compound assignment operators like `+=`, `-=`, `*=`, `/=`, and `%=`, which combine an arithmetic operation with assignment.

Code: csharp

```csharp
int a = 10; // simple assignment

a += 5; // equivalent to a = a + 5, so a now is 15
a -= 3; // equivalent to a = a - 3, so a now is 12
a *= 2; // equivalent to a = a * 2, so a now is 24
a /= 4; // equivalent to a = a / 4, so a now is 6
a %= 5; // equivalent to a = a % 5, so a now is 1
```

### Unary Operators

Unary operators are those that operate on a single operand. They include `++` (increment), `--` (decrement), `+` (unary plus, denotes positive values), and `-` (unary minus, denotes negative values). Unary operators also include the `!` logical negation operator mentioned earlier.

Code: csharp

```csharp
int a = 5;
a++; // Now a is 6
a--; // Now a is 5 again
int b = +a; // b is 5
int c = -a; // c is -5
```

### Ternary Operator

Ternary operators provide a shorthand way of writing an `if-else` condition. The syntax is `condition ? true expression : false expression`. `if-else` and other control flow statements are explained in `Control Statements and Loops`.

Consider the following example:


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



### Null Conditional Operators

These operators, introduced in C# 6.0, are used to access members and elements of an object safely. They return `null` if the object is `null` instead of throwing a `NullReferenceException`.

There are two null conditional operators:

1. `Null Conditional Member Access (?.)`: facilitates the safe access of an object's members, including properties or methods. As depicted in your second example, the Length property of the `authorName` string is being accessed. If `authorName` is `null`, a `NullReferenceException` will not be thrown. Instead, it will simply yield `null`.
2. `Null Conditional Element Access (?[])`: utilised to secure access to array or collection elements. When the array or collection is `null`, using `?[]` to access an element won't result in an exception. Rather, it will return `null`. For instance, in your first example, as `authors` is `null`, `authors?[0]` consequently returns `null`.

```csharp
string[] authors = null;
var author1 = authors?[0]; // This will not throw an exception even though authors is null. author1 will simply be null.

string authorName = null;
int? length = authorName?.Length; // Again, this will not throw an exception. length will be null.
```

### Null-coalescing Operator

In C#, the null-coalescing operator is a binary operator that simplifies checking for null values. It is represented by a double question mark `??`.

The null-coalescing operator returns the value of its left-hand operand if it is not null; otherwise, it evaluates the right-hand operand and returns its result.

```csharp
int? x = null;
int y = x ?? -1;
```

Since `x` is null, the value `-1` is assigned to `y` in this example.

This operator is particularly useful for providing default values when a variable is null. Without the null-coalescing operator, you would need to use an `if` statement to check for null values:

### Null-coalescing Operator

In C#, the null-coalescing operator is a binary operator that simplifies checking for null values. It is represented by a double question mark `??`.

The null-coalescing operator returns the value of its left-hand operand if it is not null; otherwise, it evaluates the right-hand operand and returns its result.

```csharp
int? x = null;
int y = x ?? -1;
```

Since `x` is null, the value `-1` is assigned to `y` in this example.

This operator is particularly useful for providing default values when a variable is null. Without the null-coalescing operator, you would need to use an `if` statement to check for null values:
```csharp
int? x = null;
int y;

if (x != null)
    y = x.Value;
else
    y = -1;
```

As you can see, using the null-coalescing operator `??` makes the code much more concise and easier to read. You can also use the null-coalescing assignment operator `??=`. This operator assigns the value of its right-hand operand to its left-hand operand only if the left-hand operand evaluates to null:

```csharp
int? x = null;
x ??= -1;  // x is now -1 because it was null
```

## Type Conversion

In programming, especially in a statically typed language like C#, it is often necessary to convert data from one type to another. This could be because an operation requires a certain data type, or you need to interact with an API that expects a different one. This process is called type conversion, and there are two types of conversions in C#: implicit and explicit.

### Implicit Type Conversion (or Widening Conversion)

The compiler performs These conversions automatically without the programmer's intervention. They happen when converting a smaller type to a larger type (like `int` to `long` or `float`), or a derived class to its base. These conversions are safe and do not lead to data loss.

```csharp
int numInt = 10;
long numLong = numInt; // Implicit conversion from int to long
```

### Explicit Type Conversion (or Narrowing Conversion)

The programmer performs These conversions manually using predefined functions. Explicit conversions require a cast operator. They happen when converting a larger type to a smaller type (like `long` to `int`) or a base class to its derived. These conversions can lead to data loss or a `System.OverflowException`.

```csharp
double numDouble = 10.5;
int numInt = (int)numDouble; // Explicit conversion from double to int. numInt will be 10, the fractional part is lost
```

C# also supports built-in methods for converting types, particularly from/to string types, like `ToString()`, `ToInt32()`, `ToDouble()`, etc., which are part of the `Convert` class.


```csharp
int numInt = 10;
string str = numInt.ToString(); // Converts numInt to a string
int num = Convert.ToInt32(str); // Converts str back to an integer
```

## Type Checking

C# provides operators to check the type of an object: the `is` operator and the `as` operator.
### is

It checks whether an object is compatible with a given type, and the result of the operation is `bool`.

```csharp
string str = "Hello, World!";
bool result = str is string; // result will be true
```

### as

It is used for casting between compatible reference types or nullable types. If the cast fails, it returns `null` instead of throwing an exception.

```csharp
object obj = new string("Hello, World!");
string str = obj as string; // str will be "Hello, World!" if the cast is successful; otherwise, it will be null
```

