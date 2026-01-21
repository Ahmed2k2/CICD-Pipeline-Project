pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "Cloning repository from GitHub"
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Ahmed2k2/CICD-Pipeline-Project.git',
                        credentialsId: 'git-creds'   // Jenkins me PAT wala ID
                    ]]
                ])
                
                echo "Listing workspace content to verify code"
                sh 'ls -la'
            }
        }
    }

    post {
        success {
            echo "Code checkout successful ✅"
        }
        failure {
            echo "Code checkout failed ❌"
        }
    }
}
