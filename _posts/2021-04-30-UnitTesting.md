---
layout: post
author: ledinhtrunghieu
title: Lesson 9 - Unit Testing For Data Engineering in Python
---


# 1. Unit testing basics

In this chapter, you will get introduced to the pytest package and use it to write simple unit tests. You'll run the tests, interpret the test result reports and fix bugs. Throughout the chapter, I will use examples exclusively from the data preprocessing module of a linear regression project.

## 1.1. Why unit test?

**How can we test an implementation**

The easiest way is to open an interpreter, test the function on a few arguments and check whether the return value is correct. 

```python
my_function(argument_1)
my_function(argument_2)
my_function(argument_3)
```
**Life cycle of a function**

<img src="/assets/images/20210430_UnitTesting/pic1.png" class="largepic"/>

**Manual testing vs unit tests**

Unit tests automate the repetitive testing process and saves times.

## 1.2. Write a simple unit test using pytest

**Python unit testing libraries**
* pytest 
* unittest 
* nosetests
* doctest

We will use pytest!

* Has all essential features. 
* Easiest to use.
* Most popular

<img src="/assets/images/20210430_UnitTesting/pic2.png" class="largepic"/>


**Step 1: Create a file**
* Create `test_row_to_list.py`
* `test_` indicate unit tests inside (naming convention).
* Also called **test modules**.

**Step 2: Imports**
Test module: `test_row_to_list.py`
```python
import pytest 
import row_to_list
```

**Step 3: Unit tests are Python functions**

Test module: `test_row_to_list.py`
```python
import pytest 
import row_to_list

def test_for_clean_row():
```

**Step 4: Assertion**

Test module: `test_row_to_list.py`
```python
import pytest 
import row_to_list

def test_for_clean_row():
    assert ...
```

**Theoretical structure of an assertion**
```python
assert boolean_expression
```
```python
assert True
```
<img src="/assets/images/20210430_UnitTesting/pic3.png" class="largepic"/>

```python
assert False
```
<img src="/assets/images/20210430_UnitTesting/pic4.png" class="largepic"/>

**Checking for None values**

Do this for checking if `var` is `None`.
```python
assert var is None
```
Do not do this
```python
assert var == None
```
**Testing**
```python
import pytest 
import row_to_list

def test_for_clean_row():
    assert row_to_list("2,081\t314,942\n") == \ ["2,081", "314,942"]

def test_for_missing_area():
    assert row_to_list("\t293,410\n") is None
def test_for_missing_tab():
    assert row_to_list("1,463238,765\n") is None

```

**Step 5: Running unit tests**
* Do this in the command line
```
!pytest test_row_to_list.py
```

```python
# Import the pytest package
import pytest

# Import the function convert_to_int()
from preprocessing_helpers import convert_to_int

# Complete the unit test name by adding a prefix
def test_on_string_with_one_comma():
  # Complete the assert statement
  assert convert_to_int("2,081") == 2081
```

## 1.3. Understanding test result report

<img src="/assets/images/20210430_UnitTesting/pic5.png" class="largepic"/>

```python
import pytest 
import row_to_list

def test_for_clean_row():
    assert row_to_list("2,081\t314,942\n") == \ ["2,081", "314,942"]

def test_for_missing_area():
    assert row_to_list("\t293,410\n") is None
def test_for_missing_tab():
    assert row_to_list("1,463238,765\n") is None

```

**Test result report**
```python
!pytest test_row_to_list.py
```

<img src="/assets/images/20210430_UnitTesting/pic7.png" class="largepic"/>


**Section 1: general information**

<img src="/assets/images/20210430_UnitTesting/pic6.png" class="largepic"/>

The first section provides information about the operating system, Python version, pytest package versions, the working directory and pytest plugins.

**Section 2: Test result**

<img src="/assets/images/20210430_UnitTesting/pic8.png" class="largepic"/>

* The output says "collected 3 items", which means that pytest found three tests to run
* The next line contains the test module name, which is test_row_to_list.py, followed by the characters dot, capital F and dot. Each character represents the result of a unit test.
* The character capital F stands for failure. A unit test fails if an exception is raised when running the unit test code. 
<img src="/assets/images/20210430_UnitTesting/pic9.png" class="largepic"/>

* assertion raises `AssertionError`
```python
def test_for_missing_area():
    assert row_to_list("\t293,410") is None # AssertionError from this line
```

* another exception
```python
def test_for_missing_area():
    assert row_to_list("\t293,410") is none # NameError from this line
```

**Section 3: Information on failed tests**

<img src="/assets/images/20210430_UnitTesting/pic10.png" class="largepic"/>

* The line raising the exception is marked by `>`.

<img src="/assets/images/20210430_UnitTesting/pic11.png" class="largepic"/>

* The exception is an `AssertionError`.

<img src="/assets/images/20210430_UnitTesting/pic12.png" class="largepic"/>

* The line containing `where` displays return values.

<img src="/assets/images/20210430_UnitTesting/pic13.png" class="largepic"/>

**Section 4: Test result summary**

<img src="/assets/images/20210430_UnitTesting/pic14.png" class="largepic"/>

* Result summary from all unit tests that ran: 1 failed, 2 passed tests.
* Total time for running tests: 0.03 seconds.
    * Much faster than testing on the interpreter!

```python
    def test_on_string_with_one_comma():
>     assert convert_to_int("2,081") == 2081
E     AssertionError: assert '2081' == 2081
E      +  where '2081' = convert_to_int('2,081')
```

convert_to_int("2,081") is expected to return the integer 2081, but it is actually returning the string "2081".

Fix:
```python
def convert_to_int(string_with_comma):
    return int(string_with_comma.replace(",", ""))
```

## 1.4. More benefits and test types


**Guess function's purpose by reading unit tests**

```
!cat test_row_to_list.py
```

**More trust**
* Users can run tests and verify that the package works.

**Reduced downtime**

<img src="/assets/images/20210430_UnitTesting/pic15.png" class="largepic"/>

Suppose we make a mistake and push bad code to a productive system. This will bring the system down and annoy users. We can cure this by setting up **Continuous Integration** or **CI**. CI runs all unit tests when any code is pushed, and if any unit test fails, it rejects the change, preventing downtime.


<img src="/assets/images/20210430_UnitTesting/pic16.png" class="largepic"/>

It also informs us that the code needs to be fixed. If we run productive systems that many people depend upon, we must write unit tests and setup CI.

**All benefits**
* Time savings.
* Improved documentation. 
* More trust.
* Reduced downtime.

**Data module**

<img src="/assets/images/20210430_UnitTesting/pic17.png" class="largepic"/>

They are part of the data module, which creates a clean data file from raw data on housing area and price. We will see functions from the feature module, which compute features from the clean data. Then we will meet the models module, which will output a model for predicting housing price from the features.

The tests that we wrote are called **unit tests** because they test a **unit**, such as `row_to_list()`.

**What is a unit**
* Small, independent piece of code. 
* Python function or class

**Intergration test**

<img src="/assets/images/20210430_UnitTesting/pic18.png" class="largepic"/>

Integration tests check if multiple units work well together when they are connected, and not just independently. For example, we could check if the data and the feature module work well when connected. Here, the argument will be the raw data, and the return values to check would be the features.

**End to end test**

<img src="/assets/images/20210430_UnitTesting/pic19.png" class="largepic"/>


End to end tests check the whole software at once.. They start from one end, which is the unprocessed data file, goes through all the units till the other end, and checks whether we get the correct model.

# 2. Intermediate unit testing

Write more advanced unit tests. Starting from testing complicated data types like NumPy arrays to testing exception handling. Learn how to find the balance between writing too many tests and too few tests. Get introduced to a radically new programming methodology called **Test Driven Development (TDD)** and put it to practice. 
