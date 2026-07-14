pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t employee-management:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop employee-app || true
                docker rm employee-app || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker run -d \
                --name employee-app \
                --restart unless-stopped \
                -p 5000:80 \
                employee-management:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh 'sleep 5'
                sh 'curl http://localhost:5000'
            }
        }

    }
}
