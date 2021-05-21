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

