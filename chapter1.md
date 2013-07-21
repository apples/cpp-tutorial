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

If you do not return anything at all from `main()`,
it will return `0` by default.
This only works for `main()` and is *not* true for other functions.

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

Those five operators can be used with assignment:

    int var1 = 16;
    var1 += 2;

Don't think that the `%` operator has anything to do with modulus,
it simply returns the remainder of integer division:

    int x =  13 / 5;    // x == 2
    int y =  13 % 5;    // y == 3
    int z = -13 % 5;    // z == -3

Although, for positive numbers, it's the same as modulus.

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
| <code>a &#124;&#124; b</code> | OR
| `!a`     | NOT

<!-- Plaintext readers: The second row above should read "| `a || b` | OR " -->

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

## 1.3 - Functions

Alright, it's time to do some actual programming!

### 1.3.1 - Implementation

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

### 1.3.2 - Forward Declarations

So functions are cool and all, but they have a certain limitation.
You can only use a function after it has been declared.
This is sometimes inconvenient;
what if we want to use a function before it has been implemented?

Forward declarations solve the problem!

    int add(int, int);
    
    int main()
    {
        int x = 7;
        int y = 12;
        int z = add(x, y);
    }
    
    int add(int a, int b)
    {
        return a + b;
    }

Works fine!
Note the semicolon after the declaration.
Also:

    int add(int first, int second);
    
    int main()
    {
        int x = 7;
        int y = 12;
        int z = add(x, y);
    }
    
    int add(int a, int b)
    {
        return a + b;
    }

Giving the variable names in the declaration is optional,
and they don't even have to match with the actual names
in the implementation.
They're more like comments than anything,
but you should try to make them match anyway.

### 1.3.3 - Overloading

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

## 1.4 - Console IO

Ah, finally, IO!
Simple input and output, as it were.
Communicating with the user is important and easy in C++.

To use console IO, you need to `#include <iostream>`:

    #include <iostream>
    // IO streams are now available.
    
    int main()
    {
        // IO stuff
    }

We'll cover streams in detail later on,
but it's always nice to be able to interact with
your programs for learning purposes.

### 1.4.1 - Console Output

Outputting basic datatypes could not be easier:

    #include <iostream>
    
    int main()
    {
        std::cout << "Seven: " << 7 << std::endl;
    }

So `std::cout` is the console output stream.
By using the insertion operator `<<`,
you can send any compatible type to the console.
All basic datatypes are compatible.

    #include <iostream>
    
    int main()
    {
        int i = 7;
        int j = 12;
        std::cout << "i   = " << i   << std::endl;
        std::cout << "j   = " << j   << std::endl;
        std::cout << "j+k = " << i+j << std::endl;
    }

The `std::` means that `cout` is a member of the `std` namespace.
Anyhting in `std::` is part of the standard library.

    #include <iostream>
    
    int main()
    {
        int myVar = 12;
        std::cout << "myVar = ";
        std::cout << myVar;
        std::cout << std::endl;
    }
    
The `std::endl` is a special object
that makes the stream go to the next line
and force it to display on the screen.
This is known as "flushing" the stream,
and must be done to guarantee that the user sees the data.

### 1.4.2 - Console input

Taking input from the console is almost as easy as outputting to it.

    #include <iostream>
    
    int main()
    {
        int in;
        std::cin >> in;
    }

Here we see `std::cin` (console input) and the extraction operator `>>`.
It works just like `std::cout`,
except instead of printing a value to the console,
the user provides the value by typing.

Let's make a simple addition calculator:

    #include <iostream>
    
    int main()
    {
        int a;
        int b;
        
        std::cout << "First  value: ";
        std::cin >> a;
        
        std::cout << "Second value: ";
        std::cin >> b;
        
        std::cout << "Sum of values: " << a+b << std::endl;
    }

It works, try it.
You have to press enter (send an `endl`) to the console
for it to give the value to the program.
After all, `std::cout` and `std::cin` are tied to each other.
Luckily, that's the user's job this time!

## 1.5 - Strings

Numerical types are all fine,
but what if we want to represent text?
In the old days, you would use an array of characters,
but C++ has a fancy feature called `std::string`.
You can gain access to `std::string` by using `#include <string>`.

    #include <string>
    
    int main()
    {
        std::string myStr = "Hello!";
    }

### 1.5.1 - Using Strings

The `std::string` class is used just like any other type,
and supports copy, move, and concatenation,
among other things.

    #include <string>
    
    int main()
    {
        std::string one = "One";
        std::string two = "Two";
        
        std::string onetwo = one + " and " + two;
        // onetwo == "One and Two"
    }

The things in double-quotes `" "` are not `std::string`s,
they are actually `const char*`,
or character arrays,
or just "string literals".
Obviously, they can be converted to a `std::string`.

    #include <iostream>
    #include <string>
    
    int main()
    {
        std::string str = "Hello, world!";
        std::cout << str << std::endl;
    }

Of course they support insertion and extraction.

    #include <iostream>
    #include <string>
    
    int main()
    {
        std::string in;
        std::string name;
        
        std::cout << "What is your first name? ";
        std::cin >> in;
        name += in;
        
        name += " ";
        
        std::cout << "What is your last name? ";
        std::cin >> in;
        name += in;
        
        std::cout << "Hello, " << name << "!" << std::endl;
    }

By the way, to get an entire line out of `std::cin`,
instead of just a single word,
you can use `getline()`:

    #include <iostream>
    #include <string>
    
    int main()
    {
        std::string name;
        
        std::cout << "What is your entire name? ";
        getline(std::cin, name);
        
        std::cout << "Hello, " << name << "!" << std::endl;
    }

### 1.5.2 - String Conversions

The `<string>` library provides many functions to convert between
`std::string`s and basic arithmetic types, such as `int` and `double`.

    #include <string>
    
    int main()
    {
        int i = 7;
        
        std::string myStr = std::to_string(i);
        
        int x = std::stoi(myStr);
    }

The `to_string()` function takes almost any arithmetic type
and converts it to a `std::string`.
To convert back to an arithmetic type,
you can use `std::stoi` to convert to an `int`,
and `std::stod` to get a `double`.

There are many more conversion functions,
but they are not necessary right now,
and will become apparent if you ever actually use a different type.

## 1.6 - Arrays                                                                                                                                                                                                                                                          

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

An empty list will initialize the whole array to `0`.

    int arr1[5] = {};

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

## 1.7 - Flow Control

Programming would be pretty boring if we couldn't tell the computer
how to make decisions.
Luckily, we can.

### 1.7.1 - Branching

Branching is the act of making a decision and changing the flow of the program.

#### 1.7.1.1 - `if`

The `if` statement is one of the most basic branching constructs.
Let's start with a complete program as an example:

    int main()
    {
        int x = 12;
        
        if (x == 12)
        {
            x = 7;
        }
        
        return 0;
    }

The final value of `x` is `7`. Let's dissect the `if` statement.

    if                  //the keyword itself
        (               //these parentheses are necessary
            x == 12     //the condition: test if x is equal to 12
        )
    {                   //this block is only executed if the condition is true
        x = 7;
    }

Since the branch block only contains one statement,
we could also have written it like this:

    int main()
    {
        int x = 12;
        
        if (x == 12) x = 7;
        
        return 0;
    }

Note that the condition must be a `bool`.
If it is not a `bool`,
it will be implicitly converted into a `bool`.
For example, when an `int` is converted to a `bool`,
if it is zero,
it's `bool` value is `false`,
otherwise, it is `true`.

This is pretty simple, but `if` is not complete without...

#### 1.7.1.2 - `else`

An `else` block executes only if the preceding `if`'s condition is `false`.
Since an `if`'s code block is only executed when the condition is `true`,
the two blocks are mutually exclusive.

    int x = 12;
    
    if (x == 13)
    {
        x = 1;
    }
    else
    {
        x = 2;
    }

Spoiler alert: `x`'s final value is `2`.

Following the above example of one-line branches:

    int x = 12;
    
    if (x == 13) x = 1;
    else         x = 2;

And of course we can have nested branches:

    int x = 12;
    
    if (x == 10)
    {
        x = 1;
    }
    else
    {
        if (x == 11)
        {
            x = 2;
        }
        else
        {
            x = 3;
        }
    }

Using the one-line technique,
we can flatten that last bit:

    int x = 12;
    
    if (x == 10)
    {
        x = 1;
    }
    else if (x == 11)
    {
        x = 2;
    }
    else
    {
        x = 3;
    }

Even flatter:

    int x = 12;
    
    if      (x == 10) x = 1;
    else if (x == 11) x = 2;
    else              x = 3;

So flat.

### 1.7.2 - Looping

Looping constructs allow us to repeat an action several times
until a condition is met.

#### 1.7.2.1 - `while`

A `while` loop is very similar to an `if` branch,
but it will repeat the block over and over until the condition is false.

    int x = 0;
    
    while (x < 20)
    {
        ++x;
    }

Final `x`: `20`. Break it down!

    while               //keyword
        (               //parentheses necessary
            x < 20      //condition: x is less than 20
        )
    {
        ++x;            //increment x (i.e. x += 1)
    }

The one-line trick applies here, too:

    int x = 0;
    
    while (x < 20) ++x;

If the condition is `false` when the `while` is first encountered,
the block will not be executed at all.

If you want it to execute the block at least once,
then we have...

#### 1.7.2.2 - `do`

A `do` loop is exactly like a `while` loop,
except that it is guaranteed to run at least once.

    int x = 20;
    
    do
    {
        ++x;
    } while (x < 20);

The one-line trick keeps happening:

    int x = 20;
    
    do ++x;
    while (x < 20);

I don't think it can get any simpler than that.

#### 1.7.2.3 - `break`

The `break` statement is used in very few places.
Two of those places are `switch` blocks and any kind of loop.

It is used to force the loop to immediately stop.
The rest of the current iteration will not execute,
and control will jump to the first line after the loop.

    int x = 20;
    
    while (x<20)
    {
        if (x == 10) break;
        ++x;
    }

When `x` becomes `10`, the loop will `break`,
and `x`'s final value is `10`.

#### 1.7.2.4 - `continue`

Very similar to `break`,
`continue` will force the current iteration to stop,
but the loop will not end,
instead jumping back to the start of the loop.

    int x = 0;
    int y = 0;
    
    while (x < 20)
    {
        ++x;
        if (x <= 10) continue;
        ++y;
    }

If `x` is les than or equal to `10`,
control woll skip over the last part of the loop,
and `y` will not increment.
Therefore, `x` becomes 20 as usual,
and `y` becomes `10` because the first eleven iterations of the loop
skipped the increment of `y`.

### 1.7.3 - Iterating

Alright, it's time for some serious programming.
Iteration is the driving force of any
game, database, or similarly scalable application.

These constructs are similar to normal loops,
but they're designed to operate on ranges and lists.

#### 1.7.3.1 - FOR

The `for` loop is probably the most common looping construct.
A `while` only has a condition,
but a `for` has an initialization, condition, and iteration.

They are particularly good for messing with arrays:

    int nums[20];
    
    for (int i=0; i<20; ++i)
    {
        nums[i] = i*i;
    }

Smash it to tiny bits!

    for                 //keyword
        (               //parentheses necessary
            int i=0;    //initialize a new variable i to zero
            i<20;       //perform the loop while i is less than 20
            ++i         //increment i each iteration (except the first)
        )
    {
        nums[i]         //since arrays are zero-indexed, i can be 0-19
            = i*i;      //assign the square of i to the ith value of nums
    }

Got it?
Let's try to do the same thing using a `while` loop:

    int nums[20];
    
    {
        int i=0;
        while (i<20)
        {
            nums[i] = i*i;
            ++i;
        }
    }

Quite a bit uglier; let's try to avoid that whenever possible.
We'll talk about that outer brace block later, just roll with it for now.

Each of the three sections of the `for` statement are optional.
Simply don't put anything in that section, as seen here:

    int nums[20];
    int i=0;
    
    for (; i<20; ++i)
    {
        nums[i] = i*i;
    }

If all three sections are absent,
the loop will continue indefinitely
unless stopped by a jump.

    for (;;)
    {
        //infinite loop!
    }

This is exactly equivalent to:

    while (true)
    {
        //also infinite loop!
    }

Try to not get stuck!

## 1.8 - Scope

Curly braces are your friend.
C++ is a lexically scoped language,
which means that variables can only be seen by blocks of code
that can clearly see them.

### 1.8.1 - Namespace Scope

C++ has a useful (some say) feature called namespaces.
A `namespace` is simply a collection of objects and functions
that belong together.
For example, all of the standard libraries are in the namespace `std`.

    #include <iostream>
    int main()
    {
        std::cout << "Hello!" << std::endl;
    }

Anything not in a namespace is said to be in the "global" namespace.

    int i;
    int main()
    {
        ::i = 7;
    }

Of course, the global namespace is usually implicit,
so `::i` could be replaced with just `i`.

We can make a specific namespace be implicit for the current scope
with `using namespace Name;`.

    #include <iostream>
    using namespace std;
    int main()
    {
        cout << "Hello!" << endl;
    }

Make sure you understand that the namespace is implicit
only for the current scope.
This become important in the next section.

### 1.8.2 - Block Scope

Anything with curly braces is a block.
A block can only see things in it's own scope
or a higher scope.

You can simply use empty braces:

    int i;
    /* i can be used here */
    {
        int j;
        /* i and j can be used here */
    }
    /* j is no longer in scope, only i can be used here */

Of course,
all functions and flow constructs have their own scope:

    int i;
    int main()
    {
        int j;
        if (j == i)
        {
            int k;
        }
        for (int a=0; a<10; ++a)
        {
            int l;
        }
    }

Even if there are no braces,
flow constructs still have their own scope:

    int main()
    {
        int x;
        for (int i=0; i<10; ++i) x += i;
    }

You cannot give two variable the same name,
unless they're in different scopes.
When a variable is given the same name as an exisiting variable,
it will "shadow" the old variable,
as seen here:

    int x = 10;
    {
        int x = 20;         //Not the same x!
        cout << x << endl;  // Prints "20"
    }
    cout << x << endl;      // Prints "10"

Some constructs allow variables to be declared in their parameters.

For example,
the variable declared in a `for` loop is block-scoped in that `for` block,
but it's declared outside of the curly braces:

    for (int i=0; i<10; ++i) // i declared here
    {
        // Do stuff with i
    }
    // i is no longer in scope

Other constructs also support this kind of scoping,
but these are less idiomatic.

    if (int i = someFunc())
    {
        //use i
    }

    while (int i = someFunc())
    {
        //use i
    }

This is what allows us to have two `for` loops that use the same variable name:

    for (int i=0; i<10; ++i)
    {}
    
    for (int i=0; i<300; ++i)
    {}

## 1.9 - References

As you may have noticed, it is impossible to transfer data
across scope gaps without copying it.
However, some smart people came up with the idea of "indirection".

    void increment(int& x)
    {
        ++x; // Refers to i below
    }
    
    int main()
    {
        int i = 7;
        increment(i);
    }

A datatype followed by an ampersand (`&`) is a reference.
References are a mechanism to see and modify data
that is not viewable in the current scope.

References can be copied just like any normal datatype,
but they cannot be created without something to refer to,
and they cannot be reassigned to refer to something else.

    int add(int a, int b)
    {
        return a + b;
    }
    
    void increment(int& x)
    {
        x = add(x, 1);
    }
    
    int main()
    {
        int i = 7;
        increment(i);
    }

When a reference is assigned to an actual variable,
the thing that is referred to is copied.

    int main()
    {
        int     i = 7;
        int& iref = i;    // takes reference to i
        int     k = iref; // copies i into k
    }

After a reference is created,
trying to assign to it will assign to the thing that is referred to.

    int main()
    {
        int  i = 7;
        int& a = i; // refers to i
        a = 12;     // copies 12 into i
        
        int j = 76;
        a = j;        // copies j into i
    }

Aside from avoid scope limitations,
references are also used to avoid copying large amounts of data.
For example,
arrays can be quite large and difficult to pass to functions
unless you take it by reference:

    int sum(int (&arr)[5])
    {
        return arr[0]+arr[1]+arr[2]+arr[3]+arr[4];
    }
    
    int main()
    {
        int myArr[5] = {1, 2, 3, 4, 5};
        int    mySum = sum(myArr);
    }

The syntax for a reference to an array (`int (&arr)[5]`)
is only slightly different from an array of references (`int& arr[5]`),
and it's illegal to make an array of references,
so be careful.

Another common large structure is a `std::string`:

    #include <string>
    
    void addStuff(std::string& str)
    {
        str += ", world!";
    }
    
    int main()
    {
        std::string myStr = "Hello";
        addStuff(myStr);
        // myStr == "Hello, world!"
    }

## 1.10 - Constants

The `const` modifier makes any type immutable.
Immutable objects cannot be altered in any way.

    int main()
    {
        const int i = 7;
        /* i = 12; */ // ILLEGAL
    }

Most often, `const` is used with references
to make sure that the referred-to value is not accidentally modified.

    int double(const int& x) //cannot modify x (aka i)
    {
        return 2*x;
    }
    
    int main()
    {
        int i = 7;
        int j = double(i);
    }

It's especially important to use `const` wherever possible.
It helps prevent several very common errors.

    #include <iostream>
    #include <string>
    
    void printString(const std::string& in)
    {
        std::cout << in << std::endl;
    }
    
    int main()
    {
        std::string str1 = "Hello...";
        std::string str2 = "world!";
        
        printString(str1);
        printString(str2);
    }

Oh, and when you use a `const` reference,
you can do cool things like this:

    #include <iostream>
    #include <string>
    
    void printString(const std::string& in)
    {
        std::cout << in << std::endl;
    }
    
    int main()
    {
        printString("Hello...");
        printString("world!");
    }

The literal strings there are converted into `std::string`s
when passed to the function.
Since the created `std::string`s are temporary,
you normally wouldn't be able to take a reference to them
(because they don't really exist anywhere),
but since we're sending the temporary values to a
`const` reference, it's allowed.

## 1.11 - Classes

This tutorial has been pretty boring so far, eh?
It's time to throw some excitement in here.

Yay.

### 1.11.1 - Structures

Arrays are good for storing many values,
but they can only hold a single type of value.

Enter `struct`.

    struct MyType
    {
        int i;
        double d;
    };
    
    int main()
    {
        MyType var1;
        var1.i = 7;
        var1.d = 5.8;
    }

Please notice the semicolon (`;`) after the closing curly brace (`}`).
It is very important.

Structures are similar to arrays,
but they hold many different types of values,
and each value has a name instead of an index.

    struct Point
    {
        int x;
        int y;
    };
    
    int main()
    {
        Point p;
        p.x = 7;
        p.y = 12;
    }

They can be treated almost exactly like the basic datatypes.

    struct Point
    {
        int x;
        int y;
    };
    
    Point addPoints(const Point& a, const Point& b)
    {
        Point rval;
        rval.x = a.x + b.x;
        rval.y = a.y + b.y;
        return rval;
    }
    
    int main()
    {
        Point p1;
        Point p2;
        
        p1.x = 7;
        p1.y = 12;
        
        p2.x = 87;
        p2.y = -6;
        
        Point p3 = addPoints(p1, p2);
    }

They can (usually) be copied, moved, and otherwise modified.

    struct Point
    {
        int x;
        int y;
    };
    
    int main()
    {
        Point p1;
        p1.x = 7;
        p1.y = 12;
        
        Point& pRef = p1;
        pRef.y = 90;
        
        Point p2 = pRef;
        p2.x = 54;
    }

You can list-initialize them just like arrays.

    struct Point
    {
        int x;
        int y;
    };
    
    Point addPoints(const Point& a, cont Point& b)
    {
        return {a.x + b.x, a.y + b.y};
    }
    
    int main()
    {
        Point p{7, 12};
    }

Since they are treated just like basic datatypes,
you can even nest them.

    struct Point
    {
        int x;
        int y;
    };

    struct Player
    {
        Point position;
        Point velocity;
        int health;
    };
    
    int main()
    {
        Player player{{7, 12}, {-5, 43}, 100}; //nested list initializers
        player.velocity.y += 10;
        --player.health;
    }

Looks like we're finally getting into the fun stuff, eh?
You ain't seen *nothing* yet.

### 1.11.2 - Classes

Classes are very similar to structs.

    class Point
    {
        public:
            int x;
            int y;
    };

Except they come with a lot of additional features.
Ignore the `public:` for now.

**NOTE:** In modern C++, structs and classes are essentially the same,
except for the default access modifiers.
However, it is still common convention to use structs for simple data,
and classes for more complex things.

#### 1.11.2.1 - Member Functions

Classes can contain functions that act upon their data.

    class Point
    {
        public:
            int x;
            int y;
            
            void reset()
            {
                x = 0;
                y = 0;
            }
    };
    
    int main()
    {
        Point p{7, 12};
        p.reset();
    }

Member functions act upon the object that they are invoked with.
In the example above, in `reset()`,
`x` and `y` are synonymous with `p.x` and `p.y`.
The idea is that the specific `Point` called `p`
is invoking `reset()` on itself.

    #include <iostream>
    
    class Point
    {
        public:
            int x;
            int y;
            
            void add(const Point& in)
            {
                x += in.x;
                y += in.y;
            }
    };

    class Player
    {
        public:
            Point position;
            Point velocity;
            int health;
            
            void move()
            {
                position.add(velocity);
            }
            
            void hurt(int pain)
            {
                health -= pain;
                std::cout << "Ouch!" << std::endl;
            }
    };
    
    int main()
    {
        Player player{{7, 12}, {4, -2}, 100};
        player.move();
        player.hurt(5);
    }

That *almost* looks like a real program!

#### 1.11.2.2 - Access Modifiers

So you've probably inferred that,
if there is a `public`, there must be a `private`.
You might just be correct.

    class MyClass
    {
        private:
            int nope;
    };
    
    int main()
    {
        MyClass var;
        /* var.nope = 12; */ // ILLEGAL
    }

Private members cannot be accessed directly
except by the member functions of that class.

    class MyClass
    {
        public:
            int& getVal() // note the reference
            {
                return val;
            }
        
        private:
            int val;
    };
    
    int main()
    {
        MyClass var;
        var.getVal() = 12;
    }

This doesn't appear very useful;
let's look at a more common design pattern:

    class MyClass
    {
        public:
            int getVal() const
            {
                return val;
            }
            
            void setVal(int i)
            {
                val = i;
            }
        
        private:
            int val;
    };
    
    int main()
    {
        MyClass var;
        var.setVal(12);
        int i = var.getVal();
        // i == 12
    }

Placing `const` after a function's parameters will ensure that
we cannot accidentally modify the member variables.

This is useful when we want to do weird things to the values.

    class MyClass
    {
        public:
            int getVal() const
            {
                return val/5;    // divide by 5 when getting
            }
            
            void setVal(int i)
            {
                val = i*5;    // multiply by 5 when setting
            }
        
        private:
            int val;
    };
    
    int main()
    {
        MyClass var;
        var.setVal(12);
        int i = var.getVal();
        // i == 12
    }

Getters and setters are far more common in languages such as Java,
but, in my opinion, they tend to clutter things up a bit.

    class Point
    {
        public:
            int getX() const
            {
                return x;
            }
            
            void setX(int i)
            {
                x = i;
            }
            
            int getY() const
            {
                return y;
            }
            
            void setY(int i)
            {
                y = i;
            }
            
        private:
            int x;
            int y;
    }

Quite a lot of code for so little benefit.
Of course, such a simple class should be a struct, anyway.

#### 1.11.2.3 - Constructors

Earlier, we used initializer lists to set the inital values of a `struct`.
Classes provide us with ways to control the initialization of objects
more easily than with structs.

    class Point
    {
        public:
            int x;
            int y;
            
            //constructor
            Point() : x(0), y(0)
            {}
    };
    
    int main()
    {
        Point p;
    }

The constructor is invoked upon the creation
(or "instantiation") of the object.
A constructor with no arguments is a "default" constructor.

Constructors can be overloaded just like functions.

    class Point
    {
        public:
            int x;
            int y;
            
            Point()
                : x(0)
                , y(0)
            {}
            
            Point(int a, int b)
                : x(a)
                , y(b)
            {}
    };
    
    int main()
    {
        Point p1;
        Point p2(4, 5);
        Point p3{2, 7};
    }

The comma-separated list after the `:` in the constructor
is the constructor list.
This is where the constructors for member objects are invoked.

If a list is given as an initializer and there is a matching constructor,
the constructor will be called
instead of directly initializing values from the list.

    class Point
    {
        public:
            int x;
            int y;
            
            Point()
                : x(0)
                , y(0)
            {}
            
            Point(int a, int b)
                : x(a)
                , y(b)
            {}
    };
    
    class Player
    {
        public:
            Point pos;
            Point vel;
            int health;
            
            Player()
                : pos{0, 0}
                , vel{0, 0}
                , health{100}
            {}
    };

#### 1.11.2.4 - Copying

There are several special constructors,
the first being the "default", which we've already seen.
Another special constructor is the "copy" constructor.

    class Point
    {
        public:
            int x;
            int y;
            
            Point(const Point& in)
                : x(in.x)
                , y(in.y)
            {}
    };
    
    int main()
    {
        Point p1{7, 12};
        Point p2(p1);
    }

Although a copy constructor is usually automatically generated,
sometimes we don't want to copy every part of another object.

    class Point
    {
        public:
            int x;
            int y;
            int useless;
            
            Point(const Point& in)
                : x(in.x)
                , y(in.y)
                , useless(0) // ignore in.useless
            {}
    };
    
    int main()
    {
        Point p1{7, 12, 10};
        Point p2(p1);
    }

## 1.12 - Templates

This is where C++ really stands apart from other languages.
Templates allow the programmer to participate in "metaprogramming",
which is a technique to generate code by using other code.

    template <typename T>
    T add(const T& a, const T& b)
    {
        return a+b;
    }
    
    int main()
    {
        int    x = add(3, 7);
        double d = add(3.5, 7.12);
    }

You can think of it as "overloading a function for all possible types",
but sometimes that's not entirely true.

Classes can be templatized as well.

    template <typename T>
    class MyClass
    {
        public:
            T val;
    };
    
    int main()
    {
        MyClass<int> x;
        x.val = 7;
        
        MyClass<double> y;
        y.val = 23.65;
    }

Templates are very powerful and should be used for generic programming.
After all, what good is an `add()` function that only accepts `int`?

You can specialize (think "overload") templates as well.

    template <typename T>
    class MyClass
    {
        public:
            T val;
    };
    
    template <>
    class MyClass<int>
    {
        public:
            int intVal;
    };
    
    int main()
    {
        MyClass<int> x;
        x.intVal = 7;
        
        MyClass<double> y;
        y.val = 23.65;
    }

Other than types, you can also take integers:

    template <int N>
    int sadd(int a)
    {
        return a+N;
    }
    
    int main()
    {
        int x = add<6>(3);
    }

It is impossible to give a dynamic value as a template parameter, though.

    template <int N>
    int sadd(int a)
    {
        return a+N;
    }
    
    int main()
    {
        int i = 12;
        int x = add<6>(i);
        /* int y = add<i>(7); */ // ILLEGAL
    }

## 1.13 - Automatic Type Deduction

### 1.13.1 - `auto`

Since we have generic templates now,
sometimes it is difficult or impossible to determine the type of something,
at least just by looking at it.
However, the compiler is smart and can do this for us.

    #include <iostream>
    
    template <typename T, typename U>
    void printSum(T a, U b)
    {
        auto sum = a + b; // What type is sum? Nobody cares.
        std::cout << sum << std::endl;
    }
    
    int main()
    {
        printSum(5, 7.8);
        printSum(5.12, 7);
    }

We can also use `decltype` to find the type of an expression.
In this next case, whatever type `a+b` is, `sum` will be the same type.

    #include <iostream>
    
    template <typename T, typename U>
    void printSum(T a, U b)
    {
        decltype(a+b) sum = a + b;
        std::cout << sum << std::endl;
    }
    
    int main()
    {
        printSum(5, 7.8);
        printSum(5.12, 7);
    }

Using `decltype`,
and a nice feature called "trailing returns",
we can write a proper summing function.

    template <typename T, typename U>
    auto sum(T a, U b) -> decltype(a+b)
    {
        return a+b;
    }
    
    int main()
    {
        auto   x = sum(4  , 7);
        double y = sum(4.5, 7);
    }

The `->` points to the return type of the function.
This allows us to inspect `a` and `b` before deciding on a return type.

You should only use `auto` and `decltype` in cases where it really is difficult
to determine the type of something.
Otherwise, you might actually make the code more difficult to read.
