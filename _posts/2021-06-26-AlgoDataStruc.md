---
layout: post
author: ledinhtrunghieu
title: Algorithms and Data Structures
---
# 1. Introduction

Given a problem, a computer scientist’s goal is to develop an **algorithm**, a step-by-step list of instructions for solving any instance of the problem that might arise. Algorithms are finite processes that if followed will solve the problem. Algorithms are solutions.

It is also very common to include the word **computable** when describing problems and solutions. We say that a problem is **computable** if an algorithm exists for solving it.

Computer science, as it pertains to the problem-solving process itself, is also the study of **abstraction**. **Abstraction** allows us to view the problem and solution in such a way as to separate the so-called logical and physical perspectives

```py
import math
 math.sqrt(16)
4.0
```
This is an example of **procedural abstraction**. We do not necessarily know how the square root is being calculated, but we know what the function is called and how to use it.

**Programming** is the process of taking an algorithm and encoding it into a notation, a programming language, so that it can be executed by a computer

All data items in the computer are represented as strings of binary digits. In order to give these strings meaning, we need to have **data types**. Data types provide an interpretation for this binary data so that we can think about the data in terms that make sense with respect to the problem being solved. These low-level, built-in data types (sometimes called the primitive data types) provide the building blocks for algorithm development.


**Procedural abstraction** as a process that hides the details of a particular function to allow the user or client to view it at a very high level. We now turn our attention to a similar idea, that of **data abstraction**. An **abstract data type**, sometimes abbreviated **ADT**, is a logical description of how we view the data and the operations that are allowed without regard to how they will be implemented.

By providing this level of **abstraction**, we are creating an **encapsulation** around the data. The idea is that by encapsulating the details of the implementation, we are hiding them from the user’s view. This is called **information hiding**.

The implementation of an **abstract data type**, often referred to as a **data structure**, will require that we provide a physical view of the data using some collection of programming constructs and primitive data types. 

The separation of these two perspectives will allow us to define the complex data models for our problems without giving any indication as to the details of how the model will actually be built. This provides an **implementation-independent** view of the data.

By considering a number of different algorithms, we can begin to develop pattern recognition so that the next time a similar problem arises, we are better able to solve it.


We define a **class** to be a description of what the data look like (**the state**) and what the data can do (**the behavior**). Classes are analogous to abstract data types because a user of a class only sees the state and behavior of a data item. Data items are called **objects** in the object-oriented paradigm. An object is an instance of a class.

**Built-in Atomic Data Types**
Python has two main built-in numeric classes that implement the integer and floating point data types. These Python classes are called `int` and `float`.

The standard arithmetic operators, +, -, *, /, and ** (exponentiation), can be used with parentheses forcing the order of operations away from normal operator precedence. Other very useful operations are the remainder (modulo) operator (%) and integer division (//)

`Boolean` `True` `False`

**Built-in Collection Data Types`**

`List`

| Method Name | Use | Explanation |
| ------------------- | ------------- | ------------- |
| append  | a_list.append(item) | Adds a new item to the end of a list |
| insert  | a_list.insert(i,item) | Inserts an item at the ith position in a list |

