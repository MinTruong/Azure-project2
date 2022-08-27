![Python application test with Github Action](https://github.com/MinTruong/Azure-project2/actions/workflows/pythonapp.yml/badge.svg)

# flask-ml-service
A sample Flask application to showcase the Azure Pipeline.

## Environment
Python 3.7

# Overview

In this project, you will build a Github repository from scratch and create a scaffolding that will assist you in performing both Continuous Integration and Continuous Delivery.

You'll use Github Actions along with a Makefile, requirements.txt and application code to perform an **initial lint, test, and install cycle**. Next, you'll integrate this project with **Azure Pipelines** to enable Continuous Delivery to Azure App Service

## Project Plan

* LINK Trello board: ![Trello](https://trello.com/b/brXtmkmQ/building-a-ci-cd-pipeline)

* LINK GoogleSheet:  ![GoogleSheet](https://docs.google.com/spreadsheets/d/1dvOl1CK02xtG_mlnIVhDZJkzKRHMg1FTRIITKwTk6t0/edit?usp=sharing)

## Instructions

<TODO:  
* Architectural Diagram (Shows how key parts of the system work)>

<TODO:  Instructions for running the Python project.  How could a user with no context run this project without asking you for any help.  Include screenshots with explicit steps to create that work. Be sure to at least include the following screenshots:

* Project running on Azure App Service

* Project cloned into Azure Cloud Shell

* Passing tests that are displayed after running the `make all` command from the `Makefile`

* Output of a test run

* Successful deploy of the project in Azure Pipelines.  [Note the official documentation should be referred to and double checked as you setup CI/CD](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python-webapp?view=azure-devops).

* Running Azure App Service from Azure Pipelines automatic deployment

* Successful prediction from deployed flask app in Azure Cloud Shell.  [Use this file as a template for the deployed prediction](https://github.com/udacity/nd082-Azure-Cloud-DevOps-Starter-Code/blob/master/C2-AgileDevelopmentwithAzure/project/starter_files/flask-sklearn/make_predict_azure_app.sh).
The output should look similar to this:

```bash
odl_user [ ~/Azure-project2 ]$ ./make_predict_azure_app.sh 
Port: 443
{"prediction":[20.35373177134412]}
```

* Output of streamed log files from deployed application

> 

## Enhancements

In the process of deploying project, i have some difficulty with test 
I things in the future, i must create new branch and new environment for test   

## Demo 

LINK Demo: ![Demo](https://www.youtube.com/watch?v=DoHgNxkxUw8)





