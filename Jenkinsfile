pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t issann/devsecops-app:v1 backend'
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push issann/devsecops-app:v1'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f kubernetes/'
            }
        }
    }
}