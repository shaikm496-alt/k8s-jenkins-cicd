pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage running'
            }
        }
    }
}
