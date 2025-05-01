pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Yashu0104/Multi-container-Emails-Spam-App.git'
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Pull the latest code from the main branch
                git url: "${GIT_REPO}", branch: "${GIT_BRANCH}"
            }
        }

        stage('Build Docker Images') {
            steps {
                // Build Docker images for backend and frontend
                bat 'docker build -t spam-sniffer-backend ./backend' // Assuming Dockerfile is inside backend folder
                bat 'docker build -t spam-sniffer-frontend ./frontend' // Assuming frontend Dockerfile is inside frontend folder
            }
        }

        stage('Start Containers') {
            steps {
                // Start containers in detached mode
                bat 'docker-compose up -d'
            }
        }

        stage('Health Check') {
            steps {
                // Optional: Check running containers to ensure everything is working
                bat 'docker ps'
            }
        }
    }

    post {
        always {
            // Clean up by stopping containers
            echo 'Cleaning up...'
            bat 'docker-compose down'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
