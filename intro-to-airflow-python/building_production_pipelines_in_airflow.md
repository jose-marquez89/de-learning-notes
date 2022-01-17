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

