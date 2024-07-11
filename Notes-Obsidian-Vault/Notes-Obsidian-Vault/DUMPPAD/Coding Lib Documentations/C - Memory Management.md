## Table of Contents

    - [Memory Layout of a C Program:](#Memory\Layout\of\a\C\Program:)
  - [Memory Management in C](#Memory\Management\in\C)
    - [Memory Layout of a C Program:](#Memory\Layout\of\a\C\Program:)
    - [Stack vs. Heap:](#Stack\vs.\Heap:)
    - [Functions for Memory Management:](#Functions\for\Memory\Management:)

#coding #C #memory #management
### Memory Layout of a C Program:
1. **Text Segment**: Contains the executable code.
2. **Initialized Data Segment**: Contains global, static, constant, and external variables initialized with non-zero values.
3. **Uninitialized Data Segment (BSS)**: Contains global, static, constant, and external variables initialized with zero or not initialized.
4. **Stack**: Contains function parameters, local variables, and control information.
5. **Heap**: Area for dynamic memory allocation.


  
Of course! Let's dive deep into memory management in C.

---

## Memory Management in C

Memory management is a core concept in C programming. Proper memory management ensures efficient use of resources and prevents bugs, crashes, and security vulnerabilities.

### Memory Layout of a C Program:

1. **Text Segment**: Contains the executable code.
2. **Initialized Data Segment**: Contains global, static, constant, and external variables initialized with non-zero values.
3. **Uninitialized Data Segment (BSS)**: Contains global, static, constant, and external variables initialized with zero or not initialized.
4. **Stack**: Contains function parameters, local variables, and control information.
5. **Heap**: Area for dynamic memory allocation.

---

### Stack vs. Heap:

- **Stack**:
    - Managed automatically.
    - Variables are allocated and deallocated automatically.
    - Limited in size.
    - Faster access.
- **Heap**:
    - Managed by the programmer.
    - Requires manual allocation and deallocation.
    - Larger in size.
    - ==Slower access==.

### Functions for Memory Management:

1. `malloc()`: Allocates a specified number of bytes and returns a pointer to the first byte.
2. 
3. `calloc()`: Allocates memory for an array of a specified number of elements, initializes them to zero, and returns a pointer.
4. `realloc()`: Changes the size of previously allocated memory.
5. ``free()``: Deallocates previously allocated memory.

```C

int main() {
    int x = 10;  // Stack allocation
}



int *arr; 
arr = (int*) malloc(5 * sizeof(int)); // Heap allocation for an array of 5 integers 
if (arr == NULL) 
{ 
	printf("Memory allocation failed!");
	return 1;
} 
  free(arr); // Freeing the allocated memory


```











