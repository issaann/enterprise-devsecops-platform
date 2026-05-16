pipeline {
    agent any

    environment {
        IMAGE_NAME = "issann/devsecops-app:v1"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME backend'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // This block safely binds your uploaded kubeconfig file to a temporary environment variable
                withCredentials([file(credentialsId: 'kubernetes-config', variable: 'KUBECONFIG_FILE')]) {
                    sh 'KUBECONFIG=$KUBECONFIG_FILE kubectl apply -f kubernetes/deployment.yaml'
                    sh 'KUBECONFIG=$KUBECONFIG_FILE kubectl apply -f kubernetes/service.yaml'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker login sessions from the runner
            sh 'docker logout || true'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}