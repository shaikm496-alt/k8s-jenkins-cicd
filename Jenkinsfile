pipeline {
  agent {
    kubernetes {
      label 'kaniko'
    }
  }
  stages {
    stage('Build') {
      steps {
        container('kaniko') {
          sh 'echo Hello from Kaniko'
        }
      }
    }
  }
}
