pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = 'docker-compose -f docker-compose.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashu0104/Multi-container-Emails-Spam-App.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    bat "${DOCKER_COMPOSE} build"  // Using 'bat' for Windows
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    bat "${DOCKER_COMPOSE} up -d"  // Using 'bat' for Windows
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sleep 10
                    bat 'docker ps'  // Using 'bat' for Windows
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    // Uncomment and configure the test command as per your setup
                    bat 'pytest tests/'  // Example backend test command for Windows
                }
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                script {
                    bat "${DOCKER_COMPOSE} down"  // Using 'bat' for Windows
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up'
                bat "${DOCKER_COMPOSE} down"  // Using 'bat' for Windows
            }
        }
        success {
            script {
                echo 'Build and deployment successful!'
            }
        }
        failure {
            script {
                echo 'Build failed.'
            }
        }
    }
}
