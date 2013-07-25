# 2 - Advanced Syntax and Standard Libraries

## 2.1 - The `main(argc, argv)` Function

This version of `main()` is used to examine the command-line arguments
that were given to the program when launched.

Here's what it looks like:

```C++
int main(int argc, char* argv[])
{
    return 0;
}
```

Once again, the `return 0;` is optional (*only for `main()`!*).

The `argc` parameter is the "argument count".
It tells you how many arguments were give to the program,
including the program's name.

The `argv` parameter is the "argument vector".
It's the list of arguments, and it's size is equal to `argc`.
The first argument is usually the program's name.

```Shell
$ cat main.cpp

#include <iostream>
int main(int argc, char* argv[])
{
    for (int i=0; i<argc; ++i) std::cout << argv[i] << std::endl;
    return 0;
}

$ g++ main.cpp

$ ./a.out one 2 Tree
./a.out
one
2
Tree
```

In the above example,
`argc` is `4`,
and `argv` is equivalent to:

```C++
const char* argv[] = {"./a.out", "one", "2", "Tree", nullptr};
```

That is, an array of `const char*`.
The last element is always null,
and is not included in `argc`.

These `char*` can be easily converted to `std::string` if needed.

Be careful!
The first argument is not really guaranteed to be your program's name!
The program that launches your program decides what to give you for `argv[0]`.
It's either "the name used to invoke the program" or it's empty.

A semi-real-world example:

```Shell
$ cat main.cpp

#include <iostream>
#include <string>
using namespace std;

int main(int argc, char* argv[])
{
    string verbStr = "";
    if (argc==2) verbStr = argv[1];
    
    bool verbose = (verbStr=="--verbose");
    
    if (verbose)
    {
        cout << "Program name: \"" << argv[0] << endl;
        cout << "2 + 3 = ";
    }
    cout << 2+3 << endl;
    
    return 0;
}

$ g++ main.cpp -o adder

$ ./adder
5

$ ./adder --verbose
Program name: ./adder
2 + 3 = 5

$ ./adder --nope
5
```

The program name will vary between systems.

## 2.2 - More Datatypes

C++ has many different built-in datatypes.
Some are big, some are small.
Some have no size at all.

### 2.2.1 - `short` and `long`

Until now, we've seen a lot of `int`.
This type is the default and recommended integral type.
An `int` is the natural and most efficient integer for the target system.
For example, on most 32-bit computers,
an `int` is 32 bits long, or 4 bytes.

However, there are actually 5 types of integral types.

| Name            | Size                  | Semantics
|:----------------|:----------------------|:---
| `char`          | (smallest)            | Represents a printable text character.
| `short int`     | `>=sizeof(char)`      | Used when memory is a concern.
| `int`           | `>=sizeof(short int)` | Used when performance is a concern.
| `long int`      | `>=sizeof(int)`       | Used when large numbers are a concern.
| `long long int` | `>=sizeof(long int)`  | Used when huge numbers are a concern.

The bigger an integral value is, the larger it's range is.
Most `int` implementations have a range of about 4 billion values
(-2 billion to +2 billion),
so you should use it wherever possible,
for better performance.

A `long long int` can usually hold a stupidly large range,
something like 18 quintillion.
A `short int` usually holds 65 thousand,
and a `char` is usually only 256.

All of these can also be `unsigned`, such as `unsigned short int`.
Unsigned numbers obey modulo arithmetic within their range.

However, unless memory or precision is a concern,
and you will *know* when it is a concern,
always use `int`.

### 2.2.2 - `float`

Just like the integral types have different precision,
so do the floating point types.

| Name          | Size               | Semantics
|:--------------|:-------------------|:---
| `float`       | (smallest)         | Least precise; fastest.
| `double`      | `>=sizeof(float)`  | Most common; the best overall.
| `long double` | `>=sizeof(double)` | Most precise; slowest.

The precision and speed depend on the hardware,
but it's generally accepted that a `double` is twice as big as a `float`,
hence the name.
This means it's twice as precise.

There is no signifigant performance difference
between `float` and `double`,
so `double` is the recommended floating point type.

A `long double` is quite large and slow,
and therefore should only be used when extreme precision is needed.

## 2.3 - Dynamic Memory

### 2.3.1 - `new` and `delete`

### 2.3.2 - Smart Pointers

## 2.4 - Standard Libraries

### 2.4.1 - Containers

#### 2.4.1.1 - `std::vector`

#### 2.4.1.2 - `std::list`

#### 2.4.1.3 - `std::map`

### 2.4.2 - Algorithms

#### 2.4.2.1 - Sorting

#### 2.4.2.2 - Finding

### 2.4.3 - Streams

#### 2.4.3.1 - Console

#### 2.4.3.2 - Files

#### 2.4.3.3 - Strings

## 2.5 - Easier Flow Control

### 2.5.1 - Ternary Operation

### 2.5.2 - `for(:)`

## 2.6 - Better Functions

### 2.6.1 - Default Parameters

### 2.6.2 - Functors

### 2.6.3 - Lambda

### 2.6.4 - Binding

### 2.6.5 - Recursion

## 2.7 - Advancd Classes

### 2.7.1 - Destructors

### 2.7.2 - Move

### 2.7.3 - Operator Overloading

### 2.7.4 - Inheritance

### 2.7.5 - Friends

### 2.7.6 - Uniform Initialization

## 2.8 - Temporary Values

## 2.9 - Type Aliases

## 2.10 - Constant Expressions

## 2.11 - Template Metaprogramming

### 2.11.1 - Variadic Templates

## 2.12 - Tuples

#*******************************************************************************

### 2.3.2 - Range

Range-based `for` loops are a somewhat new feature in C++.
They allow you to easily iterate over containers,
such as arrays.

    int nums[20];
    int x = 0;
    
    for (int& i : nums)
    {
        i = ++x;
    }

Notice how you get a direct reference to the element,
instead of just the element's index.

This is only useful when you don't care where the element is located,
so the above example is kind of contrived.

Range-based `for` loops also work on user-defined containers,
which we will learn about later.

## 2.5 - Recursion

Recursion is the simple act of a function calling itself.
Many problems can be solved using recursion more easily than other methods
such as iteration.

Since a function is scoped in the namespace level,
you can use the function within it's own scope.

A common example is the factorial function

    factorial(x) = x * (x-1) * (x-2) * ... * 1

This iterative function can be implemented as follows:

    int factorial(int x)
    {
        int rval = x;
        while (--x > 0) rval *= x;
        return rval;
    }

However, the problem can also be expressed recursively:

    factorial(1) = 1
    factorial(x) = x * factorial(x-1)

And implemented thus:

    int factorial(int x)
    {
        if (x <= 1) return 1;
        else        return (x * factorial(x-1));
    }

Simple, right?
You typically won't use recursion for something that can be done iteratively,
because iteration is usually better and cleaner.
However, for some problems,
such as tunnelling down a tree structure,
recursion is the way to go.
