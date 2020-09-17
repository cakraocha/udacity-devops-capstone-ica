# Udacity DevOps Capstone Project
By: I Gede Wibawa Cakramurti

This repository is a capstone project for Udacity DevOps Nanodegree 2020

## Project Tasks
- Plan for the design of the project
- Choose an app or website to deploy
- Install Jenkins
- Build a pipeline using rolling or blue/green deployment
- Use Docker to containerise the app or website
- Build Kubernetes clusters
- Deploy the app or website using the Kubernetes clusters

## Pipeline Design
Create eksctl kubernetes cluster -> Linting -> Build Docker Image -> Push Docker Image to Dockerhub -> Set config AWS EKS ->
Set current kubectl context -> Create Blue replication controller -> Create Green replication controller ->
Create the service in Kubernetes cluster in charge of routing traffic to Blue replication controller and expose to outside world ->
Wait for user's approval -> Update the service to redirect to Green

## Project Requirement
Make sure you have installed these requirements to use the CI/CD pipeline:
- Jenkins
- Blueocean Plugin in Jenkins
- Pipeline-AWS Plugin in Jenkins
- AWS Account
- AWS CLI
- Docker and Docker Account
- Pip
- eksctl
- kubectl

## Project Step-by-step
1. Install Jenkins
2. Configure Jenkins plugin
3. Create pipeline in Jenkins by connecting to GitHub
4. Create AWS credentials in Jenkins to connect to AWS
5. Configure AWS CLI to connect AWS to your local system
6. Create Kubernetes cluster using eksctl
7. Create kubeconfig in AWS EKS
8. Build a Dockerfile
9. Add stage 'Lint HTML' to Jenkins
10. Add stage 'Build Docker Image' to Jenkins
11. Add stage 'Push Image to Dockerhub' to Jenkins
12. Add stage 'Set current kubectl context' to Jenkins
13. Define blue/green deployment
14. Pipeline ready!

```
Please follow the deployment_screenshots/ for deployment screenshots
```

*Special thanks to mehmetincefidan for the repo as guidance for this project*
https://github.com/mehmetincefidan/Udacity-Cloud-DevOps-Engineer-Capstone-Project