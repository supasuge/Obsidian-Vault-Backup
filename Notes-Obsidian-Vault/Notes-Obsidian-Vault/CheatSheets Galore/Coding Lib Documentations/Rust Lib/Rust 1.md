## Table of Contents

  - [Hello, Rust!](#Hello,\Rust!)
    - [Data Structures](#Data\Structures)
    - [References & Pointers](#References\&\Pointers)
    - [Functions & Behavior](#Functions\&\Behavior)
    - [Control Flow](#Control\Flow)
    - [Organizing Code](#Organizing\Code)
    - [Type Aliases and Casts](#Type\Aliases\and\Casts)
    - [Macros & Attributes](#Macros\&\Attributes)
    - [Pattern Matching](#Pattern\Matching)
    - [Generics & Constraints](#Generics\&\Constraints)
    - [Higher-Ranked Items 🝖](#Higher-Ranked\Items\🝖)
    - [Strings & Chars](#Strings\&\Chars)
    - [Documentation](#Documentation)
    - [Miscellaneous](#Miscellaneous)
    - [Common Operators](#Common\Operators)
- [Behind the Scenes](#behind\the\scenes)
  - [The Abstract Machine](#The\Abstract\Machine)
  - [Language Sugar](#Language\Sugar)
  - [Memory & Lifetimes](#Memory\&\Lifetimes)
- [Memory Layout](#memory\layout)
  - [Basic Types](#Basic\Types)
      - [Boolean [REF](https://doc.rust-lang.org/stable/reference/types/boolean.html "See this topic in 'The Rust Reference'.") and Numeric Types [REF](https://doc.rust-lang.org/stable/reference/types/numeric.html "See this topic in 'The Rust Reference'.")](#Boolean\[REF](https://doc.rust-lang.org/stable/reference/types/boolean.html\"See\this\topic\in\'The\Rust\Reference'.")\and\Numeric\Types\[REF](https://doc.rust-lang.org/stable/reference/types/numeric.html\"See\this\topic\in\'The\Rust\Reference'."))
      - [Textual Types [REF](https://doc.rust-lang.org/stable/reference/types/textual.html "See this topic in 'The Rust Reference'.")](#Textual\Types\[REF](https://doc.rust-lang.org/stable/reference/types/textual.html\"See\this\topic\in\'The\Rust\Reference'."))
  - [Custom Types](#Custom\Types)
  - [References & Pointers](#References\&\Pointers)
    - [Pointer Meta](#Pointer\Meta)
  - [Closures](#Closures)
  - [Standard Library Types](#Standard\Library\Types)
      - [Order-Preserving Collections](#Order-Preserving\Collections)
      - [Other Collections](#Other\Collections)
      - [Owned Strings](#Owned\Strings)
      - [Shared Ownership](#Shared\Ownership)
- [Standard Library](#standard\library)
  - [One-Liners](#One-Liners)
  - [Thread Safety](#Thread\Safety)
  - [Iterators](#Iterators)
  - [Number Conversions](#Number\Conversions)
  - [String Conversions](#String\Conversions)
  - [String Output](#String\Output)
- [Tooling](#tooling)
  - [Project Anatomy](#Project\Anatomy)
  - [Cargo](#Cargo)
  - [Cross Compilation](#Cross\Compilation)
  - [Tooling Directives](#Tooling\Directives)

**Language Constructs**

- [Data Structures](https://cheats.rs/#data-structures)
- [References & Pointers](https://cheats.rs/#references-pointers)
- [Functions & Behavior](https://cheats.rs/#functions-behavior)
- [Control Flow](https://cheats.rs/#control-flow)
- [Organizing Code](https://cheats.rs/#organizing-code)
- [Type Aliases and Casts](https://cheats.rs/#type-aliases-and-casts)
- [Macros & Attributes](https://cheats.rs/#macros-attributes)
- [Pattern Matching](https://cheats.rs/#pattern-matching)
- [Generics & Constraints](https://cheats.rs/#generics-constraints)
- [Higher-Ranked Items](https://cheats.rs/#higher-ranked-items)🝖
- [Strings & Chars](https://cheats.rs/#strings-chars)
- [Documentation](https://cheats.rs/#documentation)
- [Miscellaneous](https://cheats.rs/#miscellaneous)

**Behind the Scenes**

- [The Abstract Machine](https://cheats.rs/#the-abstract-machine)
- [Language Sugar](https://cheats.rs/#language-sugar)
- [Memory & Lifetimes](https://cheats.rs/#memory-lifetimes)

**Memory Layout**

- [Basic Types](https://cheats.rs/#basic-types)
- [Custom Types](https://cheats.rs/#custom-types)
- [References & Pointers](https://cheats.rs/#references-pointers-ui)
- [Closures](https://cheats.rs/#closures-data)
- [Standard Library Types](https://cheats.rs/#standard-library-types)

**Misc**

- [Links & Services](https://cheats.rs/#links-services)
- [Printing & PDF](https://cheats.rs/#printing-pdf)

**Standard Library**

- [One-Liners](https://cheats.rs/#one-liners)
- [Thread Safety](https://cheats.rs/#thread-safety)
- [Iterators](https://cheats.rs/#iterators)
- [Number Conversions](https://cheats.rs/#number-conversions)
- [String Conversions](https://cheats.rs/#string-conversions)
- [String Output](https://cheats.rs/#string-output)

**Tooling**

- [Project Anatomy](https://cheats.rs/#project-anatomy)
- [Cargo](https://cheats.rs/#cargo)
- [Cross Compilation](https://cheats.rs/#cross-compilation)
- [Tooling Directives](https://cheats.rs/#tooling-directives)

**Working with Types**

- [Types, Traits, Generics](https://cheats.rs/#types-traits-generics)
- [Foreign Types and Traits](https://cheats.rs/#foreign-types-and-traits)
- [Type Conversions](https://cheats.rs/#type-conversions)

**Coding Guides**

- [Idiomatic Rust](https://cheats.rs/#idiomatic-rust)
- [Performance Tips](https://cheats.rs/#performance-tips)
- [Async-Await 101](https://cheats.rs/#async-await-101)
- [Closures in APIs](https://cheats.rs/#closures-in-apis)
- [Unsafe, Unsound, Undefined](https://cheats.rs/#unsafe-unsound-undefined)
- [Adversarial Code](https://cheats.rs/#adversarial-code)🝖
- [API Stability](https://cheats.rs/#api-stability)

## Hello, Rust!

If you are new to Rust, or if you want to try the things below:

 **Hello World**

```rust
fn main() {
    println!("Hello, world!");
}
```

▶️ Edit & Run

 **Strengths**

**Things Rust does measurably really well**

- Compiled code [about same performance](https://benchmarksgame-team.pages.debian.net/benchmarksgame/box-plot-summary-charts.html) as C / C++, and excellent [memory and energy efficiency](https://docente.ifsc.edu.br/mello/livros/java/paperSLE.pdf).
- Can [avoid 70% of all safety issues](https://www.chromium.org/Home/chromium-security/memory-safety) present in C / C++, and most memory issues.
- Strong type system prevents [data races](https://doc.rust-lang.org/nomicon/races.html), brings ['fearless concurrency'](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html) (amongst others).
- Seamless C interop, and [dozens of supported platforms](https://doc.rust-lang.org/rustc/platform-support.html) (based on LLVM).
- ["Most loved or admired language"](https://survey.stackoverflow.co/2023/#section-admired-and-desired-programming-scripting-and-markup-languages) for 4 5 6 7 8 years in a row. 🤷‍♀️
- Modern tooling: `cargo` (builds _just work_), `clippy` (600+ code quality lints), `rustup` (easy toolchain mgmt).

 **Weaknesses**

**Points you might run into**

- Steep learning curve;1 compiler enforcing (esp. memory) rules that would be "best practices" elsewhere.
- Missing Rust-native libs in some domains, target platforms (esp. embedded), IDE features.1
- Longer compile times than "similar" code in other languages.1
- Careless (use of `unsafe` in) libraries can secretly break safety guarantees.
- ~~No formal language specification~~, [🔗](https://spec.ferrocene.dev/ "Third-party site (mainly used in conjunction with other symbols).") ~~can prevent legal use in some domains (aviation, medical, …)~~. [🔗](https://ferrous-systems.com/ferrocene/ "Third-party site (mainly used in conjunction with other symbols).")
- Rust Foundation may offensively use their IP to affect _'Rust'_ projects (e.g, forbid names, impose policies). [🔗](https://devclass.com/2023/04/11/dont-call-it-rust-community-complains-about-draft-trademark-policy-restricting-use-of-word-marks/ "Third-party site (mainly used in conjunction with other symbols).")[🔗](https://old.reddit.com/r/rust/comments/12e7tdb/rust_trademark_policy_feedback_form/ "Third-party site (mainly used in conjunction with other symbols).")2

1 Compare [Rust Survey](https://blog.rust-lang.org/2020/04/17/Rust-survey-2019.html#why-not-use-rust).  
2 Avoiding their marks (e.g, in your name, URL, logo, dress) is probably sufficient.

 **Installation**

**Download**

- Get installer from [**rustup.rs**](https://rustup.rs/) (highly recommended)🔥

**IDEs**

- [**Rust Rover**](https://www.jetbrains.com/rust/) (paid)
- [Visual Studio Code](https://code.visualstudio.com/) with [**rust-analyzer**](https://rust-analyzer.github.io/) (free)

 **First Steps**

**Modular Beginner Resources**

- [**Tour of Rust**](https://tourofrust.com/TOC_en.html) - Live code and explanations, side by side.
- [**Rust in Easy English**](https://dhghomon.github.io/easy_rust/Chapter_3.html) - 60+ concepts, simple English, example-driven.

In addition, have a look at the usual suspects. [BK](https://doc.rust-lang.org/book/ "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/ "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std "See this topic in 'The Rust Standard Library'.")

> **Opinion** 💬 — If you have never seen or used any Rust it might be good to visit one of the links above before continuing; the next chapter might feel a bit terse otherwise.

### Data Structures

Data types and memory locations defined via keywords.

|Example|Explanation|
|---|---|
|`struct S {}`|Define a **struct** [BK](https://doc.rust-lang.org/book/ch05-00-structs.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/custom_types/structs.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/keyword.struct.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/struct-expr.html "See this topic in 'The Rust Reference'.") with named fields.|
|`struct S { x: T }`|Define struct with named field `x` of type `T`.|
|`struct S` ​`(T);`|Define "tupled" struct with numbered field `.0` of type `T`.|
|`struct S;`|Define **zero sized** [NOM](https://doc.rust-lang.org/nightly/nomicon/exotic-sizes.html#zero-sized-types-zsts "See this topic in 'The Rustonomicon'.") unit struct. Occupies no space, optimized away.|
|`enum E {}`|Define an **enum**, [BK](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/custom_types/enum.html#enums "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/enumerations.html "See this topic in 'The Rust Reference'.") _c_. [algebraic data types](https://en.wikipedia.org/wiki/Algebraic_data_type), [tagged unions](https://en.wikipedia.org/wiki/Tagged_union).|
|`enum E { A, B`​`(), C {} }`|Define variants of enum; can be unit- `A`, tuple- `B` ​`()` and struct-like `C{}`.|
|`enum E { A = 1 }`|If variants are only unit-like, allow **discriminant values**, [REF](https://doc.rust-lang.org/stable/reference/items/enumerations.html#custom-discriminant-values-for-fieldless-enumerations "See this topic in 'The Rust Reference'.") e.g., for FFI.|
|`enum E {}`|Enum w/o variants is **uninhabited**, [REF](https://doc.rust-lang.org/stable/reference/glossary.html#uninhabited "See this topic in 'The Rust Reference'.") can't be created, _c._ 'never' [↓](https://cheats.rs/#miscellaneous "On this site, below.") 🝖|
|`union U {}`|Unsafe C-like **union** [REF](https://doc.rust-lang.org/stable/reference/items/unions.html "See this topic in 'The Rust Reference'.") for FFI compatibility. 🝖|
|`static X: T = T();`|**Global variable** [BK](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#accessing-or-modifying-a-mutable-static-variable "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/custom_types/constants.html#constants "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/static-items.html#static-items "See this topic in 'The Rust Reference'.") with `'static` lifetime, single memory location.|
|`const X: T = T();`|Defines **constant**, [BK](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#constants "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/custom_types/constants.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/constant-items.html "See this topic in 'The Rust Reference'.") copied into a temporary when used.|
|`let x: T;`|Allocate `T` bytes on stack1 bound as `x`. Assignable once, not mutable.|
|`let mut x: T;`|Like `let`, but allow for **mutability** [BK](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/variable_bindings/mut.html "See this topic in 'Rust by Example'.") and mutable borrow.2|
|`x = y;`|Moves `y` to `x`, inval. `y` if `T` is not **`Copy`**, [STD](https://doc.rust-lang.org/std/marker/trait.Copy.html "See this topic in 'The Rust Standard Library'.") and copying `y` otherwise.|

1 **Bound variables** [BK](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/variable_bindings.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/variables.html "See this topic in 'The Rust Reference'.") live on stack for synchronous code. In `async {}` they become part of async's state machine, may reside on heap.  
2 Technically _mutable_ and _immutable_ are misnomer. Immutable binding or shared reference may still contain Cell [STD](https://doc.rust-lang.org/std/cell/index.html "See this topic in 'The Rust Standard Library'."), giving _interior mutability_.

Creating and accessing data structures; and some more _sigilic_ types.

|Example|Explanation|
|---|---|
|`S { x: y }`|Create `struct S {}` or `use`'ed `enum E::S {}` with field `x` set to `y`.|
|`S { x }`|Same, but use local variable `x` for field `x`.|
|`S { ..s }`|Fill remaining fields from `s`, esp. useful with `Default::default()`. [STD](https://doc.rust-lang.org/default/trait.Default.html "See this topic in 'The Rust Standard Library'.")|
|`S { 0: x }`|Like `S` ​`(x)` below, but set field `.0` with struct syntax.|
|`S`​ `(x)`|Create `struct S` ​`(T)` or `use`'ed `enum E::S`​ `()` with field `.0` set to `x`.|
|`S`|If `S` is unit `struct S;` or `use`'ed `enum E::S` create value of `S`.|
|`E::C { x: y }`|Create enum variant `C`. Other methods above also work.|
|`()`|Empty tuple, both literal and type, aka **unit**. [STD](https://doc.rust-lang.org/std/primitive.unit.html "See this topic in 'The Rust Standard Library'.")|
|`(x)`|Parenthesized expression.|
|`(x,)`|Single-element **tuple** expression. [EX](https://doc.rust-lang.org/stable/rust-by-example/primitives/tuples.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/primitive.tuple.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/tuple-expr.html "See this topic in 'The Rust Reference'.")|
|`(S,)`|Single-element tuple type.|
|`[S]`|Array type of unspec. length, i.e., **slice**. [EX](https://doc.rust-lang.org/stable/rust-by-example/primitives/array.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/primitive.slice.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/types/slice.html "See this topic in 'The Rust Reference'.") Can't live on stack. *|
|`[S; n]`|**Array type** [EX](https://doc.rust-lang.org/stable/rust-by-example/primitives/array.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/primitive.array.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/types/array.html "See this topic in 'The Rust Reference'.") of fixed length `n` holding elements of type `S`.|
|`[x; n]`|**Array instance** [REF](https://doc.rust-lang.org/stable/reference/expressions/array-expr.html "See this topic in 'The Rust Reference'.") (expression) with `n` copies of `x`.|
|`[x, y]`|Array instance with given elements `x` and `y`.|
|`x[0]`|Collection indexing, here w. `usize`. Impl. via [**Index**](https://doc.rust-lang.org/std/ops/trait.Index.html), [**IndexMut**](https://doc.rust-lang.org/std/ops/trait.IndexMut.html).|
|`x[..]`|Same, via range (here _full range_), also `x[a..b]`, `x[a..=b]`, … _c_. below.|
|`a..b`|**Right-exclusive range** [STD](https://doc.rust-lang.org/std/ops/struct.Range.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/range-expr.html "See this topic in 'The Rust Reference'.") creation, e.g., `1..3` means `1, 2`.|
|`..b`|Right-exclusive **range to** [STD](https://doc.rust-lang.org/std/ops/struct.RangeTo.html "See this topic in 'The Rust Standard Library'.") without starting point.|
|`..=b`|**Inclusive range to** [STD](https://doc.rust-lang.org/std/ops/struct.RangeToInclusive.html "See this topic in 'The Rust Standard Library'.") without starting point.|
|`a..=b`|**Inclusive range**, [STD](https://doc.rust-lang.org/std/ops/struct.RangeInclusive.html "See this topic in 'The Rust Standard Library'.") `1..=3` means `1, 2, 3`.|
|`a..`|**Range from** [STD](https://doc.rust-lang.org/std/ops/struct.RangeFrom.html "See this topic in 'The Rust Standard Library'.") without ending point.|
|`..`|**Full range**, [STD](https://doc.rust-lang.org/std/ops/struct.RangeFull.html "See this topic in 'The Rust Standard Library'.") usually means _the whole collection_.|
|`s.x`|Named **field access**, [REF](https://doc.rust-lang.org/stable/reference/expressions/field-expr.html "See this topic in 'The Rust Reference'.") might try to [Deref](https://doc.rust-lang.org/std/ops/trait.Deref.html) if `x` not part of type `S`.|
|`s.0`|Numbered field access, used for tuple types `S` ​`(T)`.|

* For now,[RFC](https://rust-lang.github.io/rfcs/1909-unsized-rvalues.html "See this topic in the official RFC documents.") pending completion of [tracking issue](https://github.com/rust-lang/rust/issues/48055).

### References & Pointers

Granting access to un-owned memory. Also see section on Generics & Constraints.

|Example|Explanation|
|---|---|
|`&S`|Shared **reference** [BK](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/primitive.reference.html "See this topic in 'The Rust Standard Library'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/references.html "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/types.html#pointer-types "See this topic in 'The Rust Reference'.") (type; space for holding _any_ `&s`).|
|`&[S]`|Special slice reference that contains (`address`, `count`).|
|`&str`|Special string slice reference that contains (`address`, `byte_length`).|
|`&mut S`|Exclusive reference to allow mutability (also `&mut [S]`, `&mut dyn S`, …).|
|`&dyn T`|Special **trait object** [BK](https://doc.rust-lang.org/book/ch17-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types "See this topic in 'The Rust Programming Language'.") reference that contains (`address`, `vtable`).|
|`&s`|Shared **borrow** [BK](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/scope/borrow.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/borrow/trait.Borrow.html "See this topic in 'The Rust Standard Library'.") (e.g., addr., len, vtable, … of _this_ `s`, like `0x1234`).|
|`&mut s`|Exclusive borrow that allows **mutability**. [EX](https://doc.rust-lang.org/stable/rust-by-example/scope/borrow/mut.html "See this topic in 'Rust by Example'.")|
|`*const S`|Immutable **raw pointer type** [BK](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#dereferencing-a-raw-pointer "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/primitive.pointer.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/types.html#raw-pointers-const-and-mut "See this topic in 'The Rust Reference'.") w/o memory safety.|
|`*mut S`|Mutable raw pointer type w/o memory safety.|
|`&raw const s`|Create raw pointer w/o going through ref.; _c_. `ptr:addr_of!()` [STD](https://doc.rust-lang.org/std/ptr/macro.addr_of.html "See this topic in 'The Rust Standard Library'.") 🚧 🝖|
|`&raw mut s`|Same, but mutable. 🚧 Needed for unaligned, packed fields. 🝖|
|`ref s`|**Bind by reference**, [EX](https://doc.rust-lang.org/stable/rust-by-example/scope/borrow/ref.html "See this topic in 'Rust by Example'.") makes binding reference type. 🗑️|
|`let ref r = s;`|Equivalent to `let r = &s`.|
|`let S { ref mut x } = s;`|Mut. ref binding (`let x = &mut s.x`), shorthand destructuring [↓](https://cheats.rs/#pattern-matching "On this site, below.") version.|
|`*r`|**Dereference** [BK](https://doc.rust-lang.org/book/ch15-02-deref.html "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/ops/trait.Deref.html "See this topic in 'The Rust Standard Library'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/vec-deref.html "See this topic in 'The Rustonomicon'.") a reference `r` to access what it points to.|
|`*r = s;`|If `r` is a mutable reference, move or copy `s` to target memory.|
|`s = *r;`|Make `s` a copy of whatever `r` references, if that is `Copy`.|
|`s = *r;`|Won't work 🛑 if `*r` is not `Copy`, as that would move and leave empty.|
|`s = *my_box;`|Special case[🔗](https://old.reddit.com/r/rust/comments/b4so6i/what_is_exactly/ej8xwg8 "Third-party site (mainly used in conjunction with other symbols).") for **`Box`**[STD](https://doc.rust-lang.org/std/boxed/index.html "See this topic in 'The Rust Standard Library'.") that can move out b'ed content not `Copy`.|
|`'a`|A **lifetime parameter**, [BK](https://doc.rust-lang.org/book/ch10-00-generics.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/scope/lifetime.html "See this topic in 'Rust by Example'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/lifetimes.html "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/items/generics.html#type-and-lifetime-parameters "See this topic in 'The Rust Reference'.") duration of a flow in static analysis.|
|`&'a S`|Only accepts address of some `s`; address existing `'a` or longer.|
|`&'a mut S`|Same, but allow address content to be changed.|
|`struct S<'a> {}`|Signals this `S` will contain address with lt. `'a`. Creator of `S` decides `'a`.|
|`trait T<'a> {}`|Signals any `S`, which `impl T for S`, might contain address.|
|`fn f<'a>(t: &'a T)`|Signals this function handles some address. Caller decides `'a`.|
|`'static`|Special lifetime lasting the entire program execution.|

### Functions & Behavior

Define units of code and their abstractions.

|Example|Explanation|
|---|---|
|`trait T {}`|Define a **trait**; [BK](https://doc.rust-lang.org/book/ch10-02-traits.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/trait.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/traits.html "See this topic in 'The Rust Reference'.") common behavior types can adhere to.|
|`trait T : R {}`|`T` is subtrait of **supertrait** [BK](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#using-supertraits-to-require-one-traits-functionality-within-another-trait "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/trait/supertraits.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/traits.html#supertraits "See this topic in 'The Rust Reference'.") `R`. Any `S` must `impl R` before it can `impl T`.|
|`impl S {}`|**Implementation** [REF](https://doc.rust-lang.org/stable/reference/items/implementations.html "See this topic in 'The Rust Reference'.") of functionality for a type `S`, e.g., methods.|
|`impl T for S {}`|Implement trait `T` for type `S`; specifies _how exactly_ `S` acts like `T`.|
|`impl !T for S {}`|Disable an automatically derived **auto trait**. [NOM](https://doc.rust-lang.org/nightly/nomicon/send-and-sync.html "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/special-types-and-traits.html#auto-traits "See this topic in 'The Rust Reference'.") 🚧 🝖|
|`fn f() {}`|Definition of a **function**; [BK](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/fn.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/functions.html "See this topic in 'The Rust Reference'.") or associated function if inside `impl`.|
|`fn f() -> S {}`|Same, returning a value of type S.|
|`fn f(&self) {}`|Define a **method**, [BK](https://doc.rust-lang.org/book/ch05-03-method-syntax.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/fn/methods.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/associated-items.html#methods "See this topic in 'The Rust Reference'.") e.g., within an `impl S {}`.|
|`struct S` ​`(T);`|More arcanely, _also_[↑](https://cheats.rs/#data-structures "On this site, above.") defines `fn S(x: T) -> S` **constructor fn**. [RFC](https://rust-lang.github.io/rfcs/1506-adt-kinds.html#tuple-structs "See this topic in the official RFC documents.") 🝖|
|`const fn f() {}`|Constant `fn` usable at compile time, e.g., `const X: u32 = f(Y)`. '18|
|`async fn f() {}`|**Async** [REF](https://doc.rust-lang.org/stable/reference/items/functions.html#async-functions "See this topic in 'The Rust Reference'.") '18 function transform, [↓](https://cheats.rs/#async-await-101 "On this site, below.") makes `f` return an `impl` **`Future`**. [STD](https://doc.rust-lang.org/std/future/trait.Future.html "See this topic in 'The Rust Standard Library'.")|
|`async fn f() -> S {}`|Same, but make `f` return an `impl Future<Output=S>`.|
|`async { x }`|Used within a function, make `{ x }` an `impl Future<Output=X>`.|
|`fn() -> S`|**Function references**, 1 [BK](https://doc.rust-lang.org/book/ch19-05-advanced-functions-and-closures.html#function-pointers "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/primitive.fn.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/types.html#function-pointer-types "See this topic in 'The Rust Reference'.") memory holding address of a callable.|
|`Fn() -> S`|**Callable Trait** [BK](https://doc.rust-lang.org/book/ch19-05-advanced-functions-and-closures.html#returning-closures "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/ops/trait.Fn.html "See this topic in 'The Rust Standard Library'.") (also `FnMut`, `FnOnce`), impl. by closures, fn's …|
|`\| {}`|A **closure** [BK](https://doc.rust-lang.org/book/ch13-01-closures.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/fn/closures.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/closure-expr.html "See this topic in 'The Rust Reference'.") that borrows its **captures**, [↓](https://cheats.rs/#closures-data "On this site, below.") [REF](https://doc.rust-lang.org/stable/reference/types/closure.html#capture-modes "See this topic in 'The Rust Reference'.") (e.g., a local variable).|
|`\|x\| {}`|Closure accepting one argument named `x`, body is block expression.|
|`\|x\| x + x`|Same, without block expression; may only consist of single expression.|
|`move \|x\| x + y`|**Move closure** [REF](https://doc.rust-lang.org/stable/reference/types/closure.html#capture-modes "See this topic in 'The Rust Reference'.") taking ownership; i.e., `y` transferred into closure.|
|`return \| true`|Closures sometimes look like logical ORs (here: return a closure).|
|`unsafe`|If you enjoy debugging segfaults; **unsafe code**. [↓](https://cheats.rs/#unsafe-unsound-undefined "On this site, below.") [BK](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#unsafe-superpowers "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/unsafe.html#unsafe-operations "See this topic in 'Rust by Example'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/meet-safe-and-unsafe.html "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/unsafe-blocks.html#unsafe-blocks "See this topic in 'The Rust Reference'.")|
|`unsafe fn f() {}`|Means "_calling can cause UB, [↓](https://cheats.rs/#unsafe-unsound-undefined "On this site, below.") **YOU must check** requirements_".|
|`unsafe trait T {}`|Means "_careless impl. of `T` can cause UB_; **implementor must check**".|
|`unsafe { f(); }`|Guarantees to compiler "_**I have checked** requirements, trust me_".|
|`unsafe impl T for S {}`|Guarantees _`S` is well-behaved w.r.t `T`_; people may use `T` on `S` safely.|

1 Most documentation calls them function **pointers**, but function **references** might be more appropriate[🔗](https://users.rust-lang.org/t/why-are-function-pointers-special-no-null/87990/16 "Third-party site (mainly used in conjunction with other symbols).") as they can't be `null` and must point to valid target.

### Control Flow

Control execution within a function.

|Example|Explanation|
|---|---|
|`while x {}`|**Loop**, [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#predicate-loops "See this topic in 'The Rust Reference'.") run while expression `x` is true.|
|`loop {}`|**Loop indefinitely** [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#infinite-loops "See this topic in 'The Rust Reference'.") until `break`. Can yield value with `break x`.|
|`for x in collection {}`|Syntactic sugar to loop over **iterators**. [BK](https://doc.rust-lang.org/book/ch13-02-iterators.html "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/iter/index.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#iterator-loops "See this topic in 'The Rust Reference'.")|
|↪  `collection.into_iter()`|Effectively converts any **`IntoIterator`** [STD](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html "See this topic in 'The Rust Standard Library'.") type into proper iterator first.|
|↪  `iterator.next()`|On proper **`Iterator`** [STD](https://doc.rust-lang.org/std/iter/trait.Iterator.html "See this topic in 'The Rust Standard Library'.") then `x = next()` until exhausted (first `None`).|
|`if x {} else {}`|**Conditional branch** [REF](https://doc.rust-lang.org/stable/reference/expressions/if-expr.html "See this topic in 'The Rust Reference'.") if expression is true.|
|`'label: {}`|**Block label**, [RFC](https://rust-lang.github.io/rfcs/2046-label-break-value.html "See this topic in the official RFC documents.") can be used with `break` to exit out of this block. 1.65+|
|`'label: loop {}`|Similar **loop label**, [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/loop/nested.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#loop-labels "See this topic in 'The Rust Reference'.") useful for flow control in nested loops.|
|`break`|**Break expression** [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#break-expressions "See this topic in 'The Rust Reference'.") to exit a labelled block or loop.|
|`break 'label x`|Break out of block or loop named `'label` and make `x` its value.|
|`break 'label`|Same, but don't produce any value.|
|`break x`|Make `x` value of the innermost loop (only in actual `loop`).|
|`continue`|**Continue expression** [REF](https://doc.rust-lang.org/stable/reference/expressions/loop-expr.html#continue-expressions "See this topic in 'The Rust Reference'.") to the next loop iteration of this loop.|
|`continue 'label`|Same but instead of this loop, enclosing loop marked with 'label.|
|`x?`|If `x` is [Err](https://doc.rust-lang.org/std/result/enum.Result.html#variant.Err) or [None](https://doc.rust-lang.org/std/option/enum.Option.html#variant.None), **return and propagate**. [BK](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#propagating-errors "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/error/result/enter_question_mark.html "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/result/index.html#the-question-mark-operator- "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/operator-expr.html#the-question-mark-operator "See this topic in 'The Rust Reference'.")|
|`x.await`|Syntactic sugar to **get future, poll, yield**. [REF](https://doc.rust-lang.org/stable/reference/expressions/await-expr.html#await-expressions "See this topic in 'The Rust Reference'.") '18 Only inside `async`.|
|↪  `x.into_future()`|Effectively converts any **`IntoFuture`** [STD](https://doc.rust-lang.org/std/future/trait.IntoFuture.html "See this topic in 'The Rust Standard Library'.") type into proper future first.|
|↪  `future.poll()`|On proper **`Future`** [STD](https://doc.rust-lang.org/std/future/trait.Future.html "See this topic in 'The Rust Standard Library'.") then `poll()` and yield flow if **`Poll::Pending`**. [STD](https://doc.rust-lang.org/std/task/enum.Poll.html "See this topic in 'The Rust Standard Library'.")|
|`return x`|**Early return** [REF](https://doc.rust-lang.org/stable/reference/expressions/return-expr.html "See this topic in 'The Rust Reference'.") from fn. More idiomatic is to end with expression.|
|`{ return }`|Inside normal `{}`-blocks `return` exits surrounding function.|
|`\| { return }`|Within closures `return` exits that _c._ only, i.e., closure is _s. fn._|
|`async { return }`|Inside `async` a `return` **only** [REF](https://doc.rust-lang.org/stable/reference/expressions/block-expr.html#control-flow-operators "See this topic in 'The Rust Reference'.") 🛑 exits that `{}`, i.e., `async {}` is _s. fn._|
|`f()`|Invoke callable `f` (e.g., a function, closure, function pointer, `Fn`, …).|
|`x.f()`|Call member fn, requires `f` takes `self`, `&self`, … as first argument.|
|`X::f(x)`|Same as `x.f()`. Unless `impl Copy for X {}`, `f` can only be called once.|
|`X::f(&x)`|Same as `x.f()`.|
|`X::f(&mut x)`|Same as `x.f()`.|
|`S::f(&x)`|Same as `x.f()` if `X` [derefs](https://doc.rust-lang.org/std/ops/trait.Deref.html) to `S`, i.e., `x.f()` finds methods of `S`.|
|`T::f(&x)`|Same as `x.f()` if `X impl T`, i.e., `x.f()` finds methods of `T` if in scope.|
|`X::f()`|Call associated function, e.g., `X::new()`.|
|`<X as T>::f()`|Call trait method `T::f()` implemented for `X`.|

### Organizing Code

Segment projects into smaller units and minimize dependencies.

|Example|Explanation|
|---|---|
|`mod m {}`|Define a **module**, [BK](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/mod.html#modules "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/modules.html#modules "See this topic in 'The Rust Reference'.") get definition from inside `{}`. [↓](https://cheats.rs/#project-anatomy "On this site, below.")|
|`mod m;`|Define a module, get definition from `m.rs` or `m/mod.rs`. [↓](https://cheats.rs/#project-anatomy "On this site, below.")|
|`a::b`|Namespace **path** [EX](https://doc.rust-lang.org/stable/rust-by-example/mod/use.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/paths.html "See this topic in 'The Rust Reference'.") to element `b` within `a` (`mod`, `enum`, …).|
|`::b`|Search `b` in **crate root** '15 [REF](https://doc.rust-lang.org/stable/reference/glossary.html#crate "See this topic in 'The Rust Reference'.") or **ext. prelude**; '18 [REF](https://doc.rust-lang.org/stable/reference/names/preludes.html#extern-prelude "See this topic in 'The Rust Reference'.") **global path**. [REF](https://doc.rust-lang.org/stable/reference/paths.html#path-qualifiers "See this topic in 'The Rust Reference'.") 🗑️|
|`crate::b`|Search `b` in crate root. '18|
|`self::b`|Search `b` in current module.|
|`super::b`|Search `b` in parent module.|
|`use a::b;`|**Use** [EX](https://doc.rust-lang.org/stable/rust-by-example/mod/use.html#the-use-declaration "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/use-declarations.html "See this topic in 'The Rust Reference'.") `b` directly in this scope without requiring `a` anymore.|
|`use a::{b, c};`|Same, but bring `b` and `c` into scope.|
|`use a::b as x;`|Bring `b` into scope but name `x`, like `use std::error::Error as E`.|
|`use a::b as _;`|Bring `b` anon. into scope, useful for traits with conflicting names.|
|`use a::*;`|Bring everything from `a` in, only recomm. if `a` is some **prelude**. [STD](https://doc.rust-lang.org/std/prelude/index.html#other-preludes "See this topic in 'The Rust Standard Library'.") [🔗](https://stackoverflow.com/questions/36384840/what-is-the-prelude "Third-party site (mainly used in conjunction with other symbols).")|
|`pub use a::b;`|Bring `a::b` into scope and reexport from here.|
|`pub T`|"Public if parent path is public" **visibility** [BK](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/visibility-and-privacy.html "See this topic in 'The Rust Reference'.") for `T`.|
|`pub(crate) T`|Visible at most1 in current crate.|
|`pub(super) T`|Visible at most1 in parent.|
|`pub(self) T`|Visible at most1 in current module (default, same as no `pub`).|
|`pub(in a::b) T`|Visible at most1 in ancestor `a::b`.|
|`extern crate a;`|Declare dependency on external **crate**; [BK](https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html#using-a-crate-to-get-more-functionality "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/items/extern-crates.html#extern-crate-declarations "See this topic in 'The Rust Reference'.") 🗑️ just `use a::b` in '18.|
|`extern "C" {}`|_Declare_ external dependencies and ABI (e.g., `"C"`) from **FFI**. [BK](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#using-extern-functions-to-call-external-code "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/std_misc/ffi.html#foreign-function-interface "See this topic in 'Rust by Example'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/ffi.html#calling-foreign-functions "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/items/external-blocks.html#external-blocks "See this topic in 'The Rust Reference'.")|
|`extern "C" fn f() {}`|_Define_ function to be exported with ABI (e.g., `"C"`) to FFI.|

1 Items in child modules always have access to any item, regardless if `pub` or not.

### Type Aliases and Casts

Short-hand names of types, and methods to convert one type to another.

|Example|Explanation|
|---|---|
|`type T = S;`|Create a **type alias**, [BK](https://doc.rust-lang.org/book/ch19-04-advanced-types.html#creating-type-synonyms-with-type-aliases "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/items/type-aliases.html#type-aliases "See this topic in 'The Rust Reference'.") i.e., another name for `S`.|
|`Self`|Type alias for **implementing type**, [REF](https://doc.rust-lang.org/stable/reference/types.html#self-types "See this topic in 'The Rust Reference'.") e.g., `fn new() -> Self`.|
|`self`|Method subject in `fn f(self) {}`, e.g., akin to `fn f(self: Self) {}`.|
|`&self`|Same, but refers to self as borrowed, would equal `f(self: &Self)`|
|`&mut self`|Same, but mutably borrowed, would equal `f(self: &mut Self)`|
|`self: Box<Self>`|[**Arbitrary self type**](https://github.com/withoutboats/rfcs/blob/arbitray-receivers/text/0000-century-of-the-self-type.md), add methods to smart ptrs (`my_box.f_of_self()`).|
|`<S as T>`|**Disambiguate** [BK](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#fully-qualified-syntax-for-disambiguation-calling-methods-with-the-same-name "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/call-expr.html#disambiguating-function-calls "See this topic in 'The Rust Reference'.") type `S` as trait `T`, e.g., `<S as T>::f()`.|
|`a::b as c`|In `use` of symbol, import `S` as `R`, e.g., `use a::S as R`.|
|`x as u32`|Primitive **cast**, [EX](https://doc.rust-lang.org/stable/rust-by-example/types/cast.html#casting "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/operator-expr.html#type-cast-expressions "See this topic in 'The Rust Reference'.") may truncate and be a bit surprising. 1 [NOM](https://doc.rust-lang.org/nightly/nomicon/casts.html "See this topic in 'The Rustonomicon'.")|

1 See [**Type Conversions**](https://cheats.rs/#type-conversions) below for all the ways to convert between types.

### Macros & Attributes

Code generation constructs expanded before the actual compilation happens.

|Example|Explanation|
|---|---|
|`m!()`|**Macro** [BK](https://doc.rust-lang.org/book/ch19-06-macros.html "See this topic in 'The Rust Programming Language'.") [STD](https://doc.rust-lang.org/std/index.html#macros "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/macros.html "See this topic in 'The Rust Reference'.") invocation, also `m!{}`, `m![]` (depending on macro).|
|`#[attr]`|Outer **attribute**, [EX](https://doc.rust-lang.org/stable/rust-by-example/attribute.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/attributes.html "See this topic in 'The Rust Reference'.") annotating the following item.|
|`#![attr]`|Inner attribute, annotating the _upper_, surrounding item.|

|Inside Macros 1|Explanation|
|---|---|
|`$x:ty`|Macro capture, the `:…` **fragment** [REF](https://doc.rust-lang.org/stable/reference/macros-by-example.html#metavariables "See this topic in 'The Rust Reference'.") declares what is allowed for `$x`. 2|
|`$x`|Macro substitution, e.g., use the captured `$x:ty` from above.|
|`$(x),*`|Macro **repetition** [REF](https://doc.rust-lang.org/stable/reference/macros-by-example.html#repetitions "See this topic in 'The Rust Reference'.") _zero or more times_.|
|`$(x),+`|Same, but _one or more times_.|
|`$(x)?`|Same, but _zero or one time_ (separator doesn't apply).|
|`$(x)<<+`|In fact separators other than `,` are also accepted. Here: `<<`.|

1 Applies to **'macros by example'**. [REF](https://doc.rust-lang.org/stable/reference/macros-by-example.html "See this topic in 'The Rust Reference'.")  
2 See [**Tooling Directives**](https://cheats.rs/#tooling-directives) below for all captures.

### Pattern Matching

Constructs found in `match` or `let` expressions, or function parameters.

|Example|Explanation|
|---|---|
|`match m {}`|Initiate **pattern matching**, [BK](https://doc.rust-lang.org/book/ch06-02-match.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/match-expr.html "See this topic in 'The Rust Reference'.") then use match arms, _c_. next table.|
|`let S(x) = get();`|Notably, `let` also **destructures** [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match/destructuring.html "See this topic in 'Rust by Example'.") similar to the table below.|
|`let S { x } = s;`|Only `x` will be bound to value `s.x`.|
|`let (_, b, _) = abc;`|Only `b` will be bound to value `abc.1`.|
|`let (a, ..) = abc;`|Ignoring 'the rest' also works.|
|`let (.., a, b) = (1, 2);`|Specific bindings take precedence over 'the rest', here `a` is `1`, `b` is `2`.|
|`let s @ S { x } = get();`|Bind `s` to `S` while `x` is bnd. to `s.x`, **pattern binding**, [BK](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#-bindings "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match/binding.html#binding "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/patterns.html#identifier-patterns "See this topic in 'The Rust Reference'.") _c_. below 🝖|
|`let w @ t @ f = get();`|Stores 3 copies of `get()` result in each `w`, `t`, `f`. 🝖|
|`let (\|x\| x) = get();`|Pathological or-pattern,[↓](https://cheats.rs/#pattern-matching "On this site, below.") **not** closure.🛑 Same as `let x = get();` 🝖|
|`let Some(x) = get();`|**Won't** work 🛑 if p. can be **refuted**, [REF](https://doc.rust-lang.org/stable/reference/expressions/if-expr.html#if-let-expressions "See this topic in 'The Rust Reference'.") use `let else` or `if let` instead.|
|`let Some(x) = get() else {};`|Try to assign [RFC](https://rust-lang.github.io/rfcs/3137-let-else.html "See this topic in the official RFC documents.") if not `else {}` w. must `break`, `return`, `panic!`, … 1.65+ 🔥|
|`if let Some(x) = get() {}`|Branch if pattern can be assigned (e.g., `enum` variant), syntactic sugar. *|
|`while let Some(x) = get() {}`|Equiv.; here keep calling `get()`, run `{}` as long as _p._ can be assigned.|
|`fn f(S { x }: S)`|Function param. also work like `let`, here `x` bound to `s.x` of `f(s)`. 🝖|

* Desugars to `match get() { Some(x) => {}, _ => () }`.

Pattern matching arms in `match` expressions. Left side of these arms can also be found in `let` expressions.

|Within Match Arm|Explanation|
|---|---|
|`E::A => {}`|Match enum variant `A`, _c_. **pattern matching**. [BK](https://doc.rust-lang.org/book/ch06-02-match.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/match-expr.html "See this topic in 'The Rust Reference'.")|
|`E::B ( .. ) => {}`|Match enum tuple variant `B`, ignoring any index.|
|`E::C { .. } => {}`|Match enum struct variant `C`, ignoring any field.|
|`S { x: 0, y: 1 } => {}`|Match _s._ with specific values (only `s` with `s.x` of `0` and `s.y` of `1`).|
|`S { x: a, y: b } => {}`|Match _s._ with _any_ 🛑 values and bind `s.x` to `a` and `s.y` to `b`.|
|`S { x, y } => {}`|Same, but shorthand with `s.x` and `s.y` bound as `x` and `y` respectively.|
|`S { .. } => {}`|Match struct with any values.|
|`D => {}`|Match enum variant `E::D` if `D` in `use`.|
|`D => {}`|Match anything, bind `D`; possibly false friend 🛑 of `E::D` if `D` not in `use`.|
|`_ => {}`|Proper wildcard that matches anything / "all the rest".|
|`0 \| 1 => {}`|Pattern alternatives, **or-patterns**. [RFC](https://rust-lang.github.io/rfcs/2535-or-patterns.html "See this topic in the official RFC documents.")|
|`E::A \| E::Z => {}`|Same, but on enum variants.|
|`E::C {x} \| E::D {x} => {}`|Same, but bind `x` if all variants have it.|
|`Some(A \| B) => {}`|Same, can also match alternatives deeply nested.|
|`\|x\| x => {}`|**Pathological or-pattern**,[↑](https://cheats.rs/#pattern-matching "On this site, above.")🛑 leading `\|` ignored, is just `x \| x`, thus `x`. 🝖|
|`(a, 0) => {}`|Match tuple with any value for `a` and `0` for second.|
|`[a, 0] => {}`|**Slice pattern**, [REF](https://doc.rust-lang.org/stable/reference/patterns.html#slice-patterns "See this topic in 'The Rust Reference'.") [🔗](https://doc.rust-lang.org/edition-guide/rust-2018/slice-patterns.html "Third-party site (mainly used in conjunction with other symbols).") match array with any value for `a` and `0` for second.|
|`[1, ..] => {}`|Match array starting with `1`, any value for rest; **subslice pattern**. [REF](https://doc.rust-lang.org/stable/reference/patterns.html#rest-patterns "See this topic in 'The Rust Reference'.") [RFC](https://rust-lang.github.io/rfcs/2359-subslice-pattern-syntax.html "See this topic in the official RFC documents.")|
|`[1, .., 5] => {}`|Match array starting with `1`, ending with `5`.|
|`[1, x @ .., 5] => {}`|Same, but also bind `x` to slice representing middle (_c._ pattern binding).|
|`[a, x @ .., b] => {}`|Same, but match any first, last, bound as `a`, `b` respectively.|
|`1 .. 3 => {}`|**Range pattern**, [BK](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#matching-ranges-of-values-with- "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/patterns.html#range-patterns "See this topic in 'The Rust Reference'.") here matches `1` and `2`; partially unstable. 🚧|
|`1 ..= 3 => {}`|Inclusive range pattern, matches `1`, `2` and `3`.|
|`1 .. => {}`|Open range pattern, matches `1` and any larger number.|
|`x @ 1..=5 => {}`|Bind matched to `x`; **pattern binding**, [BK](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#-bindings "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match/binding.html#binding "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/patterns.html#identifier-patterns "See this topic in 'The Rust Reference'.") here `x` would be `1` … `5`.|
|`Err(x @ Error {..}) => {}`|Also works nested, here `x` binds to `Error`, esp. useful with `if` below.|
|`S { x } if x > 10 => {}`|Pattern **match guards**, [BK](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#extra-conditionals-with-match-guards "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/flow_control/match/guard.html#guards "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions/match-expr.html#match-guards "See this topic in 'The Rust Reference'.") condition must be true as well to match.|

### Generics & Constraints

Generics combine with type constructors, traits and functions to give your users more flexibility.

|Example|Explanation|
|---|---|
|`struct S<T> …`|A **generic** [BK](https://doc.rust-lang.org/book/ch10-01-syntax.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/generics.html "See this topic in 'Rust by Example'.") type with a type parameter (`T` is placeholder here).|
|`S<T> where T: R`|**Trait bound**, [BK](https://doc.rust-lang.org/book/ch10-02-traits.html#using-trait-bounds-to-conditionally-implement-methods "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/generics/bounds.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/trait-bounds.html#trait-and-lifetime-bounds "See this topic in 'The Rust Reference'.") limits allowed `T`, guarantees `T` has trait `R`.|
|`where T: R, P: S`|**Independent trait bounds**, here one for `T` and one for (not shown) `P`.|
|`where T: R, S`|Compile error, 🛑 you probably want compound bound `R + S` below.|
|`where T: R + S`|**Compound trait bound**, [BK](https://doc.rust-lang.org/book/ch10-02-traits.html#specifying-multiple-trait-bounds-with-the--syntax "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/generics/multi_bounds.html "See this topic in 'Rust by Example'.") `T` must fulfill `R` and `S`.|
|`where T: R + 'a`|Same, but w. lifetime. `T` must fulfill `R`, if `T` has _lt._, must outlive `'a`.|
|`where T: ?Sized`|Opt out of a pre-defined trait bound, here `Sized`. ?|
|`where T: 'a`|Type **lifetime bound**; [EX](https://doc.rust-lang.org/stable/rust-by-example/scope/lifetime/lifetime_bounds.html "See this topic in 'Rust by Example'.") if T has references, they must outlive `'a`.|
|`where T: 'static`|Same; does _not_ mean value `t` _will_ 🛑 live `'static`, only that it could.|
|`where 'b: 'a`|Lifetime `'b` must live at least as long as (i.e., _outlive_) `'a` bound.|
|`where u8: R<T>`|Can also make conditional statements involving _other_ types. 🝖|
|`S<T: R>`|Short hand bound, almost same as above, shorter to write.|
|`S<const N: usize>`|**Generic const bound**; [REF](https://doc.rust-lang.org/stable/reference/items/generics.html#const-generics "See this topic in 'The Rust Reference'.") user of type `S` can provide constant value `N`.|
|`S<10>`|Where used, const bounds can be provided as primitive values.|
|`S<{5+5}>`|Expressions must be put in curly brackets.|
|`S<T = R>`|**Default parameters**; [BK](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#default-generic-type-parameters-and-operator-overloading "See this topic in 'The Rust Programming Language'.") makes `S` a bit easier to use, but keeps flexible.|
|`S<const N: u8 = 0>`|Default parameter for constants; e.g., in `f(x: S) {}` param `N` is `0`.|
|`S<T = u8>`|Default parameter for types, e.g., in `f(x: S) {}` param `T` is `u8`.|
|`S<'_>`|Inferred **anonymous lt.**; asks compiler to _'figure it out'_ if obvious.|
|`S<_>`|Inferred **anonymous type**, e.g., as `let x: Vec<_> = iter.collect()`|
|`S::<T>`|**Turbofish** [STD](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect "See this topic in 'The Rust Standard Library'.") call site type disambiguation, e.g., `f::<u32>()`.|
|`trait T<X> {}`|A trait generic over `X`. Can have multiple `impl T for S` (one per `X`).|
|`trait T { type X; }`|Defines **associated type** [BK](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#specifying-placeholder-types-in-trait-definitions-with-associated-types "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/items/associated-items.html#associated-types "See this topic in 'The Rust Reference'.") [RFC](https://rust-lang.github.io/rfcs/0195-associated-items.html "See this topic in the official RFC documents.") `X`. Only one `impl T for S` possible.|
|`trait T { type X<G>; }`|Defines **generic associated type** (GAT), [RFC](https://rust-lang.github.io/rfcs/1598-generic_associated_types.html "See this topic in the official RFC documents.") `X` can be generic `Vec<>`.|
|`trait T { type X<'a>; }`|Defines a GAT generic over a lifetime.|
|`type X = R;`|Set associated type within `impl T for S { type X = R; }`.|
|`type X<G> = R<G>;`|Same for GAT, e.g., `impl T for S { type X<G> = Vec<G>; }`.|
|`impl<T> S<T> {}`|Impl. `fn`'s for any `T` in `S<T>` **_generically_**, [REF](https://doc.rust-lang.org/stable/reference/items/implementations.html#generic-implementations "See this topic in 'The Rust Reference'.") here `T` ty. parameter.|
|`impl S<T> {}`|Impl. `fn`'s for exactly `S<T>` **_inherently_**, [REF](https://doc.rust-lang.org/stable/reference/items/implementations.html#inherent-implementations "See this topic in 'The Rust Reference'.") here `T` specific type, e.g., `u8`.|
|`fn f() -> impl T`|**Existential types**, [BK](https://doc.rust-lang.org/book/ch10-02-traits.html#returning-types-that-implement-traits "See this topic in 'The Rust Programming Language'.") returns an unknown-to-caller `S` that `impl T`.|
|`fn f(x: &impl T)`|Trait bound via "**impl traits**", [BK](https://doc.rust-lang.org/book/ch10-02-traits.html#trait-bound-syntax "See this topic in 'The Rust Programming Language'.") similar to `fn f<S: T>(x: &S)` below.|
|`fn f(x: &dyn T)`|Invoke `f` via **dynamic dispatch**, [BK](https://doc.rust-lang.org/book/ch17-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types "See this topic in 'The Rust Programming Language'.") [REF](https://doc.rust-lang.org/stable/reference/types.html#trait-objects "See this topic in 'The Rust Reference'.") `f` will not be instantiated for `x`.|
|`fn f<X: T>(x: X)`|Fn. generic over `X`, `f` will be instantiated ('[monomorphized](https://en.wikipedia.org/wiki/Monomorphization)') per `X`.|
|`fn f() where Self: R;`|In `trait T {}`, make `f` accessible only on types known to also `impl R`.|
|`fn f() where Self: Sized;`|Using `Sized` can opt `f` out of trait object vtable, enabling `dyn T`.|
|`fn f() where Self: R {}`|Other `R` useful w. dflt. fn. (non dflt. would need be impl'ed anyway).|
||

### Higher-Ranked Items 🝖

_Actual_ types and traits, abstract over something, usually lifetimes.

|Example|Explanation|
|---|---|
|`for<'a>`|Marker for **higher-ranked bounds.** [NOM](https://doc.rust-lang.org/nightly/nomicon/hrtb.html "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/trait-bounds.html#higher-ranked-trait-bounds "See this topic in 'The Rust Reference'.") 🝖|
|`trait T: for<'a> R<'a> {}`|Any `S` that `impl T` would also have to fulfill `R` for any lifetime.|
|`fn(&'a u8)`|Function pointer type holding fn callable with **specific** lifetime `'a`.|
|`for<'a> fn(&'a u8)`|**Higher-ranked type**1 [🔗](https://github.com/rust-lang/rust/issues/56105 "Third-party site (mainly used in conjunction with other symbols).") holding fn call. with **any** _lt._; subtype[↓](https://cheats.rs/#type-conversions "On this site, below.") of above.|
|`fn(&'_ u8)`|Same; automatically expanded to type `for<'a> fn(&'a u8)`.|
|`fn(&u8)`|Same; automatically expanded to type `for<'a> fn(&'a u8)`.|
|`dyn for<'a> Fn(&'a u8)`|Higher-ranked (trait-object) type, works like `fn` above.|
|`dyn Fn(&'_ u8)`|Same; automatically expanded to type `dyn for<'a> Fn(&'a u8)`.|
|`dyn Fn(&u8)`|Same; automatically expanded to type `dyn for<'a> Fn(&'a u8)`.|

1 Yes, the `for<>` is part of the type, which is why you write `impl T for for<'a> fn(&'a u8)` below.

|Implementing Traits|Explanation|
|---|---|
|`impl<'a> T for fn(&'a u8) {}`|For fn. pointer, where call accepts **specific** _lt._ `'a`, impl trait `T`.|
|`impl T for for<'a> fn(&'a u8) {}`|For fn. pointer, where call accepts **any** _lt._, impl trait `T`.|
|`impl T for fn(&u8) {}`|Same, short version.|

### Strings & Chars

Rust has several ways to create textual values.

|Example|Explanation|
|---|---|
|`"..."`|**String literal**, [REF](https://doc.rust-lang.org/stable/reference/tokens.html#string-literals "See this topic in 'The Rust Reference'."), 1 UTF-8, will interpret the following escapes, …|
|`"\n\r\t\0\\"`|**Common escapes** [REF](https://doc.rust-lang.org/stable/reference/tokens.html#ascii-escapes "See this topic in 'The Rust Reference'."), e.g., `"\n"` becomes _new line_.|
|`"\x36"`|**ASCII _e._** [REF](https://doc.rust-lang.org/stable/reference/tokens.html#ascii-escapes "See this topic in 'The Rust Reference'.") up to `7f`, e.g., `"\x36"` would become `6`.|
|`"\u{7fff}"`|**Unicode _e._** [REF](https://doc.rust-lang.org/stable/reference/tokens.html#unicode-escapes "See this topic in 'The Rust Reference'.") up to 6 digits, e.g., `"\u{7fff}"` becomes `翿`.|
|`r"..."`|**Raw string literal**. [REF](https://doc.rust-lang.org/stable/reference/tokens.html#raw-string-literals "See this topic in 'The Rust Reference'."), 1UTF-8, but won't interpret any escape above.|
|`r#"..."#`|Raw string literal, UTF-8, but can also contain `"`. Number of `#` can vary.|
|`b"..."`|**Byte string literal**; [REF](https://doc.rust-lang.org/stable/reference/tokens.html#byte-and-byte-string-literals "See this topic in 'The Rust Reference'."), 1 constructs ASCII `[u8]`, not a string.|
|`br"..."`, `br#"..."#`|Raw byte string literal, ASCII `[u8]`, combination of the above.|
|`'🦀'`|**Character literal**, [REF](https://doc.rust-lang.org/stable/reference/tokens.html#character-and-string-literals "See this topic in 'The Rust Reference'.") fixed 4 byte unicode '**char**'. [STD](https://doc.rust-lang.org/std/primitive.char.html "See this topic in 'The Rust Standard Library'.")|
|`b'x'`|ASCII **byte literal**, [REF](https://doc.rust-lang.org/stable/reference/tokens.html#byte-literals "See this topic in 'The Rust Reference'.") a single `u8` byte.|

1 Supports multiple lines out of the box. Just keep in mind `Debug`[↓](https://cheats.rs/#string-output "On this site, below.") (e.g., `dbg!(x)` and `println!("{x:?}")`) might render them as `\n`, while `Display`[↓](https://cheats.rs/#string-output "On this site, below.") (e.g., `println!("{x}")`) renders them _proper_.

### Documentation

Debuggers hate him. Avoid bugs with this one weird trick.

|Example|Explanation|
|---|---|
|`///`|Outer line **doc comment**,1 [BK](https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html#making-useful-documentation-comments "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/meta/doc.html#documentation "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/comments.html#doc-comments "See this topic in 'The Rust Reference'.") use these on ty., traits, fn's, …|
|`//!`|Inner line doc comment, mostly used at top of file.|
|`//`|Line comment, use these to document code flow or _internals_.|
|`/* … */`|Block comment. 2 🗑️|
|`/** … */`|Outer block doc comment. 2 🗑️|
|`/*! … */`|Inner block doc comment. 2 🗑️|

1 [Tooling Directives](https://cheats.rs/#tooling-directives) outline what you can do inside doc comments.  
2 Generally discouraged due to bad UX. If possible use equivalent line comment instead with IDE support.

### Miscellaneous

These sigils did not fit any other category but are good to know nonetheless.

|Example|Explanation|
|---|---|
|`!`|Always empty **never type**. [BK](https://doc.rust-lang.org/book/ch19-04-advanced-types.html#the-never-type-that-never-returns "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/fn/diverging.html#diverging-functions "See this topic in 'Rust by Example'.") [STD](https://doc.rust-lang.org/std/primitive.never.html "See this topic in 'The Rust Standard Library'.") [REF](https://doc.rust-lang.org/stable/reference/types.html#never-type "See this topic in 'The Rust Reference'.")|
|`fn f() -> ! {}`|Function that never ret.; compat. with any _ty._ e.g., `let x: u8 = f();`|
|`fn f() -> Result<(), !> {}`|Function that must return `Result` but signals it can never `Err`. 🚧|
|`fn f(x: !) {}`|Function that exists, but can never be called. Not very useful. 🝖 🚧|
|`_`|Unnamed **wildcard** [REF](https://doc.rust-lang.org/stable/reference/patterns.html#wildcard-pattern "See this topic in 'The Rust Reference'.") variable binding, e.g., `\|x, _\| {}`.|
|`let _ = x;`|Unnamed assign. is no-op, does **not** 🛑 move out `x` or preserve scope!|
|`_ = x;`|You can assign _anything_ to `_` without `let`, i.e., `_ = ignore_rval();` 🔥|
|`_x`|Variable binding that won't emit _unused variable_ warnings.|
|`1_234_567`|Numeric separator for visual clarity.|
|`1_u8`|Type specifier for **numeric literals** [EX](https://doc.rust-lang.org/stable/rust-by-example/types/literals.html#literals "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/tokens.html#number-literals "See this topic in 'The Rust Reference'.") (also `i8`, `u16`, …).|
|`0xBEEF`, `0o777`, `0b1001`|Hexadecimal (`0x`), octal (`0o`) and binary (`0b`) integer literals.|
|`r#foo`|A **raw identifier** [BK](https://doc.rust-lang.org/book/appendix-01-keywords.html#raw-identifiers "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/compatibility/raw_identifiers.html#raw-identifiers "See this topic in 'Rust by Example'.") for edition compatibility. 🝖|
|`x;`|**Statement** [REF](https://doc.rust-lang.org/stable/reference/statements.html "See this topic in 'The Rust Reference'.") terminator, _c_. **expressions** [EX](https://doc.rust-lang.org/stable/rust-by-example/expression.html "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/expressions.html "See this topic in 'The Rust Reference'.")|

### Common Operators

Rust supports most operators you would expect (`+`, `*`, `%`, `=`, `==`, …), including **overloading**. [STD](https://doc.rust-lang.org/std/ops/index.html "See this topic in 'The Rust Standard Library'.") Since they behave no differently in Rust we do not list them here.

---

# Behind the Scenes

Arcane knowledge that may do terrible things to your mind, highly recommended.

## The Abstract Machine

Like `C` and `C++`, Rust is based on an _abstract machine_.

 **Overview**

Rust `→` CPU  
🛑 Misleading. Rust `→` Abstract Machine `→` CPU  
Correct.

With rare exceptions you are never 'allowed to reason' about the actual CPU. You write code for an _abstracted_ CPU. Rust then (sort of) understands what you want, and translates that into actual RISC-V / x86 / … machine code.

This _abstract machine_

- is not a runtime, and does not have any runtime overhead, but is a _computing model abstraction_,
- contains concepts such as memory regions (_stack_, …), execution semantics, …
- _knows_ and _sees_ things your CPU might not care about,
- is de-facto a contract between you and the compiler,
- and **exploits all of the above for optimizations**.

 **Misconceptions**

On the left things people may incorrectly assume they _should get away with_ if Rust targeted CPU directly. On the right things you'd interfere with if in reality if you violate the AM contract.

|Without AM|With AM|
|---|---|
|`0xffff_ffff` would make a valid `char`. 🛑|AM may exploit _'invalid'_ bit patterns to pack unrelated data.|
|`0xff` and `0xff` are same pointer. 🛑|AM pointers can have _'domain'_ attached for optimization.|
|Any r/w on pointer `0xff` always fine. 🛑|AM may issue cache-friendly ops since _'no read possible'_.|
|Reading un-init just gives random value. 🛑|AM _'knows'_ read impossible, may remove all related code.|
|Data race just gives random value. 🛑|AM may split R/W, produce _impossible_ value, see above.|
|Null ref. is just `0x0` in some register. 🛑|Holding `0x0` in reference summons Cthulhu.|

> This table is only to outline what the AM does. Unlike C or C++, Rust never lets you do the wrong thing unless you force it with `unsafe`. [↓](https://cheats.rs/#unsafe-unsound-undefined "On this site, below.")

## Language Sugar

If something works that "shouldn't work now that you think about it", it might be due to one of these.

|Name|Description|
|---|---|
|**Coercions** [NOM](https://doc.rust-lang.org/nightly/nomicon/coercions.html "See this topic in 'The Rustonomicon'.")|_Weakens_ types to match signature, e.g., `&mut T` to `&T`; _c_. _type conv._ [↓](https://cheats.rs/#type-conversions "On this site, below.")|
|**Deref** [NOM](https://doc.rust-lang.org/nightly/nomicon/vec-deref.html "See this topic in 'The Rustonomicon'.") [🔗](https://stackoverflow.com/questions/28519997/what-are-rusts-exact-auto-dereferencing-rules "Third-party site (mainly used in conjunction with other symbols).")|[Derefs](https://doc.rust-lang.org/std/ops/trait.Deref.html) `x: T` until `*x`, `**x`, … compatible with some target `S`.|
|**Prelude** [STD](https://doc.rust-lang.org/std/prelude/index.html "See this topic in 'The Rust Standard Library'.")|Automatic import of basic items, e.g., `Option`, `drop()`, …|
|**Reborrow** [🔗](https://quinedot.github.io/rust-learning/st-reborrow.html "Third-party site (mainly used in conjunction with other symbols).")|Since `x: &mut T` can't be copied; moves new `&mut *x` instead.|
|**Lifetime Elision** [BK](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision "See this topic in 'The Rust Programming Language'.") [NOM](https://doc.rust-lang.org/nightly/nomicon/lifetime-elision.html#lifetime-elision "See this topic in 'The Rustonomicon'.") [REF](https://doc.rust-lang.org/stable/reference/lifetime-elision.html#lifetime-elision "See this topic in 'The Rust Reference'.")|Allows you to write `f(x: &T)`, instead of `f<'a>(x: &'a T)`, for brevity.|
|**Lifetime Extensions** [🔗](https://blog.m-ou.se/super-let/ "Third-party site (mainly used in conjunction with other symbols).") [REF](https://doc.rust-lang.org/stable/reference/destructors.html#temporary-lifetime-extension "See this topic in 'The Rust Reference'.")|In `let x = &tmp().f` and similar hold on to temporary past line.|
|**Method Resolution** [REF](https://doc.rust-lang.org/stable/reference/expressions/method-call-expr.html "See this topic in 'The Rust Reference'.")|Derefs or borrow `x` until `x.f()` works.|
|**Match Ergonomics** [RFC](https://rust-lang.github.io/rfcs/2005-match-ergonomics.html "See this topic in the official RFC documents.")|Repeatedly deref. [scrutinee](https://doc.rust-lang.org/stable/reference/glossary.html#scrutinee) and adds `ref` and `ref mut` to bindings.|
|**Rvalue Static Promotion** [RFC](https://rust-lang.github.io/rfcs/1414-rvalue_static_promotion.html "See this topic in the official RFC documents.") 🝖|Makes refs. to constants `'static`, e.g., `&42`, `&None`, `&mut []`.|
|**Dual Definitions** [RFC](https://rust-lang.github.io/rfcs/1506-adt-kinds.html#tuple-structs "See this topic in the official RFC documents.") 🝖|Defining one (e.g., `struct S(u8)`) implicitly def. another (e.g., `fn S`).|

> **Opinion** 💬 — These features make your life easier _using_ Rust, but stand in the way of _learning_ it. If you want to develop a _genuine understanding_, spend some extra time exploring them.

## Memory & Lifetimes

An illustrated guide to moves, references and lifetimes.

 **Types & Moves**

  Application Memory S(1) `t` Variables S(1) `a` `t` Moves M { … } ⛔ `c` Type Safety

S(1)▼ S(2)▼ S(3) `t` Scope & Drop

 **Call Stack**

S(1) `a` `x` Function Boundaries S(1) `a` `x` `x` Nested Functions

S(1) M { } `a` `x` `m` Repurposing Memory

 **References & Pointers**

 ▼

S(1) 0x3 `a` `r` References as Pointers  ▼ S(2) 0x3 S(1) `a` `r` `d` Access to Non-Owned Memory  ▼ 0x3 M { x } ⛔ ⛔ `a` `r` `d` References Guard Referents  ▼

0x3 `p` Raw Pointers

 **Lifetime Basics**

"Lifetime" of Things  ▼ S(2) 0xa `c` `r` Meaning of `r: &'c S`  ▼ S(0) S(3) S(2) 0x6 ⛔ `a` `b` `c` `r` Typelikeness of Lifetimes  ▼

  0x6 S(4) ⛔ `b` Borrowed State

 **Lifetimes in Functions**

S(1) S(2) ? 0x6 0xa `b` `c` `r` `x` `y` Function Parameters S(1) S(2) ? `a` `b` `c` `r` Problem of 'Borrowed' Propagation  ▼ S(1) S(2) y + _ `a` `b` `c` `r` Lifetimes Propagate Borrowed State

S(2) `a` `c` Unlocking

 **Advanced 🝖**

 ▼  ▼  

S(1) 0x2 0x6 0x2 `a` `ra` `rb` `rval` References to References

S(1)▼ `_` Drop and `_`

↕️ Examples expand by clicking.

---

# Memory Layout

Byte representations of common types.

## Basic Types

Essential types built into the core of the language.

#### Boolean [REF](https://doc.rust-lang.org/stable/reference/types/boolean.html "See this topic in 'The Rust Reference'.") and Numeric Types [REF](https://doc.rust-lang.org/stable/reference/types/numeric.html "See this topic in 'The Rust Reference'.")

`bool` `u8`, `i8` `u16`, `i16` `u32`, `i32` `u64`, `i64` `u128`, `i128` `f32` `f64` `usize`, `isize` Same as `ptr` on platform.  

 **Unsigned Types**

|Type|Max Value|
|---|---|
|`u8`|`255`|
|`u16`|`65_535`|
|`u32`|`4_294_967_295`|
|`u64`|`18_446_744_073_709_551_615`|
|`u128`|`340_282_366_920_938_463_463_374_607_431_768_211_455`|
|`usize`|Depending on platform pointer size, same as `u16`, `u32`, or `u64`.|

 **Signed Types**

|Type|Max Value|
|---|---|
|`i8`|`127`|
|`i16`|`32_767`|
|`i32`|`2_147_483_647`|
|`i64`|`9_223_372_036_854_775_807`|
|`i128`|`170_141_183_460_469_231_731_687_303_715_884_105_727`|
|`isize`|Depending on platform pointer size, same as `i16`, `i32`, or `i64`.|

|Type|Min Value|
|---|---|
|`i8`|`-128`|
|`i16`|`-32_768`|
|`i32`|`-2_147_483_648`|
|`i64`|`-9_223_372_036_854_775_808`|
|`i128`|`-170_141_183_460_469_231_731_687_303_715_884_105_728`|
|`isize`|Depending on platform pointer size, same as `i16`, `i32`, or `i64`.|

 **Float Types**🝖

Sample bit representation* for a `f32`:

`S` `E` `E` `E` `E` `E` `E` `E` `E` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F` `F`

Explanation:

|f32|S (1)|E (8)|F (23)|Value|
|---|---|---|---|---|
|Normalized number|±|1 to 254|any|±(1.F)2 * 2E-127|
|Denormalized number|±|0|non-zero|±(0.F)2 * 2-126|
|Zero|±|0|0|±0|
|Infinity|±|255|0|±∞|
|NaN|±|255|non-zero|NaN|

Similarly, for `f64` types this would look like:

|f64|S (1)|E (11)|F (52)|Value|
|---|---|---|---|---|
|Normalized number|±|1 to 2046|any|±(1.F)2 * 2E-1023|
|Denormalized number|±|0|non-zero|±(0.F)2 * 2-1022|
|Zero|±|0|0|±0|
|Infinity|±|2047|0|±∞|
|NaN|±|2047|non-zero|NaN|

* Float types follow [IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_754-2008_revision) and depend on platform endianness.

 **Casting Pitfalls** 🛑

|Cast1|Gives|Note|
|---|---|---|
|`3.9_f32 as u8`|`3`|Truncates, consider `x.round()` first.|
|`314_f32 as u8`|`255`|Takes closest available number.|
|`f32::INFINITY as u8`|`255`|Same, treats `INFINITY` as _really_ large number.|
|`f32::NAN as u8`|`0`|-|
|`_314 as u8`|`58`|Truncates excess bits.|
|`_257 as i8`|`1`|Truncates excess bits.|
|`_200 as i8`|`-56`|Truncates excess bits, MSB might then also signal negative.|

 **Arithmetic Pitfalls** 🛑

|Operation1|Gives|Note|
|---|---|---|
|`200_u8 / 0_u8`|Compile error.|-|
|`200_u8 / _0` d, r|Panic.|Regular math may panic; here: division by zero.|
|`200_u8 + 200_u8`|Compile error.|-|
|`200_u8 + _200` d|Panic.|Consider `checked_`, `wrapping_`, … instead. [STD](https://doc.rust-lang.org/std/primitive.isize.html#method.checked_add "See this topic in 'The Rust Standard Library'.")|
|`200_u8 + _200` r|`144`|In release mode this will overflow.|
|`-128_i8 * -1`|Compile error.|Would overflow (`128_i8` doesn't exist).|
|`-128_i8 * _1neg` d|Panic.|-|
|`-128_i8 * _1neg` r|`-128`|Overflows back to `-128` in release mode.|
|`1_u8 / 2_u8`|`0`|Other integer division truncates.|
|`0.8_f32 + 0.1_f32`|`0.90000004`|-|
|`1.0_f32 / 0.0_f32`|`f32::INFINITY`|-|
|`0.0_f32 / 0.0_f32`|`f32::NAN`|-|
|`x < f32::NAN`|`false`|`NAN` comparisons always return false.|
|`x > f32::NAN`|`false`|`NAN` comparisons always return false.|
|`f32::NAN == f32::NAN`|`false`|Use `f32::is_nan()` [STD](https://doc.rust-lang.org/std/primitive.f32.html#method.is_nan "See this topic in 'The Rust Standard Library'.") instead.|

1 Expression `_100` means anything that might contain the value `100`, e.g., `100_i32`, but is opaque to compiler.  
d Debug build.  
r Release build.  

#### Textual Types [REF](https://doc.rust-lang.org/stable/reference/types/textual.html "See this topic in 'The Rust Reference'.")

`char` Any Unicode scalar. `str` … `U` `T` `F` `-` `8` … unspecified times Rarely seen alone, but as `&str` instead.

 **Basics**

|Type|Description|
|---|---|
|`char`|Always 4 bytes and only holds a single Unicode **scalar value** [🔗](https://www.unicode.org/glossary/#unicode_scalar_value "Third-party site (mainly used in conjunction with other symbols).").|
|`str`|An `u8`-array of unknown length guaranteed to hold **UTF-8 encoded code points**.|

 **Usage**

|Chars|Description|
|---|---|
|`let c = 'a';`|Often a `char` (unicode scalar) can coincide with your intuition of _character_.|
|`let c = '❤';`|It can also hold many Unicode symbols.|
|`let c = '❤️';`|But not always. Given emoji is **two** `char` (see Encoding) and **can't** 🛑 be held by `c`.1|
|`c = 0xffff_ffff;`|Also, chars are **not allowed** 🛑 to hold arbitrary bit patterns.|

1 Fun fact, due to the [Zero-width joiner](https://en.wikipedia.org/wiki/Zero-width_joiner) (⨝) what the user _perceives as a character_ can get even more unpredictable: 👨‍👩‍👧 is in fact 5 chars 👨⨝👩⨝👧, and rendering engines are free to either show them fused as one, or separately as three, depending on their abilities.

|Strings|Description|
|---|---|
|`let s = "a";`|A `str` is usually never held directly, but as `&str`, like `s` here.|
|`let s = "❤❤️";`|It can hold arbitrary text, has variable length per _c._, and is hard to index.|

 **Encoding**🝖

`let s = "I ❤ Rust";`  
`let t = "I ❤️ Rust";`

|Variant|Memory Representation2|
|---|---|
|`s.as_bytes()`|`49` `20` **`e2 9d a4`** `20 52 75 73 74` 3|
|`s.chars()`1|`49 00 00 00 20 00 00 00` **`64 27 00 00`** `20 00 00 00 52 00 00 00 75 00 00 00 73 00` …|
|`t.as_bytes()`|`49` `20` **`e2 9d a4`** **`ef b8 8f`** `20 52 75 73 74` 4|
|`t.chars()`1|`49 00 00 00 20 00 00 00` **`64 27 00 00`** **`0f fe 01 00`** `20 00 00 00 52 00 00 00 75 00` …|

1 Result then collected into array and transmuted to bytes.  
2 Values given in hex, on x86.  
3 Notice how `❤`, having [Unicode Code Point (U+2764)](https://codepoints.net/U+2764), is represented as **64 27 00 00** inside the `char`, but got [UTF-8 encoded to](https://en.wikipedia.org/wiki/UTF-8#Description) **e2 9d a4** in the `str`.  
4 Also observe how the emoji [Red Heart `❤️`](https://emojipedia.org/red-heart/), is a combination of `❤` and the [U+FE0F Variation Selector](https://codepoints.net/U+FE0F), thus `t` has a higher char count than `s`.

> 💬 For what seem to be browser bugs Safari and Edge render the hearts in Footnote 3 and 4 wrong, despite being able to differentiate them correctly in `s` and `t` above.

## Custom Types

Basic types definable by users. Actual **layout** [REF](https://doc.rust-lang.org/stable/reference/type-layout.html "See this topic in 'The Rust Reference'.") is subject to **representation**; [REF](https://doc.rust-lang.org/stable/reference/type-layout.html#representations "See this topic in 'The Rust Reference'.") padding can be present.

`T` `T` Sized [↓](https://cheats.rs/#sized-types "On this site, below.") `T: ?Sized` `T` Maybe DST [↓](https://cheats.rs/#sized-types "On this site, below.") `[T; n]` `T` `T` `T` … n times Fixed array of `n` elements. `[T]` … `T` `T` `T` … unspecified times **Slice type** of unknown-many elements. Neither  
`Sized` (nor carries `len` information), and most  
often lives behind reference as `&[T]`. [↓](https://cheats.rs/#references-pointers-ui "On this site, below.") `struct S;` Zero-Sized [↓](https://cheats.rs/#sized-types "On this site, below.") `(A, B, C)` `A` `B` `C` or maybe `B` `A` `C` Unless a representation is forced  
(e.g., via `#[repr(C)]`), type layout  
unspecified. `struct S { b: B, c: C }` `B` `C` or maybe `C` `↦` `B` Compiler may also add padding.

> Also note, two types `A(X, Y)` and `B(X, Y)` with exactly the same fields can still have differing layout; never `transmute()` [STD](https://doc.rust-lang.org/std/mem/fn.transmute.html "See this topic in 'The Rust Standard Library'.") without representation guarantees.

These **sum types** hold a value of one of their sub types:

`enum E { A, B, C }` `Tag` `A` exclusive or `Tag` `B` exclusive or `Tag` `C` Safely holds A or B or C, also  
called 'tagged union', though  
compiler may squeeze tag  
into 'unused' bits. `union { … }` `A` unsafe or `B` unsafe or `C` Can unsafely reinterpret  
memory. Result might  
be undefined.

## References & Pointers

References give safe access to 3rd party memory, raw pointers `unsafe` access. The corresponding `mut` types have an identical data layout to their immutable counterparts.

`&'a T` `ptr`2/4/8 `meta`2/4/8 | `T` Must target some valid `t` of `T`,  
and any such target must exist for  
at least `'a`. `*const T` `ptr`2/4/8 `meta`2/4/8 No guarantees.  

### Pointer Meta

Many reference and pointer types can carry an extra field, **pointer metadata**. [STD](https://doc.rust-lang.org/nightly/std/ptr/trait.Pointee.html#pointer-metadata "See this topic in 'The Rust Standard Library'.") It can be the element- or byte-length of the target, or a pointer to a _vtable_. Pointers with meta are called **fat**, otherwise **thin**.

`&'a T` `ptr`2/4/8 | `T` No meta for  
sized target.  
(pointer is thin). `&'a T` `ptr`2/4/8 `len`2/4/8 | `T` If `T` is a DST `struct` such as  
`S { x: [u8] }` meta field `len` is  
count of dyn. sized content. `&'a [T]` `ptr`2/4/8 `len`2/4/8 | … `T` `T` … Regular **slice reference** (i.e., the  
reference type of a slice type `[T]`) [↑](https://cheats.rs/#custom-types "On this site, above.")  
often seen as `&[T]` if `'a` elided. `&'a str` `ptr`2/4/8 `len`2/4/8 | … `U` `T` `F` `-` `8` … **String slice reference** (i.e., the  
reference type of string type `str`),  
with meta `len` being byte length.  
`&'a dyn Trait` `ptr`2/4/8 `ptr`2/4/8 | `T` |

|   |
|---|
|`*Drop::drop(&mut T)`|
|`size`|
|`align`|
|`*Trait::f(&T, …)`|
|`*Trait::g(&T, …)`|

Meta points to vtable, where `*Drop::drop()`, `*Trait::f()`, … are pointers to their respective `impl` for `T`.

## Closures

Ad-hoc functions with an automatically managed data block **capturing** [REF](https://doc.rust-lang.org/stable/reference/types/closure.html#capture-modes "See this topic in 'The Rust Reference'."), 1 environment where closure was defined. For example, if you had:

```rust
let y = ...;
let z = ...;

with_closure(move |x| x + y.f() + z); // y and z are moved into closure instance (of type C1)
with_closure(     |x| x + y.f() + z); // y and z are pointed at from closure instance (of type C2)
```

Then the generated, anonymous closures types `C1` and `C2` passed to `with_closure()` would look like:

`move |x| x + y.f() + z` `Y` `Z` Anonymous closure type C1 `|x| x + y.f() + z` `ptr`2/4/8 `ptr`2/4/8 Anonymous closure type C2 | `Y` | `Z`

> Also produces anonymous `fn` such as `fc1(C1, X)` or `fc2(&C2, X)`. Details depend on which `FnOnce`, `FnMut`, `Fn` ... is supported, based on properties of captured types.

1 A bit oversimplified a closure is a convenient-to-write 'mini function' that accepts parameters _but also_ needs some local variables to do its job. It is therefore a type (containing the needed locals) and a function. _'Capturing the environment'_ is a fancy way of saying that and how the closure type holds on to these locals, either _by moved value_, or _by pointer_. See **Closures in APIs** [↓](https://cheats.rs/#closures-in-apis "On this site, below.") for various implications.

## Standard Library Types

Rust's standard library combines the above primitive types into useful types with special semantics, e.g.:

`UnsafeCell<T>` [STD](https://doc.rust-lang.org/std/cell/struct.UnsafeCell.html "See this topic in 'The Rust Standard Library'.") `T` Magic type allowing  
aliased mutability. `Cell<T>` [STD](https://doc.rust-lang.org/std/cell/struct.Cell.html "See this topic in 'The Rust Standard Library'.") `T` Allows `T`'s  
to move in  
and out. `RefCell<T>` [STD](https://doc.rust-lang.org/std/cell/struct.RefCell.html "See this topic in 'The Rust Standard Library'.") `borrowed` `T` Also support dynamic  
borrowing of `T`. Like `Cell` this  
is `Send`, but not `Sync`. `ManuallyDrop<T>` [STD](https://doc.rust-lang.org/std/mem/struct.ManuallyDrop.html "See this topic in 'The Rust Standard Library'.") `T` Prevents `T::drop()` from  
being called. `AtomicUsize` [STD](https://doc.rust-lang.org/std/sync/atomic/index.html "See this topic in 'The Rust Standard Library'.") `usize`2/4/8 Other atomic similarly. `Option<T>` [STD](https://doc.rust-lang.org/std/option/enum.Option.html "See this topic in 'The Rust Standard Library'.") `Tag` or `Tag` `T` Tag may be omitted for  
certain T, e.g., `NonNull`.[STD](https://doc.rust-lang.org/std/ptr/struct.NonNull.html "See this topic in 'The Rust Standard Library'.") `Result<T, E>` [STD](https://doc.rust-lang.org/std/result/enum.Result.html "See this topic in 'The Rust Standard Library'.") `Tag` `E` or `Tag` `T` Either some error `E` or value  
of `T`. `MaybeUninit<T>` [STD](https://doc.rust-lang.org/std/mem/union.MaybeUninit.html "See this topic in 'The Rust Standard Library'.") `U̼̟̔͛n̥͕͐͞d̛̲͔̦̳̑̓̐e̱͎͒̌fị̱͕̈̉͋ne̻̅ḓ̓` unsafe or `T` Uninitialized memory or  
some `T`. Only legal way  
to work with uninit data.

#### Order-Preserving Collections

`Box<T>` [STD](https://doc.rust-lang.org/std/boxed/struct.Box.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `meta`2/4/8 | `T` For some `T` stack proxy may carry  
meta[↑](https://cheats.rs/#custom-types "On this site, above.") (e.g., `Box<[T]>`). `Vec<T>` [STD](https://doc.rust-lang.org/std/vec/struct.Vec.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `capacity`2/4/8 `len`2/4/8 |

`T` `T` … len

← capacity → Regular _growable array_ vector of single type. `LinkedList<T>` [STD](https://doc.rust-lang.org/std/collections/struct.LinkedList.html "See this topic in 'The Rust Standard Library'.")🝖 `head`2/4/8 `tail`2/4/8 `len`2/4/8 | | `next`2/4/8 `prev`2/4/8 `T` Elements `head` and `tail` both `null` or point to nodes on  
the heap. Each node can point to its `prev` and `next` node.  
Eats your cache (just look at the thing!); don't use unless  
you evidently must. 🛑 `VecDeque<T>` [STD](https://doc.rust-lang.org/std/collections/struct.VecDeque.html "See this topic in 'The Rust Standard Library'.") `head`2/4/8 `len`2/4/8 `ptr`2/4/8 `capacity`2/4/8 |

`T` … empty … `T⁣H`

← capacity → Index `head` selects in array-as-ringbuffer. This means content may be  
non-contiguous and empty in the middle, as exemplified above.

#### Other Collections

`HashMap<K, V>` [STD](https://doc.rust-lang.org/std/collections/struct.HashMap.html "See this topic in 'The Rust Standard Library'.") `bmask`2/4/8 `ctrl`2/4/8 `left`2/4/8 `len`2/4/8 | `K:V` `K:V` … `K:V` … `K:V` Oversimplified! Stores keys and values on heap according to hash value, [SwissTable](https://www.youtube.com/watch?v=ncHmEUmJZf4)  
implementation via [hashbrown](https://github.com/rust-lang/hashbrown). `HashSet` [STD](https://doc.rust-lang.org/std/collections/struct.HashSet.html "See this topic in 'The Rust Standard Library'.") identical to `HashMap`,  
just type `V` disappears. Heap view grossly oversimplified. 🛑 `BinaryHeap<T>` [STD](https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `capacity`2/4/8 `len`2/4/8 |

`T⁣0` `T⁣1` `T⁣1` `T⁣2` `T⁣2` … len

← capacity → Heap stored as array with `2N` elements per layer. Each `T`  
can have 2 children in layer below. Each `T` larger than its  
children.

#### Owned Strings

`String` [STD](https://doc.rust-lang.org/std/string/struct.String.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `capacity`2/4/8 `len`2/4/8 |

`U` `T` `F` `-` `8` … len

← capacity → Observe how `String` differs from `&str` and `&[char]`. `CString` [STD](https://doc.rust-lang.org/std/ffi/struct.CString.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `len`2/4/8 |

`A` `B` `C` … len … `∅`

NUL-terminated but w/o NUL in middle. `OsString` [STD](https://doc.rust-lang.org/std/ffi/struct.OsString.html "See this topic in 'The Rust Standard Library'.") Platform Defined |

/

Encapsulates how operating system  
represents strings (e.g., [WTF-8](https://simonsapin.github.io/wtf-8/) on  
Windows). `PathBuf` [STD](https://doc.rust-lang.org/std/path/struct.PathBuf.html "See this topic in 'The Rust Standard Library'.") `OsString` |

/

Encapsulates how operating system  
represents paths.

#### Shared Ownership

If the type does not contain a `Cell` for `T`, these are often combined with one of the `Cell` types above to allow shared de-facto mutability.

`Rc<T>` [STD](https://doc.rust-lang.org/std/rc/struct.Rc.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `meta`2/4/8

| `strng`2/4/8 `weak`2/4/8 `T`

Share ownership of `T` in same thread. Needs nested `Cell`  
or `RefCell`to allow mutation. Is neither `Send` nor `Sync`. `Arc<T>` [STD](https://doc.rust-lang.org/std/sync/struct.Arc.html "See this topic in 'The Rust Standard Library'.") `ptr`2/4/8 `meta`2/4/8

| `strng`2/4/8 `weak`2/4/8 `T`

Same, but allow sharing between threads IF contained  
`T` itself is `Send` and `Sync`.  
`Mutex<T>` [STD](https://doc.rust-lang.org/std/sync/struct.Mutex.html "See this topic in 'The Rust Standard Library'.") / `RwLock<T>` [STD](https://doc.rust-lang.org/std/sync/struct.RwLock.html "See this topic in 'The Rust Standard Library'.") `inner` `poison`2/4/8 `T` Inner fields depend on platform. Needs to be  
held in `Arc` to be shared between decoupled  
threads, or via `scope()` [STD](https://doc.rust-lang.org/std/thread/fn.scope.html "See this topic in 'The Rust Standard Library'.") for scoped threads. `Cow<'a, T>` [STD](https://doc.rust-lang.org/std/borrow/enum.Cow.html "See this topic in 'The Rust Standard Library'.") `Tag` `T::Owned` or `Tag` `ptr`2/4/8

| `T`

Holds read-only reference to  
some `T`, or owns it's `ToOwned` [STD](https://doc.rust-lang.org/std/borrow/trait.ToOwned.html "See this topic in 'The Rust Standard Library'.")  
analog.

---

# Standard Library

## One-Liners

Snippets that are common, but still easy to forget. See **Rust Cookbook** [🔗](https://rust-lang-nursery.github.io/rust-cookbook/ "Third-party site (mainly used in conjunction with other symbols).") for more.

 **Strings**

|Intent|Snippet|
|---|---|
|Concatenate strings (any `Display`[↓](https://cheats.rs/#string-output "On this site, below.") that is). 1 '21|`format!("{x}{y}")`|
|Append string (any `Display` to any `Write`). '21|`write!(x, "{y}")`|
|Split by separator pattern. [STD](https://doc.rust-lang.org/std/str/pattern/trait.Pattern.html "See this topic in 'The Rust Standard Library'.") [🔗](https://stackoverflow.com/a/38138985 "Third-party site (mainly used in conjunction with other symbols).")|`s.split(pattern)`|
|… with `&str`|`s.split("abc")`|
|… with `char`|`s.split('/')`|
|… with closure|`s.split(char::is_numeric)`|
|Split by whitespace.|`s.split_whitespace()`|
|Split by newlines.|`s.lines()`|
|Split by regular expression.2|`Regex::new(r"\s")?.split("one two three")`|

1 Allocates; if `x` or `y` are not going to be used afterwards consider using `write!` or `std::ops::Add`.  
2 Requires [regex](https://crates.io/crates/regex) crate.

 **I/O**

|Intent|Snippet|
|---|---|
|Create a new file|`File::create(PATH)?`|
|Same, via OpenOptions|`OpenOptions::new().create(true).write(true).truncate(true).open(PATH)?`|

 **Macros**

|Intent|Snippet|
|---|---|
|Macro w. variable arguments|`macro_rules! var_args { ($($args:expr),*) => {{ }} }`|
|Using `args`, e.g., calling `f` multiple times.|`$( f($args); )*`|

 **Transforms 🔥**

|Starting Type|Resource|
|---|---|
|`Option<T> -> …`|See the [Type-Based Cheat Sheet](https://upsuper.github.io/rust-cheatsheet/)|
|`Result<T, R> -> …`|See the [Type-Based Cheat Sheet](https://upsuper.github.io/rust-cheatsheet/)|
|`Iterator<Item=T> -> …`|See the [Type-Based Cheat Sheet](https://upsuper.github.io/rust-cheatsheet/)|
|`&[T] -> …`|See the [Type-Based Cheat Sheet](https://upsuper.github.io/rust-cheatsheet/)|
|`Future<T> -> …`|See the [Futures Cheat Sheet](https://rufflewind.com/img/rust-futures-cheatsheet.html)|

 **Esoterics**🝖

|Intent|Snippet|
|---|---|
|Cleaner closure captures|`wants_closure({ let c = outer.clone(); move \| use_clone(c) })`|
|Fix inference in '`try`' closures|`iter.try_for_each(\|x\| { Ok::<(), Error>(()) })?;`|
|Iterate _and_ edit `&mut [T]` if `T` Copy.|`Cell::from_mut(mut_slice).as_slice_of_cells()`|
|Get subslice with length.|`&original_slice[offset..][..length]`|
|Canary so trait `T` is object safe.|`const _: Option<&dyn T> = None;`|

## Thread Safety

Assume you hold some variables in Thread 1, and want to either **move** them to Thread 2, or pass their **references** to Thread 3. Whether this is allowed is governed by **`Send`**[STD](https://doc.rust-lang.org/std/marker/trait.Send.html "See this topic in 'The Rust Standard Library'.") and **`Sync`**[STD](https://doc.rust-lang.org/std/marker/trait.Sync.html "See this topic in 'The Rust Standard Library'.") respectively:


|Example|Explanation|
|---|---|
|**`Mutex<u32>`**|Both `Send` and `Sync`. You can safely pass or lend it to another thread.|
|**`Cell<u32>`**|`Send`, not `Sync`. Movable, but its reference would allow concurrent non-atomic writes.|
|**`MutexGuard<u32>`**|`Sync`, but not `Send`. Lock tied to thread, but reference use could not allow data race.|
|**`Rc<u32>`**|Neither since it is easily clonable heap-proxy with non-atomic counters.|

|Trait|`Send`|`!Send`|
|---|---|---|
|`Sync`|_Most types_ … `Arc<T>`1,2, `Mutex<T>`2|`MutexGuard<T>`1, `RwLockReadGuard<T>`1|
|`!Sync`|`Cell<T>`2, `RefCell<T>`2|`Rc<T>`, `&dyn Trait`, `*const T`3|

1 If `T` is `Sync`.  
2 If `T` is `Send`.  
3 If you need to send a raw pointer, create newtype `struct Ptr(*const u8)` and `unsafe impl Send for Ptr {}`. Just ensure you _may_ send it.

## Iterators

Processing elements in a collection.

 **Basics**

There are, broadly speaking, four _styles_ of collection iteration:

|Style|Description|
|---|---|
|`for x in c { ... }`|_Imperative_, useful w. side effects, interdepend., or need to break flow early.|
|`c.iter().map().filter()`|_Functional_, often much cleaner when only results of interest.|
|`c_iter.next()`|_Low-level_, via explicit `Iterator::next()` [STD](https://doc.rust-lang.org/std/iter/trait.Iterator.html#tymethod.next "See this topic in 'The Rust Standard Library'.") invocation. 🝖|
|`c.get(n)`|_Manual_, bypassing official iteration machinery.|

> **Opinion** 💬 — Functional style is often easiest to follow, but don't hesitate to use `for` if your `.iter()` chain turns messy. When implementing containers iterator support would be ideal, but when in a hurry it can sometimes be more practical to just implement `.len()` and `.get()` and move on with your life.

 **Obtaining**

**Basics**

Assume you have a collection `c` of type `C` you want to use:

- **`c.into_iter()`**1 — Turns collection `c` into an **`Iterator`** [STD](https://doc.rust-lang.org/std/iter/trait.Iterator.html "See this topic in 'The Rust Standard Library'.") `i` and **consumes**2 `c`. _Std._ way to get iterator.
- **`c.iter()`** — Courtesy method **some** collections provide, returns **borrowing** Iterator, doesn't consume `c`.
- **`c.iter_mut()`** — Same, but **mutably borrowing** Iterator that allow collection to be changed.

**The Iterator**

Once you have an `i`:

- **`i.next()`** — Returns `Some(x)` next element `c` provides, or `None` if we're done.

**For Loops**

- **`for x in c {}`** — Syntactic sugar, calls `c.into_iter()` and loops `i` until `None`.

1 Requires **`IntoIterator`** [STD](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html "See this topic in 'The Rust Standard Library'.") for `C` to be implemented. Type of item depends on what `C` was.

2 If it looks as if it doesn't consume `c` that's because type was `Copy`. For example, if you call `(&c).into_iter()` it will invoke `.into_iter()` on `&c` (which will consume a _copy_ of the reference and turn it into an Iterator), but the original `c` remains untouched.

 **Creating**

**Essentials**

Let's assume you have a `struct Collection<T> {}` you authored. You should also implement:

- **`struct IntoIter<T> {}`** — Create a struct to hold your iteration status (e.g., an index) for value iteration.
- **`impl Iterator for IntoIter<T> {}`** — Implement `Iterator::next()` so it can produce elements.

`Collection<T>` `IntoIter<T>` ⌾ `Iterator` `Item = T;`

> At this point you have something that can behave as an **Iterator**, [STD](https://doc.rust-lang.org/std/iter/trait.Iterator.html "See this topic in 'The Rust Standard Library'.") but no way of actually obtaining it. See the next tab for how that usually works.

 **For Loops**

**Native Loop Support**

Many users would expect your collection to _just work_ in `for` loops. You need to implement:

- **`impl IntoIterator for Collection<T> {}`** — Now `for x in c {}` works.
- **`impl IntoIterator for &Collection<T> {}`** — Now `for x in &c {}` works.
- **`impl IntoIterator for &mut Collection<T> {}`** — Now `for x in &mut c {}` works.

`Collection<T>` ⌾ `IntoIterator` `Item = T;` `To = IntoIter<T>` Iterate over `T`. `&Collection<T>` ⌾ `IntoIterator` `Item = &T;` `To = Iter<T>` Iterate over `&T`. `&mut Collectn<T>` ⌾ `IntoIterator` `Item = &mut T;` `To = IterMut<T>` Iterate over `&mut T`.

> As you can see, the **IntoIterator** [STD](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html "See this topic in 'The Rust Standard Library'.") trait is what actually connects your collection with the **IntoIter** struct you created in the previous tab. The two siblings of **IntoIter** (**Iter** and **IterMut**) are discussed in the next tab.

 **Borrowing**

**Shared & Mutable Iterators**

In addition, if you want your collection to be useful when borrowed you should implement:

- **`struct Iter<T> {}`** — Create struct holding `&Collection<T>` state for shared iteration.
- **`struct IterMut<T> {}`** — Similar, but holding `&mut Collection<T>` state for mutable iteration.
- **`impl Iterator for Iter<T> {}`** — Implement shared iteration.
- **`impl Iterator for IterMut<T> {}`** — Implement mutable iteration.

Also you might want to add convenience methods:

- `Collection::iter(&self) -> Iter`,
- `Collection::iter_mut(&mut self) -> IterMut`.

`Iter<T>` ⌾ `Iterator` `Item = &T;` `IterMut<T>` ⌾ `Iterator` `Item = &mut T;`

> The code for borrowing interator support is basically just a repetition of the previous steps with a slightly different types, e.g., `&T` vs `T`.

 **Interoperability**

**Iterator Interoperability**

To allow **3rd party iterators** to 'collect into' your collection implement:

- **`impl FromIterator for Collection<T> {}`** — Now `some_iter.collect::<Collection<_>>()` works.
- **`impl Extend for Collection<T> {}`** — Now `c.extend(other)` works.

In addition, also consider adding the extra traits from **`std::iter`** [STD](https://doc.rust-lang.org/std/iter/index.html# "See this topic in 'The Rust Standard Library'.") to your previous structs:

`Collection<T>` ⌾ `FromIterator` ⌾ `Extend` `IntoIter<T>` ⌾ `DoubleEndedIt…` ⌾ `ExactSizeIt…` ⌾ `FusedIterator` `Iter<T>` ⌾ `DoubleEndedIt…` ⌾ `ExactSizeIt…` ⌾ `FusedIterator` `IterMut<T>` ⌾ `DoubleEndedIt…` ⌾ `ExactSizeIt…` ⌾ `FusedIterator`

> Writing collections can be work. The good news is, if you followed all these steps your collections will feel like _first class citizens_.

## Number Conversions

As-**correct**-as-it-currently-gets number conversions.

|↓ Have / Want →|`u8` … `i128`|`f32` / `f64`|String|
|---|---|---|---|
|`u8` … `i128`|`u8::try_from(x)?` 1|`x as f32` 3|`x.to_string()`|
|`f32` / `f64`|`x as u8` 2|`x as f32`|`x.to_string()`|
|`String`|`x.parse::<u8>()?`|`x.parse::<f32>()?`|`x`|

1 If type true subset `from()` works directly, e.g., `u32::from(my_u8)`.  
2 Truncating (`11.9_f32 as u8` gives `11`) and saturating (`1024_f32 as u8` gives `255`); _c_. below.  
3 Might misrepresent number (`u64::MAX as f32`) or produce `Inf` (`u128::MAX as f32`).

> Also see **Casting-** and **Arithmetic Pitfalls** [↑](https://cheats.rs/#numeric-types "On this site, above.") for more things that can go wrong working with numbers.

## String Conversions

If you **want** a string of type …

 `String`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`x`|
|`CString`|`x.into_string()?`|
|`OsString`|`x.to_str()?.to_string()`|
|`PathBuf`|`x.to_str()?.to_string()`|
|`Vec<u8>` 1|`String::from_utf8(x)?`|
|`&str`|`x.to_string()` `i`|
|`&CStr`|`x.to_str()?.to_string()`|
|`&OsStr`|`x.to_str()?.to_string()`|
|`&Path`|`x.to_str()?.to_string()`|
|`&[u8]` 1|`String::from_utf8_lossy(x).to_string()`|

 `CString`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`CString::new(x)?`|
|`CString`|`x`|
|`OsString`|`CString::new(x.to_str()?)?`|
|`PathBuf`|`CString::new(x.to_str()?)?`|
|`Vec<u8>` 1|`CString::new(x)?`|
|`&str`|`CString::new(x)?`|
|`&CStr`|`x.to_owned()` `i`|
|`&OsStr`|`CString::new(x.to_os_string().into_string()?)?`|
|`&Path`|`CString::new(x.to_str()?)?`|
|`&[u8]` 1|`CString::new(Vec::from(x))?`|
|`*mut c_char` 2|`unsafe { CString::from_raw(x) }`|

 `OsString`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`OsString::from(x)` `i`|
|`CString`|`OsString::from(x.to_str()?)`|
|`OsString`|`x`|
|`PathBuf`|`x.into_os_string()`|
|`Vec<u8>` 1|`unsafe { OsString::from_encoded_bytes_unchecked(x) }`|
|`&str`|`OsString::from(x)` `i`|
|`&CStr`|`OsString::from(x.to_str()?)`|
|`&OsStr`|`OsString::from(x)` `i`|
|`&Path`|`x.as_os_str().to_owned()`|
|`&[u8]` 1|`unsafe { OsString::from_encoded_bytes_unchecked(x.to_vec()) }`|

 `PathBuf`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`PathBuf::from(x)` `i`|
|`CString`|`PathBuf::from(x.to_str()?)`|
|`OsString`|`PathBuf::from(x)` `i`|
|`PathBuf`|`x`|
|`Vec<u8>` 1|`unsafe { PathBuf::from(OsString::from_encoded_bytes_unchecked(x)) }`|
|`&str`|`PathBuf::from(x)` `i`|
|`&CStr`|`PathBuf::from(x.to_str()?)`|
|`&OsStr`|`PathBuf::from(x)` `i`|
|`&Path`|`PathBuf::from(x)` `i`|
|`&[u8]` 1|`unsafe { PathBuf::from(OsString::from_encoded_bytes_unchecked(x.to_vec())) }`|

 `Vec<u8>`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`x.into_bytes()`|
|`CString`|`x.into_bytes()`|
|`OsString`|`x.into_encoded_bytes()`|
|`PathBuf`|`x.into_os_string().into_encoded_bytes()`|
|`Vec<u8>` 1|`x`|
|`&str`|`Vec::from(x.as_bytes())`|
|`&CStr`|`Vec::from(x.to_bytes_with_nul())`|
|`&OsStr`|`Vec::from(x.as_encoded_bytes())`|
|`&Path`|`Vec::from(x.as_os_str().as_encoded_bytes())`|
|`&[u8]` 1|`x.to_vec()`|

 `&str`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`x.as_str()`|
|`CString`|`x.to_str()?`|
|`OsString`|`x.to_str()?`|
|`PathBuf`|`x.to_str()?`|
|`Vec<u8>` 1|`std::str::from_utf8(&x)?`|
|`&str`|`x`|
|`&CStr`|`x.to_str()?`|
|`&OsStr`|`x.to_str()?`|
|`&Path`|`x.to_str()?`|
|`&[u8]` 1|`std::str::from_utf8(x)?`|

 `&CStr`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`CString::new(x)?.as_c_str()`|
|`CString`|`x.as_c_str()`|
|`OsString`|`x.to_str()?`|
|`PathBuf`|?,3|
|`Vec<u8>` 1,4|`CStr::from_bytes_with_nul(&x)?`|
|`&str`|?,3|
|`&CStr`|`x`|
|`&OsStr`|?|
|`&Path`|?|
|`&[u8]` 1,4|`CStr::from_bytes_with_nul(x)?`|
|`*const c_char` 1|`unsafe { CStr::from_ptr(x) }`|

 `&OsStr`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`OsStr::new(&x)`|
|`CString`|?|
|`OsString`|`x.as_os_str()`|
|`PathBuf`|`x.as_os_str()`|
|`Vec<u8>` 1|`unsafe { OsStr::from_encoded_bytes_unchecked(&x) }`|
|`&str`|`OsStr::new(x)`|
|`&CStr`|?|
|`&OsStr`|`x`|
|`&Path`|`x.as_os_str()`|
|`&[u8]` 1|`unsafe { OsStr::from_encoded_bytes_unchecked(x) }`|

 `&Path`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`Path::new(x)` `r`|
|`CString`|`Path::new(x.to_str()?)`|
|`OsString`|`Path::new(x.to_str()?)` `r`|
|`PathBuf`|`Path::new(x.to_str()?)` `r`|
|`Vec<u8>` 1|`unsafe { Path::new(OsStr::from_encoded_bytes_unchecked(&x)) }`|
|`&str`|`Path::new(x)` `r`|
|`&CStr`|`Path::new(x.to_str()?)`|
|`&OsStr`|`Path::new(x)` `r`|
|`&Path`|`x`|
|`&[u8]` 1|`unsafe { Path::new(OsStr::from_encoded_bytes_unchecked(x)) }`|

 `&[u8]`

|If you **have** `x` of type …|Use this …|
|---|---|
|`String`|`x.as_bytes()`|
|`CString`|`x.as_bytes()`|
|`OsString`|`x.as_encoded_bytes()`|
|`PathBuf`|`x.as_os_str().as_encoded_bytes()`|
|`Vec<u8>` 1|`&x`|
|`&str`|`x.as_bytes()`|
|`&CStr`|`x.to_bytes_with_nul()`|
|`&OsStr`|`x.as_encoded_bytes()`|
|`&Path`|`x.as_os_str().as_encoded_bytes()`|
|`&[u8]` 1|`x`|

 **Other**

|You **want**|And **have** `x`|Use this …|
|---|---|---|
|**`*const c_char`**|**`CString`**|`x.as_ptr()`|

i Short form `x.into()` possible if type can be inferred.  
r Short form `x.as_ref()` possible if type can be inferred.  
1 You must ensure `x` comes with a valid representation for the string type (e.g., UTF-8 data for a `String`).  
2 The `c_char` **must** have come from a previous `CString`. If it comes from FFI see `&CStr` instead.  
3 No known shorthand as `x` will lack terminating `0x0`. Best way to probably go via `CString`.  
4 Must ensure `x` actually ends with `0x0`.  

## String Output

How to convert types into a `String`, or output them.

 **APIs**

Rust has, among others, these APIs to convert types to stringified output, collectively called _format_ macros:

|Macro|Output|Notes|
|---|---|---|
|`format!(fmt)`|`String`|Bread-and-butter "to `String`" converter.|
|`print!(fmt)`|Console|Writes to standard output.|
|`println!(fmt)`|Console|Writes to standard output.|
|`eprint!(fmt)`|Console|Writes to standard error.|
|`eprintln!(fmt)`|Console|Writes to standard error.|
|`write!(dst, fmt)`|Buffer|Don't forget to also `use std::io::Write;`|
|`writeln!(dst, fmt)`|Buffer|Don't forget to also `use std::io::Write;`|

|Method|Notes|
|---|---|
|`x.to_string()` [STD](https://doc.rust-lang.org/std/string/trait.ToString.html "See this topic in 'The Rust Standard Library'.")|Produces `String`, implemented for any `Display` type.|

Here `fmt` is string literal such as `"hello {}"`, that specifies output (compare "Formatting" tab) and additional parameters.

 **Printable Types**

In `format!` and friends, types convert via trait `Display` `"{}"` [STD](https://doc.rust-lang.org/std/fmt/trait.Display.html "See this topic in 'The Rust Standard Library'.") or `Debug` `"{:?}"` [STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'.") , non exhaustive list:

|Type|Implements||
|---|---|---|
|`String`|`Debug, Display`||
|`CString`|`Debug`||
|`OsString`|`Debug`||
|`PathBuf`|`Debug`||
|`Vec<u8>`|`Debug`||
|`&str`|`Debug, Display`||
|`&CStr`|`Debug`||
|`&OsStr`|`Debug`||
|`&Path`|`Debug`||
|`&[u8]`|`Debug`||
|`bool`|`Debug, Display`||
|`char`|`Debug, Display`||
|`u8` … `i128`|`Debug, Display`||
|`f32`, `f64`|`Debug, Display`||
|`!`|`Debug, Display`||
|`()`|`Debug`||

In short, pretty much everything is `Debug`; more _special_ types might need special handling or conversion [↑](https://cheats.rs/#string-conversions "On this site, above.") to `Display`.

 **Formatting**

Each argument designator in format macro is either empty `{}`, `{argument}`, or follows a basic [**syntax**](https://doc.rust-lang.org/std/fmt/index.html#syntax):

```rust
{ [argument] ':' [[fill] align] [sign] ['#'] [width [$]] ['.' precision [$]] [type] }
```

|Element|Meaning|
|---|---|
|`argument`|Number (`0`, `1`, …), variable '21 or name,'18 e.g., `print!("{x}")`.|
|`fill`|The character to fill empty spaces with (e.g., `0`), if `width` is specified.|
|`align`|Left (`<`), center (`^`), or right (`>`), if width is specified.|
|`sign`|Can be `+` for sign to always be printed.|
|`#`|[Alternate formatting](https://doc.rust-lang.org/std/fmt/index.html#sign0), e.g., prettify `Debug`[STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'.") formatter `?` or prefix hex with `0x`.|
|`width`|Minimum width (≥ 0), padding with `fill` (default to space). If starts with `0`, zero-padded.|
|`precision`|Decimal digits (≥ 0) for numerics, or max width for non-numerics.|
|`$`|Interpret `width` or `precision` as argument identifier instead to allow for dynamic formatting.|
|**`type`**|`Debug`[STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'.") (`?`) formatting, hex (`x`), binary (`b`), octal (`o`), pointer (`p`), exp (`e`) … [see more](https://doc.rust-lang.org/std/fmt/index.html#traits).|

|Format Example|Explanation|
|---|---|
|`{}`|Print the next argument using `Display`.[STD](https://doc.rust-lang.org/std/fmt/trait.Display.html "See this topic in 'The Rust Standard Library'.")|
|`{x}`|Same, but use variable `x` from scope. '21|
|`{:?}`|Print the next argument using `Debug`.[STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'.")|
|`{2:#?}`|Pretty-print the 3rd argument with `Debug`[STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'.") formatting.|
|`{val:^2$}`|Center the `val` named argument, width specified by the 3rd argument.|
|`{:<10.3}`|Left align with width 10 and a precision of 3.|
|`{val:#x}`|Format `val` argument as hex, with a leading `0x` (alternate format for `x`).|

|Full Example|Explanation|
|---|---|
|`println!("{}", x)`|Print `x` using `Display`[STD](https://doc.rust-lang.org/std/fmt/trait.Display.html "See this topic in 'The Rust Standard Library'.") on std. out and append new line. '15 🗑️|
|`println!("{x}")`|Same, but use variable `x` from scope. '21|
|`format!("{a:.3} {b:?}")`|Convert `a` with 3 digits, add space, `b` with `Debug` [STD](https://doc.rust-lang.org/std/fmt/trait.Debug.html "See this topic in 'The Rust Standard Library'."), return `String`. '21|

---

# Tooling

## Project Anatomy

Basic project layout, and common files and folders, as used by `cargo`. [↓](https://cheats.rs/#cargo "On this site, below.")

|Entry|Code|
|---|---|
|📁 `.cargo/`|**Project-local cargo configuration**, may contain **`config.toml`**. [🔗](https://doc.rust-lang.org/cargo/reference/config.html "Third-party site (mainly used in conjunction with other symbols).") 🝖|
|📁 `benches/`|Benchmarks for your crate, run via **`cargo bench`**, requires nightly by default. * 🚧|
|📁 `examples/`|Examples how to use your crate, they see your crate like external user would.|
|`my_example.rs`|Individual examples are run like **`cargo run --example my_example`**.|
|📁 `src/`|Actual source code for your project.|
|`main.rs`|Default entry point for applications, this is what **`cargo run`** uses.|
|`lib.rs`|Default entry point for libraries. This is where lookup for `my_crate::f()` starts.|
|📁 `src/bin/`|Place for additional binaries, even in library projects.|
|`extra.rs`|Additional binary, run with `cargo run --bin extra`.|
|📁 `tests/`|Integration tests go here, invoked via **`cargo test`**. Unit tests often stay in `src/` file.|
|`.rustfmt.toml`|In case you want to [**customize**](https://rust-lang.github.io/rustfmt/) how **`cargo fmt`** works.|
|`.clippy.toml`|Special configuration for certain [**clippy lints**](https://rust-lang.github.io/rust-clippy/master/index.html), utilized via **`cargo clippy`** 🝖|
|`build.rs`|**Pre-build script**, [🔗](https://doc.rust-lang.org/cargo/reference/build-scripts.html "Third-party site (mainly used in conjunction with other symbols).") useful when compiling C / FFI, …|
|`Cargo.toml`|Main **project manifest**, [🔗](https://doc.rust-lang.org/cargo/reference/manifest.html "Third-party site (mainly used in conjunction with other symbols).") Defines dependencies, artifacts …|
|`Cargo.lock`|For reproducible builds. Add to git for apps, consider not for libs. 💬 [🔗](https://blog.rust-lang.org/2023/08/29/committing-lockfiles.html "Third-party site (mainly used in conjunction with other symbols).") [🔗](https://old.reddit.com/r/rust/comments/164qfjm/change_in_guidance_on_committing_lockfiles_rust/jya8ouf/ "Third-party site (mainly used in conjunction with other symbols).")|
|`rust-toolchain.toml`|Define **toolchain override**[🔗](https://rust-lang.github.io/rustup/overrides.html "Third-party site (mainly used in conjunction with other symbols).") (channel, components, targets) for this project.|

* On stable consider [Criterion](https://github.com/bheisler/criterion.rs).

**Minimal examples** for various entry points might look like:

 **Applications**

```rust
// src/main.rs (default application entry point)

fn main() {
    println!("Hello, world!");
}
```

 **Libraries**

```rust
// src/lib.rs (default library entry point)

pub fn f() {}      // Is a public item in root, so it's accessible from the outside.

mod m {
    pub fn g() {}  // No public path (`m` not public) from root, so `g`
}                  // is not accessible from the outside of the crate.
```

 **Unit Tests**

```rust
// src/my_module.rs (any file of your project)

fn f() -> u32 { 0 }

#[cfg(test)]
mod test {
    use super::f;           // Need to import items from parent module. Has
                            // access to non-public members.
    #[test]
    fn ff() {
        assert_eq!(f(), 0);
    }
}
```

 **Integration Tests**

```rust
// tests/sample.rs (sample integration test)

#[test]
fn my_sample() {
    assert_eq!(my_crate::f(), 123); // Integration tests (and benchmarks) 'depend' to the crate like
}                                   // a 3rd party would. Hence, they only see public items.
```

 **Benchmarks**🚧

```rust
// benches/sample.rs (sample benchmark)

#![feature(test)]   // #[bench] is still experimental

extern crate test;  // Even in '18 this is needed for … reasons.
                    // Normally you don't need this in '18 code.

use test::{black_box, Bencher};

#[bench]
fn my_algo(b: &mut Bencher) {
    b.iter(|| black_box(my_crate::f())); // `black_box` prevents `f` from being optimized away.
}
```

 **Build Scripts**

```rust
// build.rs (sample pre-build script)

fn main() {
    // You need to rely on env. vars for target; `#[cfg(…)]` are for host.
    let target_os = env::var("CARGO_CFG_TARGET_OS");
}
```

*[See here for list](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-build-scripts) of environment variables set.

 **Proc Macros**🝖

```rust
// src/lib.rs (default entry point for proc macros)

extern crate proc_macro;  // Apparently needed to be imported like this.

use proc_macro::TokenStream;

#[proc_macro_attribute]   // Crates can now use `#[my_attribute]`
pub fn my_attribute(_attr: TokenStream, item: TokenStream) -> TokenStream {
    item
}
```

```rust
// Cargo.toml

[package]
name = "my_crate"
version = "0.1.0"

[lib]
proc-macro = true
```

Module trees and imports:

 **Module Trees**

**Modules** [BK](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/mod.html#modules "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/items/modules.html#modules "See this topic in 'The Rust Reference'.") and **source files** work as follows:

- **Module tree** needs to be explicitly defined, is **not** implicitly built from **file system tree**. [🔗](http://www.sheshbabu.com/posts/rust-module-system/ "Third-party site (mainly used in conjunction with other symbols).")
- **Module tree root** equals library, app, … entry point (e.g., `lib.rs`).

Actual **module definitions** work as follows:

- A **`mod m {}`** defines module in-file, while **`mod m;`** will read `m.rs` or `m/mod.rs`.
- Path of `.rs` based on **nesting**, e.g., `mod a { mod b { mod c; }}}` is either `a/b/c.rs` or `a/b/c/mod.rs`.
- Files not pathed from module tree root via some `mod m;` won't be touched by compiler! 🛑

 **Namespaces**🝖

Rust has three kinds of **namespaces**:

|Namespace _Types_|Namespace _Functions_|Namespace _Macros_|
|---|---|---|
|`mod X {}`|`fn X() {}`|`macro_rules! X { … }`|
|`X` (crate)|`const X: u8 = 1;`||
|`trait X {}`|`static X: u8 = 1;`||
|`enum X {}`|||
|`union X {}`|||
|`struct X {}`|||
|← `struct X;`1 →|   ||
|← `struct X();`2 →|   ||

1 Counts in _Types_ and in _Functions_, defines type `X` _and_ constant `X`.  
2 Counts in _Types_ and in _Functions_, defines type `X` _and_ function `X`.

- In any given scope, for example within a module, only one item per namespace can exist, e.g.,
    - `enum X {}` and `fn X() {}` can coexist
    - `struct X;` and `const X` cannot coexist
- With a `use my_mod::X;` all items called `X` will be imported.

> Due to naming conventions (e.g., `fn` and `mod` are lowercase by convention) and _common sense_ (most developers just don't name all things `X`) you won't have to worry about these _kinds_ in most cases. They can, however, be a factor when designing macros.

## Cargo

Commands and tools that are good to know.

|Command|Description|
|---|---|
|`cargo init`|Create a new project for the latest edition.|
|`cargo build`|Build the project in debug mode (`--release` for all optimization).|
|`cargo check`|Check if project would compile (much faster).|
|`cargo test`|Run tests for the project.|
|`cargo doc --open`|Locally generate documentation for your code and dependencies.|
|`cargo run`|Run your project, if a binary is produced (main.rs).|
|`cargo run --bin b`|Run binary `b`. Unifies feat. with other dependents (can be confusing).|
|`cargo run -p w`|Run main of sub-worksp. `w`. Treats features more sanely.|
|`cargo … --timings`|Show what crates caused your build to take so long. 🔥|
|`cargo tree`|Show dependency graph.|
|`cargo +{nightly, stable} …`|Use given toolchain for command, e.g., for 'nightly only' tools.|
|`cargo +nightly …`|Some nightly-only commands (substitute `…` with command below)|
|`rustc -- -Zunpretty=expanded`|Show expanded macros. 🚧|
|`rustup doc`|Open offline Rust documentation (incl. the books), good on a plane!|

Here `cargo build` means you can either type `cargo build` or just `cargo b`; and `--release` means it can be replaced with `-r`.

These are optional `rustup` components. Install them with `rustup component add [tool]`.

|Tool|Description|
|---|---|
|`cargo clippy`|Additional ([lints](https://rust-lang.github.io/rust-clippy/master/)) catching common API misuses and unidiomatic code. [🔗](https://github.com/rust-lang/rust-clippy "Third-party site (mainly used in conjunction with other symbols).")|
|`cargo fmt`|Automatic code formatter (`rustup component add rustfmt`). [🔗](https://github.com/rust-lang/rustfmt "Third-party site (mainly used in conjunction with other symbols).")|

A large number of additional cargo plugins [**can be found here**](https://crates.io/categories/development-tools::cargo-plugins?sort=downloads).

## Cross Compilation

🔘 Check [target is supported](https://doc.rust-lang.org/rustc/platform-support.html).

🔘 Install target via **`rustup target install aarch64-linux-android`** (for example).

🔘 Install native toolchain (required to _link_, depends on target).

Get from target vendor (Google, Apple, …), might not be available on all hosts (e.g., no iOS toolchain on Windows).

**Some toolchains require additional build steps** (e.g., Android's `make-standalone-toolchain.sh`).

🔘 Update **`~/.cargo/config.toml`** like this:

```rust
[target.aarch64-linux-android]
linker = "[PATH_TO_TOOLCHAIN]/aarch64-linux-android/bin/aarch64-linux-android-clang"
```

or

```rust
[target.aarch64-linux-android]
linker = "C:/[PATH_TO_TOOLCHAIN]/prebuilt/windows-x86_64/bin/aarch64-linux-android21-clang.cmd"
```

🔘 Set **environment variables** (optional, wait until compiler complains before setting):

```rust
set CC=C:\[PATH_TO_TOOLCHAIN]\prebuilt\windows-x86_64\bin\aarch64-linux-android21-clang.cmd
set CXX=C:\[PATH_TO_TOOLCHAIN]\prebuilt\windows-x86_64\bin\aarch64-linux-android21-clang.cmd
set AR=C:\[PATH_TO_TOOLCHAIN]\prebuilt\windows-x86_64\bin\aarch64-linux-android-ar.exe
…
```

Whether you set them depends on how compiler complains, not necessarily all are needed.

> Some platforms / configurations can be **extremely sensitive** how paths are specified (e.g., `\` vs `/`) and quoted.

✔️ Compile with **`cargo build --target=aarch64-linux-android`**

## Tooling Directives

Special tokens embedded in source code used by tooling or preprocessing.

 **Macros**

Inside a **declarative** [BK](https://doc.rust-lang.org/book/ch19-06-macros.html#declarative-macros-with-macro_rules-for-general-metaprogramming "See this topic in 'The Rust Programming Language'.") **macro by example** [BK](https://doc.rust-lang.org/book/ch19-06-macros.html "See this topic in 'The Rust Programming Language'.") [EX](https://doc.rust-lang.org/stable/rust-by-example/macros.html#macro_rules "See this topic in 'Rust by Example'.") [REF](https://doc.rust-lang.org/stable/reference/macros-by-example.html "See this topic in 'The Rust Reference'.") `macro_rules!` implementation these work:

|Within Macros|Explanation|
|---|---|
|`$x:ty`|Macro capture (here a type).|
|`$x:item`|An item, like a function, struct, module, etc.|
|`$x:block`|A block `{}` of statements or expressions, e.g., `{ let x = 5; }`|
|`$x:stmt`|A statement, e.g., `let x = 1 + 1;`, `String::new();` or `vec![];`|
|`$x:expr`|An expression, e.g., `x`, `1 + 1`, `String::new()` or `vec![]`|
|`$x:pat`|A pattern, e.g., `Some(t)`, `(17, 'a')` or `_`.|
|`$x:ty`|A type, e.g., `String`, `usize` or `Vec<u8>`.|
|`$x:ident`|An identifier, for example in `let x = 0;` the identifier is `x`.|
|`$x:path`|A path (e.g., `foo`, `::std::mem::replace`, `transmute::<_, int>`).|
|`$x:literal`|A literal (e.g., `3`, `"foo"`, `b"bar"`, etc.).|
|`$x:lifetime`|A lifetime (e.g., `'a`, `'static`, etc.).|
|`$x:meta`|A meta item; the things that go inside `#[…]` and `#![…]` attributes.|
|`$x:vis`|A visibility modifier; `pub`, `pub(crate)`, etc.|
|`$x:tt`|A single token tree, [see here](https://stackoverflow.com/a/40303308) for more details.|
|`$crate`|Special hygiene variable, crate where macros is defined. ?|