pipeline {
    agent any

    tools {
        maven 'maven39' // Ensure Maven 3.9 is configured in Jenkins
        jdk 'java17'    // Ensure Java 17 is configured in Jenkins
    }

    environment {
        SONAR_HOST_URL = 'http://15.206.163.84:9000' // Verify this is the correct SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonor_token_id') // Ensure 'sonor-token' exists in Jenkins credentials
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs() // Cleans the workspace
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/veda-jpg/java-hello-world-with-maven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Clean and build
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' matches the configuration in Jenkins
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=java-hello-world-with-maven \
                      -Dsonar.host.url=$SONAR_HOST_URL \
                      -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Push Artifacts to Nexus') {
            steps {
                echo 'Push to Nexus repository'
                // Add Maven deploy logic or Nexus REST API script
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Deploying to development environment'
                // Add deployment logic here
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
