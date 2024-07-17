pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // örnek: sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // örnek: sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // örnek: sh 'scp target/*.war user@server:/path/to/deploy'
            }
        }
    }
}