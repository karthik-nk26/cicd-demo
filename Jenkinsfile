pipeline {
    agent any

    environment {
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Latest Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image in Minikube') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build -t cicd-app:$IMAGE_TAG .
                '''
            }
        }

        stage('Update Deployment Image') {
            steps {
                sh '''
                kubectl set image deployment/cicd-app cicd-app=cicd-app:$IMAGE_TAG
                '''
            }
        }

        stage('Apply Kubernetes Config') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
