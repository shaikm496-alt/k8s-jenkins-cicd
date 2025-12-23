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

        stage('Build and Push Image using Kaniko') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      mkdir -p /kaniko/.docker

                      cat > /kaniko/.docker/config.json <<EOF
                      {
                        "auths": {
                          "https://index.docker.io/v1/": {
                            "username": "$DOCKER_USER",
                            "password": "$DOCKER_PASS"
                          }
                        }
                      }
EOF

                      /kaniko/executor \
                        --context ${WORKSPACE} \
                        --dockerfile ${WORKSPACE}/Dockerfile \
                        --destination ${DOCKER_IMAGE}
                    '''
                }
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
            echo "CI/CD Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed. Please check logs"
        }
    }
}
