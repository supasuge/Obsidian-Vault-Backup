## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [I/O Techniques <a name="io-techniques"></a>](#I/O\Techniques\<a\name="io-techniques"></a>)
    - [Scanning Two Integers Indefinitely <a name="scanning-two-integers-indefinitely"></a>](#Scanning\Two\Integers\Indefinitely\<a\name="scanning-two-integers-indefinitely"></a>)
    - [Scanning a Line Indefinitely <a name="scanning-a-line-indefinitely"></a>](#Scanning\a\Line\Indefinitely\<a\name="scanning-a-line-indefinitely"></a>)
    - [Scanning String and Unknown Number of Integers <a name="scanning-string-and-unknown-number-of-integers"></a>](#Scanning\String\and\Unknown\Number\of\Integers\<a\name="scanning-string-and-unknown-number-of-integers"></a>)
    - [String Formatting for I/O <a name="string-formatting-for-io"></a>](#String\Formatting\for\I/O\<a\name="string-formatting-for-io"></a>)
    - [Float/Double Formatting for I/O <a name="floatdouble-formatting-for-io"></a>](#Float/Double\Formatting\for\I/O\<a\name="floatdouble-formatting-for-io"></a>)
  - [Array Initialization <a name="array-initialization"></a>](#Array\Initialization\<a\name="array-initialization"></a>)
    - [Static Multi-Dimension Array <a name="static-multi-dimension-array"></a>](#Static\Multi-Dimension\Array\<a\name="static-multi-dimension-array"></a>)
    - [Dynamic Multi-Dimension Array <a name="dynamic-multi-dimension-array"></a>](#Dynamic\Multi-Dimension\Array\<a\name="dynamic-multi-dimension-array"></a>)
  - [String Functions and Techniques <a name="string-functions-and-techniques"></a>](#String\Functions\and\Techniques\<a\name="string-functions-and-techniques"></a>)
    - [Iterating Over Characters of String <a name="iterating-over-characters-of-string"></a>](#Iterating\Over\Characters\of\String\<a\name="iterating-over-characters-of-string"></a>)
    - [Sorting and Reversing a String <a name="sorting-and-reversing-a-string"></a>](#Sorting\and\Reversing\a\String\<a\name="sorting-and-reversing-a-string"></a>)
    - [Character Type Checking <a name="character-type-checking"></a>](#Character\Type\Checking\<a\name="character-type-checking"></a>)
    - [String to Integer and Vice Versa <a name="string-to-integer-and-vice-versa"></a>](#String\to\Integer\and\Vice\Versa\<a\name="string-to-integer-and-vice-versa"></a>)
    - [Tokenizing a String <a name="tokenizing-a-string"></a>](#Tokenizing\a\String\<a\name="tokenizing-a-string"></a>)
    - [Finding Permutations <a name="finding-permutations"></a>](#Finding\Permutations\<a\name="finding-permutations"></a>)

## Table of Contents

1. [I/O Techniques](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#io-techniques)
    - [Scanning Two Integers Indefinitely](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#scanning-two-integers-indefinitely)
    - [Scanning a Line Indefinitely](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#scanning-a-line-indefinitely)
    - [Scanning String and Unknown Number of Integers](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#scanning-string-and-unknown-number-of-integers)
    - [String Formatting for I/O](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#string-formatting-for-io)
    - [Float/Double Formatting for I/O](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#floatdouble-formatting-for-io)
2. [Array Initialization](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#array-initialization)
    - [Static Multi-Dimension Array](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#static-multi-dimension-array)
    - [Dynamic Multi-Dimension Array](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#dynamic-multi-dimension-array)
3. [String Functions and Techniques](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#string-functions-and-techniques)
    - [Iterating Over Characters of String](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#iterating-over-characters-of-string)
    - [Sorting and Reversing a String](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#sorting-and-reversing-a-string)
    - [Character Type Checking](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#character-type-checking)
    - [String to Integer and Vice Versa](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#string-to-integer-and-vice-versa)
    - [Tokenizing a String](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#tokenizing-a-string)
    - [Finding Permutations](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#finding-permutations)
    - [String Comparison](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#string-comparison)
    - [SubString and Find-Replace](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#substring-and-find-replace)
4. [Vector Functions and Techniques](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#vector-functions-and-techniques)
    - [Common Vector Functions](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#common-vector-functions)
    - [Sorting Vector of Structs](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#sorting-vector-of-structs)
    - [Array of Vectors](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#array-of-vectors)
    - [Initialize Vector Using Set](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#initialize-vector-using-set)
    - [Search Functions](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#search-functions)
    - [Copying a Vector](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#copying-a-vector)
    - [Two-Dimensional Vector](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#two-dimensional-vector)
5. [Map Functions and Techniques](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#map-functions-and-techniques)
    - [Defining and Accessing Map](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#defining-and-accessing-map)
    - [Key Existence and Iteration](https://chat.openai.com/c/e58b15e9-2f42-4e86-97f0-48189d5f3fc5#key-existence-and-iteration)

---

## I/O Techniques <a name="io-techniques"></a>

### Scanning Two Integers Indefinitely <a name="scanning-two-integers-indefinitely"></a>

cppCopy code

`while(scanf("%d %d\n", &m, &n) == 2) {     cout << m << " " << n << endl; }`

Here, `scanf` returns the number of items successfully read. If it reads both integers, it will return 2, and the loop will continue.

### Scanning a Line Indefinitely <a name="scanning-a-line-indefinitely"></a>

cppCopy code

`string input_line; while(getline(cin, input_line)) {     cout << input_line << endl; }`

`getline` returns an instance of `istream`, which evaluates to `false` when the end of the file is reached.

### Scanning String and Unknown Number of Integers <a name="scanning-string-and-unknown-number-of-integers"></a>

cppCopy code

`#include <sstream> string input_line, first_string; vector<int> vec; int i; while(getline(cin, input_line)) {     stringstream ss(input_line);     ss >> first_string;     while(ss >> i) {         vec.push_back(i);     }     vec.clear(); }`

Here, a `stringstream` object is used to break the line into tokens. First, the string is read, and then the integers are read until no more integers are found.

### String Formatting for I/O <a name="string-formatting-for-io"></a>

cppCopy code

`#include <iomanip> cout << setw(20) << left << "HEADER" << endl; cout << setfill('-') << setw(20) << left << "HEADER" << endl;`

`setw` sets the width, and `left` aligns the text to the left. `setfill` fills the empty space with the specified character.

### Float/Double Formatting for I/O <a name="floatdouble-formatting-for-io"></a>

cppCopy code

`#include <iomanip> float pi = 3.1456; cout << fixed; cout << setprecision(2) << pi << endl;`

`setprecision` sets the number of digits to be displayed after the decimal point. `fixed` ensures that the output is in fixed-point notation.

## Array Initialization <a name="array-initialization"></a>

### Static Multi-Dimension Array <a name="static-multi-dimension-array"></a>

cppCopy code

`bool mat[9][9][9]; memset(mat, false, sizeof(mat));`

`memset` sets the entire array to `false`.

### Dynamic Multi-Dimension Array <a name="dynamic-multi-dimension-array"></a>

cppCopy code

`int n = 9; bool **mat = new bool*[n]; for(int i = 0; i < n; i++) {     mat[i] = new bool[n](); }`

Using `new`, memory is allocated dynamically. The empty parentheses `()` initialize all elements to zero.

## String Functions and Techniques <a name="string-functions-and-techniques"></a>

### Iterating Over Characters of String <a name="iterating-over-characters-of-string"></a>


```C

string sample_string = "utsav"; for(char &c : sample_string) {     cout << c << endl; }`

\\This is a C++11 range-based for loop to iterate over each \\character of the string.
```

### Sorting and Reversing a String <a name="sorting-and-reversing-a-string"></a>

cppCopy code
```c
string sample_string = "utsav"; sort(sample_string.begin(), sample_string.end()); reverse(sample_string.begin(), sample_string.end());`

```
`sort` sorts the string in ascending order, and `reverse` reverses the string.

### Character Type Checking <a name="character-type-checking"></a>

cppCopy code
```C

char c = 'a'; 
if(isdigit(c))
{ /* ... */ } 
if(isalpha(c)) 
{ /* ... */ }

These functions check for various types of characters.
```

### String to Integer and Vice Versa <a name="string-to-integer-and-vice-versa"></a>


```c

string str = "124"; int i = stoi(str);
string new_str = to_string(i);
```

`stoi` converts a string to an integer, and `to_string` does the reverse.

### Tokenizing a String <a name="tokenizing-a-string"></a>

cppCopy code
```c

string source = "/users/home/docs";
stringstream ss(source);
string tmp; while(getline(ss, tmp, '/')) {
	cout << tmp << endl; 
}`
```

Here, `stringstream` and `getline` are used to tokenize the string based on the delimiter `/`.

### Finding Permutations <a name="finding-permutations"></a>

cppCopy code

`string str = "abcd"; bool flag = next_permutation(str.begin(), str.end());`

`next_permutation` modifies the string to its next
```
