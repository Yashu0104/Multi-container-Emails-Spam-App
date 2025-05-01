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
                    sh "${DOCKER_COMPOSE} build"
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    sh "${DOCKER_COMPOSE} up -d"
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sleep 10
                    sh 'docker ps'
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    // Example backend test command
                    // sh 'pytest tests/'
                }
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                script {
                    sh "${DOCKER_COMPOSE} down"
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up'
                sh "${DOCKER_COMPOSE} down"
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
