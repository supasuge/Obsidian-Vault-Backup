
# Rust Type Definition Cheatsheet

## Scalar Types

### Integer Types

| Type    | Size (bits)  | Range                                                                                                             | Equivalent in Python  |
| ------- | ------------ | ----------------------------------------------------------------------------------------------------------------- | --------------------- |
| `i8`    | $8$          | $-128$ *to* $127$                                                                                                 | `int` (with range)    |
| `i16`   | $16$         | $-32,768$ *to* $32,767$                                                                                           | `int` (with range)    |
| `i32`   | $32$         | $-2,147,483,648$ *to* $2,147,483,647$                                                                             | `int` (with range)    |
| `i64`   | $64$         | $-9,223,372,036,854,775,808$ *to* $9,223,372,036,854,775,807$                                                     | `int` (with range)    |
| `i128`  | $128$        | $-170,141,183,460,469,231,731,687,303,715,884,105,727$ *to* $170,141,183,460,469,231,731,687,303,715,884,105,726$ | `int` (with range)    |
| `isize` | Pointer size | Architecture-dependent (e.g., 32-bit or 64-bit)                                                                   | `int` (dynamic range) |

| Type    | Size (bits)  | Range                                                        | Equivalent in Python  |
| ------- | ------------ | ------------------------------------------------------------ | --------------------- |
| `u8`    | $8$          | $0$ *to* $255$                                               | `int` (with range)    |
| `u16`   | $16$         | $0$ *to* $65,535$                                            | `int` (with range)    |
| `u32`   | $32$         | $0$ *to* $4,294,967,295$                                     | `int` (with range)    |
| `u64`   | $64$         | $0$ *to* $18,446,744,073,709,551,615$                        | `int` (with range)    |
| `u128`  | $128$        | $0$ *to* 340,282,366,920,938,463,463,374,607,431,768,211,455 | `int` (with range)    |
| `usize` | Pointer size | Architecture-dependent (e.g., 32-bit or 64-bit)              | `int` (dynamic range) |

### Floating-Point Types

| Type  | Size (bits) | Range                | Equivalent in Python |
| ----- | ----------- | -------------------- | -------------------- |
| `f32` | 32          | 1.2E-38 to 3.4E+38   | `float`              |
| `f64` | 64          | 2.3E-308 to 1.7E+308 | `float`              |

### Other Scalar Types

| Type   | Description          | Equivalent in Python |
| ------ | -------------------- | -------------------- |
| `bool` | Boolean (true/false) | `bool`               |
| `char` | Unicode scalar value | `str` (single char)  |

## Compound Types

### Tuples
- Fixed-size collections of values of different types.
- Equivalent to Python's tuples.

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;
println!("The value of y is: {}", y);
```


### Arrays
- Fixed-size collections of values of the same type.
- Equivalent to Python's lists, but with fixed size.

```rust
let a = [1, 2, 3, 4, 5];
let first = a[0];
```

## String Types

### `String`
- A growable, heap-allocated data structure used when you need to modify or own string data.
- Equivalent to Python's `str`.

```rust
let mut s = String::from("hello");
s.push_str(", world!");
println!("{}", s);
```

### `&str`
- A string slice; an immutable reference to a string.
- Equivalent to Python's string views (not a direct equivalent, but conceptually similar).

```rust
let s = "hello";
let slice = &s[0..2];
println!("{}", slice); // prints "he"
```

## Common Type Conversions

### Parsing Strings to Numbers
- Similar to Python's `int()` and `float()` functions.

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

### Converting Numbers to Strings
- Similar to Python's `str()` function.

```rust
let num = 42;
let num_str = num.to_string();
println!("{}", num_str);
```

