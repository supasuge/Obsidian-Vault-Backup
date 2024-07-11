In C#, a namespace is a way to group related code elements into logical units, such as `classes`, `interfaces`, `enums`, and more. Namespaces provide organisation and help avoid naming conflicts by creating a `hierarchical structure` for code. They serve as `containers` for `organising and categorising code elements`, making managing and maintaining large codebases easier. Namespaces also enable code reuse and `promote modularity` by clearly `separating concerns`.

The primary purposes of namespaces are twofold:

1. `Organization`: Namespaces systematically organise code elements based on their functionality or domain. They help developers locate and navigate code more efficiently, enhancing code readability and maintainability.
    
2. `Avoiding Naming Conflicts`: Namespaces prevent naming conflicts by providing a unique context for code elements. Code elements within a namespace are distinguished by their fully qualified names, which include the namespace name as a prefix. This ensures that code elements with the same name coexist within different namespaces.

## Creating and Organizing Code Using Namespaces

To create a namespace in C#, use the `namespace` keyword followed by the name. Code elements, such as classes, etc., are defined within the namespace.

```csharp
namespace MyNamespace
{
    class MyClass
    {
        // Class implementation
    }

    interface IMyInterface
    {
        // Interface implementation
    }
}
```

The above example defines a namespace named `MyNamespace`, containing the `MyClass` class and the `IMyInterface` interface. There are a few points to keep in mind when implementing namespaces:

1. `Group-Related Functionality`: Place code elements closely related in the same namespace. This ensures that code with similar functionality is grouped, making it easier to locate and understand.
    
2. `Avoid Over-Nesting`: Keep the namespace hierarchy concise and avoid excessive nesting. Deeply nested namespaces can lead to long, complex names hindering code readability.
    
3. `Follow a Logical Structure`: Create a logical structure for your namespaces that aligns with your project's architecture or module organisation. Consider using a naming convention that reflects the organisation of your codebase.

## Importing and Using Namespaces in C# Programs

You have two options to use code elements from a namespace in a C# program: either fully qualify the code element's name with the namespace, or import the namespace via the `using` directive.

```csharp
using System;

namespace MyNamespace
{
    class Program
    {
        static void Main()
        {
            // Using a fully qualified name
            System.Console.WriteLine("Hello, World!");

            // Using the imported namespace
            Console.WriteLine("Hello, World!");
        }
    }
}
```

## Resolving Naming Conflicts with Namespaces

A naming conflict occurs when multiple namespaces define code elements with the same name. C# provides ways to resolve such conflicts to ensure unambiguous access to code elements.

To resolve naming conflicts, you can use one of the following approaches:

1. `Fully Qualify the Code Element`: Use the fully qualified name of the code element, including the namespace, to ensure explicit identification.

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

How would you import the `System.Collections.Generic` namespace in a C# program?
> `using.System.Collections.Generic;`


