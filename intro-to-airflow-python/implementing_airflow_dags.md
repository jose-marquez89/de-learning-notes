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