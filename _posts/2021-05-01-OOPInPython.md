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

**Core principle of OOP**

**Inheritance**
* Extending functionality of existing code
**Polymorphism**
* Creating a unified interface
**Encapsulation**
* Bundling of data and methods

**Class-level data**
```python
class Employee:
    # Define a class attribute
    MIN_SALARY = 30000 #<--- no self.
    def __init__(self, name, salary): 
        self.name = name
        # Use class name to access class attribute 
        if	salary >= Employee.MIN_SALARY:
            self.salary = salary 
        else:
        self.salary = Employee.MIN_SALARY
```

* `MIN_SALARY` is shared among all instances 
* Don't use `self` to to define class attribute
* Use `ClassName.ATTR_NAME` to access the class attribute value

**Class-method**
* Methods are already "shared": same code for every instance 
* Class methods can't use instance-level data

```python
class MyClass:
    @classmethod	# <---use decorator to declare a class method 
    def my_awesome_method(cls, args...): # <---cls argument refers to the class
    # Do stuff here
    # Can't use any instance attributes :(

MyClass.my_awesome_method(args...)
```


**Why we need class-method?**

 The main use case is alternative constructors. A class can only have one init method, but there might be multiple ways to initialize an object. For example, we might want to create an Employee object from data stored in a file. We can't use a method, because it would require an instance, and there isn't one yet

* Use class methods to create objects
* Use `return` to return an object 
* `cls(...)` will call `__init__(...)`

```python

class Employee:
    # Define a class attribute
    MIN_SALARY = 30000 #<--- no self.
    def __init__(self, name, salary): 
        self.name = name
        # Use class name to access class attribute 
        if	salary >= Employee.MIN_SALARY:
            self.salary = salary 
        else:
        self.salary = Employee.MIN_SALARY


@classmethod
def from_file(cls,  filename): 
    with open(filename, "r") as f:
        name = f.readline() 
    return cls(name)


# Create an employee without calling Employee() 
emp = Employee.from_file("employee_data.txt") 
type(emp)

__main__.Employee
```

**Practice**
```python
class Player:
    MAX_POSITION = 10
    
    def __init__(self):
        self.position = 0

    # Add a move() method with steps parameter     
    def move(self, steps):
        if self.position + steps < Player.MAX_POSITION:
           self.position = self.position + steps 
        else:
           self.position = Player.MAX_POSITION
    
    # This method provides a rudimentary visualization in the console    
    def draw(self):
        drawing = "-" * self.position + "|" +"-"*(Player.MAX_POSITION - self.position)
        print(drawing)

p = Player(); p.draw()
p.move(4); p.draw()
p.move(5); p.draw()
p.move(3); p.draw()
```


**Classmethod is an alternative constructure, way to create an object**

```python
# import datetime from datetime
from datetime import datetime

class BetterDate:
    def __init__(self, year, month, day):
      self.year, self.month, self.day = year, month, day
      
    @classmethod
    def from_str(cls, datestr):
        year, month, day = map(int, datestr.split("-"))
        return cls(year, month, day)
      
    # Define a class method from_datetime accepting a datetime object
    @classmethod
    def from_datetime(cls, dateobj):
      year, month, day = dateobj.year, dateobj.month, dateobj.day
      return cls(year, month, day) 


# You should be able to run the code below with no errors: 
today = datetime.today()     
bd = BetterDate.from_datetime(today)   
print(bd.year)
print(bd.month)
print(bd.day)
```

## 2.2. Class inheritance

**Inheritance**
New class functionality = Old class functionality + extra

<img src="/assets/images/20210501_OOPInPython/pic3.png" class="largepic"/>

**Implementing class inheritance**
```python
class BankAccount:
    def __init__(self, balance): 
        self.balance = balance

    def withdraw(self, amount):
        self.balance -= amount


# Empty class inherited from BankAccount 

class SavingsAccount(BankAccount):
    pass

class MyChild(MyParent): 
    # Do stuff here
```

* `MyParent`: class whose functionality is being extended/inherited
* `MyChild`: class that will inherit the functionality and add more

**Child class has all of the the parent data**

```python
# Constructor inherited from BankAccount 
savings_acct = SavingsAccount(1000) 
type(savings_acct)

__main__.SavingsAccount

# Attribute inherited from BankAccount 
savings_acct.balance

1000

# Method inherited from BankAccount 
savings_acct.withdraw(300)
```

**Inheritance: "is-a" relationship**

A `SavingAccount` is a `BankAccount`

```python

savings_acct = SavingsAccount(1000) 
isinstance(savings_acct, SavingsAccount)

True

isinstance(savings_acct, BankAccount)

True

acct = BankAccount(500) 
isinstance(acct,SavingsAccount)

False

isinstance(acct,BankAccount)

True
```

```python
class Employee:
  MIN_SALARY = 30000    

  def __init__(self, name, salary=MIN_SALARY):
      self.name = name
      if salary >= Employee.MIN_SALARY:
        self.salary = salary
      else:
        self.salary = Employee.MIN_SALARY
        
  def give_raise(self, amount):
    self.salary += amount

        
# MODIFY Manager class and add a display method
class Manager(Employee):
  def display(self):
    print("Manager ", self.name)


mng = Manager("Debbie Lashko", 86500)
print(mng.name)

# Call mng.display()
mng.display()
```

## 2.3. Customizing functionality via inheritance

```python
class BankAccount:
    def __init__(self, balance): 
        self.balance = balance
    def withdraw(self, amount): 
        self.balance -=amount

# Empty class inherited from BankAccount 
class SavingsAccount(BankAccount):
    pass
```

**Customizing constructors**

```python
class SavingsAccount(BankAccount):
    # Constructor speficially for SavingsAccount with an additional parameter 
    def __init__(self, balance, interest_rate):
    # Call the parent constructor using ClassName.__init__()
    BankAccount.__init__(self,balance) # <--- self is a SavingsAccount but also a BankAccount
    # Add more functionality self.interest_rate = interest_rate
    self.interest_rate = interest_rate
```

* Can run constructor of the parent class first by `Parent.__init__(self, args...)`
* Add more functionality
* Don't have to call the parent constructors

**Create objects with a customized constructor**

```python
# Construct the object using the new constructor 
acct = SavingsAccount(1000, 0.03) 
acct.interest_rate

0.003
```

**Adding functionality**
* Add methods as usual
* Can use the data from both the parent and the child class

```python
class SavingsAccount(BankAccount):
    def __init__(self, balance, interest_rate): 
        BankAccount.__init__(self, balance)
        self.interest_rate = interest_rate
    
    # New functionality
    def compute_interest(self, n_periods = 1):
        return self.balance * ( (1 + self.interest_rate) ** n_periods - 1)
```

**Customizing functionality**

```python
class CheckingAccount(BankAccount): 
    def __init__(self, balance, limit):
        BankAccount.__init__(self, content) 
        self.limit = limit
    def deposit(self, amount): 
        self.balance += amount
    def withdraw(self, amount, fee=0): 
        if fee <= self.limit:
            BankAccount.withdraw(self, amount - fee)
        else:
            BankAccount.withdraw(self,amount - self.limit)
```

* Can change the signature (add parameters)
* Use `Parent.method(self, args...)` to call a method from the parent class

```python
check_acct = CheckingAccount(1000, 25)
# Will call withdraw from CheckingAccount
check_acct.withdraw(200)

# Will call withdraw from CheckingAccount 
check_acct.withdraw(200, fee=15)

bank_acct = BankAccount(1000)
# Will call withdraw from BankAccount
bank_acct.withdraw(200)

# Will produce an error 
bank_acct.withdraw(200, fee=15)

TypeError: withdraw() got an unexpected keyword argument 'fee'
```
**Practice**

```python
class Employee:
    def __init__(self, name, salary=30000):
        self.name = name
        self.salary = salary

    def give_raise(self, amount):
        self.salary += amount

        
class Manager(Employee):
    def display(self):
        print("Manager ", self.name)

    def __init__(self, name, salary=50000, project=None):
        Employee.__init__(self, name, salary)
        self.project = project

    # Add a give_raise method
    def give_raise(self,amount,bonus =1.05):
        Employee.give_raise(self, amount*bonus)

    
mngr = Manager("Ashta Dunbar", 78500)
mngr.give_raise(1000)
print(mngr.salary)
mngr.give_raise(2000, bonus=1.03)
print(mngr.salary)
```

```python
# Import pandas as pd
import pandas as pd

# Define LoggedDF inherited from pd.DataFrame and add the constructor
class LoggedDF(pd.DataFrame):
  
  def __init__(self, *args, **kwargs):
    pd.DataFrame.__init__(self, *args, **kwargs)
    self.created_at = datetime.today()
    
  def to_csv(self, *args, **kwargs):
    # Copy self to a temporary DataFrame
    temp = self.copy()
    
    # Create a new column filled with self.created_at
    temp["created_at"] = self.created_at
    
    # Call pd.DataFrame.to_csv on temp, passing in *args and **kwargs
    pd.DataFrame.to_csv(temp, *args, **kwargs)
```

# 3. Integrating with Standard Python

Learn how to make sure that objects that store the same data are considered equal, how to define and customize string representations of objects, and even how to create new error types. 

## 3.1. Operator overloading: comparison

