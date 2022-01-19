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