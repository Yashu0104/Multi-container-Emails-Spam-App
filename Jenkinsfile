pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "spam_sniffer"
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Health Check') {
            steps {
                script {
                    echo "Waiting for backend to become ready..."
                    sleep 10
                    sh 'curl --fail http://localhost:5000 || (echo "Backend failed to start" && exit 1)'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful.'
        }
        failure {
            echo '❌ Deployment failed.'
        }
        cleanup {
            echo '🧹 You can add docker-compose down or cleanup tasks here if needed.'
        }
    }
}
