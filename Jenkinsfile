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

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t cicd-app:$IMAGE_TAG .
                '''
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                minikube image load cicd-app:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
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
