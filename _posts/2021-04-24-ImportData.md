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

```python
import pandas as pd

tax_data = pd.read_csv("us_tax_data_2016.csv")
tax_data.head(4)
```

**Loading Other Flat Files**

* Specify a different delimeter with `sep`

```python
import pandas as pd

tax_data = pd.read_csv("us_tax_data_2016.csv", sep="\t")
# Backslash t represents a tab
```

Other example
```python
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
```python
tax_data = pd.read_csv('us_tax_data_2016.csv')
print(tax_data.shape)

Result: (179796,147)
```

**Limiting Columns**

* Choose columns to load with the `usecols` keyword argument
* Accepts a list of column numbers of names, or a function to filter column names.

```python
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

```python
tax_data_first1000 = pd.read_csv('us_tax_data_2016.csv',nrows=1000)
print(tax_data_first1000.shape)

Result: (1000,147)
```

* Use `nrows` and `skiprows` together to process a file in chunks
* `skiprows` accepts a list of row numbers, a number of rows, or a function to filter rows
* Set header = None so pandas knows there are no columns names.
* Note that pandas automatically makes the first row imported the header, so if you skip the row with column names, you should also specify that header equals none.

```python
tax_data_next500 = pd.read_csv('us_tax_data_2016.csv',
                                nrows=500, 
                                skiprows=1000, 
                                header=None)

```

**Assigning Column Names**
* To assign column names when there aren't any, we use another read CSV argument: `names`.
* Names takes a list of column names to use. The list **must** include a name for every column in the data
* If you only want to rename some columns, it should be done after import.

```python
col_names = list(tax_data_first1000)
tax_data_next500 = pd.read_csv('us_tax_data_2016.csv',
                               nrows=500, 
                               skiprows=1000, 
                               header=None,
                               names=col_names)
```

**Example:**

Import a subset of columns using `usecols`

```python
# Create list of columns to use
cols = ['zipcode','agi_stub','mars1','MARS2','NUMDEP']

# Create data frame from csv using only selected columns
data = pd.read_csv("vt_tax_data_2016.csv", usecols = cols)

# View counts of dependents and tax returns by income level
print(data.groupby("agi_stub").sum())
```

Import a file in chunks

```python
# Create data frame of next 500 rows with labeled columns
vt_data_next500 = pd.read_csv("vt_tax_data_2016.csv", 
                       			nrows = 500,
                       		  skiprows = 500,
                       		  header= None,
                       		 names=list(vt_data_first500))

# View the Vermont data frames to confirm they're different
print(vt_data_first500.head())
print(vt_data_next500.head())
```

## 1.3. Handling errors and missing data

**Common Flat File Import Issues**
* Column data types are wrong (Incorrect data types)
* Values are missing
* Records that cannot be read by `pandas`

**Specifying data types**
`pandas` can automatically find out the data types by using `.dtypes`

```python
print(tax_data.dtype)
```
<img src="/assets/images/20210424_ImportData/pic3.png" class="largepic"/>

Checking data types in the tax data, we see that pandas interpreted ZIP codes as integers. They're more accurately modeled as strings. Instead of letting pandas guess, we can set the data type of any or all columns with read CSV's dtype keyword argument. Dtype takes a dictionary, where each key is a column name and each value is the data type that column should be. Note that non-standard data types, like pandas categories, must be passed in quotations. Here, we specify that the zipcode column should contain strings, leaving pandas to infer the other columns. Printing the new data frame's dtypes, we see that zipcode's dtype is "object", which is the pandas counterpart to Python strings.

```python
tax_data = pd.read_csv("us_tax_data_2016.csv", 
                        dtype={"zipcode": str})
print(tax_data.dtypes)
```

<img src="/assets/images/20210424_ImportData/pic4.png" class="largepic"/>

**Customizing Missing Data Values**
* `pandas` automatically interprets some value as missing or NA

```python
printn(tax_data.head())
```
<img src="/assets/images/20210424_ImportData/pic5.png" class="largepic"/>

Sometimes missing values are represented in ways that pandas won't catch, such as with dummy codes. In the tax data, records were sorted so that the first few have the ZIP code 0, which is not a valid code and should be treated as missing.

We can tell pandas to consider these missing data with the `na_values` keyword argument. `na_values` accepts either a single value, a list of values, or a dictionary of columns and values in that column to treat as missing.

```python
tax_data = pd.read_csv("us_tax_data_2016.csv",
                        na_values={"zipcode" : 0}) 
print(tax_data[tax_data.zipcode.isna()])
```

<img src="/assets/images/20210424_ImportData/pic6.png" class="largepic"/>

**Lines with Errors**

One last issue you may face are lines that panndas just can parse. For example, a record could have more values than there are columns, like the second record in this corrupted version of the tax data. Let's try reading it. 

<img src="/assets/images/20210424_ImportData/pic7.png" class="largepic"/>


By default, trying to load this file results in a long error, and no data is imported.

```python
tax_data = pd.readcsv("...")
```

<img src="/assets/images/20210424_ImportData/pic8.png" class="largepic"/>

Luckily, we can change this behavior with two arguments, `error_bad_lines` and `warn_bad_lines`. Both take Boolean, or true/false values. 
* Setting `error_bad_lines=False` makes pandas skip bad lines and load the rest of the data, instead of throwing an error. 
* `warn_bad_lines=True` tells pandas whether to display messages when unparseable lines are skipped. 

```python
tax_data = pd.read_csv("us_tax_data_2016_corrupt.csv",
                        error_bad_lines=False, 
                        warn_bad_lines=True)

```
<img src="/assets/images/20210424_ImportData/pic9.png" class="largepic"/>

Example:

Specify data types
```python
# Create dict specifying data types for agi_stub and zipcode
data_types = {"agi_stub":"category",
			"zipcode":str}

# Load csv using dtype to set correct data types
data = pd.read_csv("vt_tax_data_2016.csv",dtype=data_types)

# Print data types of resulting frame
print(data.dtypes.head())
```

Set custom NA values

```python
# Create dict specifying that 0s in zipcode are NA values
null_values = {"zipcode" : 0}

# Load csv using na_values keyword argument
data = pd.read_csv("vt_tax_data_2016.csv", 
                   na_values=null_values)

# View rows with NA ZIP codes
print(data[data.zipcode.isna()])
```

Skip bad data
```python
try:
  # Set warn_bad_lines to issue warnings about bad records
  data = pd.read_csv("vt_tax_data_2016_corrupt.csv", 
                     error_bad_lines=False, 
                     warn_bad_lines=True)
  
  # View first 5 records
  print(data.head())
  
except pd.io.common.CParserError:
    print("Your data contained rows that could not be parsed.")
```

# 2. Importing Data From Excel Files

Build pipelines to data stored in spreadsheets, plus additional data wrangling techniques. Import part or all of a workbook and ensure boolean and datetime data are properly loaded.

## 2.1 Introduction to spreadsheets

**Spreadsheets**
* Also known as Excel files
* Data stored in tabular form, with cells arranged in rows and columns. 
* Unlike flat files, can have **formatting** (Note that pandas does not import spreadsheet formatting) and **formulas** (automatically updating results)
* Multiple spreadsheets can exist in a workbook

**Loading spreadsheets**
Spreadsheets have their own loading function in pandas : `read_excel()`

```python
import pandas as pd
# Read the Excel file
survey_data = pd.read_excel("fcc_survey.xlsx")

# View the first 5 lines of data
print(survey_data.head())
```

**Loading Select Columns and Rows** 
* read_excel has many key word argument in common with read_csv()
  * nrows: limit number of rows to load
  * skiprows: specify number of rows or row numbers to skip
  * choose columns by name, positional number, or letter (e.g. "A:P")

<img src="/assets/images/20210424_ImportData/pic10.png" class="largepic"/>

```python
# Read columns W-AB and AR of file, skipping metadata header 
survey_data = pd.read_excel("fcc_survey_with_headers.xlsx",
                            skiprows=2,
                            usecols="W:AB, AR")

# View data
print(survey_data.head())
```

<img src="/assets/images/20210424_ImportData/pic11.png" class="largepic"/>
                           
**Example**

Get data from a spreadsheet
```python
# Load pandas as pd
import pandas as pd
# Read spreadsheet and assign it to survey_responses
survey_responses = pd.read_excel("fcc_survey.xlsx")

# View the head of the data frame
print(survey_responses.head())
```
Load a portion of a spreadsheet
```python
# Create string of lettered columns to load
col_string = "AD,AW:BA"

# Load data with skiprows and usecols set
survey_responses = pd.read_excel("fcc_survey_headers.xlsx", 
                        skiprows = 2, 
                        usecols=col_string)

# View the names of the columns selected
print(survey_responses.columns)
```

## 2.2. Getting data from multiple worksheets

**Select sheets to load**
* `read_excel()` loads the first sheet in an Excel file by default
* Use the `sheet_name` keyword argument to load other sheets
* Specify spreadsheets by name and/or (zero-indexed) position number 
* Pass a list of names/numbers to load more than one sheet at a time
* If you load multiple sheets at once, any other arguments passed to read Excel apply to all sheets. For example, if you set nrows to 50, the first 50 rows of each sheet listed in sheet name will be loaded. If sheets need different parameters, load them with separate read Excel calls.

```python
# Get the second sheet by position index 
survey_data_sheet2 = pd.read_excel('fcc_survey.xlsx',
                                    sheet_name=1)

# Get the second sheet by name
survey_data_2017 = pd.read_excel('fcc_survey.xlsx',
                                sheet_name='2017')

# Test if they have the same value, the result is True
print(survey_data_sheet2.equals(survey_data_2017))
```

**Loading All Sheets**
* Passing `sheet_name=None` to read_excel() reads all sheets in a workbook

```python
survey_responses = pd.read_excel("fcc_survey.xlsx",sheet_name=None)
print(type(survey_responses))
```
<img src="/assets/images/20210424_ImportData/pic12.png" class="largepic"/>

Instead of getting a data frame, we got an ordered dictionary! Iterating through the items reveals that the keys are sheet names, and the values are data frames for each sheet.

```python
for key, value in survey_responses.items(): 
print(key, type(value))
```

<img src="/assets/images/20210424_ImportData/pic13.png" class="largepic"/>

**Combine sheets**

Combine sheet when:
* The data is not duplicated across tabs
* The columns are all named the same
* You wish to read in all tabs and combine them

First, we create an empty data frame, all responses. Then, we loop through the items in the survey responses dictionary. Remember, each value is a data frame corresponding to a worksheet, and each key is a sheet name. For each data frame, we add a column, Year, containing the sheet name, so we know which dataset a record came from. Finally, we append the data frames to all responses. We can check unique values in the year column to confirm that all responses has both years. In this case, we only combined two sheets, so we could have appended one to the other, but looping has the advantage of working for any number of sheets.

```python
# Create empty data frame to hold all loaded sheets 
all_responses = pd.DataFrame()

# Iterate through data frames in dictionary
for sheet_name, frame in survey_responses.items():
  # Add a column so we know which year data is from 
  frame["Year"] = sheet_name

  # Add each data frame to all_responses 
  all_responses = all_responses.append(frame)

# View years in data 
print(all_responses.Year.unique())
```
<img src="/assets/images/20210424_ImportData/pic14.png" class="largepic"/>

**Reminder example**
Select a single sheet

```python
# Create df from second worksheet by referencing its position
responses_2017 = pd.read_excel("fcc_survey.xlsx",
                               sheet_name=1)

# Create df from second worksheet by referencing its name
responses_2017 = pd.read_excel("fcc_survey.xlsx",
                               sheet_name="2017")

# Graph where people would like to get a developer job
job_prefs = responses_2017.groupby("JobPref").JobPref.count()
job_prefs.plot.barh()
plt.show()                              
```

Select multiple sheets

```python
# Load both the 2016 and 2017 sheets by name
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=['2016', '2017'])

# Load all sheets in the Excel file
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name = [0,"2017"])

# Load all sheets in the Excel file
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name = None)

# View the sheet names in all_survey_data
print(all_survey_data.keys())
```


* Create an empty data frame, all_responses.
* Set up a for loop to iterate through the values in the responses dictionary.
* Append each data frame to all_responses and reassign the result to the same variable name.

```python
# Create an empty data frame
all_responses = pd.DataFrame()

# Set up for loop to iterate through values in responses
for df in responses.values():
  # Print the number of rows being added
  print("Adding {} rows".format(df.shape[0]))
  # Append df to all_responses, assign result
  all_responses = all_responses.append(df)

# Graph employment statuses in sample
counts = all_responses.groupby("EmploymentStatus").EmploymentStatus.count()
counts.plot.barh()
plt.show()
```
# 2.3. Modifying imports: true/false data

A Boolean variable has only two possible values: True or False, which makes them convenient for tasks like filtering. Despite this simplicity, Booleans can be tricky. True and false are represented in a few ways for demonstration purposes:
* 1 and 0, which are common among people with coding experience,
* Trues and Falses
* Yes and No, which tend to appear in surveys and forms.

<img src="/assets/images/20210424_ImportData/pic15.png" class="largepic"/>

**pandas and Boolean**

```python
bootcamp_data = pd.read_excel("fcc_survey_booleans.xlsx") 
print(bootcamp_data.dtypes)
```
pandas interpreted no columns as Boolean! Even True/False columns were loaded as floats

<img src="/assets/images/20210424_ImportData/pic16.png" class="largepic"/>


```python
# Count True values 
print(bootcamp_data.sum())
```
<img src="/assets/images/20210424_ImportData/pic17.png" class="largepic"/>
```python
# Count NAs 
print(bootcamp_data.isna().sum())
```
<img src="/assets/images/20210424_ImportData/pic18.png" class="largepic"/>
38 attended a bootcamp and 14 took out a loan for it. Let's also check how many values in each column are missing by summing the results of is NA. Every record has a value for bootcamp attendance, but most of the loan values are blank, even for some students who attended a bootcamp.
```python
# Load data, casting True/False columns as Boolean 
bool_data = pd.read_excel("fcc_survey_booleans.xlsx",
                          dtype={"AttendedBootcamp": bool, 
                                "AttendedBootCampYesNo": bool, 
                                "AttendedBootcampTF":bool, 
                                "BootcampLoan": bool, 
                                "LoanYesNo": bool,
                                "LoanTF": bool})
print(bool_data.dtypes)
```

<img src="/assets/images/20210424_ImportData/pic19.png" class="largepic"/>

Check again

<img src="/assets/images/20210424_ImportData/pic20.png" class="largepic"/>

Checking counts of True values reveals issues. The loan columns have too many Trues, and the yes/no ones are all True. Checking NA values by column, we see there aren't any.

*`pandas` automatically loads True/False columns as floats, but that can be changed with `dtype`. 
* Boolean values must be either True or False, NAs were re-coded as True
* `pandas` recognized that 1 and 0 are False and True
* It did not know what to do with Yes and No, so they were all coded as True.

