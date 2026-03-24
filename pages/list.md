---
# permalink: /about/
layout: single
title: "DS&A: List"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-03-09
---

# List

## Console Project vs. Library Project

* Create the MyBusiness console project
```bash
$ dotnet new console --use-program-main --name MyBusiness
```

* Create the DataStructureLibrary classlib project
```bash
$ dotnet new classlib --name DataStructureLibrary
```

* Do not forget to add the environment variables in MyBusiness.csproj
```csharp
  <ItemGroup>
    <ProjectReference
      Include="../DataStructureLibrary/DataStructureLibrary.csproj" />
  </ItemGroup>
```

## Singly Linked List

In MyBusiness/Program.cs
```csharp
namespace MyBusiness;

using DataStructureLibrary.SinglyLinkedList;
// using DataStructureLibrary.DoublyLinkedList;

class Program
{
    static void Main(string[] args)
    {
        // Single linked list
        SinglyLinkedList list = new SinglyLinkedList();
        // Doubly linked list
        // DoublyLinkedList list = new DoublyLinkedList();
        list.InsertLast(6);
        list.InsertLast(4);
        list.InsertFront(2);
        list.PrintList();
        list.DeleteNodebyKey(2);
        list.PrintList();
        list.InsertAfter(list.FindbyKey(4), 5);
        list.PrintList();
        list.Sort();
        list.PrintList();
    }
}
```
```bash
The singlyLinkedList: 2 6 4 
The singlyLinkedList: 6 4   
The singlyLinkedList: 6 4 5
The singlyLinkedList: 4 5 6
```

In DataStructureLibrary/SinglyLinkedList.cs
```csharp
namespace DataStructureLibrary.SinglyLinkedList;

public class Node
{
    // Fields
    public int Data;
    public Node? Next;

    // Constructors
    public Node(int d)
    {
        Data = d;
        Next = null;
    }

    // Finalizers
    ~Node() { }
}

public class SinglyLinkedList
{
    // Fields
    private Node? _head;

    // Constructors
    public SinglyLinkedList()
    {
        _head = null;
    }

    // Finalizer
    ~SinglyLinkedList() { }

    // Methods
    // Public Methods
    public void InsertFront(int newData)
    {
        Node newNode = new Node(newData);

        newNode.Next = _head;
        _head = newNode;
    }

    public void InsertLast(int newData)
    {
        Node newNode = new Node(newData);

        if (_head == null)
        {
            _head = newNode;
        }
        else
        {
            Node? lastNode = GetLastNode();
            // Exclamation mark ! tells C# compiler explicitly that lastNode 
            // at this moment is not a null object. This is a way to bypass the 
            // warning.
            lastNode!.Next = newNode;
        }
    }

    public Node? GetLastNode()
    {
        Node? temp = _head;

        if (temp != null)
        {
            while (temp.Next != null)
            {
                temp = temp.Next;
            }
        }

        return temp;
    }

    public void InsertAfter(Node? prevNode, int newData)
    {
        if (prevNode == null)
        {
            Console.WriteLine("The given previous node Cannot be null!");
        }
        else
        {
            Node newNode = new Node(newData);
            newNode.Next = prevNode.Next;
            prevNode.Next = newNode;
        }
    }

    public Node? FindbyKey(int data)
    {
        Node? temp = _head;

        while (temp != null)
        {
            if (temp.Data == data)
            {
                return temp;
            }
            temp = temp.Next;
        }

        // The target data is not found
        return null;
    }

    public void DeleteNodebyKey(int key)
    {
        Node? temp = _head;
        Node? prev = null;

        if (temp != null)
        {
            // 1st node is the key
            if (temp.Data == key)
            {
                _head = temp.Next;
                return;
            }
            else
            {
                // Navigate the list to find the key
                while (temp!.Data != key)
                {
                    prev = temp;
                    temp = temp.Next;

                    // The key is not found when reaching to the end of the list
                    if (temp == null) return;
                }

                // The node that has the target key is found
                prev!.Next = temp.Next;
            }
        }
    }

    public void Sort()
    {
        Node? temp = _head;

        if (_head == null)
        {
            return;
        }
        else
        {
            while (temp != null)
            {
                Node? index = temp.Next;

                while (index != null)
                {
                    if (temp.Data > index.Data)
                    {
                        int tempData = temp.Data;
                        temp.Data = index.Data;
                        index.Data = tempData;
                    }
                    index = index.Next;
                }
                temp = temp.Next;
            }
        }
    }

    public void PrintList()
    {
        Node? temp = _head;
        Console.Write("The singlyLinkedList: ");
        while (temp != null)
        {
            Console.Write(temp.Data + " ");
            temp = temp.Next;
        }
        Console.WriteLine("");
    }
}
```

## Doubly Linked List

In DataStructureLibrary/DoublyLinkedList.cs
```csharp
namespace DataStructureLibrary.DoublyLinkedList;

public class Node
{
    // Fields
    public int Data;
    public Node? Prev;
    public Node? Next;

    // Constructors
    public Node(int d)
    {
        Data = d;
        Prev = null;
        Next = null;
    }

    // Finalizers
    ~Node() { }
}

public class DoublyLinkedList
{
    // Fields
    private Node? _head;

    // Constructors
    public DoublyLinkedList()
    {
        _head = null;
    }

    // Finalizer
    ~DoublyLinkedList() { }

    // Methods
    public void InsertFront(int data)
    {
        Node newNode = new Node(data);
        newNode.Next = _head;
        newNode.Prev = null;

        if (_head != null)
        {
            _head.Prev = newNode;
        }
        _head = newNode;
    }

    public void InsertLast(int data)
    {
        Node newNode = new Node(data);

        if (_head == null)
        {
            newNode.Prev = null;
            _head = newNode;
            return;
        }
        else
        {
            Node? lastNode = GetLastNode();

            lastNode!.Next = newNode;
            newNode.Prev = lastNode;
        }
    }

    public Node? GetLastNode()
    {
        Node? temp = _head;

        if (temp != null)
        {
            while (temp.Next != null)
            {
                temp = temp.Next;
            }
        }
        return temp;
    }

    public void InsertAfter(Node? prevNode, int newData)
    {
        if (prevNode == null)
        {
            Console.WriteLine("The method does nothing becase the node is not found.");
        }
        else
        {
            Node newNode = new Node(newData);
            newNode.Next = prevNode.Next;
            prevNode.Next = newNode;
            newNode.Prev = prevNode;
            if (newNode.Next != null)
            {
                newNode.Next.Prev = newNode;
            }
        }
    }

    public Node? FindbyKey(int data)
    {
        Node? temp = _head;

        while (temp != null)
        {
            if (temp.Data == data)
            {
                return temp;
            }
            temp = temp.Next;
        }
        return null;
    }

    public void DeleteNodebyKey(int key)
    {
        Node? temp = _head;

        // The list is not empty
        if (temp != null)
        {
            // 1st node is the key
            if (temp.Data == key)
            {
                _head = temp.Next;
                if (_head != null) _head.Prev = null;
                return;
            }
            else
            {
                while (temp!.Data != key)
                {
                    temp = temp.Next;

                    // The key is not found when reaching to the end of the list
                    if (temp == null) return;
                }

                if (temp.Next != null)
                {
                    temp.Next.Prev = temp.Prev;
                }
                if (temp.Prev != null)
                {
                    temp.Prev.Next = temp.Next;
                }
            }
        }
    }

    public void PrintList()
    {
        Node? temp = _head;
        Console.Write("The doublyLinkedList: ");
        while (temp != null)
        {
            Console.Write(temp.Data + " ");
            temp = temp.Next;
        }
        Console.WriteLine("");
    }
}
```



---
# External Resources

## [C# Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)


