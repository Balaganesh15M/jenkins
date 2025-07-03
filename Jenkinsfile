pipeline {
    agent any

    stages {
        stage('Go Build') {
            steps {
                sh 'go version'
                sh 'go build -o userapi'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t userapi:latest .'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                 sh '''
        ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 kubectl apply -f /home/balaganeshm/Desktop/jenkins/deployment.yaml
        ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 kubectl rollout restart deployment userapi
        '''
            }
        }
        stage('Docker Build') {
            steps {
                sh '''
                    # Set Docker to point to Minikube's Docker
                    ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 '
                      eval $(minikube docker-env) &&
                      docker build -t userapi:latest /home/balaganeshm/Desktop/jenkins
                    '
                '''
            }
        }
        '''
    }
}


    }
}
