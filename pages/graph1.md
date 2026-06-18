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

## Introduction Graph

## Graph I (Node/Edge List)

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


---
# External Resources

* [Debugging using VSCode](https://code.visualstudio.com/docs/debugtest/debugging)
* [Dictionary<TKey,TValue>](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-10.0)
* [new constraint (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/new-constraint)
* [where (generic type constraint) (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/where-generic-type-constraint)
* [C# Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
* [LINQ (Language Integrated Query)](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)
* [Functional programming vs. imperative programming](https://learn.microsoft.com/en-us/dotnet/standard/linq/functional-vs-imperative-programming)