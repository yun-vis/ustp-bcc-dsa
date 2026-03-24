---
# permalink: /about/
layout: single
title: "Assignment"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2023-03-08
---

# ASCII Art [Doc]()

**ASCII Code:** It stands for American Standard Code for Information Interchange. [Doc](https://en.wikipedia.org/wiki/ASCII)

**ASCII Art:** ASCII art is a graphic design technique that uses computers for presentation and consists of pictures pieced together from the 95 printable (from a total of 128) characters defined by the ASCII Standard. [Examples](https://ascii.co.uk/art).
```csharp
using System;

// namespace
namespace MyApplication
{
    class ASCIIArt
    {
        public static void Main(string[] args)
        {
            int num = 5;
            for (int i = 0; i < num; i++)
            {
                string s = "";
                for (int j = 0; j < num; j++)
                {
                    if (j > num-i-2)
                        s += "*";
                    else
                        s += " ";
                }
                for (int j = 0; j < num; j++)
                {
                    if (j > i)
                        s += " ";
                    else
                        s += "*";
                }
                Console.WriteLine(s);
            }
        }
    }
}
```
```bash
$     **    
$    ****   
$   ******  
$  ********
$ **********
```
