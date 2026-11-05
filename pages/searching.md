---
# permalink: /about/
layout: single
title: "DS&A: Searching"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-05-11
---

## Console Project vs. Library Project

* Create the DataStructureLibrary classlib project
```bash
$ dotnet new classlib --name AlgorithmLibrary
```

* Do not forget to add the environment variables in MyBusiness.csproj
```csharp
  <ItemGroup>
    <ProjectReference
      Include="../DataStructureLibrary/DataStructureLibrary.csproj" />
    <ProjectReference
      Include="../AlgorithmLibrary/AlgorithmLibrary.csproj" />      
  </ItemGroup>
```

# What is Algorithm?
An algorithm is a precise, step-by-step set of instructions or rules designed to accomplish a specific task or solve a problem. It takes an input, processes it through a defined series of finite steps, and produces a desired output. 


# Searching Algorithms

A search algorithm is defined as a precisely defined procedure for locating specific data within a structure.

## Linear Search

In MyBusiness/Program.cs,
```csharp
using AlgorithmLibrary.LinearSearch;
using AlgorithmLibrary.BinarySearch;

namespace MyBusiness;

class Program
{
    static void Main(string[] args)
    {
        // Input validation
        if (args.Length != 2)
        {
            Console.WriteLine("Sorting type is not specified or the list is empty. Please use \n -Linear YourList \n -Binary YourList \n -Bubble YourList \n -Merge YourList \n -Quick YourList \n for this command.");
            return;
        }

        // Read list to be sorted and convert elements to integers
        string[] elements = args[1].Split(',');
        Console.WriteLine("User Input List: " + args[1]);

        // Select the specified sorting algorithm 
        switch (args[0])
        {
            case "-Linear":
                LinearSearch<string>.PrintList("Initial List", elements);
                int idL = LinearSearch<string>.Search(elements, "a");
                Console.WriteLine("Index of the List: " + idL);
                break;
            case "-Binary":
                BinarySearch<string>.PrintList("Initial List", elements);
                int idB = BinarySearch<string>.Search(elements, "c");
                Console.WriteLine("Index of the List: " + idB);
                break;
            default:
                Console.WriteLine("Specified algorithm is not found.");
                break;
        }
    }
}
```
```bash
$ dotnet run -Linear s,a,b  
$ User Input List: s,a,b
$ 
$ ##################################
$ Initial List:  s a b
$ ##################################
$ Index of the List: 1
```

In AlgorithmLibrary/ConsoleIO.cs,
```csharp
namespace AlgorithmLibrary;

public class ConsoleIO<T> where T : IComparable<T>
{
    // Constructor
    public ConsoleIO()
    {
    }

    // IO
    public static void PrintList(string listType, T[] array)
    {
        // \n introduces a new line in console
        Console.WriteLine("\n##################################");
        Console.Write(listType + ": ");
        for (int i = 0; i < array.Length; i++)
        {
            Console.Write($" {array[i]}");
        }
        Console.WriteLine("\n##################################");
    }
}
```

In AlgorithmLibrary/LinearSearch.cs,
```csharp
namespace AlgorithmLibrary.LinearSearch;

public class LinearSearch<T> : ConsoleIO<T> where T : IComparable<T>
{
    LinearSearch() { }

    public static int Search(T[] array, T x)
    {
        // Iterate over the array in order to find the key x
        for (int i = 0; i < array.Length; i++)
        {
            if (array[i].CompareTo(x) == 0)
            {
                return i;
            }
        }
        return -1;
    }
}
```

## Binary Search

In AlgorithmLibrary/BinarySearch.cs,
```csharp
namespace AlgorithmLibrary.BinarySearch;

// [IMPORTANT] can be applied only on a sorted list
public class BinarySearch<T> : ConsoleIO<T> where T : IComparable<T>
{
    BinarySearch() { }
    public static int Search(T[] array, T x)
    {
        int low = 0, high = array.Length - 1;

        while (low <= high)
        {
            int mid = low + (high - low) / 2;

            // Check if x is present at mid
            if (array[mid].CompareTo(x) == 0)
            {
                return mid;
            }
            // If x greater, ignore left half
            else if (array[mid].CompareTo(x) < 0)
            {
                low = mid + 1;
            }
            // If x is smaller, ignore right half
            else
            {
                high = mid - 1;
            }
        }

        // If we reach here, then element does not exist
        return -1;
    }
}
```


---
# External Resources

## [Searching Algorithms](https://www.geeksforgeeks.org/dsa/searching-algorithms/)

## [Sorting Algorithms](https://www.geeksforgeeks.org/dsa/sorting-algorithms/)

## [Functional Programming](https://learn.microsoft.com/en-us/archive/msdn-magazine/2009/october/functional-programming-for-everyday-net-development)
