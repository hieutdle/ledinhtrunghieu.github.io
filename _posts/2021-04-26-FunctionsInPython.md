---
layout: post
author: ledinhtrunghieu
title: Lesson 5 - Writing Functions in Python
---
    
# 1. Best Practices

Learn about Docstrings, why they matter, how to know when you need to turn a chunk of code into a function and the details of how Python passes arguments to functions.

## 1.1. Docstrings

**A Complex function**
```python
"""Split a DataFrame's columns into two halves and then stack   them vertically, returning a new DataFrame with `new_names` as the column names.

Args:
df (DataFrame): The DataFrame to split.
new_names (iterable of str): The column names for the new DataFrame.

Returns:
DataFrame """

def split_and_stack(df, new_names): 
    half = int(len(df.columns) / 2) 
    left = df.iloc[:, :half]
    right = df.iloc[:, half:] 
    return pd.DataFrame(
        data=np.vstack([left.values, right.values]), 
        columns=new_names
)
```

With a **docstring** though, it is much easier to tell what the expected inputs and outputs should be, as well as what the function does. This makes it easier for you and other engineers to use your code in the future.

```python
def function_name(arguments):
    """
    Description	of	what the function does.
    
    Description	of	the arguments, if any.

    Description	of	the return value(s), if any.

    Description	of	errors raised, if any.

    Optional extra notes or examples of usage. 
    """
```

A **docstring** is a string written as the first line of a function. Because docstrings usually span multiple lines, they are enclosed in triple quotes, Python's way of writing multi-line strings. Every docstring has some (although usually not all) of these five key pieces of information: what the function does, what the arguments are, what the return value or values should be, info about any errors raised, and anything else you'd like to say about the function.

**Docstring format**
* Google Style
* Numpydoc
* reStructuredText
* EpyText

**Google Style - description**

```python
def function(arg_1, arg_2=42): 
    """Description of what the function does.

    Args:
    arg_1 (str): Description of arg_1 that can break onto the next line if needed.
    arg_2 (int, optional): Write optional when an argument has a default value.

    Returns:
    bool: Optional description of the return value 
    Extra lines are not indented.

    Raises:
    ValueError: Include any error types that the function intentionally raises.

    Notes:
    See https://www.datacamp.com/community/tutorials/docstrings-python for 

```

* In Google style, the docstring starts with a concise description of what the function does. This should be in imperative language. For instance: "Split the data frame and stack the columns" instead of "This function will split the data frame and stack the columns".
* Next comes the "Args" section where you list each argument name, followed by its expected type in parentheses, and then what its role is in the function.
* If an argument has a default value, mark it as "optional" when describing the type. If the function does not take any parameters, feel free to leave this section out.
* "Returns" section is where you list the expected type or types of what gets returned. You can also provide some comment about what gets returned, but often the name of the function and the description will make this clear. Additional lines should not be indented.
* Finally, if your function intentionally raises any errors, you should add a "Raises" section. You can also include any additional notes or examples of usage in free form text at the end.

**Numpydoc**

The Numpydoc format is very similar and is the most common format in the scientific Python community

```python
def function(arg_1, arg_2=42):
    """
    Description of what the function does. 
    
    Parameters
    ----------
    arg_1 : expected type of arg_1 
        Description of arg_1.
    arg_2 : int, optional
        Write optional when an argument has a default value. 
        Default=42.

    Returns
    -------
    The type of the return value
        Can include a description of the return value.
        Replace "Returns" with "Yields" if this function is a generator. """
    """
```

**Retrieving docstrings**

```python
def the_answer():
    """Return the answer to life, the universe, and everything.
    
    Returns: int
    """
return 42
print(the_answer.__doc__)
```
<img src="/assets/images/20210426_FunctionsInPython/pic1.png" class="largepic"/>


```python
import inspect 
print(inspect.getdoc(the_answer))
```
<img src="/assets/images/20210426_FunctionsInPython/pic2.png" class="largepic"/>


## 1.2. DRY and "Do One Thing"

**Don't repeat yourself (DRY)**

```python
train = pd.read_csv('train.csv')
train_y = train['labels'].values
train_X = train[col for col in train.columns if col != 'labels'].values 
train_pca = PCA(n_components=2).fit_transform(train_X) 
plt.scatter(train_pca[:,0], train_pca[:,1])
```

```python
val = pd.read_csv('validation.csv') 
val_y = val['labels'].values
val_X = val[col for col in val.columns if col != 'labels'].values 
val_pca = PCA(n_components=2).fit_transform(val_X) 
plt.scatter(val_pca[:,0], val_pca[:,1])
```

```python
test = pd.read_csv('test.csv') 
test_y = test['labels'].values
test_X = test[col for col in test.columns if col != 'labels'].values 
test_pca = PCA(n_components=2).fit_transform(train_X) 
plt.scatter(test_pca[:,0], test_pca[:,1])
```

**Use functions to avoid repetition**

```python
def load_and_plot(path):
    """Load a data set and plot the first two principal components.

    Args:
        path (str): The location of a CSV file.

    Returns:
        tuple of ndarray: (features, labels) 
    """
    # load the data
    data = pd.read_csv(path) 
    y = data['label'].values
    X = data[col for col in train.columns if col != 'label'].values 

    # plot the first two principle components 
    pca = PCA(n_components=2).fit_transform(X) 

    # return loaded data 
    plt.scatter(pca[:,0], pca[:,1])
    return X, y
```

```python
train_X, train_y = load_and_plot('train.csv') 
val_X, val_y = load_and_plot('validation.csv') 
test_X, test_y = load_and_plot('test.csv')
```

Every function should have a single responsibility. Instead of one big function, we could have a more nimble function that just loads the data and a second one for plotting. Imagine that later on in your script, you just want to load the data and not plot it. 

```python
def load_data(path):
    """Load a data set.
    Args:
    path (str): The location of a CSV file.
    Returns:
    tuple of ndarray: (features, labels) 
    """
    data = pd.read_csv(path) 
    y = data['labels'].values
    X = data[col for col in data.columns 
        if col != 'labels'].values
return X, y
```

```python
def plot_data(X):
    """Plot the first two principal components of a matrix.
    
    Args:
        X (numpy.ndarray): The data to plot. 
    """
    pca = PCA(n_components=2).fit_transform(X)
    plt.scatter(pca[:,0], pca[:,1])
```

**Advantages of doing one thing**
* More flexible
* More easily understood
* Simpler to test
* Simpler to debug
* Easier to change

## 1.3. Pass by assignment

<img src="/assets/images/20210426_FunctionsInPython/pic2.png" class="largepic"/>

In Python, integers are immutable, meaning they can't be changed.

<img src="/assets/images/20210426_FunctionsInPython/pic4.png" class="largepic"/>

There are only a few immutable data types in Python because almost everything is represented as an object. The only way to tell if something is mutable is to see if there is a function or method that will change the object without assigning it to a new variable.

# 2. Context Managers

Context managers are a convenient way to provide connections in Python and guarantee that those connections get cleaned up when you are done using them. 

## 2.1. Using context managers

**What is a context manager**
A context manager:
* Sets up a context 
* Runs your code 
* Removes the context

**A real-world example**
```python
with open('my_file.txt') as my_file: 
    text = my_file.read()
    length = len(text)

print('The file is {} characters long'.format(length))
```

`open` does three things:
* Sets up a context by opening a file
* Lets you run any code you want on that file 
* Removes the context by closing the file

**Using a context manager**
```python
with <context-manager>(<args>) as <variable-name>: 
    # Run your code here
    # This code is running "inside the context"

# This code runs after the context is removed
```

**Practice**

```python
image = get_image_from_instagram()

# Time how long process_with_numpy(image) takes to run
with timer():
  print('Numpy version')
  process_with_numpy(image)

# Time how long process_with_pytorch(image) takes to run
with timer():
  print('Pytorch version')
  process_with_pytorch(image)
```

## 2.2. Writing context managers

**Two ways to define a context manager**
* Class-based 
* Function-based

```python
@contextlib.contextmanager 
def my_context():
    # Add any set up code you need yield
    yield
    # Add any teardown code you need
```

1.	Define a function.
2.	(optional) Add any set up code your context needs.
3.	Use the "yield" keyword.
4.	(optional) Add any teardown code your context needs.
5.	Add the `@contextlib.contextmanager` decorator.


**The "yield" keyword**

```python
@contextlib.contextmanager 
def my_context():
    print('hello') 
    yield 42 
    print('goodbye')
```

```python
with my_context() as foo: 
    print('foo is {}'.format(foo))
```

<img src="/assets/images/20210426_FunctionsInPython/pic5.png" class="largepic"/>

The "yield" keyword may also be new to you. When you write this word, it means that you are going to return a value, but you expect to finish the rest of the function at some point in the future. The value that your context manager yields can be assigned to a variable in the "with" statement by adding "as <variable name>". Here, we've assigned the value 42 that my_context() yields to the variable "foo". By running this code, you can see that after the context block is done executing, the rest of the my_context() function gets run, printing "goodbye". Some of you may recognize the "yield" keyword as a thing that gets used when creating generators. In fact, a context manager function is technically a generator that yields a single value.


**Setup and teardown**

```python
@contextlib.contextmanager 
def database(url):
    # set up database connection
    db = postgres.connect(url)

    yield db

    # tear down database connection
    db.disconnect()
```

```python
url = 'http://datacamp.com/data' 
with database(url) as my_db:
    course_list = my_db.execute( 
        'SELECT * FROM courses'
)

```

**Yielding a value or None**
```python
@contextlib.contextmanager 
    def in_dir(path):
    # save current working directory
    old_dir = os.getcwd()

    # switch to new working directory
    os.chdir(path)

    yield

    # change back to previous # working directory
    os.chdir(old_dir)
```

```pyhon
with in_dir('/data/project_1/'): 
    project_files = os.listdir()
```

**Practice**
```python
@contextlib.contextmanager
def open_read_only(filename):
  """Open a file in read-only mode.

  Args:
    filename (str): The location of the file to read

  Yields:
    file object
  """
  read_only_file = open(filename, mode='r')
  # Yield read_only_file so it can be assigned to my_file
  yield read_only_file
  # Close read_only_file
  read_only_file.close()

with open_read_only('my_file.txt') as my_file:
  print(my_file.read())
```

## 1.3. Advanced topics

**Nested context**

```python
def copy(src, dst):
    """Copy the contents of one file to another.

    Args:
        src (str): File name of the file to be copied. 
        dst (str): Where to write the new file.
    """
    # Open the source file and read in the contents 
    with open(src) as f_src:
        contents = f_src.read()

    # Open the destination file and write out the contents 
    with open(dst, 'w') as f_dst:
        f_dst.write(contents)
```

```python
def copy(src, dst):
    """Copy the contents of one file to another.

    Args:
        src (str): File name of the file to be copied. 
        dst (str): Where to write the new file.
    """
    # Open both files
    with open(src) as f_src:
        with open(dst, 'w') as f_dst:
        # Read and write each line, one at a time 
        for line in f_src:
            f_dst.write(line)
```

**Handling errors**
```python
def get_printer(ip):
    p = connect_to_printer(ip)

    # This MUST be called or no one else will 
    # be able to connect to the printer 
    p.disconnect()
    print('disconnected from printer')

doc = {'text': 'This is my text.'}

with get_printer('10.0.34.111') as printer:
    printer.print_page(doc['txt'])
```

<img src="/assets/images/20210426_FunctionsInPython/pic6.png" class="largepic"/>


This will raise a KeyError because "txt" is not in the "doc" dictionary. And that means "p.disconnect()" doesn't get called.

```python
try:
    # code that might raise an error 
except:
    # do something about the error 
finally:
    # this code runs no matter what
```

```python
def get_printer(ip):
    p = connect_to_printer(ip)

    try:
        yield 
    finally:
        p.disconnect()  
        print('disconnected from printer')

doc = {'text': 'This is my text.'}

with get_printer('10.0.34.111') as printer: 
    printer.print_page(doc['txt'])
```

<img src="/assets/images/20210426_FunctionsInPython/pic7.png" class="largepic"/>

When the sloppy programmer runs their code, they still get the KeyError, but "finally" ensures that "p.disconnect()" is called before the error is raised.


**Context Manager Pattern**

<img src="/assets/images/20210426_FunctionsInPython/pic8.png" class="largepic"/>

**Practice**

```python
# Use the "stock('NVDA')" context manager
# and assign the result to the variable "nvda"
with stock('NVDA') as nvda:
  # Open 'NVDA.txt' for writing as f_out
  with open('NVDA.txt', 'w') as f_out:
    for _ in range(10):
      value = nvda.price()
      print('Logging ${:.2f} for NVDA'.format(value))
      f_out.write('{:.2f}\n'.format(value))
```

```python
def in_dir(directory):
  """Change current working directory to `directory`,
  allow the user to run some code, and change back.

  Args:
    directory (str): The path to a directory to work in.
  """
  current_dir = os.getcwd()
  os.chdir(directory)

  # Add code that lets you handle errors
  try:
    yield
  # Ensure the directory is reset,
  # whether there was an error or not
  finally:
    os.chdir(current_dir)
```

# 3. Decorators

**Functions are just another type of object**

Python objects:

```python
def x(): 
    pass
x = [1, 2, 3]
x = {'foo': 42}
x = pandas.DataFrame()
x	= 3
x	= 71.2
```

**Functions as variable**
```python
def my_function(): 
    print('Hello')
x = my_function 
type(x)
```

```
<type 'function'>
```

```python
x()
```

```
Hello
```

```python
PrintyMcPrintface = print 
PrintyMcPrintface('Python is awesome!')
```

```
Python is awesome!
```


