pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_VERSION = '1.29.2'
        COMPOSE_FILE = 'docker-compose.yml' // Docker Compose dosya adı
        PROJECT_NAME = 'myproject' // Docker Compose proje adı
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
                sh "docker-compose -f ${COMPOSE_FILE} -p ${PROJECT_NAME} build --no-cache"
            }
        }

        stage('Deploy') {
            steps {
                // Docker Compose ile servisi başlat
                sh "docker-compose -f ${COMPOSE_FILE} -p ${PROJECT_NAME} up -d"
                // Servisin sağlıklı başlaması için bekle
                sleep 20
            }
        }

        stage('Test') {
            steps {
                // Servisin çalıştığını ve beklenen sonuçları kontrol et
                script {
                    def apiUrl = "http://localhost:8090" // Servis URL'si
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' ${apiUrl}", returnStdout: true).trim()

                    if (response == '200') {
                        echo "Servis başarıyla çalışıyor (HTTP 200 OK)"
                    } else {
                        error "Servis çalışmıyor veya beklenmeyen bir hata var (HTTP ${response})"
                    }
                }
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
                sh "docker-compose -f ${COMPOSE_FILE} -p ${PROJECT_NAME} down"
            }
        }
    }
}
