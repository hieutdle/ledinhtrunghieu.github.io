---
layout: post
author: ledinhtrunghieu
title: Lesson 4 - Writing Efficient Python Code
---

# 1. Foundations for efficiencies

What it means to write efficient Python code? Explore Python's Standard Library, learn about NumPy arrays, and practice using some of Python's built-in tools. This chapter builds a foundation for the concepts covered ahead.

## 1.1. Introduction

**Defining efficient**
* Writign efficient Python code
    * Minimal completion time (fast runtime)
    * Minimal resource consumption (small memory footprint)

**Defining Pythonic**
* Writing efficient **Python** code
* Using Python's contructs as intended (i.e.,Python)

```python
# Non-Pythonic
doubled_numbers = []

for i in range(len(numbers)): 
    doubled_numbers.append(numbers[i] * 2)

# Pythonic
doubled_numbers = [x * 2 for x in numbers]
```

**The Zen of Python by Tim Peters**
```
Beautiful is better than ugly. 
Explicit is better than implicit. 
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense. 
Readability counts.
Special cases aren't special enough to break the rules. 
Although practicality beats purity.
Errors should never pass silently. 
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
```

**Example**:
```python
# Print the list created using the Non-Pythonic approach
i = 0
new_list= []
while i < len(names):
    if len(names[i]) >= 6:
        new_list.append(names[i])
    i += 1

# Print the list created by looping over the contents of names
better_list = []
for name in names:
    if len(name) >= 6:
        better_list.append(name)

# Print the list created by using list comprehension
best_list = [name for name in names if len(name) >= 6]
```

## 1.2. Building with built-ins

**Python standard library**
* Built-in types
    * `list`,`tuple`,`set`,`dict`, and others
* Built-in functions
    * `print()`,`len()`,`range()`,`round()`,`enumerate()`,`map()`,`zip()`, and others
* Built-in modules
    * `os`,`sys`,`itertools`,`collections`,`math`, and others

**Built-in fuction: range()**

Explicitly typing a list of numbers
```python
nums = [0,1,2,3,4,5,6,7,8,9,10]
```

Using `range()` to create the same list
```python
# range(start,stop) 
nums = range(0,11)	

nums_list = list(nums) 


# range(stop) 
nums = range(11)

nums_list = list(nums) 

```
The `range()` function returns a range object, which we can convert into a list and print.

Using `range()` with a step value

```python
even_nums = range(2, 11, 2)
even_nums_list = list(even_nums)
```
```
[2,4,6,8,10]
```
**Built-in function: enumarate()**

`Enumerate()` creates an index item pair for each item in the object provided
```python
letters = ['a', 'b', 'c', 'd' ]
indexed_letters = enumerate(letters)
indexed_letters_list = list(indexed_letters)
print(indexed_letters_list)
```

```
[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]
```

Similar to range, enumerate returns an enumerate object, which can also be converted into a list and printed.\

We can specify a start value
```python
letters = ['a', 'b', 'c', 'd' ]

indexed_letters2 = enumerate(letters, start=5)

indexed_letters2_list = list(indexed_letters2)
print(indexed_letters2_list)
```
```
[(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
```

**Built-in function: map()**
Applies a function over an object
```pyhon
nums = [1.5, 2.3, 3.4, 4.6, 5.0]

rnd_nums = map(round, nums)

print(list(rnd_nums))
```
```
[2, 2, 3, 5, 5]
```
`map()` with `lambda` (anonymous function)

```python
nums = [1, 2, 3, 4, 5]

sqrd_nums = map(lambda x: x ** 2, nums)

print(list(sqrd_nums))
```
```
[1, 4, 9, 16, 25]
```
**Example**
You can convert the range object into a list by using the list() function or by unpacking it into a list using the star character (*)
```python
# Create a new list of odd numbers from 1 to 11 by unpacking a range object
nums_list2 = [*range(1,12,2)]
print(nums_list2)
```

```python
# Rewrite the for loop to use enumerate
indexed_names = []
for i,name in enumerate(names):
    index_name = (i,name)
    indexed_names.append(index_name) 
print(indexed_names)

# Rewrite the above for loop using list comprehension
indexed_names_comp = [(i,name) for i,name in enumerate(names)]
print(indexed_names_comp)

# Unpack an enumerate object with a starting index of one
indexed_names_unpack = [*enumerate(names, 1)]
print(indexed_names_unpack)

# Easy rewrite
indexed_names = enumerate(names)
print(list(indexed_names))
```
```
[(0, 'Jerry'), (1, 'Kramer'), (2, 'Elaine'), (3, 'George'), (4, 'Newman')]
[(0, 'Jerry'), (1, 'Kramer'), (2, 'Elaine'), (3, 'George'), (4, 'Newman')]
[(1, 'Jerry'), (2, 'Kramer'), (3, 'Elaine'), (4, 'George'), (5, 'Newman')]
[(0, 'Jerry'), (1, 'Kramer'), (2, 'Elaine'), (3, 'George'), (4, 'Newman')]
```
Built-in practice: map()
```python
# Use map to apply str.upper to each element in names
names_map  = map(str.upper, names)

# Print the type of the names_map
print(type(names_map))

# Unpack names_map into a list
names_uppercase = [*names_map]

# Print the list created above
print(names_uppercase)
```

## 1.3. The power of NumPy arrays

**NumPy** or **Numerical Python**

* Alternative to Python lists
```python
nums_list = list(range(5))
```
```
[0, 1, 2, 3, 4]
```
```python
import numpy as np
nums_np = np.array(range(5))
```
```
array([0, 1, 2, 3, 4])
```

```python
# NumPy array homogeneity (must contain element of the same type)
nums_np_ints = np.array([1, 2, 3])

nums_np_ints.dtype

# NumPy will convert all elements to float to remain homogeneity nature
nums_np_floats = np.array([1, 2.5, 3]) 

nums_np_floats.dtype
```
```
array([1, 2, 3])
dtype('int64')
array([1. , 2.5, 3. ]) 
dtype('float64')
```

**NumPy array broadcasting**
* Python lists don't support broading castingn
```python
nums = [-2,-1,0,1,2]
numns ** 2
```
```
TypeError: unsupported operand type(s) for ** or pow(): 'list' and 'int'
```
* NumPy array can do this

```python
nums_np = np.array([-2, -1, 0, 1, 2])
nums_np ** 2
```
```
array([4, 1, 0, 1, 4])
```

**Indexing**
<img src="/assets/images/20210425_EfficientPython/pic1.png" class="largepic"/>

More clear with 2-D indexing

<img src="/assets/images/20210425_EfficientPython/pic2.png" class="largepic"/>

**NumPy arrary boolean indexing**

```python
nums = [-2, -1, 0, 1, 2]
nums_np = np.array(nums)
```
* Boolean indexing 
```python
num_np > 0
```
```
array([False, False, False,True,True])
```
```python
nums_np[nums_np > 0]
```
```
array([1, 2])
```
* No boolean indexing for lists
```python
# For loop (inefficient option) pos = []
for num in nums: 
    if num > 0:
    pos.append(num) 
print(pos)

# List comprehension (better option but not best)
pos = [num for num in nums if num > 0]
print(pos)
```

```
[1, 2]
[1, 2]
```
**Practice**
```python
# Print second row of nums
print(nums[1,:])

# Print all elements of nums that are greater than six
print(nums[nums > 6])

# Double every element of nums
nums_dbl = nums * 2
print(nums_dbl)

# Replace the third column of nums
nums[:,2] = nums[:,2] + 1
print(nums)
```

**Sum Practice**
```python
# Create a list of arrival times
arrival_times = [*range(10,60,10)]

# Convert arrival_times to an array and update the times
arrival_times_np = np.array(arrival_times)
new_times = arrival_times_np - 3

# Use list comprehension and enumerate to pair guests to new times
guest_arrivals = [(names[i],time) for i,time in enumerate(new_times)]

# Map the welcome_guest function to each (guest,time) pair
welcome_map = map(welcome_guest, guest_arrivals)

guest_welcomes = [*welcome_map]
print(*guest_welcomes, sep='\n')
```

# 2. Timing and profiling code
Learn how to gather and compare runtimes between different coding approaches. Practice using the line_profiler and memory_profiler packages to profile your code base and spot bottlenecks. Then, you'll put your learnings to practice by replacing these bottlenecks with efficient Python code.

# 2.1. Examining runtime

**Calculate time***
* Calculate runtime with IPython magic command `%timeit`
* **Magic commands**: enhancements on top of normal Python syntax 
    * Prefixed by the "%" character
    * Link to docs [here](https://ipython.readthedocs.io/en/stable/interactive/magics.html)
    * See all available magic commands with `%lsmagic`


**Using `%timeit`**
Code to be timed

```python
import numpy as np
rand_nums = np.random.rand(1000)
```

Timing with `%timeit`
```python
%timeit rand_nums = np.random.rand(1000)
```
<img src="/assets/images/20210425_EfficientPython/pic3.png" class="largepic"/>

**Specifying number of runs/loop
Setting the number of runs(`-r`) and/or loop (`-n`)

```python
# Set number of runs to 2 (-r2)
# Set number of loops to 10 (-n10)

%timeit -r2 -n10 rand_nums = np.random.rand(1000)
```

<img src="/assets/images/20210425_EfficientPython/pic4.png" class="largepic"/>

Line magic: %timeit

Cell magic: %%timeit

```python
%%timeit 
nums = []
for x in range(10):
nums.append(x)
```

**Saving output**
```python
times = %timeit -o rand_nums = np.random.rand(1000)
times.timings # print all runsn
times.best
times.worst
```

**Comparing times**

Python data structures can be created using formal name
```python
formal_list = list()
formal_dict = dict() 
formal_tuple = tuple()
```

Python data structures can be created using literal syntax
```python
literal_list = [] 
literal_dict = {} 
literal_tuple = ()
```

