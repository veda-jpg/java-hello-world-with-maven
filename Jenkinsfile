pipeline {
    agent any

    tools {
        maven 'maven39'
        jdk 'java17'
    }

    environment {
        SONAR_HOST_URL = 'http://4.240.103.236:9000'
        SONAR_AUTH_TOKEN = credentials('sonor-token')
        NEXUS_URL = 'http://4.240.103.236:8081/repository/maven-releases/'
        NEXUS_CREDENTIALS = credentials('nexus-credentials')
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
                sh 'mvn clean package'
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') {
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
                sh '''
                mvn deploy \
                  -DaltDeploymentRepository=nexus::default::${NEXUS_URL} \
                  -DrepositoryId=nexus \
                  -Durl=${NEXUS_URL} \
                  -Dnexus.username=${NEXUS_CREDENTIALS_USR} \
                  -Dnexus.password=${NEXUS_CREDENTIALS_PSW}
                '''
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Deploying to development environment'
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
