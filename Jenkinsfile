pipeline {
    agent any

    tools {
        maven 'maven39' // Ensure Maven 3.9 is configured in Jenkins
        jdk 'java17'    // Ensure Java 17 is configured in Jenkins
    }

    environment {
        SONAR_HOST_URL = 'http://13.127.230.91:9000' // Verify this is the correct SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonor_token_id') // Ensure 'sonor-token' exists in Jenkins credentials
        
        DOCKER_HUB_CREDENTIALS = 'DOCKER_CREDENTIALS_ID' // Replace with your Jenkins credentials ID
        DOCKER_IMAGE = 'vedalakshmi/java-hello-world' // Replace with your Docker Hub repository
        DOCKER_TAG = 'latest' // Change the tag if needed
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

        stage('Build Docker Image') {
            steps {
                script {
                    echo "*****************************************************"
                    sh "pwd"
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                        docker logout
                        """
                    }
                }
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
