pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t issann/devsecops-app:v1 backend'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push issann/devsecops-app:v1'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f kubernetes/'
            }
        }
    }
}