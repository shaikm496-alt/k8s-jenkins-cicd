pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mastan404/web-app:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shaikm496-alt/k8s-jenkins-cicd.git'
            }
        }

        stage('Build & Push Image (Kaniko Job)') {
            steps {
                sh '''
                kubectl delete job kaniko-build --ignore-not-found=true

                kubectl apply -f kaniko-job.yaml

                kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s.yaml
                '''
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
