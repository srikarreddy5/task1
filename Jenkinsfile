pipeline {
    agent any
    tools {
        nodejs 'NodeJS' // Replace with your Node.js installation name in Jenkins
    }
    environment {
        // Define environment variables for SonarQube and ChromeDriver path
        SONAR_HOST_URL = 'http://192.168.164.58:9000/' // Ensure this is the correct URL for your SonarQube server
        SONAR_AUTH_TOKEN = credentials('sonarqube_id') // Replace 'sonarqube_id' with Jenkins credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from GitHub
                git branch: 'master', url: 'https://github.com/srikarreddy5/task_1.git' // Replace with your repo URL
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install dependencies using npm for Windows
                bat 'npm install' // For Windows, use 'bat' instead of 'sh'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using the environment variables
                    withSonarQubeEnv('srikar_reddy') { // Replace with your SonarQube installation name in Jenkins
                        bat 'npm run sonar' // Make sure 'npm run sonar' is correctly defined in your package.json
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
