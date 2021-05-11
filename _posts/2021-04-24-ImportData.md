---
layout: post
author: ledinhtrunghieu
title: Lesson 3 - Streamlined Data Ingestion with pandas
---

# 1. Importing Data from Flat Files

Using pandas to get just the data you want from flat files, learn how to wrangle data types and handle errors.

## 1.1. Introduction to flat files

<img src="/assets/images/20210424_ImportData/pic1.png" class="largepic"/>

**pandas**: pandas was originally developed by Wes McKinney in 2008 for financial quantitative analysis, but today it has a large development community and is used in many disciplines. pandas makes it easy to load and manipulate data, and it integrates with loads of analysis and visualization libraries.

**Data Frame**
Central to pandas is the data frame. Data frames are two-dimensional data structures. This means they have columns, typically labeled with variable names, and rows, which also have labels, known as an index in pandas. The default index is the row number, but you can specify a column as the index, and many types of data can be used.

<img src="/assets/images/20210424_ImportData/pic2.png" class="largepic"/>

While you can create data frames by hand, you'll usually want to load existing data. Pandas handles many data formats, but let's start with a basic one: **flat files**.

**Flat Files**
* Simple, making them popular for storing and sharing data
* Can be exported from database management systems and spreadsheet applications, and many online data portals provide flat file downloads
* Data is stored as plain text, with no formatting like colors or bold type
* Each line in the file represents one row, with column values separated by a chosen character called a delimiter
* The delimiter is a comma, and such files are called CSVs, or comma-separated values, but other characters can be used. 
* A single `pandas` function loads all flat files, no matter the delimiter: `read_csv()`.

**Loading CSVs**

```
import pandas as pd

tax_data = pd.read_csv("us_tax_data_2016.csv")
tax_data.head(4)
```

**Loading Other Flat Files**

* Specify a different delimeter with `sep`

```
import pandas as pd

tax_data = pd.read_csv("us_tax_data_2016.csv", sep="\t")
# Backslash t represents a tab
```

Other example
```
# Import pandas with the alias pd
import pandas as pd

# Load TSV using the sep keyword argument to set delimiter
data = pd.read_csv("vt_tax_data_2016.tsv",sep="\t")

# Plot the total number of tax returns by income group
counts = data.groupby("agi_stub").N1.sum()
counts.plot.bar()
plt.show()
```

## 1.2. Modifying flat file imports

We'll look at ways to limit the amount of data imported, and how to make that data easier to work with by naming columns.

Example
```
tax_data = pd.read_csv('us_tax_data_2016.csv')
print(tax_data.shape)

Result: (179796,147)
```

**Limiting Columns**

* Choose columns to load with the `usecols` keyword argument
* Accepts a list of column numbers of names, or a function to filter column names.

```
col_names = ['STATEFIPS', 'STATE', 'zipcode', 'agi_stub', 'N1'] 
col_nums = [0, 1, 2, 3, 4]
# Choose columns to load by name
tax_data_v1 = pd.read_csv('us_tax_data_2016.csv',
                          usecols=col_names) 

# Choose columns to load by number
tax_data_v2 = pd.read_csv('us_tax_data_2016.csv',
                          usecols=col_nums) 
print(tax_data_v1.equals(tax_data_v2))
```

**Limiting Rows**
Limit the number of rows loaded with the `nrows` keyword argument

```
tax_data_first1000 = pd.read_csv('us_tax_data_2016.csv',nrows=1000)
print(tax_data_first1000.shape)

Result: (1000,147)
```
