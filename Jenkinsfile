pipeline {
  agent {
    kubernetes {
      label 'kaniko-agent'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    imagePullPolicy: Always
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: dockerhub-secret
      items:
      - key: .dockerconfigjson
        path: config.json
"""
    }
  }

  stages {
    stage('Build & Push Docker Image') {
      steps {
        container('kaniko') {
          sh '''
            echo "Starting Kaniko build..."

            /kaniko/executor \
              --context $WORKSPACE/app \
              --dockerfile $WORKSPACE/app/Dockerfile \
              --destination docker.io/mastan404/nginx-app:latest \
              --skip-tls-verify \
              --verbosity=info
          '''
        }
      }
    }
  }
}
