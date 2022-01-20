# Building Production Pipelines in Airflow

## Templates
- allow substitution of information during a dag run
- adds flexibility for tasks
- uses `Jinja`

#### Templated BashOperator
```python
templated_command="""
    echo "Reading {{ params.filename }} 
"""

t1 = BashOperator(task_id="template_task",
                  bash_command=templated_command,
                  params={'filename': 'file1.txt'},
                  dag=example_dag)
t2 = BashOperator(task_id="template_task",
                  bash_command=templated_command,
                  params={'filename': 'file2.txt'},
                  dag=example_dag)
```

## More templates
Using a for loop with Jinja
```python
templated_command="""
{% for filename in params.filnames %}
    echo "Reading {{ filename }}"
{% endfor %}
"""
t1 = BashOperator(task_id="template_task",
                  bash_command=templated_command,
                  params={"filenames": ["file1.txt", "file2.txt"]},
                  dag=example_dag)
```

_Note: while using for loops can be convenient, there are also benefits to running commands as separate tasks as
it's done above. You can monitor these better and they fail separately_

### Built-in runtime variables
You can use this within templated commands
```
- ds: YYYY-MM-DD
- ds_nodash: YYYYMMDD
- prev_ds: previous execution date, dashed
- prev_ds_nodash: like above but not dashed
- dag: dag object
- conf: airflow config object

{{ variable_goes_here }}
```

### Macros
Provides useful objects and methods, just a few listed here, check docs for more
```
- macros.datetime: the datetime.datetime object
- macros.timedelta
- macros.uuid: python's uuid object
- macros.ds_add('2020-04-15', 5): add five days to the first arg
```

## Branching
Allows for conditional logic in workflows
```python
from airflow.operators.python_operator import BranchPythonOperator

# first you create the python callable
def branch_test(**kwargs):
    if int(kwargs['ds_nodash']) % 2 == 0:
        return 'even_day_task'
    else:
        return 'odd_day_task'

# provide_context tells airflow to provide access to runtime
# variables and macros to the function
branch_task = BranchPythonOperator(task_id='branch_task',
                                   dag=dag,
                                   provide_context=True,
                                   python_callable=branch_test)
# set dependencies
# if you don't set these, all the tasks will run as normal
# regardless of what the branch task returns
start_task >> branch_task >> even_day_task >> even_day_task2
branch_task >> odd_day_task >> odd_day_task2
```
_Pro Tip: always look for simple issues before going to try to heavily modify some part of your code_