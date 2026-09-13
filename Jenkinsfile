pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'eesho'
        IMAGE_NAME = 'myapp'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/AvinCarvalho/my-docker-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:latest'
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
                sh 'docker run -d --restart always -p 4000:80 --name myapp $DOCKERHUB_USER/$IMAGE_NAME:latest'
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
