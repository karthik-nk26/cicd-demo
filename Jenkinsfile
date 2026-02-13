pipeline {
    agent any

    stages {

        stage('Build Docker Image in Minikube') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build --no-cache -t cicd-app:latest .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply --validate=false -f k8s/deployment.yaml'
                sh 'kubectl apply --validate=false -f k8s/service.yaml'
            }
        }

        stage('Restart Deployment') {
            steps {
                sh 'kubectl rollout restart deployment cicd-app'
            }
        }
    }
}
