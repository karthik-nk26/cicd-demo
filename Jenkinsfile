pipeline {
    agent any

    environment {
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Latest Code') {
            steps {
                git branch: 'main', url: 'https://github.com/karthik-nk26/cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build --no-cache -t cicd-app:$IMAGE_TAG .
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

        stage('Update Deployment Image') {
            steps {
                sh '''
                kubectl set image deployment/cicd-app cicd-app=cicd-app:$IMAGE_TAG
                '''
            }
        }
    }
}
