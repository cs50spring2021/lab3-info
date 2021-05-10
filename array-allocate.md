# Allocating an array with calloc

We've seen the use of a one-dimensional array of pointers to strings, in the sorter2.c example.
In that case, the number of slots in the array was a constant.
What if we don't know that number at compile time?

Here's a snippet of code to hint at how to allocate an array of `num` slots; here we leverage a variant of `malloc` called `calloc`, which takes two parameters: the number of items, and the size of each item.

```c
// the number of elements in the array
int num = ...; // somehow obtained; below we assume num>=2
...
// array of ints
int* array = calloc(num, sizeof(int));
array[1] = 42;

// array of pointers to int
int** array = calloc(num, sizeof(int*));// here, size of a pointer to an int
array[1] = malloc(sizeof(int)); // here, size of a whole int
*array[1] = 42;

// for other types, the process is analogous
typedef struct foo foo_t;
foo_t** array = calloc(num, sizeof(foo_t*); // here, size of a pointer to a foo
array[1] = malloc(sizeof(foo_t));  // here, size of a whole foo
array[1]->foomember = 42; // assuming foomember is an integer member of foo_t

// or, sometimes, you don't know how to initialize the structure
// because it provides a 'constructor' function. first step is the same:
foo_t** array = calloc(num, sizeof(foo_t*);
array[1] = foo_new(42); // let the constructor allocate and initialize a new foo; here we assume it returns foo_t*.
```

**Warning:** for brevity the above code does not check for NULL return value from `calloc` or `malloc`; all good CS50 code should do so!
