pipeline {
    agent any

    tools {
        maven 'maven39' // Ensure Maven 3.9 is configured in Jenkins
        jdk 'java17'    // Ensure Java 17 is configured in Jenkins
    }

    environment {
        SONAR_HOST_URL = 'http://13.127.230.91:9000' // Verify this is the correct SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonor_token_id') // Ensure 'sonor-token-id' exists in Jenkins credentials
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
                sh 'mvn clean package' // Clean and build the Maven project
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' matches your Jenkins configuration
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=java-hello-world-with-maven \
                      -Dsonar.host.url=$SONAR_HOST_URL \
                      -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                    echo "Docker image ${DOCKER_IMAGE}:${DOCKER_TAG} built successfully."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_CREDENTIALS}") {
                        def dockerImage = docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}")
                        dockerImage.push()
                        echo "Docker image ${DOCKER_IMAGE}:${DOCKER_TAG} pushed to Docker Hub successfully."
                    }
                }
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Deploying to development environment...'
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
