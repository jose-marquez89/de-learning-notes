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




