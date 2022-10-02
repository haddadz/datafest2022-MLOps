---
tags: [Notebooks/MLOps]
title: 12-MLflow
created: '2022-09-23T02:31:44.074Z'
modified: '2022-09-23T02:32:57.661Z'
---
############################################## DELETE 
# 13-MLflow Models

## MLflow Models 

### MLflow Model: standard format for packaging ML models used in a variety of downstream tools

— real-time serving through a REST API
- batch inference on Apache Spark

### Defines a convention to save a model in different “flavors” that can be understood by different downstream tools.


## Storage Format
All of the flavors defined in its MLmodel file in YAML format. 

```
# Directory written by mlflow.sklearn.save_model(model, "my_model")
my_model/
├── MLmodel
├── model.pkl
├── conda.yaml
├── python_env.yaml
└── requirements.txt
```

############################################## DELETE 
############################################## DELETE 
############################################## DELETE 
############################################## DELETE 