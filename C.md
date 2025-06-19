C is a low level language creating in Bell Labs.

# Useful headers

`stdio.h`: For input and outputs
`stdbool.h`: For the boolean type... (C99 only)
`stdint.h`: Standardized types with defined sizes
`ctype.h`: 
`stdlib.h`: contains `exit()` and `EXIT_SUCCESS`
# Reading Input

Reading and writing input and output is done by `scanf()` and `printf()` functions located in `stdio.h` header.

```C
#include <stdio.h>

int main() {
	printf("Enter Number: ");
	int x;
	scanf("%d", &x);
	return 0;
}
```

The f in both these functions means formatted, which means we format the string and let it know what dynamic values to expect to slot inside.

These are called conversion specifications:

* `%d` ===> Works for `int` in base 10
* `%i` ===> Works for `int` in any base system (056 for octal and 0x125 for base 16)
* `%u` ===> `unsigned int` in base 10
* `%o` ===> `unsigned int` in octal
* `%x` ===> `unsigned int` in hexedecimal
* `%hd` ===> each one of above unsigned specifications can take `h` meaning it is a `short` length
* `%ld` ===> The same for `long` length
* `%f` ===> Works only for `float`
* `%e` ===> showcases floating point numbers in scientific notation
* `%g` ===> Shows a suitable version of the float depending on the number 
* `%c` ===> for a single character
* `%p` ===> for pointer values

Printing a number works like below:

```C
#include <stdio.h>

int main() {
	int x = 5;
	printf("I'm inputting a number: %d", x);
	return 0;
}
```

`printf()` returns the number of characters it printed.

In C, constants are usually defined by **macro definitions**.

```C
#define GRAVITY 9.8 
// The value can also be an expression
#define RECIPROCAL_OF_PI (1.0f / 3.14159f)
```

If the expression in a macro definition contains operators, it should be enclosed in a paranthesis.

The `limits.h` header contains macros that have the largest and smallest numbers of each integer type.

**C treats `char` as an integer** therefore a char can also be either signed or unsigned and it can be treated like an integer. `char` itself is not specified to be signed or unsigned by default and each compiler might take that default differently.

`ctype.h` header has the `toupper()` function that capitalizes a character it is given as input.

scanf doesn't skip whitespace when reading characters, because of that we can do something like this:

```C
do {
	scanf("%c", &ch)
} while(ch != '\n');
```

however for working with chars, we can also use `getchar()` and `putchar()`

`getchar()` reads a single character from input and it also does not skip white spaces just like scanf.

These two functions are much faster than scanf and printf both because they are simpler (just written for chars) but also that they are implemented with macros, making them faster.

```C
// Similiar to scanf, we can do something like this
char ch = '';
while(ch != '\n') {
	ch = getchar();
}

// Since getchar() works the same as any other C expression we can do this too:
while(( ch = getchar() ) != '\n')
	;
```

When `scanf()` is not reading characters, it *peeks* at some characters but doesn't read them. If you use `gertchar()`, it will read that extra character. (for example the `'\n'` character at the end of an input line)

The `sizeof` operator returns the size of any operand it is used with. The type of this expression is an implementation defined `size_t`. It is usually casted to unsigned long. In C99, you can print sizeof value directly using `%zu`
# Arrays

It's best practice to indicate the size of arrays as macros.

We can initialize arrays like this:
```C
int array[4] = {1, 2, 3, 4};

int array[10] = {1, 2, 3, 4}; // the rest will be 0s

int array[10] = {0}; // This is a simple way of initializing an array with 0s
```

**It's illegal for an array initializer to be empty.**

We can use **designated initialization** to only initilize particular array elements. (C99+)

```C
int array[10] = {[2] = 5, [7] = 6};
```

Each number in the brackets are called designators.

If an array's length is specified, the designators have to be between 0 and n-1. If the length is not specified then the largest designator is taken to be the last element in the array and the length of the array will be set accordingly.

use `sizeof` to get the size of any array in bytes. dividing the resulting byte size by the sizeof of one of its elements will give you the actual count of the array.

```C
int array[2];
printf("%zu\n", sizeof(array) / sizeof(array[0])); // 8 / 4 = 2
```

A matrix is defined like this:

```C
int matrix[2][3]; // row size 2 and column size 3

int matrix[3][3] = {
	{1, 0, 2},
	{2, 3, 2},
	{7, 2, 3}
};
```

designators work like this for matrices:

```C
int iden[2][2] = {[0][0] = 1, [1][1] = 1}; // INitializes an identity matrix
```

Before C99, arrays could only have static lengths however in C99+, arrays can be variable length arrays (VLAs), their size can be calculated during execution rather than during compile. There are however two restrictions:
* A VLA can't be initialized
* A VLA can't be a static storage type

Pass arrays into functions like this:
```C
int sum(int array[], int n)
{
	int sum = 0;
	for(int i = 0; i < n; i++)
	{
		sum += array[i];
	}
	return sum;
}

int main() 
{
	int b[5] = {...};
	sum(b, 5);
	return 0;
}
```

Because an array collapses into a pointer to the first element of the array, any changes we make on the array inside the function will be present after the function is done.

In C99, we can use variable length parameters,

```C
int sum(int n, int m, int matrix[n][m])
{

}
// function declarations
int sum(int n, int m, int matrix[n][m]);
int sum(int n, int m, int matrix[*][*]);
int sum(int n, int m, int matrix[][m]);
int sum(int n, int m, int matrix[][*]);
```

The static keyword gives a hint as to how large an array is at least.

```C
int sum(int a[static 3], int n);
```

static can only be used on the first dimension.

It can in some cases make the compiler better make the program faster because it can prefetch more at the start of the function.

We can use compouned literals to pass in anonymous arrays.

```C
sum((int []) {1, 2, 3, 8, 9}, 5);
```
# Pointers

Functions can declare that they won't change the value that the pointer points to.

```C
void foo(const int* ptr);
```

You can do pointer arithmetic if you have pointed to an element of an array.

C supports three operations on pointers:
* Adding an integer
* Subtracting an integer
* subtracting with another pointer

```C
// Integer Addition
int array[4] = {1, 2, 3, 4};
int* p = &array[1];
int* q = p + 1 // points to index 2, *q is 3
q = p - 1 // points to index 0, *q is 1
int
```

```C
// A function written using pointer arithmetic
int sum(int array[], int n)
{
    int sum = 0;
    for(int* p = &array[0]; p < &array[n]; p++)
    {
        sum += *p;
    }
   return sum;
}
```
It used to be that the above method would save execution time but that is now implementation based. Some compilers even work better with subscripting.

Multidimensional arrays are stored in major row order, meaning the rows are right after each other. By using pointer arithmetic we can access all elements of multidimensional arrays like a 1 Dimensional array.

```C
    int matrix[LENGTH][LENGTH] = {
        {-11, -10,  -9, -8, -6},
        {-5,   -4,  -3, -2, -1},
        { 0,    1,   2,  3,  4},
        { 5,    6,   7,  8,  9},
    };
    
    for(int* p = &matrix[0][0] ; p < &matrix[LENGTH-1][LENGTH-1]; p++)
    {
        printf("%i\n", *p);
    }
```

