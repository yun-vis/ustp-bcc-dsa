---
# permalink: /about/
layout: single
title: "C# Basics II"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-02-25
---

# C# Basic Syntax (PART II)

## Using Methods

A method is a code block that contains a series of statements.

In C#, a method declaration includes
OptionalModifier ReturnType TheMethodName( ParameterType parameterName )
Also note that **return** only takes one parameter.

### Variables Inside Methods (The Scope of Methods)

Parameters and variables declared inside of a method can be only used under the scope of the method.
Return type should be matched.
A method can have optional parameters

```csharp
using System;

namespace CRC_CSD_03; // File scoped namespaces

class Program
{
    static void Main(string[] args)
    {
        // Any of the following are valid method calls.
        AddSomeNumbers(1);          // Returns 6.
        AddSomeNumbers(1, 1);       // Returns 4.
        AddSomeNumbers(3, 3, 3);    // Returns 9.

        int value = 0;
        if (args.Length == 0)
        {
            Console.WriteLine($"The value is {value}.");
        }
        else if (args.Length == 1)
        { 
            // int.Parse is a system method: Convert string to int
            value = AddSomeNumbers(int.Parse(args[0]));
            Console.WriteLine($"The value is {value}.");
        }
        else if (args.Length == 2)
        { 
            value = AddSomeNumbers(int.Parse(args[0]), int.Parse(args[1]));
            Console.WriteLine($"The value is {value}.");
        }
        else
        { 
            value = AddSomeNumbers(int.Parse(args[0]), int.Parse(args[1]), int.Parse(args[2]));
            Console.WriteLine($"The value is {value}.");
        }

        // a = 10; // compile error! The scope for a is not correct!
        PrintMyName("Yun");
    }

    /*
    Main: The class of my main program, where the application begins.
    Input:
        x: int
        y: int, optional
        z: int, optional
    Output:
        int
    */
    static int AddSomeNumbers(int x, int y = 3, int z = 2)
    {
        int a = 5;

        // return int
        return x + y + z + a;
    }

    // return void, nothing to return
    static void PrintMyName(string myName)
    {
        Console.WriteLine($"My name is {myName}.");
    }
}
```

### System Methods

Math.Sqrt() is a Math class method to calculate the square root of a value. "." operator allows us to access a method of a class.
```csharp
Console.WriteLine(Math.Sqrt(9));
```
```bash
$ 3
```

### Call by Value vs. Call by Reference
Passing parameters in a method can be done by deep copying or referencing.

```csharp
namespace CRC_CSD_03; // File scoped namespaces

using System;

class Program
{
    static void Main(string[] args)
    {
        int number = 4;
        SquareVal(number);
        Console.WriteLine(number);
        // Output: 4

        // ref is used to state that the parameter passed may be modified by the method.
        SquareRef(ref number);
        Console.WriteLine(number);
        // Output: 16

        int result;        
        // in is used to state that the parameter passed cannot be modified by the method.
        // out is used to state that the parameter passed must be modified by the method.
        SquareRef(in number, out result);
        Console.WriteLine(result);
        // Output: 256
    }

    /*
    SquareVal: square a number.
    Input:
        num: int
    Output:
        none
    */
    static void SquareVal(int num)
    {
        num *= num; // num = num * num
    }

    /*
    SquareRef: square a number.
    Input:
        num: ref int
    Output:
        none
    */
    static void SquareRef(ref int num)
    {
        num *= num; // num = num * num
    }

    static void SquareRef(in int num, out int result)
    {
        // This will generate an error
        // num *= num; // num = num * num
        result = num * num;
    }
}
```
```bash
$ 4
$ 16
$ 256
```

---
# Computational Thinking 

## Recursion vs. Iteration

Factorial of a Given Number can be implemented using **Recursion** or **Iteration**.

```csharp
namespace CRC_CSD_03;

using System;

class Program
{
    static void Main(string[] args)
    {
        int num = 5;
        Console.WriteLine("Factorial of " + num +
                            " using Recursion is: " +
                            FactorialUsingRecursion(num));

        Console.WriteLine("Factorial of " + num +
                            " using Iteration is: " +
                            FactorialUsingIteration(num));
    }

    /*
    FactorialUsingRecursion: find factorial of given number using recurrsion.
    Input:
        num: int
    Output:
        int
    */
    static int FactorialUsingRecursion(int n)
    {
        if (n == 1)
            return 1;

        // recursion call
        return n * FactorialUsingRecursion(n - 1);
    }

    /*
    FactorialUsingIteration: find factorial of given number using iteration.
    Input:
        num: int
    Output:
        int
    */
    static int FactorialUsingIteration(int n)
    {
        int res = 1;

        // using iteration
        for (int i = 2; i <= n; i++) 
        {
            res *= i;
        }

        return res;
    }
}
```
```bash
$ Factorial of 5 using Recursion is: 120
$ Factorial of 5 using Iteration is: 120
```


Fibonacci Sequence can be implemented using **Recursion** or **Iteration**.

```csharp
namespace CRC_CSD_03;

using System;

class Program
{
    static void Main(string[] args)
    {
        // initialization
        int num = 5;
        int[] sequence = new int[num];

        // Calculate and print out the Fibonacci Sequence
        // Recurrsion
        resetFibonacciSequence(sequence);
        Console.Write("A Fibonacci Sequence of elements " + num +
                            " using Recursion is: ");
        FibonacciUsingRecursion(num, sequence);
        printFibonacciSequence(sequence);

        // Iteration
        resetFibonacciSequence(sequence);
        Console.Write("A Fibonacci Sequence of elements " + num +
                            " using Iteration is: ");
        FibonacciUsingIteration(num, sequence);
        printFibonacciSequence(sequence);
    }

    /*
    resetFibonacciSequence: reset the sequence
    Input:
        sequence: int[]
    Output:
        none
    */
    static void resetFibonacciSequence(int[] sequence)
    {
        for (int i = 0; i < sequence.Length; i++)
        {
            sequence[i] = -1;
        }
    }

    /*
    printFibonacciSequence: print the sequence
    Input:
        sequence: int[]
    Output:
        none
    */
    static void printFibonacciSequence(int[] sequence)
    {
        for (int i = 0; i < sequence.Length; i++)
        {
            Console.Write(sequence[i] + " ");
        }
        Console.WriteLine("");
    }

    /*
    FibonacciUsingRecursion: find the Fibonacci sequence of a given element 
    number using recurrsion.
    Input:
        num: int
        sequence: int[]
    Output:
        int
    */
    static int FibonacciUsingRecursion(int num, int[] sequence)
    {
        if (num == 1)
        {
            sequence[0] = 0;
            return 0;
        }
        else if (num == 2)
        {
            sequence[0] = 0;
            sequence[1] = 1;
            return 1;
        }
        else
        {
            // recursion call
            int value = FibonacciUsingRecursion(num - 2, sequence) 
                        + FibonacciUsingRecursion(num - 1, sequence);
            sequence[num - 1] = value;
            return value;
        }
    }

    /*
    FibonacciUsingIteration: find the Fibonacci sequence of a given element 
    number using iteration.
    Input:
        num: int
        sequence: int[]
    Output:
        none
    */
    static void FibonacciUsingIteration(int num, int[] sequence)
    {
        if (num == 1)
        {
            sequence[0] = 0;
            return;
        }
        else if (num == 2)
        {
            sequence[0] = 0;
            sequence[1] = 1;
            return;
        }
        else
        {
            sequence[0] = 0;
            sequence[1] = 1;
            // using iteration
            for (int i = 2; i < num; i++)
            {
                sequence[i] = sequence[i - 2] + sequence[i - 1];
            }
        }
    }
}
```
```bash
$ A Fibonacci Sequence of elements 5 using Recursion is: 0 1 1 2 3
$ A Fibonacci Sequence of elements 5 using Iteration is: 0 1 1 2 3
```

---
# Selected Theory

## Naming Convention [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)

### Pascal Case
Use pascal casing ("PascalCasing") when naming a class, record, or struct. When naming public members of types, such as fields, properties, events, methods, and local functions, use pascal casing. When naming an interface, use pascal casing in addition to prefixing the name with an I. This clearly indicates to consumers that it's an interface.

### Camel Case
Use camel casing ("camelCasing") when naming private or internal fields, and prefix them with _.

## Comments, an Example
```csharp
// This is a single line comment

/*
This is a multi-line comment
and continues until the end
of comment symbol is reached
*/

/*
The following codes show how I often add comments to my programs.
This is a my application namespace, called My Business.
*/
namespace CRC_CSD_01;
/*
The class of my main program
*/
class Program
{
    /*
    Main: The class of my main program, where the application begins.
    Input:
      args: input parameters
    Output:
      none
    */
    static void Main(string[] args)
    {
        System.Console.WriteLine("Hello World!");
    }
}
```

---
# External Resources

- [Method Overloading/Function Overloading](https://www.w3schools.com/cs/cs_method_overloading.php)
- [out (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out)
- [ref (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/ref)
- [Heap Memory vs. Stack Memory](https://www.geeksforgeeks.org/dsa/stack-vs-heap-memory-allocation/)