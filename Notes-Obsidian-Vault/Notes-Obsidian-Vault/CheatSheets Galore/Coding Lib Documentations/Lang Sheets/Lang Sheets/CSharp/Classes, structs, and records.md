## Table of Contents

 - [Encapsulation](#Encapsulation)
  - [Members](#Members)
  - [Accessibility](#Accessibility)
    - [Inheritance](#Inheritance)
      - [Intefaces](#Intefaces)
  - [Static Types](#Static\Types)





a class or `struct` or `record` is like a blueprint that specifies what the type can do. An object is basically a block of memory that has been allocated and configured according to the blueprint.



#### Encapsulation
Referred to as the first pillar/principle of OOP. Class/struct can specify how accessuble each of it memebrs is to code outside of the class or strucs.

The _members_ of a type include all methods, fields, constants, properties, and events. In C#, there are no global variables or methods as there are in some other languages. Even a program's entry point, the `Main` method, must be declared within a class or struct (implicitly when you use [top-level statements](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/top-level-statements)).

The following list includes all the various kinds of members that may be declared in a class, struct, or record.

- Fields
- Constants
- Properties
- Methods
- Constructors
- Events
- Finalizers
- Indexers
- Operators
- Nested Types

## Members

The _members_ of a type include all methods, fields, constants, properties, and events.

The following list includes all the various kinds of members that may be declared in a class, struct, or record.

- Fields
- Constants
- Properties
- Methods
- Constructors
- Events
- Finalizers
- Indexers
- Operators
- Nested Types

## Accessibility

Some methods and properties are meant to be called or accessed from code outside a class or struct, known as _client code_. Other methods and properties might be only for use in the class or struct itself. It's important to limit the accessibility of your code so that only the intended client code can reach it. You specify how accessible your types and their members are to client code by using the following access modifiers:

- [public](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/public)
- [protected](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/protected)
- [internal](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/internal)
- [protected internal](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/protected-internal)
- [private](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/private)
- [private protected](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/private-protected).

The default accessibility is `private`

### Inheritance
Classes *But not Structs* support the concpet of inheritance, A class that derives from another class, called the base class, automatically contains all the public, protected, and internal members of the base class except its constructors and finalizers


#### Intefaces
`Classes, structs, and records` can be defined with oen or more type parameters. Client code supplies the type when it creates an instance of the type. Ex: List<T> class in the System.Collections.Generic namespace is defined with one type parameter. Client code creates an instance of a List<string> or List<int> 



## Static Types

Classes (but not structs or records) can be declared as `static


