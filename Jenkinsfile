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
                docker build --no-cache -t cicd-app:${IMAGE_TAG} .
                '''
            }
        }

        stage('Deploy Updated Image to Kubernetes') {
            steps {
                sh '''
                kubectl set image deployment/cicd-app cicd-app=cicd-app:${IMAGE_TAG} --record
                '''
            }
        }

        stage('Verify Rollout') {
            steps {
                sh 'kubectl rollout status deployment/cicd-app'
            }
        }
    }
}
