---
# permalink: /about/
layout: single
title: "Class"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2026-02-27
---


# Class and Members in a Class
In C#, classes are used to create custom types.

## C# Fields, Properties and Methods
**Fields:** are variables and **Methods:** are functions in a class.

```csharp
/*
The animal namespace
*/
namespace Animals;

// An animal class
public class Animal
{
}

// A cat class
public class Cat
{
    // Class fields
    // The name of the cat
    public string Name = "Unknown";
    // The birthday of the cat
    public DateTime DateOfBirth;

    // Class methods
    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
    }
}

// A dog class
public class Dog
{
}
```



## C# Class Constructor and Finalizer (Destructor)

**this** is a keyword refers to the current instance of a class.
**Dot notation .** is an operator to access a member (a field or a method) of a clas.
An **object** is an **instance** of a class. An object can be created from a class using the **new** keyword.

```csharp
using System;
using Animals;

// namespace
namespace CRC_CSD_03
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            Cat myCat = new Cat();
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));

            myCat.WriteToConsole();
            Console.WriteLine($"{nana.Name} was born on a {nana.DateOfBirth:dddd}.");
            nana.WriteToConsole();
        }
    }
}

/*
The animal namespace
*/
namespace Animals
{
    // A cat class
    public class Cat
    {
        // Class fields
        // The name of the cat
        public string Name = "Unknown";
        // The birthday of the cat
        public DateTime DateOfBirth;

        // Class methods
        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            this.Name = "Unknown";
            this.DateOfBirth = DateTime.Today;
            // You can use the following instread
            // name = "Unknown";
            // dateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
            // You "cannot" use the following instread!
            // name = name;
            // dateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
            // This function will be called when the object is desctroyed.
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }
}
```

```bash
$ Unknown was born on a Sunday.
$ Nana was born on a Monday.
```

## C# Access Modifiers [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)

**Accessibility Levels [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/accessibility-levels):** Depending on the context in which a member declaration occurs, only certain declared accessibilities are permitted. If no access modifier is specified in a member declaration, a default accessibility is used.

```csharp
using System;
using Animals;

// namespace
namespace CRC_CSD_03
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));

            nana.Poop(); // Compile error
            nana.Speak();
        }
    }
}

/*
The animal namespace
*/
namespace Animals
{
    // A cat class
    public class Cat
    {
        // Class fields
        // The name of the cat
        public string Name = "Unknown";
        // The birthday of the cat
        public DateTime DateOfBirth;

        // Class methods
        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            this.Name = "Unknown";
            this.DateOfBirth = DateTime.Today;
            // You can use the following instread
            // name = "Unknown";
            // dateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
            // You "cannot" use the following instread!
            // name = name;
            // dateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
            // This function will be called when the object is desctroyed.
        }

        // Public method
        public void Speak()
        {
            Console.WriteLine("Meow!");
            Poop();
        }

        // Private method
        private void Poop()
        {
            Console.WriteLine($"{Name} is pooping!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }
}
```

```bash
$ Meow!
$ Nana is pooping!
```

# [Properties and Encapsulation](https://www.w3schools.com/cs/cs_properties.php)

## C# Fields & Properties, What is the difference?

## C# Property [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties)

* Field: A field, also known as an instance variable or member variable, is a data storage location that belongs to an object or class. It represents the state or characteristics of an object. Fields store values and can have different data types, such as integers, strings, or custom objects. They are typically declared at the class level and can be accessed and modified by the methods within the class. Fields can have different access modifiers, such as public, private, or protected, which control their visibility and accessibility from other classes or objects.

* Property: A property, also known as an accessor or getter/setter, provides a way to access or modify the values of private fields in a controlled manner. It is a combination of methods (or functions) that are used to retrieve or assign values to a field. Properties provide encapsulation and help in maintaining the integrity of data by imposing validation or additional logic when accessing or modifying the field values. They allow controlled access to the underlying fields and can be used to expose or hide implementation details. Properties often have public visibility to allow external code to…

**Read-only property:** they have a get accessor but no set accessor. \\
**Write only property:** they have a set accessor, but no get accessor. \\
**Read Write property:** they have both a get and a set accessor. \\
**Auto-implemented property:** auto-implemented properties make property-declaration more concise when no additional logic is required in the property accessors.

```csharp
using System;
using Animals;

// namespace
namespace CRC_CSD_03
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));

            Console.WriteLine($"{nana.Name}'s age is {nana.Age}.");            
        }
    }
}

/*
The animal namespace
*/
namespace Animals
{
    // A cat class
    public class Cat
    {
        // Class fields
        // The name of the cat
        public string Name = "Unknown";
        // The birthday of the cat
        public DateTime DateOfBirth;

        // Auto-implemented property
        public int Age { get; set; }

        // private int _age;            // Camel Case for a private fields and prefix with _

        // Read-Write property
        // public int Age              // Pascal Case for a public fields or methods
        // {
        //     get { return _age; }
        //     set { _age = value; }   // Must use the keyword value
        // }

        // Read-only property
        // public int Age              // Pascal Case for a public fields or methods
        // {
        //     get { return _age; }
        // }

        // Write only property
        // public int Age              // Pascal Case for a public fields or methods
        // {
        //     set { _age = value; }   // Must use the keyword value
        // }

        // Class methods
        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            this.Name = "Unknown";
            this.DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
            // Calculate the age of the cat instance
            this.Age = DateTime.Today.Year - dateOfBirth.Year;
        }
        // Finalizer
        ~Cat()
        {
            // This function will be called when the object is desctroyed.
        }

        // Public method
        public void Speak()
        {
            Console.WriteLine("Meow!");
            Poop();
        }

        // Private method
        private void Poop()
        {
            Console.WriteLine($"{Name} is pooping!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }
}
```

```bash
$ Nana\'s age is 3.
```

## Overloading Methods

Another way to simplify methods is to make parameters optional.

```csharp
// The function describes what happens if a cat speaks
public string Meow()
{
    return "Meow!";
}

// Overloaded function of Meow(), while the input parameters are different
public string Meow(string target)
{
    return $"{name} says 'Meow {target}!'";
}

// Another function that does the same thing as the above one,
// but with a different method name
public string SayMeowTo(string target)
{
    return $"{name} says 'Meow {target}!'";
}
```

## Properties and Indexers

In namespace Animals, add
```csharp
using System;
using System.Collections.Generic; // To be able to use the data structure "list"
using Animals;

// namespace
namespace CRC_CSD_03
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Declare a cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            // Declare a cat array, static at runtime
            Cat[] catArray = new Cat[3];
            // Declare a cat list, dynamic at runtime, more flexible
            List<Cat> catList = new List<Cat>();

            for (int i = 0; i < catArray.Length; i++)
            {
                if (i == 0) catArray[i] = new Cat("Nana", new DateTime(2019, 12, 9));
                if (i == 1) catArray[i] = new Cat("Coffee", new DateTime(2019, 6, 20));
                if (i == 2) catArray[i] = new Cat("Kiwi", new DateTime(2018, 11, 19));
            }

            // Iterate the array to create cats
            for (int i = 0; i < catArray.Length; i++)
            {
                catList.Add(new Cat("Nana", new DateTime(2019, 12, 9)));
                catList.Add(new Cat("Coffee", new DateTime(2019, 6, 20)));
                catList.Add(new Cat("Kiwi", new DateTime(2018, 11, 19)));
            }

            // indexers, are another way to access a list-like data structure through indices
            catList[0].Children.Add(new Cat("Naffee", new DateTime(2019, 12, 9)));
            catList[0].Children.Add(new Cat("Kiffee", new DateTime(2019, 12, 9)));

            // Iterate the list to create cats, and you can access children like an array
            for (int i = 0; i < catList[0].Children.Count; i++)
            {
                Console.WriteLine($"The children of {catList[0].Name} include {catList[0].Children[i].Name}.");
                // The statement above and below show the same information.
                Console.WriteLine($"The children of {catList[0].Name} include {catList[0][i].Name}.");
            }
        }
    }
}

/*
The animal namespace
*/
namespace Animals
{
    // A cat class
    public class Cat
    {
        // Class fields
        // The name of the cat
        public string Name = "Unknown";
        // The birthday of the cat
        public DateTime DateOfBirth;

        // Auto-implemented property
        public int Age { get; set; }

        private static int _sharedToilet;
        public List<Cat> Children = new List<Cat>();

        // Class methods
        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            this.Name = "Unknown";
            this.DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
            // Calculate the age of the cat instance
            this.Age = DateTime.Today.Year - dateOfBirth.Year;
        }
        // Finalizer
        ~Cat()
        {
            // This function will be called when the object is desctroyed.
        }

        // Public method
        public void Speak()
        {
            Console.WriteLine("Meow!");
            Poop();
        }

        // Private method
        private void Poop()
        {
            Console.WriteLine($"{Name} is pooping!");
        }

        // Self-implemented property method, if you want to achieve something more than
        // getting and setting the variables, if necessary
        public int SharedToilet
        {
            get
            {
                return _sharedToilet;
            }
            set
            {
                _sharedToilet = value;
            }
        }

        // If you want to get or set a specific content in a list, you can use indexers
        public Cat this[int index]
        {
            get
            {
                return Children[index];
            }
            set
            {
                Children[index] = value;
            }
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }
}
```

## The keyword static [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/static)
The static keyword is also part of the using static directive. It is a member shared by all class instance.

```csharp
private static int sharedToilet;
```

---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.
