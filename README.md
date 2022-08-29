![Python application test with Github Action](https://github.com/MinTruong/Azure-project2/actions/workflows/pythonapp.yml/badge.svg)

# flask-ml-service
A sample Flask application to showcase the Azure Pipeline.

## Environment
Python 3.7

# Overview

In this project, you will build a Github repository from scratch and create a scaffolding that will assist you in performing both Continuous Integration and Continuous Delivery.

You'll use Github Actions along with a Makefile, requirements.txt and application code to perform an **initial lint, test, and install cycle**. Next, you'll integrate this project with **Azure Pipelines** to enable Continuous Delivery to Azure App Service

## Project Plan

* LINK Trello board: [Trello](https://trello.com/b/brXtmkmQ/building-a-ci-cd-pipeline)

* LINK GoogleSheet:  [GoogleSheet](https://docs.google.com/spreadsheets/d/1dvOl1CK02xtG_mlnIVhDZJkzKRHMg1FTRIITKwTk6t0/edit?usp=sharing)

## Instructions
### **Continuous Integration**
This diagram shows how code can be tested automatically by enabling GitHub Actions. The push change to GitHub triggers the GitHub Actions container, which in turn runs a series of commands.

![infra](https://github.com/MinTruong/Azure-project2/blob/master/evidence/infra_1.png)

**Using GitHub Actions for Continuous Integration**

To do continuous integration, we need a build server—a centralized machine that is dedicated to continuously building the project every time code is committed to it. In this course, we'll be using GitHub actions as our build server. One of the things that makes GitHub Actions extremely convenient is that it is integrated with GitHub, which means that we can have it run builds automatically whenever we commit code to our GitHub repo. Let's check out how to set it up.

We have to configure **`.github\workflows\pythonapp.ymal`** like so:

![actions file](https://github.com/MinTruong/Azure-project2/blob/master/evidence/pythonapp.png)

After, we have use Github actions.

![Github Actions](https://github.com/MinTruong/Azure-project2/blob/master/evidence/GitHub_Action_2.png)

Next part, you reference Part I and Part II in **Continuous Delivery** of `Agile Development with Azure`
*   Part I - Set Up Azure DevOps - Udacity Cloud Lab
*   Part II - Set Up Github Repo

Commit [these starter files](https://github.com/MinTruong/Azure-project2) to your new repo. To do so, you will follow these steps:

```
cd Documents
# Clone the starter repo
git clone https://github.com/MinTruong/Azure-project2.git
# Clone the new repo
git clone https://github.com/[username]/flask-ml-service.git
# Copy the files from Azure-project2/* to the new repo manually.
cp -r Azure-project2/ flask-ml-service/
# Change the repo path as applicbale to you.
cd flask-ml-service
# Prepare for a push
git add -A
git status
git commit -m "Upload the starter flask app"
git push
```
After "pushing" the new repository, ensure to enable Azure Pipelines. To enable it in your Github, navigate to https://github.com/marketplace/azure-pipelines

Install/enable the Azure Pipelines marketplace app.

![Install/enable the Azure Pipeline marketplace app](https://github.com/MinTruong/Azure-project2/blob/master/evidence/install_azure_in_marketplay_app.png)

After that, you continue follow Part II 


### **Continuous Delivery**
We have referred in **Continuous Delivery** of `Agile Development with Azure`:

* Part III - Create WebApp Manually
* Part IV - Azure Pipeline Agent
* Part V - Create a Pipeline

In prat III, Create a web app service

Create an app service and initially deploy your app in Cloud Shell, you can run command.sh file or run by command:
```
# Provide the web app name as a globally unique value. 
az webapp up --name <Your_unique_app_name> --resource-group Azuredevops --runtime "PYTHON:3.7"
```
**Perform Prediction:**

Update the make_predict_azure_app.sh file to match the deployed URL:
```
Update the make_predict_azure_app.sh file to match the deployed URL:
```

After that you continue follow part III and IV. in part V, Update azure-pipelines.yml file like that:
```
# Starter pipeline    
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml
trigger:
- master

# TODO: Replace the agent pool name
pool: myAgentPool

variables:
  # TODO: Replace the service connection name
  azureServiceConnectionId: 'myserviceconnection'
  
  # TODO: Replace 'flaskapp-minh' with the existing Web App name
  webAppName: 'flaskapp-minh'

  # Environment name
  environmentName: 'Flask-ML-Deploy'

  # Project root folder. Point to the folder containing manage.py file.
  projectRoot: $(System.DefaultWorkingDirectory)

stages:
- stage: Build
  displayName: Build stage
  jobs:
  - job: BuildJob
    pool: myAgentPool
    steps:    
    - script: |
        python3.7 -m pip install --upgrade pip
        python3.7 -m pip install setup
        python3.7 -m pip install -r requirements.txt
      workingDirectory: $(projectRoot)
    - script: |
        export PATH=$HOME/.local/bin:$PATH
        make install
        make lint
      workingDirectory: $(projectRoot)
      displayName: 'Run lint tests'
    - task: ArchiveFiles@2
      displayName: 'Archive files'
      inputs:
        rootFolderOrFile: '$(projectRoot)'
        includeRootFolder: false
        archiveType: zip
        archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
        replaceExistingArchive: true

    - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
      displayName: 'Upload package'
      artifact: drop

- stage: Deploy
  displayName: 'Deploy Web App'
  dependsOn: Build
  condition: succeeded()
  jobs:
  - deployment: DeploymentJob
    pool: myAgentPool
    environment: $(environmentName)
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Deploy Azure Web App : flask-ml-service'
            inputs:
              azureSubscription: $(azureServiceConnectionId)
              appName: $(webAppName)
              package: $(Pipeline.Workspace)/drop/$(Build.BuildId).zip
```

### **Successful prediction from deployed flask app in Azure Cloud Shell.** 
The output should look similar to this:
```bash
odl_user [ ~/Azure-project2 ]$ ./make_predict_azure_app.sh 
Port: 443
{"prediction":[20.35373177134412]}
```


![Output of streamed log files from deployed application](https://github.com/MinTruong/Azure-project2/blob/master/evidence/Prediction_and_Log.png)


> 

## Enhancements

In the process of deploying project, i have some difficulty with test 
I things in the future, i must create new branch and new environment for test   

## Demo 

LINK Demo: [Demo](https://www.youtube.com/watch?v=DoHgNxkxUw8)





