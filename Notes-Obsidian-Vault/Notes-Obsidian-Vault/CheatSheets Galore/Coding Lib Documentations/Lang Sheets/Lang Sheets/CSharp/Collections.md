A collection is used to group related objects. Collections provide a more flexible way to work with groups of objects, as unlike arrays, the group of objects you work with can grow and shrink dynamically as the demands of the application change. Collections are defined in the `System.Collections` namespace

## Iterating through a collection
The `foreach` loop is an efficient and straightforward way to iterate through any collection. It automatically moves to the next item in the collection at the end of each loop iteration, making it an excellent choice for reading collections. Suppose you want to modify the collection while iterating over it. In that case, you might need to use a different looping construct, like a `for` loop, as `foreach` does not support collection modification during iteration.


```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

for (int i = 0; i < numbers.Count; i++)
{
    // Modify the element at index i
    numbers[i] *= 2;
}

foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

