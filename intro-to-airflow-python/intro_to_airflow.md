# Intro to Airflow

#### What is Data Engineering?
Data engineering is the process of taking any action involving data and turning it into a reliable, repeatable and maintainable process.

#### What is a workflow?
A set of steps to accomplish a given data engineering task, example:
download sales data -> clean sales data -> run ML pipeline -> upload results to webserver -> generate report and send to ceo

#### What is Airflow?
Platfrom that programs workflows including
- creating
- scheduling 
- monitoring
- workflows are written in python but can be implemented in any language
- workflows are implemented as Directed Acyclic Graphs
- accessed via code, command line or via web interface

#### Other tools
- luigi
- Microsoft SSIS
- Airflow

#### DAGs Intro
Dags are created with details about it such as name, start date, owner. Example:
```
                  task2_a`\
task1 -> task2 -> task2_b -> end
                  task2_c /
```

Example DAG code
```python
etl_dag = DAG(
    dag_id='etl_pipeline',
    default_args={"start_date": "2021-01-08"}
)
```

### Running an Airflow task
A component of an Airflow workflow is called a task.
```shell
# syntax
airflow run <dag_id> <task_id> <start_date>

# command
airflow run example-etl download-file 2020-01-01
```

## Airflow DAGs
What is a dag?
- directed: flow representing dependencies between components
- acyclic: doesn't loop
- graph: the set of components

DAGs are not specific to Airflow

### Defining a DAG
```python
from airflow.models import DAG

from datetime import datetim

default_arguments = {
    'owner': 'jdoe',
    'email': 'jdoe@hotmail.com',
    'start_date': datetime(2020, 1, 20)
}

etl_dag = DAG('etl_workflow', default_args=default_arguments)
```

### DAG on the command line
- use `airflow` on the command line
- `airflow list_dags` to see all recognized DAGs

### CLI vs Python
Use the command line to
- start airflow processes
- manually run DAGs / Tasks
- get logging info

Use python to
- create a DAG
- edit DAG properties

## Airflow web interface
In most cases
- equally powerful in most cases
- web ui is easier
- command line tool may be easier to access depending on settings
