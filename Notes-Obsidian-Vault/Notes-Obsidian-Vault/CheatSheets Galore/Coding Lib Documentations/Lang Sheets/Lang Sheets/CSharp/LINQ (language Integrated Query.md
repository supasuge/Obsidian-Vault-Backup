## Table of Contents

- [Where](#Where)
- [Select](#Select)
 - [OrderBy/OrderByDescending](#OrderBy/OrderByDescending)
- [GroupBy](#GroupBy)

LINQ is a set of methods, provided as extension methods in .NET that provide a universal approach to querying data of any type. This data can be in-memory objects (like lists or arrays), XML, databases, or any other format for which a LINQ provider is available. These methods take lambda expressions as arguments.
- Simple
- Type safety
- Seamless Integration

## Syntax


```csharp
/ This creates a new list of integers named 'numbers' and populates it with the numbers from 1 to 10.
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// This is a LINQ query that will create a new collection called 'evenNumbers'. 
// The 'from num in numbers' part signifies that we're querying over the 'numbers' list and will refer to each element as 'num'.
// The 'where num % 2 == 0' part is a condition that each number in the list must satisfy to be included in the new collection - in this case, the number must be even. 
// The '%' operator is the modulus operator, which gives the remainder of integer division. So 'num % 2' gives the remainder when 'num' is divided by 2. If this remainder is 0, then the number is even.
// The 'select num' part signifies that if a number satisfies the condition, then it should be included in the 'evenNumbers' collection.
var evenNumbers = from num in numbers
                  where num % 2 == 0
                  select num;

```

In the above code, we use the `from` clause to define a range variable `num` representing each element in the `numbers` list. The `where` clause filters the numbers based on the condition `num % 2 == 0`, selecting only the even numbers. Finally, the `select` clause projects the selected numbers into the `evenNumbers` variable.

The equivalent code using method syntax would look like this:

Code: csharp

```csharp
// This creates a new list of integers named 'numbers' and populates it with the numbers from 1 to 10.
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// This is a LINQ query using method syntax. It creates a new collection called 'evenNumbers' from the 'numbers' list.
// The 'Where' method filters the 'numbers' list based on the provided lambda expression 'num => num % 2 == 0'.
// The lambda expression takes each number 'num' in the 'numbers' list and returns true if 'num' is even (i.e., if the remainder when 'num' is divided by 2 is 0), and false otherwise.
// The 'Where' method then includes in 'evenNumbers' only those numbers for which the lambda expression returned true.
// As a result, 'evenNumbers' will include all even numbers from the original 'numbers' list. The output will be: 2, 4, 6, 8, 10.
var evenNumbers = numbers.Where(num => num % 2 == 0); // Output: 2, 4, 6, 8, 10
```

## LINQ Operators
`query operators` each perform a specific operation on a data source. The power of LINQ comes from these operators which can be combined in various ways to compose complex queries.
### Where

The `Where` operator filters a sequence based on a specified condition.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// This line filters the 'numbers' list using a LINQ query. The query uses a lambda expression to select only the numbers that are even (i.e., numbers where the remainder of the division by 2 is equal to zero). 
// The result is a new collection 'evenNumbers' containing all the even numbers from the original 'numbers' list.
var evenNumbers = numbers.Where(num => num % 2 == 0);

// Output: 2, 4, 6, 8, 10
foreach (var num in evenNumbers)
{
    Console.WriteLine(num);
}
```

### Select

The `Select` operator projects each element of a sequence into a new form.

```csharp
List<string> names = new List<string> { "John", "Alice", "Michael" };

// This line uses a LINQ query with the Select method to create a new collection 'upperCaseNames'. 
// The query takes the 'names' collection and applies the 'ToUpper' method to each element. 
// The ToUpper method is a built-in C# method that converts all the characters in a string to uppercase.
// The result is a new collection where all the names from the original 'names' collection are transformed into uppercase.
var upperCaseNames = names.Select(name => name.ToUpper());

// Output: JOHN, ALICE, MICHAEL
foreach (var name in upperCaseNames)
{
    Console.WriteLine(name);
}
```

### OrderBy/OrderByDescending

The `OrderBy` and `OrderByDescending` operators sort the elements of a sequence in ascending or descending order.

```csharp
List<int> numbers = new List<int> { 5, 2, 8, 1, 9 };

// The OrderBy method is a LINQ operation that sorts the elements of a collection in ascending order according to a key. In this case, the key is the numbers themselves.
var sortedNumbersAsc = numbers.OrderBy(num => num);

// Output: 1, 2, 5, 8, 9
foreach (var num in sortedNumbersAsc)
{
    Console.WriteLine(num);
}

// The OrderByDescending method is similar to OrderBy, but sorts the elements in descending order. Like in the previous example, the key is the numbers themselves.
var sortedNumbersDesc = numbers.OrderByDescending(num => num);

// Output: 9, 8, 5, 2, 1
foreach (var num in sortedNumbersDesc)
{
    Console.WriteLine(num);
}
```

### GroupBy

The `GroupBy` operator groups elements of a sequence based on a specified key.

```csharp
// Define a class 'Student' with two properties: 'Name' and 'Grade'. The 'get' and 'set' are accessors which control the read-write status of these properties.

class Student
{
    public string Name { get; set; }
    public string Grade { get; set; }
}

// Create a list of students, where each student is an instance of the 'Student' class. Each student has a 'Name' and a 'Grade'.
List<Student> students = new List<Student>
{
    new Student { Name = "John", Grade = "A" },
    new Student { Name = "Alice", Grade = "B" },
    new Student { Name = "Michael", Grade = "A" },
    new Student { Name = "Emily", Grade = "B" }
};

// Using the LINQ GroupBy method, we group the students by their grades. This method returns a collection of `IGrouping<TKey,TElement>` objects, where each `IGrouping` object contains a collection of objects that have the same key.
var studentsByGrade = students.GroupBy(student => student.Grade);

foreach (var group in studentsByGrade)
{
    Console.WriteLine("Students in Grade " + group.Key + ":");
    foreach (var student in group)
    {
        Console.WriteLine(student.Name);
    }
}
// Students in Grade A:
// John
// Michael
// Students in Grade B:
// Alice
// Emily
```

When the `GroupBy` method is called, it groups the elements of the original collection (`students` in this case) based on a specified key. In this case, the key is `student.Grade`, which means the students are grouped by their grades. Each `group` is an `IGrouping<TKey, TElement>` object (where `TKey` is the type of the key and `TElement` is the type of the elements in the group). In this specific case, `TKey` is a `string` (the grade) and `TElement` is a `Student`.


So, in the foreach loop, `group` represents each of these `IGrouping<string, Student>` objects. The `Key` property of each `group` holds the grade (A or B in this example), and iterating over `group` gives you each student in that grade.
### Join

The `Join` operator combines two sequences based on a common key.

```csharp
// This is the Student class with properties for Id, Name, and CourseId. The 'get' and 'set' are accessors which control the read-write status of these properties.
class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int CourseId { get; set; }
}

// This is the Course class with properties for Id and Title. The 'get' and 'set' are accessors which control the read-write status of these properties.
class Course
{
    public int Id { get; set; }
    public string Title { get; set; }
}

// Here we create a list of students, where each student is an instance of the 'Student' class. Each student has an 'Id', 'Name', and a 'CourseId'.
List<Student> students = new List<Student>
{
    new Student { Id = 1, Name = "John", CourseId = 101 },
    new Student { Id = 2, Name = "Alice", CourseId = 102 },
    new Student { Id = 3, Name = "Michael", CourseId = 101 },
    new Student { Id = 4, Name = "Emily", CourseId = 103 }
};

// We create a list of courses, where each course is an instance of the 'Course' class. Each course has an 'Id' and a 'Title'.
List<Course> courses = new List<Course>
{
    new Course { Id = 101, Title = "Mathematics" },
    new Course { Id = 102, Title = "Science" },
    new Course { Id = 103, Title = "History" }
};

// Here we perform a join operation between the 'students' and 'courses' lists using LINQ's Join method.
// We match each student with their corresponding course based on the CourseId from the student and the Id from the course.
// The result is a new anonymous object that includes each student's name and the title of their course.
var studentCourseInfo = students.Join(courses,
                                      student => student.CourseId,
                                      course => course.Id,
                                      (student, course) => new
                                      {
                                          student.Name,
                                          course.Title
                                      });

foreach (var info in studentCourseInfo)
{
    Console.WriteLine(info.Name + " - " + info.Title);
}

// John - Mathematics
// Alice - Science
// Michael - Mathematics
// Emily - History
```


### Aggregate

The `Aggregate` operator applies an accumulator function over a sequence.

Code: csharp

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// This line uses the LINQ Aggregate method to generate a single value from the 'numbers' collection.
// The Aggregate method applies a specified function to the first two elements of the collection, then to the result and the next element, and so on. 
// In this case, the function is a lambda expression '(acc, num) => acc + num', where 'acc' represents the accumulated value so far and 'num' represents the current element.
// So essentially, this code sums up all the numbers in the 'numbers' collection. The resulting sum is then stored in the 'sum' variable.
var sum = numbers.Aggregate((acc, num) => acc + num);

// Output: 15
Console.WriteLine(sum);
```


### Count/Sum/Average/Min/Max

These methods compute a sequence's `count`, `sum`, `average`, `minimum`, or `maximum` value.

Code: csharp

```csharp
List<int> numbers = new List<int> { 5, 2, 8, 1, 9 };

// The Count method is a LINQ extension method that returns the number of elements in the 'numbers' collection. The result is stored in the 'count' variable.
int count = numbers.Count();
// The Sum method calculates the sum of all elements in the 'numbers' collection. The resulting sum is stored in the 'sum' variable.
int sum = numbers.Sum();
// The Average method calculates the average value of all elements in the 'numbers' collection. Since an average can be a fractional number, it's stored in a variable of type double.
double average = numbers.Average();
// The Min method finds the smallest number in the 'numbers' collection. The minimum value found is stored in the 'min' variable.
int min = numbers.Min();
// The Max method finds the largest number in the 'numbers' collection. The maximum value found is stored in the 'max' variable.
int max = numbers.Max();

Console.WriteLine("Count: " + count);        // Output: Count: 5
Console.WriteLine("Sum: " + sum);            // Output: Sum: 25
Console.WriteLine("Average: " + average);    // Output: Average: 5
Console.WriteLine("Min: " + min);            // Output: Min: 1
Console.WriteLine("Max: " + max);            // Output: Max: 9
```




