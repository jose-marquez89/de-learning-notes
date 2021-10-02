# Python on the command line
This section was basically about creating and writing python scripts via the command line.

Here's an example of a very short script:
```bash
echo "print('Hello World')" > hello_world.py
```

### Python package installation with pip
It's a good idea to see if the installed pip version is compatible with your python version

```bash
# there should be an obvious match in versions
pip --version
python --version
```
Upgrading pip
```bash
pip install --upgrade pip
```
Print a list of installed packages with `pip list`

Installing an older version of a package
```bash
pip install scikit-learn==0.19.2
```

### Data job automation with cron
List crontab tasks:
```bash
# crontab is the central scheduling file on a unix/linux system
crontab -l
```

You can modify the crontab either with a text editor or you can do this:
```bash
echo "* * * * * python create_model.py" | crontab

# check if the job is properly scheduled
crontab -l
```

### Timing considerations
- cron can run jobs with a frequency of no more than 1 minute
- [reference](https://crontab.guru/) for cron schedules