# Maintaining and monitoring Airflow workflows


## Airflow Sensors
Sensors wait for a condition to be true
- creation of a file
- upload of a database record
- a certain response of a web request
- you can define how often to check
- get assigned to airflow tasks

### Sensor details
- `airflow.sensors.base_sensor_operator`
- arguments
    - `mode`: how to check for a condition
        - `mode='poke'`: default, run repeatedly
        - `mode='reschedule`: give up task slot and try again later
    - `poke_interval`: how often to wait between checks
    - `timeout`: how long to wait before determining that a task has failed
    - includes normal operator attributes

#### File sensor
- `airflow.contrib.sensors`
- check for the existence of a file in certain location
- can also check for the existence of file in a particular directory

Example
```python
from airflow.contrib.sensors.file_sensor import FileSensor

file_sensor_task = FileSensor(task_id="file_sense",
                              filepath='salesdata.csv',
                              poke_interval=300,
                              dag=sales_report_dag)
init_sales_cleanup >> file_sensor_task >> generate_report
```

### Other sensors
- `ExternalTaskSensor`: wait for a task in another dag to complete
- `HttpSensor`: request a web URL and check for content 
- `SqlSensor`: runs a sql query to check for content
- many other sensors in `airflow.sensors` and `airflow.contrib.sensors` libraries

### Why sensors
use a sensor when
- uncertain if some condition will be true
- if failure is not immediately desired
- to add task repetition without loops

## Airflow executors

What are executors?
- are the actual component that runs tasks
- different executors run tasks differently
- example executors
    - `SequentialExecutor`
    - `LocalExecutor`
    - `CeleryExector` 

### Sequential executor
- default executor
- one task at a time
- useful for debugging and learning
- not really recommended for production

### Local executor
- Runs on a single system
- treats tasks as processes
- can run tasks concurrently
    - this is limited by the system resources
- _parallelism_ is defined by the user
- can use all the resources of a given host system
- good choice for a single production airflow system

### Celery exector
- uses a Celery backend as a task manager
    - queuing system written in python that allows multiple systems to communicate as basic cluster
- multiple worker systems can be defined
- you can add more systems at any time to balance out workflows
- significantly more difficult to configure
- requires a working Celery configuration prior to configuring Airflow
    - also requires some methods to share DAGs between systems
    - git server, network file system, etc
- extremely powerful for method for organizations with extensive workflows

### Determine your executor
- via the `airflow.cfg` file
    - `cat airflow/airflow.cfg | grep "executor = "`
- you can also use `airflow list_dags` on the command line
    - `INFO - Using SequentialExecutor`

## Debugging and troubleshooting in Airflow

### Typical issues
- dag won't run on schedule
- dag won't load
- syntax errors

#### Dag won't run on schedule
- check if the scheduler is running
- you can run `airflow scheduler` on the scheduler
- at least one `schedule_interval` has not passed
    - modify the attributes to fit requirements
- not enough tasks free within the executor to run
    - change the executor type
    - add system resources
    - add more systems
    - change dag scheduling

#### Dag won't load
- dag not in the web UI
- dag not in `airflow list_dags`

Possible solutions
- make sure the dag is in the right folder
- determine the dag's folder via `airflow.cfg`
    - the folder needs to be an absolute path

#### Syntax errors
- one of the most common reasons a dag won't appear
- sometimes these are difficult to find
- two quick methods
    - `airflow list_dags`: this will show error output if something is unable to execute properly
    - `python 3 <dagfile>`: will only help if there are syntax errors

## SLAs and Reporting in Airflow
- _Service Level Agreement_
- in airflow, this is the amount of time a task or DAG should require to run
- an _SLA miss_ is exactly what it sounds like, the task or dag went over time
- if an SLA is missed and email is properly configured for airflow, an email will be sent out
    - a log is also stored
- you can see SLA misses in the web UI
    - browse > SLA misses

### Defining an SLA
Method 1
```python
task1 = BashOperator(task_id='sla_task',
                     bash_command='runcode.sh',
                     sla=timedelta(seconds=30),
                     dag=dag)
```

Method 2: Default args passed into dag
```python
# remember to import timedelta from the datetime library
default_args = {
    'sla': timedelta(minutes=20),
    'start_date': datetime(2020, 2, 20)
}

dag = DAG('sla_dag', default_args=default_args)
```

### General reporting
- options for success / failure / error
- keys in the default_args dict

```python
default_args = {
    'email': ['someonesemail@company.net'],
    'email_on_failure': True,
    'email_on_retry': False,
    'email_on_success': True
}
```


