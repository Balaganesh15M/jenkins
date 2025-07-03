pipeline {
    agent any

    environment {
        // Permanently fix Go version path for Jenkins pipeline
        PATH = "/usr/local/go/bin:$PATH"
    }

    stages {
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
                sh '''
                    docker build -t userapi:latest .
                '''
            }
        }

        stage('Deploy to Minikube') {
  steps {
    sh 'kubectl apply -f deployment.yaml'
  }
}

    }
}
