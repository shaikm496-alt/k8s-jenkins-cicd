pipeline {
  agent any

  stages {

    stage('Checkout Code') {
      steps {
        git branch: 'main',
            url: 'https://github.com/shaikm496-alt/k8s-jenkins-cicd.git'
      }
    }

    stage('Build & Push Image (Kaniko Job)') {
      steps {
        sshagent(credentials: ['master-ssh']) {
          sh '''
          ssh -o StrictHostKeyChecking=no mastan@localhost << EOF
            kubectl get nodes
            kubectl delete job kaniko-build --ignore-not-found=true
            kubectl apply -f kaniko-job.yaml
            kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
          EOF
          '''
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sshagent(credentials: ['master-ssh']) {
          sh '''
          ssh -o StrictHostKeyChecking=no mastan@localhost << EOF
            kubectl apply -f k8s.yaml
          EOF
          '''
        }
      }
    }
  }

  post {
    success {
      echo "✅ CI/CD Pipeline completed successfully"
    }
    failure {
      echo "❌ Pipeline failed. Check logs."
    }
  }
}
