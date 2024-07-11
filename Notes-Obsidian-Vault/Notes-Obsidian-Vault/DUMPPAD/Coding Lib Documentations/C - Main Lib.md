## Table of Contents

  - [String Handling Functions:](#String\Handling\Functions:)
    - [1. `strcpy`:](#1.\`strcpy`:)
    - [2. `strncpy`:](#2.\`strncpy`:)
    - [3. `strcmp`:](#3.\`strcmp`:)
    - [4. `strncmp`:](#4.\`strncmp`:)
    - [5. `strlen`:](#5.\`strlen`:)
    - [6. `strcat`:](#6.\`strcat`:)
    - [7. `strncat`:](#7.\`strncat`:)
    - [8. `strchr`:](#8.\`strchr`:)
    - [9. `strrchr`:](#9.\`strrchr`:)
    - [10. `strstr`:](#10.\`strstr`:)
    - [11. `strtok`:](#11.\`strtok`:)
  - [Memory Management Functions:](#Memory\Management\Functions:)
    - [1. `calloc`:](#1.\`calloc`:)
    - [2. `free`:](#2.\`free`:)

Sure! Let's start with the detailed explanations for each of the provided functions. Once that's done, I'll provide an overview for the curriculum notes.

---

## String Handling Functions:
#stringhandling #C
### 1. `strcpy`:
- **Prototype**: `char *strcpy (char *dest, char *src);`
- **Description**: This function copies the `src` string into `dest` string.
- **Example**:
```c
char source[] = "Hello, World!";
char destination[50];
strcpy(destination, source);
printf("%s", destination);  // Outputs: Hello, World!
```

### 2. `strncpy`:
#C
- **Prototype**: `char *strncpy(char *string1, char *string2, int n);`
- **Description**: Copies the first `n` characters of `string2` to `string1`.
- **Example**:
```c
char source[] = "Hello, World!";
char destination[50];
strncpy(destination, source, 5);
printf("%s", destination);  // Outputs: Hello
```

### 3. `strcmp`:
- **Prototype**: `int strcmp(char *string1, char *string2);`
- **Description**: Compares `string1` and `string2` to determine their alphabetic order.
- **Example**:
```c
if(strcmp("apple", "banana") < 0) {
    printf("'apple' comes before 'banana' in the dictionary.");
}
```

### 4. `strncmp`:
- **Prototype**: `int strncmp(char *string1, char *string2, int n);`
- **Description**: Compares the first `n` characters of two strings.
- **Example**:
```c
if(strncmp("apple", "applet", 5) == 0) {
    printf("The first 5 characters of both strings are equal.");
}
```

### 5. `strlen`:
- **Prototype**: `int strlen(char *string);`
- **Description**: Determines the length of a string.
- **Example**:
```c
int len = strlen("Hello, World!");
printf("%d", len);  // Outputs: 13
```

### 6. `strcat`:
- **Prototype**: `char *strcat(char *dest, const char *src);`
- **Description**: Concatenates the `src` string to the `dest` string.
- **Example**:
```c
char dest[50] = "Hello,";
strcat(dest, " World!");
printf("%s", dest);  // Outputs: Hello, World!
```

### 7. `strncat`:
- **Prototype**: `char *strncat(char *dest, const char *src, int n);`
- **Description**: Concatenates `n` characters from the `src` string to the `dest` string.
- **Example**:
```c
char dest[50] = "Hello,";
strncat(dest, " World!", 6);
printf("%s", dest);  // Outputs: Hello, World
```

### 8. `strchr`:
- **Prototype**: `char *strchr(char *string, int c);`
- **Description**: Finds the first occurrence of character `c` in the string.
- **Example**:
```c
char *result = strchr("Hello, World!", 'W');
if(result) {
    printf("%s", result);  // Outputs: World!
}
```

### 9. `strrchr`:
- **Prototype**: `char *strrchr(char *string, int c);`
- **Description**: Finds the last occurrence of character `c` in the string.
- **Example**:
```c
char *result = strrchr("Hello, World! World!", 'W');
if(result) {
    printf("%s", result);  // Outputs: World!
}
```

### 10. `strstr`:
- **Prototype**: `char *strstr(char *string2, char *string1);`
- **Description**: Finds the first occurrence of `string1` in `string2`.
- **Example**:
```c
char *result = strstr("Hello, World!", "World");
if(result) {
    printf("%s", result);  // Outputs: World!
}
```

### 11. `strtok`:
- **Prototype**: `char *strtok(char *s, const char *delim);`
- **Description**: Parses the string `s` into tokens using `delim` as the delimiter.
- **Example**:
```c
char str[] = "Hello, World!";
char *token = strtok(str, ", ");
while(token) {
    printf("%s\n", token);  // Outputs: Hello \n World!
    token = strtok(NULL, ", ");
}
```

## Memory Management Functions:

### 1. `calloc`:
- **Prototype**: `void *calloc(int num_elems, int elem_size);`
- **Description**: Allocates memory for an array and initializes all elements to zero.
- **Example**:
```c
int *arr = (int *) calloc(5, sizeof(int));
if(arr) {
    for(int i = 0; i < 5; i++) {
        printf("%d ", arr[i]);  // Outputs: 0 0 0 0 0
    }
    free(arr);
}
```

### 2. `free`:
- **Prototype**: `void free(void *mem_address);`
- **Description