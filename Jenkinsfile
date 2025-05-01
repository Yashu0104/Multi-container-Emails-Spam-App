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
                    bat """
                    echo Building Docker images...
                    ${DOCKER_COMPOSE} build || exit /b 1
                    """
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    bat """
                    echo Starting Docker containers...
                    ${DOCKER_COMPOSE} up -d || exit /b 1
                    """
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    echo 'Waiting for containers to initialize...'
                    sleep 10
                    bat 'docker ps'
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    // Update path to your actual test directory
                    bat """
                    echo Running backend tests...
                    pytest tests/ || exit /b 1
                    """
                }
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                script {
                    bat """
                    echo Cleaning up containers...
                    ${DOCKER_COMPOSE} down
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Post-build cleanup...'
                bat "${DOCKER_COMPOSE} down"
            }
        }
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
