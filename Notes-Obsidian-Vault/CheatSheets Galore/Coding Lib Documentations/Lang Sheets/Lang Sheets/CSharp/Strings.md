## Table of Contents

- [Difference between a string and a characters](#Difference\between\a\string\and\a\characters)
  - [Dictionary](#Dictionary)
  - [HashSet](#HashSet)
  - [List vs Dictionary vs HashSet](#List\vs\Dictionary\vs\HashSet)
  - [Collection Performance](#Collection\Performance)

In C# a string is not simply a character array, although it can be thought of as akin to an array of characters for some operations. 
- String is an instance of the `System.String` class that provides sophisticated methods and properties, encapsulating a sequence of Unicode characters.



#### Difference between a string and a characters
- Strings in C# are immutable, meaning that once created they cannot be changed.
- Any operations that appear to alter the string are actually creating a new string and discarding the old one. 
- Enhances security and performance for static or barely changing text.

- Character arrays are mutable, and individual elements can be changed freely. This `mutabilitiy` comes at the cost of not having built-in text manipulation and comparison methods as strings do.

```C#
string welcomeMessage = "Welcome";
```

Once you have a string in C#, there are many operations you can perform on it. The `Length` property, for example, returns the number of characters in the string.

```csharp
Console.WriteLine(welcomeMessage.Length); // Outputs: 19
```

This tells us that our `welcomeMessage` string is 19 characters long.

String concatenation is another operation that is used frequently. It is performed using the `+` operator.

```csharp
string firstString = "Welcome ";
string secondString = "to Academy!";
string concatenatedString = firstString + secondString;
Console.WriteLine(concatenatedString); // Outputs: "Welcome to Academy!"
```

When it comes to manipulating the casing of strings, the `String` class provides the `ToLower` and `ToUpper` methods. `ToLower` converts all the characters in a string to lowercase, while `ToUpper` converts them all to uppercase.

Code: csharp

```csharp
string lowerCaseString = welcomeMessage.ToLower();
Console.WriteLine(lowerCaseString); // Outputs: "welcome to academy!"

string upperCaseString = welcomeMessage.ToUpper();
Console.WriteLine(upperCaseString); // Outputs: "WELCOME TO ACADEMY!"
```

There are also methods to check whether a string starts or ends with a specific substring. These are the `StartsWith` and `EndsWith` methods, respectively.

```csharp
bool startsWithWelcome = welcomeMessage.StartsWith("Welcome");
Console.WriteLine(startsWithWelcome); // Outputs: True

bool endsWithProgramming = welcomeMessage.EndsWith("Academy!");
Console.WriteLine(endsWithProgramming); // Outputs: True
```


A common requirement in programming is to determine whether a specific substring exists within a larger string. This can be accomplished with the `Contains` method.

```csharp
bool containsCsharp = welcomeMessage.Contains("C#");
Console.WriteLine(containsCsharp); // Outputs: False
```

Sometimes, you may need to replace all occurrences of a substring within a string with another substring. The `Replace` method allows you to do this.

Code: csharp

```csharp
string replacedMessage = welcomeMessage.Replace("Academy", "HTB Academy");
Console.WriteLine(replacedMessage); // Outputs: "Welcome to HTB Academy!"
```

string interpolation, which provides a more readable and convenient syntax to format strings. Instead of using complicated string concatenation to include variable values within strings, string interpolation allows us to insert expressions inside string literals directly. To create an interpolated string in C#, prefix the string with a `$` symbol, and enclose any variables or expressions you want to interpolate in curly braces `{}`. When the string is processed, these expressions are replaced by their evaluated string representations.

```csharp
string name = "Alice";
string greeting = $"Hello, {name}!";
Console.WriteLine(greeting); // Outputs: "Hello, Alice!"
```

In the above example, `{name}` inside the string literal is replaced by the value of the variable `name`.

Another important string operation is trimming, which is performed using the `Trim` method. This is commonly used to remove a string's leading and trailing white space.

```csharp
string paddedString = "    Extra spaces here    ";
string trimmedString = paddedString.Trim();
Console.WriteLine(trimmedString); // Outputs: "Extra spaces here"
```

The `Substring` method extracts a portion of a string starting at a specified index and continuing for a specified length. For instance:

```csharp
string fullString = "Hello, World!";
string partialString = fullString.Substring(7, 5);
Console.WriteLine(partialString); // Outputs: "World"
```

In the above example, `Substring(7, 5)` returns a new string starting at index 7 and of length 5 from the `fullString`.

Moreover, using the `Split` method, strings can be split into arrays of substrings based on delimiters. This is especially useful when parsing input or handling data that comes in string form.


In the above example, `Substring(7, 5)` returns a new string starting at index 7 and of length 5 from the `fullString`.

Moreover, using the `Split` method, strings can be split into arrays of substrings based on delimiters. This is especially useful when parsing input or handling data that comes in string form.

Code: csharp

```csharp
string sentence = "This is a sentence.";
string[] words = sentence.Split(' ');
foreach (string word in words)
{
    Console.WriteLine(word);
}
// Outputs: 
// "This"
// "is"
// "a"
// "sentence."
```

In this example, the `Split` method splits the `sentence` string into an array of words based on the space character delimiter.

Lastly, the `Join` method concatenates all elements in a string array or collection, using a specified separator between each element.

Code: csharp

```csharp
string[] words = { "This", "is", "a", "sentence" };
string sentence = string.Join(" ", words);
Console.WriteLine(sentence); // Outputs: "This is a sentence"
```


## Dictionary

A `Dictionary<TKey, TValue>` is a collection that stores and retrieves data using a key-value relationship. It is part of the `System.Collections.Generic` namespace in C#.

To use a `Dictionary<TKey, TValue>`, specify the key type (`TKey`) and the value (`TValue`) in the angle brackets. For example, `Dictionary<int, string>` indicates a dictionary where the keys are integers and the values are strings.

Code: csharp

```csharp
Dictionary<string, int> studentGrades = new Dictionary<string, int>();

// Adding key-value pairs to the dictionary
studentGrades.Add("John", 85);
studentGrades.Add("Jane", 92);
studentGrades.Add("Alice", 78);

// Accessing values by key
int johnGrade = studentGrades["John"]; // O(1) lookup by key

// Modifying an existing value
studentGrades["Jane"] = 95;

// Checking if a key exists
bool hasAlice = studentGrades.ContainsKey("Alice");

// Removing a key-value pair
studentGrades.Remove("John");

// Iterating over the key-value pairs
foreach (KeyValuePair<string, int> pair in studentGrades)
{
    Console.WriteLine($"Name: {pair.Key}, Grade: {pair.Value}");
}
```



## HashSet

A `HashSet<T>` collection stores an unordered set of unique elements. The primary characteristic of a `HashSet` is its ability to store unique elements, completely disallowing duplication. Adding elements to a `HashSet` will check if the element already exists before adding it. This makes `HashSet` an optimal choice when you need to store a collection of items without any duplicates and do not require a specific order.

To use a `HashSet`, specify the type of elements (`T`) within the angle brackets. For example, `HashSet<int>` indicates a set of integers.

Code: csharp

```csharp
HashSet<string> namesHashSet = new HashSet<string>();

// Adding elements to the set
namesHashSet.Add("John");
namesHashSet.Add("Jane");
namesHashSet.Add("Alice");

// Checking if an element exists
bool hasAlice = namesHashSet.Contains("Alice"); // O(1) membership check

// Removing an element
namesHashSet.Remove("John");

// Iterating over the elements
foreach (string name in namesHashSet)
{
    Console.WriteLine(name);
}
```


## List vs Dictionary vs HashSet

Each collection type has its unique characteristics, behaviours, and use cases.

|List|Dictionary|HashSet|
|---|---|---|---|
|Data Structure|Ordered|Key-Value Pairs|Unordered, Unique Elements|
|Duplication|Allows duplicates|Keys must be unique|Ensures uniqueness|
|Access and Lookup|Indexed access by index|Fast lookup by unique key|Membership checks|
|Ordering|Maintains order|No specific order|No specific order|
|Element Removal|By index or value|By key|By value|
|Memory Overhead|Consumes memory based on elements|Memory for keys and values|Memory for unique elements|
|Use Cases|Ordered collection, indexed access|Associating values with keys, key-based lookup|Unordered collection, uniqueness and membership checks|

## Collection Performance

Performance considerations vary for each collection type based on the operations performed and the specific use case.

`Big-O notation` is a notation used in computer science to describe the performance characteristics of an algorithm, specifically its time complexity and space complexity.

In terms of time complexity, `Big-O notation` quantifies the worst-case scenario of an algorithm as the size of the input data approaches infinity. For instance, if an algorithm has a time complexity of `O(n)`, it indicates that the time it takes to execute the algorithm grows linearly with the input data size. On the other hand, an algorithm with a time complexity of `O(n^2)` would suggest that the execution time increases quadratically with the input size.

While analysed less frequently, `Big-O` notation can also describe space complexity by measuring the amount of memory an algorithm needs relative to the input size. For example, an algorithm with a space complexity of `O(1)` uses a constant amount of memory regardless of the input size.

Here are some general performance considerations for `List`, `Dictionary`, and `HashSet`:


|List|Dictionary|HashSet|
|---|---|---|---|
|Access Speed|Very fast, O(1)|Average: O(1), Worst: O(n)|Average: O(1), Worst: O(n)|
|Insertion/Removal|Insertion and removal at ends: O(1)|Average: O(1), Worst: O(n)|Average: O(1), Worst: O(n)|
|Searching|Unsorted: O(n)  <br>Sorted (Binary Search): O(log n)|Key-based lookup: Average O(1), Worst O(n)|Membership check: Average O(1), Worst O(n)|
|Memory Overhead|Relatively low|Keys and values, additional structure fields|Unique elements, additional structure fields|


