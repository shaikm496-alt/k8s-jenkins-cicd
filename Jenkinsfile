pipeline {
  agent {
    kubernetes {
      label 'kaniko'
      defaultContainer 'kaniko'
    }
  }

  environment {
    IMAGE_NAME = "mastan404/sample-app"
    IMAGE_TAG  = "latest"
  }

  stages {
    stage('Build & Push Image') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --context $WORKSPACE \
              --dockerfile Dockerfile \
              --destination ${IMAGE_NAME}:${IMAGE_TAG} \
              --skip-tls-verify
          '''
        }
      }
    }
  }
}
