pipeline {
    agent any

    environment {
        IMAGE_NAME = "dockerized-smart-todo-app"
        CONTAINER_NAME = "dockerized-smart-todo-container"
        SONAR_HOST_URL = "http://sonarqube:9001"          // SonarQube URL
        SONAR_TOKEN = credentials('docker-sonar-token')         // Jenkins secret text
        scannerHome = tool 'SonarScanner'                 // Jenkins Global Tool Name
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling code from GitHub"
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image now"
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis"
                withSonarQubeEnv('Docker-SonarQube') {          // exact server name in Jenkins
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=DockerSmartTodoApp \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_TOKEN"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker container"
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 5001:5001 --network cicd-network $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment finally successful 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}

