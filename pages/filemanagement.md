---
# permalink: /about/
layout: single
title: "File Management"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2026-03-01
---

# Other Properties of Methods

## Types of the Methods (Instance/Static) and Operator Overloading

* **Instance methods** are actions that an object does to itself.
  * A mom cat procreates a kitty by taking a dad cat as an input.
  * The mom cat cannot produce kitties from multiple dad cats.
* **Static methods** are actions the type does.
  * The owner of the cats encourages them to procreate a kitty.
  * The owner can pair many cat couples at the same time.

```csharp
using System;

namespace MyBusiness; // File scoped namespaces

class Program
{
    static void Main(string[] args)
    {
        // Create 3 cats and initialize them
        Cat[] cats = {
            new Cat("Nana", new DateTime(2019, 12, 9)), // 2029
            new Cat("Coffee", new DateTime(2019, 6, 20)),
            new Cat("Kiwi", new DateTime(2018, 11, 19))
        };

        // Call instance method
        Cat kitty1 = cats[0].ProduceKittyWith(cats[1]);

        // Call static method
        Cat kitty2 = Cat.ProduceKitty(cats[2], cats[1]);
        // The following statement functions as the above one
        // Cat kitty2 = cats[2] * cats[1];

        // Print out the present content
        foreach (Cat cat in cats)
        {
            cat.WriteToConsole();
        }

        // Use Console.WriteLine with arguments
        Console.WriteLine(
        format: "{0}'s first kitty is named \"{1}\".",
        arg0: cats[1].Name,
        arg1: cats[1].Children[0].Name);
    }
}


/*
The cat class
*/
public class Cat
{
    /*
    Class field, including different variables
    they can be organized by similar characteristics
    */
    // In C#, a backing field is a private variable that stores the actual data for a property.
    private DateTime _dateOfBirth;
    // The children of the cat
    public List<Cat> Children = new List<Cat>();

    /*
    Class properties
    */
    // The name of the cat
    public string Name { get; private set; }
    // The birthday of the cat
    public DateTime DateOfBirth
    {
        //get; // Get compile errors (C# 13 or lower) and run-time errors and warnings (C# 14 or above) because you cannot mix an auto accessor with a custom accessor.
        // Use $ dotnet new globaljson --force --sdk-version 8.0.302 --roll-forward latestFeature
        // and run other .Net version (e.g., .Net 8) for testing
        get
        {
            return _dateOfBirth;
        }

        private set
        {
            // Stack overflow. Repeated 24114 times.
            // DateOfBirth = value;

            // Data validation
            if (value > DateTime.Today)
                throw new ArgumentException("Date of birth must be in the past.");

            _dateOfBirth = value;
        }
    }

    /*
    Class methods, where functions should be implemented
    */

    // Constructors
    // Default constructor. It will be called by default
    public Cat()
    {
        Name = "UnknownCat";
        DateOfBirth = DateTime.Today;
    }
    // Parameterized Constructor
    public Cat(string name, DateTime dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
    }
    // Finalizer/Desctructor
    ~Cat()
    {
    }

    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd} and has {Children.Count} kitties.");
    }

    // Static method to "multiply"
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat ($"{cat1.Name}-{cat2.Name}", DateTime.Today);
        // The same functionaly as above
        // Cat kitty = new Cat
        // {
        //     Name = $"{cat1.Name}-{cat2.Name}",
        //     DateOfBirth = DateTime.Today
        // };
        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);
        return kitty;
    }

    // Use operators instead of the above method
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return Cat.ProduceKitty(cat1, cat2);
    }

    // Instance method to "multiply"
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }
}
```
```bash
$ Nana was born on a Monday and has 1 kitties.
$ Coffee was born on a Thursday and has 2 kitties.
$ Kiwi was born on a Monday and has 1 kitties.
$ Coffee's first kitty is named "Nana-Coffee".
```

## Field backed property declarations (only from C# 14)

```csharp
// private DateTime _dateOfBirth;
public DateTime DateOfBirth
{
    // auto-implemented getter
    get;

    // you can only mix auto-implemented getter and a customized setter in C# 14 or above
    private set
    {
        // Data validation
        if (value > DateTime.Today)
            throw new ArgumentException("Date of birth must be in the past.");

        field = value;
    }        

    // lambda-expression settor
    // private set => field = (value > DateTime.Today)
    // ? throw new ArgumentOutOfRangeException(nameof(value), "The value must not be negative")
    // : value;
}
```

<!-- ## Lazy Initialization -->


## Local (Nested/Inner) Functions

**Local functions** are private methods of a type that are nested in another member.

```csharp
public Cat ProduceKittyWith(Cat partner)
{
    // This is a local function
    bool testIfInLove(Cat partner)
    {
        return true;
    }

    // Test if the cat is in love with the partner.
    testIfInLove( partner);

    return ProduceKitty(this, partner);
}
```

---
# Splitting Files to Organize Them [Doc](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new)
```bash
// Create a new project named MyBusiness
CRC_CSD-05> dotnet new console --use-program-main --name MyBusiness
// Set up a class library called PetLibrary
CRC_CSD-05> $ dotnet new classlib --name PetLibrary
// Change the folder to PetLibrary
CRC_CSD-05> $ cd PetLibrary
// Change the file name and add content to Class Cat
CRC_CSD-05/PetLibrary> $ mv Class1.cs Cat.cs
// Go back to the folder MyBusiness and run the program
CRC_CSD-05/PetLibrary> $ cd ../MyBusiness
CRC_CSD-05/MyBusiness> $ dotnet run
```

In MyBusiness.csproj, you see the configuration of the program
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

<!-- Set environment variable -->
  <ItemGroup>
    <ProjectReference
      Include="../PetLibrary/PetLibrary.csproj" />
      <!-- The slash and backslash presentation used to describe a path were not
      standardized. It causes a lot of pain.
      Some morden editors automatically convert them. -->
  </ItemGroup>

</Project>
```

In MyBusiness/Program.cs,
```csharp

using System;
using PetLibrary;

namespace MyBusiness; // File scoped namespaces

class Program
{
    static void Main(string[] args)
    {
        // Create a cat
        Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
        nana.WriteToConsole();

        // Create a dog
        // Error: 'Dog' is inaccessible due to its protection level
        // Dog aDog = new Dog();

        // Create pets
        OwnPets myPets = new OwnPets();
        myPets.WriteToConsole();
    }
}
```

In PetLibrary/Cat.cs,
```csharp
// The same as namespace PetLibrary{}. We use ";" just to save indent space
namespace PetLibrary;

/*
Cat class
*/
public class Cat
{
    /*
    Class field, including different variables
    they can be organized by similar characteristics
    */
    // The children of the cat
    private DateTime _dateOfBirth;
    public List<Cat> Children = new List<Cat>();

    /*
    Class properties
    */
    // The name of the cat
    public string Name { get; private set; }
    // The birthday of the cat
    public DateTime DateOfBirth
    {
        //get; // Get compile errors (C# 13 or lower) and run-time errors and warnings (C# 14 or above) because you cannot mix an auto accessor with a custom accessor.
        get
        {
            return _dateOfBirth;
        }

        private set
        {
            // Stack overflow. Repeated 24114 times.
            // DateOfBirth = value;

            // Data validation
            if (value > DateTime.Today)
                throw new ArgumentException("Date of birth must be in the past.");

            _dateOfBirth = value;
        }
    }

    /*
    Class methods, where functions should be implemented
    */

    // Constructors
    // Default constructor. It will be called by default
    public Cat()
    {
        Name = "UnknownCat";
        DateOfBirth = DateTime.Today;
    }
    // Parameterized Constructor
    public Cat(string name, DateTime dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
    }
    // Finalizer/Desctructor
    ~Cat()
    {
    }

    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd} and has {Children.Count} kitties.");
    }

    // Static method to "multiply"
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat ($"{cat1.Name}-{cat2.Name}", DateTime.Today);
        // The same functionaly as above
        // Cat kitty = new Cat
        // {
        //     Name = $"{cat1.Name}-{cat2.Name}",
        //     DateOfBirth = DateTime.Today
        // };
        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);
        return kitty;
    }

    // Use operators instead of the above method
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return Cat.ProduceKitty(cat1, cat2);
    }

    // Instance method to "multiply"
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }
}
```

In PetLibrary/Dog.cs,
```csharp
namespace PetLibrary;

// Access modifier internal can be only used inside the project
internal class Dog
{
    // Fields
    // The name of the cat
    public string Name;

    // Constructors
    // Default constructor. It will be called by default
    public Dog()
    {
        Name = "UnknownDog";
    }
}
```

In PetLibrary/OwnPets.cs,
```csharp
namespace PetLibrary;

public class OwnPets
{
    // Fields
    Cat houseCat;
    Dog houseDog;

    // Constructors
    public OwnPets()
    {
        houseCat = new Cat();
        houseDog = new Dog();
    }

    // Finalizers

    // Methods
    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{houseCat}'s name is  {houseCat.Name}.");
        Console.WriteLine($"{houseDog}'s name is  {houseDog.Name}.");
    }
}
```


---
# Inheriting Classes
In C#, classes are used to create custom types. Inheritance is the process by which one class inherits the members of another class.

# Self-Defined Interface [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)
An **interface** defines a contract. Any class or struct that implements that contract must provide an implementation of the members defined in the interface. Note that you cannot apply access modifiers to interface members.

The basic difference is that a **class** has both a definition and an implementation whereas an **interface** only has a definition.

**Interfaces** don't contain fields because fields represent a specific implementation of data representation, and exposing them would break encapsulation.

In MyBusiness/Program.cs,
```csharp
using System;
using PetLibrary;

namespace MyBusiness; // File scoped namespaces

class Program
{
    static void Main(string[] args)
    {
        // Create a cat
        Cat[] cats =
        {
            new Cat("Nana", new DateTime(2019, 12, 9)),
            new Cat("Coffee", new DateTime(2019, 6, 20)),
            new Cat("Kiwi", new DateTime(2018, 11, 19))
        };

        double speed = cats[0].SpeedUp(2);
        Console.WriteLine($"{cats[0].Name} is running at speed {speed} km/hr.");
    }
}
```
In PetLibrary/Cat.cs,
```csharp
// The same as namespace PetLibrary{}. We use ";" just to save indent space
namespace PetLibrary;


/*
Cat class
*/
public class Cat : IRun
{
    /*
    Class field, including different variables
    they can be organized by similar characteristics
    */
    // The children of the cat
    private DateTime _dateOfBirth;
    public List<Cat> Children = new List<Cat>();

    /*
    Class properties
    */
    // The name of the cat
    public string Name { get; private set; }
    // The birthday of the cat
    public DateTime DateOfBirth
    {
        //get; // Get compile errors (C# 13 or lower) and run-time errors and warnings (C# 14 or above) because you cannot mix an auto accessor with a custom accessor.
        get
        {
            return _dateOfBirth;
        }

        private set
        {
            // Stack overflow. Repeated 24114 times.
            // DateOfBirth = value;

            // Data validation
            if (value > DateTime.Today)
                throw new ArgumentException("Date of birth must be in the past.");

            _dateOfBirth = value;
        }
    }

    /*
    Class methods, where functions should be implemented
    */

    // Constructors
    // Default constructor. It will be called by default
    public Cat()
    {
        Name = "UnknownCat";
        DateOfBirth = DateTime.Today;
        Speed = 0.0;
    }
    // Parameterized Constructor
    public Cat(string name, DateTime dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
        Speed = 0.0;
    }
    // Finalizer/Desctructor
    ~Cat()
    {
    }

    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd} and has {Children.Count} kitties.");
    }

    // Static method to "multiply"
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat($"{cat1.Name}-{cat2.Name}", DateTime.Today);
        // The same functionaly as above
        // Cat kitty = new Cat
        // {
        //     Name = $"{cat1.Name}-{cat2.Name}",
        //     DateOfBirth = DateTime.Today
        // };
        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);
        return kitty;
    }

    // Use operators instead of the above method
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return Cat.ProduceKitty(cat1, cat2);
    }

    // Instance method to "multiply"
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }

    // Implementation of interface IRun
    public double Speed { get; private set; }
    public int Distance { get; }
    public double SpeedUp(double velocity)
    {
        Speed += velocity;
        return Speed;
    }
}

// IRun is an interface
// You have variables and methods, but you don't implement them.
// Interfaces default to internal access
public interface IRun
{
    // Instance field
    // double Velocity; // compile error
    // Property
    double Speed { get; }
    int Distance { get; }

    double SpeedUp(double velocity);
}
```
```bash
$ Nana is running at speed 2 km/hr.
```

# System-Defined Interface [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)

In MyBusiness/Program.cs,
```csharp
using System;
using PetLibrary;

namespace MyBusiness; // File scoped namespaces

class Program
{
    static void Main(string[] args)
    {
        // Create cats
        Cat[] cats =
        {
            new Cat("Nana", new DateTime(2019, 12, 9)),
            new Cat("Coffee", new DateTime(2019, 6, 20)),
            new Cat("Kiwi", new DateTime(2018, 11, 19))
        };

        // Print the array
        foreach (Cat cat in cats)
        {
            cat.WriteToConsole();
        }

        // Sort the array
        Array.Sort(cats);

        // Print the array again
        Console.WriteLine("Use Cat's IComparable implementation to sort the cat instance:");
        foreach (Cat cat in cats)
        {
            cat.WriteToConsole();
        }
    }
}
```
In PetLibrary/Cat.cs,
```csharp
// The same as namespace PetLibrary{}. We use ";" just to save indent space
namespace PetLibrary;

/*
Cat class
*/
public class Cat : IRun, IComparable<Cat>
{
    /*
    Class field, including different variables
    they can be organized by similar characteristics
    */
    // The children of the cat
    private DateTime _dateOfBirth;
    public List<Cat> Children = new List<Cat>();

    /*
    Class properties
    */
    // The name of the cat
    public string Name { get; private set; }
    // The birthday of the cat
    public DateTime DateOfBirth
    {
        //get; // Get compile errors (C# 13 or lower) and run-time errors and warnings (C# 14 or above) because you cannot mix an auto accessor with a custom accessor.
        get
        {
            return _dateOfBirth;
        }

        private set
        {
            // Stack overflow. Repeated 24114 times.
            // DateOfBirth = value;

            // Data validation
            if (value > DateTime.Today)
                throw new ArgumentException("Date of birth must be in the past.");

            _dateOfBirth = value;
        }
    }

    /*
    Class methods, where functions should be implemented
    */

    // Constructors
    // Default constructor. It will be called by default
    public Cat()
    {
        Name = "UnknownCat";
        DateOfBirth = DateTime.Today;
        Speed = 0.0;
    }
    // Parameterized Constructor
    public Cat(string name, DateTime dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
        Speed = 0.0;
    }
    // Finalizer/Desctructor
    ~Cat()
    {
    }

    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd} and has {Children.Count} kitties.");
    }

    // Static method to "multiply"
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat($"{cat1.Name}-{cat2.Name}", DateTime.Today);
        // The same functionaly as above
        // Cat kitty = new Cat
        // {
        //     Name = $"{cat1.Name}-{cat2.Name}",
        //     DateOfBirth = DateTime.Today
        // };
        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);
        return kitty;
    }

    // Use operators instead of the above method
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return Cat.ProduceKitty(cat1, cat2);
    }

    // Instance method to "multiply"
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }

    // Implementation of interface IRun
    public double Speed { get; private set; }
    public int Distance { get; }
    public double SpeedUp(double velocity)
    {
        Speed += velocity;
        return Speed;
    }

    // Implementation of interface IComparible
    // Also check the class "public class Cat : IComparable<Cat>"
    public int CompareTo(Cat? anotherCat)
    {
        if (anotherCat != null)
            return Name.CompareTo(anotherCat.Name);
        else
            return 0;
    }
}

// IRun is an interface
// You have variables and methods, but you don't implement them.
// Interfaces default to internal access
public interface IRun
{
    // Instance field
    // double Velocity; // compile error
    // Property
    double Speed { get; }
    int Distance { get; }

    double SpeedUp(double velocity);
}
```
```bash
$ Nana was born on a Monday and has 0 kitties.
$ Coffee was born on a Thursday and has 0 kitties.
$ Kiwi was born on a Monday and has 0 kitties.
$ Use Cat's IComparable implementation to sort the cat instance:
$ Coffee was born on a Thursday and has 0 kitties.
$ Kiwi was born on a Monday and has 0 kitties.
$ Nana was born on a Monday and has 0 kitties.
```

---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.

## File Management

* Create and publish a package with the dotnet CLI [Doc](https://learn.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

* Visual Studio Code Tips and Tricks [Doc](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)

## Assembly

* When you write C# code and compile it (using tools like the Microsoft .NET SDK), it gets turned into an assembly. This is usually a *.exe file (executable program) or a *.dll file (reusable library)

* [Assemblies in .NET](https://learn.microsoft.com/en-us/dotnet/standard/assembly/)

## [Contextual keywords](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/)

A contextual keyword provides a specific meaning in the code, but it isn't a reserved word in C#. Some contextual keywords, such as partial and where, have special meanings in two or more contexts.

### [The value implicit parameter](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/value)

* The set accessor in property and indexer declarations uses the implicit parameter value. This parameter acts as an input for the method. The word value refers to the value that client code tries to assign to the property or indexer.

### [field keyword introduced from C# 14](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/field)
  
* Use the contextual keyword field, introduced in C# 14, in a property accessor to access the compiler-synthesized backing field of a property. 
