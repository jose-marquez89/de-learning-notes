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

## Additional operators

### Python operator
```python
from airflow.operators.python_operator import PythonOperator

def printme():
    print("This goes in the logs!")

python_task = PythonOperator(
    task_id="simple_print",
    python_callable=printme,
    dag=example_dag
)
```

### Passing arguments to functions from task definition
`op_kwargs`:
```python
def sleep(length_of_time):
    time.sleep(length_of_time)

sleep_task = PythonOperator(
    task_id='sleep',
    python_callable=sleep,
    op_kwargs={'length_of_time': 5}, # make sure arg names match function def
    dag=example_dag
)
```

### EmailOperator
Airflow server details need to be configured with an email server to successfully send emails
```python
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

## Airflow Scheduling

### Dag runs
- you can run dags manually or with `schedule_interval`
- each workflow and tasks within maintain a state
    - running
    - failed
    - success
- on the UI you can go to Browse > Dag runs to see states

### Schedule details
Attributes
- `start_date`: initial dag run start
- `end_date`: when to stop the dag's runs
- `max_tries`: optional, how many times to try
- `schedule_interval`: how many times to run
    - uses standard cron syntax
    - runs between `start_date` and `end_date`

### Scheduler presets
- @hourly
- @daily
- @monthly
- @yearly

Two special `schedule_interval` presets
- `None`: don't schedule for manually triggered workflows
- @once: only schedule once

#### Scheduler considerations
- airflow uses `start_date` as the earliest possible value
- dag will not run until at least one schedule interval beyond the start date has passed

Example
```json
'start_date': datetime(2020, 2, 25),
'schedule_interval: @daily
```
Earliest DAG run will be Feb 26, 2020

#### Schedule example
```python
# Update the scheduling arguments as defined
default_args = {
  'owner': 'Engineering',
  'start_date': datetime(2019, 11, 1),
  'email': ['airflowresults@datacamp.com'],
  'email_on_failure': False,
  'email_on_retry': False,
  'retries': 3,
  'retry_delay': timedelta(minutes=20)
}

dag = DAG('update_dataflows', default_args=default_args, schedule_interval='30 12 * * 3')
```

## Airflow Sensors
