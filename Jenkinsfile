pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/shaikm496-alt/k8s-jenkins-cicd.git'
      }
    }

    stage('Build & Push Image (Kaniko Job)') {
      steps {
        sh '''
          kubectl delete job kaniko-build --ignore-not-found
          kubectl apply -f kaniko-job.yaml
          kubectl wait --for=condition=complete job/kaniko-build --timeout=300s
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s.yaml'
      }
    }
  }
}
