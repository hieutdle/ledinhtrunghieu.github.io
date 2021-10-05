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
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
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
# Create a range object that goes from 0 to 5
nums = range(0,6)
print(type(nums))

# Convert nums to a list
nums_list = list(nums)
print(nums_list)

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

```
[10, 20, 30, 40, 50]

[ 7 17 27 37 47]

[('Jerry', 7), ('Kramer', 17), ('Elaine', 27), ('George', 37), ('Newman', 47)]

Welcome to Festivus Jerry... You're 7 min late.
Welcome to Festivus Kramer... You're 17 min late.
Welcome to Festivus Elaine... You're 27 min late.
Welcome to Festivus George... You're 37 min late.
Welcome to Festivus Newman... You're 47 min late.
```
# 2. Timing and profiling code
Learn how to gather and compare runtimes between different coding approaches. Practice using the line_profiler and memory_profiler packages to profile your code base and spot bottlenecks. Then, you'll put your learnings to practice by replacing these bottlenecks with efficient Python code.

## 2.1. Examining runtime

**Calculate time***
* Calculate runtime with IPython magic command `%timeit`
* **Magic commands**: enhancements on top of normal Python syntax 
    * Prefixed by the "%" character
    * Link to docs [here](https://ipython.readthedocs.io/en/stable/interactive/magics.html)
    * See all available magic commands with `%lsmagic`

<img src="/assets/images/20210425_EfficientPython/pic27.png" class="largepic"/>

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

```python
f_time = %timeit -o formal_dict = dict()
```
```
145 ns ± 1.5 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```
```python
l_time = %timeit -o literal_dict = {}
```
```
93.3 ns ± 1.88 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```
```python
diff = (f_time.average - l_time.average) * (10**9) 
print('l_time better than f_time by {} ns'.format(diff))
```
```
l_time better than f_time by 51.90819192857814 ns
```

**Practice**:
```python
# Create a list of integers (0-50) using list comprehension
nums_list_comp = [num for num in range(51)]
print(nums_list_comp)

# Create a list of integers (0-50) by unpacking range
nums_unpack = [*range(50)]
print(nums_unpack)
```

```
%timeit nums_list_comp = [num for num in range(51)]
2.09 us +- 124 ns per loop (mean +- std. dev. of 7 runs, 1000000 loops each)
%timeit nums_list_comp = nums_unpack = [*range(50)]
390 ns +- 4.14 ns per loop (mean +- std. dev. of 7 runs, 1000000 loops each)
```
**What is the correct syntax when using %timeit and only using 5 runs with 25 loops per each run?**
```python
# A list of 480 superheroes has been loaded into your session (called heroes). You'd like to analyze the runtime for converting this heroes list into a set. Instead of relying on the default settings for %timeit, you'd like to only use 5 runs and 25 loops per each run.



```

```python
# Create a list using the formal name
formal_list = list()
print(formal_list)

# Create a list using the literal syntax
literal_list = []
print(literal_list)

# Print out the type of formal_list
print(type(formal_list))

# Print out the type of literal_list
print(type(literal_list))

# Using the literal syntax ([]) to create a list is faster.
```

```
[]
[]
<class 'list'>
<class 'list'>
```





```python

In [2]:

%%timeit hero_wts_lbs = []
for wt in wts:
    hero_wts_lbs.append(wt * 2.20462)

746 us +- 9.66 us per loop (mean +- std. dev. of 7 runs, 1000 loops each)

In [3]:

%%timeit wts_np = np.array(wts)
hero_wts_lbs_np = wts_np * 2.20462

948 ns +- 51.5 ns per loop (mean +- std. dev. of 7 runs, 1000000 loops each)


The numpy technique was faster.
```

## 2.2. Code profiling for runtime

**Code profiling**
* Detailed stats on frequency and duration of functions call
* Line-by-line analyses
* Package used: `line_profiler`

```python
pip install linen_profiler
```

**Code profiling: runtime**
```python
heroes = ['Batman', 'Superman', 'Wonder Woman']
hts = np.array([188.0, 191.0, 183.0])
wts = np.array([ 95.0, 101.0,74.0])
```

```python
def convert_units(heroes, heights, weights):
    new_hts = [ht * 0.39370 for ht in heights]
    new_wts = [wt * 2.20462 for wt in weights]

    hero_data = {}

    for i,hero in enumerate(heroes):
        hero_data[hero] = (new_hts[i], new_wts[i])

    return hero_data
```

Suppose you have a list of superheroes (named heroes) along with each hero's height (in centimeters) and weight (in kilograms) loaded as NumPy arrays (named hts and wts respectively).
**What are the necessary steps you need to take in order to profile the convert_units() function acting on your superheroes data if you'd like to see line-by-line runtimes?**
* Use `%load_ext line_profiler` to load the `line_profiler` within your IPython session.
* Use `%lprun -f convert_units convert_units(heroes, hts, wts)` to get line-by-line runtimes

<img src="/assets/images/20210425_EfficientPython/pic28.png" class="largepic"/>



```python
convert_units(heroes, hts, wts)
```
```
{'Batman': (74.0156, 209.4389),
'Superman': (75.1967, 222.6666),
'Wonder Woman': (72.0471, 163.1419)}
```
```
%timeit convert_units(heroes,hts,wts)
```
```
3 µs ± 32 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```
<img src="/assets/images/20210425_EfficientPython/pic5.png" class="largepic"/>

A lot of manual work and not very efficientn

**Code profiling: line_profiler**
* Using `line_profiler` package

```
%load_ext line_profiler
```
Magic command for line-by-line times
```
%lprun -f convert_units convert_units(heroes,hts,wts)
```

<img src="/assets/images/20210425_EfficientPython/pic6.png" class="largepic"/>

* First, We use the -f flag to indicate we'd like to profile a function.
* Next, we specify the name of the function we'd like to profile. Note, the name of the function is passed without any parentheses.
* Finally, we provide the exact function call we'd like to profile by including any arguments that are needed.

The output from %lprun provides a nice table that summarizes the profiling statistics.
1. First, a column specifying the line number followed by a column displaying the number of times that line was executed (called the Hits column).
2. Next, the Time column shows the total amount of time each line took to execute. This column uses a specific timer unit that can be found in the first line of the output. Here, the timer unit is listed in microseconds using scientific notation.
3. The Per Hit column gives the average amount of time spent executing a single line. This is calculated by dividing the Time column by the Hits column.
4. The % Time column shows the percentage of time spent on a line relative to the total amount of time spent in the function. This can be a nice way to see which lines of code are taking up the most time within a function.
5. Finally, the source code is displayed for each line in the Line Contents column.

<img src="/assets/images/20210425_EfficientPython/pic29.png" class="largepic"/>

## 2.3. Code profiling for memory usage

**Quick and dirty approach**
```python
import sys
import numpy as np

nums_list = [*range(1000)] 
sys.getsizeof(nums_list)

nums_np = np.array(range(1000))
sys.getsizeof(nums_np)
```

```
9112
8069
```

**Code profiling: memory**
* Detailed stats on memory consumption 
* Line-by-line analyses
* Package used: `memory_profiler`

```python
pip install memory_profiler
```
* Using `memory_profiler` package

```
%load_ext memory_profiler

%mprun -f convert_units convert_units(heroes, hts, wts)
```
* Functnion must be imported when using `memory_filter`
    * hero_func.py

```python
from hero_funcs import convert_units
%load_ext memory_profiler
%mprun -f convert_units convert_units(heroes, hts, wts)
```
<img src="/assets/images/20210425_EfficientPython/pic7.png" class="largepic"/>

```python
In [3]:
%load_ext memory_profiler
In [4]:
from bmi_lists import calc_bmi_lists
In [5]:
%mprun -f calc_bmi_lists calc_bmi_lists(sample_indices, hts, wts)
Filename: /tmp/tmpfz_omoyf/bmi_lists.py

Line #    Mem usage    Increment   Line Contents
================================================
     1     91.9 MiB     91.9 MiB   def calc_bmi_lists(sample_indices, hts, wts):
     2                             
     3                                 # Gather sample heights and weights as lists
     4     92.4 MiB      0.3 MiB       s_hts = [hts[i] for i in sample_indices]
     5     93.2 MiB      0.3 MiB       s_wts = [wts[i] for i in sample_indices]
     6                             
     7                                 # Convert heights from cm to m and square with list comprehension
     8     94.3 MiB      0.4 MiB       s_hts_m_sqr = [(ht / 100) ** 2 for ht in s_hts]
     9                             
    10                                 # Calculate BMIs as a list with list comprehension
    11     95.0 MiB      0.3 MiB       bmis = [s_wts[i] / s_hts_m_sqr[i] for i in range(len(sample_indices))]
    12                             
    13     95.0 MiB      0.0 MiB       return bmis
```


* Small memory allocations could result in 0.0 MiB output.
* Inspects memory by querying the operating system
* Results may differ between platforms and runs
    * Can still observe how each line of code compares to others based on memory consumption

**Compare**

```python
def get_publisher_heroes(heroes, publishers, desired_publisher):

    desired_heroes = []

    for i,pub in enumerate(publishers):
        if pub == desired_publisher:
            desired_heroes.append(heroes[i])

    return desired_heroes

def get_publisher_heroes_np(heroes, publishers, desired_publisher):

    heroes_np = np.array(heroes)
    pubs_np = np.array(publishers)

    desired_heroes = heroes_np[pubs_np == desired_publisher]

    return desired_heroes
```

* `get_publisher_heroes_np()` is faster.
* Both functions have the same memory consumption.
* Should use get_publisher_heroes_np().

# 3. Gaining efficiencies

Learn a few useful built-in modules for writing efficient code and practice using set theory. You'll then learn about looping patterns in Python and how to make them more efficient.

## 3.1. Efficiently combining, counting, and iterating

**Combining object**

```python
names = ['Bulbasaur', 'Charmander', 'Squirtle'] 
hps = [45, 39, 44]
combined = []
for i,pokemon in enumerate(names):
    combined.append((pokemon, hps[i]))
print(combined)
```
```
[('Bulbasaur', 45), ('Charmander', 39), ('Squirtle', 44)]
```

**Combining object with zip**
Python's built-in function zip provides a more elegant solution. The name "zip" describes how this function combines objects like a zipper on a jacket (making two separate things become one). zip returns a zip object that must be unpacked into a list and printed to see the contents. Each item is a tuple of elements from the original lists.

```python
combined_zip = zip(names, hps) 
print(type(combined_zip))

combined_zip_list = [*combined_zip]
print(combined_zip_list)
```
```
<class 'zip'>
[('Bulbasaur', 45), ('Charmander', 39), ('Squirtle', 44)]
```

**The Collection module**
* Part of Python's Standard Library (built-in module)
* Specialized container datatypes
    *Alternatives to general purpose dict, list, set, and tuple
* Notable:
    * `namedtuple`: tuple subclasses with named fields
    * `deque`: list-like container with fast appends and pops
    * **`Counter`: dict for counting hashable objects**
    * `OrderedDict` : dict that retains order of entries
    * `defaultdict`: dict that calls a factory function to supply missing values
**Counting with loop**
```python
# Each Pokémon's type (720 total)
poke_types = ['Grass', 'Dark', 'Fire', 'Fire', ...]
type_counts = {}
for poke_type in poke_types:
    if poke_type not in type_counts: 
        type_counts[poke_type] = 1
    else:
    type_counts[poke_type] += 1 
print(type_counts)
```
<img src="/assets/images/20210425_EfficientPython/pic8.png" class="largepic"/>

**collection.Counter()**
```python
# Each Pokémon's type (720 total)
poke_types = ['Grass', 'Dark', 'Fire', 'Fire', ...] 
from collections import Counter
type_counts = Counter(poke_types) 
print(type_counts)
```

<img src="/assets/images/20210425_EfficientPython/pic9.png" class="largepic"/>

**Counter** returns a Counter dictionary of key-value pairs. When printed, it's ordered by highest to lowest counts. If comparing runtime times, we'd see that using Counter takes half the time as the standard dictionary approach

**The itertools module**
* Part of Python's Standard Library (built-in module)
* Functional tools for creating and using iterators
* Notable:
    * Infinite iterators: `count`,`cycle`,`repeat`
    * Finnite iterators: `accumlate`,`chain`,`zip_longest`,etc.
    * **Combination generators**: `product`, `permutationsn`,`combinantions`

**Combinations with loop**
```python
poke_types = ['Bug', 'Fire', 'Ghost', 'Grass', 'Water']
combos = []

for x in  poke_types: 
    for y in poke_types:
        if x == y: #  skip pairs having the same type twice
            continue
        if ((x,y) not in combos) & ((y,x) not in combos): # Either order of the pair doesn't already exist within the combos list before appending it
            combos.append((x,y))
print(combos)
```
<img src="/assets/images/20210425_EfficientPython/pic10.png" class="largepic"/>

**itertools.combinations()**
```python
poke_types = ['Bug','Fire','Ghost','Grass','Water'] 
from itertools import combinations
combos_obj = combinations(poke_types, 2) 
print(type(combos_obj))
```
<img src="/assets/images/20210425_EfficientPython/pic11.png" class="largepic"/>

```python
combos = [*combos_obj] 
print(combos)
```
<img src="/assets/images/20210425_EfficientPython/pic12.png" class="largepic"/>

**Practice**

```python

The first five items in the names list are: ['Abomasnow', 'Abra', 'Absol', 'Accelgor', 'Aerodactyl']

The first five items in the primary_types list are: ['Grass', 'Psychic', 'Dark', 'Bug', 'Rock']

The first five items in the secondary_types list are: ['Ice', nan, nan, nan, 'Flying']

# Combine names and primary_types
names_type1 = [*zip(names, primary_types)]

print(*names_type1[:5], sep='\n')


<script.py> output:
    ('Abomasnow', 'Grass')
    ('Abra', 'Psychic')
    ('Absol', 'Dark')
    ('Accelgor', 'Bug')
    ('Aerodactyl', 'Rock')

# Combine all three lists together
names_types = [*zip(names,primary_types,secondary_types)]

print(*names_types[:5], sep='\n')


<script.py> output:
    ('Abomasnow', 'Grass', 'Ice')
    ('Abra', 'Psychic', nan)
    ('Absol', 'Dark', nan)
    ('Accelgor', 'Bug', nan)
    ('Aerodactyl', 'Rock', 'Flying')

# Combine five items from names and three items from primary_types
differing_lengths = [*zip(names[:5],(primary_types[:3]))]

print(*differing_lengths, sep='\n')


<script.py> output:
    ('Abomasnow', 'Grass')
    ('Abra', 'Psychic')
    ('Absol', 'Dark')
```



```python
# Collect the count of primary types
type_count = Counter(primary_types)
print(type_count, '\n')

# Collect the count of generations
gen_count = Counter(generations)
print(gen_count, '\n')

# Use list comprehension to get each Pokémon's starting letter
starting_letters = [name[0] for name in names]

# Collect the count of Pokémon for each starting_letter
starting_letters_count = Counter(starting_letters)
print(starting_letters_count)
```

```
Counter({'Water': 66, 'Normal': 64, 'Bug': 51, 'Grass': 47, 'Psychic': 31, 'Rock': 29, 'Fire': 27, 'Electric': 25, 'Ground': 23, 'Fighting': 23, 'Poison': 22, 'Steel': 18, 'Ice': 16, 'Fairy': 16, 'Dragon': 16, 'Ghost': 13, 'Dark': 13}) 

Counter({5: 122, 3: 103, 1: 99, 4: 78, 2: 51, 6: 47}) 

Counter({'S': 83, 'C': 46, 'D': 33, 'M': 32, 'L': 29, 'G': 29, 'B': 28, 'P': 23, 'A': 22, 'K': 20, 'E': 19, 'W': 19, 'T': 19, 'F': 18, 'H': 15, 'R': 14, 'N': 13, 'V': 10, 'Z': 8, 'J': 7, 'I': 4, 'O': 3, 'Y': 3, 'U': 2, 'X': 1})
```

```python
# Import combinations from itertools

from itertools import combinations

# Create a combination object with pairs of Pokémon
combos_obj = combinations(pokemon,2)
print(type(combos_obj), '\n')

# Convert combos_obj to a list by unpacking
combos_2 = [*combos_obj]
print(combos_2, '\n')

# Collect all possible combinations of 4 Pokémon directly into a list
combos_4 = [*combinations(pokemon,4)]
print(combos_4)
```

```
<class 'itertools.combinations'> 

[('Geodude', 'Cubone'), ('Geodude', 'Lickitung'), ('Geodude', 'Persian'), ('Geodude', 'Diglett'), ('Cubone', 'Lickitung'), ('Cubone', 'Persian'), ('Cubone', 'Diglett'), ('Lickitung', 'Persian'), ('Lickitung', 'Diglett'), ('Persian', 'Diglett')] 

[('Geodude', 'Cubone', 'Lickitung', 'Persian'), ('Geodude', 'Cubone', 'Lickitung', 'Diglett'), ('Geodude', 'Cubone', 'Persian', 'Diglett'), ('Geodude', 'Lickitung', 'Persian', 'Diglett'), ('Cubone', 'Lickitung', 'Persian', 'Diglett')]
```

## 3.2. Set theory

* Branch of Mathematics applied to collections of objects
* Python has built-in `set` datatype with accompanying methods:
    * `intersection()`: all elements that are in both sets
    * `difference()`: all elements in one set but not the other
    * `symmetric_difference`: all elements in exactly one set
    * `union()`: all elements that are in either set
* Fast membership testing
    * Check if a value exists in a sequence or not
    * Using the	operator `in`

**Check similar object with loop (stupid way)**:

```python
in_common = []
for pokemon_a in  list_a: 
    for pokemon_b in list_b:
        if pokemon_a == pokemon_b:
            in_common.append(pokemon_a)
print(in_common)
``` 

**Smart way (also faster in runtime)**

```python
set_a.intersection(set_b)
```
**Check difference**

```python
set_a.difference(set_b)

# Set method: symmetric difference
set_a.symmetric_difference(set_b)
```
**Set method: union**

```python
set_a.union(set_b)
```

**Membership testing with sets**
```python
# The same 720 total Pokémon in each data structure
names_list	=	['Abomasnow','Abra','Absol',...]
names_tuple	=	('Abomasnow','Abra','Absol',...)
names_set	=	{'Abomasnow','Abra','Absol',...}
```

```python

%timeit 'Zubat' in	names_list

7.63 µs ± 211 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit 'Zubat' in	names_tuple

7.6 µs ± 394 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit 'Zubat' in	names_set

37.5 ns ± 1.37 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

**Unique with sets**

```python
# 720 Pokémon primary types corresponding to each Pokémon
primary_types = ['Grass',	'Psychic',	'Dark',	'Bug', ...]

unique_types = []
for prim_type in primary_types:
    if	prim_type not in unique_types: 
        unique_types.append(prim_type)

print(unique_types)
```

**Smart way unique**

```python
# 720 Pokémon primary types corresponding to each Pokémon 
primary_types = ['Grass', 'Psychic', 'Dark', 'Bug', ...]
unique_types_set = set(primary_types)
print(unique_types_set)
```

```python
{'Grass', 'Psychic', 'Dark', 'Bug', 'Steel', 'Rock', 'Normal',
'Water', 'Dragon', 'Electric', 'Poison', 'Fire', 'Fairy', 'Ice', 'Ground', 'Ghost', 'Fighting', 'Flying'}
```

**Practice**
```python
# Convert both lists to sets
ash_set = set(ash_pokedex)
misty_set = set(misty_pokedex)

# Find the Pokémon that exist in both sets
both = ash_set.intersection(misty_set)
print(both)

# Find the Pokémon that Ash has and Misty does not have
ash_only = ash_set.difference(misty_set)
print(ash_only)

# Find the Pokémon that are in only one set (not both)
unique_to_set = ash_set.symmetric_difference(misty_set)
print(unique_to_set)

{'Squirtle', 'Psyduck'}
{'Koffing', 'Spearow', 'Zubat', 'Wigglytuff', 'Rattata', 'Vulpix', 'Pikachu', 'Bulbasaur'}
{'Koffing', 'Spearow', 'Zubat', 'Starmie', 'Slowbro', 'Krabby', 'Poliwag', 'Magikarp', 'Vaporeon', 'Wigglytuff', 'Tentacool', 'Rattata', 'Vulpix', 'Horsea', 'Pikachu', 'Bulbasaur'}
```

```python
# Convert Brock's Pokédex to a set
brock_pokedex_set = set(brock_pokedex)
print(brock_pokedex_set)

{'Geodude', 'Zubat', 'Tauros', 'Onix', 'Dugtrio', 'Golem', 'Vulpix', 'Kabutops', 'Machop', 'Omastar'}

# Check if Psyduck is in Ash's list and Brock's set
print('Psyduck' in ash_pokedex)
print('Psyduck' in brock_pokedex_set)

True
False

# Check if Machop is in Ash's list and Brock's set
print("Machop" in ash_pokedex)
print("Psyduck" in brock_pokedex_set)

False
False
```
**Gathering unique Pokémon**
```python
A sample of 500 Pokémon has been created with replacement (meaning a Pokémon could be selected more than once and duplicates exist within the sample).

Three lists have been loaded into your session:

The names list contains the names of each Pokémon in the sample.
The primary_types list containing the corresponding primary type of each Pokémon in the sample.
The generations list contains the corresponding generation of each Pokémon in the sample.
The below function was written to gather unique values from each list:

def find_unique_items(data):
    uniques = []

    for item in data:
        if item not in uniques:
            uniques.append(item)

    return uniques
```

```python
# Use find_unique_items() to collect unique Pokémon names
uniq_names_func = find_unique_items(names)
print(len(uniq_names_func))

# Convert the names list to a set to collect unique Pokémon names
uniq_names_set = set(names)
print(len(uniq_names_set))

# Check that both unique collections are equivalent
print(sorted(uniq_names_func) == sorted(uniq_names_set))

# Use the best approach to collect unique primary types and generations
uniq_types = set(primary_types)
uniq_gens = set(generations)
print(uniq_types, uniq_gens, sep='\n') 
```

## 3.3. Eliminating loops

**Looping in Python**
* Looping patterns:
    * `for` loop: iterate over sequence piece-by-piece 
    * `while` loop: repeat loop as long as condition is met
    * "nested" loops: use one loop inside another loop 
    * Costly!

**Benefits of eliminating loops**
* Fewer lines of code
* Better code readability
    * "Flat is better than nested" 
* Efficiency gains

**Eliminating loops with builts-ins**

```python
# List of HP, Attack, Defense, Speed poke_stats = [
poke_stats = [	
    [90,92,75,60],
	[25,20,15,90],
    [65,130,60,75],
...
]
# For loop approach
totals = []
for row in poke_stats: totals.append(sum(row))
# List comprehension
totals_comp = [sum(row) for row in poke_stats] 
# Built-in map() function
totals_map = [*map(sum, poke_stats)]
```

```python
poke_types = ['Bug','Fire','Ghost','Grass','Water']
# Nested for loop approach 
combos = []
for x in poke_types:  
    for y in poke_types:
        if	x == y:
            continue
        if	((x,y) not in combos) & ((y,x) not in combos): 
            combos.append((x,y))

# Built-in module approach
from itertools import combinations 
combos2 = [*combinations(poke_types, 2)]
```

**Eliminate loops with NumPy**

```python
# Array of HP, Attack, Defense, Speed import numpy as np
poke_stats = np.array([
	[90,92,75,60],
	[25,20,15,90],
	[65,130,60,75],
    ...
])

avgs = []
for row in poke_stats: 
    avg = np.mean(row) 
    avgs.append(avg)
print(avgs)

# Better approach
avgs_np = poke_stats.mean(axis=1) 
print(avgs_np)
```

**Gathering Pokémon without a loop**
A list containing 720 Pokémon has been loaded into your session as poke_names. Another list containing each Pokémon's corresponding generation has been loaded as poke_gens.

A for loop has been created to filter the Pokémon that belong to generation one or two, and collect the number of letters in each Pokémon's name

```python

gen1_gen2_name_lengths_loop = []

for name,gen in zip(poke_names, poke_gens):
    if gen < 3:
        name_length = len(name)
        poke_tuple = (name, name_length)
        gen1_gen2_name_lengths_loop.append(poke_tuple)

# Collect Pokémon that belong to generation 1 or generation 2
gen1_gen2_pokemon = [name for name,gen in zip(poke_names, poke_gens) if gen < 3]

# Create a map object that stores the name lengths
name_lengths_map = map(len, gen1_gen2_pokemon)

# Combine gen1_gen2_pokemon and name_lengths_map into a list
gen1_gen2_name_lengths = [*zip(gen1_gen2_pokemon, name_lengths_map)]

print(gen1_gen2_name_lengths_loop[:5])
print(gen1_gen2_name_lengths[:5])


[('Abra', 4), ('Aerodactyl', 10), ('Aipom', 5), ('Alakazam', 8), ('Ampharos', 8)]
[('Abra', 4), ('Aerodactyl', 10), ('Aipom', 5), ('Alakazam', 8), ('Ampharos', 8)]
```

```python

# Create a total stats array
total_stats_np = stats.sum(axis=1)

# Create an average stats array
avg_stats_np = stats.mean(axis=1)

# Combine names, total_stats_np, and avg_stats_np into a list
poke_list_np = [*zip(names,total_stats_np,avg_stats_np)]

print(poke_list_np == poke_list, '\n')
print(poke_list_np[:3])
print(poke_list[:3], '\n')
top_3 = sorted(poke_list_np, key=lambda x: x[1], reverse=True)[:3]
print('3 strongest Pokémon:\n{}'.format(top_3))
```

## 3.4. Writing better loops

* Understand what is being done with each loop iteration
* Move one-time calculations outside (above) the loop : If a calculation is performed for each iteration of a loop, but its value doesn't change with each iteration, it's best to move this calculation outside (or above) the loop
* Use holistic conversions outside (below) the loop: If a loop is converting data types with each iteration, it's possible that this conversion can be done outside (or below) the loop using a map function
* Anything that is done once should be outside the loop

**Moving calculations above a loop**
```python
import numpy as	np
names = ['Absol','Aron','Jynx','Natu','Onix'] 
attacks = np.array([130,70,50,50,45])
for pokemon,attack in zip(names, attacks): 
    total_attack_avg = attacks.mean()
    if	attack > total_attack_avg:
    print(
        "{}'s attack: {} > average: {}!"
        .format(pokemon, attack, total_attack_avg)
)

Absol's attack: 130 > average: 69.0!
Aron's attack: 70 > average: 69.0!
```

Move `total_attack_avg = attacks.mean()` above `for` loop

**Using holistic conversions**

Before:

```python
names = ['Pikachu', 'Squirtle', 'Articuno', ...] 
legend_status = [False, False, True, ...] 
generations = [1, 1, 1, ...]
poke_data = []
for poke_tuple in zip(names, legend_status, generations): 
    poke_list = list(poke_tuple) 
    poke_data.append(poke_list)
print(poke_data)
```

After:

```python
names = ['Pikachu', 'Squirtle', 'Articuno', ...] 
legend_status = [False, False, True, ...] 
generations = [1, 1, 1, ...]
poke_data_tuples = []
for poke_tuple in zip(names, legend_status, generations): 
    poke_data_tuples.append(poke_tuple)
poke_data = [*map(list, poke_data_tuples)] 
print(poke_data)

```

**Practice**:
Bad function:
```python
for gen,count in gen_counts.items():
    total_count = len(generations)
    gen_percent = round(count / total_count * 100, 2)
    print(
      'generation {}: count = {:3} percentage = {}'
      .format(gen, count, gen_percent)
    )
```
Improve:
```python
# Import Counter
from collections import Counter
# Collect the count of each generation
gen_counts = Counter(generations)

# Improve for loop by moving one calculation above the loop
total_count = len(generations)

for gen,count in gen_counts.items():
    gen_percent = round(count / total_count * 100, 2)
    print('generation {}: count = {:3} percentage = {}'
          .format(gen, count, gen_percent))

           
<script.py> output:
    generation 4: count = 112 percentage = 15.56
    generation 1: count = 151 percentage = 20.97
    generation 3: count = 136 percentage = 18.89
    generation 5: count = 154 percentage = 21.39
    generation 2: count =  99 percentage = 13.75
    generation 6: count =  68 percentage = 9.44
```
```python
# Original fuction
enumerated_pairs = []

for i,pair in enumerate(possible_pairs, 1):
    enumerated_pair_tuple = (i,) + pair
    enumerated_pair_list = list(enumerated_pair_tuple)
    enumerated_pairs.append(enumerated_pair_list)

# Collect all possible pairs using combinations()
possible_pairs = [*combinations(pokemon_types, 2)]

# Create an empty list called enumerated_tuples
enumerated_tuples= []

# Append each enumerated_pair_tuple to the empty list above
for i,pair in enumerate(possible_pairs, 1):
    enumerated_pair_tuple = (i,) + pair
    enumerated_tuples.append(enumerated_pair_tuple)

# Convert all tuples in enumerated_tuples to a list
enumerated_pairs = [*map(list, enumerated_tuples)]
print(enumerated_pairs)

[[1, 'Bug', 'Dark'], [2, 'Bug', 'Dragon'], [3, 'Bug', 'Electric'], [4, 'Bug', 'Fairy'], [5, 'Bug', 'Fighting'], [6, 'Bug', 'Fire'], [7, 'Bug', 'Flying'], [8, 'Bug', 'Ghost'], [9, 'Bug', 'Grass'], [10, 'Bug', 'Ground'], [11, 'Bug', 'Ice'], [12, 'Bug', 'Normal'], [13, 'Bug', 'Poison'], [14, 'Bug', 'Psychic'], [15, 'Bug', 'Rock'], [16, 'Bug', 'Steel'], [17, 'Bug', 'Water'], [18, 'Dark', 'Dragon'], [19, 'Dark', 'Electric'], [20, 'Dark', 'Fairy'], [21, 'Dark', 'Fighting'], [22, 'Dark', 'Fire'], [23, 'Dark', 'Flying'], [24, 'Dark', 'Ghost'], [25, 'Dark', 'Grass'], [26, 'Dark', 'Ground'], [27, 'Dark', 'Ice'], [28, 'Dark', 'Normal'], [29, 'Dark', 'Poison'], [30, 'Dark', 'Psychic'],... , [150, 'Psychic', 'Water'], [151, 'Rock', 'Steel'], [152, 'Rock', 'Water'], [153, 'Steel', 'Water']]
```

**Pokémon z-scores**

A list of 720 Pokémon has been loaded into your session as names. Each Pokémon's corresponding Health Points is stored in a NumPy array called hps. You want to analyze the Health Points using the z-score to see how many standard deviations each Pokémon's HP is from the mean of all HPs.

Original code:
```python
poke_zscores = []

for name,hp in zip(names, hps):
    hp_avg = hps.mean()
    hp_std = hps.std()
    z_score = (hp - hp_avg)/hp_std
    poke_zscores.append((name, hp, z_score))

highest_hp_pokemon = []

for name,hp,zscore in poke_zscores:
    if zscore > 2:
        highest_hp_pokemon.append((name, hp, zscore))
```

```python
# Calculate the total HP avg and total HP standard deviation
hp_avg = hps.mean()
hp_std = hps.std()

# Use NumPy to eliminate the previous for loop
z_scores = (hps - hp_avg)/hp_std

# Combine names, hps, and z_scores
poke_zscores2 = [*zip(names, hps, z_scores)]
print(*poke_zscores2[:3], sep='\n')

('Abomasnow', 80.0, 0.46797638117739043)
('Abra', 60.0, -0.3271693284337512)
('Absol', 131.0, 2.4955979406858013)
```

```python
# Calculate the total HP avg and total HP standard deviation
hp_avg = hps.mean()
hp_std = hps.std()

# Use NumPy to eliminate the previous for loop
z_scores = (hps - hp_avg)/hp_std

# Combine names, hps, and z_scores
poke_zscores2 = [*zip(names, hps, z_scores)]
print(*poke_zscores2[:3], sep='\n')

# Use list comprehension with the same logic as the highest_hp_pokemon code block
highest_hp_pokemon2 = [(name, hp, zscore) for name,hp,zscore in poke_zscores2 if zscore > 2]
print(*highest_hp_pokemon2, sep='\n')

('Abomasnow', 80.0, 0.46797638117739043)
('Abra', 60.0, -0.3271693284337512)
('Absol', 131.0, 2.4955979406858013)
('Absol', 131.0, 2.4955979406858013)
('Bonsly', 127.0, 2.3365687987635733)
('Caterpie', 122.0, 2.137782371360788)
('Cofagrigus', 133.0, 2.575112511646916)
('Cresselia', 126.0, 2.296811513283016)
('Dewgong', 122.0, 2.137782371360788)
('Druddigon', 126.0, 2.296811513283016)
('Froakie', 123.0, 2.1775396568413448)
('Kadabra', 135.0, 2.65462708260803)
('Klang', 123.0, 2.1775396568413448)
('Kricketune', 122.0, 2.137782371360788)
('Lumineon', 129.0, 2.4160833697246873)
('Magnemite', 137.0, 2.734141653569144)
('Nidorina', 119.0, 2.0185105149191167)
('Onix', 126.0, 2.296811513283016)
('Prinplup', 124.0, 2.217296942321902)
('Skuntank', 128.0, 2.3763260842441305)
('Swellow', 125.0, 2.2570542278024592)
```

# 4. Intro to pandas DataFrame iteration

A brief introduction on how to efficiently work with pandas DataFrames. Learn the various options you have for iterating over a DataFrame, learn how to efficiently apply functions to data stored in a DataFrame.

## 4.1 Intro to pandas DataFrame iteration
**Pandas**:
* Library used for data analysis
* Main data structure is the DataFrame
* Tabular data with labeled rows and columns 
* Built on top of the NumPy array structure

**Baseball stats**
```python
import pandas as pd
baseball_df = pd.read_csv('baseball_stats.csv')
print(baseball_df.head())
```

<img src="/assets/images/20210425_EfficientPython/pic13.png" class="largepic"/>

**Calculating win percentage**

```python
import numpy as np
def calc_win_perc(wins, games_played):
    win_perc = wins / games_played
    return np.round(win_perc,2)

# Adding in percentage to DataFrame

win_perc_list = []
for i in range(len(baseball_df)): 
    row = baseball_df.iloc[i] 
    wins = row['W']
    games_played = row['G']
    win_perc = calc_win_perc(wins, games_played) 
    win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list
print(baseball_df.head())
```

<img src="/assets/images/20210425_EfficientPython/pic14.png" class="largepic"/>

`iloc()`: With iloc() function, we can retrieve a particular value belonging to a row and column using the index values assigned to it.

**Faster method: Iterating with .iterrow()**

```python
win_perc_list = []
for i,row in baseball_df.iterrows(): 
    wins = row['W']
    games_played = row['G']

    win_perc = calc_win_perc(wins, games_played)

    win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list
```

**Practice**

```python
# Iterate over pit_df and print each index variable and then each row
for i,row in pit_df.iterrows():
    print(i)
    print(row)
    print(type(row))
```
```
0
Team         PIT
League        NL
Year        2012
RS           651
RA           674
W             79
G            162
Playoffs       0
Name: 0, dtype: object
<class 'pandas.core.series.Series'>
1
Team         PIT
League        NL
Year        2011
RS           610
RA           712
W             72
G            162
Playoffs       0
...
4
Team         PIT
League        NL
Year        2008
RS           735
RA           884
W             67
G            162
Playoffs       0
Name: 4, dtype: object
<class 'pandas.core.series.Series'>
```


```
<class 'pandas.core.series.Series'>
```

```python
# Print the row and type of each row
for row_tuple in pit_df.iterrows():
    print(row_tuple)
    print(type(row_tuple))
```

```
(0, Team         PIT
League        NL
Year        2012
RS           651
RA           674
W             79
G            162
Playoffs       0
Name: 0, dtype: object)
<class 'tuple'>
(1, Team         PIT
League        NL
Year        2011
RS           610
RA           712
W             72
G            162
Playoffs       0
...
(4, Team         PIT
League        NL
Year        2008
RS           735
RA           884
W             67
G            162
Playoffs       0
Name: 4, dtype: object)
<class 'tuple'>
```

Since `.iterrows()` returns each DataFrame row as a tuple of (index, pandas Series) pairs, you can either split this tuple and use the index and row-values separately (as you did with for i,row in `pit_df.iterrows())`, or you can keep the result of .iterrows() in the tuple form (as you did with for row_tuple in pit_df.iterrows()).

If using i,row, you can access things from the row using square brackets (i.e., row['Team']). If using row_tuple, you would have to specify which element of the tuple you'd like to access before grabbing the team name (i.e., row_tuple[1]['Team']).

With either approach, using `.iterrows()` will still be substantially faster than using `.iloc`.

```python

# Original function
def calc_run_diff(runs_scored, runs_allowed):

    run_diff = runs_scored - runs_allowed

    return run_diff

#Improved:

# Create an empty list to store run differentials
run_diffs = []

# Write a for loop and collect runs allowed and runs scored for each row
for i,row in giants_df.iterrows():
    runs_scored = row['RS']
    runs_allowed = row['RA']
    
    # Use the provided function to calculate run_diff for each row
    run_diff = calc_run_diff(runs_scored, runs_allowed)
    
    # Append each run differential to the output list
    run_diffs.append(run_diff)

giants_df['RD'] = run_diffs
print(giants_df)
```
```

<script.py> output:
      Team League  Year   RS   RA   W    G  Playoffs   RD
    0  SFG     NL  2012  718  649  94  162         1   69
    1  SFG     NL  2011  570  578  86  162         0   -8
    2  SFG     NL  2010  697  583  92  162         1  114
    3  SFG     NL  2009  657  611  88  162         0   46
    4  SFG     NL  2008  640  759  72  162         0 -119
```
## 4.2. Another iterator method: .itertuples()

**Iterating with .iterrows()**
```python
for row_tuple in team_wins_df.iterrows(): 
    print(row_tuple) 
    # We have to access the row's values with square bracket indexing.
    print(type(row_tuple[1]))
```
<img src="/assets/images/20210425_EfficientPython/pic15.png" class="largepic"/>

**Iterating with .itertuples() (faster)**

```python
for row_namedtuple in team_wins_df.itertuples(): 
    print(row_namedtuple)
```
<img src="/assets/images/20210425_EfficientPython/pic18.png" class="largepic"/>


```python
print(row_namedtuple.Index)
```
<img src="/assets/images/20210425_EfficientPython/pic16.png" class="largepic"/>


```python
print(row_namedtuple.Team)
```

<img src="/assets/images/20210425_EfficientPython/pic17.png" class="largepic"/>

```python
for row_tuple in team_wins_df.iterrows(): 
    print(row_tuple[1]['Team'])
```
<img src="/assets/images/20210425_EfficientPython/pic19.png" class="largepic"/>

```python
for row_namedtuple in team_wins_df.itertuples(): 
    print(row_namedtuple['Team'])
```

<img src="/assets/images/20210425_EfficientPython/pic20.png" class="largepic"/>

```python
for row_namedtuple in team_wins_df.itertuples(): 
    print(row_namedtuple.Team)
```
<img src="/assets/images/20210425_EfficientPython/pic21.png" class="largepic"/>

When using `.iterrows()`, we can use square brackets to reference a column within our team_wins_df DataFrame. 
When using `.itertuples()`, we have to use a dot when referring to a column within our DataFrame

**Practice**
**Iterating with .itertuples()**
```python
# Loop over the DataFrame and print each row
for row in rangers_df.itertuples():
  print(row)

Pandas(Index=0, Team='TEX', League='AL', Year=2012, RS=808, RA=707, W=93, G=162, Playoffs=1)
Pandas(Index=1, Team='TEX', League='AL', Year=2011, RS=855, RA=677, W=96, G=162, Playoffs=1)
...
Pandas(Index=35, Team='TEX', League='AL', Year=1974, RS=690, RA=698, W=83, G=161, Playoffs=0)
Pandas(Index=36, Team='TEX', League='AL', Year=1973, RS=619, RA=844, W=57, G=162, Playoffs=0)
```
```python
# Loop over the DataFrame and print each row's Index, Year and Wins (W)
for row in rangers_df.itertuples():
  i = row.Index
  year = row.Year
  wins = row.W

0 2012 93
1 2011 96
2 2010 90
...
33 1976 76
34 1975 79
35 1974 83
36 1973 57

  # Check if rangers made Playoffs (1 means yes; 0 means no)
  if row.Playoffs == 1:
    print(i,year, wins)

<script.py> output:
    0 2012 93
    1 2011 96
    2 2010 90
    13 1999 95
    14 1998 88
    16 1996 90
```
**Run differentials with .itertuples()**
```python
def calc_run_diff(runs_scored, runs_allowed):

    run_diff = runs_scored - runs_allowed

    return run_diff

run_diffs = []

# Loop over the DataFrame and calculate each row's run differential
for row in yankees_df.itertuples():
    
    runs_scored = row.RS
    runs_allowed = row.RA

    run_diff = calc_run_diff(runs_scored, runs_allowed)
    
    run_diffs.append(run_diff)

# Append new column
yankees_df['RD'] = run_diffs
# Sort to take highest run differential
final_df = yankees_df.sort_values(by=['RD'], ascending=False)
print(final_df )

<script.py> output:
       Team League  Year   RS   RA    W    G  Playoffs   RD
    0   NYY     AL  2012  804  668   95  162         1  136
    1   NYY     AL  2011  867  657   97  162         1  210
    2   NYY     AL  2010  859  693   95  162         1  166
    3   NYY     AL  2009  915  753  103  162         1  162
```

## 4.3. pandas alternative to looping

**pandas.apply() method**
Previous example:

```python
run_diffs_iterrows = []
for i,row in baseball_df.iterrows():
    run_diff = calc_run_diff(row['RS'], row['RA']) 
    run_diffs_iterrows.append(run_diff)

baseball_df['RD'] = run_diffs_iterrows
```

* Takes a function and applies it to a DataFrame
    * Must specify an axis to apply ( `0` for columns; `1` for rows)
* Can be used with anonymous functions (lambda functions)
* Example:

```python
def calc_run_diff(runs_scored, runs_allowed):

    run_diff = runs_scored - runs_allowed

    return run_diff

baseball_df.apply(
    lambda row: calc_run_diff(row['RS'], row['RA']), 
    axis=1
)
```

**Run differentials with `.apply()`**
```python
run_diffs_apply = baseball_df.apply(
    lambda row: calc_run_diff(row['RS'], row['RA']), 
    axis=1)
baseball_df['RD'] = run_diffs_apply 
print(baseball_df)
```

Faster than iterrows.

**Practice**
```python
# Gather sum of all columns
stat_totals = rays_df.apply(sum, axis=0)

RS          3783
RA          3265
W            458
Playoffs       3
dtype: int64


# Gather total runs scored in all games per year
total_runs_scored = rays_df[['RS', 'RA']].apply(sum, axis=1)

2012    1274
2011    1321
2010    1451
2009    1557
2008    1445
dtype: int64

def text_playoffs(num_playoffs): 
    if num_playoffs == 1:
        return 'Yes'
    else:
        return 'No' 

# Convert numeric playoffs to text by applying text_playoffs()
textual_playoffs = rays_df.apply(lambda row: text_playoffs(row['Playoffs']), axis=1)

2012     No
2011    Yes
2010    Yes
2009     No
2008    Yes
dtype: object


# Display the first five rows of the DataFrame
print(dbacks_df.head())

  Team League  Year   RS   RA   W    G  Playoffs
0  ARI     NL  2012  734  688  81  162         0
1  ARI     NL  2011  731  662  94  162         1
2  ARI     NL  2010  713  836  65  162         0
3  ARI     NL  2009  720  782  70  162         0
4  ARI     NL  2008  720  706  82  162         0

# Create a win percentage Series
def calc_win_perc(wins, games_played):
    win_perc = wins / games_played
    return np.round(win_perc,2)
 
win_percs = dbacks_df.apply(lambda row: calc_win_perc(row['W'], row['G']), axis=1)
print(win_percs, '\n')

0     0.50
1     0.58
2     0.40
3     0.43
4     0.51
5     0.56
6     0.47
7     0.48
8     0.31
9     0.52
10    0.60
11    0.57
12    0.52
13    0.62
14    0.40
dtype: float64 

# Append a new column to dbacks_df
dbacks_df['WP'] = win_percs
print(dbacks_df, '\n')

   Team League  Year   RS   RA    W    G  Playoffs    WP
0   ARI     NL  2012  734  688   81  162         0  0.50
1   ARI     NL  2011  731  662   94  162         1  0.58
4   ARI     NL  2008  720  706   82  162         0  0.51
5   ARI     NL  2007  712  732   90  162         1  0.56
9   ARI     NL  2003  717  685   84  162         0  0.52
10  ARI     NL  2002  819  674   98  162         1  0.60
11  ARI     NL  2001  818  677   92  162         1  0.57
12  ARI     NL  2000  792  754   85  162         0  0.52
13  ARI     NL  1999  908  676  100  162         1  0.62


# The manager who claimed the team has not made the playoffs every year they've had a win percentage of 0.50 or greater.

# Display dbacks_df where WP is greater than 0.50
print(dbacks_df[dbacks_df['WP'] >= 0.50])

   Team League  Year   RS   RA    W    G  Playoffs    WP
0   ARI     NL  2012  734  688   81  162         0  0.50
1   ARI     NL  2011  731  662   94  162         1  0.58
4   ARI     NL  2008  720  706   82  162         0  0.51
5   ARI     NL  2007  712  732   90  162         1  0.56
9   ARI     NL  2003  717  685   84  162         0  0.52
10  ARI     NL  2002  819  674   98  162         1  0.60
11  ARI     NL  2001  818  677   92  162         1  0.57
12  ARI     NL  2000  792  754   85  162         0  0.52
13  ARI     NL  1999  908  676  100  162         1  0.62
```

## 4.4. Optimal pandas iterating

**pandas internals**
* Eliminating loops applies to using pandas as well 
* pandas is built on NumPy
    * Take advantage of NumPy array effciencies

<img src="/assets/images/20210425_EfficientPython/pic22.png" class="largepic"/>

```python
print(baseball_df)
```
<img src="/assets/images/20210425_EfficientPython/pic23.png" class="largepic"/>


```
wins_np = baseball_df['W'].values 
print(type(wins_np))
```
<img src="/assets/images/20210425_EfficientPython/pic24.png" class="largepic"/>


```
print(wins_np)
```
<img src="/assets/images/20210425_EfficientPython/pic25.png" class="largepic"/>

**Power of vectorization**
*Broadcasting (vectorizing) is extremely efficient!

```python
baseball_df['RS'].values - baseball_df['RA'].values
```
Since `pandas` is built on top of `NumPy`, we can grab any of these DataFrame column's values as a `NumPy` array using the `.values()` method. 
Instead of looping over a DataFrame, and treating each row independently, like we've done with `.iterrows()`, `.itertuples()`, and `.apply()`, we can perform calculations on the underlying NumPy arrays of our baseball_df DataFrame. Here, we gather the RS and RA columns in our DataFrame as NumPy arrays, and use broadcasting to calculate run differentials all at once!

**Run differentials with arrays**
```python
run_diffs_np = baseball_df['RS'].values - baseball_df['RA'].values 
baseball_df['RD'] = run_diffs_np
```

**Last Practice**
```python
OrderedDict([('Team', 'Abbreviated team name'),
             ('League', 'Specifies National League or American League'),
             ('Year', "Each season's year"),
             ('RS', 'Runs scored in a season'),
             ('RA', 'Runs allowed in a season'),
             ('W', 'Wins in a season'),
             ('G', 'Games played in a season'),
             ('Playoffs', '`1` if a team made the playoffs; `0` if they did not'),
             ('WP', 'True win percentage for a season')])
```

```python
def calc_win_perc(wins, games_played):
    win_perc = wins / games_played
    return np.round(win_perc,2)

win_percs_list = []

for i in range(len(baseball_df)):
    row = baseball_df.iloc[i]

    wins = row['W']
    games_played = row['G']

    win_perc = calc_win_perc(wins, games_played)

    win_percs_list.append(win_perc)

baseball_df['WP'] = win_percs_list
```

```python
# Use the W array and G array to calculate win percentages
win_percs_np = calc_win_perc(baseball_df['W'].values, baseball_df['G'].values)

# Append a new column to baseball_df that stores all win percentages
baseball_df['WP'] = win_percs_np

print(baseball_df.head())

<script.py> output:
      Team League  Year   RS   RA   W    G  Playoffs    WP
    0  ARI     NL  2012  734  688  81  162         0  0.50
    1  ATL     NL  2012  700  600  94  162         1  0.58
    2  BAL     AL  2012  712  705  93  162         1  0.57
    3  BOS     AL  2012  734  806  69  162         0  0.43
    4  CHC     NL  2012  613  759  61  162         0  0.38
```

```python
def predict_win_perc(RS, RA):
    prediction = RS ** 2 / (RS ** 2 + RA ** 2)
    return np.round(prediction, 2)

win_perc_preds_loop = []

# Use a loop and .itertuples() to collect each row's predicted win percentage
for row in baseball_df.itertuples():
    runs_scored = row.RS
    runs_allowed = row.RA
    win_perc_pred = predict_win_perc(runs_scored, runs_allowed)
    win_perc_preds_loop.append(win_perc_pred)

# Apply predict_win_perc to each row of the DataFrame
win_perc_preds_apply = baseball_df.apply(lambda row: predict_win_perc(row['RS'], row['RA']), axis=1)

print(win_perc_preds_apply)
0       0.53
1       0.58
2       0.50
...
1229    0.62
1230    0.58
1231    0.41
Length: 1232, dtype: float64


# Calculate the win percentage predictions using NumPy arrays
win_perc_preds_np = predict_win_perc(baseball_df['RS'].values, baseball_df['RA'].values)
baseball_df['WP_preds'] = win_perc_preds_np
print(baseball_df.head())

  Team League  Year   RS   RA   W    G  Playoffs    WP  WP_preds
0  ARI     NL  2012  734  688  81  162         0  0.50      0.53
1  ATL     NL  2012  700  600  94  162         1  0.58      0.58
2  BAL     AL  2012  712  705  93  162         1  0.57      0.50
3  BOS     AL  2012  734  806  69  162         0  0.43      0.45
4  CHC     NL  2012  613  759  61  162         0  0.38      0.39


```

**After `%timeit` test, Using NumPy arrays was the fastest approach, followed by the `.itertuples()` approach, and the `.apply()` approach was slowest.**


# 5. Reference


1. [Writing Efficient Python Code - DataCamp](https://learn.datacamp.com/courses/writing-efficient-python-code)

