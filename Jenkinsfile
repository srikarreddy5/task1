pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs\\:${env.PATH};${env.WORKSPACE}\\node_modules\\.bin"
        SONAR_HOST_URL = 'http://192.168.164.58:9000/' // Your SonarQube server URL
        SONAR_PROJECT_KEY = 'srikar' // Set your SonarQube Project Key
        SONAR_PROJECT_NAME = 'SrikarProject' // Set your SonarQube Project Name (no spaces)
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout from GitHub using the PAT stored in Jenkins credentials
                git credentialsId: 'token_github', branch: 'master', url: 'https://github.com/srikarreddy5/task1.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install' // Install dependencies using npm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Use withCredentials to securely inject the SonarQube authentication token
                    withCredentials([string(credentialsId: 'sonarqube_id', variable: 'SONAR_AUTH_TOKEN')]) {
                        // Run SonarQube analysis using the SonarQube Scanner with parameters
                        bat """
                        "%WORKSPACE%\\node_modules\\sonar-scanner\\bin\\sonar-scanner" -X ^
                        -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} ^
                        -Dsonar.projectName="${env.SONAR_PROJECT_NAME}" ^
                        -Dsonar.sources=. ^  // Adjusted to point to the root directory or correct folder
                        -Dsonar.host.url=${env.SONAR_HOST_URL} ^
                        -Dsonar.login=${env.SONAR_AUTH_TOKEN}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
