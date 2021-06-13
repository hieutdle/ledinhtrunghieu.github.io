---
layout: post
author: ledinhtrunghieu
title: Lesson 11 - Introduction to Airflow in Python
---

# 1. Introduction to Airflow

Introduction to the components of Apache Airflow and learn how and why we should use them.

## 1.1. Running a task in Airflow

**Data Engineering**: Taking any action involving data and turning it into a **reliable, repeatable, and maintainable** process.

A **work flow is**: 
* A set of steps to accomplish a given data engineering task. Such as: downloading files, copying data, filtering information, writing to a database, etc.
* The complexity of a workflow is completely dependent on the needs of the user
* A term with various meaning depending on context

<img src="/assets/images/20210502_AirflowPython/pic1.png" class="largepic"/>

**Airflow**: 

<img src="/assets/images/20210502_AirflowPython/pic2.png" class="largepic"/>


* A platform to program workflows (general), including the creation, scheduling, and monitoring of said workflows.
* Airflow can use various tools and languages, but the actual workflow code is written with Python.
* Airflow implements workflows as DAGs, or Directed Acyclic Graphs. 
* Airflow can be accessed and controlled via code, via the command-line, or via a built-in web interface. 

**Other workflow**
* Luigi
* SSIS
* Bash Scripting

**Directed Acyclic Graph**

<img src="/assets/images/20210502_AirflowPython/pic3.png" class="largepic"/>

* In Airflow, this represents the set of tasks that make up your workflow. 
* It consists of the tasks and the dependencies between tasks. 
* DAGs are created with various details about the DAG, including the name, start date, owner, email alerting options, etc.

```python
    etl_dag = DAG( dag_id='etl_pipeline',
                   default_args={"start_date": "2020-01-08"}
)
```

**Running a workflow in Airflow**
Running a simple Airflow task
```
airflow run <dag_id> <task_id> <start_date>

airflow run eexample-etl download-file 2020-01-10
```

## 1.2. Airflow DAGs

**Directed Acyclic Graph**:
* **Directed**, meaning there is an inherent flow representing the dependencies or order between execution of components. These dependencies (even implicit ones) provide context to the tools on how to order the running of components
* **Acyclic** - it does not loop or repeat. This does not imply that the entire DAG cannot be rerun, only that the individual components are executed once per run
* **Graph** represents the components and the relationships (or dependencies) between them. 

**DAG in Airflow**
* Are written in Python
* Are made up of components (typically tasks) to be executed, such as operators, sensors, etc.  Typically refers to these as tasks.
* Airflow DAGs contain dependencies that are defined, either explicitly or implicitly. These dependencies define the execution order so Airflow knows which components should be run at what point within the workflow. For example, you would likely want to copy a file to a server prior to trying to import it to a database.

**Define a DAG**
```python
from airflow.models import DAG

from datetime import datetime 
default_arguments = {
    'owner': 'jdoe',
    'email': 'jdoe@datacamp.com', 
    'start_date': datetime(2020, 1, 20)
}

etl_dag = DAG( 'etl_workflow', default_args=default_arguments )
```
The **`start_date`** represents the earliest datetime that a DAG could be run.

**DAGs on the command line**
* The `airflow` command line program contains many subcommands. 
* `airflow -h` for descriptions
* `airflow list_dags` to show all recognized DAGs


**Use python to**
* Create a DAG
* Edit the individual properties of a DAG

**Use the command line tool to**
* Start Airflow processes
* Manually run DAGs/task
* Get logging information from Airflow


```python
# Import the DAG object
from airflow.models import DAG

# Define the default_args dictionary
default_args = {
  'owner': 'dsmith',
  'start_date': datetime(2020, 1, 14),
  'retries': 2
}

# Instantiate the DAG object to a variable called etl_dag with a DAG named example_etl.
etl_dag = DAG('example_etl', default_args=default_args)
```

## 1.3. Airflow web interface

<img src="/assets/images/20210502_AirflowPython/pic4.png" class="largepic"/>

<img src="/assets/images/20210502_AirflowPython/pic5.png" class="largepic"/>

Starting the Airflow webserver

```
airflow webserver -p 9090
```

Remember that the Airflow UI allows various methods to view the state of DAGs. The `Tree View` lists the tasks and any ordering between them in a tree structure, with the ability to compress / expand the nodes. The `Graph View` shows any tasks and their dependencies in a graph structure, along with the ability to access further details about task runs. The Code view provides full access to the Python code that makes up the DAG.


# 2. Implementing Airflow DAGs

Learn the basics of implementing Airflow DAGs, how to set up and deploy operators, tasks, and scheduling.

## 2.1. Airflow operators

**Operator**
* Airflow operators represent a single task in a workflow. This can be any type of task from running a command, sending an email, running a Python script,...
* Airflow operators run independently - meaning that all resources needed to complete the task are contained within the operator.
* Airflow operators do not share information between each other. This is to simplify workflows and allow Airflow to run the tasks in the most efficient manner. It is possible to share information between operators.
* Airflow contains many various operators to perform different tasks. For example, the DummyOperator can be used to represent a task for troubleshooting or a task that has not yet been implemented
```python
DummyOperator(task_id='example', dag=dag)
```

**BashOperator**
```bash
BashOperator(
task_id='bash_example', bash_command='echo "Example!"', dag=ml_dag)
```
```bash
BashOperator(
task_id='bash_script_example', bash_command='runcleanup.sh', dag=ml_dag)
```
* Executes a given Bash command or script.
* Runs the command in a temporary directory.
* Can specify environment variables for the command.

**Example**
```py
from airflow.operators.bash_operator import BashOperator 

example_task = BashOperator(task_id='bash_ex',
                            bash_command='echo 1', 
                            dag=dag)

bash_task = BashOperator(task_id='clean_addresses', 
                         bash_command='cat addresses.txt | awk "NF==10" > cleaned.txt', 
                         dag=dag)
```

**Note**
* Individual operators are not guaranteed to run in the same location or environment. This means that just because one operator ran in a given directory with a certain setup, it does not necessarily mean that the next operator will have access to that same information.
* You may need to set up environment variables, especially for the BashOperator. For example, it's common in bash to use the tilde character to represent a home directory. This is not defined by default in Airflow. Another example of an environment variable could be AWS credentials, database connectivity details, or other information specific to running a script.
* It can also be tricky to run tasks with any form of elevated privilege. This means that any access to resources must be setup for the specific user running the tasks. If you're uncertain what elevated privileges are, think of running a command as root or the administrator on a system.

## 2.2. Airflow tasks

**Tasks** are:
* Instances of operators
* Usually assigned to a variable in Python
```
example_task = BashOperator(task_id='bash_example',bash_command='echo "Example!"', dag=dag)
```
* Referred to by the task_id within the Airflow tools

**Task dependencies**
* Define a given order of task completion
* Are not required for a given workflow, but usually present in most 
* Are referred to as *upstream* or *downstream* tasks. An upstream task means that it must complete prior to any downstream tasks.
* In Air ow 1.8 and later, are defined using the bitshift operators
  * **>>** or the upstream operator
  * **<<** or the downstream operator
* Upstream means before and downstream means after. This means that any upstream tasks would need to complete prior to any downstream ones.

**Example**

```python
 Define the tasks
task1 = BashOperator(task_id='first_task',
                     bash_command='echo 1', 
                     dag=example_dag)

task2 = BashOperator(task_id='second_task',
                     bash_command='echo 2', 
                     dag=example_dag)

# Set first_task to run before second_task 
task1 >> task2	# or task2 << task1
```

**Task dependencies in the Airflow UI**

<img src="/assets/images/20210502_AirflowPython/pic6.png" class="largepic"/>

**Multiple dependencies**

**Chained dependencies**

```py
task1 >> task2 >> task3 >> task4
```
<img src="/assets/images/20210502_AirflowPython/pic7.png" class="largepic"/>

**Mixed dependencies**

```py
task1 >> task2 << task3
```

```py
task1	>>	task2
task3	>>	task2
```
<img src="/assets/images/20210502_AirflowPython/pic8.png" class="largepic"/>

## 2.3. Additional operators

**Python Operators**
* Executes a Python function / callable
* Operates similarly to the BashOperator, with more options 
* Can pass in arguments to the Python code

```py
from airflow.operators.python_operator import PythonOperator

def printme():
    print("This goes in the logs!") 
python_task = PythonOperator(
    task_id='simple_print', 
    python_callable=printme, 
    dag=example_dag
)
```

**Arguments**
* Supports arguments to tasks
  * Positional
  * Keyword
* Use the `op_kwargs` dictionary

**op_kwargs Example**
```py
def sleep(length_of_time): 
    time.sleep(length_of_time)

sleep_task = PythonOperator( 
    task_id='sleep', 
    python_callable=sleep, 
    op_kwargs={'length_of_time': 5} 
    dag=example_dag
)
```

We'll add our `op_kwargs` dictionary with the length of time variable and the value of 5. Note that the dictionary key must match the name of the function argument


**Email Operator**
* Found in the `airflow.operators` library
* Send an email 
* Can contain typical comments
  * HTML content
  * Attachments
* Does require the Airflow system to be configured with email server details

```py
from airflow.operators.email_operator import EmailOperator

email_task = EmailOperator( 
    task_id='email_sales_report', 
    to='sales_manager@example.com', 
    subject='Automated Sales Report',
    html_content='Attached is the latest sales report', 
    files='latest_sales.xlsx',
    dag=example_dag
)
```

**Using the PythonOperator Practice**
```py
def pull_file(URL, savepath):
    r = requests.get(URL)
    with open(savepath, 'wb') as f:
        f.write(r.content)   
    # Use the print method for logging
    print(f"File pulled from {URL} and saved to {savepath}")

from airflow.operators.python_operator import PythonOperator

# Create the task
pull_file_task = PythonOperator(
    task_id='pull_file',
    # Add the callable
    python_callable=pull_file,
    # Define the arguments
    op_kwargs={'URL':'http://dataserver/sales.json', 'savepath':'latestsales.json'},
    dag=process_sales_dag
)
```

**Email Operators**
```py
# Import the Operator
from airflow.operators.email_operator import EmailOperator


# Define the task
email_manager_task = EmailOperator(
    task_id='email_manager',
    to='manager@datacamp.com',
    subject='Latest sales JSON',
    html_content='Attached is the latest sales JSON file as requested.',
    files='parsedfile.json',
    dag=process_sales_dag
)

# Set the order of tasks
pull_file_task >> parse_file_task >> email_manager_task
```

## 2.4. Airflow scheduling

**DAG Runs**
* A specific instance of a work ow at a point in time 
* Can be run manually or via `schedule_interval`
* Maintain state for each workflow and the tasks within
  * `running`
  * `failed`
  * `sucess`

**DAG Runs View**
<img src="/assets/images/20210502_AirflowPython/pic9.png" class="largepic"/>


**Schedule details**
When scheduling a DAG, there are several attributes of note:
* `start_date` - The date / time to initially schedule the DAG run
* `end_date` - Optional attribute for when to stop running new DAG instances
* `max_tries` - Optional attribute for how many times to retry before fully failing the DAG run
* `schedule_interval` - How often to schedule the DAG for execution

**Schedule interval**
* How often to schedule the DAG
* Occurs between the `start_date` and the potential `end_date`
* Can be defined with a `cron` style syntax or via built-in presets.

**cron syntax**
<img src="/assets/images/20210502_AirflowPython/pic10.png" class="largepic"/>

* Is pulled from the Unix cron format 
* Consists of 5 fields separated by a space. Starting with the minute value (0 through 59), the hour (0 through 23), the day of the month (1 through 31), the month (1 through 12), and the day of week (0 through 6).
* An asterisk `*` represents running for every interval (ie, every minute, every day, etc)
* Can be comma separated values in fields for a list of values. For example, an asterisk in the minute field means run every minute
* A list of values can be given on a field via comma separated values.

**cron example**
```
0 12 * * *                  # Run daily at noon

* * 25 2 *                  # Run once per minute on February 25

0,15,30,45 * * * *          # Run every 15 minutes
```


**Airflow scheduler presets**
Preset:
* @hourly : `0 * * * *`
* @daily : `0 0 * * *`
* @weekly: `0 0 * * 0`
* @monthly: `0 0 1 * *`
* @yearly: `0 0 1 1 *`

**Special present**
* `None`- Don't schedule ever, used for manually triggered DAGs
* `@once` - Schedule only once

**schedule_interval issues**
When scheduling a DAG, Airflow will:
* Use the `start_date` as the earliest possible value
* Schedule the task at `start_date` + `schedule_interval`
```py
'start_date': datetime(2020, 2, 25), 
'schedule_interval': @daily
```
This means the earliest starting time to run the DAG is on February 26th, 2020

```py
# Update the scheduling arguments as defined
default_args = {
  'owner': 'Engineering',
  'start_date': datetime(2019,  11, 1),
  'email': ['airflowresults@datacamp.com'],
  'email_on_failure': False,
  'email_on_retry': False,
  'retries': 3,
  'retry_delay': timedelta(minutes=20)
}

dag = DAG('update_dataflows', default_args=default_args, schedule_interval='30 12 * * 3')
```

# 3. Maintaining and monitoring Airflow workflows

Save time using Airflow components such as sensors and executors while monitoring and troubleshooting Airflow workflows.

# 3.1. Airflow sensors

**Sensor**
* An operator that waits for a certain condition to be true
  * Creation of a file
  * Upload of a database record
  * Certain response from a web request
* Can define how often to check for the condition to be true
* Are assigned to tasks

**Sensor details**
* Derived from `airflow.sensors.base_sensor_operator`
* Sensor arguments:
* `mode`- How to check for the condition
  * `mode='poke'` - The default is poke, and means to continue checking until complete without giving up a worker slot.
  * `mode='reschedule'` - Give up task slot and try again later
* `poke_interval` -	How often to wait between checks. 
* `timeout` - How long to wait before failing task
* Also includes normal operator attributes: `task_id` and `dag`

**File Sensor**
* Is part of the `airflow.contrib.sensors` library
* Checks for the existence of a file at a certain location
* Can also check if any files exist within a directory

```py
from airflow.contrib.sensors.file_sensor import FileSensor

file_sensor_task = FileSensor(task_id='file_sense',
                              filepath='salesdata.csv', 
                              poke_interval=300, 
                              dag=sales_report_dag)

init_sales_cleanup >> file_sensor_task >> generate_report

```
**Other sensors**
* `ExternalTaskSensor` - wait for a task in another DAG to complete
* `HttpSensor` - - Request a web URL and check for content
* `SqlSensor` - Runs a SQL query to check for content
* Many others in `airflow.sensors` and `airflow.contrib.sensors`

**Sensors vs Operators**
Use a sensor when:
* Uncertain when conditions will be true
* If you want to continue to check for a condition but not necessarily fail the entire DAG immediately.
* If you want to repeatedly run a check without adding cycles to your DAG, sensors are a good choice.

# 3.2. Airflow executors




# 4. Building production pipelines in Airflow



# 5. Reference

1. [Introduction to Airflow- DataCamp](https://learn.datacamp.com/courses/introduction-to-airflow-in-python)
