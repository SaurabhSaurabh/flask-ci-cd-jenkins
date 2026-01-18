# 🚀 CI/CD with Jenkins – Assignment

---

## 1️⃣ Objective
Set up a Jenkins CI/CD pipeline that:
- Pulls code from GitHub  
- Builds and tests automatically  
- Sends email notifications (via Gmail SMTP)  
- Optionally deploys artifacts  

---

## 2️⃣ Prerequisites
- **Ubuntu server / EC2 instance** with Jenkins installed  
- **Java & Maven** (or Node.js, depending on project)  
- **GitHub repository** with sample project  
- **Gmail App Password** for SMTP  
- Installed Jenkins plugins:
  - GitHub Integration  
  - Pipeline  
  - Email Extension (emailext)  

---

## 3️⃣ Jenkins Setup
1. **Install Jenkins**  
   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk -y
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install jenkins -y
   sudo systemctl enable jenkins
   sudo systemctl start jenkins
   ```

2. **Access Jenkins**  
   - Open browser → `http://<server-ip>:8080`  
   - Unlock Jenkins with initial admin password (`/var/lib/jenkins/secrets/initialAdminPassword`)
   - Jenkins Dashboard →
     ![Dashboard](screenshots/jenkinsui.png)

3. **Install Plugins**  
   - Go to **Manage Jenkins → Plugins → Available**  
   - Install **Pipeline** and **Email Extension Plugin**  

---

## 4️⃣ Configure SMTP (Gmail TLS)
- Go to **Manage Jenkins → Configure System → Extended E‑mail Notification**  
- Fill in:
  - **SMTP server**: `smtp.gmail.com`  
  - **SMTP Port**: `587`  
  - **Use TLS**: ✔️  
  - **Authentication**: ✔️  
    - Username: `sumansaurabh123@gmail.com`  
    - Password: Gmail App Password  
  - Email configuration →
     ![Emailconfig](screenshots/extendedEmailConfig.png)

---

## 5️⃣ Create Pipeline Job
1. Go to **New Item → Pipeline**  
2. Link to your GitHub repo (with Jenkinsfile).  
3. Save.
- Pipeline Config → ![Pipeline Config](screenshots/createpipeline.png)

---

## 6️⃣ Jenkinsfile Example
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/username/repo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
    post {
        success {
            emailext(
                subject: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build succeeded!\nCheck console output at ${env.BUILD_URL}",
                to: "sumansaurabh123@gmail.com, saiyedin786@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build failed.\nCheck console output at ${env.BUILD_URL}",
                to: "sumansaurabh123@gmail.com, saiyedin786@gmail.com"
            )
        }
    }
}
```

---

## 7️⃣ Results
- Code automatically built and tested on commit.   
  Build Execution →
  ![Build](screenshots/console1.png)
  ![Build](screenshots/console2.png)
  ![Build](screenshots/console3.png)
  ![Build](screenshots/console4.png)
  
- Notifications sent via Gmail SMTP.  
  Email Notification →
  ![Email](screenshots/successemail.png)
  



