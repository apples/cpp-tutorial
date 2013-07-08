# 2 - Flow Control and Scoping

This is the chapter where you learn all about how to tell your program
how to test boolean conditions and make decisions.

## 2.1 - Branching

Branching is the act of making a decision and changing the flow of the program.

### 2.1.1 - `if`

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

### 2.1.2 - `else`

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
We're only comparing `x` to two other values, though.
What if we want to compare it to ten values?
That brings us to...
 
### 2.1.3 - `switch`

Honestly, the `switch` statement is not entirely useful.
It only works for integer types,
and can only test equivalence.
It also
Nevertheless, a C++ tutorial would not be complete without this section.

First, an example:

    int x = 2;
    
    switch (x)
    {
    case 1:
        x = 10;
        break;
    case 2:
        x = 11;
        break;
    case 3:
        x = 12;
        break;
    default:
        x = -1;
    }

Notice the `break` statements.
They force a branch to the end of the `switch` block,
otherwise the next `case` would run.

So it's basically a big `if ... else if ... else ...` tree.

The above example can be expanded to:

    int x = 2;
    
    if      (x == 1) x = 10;
    else if (x == 2) x = 11;
    else if (x == 3) x = 12;
    else             x = -1;

Quite simple.

Remember:
`switch` blocks only work for integral types like `char` and `int`,
and they are usually not the best solution to a problem.

Due to these limitations, I will not go into detail about `switch`.

## 2.2 - Looping

Looping constructs allow us to repeat an action several times
until a condition is met.

### 2.2.1 - `while`

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

### 2.2.2 - `do`

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

### 2.2.3 - `break`

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

### 2.2.4 - `continue`

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

## 2.3 - Iterating

Alright, it's time for some serious programming.
Iteration is the driving force of any
game, database, or similarly scalable application.

These constructs are similar to normal loops,
but they're designed to operate on ranges and lists.

### 2.3.1 - FOR

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

## 2.4 - Scope

Curly braces are your friend.
C++ is a lexically scoped language,
which means that variables can only be seen by blocks of code
that can clearly see them.

### 2.4.1 - Namespace Scope

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

### 2.4.2 - Block Scope

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
    
    for (int i=0; i<20; ++i)
    {}

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
