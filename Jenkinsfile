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

        
    }

    post {
        success {
            echo 'Pipeline başarıyla tamamlandı....'
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
