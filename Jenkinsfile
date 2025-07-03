pipeline {
    agent any

    environment {
        // Set path for Kubernetes YAML on host
        DEPLOYMENT_YAML = "/home/balaganeshm/Desktop/jenkins/deployment.yaml"
        MINIKUBE_HOST = "balaganeshm@172.17.0.1"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Go Build') {
            steps {
                sh 'go version'
                sh 'go build -o userapi'
            }
        }

        stage('Docker Build (in Minikube)') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no $MINIKUBE_HOST '
                  eval $(minikube docker-env) &&
                  docker build -t userapi:latest /home/balaganeshm/Desktop/jenkins
                '
                '''
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no $MINIKUBE_HOST '
                  kubectl apply -f ${DEPLOYMENT_YAML}
                '
                '''
            }
        }
    }
}
