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

# Strings

A C string literal is a series of characters that are enclosed in " "s. If you need to write the rest of a string literal on a new line you can add a \ at the end.

```C
printf("This is some random text that mean absolutely nothing \
While it is true I am useless it doesn't mean I'm not worth \
a damn");
```

This is called splicing in C. The problem is it has to start from the beginning of the next line.

Two string literals separated by white space will be printed together:
```C
printf("Hello, We are at the "
		"cusp of war!\n");
```

**You can not modify a string literal**

You can store a string literal both in char arrays and a char pointer. If a string has n characters, the array has to at least have n + 1 size otherwise the null character may not be stored which causes problems in C algorithms.

Just like initializing any array with fewer elements than its size, initializing a string array with lower sized string, is filled with null characters.

Two differences between pointer strings and array strings:
* an array can be modified but pointers can't
* the pointer version is a varaible that can point to something else.

A pointer sets aside enough variable for a pointer varaible but it does not save the string literal anywhere.

## Reading and writing strings

`puts()` function outputs a string with a new line tacked on at the end.

`printf()` can also print strings by using the type conversion `%s` (It is for char pointers), it will print the string character by character until it reaches the null character.

Specify how many characters to print like this:
```C
printf("%.6s\n", str);
```

Right justify a string with a smaller size than m with:

```C
char* string = "Hello"
printf("%12s\n", string); //prints with white space:
//------Hello
```

scanf can take a string array and read strings. It ignores all white space however, so you can not have a space in your string.

`gets()` also reads strings into a char array however it doesn't skip whitespace before or after. It reads until it sees a new line character, the new line character is discarded. (gets is currently deprecated for some reason). gets is dangerous because it can potentially write more than the string array has room for, potentially writing in memory space that it should not.

Because reading strings is dangerous, it is common to read strings character by character. Make sure to remember inputting the null character ourselves.

`string.h` has many functions for working with strings. Be careful with functions that do not state const in their parameters, they could change your string.

`strcpy(str1, const str2)` copies str2 into str1 and returns the value it copied (it continues until it reaches a null character), but it doesn't have any idea of knowing if str2 will fit in str1 which makes this function unsafe.

`strncpy()` is safer but slower. 
```C
strncpy(str1, str2, sizeof(str1));
```

But it will not insert a null character at the end of str1 if the length of str2 is more than str1. Here's a safer way:

```C
strncpy(str1, str2, sizeof(str1) - 1);
str1[sizeof(str1)-1] = '\0';
```

`size_t strlen(const char* s)` returns size of a string.

`strcat()` concatenates str2 at the end of str1 and returns a pointer to str1.

If the array of str1 is not large enough to accomodate the new string the function's behavior will be undefined. This function then is unsafe in that manner.

`strncat()` is the safer but slower version. Unlike the other n functions, it will terminate the string with a '\0' if it has enough space.

```C
strncat(str1, str2, sizeof(str1) - strlen(str1) - 1);
```

`strcmp(char* str1, char* str2)` compares two strings. If str1 is less than str2 it returns a number less than 0, if they are equal it returns 0, if str1 is greater than str2 it returns a number greater than 0.

It defines greatness by lexicographic ordering (the same used in dictionaries) C < Z and C > A and A == A.

## String Arrays

We can have an array of pointers that point to strings instead of a two-dimensional array of characters.

```C
char *planets[] = {"Hi", "There", "Lovely", "To", "M"};
```










