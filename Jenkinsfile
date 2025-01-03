pipeline {
    agent any
    environment {
        // Set the path to include Node.js location
        PATH = "C:\\Program Files\\nodejs\\:$PATH"
        
        SONAR_HOST_URL = 'http://192.168.164.58:9000/' // Adjust the SonarQube URL as needed
        SONAR_AUTH_TOKEN = credentials('sonarqube_id') // Adjust with your SonarQube credentials ID
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
                bat 'npm install' // Use 'bat' on Windows systems
                bat '"C:\\Program Files\\nodejs\\npm.cmd" install' // Directly use full path to npm.cmd
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('srikar_reddy') { // Adjust the SonarQube installation name as needed
                        bat 'npm run sonar' // Run SonarQube analysis
                    withSonarQubeEnv('srikar_reddy') {
                        bat '"C:\\Program Files\\nodejs\\npm.cmd" run sonar' // Run SonarQube analysis
                    }
                }
            }
