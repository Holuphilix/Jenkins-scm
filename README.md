# Capstone Project : CI/CD Mastery 
 
## Introduction
Welcome to my **CI/CD Mastery** Capstone Project! As part of my DevOps journey, I have designed and implemented a robust **CI/CD pipeline** using **Jenkins** to automate the deployment of a web application. This project simulates a **real-world scenario** where a **technology consulting firm** is adopting cloud architecture, requiring a scalable and reliable CI/CD process.

This project focuses on **continuous integration (CI), continuous deployment (CD), and automation** to streamline software delivery. Below, you will find detailed steps on how the pipeline is structured and executed.

## Project Scenario
A technology consulting firm is transitioning to cloud-based software deployment. As a **DevOps Engineer**, my responsibility is to build an automated CI/CD pipeline to enhance efficiency and reliability in software deployment using **Jenkins, Docker, and GitHub**.

## Pre-requisites
To follow along with this project, ensure you have:

- Basic knowledge of Jenkins.
- Completed: 
  - Introduction to Jenkins
  - Jenkins Freestyle Project
  - Jenkins Pipeline Job mini projects
- A system with **Docker**, **Jenkins**, and **Git** installed.

## Project Deliverables

### Documentation
- Detailed setup instructions for Jenkins.
- Explanation of security measures implemented.

### Demonstration
- Live demonstration of the working **CI/CD pipeline**.

## Project Components

### Task 1: Implement Version Control with Git

#### Step 1.1: Initialize Git Repository

- To begin, create the project directory named MarketPeak_Ecommerce, navigate into it, and initialize it as a Git repository.

**Note:** For this project, I am using Git Bash on a Windows workstation to execute these shell commands, as it provides a Unix-like command-line experience.

**Command:**
```bash
mkdir CI-CD-Mastery
cd CI-CD-Mastery
git init
```
**Screenshot:** Project Directory CI-CD-Mastery
![Make directory](./Images/1.mkdir_ci-cd-mastery.png)

### Task 2: **Jenkins Server Setup**
#### Objetive: Configure Jenkins for CI/CD automation.

#### Steps 2.1: Run these commands one by one:

```bash
# Update system packages
sudo apt update -y && sudo apt upgrade -y  

# Install Java (required for Jenkins)
sudo apt install -y fontconfig openjdk-11-jdk  

# Add Jenkins repository key properly
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null  

# Add the Jenkins repository
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null  

# Update package lists
sudo apt update  

# Install Jenkins
sudo apt install -y jenkins  

# Start and enable Jenkins service
sudo systemctl enable --now jenkins  

# Check Jenkins status
sudo systemctl status jenkins  
```
- Jenkins Status and Home Dashboard:

**Screenshot:** Jenkins Status Output
![Jenkins Status](./Images/2.Jenkins_status.png)

**Screenshot:** Jenkins Dashboard
![Jenkins Dashboard](./Images/5.Jenkins_dashboard.png)

#### Step 2.2: Security Measures:
- Set up **admin credentials**

**Screenshot:** Admin credentials
![admin credentials](./Images/3.Jenkins_Admincredentials.png)

**Screenshot:** Create First Admin credentials
![Create First Admin credentials](./Images/6.Jenkins_username.png)

- Install essential plugins (**Git**, **Docker**, etc.).

**Screenshot:** Install essential plugin Git
![Git plugin](./Images/8.Plugins_install_Git.png)

**Screenshot:** Install essential plugin Docker
![Docker plugin](./Images/7.Plugins_install_Docker.png)

### Task 3: **Source Code Management (SCM) Integration**
#### Objective: Connect Jenkins to **GitHub** for automated builds.

#### Step 3.1:

```bash
# Install Git
sudo apt install -y git

# Clone the repository
git clone https://github.com/Holuphilix/Jenkins-scm.git
```
- Install **Git** on EC2 instance

**Screenshot:** Install Git
![Install Git](./Images/9.Install_git.png)

- Connect Jenkins to **GitHub**

**Screenshot:** Connect Jenkins
![Connect Jenkins to GitHub](./Images/14.Source_Code_Management_Git..png)

- Configure **webhooks** in GitHub to trigger builds automatically.

**Screenshot:** GitHub Webhook Setup 
 ![GitHub Webhook Setup](./Images/12.Git_Webhooks.png)

### Task 4: **Jenkins Freestyle Job for Build and Unit Tests**
#### Objective: Automate application builds and unit tests.

#### **Steps to Create the Job:**
1. **Login to Jenkins** and click **"New Item"**.
2. Enter a job name: `Build-and-Test-Job`.
3. Select **"Freestyle project"** and click **OK**.

**Screenshot:** New Item `Build-and-Test-Job`
![New Item `Build-and-Test-Job`](./Images/13.Freestyle_project.png)

4. Under **"Source Code Management"**, select **Git** and enter the repository URL:

```sh
https://github.com/Holuphilix/jenkins-scm.git
```

**Screenshot:** Connect Jenkins
![Connect Jenkins to GitHub](./Images/14.Source_Code_Management_Git..png)


5. Under **"Build Triggers"**, enable **Poll SCM** or configure a **Webhook**.

- **Screenshot:** Enable Poll SCM

![enable Poll SCM](./Images/15.Github_build_triggers.png)

6. Click **Save**, then click **Build Now** to test.

- **Screenshot:** Build Now Status

![Build Now Status](./Images/17.Build_Now.png)

- **Screenshot:** Console Output

![Console Output](./Images/18.Console%20Output.png)

## Task 4: Jenkins Pipeline for Web Application

### Objective
Develop a Jenkins Pipeline that automates the deployment of a simple web application.

### Steps
1. Create a simple `index.html` file.
2. Configure a `Jenkinsfile` to automate building and deploying the web application.

## Task 5: Docker Image Creation and Registry Push

### Objective
Automate the creation of a Docker image for the web application and push it to a container registry.

### Steps
1. Write a `Dockerfile` for containerizing the web application.
2. Modify the `Jenkinsfile` to include Docker build and push steps.


### **Task 5: Pipeline Job for Web Application Deployment**
**Objective:** Automate the web application deployment using Jenkins Pipeline and Docker.

#### **Steps to Create the Job:**
1. Click **"New Item"**, enter `WebApp-Deployment-Pipeline`, and select **"Pipeline"**.
2. Click **OK**

**Screenshot:**  New Item `WebApp-Deployment-Pipeline`
![New Item](./Images/19.Pipeline_Project.png)

3. Under **"Pipeline"**, select **Pipeline script from SCM**
4. Choose **Git** and enter the repository URL:
   
```sh
https://github.com/Holuphilix/Jenkins-scm.git
```

**Screenshot:**  Pipeline SCM `WebApp-Deployment-Pipeline`
![Pipeline SCM](./Images/20.WebApp-Pipeline_SCM.png)

5. Set **Branch** to `main`.
6. In **Script Path**, enter:

```sh
Jenkinsfile
```

**Screenshot:** Script Path: Jenkinsfile

![Pipeline Job Configuration](./Images/21.Pipeline_config_advance.png)

#### **index.html Script for WebApp-Deployment-Pipeline:**

```groovy
<!DOCTYPE html>
<html>
<head>
    <title>Jenkins CI/CD Pipeline</title>
</head>
<body>
    <h1>Welcome to My Web Application</h1>
    <p>Deployed using Jenkins CI/CD Pipeline</p>
</body>
</html>
```

**Screenshot:** Index.html Execution
![index.html Execution](./Images/24.Index.html_script.png)

#### **Configure a `Jenkinsfile` to automate building and deploying the web application.**

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'holuphilix/webapp:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning GitHub Repository...'
                git branch: 'main', url: 'https://github.com/Holuphilix/Jenkins-scm.git'
            }
        }

        stage('Build Web Application') {
            steps {
                echo 'Building Web Application...'
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Run Application') {
            steps {
                echo 'Running Web Application Container...'
                sh 'docker run -d -p 8080:80 ${DOCKER_IMAGE}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
```

### **Task 6: Docker Image Creation and Push to Docker Hub**

#### **Objective**
Automate the creation of a Docker image for the web application and push it to Docker Hub using Jenkins.

#### **Step 1: Create a Repository on DockerHub:**

1. Log in to DockerHub:
- Visit https://hub.docker.com/ and log in to your account.

2. Create a New Repository:
- Click on Create Repository.

3. Fill in the Details:
- Repository Name: jenkins-pipeline-app
- Visibility: Choose Public or Private (Public is free and easier to access).
- Leave other options as default.

4. Save:
- Click Create to save the repository.

**Screenshot:** Dockerhub Repository Name 
![Dockerhub Repository Name](./Images/30.Dockerhub_repos_name.png)

#### **Step 2: Update Jenkinsfile in GitHub Repository**
Modify the `Jenkinsfile` in your repository to include Docker image creation and pushing to Docker Hub.

#### **Updated Jenkinsfile Script for WebApp-Deployment-Pipeline:**

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "holuphilix/jenkins-pipeline-app:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Holuphilix/Jenkins-scm.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'echo "<h1>Welcome to Jenkins Pipeline!</h1>" > index.html'
            }
        }

        stage('Validate Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:80 $DOCKER_IMAGE'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
        always {
            sh 'docker system prune -f'
        }
    }
}
```

**Screenshot:** Jenkins Pipeline Execution
![Jenkins Pipeline Execution](./Images/22.Jenkinsfile_script.png)

#### **Step 3: Create Dockerfile Script for WebApp-Deployment-Pipeline:**

```groovy
# Use an official Nginx image as the base image
FROM nginx:latest

# Copy the web application files to the container
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80
```

**Screenshot:** Dockerfile Execution
![Dockerfile Execution](./Images/23.Dockerfile_Script.png)

#### **Step 4: Create .gitignore Script for WebApp-Deployment-Pipeline:**

```groovy
# Ignore temporary files
*.log
*.tmp
*.swp
# Ignore Node modules (if applicable)
node_modules/
# Ignore Python virtual environment
venv/
# Ignore Docker cache
*.tar
```

**Screenshot:** .gitignore Execution
![.gitignore Execution](./Images/25.gitignore_script.png)

#### **Step 2: Commit and Push Changes**

#### After updating the Jenkinsfile, save the changes and push them to your GitHub repository.

**Commands:**

```bash
git add .
git commit -m "Updated Readme.md for Docker image creation and push"
git push origin main
```

#### Step 4: Trigger the Build

1. Navigate to the **WebApp-Deployment-Pipeline** job in Jenkins.
2. Click "Build Now".
3. Monitor the Console Output for errors.

### Expected Outcome
**The Jenkins pipeline should:** 
    ✅ Pull the source code from GitHub.
    ✅ Build a Docker image.
    ✅ Push the image to Docker Hub.
    ✅ Deploy and run the container using the built Docker image.

### Conclusion

By completing this step, we have successfully automated the deployment process using **Jenkins, Docker, and GitHub**. The Docker image is now hosted in **Docker Hub**, making it easier to deploy the web application across different environments.

### Key Achievements
- Automated **code integration and testing**.
- Created **Jenkins jobs** for different stages.
- Built and deployed a **Dockerized application**.
- Published a Docker image to **Docker Hub**.

### Future Improvements
- Implement **Jenkinsfile** in a more advanced **multi-branch pipeline**.
- Integrate **Kubernetes** for better **orchestration**.
- Automate **testing with Selenium**.

### Author
**Holuphilix** – *Aspiring DevOps Engineer* 🚀

🔗 **GitHub Repository:** [Holuphilix/jenkins-scm](https://github.com/Holuphilix/jenkins-scm)

💡 **Let's Connect:** [LinkedIn Profile](#)

Happy Learning! 🎯
