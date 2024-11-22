
# CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/CI_PIPELINE.png)

This project provides a comprehensive guide for setting up a CI/CD pipeline on an AWS EC2 instance using Jenkins, Docker, SonarQube, Maven, and Trivy.
It covers all essential steps, including:

▪ Creating and configuring the EC2 instance.

▪ Installing and setting up Jenkins.

▪ Integrating tools like SonarQube for code quality analysis, Docker for containerization, Maven for build management, and Trivy for vulnerability scanning.


**The guide emphasizes:**

▪ Automated pipelines for seamless integration and testing.

▪ Reusable scripts through shared libraries.

▪ Robust quality checks to ensure reliability and security.

This process streamlines deployment by focusing on automation, quality, and scalability, making it a valuable resource for DevOps and cloud practitioners.





## Step-1 – Create the EC2 instance in AWS account with these parameters.

▪ EC2 type – Ubuntu t2.medium 

▪ EBS volume – 30 GB 

▪ Region – Asia Pacific(Mumbai) ap-south-1


![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/1.png)


## Step-2 : Connect to EC2 and Install all tools in that system as root user
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/2.png)
## Step-3 : Install Jenkins on Ubuntu 

```bash
https://github.com/DandaleAman/tools_installation_scripts/blob/main/jenkins.sh
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/3.png)

## Step-4 : Change the security group of EC2 instance 

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/4.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/5.png)

## Step-5 : Sign-In to Jenkins console

To access the Jenkins console run

```bash
  http://<EC2_Public_IP>:8080/
```

## Step-6 : Get the Administrator password by hitting the below command on EC2 terminal.

```bash
cat /var/lib/jenkins/secrets/initialPassword
```

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/6.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/7.png)

## Step-7 : Install all suggested plugins

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/8.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/9.png)

## Step-8 : Create first user on Jenkin

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/10.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/11.png)

## Step-9 : Create a pipeline Job

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/12.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/13.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/14.png)

## Step-10 : Add pipeline script as SCM

Add git repository as: 
```bash
https://github.com/DandaleAman/Java_app_3.0.git
```
Add branch Specifier: 
```bash
*/main
```
Add Script Path:
```bash
Jenkinsfile
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/15.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/16.png)

## Step-11 : Add the Plugins to Jenkins

Plugins for Sonar/Jfrog :
```bash
1. Sonar Gerrit 
2. SonarQube Scanner
3. SonarQube Generic Coverage 
4. Sonar Quality Gates 
5. Quality Gates 
6. Artifactory 
7. Jfrog
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/17.png)


## Step-12 : Setup Docker

```bash
https://github.com/DandaleAman/tools_installation_scripts/blob/main/docker.sh
```

```bash
sudo apt update -y

sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" -y

sudo apt update -y

apt-cache policy docker-ce -y

sudo apt install docker-ce -y

sudo systemctl status docker

sudo chmod 777 /var/run/docker.sock

docker -v
```

## Step-13 : Install SonarQube

```bash
docker run -d   --name   sonarqube  -p  9000:9000  -p  9092:9092 sonarqube
```

## Step-13.1 : Start docker container, If not in running state
```bash
docker ps  -a
docker start  <Container ID>
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/18.png)

## Step-13.2 : Login into Sonar Dashboard
```bash
Username – admin    
Password – admin
http://<EC2_Public_IP>:9000/
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/19.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/20.png)


## Step-13.3 : Create Sonar token for Jenkins

Sonar Dashboard -> Administration -> My Account -> Security -> Create token -> Save the token to some text file

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/21.png)

## Step-13.4 :  Integrate Sonar to Jenkins
Sonar Dashboard -> Administration -> Configuration -> webhooks -> Add the below name and url and save
```bash
http://<EC2_Public_Ip>:8080/sonarqube-webhook/
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/22.png)

## Step 13.5– Add Sonarqube to Jenkins
Jenkins Dashboard -> Manage Jenkins -> System configuration 

Click on sonarqube servers -> add url and name -> Click on add token -> Select Secret text -> Add the sonar token from step13.3 -> Give name of token as sonarqube-api

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/23.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/24.png)

## Step-14 : Install Maven
```bash
sudo apt update -y
sudo apt install maven -y
mvn -version
```

## Step-15 : Install TRIVY for Docker Image scan 

```bash
https://github.com/DandaleAman/tools_installation_scripts/blob/main/trivy.sh
```

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

## Step 16– Add the Jenkins Shared library (Global Trusted Pipeline Libraries)

Go to Manage Jenkins -> Configure system -> Global Trusted Pipeline Libraries-> Add below data 

```bash
Name - my-shared-library 
Default version – main 
Git - https://github.com/DandaleAman/jenkins_shared_lib.git
```
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/25.png)

## Step-17 : Once pipeline is Run Check

Console Output

The Jenkins logs

The Trivy scan vulnerabilities

The sonarqube dashboard for report 

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/26.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/27.png)

## Step- 18 : Sonarqube Dashboard for improve the quality of code
SonarQube is a Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project.

![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/28.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/29.png)
![EC2_Instance](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/30.png)

*******************************************************
![Thanks](https://github.com/DandaleAman/CI_PIPELINE_SETUP_WITH_VARIOUS_TOOLS/blob/main/Thanks.png)
*******************************************************
