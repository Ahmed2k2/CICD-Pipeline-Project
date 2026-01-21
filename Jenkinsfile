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

        stage('Code Ready') {
            steps {
                echo "Code already checked out from SCM"
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis"
                withSonarQubeEnv('Docker-SonarQube') {
                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=DockerSmartTodoApp \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=${SONAR_HOST_URL} \
                    -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker container"
                sh '''
                docker rm -f dockerized-smart-todo-container || true
                docker run -d --name dockerized-smart-todo-container \
                -p 5001:5001 --network cicd-network dockerized-smart-todo-app
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
