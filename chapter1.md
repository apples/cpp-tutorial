# 1 - Basic Syntax and Usage

Some notes before we begin:

- This tutorial assumes you have working knowledge of your development environment.

- This tutorial assumes standard C++ and does not cover any extensions

- This tutorial is only syntax and language

If you need a compiler, get `gcc`.
If you're on Windows, I recommend a version of [MinGW](http://nuwen.net/mingw.html).

For editing code, I recommend [Geany](http://www.geany.org/).
It has the nice ability to build and run code from within the editor,
so you don't have to muck about with terminals and such.

Basic usage of `gcc`:

    gcc my_code.cpp

Where `my_code.cpp` is your source code file.
This will produce an executable called `a.out` that you can run from a terminal.

## 1.0 - Basic Input/Output

To help you out in the coming battle,
here is a quick and dirty intro to console interaction:

    #include <iostream>
    using namespace std;
    
    int main()
    {
        int var1 = 7;
        
        cout << "var1 is " << var1 << endl;
        
        cout << "Enter a new value: ";
        cin >> var1;
        
        cout << "var1 is " << var1 << endl;
        
        return 0;
    }

So you probably don't understand any of this yet,
but just bear with me for a while.

The objects `cout` and `cin` are streams that represent the console;
`cout` is the output stream, and `cin` is the input stream.

To output a value using `cout`, you must insert (`<<`) a value into it.
Similarly, you can extract (`>>`) values from `cin`.

That's all you get for now,
more details will come along later.

## 1.1 - The `main()` Function

Every program needs a `main()` function. It looks like this:

    int main()
    {
        return 0;
    }

More often, it looks like this:

    int main(int argc, char** argv)
    {
        return 0;
    }

Those are the only two accepted forms of `main()`.
In the second form, `argc` and `argv` represent the command used to run the program.
You'll learn how to use these later,
so just use the first version of `main()` for now.

The `return 0;` is the return code of the program.
Returning `0` means that the program ended normally.

## 1.2 - Basic Datatypes and Arithmetic

### 1.2.1 - Datatypes and Declarations

The following basic datatypes are available:

| Name     | Description |
|:---------|:------------|
| `bool`   | Must be either `true` or `false`.
| `char`   | An ASCII character; the smallest datatype.
| `int`    | An integer.
| `double` | A floating-point number.

There are a few more datatypes that I will not describe here because they are not necessary for common uses.

Variables are declared as follows:

    bool   var1 = false;
    char   var2 = 'A';
    int    var3 = 278;
    double var4 = 9.3;

Note the differences in how raw values are represented:

- The values `true` and `false` are `bool`.

- Anything in single quotes (e.g. `'A'` or `'5'`) is a `char`.

- A plain integer number is an `int`.

- A number with a decimal point is a `double`.

There are three ways to initialize variables:

    int var1;
    int var2 = 7;
    int var3(12);

The first method is called "default initialization".
In the case of basic datatypes,
the default value is undefined,
so there is no way of knowing what value `var1` will hold.

The second method is "assignment initialization".
If the value given is of the same type as the variable,
it will be copied into the variable.
If the value is of a different type,
it will be implicitly converted to the proper type.

The third method calls the variable's constructor.
We will learn more about this method later.

You can also declare multiple variables on one line:

    int x, y, z;

This is usually frowned upon for anything more complex, though.

### 1.2.2 - Assignment

Variable can also be reassigned and copied,
like so:

    int var1 = 1;       //1 stored into var1
    var1 = 2;           //2 stored into var1
    
    int var2 = 5;       //5 stored into var2
    var2 = var1;        //var1 copied into var2
    
    int var3 = var2;    //var2 copied into var3

I challenge you to figure out the resulting values of the three variables.

### 1.2.3 - Basic Operations

You'll find that the most basic arithmetic operations are avaialable in C++.

| Usage   | Name |
|:--------|:-----|
| `a + b` | Add
| `a - b` | Subtract
| `a * b` | Multiply
| `a / b` | Divide
| `a % b` | Remainder (integers only!)

They can be used as expected:

    int var1 = 7 + 3;
    int var2 = var1 - 4;

Those 5 operators can be used with assignment:

    int var1 = 16;
    var1 += 2;

Here are some unary operators:

| Usage | Name |
|:------|:-----|
| `-a`  | Negation
| `++a` | Increment
| `--a` | Decrement
| `a++` | Deferred increment
| `a--` | Deferred decrement

Comparison operations (returns a `bool`):

| Usage    | Name |
|:---------|:-----|
| `a == b` | Equal
| `a != b` | Not equal
| `a > b`  | Greater than
| `a >= b` | Greater than or equal
| `a < b`  | Less than
| `a <= b` | Less than or equal

Boolean operations (only works on `bool`):

| Usage    | Name |
|:---------|:-----|
| `a && b` | AND
| `a || b` | OR
| `!a`     | NOT

### 1.2.4 - Conversion and Promotion

If a variable is assigned a value that is a different type,
the value will be converted to the type of the variable.

If an operation is given two values of differing types,
the smaller type will be converted to the larger type.

For example:

    int  var1 = 7;
    char var2 = var1;

`var1` will be copied and converted to a `char`.

    int    var1 = 7;
    double var2 = 6.5;
    double var3 = var1 * var2;

`var1` is converted to a `double` and multiplied with `var2`,
and the result is stored in `var3`.

## 1.3 - Arrays

Arrays are pretty simple.
They are contiguous lists of specific type.

Here is an array of 20 `int`s:

    int arr1[20];

Arrays are accessed using subscripts:

    int arr1[20];
    int zeroth = arr1[0];
    int first  = arr1[1];

You can have an array of arrays:

    int arr1[10][10];

Arrays are initialized using `initializer lists`:

    int arr1[5] = {6, 7, 2, 56, 97};

That's pretty much all there is about arrays.
Note that you cannot use arithmetic or copy assignment on entire arrays.

Be careful!

Using arrays incorrectly will cause your program to crash.

    int  arr1[20];
    arr1[9000] = 12;

What's wrong with this code?
The array `arr1` only has 20 elements,
but we're trying to access it's 9000th element!

Accessing data that does not exist is undefined behaviour.
You just get junk numbers,
and sometimes the operating system will blow up your program.

## 1.4 - Pointers and References

### 1.4.1 - Pointers

Pointers are the most basic form of indirection.
Essentially, you can create a pointer to a variable and store it, copy it, and otherwise manipulate it.

This is done using the following syntax:

    int  var1;
    int* ptr1 = &var1;

So `&var1` is a pointer to `var1`,
and an `int*` is a pointer to an `int`.
Therefore, we can take `&var1` and store it in `ptr1`.

To access the variable that a pointer represents:

    int  var1 = 7;      //7 stored into var1
    int* ptr1 = &var1;  //pointer to var1 stored in ptr1
    int var2 = *ptr1;   //value pointed to by ptr1 stored into var2
    *ptr1 = 12;         //12 stored into value pointed to by ptr1

You can also store a `nullptr` in any pointer,
so that it points to nothing.

    int* ptr1 = nullptr;

And, of course, pointers can be reassigned and whatnot:

    int  var1 = 7;
    int* prt1 = &var1;

    int* ptr2;
    ptr2 = ptr1;
    ptr1 = nullptr;
    *ptr2 = 12;

You can treat pointers like arrays:

    int  arr1[20];          //declare an array of 20 ints
    int* ptr1 = &arr1[0];   //pointer to zeroth element of arr1 stored into ptr1
    ptr1[12] = 54;          //54 stored into the 12th elemnent after ptr1
    
    int  arr1[20];          //declare an array of 20 ints
    int* ptr1 = &arr1[5];   //pointer to 5th element of arr1 stored into ptr1
    ptr1[7] = 54;           //54 stored into the 7th elemnent after ptr1 (12th of arr1)

Also, character arrays can be used like this:

    const char* ptr1 = "I am a character array!";

The `const` simply means that you cannot alter the data.
You can reassign the pointer itself, though.
We will talk more about character strings later.

### 1.4.2 - References

References are very similar to pointers,
except they *must* refer to a valid object.
You cannot have a reference to `nullptr`,
and you cannot reassign references.
You can, however, copy references,
and get a pointer to the object they refer to.

Getting a reference:

    int  var1;
    int& ref1 = var1;

There is no special syntax for accessing the referred object.

    int  var1 = 7;
    int& ref1 = var1;
    ref1 = 12;

Another example:

    int  var1 = 7;      //7 stored into var1
    int& ref1 = var1;   //reference to var1 created as ref1
    int* ptr1 = &ref1;  //pointer to value referred to by ref1 stored into ptr1
    *ptr1 = 12;         //12 stored into value pointed to by ptr1
    int  var2 = ref1;   //value referred to by ref1 copied into var2
    ptr1 = &var2;       //pointer to var2 stored into ptr1
    *ptr1 = 7;          //7 stored into value pointed to by ptr1

## 1.5 - Functions

Alright, it's time to do some actual programming!

### 1.5.1 - Implementation

Make a function! Go!

    int add(int a, int b)
    {
        return a + b;
    }
    
    int main()
    {
        int x = 7;
        int y = 12;
        int z = add(x, y);
    }

Done! Let's break it down:

    int                 //This is the type of the value that will be returned
        add             //This is the name of the function
        (   int a       //Function parameters are similar to declarations
          , int b       //A function can take any number of parameters
        )
    {
        return a + b;   //Most functions *must* return a value
    }

Simple, eh?
We can also take function parameters "by reference":

    void set12(int& a)
    {
        a = 12;
    }

A `void` function is one with no return value.
You can still use `return` in a `void` function, though:

    void doNothing()
    {
        return;
    }
    
    void doMoreNothing()
    {
        return doNothing();
    }

## 1.5.2 - Overloading

What if we want two functions with the same name,
but take different paramters?

Well, you just do it!
This is called "overloading":

    int add(int a, int b)
    {
        return a+b;
    }
    
    double add(double a, double b)
    {
        return a+b;
    }
    
    int main()
    {
        int x = add(7, 12);         //Calls the int version
        double y = add(7.7, 12.0);  //Calls the double version
    }

Easy.

## 1.5.3 - Default Parameters

You can specify certain parameters to be optional
by giving them a default value:

    int add(int a, int b = 12)
    {
        return a+b;
    }
    
    int main()
    {
        int x = add(32); //Same as add(32, 12);
    }

Even in the middle of the paramter list:

    int add(int a, int b = 12, int c)
    {
        return a+b+c;
    }
    
    int main()
    {
        int x = add(32, , 50); //Same as add(32, 12, 50);
    }

Obviously, default parameters are usually only useful as the last parameters.
