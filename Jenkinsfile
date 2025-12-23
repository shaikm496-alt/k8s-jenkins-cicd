pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/mastan404/k8s-jenkins-cicd.git'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t mastan404/web-app:latest .'
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push mastan404/web-app:latest'
      }
    }

    stage('Deploy to K8s') {
      steps {
        sh 'kubectl apply -f k8s.yaml'
      }
    }
  }
}
