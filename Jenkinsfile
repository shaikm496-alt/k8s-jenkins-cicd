pipeline {
  agent {
    kubernetes {
      label 'kaniko'
      defaultContainer 'kaniko'
    }
  }

  environment {
    IMAGE = "mastan404/demo-app:latest"
  }

  stages {
    stage('Build & Push') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --context $WORKSPACE \
              --dockerfile Dockerfile \
              --destination $IMAGE \
              --skip-tls-verify
          '''
        }
      }
    }
  }
}
