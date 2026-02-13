pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Image in Minikube Docker') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build --no-cache -t cicd-app:latest .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl rollout restart deployment cicd-app
                '''
            }
        }
    }
}
