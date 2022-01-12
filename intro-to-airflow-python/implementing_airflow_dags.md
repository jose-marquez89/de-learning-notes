# Implementing Airflow DAGs

## Airflow Operators
Operators:
- represent a single task in a workflow
- run independently
- typically don't share information
- there are various operators to perform different tasks

Example:
```python
DummyOperator(task_id='example', dag=dag)
```

### Bash operator
Executes pretty much anything bash is capable of in a given workflow
- runs the command in a temporary directory
- environment variables can be specified for the command
```python
BashOperator(
    task_id='bash_example',
    bash_command='echo "Example!"',
    dag=ml_dag
)
```
```python
BashOperator(
    task_id="bash_script_example",
    bash_command="run_cleanup.sh",
    dag=ml_dag
)
```

### Bash operator examples
```python
from airflow.operators.bash_operator import BashOperator
example_task = BashOperator(task_id="bash_ex",
                            bash_command="echo 1",
                            dag=dag)
```
```python
bash_task = BashOperator(task_id='clean_address', 
                         bash_command='cat addresses.txt | awk "NF==10" > cleaned.txt')
```

### Operator gotchas
- operators are not guaranteed to run in the same directory
    - it seems like this has to be explicitly set up 
- operators may require extensive use of environment variables
    - things like the home directory (`~`) are not explicitly defined in Airflow
- can be difficult to run tasks with elevated priveleges

## Airflow tasks
A task is an instance of an operator. We refer to it by its task id (see below)
```python
example_task = BashOperator(task_id="bash_ex",
                            bash_command="echo 1",
                            dag=dag)
```                            

### Task dependencies
- this is how you define a given order of task completion
- not required for a workflow but defined in most
- you can have _upstream_ and _downstream_ tasks
    - _upstream_ is **before**
    - _downstream_ is **after**
- from Airflow 1.8, these are defined using bitshift operators:
    - `>>`, the upstream operator
    - `<<`, the downstream operator

Example
```python
# Define the tasks
task1 = BashOperator(task_id='first_task',
                     bash_command="echo 1",
                     dag=example_dag)

task2 = BashOperator(task_id='second_task',
                     bash_command="echo 2",
                     dag=example_dag)

# set task1 to run before task 2                
# "task1 *before* task2"
task1 >> task2 # or task2 << task1
```

Multiple dependencies
```python
# chained
task1 >> task2 >> task3 >> task4

# mixed, task 1 and 3 run before task 2
# no particular order for task1 and task3
task1 >> task2 << task3

# alternativel for mixed
task1 >> task2
task3 >> task2
```
