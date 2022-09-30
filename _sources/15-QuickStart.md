# 15-Quick Start



- MLflow is an open source platform for managing the end-to-end machine learning lifecycle. 
- 3 primary components: Tracking, Models, and Projects. 
- The MLflow Tracking component lets you log and query machine model training sessions (runs) using:
    - `Java`
    - `Python`
    - `R`
    - `REST APIs`
- An MLflow run is a collection of parameters, metrics, tags, and artifacts associated with a machine learning model training process.

- Experiments are the primary unit of organization in MLflow; all MLflow runs belong to an experiment. - Each experiment lets you visualize, search, and compare runs, as well as download run artifacts or metadata for analysis in other tools.
- Experiments are maintained in a Databricks hosted MLflow tracking server.

Experiments are located in the Navigate the workspace file tree. You manage experiments using the same tools you use to manage other workspace objects such as folders, notebooks, and libraries.

## Quick Start with Python

### Installing
With Databricks, select `Databricks Runtime for Machine Learning`. MLflow is already installed.

### Automatically log training runs to MLflow
With `Databricks Runtime 10.3 ML` and above, Databricks Autologging is enabled by default and automatically captures model parameters, metrics, files, and lineage information when you train models from a variety of popular machine learning libraries.

#### TensorFlow
```
# Also autoinstruments tf.keras
import mlflow.tensorflow
mlflow.tensorflow.autolog()
```

#### Scikit-learn
```
import mlflow.sklearn
mlflow.sklearn.autolog()
```


### Track additional metrics, parameters, and models
You can log additional information by directly invoking the MLflow Tracking logging APIs.

* Numerical metrics:

```
import mlflow
mlflow.log_metric("accuracy", 0.9)
```

* Training parameters:

```
import mlflow
mlflow.log_param("learning_rate", 0.001)
```

* Models:

    * Scikit-learn

```
import mlflow.sklearn
mlflow.sklearn.log_model(model, "myModel")
```

    * TensorFlow

```
import mlflow.tensorflow
mlflow.tensorflow.log_model(model, "myModel")
```

* Other artifacts (files):

```
import mlflow
mlflow.log_artifact("/tmp/my-file", "myArtifactPath")

```

#### Notebooks

```
# Import the required libraries.

import mlflow
import mlflow.sklearn
import pandas as pd
import matplotlib.pyplot as plt
 
from numpy import savetxt
 
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_diabetes
 
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error
Import the dataset from scikit-learn and create the training and test datasets.

db = load_diabetes()
X = db.data
y = db.target
X_train, X_test, y_train, y_test = train_test_split(X, y)
Create a random forest model and log parameters, metrics, and the model using mlflow.sklearn.autolog().

# Enable autolog()
# mlflow.sklearn.autolog() requires mlflow 1.11.0 or above.
mlflow.sklearn.autolog()
 
# With autolog() enabled, all model parameters, a model score, and the fitted model are automatically logged.  
with mlflow.start_run():
  
  # Set the model parameters. 
  n_estimators = 100
  max_depth = 6
  max_features = 3
  
  # Create and train model.
  rf = RandomForestRegressor(n_estimators = n_estimators, max_depth = max_depth, max_features = max_features)
  rf.fit(X_train, y_train)
  
  # Use the model to make predictions on the test dataset.
  predictions = rf.predict(X_test)
```

## Quick Start with R

```
#install mlflow pkg
install.packages("mlflow")

#import the mlflow library
library(mlflow)
# "install" mlflow in R session
install_mlflow()

# define parameters
my_int <- mlflow_param("my_int", 1, "integer")
my_num <- mlflow_param("my_num", 1.0, "numeric")

# log parameters
mlflow_log_param("param_int", my_int)
mlflow_log_param("param_num", my_num)

# read parameters
column <- mlflow_log_param("column", 1)

# log total rows
mlflow_log_metric("rows", nrow(iris))
```