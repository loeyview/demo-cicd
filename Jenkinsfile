4. Configure Jenkins Pipeline for SonarQube Analysis
Create a Jenkinsfile in your project:

groovy
คัดลอก
แก้ไข
pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_TOKEN = credentials('sonarqube-token')  // Store in Jenkins Credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/example/repo.git'  // Replace with your repo
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {  // The name given in Manage Jenkins
                    sh 'mvn clean verify sonar:sonar -Dsonar.login=$SONARQUBE_TOKEN'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
