pipeline {
    agent any

    stages {

        stage('Pull Code') {
            steps {
                git 'https://github.com/AvinCarvalho/my-docker-app.git' 
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop myapp || true'
                sh 'docker rm myapp || true'
                sh 'docker run -d -p 3000:80 --name myapp myapp'
            }
        }
    }
}
