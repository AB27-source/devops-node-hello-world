pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Install npm dependencies
                    sh 'npm install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    // Run tests, if you have any
                    sh 'npm test || echo "No tests defined."'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image with a project-specific name
                    sh 'docker build -t devops-node-app .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Remove any existing container with the same name to avoid conflicts
                    sh 'docker rm -f devops-node-app-container || true'
                    // Run the Docker container using the newly built image
                    sh 'docker run -d -p 3000:3000 --name devops-node-app-container devops-node-app'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up exited containers and dangling images
                sh 'docker system prune -f'
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
