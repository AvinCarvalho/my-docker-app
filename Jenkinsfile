pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'taskly-web',
                url: 'https://github.com/AvinCarvalho/taskly-web.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t eesho/taskly-web:latest .'
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
                    sh 'docker push eesho/taskly-web:latest'
                }
            }
        }
        stage('Deploy to k3s') {
            steps {
                sh 'sudo kubectl rollout restart deployment taskly-web'
            }
        }
    }
    post {
        success {
            echo 'taskly-web Deployed!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
