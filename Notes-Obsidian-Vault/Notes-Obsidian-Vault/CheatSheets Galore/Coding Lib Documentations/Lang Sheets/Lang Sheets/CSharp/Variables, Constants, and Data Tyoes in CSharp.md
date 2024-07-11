## Table of Contents

- [Variables](#Variables)
    - [Constants](#Constants)
    - [Enums/Enumerations](#Enums/Enumerations)
- [Data Types](#data\types)
      - [Relational Operators](#Relational\Operators)
      - [Logical operators](#Logical\operators)
      - [Bitwise Operations](#Bitwise\Operations)
    - [Unary Operators](#Unary\Operators)
    - [Ternary Operator](#Ternary\Operator)


#### Variables
A Variable in C# is a name given to a **storage area** in memory with the value stored being changeable. Variables are defined by two properties: name and a data type.

 To declare a variable in C#, you first specify the data type, followed by the variable name:

```csharp
int myNumber;
```

You can also assign a value to the variable at the time of declaration:

```csharp
int myNumber = 10;
```

#### Constants
	A Contstant in C# is similar in that it's a name given to a storage area in memeory. However the value of a constant, as the name implies, remains constant throughout the program; once it's been set, it cant be changed.
- Constants are ==Immutable==
- C# will throw an error if you try to modify a constant. Constants can be helpful when you use a value repeatedly throughout your code and know it will not change.
```C#
const int myConstant = 1;
```

#### Enums/Enumerations
- `enums` are **useful for representing a distinct set of named constants**, providing a more human-readable form for a specific set of intergral values.

In this case, `DayOfWeek` is an `enum` that represents the seven days of the week. Each day is an enumerator. By default, the first enumerator has the value 0, and the value of each successive enumerator is increased by 1. You can also specify the underlying integral type and assign specific values to the enumerators, like so:

```csharp
public enum Month : byte
{
    January = 1,
    February = 2,
    // And so on...
}
```

`Month` is an `enum` with an underlying type of `byte` and each month is explicitly assigned a value representing its positin in the year.

`enums` are good for readability and safety, as it limits the possible values you can assign

# Data Types
nteger Types are used to store whole numbers without decimal points. There are several integer types in C#, each of which can store a different range of values:

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


```csharp
float aFloat = 3.1415927f; // Range: ±1.5 x 10^-45 to ±3.4 x 10^38, Precision: 7 digits

double aDouble = 3.141592653589793; // Range: ±5.0 x 10^-324 to ±1.7 x 10^308, Precision: 15-16 digits
```

The Decimal Type is used for large or small decimal numbers and for financial and monetary calculations where precision is critical.


```csharp
decimal aDecimal = 3.14159265358979323846m; // Range: ±1.0 x 10^-28 to ±7.9 x 10^28, Precision: 28-29 
```

The Boolean Type is a logical data type with two values, `true` or `false`.

```csharp
bool aBool = true; // or false
```

The Character Type is used to store a single Unicode character.

```csharp
char aChar = 'C';
```

In addition to these types, C# also supports Nullable Types, which can represent the normal range of values for its underlying value type, plus an additional null value. These are explained more on the next page.

```csharp
int? nullableInt = null;
```

```csharp
int a = 10;
int b = 5;

int sum = a + b; // Result: 15
int difference = a - b; // Result: 5
int product = a * b; // Result: 50
int quotient = a / b; // Result: 2
int remainder = a % b; // Result: 0
```

#### Relational Operators
These compare two values and determine the relationship between them. C# includes `==` euqal to, `!=` not equal to, `>` greater than, `<` less than, `>=` greater than or equal to, `<=` less than or equal to 

#### Logical operators
Evaluate to TRUE or FALSE to create logical expressions. 

- `&&`: Logical AND
- `||`: logical OR
- `!`: logicalt NOT

#### Bitwise Operations
Useful when working with binary data, which is common in crypto or image processing.

- `&`(Bitwise **AND**): compares each bit of the first operant to the correspondig bit of the seccond
- `|`(bitwise **OR**)
- `^`(bitwise **XOR**) compares each bit of the first operand to the corresponding bit of the second operand. If the bits are not identical, the corresponding result bit is set to 0.
- `~`(bitwise **NOT**)
- `<<`(left shift) Moves the bits of the operand to the left by the number of places specified by the second operand. 
- `>>`(right shift) Moves the bits of the operand to the right by the number of places specified by the second operand. Bits that are shifted off the right are discarded, and zeros are shifted in on the left.

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

C# includes `=` (simple assignment), and compound assignment operators like `+=`, `-=`, `*=`, `/=`, and `%=`, which combine an arithmetic operation with assignment.

Code: csharp

```csharp
int a = 10; // simple assignment

a += 5; // equivalent to a = a + 5, so a now is 15
a -= 3; // equivalent to a = a - 3, so a now is 12
a *= 2; // equivalent to a = a * 2, so a now is 24
a /= 4; // equivalent to a = a / 4, so a now is 6
a %= 5; // equivalent to a = a % 5, so a now is 1
```

### Unary Operators

Unary operators are those that operate on a single operand. They include `++` (increment), `--` (decrement), `+` (unary plus, denotes positive values), and `-` (unary minus, denotes negative values). Unary operators also include the `!` logical negation operator mentioned earlier.

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

