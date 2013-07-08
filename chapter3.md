# 3 - Classes and the STL

## 3.1 - Structs

Until now, this tutorial has only dealt with basic datatypes.
If you've tried to make any kind of program,
you may have noticed that having 20 different variables,
all with single-letter names,
gets a bit confusing.

For this reason, C++ has a feature that is common to all
object-oriented programming languages:
`struct`.

A structure is a user-defined datatype
that is a collection of other datatypes.

For all you wannabe game devs out there,
a common structure you might have is a "Player".

    struct Player
    {
        int x;
        int y;
        double health;
    };

Well, you might need a bit more than that, but that's the example we're using.
Note tha `;`.
This means that the `struct` can be used as a statement,
so you can declare new structs inside of other structs and functions,
but that doesn't really affect much for the purposes of this tutorial.

Anyway, how do we use it?
Since a `struct` is a custom datatype,
we use it just like an `int` or `double`:

    Player p1;

And to access it's members:

    Player p1;
    p1.x = 12;
    p1.y = 15;
    p1.health = 100.0;

Each `Player` that we declare will have its own separate members:

    Player p1;
    Player p2;
    p1.health = 100.0;
    p2.health = 75.0;

And you can copy and reference structs just like anything else:

    void movePlayer(Player& p)
    {
        ++p.x;
        ++p.y;
    }
    
    int main()
    {
        Player p1;
        movePlayer(p1);
    }

Before we move on, how about a complete program:

    struct Player
    {
        int x;
        int y;
        double health;
    };
    
    #include <iostream>
    using namespace std;
    
    void movePlayer(Player& p)
    {
        ++p.x;
        ++p.y;
    }
    
    void printHealth(const Player& p1, const Player& p2)
        //const so that we don't accidentally modify the players
    {
        cout << "p1: " << p1.health << ";  p2: " << p2.health << endl;
    }
    
    void damagePlayer(const Player& p)
    {
        p.health -= 5.0;
    }
    
    int main()
    {
        Player p1;
        p1.x = 0;
        p1.y = 0;
        p1.health = 100.0;
        
        Player p2 = p1; //Copies p1
        
        printHealth(p1, p2);
        
        cout << "p1 attacks!" << endl;
        damagePlayer(p2);
        
        printHealth(p1, p2);
        
        cout << "p2 attacks!" << endl;
        cout << "p1 dodges!" << endl;
        movePlayer(p1);
        printHealth(p1, p2);
        
        return 0;
    }

Well, that was pretty bad.
But hey, it works, right?

Let's add a bit of functionality to our structures...

## 3.2 - Classes

Structs are all well and good,
but they lack a bit of functionality.
We've defined what a `Player` *has*,
but how do we define what a `Player` *does*?

Quite simply, we just define a function inside the `struct`:

    struct Player
    {
        int x;
        int y;
        double health;
        
        void hurt()
        {
            health -= 5.0;
        }
    };

And then:

    Player p1;
    p1.hurt();

Inside that function, all of `p1`'s member variables
are implicitly scoped.

### 3.2.1 - Classes vs. Structs

It's time to introduce `class`:

    class Player
    {
    public:
        int x;
        int y;
        double health;
        
        void hurt()
        {
            health -= 5.0;
        }
    };

Notice the `public:` label.
This specifies that all of the following declarations are `public`.
A `struct` is `public` by default, so this was unnecessary.
However, a `class` is `private` by default.

What does all that junk mean?

### 3.2.2 - Access Modifiers

There are three access modifiers: `public`, `private`, and `protected`.
Anything that's `public` is visible to everything.
Anything that's `private` is only visible to the containing `class` or `struct`.
Anything that's `protected` is visible to related classes.

Have a chart:

| Access      | Self  | Friends | Descendants | Everyone |
|:------------|:-----:|:-------:|:-----------:|:--------:|
| `public`    |   X   |    X    |      X      |     X    |
| `protected` |   X   |    X    |      X      |          |
| `private`   |   X   |    X    |             |          |

We'll talk about descendants and friends later;
feel free to refer back to this chart.

So if a `struct` is `private` by default, why use `class`?
To put it bluntly: because you should!
It's better to give explicit access to `public` members
than to have everything visible by default.
This way, you won't accidentally modify a value directly
that should not be modified.

Also, it's just common convention to use `struct` only for objects
that have only `public` members,
and no member functions.
If it has a function, or needs `private` members,
make it a `class`.

Enough babbling,
I want an example!

    class Player
    {
    public:
        void hurt() {health -= 5.0;}
        void getHealth() {return health;}
    private:
        double health;
    };

See how you cannot modify the player's health directly?
But wait!
How do we initialize the player's health if we cannot set it?

### 3.2.3 - Constructors

Use a constructor, silly!

    class Player
    {
    public:
        Player() //This is a constructor
        {
            health = 100.0;
        }
    
        void hurt() {health -= 5.0;}
        void getHealth() {return health;}
    private:
        double health;
    };

A constructor is called whenever an object of that class is instantiated.

    Player p1; //Constructor called right here

Constructors are implemented like functions that have no return value.
In fact, they can take parameters as well:

    Player(double hp)
    {
        health = hp;
    }

And called likewise:

    Player p1(80.0);

Also, you should use constructor lists whenever possible:

    class Player
    {
    public:
        Player()
            : x(0)
            , y(0)
            , health(100.0)
        {}
        
        Player(double hp)
            : x(0)
            , y(0)
            , health(hp)
        {}
        
        Player(double hp, int a, int b)
            : x(a)
            , y(b)
            , health(hp)
        {}
        
        void hurt() {health -= 5.0;}
        void getHealth() {return health;}
    private:
        int x;
        int y;
        double health;
    };

