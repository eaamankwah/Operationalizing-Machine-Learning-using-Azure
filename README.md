# **Operationalizing-Machine-Learning-using-Azure**

# **Table of Contents**

* Overview
* Architecture
* Project Steps
* Future Improvements
* Screencast Video

## Overview

This project is part of the Udacity Azure ML Nanodegree. In this project, I built and operationalized Azure to configure a cloud-based machine learning production model, deployed and consumed it. I also created, published, and consumed a pipeline. 

The dataset for this project was obtained from the [UC Irvine Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/bank+marketing).
The dataset contained data about a bank telemarking campaign in Portuguese. The target was to do a binary classification indicating whether an individual will sign up for a term deposit or not.

## Workspace and Architecture 

Workspace forms the top level of resource of Azure Machine Learning.  The workspace is used to manage data, compute resources, code, models, and other artifacts related to machine learning workloads.![workspace](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ws.png)

The images below indicate the architectural diagram and the overall workflow of the project:

![architecture](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/arch.png)

![workflow](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/wf.png)

## Project Steps

The following are the main steps that was completed in the project

* Step 1 - Authentication
* Step 2 - Auto ML Experiment
* Step 3 - Deploy the Best Model
* Step 4 - Enable Logging
* Step 5 - Swagger Documentation
* Step 6 - Consume Model Endpoints
* Step 7 - Create and Publish a Pipeline

## **Step 1 - Authentication**

Authentication step involves security principles and practices to keep data from being compromised such that access is restricted so that people who don't need to log in into the Azure system can't log in. A Service Principal account  associated  with my specific workspace was created within the Udacity lab which completed the authentication process.
Therefore, there was no need to repeat the step 1. This Service Principal assigns role to user with controlled permissions to access specific resources.

## **Step 2 - Auto ML Experiment**

The bankmarketing dataset was uploaded into Azure Machine Learning Datastore so that it could be used during model training. By enabling security and authentication, an experiment was created using automated machine learning (ML). This involved configuring a compute cluster, and using that cluster to run the experiment.
Azure ML created pipelines of different algorithms and parameter sets in parallel that that went through a series of iterations that eventually converged to produce models with associated training score. The model with the highest training score based on the criterion defined in the experiment was selected as the best model to fit the dataset. The image below indicates the registered dataset listed in Azure ML Studio:

![dataset](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss1.png)

* The image below indicates an auto-ml compute cluster that was configured to run the bankmarketing experiment in Azure ML Studio.
![compute cluster](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss2.png)

The experiment was run to completion indicating the best model among other ensemble and classification models.

![experimental models](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss3.png)

### **Best Model**

VotingEnsemble was selected as the best model with an accuracy of 91.9 % lasting for 18minute and 38.82 seconds. 
![best model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss4.png)

## **Step 3 - Deploy the Best Model**
A deployment is the way you deliver a trained model into production so that it can be consumed by others or other systems. There are many different ways and paths  that model can be made available for others to consume. The Azure Python SDK allows for local deployments, but not good for a production environment. Before deployment, the deployment settings must be configured, the type of machine and desired robustness must be decided to get the get proper results. The typical settings for machine robustness may include CPU, GPU, and memory requirements, followed by the type of cluster suitable for the selected the type of machine. The image below indicates the process of the best model being deployed with enabled authentication.

![deploying best model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss5.png)

An Azure container instance (ACI) service from Azure, which uses container technology was used quickly to deploy compute instance. The image below indicates the healthy state of the ACI after the deployment.

![healthy deployed model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss6.png)

Although authentication was enabled before deployment happened, the ability to authenticate and interact with the deployed model produce an HTTP API endpoint finally happened after the deployment.

![authenticated deployed model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss7.png)

## **Step 4 - Enable Logging**

Logging is a core pillar of operations in DevOps in general. It is important because it is at the forefront of discovering irregularities in services and most problems in general. Enabling logging allows a Cloud service like Azure to provide information about how the poise service is behaving. 

**Enabled Application insight**

Application Insights is an Azure feature for developers and DevOps professionals that can be used to detect anomalies. Application Insights also include powerful analytics tools that help visualize performance. This feature can be enabled before or after the deployment and is modifiable with the Azure SDK.

Before enabling the Application Insights using the SDK, both the workspace and web service classes were imported by downloading a config.json file from Azure Machine Learning Studio. These classes allowed the mapping of the workspace defined in the config from a previously deployed model that doesn't have Application Insights enabled, capture the service ID. The service ID is used for setting the name of the deployed model. 

Application Insights was enabled in Azure service to monitor the performance and health of web applications. Events such as business domain or application
runtime and metrics such as values of measurements taken at specific time intervals relating to application runtime, infrastructure performance or user's resource usage  were monitored.

The provided python logs.py script was edited by turning on the Application insights for deployed endpoint and to retrieve the logs. The following image indicate that the endpoint section in Azure ML Studio providing Application Insights was enabled as "True"

![enabling application insight](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss8.png)

The following screenshots indicate the running of the logs and its outputs:

![config.json](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss9.png)

![running log script](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss10.png)

![log insights](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss11.png)

## **Step 5 - Swagger Documentation**

Swagger is a tool that helps build, document, and consume RESTful web services in Azure ML Studio. It also explains what types of HTTP requests that an API can consume, like POST and GET. Azure provides a swagger.json that is used to create a web site that documents the HTTP endpoint for a deployed model.

The deployed endpoint was consumed in this step by deploying a docker container UI to view swagger documentation for the endpoint. A swagger.json
file provided by Azure was downloaded and kept in the same directory as the serve.py and the swagger.sh files. Running the serve.py file started a Python server
to listen on port 8000 while the swagger.sh updated the latest Swagger container, and run it on localhost port 9020.

![serve.py runs](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss12.png)

![swagger.sh runs](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss13.png)

![port 9010](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss14.png)

This created an interaction with the swagger instance running with the documentation for the HTTP API of the best automl-VotingEnsemble model.
The images below show the GET and POST requests produced on the Swagger UI:

![swagger documentation](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss15.png)

![get request](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss16.png)

![post request running](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss17.png)

## **Step 6 - Consume Model Endpoints**

Consuming deployed services is a source of truth when interacting with a deployed model. Users can consume a deployed service via an HTTP API. An HTTP API is a URL that is exposed over the network so that interaction with a trained model can happen via HTTP requests.
Users can initiate an input request, usually via an HTTP POST request. HTTP POST is a request method that is used to submit data. The HTTP GET is used to request or  retrieve information from a URL back to the user.  The allowed requests methods and the different URLs exposed by Azure create a bi-directional flow of information. The APIs exposed by Azure ML uses JSON (JavaScript Object Notation) to accept data and submit responses, which serves as a bridge language among different environment.

The provided endpoint.py script was edited by providing the appropriate primary key and scoring URL, which was used to interact with the deployed best model. 
This script issued a POST request to the deployed model and GETs a JSON response that was printed to the git bash terminal. The image below shows the output of the endpoint.py script execution.

![endpoint.py execution](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss18.png)

## **Apache Benchmark (optional step)**

A benchmark is used to create a baseline or acceptable performance measure that check the health of the endpoint. It creates a baseline by response times and failed requests, as well as identifies error rates and slow responses. The Apache Benchmark (ab) was run against the HTTP API using the same  authentication key and URL to retrieve performance results.  During the endpoint.py execution, a  data.json file was created, which was used to  HTTP POST to the endpoint. The following screenshots indicated the benchmark launch and that that no requests were failed during the benchmarking the benchmarking the endpoint:

![benchmark launch](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss19.png)

![benchmark output](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss20.png)

![benchmark output](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss21.png)

## **Step 7 - Create and Publish a Pipeline**

This step involved running the provided Jupyter notebook from beginning to end from Azure SDK. This section is more about automation and the main steps include creating a pipeline, publishing the pipeline and interacting with the pipeline through an HTTP API endpoint.

Machine Learning pipeline is an independently executable workflow of a complete machine learning task in Azure. Azure Machine Learning pipelines help to build, optimize, and manage machine learning workflows. These workflows in a pipeline have a number of benefits, including, simplicity, speed, repeatability, flexibility, versioning and tracking, modularity, quality assurance and cost control.

In the SDK notebook, the workspace was initially was initialized, the ML experiment was specified and attached to the same compute cluster created during previous experiments. The Bank marketing dataset was loaded and the AutoML was configured using the AutoMlConfig class. Moreover, the AutoMLStep class was used to specific the steps of the pipeline, then the experiment was submitted after the pipeline was created. 

The screenshot below indicates the pipeline experiment creation and completion
![pipeline creation](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss22.png)

**Pipeline Workflow and Endpoint**

In the Azure ML studio pipeline section, the pipeline workflow is observed as the bankmarkting dataset module is created and followed by the AutoML module after the experiment was completed.

![pipeline running](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss23.png)

The image below shows the pipeline endpoint in Azure ML Studio.
![pipeline endpoint](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss24.png)


**Publishing the Pipeline**

Publishing a pipeline is the process of making a pipeline publicly available so that it can run with different inputs. Pipeline publication can be done in Azure Machine Learning Studio as well as with the Python SDK. When a Pipeline is published, a public HTTP endpoint becomes available, allowing other services, including external ones, to interact with an Azure Pipeline. In Azure ML, all  published pipeline has a rest-endpoint.

In the SDK, the pipeline was published using the publish_pipeline method. This generated the pipeline rest-endpoint, in this case called "Bank Marketing Train" and was shown in the Azure ML pipeline section as the REST endpoint with status indicated as “Active”. The same active state was also observed in the Portal.

The following screenshots indicate pipeline overview in the SDK and the Azure Portal:

![published pipeline in sdk](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss25.png)

![published pipeline in Azure portal](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss26.png)

**Pipeline Run from RunDetails widget in SDK**

When a Pipeline endpoint is created, it can be scheduled to run in specific time intervals. This is done by using the ScheduleRecurrence class from the Azure SDK. In the end, the pipeline endpoint can be consumed using the HTTP through Python SDK. 

The screenshots below show the monitoring of the pipeline runs from RunDetails widget in SDK and the status of the results after triggering the pipeline using the published pipeline rest endpoint in SDK.

![pipeline run widget in sdk](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss27.png)

![published rest endpoint status](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss28.png)

## **Future Recommendations**

In future, the following areas could be improved:

* More data should be collected to improve the accuracy of the model as the algorithms can learn more from more dataset.

* Automate efficient hyperparameter tuning by using Azure Machine Learning HyperDrive package could be used for comparison purposes . Different combination of  hyperparameter values  --C and --Max_iter could be tried. The C value could be selected by using the Uniform or Choice functions to define the search space. The parameter search space could be defined as discrete or continuous, and a sampling method over the search space as grid, or Bayesian. New HyperDrive runs with difference estimators including the best performing estimator VotingEnsemble could be tried to improve the accuracy score. 

* It is also recommended to increase cross validation in search of a better model accuracy.

* The Deep Learning option in AutoML could be enabled for the binary classification task to ensure that the text data are classified. Deep learning models can improve the accuracy but may be hard to interpret.

* Since the dataset is not “big”, tuning the hyperparameter to a full completion may improve the accuracy score and using an AUC_weighted metric or balancing the dataset may improve the accuracy results.

* Since the bank marketing dataset has class imbalanced flaws, the data class with less representation could be oversampled while the class with over representation could be undersampled, keeping the variance in the dataset. More training time could be allowed after solving the imbalance issue so that the algorithms robustness and precision could be enhanced. 

* Although it was recommended to decrease the exit criterion of the AutoML model to an hour in order to save compute resources, this criterion could be increased to identify the model with the best performance.

## **Screencast Video Link**

The link below demonstrates the main processes undertaken in this project

https://drive.google.com/file/d/1eq_OmfJuHt8KZKbY0twbnG1EarH3JeGu/view?usp=sharing

* [Azure Machine Learning Pipelines](https://docs.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines)

* [Publish Machine Learning Pipelines in SDK](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-pipelines)

* [Azure Machine Learning Documentation](https://docs.microsoft.com/en-us/azure/machine-learning/)

* [Udacity Q & A Platform](https://knowledge.udacity.com/?nanodegree=nd00333&page=1&project=755&rubric=2893)







