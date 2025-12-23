pipeline {
  agent {
    kubernetes {
      label 'k8s-test'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest
"""
    }
  }

  stages {
    stage('Kubernetes Agent Test') {
      steps {
        echo '✅ Jenkins is running inside a Kubernetes pod'
        sh 'uname -a'
        sh 'echo Hello from Kubernetes Agent'
      }
    }
  }
}
