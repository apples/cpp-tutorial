# 2 - Advanced Syntax and Standard Libraries

It is recommended that you make sure to understand
all previously covered topics
before continuing.
The topics from now on will assume that you have
complete working knowledge of all previous topics.

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

There is usually no signifigant performance difference
between `float` and `double`,
so `double` is the recommended floating point type.

A `long double` is quite large and slow,
and therefore should only be used when extreme precision is needed.

## 2.3 - Dynamic Memory

An object that exists within a scope
is created when it's declared
and destroyed when the scope is closed.
This might create a few problems in certain situations.

Luckily, C++ provides a way to create objects outside of any scope.

### 2.3.1 - Pointers

Before we can create unscoped objects,
we need to know what a "pointer" is.

A pointer is very similar to a reference,
but it's used a bit differently:

```C++
void square(int& i)
{
    i = i * i;
}

void square(int* p)
{
    *p = (*p) * (*p);
}
```

To access the object referred to by the pointer,
the pointer but be "dereferenced" using the `*` operator.
The `*` operator returns a reference to the object.

For example, dereferencing `int*` will give you `int&`.

To create a pointer to an existing object,
use the `&` operator.

```C++
void square(int* pi)
{
    *p = (*p) * (*p);
}

int main()
{
    int i = 7;
    square(&i);
}
```

One of the major differences between pointers and references
is that a pointer can be reassigned.

```C++
void square(int* pi)
{
    *p = (*p) * (*p);
}

int main()
{
    int i = 7;
    int j = 12;
    int* p = nullptr;
    
    p = &i;
    square(p);
    
    p = &j;
    square(p);
}
```

Pointers can also hold a special value, `nullptr`,
which makes the pointer "null".
A null pointer is a pointer that points to nothing.
Dereferencing a null pointer is undefined behaviour,
and is considered a logical error.

```C++
void square(int* pi)
{
    if (p) *p = (*p) * (*p); // Does nothing if p is null
}

int main()
{
    int i = 7;
    int j = 12;
    int* p = nullptr;
    
    square(p); // Safe: nothing happens
    
    p = &i;
    square(p);
    
    p = &j;
    square(p);
}
```

Pointers can be implicitly cast to `bool`.
If the pointer is null,
it's `bool` value is `false`.
If the pointer is *not* null,
it's `bool` value is `true`.

```C++
void square(int* pi)
{
    *p = (*p) * (*p);
}

int main()
{
    int arr[2] = {7, 12};
    
    square(&arr[0]);
    square(&arr[1]);
}
```

Pointers can be manipulated using "pointer arithmetic",
as long as they point to something in an array.

```C++
void square(int* pi)
{
    *p = (*p) * (*p);
}

int main()
{
    int arr[2] = {7, 12};
    int* ptr = &arr[0];
    
    square(ptr);
    ++ptr;
    square(ptr);
}
```

Pointers can be incremented and decremented,
and support addition and subtraction with integers.

```C++
void square(int* pi)
{
    *p = (*p) * (*p);
}

int main()
{
    int arr[2] = {7, 12};
    int* ptr = &arr[0];
    
    square(ptr);
    square(ptr+1);
}
```

Pointers to elements outside of the array are still valid pointers,
but they cannot be dereferenced,
similar to `nullptr`.

Pointers can also be compared to each other.

```C++
void square(int& i)
{
    i = i * i;
}

int main()
{
    int arr[6] = {7, 12, 5, -8, 32, -43};
    
    for (int* ptr = arr; ptr != arr+6; ++ptr)
    {
        square(*ptr);
    }
}
```

In the above example,
the array `arr` is implicitly cast to a pointer.
When an array is cast to a pointer,
it gives a pointer to it's zeroth element.
Since `arr` has `6` elements,
it's valid subscripts are `0` through `5`.
Adding `arr+6` gives us a pointer to `arr[6]`,
which is one element past the last element in `arr`.
Since we can't dereference this pointer,
we use it as the stopping point for our loop.

That is the idiomatic form of "iteration".

```C++
void square(int& i)
{
    i = i * i;
}

int main()
{
    int arr[6] = {7, 12, 5, -8, 32, -43};
    
    for (int& i : arr)
    {
        square(&i);
    }
}
```

Taking the address of a reference
gives the address of the object it refers to,
since it's not reasonable to take the address of a reference.

However, it is possible to take the address of a pointer.

```C++
void square(int& i)
{
    i = i * i;
}

void square(int* p)
{
    *p = (*p) * (*p);
}

int main()
{
    int i = 7;
    int* ptr = &i;
    int** ptrptr = &ptr;
    
    square(*ptrptr);    // Calls square(int*)
    square(**ptrptr);   // Calls square(int&)
}
```

While references only refer to other objects,
pointers are objects themselves,
and have the associated semantics.

```C++
void square(int*& p)
{
    *p = (*p) * (*p);
    p = nullptr;
}

int main()
{
    int i = 7;
    int* ptr = &i;
    
    square(ptr);    // square() sets ptr to nullptr
    //square(&i);   // ILLEGAL: cannot take reference to temporary object (&i)
}
```

A pointer can be `const`,
but it's more common to see a pointer point to a `const` object.

```C++
#include <iostream>

void print(const int* p)
{
    std::cout << *p << std::endl;
}

int main()
{
    int i = 7;
    int* ptr = &i;
    
    print(ptr);
}
```

In the above example,
in the `print()` function,
`p` is mutable and can be reassigned and such,
but `(*p)` is `const`.

### 2.3.2 - `new` and `delete`

To create an object outside of scope,
we need to use `new`.

```C++
int main()
{
    int* ptr = new int;
    delete ptr;
}
```

Using `new` is similar to declaring a variable,
except you do not give it a name.
Instead, `new` returns a pointer to the object.

Constructors can be easily called.

```C++
int main()
{
    int* ptr1 = new int(7);
    int* ptr2 = new int(12);
    delete ptr2;
    delete ptr1;
}
```

When a pointer is obtained from `new`,
the object must be deleted with a matching call to `delete`.
If it is not deleted,
and the pointer is lost,
the object remains in memory,
but it is inaccessible.
This is known as a "memory leak".

Arrays have special handling.

```C++
int main()
{
    int* ptr = new int[128];
    delete[] ptr;
}
```

All elements in the array are default constructed.

Pointers can be copied around like the basic types.

```C++
struct Point
{
    double x;
    double y;
};

Point* makePoint()
{
    return new Point{0.0, 0.0};
}

void killPoint(Point*& p)
{
    delete p;
    p = nullptr;
}

int main()
{
    Point* ptr = makePoint();
    killPoint(ptr);
}
```

When using a pointer to a structure or class,
it's members can be accessed in two ways:

```C++
struct Point
{
    double x;
    double y;
};

Point* makePoint()
{
    return new Point{0.0, 0.0};
}

void killPoint(Point*& p)
{
    delete p;
    p = nullptr;
}

int main()
{
    Point* ptr = makePoint();
    
    (*ptr).x = 7.34;
    ptr->y   = 12.8;
    
    killPoint(ptr);
}
```

The arrow `->` (or "drill") is preferred in most cases.

### 2.3.3 - Smart Pointers

As you may have noticed,
deleting pointers is a bit of a pain.
It's a good thing some smart people invented smart pointers.

#### 2.3.3.1 - `std::shared_ptr`

```C++
#include <memory>

struct Point
{
    double x;
    double y;
};

std::shared_ptr<Point> makePoint()
{
	std::shared_ptr<Point> rval(new Point{0.0, 0.0});
    return rval;
}

int main()
{
    std::shared_ptr<Point> ptr = makePoint();
    
    (*ptr).x = 7.34;
    ptr->y   = 12.8;
}
```

A `std::shared_ptr` works exactly like a normal pointer,
except the object it points to is automatically deleted
when all copies of the `std::shared_ptr` are lost.

```C++
#include <memory>

struct Point
{
    double x;
    double y;
};

std::shared_ptr<Point> makePoint()
{
	std::shared_ptr<Point> rval(new Point{0.0, 0.0});
    return rval;
}

void movePoint(std::shared_ptr<Point> p)
{
    p->x += 5.0;
    p->y += 5.0;
}

int main()
{
    std::shared_ptr<Point> ptr1 = makePoint();
    std::shared_ptr<Point> ptr2(new Point{0.0, 0.0});
    
    (*ptr1).x = 7.34;
    ptr1->y   = 12.8;
    
    movePoint(ptr2);
}
```

It is important to know that the object is not deleted
until all `std::shared_ptr` that reference it are
deleted.

```C++
#include <memory>
int main()
{
    std::shared_ptr<int> ptr{nullptr};
    // *ptr = 7; ILLEGAL: ptr is null
    ptr.reset(new int);
    *ptr = 7;
}
```

Resetting a smart pointer replaces the existing object with a new one,
deleting the old one if necessary.

#### 2.3.3.2 - `std::unique_ptr`

A `std::unique_ptr` acts just like a `std::shared_ptr`,
but it cannot be copied,
only moved.

```C++
#include <memory>
using namespace std;

struct Point
{
    double x;
    double y;
};

unique_ptr<Point> makePoint()
{
	unique_ptr<Point> rval{new Point{0.0, 0.0}};
    return rval;
}

//void func(unique_ptr<Point>); ILLEGAL: can't copy a std::unique_ptr

int main()
{
    unique_ptr<Point> ptr = makePoint();
    
    (*ptr).x = 7.34;
    ptr->y   = 12.8;
    
    //unique_ptr<Point> nope = ptr; ILLEGAL: can't copy a std::unique_ptr
}
```

Since a `std::unique_ptr` can be moved,
it can be returned from a function.
A `std::unique_ptr` is typically used when a smart pointer is needed,
but control over the object's deletion needs to be guaranteed.
A `std::shared_ptr` might keep an object alive for longer than necessary,
but a `std::unique_ptr` cannot be copied,
so it's guaranteed to delete the referenced object.

```C++
#include <memory>
int main()
{
    std::unique_ptr<int> ptr{nullptr};
    // *ptr = 7; ILLEGAL: ptr is null
    ptr.reset(new int(12));
    *ptr = 7;
}
```

Even though a `std::unique_ptr` cannot be copied, it can still be reset.

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
