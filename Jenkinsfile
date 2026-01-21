pipeline {
    agent any

    environment {
        IMAGE_NAME     = "dockerized-smart-todo-app"
        CONTAINER_NAME = "dockerized-smart-todo-container"
        SONAR_HOST_URL = "http://sonarqube:9001"
        SONAR_TOKEN    = credentials('docker-sonar-token')
        scannerHome    = tool 'SonarScanner'
    }

    stages {
        stage('Check Code') {
            steps {
                echo "Code is already checked out by SCM"
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
    }
}
