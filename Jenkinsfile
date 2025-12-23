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
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                container('kubectl') {
                    sh '''
                      pwd
                      ls -l
                    '''
                }
            }
        }

        stage('Run Kaniko Job') {
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

    post {
        success {
            echo "✅ Kaniko image build & push successful"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
