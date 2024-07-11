## Table of Contents

- [Null Conditional Operators](#Null\Conditional\Operators)

### Null Conditional Operators
Used to access member and elements of an object safely. They return null if the object is `null` instead of throwing a `NullReferenceException`

1. `Null Conditional Member Access (?.)`: facilitates the safe access of an object's members, including properties or methods. As depicted in your second example, the Length property of the `authorName` string is being accessed. If `authorName` is `null`, a `NullReferenceException` will not be thrown. Instead, it will simply yield `null`.
2. `Null Conditional Element Access (?[])`: utilised to secure access to array or collection elements. When the array or collection is `null`, using `?[]` to access an element won't result in an exception. Rather, it will return `null`. For instance, in your first example, as `authors` is `null`, `authors?[0]` consequently returns `null`.
