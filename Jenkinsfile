pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/AvinCarvalho/my-docker-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh 'docker stop myapp || true'
                sh 'docker rm myapp || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d --restart always -p 3000:80 --name myapp myapp:latest'
            }
        }
    }

    post {
        success {
            echo '🔥 Deployment Successful!'
        }

        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
