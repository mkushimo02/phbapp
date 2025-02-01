Below is a `README.md` format for your GitHub repository that explains the provided Jenkins pipeline code. It includes a description of the pipeline, its stages, and the infrastructure setup. You can include infrastructure images as needed.

---

# Jenkins Pipeline for Building, Testing, and Deploying a Java Web Application

This repository contains a Jenkins pipeline script for automating the build, test, and deployment process of a Java web application. The pipeline uses Maven as the build tool and deploys the application to a Tomcat server.

## Pipeline Overview

The pipeline is defined in a `Jenkinsfile` and consists of the following stages:

1. **Cloning Repository**: Clones the source code from the specified GitHub repository.
2. **Building**: Compiles the source code and packages it into a WAR file using Maven.
3. **Testing**: Runs static code analysis using SonarQube to ensure code quality.
4. **Deploy to Web**: Deploys the packaged WAR file to a Tomcat server.

### Pipeline Code

```groovy
pipeline {
    agent any
    tools {
        maven "maven3.9.1"
    }
    stages {
        stage("Cloning Repository") {
            steps {
                echo "Cloning repository"
                git branch: 'main', url: 'https://github.com/mkushimo02/phbapp.git'
                echo "Cloning repository completed"
            }
        }
        stage("Building") {
            steps {
                echo "Building package"
                sh "mvn package"
                echo "Building package completed"
            }
        }
        stage("Testing") {
            steps {
                echo "Testing package Source"
                sh "mvn sonar:sonar"
                echo "Testing package Source completed"
            }
        }
        //stage("Artifactory") {
        //    steps {
        //        echo "Pushing to artifact"
        //        sh "mvn deploy -s ./settings.xml"
        //        echo "Pushing to artifact completed"
        //    }
        //}
        stage("Deploy to web") {
            steps {
                echo "Deploy to Tomcat"
                deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', path: '', url: 'http://tomcat:8080')], contextPath: null, war: 'target/*.war'
                echo "Deploy to Tomcat completed"
            }
        }
    }
}
```

---

## Pipeline Stages

### 1. Cloning Repository
- **Purpose**: Fetches the source code from the GitHub repository.
- **Tools Used**: Git.
- **Branch**: `main`.
- **Repository URL**: `https://github.com/mkushimo02/phbapp.git`.

### 2. Building
- **Purpose**: Compiles the source code and packages it into a WAR file.
- **Tools Used**: Maven (`mvn package`).

### 3. Testing
- **Purpose**: Performs static code analysis using SonarQube to ensure code quality.
- **Tools Used**: Maven (`mvn sonar:sonar`).

### 4. Deploy to Web
- **Purpose**: Deploys the WAR file to a Tomcat server.
- **Tools Used**: Jenkins Tomcat deployment plugin.
- **Credentials**: `tomcat-cred` (stored in Jenkins credentials).
- **Tomcat URL**: `http://tomcat:8080`.

---

## Infrastructure Setup

The pipeline assumes the following infrastructure setup:

1. **Jenkins Server**: Hosts the Jenkins pipeline and orchestrates the build, test, and deployment process.
2. **GitHub Repository**: Contains the source code for the Java web application.
3. **Tomcat Server**: Hosts the deployed application.
4. **SonarQube Server**: Used for static code analysis during the testing stage.

### Infrastructure Diagram

Below is a high-level diagram of the infrastructure:

```
+-------------------+       +-------------------+       +-------------------+
|                   |       |                   |       |                   |
|   GitHub Repo     |       |   Jenkins Server  |       |   Tomcat Server   |
|                   |       |                   |       |                   |
+--------+----------+       +--------+----------+       +--------+----------+
         |                           |                           |
         | Clone Repo                | Deploy WAR                |
         +-------------------------->+-------------------------->+
                                     |
                                     | Run Build & Test
                                     +-------------------------->+
                                                               SonarQube
```
![Dev-Muisi-Page-6](https://github.com/user-attachments/assets/791db412-2c05-4829-ae5f-c6912e9f7d15)


---

## Prerequisites

To run this pipeline, ensure the following are set up:

1. **Jenkins**: Installed and configured with the necessary plugins (Maven, Git, SonarQube, and Tomcat deployment).
2. **Maven**: Installed and configured on the Jenkins server.
3. **GitHub Repository**: Accessible by Jenkins.
4. **Tomcat Server**: Running and accessible with valid credentials stored in Jenkins.
5. **SonarQube Server**: Configured for static code analysis.

---

## How to Use

1. Clone this repository or copy the `Jenkinsfile` into your project.
2. Configure Jenkins to use the `Jenkinsfile` as the pipeline script.
3. Ensure all prerequisites are met.
4. Run the pipeline in Jenkins.

---

## Notes

- The `Artifactory` stage is commented out in the pipeline. Uncomment and configure it if you want to push the artifact to a repository like JFrog Artifactory.
- Replace `tomcat-cred` with your actual Jenkins credential ID for Tomcat access.
- Update the `url` in the `Deploy to web` stage to match your Tomcat server's URL.

---
![image](https://github.com/user-attachments/assets/8bdea1a1-787d-40fc-8ab4-2ecf52f213ba)

