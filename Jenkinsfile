pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: dockerhub-secret
"""
    }
  }

  stages {
    stage('Build & Push Image') {
      steps {
        container('kaniko') {
          sh '''
          /kaniko/executor \
            --context=app \
            --dockerfile=app/Dockerfile \
            --destination=mastan404/k8s-jenkins-cicd:latest
          '''
        }
      }
    }
  }
}
