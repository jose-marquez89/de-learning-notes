# Model tuning and selection

## What is logistic regression?
- like linear regression
- uses the sigmoid function
- has a probability cutoff point to make it a classifier
- you can tune _hyperparameters_ to improve the performance of the classifier

Initializing an estimator
```python
# Import LogisticRegression
from pyspark.ml.classification import LogisticRegression

# Create a LogisticRegression Estimator
lr = LogisticRegression()
```

### Cross validation
- _k-fold cross validation_ is a variety of cross validation
- helps you estimate performance on unseen data
- works by partitioning the dataset
- data is split up into equally sized hold-out sets
    - every set is used as a test set once
- you can use cross validation to see which hyperparameters perform best

Using `pyspark.ml.evaluation` for cross validation:
```python
# Import the evaluation submodule
import pyspark.ml.evaluation as evals

# Create a BinaryClassificationEvaluator
evaluator = evals.BinaryClassificationEvaluator(metricName="areaUnderROC")
```

Creating a grid of values to search over:
```python
# Import the tuning submodule
import pyspark.ml.tuning as tune

# Create the parameter grid
grid = tune.ParamGridBuilder()

# Add the hyperparameter
grid = grid.addGrid(lr.regParam, np.arange(0, .1, .01))
grid = grid.addGrid(lr.elasticNetParam, [0, 1])

# Build the grid
grid = grid.build()
```

Making the validator:
```python
# Create the CrossValidator
cv = tune.CrossValidator(estimator=lr,
               estimatorParamMaps=grid,
               evaluator=evaluator
               )
```

Fitting the model:
```python
'''In General'''
# Fit cross validation models
models = cv.fit(training)

# Extract the best model
best_lr = models.bestModel

'''Specific to the exercise'''
# Call lr.fit()
best_lr = lr.fit(training)

# Print best_lr
print(best_lr)
```
Evaluating the model:
```
# Use the model to predict the test set
test_results = best_lr.transform(test)

# Evaluate the predictions
print(evaluator.evaluate(test_results))
```
