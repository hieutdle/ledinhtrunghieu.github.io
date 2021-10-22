---
layout: post
author: ledinhtrunghieu
title: Algorithms and Data Structures
---


**List**
Two common operations are indexing and assigning to an index position. Both of these operations take the same amount of time no matter how large the list becomes. When an operation like this is independent of the size of the list they are O(1).
append 0(1)
The pop() method removes the element at the specified position.
When pop is called on the end of the list it takes O(1) but when pop is called on the first element in the list or anywhere in the middle it is O(n). The reason for this lies in how Python chooses to implement lists. When an item is taken from the front of the list, in Python’s implementation, all the other elements in the list are shifted one position closer to the beginning.
that the time required to pop from the end of the list will stay constant even as the list grows in size, while the time to pop from the beginning of the list will continue to increase as the list grows.
<img src="/assets/images/20210626_AlgoDataStruc/pic2.png" class="largepic"/>

**The second major Python data structure is the dictionary**
dictionaries differ from lists in that you can access items in a dictionary by a key rather than a position.

The thing that is most important to notice right now is that the get item and set item operations on a dictionary are O(1). Another important dictionary operation is the contains operation

**Linear data structure**
Stacks, queues, deques, and lists are examples of data collections whose items are ordered depending on how they are added or removed. Once an item is added, it stays in that position relative to the other elements that came before and came after it. Collections such as these are often referred to as linear data structures.

**Stack**
A stack (sometimes called a “push-down stack”) is an ordered collection of items where the addition of new items and the removal of existing items always takes place at the same end. This end is commonly referred to as the “top.” The end opposite the top is known as the “base.”

The base of the stack is significant since items stored in the stack that are closer to the base represent those that have been in the stack the longest. The most recently added item is the one that is in position to be removed first. This ordering principle is sometimes called LIFO, last-in first-out. It provides an ordering based on length of time in the collection. Newer items are near the top, while older items are near the base.

`Stack()` creates a new stack that is empty. It needs no parameters and returns an empty stack.
`push(item)` adds a new item to the top of the stack. It needs the item and returns nothing.
`pop()` removes the top item from the stack. It needs no parameters and returns the item. The stack is modified.
`peek()` returns the top item from the stack but does not remove it. It needs no parameters. The stack is not modified.
`is_empty()` tests to see whether the stack is empty. It needs no parameters and returns a boolean value.
`size()` returns the number of items on the stack. It needs no parameters and returns an integer.













































* Given a problem, a computer scientist’s goal is to develop an **algorithm**, a step-by-step list of instructions for solving any instance of the problem that might arise. Algorithms are finite processes that if followed will solve the problem. Algorithms are solutions.
* It is also very common to include the word **computable** when describing problems and solutions. We say that a problem is **computable** if an algorithm exists for solving it.
* Computer science, as it pertains to the problem-solving process itself, is also the study of **abstraction**. **Abstraction** allows us to view the problem and solution in such a way as to separate the so-called logical and physical perspectives

```py
import math
 math.sqrt(16)
4.0
```

* This is an example of **procedural abstraction**. We do not necessarily know how the square root is being calculated, but we know what the function is called and how to use it.
* **Programming** is the process of taking an algorithm and encoding it into a notation, a programming language, so that it can be executed by a computer
* All data items in the computer are represented as strings of binary digits. In order to give these strings meaning, we need to have **data types**. Data types provide an interpretation for this binary data so that we can think about the data in terms that make sense with respect to the problem being solved. These low-level, built-in data types (sometimes called the primitive data types) provide the building blocks for algorithm development.
* **Procedural abstraction** as a process that hides the details of a particular function to allow the user or client to view it at a very high level. We now turn our attention to a similar idea, that of **data abstraction**. An **abstract data type**, sometimes abbreviated **ADT**, is a logical description of how we view the data and the operations that are allowed without regard to how they will be implemented.
* By providing this level of **abstraction**, we are creating an **encapsulation** around the data. The idea is that by encapsulating the details of the implementation, we are hiding them from the user’s view. This is called **information hiding**.
* The implementation of an **abstract data type**, often referred to as a **data structure**, will require that we provide a physical view of the data using some collection of programming constructs and primitive data types. 
* The separation of these two perspectives will allow us to define the complex data models for our problems without giving any indication as to the details of how the model will actually be built. This provides an **implementation-independent** view of the data.
* We define a **class** to be a description of what the data look like (**the state**) and what the data can do (**the behavior**). Classes are analogous to abstract data types because a user of a class only sees the state and behavior of a data item. Data items are called **objects** in the object-oriented paradigm. An object is an instance of a class.

**Built-in Atomic Data Types**
* Python has two main built-in numeric classes that implement the integer and floating point data types. These Python classes are called `int` and `float`.
* The standard arithmetic operators, +, -, *, /, and ** (exponentiation), can be used with parentheses forcing the order of operations away from normal operator precedence. Other very useful operations are the remainder (modulo) operator (%) and integer division (//)

`Boolean` `True` `False`

**Built-in Collection Data Types`**

`List`

<img src="/assets/images/20210626_AlgoDataStruc/pic1.png" class="largepic"/>


`Set`

`Dict`

`Tuple`

https://runestone.academy/runestone/books/published/pythonds3/Introduction/GettingStartedwithData.html#built-in-collection-data-types

**Input and Ouput**

```py
a_name = input("Please enter your name ")
print("Your name in all capitals is",a_name.upper(),
      "and has length", len(a_name))
```
```py
>>> s_radius = input("Please enter the radius of the circle ")
Please enter the radius of the circle 10
>>> s_radius
'10'
>>> radius = float(s_radius)
>>> radius
10.0
>>> diameter = 2 * radius
>>> diameter
20.0
>>>
```
**String Formatting**

https://runestone.academy/runestone/books/published/pythonds3/Introduction/InputandOutput.html

```py
>>> print("Hello")
Hello
>>> print("Hello", "World")
Hello World
>>> print("Hello", "World", sep="***")
Hello***World
>>> print("Hello", "World", end="***")
Hello World***>>>
```


```py
>>> price = 24
>>> item = "banana"
>>> print("The %s costs %d cents" % (item, price))
The banana costs 24 cents
>>> print("The %+10s costs %5.2f cents" % (item, price))
The     banana costs 24.00 cents
>>> print("The %+10s costs %10.2f cents" % (item, price))
The     banana costs      24.00 cents
>>> itemdict = {"item": "banana", "cost": 24}
>>> print("The %(item)s costs %(cost)7.1f cents" % itemdict)
The banana costs    24.0 cents
>>>
```

`format` method `formatter` class

```py
>>> print("The {} costs {} cents".format(item, price))
The banana costs 24 cents
>>> print("The {:s} costs {:d} cents".format(item, price))
The banana costs 24 cents
>>>
```

`f-string`
```py
>>> print(f"The {item:10} costs {price:10.2f} cents")
The banana     costs      24.00 cents
>>> print(f"The {item:<10} costs {price:<10.2f} cents")
The banana     costs 24.00      cents
>>> print(f"The {item:^10} costs {price:^10.2f} cents")
The   banana   costs   24.00    cents
>>> print(f"The {item:>10} costs {price:>10.2f} cents")
The     banana costs      24.00 cents
>>> print(f"The {item:>10} costs {price:>010.2f} cents")
The     banana costs 0000024.00 cents
>>> itemdict = {"item": "banana", "price": 24}
>>> print(f"Item:{itemdict['item']:.>10}\n" +
... f"Price:{'$':.>4}{itemdict['price']:5.2f}")
Item:....banana
Price:...$24.00
>>>
```

