# 🚀 Mini Jenkins CI/CD Pipeline Project

This project is a hands-on demonstration of setting up a complete CI/CD pipeline using Jenkins, GitHub, SonarCloud, Slack, and Maven. It's designed to simulate a real-world development lifecycle with environment-based deployments and automated quality checks.

---

## 🔧 Technologies Used

- **Jenkins** – Automation server to build and manage pipelines  
- **GitHub** – Source code hosting and version control  
- **SonarCloud** – Static code analysis and quality gate enforcement  
- **Slack** – Notifications on pipeline events  
- **Maven** – Build and dependency management  
- **Groovy** – Jenkins pipeline scripting  
- **Shell** – Deployment scripts

---

## 📂 Project Structure

mini-jenkins-project/
├── Jenkinsfile
├── pom.xml
├── deploy-dev.sh
├── deploy-staging.sh
├── deploy-prod.sh
└── src/
└── main/
└── java/
└── com/
└── afzal/
└── App.java

---

## ⚙️ Pipeline Stages

1. **SCM Trigger**: Pipeline starts on GitHub push  
2. **Checkout**: Pulls source code from the GitHub repository  
3. **Build**: Compiles and packages the code using Maven  
4. **SonarCloud Analysis**: Runs static code checks and displays quality gate results  
5. **Deploy**: Deploys based on the selected environment (`dev`, `staging`, or `prod`)  
6. **Slack Notification**: Sends build status to a Slack channel  
7. **Cleanup**: Deletes the workspace after each build

---

## 🚀 How to Run

1. Fork or clone this repository  
2. Set up Jenkins with:
   - Maven (tool name: `mymaven`)
   - SonarCloud (server name: `mysonar`)
   - Slack credentials (token ID: `slacktoken`)
3. Create a pipeline job in Jenkins using GitHub SCM
4. Trigger a build and monitor each stage in Jenkins

---

## 🧠 What I Learned

- Jenkins pipeline as code using Groovy  
- SonarCloud integration with proper tokens and project keys  
- Slack alerts using Jenkins plugins  
- Maven project structure and plugins for build & coverage  
- Handling Jenkins environment variables and credentials  
- Troubleshooting real-world CI/CD issues as a beginner

> 📌 This project required **22 builds** to reach successful execution — each failure helped me learn and improve the pipeline.

---

## 🙌 Author

**Afzal Mansuri**  
DevOps Engineer | CI/CD Enthusiast | Fresher  
[LinkedIn Profile](https://linkedin.com/in/afzal-mansuri)

