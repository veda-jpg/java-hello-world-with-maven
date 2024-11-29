pipeline {
    agent any

    tools {
        maven 'maven39'
        jdk 'java17'
    }

    environment {
        SONAR_HOST_URL = 'http://3.110.155.176:9000'
        SONAR_AUTH_TOKEN = credentials('sonarqube-token-id')
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'master'
                    url: 'https://github.com/veda-jpg/java-hello-world-with-maven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') { // SonarQube server name in Jenkins
                    sh 'mvn sonar:sonar -Dsonar.projectKey=java-hello-world-with-maven'
                }
            }
        }

        stage('Push Artifacts to Nexus') {
            steps {
                sh 'echo pushtonexus'
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                sh 'echo deploytodev'
            }
        }
    }
}
