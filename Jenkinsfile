pipeline {
    agent any

    environment {
        // Ensures Jenkins locates node/npm on macOS
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shi092/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }
    }

    post {
        always {
            mail to: 'shinga.goto0@gmail.com',
                 subject: "Jenkins Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                 body: """
                    Jenkins Build Details

                    Project: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}
                    Status: ${currentBuild.currentResult}
                    
                    Console Output: ${env.BUILD_URL}console
                 """
        }
    }
}