pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                sh '''
                  echo "Workspace:"
                  pwd
                  echo "Files:"
                  ls -l
                '''
            }
        }

        stage('Run Kaniko Job (from master kubectl)') {
            steps {
                sh '''
                  echo "Deleting old Kaniko job if exists"
                  kubectl delete job kaniko-build --ignore-not-found=true

                  echo "Applying Kaniko job"
                  kubectl apply -f kaniko-job.yaml

                  echo "Waiting for Kaniko build to complete"
                  kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Image build & push completed successfully"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}
