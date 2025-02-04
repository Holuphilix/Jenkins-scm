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

**Screenshot:** Console Output

![Console Output](./Images/18.Console%20Output.png)


### **Task 4: Pipeline Job for Web Application Deployment**
**Objective:** Automate the web application deployment using Jenkins Pipeline and Docker.

#### **Steps to Create the Job:**
1. Click **"New Item"**, enter `WebApp-Pipeline`, and select **"Pipeline"**.
2. Click **OK**

**Screenshot:**  New Item `WebApp-Pipeline`
![New Item](./Images/19.Pipeline_Project.png)

3. Under **"Pipeline"**, select **Pipeline script from SCM**
4. Choose **Git** and enter the repository URL:
   ```sh
   https://github.com/Holuphilix/jenkins-scm.git
   ```

**Screenshot:**  New Item `WebApp-Pipeline`
![Pipeline SCM](./Images/20.WebApp-Pipeline_SCM.png)

5. Set **Branch** to `main`.
6. In **Script Path**, enter:
   ```sh
   Jenkinsfile
   ```

**Screenshot:** Pipeline Configuration
![Pipeline Job Configuration](./Images/21.Pipeline_config_advance.png)

7. Click **Save**, then click **Build Now** to test.

#### **Jenkinsfile Script for WebApp Pipeline:**

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          extensions: [],
                          userRemoteConfigs: [[url: 'https://github.com/Holuphilix/jenkins-scm.git']]
                ])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-webapp .'  
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd -p 8081:80 my-webapp'
                }
            }
        }
    }
}
```

**Screenshot:** Jenkins Pipeline Execution
![Jenkins Pipeline Execution](./Images/22.Jenkinsfile_script.png)

#### **Dockerfile Script for WebApp Pipeline:**

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

#### **index.html Script for WebApp Pipeline:**

```groovy
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Web App</title>
</head>
<body>
    <h1>Welcome to My CI/CD Web App!</h1>
    <p>Deployed using Jenkins and Docker.</p>
</body>
</html>
```

**Screenshot:** Index.html Execution
![index.html Execution](./Images/24.Index.html_script.png)

#### **.gitignore Script for WebApp Pipeline:**

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

### **Task 5: Docker Image Creation and Registry Push**
**Objective:** Automate the creation of Docker images and push them to Docker Hub.

#### **Steps:**
1. Configure Jenkins to build a **Docker image**.
2. Run a container using the built **Docker image**.
3. Access the web application in a browser.
4. Push the **Docker image** to Docker Hub.

#### **Commands:**

```sh
# Build Docker Image
sudo docker build -t holuphilix/my-webapp .

# Run Docker Container
sudo docker run -itd -p 8081:80 holuphilix/my-webapp

# Push to Docker Hub
sudo docker login
sudo docker tag holuphilix/my-webapp holuphilix/my-webapp:latest
sudo docker push holuphilix/my-webapp:latest
```

📸 **Screenshot Placeholder:**
> ![Docker Build and Push](screenshots/docker-push.png)

### **Conclusion**
This project successfully demonstrates how to set up a **CI/CD pipeline using Jenkins and Docker** to automate web application deployment. It ensures **continuous integration, automated testing, and scalable deployments**, which are essential for modern software delivery.

✅ **[Screenshot: Final Application Running]** *(Insert screenshot here)*


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
