pipeline {
  agent any

  environment {
    KUBECONFIG = "/var/jenkins_home/.kube/config"
  }

  stages {

    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Verify Files') {
      steps {
        sh '''
          echo "Workspace path:"
          pwd
          echo "Files in workspace:"
          ls -l
        '''
      }
    }

    stage('Run Kaniko Job') {
      steps {
        sh '''
          echo "Deleting old Kaniko job (if exists)..."
          kubectl delete job kaniko-build --ignore-not-found=true

          echo "Applying Kaniko job..."
          kubectl apply -f kaniko-job.yaml

          echo "Waiting for Kaniko job to complete..."
          kubectl wait --for=condition=complete job/kaniko-build --timeout=600s

          echo "Kaniko job completed successfully"
        '''
      }
    }
  }

  post {
    success {
      echo "✅ CI/CD Pipeline completed successfully"
    }
    failure {
      echo "❌ Pipeline failed. Check logs above."
    }
  }
}
