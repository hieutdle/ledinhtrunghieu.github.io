---
layout: post
author: ledinhtrunghieu
title: Lesson 17 - Introduction to Scala
---

# 1. Introduction to Scala

What Scala is, who uses it, and why you should use it. Exploring four common data types: integers, floating-point numbers, logical values, and text, using the Scala interpreter.

## 1.1. A Scalable language

**Scala** is a general-purpose programming language providing support for functional programming and a strong static type system. Designed to be concise, many of Scala's design decisions aimed to address criticisms of Java.

**Scala** source code is intended to be compiled to Java bytecode, so that the resulting executable code **runs on a Java virtual machine**.

**Staircase to mastery**
<img src="/assets/images/20210508_ScalaIntroduction/pic4.png" class="largepic"/>

**"Scala"** means **"staircase"** in Italian and the logo was inspired by a spiral staircase in the building where the language was born!

**Why use Scala?**

<img src="/assets/images/20210508_ScalaIntroduction/pic1.png" class="largepic"/>

Scala stands for scalable language. It is designed to grow with the demands of its users, from writing small scripts (which you'll do in this course) to building massive systems for data processing, distributed computing, and more (just like the engineers at these companies do).

<img src="/assets/images/20210508_ScalaIntroduction/pic2.png" class="largepic"/>

**The cathedral vs. the bazaar**

<img src="/assets/images/20210508_ScalaIntroduction/pic3.png" class="largepic"/>

An analogy borrowed from Eric Raymond's The Cathedral and the Bazaar book is apt. A cathedral is a building with rigid perfection. It takes a long time to build and rarely changes after being built. A bazaar, on the other hand, is flexible. It is adapted and extended often by the people working in it. Scala is like a bazaar as it is designed to be adapted and extended by the people programming in it. 

**Flexible and convinient**

**Flexible**
Scala lets you add new types, collections, and control constructs that feel like they are built-in to the language

**Convenient**
The Scala standard library has a set of convenient predefined types, collections, and control constructs.

**Who use Scala?**

**Roles**
* Software Engineer
* Data Engineer
* Data Scientist
* Machine Learning Engineer

**Industries**
* Finance
* Tech
* Healthcare
* So many more...

**Practice**
<img src="/assets/images/20210508_ScalaIntroduction/pic5.png" class="largepic"/>

<img src="/assets/images/20210508_ScalaIntroduction/pic6.png" class="largepic"/>


## 1.2. Scala code and the Scala interpreter

**What is Scala?**

Scala combines object-oriented and functional programming in one concise, high-level language. Scala's static types help avoid bugs in complex applications, and its JVM and JavaScript runtimes let you build high-performance systems with easy access to huge ecosystems of libraries.

**Scala fuses OOP and FP**

* Scala combines object-oriented and functional programming
* Scala is scalable

**Scala is object-oriented**
* Every value is an object
* Every operation is a method call

```py
val sumA = 2 + 4
val sumA = 2.+(4)

sumA: Int = 6
```

**Scala is functional**
1. Functions are first-class values like Int or Str
2. Operations of a program should map input values to output values rather than change data in place (Function should not have side effects)

**More answer to "Why use Scala**

Scala combines object-oriented and functional programming in one concise, high-level language. Scala's static types help avoid bugs in complex applications, and its JVM and JavaScript runtimes let you build high-performance systems with easy access to huge ecosystems of libraries.

* Scala is **concise**. Scala programs tend to be short, often down to one tenth of the number of lines compared to Java programs.
* Scala is **high-level**, which means you won't deal with the details of the computer in your Scala code. In turn, your code becomes shorter and easier to understand. You have fewer opportunities to make mistakes.
* Scala has an **ADVANCED static type system** that reduces verbosity in your code and adds language flexibility. These are two common criticisms of static typing.
* Scala is **compatible**, which means you can build on previously existing Java code. That's huge. Scala runs primarily on the Java Virtual Machine. It heavily reuses Java types. Plus more.











