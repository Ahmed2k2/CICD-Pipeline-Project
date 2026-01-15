pipeline {
    agent any

    environment {
        IMAGE_NAME = "smart-todo-app"
        CONTAINER_NAME = "smart-todo-container"
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
                echo "Building Docker image"
                sh 'sudo docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker container"
                sh '''
                sudo docker rm -f $CONTAINER_NAME || true
                sudo docker run -d --name $CONTAINER_NAME -p 5000:5000 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
