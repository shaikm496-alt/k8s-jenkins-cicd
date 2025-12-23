pipeline {
    agent {
        kubernetes {
            defaultContainer 'kubectl'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true

  - name: jnlp
    image: jenkins/inbound-agent:latest
"""
        }
    }

    stages {

        stage('Verify') {
            steps {
                sh '''
                  echo "Running inside Kubernetes pod"
                  whoami
                  pwd
                  ls -l
                '''
            }
        }

        stage('Delete old Kaniko job') {
            steps {
                sh '''
                  kubectl delete job kaniko-build -n jenkins --ignore-not-found=true
                '''
            }
        }

        stage('Apply Kaniko job') {
            steps {
                sh '''
                  kubectl apply -f kaniko-job.yaml -n jenkins
                '''
            }
        }

        stage('Wait for Kaniko') {
            steps {
                sh '''
                  kubectl wait --for=condition=complete job/kaniko-build -n jenkins --timeout=600s
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build & image creation successful"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
