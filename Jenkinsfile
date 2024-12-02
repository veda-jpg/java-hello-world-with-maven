pipeline {
    agent any

    tools {
        maven 'maven39' // Ensure Maven 3.9 is configured in Jenkins
        jdk 'java17'    // Ensure Java 17 is configured in Jenkins
    }

    environment {
        SONAR_HOST_URL = 'http://4.240.103.236/:9000'
        SONAR_AUTH_TOKEN = credentials('sonor-token') // Ensure this is set in Jenkins credentials
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
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
                sh 'mvn package'
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' is configured in Jenkins
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
                // Replace this with Nexus deployment logic, e.g., using Maven deploy plugin:
                // sh 'mvn deploy -DaltDeploymentRepository=nexus::default::http://<nexus-url>/repository/maven-releases/'
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Deploying to development environment'
                // Add deployment logic here, e.g., SSH to the target server or use Ansible scripts
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
