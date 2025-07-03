pipeline {
  agent any
  environment {
    DEPLOYMENT_YAML = '/home/balaganeshm/Desktop/jenkins/deployment.yaml'
  }
  stages {
    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }
    stage('Go Build') {
      steps {
        sh '''
          ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 '
            cd /home/balaganeshm/Desktop/jenkins &&
            go version &&
            go build -o userapi
          '
        '''
      }
    }
    stage('Docker Build (in Minikube)') {
      steps {
        sh '''
          ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 "
            eval \$(minikube docker-env) &&
            docker build -t userapi:latest /home/balaganeshm/Desktop/jenkins
          "
        '''
      }
    }
    stage('Deploy to Minikube') {
      steps {
        sh '''
          ssh -o StrictHostKeyChecking=no balaganeshm@172.17.0.1 "
            kubectl apply -f ${DEPLOYMENT_YAML}
          "
        '''
      }
    }
  }
}
