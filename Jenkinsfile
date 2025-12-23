pipeline {
  agent {
    kubernetes {
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

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/shaikm496-alt/k8s-jenkins-cicd.git'
      }
    }

    stage('Build & Push Image (Kaniko)') {
      steps {
        container('kubectl') {
          sh '''
            kubectl delete job kaniko-build --ignore-not-found=true
            kubectl apply -f kaniko-job.yaml
            kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
          '''
        }
      }
    }
  }
}
