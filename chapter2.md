# 2 - Advanced Syntax and Standard Libraries

## 2.1 - The `main(argc, argv)` Function

## 2.2 - More Datatypes

### 2.2.1 - `short` and `long`

### 2.2.2 - `float`

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
