pipeline {
    agent any

    environment {
        IMAGE_NAME = "smart-todo-app"
        CONTAINER_NAME = "smart-todo-container"
        SONAR_HOST_URL = "http://localhost:9000"  // SonarQube URL
        SONAR_TOKEN = credentials('sonar-token')  // Jenkins me add kiya hua token
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
                withSonarQubeEnv('SonarQube') {   // Jenkins me SonarQube server ka name
                    sh "sonar-scanner \
                        -Dsonar.projectKey=SmartTodoApp \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_TOKEN"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
