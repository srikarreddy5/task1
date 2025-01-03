pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs\\:${env.PATH};${env.WORKSPACE}\\node_modules\\.bin"
        SONAR_HOST_URL = 'http://192.168.164.58:9000/' // Your SonarQube server URL
        SONAR_PROJECT_KEY = 'srikar' // Set your SonarQube Project Key
        SONAR_PROJECT_NAME = 'Srikar Project' // Set your SonarQube Project Name
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

        stage('Verify sonar-scanner Installation') {
            steps {
                script {
                    // Display the workspace path to ensure it's correct
                    echo "WORKSPACE: ${env.WORKSPACE}"

                    // List the contents of the node_modules directory
                    bat "dir ${env.WORKSPACE}\\node_modules"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Use withCredentials to securely inject the SonarQube authentication token
                    withCredentials([string(credentialsId: 'sonarqube_id', variable: 'SONAR_AUTH_TOKEN')]) {
                        // Run SonarQube analysis using the SonarQube Scanner with parameters
                        bat """
                        "%WORKSPACE%\\node_modules\\sonar-scanner\\bin\\sonar-scanner" ^
                        -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} ^
                        -Dsonar.projectName=${env.SONAR_PROJECT_NAME} ^
                        -Dsonar.sources=src ^
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
