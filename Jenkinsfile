pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myuserapi:latest'
        DEPLOYMENT_YAML = 'k8s/deployment.yaml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Balaganesh15M/jenkins.git'
            }
        }

        stage('Go Build') {
            steps {
                script {
                    docker.image('golang:1.21').inside {
                        sh '''
                            go version
                            go mod init example.com/myapp || true
                            go build -o userapi
                        '''
                    }
                }
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

