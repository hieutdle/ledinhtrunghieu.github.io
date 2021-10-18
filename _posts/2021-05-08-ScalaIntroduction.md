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

**The Scala interpreter**
<img src="/assets/images/20210508_ScalaIntroduction/pic7.png" class="largepic"/>
<img src="/assets/images/20210508_ScalaIntroduction/pic8.png" class="largepic"/>

**What makes Scala scalable?**

The scalability of a language depends on several factors. The creator of the language (Martin Odersky), however, believes there are two main factors that contribute the most to Scala being scalable: Scala being object-oriented and Scala being functional.

<img src="/assets/images/20210508_ScalaIntroduction/pic9.png" class="largepic"/>

**Scala is object-oriented**
```scala
// Same result
val sumB = 2.+(4)
val sumA = 2 + 4
```
**Reasons for programming in Scala**
* Scala is compatible
* Scala is concise
* Scala is statically-typed
* Scala is high-level
* Scala lets you write small and/or large programs, elegantly

## 1.3. Immutable variables (val) and value types

**Scala has two kinds of variables**
<img src="/assets/images/20210508_ScalaIntroduction/pic10.png" class="largepic"/>
Scala has two kinds of variables: vals and vars. vals are immutable, which means once initialized, vals can't be reassigned. In Twenty-One, a val is like any non-ace card. The value of the 4 of hearts will always be 4 points. One way to define the val fourHearts is by typing val fourHearts colon Int, then equals 4. If you're familiar with Java, a val is kind of like a final variable.

**Reassigning a val produces an error**
<img src="/assets/images/20210508_ScalaIntroduction/pic11.png" class="largepic"/>

**Scala value types**
* Double: 64-bit IEEE-754 double-precision foating point number. 4.94065645841246544e-324d to 1.79769313486231570e+308d (positive or negative). **Double is more precise than Float. Double stores pi to 15 points beyond the decimal. Float, specified by adding an f at the end, stores pi to 7 points.**
* Float
* Long
* Int 32-bit signed integer. -2^31 to 2^31-1, inclusive. -2,147,483,648 to 2,147,483,647
* Short
* Byte
* Char
* Boolean
* Unit

**Char and String**

**Char**
* 16-bit unsigned Unicode integer 
* 0 to 2^16-1, inclusive
* 0 to 65,535

**String**
* `String`:a sequence of `Char`

<img src="/assets/images/20210508_ScalaIntroduction/pic12.png" class="largepic"/>

The type Int names the class Int in the package scala. The full name of Int is scala dot Int, but you can use the simple name Int since the package scala is automatically imported into every Scala source file.

<img src="/assets/images/20210508_ScalaIntroduction/pic13.png" class="largepic"/>

**Every value in Scala is an object**. Here, fourHearts is an object. That is, an instance of class Int. So is fiveHearts.

<img src="/assets/images/20210508_ScalaIntroduction/pic14.png" class="largepic"/>
All of these Scala types (except Unit, which you'll learn about later) have equivalent Java primitive types that live in the package java dot lang. When you compile Scala code to Java bytecode, the Scala compiler will use these Java types where possible which is HUGE for code performance.

**Practice**
**Define immutable variables (val)**
```scala
// Define immutable variables for clubs 2♣ through 4♣
val twoClubs: Int = 2
val threeClubs: Int = 3
val fourClubs: Int = 4
```

**Don't try to change**
```scala
// Define immutable variables for player names
val playerA: String = "Alex"
val playerB: String = "Chen"
val playerC: String = "Marta"

// Change playerC from Marta to Umberto
playerC = "Umberto"

cmd0.sc:6: reassignment to val
val res0_3 = playerC = "Umberto"
                     ^Compilation Failed
```

## 1.4. Mutable variables (var) and type inference
<img src="/assets/images/20210508_ScalaIntroduction/pic15.png" class="largepic"/>

**Reassigning a var works**
```sql
scala> var aceSpades: Int = 1

aceSpades: Int = 1

scala> aceSpades = 11

aceSpades: Int = 11
```

**Pros and cons of immutability**
It seems like having a variable that you CAN'T change is a terrible idea and that we should always use vars over vals. But, in Scala, we actually prefer immutable variables (vals, that is) where possible.
**Pros**
* Your data won't be changed accidentally, by an error in your program's logic for example. Preferring immutability is a form of defensive coding
* Your code is easier to reason about since you don't have to mentally juggle all of the places where your data can change. 
* Immutability means you'll have to write fewer unit tests to make sure your program works as expected.

**Cons**
* The extra memory generated by copying objects. Since vals can't be reassigned, to change them you'll need to create a new object. Your programs will be a little larger. 

Fortunately, unless you are working with massive data structures and copying them thousands of times a second, the pros often outweigh the cons.

**Scala nudges use towards immutability.** BECAUSE we are forced to copy our objects each time we want to make a change, we become much more conscious of HOW and WHEN we change our program's state. With this philosophy, we receive fewer possible states within our programs, fewer defects, and codebases that are easier to maintain.

**Type inference**

Scala can infer that fourHearts is an Int because it knows whole numbers like 4 are typically meant to be integers. Type inference not only applies to variables, but also collections, functions, and more, with the rule of thumb being programmers can omit almost all type information that's considered annoying. Type inference saves keystrokes and characters, which is a big deal in massive codebases.
<img src="/assets/images/20210508_ScalaIntroduction/pic16.png" class="largepic"/>

**Semicolons**
There are no semicolons at the end of these statements. There could be, but Scala tries very hard to let us skip semicolons wherever possible. 

**Practice**

**Define mutable variables (var)**
```scala
// Define mutable variables for all aces
var aceClubs, aceDiamonds, aceHearts, aceSpades = 1

// Create a mutable variable for player A (Alex)
var playerA = "Alex"

// Change the point value of A♦ from 1 to 11
aceDiamonds = 11

// Calculate hand value for J♣ and A♦
println(jackClubs + aceDiamonds)
```
<img src="/assets/images/20210508_ScalaIntroduction/pic17.png" class="largepic"/>

# 2. Workflows, Functions, Collections

Discover two more ways of writing Scala code (writing a script and building an application) and popular tools that make writing these programs easier. Learn what functions do and how to use them, and structure your data using the Array and List collections.

## 2.1. Scripts, applications, and real-world workflows

**Scala Scripts**
* A script is a sequence of instructions in a file that are executed sequentially. 
* Scripts are great for smaller tasks that can fit into a single file, like sending a templated email from the command line. When you clicked "Run Code" and "Submit Answer", you actually ran a Scala script!
* At a command prompt, the `scala` command executes a script by wrapping it in a template, then compiling and executing the resulting program.

If we put this code into a  file named `game.scala`:
```scala
// Start game
println("Let's play Twenty-One!")
```
Then run:
```scala
$ scala game.scala

Let's play Twenty-One!
```

**Interpreted language vs. compiled language**
**Interpreter**: a program that directly executes instructions written in a programming language, without requiring them previously to have been compiled into machine code.
**Compiler:** a program that translates source code from a high- level programming language to a lower level language (e.g., machine code) to create an executable program.
The Scala interpreter "hides" the compilation step from you, the programmer, and it appears that the code is run immediately. Same thing for scripts.

**Scala Application**
* Must be compiled explicitly then run explicitly
* Consist of many source files that can be compiled individually 
* Useful for larger programs and complex project
* No lag time since applications are precompiled, whereas scripts are compiled and executed every time.

If we put this code into a  file named `game.scala`:
```scala
object Game extends App 
    { println("Let's play Twenty-One!")
}
```
First, compile with	`scalac`: The Scala compiler, which is named `scalac`, translates the code you wrote to Java bytecode
```scala
$ scalac Game.scala
```
Second, run with `scala`: That's when that compiled code is executed on a Java virtual machine.
```scala
$ scala Game

Let's play Twenty-One!
```

**Pros and cons of compiled languages**

**Pros**
* Increased performance once compiled (execution speed since code doesn't need to be interpreted on the fly).

**Cons**
* It takes time to compile code, sometimes minutes for large Scala programs. 

This trade-off is usually worth it, which is why companies are switching to Scala since milliseconds of load times can mean millions in added revenue.

**Scala workflow**
There are two main ways people prefer to work in Scala:
* Using the command line
* Using an IDE (integrated development environment)

**IDE**
IDEs shine when programmers need to build or work within large-scale systems.

**sbt**
The most popular tool for building Scala apps is called sbt, "simple build tool". sbt lets you compile, run, and test your Scala applications. 

**Scala kernel for Jupyter**
Scala also works in Jupyter Notebooks with a Scala kernel called Almond. With this setup, the data scientist can create plots to visualize their data, do big data analysis using Apache Spark, and more.

**Practice**
**Benefits of compiled languages**
Compiled code performs better (i.e., is faster). This performance boost tend to grow in importance as program size grows.

## 2.2. Functions

**What is function**
Functions are invoked with a list of arguments to **produce a result**

**What are the parts of a function?**
1. Parameter list
2. Body
3. Result type

One of the defining features of Scala being functional is that functions are first-class VALUES. The equals sign that precedes the code block is a tell: all functions produce RESULTS, and all results have VALUES, and all values have VALUE TYPES.

**The bust function**
```scala
// Define a function to determine if hand busts 
def bust(hand: Int): Boolean = {
hand > 21
}
println(bust(20)) 
println(bust(22))

false 
true
```

**Call a function with variables**

```scala
println(bust(kingSpades + tenHearts))

false
```

**Kinds of functions**
* Method: functions that are members of a class, trait, or singleton object
* Local function: functions that are defined inside other functions
* Procedure: functions with the result type of `Unit`
* Function literal: anonymous functions in source code (at run time, function literals are instantiated into objects called function values)

**Practice**

**Call a function**

```scala
def maxHand(handA: Int, handB: Int): Int = {
  if (handA > handB) handA
  else handB
}

// Calculate hand values
var handPlayerA: Int = queenDiamonds + threeClubs + aceHearts + fiveSpades
var handPlayerB: Int = kingHearts + jackHearts

// Find and print the maximum hand value
println(maxHand(handPlayerA, handPlayerB))

20
```

## 2.3. Arrays
**Collection**



