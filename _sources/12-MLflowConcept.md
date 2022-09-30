# 12-MLflow Concept

MLflow is organized into four components: 
- Tracking
- Projects
- Models 
- Model Registry

Each of these components can be used on their own.

### MLflow aims to take any codebase written in its format and make it reproducible and reusable by multiple data scientists. 

## The Machine Learning Workflow

ML experiments with:
- datasets
- data prep steps
- algorithms
 to build a model that maximizes some target metric. 
 
 Once built:
 - model deploy to a production system 
 - monitor performance 
 - continuously retrain it on new data & compare with alternative models

Challengenges may arise:

- Difficult to keep track of experiments. Files on your lapto or notebook, how do you tell which data, code and parameters went into getting a particular result? Often times messy excels or tables tracked manually. 

- Difficult to reproduce code. Challenging if another data scientist want to use your code, or ifrun the same code at scale on another platform/cloud.

- No standard way to package & deploy. 

- No central store to manage models (their versions and stage transitions). Many models are created and in absence of a central place to collaborate and manage model lifecycle, data science teams face challenges in how they manage models stages: from development to staging, and finally, to archiving or production, with respective versions, annotations, and history.

Moreover, what if you want to try multiple ML libraries? Things can get messy. MLflow lets you train, reuse, and deploy models with any library and package them into reproducible steps that other data scientists can use as a “black box,” without even having to know which library you are using.

### MLflow Components


- **MLflow Tracking** is an API and UI for logging parameters, code versions, metrics, and artifacts when running your machine learning code and for later visualizing the results. Can be used in any environment  to log results to local files or to a server, then compare multiple runs. Teams can also use it to compare results from different users.

- **MLflow Projects** are a standard format for packaging reusable data science code. Each project is simply a directory with code or a Git repository, and uses a descriptor file or simply convention to specify its dependencies and how to run the code. Ex. Projects have conda.yaml file for specifying a Python Conda environment. When you use the MLflow Tracking API in a Project, MLflow automatically remembers the project version (for example, Git commit) and any parameters. You can easily run existing MLflow Projects from GitHub or your own Git repository, and chain them into multi-step workflows.

- **MLflow Models** offer a convention for packaging machine learning models in multiple flavors, and a variety of tools to help you deploy them. Each Model is saved as a directory containing arbitrary files and a descriptor file that lists several “flavors” the model can be used in. For example, a TensorFlow model can be loaded as a TensorFlow DAG, or as a Python function to apply to input data. MLflow provides tools to deploy many common model types to diverse platforms: for example, any model supporting the “Python function” flavor can be deployed to a Docker-based REST server, to cloud platforms such as Azure ML and AWS SageMaker, and as a user-defined function in Apache Spark for batch and streaming inference. If you output MLflow Models using the Tracking API, MLflow also automatically remembers which Project and run they came from.

- **MLflow Registry** offers a centralized model store, set of APIs, and UI, to collaboratively manage the full lifecycle of an MLflow Model. It provides model lineage (which MLflow experiment and run produced the model), model versioning, stage transitions (for example from staging to production or archiving), and annotations.

### Referencing Artifacts
When you specify the location of an artifact in MLflow APIs, the syntax depends on whether you are invoking the Tracking, Models, or Projects API. For the Tracking API, you specify the artifact location using a (run ID, relative path) tuple. For the Models and Projects APIs, you specify the artifact location in the following ways:

```
/Users/me/path/to/local/model

relative/path/to/local/model

<scheme>/<scheme-dependent-path>. For example:

s3://my_bucket/path/to/model

hdfs://<host>:<port>/<path>

runs:/<mlflow_run_id>/run-relative/path/to/model

models:/<model_name>/<model_version>

models:/<model_name>/<stage>

mlflow-artifacts:/path/to/model when running the tracking server in --serve-artifacts proxy mode.
```

For example:

##### Tracking API

```
mlflow.log_artifacts("<mlflow_run_id>", "/path/to/artifact")
```

##### Models API

```
mlflow.pytorch.log_model("runs:/<mlflow_run_id>/run-relative/path/to/model", registered_model_name="mymodel")
mlflow.pytorch.load_model("models:/mymodel/1")
```

### Scalability and Big Data
**Data** is the key for good results in ML, so **MLflow** is designed to scale to large data sets, large output files (for example, models), and large numbers of experiments. Specifically, MLflow supports scaling in four dimensions:

- An individual **MLflow** run can execute on a **distributed cluster**, for example, using Apache Spark. You can launch runs on the distributed infrastructure of your choice and report results to a Tracking Server to compare them. MLflow includes a **built-in API** to launch runs on Databricks.

- MLflow supports launching multiple runs in **parallel** with different parameters, for example, for hyperparameter tuning. You can simply use the **Projects API** to start multiple runs and the Tracking API to track them.

- **MLflow Projects** can take input from, and write output to, *distributed* storage systems such as AWS S3 and DBFS. MLflow can automatically download such files locally for projects that can only run on local files, or give the project a distributed storage URI if it supports that. This means that you can write projects that build large datasets, such as featurizing a **100 TB** file.

- **MLflow Model Registry** offers large organizations a **central hub** to collaboratively manage a complete model lifecycle. Many data science teams within an organization develop hundreds of models, each model with its experiments, runs, versions, artifacts, and stage transitions. A central registry facilitates model discovery and model’s purpose across multiple teams in a large organization.

### Example Use Cases
There are multiple ways you can use MLflow, whether you are a data scientist working alone or part of a large organization:

**Individual** Data Scientists can use MLflow Tracking to track experiments locally on their machine, organize code in projects for future reuse, and output models that production engineers can then deploy using MLflow’s deployment tools. MLflow Tracking just reads and writes files to the local file system by default, so there is no need to deploy a server.

**Data Science Teams** can deploy an MLflow Tracking server to log and compare results across multiple users working on the same problem. By setting up a convention for naming their parameters and metrics, they can try different algorithms to tackle the same problem and then run the same algorithms again on new data to compare models in the future. Moreover, anyone can download and run another model.

**Large Organizations** can share projects, models, and results using MLflow. Any team can run another team’s code using MLflow Projects, so organizations can package useful training and data preparation steps that other teams can use, or compare results from many teams on the same task. Moreover, engineering teams can easily move workflows from R&D to staging to production.

**Production Engineers** can deploy models from diverse ML libraries in the same way, store the models as files in a management system of their choice, and track which run a model came from.

**Researchers and Open Source Developers** can publish code to GitHub in the MLflow Project format, making it easy for anyone to run their code using the mlflow run github.com/... command.

**ML Library Developers** can output models in the MLflow Model format to have them automatically support deployment using MLflow’s built-in tools. In addition, deployment tool developers (for example, a cloud vendor building a serving platform) can automatically support a large variety of models.

### In thoery we can use MLflow to track anything from artifacts, metrics to data for single projects or large teams. 