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
    stage('Debug') {
      steps {
        echo 'Jenkinsfile is executing stages'
      }
    }

    stage('Build and Push Image') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --dockerfile=app/Dockerfile \
              --context=dir://. \
              --destination=mastan404/apache-tulasi:latest
          '''
        }
      }
    }
  }
}
