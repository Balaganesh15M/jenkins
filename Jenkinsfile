pipeline {
    agent any

    environment {
        IMAGE_NAME = "userapi"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Go Build') {
            steps {
                sh 'go version'
                sh 'go build -o userapi'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    kubectl delete deployment userapi --ignore-not-found
                    kubectl apply -f deployment.yaml
                '''
            }
        }
    }
}
