pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://192.168.164.58:9000/' // Adjust the SonarQube URL as needed
        SONAR_AUTH_TOKEN = credentials('sonarqube_id') // Adjust with your SonarQube credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'token_github', branch: 'master', url: 'https://github.com/srikarreddy5/task1.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" install' // Directly use full path to npm.cmd
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('srikar_reddy') {
                        bat '"C:\\Program Files\\nodejs\\npm.cmd" run sonar' // Run SonarQube analysis
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
