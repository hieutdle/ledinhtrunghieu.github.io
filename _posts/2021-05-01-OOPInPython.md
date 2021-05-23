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

**How to compare two classes**

**Overloading __eq__()**

```python
class Customer:
    def __init__(self, id, name): 
        self.id, self.name = id, name
    # Will be called when == is used 
    def __eq__(self, other):
        # Diagnostic printout
        print("__eq__() is called")

    # Returns True if all attributes match 
    return (self.id == other.id) and \
           (self.name == other.name)
```

* `__eq__()` is called when 2 objects of a class are compared using
* accepts 2 arguments, `self` and `other` objects to compare
* return a boolean


**Comparison of objects**

```python
# Two equal objects 

customer1 = Customer(123,"MaryamAzar")
customer2 = Customer(123,"MaryamAzar")

customer1 == customer2
```

**Other comparison operators**

<img src="/assets/images/20210501_OOPInPython/pic4.png" class="largepic"/>
* `__hash__()` to use objects as dictionary keys and in sets

**Practice**

```python
class BankAccount:
    def __init__(self, number, balance=0):
        self.number, self.balance = number, balance
      
    def withdraw(self, amount):
        self.balance -= amount 

    # MODIFY to add a check for the type()
    def __eq__(self, other):
        return (type(self) == type(other))

acct = BankAccount(873555333)
pn = Phone(873555333)
print(acct == pn)
```
**Comparison and inheritance**

```python
class Parent:
    def __eq__(self, other):
        print("Parent's __eq__() called")
        return True

class Child(Parent):
    def __eq__(self, other):
        print("Child's __eq__() called")
        return True

p = Parent()
c = Child()

p == c 

```
Child's __eq__() method will be called. Python always calls the child's __eq__() method when comparing a child object to a parent object.

## 3.2. Operator overloading: string representation

**Printing an object** 

```python
class Customer:
    def __init__(self, name, balance): 
        self.name, self.balance = name, balance

cust = Customer("Maryam Azar", 3000)
print(cust)
```
<img src="/assets/images/20210501_OOPInPython/pic5.png" class="largepic"/>

Two method to print: `__str__()` and `__repr__()`:

<img src="/assets/images/20210501_OOPInPython/pic6.png" class="largepic"/>


The difference is that `str` is supposed to give an informal representation, suitable for an end user, and `repr` is mainly used by developers. The best practice is to use `repr` to print a string that can be used to reproduce the object -- for example, with numpy array, this shows the exact method call that was used to create the object. If you only choose to implement one of them, chose `repr`, because it is also used as a fall-back for print when `str` is not defined.

**Implementation: str**

```python
class Customer:
    def __init__(self, name, balance): 
        self.name, self.balance = name, balance

    def __str__(self): 
        cust_str = """ 
        Customer:
            name: {name} 
            balance: {balance}
        """.format(name = self.name, \
                   balance = self.balance) 
        return cust_str

cust = Customer("Maryam Azar", 3000)
# Will implicitly call 	__str__()
print(cust)
```

**The triple quotes are used in Python to define multi-line strings, and the format method is used on strings to substitute values inside curly brackets with variables. If we create a customer object now and call print on that object, we will see a user-friendly output**

<img src="/assets/images/20210501_OOPInPython/pic7.png" class="largepic"/>

**Implementation: repr**


```python
class Customer:
    def __init__(self, name, balance): 
        self.name, self.balance = name, balance

    def __repr__(self): 
        # Notice the '...' around name
        return "Customer('{name}', {balance})".format(name = self.name, balance = self.balance) 

cust = Customer("Maryam Azar", 3000)
cust # <--- # Will implicitly call 	__repr__()

```
Notice the single quotes around the name in the return statement. Without the quotes, the name of the customer would be substituted into the string as-is, but the point of repr is to give the exact call needed to reproduce the the object, where the name should be in quotes. Notice also that we can use single quotes inside double quotes and vice versa.

<img src="/assets/images/20210501_OOPInPython/pic8.png" class="largepic"/>

**Practice**
```python
my_num = 5
my_str = "Hello"

f = ...
print(f)

f = "my_num is {}, and my_str is \"{}\".".format(my_num, my_str)

my_num is 5, and my_str is "Hello".
```

**Practice**

```python
class Employee:
    def __init__(self, name, salary=30000):
        self.name, self.salary = name, salary
            
    # Add the __str__() method
    def __str__(self): 
        cust_str = """ 
            Employee name:: {name} 
            Employee salary: {salary}
        """.format(name = self.name, \
                   salary = self.salary) 
        return cust_str

emp1 = Employee("Amar Howard", 30000)
print(emp1)
emp2 = Employee("Carolyn Ramirez", 35000)
print(emp2)
```
```python
class Employee:
    def __init__(self, name, salary=30000):
        self.name, self.salary = name, salary
      

    def __str__(self):
        s = "Employee name: {name}\nEmployee salary: {salary}".format(name=self.name, salary=self.salary)      
        return s
      
    # Add the __repr__method  
    def __repr__(self): 
        
        return "Employee('{name}', {salary})".format(name = self.name, salary = self.salary) 

emp1 = Employee("Amar Howard", 30000)
print(repr(emp1))
emp2 = Employee("Carolyn Ramirez", 35000)
print(repr(emp2))
```

## 3.3. Exceptions

**Exception handling**
* Prevent the program from terminating when an exception is raised
* `try`-`except`-`finally`

```python
try:
    # Try running some code 
except ExceptionNameHere:
    # Run this code if ExceptionNameHere happens
except AnotherExceptionHere: #<-- multiple except blocks
    # Run this code if AnotherExceptionHere happens
...
finally: #<-- optional
    # Run this code no matter what
```

**Raising Exception**
* raise ExceptionNameHere('Error message here')
```python
def make_list_of_ones(length): 
    if length <= 0:
    raise ValueError("Invalid length!") # <--- Will stop the program and raise an error
return [1]*length

make_list_of_ones(-1)
```
<img src="/assets/images/20210501_OOPInPython/pic9.png" class="largepic"/>

**Exception are classes**
* standard exceptions are inherited from `BaseException` or `Exception`

```
BaseException
+-- Exception	
    +-- ArithmeticError	#	<---
    |	    +-- FloatingPointError		
    |	    +-- OverflowError		
    |	    +-- ZeroDivisionError	#	<---
    +-- TypeError
    +-- ValueError
    |	+-- UnicodeError
    |	    +-- UnicodeDecodeError
    |	    +-- UnicodeEncodeError
    |	    +-- UnicodeTranslateError
    +-- RuntimeError
    ...
+-- SystemExit
...
```

**Custom exceptions**
* Inherit from `Exception` or one of its subclasses
* Usually an empty class

```python
class BalanceError(Exception): pass

class Customer:
    def __init__(self, name, balance): 
        if balance < 0 :
            raise BalanceError("Balance has to be non-negative!") 
        else:
            self.name, self.balance = name, balance

cust = Customer("Larry Torres",	-100)
```
<img src="/assets/images/20210501_OOPInPython/pic10.png" class="largepic"/>

* Exception interrupted the constructor → object not created
```
cust
```
<img src="/assets/images/20210501_OOPInPython/pic11.png" class="largepic"/>

**Catching custom exceptions**
```python
try:
    cust = Customer("Larry Torres", -100) 
except BalanceError:
    cust = Customer("Larry Torres", 0)
```

**Practice**
```python
class SalaryError(ValueError): pass
class BonusError(SalaryError): pass

class Employee:
  MIN_SALARY = 30000
  MAX_BONUS = 5000

  def __init__(self, name, salary = 30000):
    self.name = name    
    if salary < Employee.MIN_SALARY:
      raise SalaryError("Salary is too low!")      
    self.salary = salary
    
  # Rewrite using exceptions  
  def give_bonus(self, amount):
    if amount > Employee.MAX_BONUS:
       raise BonusError("The bonus amount is too high!")  
        
    elif self.salary + amount <  Employee.MIN_SALARY:
       raise SalaryError("The salary after bonus is too low!")
      
    else:  
      self.salary += amount
```

It's better to include an `except` block for a child exception before the block for a parent exception, otherwise the child exceptions will be always be caught in the parent block, and the `except` block for the child will never be executed.

# 4. Best Practices of Class Design

## 4.1. Designing for inheritance and polymorphism

**Polymorphism: Using a unified interface to operate on objects of different classes**

<img src="/assets/images/20210501_OOPInPython/pic12.png" class="largepic"/>

**All that matter is the interface**

```python
# Withdraw amount from each of accounts in list_of_accounts 
def batch_withdraw(list_of_accounts, amount):
    for acct in list_of_accounts: 
        acct.withdraw(amount)

b, c, s = BankAccount(1000), CheckingAccount(2000), SavingsAccount(3000) 
batch_withdraw([b,c,s]) # <-- Will use BankAccount.withdraw(),
                            #	then CheckingAccount.withdraw(),
                            #	then SavingsAccount.withdraw()

```

* `batch_withdraw()` doesn't need to check the object to know which `withdraw()` to call. This function doesn't know -- or care -- whether the objects passed to it are checking accounts, savings accounts or a mix -- all that matters is that they have a withdraw method that accepts one argument. That is enough to make the function work.
* When the withdraw method is actually called, Python will dynamically pull the correct method: modified withdraw for whenever a checking account is being processed,and base withdraw for whenever a savings or generic bank account is processed. 
* As a person writing this batch processing function,you don't need to worry about what exactly is being passed to it, only what kind of interface it has. To really make use of this idea, you have to design your classes with inheritance and polymorphism - the uniformity of interface - in mind

**Liskov substitution principle**
* **Base class should be interchangeable with any of its subclasses without altering any properties of the program**
* Wherever `Bank Account`works, `CheckingAccount` should work as well. For example, the batch withdraw function worked regardless of what kind of account was used.
* Syntactically
    * the method in a subclass should have a signature with parameters 
    * returned values compatible with the method in the parent class.
* Semantically
    * the state of the object and the program must remains consistent
        * the subclass method shouldn't rely on stronger input conditions
        * subclass method should not provide weaker output conditions
        * should not throw additional exceptions.


**Violating LSP**
* Syntactic incompatibility: `BankAccount.withdraw()` requires  1 parameter, but `CheckingAccount.withdraw()` requires 2
* Subclass strengthening input conditions:  `BankAccount.withdraw()`accepts any amount, but `CheckingAccount.withdraw()` assumes that the amount is limited
* Subclass weakening output conditions: `BankAccount.withdraw()` can only leave a positive balance or cause an error, `CheckingAccount.withdraw()` can leave balance negative
* Changing additional attributes in subclass's method
* Throwing additional exceptions in subclass's method

## 4.2. Managing data access: private attributes

**All class data is public**
*All class data in Python is technically public. Any attribute or method of any class can be accessed by anyone. If you are coming from a background in another programming language like Java, this might seem unusual or an oversight, but it is by design. The fundamental principle behind much of Python design "we are all adults here". It is a philosophy that goes beyond just code, and describes how the Python community interacts with each other: you should have trust in your fellow developers.*

**Restricting access**
* Naming conventions
* Use `@property` to customize access
* Overriding `__getattr__()` and `__setattr__()`

**Naming convention: internal attributes**
`obj._att_name`,`obj._method_name()` : The first and most important convention is using a single leading underscore to indicate an attribute or method that isn't a part of the public class interface, and can change without notice. 
* Starts with a single _ → "internal" 
* Not a part of the public API
* As a class user: "don't touch this"
* As a class developer: use for implementation details, helper functions..
`df._is_mixed_type`:  that indicates whether the DataFrame contains data of mixed types, `datetime._ymd2ord()`: converts a date into a number containing how many days have passed since January 1st of year 1.

Nothing is technically preventing you from using these attributes, but a single leading underscore is the developer's way of saying that you shouldn't. The class developer trusts that you are an adult and will be able to use the class responsibly.

**Naming convention: pseudoprivate attributes**
`obj.__attr_name`,`obj.__method_name()`
* Attributes and methods whose names start with a double underscore are the closest thing Python has to "private" fields and methods of other programming languages. 
* It means that this data is not inherited - at least, not in a way you're used to, because Python implements name mangling: any name starting with a double underscore will be automatically prepended by the name of the class when interpreted by Python, and that new name will be the actual internal name of the attribute or method: `obj.__attr_name` is is interpreted as `obj.MyClass_attr_name`
* The main use of these pseudo-private attributes is to prevent name clashes in child classes: you can't control what attributes or methods someone will introduce when inheriting from your class, and it's possible that someone will unknowingly introduce a name that already exists in you class, thus overriding the parent method or attribute! You can use double leading underscores to protect important attributes and methods that should not be overridden. 
* Finally, be careful: leading AND trailing double underscores are only used for build-in Python methods (`__init__()`,`__repr__()`), so your name should only start -- but not end! -- with double underscores.

<img src="/assets/images/20210501_OOPInPython/pic13.png" class="largepic"/>

```python
# MODIFY to add class attributes for max number of days and months
class BetterDate:
    _MAX_DAYS = 30
    _MAX_MONTHS = 12
    
    def __init__(self, year, month, day):
      self.year, self.month, self.day = year, month, day
      
    @classmethod
    def from_str(cls, datestr):
        year, month, day = map(int, datestr.split("-"))
        return cls(year, month, day)
    
    # Add _is_valid() checking day and month values
    def _is_valid(self):
        return (self.day <= BetterDate._MAX_DAYS) and \
               (self.month <= BetterDate._MAX_MONTHS)
         
bd1 = BetterDate(2020, 4, 30)
print(bd1._is_valid())

bd2 = BetterDate(2020, 6, 45)
print(bd2._is_valid())

True
False
```

## 4.3. Properties

**Properties: a special kind of attribute that allow customized access**

```python
class Employee:
    def set_name(self, name): 
        self.name = name
    def set_salary(self, salary): 
        self.salary = salary
    def give_raise(self, amount): 
        self.salary = self.salary + amount
    def __init__(self, name, salary): 
        self.name, self.salary = name, salary

emp = Employee("Miriam Azari", 35000)
# Use dot syntax and = to alter atributes 
emp.salary = emp.salary + 5000
```

This means that with a simple equality we can assign anything to salary: a million, a negative number, or even the word "Hello". Salary is an important attribute, and that should not be allowed.

**How can we control attributes access?**
* Check the value for validity
* or make attributes read-only
* modifyng `set_salary()` wouldn't help because we can still do `emp.salary = -100`

**Restricted and read-only attributes example**
<img src="/assets/images/20210501_OOPInPython/pic14.png" class="largepic"/>

**@property**

```python
class Employer:
    def __init__(self, name, new_salary):  # Use "protected" attribute with leading	__ to store data ( _salary)
        self._salary = new_salary

    @property
    def salary(self):  # Use @property on a method whose name is exactly the name of the restricted attribute (salary with out underscore); return the internal attribute
        return self._salary

    @salary.setter
    def salary(self, new_salary):  # Use decorator: @attr.setter on a method attr(). It will be called when a value is assigned to the property attribute
        if new_salary < 0:
            raise ValueError("Invalid salary")
        self._salary = new_salary #  the value to assign passed as argument

emp = Employee("Miriam Azari", 35000) 
# accessing the "property" 
emp.salary

35000

emp.salary = 60000 # <-- @salary.setter

emp.salary = -1000

ValueError: Invalid salary
```
There are two methods called salary -- the name of the property -- that have different decorators. The method with property decorator returns the data, and the method with salary dot setter decorator implements validation and sets the attribute.

We can use this property just as if it was a regular attribute (remember the only real attribute we have is the internal underscore-salary). Use the dot syntax and equality sign to assign a value to the salary property. 

**Why use @property?**
* User-facing: behave like attributes : the user of your class will not have to do anything special. They won't even be able to distinguish between properties and regular attributes. 
* Developer-facing: give control of access

**Other possibilities**
* If you do not define a `setter` method, the property will be read-only, like Dataframe shape. 
* Add `@attr.getter`: Use for the method that is called when the property's value is retrieved
* Add `@attr.deletr`: Use for the method that is called when the property is deleted using `del`

**Practice**
```python
class Customer:
    def __init__(self, name, new_bal):
        self.name = name
        if new_bal < 0:
           raise ValueError("Invalid balance!")
        self._balance = new_bal  

    # Add a decorated balance() method returning _balance        
    @property
    def balance(self):
        return self._balance

    # Add a setter balance() method
    @balance.setter
    def balance(self, new_bal):
        # Validate the parameter value
        if new_bal < 0:
           raise ValueError("Invalid balance!")
        self._balance = new_bal
        print("Setter method called")

# Create a Customer        
cust = Customer("Belinda Lutz",2000)

# Assign 3000 to the balance property
cust.balance = 3000

# Print the balance property
print(cust.balance)
```

**Read-only properties**

```python
import pandas as pd
from datetime import datetime

# MODIFY the class to use _created_at instead of created_at
class LoggedDF(pd.DataFrame):
    def __init__(self, *args, **kwargs):
        pd.DataFrame.__init__(self, *args, **kwargs)
        self._created_at = datetime.today()
    
    def to_csv(self, *args, **kwargs):
        temp = self.copy()
        temp["created_at"] = self._created_at
        pd.DataFrame.to_csv(temp, *args, **kwargs)   
    
    # Add a read-only property: _created_at
    @property  
    def created_at(self):
        return self._created_at

# Instantiate a LoggedDF called ldf
ldf = LoggedDF({"col1": [1,2], "col2":[3,4]}) 
```
# 5. Reference

1. [Object-Oriented Programming in Python- DataCamp](https://learn.datacamp.com/courses/object-oriented-programming-in-python)

