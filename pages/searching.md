---
# permalink: /about/
layout: single
title: "DS&A: Searching"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-06-11
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
namespace MyBusiness;

using System.Diagnostics;

using AlgorithmLibrary;
using AlgorithmLibrary.LinearSearch;
using AlgorithmLibrary.BinarySearch;

class Program
{
    enum SearchAlgorithm { Linear, Binary }

    static void Main()
    {
        // Random(0) constructor with a seed ensures the same random numbers are generated each time
        // Random() with no seed will get a different sequence each run.
        Random random = new Random(0);

        // Part b: Searching with generics, the same algorithms work on int and string
        // Binary search requires a sorted list
        int[] sortedNumbers = { 1, 3, 5, 7, 9, 11 };
        string[] sortedLetters = { "a", "b", "c", "d", "e" };
        ConsoleIO<int>.PrintList("Integer List", sortedNumbers);
        Search(SearchAlgorithm.Linear, sortedNumbers, 7, true);
        Search(SearchAlgorithm.Binary, sortedNumbers, 7, true);

        ConsoleIO<string>.PrintList("String List", sortedLetters);
        Search(SearchAlgorithm.Linear, sortedLetters, "c", true);
        Search(SearchAlgorithm.Binary, sortedLetters, "c", true);

        // Runtime comparison on a large sorted int list
        SearchBenchmarkDemo(random, 100_000, 1000);
    }

    // Returns the index of the key with the given searching algorithm, works for any comparable type T
    // If printResult is true, it also prints the index that was found
    static int Search<T>(SearchAlgorithm algorithm, T[] list, T key, bool printResult) where T : IComparable<T>
    {
        int index;
        switch (algorithm)
        {
            case SearchAlgorithm.Linear:
                index = LinearSearch<T>.Search(list, key);
                break;
            case SearchAlgorithm.Binary:
                index = BinarySearch<T>.Search(list, key);
                break;
            default:
                Console.WriteLine($"Unknown searching algorithm: {algorithm}");
                index = -1;
                break;
        }

        if (printResult)
        {
            Console.WriteLine($"{algorithm} Search - Index of {key}: {index}");
        }
        return index;
    }

    // Builds a large sorted list and random keys, then runs the searching benchmark on them
    static void SearchBenchmarkDemo(Random random, int searchSize, int repeats)
    {
        int[] largeNumbers = new int[searchSize];
        for (int i = 0; i < searchSize; i++)
        {
            largeNumbers[i] = i * 2;
        }
        // Search for random keys, half of them odd (i.e. not in the list)
        int[] numberKeys = new int[repeats];
        for (int i = 0; i < repeats; i++)
        {
            numberKeys[i] = random.Next(0, searchSize * 2);
        }
        SearchBenchmark("int", largeNumbers, numberKeys);
    }

    // Times every searching algorithm over the same keys
    static void SearchBenchmark<T>(string label, T[] list, T[] keys) where T : IComparable<T>
    {
        Console.WriteLine($"\nSearching performance test ({label}): {keys.Length} searches in a list of {list.Length} elements");
        SearchAlgorithm[] searchers = Enum.GetValues<SearchAlgorithm>();
        for (int i = 0; i < searchers.Length; i++)
        {
            // Warm up so JIT compilation is not included in the measurement
            Search(searchers[i], list, keys[0], false);

            // Provides a set of methods and properties that you can use to accurately measure elapsed time.
            Stopwatch stopwatch = Stopwatch.StartNew();
            for (int j = 0; j < keys.Length; j++)
            {
                Search(searchers[i], list, keys[j], false);
            }
            stopwatch.Stop();
            double time = stopwatch.Elapsed.TotalMilliseconds;
            // F3 formats the number to 3 decimal places, F6 formats to 6 decimal places
            Console.WriteLine($"{searchers[i]} Search: {time:F3} ms total, {time / keys.Length:F6} ms per search");
        }
    }
}
```
```bash
$ ##################################
$ Integer List:  1 3 5 7 9 11
$ ##################################
$ Linear Search - Index of 7: 3
$ Binary Search - Index of 7: 3
$ 
$ ##################################
$ String List:  a b c d e
$ ##################################
$ Linear Search - Index of c: 2
$ Binary Search - Index of c: 2
$ 
$ Searching performance test (int): 1000 searches in a list of 100000 elements
$ Linear Search: 114.343 ms total, 0.114343 ms per search
$ Binary Search: 0.115 ms total, 0.000115 ms per search
```

In AlgorithmLibrary/ConsoleIO.cs,
```csharp
namespace AlgorithmLibrary;

public class ConsoleIO<T>
{
    // Constructor
    public ConsoleIO() { }

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

// <T> is a generic type parameter 
// where keyword 'where T : IComparable<T>' is a generic type constraint that specifies 
// that the type parameter T must implement the IComparable<T> interface. This ensures 
// that the elements being searched can be compared to each other.
public class LinearSearch<T> : ConsoleIO<T> where T : IComparable<T>
{
    LinearSearch() { }

    public static int Search(T[] array, T x)
    {
        // Iterate over the array in order to find the key x
        for (int i = 0; i < array.Length; i++)
        {
            // This does not work for self-defined types if == is not overloaded.
            // if (array[i] == x) 
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
    // Defualt constructor
    BinarySearch() { }

    // Returns the index of x if found, otherwise -1
    public static int Search(T[] array, T x)
    {
        // Validation
        ArgumentNullException.ThrowIfNull(array);

        int low = 0, high = array.Length - 1;

        while (low <= high)
        {
            int mid = low + (high - low) / 2;
            // CompareTo comes from the IComparable<T> interface required by the class constraint
            int order = array[mid].CompareTo(x);

            // Check if x is present at mid
            if (order == 0)
            {
                return mid;
            }
            // If array[mid] is smaller than x, ignore left half
            else if (order < 0)
            {
                low = mid + 1;
            }
            // If array[mid] is larger than x, ignore right half
            else
            {
                high = mid - 1;
            }
        }

        // If we reach here, then element does not exist;
        return -1;
        // low is the index of the first element larger than x
        // return ~low; 
        // returns the index of x if found, otherwise a negative number that is the bitwise 
        // complement (~) of the index where x would be inserted 
        // return Array.BinarySearch(array, x); 
    }
}
```


---
# External Resources

## [Searching Algorithms](https://www.geeksforgeeks.org/dsa/searching-algorithms/)

## [Sorting Algorithms](https://www.geeksforgeeks.org/dsa/sorting-algorithms/)

## [Functional Programming](https://learn.microsoft.com/en-us/archive/msdn-magazine/2009/october/functional-programming-for-everyday-net-development)
