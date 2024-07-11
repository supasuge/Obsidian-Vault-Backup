## Table of Contents

- [Arrays](#arrays)
  - [Arrays in C\](#Arrays\in\C\)
  - [Multidimensional Arrays in C](#Multidimensional\Arrays\in\C)
    - [The Array Class](#The\Array\Class)
    - [Array.Sort()](#Array.Sort())
    - [Array.Reverse()](#Array.Reverse())
    - [Array.IndexOf()](#Array.IndexOf())

#C #csharp #programming #arrays #introtocsharp #academy 
# Arrays
- Important due to their ability to store multiple values of the same type in a structured manner.


## Arrays in C\#
to declare an array in C# the syntax involves specifying the type of elements that the array will hold. Followed by brackets `[]`
```C#
int[] array;
```
- Declares an array name `array` that will hold integers.
- Does not yet exist in memory yet at this point.

	To Create the array in memory, instantiate it using the `new` keyword, followed by the type of the array elements and the number of elements enclosed in square brackets.
```C#
array = new int[5];
```
- Telling the compiler to create an array of integers with a size of 5. 
	- The `array` variabled references an array of five integer elements, all of which are initialised to 0, the default value for integers.

array can also be declared, instantiated, and initialized in a single line of code. 
```C#
int[] array = new int[] {1, 2, 3, 4, 5};
```
This line declares an array of integers, creates it with a size of 5 and assigns the specified values to the five elements.

## Multidimensional Arrays in C#

C# supports multidimensional arrays. This concept can be extended to two, three, or more dimensions. A two-dimensional array can be considered a table with rows and columns.

Syntax for declaring two-dimensional arrays:
- Specify type of elements that the array will hold, followed by two sets of square brackets. `[,]`

```C# 
int[,] matrix;
```
`mtrix` is a two-dimensional array that will hold integers.


```csharp
matrix = new int[3, 3];
```

This line creates a matrix with 3 rows and 3 columns.

Two-dimensional arrays can also be declared, instantiated, and intialized in a single line of code.
```C#
int[,] matrix = new int[,] {{ 1, ,2,3 }, {4, 5, 6}, {7, 8, 9}};
//Use the GetLength method to get the number of rows (dimension 0)
//and columns (dimension 1)
for (int i =0; i < matrix.GetLength(0); i++) {
    for (int j=0; < matrix.getLength(1)); j++) 
    {
        //Access the elemenet of the array using the indices
        Console.Write(matrix[i, j]+ " ");
    }
    Console.WriteLine(); //Print a newline at the end of each row.
}
```


### The Array Class
	part of the System namespace

- Provides `Length`, `Sort()`, `Reverse()` and many more that allow you to manipulate arrays
	- array's on the other hand are fundamental data type's in C\#.
	- Represents a fixed size (Immutabele)
		- `int`, `string,` `char`, `custom-objects`


```csharp
int[] arr = new int[5]; //arr is an array
```

Here, `arr` is an array of integers. You can add, retrieve, or modify elements using their indices.


```csharp
arr[0] = 1; // Assigns the value 1 to the first element of the array.
```

On the other hand, if you want to use the functionality provided by the `Array` class on this array:


```csharp
Array.Sort(arr); // Uses the Sort method from Array class to sort 'arr'.
```

### Array.Sort()
The `Sort()` method is used to sort the elements in an entire one-dimensional `Array` or, alternatively, a portion of an `Array`.

```csharp
int[] numbers = {8, 2, 6, 3, 1};
Array.Sort(numbers);
```

After sorting, our array would look like:`{1, 2, 3, 6, 8}`.

### Array.Reverse()
The `Reverse()` method reverses the sequence of the elements in the entire one-dimensional `Array` or a portion of it.

For instance:

```csharp
int[] numbers = {1, 2, 3};
Array.Reverse(numbers);
```

The result will be a reversed array: `{3, 2, 1}`.

### Array.IndexOf()

The `IndexOf()` method returns the index of the first occurrence of a value in a one-dimensional `Array` or in a portion of the `Array`.

Consider this example:

Code: csharp

```csharp
int[] numbers = {1, 2, 3};
int index = Array.IndexOf(numbers, 2);
```

The variable `index` now holds the value `1`, which is the index of number `2` in the array.

**How do you access the element in the third row and second column of a two-dimensional array?**
```C#
grid[2,1];
```

### Array.Clear()

The `Clear()` method sets a range of elements in the `Array` to zero (in case of numeric types), false (in case of boolean types), or null (in case of reference types).

Take a look at this example:

```csharp
int[] numbers = {1, 2, 3};
Array.Clear(numbers, 0, numbers.Length);
```


Now all elements in our array are set to zero: `{0, 0, 0}`.





