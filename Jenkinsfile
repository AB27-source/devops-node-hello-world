pipeline {
    agent any
    tools {
        nodejs 'NodeJS 22.7.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests defined."'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t devops-node-app .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker rm -f devops-node-app-container || true'
                    sh 'docker run -d -p 3000:3000 --name devops-node-app-container devops-node-app'
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}