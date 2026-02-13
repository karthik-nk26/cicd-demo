pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build -t cicd-app:latest .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply --validate=false -f k8s/deployment.yaml'
                sh 'kubectl apply --validate=false -f k8s/service.yaml'
            }
        }

    }
}
