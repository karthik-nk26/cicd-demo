pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cicd-app .'
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
