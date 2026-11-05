---
# permalink: /about/
layout: single
title: "DS&A: Sorting"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-05-11
---


# Sorting Algorithms

A Sorting Algorithm is used to rearrange a given array or list of elements in an order. For example, a given array [10, 20, 5, 2] becomes [2, 5, 10, 20] after sorting in increasing order and becomes [20, 10, 5, 2] after sorting in decreasing order.


In MyBusiness/Program.cs,
```csharp
namespace MyBusiness;

using System.Diagnostics;

using DataStructureLibrary.SinglyLinkedList;
// using DataStructureLibrary.DoublyLinkedList;
using AlgorithmLibrary;
using AlgorithmLibrary.LinearSearch;
using AlgorithmLibrary.BinarySearch;
using AlgorithmLibrary.BubbleSort;
using AlgorithmLibrary.MergeSort;
using AlgorithmLibrary.QuickSort;

class Program
{
    enum SearchAlgorithm { Linear, Binary }
    enum SortAlgorithm { Bubble, Merge, Quick }
    // enum SortAlgorithm { Bubble = 10, Merge = 20, Quick = 30 }


    static void Main()
    {
        // Part 1: Demonstration of Linked List operations
        ListDemo();

        // Part 2: Demonstration of sorting and searching algorithms
        // Part 2a: Sorting (int only)
        int[] numbers = { 1, 9, 5, 7, 3, 11 };
        // Can be SortAlgorithm.Bubble, SortAlgorithm.Merge, SortAlgorithm.Quick
        SortDemo(SortAlgorithm.Merge, numbers);

        // Runtime comparison on a large random list (steps are not printed)
        int sortSize = 10_000; // The _ is just a digit separator. 
        // Random(0) constructor with a seed ensures the same random numbers are generated each time
        // Random() with no seed will get a different sequence each run.
        Random random = new Random(0); 
        int[] largeUnsorted = new int[sortSize];
        // Initialize the array with random numbers
        for (int i = 0; i < sortSize; i++)
        {
            largeUnsorted[i] = random.Next(0, sortSize * 10);
        }
        SortBenchmark(largeUnsorted);

        // Part 2b: Searching with generics, the same algorithms work on int and string
        // Binary search requires a sorted list
        int[] sortedNumbers = { 1, 3, 5, 7, 9, 11 };
        string[] sortedLetters = { "a", "b", "c", "d", "e" };
        SearchDemo("Integer List", sortedNumbers, 7);
        SearchDemo("String List", sortedLetters, "c");

        // Runtime comparison on a large sorted int list
        int searchSize = 100_000;
        int repeats = 1000;
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

    static void ListDemo()
    {
        // Single linked list
        SinglyLinkedList list = new SinglyLinkedList();
        // Doubly linked list
        // DoublyLinkedList list = new DoublyLinkedList();
        list.InsertLast(6);
        list.InsertLast(4);
        list.InsertFront(2);
        list.PrintList();
        // list.DeleteNodebyKey(2);
        // list.PrintList();
        list.InsertAfter(list.FindbyKey(4), 5);
        // list.PrintList();
        // list.Sort();
        // list.PrintList();
    }

    // Sorts the array in place with the given sorting algorithm
    static void Sort(SortAlgorithm algorithm, int[] array, bool printSteps)
    {
        switch (algorithm)
        {
            case SortAlgorithm.Bubble:
                BubbleSort<int>.Sorter(array, printSteps);
                break;
            case SortAlgorithm.Merge:
                MergeSort<int>.Sorter(array, 0, array.Length - 1, printSteps);
                break;
            case SortAlgorithm.Quick:
                QuickSort<int>.Sorter(array, 0, array.Length - 1, printSteps);
                break;
            default:
                Console.WriteLine($"Unknown sorting algorithm: {algorithm}");
                break;
        }
    }

    // Sorts the list in place with the given sorting algorithm and prints every step
    static void SortDemo(SortAlgorithm algorithm, int[] list)
    {
        ConsoleIO<int>.PrintList("Unsorted Integer List", list);
        Console.WriteLine($"\n--- {algorithm} Sort ---");
        Sort(algorithm, list, true);
        ConsoleIO<int>.PrintList($"Sorted by {algorithm} Sort", list);
    }

    // Times every sorting algorithm on its own copy of the same list (steps are not printed)
    static void SortBenchmark(int[] list)
    {
        SortAlgorithm[] sorters = Enum.GetValues<SortAlgorithm>();
        Console.WriteLine($"\nSorting performance test: {list.Length} random elements");
        for (int i = 0; i < sorters.Length; i++)
        {
            // Warm up so JIT compilation is not included in the measurement
            // C# isn't compiled straight to machine code. dotnet build produces IL, 
            // an intermediate bytecode.
            // At run time, the JIT (just-in-time) compiler turns each method into machine code 
            // the first time it is called. Later calls reuse that machine code.
            // Maybe it is not that important for now
            Sort(sorters[i], list.Take(10).ToArray(), false);

            // ToArray() is a generic LINQ method, so called on an int[] it returns a deep copy int[]. 
            int[] copy = list.ToArray();
            Stopwatch stopwatch = Stopwatch.StartNew();
            Sort(sorters[i], copy, false);
            stopwatch.Stop();
            Console.WriteLine($"{sorters[i] + " Sort",-12}: {stopwatch.Elapsed.TotalMilliseconds:F3} ms");
        }
    }

    // Returns the index of the key with the given searching algorithm, works for any comparable type T
    static int Search<T>(SearchAlgorithm algorithm, T[] list, T key) where T : IComparable<T>
    {
        switch (algorithm)
        {
            case SearchAlgorithm.Linear:
                return LinearSearch<T>.Search(list, key);
            case SearchAlgorithm.Binary:
                return BinarySearch<T>.Search(list, key);
            default:
                Console.WriteLine($"Unknown searching algorithm: {algorithm}");
                return -1;
        }
    }

    // Finds the key with every searching algorithm
    static void SearchDemo<T>(string label, T[] list, T key) where T : IComparable<T>
    {
        ConsoleIO<T>.PrintList(label, list);
        SearchAlgorithm[] searchers = Enum.GetValues<SearchAlgorithm>();
        for (int i = 0; i < searchers.Length; i++)
        {
            Console.WriteLine($"{searchers[i]} Search - Index of {key}: {Search(searchers[i], list, key)}");
        }
    }

    // Times every searching algorithm over the same keys
    static void SearchBenchmark<T>(string label, T[] list, T[] keys) where T : IComparable<T>
    {
        Console.WriteLine($"\nSearching performance test ({label}): {keys.Length} searches in a list of {list.Length} elements");
        SearchAlgorithm[] searchers = Enum.GetValues<SearchAlgorithm>();
        for (int i = 0; i < searchers.Length; i++)
        {
            // Warm up so JIT compilation is not included in the measurement
            Search(searchers[i], list, keys[0]);

            // Provides a set of methods and properties that you can use to accurately measure elapsed time.
            Stopwatch stopwatch = Stopwatch.StartNew();
            for (int j = 0; j < keys.Length; j++)
            {
                Search(searchers[i], list, keys[j]);
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
$ The singlyLinkedList: 2 6 4 
$ The given previous node Cannot be null!
$ 
$ ##################################
$ Unsorted Integer List:  1 9 5 7 3 11
$ ##################################
$ 
$ --- Merge Sort ---
$    Left:  1 9 5
$    Left:  1 9
$    Left:  1
$    Right:  9
$   Merged:  1 9
$    Right:  5
$   Merged:  1 5 9
$    Right:  7 3 11
$    Left:  7 3
$    Left:  7
$    Right:  3
$   Merged:  3 7
$    Right:  11
$   Merged:  3 7 11
$   Merged:  1 3 5 7 9 11
$ 
$ ##################################
$ Sorted by Merge Sort:  1 3 5 7 9 11
$ ##################################
$ 
$ Sorting performance test: 10000 random elements
$ Bubble Sort : 285.434 ms
$ Merge Sort  : 1.321 ms
$ Quick Sort  : 0.865 ms
$ 
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
$ Linear Search: 114.767 ms total, 0.114767 ms per search
$ Binary Search: 0.119 ms total, 0.000119 ms per search
```

## Bubble Sort
In AlgorithmLibrary/BubbleSort.cs,
```csharp
namespace AlgorithmLibrary.BubbleSort;

public class BubbleSort<T> : ConsoleIO<T> where T : IComparable<T>
{
    // Constructor
    public BubbleSort() { }

    // Methods
    public static void Sorter(T[] array, bool printSteps = true)
    {
        int length = array.Length;

        if (printSteps) Console.WriteLine("\nSteps:");
        for (int i = 0; i < length - 1; i++)
        {
            for (int j = 1; j < length; j++)
            {
                // The two elements of the exchange
                if (array[j].CompareTo(array[j - 1]) < 0)
                {
                    T temp = array[j];
                    array[j] = array[j - 1];
                    array[j - 1] = temp;
                    if (printSteps) _printSteps(array);
                }
            }
        }
    }

    static void _printSteps(T[] array)
    {
        // Print the current state of the arry
        for (int i = 0; i < array.Length; i++)
        {
            Console.Write($" {array[i]}");
        }
        Console.WriteLine("");
    }
}
```

## Merge Sort
In AlgorithmLibrary/MergeSort.cs,
```csharp
namespace AlgorithmLibrary.MergeSort;

public class MergeSort<T> : ConsoleIO<T> where T : IComparable<T>
{
    // Constructor
    public MergeSort() { }

    // Methods
    public static void Sorter(T[] array, int left, int right, bool printSteps = true)
    {
        int length = array.Length;

        // If left index is still at the left side of the right index
        if (left < right)
        {
            int middle = (left + right) / 2;

            // Sort the left array
            if (printSteps) _printSteps("  Left: ", array, left, middle);
            Sorter(array, left, middle, printSteps);

            // Sort the right array
            if (printSteps) _printSteps("  Right: ", array, middle + 1, right);
            Sorter(array, middle + 1, right, printSteps);

            // Merge the left and the right array
            _merge(array, left, middle, right);
            if (printSteps) _printSteps(" Merged: ", array, left, right);
        }
        else
        {
            // Used for debug purpose
            // Console.WriteLine($"The indices swapped. Reach the base case.");
        }
    }

    static void _printSteps(string label, T[] array, int left, int right)
    {
        // Print the current state of the arry
        Console.Write($" {label}");
        for (int i = left; i < right + 1; i++)
        {
            Console.Write($" {array[i]}");
        }
        Console.WriteLine("");
    }

    // Merge two sub arrays
    static void _merge(T[] array, int left, int middle, int right)
    {
        // Sizes of Subarrays to be merged
        int numLeft = middle - left + 1;
        int numRight = right - middle;

        // Create two temporary arrays
        T[] leftArray = new T[numLeft];
        T[] rightArray = new T[numRight];

        // Copy data to temp arrays
        for (int i = 0; i < numLeft; i++)
        {
            leftArray[i] = array[left + i];
        }
        for (int i = 0; i < numRight; i++)
        {
            rightArray[i] = array[middle + 1 + i];
        }

        // Merge the temporary arrays
        // Create two indices
        int indexL = 0, indexR = 0;
        // Current left index
        int indexC = left;
        while (indexL < numLeft && indexR < numRight)
        {
            // The first value in the leftArray is smaller than the first value in the rightArray
            if (leftArray[indexL].CompareTo(rightArray[indexR]) < 0)
            {
                array[indexC] = leftArray[indexL];
                indexL++;
            }
            else
            {
                array[indexC] = rightArray[indexR];
                indexR++;
            }
            indexC++;
        }

        // Concatenate the remaining array
        if (indexL < numLeft)
        {
            // Concatenate the left array
            for (int i = indexL; i < numLeft; i++)
            {
                array[indexC] = leftArray[i];
                indexC++;
            }
        }
        else if (indexR < numRight)
        {
            // Concatenate the right array
            for (int i = indexR; i < numRight; i++)
            {
                array[indexC] = rightArray[i];
                indexC++;
            }
        }
        else
        {
            Console.WriteLine("Something went wrong here. Check!");
        }
    }
}
```

## Quick Sort

In AlgorithmLibrary/QuickSort.cs,
```csharp
namespace AlgorithmLibrary.QuickSort;

public class QuickSort<T> : ConsoleIO<T> where T : IComparable<T>
{
    // Constructor
    public QuickSort() { }

    // Methods
    public static void Sorter(T[] array, int low, int high, bool printSteps = true)
    {
        if (low < high)
        {
            // pi is the partition return index of pivot
            int pi = _partition(array, low, high);
            if (printSteps) _printSteps($" Pivot {array[pi]}: ", array, low, high);

            // recursion calls for smaller elements
            // and greater or equals elements
            Sorter(array, low, pi - 1, printSteps);
            Sorter(array, pi + 1, high, printSteps);
        }
    }

    static void _printSteps(string label, T[] array, int low, int high)
    {
        // Print the current state of the partitioned range
        Console.Write($" {label}");
        for (int i = low; i < high + 1; i++)
        {
            Console.Write($" {array[i]}");
        }
        Console.WriteLine("");
    }

    static int _partition(T[] array, int low, int high)
    {

        // choose the pivot
        T pivot = array[high];

        // index of smaller element and indicates 
        // the right position of pivot found so far
        int i = low - 1;

        // traverse arr[low..high] and move all smaller
        // elements to the left side. Elements from low to 
        // i are smaller after every iteration
        for (int j = low; j <= high - 1; j++)
        {
            if (array[j].CompareTo(pivot) < 0)
            {
                i++;
                _swap(array, i, j);
            }
        }

        // move pivot after smaller elements and
        // return its position
        _swap(array, i + 1, high);
        return i + 1;
    }

    // swap function
    static void _swap(T[] array, int i, int j)
    {
        T temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

# Big O Notation

---
# External Resources

## [Searching Algorithms](https://www.geeksforgeeks.org/dsa/searching-algorithms/)

## [Sorting Algorithms](https://www.geeksforgeeks.org/dsa/sorting-algorithms/)

## [Functional Programming](https://learn.microsoft.com/en-us/archive/msdn-magazine/2009/october/functional-programming-for-everyday-net-development)
