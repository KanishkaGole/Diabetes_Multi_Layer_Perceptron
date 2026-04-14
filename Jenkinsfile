pipeline {
  agent any

  environment {
    IMAGE_NAME = 'diabetes_multi_layer_perceptron'
    CONTAINER_NAME = 'diabetes_multi_layer_perceptron-app'
    APP_PORT = '8020'
    PATH = '/usr/local/bin:/opt/homebrew/bin:/Applications/Docker.app/Contents/Resources/bin:/usr/bin:/bin:/usr/sbin:/sbin'
  }

  stages {
    stage('Validate Build Tools') {
      steps {
        sh '''
          set -e
          echo "PATH=$PATH"
          command -v docker >/dev/null || {
            echo "ERROR: docker CLI not found in PATH for Jenkins user."
            echo "Install Docker Desktop and expose docker to Jenkins PATH."
            exit 127
          }
          command -v curl >/dev/null || {
            echo "ERROR: curl not found in PATH for Jenkins user."
            exit 127
          }
          docker --version
          curl --version | head -n 1
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          set -e
          docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
        '''
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
          set -e
          docker rm -f ${CONTAINER_NAME} || true
          docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:8020 ${IMAGE_NAME}:latest
        '''
      }
    }

    stage('Health Check') {
      steps {
        sh '''
          set -e
          echo "Waiting for application to become healthy on port ${APP_PORT}..."
          for i in $(seq 1 30); do
            if curl -fsS http://localhost:${APP_PORT}/ >/dev/null; then
              echo "Health check passed on attempt ${i}."
              exit 0
            fi
            echo "Health check attempt ${i}/30 failed; retrying in 2s..."
            sleep 2
          done
          echo "Health check failed after 30 attempts."
          exit 1
        '''
      }
    }
  }

  post {
    success {
      echo 'Deployment succeeded. App is running in Docker.'
    }
    failure {
      echo 'Pipeline failed. Collecting container diagnostics...'
      sh '''
        docker ps -a --filter name=${CONTAINER_NAME} || true
        docker logs --tail 200 ${CONTAINER_NAME} || true
      '''
    }
  }
}