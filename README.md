# MERN-Stack-application
This is a MERN Stack application, using CI/CD will be deployed on EKS Cluster on AWS Cloud within private VPC
* Using Terraform to automate entire infrastructure on AWS Cloud. No manual intervention

* Web application has 3 tiers - Frontend, Backend and Database.
* Front-end = presentation layer , Backend = Business layer.

### What is MERN Stack application ?
* Front-end is written n React JS
* Back-end part is written in Express JS which is a backend framework
* For server deploy , Node JS is used
* For Database, Mongo DB is used
* So MERN = MongoDB, Express JS, Node JS, React JS (MERN) . All these belongs to Java based family
* Its very simple to write  docker files for MERN Stack applications for both frontend and backend as they are from same java family
* For Database there is no docker image creation and no need to write docker file, I am using publicly available mongo DB image. 




## Step 1 - Infrastructure Automation
* Launch a EC2 instance on AWS, within this instance, install Terraform, Jenkins
* Create scripts using Terraform, to setup complete AWS Infrastructure required for project.
* Using Jenkins run the terraform scripts, which will create a private VPC in AWS, within VPC , there will be EKS cluster with 2 nodes - worker node 1 & 2
* Along with worker nodes, there will be Jump server useful to connect to EKS cluster to perform any administrative activities on EKS Cluster. Except the Jump server, no one will have access to EKS Cluster. 

## Step 2 - A comprehensive CI/CD using Jenkins and Argo CD 

### CI Part
* There will be multiple steps in Jenkins CI.
* Code Check out from github repo
* Perform code quality analysis on code using Sonarqube. Sonar scanning
* Run dependency checks on code using OWASP
* Run file scanning
* Create Docker Image and push it to AWS ECR private repo (private container registry)
* Perform container Image scanning using Trivy
* Update the K8S manifests with the new version of the image

### CD Part 
Using Argo CD deploy K8S manifests on to EKS CLuster

## Step 3 - Monitoring & Visualization
* How to use custom domain and how to integrate it with Route 53 of AWS
* Setting up Prometheus and use it as data source for monitoring
* Visualize complete observability metrics using Graphana
