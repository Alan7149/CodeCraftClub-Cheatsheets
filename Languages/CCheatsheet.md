# 🇨 C Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · C programming quick reference.

---

## Hello World & Compiling

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

```bash
gcc main.c -o main && ./main
gcc -Wall -g main.c -o main      # warnings + debug info
```

## Data Types

```c
int     n = 42;
long    big = 10000000L;
float   f = 3.14f;
double  d = 3.14159;
char    c = 'A';
unsigned int u = 10;
_Bool   flag = 1;          // or #include <stdbool.h> for bool/true/false
const int MAX = 100;

printf("%d %f %c %s\n", n, d, c, "str");   // format specifiers
// %d int, %f float/double, %c char, %s string, %x hex, %p pointer, %lu unsigned long
```

## Input / Output

```c
int age;
scanf("%d", &age);                 // note the & (address)
char name[50];
scanf("%49s", name);               // read a word
fgets(name, sizeof(name), stdin);  // read a line (safer)
printf("Age: %d\n", age);
```

## Control Flow

```c
if (age >= 18) { } else if (age > 12) { } else { }

for (int i = 0; i < 5; i++) { }
while (cond) { }
do { } while (cond);

switch (day) {
    case 1: printf("Mon"); break;
    default: printf("?");
}

// Ternary
int max = (a > b) ? a : b;
```

## Functions

```c
int add(int a, int b) {
    return a + b;
}

void greet(const char *name) {
    printf("Hi, %s\n", name);
}

// Pass by reference (via pointers)
void increment(int *x) {
    (*x)++;
}
int n = 5;
increment(&n);    // n is now 6
```

## Pointers (core concept)

```c
int x = 10;
int *p = &x;       // p holds address of x
printf("%d", *p);  // dereference -> 10
*p = 20;           // changes x to 20
int *null_ptr = NULL;

// Pointer arithmetic
int arr[3] = {1, 2, 3};
int *ap = arr;     // points to arr[0]
*(ap + 1);         // arr[1] -> 2
```

## Arrays & Strings

```c
int nums[5] = {1, 2, 3, 4, 5};
int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
int len = sizeof(nums) / sizeof(nums[0]);   // 5

char str[] = "hello";        // char array (null-terminated)
#include <string.h>
strlen(str);                 // 5
strcpy(dest, src);
strcat(dest, src);
strcmp(a, b);                // 0 if equal
```

## Structs

```c
struct Point {
    int x;
    int y;
};

struct Point p = {3, 4};
p.x;                         // member access
struct Point *pp = &p;
pp->y;                       // member via pointer (same as (*pp).y)

typedef struct {
    char name[50];
    int age;
} Person;                    // now use "Person" directly
Person alan = {"Alan", 21};
```

## Dynamic Memory

```c
#include <stdlib.h>

int *arr = malloc(5 * sizeof(int));     // allocate
if (arr == NULL) { /* handle failure */ }
arr[0] = 10;
arr = realloc(arr, 10 * sizeof(int));   // resize
free(arr);                               // ALWAYS free
arr = NULL;

int *zeros = calloc(5, sizeof(int));     // allocate + zero
```

## File I/O

```c
FILE *f = fopen("data.txt", "r");   // "w" write, "a" append
if (f == NULL) { /* error */ }
char line[256];
while (fgets(line, sizeof(line), f)) {
    printf("%s", line);
}
fclose(f);

FILE *out = fopen("out.txt", "w");
fprintf(out, "Age: %d\n", 21);
fclose(out);
```

## Preprocessor

```c
#include <stdio.h>          // header
#define PI 3.14159          // macro constant
#define SQUARE(x) ((x)*(x)) // macro function
#ifdef DEBUG
    printf("debug\n");
#endif
```

---

[🔝 Back to README](../README.md)
