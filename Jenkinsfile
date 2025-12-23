stage('Build & Push Image (Kaniko)') {
  steps {
    sh '''
      kubectl apply -f kaniko-job.yaml
      kubectl wait --for=condition=complete job/kaniko-build --timeout=300s
    '''
  }
}
