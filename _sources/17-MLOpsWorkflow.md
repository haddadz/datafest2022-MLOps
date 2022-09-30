# 17-MLOps workflow on Databricks



## What is MLOps?
MLOps is a set of processes and automated steps to manage code, data, and models. It combines DevOps, DataOps, and ModelOps.

![image](https://docs.databricks.com/_images/mlops-lakehouse.png)

ML assets such as 
- code,
-  data, and 
- models
developed in stages that progress from early development stages that do not have tight access limitations and are not rigorously tested, through an intermediate testing stage, to a final production stage that is tightly controlled. 

The Databricks Lakehouse platform lets you manage these assets on a single platform with unified access control. You can develop data applications and ML applications on the same platform, reducing the risks and delays associated with moving data around.

## General recommendations for MLOps

### Create a separate environment for each stage
- Development
- Staging
- Production

### Access control and versioning
Access control and versioning are key components of any software operations process. Databricks recommends the following:

- Use **Git** for version control. Pipelines and code in Git for version control. Facilitates moving ML  between stages by **promoting** code using stage branches i.e. code from the development branch -> to the staging branch -> to the release branch

- Store data in a Lakehouse architecture using **Delta tables**. Both raw data and feature tables should be stored as Delta tables with access controls to determine who can read and modify them.

- Manage models and model development with **MLflow**. MLflow to **track** the model development process and save code **snapshots**, model **parameters**, **metrics**, and **other metadata**. Use the **Model Registry** to manage model versioning and deployment status. The Model Registry provides webhooks and an API so you can integrate with CD systems, and also handles access control for models.

### Deploy code, not models
 Databricks recommends that during the ML development process, you promote code, rather than models, from one environment to the next. Moving project assets this way ensures that all code in the ML development process goes through the same code review and integration testing processes. It also ensures that the production version of the model is trained on production code. 





