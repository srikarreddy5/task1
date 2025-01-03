pipeline {
    agent any
   

    environment {
    PATH = "C:\\Program Files\\nodejs\\:${env.PATH};${env.WORKSPACE}\\node_modules\\.bin"
    SONAR_HOST_URL = 'http://192.168.164.58:9000/'
    SONAR_AUTH_TOKEN = credentials('sonarqube_id')
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
                        bat 'npm run sonar-scanner' // Run SonarQube analysis using npm script
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
