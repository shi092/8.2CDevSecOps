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
            emailext (
                subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: """
                    <h3>Jenkins Build Details</h3>
                    <p><b>Project:</b> ${env.JOB_NAME}</p>
                    <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                    <p><b>Status:</b> ${currentBuild.currentResult}</p>
                    <p>Check console output at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: 's225546927@deakin.edu.au',
                attachLog: true
            )
        }
    }
}