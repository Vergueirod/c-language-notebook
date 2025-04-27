# 📘 Personal C Language Guide

## 1. Introduction to C
C is a general-purpose, procedural programming language developed in the early 1970s by Dennis Ritchie at Bell Labs. It's widely used for system programming, embedded systems, and operating systems due to its low-level access to memory and efficient performance.

**Compilation Example:**
```bash
gcc hello.c -o hello
./hello
```

---

## 2. Data Types and Variables
### Primitive Types:
- `int`, `char`, `float`, `double`
- Modifiers: `short`, `long`, `unsigned`

**Example:**
```c
int age = 25;
unsigned int distance = 100;
float pi = 3.14;
```

### `typedef` and `enum`:
```c
typedef unsigned int uint;

enum Weekday { MON, TUE, WED, THU, FRI };
```

---

## 3. Operators
- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logical: `&&`, `||`, `!`
- Bitwise: `&`, `|`, `^`, `~`, `<<`, `>>`

**Precedence Table:** (add link or table later)

---

## 4. Input/Output (I/O)
### Standard I/O:
```c
#include <stdio.h>

int main() {
    int num;
    printf("Enter a number: ");
    scanf("%d", &num);
    printf("You entered: %d\n", num);
    return 0;
}
```

### File I/O:
```c
FILE *fp = fopen("file.txt", "w");
if (fp != NULL) {
    fprintf(fp, "Hello File!\n");
    fclose(fp);
}
```

---

## 5. Control Flow
### Conditionals:
```c
if (num > 0) {
    printf("Positive");
} else if (num < 0) {
    printf("Negative");
} else {
    printf("Zero");
}
```

### Loops:
```c
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}
```

---

## 6. Functions
```c
int add(int a, int b) {
    return a + b;
}

int main() {
    int sum = add(3, 4);
    printf("Sum: %d\n", sum);
    return 0;
}
```

- Pass by value vs. reference:
```c
void increment(int *n) {
    (*n)++;
}
```

---

## 7. Pointers
### Basic Example:
```c
int val = 10;
int *ptr = &val;
printf("%d", *ptr);  // Dereferencing pointer
```

### Pointer to Pointer:
```c
int **pptr = &ptr;
printf("%d", **pptr);
```

---

## 8. Memory: Stack vs Heap
### Conceptual Differences:
- **Stack:**
  - Managed by the compiler.
  - Stores function parameters, return addresses, and local variables.
  - LIFO (Last-In, First-Out) structure.
  - Size is typically limited (determined at compile time).
  - Fast allocation and deallocation.
- **Heap:**
  - Managed by the programmer (via `malloc`, `free`, etc.).
  - Used for dynamic memory allocation.
  - Size is larger (determined at runtime).
  - Slower allocation (due to bookkeeping) but flexible.

### Under the Hood:
- **Stack:**
  - Implemented as a contiguous block of memory.
  - Grows downward (from higher to lower memory addresses) on most architectures.
  - Stack frames hold local variables, return addresses, and saved registers.
  - Stack overflow occurs when the stack pointer exceeds the stack limits (due to deep recursion or large local arrays).

- **Heap:**
  - Managed via the OS and memory allocator (e.g., `glibc malloc`).
  - Uses free lists, bins, or slab allocation depending on the allocator implementation.
  - Fragmentation can occur due to varying sizes of allocations.

### Dynamic Memory Examples:
- **Vector (1D array):**
```c
int *vector = (int *)malloc(5 * sizeof(int));
if (vector != NULL) {
    for (int i = 0; i < 5; i++) vector[i] = i * 2;
    free(vector);
}
```

- **Matrix (2D array):**
```c
int rows = 3, cols = 4;
int **matrix = (int **)malloc(rows * sizeof(int *));
for (int i = 0; i < rows; i++) {
    matrix[i] = (int *)malloc(cols * sizeof(int));
}
// Use matrix
for (int i = 0; i < rows; i++) free(matrix[i]);
free(matrix);
```

### Cleaning Up Memory (Best Practices):
- Always `free` memory once you're done using it.
- Nullify the pointer after freeing (`ptr = NULL;`) to avoid dangling pointers.
- For buffers (e.g., file I/O), ensure that allocated buffers are properly flushed and freed.
- Example of flushing a file buffer:
```c
fflush(fp);
fclose(fp);
```

- Use tools like `valgrind` or `AddressSanitizer` to check for memory leaks or improper accesses.

---

## 9. Structs and Unions
### Struct Example:
```c
typedef struct {
    int id;
    char name[20];
} Person;

Person p1 = {1, "Alice"};
printf("%s", p1.name);
```

### Union Example:
```c
union Data {
    int i;
    float f;
};
```

---

## 10. Macros and Preprocessor
```c
#define PI 3.1415
#define SQUARE(x) ((x) * (x))
```

---

## 11. Common Errors and Best Practices
### Detailed Causes:
- **Segmentation Fault:**
  - Occurs when a program tries to access memory outside its allowed address space.
  - **Hardware Level:** Triggered by the MMU (Memory Management Unit) when a virtual address translation fails (invalid page or protection violation).
  - **Common Causes:**
    - Dereferencing NULL or uninitialized pointers.
    - Accessing memory after freeing (dangling pointer).
    - Buffer overflows (writing past array bounds).

- **Buffer Overflow:**
  - Writing more data to a buffer than it can hold, overwriting adjacent memory.
  - **Hardware Level:** Can corrupt stack frames, overwrite return addresses (stack smashing), leading to unpredictable behavior or exploits.

- **Memory Leak:**
  - Allocated heap memory that is no longer reachable.
  - **System Impact:** Consumes RAM unnecessarily, leading to resource exhaustion.
  - **Cause:** Forgetting to `free` or losing all references to allocated memory.

- **Race Conditions (Multithreaded):**
  - Two or more threads access shared data concurrently, and the outcome depends on the sequence of execution.

### Tips:
- Always initialize variables.
- Check return values (`malloc`, `fopen`, etc.).
- Use defensive programming (e.g., size checks, assertions).
- Validate all inputs to prevent buffer overflows.
- Regularly use debugging tools (GDB, Valgrind, Sanitizers).

---

## 12. Debugging and Tools
- **GDB**: GNU Debugger for inspecting memory, stack frames, variables.
- **Valgrind**: Memory leak checker, detects invalid reads/writes.
- **Cppcheck**: Static code analysis for coding errors.
- **AddressSanitizer**: Runtime memory error detector (integrated with GCC/Clang).
