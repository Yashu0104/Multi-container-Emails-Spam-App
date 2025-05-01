pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = 'docker-compose -f docker-compose.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pull the latest code from Git repository
                git branch: 'main', url: 'https://github.com/Yashu0104/Multi-container-Emails-Spam-App.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build frontend and backend Docker images
                    sh "${DOCKER_COMPOSE} build"
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    // Start the containers using docker-compose
                    sh "${DOCKER_COMPOSE} up -d"
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    // Wait for containers to be up and check if they are healthy
                    sleep 10 // Give some time for containers to start
                    sh 'docker ps'  // List running containers to verify
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    // Run backend tests if any (e.g., unit tests for Flask app)
                    // Example:
                    // sh 'pytest tests/'
                }
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                script {
                    // Stop and remove any running containers
                    sh "${DOCKER_COMPOSE} down"
                }
            }
        }
    }

    post {
        always {
            // Cleanup actions after the pipeline completes
            echo 'Cleaning up'
            sh "${DOCKER_COMPOSE} down"
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
