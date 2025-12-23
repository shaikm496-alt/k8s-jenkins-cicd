pipeline {
  agent any

  environment {
    IMAGE_NAME = "mastan404/k8s-jenkins-demo"
    IMAGE_TAG  = "latest"
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
        sh '''
          kubectl delete job kaniko-build --ignore-not-found=true
          kubectl apply -f kaniko-job.yaml
          kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
        '''
      }
    }

  }
}
