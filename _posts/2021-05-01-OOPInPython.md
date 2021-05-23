---
layout: post
author: ledinhtrunghieu
title: Lesson 10 - Object-Oriented Programming in Python
---

# 1. OOP Fundamentals

What object-oriented programming (OOP) is, how it differs from procedural-programming, and how it can be applied. You'll then define your own classes, and learn how to create methods, attributes, and constructors.

## 1.1. What is OOP?

**Produceral Programming**
* Code as sequence of steps
* Greate for data analysis

**Thinking in sequences**

<img src="/assets/images/20210501_OOPInPython/pic1.png" class="largepic"/>

Procedural thinking is natural. You get up, have breakfast, go to work. But if you are a city planner, you have to think about thousands of people with their own routines. Trying to map out a sequence of actions for each individual would quickly get unsustainable. Instead, you are likely to start thinking about patterns of behaviors.

**Object-oriented programming**
* Code as interactions of objects
* Great for building frameworks and tools
* Maintainable and reusable code

**Object as data structure**
Object = state + behavior
**Class as blueprint**
Class = blueprint for objects outlining possible states and behaviors

<img src="/assets/images/20210501_OOPInPython/pic2.png" class="largepic"/>

**Object in Python**
* Everything in Python is an object
* Every object has a class
* Use `type()` to find the class

```python
import numpy as np
a = np.array([1,2,3,4]) 
print(type(a))

numpy.ndarray
```

**Attributes and methods**

State <-> Attributes
```python
import numpy as np
a = np.array([1,2,3,4]) 
# shape attribute 
a.shape

(4,)
```

Behavior <-> methods
```python
import numpy as np
a = np.array([1,2,3,4]) 
# reshape method
 a.reshape(2,2)

array([[1, 2],
       [3, 4]])
```

Object = attributes + methods
* attributes <-> variables <-> `obj.my_attribute`
* method <-> function() <-> `obj.my_method()`

```python
import numpy as	np
a = np.array([1,2,3,4])
dir(a) # <--- list all attributes and methods
```

```
['T',
' 	abs 	',
...
'trace', 'transpose', 'var',
'view']
```

## 1.2. Class anatomy: attributes and methods

**A basic class**

```python
class Customer:
    # code for class goes here pass
    pass

c1 = Customer() 
c2 = Customer()

```

* Use `pass` to create an "empty" class

**Add methods to a class**

```python
class Customer:
    def identify(self, name):
        print("I am Customer " + name)

cust = Customer() 
cust.identify("Laura")
```

* Use `self` as the 1st argument in method
* ignore `self` when calling method on an object

**What is self**
* classes are templates, how to refer data of a particular object? 
* `self` is a stand-in for a particular object used in class definition
* should be the first argument of any method
* Python will take care of `self` when method called from an object:
`cust.identify("Laura")` will be interpreted as `Customer.identify(cust, "Laura")`

**Add an attributes to class**
```python
class Customer:
    # set the name attribute of an object to new_name 
    def set_name(self, new_name):
        # Create an attribute by assigning a value
        self.name = new_name

cust = Customer() 
cust.set_name("Lara de Silva")
print(cust.name)
```
**Practice**

```python
class Employee:
    def set_name(self, new_name):
        self.name = new_name

    def set_salary(self, new_salary):
        self.salary = new_salary 

    def give_raise(self, amount):
        self.salary = self.salary + amount

    # Add monthly_salary method that returns 1/12th of salary attribute
    def monthly_salary(self):
        return self.salary / 12
    
emp = Employee()
emp.set_name('Korel Rossi')
emp.set_salary(50000)

# Get monthly salary of emp and assign to mon_sal
mon_sal = emp.monthly_salary()

# Print mon_sal
print(mon_sal)
```

## 1.3. Class anatomy: the __init__ constructor

**Constructor**
* Add a data to object when creating it
* **Constructor** `__init__()`method is called every time an object is created.

```python
class Customer:
    def __init__ (self, name, balance):	# <-- balance parameter added 
    self.name = name
    self.balance = balance	# <-- balance attribute added
    print("The __init__ method was called")
    
cust = Customer("Lara de Silva", 1000)	# <-- 	init 	
print(cust.name) 
print(cust.balance)

```
```python
class Customer:
    def __init__ (self, name, balance=0):	#<--set default value for balance 
    self.name = name
    self.balance = balance	# <-- balance attribute added
    print("The __init__ method was called")
    
cust = Customer("Lara de Silva")	# <-- don't specify balance explicitly 
print(cust.name) 
print(cust.balance)
```

**Best practice**
1. Initialize attributes in `__init__()`
2. Naming: `CamelCase` for classes, `lower_snake_case` for functions and attributes
3. Keep `self` as `self`
```python
class MyClass:
    # This works but isn't recommended 
    def my_method(kitty, attr):
    kitty.attr = attr
```
4. Use docstrings
```python
class MyClass:
    """This class does nothing""" 
    pass
```

**Practice**
```python
# Import datetime from datetime
from datetime import datetime 

class Employee:
    
    def __init__(self, name, salary=0):
        self.name = name
        if salary > 0:
          self.salary = salary
        else:
          self.salary = 0
          print("Invalid salary!")
          
        # Add the hire_date attribute and set it to today's date
        self.hire_date = datetime.today()
        
   # ...Other methods omitted for brevity ...
      
emp = Employee("Korel Rossi", -1000)
print(emp.name)
print(emp.salary)
```

```python
import math
# Write the class Point as outlined in the instructions
class Point:
    def __init__(self,x=0,y=0):
        self.x = x
        self.y = y
    def distance_to_origin(self):
        return math.sqrt(pow(self.x,2)+pow(self.y,2))
    def reflect(self,axis):
        if axis =='x':
            self.y = -self.y
        elif axis == 'y':
            self.x = -self.x
        else:
            print("Error")
```
# 2. Inheritance and Polymorphism

## 2.1. Instance and class data

