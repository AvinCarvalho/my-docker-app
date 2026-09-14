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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:latest'
                }
         }}
      
        stage('Deploy to k3s') {
            steps {
                sh 'sudo kubectl rollout restart deployment eeshoapp'
            }
        }
    }
    post {
        success {
            echo ' Deployment Successful!'
        }
        failure {
            echo ' Deployment Failed!'
        }
    }
}
