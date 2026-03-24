---
# permalink: /about/
layout: single
title: "Polymorphism"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2026-03-09
---


---
# Key OOP Concept

It is like a phylogenetic tree. The higher hierarchy you climb, the more abstract concept about the object you get.

* **Objects:** instances of classes. E.g., my cat.

* **Class:** the blueprint for the data type (variables) and available methods for a given type or class of object. E.g., cat. Something that is cute, fluffy, with two eyes, four legs and a tail.

---
* **Encapsulation:** the combination of the data and actions that are related to an object. Achieve by using access modifiers, which is a way to ensure security. E.g., a cat can access its toilet.

* **Composition:** is about what an object is made of. E.g., A cat has two eyes, four legs and a tail.

* **Aggregation:** is about what can be combined with an object. E.g., a cat does not have a horn. You cannot ask a cat to fly like a bird, but it can walk like a quadruped.

* **Inheritance:** reuse code by having a subclass derive from a base or superclass. All functionality in the base class is inherited by and becomes available in the derived class. E.g., quadruped. A cat inherits the function of a quadruped.

* **Polymorphism:** allow a derived class to override an inherited action to provide custom behavior. E.g., animal. Animals like dogs speak "Woof!", but cat speaks "Meow!".

* **Abstraction:** is about capturing the core idea of an object and ignoring the details or specifics. C# has an abstract keyword which formalizes the concept. If a class is not explicitly abstract, then it can be described as being concrete. E.g., define a abstract class called **Animal** (a live creature that moves), but implement its derived class **Cat** by giving actual behaviors of a cat.


# Understanding Polymorphism

**Polymorphism:** allow a derived class to override an inherited action to provide custom behavior. E.g., animal. Animals like dogs speak "Woof!", but cats speak "Meow!".


# Generics [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)

Generics introduces the concept of type parameters to .NET, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code.

## Generic Method

Convert.ChangeType [Doc](https://learn.microsoft.com/en-us/dotnet/api/system.convert.changetype?view=net-8.0)
IConvertible Interface [Doc](https://learn.microsoft.com/en-us/dotnet/api/system.iconvertible?view=net-8.0)

```csharp
namespace CRC_CSD_07;

class Program
{
    static void Main(string[] args)
    {
        // Function overloading
        int r = 2;
        int newR = DoubleMyResource(r);
        Console.WriteLine($"My old resource is {r} and the new one is {newR}");
        float f = 2.5f;
        float newF = DoubleMyResource(f);
        Console.WriteLine($"My old resource is {f} and the new one is {newF}");
        List<int> s = new List<int> { 1, 2, 3 };
        List<int> newS = DoubleMyResource(s);
        Console.WriteLine("My old resource is {" + string.Join(", ", s) + "} and the new one is {" + string.Join(", ", newS) + "}");

        // Generic functions
        int gr = 2;
        int newGR = DoubleMyResource<int>(gr);
        Console.WriteLine($"My old resource is {gr} and the new one is {newGR}");

        float gf = 2.5f;
        float newGF = DoubleMyResource<float>(gf);
        Console.WriteLine($"My old resource is {gf} and the new one is {newGF}");

        string gs = "12.5";
        string newGS = DoubleMyResource<string>(gs);
        Console.WriteLine($"My old resource is {gs} and my new resource is {newGS}");
    }

    // Function overloading
    public static int DoubleMyResource(int resource)
    {
        resource *= 2;
        return resource;
    }

    public static float DoubleMyResource(float resource)
    {
        resource *= 2.0f;
        return resource;
    }

    public static List<int> DoubleMyResource(List<int> resource)
    {
        List<int> newResource = new List<int>();
        // List<int> newResource = resource;

        // Copy the resource to a new list
        foreach (int r in resource)
        {
            newResource.Add(r);
        }
        // Add the elements in the newResource to the resource list
        foreach (int r in newResource)
        {
            resource.Add(r);
        }
        return resource;
    }

    // Generic function
    // T stands for a type that implements the IConvertible interface
    // IConvertible: Defines methods that convert the value of the implementing 
    // reference or value type to a common language runtime type that has an 
    // equivalent value.
    public static T DoubleMyResource<T>(T resource) where T : IConvertible
    {
        double d = resource.ToDouble(Thread.CurrentThread.CurrentCulture);
        // ToDouble: A system method to convert a type to a double. We just use it for now.
        // Thread.CurrentThread.CurrentCulture: ToDouble requires a parameter 
        // that implements IFormatProvider to understand the format of numbers 
        // for a language and region. We can pass the CurrentCulture property 
        // of the current thread to specify the language and region used by 
        // your computer. 
        d *= 2.0; // is equal to r = r * 2.0M

        return (T)Convert.ChangeType(d, typeof(T));
    }
}
```
```bash
$ My old resource is 2 and the new one is 4
$ My old resource is 2.5 and the new one is 5
$ My old resource is {1, 2, 3, 1, 2, 3} and the new one is {1, 2, 3, 1, 2, 3}
$ My old resource is 2 and the new one is 4
$ My old resource is 2.5 and the new one is 5
$ My old resource is 12.5 and my new resource is 25
```

## Generic Class

In MyBusiness/Program.cs,
```csharp
using PetLibrary;

// namespace
namespace MyBusiness;

class Program
{
    static void Main(string[] args)
    {
        // Cat is currently flexible, because any type can be set for the food 
        // field and input parameter. But there is no type checking, so inside 
        // the CheckFood method, we cannot safely do much and the results are 
        // sometimes not what you might expect
        Cat cat1 = new Cat();
        cat1.Food = 5;
        Console.WriteLine($"Cat food with an integer: {cat1.checkFood(5)}");

        Cat cat2 = new Cat();
        cat2.Food = "fish";
        Console.WriteLine($"Cat food with a string: {cat2.checkFood("fish")}");
        // The .NET runtime maintains an intern pool, which is a table of unique string literals.
        
        // The type is defined at allocation
        GenericCat<int> gCat1 = new GenericCat<int>();
        gCat1.Food = 5;
        Console.WriteLine($"Cat food with an integer: {gCat1.checkFood(5)}");

        GenericCat<string> gCat2 = new GenericCat<string>();
        gCat2.Food = "fish";
        Console.WriteLine($"Cat food with a string: {gCat2.checkFood("fish")}");

        // generic methods
        string food1 = "4";
        Console.WriteLine("Double cat food {0} to {1}",
            arg0: food1,
            arg1: GenericMethodCat.DoubleMyFood<string>(food1)
        );

        byte food2 = 3;
        Console.WriteLine("Double cat food {0} to {1}",
            arg0: food2,
            arg1: GenericMethodCat.DoubleMyFood<byte>(food2)
        );
    }
}
```

In PetLibrary/Cat.cs,
```csharp
namespace PetLibrary;

public class Cat
{
    // fields
    public string Name;
    private DateTime _dateOfBirth; // backing field
    public List<Cat> Children = new List<Cat>();
    public object Food = default(object);

    // properties
    // public string Name { 
    //     get; 
    //     private set; 
    // }
    public DateTime DateOfBirth
    {
        // get;set;
        get
        {
            return _dateOfBirth;
        }
        set
        {
            if (value > DateTime.Today)
                throw new ArgumentException("Date of birth must be in the past");

            _dateOfBirth = value;
        }
    }

    // constructors
    // method overloading
    // default constructor
    public Cat()
    {
        Name = "UnknownCat";
        DateOfBirth = DateTime.Today;
    }
    // parametrized constructor
    public Cat(string name, DateTime dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
    }

    // methods
    public void WriteToLine()
    {
        Console.WriteLine($"{Name} was born on a {_dateOfBirth:dddd} and has {Children.Count} kitties.");
    }

    // instance method
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }

    // static method
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat($"{cat1.Name}-{cat2.Name}", DateTime.Today);

        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);

        return kitty;
    }

    // operator overloading
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return ProduceKitty(cat1, cat2);
    }

    public string checkFood(object input)
    {
        if (Food == null)
        {
            return "Expected food is empty.";
        }
        else if (Food == input)
        // else if (Food.Equals(input)) // Here, you explicitly compare the value not the address. 
        // Cat is currently flexible, because any type can be set for the food 
        // field and input parameter. But there is no type checking, so inside 
        // the Process method, we cannot safely do much and the results are sometimes 
        // not what you might expect; for example, when passing int values into 
        // an object parameter! This is because the value 5 stored in the food is 
        // stored in a different memory address to the value 5 passed as a parameter 
        // and when comparing reference types like any value stored in object, they 
        // are only equal if they are stored at the same memory address, that is, the 
        // same object, even if their values are equal. We can solve this problem by 
        // using generics.
        {
            return "Expected food and input are the same.";
        }
        else
        {
            return "Expected food and input are NOT the same.";
        }
    }
}
```

In PetLibrary/GenericCat.cs,
```csharp
namespace PetLibrary;

public class GenericCat<T> where T : IComparable
{
    public T Food = default(T);

    public string checkFood(T input)
    {
        if (Food == null)
        {
            return "Expected food is empty!";
        }
        else if (Food.CompareTo(input) == 0)
        {
            return "Expected food and input are the same.";
        }
        else
        {
            return "Expected food and input are NOT the same.";
        }
    }
}

public class GenericMethodCat
{
    public static double DoubleMyFood<T>(T input) where T : IConvertible
    {
        double d = input.ToDouble(Thread.CurrentThread.CurrentCulture);
        // ToDouble: A system method to convert a type to a double. We just use it for now.
        // Thread.CurrentThread.CurrentCulture: ToDouble requires a parameter that 
        // implements IFormatProvider to understand the format of numbers for a language 
        // and region. We can pass the CurrentCulture property of the current thread to 
        // specify the language and region used by your computer. 

        return 2.0 * d;
    }
}
```
```bash
$ Cat food with an integer: Expected food and input are NOT the same.
$ Cat food with an integer: Expected food and input are the same.
$ Cat food with an integer: Expected food and input are the same.
$ Cat food with an integer: Expected food and input are the same.
$ Double cat food 4 to 8
$ Double cat food 3 to 6
```

Do not forget to add the environment variables in MyBusiness.csproj
```csharp
  <ItemGroup>
    <ProjectReference
      Include="../PetLibrary/PetLibrary.csproj" />
  </ItemGroup>
```

## Thread.CurrentUICulture Property [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.threading.thread.currentuiculture?view=net-6.0)


<!-- ## in (Generic Modifier) [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/in-generic-modifier) -->



---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.

# References

* [where (generic type constraint)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/where-generic-type-constraint)
  In a generic definition, use the where clause to specify constraints on the types that you use as arguments for type parameters in a generic type, method, delegate, or local function. 

