pipeline {
    agent any

    environment {
        KUBECTL = "kubectl"
        KANIKO_JOB = "kaniko-build"
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

        stage('Delete Old Kaniko Job (if exists)') {
            steps {
                sh '''
                  echo "Deleting old Kaniko job if exists..."
                  ${KUBECTL} delete job ${KANIKO_JOB} --ignore-not-found=true
                '''
            }
        }

        stage('Apply Kaniko Job') {
            steps {
                sh '''
                  echo "Applying Kaniko job..."
                  ${KUBECTL} apply -f kaniko-job.yaml
                '''
            }
        }

        stage('Wait for Kaniko Build') {
            steps {
                sh '''
                  echo "Waiting for Kaniko job to complete..."
                  ${KUBECTL} wait --for=condition=complete job/${KANIKO_JOB} --timeout=600s
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Image build completed successfully"
        }
        failure {
            echo "❌ Pipeline failed. Check logs above."
        }
    }
}
