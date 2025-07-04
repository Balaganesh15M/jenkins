pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myuserapi:latest'
        DEPLOYMENT_YAML = 'deployment.yaml'
        GOROOT = '/tmp/go/go'
        PATH = '/tmp/go/go/bin:$PATH'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Balaganesh15M/jenkins.git'
            }
        }

        stage('Install Go') {
            steps {
                sh '''
                    echo "Installing Go..."
                    curl -LO https://golang.org/dl/go1.20.7.linux-amd64.tar.gz
                    rm -rf /tmp/go
                    mkdir -p /tmp/go
                    tar -C /tmp/go -xzf go1.20.7.linux-amd64.tar.gz
                    /tmp/go/go/bin/go version
                '''
            }
        }

        stage('Go Build') {
            steps {
                sh '''
                    export PATH=/tmp/go/go/bin:$PATH
                    go mod tidy
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
