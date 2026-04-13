pipeline {
  agent any

  environment {
    IMAGE_NAME = 'diabetes_multi_layer_perceptron'
    CONTAINER_NAME = 'diabetes_multi_layer_perceptron-app'
    APP_PORT = '8020'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .'
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
          docker rm -f ${CONTAINER_NAME} || true
          docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:8020 ${IMAGE_NAME}:latest
        '''
      }
    }

    stage('Health Check') {
      steps {
        sh 'curl -f http://localhost:${APP_PORT}/ >/dev/null'
      }
    }
  }

  post {
    success {
      echo 'Deployment succeeded. App is running in Docker.'
    }
    failure {
      echo 'Pipeline failed. Check logs for details.'
    }
  }
}