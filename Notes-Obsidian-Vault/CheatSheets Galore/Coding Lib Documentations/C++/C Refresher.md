## Table of Contents

- [Introduction to C Concepts](#introduction\to\c\concepts)
  - [Structures in C](#Structures\in\C)
    - [Overview](#Overview)
    - [Key Concepts](#Key\Concepts)
    - [Initializing and Accessing Structures](#Initializing\and\Accessing\Structures)
  - [Enumeration (enum)](#Enumeration\(enum))
    - [Overview](#Overview)
    - [Example](#Example)
  - [Union](#Union)
    - [Overview](#Overview)
    - [Example](#Example)
  - [Bitwise Operators](#Bitwise\Operators)
    - [Types](#Types)
    - [Example of Shift Operators](#Example\of\Shift\Operators)
  - [Function Arguments in C](#Function\Arguments\in\C)
    - [Passing by Value](#Passing\by\Value)
    - [Passing by Reference](#Passing\by\Reference)


 >> Structures
> User defined data types that allow the programmer to grou related data items of different types into a single unit.
> Can help organize large amounts of ata

# Introduction to C Concepts

## Structures in C

### Overview
- **Definition**: Structures (`structs`) are user-defined data types in C, used for grouping different data types into a single unit.
- **Usage**: Helpful in organizing large amounts of data related to an object. 

### Key Concepts
- **Members/Elements**: Individual data items in a struct.
- **Declaration**: Typically uses `typedef` for aliasing. For example:
  ```c
  typedef struct _STRUCTURE_NAME {
      // elements
  } STRUCTURE_NAME, *PSTRUCTURE_NAME;
  ```

### Initializing and Accessing Structures
- **Direct Initialization**: Using the structure name directly.
  ```c
  STRUCTURE_NAME struct1 = {0};
  struct1.ID = 1470;
  struct1.Age = 34;
  ```
- **Indirect Initialization (Pointer)**: Using a pointer to the structure.
  ```c
  PSTRUCTURE_NAME structpointer = NULL;
  structpointer->ID = 8765;  // Using arrow operator
  ```

## Enumeration (enum)

### Overview
- **Definition**: `enum` is used to define a set of named constants.
- **Usage**: Commonly represents states, error codes, etc.

### Example
```c
enum Weekdays {
    Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
};

enum Weekdays today = Friday;
```

## Union

### Overview
- **Definition**: A union allows storing different data types in the same memory location.
- **Usage**: Efficient for multiple purposes but less common.

### Example
```c
union ExampleUnion {
    int IntegerVar;
    char CharVar;
    float FloatVar;
};
```

## Bitwise Operators

### Types
1. **Right Shift (>>)** and **Left Shift (<<)**
   - Shift bits to the right or left.
2. **Bitwise OR (|)**
   - Combines bits with OR logic.
3. **Bitwise AND (&)**
   - Combines bits with AND logic.
4. **Bitwise XOR (^)**
   - Combines bits with XOR logic.
5. **Bitwise NOT (~)**
   - Inverts all bits.

### Example of Shift Operators
```c
int value = 0b10100111; // Binary number
int rightShifted = value >> 2;  // Shift right by 2
int leftShifted = value << 2;   // Shift left by 2
```

## Function Arguments in C

### Passing by Value
- **Definition**: Copy of the value is passed; original value remains unchanged.
- **Example**:
  ```c
  int add(int a, int b) {
      return a + b;
  }
  ```

### Passing by Reference
- **Definition**: Address of the value is passed; allows modification of the original value.
- **Example**:
  ```c
  void add(int *a, int *b, int *result) {
      *result = *a + *b;
  }
  ```

---

These explanations and code examples should provide a more digestible understanding of these C programming concepts, suited for your Obsidian notes. If you need further clarification on any of these points, feel free to ask!

