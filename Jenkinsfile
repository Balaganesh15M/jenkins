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

        stage('Install Go') {
            steps {
                sh '''
                    curl -LO https://golang.org/dl/go1.20.7.linux-amd64.tar.gz
                    sudo rm -rf /usr/local/go
                    sudo tar -C /usr/local -xzf go1.20.7.linux-amd64.tar.gz
                    echo "export PATH=$PATH:/usr/local/go/bin" >> $HOME/.profile
                    export PATH=$PATH:/usr/local/go/bin
                    go version
                '''
            }
        }

        stage('Go Build') {
            steps {
                sh '''
                    export PATH=$PATH:/usr/local/go/bin
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
