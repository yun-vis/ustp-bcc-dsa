---
# permalink: /about/
layout: single
title: "DS&A: Graph"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
# kramdown:
#   auto_ids: true
last_modified_at: 2026-03-24
---

## Graph II (Node/Edge List)

In MyBusiness/Program.csproj
```csharp
using DataStructureLibrary.Graph;

namespace MyBusiness;

class Program
{
    static void Main(string[] args)
    {
        Graph graph = new Graph();
        Vertex v1 = graph.AddVertex("Victor");
        Vertex v2 = graph.AddVertex("Markus");
        Vertex v3 = graph.AddVertex("Yun");
        Vertex v4 = graph.AddVertex("Anna");
        // graph.RemoveVertex("Yun");
        graph.AddEdge(v1, v2);
        graph.AddEdge(v1, v3);
        graph.AddEdge(v2, v3);
        graph.AddEdge(v3, v4);
        graph.AddEdge(v3, graph.HasVertex("Victor"));
        graph.PrintGraph();
        graph.RemoveVertex("Victor");
        // graph.RemoveEdge(v1, v3);
        graph.PrintGraph();
    }
}
```
```bash
$ The total number of vertices is 4
$ The total number of edges is 5
$ ==============================
$ V(0) = Victor
$ V(1) = Markus
$ V(2) = Yun
$ V(3) = Anna
$ ==============================
$ E(0) = V(Victor) -- V(Markus)
$ E(1) = V(Victor) -- V(Yun)
$ E(2) = V(Markus) -- V(Yun)
$ E(3) = V(Yun) -- V(Anna)
$ E(4) = V(Yun) -- V(Victor)
$ ==============================
$ The total number of vertices is 3
$ The total number of edges is 2
$ ==============================
$ V(1) = Markus
$ V(2) = Yun
$ V(3) = Anna
$ ==============================
$ E(2) = V(Markus) -- V(Yun)
$ E(3) = V(Yun) -- V(Anna)
$ ==============================
```

In DataStructureLibrary/Graph.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Graph
{
    // Fields
    // The list of vertices in the graph
    // LinkedList is the doubly linked list implementation in C#
    private LinkedList<Vertex> _vertices;
    private LinkedList<Edge> _edges;

    // Constructors
    public Graph()
    {
        // Keeping instiantiation in construcor allows lazy instanciation in later stage
        _vertices = new LinkedList<Vertex>();
        _edges = new LinkedList<Edge>();
    }

    // Methods
    // Manipulate vertices
    public Vertex AddVertex(string name)
    {
        // Check if the vertex exist
        Vertex? v = HasVertex(name);

        // If not, add a new vertex
        if (v == null)
        {
            Vertex newV = new Vertex((uint)_vertices.Count, name);
            _vertices.AddLast(newV);

            return newV;
        }

        return v;
    }

    public void RemoveVertex(string name)
    {
        Vertex? v = HasVertex(name);

        if (v != null)
        {
            // Remove the adjacent edges

            // v.1 with run-time error
            // Unhandled exception. System.InvalidOperationException: 
            // Collection was modified after the enumerator was instantiated.
            // foreach (Edge e in _edges)
            // {
            //     if (e.Source == v || e.Target == v)
            //     {
            //         _edges.Remove(e);
            //     }
            // }

            // v.2 with logical error
            // for (int i = 0; i < _edges.Count; i++)
            // {
            //     // Equal to source id or target id
            //     if ((_edges.ElementAt(i).Source == v) || (_edges.ElementAt(i).Target == v))
            //     {
            //         _edges.Remove(_edges.ElementAt(i));
            //     }
            // }

            // v.3 no error
            for (int i = 0; i < _edges.Count; i++)
            {
                // bool isRemoved = false;
                Edge e = _edges.ElementAt(i);

                if (e.Source == v || e.Target == v)
                {
                    _edges.Remove(e);
                    // isRemoved = true;
                    i--;
                }
                // if (isRemoved == true) i--;
            }

            // Remove the vertex from the list
            _vertices.Remove(v);
        }
    }

    // Method overloading
    public Vertex? HasVertex(string name)
    {
        foreach (Vertex v in _vertices)
        {
            if (v.Name == name)
                return v;
        }
        return null;
    }

    // Method overloading
    public Vertex? HasVertex(uint id)
    {
        foreach (Vertex v in _vertices)
        {
            if (v.Id == id)
                return v;
        }
        return null;
    }

    // Manipulate edges
    public Edge? AddEdge(Vertex? source, Vertex? target)
    {
        // Check if the source and target vertices exist
        if (source == null || target == null)
        {
            Console.WriteLine("Source or Target Vertex could not be found. Please add vertices first");
            return null;
        }

        // Check if the edge exists
        Edge? e = HasEdge(source, target);

        // If not, add a new edge
        if (e == null)
        {
            Edge newE = new Edge((uint)_edges.Count, source, target);
            _edges.AddLast(newE);
            return newE;
        }

        return e;
    }

    public void RemoveEdge(Vertex? source, Vertex? target)
    {
        if (source == null || target == null) return;

        Edge? e = HasEdge(source, target);
        if (e != null)
        {
            _edges.Remove(e);
        }
        else
        {
            Console.WriteLine("Edge could not be found. This method does nothing.");
        }
    }

    public Edge? HasEdge(Vertex? source, Vertex? target)
    {
        if (source == null || target == null) return null;

        foreach (Edge e in _edges)
        {
            if ((e.Source == source) &&
                (e.Target == target))
                return e;
        }

        return null;
    }

    // Graph
    public void PrintGraph()
    {
        Console.WriteLine("The total number of vertices is " + _vertices.Count);
        Console.WriteLine("The total number of edges is " + _edges.Count);
        Console.WriteLine("==============================");

        // Vertex list
        foreach (Vertex v in _vertices)
        {
            Console.WriteLine($"V({v.Id}) = {v.Name}");
        }
        Console.WriteLine("==============================");

        // Edge list
        foreach (Edge e in _edges)
        {
            Console.WriteLine($"E({e.Id}) = V({e.Source.Name}) -- V({e.Target.Name})");
        }
        Console.WriteLine("==============================");
    }
}
```

In DataStructureLibrary/Vertex.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Vertex
{
    // Fields
    public uint Id;
    public string Name = "unknownName";

    // Constructors
    public Vertex(uint id, string name)
    {
        Id = id;
        Name = name;
    }
}
```

In DataStructureLibrary/Edge.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Edge
{
    // Fields
    public uint Id;
    public Vertex Source;
    public Vertex Target;

    // Constructors
    public Edge(uint id, Vertex source, Vertex target)
    {
        Id = id;
        Source = source;
        Target = target;
    }
}
```

## Graph III (Node/Edge List Refactored using Abstraction)

* Fix Id inconsistency problem, or use the Dictionary<TKey,TValue> collection instead
* Replace Console.WriteLine() in PrintGraph() by overriding ToString()
* Remove duplicated "if (source == null || target == null) return null;" in AddEdge() and RemoveEdge()
* Split data and methods to achieve abstraction and polymorphism

In MyBusiness/Program.cs
```csharp
using DataStructureLibrary.Graph;

namespace MyBusiness;

class Program
{
    public class VertexProperty : BasicVertexProperty
    {
    }

    public class EdgeProperty<TVertex> : BasicEdgeProperty<TVertex>
    {
        public double Weight;
    }

    static void Main(string[] args)
    {
        Graph<VertexProperty, EdgeProperty<Vertex<VertexProperty>>> graph = new Graph<VertexProperty, EdgeProperty<Vertex<VertexProperty>>>();
        Vertex<VertexProperty> v1 = graph.AddVertex("Victor");
        Vertex<VertexProperty> v2 = graph.AddVertex("Markus");
        Vertex<VertexProperty> v3 = graph.AddVertex("Yun");
        Vertex<VertexProperty> v4 = graph.AddVertex("Anna");
        // graph.RemoveVertex("Yun");
        // graph.RemoveVertex("Anna");
        Vertex<VertexProperty> v5 = graph.AddVertex("Michael");
        Edge<Vertex<VertexProperty>, EdgeProperty<Vertex<VertexProperty>>>? e = graph.AddEdge(v1, v2);
        if (e != null)
        {
            e.Property.Weight = 1.0;
        }
        graph.AddEdge(v1, v3);
        graph.AddEdge(v2, v3);
        graph.AddEdge(v3, v4);
        graph.PrintGraph();
        graph.RemoveVertex("Victor");
        graph.RemoveEdge(v3, v4);
        graph.PrintGraph();
    }
}
```
```bash
$ The total number of vertices is 5
$ The total number of edges is 4
$ ==============================
$ V(0) = Victor
$ V(1) = Markus
$ V(2) = Yun
$ V(3) = Anna
$ V(4) = Michael
$ ==============================
$ E(0): (V(0) = Victor) --> (V(1) = Markus)
$ E(1): (V(0) = Victor) --> (V(2) = Yun)
$ E(2): (V(1) = Markus) --> (V(2) = Yun)
$ E(3): (V(2) = Yun) --> (V(3) = Anna)
$ ==============================
$ The total number of vertices is 4
$ The total number of edges is 1
$ ==============================
$ V(1) = Markus
$ V(2) = Yun
$ V(3) = Anna
$ V(4) = Michael
$ ==============================
$ E(2): (V(1) = Markus) --> (V(2) = Yun)
$ ==============================
```

In DataStructureLibrary/Graph.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Graph<TVertexProperty, TEdgeProperty>
where TVertexProperty : BasicVertexProperty, new()
where TEdgeProperty : BasicEdgeProperty<Vertex<TVertexProperty>>, new()
{
    // Fields
    // The list of vertices in the graph
    private readonly LinkedList<Vertex<TVertexProperty>> _vertices;
    private readonly LinkedList<Edge<Vertex<TVertexProperty>, TEdgeProperty>> _edges;

    // Constructors
    public Graph()
    {
        _vertices = new LinkedList<Vertex<TVertexProperty>>();
        _edges = new LinkedList<Edge<Vertex<TVertexProperty>, TEdgeProperty>>();
    }

    // Methods
    // Manipulate vertices
    public Vertex<TVertexProperty> AddVertex(string name)
    {
        Vertex<TVertexProperty>? v = HasVertex(name);

        if (v == null)
        {
            Vertex<TVertexProperty> newV = new Vertex<TVertexProperty>();

            // Add vertex attributes
            newV.Property.Name = name;
            _vertices.AddLast(newV);

            return newV;
        }

        return v;
    }

    public void RemoveVertex(string name)
    {
        Vertex<TVertexProperty>? v = HasVertex(name);

        if (v != null)
        {
            // v1
            for (int i = 0; i < _edges.Count; i++)
            {
                Edge<Vertex<TVertexProperty>, TEdgeProperty> e = _edges.ElementAt(i);

                // bool isRemoved = false;
                // Equal to source or target
                if ((e.Property.Source == v) || (e.Property.Target == v))
                {
                    _edges.Remove(e);
                    // isRemoved = true;
                    i--;
                }

                // if (isRemoved == true) i--;
            }

            // v2
            // List<Edge<Vertex<TVertexProperty>, TEdgeProperty>> deleteEdgeList = new List<Edge<Vertex<TVertexProperty>, TEdgeProperty>>();
            // // Collect the adjacent edges to be removed
            // foreach (Edge<Vertex<TVertexProperty>, TEdgeProperty> e in _edges)
            // {
            //     // Equal to source or target
            //     if ((e.Property.Source == v) || (e.Property.Target == v))
            //     {
            //         deleteEdgeList.Add(e);
            //     }
            // }
            // // Remove the collected edges
            // foreach (Edge<Vertex<TVertexProperty>, TEdgeProperty> e in deleteEdgeList)
            // {
            //     _edges.Remove(e);
            // }

            // v3
            // Collect the adjacent edges to be removed
            // List<Edge<Vertex<TVertexProperty>, TEdgeProperty>> deleteEdgeList = _edges.Where(e => (e.Property.Source == v) || (e.Property.Target == v)).ToList();
            // // Remove the collected edges
            // foreach (Edge<Vertex<TVertexProperty>, TEdgeProperty> e in deleteEdgeList)
            // {
            //     // Console.WriteLine(e);
            //     _edges.Remove(e);
            // }

            // v4
            // _edges.RemoveAll(e => e.Property.Source == v || e.Property.Target == v);

            // Remove the vertex from the list
            _vertices.Remove(v);
        }
    }

    public Vertex<TVertexProperty>? HasVertex(string name)
    {
        foreach (Vertex<TVertexProperty> v in _vertices)
        {
            if (v.Property.Name == name)
                return v;
        }
        return null;
    }

    // Function overloading
    public Vertex<TVertexProperty>? HasVertex(uint id)
    {
        foreach (Vertex<TVertexProperty> v in _vertices)
        {
            if (v.Property.Id == id)
                return v;
        }
        return null;
    }

    // Manipulate edges
    public Edge<Vertex<TVertexProperty>, TEdgeProperty>? AddEdge(Vertex<TVertexProperty>? source, Vertex<TVertexProperty>? target)
    {
        // Check if the source and target vertices exist
        // But this is done in HasEdge(source, target) already
        // if (source == null || target == null) return null;

        Edge<Vertex<TVertexProperty>, TEdgeProperty>? e = HasEdge(source, target);

        if (e == null)
        {
            Edge<Vertex<TVertexProperty>, TEdgeProperty> newE = new Edge<Vertex<TVertexProperty>, TEdgeProperty>(source!, target!);
            _edges.AddLast(newE);

            return newE;
        }

        return e;
    }

    public void RemoveEdge(Vertex<TVertexProperty>? source, Vertex<TVertexProperty>? target)
    {
        // But this is done in HasEdge(source, target) already
        // if (source == null || target == null) return;

        Edge<Vertex<TVertexProperty>, TEdgeProperty>? e = HasEdge(source, target);

        if (e != null)
        {
            _edges.Remove(e);
        }
        else
        {
            Console.WriteLine("Edge could not be found. This method does nothing.");
        }
    }

    public Edge<Vertex<TVertexProperty>, TEdgeProperty>? HasEdge(Vertex<TVertexProperty>? source, Vertex<TVertexProperty>? target)
    {
        if (source == null || target == null) return null;

        foreach (Edge<Vertex<TVertexProperty>, TEdgeProperty> e in _edges)
        {
            if ((e.Property.Source == source) &&
                (e.Property.Target == target))
                return e;
        }

        return null;
    }

    // Graph
    public void PrintGraph()
    {
        Console.WriteLine("The total number of vertices is " + _vertices.Count);
        Console.WriteLine("The total number of edges is " + _edges.Count);
        Console.WriteLine("==============================");

        // Vertex list
        foreach (Vertex<TVertexProperty> v in _vertices)
        {
            Console.WriteLine(v);
            // Console.WriteLine($"V({v.Property.Id}) = {v.Property.Name}");
        }
        Console.WriteLine("==============================");

        // Edge list
        foreach (Edge<Vertex<TVertexProperty>, TEdgeProperty> e in _edges)
        {
            Console.WriteLine(e);
            // Console.WriteLine($"E({e.Property.Id}) = V({e.Property.Source!.Property.Name}) -- V({e.Property.Target!.Property.Name})");
        }
        Console.WriteLine("==============================");
    }
}
```

The where clause can also include a constructor constraint, new(). That constraint makes it possible to create an instance of a type parameter by using the new operator. The new() Constraint lets the compiler know that any type argument supplied must have an accessible parameterless constructor. 

In C#, the new() constraint must be parameterless because it serves as a compile-time guarantee that the generic type can be instantiated with a specific, uniform syntax: new T(). C# does not currently support constraints that specify specific constructor parameters (e.g., where T : new(string)). Implementing parameterized constructor constraints would require a complex "structural typing" system for constructors, which hasn't been added to the language yet. 

Common Workarounds:

* **Factory Pattern**: Pass a delegate like Func<T> to your generic class, allowing you to define custom instantiation logic outside the generic context.
* **Interface Initialization**: Use a new() constraint to create the object, then call an Initialize(...) method defined in a shared interface.

In DataStructureLibrary/Vertex.cs
```csharp
namespace DataStructureLibrary.Graph;

//  An abstract class is a class that cannot be instantiated directly.
public abstract class BasicVertexProperty
{
    // Fields
    public uint Id;
    public string Name = "unknownName";

}

// BasicVertexProperty, new() are generic type constraints
public class Vertex<TVertexProperty> where TVertexProperty : BasicVertexProperty, new()
{
    // Fields
    public TVertexProperty Property;
    // starts from 0
    private static uint _idCounter = 0;

    // Constructors
    public Vertex()
    {
        // The new() Constraint lets the compiler know that any type argument supplied must have an accessible parameterless constructor.
        Property = new TVertexProperty();
        Property.Id = _idCounter++;
    }

    // Do we need to override Equals()?
    // public override bool Equals(object? obj)
    // {
    //     return obj is Vertex<TVertexProperty> vertex && Property.Id == vertex.Property.Id;
    // }

    public override string ToString()
    {
        return $"V({Property.Id}) = {Property.Name}";
    }
}
```

In DataStructureLibrary/Edge.cs
```csharp
namespace DataStructureLibrary.Graph;

public abstract class BasicEdgeProperty<TVertex>
{
    // Fields
    public uint Id;
    public TVertex? Source;
    public TVertex? Target;
}

// BasicEdgeProperty, new() are generic type constraints
public class Edge<TVertex, TEdgeProperty> where TEdgeProperty : BasicEdgeProperty<TVertex>, new()
{
    // Fields
    public TEdgeProperty Property;
    // starts from 0
    private static uint _idCounter = 0;

    // Constructors
    public Edge(TVertex source, TVertex target)
    {
        // The new() Constraint lets the compiler know that any type argument supplied must have an accessible parameterless constructor.
        Property = new TEdgeProperty();
        Property.Id = _idCounter++;
        Property.Source = source;
        Property.Target = target;
    }

    public override string ToString()
    {
        // check how to make it generic
        return $"E({Property.Id}): ({Property.Source}) --> ({Property.Target})";
    }
}
```

Language-Integrated Query (LINQ) is the name for a set of technologies based on the integration of query capabilities directly into the C# language. LINQ technology is a form of declarative, functional programming.

In DataStructureLibrary/Extensions.cs
```csharp
namespace DataStructureLibrary.Graph;

public static class LinkedListExtensions
{
    // Before C# 14, you declare an extension method by adding the this modifier to the first parameter.
    public static void RemoveAll<T>(this LinkedList<T> linkedList,
                                    Func<T, bool> predicate)
    {
        if (linkedList != null)
        {
            for (LinkedListNode<T> node = linkedList.First!; node != null;)
            {
                LinkedListNode<T> next = node.Next!;
                if (predicate(node.Value))
                    linkedList.Remove(node);
                node = next;
            }
        }
    }

    // Beginning with C# 14, you can declare extension blocks. It has the same functionality as above.
    // extension<T>(LinkedList<T>): Static-style extension members
    // extension<T>(LinkedList<T> linkedList): Instance-style extension members
    // extension<T>(LinkedList<T> linkedList)
    // {
    //     public void RemoveAll(Func<T, bool> predicate)
    //     {
    //         if (linkedList != null)
    //         {
    //             for (LinkedListNode<T> node = linkedList.First!; node != null;)
    //             {
    //                 LinkedListNode<T> next = node.Next!;
    //                 if (predicate(node.Value))
    //                     linkedList.Remove(node);
    //                 node = next;
    //             }
    //         }
    //     }
    // }
}
```

---
# External Resources

* [Debugging using VSCode](https://code.visualstudio.com/docs/debugtest/debugging)
* [Dictionary<TKey,TValue>](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-10.0)
* [new constraint (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/new-constraint)
* [where (generic type constraint) (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/where-generic-type-constraint)
* [C# Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
* [LINQ (Language Integrated Query)](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)
* [Functional programming vs. imperative programming](https://learn.microsoft.com/en-us/dotnet/standard/linq/functional-vs-imperative-programming)