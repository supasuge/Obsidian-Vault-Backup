## Table of Contents



==Body of a given class works as a namespace==


In Python, a **class** serves as a blueprint for creating objects. It encapsulates both **data** (attributes) and **behavior** (methods). Here are the key points about Python classes:

1. **Creating a Class:**
    
    - To define a class, use the `class` keyword followed by the class name. For example:
        
        PythonAI-generated code. Review and use carefully. .
        
        ```python
        class MyClass:
            x = 5
        ```
        
    - In this example, `MyClass` has a property `x` with a value of 5.
2. **Creating Objects (Instances):**
    
    - Once you have a class, you can create objects (instances) from it. For instance:
        
      
        
        ```python
        p1 = MyClass()
        print(p1.x)  # Output: 5
        ```
        
    - Here, `p1` is an object of the `MyClass` class.
3. **The `__init__()` Function:**
    
    - The `__init__()` function is called automatically when an object is created from a class.
    - It initializes object properties. For example:
        
        PythonAI-generated code. Review and use carefully. .
        
        ```python
        class Person:
            def __init__(self, name, age):
                self.name = name
                self.age = age
        
        p1 = Person("John", 36)
        print(p1.name)  # Output: John
        print(p1.age)   # Output: 36
        ```
        
4. **The `__str__()` Function:**
    
    - The `__str__()` function controls what’s returned when an object is represented as a string.
    - If not set, the default string representation of the object is returned.
    - You can customize it like this:
        
        
        
        ```python
        class Person:
            def __init__(self, name, age):
                self.name = name
                self.age = age
        
            def __str__(self):
                return f"{self.name} ({self.age})"
        
        p1 = Person("John", 36)
        print(p1)  # Output: John (36)
        ```
        
5. **Object Methods:**
    
    - Objects can also have methods (functions).
    - Methods belong to the object and can access its variables.
    - Example:
        
       
        
        ```python
        class Person:
            def __init__(self, name, age):
                self.name = name
                self.age = age
        
            def myfunc(self):
                print(f"Hello, my name is {self.name}")
        
        p1 = Person("John", 36)
        p1.myfunc()  # Output: Hello, my name is John
        ```
