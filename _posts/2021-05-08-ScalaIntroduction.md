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
* Mutable collections: can be updated or extended in place, can change, add, or remove elements of the originally created collection
* Immutable collections: never change

**Array**
* Mutable sequence of objects that share the same type
* Parameterize an array: configure its types and parameter values
```scala
scala> val players = new Array[String](3)

players: Array[String] = Array(null, null, null)

Type parameter: String
Value parameter: length which is 3
```
* Initialize elements of an array: give the array data
```scala
scala>	players(0)	=	"Alex"
scala>	players(1)	=	"Chen"
scala>	players(2)	=	"Marta"
scala>	players		

res3: Array[String] = Array(Alex, Chen, Marta)
```

```scala
scala> val players = Array("Alex", "Chen", "Marta")

players: Array[String] = Array(Alex, Chen, Marta)
```

**Array are mutable**
```scala
scala> val players = Array("Alex", "Chen", "Marta")

players: Array[String] = Array(Alex, Chen, Marta)

scala> players(0) = "Sindhu"

res5: Array[String] = Array(Sindhu, Chen, Marta)
```

You can only update arrays with values of the same type of the array
<img src="/assets/images/20210508_ScalaIntroduction/pic18.png" class="largepic"/>

**Recommendation: use val with Array**
<img src="/assets/images/20210508_ScalaIntroduction/pic19.png" class="largepic"/>
it is recommended that you make mutable collections vals so you don't have to keep track of two things that can change. If we made players a var, the elements in our original array could change, like the update to include Sindhu, AND an entirely different Array object could be assigned to players.

Scala nudges us towards immutability, and arrays are NOT immutable. Because collections store more data than the single value variables you previously created, immutability with collections increases the potential for hard-to-find bugs.

**The Any supertype**
<img src="/assets/images/20210508_ScalaIntroduction/pic20.png" class="largepic"/>
you can mix and match types by parameterizing with Any, the supertype of all types.

**Practice**
**Create and parameterize an array**
```scala
// Create and parameterize an array for a round of Twenty-One
val hands: Array[Int] = new Array[Int](3)
```

**Initialize an array**
```scala
// Create and parameterize an array for a round of Twenty-One
val hands: Array[Int] = new Array[Int](3)

// Initialize the first player's hand in the array
hands(0) = tenClubs + fourDiamonds

// Initialize the second player's hand in the array
hands(1) = nineSpades + nineHearts

// Initialize the third player's hand in the array
hands(2) = twoClubs + threeSpades

// Create, parameterize, and initialize an array for a round of Twenty-One
val hands = Array(tenClubs + fourDiamonds,
                  nineSpades + nineHearts,
                  twoClubs + threeSpades)

hands: Array[Int] = Array(14, 18, 5)
```

**Updating arrays**
```scala
// Initialize player's hand and print out hands before each player hits
hands(0) = tenClubs + fourDiamonds
hands(1) = nineSpades + nineHearts
hands(2) = twoClubs + threeSpades
hands.foreach(println)

// Add 5♣ to the first player's hand
hands(0) = hands(0) + fiveClubs

// Add Q♠ to the second player's hand
hands(1) = hands(1) + queenSpades

// Add K♣ to the third player's hand
hands(2) = hands(2) + kingClubs

// Print out hands after each player hits
hands.foreach(println)

14
18
5
19
28
15
```

## 2.4. Lists

**Mutable collections:** can be updated or extended in place. Mutable meaning you can change, add, or remove elements of an Array, though you can't change the parameter values like the length of the array
* Array: mutable sequence of objects with the same type

**Immutable collections:** never change
* List: immutable sequence of objects with the same type. Like with Array, "share the same type" unintuitively means you can have mixed types when you use the Any supertype.

**Lists have a type parameter**
```scala
Array
scala> val players = Array("Alex", "Chen", "Marta")
players: Array[String] = Array(Alex, Chen, Marta)

List
scala> val players = List("Alex", "Chen", "Marta")
players: List[String] = List(Alex, Chen, Marta)
```

**How Lists are useful while immutable**
* `List`: has methods, like all of Scala collections
  * Method: a function that belongs to an object
* There are many `List` methods
 * myList.drop()
 * myList.mkString(", ")
 * myList.length
 * myList.reverse

```scala
scala> val players = List("Alex", "Chen", "Marta")
players: List[String] = List(Alex, Chen, Marta)

scala> val newPlayers = "Sindhu" :: players
newPlayers: List[String] = List(Sindhu, Alex, Chen, Marta)
```
or make `players` a `var`
```scala
scala> var players = List("Alex", "Chen", "Marta")
players: List[String] = List(Alex, Chen, Marta)

scala> var players = "Sindhu" :: players
newPlayers: List[String] = List(Sindhu, Alex, Chen, Marta)
```

**cons(:)**
* Prepends a new element to the **beginning** of an existing `List` and returns the resulting `List`

**Nil**
* Nil is an empty list
```scala
scala> Nil

res0: scala.collection.immutable.Nil.type = List()
```

A common way to initialize new lists combines `Nil` and `::`
```scala
scala> val players = "Alex" :: "Chen" :: "Marta" :: Nil

players: List[String] = List(Alex, Chen, Marta)

scala> val playersError = "Alex" :: "Chen" :: "Marta"

<console>:11: error: value :: is not a member of String val playersError = "Alex" :: "Chen" :: "Marta"
```

**Concatenating Lists**
* `:::` for concatenation
```scala
val	playersA =	List("Sindhu", "Alex")
val	playersB =	List("Chen", "Marta")
val	allPlayers	= playersA ::: playersB
println(playersA + " and " + playersB + " were not mutated,")
println("which means " + allPlayers + " is a new List.")

List(Sindhu, Alex) and List(Chen, Marta) were not mutated, 
which means List(Sindhu, Alex, Chen, Marta) is a new List.
```

**Practice**
**Initialize and prepend to a list**
```scala
// Initialize a list with an element for each round's prize
val prizes = List(10, 15, 20, 25, 30)
println(prizes)

// Prepend to prizes to add another round and prize
val newPrizes = 5 :: prizes
println(newPrizes)

prizes: List[Int] = List(10, 15, 20, 25, 30)
newPrizes: List[Int] = List(5, 10, 15, 20, 25, 30)
List(10, 15, 20, 25, 30)
List(5, 10, 15, 20, 25, 30)
```

**Initialize a list using cons and Nil**
```scala
// Initialize a list with an element each round's prize
val prizes = 10 :: 15 :: 20 :: 25 :: 30 :: Nil
println(prizes)
```

**Concatenate Lists**
```scala
// The original NTOA and EuroTO venue lists
val venuesNTOA = List("The Grand Ballroom", "Atlantis Casino", "Doug's House")
val venuesEuroTO = "Five Seasons Hotel" :: "The Electric Unicorn" :: Nil

// Concatenate the North American and European venues
val venuesTOWorld = venuesNTOA ::: venuesEuroTO

venuesTOWorld: List[String] = List(
  "The Grand Ballroom",
  "Atlantis Casino",
  "Doug's House",
  "Five Seasons Hotel",
  "The Electric Unicorn"
)
```

# 3. Type Systems, Control Structures, Style
Learn about Scala's advanced static type system. After learning how to control your program with if/else, while loops, and the foreach method, Convert imperative-style code to the Scala-preferred functional style.

## 3.1. Scala's static type system

**Some definitions**
* Type: restricts the possible values to which a variable can refer, or an expression can produce, at run time
* Compile time: when source code is translated into machine code, i.e., code that a computer can read
* Run time: when the program is executing commands (a er compilation, if compiled)

**Type systems**
* Static type systems: A language is statically typed if the type of a variable is known at compile time. That is, types checked before run-time: C/C++, Fortran, Java, Scala
* Dynamic type systems A language is dynamically typed if types are checked on the fly. That is, types are checked during execution (i.e., run-time): Javascript, Python, Ruby, R

**Pros of static type system**
* Increased performance at run-time: statically typed languages don't HAVE to check types when someone hits "run program", they execute a little faster.
* Properties of your program verified (i.e., prove the absence of common type-related bugs) Static type systems can prove the absence of certain run-time errors,
* Safe refactorings.  A static type system lets you make changes to a codebase with a high degree of confidence. When you recompile your system after a refactor, Scala will alert you of the lines you tweaked that caused a type error, then you fix them
* Documentation in the form of type annotations ( `:Int` in `val fourHearts: Int = 4`)

**Cons of static type systems**
* It takes time to check types (i.e, delay before execution)
* Code is verbose (i.e., code is longer/more annoying to write)
* The language is inflexible, preventing coders from expressing themselves as they wish (e.g., one strict way of composing a type)

**Reducing verbosity (with variables)**
Without type inference
```scala
scala> val fourHearts: Int = 4

fourHearts: Int = 4

scala> val players: Array[String] = Array("Alex", "Chen", "Marta")

players: Array[String] = Array(Alex, Chen, Marta)
```
With type inference
```scala
scala> val fourHearts = 4

fourHearts: Int = 4

scala> val players = Array("Alex", "Chen", "Marta")

players: Array[String] = Array(Alex, Chen, Marta)
```

**Promoting flexibility**
* Pattern matching
* Innovative ways to write and compose types

**Compiled, statically-typed languages**
Compiled languages
* Increased performance at run time
Statically-typed languages
* Increased performance at run time

Scala being both compiled AND statically-typed doubles down on the "increased performance" benefit. Because the compiler knows the exact data types that are used, it can allocate memory accordingly to make the code faster and memory efficient.


**Static typing vs. dynamic typing**
<img src="/assets/images/20210508_ScalaIntroduction/pic21.png" class="largepic"/>

**Pros and cons of static type systems**
<img src="/assets/images/20210508_ScalaIntroduction/pic22.png" class="largepic"/>

## 3.2. Make decisions with if and else

**A program for playing Twenty-One**
**Variables**
```scala
val fourHearts: Int = 4 
var aceClubs: Int = 1
```
Collections
```scala
val hands: Array[Int] = new Array[Int](3)`
```
Functions
```scala
// Define a function to determine if hand busts def bust(hand: Int) = {
hand > 21
}
```
A control structure is a block of programming that analyses variables and chooses a direction in which to go based on given parameters. The term flow control details the direction the program takes (which way program control "flows")
* if/else

**if-else control flow**
```scala
def maxHand(handA: Int, handB: Int): Int = { if (handA > handB) handA
else handB
}

//	Point	values	for	two	competing	hands
val	handA	= 17				
val	handB	= 19				

// Print the value of the hand with the most points 
if (handA > handB) println(handA)
else println(handB)
```
```scala
// Point values for two competing hands val handA = 26
val handB = 20

// If both hands bust, neither wins
if (bust(handA) & bust(handB)) println(0)
// If hand A busts, hand B wins  
else if (bust(handA)) println(handB)
// If hand B busts, hand A wins  
else if (bust(handB)) println(handA)
// If hand A is greater than hand B, hand A wins 
else if (handA > handB) println(handA)
// Hand B wins otherwise 
else println(handB)
```
```scala
scala> val maxHand = if (handA > handB) handA else handB

maxHand: Int = 19
```
**Relational and logical operators, these "operators" are actually methods!**
<img src="/assets/images/20210508_ScalaIntroduction/pic23.png" class="largepic"/>

**Practice**
```scala
// Point value of a player's hand
val hand = sevenClubs + kingDiamonds + threeSpades

// Inform a player where their current hand stands
val informPlayer: String = {
  if (hand > 21)
    "Bust! :("
  else if (hand == 21) 
    "Twenty-One! :)"
  else
    "Hit or stay?"
}

// Print the message
print(informPlayer)

// Find the number of points that will cause a bust
def pointsToBust(hand: Int): Int = {
  // If the hand is a bust, 0 points remain
  if (bust(hand))
    0
  // Otherwise, calculate the difference between 21 and the current hand
  else
    21 - hand
}

// Test pointsToBust with 10♠ and 5♣
val myHandPointsToBust = pointsToBust(tenSpades + fiveClubs)
println(myHandPointsToBust)

myHandPointsToBust: Int = 6
6
```

## 3.3. while and the imperative style
**Loop with while**
```scala
// Define counter variable
var i = 0

// Define the number of times for the cheer to repeat
val numRepetitions = 3

// Loop to repeat the cheer 
while (i < numRepetitions) {
 println("Hip hip hooray!") 
 i += 1 // i = i + 1, ++i and i++ don't work!
}
```
**Loop with while over a collection**
```scala
// Define counter variable
var i = 0

// Create an array with each player's hand
var hands = Array(17, 24, 21)

// Loop through hands and see if each busts 
while (i < hands.length) {
  println(bust(hands(i))) 
  i = i + 1
}
```
<img src="/assets/images/20210508_ScalaIntroduction/pic24.png" class="largepic"/>
Can't do it like in python
<img src="/assets/images/20210508_ScalaIntroduction/pic25.png" class="largepic"/>

**Practice**
```scala
// Define counter variable
var i = 0

// Define the number of loop iterations
val numRepetitions = 3

// Loop to print a message for winner of the round
while (i < numRepetitions) {
  if (i < 2)
    println("winner")
  else
    println("chicken dinner")
  // Increment the counter variable
  i = i + 1
}

winner
winner
chicken dinner

// Define counter variable
var i = 0

// Create list with five hands of Twenty-One
var hands = List(16, 21, 8, 25, 4)

// Loop through hands
while (i < hands.length) {
  // Find and print number of points to bust
  println(pointsToBust(hands(i)))
  // Increment the counter variable
  i += 1
}
```
## 3.3. foreach and the functional style


