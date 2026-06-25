pipeline {
    agent any
    stages {
        stage('Trivy Scan') {
            steps {
                sh 'trivy image nginx:latest'
            }
        }
    }
}
