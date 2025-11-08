# 🎓 Student App - Complete DevOps CI/CD Project 🚀

A **Java-based Student Management Web Application** fully deployed using an **end-to-end CI/CD pipeline** built with **Git, GitHub, Jenkins, AWS EC2 (Ubuntu), Maven, and Apache Tomcat**.  

This project demonstrates how to automate the entire development and deployment process — from **code commit → build → deployment** — using Jenkins pipelines and cloud infrastructure.

---

## 🎯 Project Overview

The **Student App** allows users to enter and manage student data such as name, age, qualification, percentage, and passing year.  

This project focuses on automating deployment through **Continuous Integration and Continuous Deployment (CI/CD)** using Jenkins and AWS EC2 instances.  
It provides a real-world example of DevOps implementation for Java applications.

---
![Project Overview](./img/project1.png)

---

## 🧠 Objective

To create a **fully automated deployment pipeline** using Jenkins that performs the following tasks:

1. Developer clones project from GitHub and makes changes locally.  
2. Changes are pushed to a personal GitHub repository (`master` branch).  
3. Jenkins automatically detects changes and pulls the latest code.  
4. Maven builds the Java project and generates a `.war` file.  
5. Jenkins securely copies the `.war` file to a Tomcat server hosted on another EC2 instance.  
6. The web application automatically restarts and is available for access on the browser.

---

## ⚙️ Technologies Used

| Category | Tools / Technologies |
|-----------|----------------------|
| **Language** | Java |
| **Build Tool** | Maven |
| **Version Control** | Git & GitHub |
| **Automation Tool** | Jenkins |
| **Cloud Platform** | AWS EC2 (Ubuntu) |
| **Server** | Apache Tomcat 10 |
| **Pipeline Type** | Jenkins Declarative Pipeline |

---

## 🔁 CI/CD Workflow (Animated Diagram)

![Project Workflow](./img/project2.png)

Each stage is fully automated, ensuring zero manual deployment effort.

---

## 🧰 Jenkinsfile Explanation

Here’s the Jenkinsfile used for building and deploying the application automatically 👇

```groovy
pipeline {
    agent any

    environment {
        SERVER_IP   = '172.31.4.42'
        SSH_CRED_ID = 'jenkinskey'
        TOMCAT_PATH = '/var/lib/tomcat10/webapps'
        TOMCAT_SVC  = 'tomcat10'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Pulling code from GitHub repository...'
                git branch: 'master', url: 'https://github.com/Maheshshelke05/student-app.git'
            }
        }

        stage('Build WAR File') {
            steps {
                echo '🏗️ Building project using Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat Server') {
            steps {
                echo '🚀 Deploying WAR to EC2 Tomcat Server...'
                sshagent(['jenkinskey']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/*.war ubuntu@${SERVER_IP}:${TOMCAT_PATH}
                    ssh ubuntu@${SERVER_IP} 'sudo systemctl restart ${TOMCAT_SVC}'
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Application live on Tomcat server.'
        }
        failure {
            echo '❌ Build failed. Please check console logs for error details.'
        }
    }
}
```

### 📄 Jenkins Pipeline Stages:
- **Checkout:** Fetches latest code from GitHub master branch.  
- **Build:** Uses Maven to compile and package the application into a `.war` file.  
- **Deploy:** Transfers the WAR file to the Tomcat server on EC2 and restarts the service.  

---

## ☁️ AWS EC2 Setup Details

| Instance | Purpose | Configuration |
|-----------|----------|----------------|
| **EC2-1** | Jenkins Server | Ubuntu 22.04, Jenkins, Java, Maven, Git |
| **EC2-2** | Application Server | Ubuntu 22.04, Apache Tomcat 10, Java |

### 🔐 SSH Connection:
- SSH key pair `jenkinskey` generated.  
- Private key added to Jenkins Credentials under “SSH Agent”.  
- Enabled passwordless SSH between Jenkins and Tomcat EC2 servers.  

---

## 🧱 Jenkins Configuration Steps

1. Install Jenkins on EC2 Ubuntu instance.  
2. Install required plugins:  
   - Git Plugin  
   - Maven Integration Plugin  
   - SSH Agent Plugin  
   - Pipeline Plugin  
3. Create credentials for SSH deployment.  
4. Create a new **Pipeline Project** in Jenkins.  
5. Link to your GitHub repository containing Jenkinsfile.  
6. Build the project manually the first time, then Jenkins automates it afterward.

---

## 🌐 Deployment Steps Summary

1. Developer commits changes → GitHub Repository.  
2. Jenkins auto-triggers pipeline.  
3. Maven builds and generates the `.war` file.  
4. Jenkins deploys `.war` to EC2 (Tomcat).  
5. Application automatically restarts on Tomcat.  
6. Access the app in browser via:  

```
http://<EC2-Public-IP>:8080/student-app
```

---

## 🌟 Key Features

- 🔄 Fully automated CI/CD pipeline using Jenkins  
- ☁️ Cloud-based deployment using AWS EC2  
- 🧠 Java + Maven based build system  
- 🧱 Version control with Git & GitHub  
- 🔐 Secure deployment via SSH Agent  
- 📊 Live build tracking via Jenkins dashboard  
- 🚀 Instant auto-deployment to Tomcat server  

---

## 🧩 Output (Deployed App)

Once deployed successfully, the app displays the **Student Admission Form**:  

Fields include:  
- Student Name  
- Address  
- Age  
- Qualification  
- Percentage  
- Year Passed  

---

## 📸 Screenshots Showcase

| Screenshot | Description |
|-------------|-------------|
| ![GitHub Repository](./img/Screenshot%202025-11-08%20162333.png) | GitHub repository with project code |
| ![VS Code Jenkinsfile](./img/Screenshot%202025-11-08%20162202.png) | Jenkinsfile viewed in VS Code |
| ![Jenkins Credentials](./img/Screenshot%202025-11-08%20162145.png) | Jenkins SSH credentials configuration |
| ![Jenkins Build Console](./img/Screenshot%202025-11-08%20162129.png) | Jenkins console logs showing build success |
| ![Pipeline Success](./Screenshot%202025-11-08%20162111.png)| Successful pipeline view in Jenkins |
| ![Running Web App](./img/Screenshot%202025-11-08%20162034.png) | Student App running live on EC2 Tomcat server |

---

## 👨‍💻 Author

**Mahesh Shelke**  
💼 DevOps | Java Developer | Cloud Enthusiast  
🌐 [GitHub Profile](https://github.com/Maheshshelke05)  

---

## 🏁 Conclusion

This project demonstrates a **complete DevOps workflow** — integrating **Git, GitHub, Jenkins, Maven, AWS EC2, and Tomcat** for a real-world CI/CD implementation.  

> 💡 It automates the entire deployment lifecycle, improves consistency, and eliminates manual errors — a core DevOps principle.  

⭐ **If you found this project helpful, don’t forget to star the repo!** ⭐
