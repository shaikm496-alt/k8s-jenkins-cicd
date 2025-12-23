pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
    resources:
      requests:
        cpu: "25m"
        memory: "48Mi"
      limits:
        cpu: "100m"
        memory: "96Mi"
"""
    }
  }

  stages {
    stage('Test') {
      steps {
        echo "Agent connected successfully"
      }
    }
  }
}
