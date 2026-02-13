pipeline {
    agent any

    stages {

        stage('Build Docker Image in Minikube') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build -t cicd-app:latest .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl rollout restart deployment cicd-app'
            }
        }

    }
}
