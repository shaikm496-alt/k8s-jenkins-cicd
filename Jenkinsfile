pipeline {
  agent {
    kubernetes {
      label 'kaniko'
      inheritFrom 'jnlp-agent'
      defaultContainer 'kaniko'
    }
  }

  environment {
    IMAGE_NAME = "docker.io/mastan404/demo-app"
    IMAGE_TAG  = "v1"
  }

  stages {

    stage('Agent Check') {
      steps {
        sh '''
          echo "Jenkins agent started"
          uname -a
          ls -la
        '''
      }
    }

    stage('Build & Push Image (Kaniko)') {
      steps {
        sh '''
          /kaniko/executor \
            --context $(pwd) \
            --dockerfile Dockerfile \
            --destination ${IMAGE_NAME}:${IMAGE_TAG} \
            --verbosity info
        '''
      }
    }
  }
}
