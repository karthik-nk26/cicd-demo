pipeline {
    agent any

    environment {
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
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

        stage('Update Kubernetes Deployment Image') {
            steps {
                sh '''
                kubectl set image deployment/cicd-app cicd-app=cicd-app:$IMAGE_TAG
                '''
            }
        }

        stage('Verify Rollout') {
            steps {
                sh 'kubectl rollout status deployment cicd-app'
            }
        }
    }
}




