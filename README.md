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

Workspace forms the top level of resource of Azure Machine Learning.
![workspace](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ws.png)

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

A Service Principal account  associated  with my specific workspace was ceated within the Udacity lab which completed the authentication process.
Therefore, there was no need to repeat the step 1. This Service Principal assigns role to user with controlled permissions to access specific resources.

## **Step 2 - Auto ML Experiment**

The bankmarketing dataset was uploaded into Azure Machine Learning Datastore so that it could be used during model training. By enabling security security and authentication, an experiment was ceated using automated machine learnind (ML). Azure ML created pipelines of different algorithms and parameter sets in parallel that that went through a series of iterations that eventually converged to produce models with associated training score. The model with the highest training score based on the criterion defined in the experiment was selected as the best model to fit the dataset. The image below indicate the registered dataset listed in Azure ML Studio:

![dataset](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss1.png)

* A new compute cluster was created and configured to run the experiments
![compute cluster](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss2.png)

The experiment was run to completion indicating the best model

![experiment completion](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss3.png)
![experiment runs](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss4.png)
### **Best Model**

VotingEnsemble was selected as the best model
![best model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss5.png)

## **Step 3 - Deploy the Best Model**

The best model was deployed as a REST endpoint using Azure Container Instance (ACI) and with Authentication enabled. 

![healthy deployed model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss6.png)

![authenticated deployed model](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss7.png)

## **Step 4 - Enable Logging**

**Enabled Application insight**
Application Insights was enabled in Azure service to monitor the performance and healthr of web applications. Events such as business domain or application
runtime and matrics such as values of measurements taken at speficic time intervals relating to application runtime, infrastructure performnce or user,s resource usage  were monitored.

The provided python logs.py script was edited by turning on the Application insights for deployed endpoint and to retrieve the logs. The following image indicate that the endpoint section in Azure ML Studio providing Application Insights was enabled as "True"

![enabling application insight](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss8.png)

The following images show the log enabling details:

![config.json](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss9.png)

![running log script](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss10.png)

![log insights](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss11.png)

## **Step 5 - Swagger Documentation**

The deployed endpoint was consumed in this step by deploying a docker container ui to view swagger documentaton for the endpoint. A swagger.json
file provided by Azure was downloaded and kept in the same directory as the serve.py and the swagger.sh files. Running the serve.py file started a Python server
to listen on port 8000 while the swagger.sh updated the latest Swagger container, and run it on port 9010.

![serve.py and swagger.sh runs](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss12.png)

This created an interaction with the swagger instance running with the documentation for the HTTP API of the best automl-VotingEnsemble model.
The image below show the GET and POST requests produced on the Swagger UI:

![swagger documentation](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss13.png)

The screenshots below indicate the GET and POST request details:

![get response](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss14.png)

![ret request running](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss15.png)

![post request](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss16.png)

![post request running](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss17.png)

## **Step 6 - Consume Model Endpoints**

The provided endpoint.py script was edited by providing the appropriate primary key and scoring url, which was used to interact with the deployed best model. 
This script issuesd a POST request to the deployed model and GETs a JSON response that was printed to the git bash terminal. The image below shows the output of the endpoint.py script execution.

![endpoint.py execution](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss18.png)

## **Apache Bechmark (optional step)**

A benchmark is used to create a baseline or acceptable performance measure that check the health of the endpoint. The Apache Benchmark (ab) was run against the HTTP API using the same  authentication key and url to retrieve performance results.  During the endpoint.py execution, a  data.json file was created, which was used to  HTTP POST to the endpoint. The following screenshots indicated that no requests were failed during the benchmark.sh launch:

![benchmark launch](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss19.png)

![benchmark output](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss20.png)

## **Step 7 - Create and Publish a Pipeline**

This step involved running the provided Jupyter notebook from beginning to end from Azure SDK. 

**Pipeline Running**

![pipeline running](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss21.png)

**Pipeline Run Completed**

![pipeline run completed](https://github.com/eaamankwah/Operationalizing-Machine-Learning-using-Azure/blob/main/screenshots/ss22.png)




