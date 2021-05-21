---
layout: post
author: ledinhtrunghieu
title: Lesson 5 - Writing Functions in Python
---
    
# 1. Best Practices

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
<img src="/assets/images/20210426_FunctionsInPytho/pic1.png" class="largepic"/>


```python
import inspect 
print(inspect.getdoc(the_answer))
```
<img src="/assets/images/20210426_FunctionsInPytho/pic2.png" class="largepic"/>


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

<img src="/assets/images/20210426_FunctionsInPytho/pic2.png" class="largepic"/>

In Python, integers are immutable, meaning they can't be changed.

<img src="/assets/images/20210426_FunctionsInPytho/pic4.png" class="largepic"/>

There are only a few immutable data types in Python because almost everything is represented as an object. The only way to tell if something is mutable is to see if there is a function or method that will change the object without assigning it to a new variable.

# 2. Context Managers

# 2.1. Using context managers

**What is a context manager**
A context manager:
* Sets up a context 
* Runs your code 
* Removes the context


