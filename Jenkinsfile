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
                echo "Code already checked out by SCM"
                sh 'ls -la'   // workspace me files check karne ke liye
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
                docker rm -f $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME \
                -p 5001:5001 --network cicd-network $IMAGE_NAME
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
