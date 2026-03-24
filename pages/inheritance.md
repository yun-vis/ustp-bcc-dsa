---
# permalink: /about/
layout: single
title: "Inheritance"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2025-03-06
---

# Class Inheritance [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance)

**Inheritance**, together with encapsulation and polymorphism, is one of the three primary characteristics of object-oriented programming.

In PetLibrary/Animals.cs, add another class
```csharp
public class WildCat : Cat
{
    public string? CountryCode { get; set; }
    public DateTime FoundDate { get; set; }

    // Default Constructor
    public WildCat()
    {
    }

    // Parameterized Constructor
    public WildCat(Cat cat)
    {
        this.Name = cat.Name;
        this.DateOfBirth = cat.DateOfBirth;
    }

    // Overridden methods
    public override string ToString()
    {
        return $"{Name}'s code is {CountryCode}";
    }

    // Hidden methods
    public new void WriteToConsole()
    {
        Console.WriteLine(format:
          "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
          arg0: Name,
          arg1: DateOfBirth,
          arg2: FoundDate);
    }
}
```

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Create a cat array
            Cat[] cats =
            {
                new Cat("Nana", new DateTime(2019, 12, 9)),
                new Cat("Coffee", new DateTime(2019, 6, 20)),
                new Cat("Kiwi", new DateTime(2018, 11, 19))
            };

            // Initialize objects by using an object initializer
            // Cat: base class or parent class
            // Wild: derived class or child class
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            Cat petCat = leopard;

            // Method that used new keyword
            leopard.WriteToConsole();
            petCat.WriteToConsole();
            // Method that used override keyword
            Console.WriteLine(leopard.GetName());
            Console.WriteLine(petCat.GetName());        
            // It is calling the method from the derived class. Strange?
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public class Cat
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;
        // The children of the cat
        public List<Cat> Children = new List<Cat>();

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
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        // Must should have existed from the base class, from System.Object in this case
        // ToString has been defined in the namespace System
        public override string ToString()
        {
            return Name;
        }

        public virtual string GetName()
        {
            return Name;
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Overridden methods
        // Based on the keyword override
        public override string ToString()
        {
            return $"{Name}: from {CountryCode}";
        }

        public override string GetName()
        {
            return $"{Name}: from {CountryCode}";
        }

        // Hidden methods
        // Based on the keyword new
        public new void WriteToConsole()
        {
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```
```bash
$ Alice was born on 15/03/22 and found on 01/01/01
$ Alice was born on a Tuesday.
$ Alice: from Taiwan
$ Alice: from Taiwan
```

## Initialize a Derived Class

You can create a constructor and pass the base object during the declaration.

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // Main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Create a cat array
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            WildCat nanaQ = new WildCat(nana);

            nana.WriteToConsole();
            leopard.WriteToConsole();
            nanaQ.WriteToConsole();
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
public class WildCat : Cat
{
    public string? CountryCode { get; set; }
    public DateTime FoundDate { get; set; }

    // Default Constructor
    public WildCat()
    {
    }

    // Parameterized Constructor
    public WildCat(Cat cat)
    {
        this.Name = cat.Name;
        this.DateOfBirth = cat.DateOfBirth;
    }

    // Hidden methods
    // Based on the keyword new
    public new void WriteToConsole()
    {
        // base.WriteToConsole();
        Console.WriteLine(format:
          "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
          arg0: Name,
          arg1: DateOfBirth,
          arg2: FoundDate);
    }
}
```

## base Keyword [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/base)

The base keyword is used to access members of the base class from within a derived class.


## Virtual Function [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/virtual)

By default, methods are non-virtual. You cannot override a non-virtual method.


## Override and New Keywords [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords)

In **method overriding** (using the keyword **override**), when base class reference variable pointing to the object of the derived class, then it will call the overridden method in the derived class.

In the **method hiding** (using the keyword **new**), when base class reference variable pointing to the object of the derived class, then it will call the hidden method in the base class.

E.g., when a method is hidden with new, the compiler is not smart enough to know that the object is an WildCat, so it calls the WriteToConsole method in Cat.


## Single Inheritance

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Declare a dog
            Dog shiba = new Dog();
            shiba.Speak();

            // Declare a cat and a wild cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            nana.Speak();
            leopard.Speak();
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public class Animal
    {
        public virtual void Speak()
        {
            Console.WriteLine("");
        }
    }

    public class Dog : Animal
    {
        public override void Speak()
        {
            Console.WriteLine("Woof!");
        }
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

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
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }

        // Parameterized Constructor
        public WildCat(Cat cat)
        {
            this.Name = cat.Name;
            this.DateOfBirth = cat.DateOfBirth;
        }

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // Hidden methods
        // Based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```

## Nested Inheritance [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance)

Note that a class can derive from only a single direct base class. But what if I need information from multiple classes? Use interface.

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Declare a dog
            Dog shiba = new Dog();
            shiba.Speak();

            // Declare a cat and a wild cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            nana.Speak();
            leopard.Speak();
        }
    }
}
```

In PetLibrary/Animals.cs
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public class Animal
    {
        public virtual void Speak()
        {
            Console.WriteLine("");
        }
    }

    public class Dog : Animal, IRun
    {
        public override void Speak()
        {
            Console.WriteLine("Woof!");
        }

        // Implementation of interface IRun
        public double Speed { get; set; }
        public int Distance { get; }
        public double SpeedUp(double velocity)
        {
            Speed += 2.0 * velocity;
            return Speed;
        }
    }

    interface IRun
    {
        // Instance field
        // double Velocity; // compile error
        // Property
        double Speed { get; set; }
        int Distance { get; }

        double SpeedUp(double velocity);
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

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
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }
     
        // Parameterized Constructor
        public WildCat(Cat cat)
        {
            this.Name = cat.Name;
            this.DateOfBirth = cat.DateOfBirth;
        }

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // Hidden methods
        // Based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```
```bash
$ Woof!
$ Meow!
$ WildMeow!
```

## Abstract Class [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/abstract)

You have now seen two ways to change the behavior of an inherited method. We can hide it using the **new** keyword (known as non-polymorphic inheritance), or we can **override** it (known as polymorphic inheritance).

In MyBusiness/Program.cs,
```csharp
using System;
namespace CRC_CSD_06;

class Program
{
    static void Main(string[] args)
    {
        // Create a cat array
        Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
        WildCat leopard = new WildCat
        {
            Name = "Alice",
            CountryCode = "Taiwan"
        };

        nana.Speak();
        nana.Move();
        leopard.Speak();
        leopard.Move();
    }
}
```

In PetLibrary/Animals.cs,
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public abstract class Animal
    {
        // Abstract method only defines the method name, but no implementation
        public abstract void Speak();
        // Virtual method defines the name and implementation
        public virtual void Move()
        {
            Console.WriteLine("Move like an animal!");
        }
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

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
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        /*
        Class methods, where functions should be implemented
        */
        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        public override void Move()
        {
            Console.WriteLine("Move like a cat!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }
    
        // Parameterized Constructor
        public WildCat(Cat cat)
        {
            this.Name = cat.Name;
            this.DateOfBirth = cat.DateOfBirth;
        }        

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // Hidden methods
        // Based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```

## Preventing Inheritance and Overriding

Using the keyword sealed.

In PetLibrary/Animals.cs, add
```csharp
public class WildCat : Cat
{
    public sealed override string GetName()
    {
        return $"{Name}: from {CountryCode}";
    }  
}

public class MonsterCat : WildCat
{
    public override string GetName()
    {
        return "I am a monster.";
    }
}
```
```bash
$ cannot override inherited member because it is sealed
```

---
# Selected Theory
