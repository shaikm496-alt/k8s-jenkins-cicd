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
                sshagent(credentials: ['mastan-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no mastan@192.168.1.20 << 'EOF'
                      echo "Deleting old Kaniko job (if exists)"
                      kubectl delete job kaniko-build --ignore-not-found=true

                      echo "Applying Kaniko job"
                      kubectl apply -f kaniko-job.yaml

                      echo "Waiting for Kaniko job to complete"
                      kubectl wait --for=condition=complete job/kaniko-build --timeout=600s
                    EOF
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sshagent(credentials: ['mastan-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no mastan@192.168.1
