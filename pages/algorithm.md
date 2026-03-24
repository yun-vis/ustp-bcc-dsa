---
# permalink: /about/
layout: single
title: "DS&A: Searching & Sorting"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-03-09
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

# Sorting Algorithms

A Sorting Algorithm is used to rearrange a given array or list of elements in an order. For example, a given array [10, 20, 5, 2] becomes [2, 5, 10, 20] after sorting in increasing order and becomes [20, 10, 5, 2] after sorting in decreasing order.


In MyBusiness/Program.cs,
```csharp
using AlgorithmLibrary.LinearSearch;
using AlgorithmLibrary.BinarySearch;
using AlgorithmLibrary.BubbleSort;
using AlgorithmLibrary.MergeSort;
using AlgorithmLibrary.QuickSort;

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
            case "-Bubble":
                BubbleSort<string>.PrintList("Initial List", elements);
                BubbleSort<string>.Sorter(elements);
                BubbleSort<string>.PrintList("Sorted List", elements);
                break;
            case "-Merge":
                MergeSort<string>.PrintList("Initial List", elements);
                MergeSort<string>.Sorter(elements, 0, elements.Length - 1);
                MergeSort<string>.PrintList("Sorted List", elements);
                break;
            case "-Quick":
                QuickSort<string>.PrintList("Initial List", elements);
                QuickSort<string>.Sorter(elements, 0, elements.Length - 1);
                QuickSort<string>.PrintList("Sorted List", elements);
                break;
            default:
                Console.WriteLine("Specified algorithm is not found.");
                break;
        }
    }
}
```
```bash
$ User Input List: 3,2,1
$
$ ##################################
$ Initial List:  3 2 1
$ ##################################
$ 
$ ##################################
$ Sorted List:  1 2 3
$ ##################################
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
    public static void Sorter(T[] array)
    {
        int length = array.Length;

        Console.WriteLine("\nSteps:");
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
                    _printSteps(array);
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
    public static void Sorter(T[] array, int left, int right)
    {
        int length = array.Length;

        // If left index is still at the left side of the right index
        if (left < right)
        {
            int middle = (left + right) / 2;

            // Sort the left array
            _printSteps("  Left: ", array, left, middle);
            Sorter(array, left, middle);

            // Sort the right array
            _printSteps("  Right: ", array, middle + 1, right);
            Sorter(array, middle + 1, right);

            // Merge the left and the right array
            _merge(array, left, middle, right);
            _printSteps(" Merged: ", array, left, right);
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
    public static void Sorter(T[] array, int low, int high)
    {
        if (low < high)
        {
            // pi is the partition return index of pivot
            int pi = _partition(array, low, high);

            // recursion calls for smaller elements
            // and greater or equals elements
            Sorter(array, low, pi - 1);
            Sorter(array, pi + 1, high);
        }
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

---
# External Resources

## [Searching Algorithms](https://www.geeksforgeeks.org/dsa/searching-algorithms/)

## [Sorting Algorithms](https://www.geeksforgeeks.org/dsa/sorting-algorithms/)

## [Functional Programming](https://learn.microsoft.com/en-us/archive/msdn-magazine/2009/october/functional-programming-for-everyday-net-development)
