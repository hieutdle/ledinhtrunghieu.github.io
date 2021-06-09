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




# 5. Reference

1. [Introduction to Airflow- DataCamp](https://learn.datacamp.com/courses/introduction-to-airflow-in-python)
