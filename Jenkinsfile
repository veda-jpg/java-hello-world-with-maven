pipeline{
    agent any

    tools {
         maven 'maven39'
         jdk 'java17'
    }

    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/veda-jpg/java-hello-world-with-maven.git']]])
            }
        }
        stage('build'){
            steps{
               sh 'mvn package'
            }
        }
             stage('sonarqualitycheck'){
            steps{
               sh 'echo sonarqualitycheck'
            }
        }
             stage('pushartifactorytonexus'){
            steps{
               sh 'echo pushtonexus'
            }
        }
                  stage('deploytodevenv'){
            steps{
               sh 'echo deploytodev'
            }
        }

         environment {
        SONAR_HOST_URL = 'http://<docker-host-ip>:9000'
        SONAR_AUTH_TOKEN = credentials('sonarqube-token-id')
    }
    }
}
