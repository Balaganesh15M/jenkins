pipeline {
    agent any

    environment {
        IMAGE_NAME = "userapi:latest"
        DEPLOYMENT_YAML = "deployment.yaml"
        PATH = "/usr/local/go/bin:$PATH"
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
                    echo "Checking Go version..."
                    go version || true
                    
                    echo "Building Go app..."
                    go mod tidy || true
                    go build -o userapi
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    echo "Setting Docker env to Minikube..."
                    eval $(minikube docker-env)

                    echo "Building Docker image for Minikube..."
                    docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    echo "Applying deployment to Minikube..."
                    kubectl apply -f $DEPLOYMENT_YAML
                '''
            }
        }
    }
}
