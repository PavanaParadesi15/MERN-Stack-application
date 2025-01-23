Welcome to the MERN-Stack-application wiki!

# Step - 1: Infrastructure setup

* Launch EC2 instance for jenkins setup and execute terraform files. We can either SSH into instance and install all the necessary tools or disable ssh access and upload all the commands to install tools through User Data before creation of instance. 
* We can automate installations through user data. When SSH access is disabled, we can connect to instance using Session Manager.
```
sudo su ubuntu             // switch to ubuntu user
sudo htop                 // to check the commands running automatically according to the user script given 
```

## Install Java

### Intsalling Java

```
sudo apt update -y
sudo apt install openjdk-17-jre -y
sudo apt install openjdk-17-jdk -y
java --version
```

### Installing Jenkins
Jenkins runs on port - 8080
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
jenkins -version
```

### Installing Docker 
```
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
sudo chmod 777 /var/run/docker.sock
docker --version
```

### Sonarqube
I am not installing sonarqube, instead using sonarqube container. Sonarqube runs on port - 9000

```
docker run -d  --name sonar -p 9000:9000 sonarqube:lts-community
```

### Installing Terraform

```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform --version
```

### Installing Trivy

```
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt update
sudo apt install trivy -y
trivy --version
```

### Installing AWS CLI

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

### Installing Kubectl

```
sudo apt update
sudo apt install curl -y
sudo curl -LO "https://dl.k8s.io/release/v1.28.4/bin/linux/amd64/kubectl"
sudo chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```

###  Installing eksctl

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

### Intalling Helm
```
sudo snap install helm --classic
helm version
```

* We are using single jenkins node architecture, where jenkins is used as both master  and agent/slave 
* Install plugins in Jenkins. As I am running terraform scripts through jenkins and create infra on AWS like EKS Cluster, VPC using terraform scripts
1. AWS Credentials - Allows storing Amazon IAM credentials within the Jenkins Credentials API. Store Amazon IAM access keys (AWSAccessKeyId and AWSSecretKey) within the Jenkins Credentials API
2. Pipeline: AWS steps 
3. Pipeline: Stage view
4. Terraform plugin

* Add AWS security credentials (access key and secrect access key) . Manage Jenkins -> Credentials
* Add Terraform Path . Manage Jenkins -> Tools -> Terraform -> Add terraform .


# Create Jump Server
* EKS cluster and nodes are in private subnet. We cannot access the private cluster by default.
* So to interact with these, we are creating a Jump server within the same VPC as EKS cluster to communicate with EKS cluster.   








