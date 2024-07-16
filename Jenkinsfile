pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_VERSION = '1.29.2'
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHub'dan kodu çek
                git branch: 'main', url: 'https://github.com/EbruAzkul/YMGFinal.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Docker Compose versiyonunu kur
                    sh 'curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                    sh 'chmod +x /usr/local/bin/docker-compose'
                }
                // Docker Compose ile servisi build et
                sh 'docker-compose -f docker-compose.yml build --no-cache'
            }
        }

        stage('Deploy') {
            steps {
                // Docker Compose ile servisi başlat
                sh 'docker-compose -f docker-compose.yml up -d'
                // Servisin sağlıklı başlaması için bekle
                sleep 20
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
                sh 'docker-compose -f docker-compose.yml down'
            }
        }
    }
}
