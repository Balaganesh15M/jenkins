pipeline {
    agent {
        docker {
            image 'golang:1.21'  // Official Go image
        }
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Balaganesh15M/jenkins.git'
            }
        }

        stage('Go Build') {
            steps {
                sh '''
                    go version
                    go build -o userapi
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f $DEPLOYMENT_YAML'
            }
        }
    }
}
