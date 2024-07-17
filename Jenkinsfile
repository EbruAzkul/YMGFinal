pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "myproject"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t myapp:latest .'
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    echo 'Deploying with Docker Compose...'
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'docker exec ${COMPOSE_PROJECT_NAME}_glassfish_1 ./run_tests.sh'
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up...'
                sh 'docker-compose down'
            }
        }
    }
}
