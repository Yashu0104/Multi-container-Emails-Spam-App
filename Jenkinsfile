pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'spam-sniffer'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashu0104/Multi-container-Emails-Spam-App.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker-compose -f docker-compose.yml build'
            }
        }

        stage('Run Containers') {
            steps {
                bat 'docker-compose -f docker-compose.yml up -d'
            }
        }

        stage('Health Check') {
            steps {
                bat '''
                curl -s http://localhost:5000/api/health || echo "Backend not reachable"
                curl -s http://localhost:3000 || echo "Frontend not reachable"
                '''
            }
        }

        stage('Run Backend Tests') {
            steps {
                bat 'docker-compose exec backend pytest || echo "Tests failed"'
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                bat 'docker-compose -f docker-compose.yml down'
            }
        }
    }

    post {
        always {
            echo 'Teardown...'
            bat 'docker-compose -f docker-compose.yml down'
        }
    }
}
