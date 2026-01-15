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
                echo "Building Docker image now"
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube code analysis"

                withSonarQubeEnv('Local-SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=smart-todo-app -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker container"
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 5000:5000 $IMAGE_NAME
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
