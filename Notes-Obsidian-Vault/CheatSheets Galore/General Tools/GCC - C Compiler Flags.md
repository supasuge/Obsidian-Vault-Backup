### Table of Contents

  - [Basic Compilation](#basic-compilation)
  - [Debugging and Optimization](#debugging-and-optimization)
  - [Warnings and Errors](#warnings-and-errors)
  - [Linking](#linking)
  - [Preprocessor Options](#preprocessor-options)
  - [Language Selection and Standards](#language-selection-and-standards)
  - [Miscellaneous Flags](#miscellaneous-flags)
  - [Profiling and Instrumentation](#profiling-and-instrumentation)
  - [Warning Control](#warning-control)
  - [Machine-Dependent Options](#machine-dependent-options)
  - [Dependency Generation](#dependency-generation)
  - [Code Generation and Optimization Flags](#code-generation-and-optimization-flags)
  - [Other Flags](#other-flags)

### Basic Compilation

**Compile a single C file into an object file:**
```bash
gcc -c myfile.c
```
**Compile and link a C program:**
```bash
gcc myfile.c -o myprogram
```

### Debugging and Optimization

**Compile with debug information:**
```bash
gcc -g myfile.c -o myprogram
```
**Compile with maximum optimization:**
```bash
gcc -O3 myfile.c -o myprogram
```

### Warnings and Errors

**Enable all warnings and treat them as errors:**
```bash
gcc -Wall -Werror myfile.c -o myprogram
```
**Enable specific warnings:**
```bash
gcc -Wextra -Wuninitialized myfile.c -o myprogram
```

### Linking

**Link with a specific library (e.g., math library):**
```bash
gcc myfile.c -lm -o myprogram
```
**Generate a shared library from an object file:**
```bash
gcc -shared -o libmysharedlib.so myfile.o
```

### Preprocessor Options

**Define a macro:**
```bash
gcc -D MY_MACRO myfile.c -o myprogram
```
**Include a specific directory for header files:**
```bash
gcc -I /path/to/directory myfile.c -o myprogram
```

### Language Selection and Standards

**Compile a C++ program using C++11 standard:**
```bash
gcc -std=c++11 myfile.cpp -o myprogram
```
**Specify the language of the subsequent input files:**
```bash
gcc -x c myfile.c -o myprogram
```

### Miscellaneous Flags

**Generate position-independent code (for shared libraries):**
```bash
gcc -fPIC -c myfile.c
```
**Save all temporary intermediate files:**
```bash
gcc -save-temps myfile.c -o myprogram
```

### Profiling and Instrumentation

**Compile for profiling with gprof:**
```bash
gcc -pg myfile.c -o myprogram
```
**Produce a coverage test file:**
```bash
gcc -ftest-coverage myfile.c -o myprogram
```

### Warning Control

**Enable extra warnings and specific warnings:**
```bash
gcc -Wall -Wextra -Wuninitialized myfile.c -o myprogram
```
**Stop on the first error encountered:**
```bash
gcc -Wfatal-errors myfile.c -o myprogram
```

### Machine-Dependent Options

**Specify an architecture (e.g., Intel Core i7):**
```bash
gcc -march=corei7 myfile.c -o myprogram
```
**Tune the performance of the code to run best on a specific CPU:**
```bash
gcc -mtune=corei7 myfile.c -o myprogram
```

### Dependency Generation

**Generate dependencies for makefile:**
```bash
gcc -M myfile.c
```

### Code Generation and Optimization Flags

**Omit the frame pointer in functions that don't need one:**
```bash
gcc -fomit-frame-pointer myfile.c -o myprogram
```
**Consider all "simple enough" functions for inlining:**
```bash
gcc -finline-functions myfile.c -o myprogram
```
**Disable inlining of functions:**
```bash
gcc -fno-inline myfile.c -o myprogram
```

### Other Flags

**Direct output to a specified file:**
```bash
gcc -o outputfile myfile.c
```
**Use pipes rather than temporary files for communication between stages of compilation:**
```bash
gcc -pipe myfile.c -o myprogram
```
**Verbose output to see what is happening during compilation:**
```bash
gcc -v myfile.c -o myprogram
```
**Get help on a specific topic:**
```bash
gcc --help=optimizers
```

### Additional Examples

**Creating a shared library from source files:**
```bash
gcc -fPIC -c file1.c file2.c
gcc -shared -o libmylib.so file1.o file2.o
```
**Compiling and linking with debugging and optimization:**
```bash
gcc -g -O2 myfile.c -o myprogram
```
**Compiling a program with specific include and library directories:**
```bash
gcc -I /path/to/includes -L /path/to/libs myfile.c -o myprogram -lmylib
```

