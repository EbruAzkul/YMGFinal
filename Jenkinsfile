pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_VERSION = '1.29.2'
    }

    stages {
        stage('Checkout') {
            steps {
                // Kaynak kodu GitHub'dan çekiyoruz
                git branch: 'main', url: 'https://github.com/EbruAzkul/YMGFinal.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Docker Compose versiyonunu kuruyoruz
                    sh 'curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                    sh 'chmod +x /usr/local/bin/docker-compose'
                }
                // Docker Compose kullanarak servisi build ediyoruz
                sh 'docker-compose build --no-cache'
            }
        }
        stage('Deploy') {
            steps {
                // Docker Compose kullanarak servisi başlatıyoruz
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo 'Pipeline başarıyla tamamlandı.'
        }
        failure {
            echo 'Pipeline başarısız oldu.'
            script {
                // Docker Compose ile çalışan servisleri durdur
                sh 'docker-compose down'
            }
        }
    }
}
