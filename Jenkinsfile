pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: test
    image: busybox
    command: ["sh", "-c", "sleep 30"]
"""
    }
  }

  stages {
    stage('Test') {
      steps {
        container('test') {
          sh 'echo POD_STARTED && hostname'
        }
      }
    }
  }
}
