## Control Statements

In C#, control flow statements allow your code to execute different branches based on conditions being met or not. `If`, `else-if`, and `switch` are three types of control statements that are commonly used.

### if

The `if` statement represents the most fundamental type of control flow statement. It evaluates a condition, and if that condition returns `true`, it executes a corresponding code block.

```csharp
int number = 10;
if (number > 0) {
    Console.WriteLine("The number is positive.");
}
```

In the above example, the `if` statement checks whether the number is greater than 0. As this condition is `true`, it prints "The number is positive."

### else

The `else` statement is used alongside the `if` statement. It allows you to specify a block of code to be executed if the condition in the `if` statement is `false`.


```csharp
int number = -10;
if (number > 0) {
    Console.WriteLine("The number is positive.");
}
else {
    Console.WriteLine("The number is not positive.");
}
```

In this case, since the number is not greater than 0, the condition in the `if` statement is `false`, so the code inside the `else` block is executed, printing "The number is not positive."

### else if

The `else if` statement presents an opportunity to specify new conditions for testing if the initial `if` statement's condition evaluates to `false`.

```csharp
int number = 0;
if (number > 0) {
    Console.WriteLine("The number is positive.");
}
else if (number < 0) {
    Console.WriteLine("The number is negative.");
}
else {
    Console.WriteLine("The number is zero.");
}
```

In this example, if the number is greater than 0, it prints "The number is positive." If the number is less than 0, it prints "The number is negative." If neither condition is `true`, it prints "The number is zero."

### switch

The `switch` statement is a type of selection statement that chooses a single switch section to execute from a list of candidates based on a pattern match with the match expression.


```csharp
int number = 1;
switch (number) {
    case 1:
        Console.WriteLine("One");
        break;
    case 2:
        Console.WriteLine("Two");
        break;
    default:
        Console.WriteLine("None");
        break;
}
```

In this case, the `switch` statement evaluates the `number`. It matches the `number` with the cases, and if it finds a match, it executes the code in that case block. In the example above, it would print "One."

The `break` statement serves to terminate the `switch` statement. The absence of the `break` can result in a fall-through to subsequent cases, potentially causing undesired outcomes.

The `default` case within a `switch` statement defines the block of code to be executed should no other cases match the `number`. In the example, if the number was neither 1 nor 2, "None" would be printed.

These conditional statements constitute powerful instruments enabling programmers to control the flow of execution based on varying conditions. Gaining a comprehensive understanding and effectively employing these tools can yield more efficient and adaptable C# programs.

## Loops

In C#, looping constructs are used to execute a block of code multiple times, which is necessary for many programming tasks. The major looping constructs in C# include `for`, `while`, and `do-while`.

### for

The `for` loop is a control flow statement facilitating repeated execution of code. It is typically characterised by three components: an initialiser (where you establish your counter variable), a condition (which prompts continuation of the loop if `true` and termination if `false`), and an iterator (which typically increments the counter variable).

```csharp
for (int i = 0; i < 5; i++) {
    Console.WriteLine(i);
}
```

### while

The `while` loop is another control flow statement that allows code to be executed repeatedly based on a given condition. The loop can be thought of as a repeating `if` statement.

```csharp
int i = 0;
while (i < 5) {
    Console.WriteLine(i);
    i++;
}
```


