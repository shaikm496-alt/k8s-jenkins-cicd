pipeline {
  agent {
    kubernetes {
      label 'simplefile'
      defaultContainer 'kubectl'
      yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  containers:
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true
"""
    }
  }

  stages {
    stage('Test') {
      steps {
        sh 'kubectl get pods -n jenkins'
      }
    }
  }
}
