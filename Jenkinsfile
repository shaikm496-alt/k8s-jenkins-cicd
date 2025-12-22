pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
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
      items:
      - key: .dockerconfigjson
        path: config.json
"""
    }
  }

  stages {

    stage('Verify Docker Config') {
      steps {
        container('kaniko') {
          sh '''
            echo "Listing /kaniko/.docker"
            ls -l /kaniko/.docker
          '''
        }
      }
    }

    stage('Build and Push Image') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --dockerfile=app/Dockerfile \
              --context=dir:///home/jenkins/agent/workspace/${JOB_NAME} \
              --destination=mastan404/apache-tulasi:latest \
              --verbosity=debug
          '''
        }
      }
    }
  }
}
